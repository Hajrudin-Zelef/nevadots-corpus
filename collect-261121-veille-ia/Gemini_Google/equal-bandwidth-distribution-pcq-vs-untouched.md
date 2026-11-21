---
id: collect-261121-veille-ia/gemini-google/equal-bandwidth-distribution-pcq-vs-untouched
title: "Equal Bandwidth Distribution: PCQ vs. 'Untouched'"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["bandwidth", "connection", "queue", "pcq-down", "pcq-up", "packet", "mangling", "equal"]
source: https://forum.mikrotik.com/t/equal-bandwidth-distribution-pcq-vs-untouched/129970
source_anchor: ""
source_lines: [1, 256]
section: "Gemini_Google"
---
Say I have 100/10 Mb WAN available bandwidth and I want to go with “certain bandwidth equal distribution between users” - so that if I have only one user connected they would get 100/10, two would get 50/5 and so on. Here’s the configuration I currently have:

ether1 = WAN

ether2 = LAN

Mangle:

```
/ip firewall mangle
add action=mark-connection chain=prerouting comment=pcq-down_connection in-interface=ether1 new-connection-mark=pcq-down_connection \
    passthrough=no
add action=mark-connection chain=prerouting comment=pcq-up_connection in-interface=ether2 new-connection-mark=pcq-up_connection passthrough=\
    no
add action=mark-packet chain=forward comment=pcq-down_packet connection-mark=pcq-down_connection new-packet-mark=pcq-down_packet \
    passthrough=no
add action=mark-packet chain=forward comment=pcq-up_packet connection-mark=pcq-up_connection new-packet-mark=pcq-up_packet passthrough=no
```

Queue Types:

```
/queue type
add kind=pcq name=pcq-down pcq-classifier=dst-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64
add kind=pcq name=pcq-up pcq-classifier=src-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64
```

Queue Tree:

```
/queue tree
add max-limit=100M name=down packet-mark=pcq-down_packet parent=global queue=pcq-down
add max-limit=10M name=up packet-mark=pcq-up_packet parent=global queue=pcq-up
```

A few questions:

1. Is the above configuration correct?
2. If two users are connected, will they only be able to use 50/5 **all the time** or if one is not requiring bandwidth the other will be able to use more than half?
3. In terms of bandwidth distribution, what would be the router default behaviour (for this example)? How would this (PCQ) differ from not having any queue configured at all?

Thanks in advance!

             
            
           
          
            
            
              Hi

1. No, connection have both up and down “legs”. Hence your mangling changes the connection marks on a connection back and forth to “up” and “down”, with unpredictable results on the actual packet mangling…
2. available bandwidth will be split equally and depending on load. If one users is not using it’s allotment, it will be spread over other “requesters”.
3. Without PCQ, a user with 100 connections will have more bandwidth than a users with only 1 connection. With PCQ total bandwidth is accounted.

Wrt 1, remove all the mangling and just match on “no-mark” in queue tree. This will also allow you to use FastTrack…

Suggestion: max limit should be chosen so that your latency is not increased. Either measure that point (try some values) or reserve say 5% for QOS; The max-limits should be then 95% of your total bandwidth.

Edit: oh, and use interfaces as parents, not global

             
            
           
          
            
            
              Hi Sebastia was the message as follows (in a simpleton construct for moi):

a.   apportioning **bandwidth per user** is best a dish served with **queues** - mangling not required.

b.   **pcc** is best a dish served for **equal access/usage to multiple WANs** (how one skews the PCC, will determine any ratios required for the Load Balancing for unequal throughput ISPs) - mangling required.

note to self:  next time don’t confuse pcc and pcq 

             
            
           
          
            
            
              if accounting bandwidth per users it’s usually because it’s scarce, and hence queueing will be used. PCQ is a type of queue.

PCC is load balancing / routing method (pcc requires mangling). so indeed spreading the load is the essence.

             
            
           
          
            
            
              
*1. No, connection have both up and down “legs”. Hence your mangling changes the connection marks on a connection back and forth to “up” and “down”, with unpredictable results on the actual packet mangling…*

It was working the way it was (both upload and download queues, I run several tests). But I did some reading based on what you said and modified the mangle rules. I do want to use mangle rules because I want to apply the queues to only one specific subnet. Queues still working fine after changes. Is it correct now?

Lan (ether2): 10.10.1.1/24

**Mangle**:

```
/ip firewall mangle
add action=mark-connection chain=prerouting comment=pcq-connection dst-address=0.0.0.0/0 in-interface=ether2 new-connection-mark=\
    pcq-connection passthrough=no src-address=10.10.1.2-10.10.1.254
add action=mark-packet chain=forward comment=pcq-down connection-mark=pcq-connection dst-address=10.10.1.2-10.10.1.254 new-packet-mark=\
    pcq-down passthrough=no
add action=mark-packet chain=forward comment=pcq-up connection-mark=pcq-connection new-packet-mark=pcq-up passthrough=no src-address=\
    10.10.1.2-10.10.1.254
```

Does the “dst-address=0.0.0.0/0” parameter on the first rule guarantee only outbound connections to the internet will be marked? Or would it be better to specify ether1 (WAN) interface on the following rules (por packet markings) accordingly?

**Queue:**

```
/queue type
add kind=pcq name=pcq-down pcq-classifier=dst-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64 pcq-total-limit=12650KiB
add kind=pcq name=pcq-up pcq-classifier=src-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64 pcq-total-limit=12650KiB
/queue tree
add max-limit=85M name=down packet-mark=pcq-down parent=global queue=pcq-down
add max-limit=8500k name=up packet-mark=pcq-up parent=global queue=pcq-up
```

*2. available bandwidth will be split equally and depending on load. If one users is not using it’s allotment, it will be spread over other “requesters”.*

I could verify that by running simultaneous tests on two computers.

*3. Without PCQ, a user with 100 connections will have more bandwidth than a users with only 1 connection. With PCQ total bandwidth is accounted.*

Thank you for this explanation!

About the max-limits, I’m already aware about the best practices. The bandwidth values that I’d been posting so far were fictitious in order to simplify.

*Edit: oh, and use interfaces as parents, not global*

Could you please provide some more information on why is that?

             
            
           
          
            
            
              - 
current mangles should work, but specifying interface is easier than working with ip’s
 ** to eth2 → download
** to eth1 → upload
- 
at this time there is no advantage in mangling as all to-from-lan is marked with “pcq-connection”, still a to-do for future?
- 
usually (except some specific situations) only mark a connection if it is not marked yet
- 
“0.0.0.0/0” means “anything”, internal or external
- 
“pcq-total-limit=12650KiB” that’s a lot of buffer = 100MBit
 for download that’s more than 1 sec at full speed
 for upload more than 12 sec → very bad for latency!!
aim should be sub-second.

interface queue or global queue have both their strengths and weaknesses

Interface htb:

- view on traffic linked to interface only
global htb

- view on overall bandwidth, across all interfaces

- incompatible with fasttrack

you only have one wan, so there is no advantage in using global at this time.

             
            
           
          
            
            
              ** current mangles should work, but specifying interface is easier than working with ip’s*

Thanks.




```
/ip firewall mangle
add action=mark-connection chain=prerouting comment=pcq-connection in-interface=ether2 new-connection-mark=pcq-connectio
add action=mark-packet chain=forward comment=pcq-down connection-mark=pcq-connection in-interface=ether1 new-packet-mark
    passthrough=no
add action=mark-packet chain=forward comment=pcq-up connection-mark=pcq-connection new-packet-mark=pcq-up out-interface=
    passthrough=no
```

** at this time there is no advantage in mangling as all to-from-lan is marked with “pcq-connection”, still a to-do for future?*

Yes!

** “0.0.0.0/0” means “anything”, internal or external*

Ok.

*interface queue or global queue have both their strengths and weaknesses[…]*

Thanks for the explanation! I changed the parent queues to interface:




```
/queue type
add kind=pcq name=pcq-down pcq-classifier=dst-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64 pcq-total-limit=12650KiB
add kind=pcq name=pcq-up pcq-classifier=src-address pcq-dst-address6-mask=64 pcq-src-address6-mask=64 pcq-total-limit=12650KiB
/queue tree
add max-limit=85M name=down packet-mark=pcq-down parent=ether2 queue=pcq-down
add max-limit=8500k name=up packet-mark=pcq-up parent=ether1 queue=pcq-up
```

** “pcq-total-limit=12650KiB” that’s a lot of buffer[…]*

Well, I didn’t quite understand these parameters yet. I saw some examples on this MUM document (pages 26-27): https://mum.mikrotik.com/presentations/US08/janism.pdf

Based on those examples, I thought the values I set meant 253 users (/24) could use the queues simultaneously (although very unlikely to happen).

Currently, values on both queue types are set to:

Limit: 50

Total limit: 12650

Could you please give me some directions about these parameters and suggest something? I think it’s the only missing piece to my setup now!

             
            
           
          
            
            
              If it was up to me, the pcq-total-limit shouldn’t be much larger than 1/10 s of max transmission on upload side: suppose you have a gamer, that would have latency of 100ms… he wouldn’t be happy.

On download the queue is only there to account for and spread the bandwidth. It’s an artificial bottleneck to control throughput. So there is no need to have a large buffer. => default values will do.

Large queues are good to fill the pipe, but bad for QoS.

             
            
           
          
            
            
              
The bandwidth I provided on the post didn’t correspond to the actual scenario (I thought using a round number would make it more simple to understand), so not that many users will be queued at the same time.

I did some research (even on this forum) and I thought there’s not much straightforward information on this topic (pcq-limit and pcq-total-limit). I ran some tests on my own and set those parameters to values that I judged to be balanced. Not worthy sharing/explaining the values that I’ve chosen because it’s NOT based on deep reading/lots of experience, so it could be misleading. But one thing that I noticed and should note is that “pcq-total-limit” doesn’t affect users individually, since they are being limited by “pcq-limit” (single sub-stream size), so despite how big “pcq-total-limit” is, single sub-streams are never gonna be bigger than “pcq-limit”.

Thank you very much for your help.
