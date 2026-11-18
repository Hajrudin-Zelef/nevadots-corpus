- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 12:12 PM - edited 03-07-2019 09:13 AM
Hello All,
I was hired on with a state.. Now its been awhile but I can't remember how subinterfaces and VLANS all tie together!
Now correct me where I'm wrong (please), but VLANS are created on switches first correct? When you create a VLAN on a switch you don't need a default gateway or ip address because the VLANS are local to the switch. If you want intervlan routing you need a router. You then setup a trunking port between the switch and router (ISL, 802.1Q). Now in the router you can create VLANS and here you inter the ip addresses of the subnet or default gateway correct? This is where I get confused because for what reasons do you need subinterfaces? How do they tie into VLANS and what would be the logical flow of data?
Anyhelp will be appreciated!
Solved! Go to Solution.
- Labels:
- 
						
							
		
			LAN Switching
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 01:59 PM
Yes you are correct. if you are using layer 2 switch and you want to do intervlan routing then you need layer 3 device like router. But you need to configure sub interfaces with default gateway to route the traffic. Because there is one trunk between swich and router so we need sub interfaces for multiple vlans.
Interface FastEthernet0/0.1
Encapsulation dot1q 10 (10 represent VLAN ID 10 )
IP address 10.1.1.1 255.255.255.0
If you are using a layer 3 switch then you dot need any sub interfaces so then you can create vlan interface with the default gateway. You need to enable ip routing first.
Interface vlan 10
IP address 10.1.1.1 255.255.255.0
Hope this will help.
Please rate if this helps.
thanks
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 01:59 PM
Yes you are correct. if you are using layer 2 switch and you want to do intervlan routing then you need layer 3 device like router. But you need to configure sub interfaces with default gateway to route the traffic. Because there is one trunk between swich and router so we need sub interfaces for multiple vlans.
Interface FastEthernet0/0.1
Encapsulation dot1q 10 (10 represent VLAN ID 10 )
IP address 10.1.1.1 255.255.255.0
If you are using a layer 3 switch then you dot need any sub interfaces so then you can create vlan interface with the default gateway. You need to enable ip routing first.
Interface vlan 10
IP address 10.1.1.1 255.255.255.0
Hope this will help.
Please rate if this helps.
thanks
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 09:46 PM
Amrinder,
Thanks for your brief explanation! It really cleared things up.. So let me get this right! The trunking protocol tags frames and sends them through the trunked port but the router doesn't know what to do with the tagged frames. But by creating subinterfaces, encapsulating them with 802.1Q, and using the VLAN ID.. this tells the router which vlan tagged frames belong to? Then with a routing protocol (OSPF) I would need to advertise all the VLAN networks within the same router to make intervlan routing possible. Is this correct?
You also saved me another question because we have a layer 3 core switch. It was going to be how intervlan routing will work on a layer 3 switch.. but I see that when you create the VLANS you also add an IP address.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 10:35 PM
Hello Miguel,
Yes 802.1q and vlan ID route the traffic to its relevant vlan. You can use OSPF, when a Host can ping its default gateway because it is on the same local subnet. Host can ping both switches because the management interfaces are set to VLAN1. Because a host does not have a route to get to the other VLANs/subnets, it forwards the packets to its default gateway,. Although router has a route to get to the majority of the other subnets, remember that the Internet Control Message Protocol (ICMP) packets need to return as well. if a router has two directly connected routes in the routing table, but no routing protocols or static routes are configured to facilitate communicating from one network to another. You should configure Open Shortest Path First (OSPF) as the routing protocol to allow inter-VLAN routing.
http://www.informit.com/library/content.aspx?b=CCNP_Studies_Troubleshooting&seqNum=77
For layer 3 switch. You need to enable routing first with command IP Routing. You can’t assign IP address to an interface unless you enter No switchport to a interface where you want to use Ip address. By default every port is in layer 2 mode and you need to change it to layer 3 by issue no switchport command . then you will be able to assign IP.
thanks
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 03:16 PM
Hi Miguel De Santiago
My name is Johnnatan Rodriguez, your information about the vlan's is correct, now I'm goint to explain to you, why does the router need subinterfaces?
Subinterfaces: we need it when we have more vlans than physical links
As you said, we have setup a trunking port between the switch and router, then configure the configure the interface and sub interfaces in your router:
Switch(config)#interface fa0/2
Switch(config-if)#switchport mode trunk
Router(config)#interface fa0/1
Router(config-if)#no shutdown
Router(config)#interface fa0/1.1
Router(config-subif)#encapsulation dot1q 1
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config)#interface fa0/1.2
Router(config-subif)#encapsulation dot1q 15
Router(config-subif)#ip address 192.168.15.1 255.255.255.0
Router(config)#interface fa0/1.3
Router(config-subif)#encapsulation dot1q 35
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
We configure sub interfaces because we have 3 vlan and just one physical link, for this reason we need that all data passing through a single link, how do we fix that?
Creating 3 sub-interfaces, one for each vlan, however it reduces the bandwidth one third.
Physical Interfaces: we need it when we have more physical links than vlans
We configure a normal ip in each interface, (one per vlan), now in the switch instead of create trunk links, we create access links (one per vlan).
Router(config)#interface FastEthernet0/0
Router(config-subif)# ip address 10.10.10.1 255.255.255.0
Router(config)interface FastEthernet0/1
Router(config-subif)# ip address 10.10.20.1 255.255.255.0
Router(config)#interface FastEthernet0/3
Router(config-subif)#ip address 10.10.30.1 255.255.255.0
Switch(config)#interface range fa0/1, fa0/10
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config)#interface range fa0/5,fa0/20
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config)#interface range fa0/8, fa0/30
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
Here we have one link per vlan and we can use the full bandwidth of each interface.
I hope you find this answer useful, we will help you with any doubt that you have, if you found this answer useful please mark the question as Answered and rate the anwer.
Thanks for using our forum.
Greetings,
Johnnatan Rodriguez Miranda.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 08:35 PM
hi johnnatan,
nicely done! +5
i felt like attending a CCNA class. keep it up!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-01-2012 09:51 PM
Johnnatan,
You post was very helpful also and it all makes sence. I was, however, not aware that by creating subinterfaces on one physical link, it would reduce or split the bandwidth! It makes sense that this would occur but I guess overlooking the easy stuff is easy to!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-04-2013 04:37 AM
Johnnatan, great answer.
One question:
Do you configure fa 0/1,5 and 8 as access mode because there is only one VLAN per port? And if the answer is yes, am I right in saying this is a valid configuration because only one adjacency is made (maximum for this port)?
Thank you for clarifying in advance. I am just confused as to why you wouldn't configure the port as a trunk, even if it only does have one VLAN across it
Regards,
pp
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-08-2013 12:10 PM
Hi,
Explaining the scenario:
When a port connected to 1 host belonging to a VLAN is access and not trunk.
Access Port: Such ports belong to 1 Vlan only ( 1 data Vlan + 1 Voice Vlan)
Data from and to access ports are always untagged.
TrunkPort: It offers data belonging to multiple VLAN to pass through. Trunk port does that by tagging.
Frame Tagging helps the recieving port to switch to differentiate between data of many VLANs.
Now this VLAN tag is not understood by the machines (Host or Server) which is connected to Switch.
To conclude:
1. Trunk port does Vlan tagging of frames.
2. Host Machine doesn't understand VLAN.
Therefore Host Machine is always connected to the switch's access port.
Interconnectivity of switch is using trunk ports.
Plz correct if I am wrong somewhere or it is improperly explained.
Regards,
Azmun
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
01-05-2017 12:33 PM
Hey,
I have a Cisco 2650 router with a NM-16ESW module attached, I'm looking for the same result as in the vlans inter-connecting. I have a DHCP server on vlan 10, PC's on vlan 30, etc... I can get IP addresses to each devices on each vlan from DHCP server, but when I go to ping or access resources from vlan 10 no luck. I have this all working without the use of sub-interfaces, do I need the sub-interfaces for this to work properly? Is there a document on this for my type of setup?
Thanks in advance for the help.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-23-2017 10:45 AM
Thanks JohnNathan,
Nicely explained , Iwas also having confussion with sub interfaces , now its cleared.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-29-2018 01:03 AM
I have an Extreme Network environment where I have a L3 core switch and other L2 access switches across the office. I did created 4 VLANS with 24 bit subnet. The topology is something like this.
For IT users, the VLAN Network range is X.X.1.0/24 with X.X.1.254 being the gateway.
For FINANCE users, the VLAN Network range is X.X.2.0/24 with X.X.2.254 being the gateway
For HR users, the VLAN Network range is X.X.3.0/24 with X.X.3.254 being the gateway
Now all of these .254 ip is configured in Core switch. Intervlan routing is configured and working fine. Now the trouble start when users from FINANCE and HR are trying to access our remote office networks. The router (Cisco 4331) is connected to Core switch with its LAN port ip as X.X.1.1/24. Now IT users are facing no problem in accessing remote location servers as the traffic is directly being routed to Router LAN port and from there to remote site through WAN.
But when I give a traceroute from FINANCE or HR users, the traffic doesn't cross over from their local gateway which is X.X.2.254 and X.X.3.254 respectively.
Now my question is, if I do configure the port in Switch connected to router interface as Trunk and create sub interfaces in Router LAN port, would my issue be resolved?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-03-2018 04:06 AM