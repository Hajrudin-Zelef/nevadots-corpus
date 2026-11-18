- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 05:54 AM
FOr one of our customer , they have two router one in each DC
The router in DC1 is connected to COre Switch in DC1 >> This is a Layer 2 connection
Similarly router in DC2 is connected to Core switch in DC 2 ( again L2 connection)
The DCs are connected over a Physical underground link .
Between Router 1 and Router 2 , we have defined a VLAN 11 where we run OSPF . BGP is also configured between two routers.
They use BGP peer Group . I want to understand how the failover will work in case ISP link goes down .
Both the router have a default route pointing to their respective ISP .
In OspF it have defined the redistribute connected and static plus network address for4 subnets also .
Below is the config
interface Port-channel1.11
encapsulation dot1Q 11
ip address 10.2.2.2 255.255.255.248
ip ospf message-digest-key 1 md5 7 XXXXXXXXXXXXXXX
!
router ospf 11
router-id 10.2.2.10
area 0 authentication message-digest
redistribute connected subnets
redistribute static subnets
passive-interface default
no passive-interface Port-channel1.11
network 10.2.2.0 0.0.0.255 area 0
network 10.2.3.0 0.0.0.255 area 0
network 10.2.4.0 0.0.0.255 area 0
network 10.2.5.0 0.0.0.255 area 0
!
router bgp 27272
bgp router-id 10.2.2.10
bgp log-neighbor-changes
bgp graceful-restart
bgp maxas-limit 100
timers bgp 10 30
neighbor TEST_GROUP peer-group
neighbor TEST_GROUP remote-as 27272
neighbor TEST_GROUP password 7 XXXXXXXXXXXXXXXXXX
neighbor TEST_GROUP update-source Loopback0
neighbor 10.2.2.11 peer-group TEST_GROUP   >>>> this IS router 2 loop back address in DC2
neighbor 22.2.22.12 remote-as XXXXX
neighbor 22.2.22.12 description ISP
neighbor 22.2.22.12 password 7 XXXX
!
address-family ipv4
network 10.2.2.0 mask 255.255.255.0 route-map INTERNAL-ROUTES
network 10.2.3.0 mask 255.255.255.0 route-map INTERNAL-ROUTES
network 10.2.4.0 mask 255.255.255.0 route-map INTERNAL-ROUTES
network 10.2.5.0 mask 255.255.255.0 route-map INTERNAL-ROUTES
neighbor TEST_GROUP send-community both
neighbor TEST_GROUP next-hop-self
neighbor TEST_GROUP soft-reconfiguration inbound
neighbor 10.2.2.11 activate
neighbor 22.2.22.12 activate
neighbor 22.2.22.12 send-community both
neighbor 22.2.22.12 prefix-list FILTER-OUT out
exit-address-family
ip route 0.0.0.0 0.0.0.0 22.2.22.12
ip route 10.2.2.0 255.255.255.0 Null0
ip route 10.2.3.0 255.255.255.0 10.2.3.254
ip route 10.2.4.0 255.255.255.0 Null0
ip route 10.2.5.0 255.255.255.0 Null0
ip prefix-list FILTER-OUT seq 10 permit 10.2.2.0/24
ip prefix-list FILTER-OUT seq 20 permit 10.2.3.0/24
ip prefix-list FILTER-OUT seq 30 permit 10.2.4.0/24
ip prefix-list FILTER-OUT seq 40 permit 10.2.5.0/24
interface Loopback0
description Loopback OSPFBGP
ip address 10.2.2.10 255.255.255.255
!
interface Port-channel1.14
encapsulation dot1Q 14
ip address 10.2.3.226 255.255.255.248
glbp 1 ip 10.2.3.227
glbp 1 priority 200
no glbp 1 load-balancing
glbp 1 authentication md5 key-chain glbp
interface GigabitEthernet0/0/4
description ISP
ip address 22.2.22.11 255.255.255.252
no negotiation auto
!
!
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Routing Protocols
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-27-2020 03:22 PM
There are several things I would like to address. First you ask this question "Cant we use simply the VLAN 15 address as loopback." Perhaps it is simply a confusion about terminology. A loopback is a particular type of virtual interface. A vlan interface is a particular type of virtual interface. And they are not interchangeable. A vlan interface can not be a loopback and a loopback interface can not be a vlan.
Then let me address this statement that you make "OSPF is having two routes - the vlan 15 interface and ISP address" This is not correct. It is not the vlan 15 address that is advertised but is the loopback interface address that is used. (note that the loopback interface address is used both as the OSPF Router ID and used as the IBGP peer address.
Then let me try to clarify my statement about OSPF and IBGP. Looking into the configuration of IBGP we find that one router defines the IBGP neighbor as 10.2.52.240. This address is the loopback interface address of the peer router. To form the IBGP peer relationship the router must know how to reach the peer address. So how does this router know how to reach 10.2.52.240? That address is advertised by OSPF. So this is the basis of my statement about OSPF and IBGP. If OSPF did not advertise that address then IBGP would not work. So OSPF is necessary for this implementation of IBGP.
It does not have to be done this way. I suggest an alternative: configure IBGP so that the IBGP neighbor address is the vlan 15 address of the peer router (rather than as the loopback address). If the neighbor address is the vlan 15 address then the router knows how to reach that address (it is in a directly connected subnet) and there is no need for OSPF.
This is one example of making things more complex than they need to be. There are some good reasons why you might choose to make the IBGP neighbor address be on a loopback interface. But those reasons are not present in this network. By changing the IBGP neighbor address to the vlan interface rather than the loopback interface we remove the requirement for OSPF and make the configuration more simple.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-28-2020 09:36 AM
You are welcome. It has been a long discussion and I am glad that our explanations have been helpful. This community is an excellent place to ask questions and to learn about networking. I hope to see you continue to be active in the community.
I tried to explain about BGP neighbors using loopback addresses or not in a previous response. It seems that was not so clear so let me try again from a slightly different perspective. Let us think about 2 routers, routerA and routerB who want to become BGP neighbors (it might be IBGP or it might be EBGP, same things apply to both). Let us assume that the routers are connected using vlan 15. The simple way is for both routers in their bgp neighbor commands to use the vlan 15 interface IP address. When it is configured this way there is no need for any other routing protocol to be running on the routers. The routers are attempting to access the neighbor on a connected subnet and all they need to do is to arp for each other. When this is configured the BGP neighbor relationship will be negotiated and they become active neighbors and remain active neighbors as long as vlan 15 continues to work. But if something happens on vlan 15 and it stops working then the BGP neighbor relationship is terminated.
That is the more simple approach for BGP neighbors (to use directly connected subnet addresses as neighbor address). But let us think about routerA and routerB and what might happen if both routers are also connected using vlan 10 (in addition to vlan 15). If the routers continue to use the subnet address of vlan 15 as the neighbor address then they are dependent on vlan 15 to maintain the neighbor relationship. But if the routers were to use some IP address on the other router that was reachable using either vlan 10 or vlan 15 then the routers are not dependent on a single connection and can take advantage of the redundancy of their connections. A loopback interface address is frequently used for this.
So using a loopback interface address as the BGP neighbor address takes advantage of possible redundant connections. Since the routers are now using an address that is not directly connected they can no longer simply arp for the neighbor address but must have some routing information about how to reach the neighbor. It might use some routing protocol like OSPF or might use something simple like static routes.
So configuring BGP neighbor statements with loopback interfaces is more complicated but takes advantage of potential redundancy. And configuring BGP neighbor statements with a connected subnet address is more simple, but does not provide redundancy. It is a choice to be made when the network design is being done.
So let us now think about your discussion. Your routers were configured with loopback interface addresses in the BGP neighbor statements, which suggests redundancy. And OSPF was being used and the loopback interface addresses were being advertised which also suggests redundancy. But you were using only vlan 15. It was quite clear that in the network design of your network that vlan 14 was for inside traffic and vlan 15 was for outside traffic (like BGP). So you had the more complex configuration (loopback interfaces and OSPF) but were using only a single connection. So my advice was to use the more simple configuration of BGP neighbor using the vlan 15 interface addresses.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 06:50 AM
Hello,
what are the two DC routers connected to ? An overview of your entire topology would help. If there is only one uplink to one ISP, there is no failover...everything will fail regardless of which routing protocol is being used.
In case there are different links connecting through OSPF and iBGP, OSPF will be selected first, if that (link) fails, iBGP will be selected, and if that fails, your static default route.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 07:17 AM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 07:20 AM
Subinterface 11 is configured for OSPF
encapsulation dot1Q 11
ip address 10.2.2.2 255.255.255.248
ip ospf message-digest-key 1 md5 7 0468031357205E1F2D18
!
router ospf 11
router-id 10.2.2.10
area 0 authentication message-digest
redistribute connected subnets
redistribute static subnets
passive-interface default
no passive-interface Port-channel1.11
network 10.2.2.0 0.0.0.255 area 0
network 10.2.3.0 0.0.0.255 area 0
network 10.2.4.0 0.0.0.255 area 0
network 10.2.5.0 0.0.0.255 area 0
!
My query is if ISP link in DC1 goes down , what OSPF will do in that case
Also , BGP is between Router peer group and ISP . and BGP is in between two routers also
how OSPF and BGP work in this case in case ISP1 goes down .
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 08:11 AM
Hello,
I am not sure I fully understand your topology, sorry for that. Do you have only one link to ISP1 and ISP2 respectively from each router ?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 08:19 AM
Hello,
Yes link to isp1 from router1
Link to isp2 from router 2
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 11:29 AM
Hello,
if that is the case, one link to the ISP, the routing will execute in the order I outlined in my first answer. OSPF first, then BGP, then the static route. As everything goes over one physical link, you don't really have a 'failover'.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 11:43 AM
Hi,
I need to understand this ,
If isp1 link goes down , this means static default route configured in router 1 will no longer works.
In ospf , we are redistributing static and connected but we are also using specific network by mentioning them
redistribute connected subnets
redistribute static subnets
passive-interface default
no passive-interface Port-channel1.11
network 10.2.2.0 0.0.0.255 area 0
network 10.2.3.0 0.0.0.255 area 0
network 10.2.4.0 0.0.0.255 area 0
network 10.2.5.0 0.0.0.255 area 0
So how does.default route will redistrbute because if I run show ip route ospf ,it does not show default route.
If I run show ip route , default route is static pointing to isp1.
From the setup , i understand that purpose of setting ospf is to act as a carrier for bgp.
I am still not able to understand , if isp1 goes down , what will happen to
Ospf.
Bgp.
Default route .
Traffic flows through below order if isp1 goes down.
Switch in dc1 >router1vlan 14>backto switch dc1 ( tunneled in vlan 14).>Switch dc2 ( tunnels in vlan 14) > router in dc2 vlan 14> isp2
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 12:37 PM
Hello,
in OSPF, you cannot redistribute a static default route. You need to use the 'default-information originate' command (since you have a default route configured), or the 'default-information originate always' command (which will generate a default route without one actually existing).
If your physical link goes down, neither your BGP, OSPF, or the static route will work.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 12:45 PM
Hello , This setup did work .
It was tested 2 years back and ISP1 link was switched off and traffic moved dynamically to ISP2
I know my knowledge of routing is limited . but what is the point of OSPF and BGP if traffic wont shift to another DC .
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 01:20 PM
Hello,
as I said earlier, it is unclear from your topology drawing how (and if) the two routers are connected. If they are, the backup (failover) will work. If possible, post the full configs of both routers so we can see how this is set up.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 01:53 PM
OSPF will redistribute whatever you'll tell it, however Georg is right and it wouldn't redistribute default route. You have to have "default-information originate" under OSPF configuration to make sure default route got advertised to neighbors. Moreover - static route is not reliable as it is not detecting the peer status. You would have a valid static route as long as interface is in the up/up state.
Ideally you want to run BGP with your ISP and then make sure the 0.0.0.0/0 goes into OSPF and get redistributed between DCs. It is a bit tricky since OSPF has a better administrative distance so in one of the DCs it would prefer OSPF route over BGP route. You can overcome this by altering AD from specific gateway using route-map to match specific route.
Another option is to use "ip sla" with "tracking" option. In that case you can detect your ISP's PE router failure even if the interface status is in the "up/up" state.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 02:47 PM
Hello , I am attaching the config of both the routers . I have removed the Public IP address of ISP and all other private details .
Regarding connection :
Router 1 connect to physically core switch 1 in DC1 . Router 1 also connected to ISP1 on a separate physical interface
Router 2 connect to Physically core switch 2 in DC2 Router 2 also connected to ISP2 on a separate physical interface
and both the core switches connect via Physical links .
So any vlan created on router1 will flow via cross DC physical link and extend over to Router 2 .
In this case vlan 14( GLB vlan ) and vlan 15 ( OSPF vlan) flows over cross DC link . The BGP uses the same loopback and flows over vlan 15
The goal of this conversation is to validate what happen if only and only ISP1 link goes down - Router 1 and Router 2 will still remain active and how traffic flows via OSPF/BGP . Will it dynamically send default route to Router 2 or manual intervention is required
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 03:07 PM
There is much that is not clear about this environment and that makes it difficult to give good advice. Some clarification of the environment would be helpful. But one thing stands out to me and that is that the posted partial config shows that the router has a static default route. In my experience the main reason for running BGP with an ISP to to allow the ISP to advertise a default route. And loss of the advertised default route from the ISP is the main mechanism for failover. But with this router and its static default route you could lose access to the ISP but the static default route might very well remain in the routing table and you would not fail over.
The first thing that I would suggest is to remove the static default route. Verify that your ISP is advertising a default route to you, and if so trust the ISP default route. Beyond that there are other things to figure out including whether you want the ISPs to operate as Primary and Backup or as both active and sharing the load, and how OSPF will treat the default route.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-22-2020 03:13 PM
Hi Richard , i have attached the router config in the post trail
The special thing is that BGP peer group is being used here . So if ISP1 goes down ; BGP peer group ( having both R1 and R2 loopbacks) will be updated .
Also , how the default route will be propagated ( if ) via OSPF/BGP if ISP1 goes down .