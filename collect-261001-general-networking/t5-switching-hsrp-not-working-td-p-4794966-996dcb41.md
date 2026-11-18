- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 12:59 PM
Hello,
I've configured our two Core Switches to be in HSRP for the VLANs on the network. Upstream from the two Core Switches is an ASA that I configured to have a redundant interface so we have redundancy if an upstream interface fails for the Core Switches. The interfaces failing over works. However, when I test to see if HSRP will failover by pulling the power on Core Switch 1, the entire network goes down. Now, we have some physical limitations so not all access switches all connect to the distribution switches. Some access switches connect directly to the Core Switches and other access switches only have one connection to either Distribution Switch 1 or Distribution Switch 2. I configured the Core Switches to be the Root for the STP for all of the VLANs on the network. Core Switch 1 being primary root and Core Switch 2 being secondary root (spanning-tree vlan x priority 24576 and spanning-tree vlan x priority 28672). Am I missing any configurations that's preventing the HSRP from failing over?
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Catalyst 9000
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:01 PM
I think since you only have one firewall, the easiest way to do this is to configure the 2 9500 in Virtual Stack mode, where 2 switches logicaly became one and than use a Portchannel to connect the interface from switch-1 and 2 to the firewall. This way, logically you are connecting one firewall to one logical switch with redundant links and redundant switches.
HTH
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 01:21 PM
Hi,
What type of switcges are these and how are they connected to the firewall?
Can you post the output of "sh hsrp" and "sh run" from both switches?
HTH
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 01:27 PM
The Core Switches 1 and 2 are Cat 9500 40 ports. They are connected to Firewall using a single interfaces from each of the switches as access ports using a VLAN and HSRP and virtual gateway for the ASA to send traffic down to. On the ASA i've configured a virtual interface that includes two physical interfaces for redundancy.
I didn't get any results with 'show hsrp' due to Cat9500 not having that command, but I am assuming this is what you're referring to? I have removed the IP address when copy pasting.
Core Switch 1:
Interface Grp Pri P State Active Standby Virtual IP
Vl10 10 110 P Active local 
Vl20 20 110 P Active local 
Vl23 23 110 P Active local 
Vl30 30 110 P Active local 
Vl40 40 110 P Active local 
Vl80 80 110 P Active local 
Vl100 100 110 P Active local 
Vl570 570 110 P Active local 
Vl581 581 110 P Active local 
Vl590 590 110 P Active local 
Vl610 610 110 P Active local 
Core Switch 2:
Interface Grp Pri P State Active Standby Virtual IP
Vl10 10 100 P Standby 
Vl20 20 100 P Standby 
Vl23 23 100 P Standby 
Vl30 30 100 P Standby 
Vl40 40 100 P Standby 
Vl80 80 100 P Standby 
Vl100 100 100 P Standby 
Vl570 570 100 P Standby 
Vl581 581 100 P Standby 
Vl590 590 100 P Standby 
Vl610 610 100 P Standby 
I don't think I can post 'sh run' because of how much info it shows. If you need specific information from 'sh run' I can post it by adjusting the type of info that is shown.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:06 PM
I dont image your topology, 
two Core SW run HSRP and both must connect to one other L2SW and ASA connect to both Core via this L2SW. 
ASA dont not forward multicast need by HSRP to exchange hello between two Cores. 
this reason I think
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:10 PM
Yes that is how it's configured. Core Switch 1 has a physical link to ASA. Core Switch 2 has a physical link to ASA. The two interfaces on the ASA that have the Core Switch 1 and 2 connections are configured to be members of a virtual interface that routes the traffic to and from the internal network. There is a trunk interface between Core Switch 1 and 2 that is also connected.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:14 PM
Not work, you need L2SW connect three, two Cores and one ASA, this will make both Cores exchange multicast hello and make ASA have static route toward the VIP of Cores HSRP group
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:18 PM
I don't quite understand what you mean by 'L2SW connect three, two Cores and one ASA.' Please elaborate on this.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:33 PM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:43 PM
So how will Switch 1 be configured in this situation? Will it have the access ports to Core Switch 1 and 2? then the interface going to ASA be a Layer 3 interface?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:17 PM
No all interface in L2SW is L2 interface, 
the Core config with SVI or router port and ASA config with L3 interface (interface in ASA will be L3 not interface in L2SW)
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 02:54 PM
Also, when I pull the ethernet cable that's going from Core Switch 1 to the ASA, and leave the connection between ASA and Core Switch 2 connected, the internet still works. It's only when Core Switch 1 is powered off is when I lost internet connection from switches and Access Points that should still have connection to the internet through Core Switch 2.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:01 PM
I think since you only have one firewall, the easiest way to do this is to configure the 2 9500 in Virtual Stack mode, where 2 switches logicaly became one and than use a Portchannel to connect the interface from switch-1 and 2 to the firewall. This way, logically you are connecting one firewall to one logical switch with redundant links and redundant switches.
HTH
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:04 PM
BTW, this design eliminates the need for any HRSP, VRRP, etc..
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:06 PM
I looked into VSS before then got scared of the bugs that it had, only to learn recently that those bugs have been worked out long time ago. I want to switch it over, but I am already so far into HSRP. Also I don't know if my supervisor will like the idea of changing this since we've been working on migrating to a redundant network for the last year.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-15-2023 03:13 PM - edited 03-15-2023 03:15 PM
VSS or Stackwise Virtual, still has bugs but not to that level to bring down the network to its knees. I started working on VSS in 2008 and can tell you it has come a long way. So, I would not be too concern about the bugs. convincing your management is a different a concern and I understand that completely. So, to understand, you are connecting 2 switches to one firewall using physical links and the gateways for all the subnets are on the firewall right?