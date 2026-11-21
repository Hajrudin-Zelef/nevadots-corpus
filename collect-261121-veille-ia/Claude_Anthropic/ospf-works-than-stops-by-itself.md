---
id: collect-261121-veille-ia/claude-anthropic/ospf-works-than-stops-by-itself
title: "OSPF works than STOPS by ITSELF"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["disabled", "ospf", "network", "backbone", "comment", "area", "address", "interface"]
source: https://forum.mikrotik.com/t/ospf-works-than-stops-by-itself/34964
source_anchor: ""
source_lines: [1, 285]
section: "Claude_Anthropic"
---
ok here is some backround.

I have 2 routers connected by a simple switch.

10.26.1.1 is backbone router. R1

10.26.1.3 is a wireless AP and PPPoE Server




R1 configuration v4.5

```
/ip address
add address=10.4.1.1/24 broadcast=10.4.1.255 comment="" disabled=no interface=HalkenSector2 network=10.4.1.0
add address=10.26.1.1/24 broadcast=10.26.1.255 comment="" disabled=no interface=HalkenSector1 network=10.26.1.0
network=95.0.133.252
```




```
/routing ospf instance
set default comment="" disabled=no distribute-default=always-as-type-2 in-filter=ospf-in metric-bgp=20 metric-connected=20 metric-default=1 metric-other-ospf=auto \
    metric-rip=20 metric-static=20 name=default out-filter=ospf-out redistribute-bgp=no redistribute-connected=as-type-2 redistribute-other-ospf=as-type-2 \
    redistribute-rip=no redistribute-static=as-type-2 router-id=0.0.0.0
/routing ospf area
set backbone area-id=0.0.0.0 comment="" disabled=no instance=default name=backbone type=default
/routing ospf area range
add advertise=yes area=backbone comment="" cost=calculated disabled=yes range=0.0.0.0/0
/routing ospf network
add area=backbone comment="" disabled=no network=10.26.1.0/24
add area=backbone comment="" disabled=no network=10.4.1.0/24
```

R2 conf v3.30 Routing-TEST

```
/ip address
add address=10.26.1.3/24 broadcast=10.26.1.255 comment="" disabled=no \
    interface=vlan1 network=10.26.1.0
```




```
/routing ospf instance
set default comment="" disabled=no distribute-default=always-as-type-2 \
    in-filter=ospf-in metric-bgp=20 metric-connected=20 metric-default=1 \
    metric-other-ospf=auto metric-rip=20 metric-static=20 name=default \
    out-filter=ospf-out redistribute-bgp=no redistribute-connected=as-type-2 \
    redistribute-other-ospf=as-type-2 redistribute-rip=no \
    redistribute-static=as-type-2 router-id=0.0.0.0
/routing ospf area
set backbone area-id=0.0.0.0 disabled=no instance=default name=backbone type=\
    default
/routing ospf interface
add authentication=none authentication-key="" authentication-key-id=1 cost=10 \
    dead-interval=40s disabled=yes hello-interval=10s instance-id=0 \
    interface=all network-type=ptmp passive=no priority=1 \
    retransmit-interval=5s transmit-delay=1s
add authentication=none authentication-key="" authentication-key-id=1 cost=10 \
    dead-interval=40s disabled=yes hello-interval=10s instance-id=0 \
    interface=wlan1 network-type=ptmp passive=no priority=1 \
    retransmit-interval=5s transmit-delay=1s
/routing ospf network
add area=backbone disabled=no network=10.26.1.0/24
add area=backbone disabled=yes network=0.0.0.0/0
add area=backbone disabled=yes network=95.0.133.0/24
```

OSPF stops working after a few hours. Neigbors are still active but no routes are received or imported.

I tried every possible scenario to solve this problem.

Any ideas ?

             
                
            
           
          
            
            
              Anyone out here ? MT team ?

             
            
           
          
            
            
              An OSPF loopback interface solved the problem. Thanks to MT support but they need to further investigate the issue if this is by design or is a serious bug.

             
            
           
          
            
            
              Did you try both routers with the same version?  If so were you able to duplicate the problem?

             
            
           
          
            
            
              I have created a loopback /32 bridge interface and assigned an IP address to it. Than assigned PPPoE Servers Local address the same IP address as loopback. Now it works and routes doesnt dissapear after some time like before.

As I said, I dont really know why it works this way because it is suggested to me by Mikrotik support after some months of struggling about this problem.

my previous PPPoE configuration was correct network wise either. But was loosing all the routes after 30mins or so.

             
            
           
          
            
            
              i’ve the same issue. Using 4.10, 3.30 mixed. Could you give me more specific configuration?

Regards

NoXy

             
            
           
          
            
            
              Bring your OS versions to the same level and things will improve.  Add in loopbacks and they will get even better.

             
            
           
          
            
            
              Sorry guys, but how do I do this loopbacks. someone can write me an example?

Many thanks

             
            
           
          
            
            
              To fake out a loopback interface just create a bridge interface and don’t add physical interfaces to it. Assign an IP to the bridge and it acts like a loopback.

             
            
           
          
            
            
              Thank you. I’ll try that too.

Sorry for my bad english.


             
            
           
          
            
            
              
Noxy;

If you are loosing routes in a PPPoE environment, you probably need to use the loopback method. I have described it above in detail.

             
            
           
          
            
            
              
I would ask others for their opinion on that statement, is it just a fix for a issue or a possible tweak?

             
            
           
          
            
            
              I note that the configuration at the top of this thread shows both router ids are 0.0.0.0. Its possible that 0.0.0.0 for the router id really means “pick one of my existing IP addresses, and use that as the router-id”, but i’m not sure if thats what happens with routeros or not; I always explicitly set a router-id.

Otherwise, assuming Halkensector1 and vlan1 are connected together, and I didn’t miss anything, I would say that configuration should work (and it was stated that it did for several hours); but best practice would be to add a loopback, and in ospfv2, use the loopback’s IP as the router id.

Loopbacks never go down, and give the router an always reachable and up IP address regardless of the state of its physical interfaces (reachable as long as you have some sort of connectivity to the router that is).

So, I guess my opinion on that statement is that it is a fix for an issue (it implies the ospf implementation is not bug free), and using the same routeros versions and loopbacks are less likely to trigger or manifest the bug(s). I’m not quite sure what you mean by tweak in this case.

             
            
           
          
            
            
              Whenever you have the local IP address of PPPoE Server Profile same with the local LAN, OSPF stops working after sometime.

So If you have OSPF in your network, the example in the wiki :

http://wiki.mikrotik.com/wiki/PPPoE

is WRONG.

**You need to set the local address of the PPPoE Profile to the loopback address.**

I dont know if this is by design or a bug but I hope they will mention or give a warning about it in the related wiki page.

             
            
           
          
            
            
              Are you saying that adding a bridge, with no physical interfaces linked to it, and adding an ip address on that bridge, solves this issue ?

What would you suggest that the router id might be: the ip on the recently created bridge ?

And also, would the subnet on the bridge added to the ospf/networks list ?

I would appreciate an answer from someone who did it !

Thank you in advance ! 

             
            
           
          
            
            
              
The IP you add on the loopback should be a /32 (single IP, not a range). You then use thes the router-id is OSPF. Because the IP is on a loopback/bridge, it never “goes down” and so the OSPF process is more stable.

             
            
           
          
            
            
              K. 'll try that today, asap.

             
            
           
          
            
            
              

It works !



Add /32 on bridge, all ok.

             
            
           
          
            
            
              
Not sure if my recent issue was directly OSPF or a bug with 4.17 using virtual AP,

Each AP’s are using OSPF servers to clients, on a routed network, works OK but when i setup a Virtual AP to any of the sector AP’s, on a ping test beyond the AP after 10pings it would drop for 4 and then resume - OK for 10 and drop for 4,etc,

There no dropouts when pinging from client to VAP,



The Ap’s using 4.17 (433AH) and connected to a 493AH (also was 4.17) using a /28

I simply downgraded the 493AH from 4.17 to 3.30 and problem solved, so for now any MT device not using NV2 is downgraded to 3.30 and works much stable for now.

I am going against the logic of *"…using the same* routeros versions and loopbacks are less likely to trigger or manifest the bug(s)… "
