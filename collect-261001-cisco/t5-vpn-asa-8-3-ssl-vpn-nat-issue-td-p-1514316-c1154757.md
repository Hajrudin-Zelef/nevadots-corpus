- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-22-2010 01:38 PM
Need help in figuring out how to setup anyconnect VPN with VPN client NATed into internal network.
There're a lot articles about opposite - how to disable NAT for vpn pool.
I need to create VPN gateway to complex interna lnetwork, vpnpool is out of regular subnet range of that network, so it'll be routing issues witout NAT.
So I need vpn clients connected to <outside> to be PATed to <inside>. The problem is that there's also dynamic PAT rule from <inside> to <outside> for regular Iternet acccess which results in "Asymetric NAT rules..." error.
Creating different Twice NAT rules and moving them on top/bottom doesn't make any difference. There're also some hidden rules from vpn setup which couldn't be seen.
v8.3 seems is trying to destroy confidence in Cisco firewalls...
Thank you.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			VPN
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 09:04 AM
Stan,
Something like this works for me.
192.168.0.0/24 --- Router -- 172.16.0.0/24 ----ASA ==== cloud ==== Host. (inside the tunnel it get IP address from "over" pool, which is also Connected on inside)
bsns-asa5520-10(config)# clear xlate
INFO: 762 xlates deleted
bsns-asa5520-10(config)# sh run nat
nat (inside,outside) source static any any destination static SHARED SHARED
!
nat (inside,outside) after-auto source dynamic any interface
bsns-asa5520-10(config)# sh run object network
object network LOCAL_NETWORK
 subnet 192.168.0.0 255.255.255.0
object network SHARED
 subnet 172.16.0.0 255.255.255.0
bsns-asa5520-10(config)# sh run ip local pool
ip local pool ANY 10.0.0.100-10.0.0.200
ip local pool OVER 172.16.0.100-172.16.0.155
bsns-asa5520-10(config)# sh run tunne
bsns-asa5520-10(config)# sh run tunnel-group
tunnel-group DefaultWEBVPNGroup general-attributes
 address-pool OVER
If I catch your drift ... bridging inside and outside is not really needed on Cisco equipment as it should work via proxy arp straight out of the box, but I'm not faimilar with neither of vendors' solution for remote access.
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-22-2010 03:28 PM
Stan,
Show us exactly what you've tried.
auto-nat from outside to inside with to do dyanmic PAT to interface should be OK.
You might need to indeed add twice NAT to traffic from inside subnets to said PATed (or NATed IPs).
As I said that should work in theory.
Let's see what you tried and on what version and we'll start from there.
Marcin
P.S. Regarding assimetric NAT I believe either Jay or Rama published an article recently.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-22-2010 07:21 PM
I attached config file (a little bit reduced, cut off some unnessessary details).
In short:
VPN SSL is enabled on 
"nat (Collab,Outside) after-auto source dynamic..." creates regular access from Collab to the Internet (outside)
"nat (Collab,Outside) after-auto source static..." creates NAT exemption for vpnpool, so that vpn client can access hosts on Collab
In this variant everything is Ok, as far as I'm not going further than directly copnnected subnets.
The point is that I have several subnets in a cloud behind Collab and those routers are not aware of vpnpool subnet, i.e. no routing back to vpn client.
To make a trick I need vpn client's packets to be PATed to Collab.
If I disable "nat (Collab,Outside) after-auto source static..." and add
nat (Outside,Collab) 1 source dynamic vpn-pools-group interface destination static Collab-netwok-group Collab-netwok-group
or this one
nat (Outside,Collab) 1 source static vpn-pools-group interface destination static Collab-netwok-group Collab-netwok-group
it immediately stops working (notorious "Asymetric NAT rule" error)
Another thing is that vpn traffic originates from 
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 03:35 AM
Stan,
Here's the way I got it working.
object network Anyconnect
 nat (outside,inside) dynamic interface
object network LOCAL_NET_N
 nat (inside,outside) static LOCAL_NET_N
object network LOCAL_NET_M
 nat (inside,outside) static LOCAL_NET_M
nat (inside,outside) after-auto source dynamic any interface
sh run object
object network Anyconnect
 subnet 10.0.0.0 255.255.255.0
object network LOCAL
object network LOCAL_NET_N
 subnet 192.168.0.0 255.255.255.0
object network LOCAL_NET_M
 subnet 172.16.0.0 255.255.255.0
Based on outside NAT from here:
http://www.cisco.com/en/US/docs/security/asa/asa83/upgrading/migrating.html
If I'll find the time I'll try to optimize it, but cannot promise.
Marcin
edit: Configurtion done on 8.3.2
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 12:17 PM
Sorry, not working.
# sh ru object
object network vpn-pool-Outside
 subnet 192.168.110.0 255.255.255.0
object network inside-network
 subnet 10.137.0.0 255.255.0.0
# sh nat
Auto NAT Policies (Section 2)
1 (inside) to (outside) source static inside-network inside-network
    translate_hits = 19, untranslate_hits = 5
2 (outside) to (inside) source dynamic vpn-pool-Outside interface
    translate_hits = 9, untranslate_hits = 0
Manual NAT Policies (Section 3)
1 (inside) to (outside) source dynamic any interface
    translate_hits = 0, untranslate_hits = 0
Once I put auto NAT 1 vpn client can access inside-network but everyone from inside cannot access the Internet.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 01:11 PM
Stan,
Please note that this is not EXACTLY what you're looking for (note the static identity)
Possible differences:
- did you clear xlates ? Note that I get both translate and untranstale hits.
- ASA version?
Tomorrow I'll dig into this properly.
Marcin
From my setup:
bsns-asa5520-10# sh run nat
!
object network Anyconnect
 nat (outside,inside) dynamic interface
object network LOCAL_NET_N
 nat (inside,outside) static LOCAL_NET_N
object network LOCAL_NET_M
 nat (inside,outside) static LOCAL_NET_M
!
nat (inside,outside) after-auto source dynamic any interface
bsns-asa5520-10# sh run obj
bsns-asa5520-10# sh run object
object network Anyconnect
 subnet 10.0.0.0 255.255.255.0
object network LOCAL
object network LOCAL_NET_N
 subnet 192.168.0.0 255.255.255.0
object network LOCAL_NET_M
 subnet 172.16.0.0 255.255.255.0
bsns-asa5520-10# sh nat
Auto NAT Policies (Section 2)
1 (inside) to (outside) source static LOCAL_NET_M LOCAL_NET_M
    translate_hits = 3, untranslate_hits = 3
2 (inside) to (outside) source static LOCAL_NET_N LOCAL_NET_N
    translate_hits = 3, untranslate_hits = 3
3 (outside) to (inside) source dynamic Anyconnect interface
    translate_hits = 7, untranslate_hits = 7
Manual NAT Policies (Section 3)
1 (inside) to (outside) source dynamic any interface
    translate_hits = 156438, untranslate_hits = 110871
bsns-asa5520-10#
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 01:38 PM
Yes, I cleared xlate, though it can confuse only by keeping old nat translations, i.e. you'll have an access which shouldn't be there, and I'm speaking of loosing connection immediately once I put static NAT from inside to outside (which is practically NAT exemption).
Show version:
-----------------------
Cisco Adaptive Security Appliance Software Version 8.3(2)
Device Manager Version 6.3(3)
Compiled on Fri 30-Jul-10 17:49 by builders
System image file is "disk0:/asa832-k8.bin"
Config file at boot was "startup-config"
CS-FWA up 1 day 6 hours
failover cluster up 1 day 6 hours
Hardware:   ASA5510, 1024 MB RAM, CPU Pentium 4 Celeron 1599 MHz
Internal ATA Compact Flash, 256MB
BIOS Flash M50FW016 @ 0xfff00000, 2048KB
-----------------------
You can even simplify config using only one subnet inside.
Maybe I didn't put it clear, I need just to do this:
SSL VPN client terminated on outside interface should be able to access inside network and be dynamically NATed there through inside interface (all remote access vpn setup examples are speaking about EXEMPTING vpn address pool from NAT, I need the opposite).
At the same time all inside network should be able to access the Internet by dynamic NAT through outside interface.
I'm not sure this is possible though, haven't done that before with Cisco VPN.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 01:50 PM
Stan,
I know what you want to do, that's why I said the lines I gave above are NOT EXACTLY what you're looking for and will lab it out tomorrow.
Marcin
edit: I'm curious how that would work in case of pre 8.3
Anyhoo if I'll have the time I'll check it.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-23-2010 03:06 PM
Ok,
Thanks a lot.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 02:41 AM
Stan,
First of all I found the solution dirty but it's working for me.
I would say, it's better to assign routble IP addresses to your RA clients and not bother with solution like this.
The NAT RPF checks are going to be addressed soon (redesign to an extent) in new code (I cannot give exact ETA now).
Here's what our local 8.3 NAT wizard come up with to cheat RPF check.
bsns-asa5520-10(config)# sh run nat
nat (outside,inside) source dynamic Anyconnect interface destination static LOCAL_NET_M LOCAL_NET_M
nat (outside,inside) source dynamic Anyconnect interface destination static LOCAL_NET_N LOCAL_NET_N
nat (inside,outside) source static LOCAL_NET_M LOCAL_NET_M destination static interface Anyconnect
nat (inside,outside) source dynamic any interface
bsns-asa5520-10(config)# nat (inside,outside) source static LOCAL_NET_N LOCAL_$
WARNING: All traffic destined to the IP address of the inside interface is being redirected.
WARNING: Users may not be able to access any service enabled on the inside interface.
bsns-asa5520-10(config)#
bsns-asa5520-10(config)#
bsns-asa5520-10(config)# sh run nat
nat (outside,inside) source dynamic Anyconnect interface destination static LOCAL_NET_M LOCAL_NET_M
nat (outside,inside) source dynamic Anyconnect interface destination static LOCAL_NET_N LOCAL_NET_N
nat (inside,outside) source static LOCAL_NET_M LOCAL_NET_M destination static interface Anyconnect
nat (inside,outside) source dynamic any interface
nat (inside,outside) source static LOCAL_NET_N LOCAL_NET_N destination static interface Anyconnect
bsns-asa5520-10(config)# sh run obj
bsns-asa5520-10(config)# sh run object
object network Anyconnect
 subnet 10.0.0.0 255.255.255.0
object network LOCAL_NET_N
 subnet 192.168.0.0 255.255.255.0
object network LOCAL_NET_M
 subnet 172.16.0.0 255.255.255.0
object network ALL
 subnet 0.0.0.0 0.0.0.0
object network INTERFACE
 host 172.16.0.1
bsns-asa5520-10(config)# sh ip
System IP Addresses:
Interface                Name                   IP address      Subnet mask     Method
GigabitEthernet0/0       outside                10.48.66.237    255.255.254.0   CONFIG
GigabitEthernet0/1       inside                 172.16.0.1      255.255.255.0   CONFIG
Current IP Addresses:
Interface                Name                   IP address      Subnet mask     Method
GigabitEthernet0/0       outside                10.48.66.237    255.255.254.0   CONFIG
GigabitEthernet0/1       inside                 172.16.0.1      255.255.255.0   CONFIG
bsns-asa5520-10(config)#                                                                                          
bsns-asa5520-10(config)# sh nat
Manual NAT Policies (Section 1)
1 (outside) to (inside) source dynamic Anyconnect interface destination static LOCAL_NET_M LOCAL_NET_M
    translate_hits = 28, untranslate_hits = 3
2 (outside) to (inside) source dynamic Anyconnect interface destination static LOCAL_NET_N LOCAL_NET_N
    translate_hits = 15, untranslate_hits = 0
3 (inside) to (outside) source static LOCAL_NET_M LOCAL_NET_M destination static interface Anyconnect
    translate_hits = 3, untranslate_hits = 3
4 (inside) to (outside) source dynamic any interface
    translate_hits = 2712, untranslate_hits = 1165
5 (inside) to (outside) source static LOCAL_NET_N LOCAL_NET_N destination static interface Anyconnect
    translate_hits = 1, untranslate_hits = 1
HTH,
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 06:17 AM
Well, your' right, working but showing "lack of elegance"
In other words, if I have 255 subnets behind vpn gateway I need to create 510 NAT rules.
Maybe it's better to try to terminate VPN on another subinterface, or this is also impossible?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 06:44 AM
Stan,
First of all it's poor design choice to mask those users, since you can choose any pool you like
Coming back to "solution" ...
you don't need so many rules, you can pack things into object groups to some extent.
I wouldn't give up on 8.3 NAT just yet, it has it's shortcomings but they will be addressed as market depends it 
Regarding terminating SSL on different interface - usual rule applied. Routing to clients need to point out through the interface you're terminating the clients on.
Maybe you can ellaborate a bit more about why you're trying to PAT users instead of giving them a new routable subnet, or statically NATing client subnet to something else? I might come up with a more elegant (as you put it) solution?
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 07:18 AM
>> First of all it's poor design choice to mask those users, since you can choose any pool you like
Absolutely agree, but I don't want to amend routing design of existing network (and it's MPLS network and a big one). If I can for ex. choose vpn pool from inside network range (as I can do with others vpn solutions) that will be the answer.
>> Regarding terminating SSL on different interface...
The difference is that this interface won't be included in (inside, outside) dynamic NAT rule and therefore it will work.
Or even better, I think, if I can terminate vpn on inside interface and choose vpn pool range from inside LAN IPs.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 08:05 AM
Stan,
Why not usre part of the pool assigned to "inside"? It should indeed work as any other vpn solution in that regard.
Regarding termination on different interface. There is no problem to do this if routing allows, ie. you cannot have users coming in through outside interface being terminated on inside interface, that will just not work to the best of my knowledge and has never worked.
Client "external" IP addresses need to be reachable via inetrface you're terminating them on. ie. for that default route pointing to the outside is good enough if you're terminating on the outside.
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-24-2010 08:21 AM
>>> Why not usre part of the pool assigned to "inside"?
You mean just to use IPs from inside network range withing the same subnet? It's not working for me, I think because this address pool is linked to outside interface and thus no routing to this IP range from inside to outside, because they are supposed to be on the (inside). If you mean subnetting inside network then on one hand it's not possible, on the other hand I'll stuck with the same routing issue. The only answer could be choosing bridging to vpn pool which I don't know how to do with ASA and can be easily achieved with MS or old Baynetworks stuff.