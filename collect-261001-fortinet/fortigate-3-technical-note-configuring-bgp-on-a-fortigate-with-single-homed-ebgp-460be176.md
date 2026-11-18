**Purpose**

This article describes the steps to configure FortiGates in a BGP scenario which involves iBGP, eBGP peering, OSPF as IGP for the Customer network, and an access-list to filter routes in.

It includes the network diagram, requirements, configuration, and verification steps for all FortiGates used in this example.

**Scope**

- Fortigate or VDOMs in NAT mode only
- FortiOS v3.0

**Diagram**

The following network diagram illustrates this case study:

**Expectations, Requirements**

- FGT-ISP acts as the ISP router ; it advertises to FGT-1 all BGP routes it does learn from the Internet (In this example, only 1.0.0.0/8 and 2.0.0.0/8 are used as routes advertised by the ISP).
- FGT-1 and FGT-2 learn all BGP routes advertised by the ISP’s router FGT-ISP.
- iBGP peering is done via loopback interfaces (*).
- The ISP should only learn the DMZ network from the Customer, 10.160.0.0/23.
- Customer uses OSPF as its IGP to advertise the loopback interface internally and allow BGP peering

Note: all requirements, addresses and network information above are only given as examples for this case study.



**(*) USING LOOPBACK INTERFACES**

The use of loopback interfaces into this BGP/OSPF design is a simple rule that eases the OSPF troubleshooting and administration, where we will set the OSPF router ID identically to the loopback interface.

It is also used within the Customer network for the iBGP peering.

Using loopback interface allows having an IP address that does not depend on a physical interface, hence is always up.

Note that using a loopback interfaces requires the configuration of appropriate firewall policies that allow traffic to it.


To enable OSPF on an interface, you only need to add it into the prefix list. To enable all interfaces with a single setting; you can use the prefix “0.0.0.0 0.0.0.0”

**Configuration**

 **Only the relevant configuration parts to this case study are given.**




Note : the access-list and route-map above are used to match the requirement that the ISP should only learn the DMZ network from the Customer, 10.160.0.0/23.






**Verification**

The verification steps will show the BGP and OSPF neighbours’ state and the routes in the routing table. The following commands will be used:

 **FGT_ # get router info bgp summary**

**FGT_ # get router info bgp neighbors**

**FGT_ # get router info bgp network**

**FGT_ # get router info routing-table all** 

**FGT_ # get router info ospf neighbor**



Note : Further information about OSPF can be found in related articles.




**FGT_ISP # get router info bgp summary**

BGP router identifier 10.0.0.1, local AS number 1

BGP table version is 2

2 BGP AS-PATH entries

0 BGP community entries


Neighbor V AS MsgRcvd MsgSent TblVer InQ OutQ Up/Down State/PfxRcd

10.142.0.74 4 1000 4528 4546 1 0 0 2d18h02m 1


Total number of neighbors 1


**FGT_ISP # get router info bgp neighbors**

BGP neighbor is 10.142.0.74, remote AS 1000, local AS 1, external link

BGP version 4, remote router ID 10.0.0.2

BGP state = Established, up for 2d18h02m

Last read 00:00:10, hold time is 180, keepalive interval is 60 seconds

Configured hold time is 180, keepalive interval is 60 seconds

Neighbor capabilities:

Route refresh: advertised and received (old and new)

Address family IPv4 Unicast: advertised and received

Received 4529 messages, 0 notifications, 0 in queue

Sent 4547 messages, 0 notifications, 0 in queue

Route refresh request: received 0, sent 0

Minimum time between advertisement runs is 30 seconds


For address family: IPv4 Unicast

BGP table version 2, neighbor version 1

Index 1, Offset 0, Mask 0x2

Community attribute sent to this neighbor (both)

Inbound path policy configured

Route map for incoming advertisements is *1root

1 accepted prefixes

3 announced prefixes


Connections established 1; dropped 0

Local host: 10.142.0.110, Local port: 11663

Foreign host: 10.142.0.74, Foreign port: 179

Nexthop: 10.142.0.110


**FGT_ISP # get router info bgp network**

BGP table version is 2, local router ID is 10.0.0.1

Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,

S Stale

Origin codes: i - IGP, e - EGP, ? - incomplete


Network Next Hop Metric LocPrf Weight Path

*> 0.0.0.0/0 192.168.183.254 32768 ?

*> 1.0.0.0 192.168.183.254 32768 ?

*> 2.0.0.0 192.168.183.254 32768 ?

*> 10.160.0.0/23 10.142.0.74 0 0 1000 ?


Total number of prefixes 4


**FGT_ISP # get router info routing-table all**

Codes: K - kernel, C - connected, S - static, R - RIP, B - BGP

O - OSPF, IA - OSPF inter area

N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2

E1 - OSPF external type 1, E2 - OSPF external type 2

i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area

* - candidate default


S* 0.0.0.0/0 [10/0] via 192.168.183.254, port2

S 1.0.0.0/8 [10/0] via 192.168.183.254, port2

S 2.0.0.0/8 [10/0] via 192.168.183.254, port2

C 10.142.0.0/23 is directly connected, port3

B 10.160.0.0/23 [20/0] via 10.142.0.74, port3, 2d18h02m

C 192.168.182.0/23 is directly connected, port2







**FGT-1 # get router info bgp summary**

BGP router identifier 10.0.0.2, local AS number 1000

BGP table version is 9

2 BGP AS-PATH entries

0 BGP community entries


Neighbor V AS MsgRcvd MsgSent TblVer InQ OutQ Up/Down State/PfxRcd

10.0.0.3 4 1000 4558 4562 8 0 0 2d18h25m 0

10.142.0.110 4 1 4594 4573 7 0 0 2d17h56m 3


Total number of neighbors 2



**FGT-1 # get router info bgp neighbors**

BGP neighbor is 10.0.0.3, remote AS 1000, local AS 1000, internal link

BGP version 4, remote router ID 10.0.0.3

BGP state = Established, up for 2d18h26m

Last read 00:00:04, hold time is 180, keepalive interval is 60 seconds

Configured hold time is 180, keepalive interval is 60 seconds

Neighbor capabilities:

Route refresh: advertised and received (old and new)

Address family IPv4 Unicast: advertised and received

Received 4559 messages, 0 notifications, 0 in queue

Sent 4562 messages, 0 notifications, 0 in queue

Route refresh request: received 0, sent 0

Minimum time between advertisement runs is 30 seconds

Update source is loopback


For address family: IPv4 Unicast

BGP table version 9, neighbor version 8

Index 2, Offset 0, Mask 0x4

Community attribute sent to this neighbor (both)

0 accepted prefixes

6 announced prefixes


Connections established 1; dropped 0

Local host: 10.0.0.2, Local port: 1101

Foreign host: 10.0.0.3, Foreign port: 179

Nexthop: 10.0.0.2


BGP neighbor is 10.142.0.110, remote AS 1, local AS 1000, external link

BGP version 4, remote router ID 10.0.0.1

BGP state = Established, up for 2d17h57m

Last read 00:00:15, hold time is 180, keepalive interval is 60 seconds

Configured hold time is 180, keepalive interval is 60 seconds

Neighbor capabilities:

Route refresh: advertised and received (old and new)

Address family IPv4 Unicast: advertised and received

Received 4595 messages, 0 notifications, 0 in queue

Sent 4574 messages, 0 notifications, 0 in queue

Route refresh request: received 0, sent 0

Minimum time between advertisement runs is 30 seconds


For address family: IPv4 Unicast

BGP table version 9, neighbor version 7

Index 1, Offset 0, Mask 0x2

Community attribute sent to this neighbor (both)

3 accepted prefixes

3 announced prefixes


Connections established 2; dropped 1

Local host: 10.142.0.74, Local port: 179

Foreign host: 10.142.0.110, Foreign port: 11663

Nexthop: 10.142.0.74



**FGT-1 # get router info bgp network**

BGP table version is 9, local router ID is 10.0.0.2

Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,

S Stale

Origin codes: i - IGP, e - EGP, ? - incomplete


Network Next Hop Metric LocPrf Weight Path

*> 0.0.0.0/0 10.142.0.110 0 0 1 ?

*> 1.0.0.0 10.142.0.110 0 0 1 ?

*> 2.0.0.0 10.142.0.110 0 0 1 ?

*> 10.0.0.2/32 0.0.0.0 32768 ?

*> 10.142.0.0/23 0.0.0.0 32768 ?

*> 10.160.0.0/23 0.0.0.0 32768 ?

* 0.0.0.0 100 32768 i


Total number of prefixes 6



**FGT-1 # get router info ospf neighbor**


OSPF process 0:

Neighbor ID Pri State Dead Time Address Interface

10.0.0.3 1 Full/DR 00:00:30 10.160.0.75 dmz



**FGT-1 # get router info routing-table all**

Codes: K - kernel, C - connected, S - static, R - RIP, B - BGP

O - OSPF, IA - OSPF inter area

N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2

E1 - OSPF external type 1, E2 - OSPF external type 2

i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area

* - candidate default


B* 0.0.0.0/0 [20/0] via 10.142.0.110, wan1, 2d17h58m

B 1.0.0.0/8 [20/0] via 10.142.0.110, wan1, 2d17h58m

B 2.0.0.0/8 [20/0] via 10.142.0.110, wan1, 2d17h58m

S 10.0.0.1/32 [10/0] via 10.142.0.110, wan1

C 10.0.0.2/32 is directly connected, loopback

O 10.0.0.3/32 [110/110] via 10.160.0.75, dmz, 2d18h18m

C 10.142.0.0/23 is directly connected, wan1

O 10.143.0.0/23 [110/20] via 10.160.0.75, dmz, 2d18h18m

C 10.160.0.0/23 is directly connected, dmz




**FGT-2 # get router info bgp summary**

BGP router identifier 10.0.0.3, local AS number 1000

BGP table version is 1

2 BGP AS-PATH entries

0 BGP community entries


Neighbor V AS MsgRcvd MsgSent TblVer InQ OutQ Up/Down State/PfxRcd

10.0.0.2 4 1000 4555 4563 0 0 0 2d18h28m 6


Total number of neighbors 1



**FGT-2 # get router info bgp neighbors**

BGP neighbor is 10.0.0.2, remote AS 1000, local AS 1000, internal link

BGP version 4, remote router ID 10.0.0.2

BGP state = Established, up for 2d18h28m

Last read 00:00:06, hold time is 180, keepalive interval is 60 seconds

Configured hold time is 180, keepalive interval is 60 seconds

Neighbor capabilities:

Route refresh: advertised and received (old and new)

Address family IPv4 Unicast: advertised and received

Received 4556 messages, 0 notifications, 0 in queue

Sent 4563 messages, 0 notifications, 0 in queue

Route refresh request: received 0, sent 0

Minimum time between advertisement runs is 30 seconds

Update source is loopback


For address family: IPv4 Unicast

BGP table version 1, neighbor version 0

Index 1, Offset 0, Mask 0x2

Community attribute sent to this neighbor (both)

6 accepted prefixes

0 announced prefixes


Connections established 1; dropped 0

Local host: 10.0.0.3, Local port: 179

Foreign host: 10.0.0.2, Foreign port: 1101

Nexthop: 10.0.0.3



**FGT-2 # get router info bgp network**

BGP table version is 1, local router ID is 10.0.0.3

Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,

S Stale

Origin codes: i - IGP, e - EGP, ? - incomplete


Network Next Hop Metric LocPrf Weight Path

*>i0.0.0.0/0 10.142.0.110 0 100 0 1 ?

*>i1.0.0.0 10.142.0.110 0 100 0 1 ?

*>i2.0.0.0 10.142.0.110 0 100 0 1 ?

*>i10.0.0.2/32 10.0.0.2 0 100 0 ?

*>i10.142.0.0/23 10.0.0.2 0 100 0 ?

*>i10.160.0.0/23 10.0.0.2 0 100 0 ?


Total number of prefixes 6



**FGT-2 # get router info ospf neighbor**


OSPF process 0:

Neighbor ID Pri State Dead Time Address Interface

10.0.0.2 1 Full/Backup 00:00:38 10.160.0.74 dmz




**FGT-2 # get router info routing-table all**

Codes: K - kernel, C - connected, S - static, R - RIP, B - BGP

O - OSPF, IA - OSPF inter area

N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2

E1 - OSPF external type 1, E2 - OSPF external type 2

i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area

* - candidate default


B* 0.0.0.0/0 [200/0] via 10.142.0.110 (recursive via 10.160.0.74), 2d18h00m

B 1.0.0.0/8 [200/0] via 10.142.0.110 (recursive via 10.160.0.74), 2d18h00m

B 2.0.0.0/8 [200/0] via 10.142.0.110 (recursive via 10.160.0.74), 2d18h00m

O 10.0.0.2/32 [110/110] via 10.160.0.74, dmz, 2d18h20m

C 10.0.0.3/32 is directly connected, loopback

O 10.142.0.0/23 [110/20] via 10.160.0.74, dmz, 2d18h20m

C 10.143.0.0/23 is directly connected, internal

C 10.160.0.0/23 is directly connected, dmz

**Troubleshooting**

 “get router info bgp <subcommand>”, where subcommand can be :


cidr-only                  display routes with non-natural netmasks

community display routes matching the communities

community-info list all bgp community information

community-list display routes matching the community-list

dampening display router dampening infomation

filter-list display routes conforming to the filter-list

inconsistent-as display routes with inconsistent AS Paths

neighbors show BGP neighbors

network show BGP info for network

network-longer-prefixes show BGP info for route and more specific routes

paths path information

prefix-list display routes conforming to the prefix-list

regexp display routes matching the AS path regular expression

quote-regexp display routes matching the AS path regular expression

route-map display routes conforming to the route-map

scan display BGP scan status

summary summary of BGP neighbor status

memory                     BGP memory table



 FGT# diag ip router bgp all enable (or “disable” to stop the trace)

And the CLI sniffer (see related article for using the sniffer)

**Related Articles**