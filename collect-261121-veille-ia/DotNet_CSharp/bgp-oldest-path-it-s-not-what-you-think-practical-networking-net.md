---
id: collect-261121-veille-ia/dotnet-csharp/bgp-oldest-path-it-s-not-what-you-think-practical-networking-net
title: "BGP Oldest Path – It's not what you think... – Practical Networking .net"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["path", "address", "interface", "prefer", "neighbor", "description", "link", "oldest"]
source: https://www.practicalnetworking.net/stand-alone/bgp-oldest-path/
source_anchor: ""
source_lines: [1, 477]
section: "DotNet_CSharp"
---
BGP is the predominant dynamic routing protocol used to exchange routes between different autonomous systems. BGP’s popularity lies in the Path Selection process which allows extremely granular control of the path for incoming and outgoing traffic. One of the steps in this process states that if a tie still exists between two paths, BGP will prefer the Oldest Path. However, the BGP Oldest Path criteria does not work as most people think.

In this article, we will describe the BGP Oldest Path selection criteria, illustrate how it works, and explain specifically how it doesnât work as most people think.

To provide context, below is the full BGP Path Selection criteria that BGP uses to select a single best path among multiple known paths:

Path Selection:

1. If next-hop is inaccessible, drop the update
2. Prefer path with the largest Weight (Cisco proprietary)
3. Prefer path with the largest Local-Preference
4. Prefer path with locally originated routes vs externally learned
5. Prefer path with shortest AS-Path
6. Prefer path with best Origin (IGP > EGP > Incomplete)
7. Prefer path with lowest MED
8. Prefer path learned from eBGP over iBGP
9. Prefer path with the lowest IGP metric to the next-hop IP
10. **Prefer path with the greatest age (oldest path â eBGP only)**
11. Prefer path learned from neighbor with lowest Router ID
12. Prefer path learned from neighbor with lowest Neighbor IP Address

If two paths to a particular prefix exist, and all of the attributes in Steps 1-9 are identical, Step 10 will attempt to break the tie by preferring the oldest path.


### Topology

We will use the following topology to put the BGP Oldest Path criteria to the test. R5 in AS-55 will announce the 5.5.5.0/24 network, which will be shared via R2, R3, and R4 in AS-22, AS-33, and AS-44 (respectively). Each of these AS’s will then share the path to R1 in AS-11.

This is the initial configuration of each Router. All BGP adjacencies have been configured and R5 is announcing the only prefix in the BGP topology for the 5.5.5.0/24 network). R1 has learned of three paths to the 5.5.5.0/24 network â one via each of R2, R3, and R4.

*Click tabs to view initial configuration for each router.*

!
hostname **R1**
!
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.0
!
interface Ethernet1/0
 description Link to R2 in AS-22
 ip address 9.22.11.1 255.255.255.0
!
interface Ethernet1/1
 description Link to R3 in AS-33
 ip address 9.33.11.1 255.255.255.0
!
interface Ethernet1/2
 description Link to R4 in AS-44
 ip address 9.44.11.1 255.255.255.0
!
!
**router bgp 11**
 bgp log-neighbor-changes
 neighbor 9.22.11.2 remote-as 22
 neighbor 9.33.11.3 remote-as 33
 neighbor 9.44.11.4 remote-as 44
!

!
hostname **R2**
!
!
interface Loopback0
 ip address 2.2.2.2 255.255.255.0
!
interface Ethernet0/0
 description Link to R5 in AS-55
 ip address 9.55.22.2 255.255.255.0
!
interface Ethernet1/0
 description Link to R1 in AS-11
 ip address 9.22.11.2 255.255.255.0
!
!
**router bgp 22**
 bgp log-neighbor-changes
 neighbor 9.22.11.1 remote-as 11
 neighbor 9.55.22.5 remote-as 55
!

!
hostname **R3**
!
!
interface Loopback0
 ip address 3.3.3.3 255.255.255.0
!
interface Ethernet0/1
 description Link to R5 in AS-55
 ip address 9.55.33.3 255.255.255.0
!
interface Ethernet1/1
 description Link to R1 in AS-11
 ip address 9.33.11.3 255.255.255.0
!
!
**router bgp 33**
 bgp log-neighbor-changes
 neighbor 9.33.11.1 remote-as 11
 neighbor 9.55.33.5 remote-as 55
!

!
hostname **R4**
!
!
interface Loopback0
 ip address 4.4.4.4 255.255.255.0
!
interface Ethernet0/2
 description Link to R5 in AS-55
 ip address 9.55.44.4 255.255.255.0
!
interface Ethernet1/2
 description Link to R1 in AS-11
 ip address 9.44.11.4 255.255.255.0
!
!
**router bgp 44**
 bgp log-neighbor-changes
 neighbor 9.44.11.1 remote-as 11
 neighbor 9.55.44.5 remote-as 55
!

!
hostname **R5**
!
!
**interface Loopback0
 ip address 5.5.5.5 255.255.255.0**
!
interface Ethernet0/0
 description Link to R2 in AS-22
 ip address 9.55.22.5 255.255.255.0
!
interface Ethernet0/1
 description Link to R3 in AS-33
 ip address 9.55.33.5 255.255.255.0
!
interface Ethernet0/2
 description Link to R4 in AS-44
 ip address 9.55.44.5 255.255.255.0
!
!
**router bgp 55**
 bgp log-neighbor-changes
 **network 5.5.5.0 mask 255.255.255.0**
 neighbor 9.55.22.2 remote-as 22
 neighbor 9.55.33.3 remote-as 33
 neighbor 9.55.44.4 remote-as 44
!

None of the path selection attributes have been modified, ensuring Steps 1-9 are all identical and do not explicitly prefer a path. Leaving only Steps 10-12 to break any ties that may exist.


### Initial Setup

R1 has three neighbor adjacencies, each of which announce a path to the 5.5.5.0/24 prefix. In order to deterministically set the age of each path for our experiment we will shut down all neighbor adjacencies:

R1(config)# **router bgp 11**
R1(config-router)# **neighbor 9.22.11.2 shutdown**
R1(config-router)# **neighbor 9.33.11.3 shutdown**
R1(config-router)# **neighbor 9.44.11.4 shutdown**
R1(config-router)#
*May  9 19:00:10.057: %BGP-5-ADJCHANGE: neighbor 9.22.11.2 Down Admin. shutdown
*May  9 19:00:10.058: %BGP-5-ADJCHANGE: neighbor 9.33.11.3 Down Admin. shutdown
*May  9 19:00:10.892: %BGP-5-ADJCHANGE: neighbor 9.44.11.4 Down Admin. shutdown

Then re-enable them one by one:

R1(config-router)# **no neighbor 9.22.11.2 shutdown**
***May  9 19:01:48.258: %BGP-5-ADJCHANGE: neighbor 9.22.11.2 Up**
R1(config-router)#
R1(config-router)# **no neighbor 9.33.11.3 shutdown**
***May  9 19:03:07.507: %BGP-5-ADJCHANGE: neighbor 9.33.11.3 Up**
R1(config-router)#
R1(config-router)# **no neighbor 9.44.11.4 shutdown**
***May  9 19:04:22.675: %BGP-5-ADJCHANGE: neighbor 9.44.11.4 Up**
R1(config-router)#

Between each âno shutdownâ command, we waited about a minute. This can be verified against the time stamp in the debug messages stating the neighbor adjacency came back up.

This gives us a topology where R1 has three neighbor adjacencies, with R2 being the oldest, followed by R3, followed by R4:

R1# **show ip bgp summary**
BGP router identifier 1.1.1.1, local AS number 11
...
Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
**9.22.11.2**       4           22      11       9        7    0    0 **00:04:58**        1
**9.33.11.3**       4           33       9       9        7    0    0 **00:03:39**        1
**9.44.11.4**       4           44       8       8        7    0    0 **00:02:24**        1

R1 has three paths to the 5.5.5.0/24 prefix, with the path through R2 currently selected as the best path (9.22.11.2):

R1# **show ip bgp**
...
     Network          Next Hop            Metric LocPrf Weight Path
 *   5.5.5.0/24       9.44.11.4                              0 44 55 i
 *                    9.33.11.3                              0 33 55 i
 ***>                   9.22.11.2**                              0 22 55 i

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 7
Paths: (3 available, best #3, table default)
  Advertised to update-groups:
     3
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 (4.4.4.4)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  33 55
    9.33.11.3 from 9.33.11.3 (3.3.3.3)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  22 55
    **9.22.11.2 from 9.22.11.2 (2.2.2.2)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0

The best path is indicated by the “**>****show ip bgp**`best`” in theÂ **show ip bgp 5.5.5.5.0/24**


### BGP Oldest Path – Trial 1

Weâll begin by shutting down the present oldest path to the 5.5.5.0/24 prefix â the path through R2:

R1(config)# **router bgp 11**
R1(config-router)# **neighbor 9.22.11.2 shutdown**
*May  9 19:23:25.912: %BGP-5-ADJCHANGE: neighbor 9.22.11.2 Down Admin. shutdown

Because of the order in which we brought the neighbor adjacencies back up, we know that R3 had the “next oldest” path. And indeed, this is the path R1 now selects:

R1# **show ip bgp**
...
     Network          Next Hop            Metric LocPrf Weight Path
 *   5.5.5.0/24       9.44.11.4                              0 44 55 i
 ***>                   9.33.11.3**                              0 33 55 i

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 8
Paths: (2 available, best #2, table default)
  Advertised to update-groups:
     3
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 (4.4.4.4)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  33 55
    **9.33.11.3 from 9.33.11.3 (3.3.3.3)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0

We can then re-enable the adjacency to R2:

R1(config)# **router bgp 11**
R1(config-router)# **no neighbor 9.22.11.2 shutdown**
*May  9 19:27:22.811: %BGP-5-ADJCHANGE: neighbor 9.22.11.2 Up

And as expected, we see R1 learns of the new path through R2 but does not select it as best, as the current oldest (and best) path is still through R3:

R1# **show ip bgp**
...
     Network          Next Hop            Metric LocPrf Weight Path
 *   5.5.5.0/24       **9.22.11.2**                              0 22 55 i
 *                    9.44.11.4                              0 44 55 i
 ***>                   9.33.11.3**                              0 33 55 i

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 8
Paths: (3 available, best #3, table default)
  Advertised to update-groups:
     3
  Refresh Epoch 2
  22 55
    **9.22.11.2 from 9.22.11.2 (2.2.2.2)**
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 (4.4.4.4)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  33 55
    **9.33.11.3 from 9.33.11.3 (3.3.3.3)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0

Notice also the output of the **`show ip bgp`** and **`show ip bgp 5.5.5.0/24`** commands are displaying the paths in the order they have learned them. R3 being the oldest (at the bottom), followed by R4, followed by R2 (at the top).

So far, everything is working as we expected it to. But maybe not exactly for the reason you thought it might. Continue reading to find out exactly why R3 was chosen and remained the best path after the path through R2 was lost and then reacquired.

### BGP Oldest Path – Trial 2

This is where it gets interesting.

At the moment, we have three paths to 5.5.5.0/24, learned in this order: R3, R4, R2. We will go ahead and shut down the R3 adjacency (the current oldest path) to see what R1 picks next for the best path.

R1(config)# **router bgp 11**
R1(config-router)# **neighbor 9.33.11.3 shutdown**
*May  9 19:35:16.041: %BGP-5-ADJCHANGE: neighbor 9.33.11.3 Down Admin. shutdown

If Step 10 was purely based upon a pathâs absolute age, then R4 should be picked as the best path, since it is the next oldest path. But youâll see that is not what happens:

R1# **show ip bgp**
...
     Network          Next Hop            Metric LocPrf Weight Path
 ***>**  5.5.5.0/24       **9.22.11.2**                              0 22 55 i
 *                    9.44.11.4                              0 44 55 i

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 9
Paths: (2 available, best #1, table default)
  Advertised to update-groups:
     3	
  Refresh Epoch 2
  22 55
    **9.22.11.2 from 9.22.11.2 (2.2.2.2)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 (4.4.4.4)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0

R2 was selected as the best path, even though we know that the absolute oldest path was the path through R4 (and the order of the output in the commands above also confirm this).

We will discuss the reason for this behavior, but before we do lets re-enable the R3 adjacency and ensure R1 knows of all three paths to 5.5.5.0/24:

R1(config)# **router bgp 11**
R1(config-router)# **no neighbor 9.33.11.3 shutdown**
*May  9 19:37:46.989: %BGP-5-ADJCHANGE: neighbor 9.33.11.3 Up

R1# **show ip bgp**
...
     Network          Next Hop            Metric LocPrf Weight Path
 *   5.5.5.0/24       9.33.11.3                              0 33 55 i
 ***>                   9.22.11.2**                              0 22 55 i
 *                    9.44.11.4                              0 44 55 i

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 9
Paths: (3 available, best #2, table default)
  Advertised to update-groups:
     3
  Refresh Epoch 2
  33 55
    9.33.11.3 from 9.33.11.3 (3.3.3.3)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  22 55
    **9.22.11.2 from 9.22.11.2 (2.2.2.2)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 (4.4.4.4)
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0


### What Happened

Initially, R1 learned of three paths to the 5.5.5.0/24 prefix in the order of R2, then R3, then R4. When we disabled the R2 adjacency, R1âs next best path was through R3. It is generally accepted that this occurred because R3 was the next oldest path, but what happened next proved that this is not entirely true.

After R2 came back up, R1 had three paths to the 5.5.5.0/24 prefix in the order of R3, then R4, then R2. When we disabled R3, R1 did not choose R4 as the next best path. R1 instead chose R2 as the next best path. But why?

This happened because when the path through R3 was lost, BGP looked at its topology table and used the Path Selection process to pick the next best path. Steps 1-9 were all tied, which brought the BGP speaker to Step 10. At the time R1 lost the path through R3, R1Â *already had both paths through R2 and R4* in the topology table. Since they both already existed *when Step 10 was being processed*, from R1’s perspective neither one was older than the other. As such, Step 10 resulted in a tie.

BGP then used the *next step* in the Path Selection process to break the tie: Step 11 â Preferring the path with the lowest Router-ID. Between R2 and R4, R2 had a better Router-ID (`2.2.2.2`). In the output of **`show ip bgp 5.5.5.0/24`** we can see the Router-ID of each neighbor listed next to the next-hop IP address:

R1# **show ip bgp 5.5.5.0/24**
BGP routing table entry for 5.5.5.0/24, version 9
Paths: (3 available, best #2, table default)
  Advertised to update-groups:
     3
  Refresh Epoch 2
  33 55
    9.33.11.3 from 9.33.11.3 **(3.3.3.3)**
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  22 55
    9.22.11.2 from 9.22.11.2 **(2.2.2.2)**
      Origin IGP, localpref 100, valid, external, **best**
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 2
  44 55
    9.44.11.4 from 9.44.11.4 **(4.4.4.4)**
      Origin IGP, localpref 100, valid, external
      rx pathid: 0, tx pathid: 0

So what happened in the first example? R1 initially knew of the paths in this order R2, then R3, then R4. When R1 lost the path through R2, the next best path selected was R3. Not because R3 was the next oldest route, but because between the remaining paths R3 had a better Router-ID than R4. Step 11 preferred the path with the lower Router-ID.


### Synopsis

Ultimately, Step 10 exists to prevent route table instability.

Consider what could happen if Step 10 did not exist, and instead only Step 11 (prefer the path with the lowest router-id) was breaking ties (presuming steps 1-9 were equal). The Router-ID of the next eBGP peer you build an adjacency with cannot be known, therefore the Router-ID is essentially random.

If at some point in the future you buildÂ a redundant ISP connection with another eBGP peer which happens to have a better (lower) Router-ID than your current ISPâs eBGP peers, all your traffic would shift off of the current âknown goodâ path, to the new âuntestedâ path.

Moreover, if you happened to have a neighbor adjacency start to flap, and that neighbor happened to have the best Router-ID, then you would continually have traffic âflapâ to the peer when it is up, and âflapâ away when it goes down. This could cause all sorts of instability if all of your egress traffic kept changing its path whenever an eBGP peer was having issues.


In the end, Step 10 does not necessarily prefer the *oldest* path, so much as it prefers a *current* path if a new path isÂ learned.

Of course, if the new path is actually desired, you can force traffic along that path by modifying some of the attributes in Step 1-9, but if all these attributes are identical then both paths will have an identical preference. BGP will therefore prefer the current, stable, âknown goodâ path instead of the newly learned, identically preferred path.

Step 10 assures that a new, identically preferred, path will not supersede a path that is already known. Step 10 does not prefer the oldest path as an absolute age, it simply prefers theÂ current path if a new one is learned.


Have you ever wondered why there is no Cisco command that can tell you the absolute age that a particular path to a prefix was learned (not a neighbor adjacency, but the specific age of a specific path advertised by a neighbor)? It is because that absolute age is not used anywhere, and therefore not tracked.

When a new path is learned and nothing in Steps 1-9 causes the new path to be more preferred, Step 10 assures the current best path remains marked as the best path.

When a best path is lost and BGP runs through the path selection process to elect a successor, all *remaining* paths to the prefix *already* exist in the BGP table. Therefore, they all have the *same* age and Step 10 cannot break the tie. Instead, Step 11 breaks the tie.


### Conclusion

BGPâs oldest path is often stated as âprefer the oldest pathâ, but as we’ve demonstrated, this isnât entirely accurate. A more accurate way of stating it would be â*when learning of a new path*, prefer the current (stable) path over the newly learned path (if nothing in step 1-9 explicitly implies the new path is more desirable).â

Awesome explanation in a very different way. Thank-you so much

If you could get some time to post for IGMP, Multicast it would be great help

Great job with the detailed explanation!

Possibly another way to present this behavior would be that BGP process does not maintain historical state information on the peers it already knows about. It is a stateless comparison between the ones already known and the one(s) newly learnt. This is being specific to the step 10 (Oldest peer). Hence it will always ends up preferring the “already” known and if there are more than one in the set known then it proceeds to the next step to select one form them.

Please post about the OSPF also.

This is such a brilliant article. Thank you!

Very good, thank you.

Excellent article.

is there a way to force a preferred path by using “clear IP bgp …” to prefer a different path if there is a tie break?

Brilliant article! Thanks very much for the efforts. Very well-explained.

can not see the BGP, OSPF, EIGRP videos

Hi Ed! How does Trial 2 differ from Trial 1? When you shut down the 9.22.11.2 peer, both remaining paths were also present in the topology table, so following this logic – there should be a tie, with step 11 as a tie-breaker.

That is exactly what happened. When the link to R2 was shut down, the remaining paths through R3 and R4 were tied since they were both present in the BGP table when R1 lost the path through R2. R1 used Step 11 (lowest Router-ID) to break the tie to select R3 as the next best path.

Often people consider that R3 was selected because the R3 path was learned before R4, meaning it had an older absolute age. But Trial 2 proves that wasn’t the case. I elaborate on it further in the “What Happened” section.

Hi Ed !

Can you publish a series on OSPF topics.

OSPF is on my list =)

If OSPF is on your list, Forwarding Address would be a great topic for discussion. Great Article on BGP.

Noted, thanks Dinesh =)

Hello Ed,

Can you publish BGP video series in Youtube.

Ed – this was excellent! Thanx

Hey Ed. How about make a video of such a beautiful content. I think this will be excellent addition for the networking fundamentals course
