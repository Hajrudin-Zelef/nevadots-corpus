How does switch knows which VLAN
						
					
					
				
			
		
	
			
	
	
	
	
	
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-25-2023 11:32 AM
I understand VLAN creates a broadcast domain and L2 switch only fwd within the VLAN without an L3 device. However, how does the L2 switch knows which VLAN the frame needs to go without a dot1q Header?
For eg. I did a pcap and I don't see any VLAN ID or info on the ethernet header for access ports so when access ports send a broadcast or unicast how does the switch know which VLAN it's coming from and the other host is on the same VLAN?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-27-2023 03:00 AM
"However, when the frame comes in it doesn't say anywhere this frame belongs to VLAN 5 (at least I couldn't see it on a Packet Capture)."
Incorrect, it does "say". See my posting on how switch knows frame's VLAN membership even without VLAN tag.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-27-2023 03:06 AM
"So If the port is configured as an access VLAN 5 then the source mac = VLAN 5?"
MAC alone, no, because they only need to be unique per L2 domain.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-25-2023 02:25 PM
Adding to what @Richard Burts mentioned, by default nothing configured on the switch al in VLAN 1 (that is cisco default)
you can also view where the port connected end device - is this access port configured with access port vlan x ?
you did pcap where ? on the Access port ? can you share here your PCAP to understand ?
=====️ Preenayamo Vasudevam ️=====
***** Rate All Helpful Responses *****
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-25-2023 02:54 PM
Basically it's a simple scenario and I am trying to cement down the basic switching foundtation:
For Eg.
Host A in VLAN 5 is connected to an L2 switch access port VLAN 5. It sends a broadcast frame or an ICMP to a host on Access vlan 10 same switch. From what I saw on PCAP, there is nth on L2 header that tells a switch ok this frame is coming from VLAN 5 unlike if there is a dot1q tag where it adds the header. So I am trying to understand how switch makes a decision without that info.
When I took packet capture I did it on the actual wire. Does smth else happen on the backplane of the switch and changes while sending out to the wire?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 05:00 AM
I’m not an expert, but maybe my understanding of this will help.
Technically, Host A may be in VLAN 5, but the host itself does not have a clue what VLAN it is part of. Actually, it does not even know that there is some kind of VLAN. It has no concept of the VLAN.
A host sends and receives frames that do not contain a VLAN tag. A host is just connected to some access port. However, that access port is a member of a certain VLAN. And, only one VLAN, not many. It is that membership that tells the switch what VLAN a frame should go to. So, a switch does not need a VLAN tag on an access port at all. VLAN tags play role on trunk ports, not access ports. Although it was suggested, I do not think MAC address tables play any role in that, either. To me, MAC addresses are associated with VLAN IDs only to identify the VLANs MAC addresses belongs to. That’s just for switching purposes. When a frame exits a switch, the switch also looks at the port-VLAN membership and can send out a frame only through ports that are members of the VLAN the frame is in. The port-VLAN membership defines the boundaries of a VLAN broadcast domain.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 06:15 AM
Kris K
You may not be expert but you make some good points. So +5 for you. You are correct that a typical host in the network does not know about vlan and that there is not anything in the ethernet frame that the typical host sends. Knowledge of the vlan is in the switch. So the typical host sends a ethernet frame with no indication of vlan membership. The frame arrives on an access port of the switch. Based on the access port the switch determines which vlan this frame is associated with. If it is a layer 2 switch it can forward the frame only to other devices in that vlan.
There has been some discussion about vlan 5 and vlan 10. If it is a layer 2 switch it can not forward from vlan 5 to vlan 10. A layer 2 switch can only forward within the vlan that it received the frame from. To get from vlan 5 to vlan 10 the switch would need to forward the frame to some gateway/router.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 07:07 AM - edited 02-26-2023 07:10 AM
Yes, def +5 for Kris K. Sometimes a non-expert explanation is the best way to understand
So why cannot a layer 2 switch forward frame between 2 VLANs (VLAN 5<>VLAN 10) as it has knowledge of all port mappings & VLAN ID?
Is it bcoz a Host will send the frame to its default GW rather than directly to Host B to reach another VLAN which will be an L3 device?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 07:35 AM
If arp source from port with vlan 5' what port that SW will forward to??
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 08:05 AM
“So why cannot a layer 2 switch forward frame between 2 VLANs (VLAN 5<>VLAN 10) as it has knowledge of all port mappings & VLAN ID?”
Well, an L2 switch just does not want to do that . The IP subnet in VLAN10 would be different than the IP subnet in VLAN5. Changing subnets is routing, not switching. L2 simply does not deal with routing. Routing is dealt with in L3. That’s how the standards are defined.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 02:08 PM
Somehow switch associates frame with VLAN.
Exactly how this is done would depend on the switch's architecture and likely considered proprietary.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 03:24 PM - edited 02-26-2023 03:33 PM
Interesting, when I responded, initially, I didn't notice there have been a whole lot of replies, already.
@Gucamole you appear to understand that on a shared wire, VLAN tags are needed to identify what VLAN a frame belong to. But wonder, how a switch knows what VLAN a switch belongs to within the switch.
Well, as I wrote, "somehow" switch keeps track of what VLAN a frame belongs to, using some method that, generally, a switch manufacturer doesn't reveal. For all we know, it's using .Q VLAN frame tags, or ISL frame tags, or some other method.
How the switch keeps track of what VLAN a frame belongs to, can be the "secret sauce", but how the switch knows which VLAN a frame should belong to is done in different ways.
On a trunk link, all the frames will have a .Q or ISL (unlikely now a-days) frame tag, except for on Cisco trunks which have the concept of a "native" VLAN. The latter, which are untagged frames, by default, are assumed part of VLAN 1, unless we explicitly config what other VLAN should be used for any untagged frames:
E.g.
interface GigabitEthernet1/0/1
switchport trunk native vlan 10 !non-default explicit assignment
switchport mode trunk
An "access" ports, again the switch knows what VLAN frames entering/exiting that port should be, either, again, by default to VLAN 1, or explicitly assigned:
E.g.
interface GigabitEthernet1/0/1
switchport access vlan 10 !non-default explicit assignment
An access port, though, doesn't used tagged frames, but because the prior example configures the port as a VLAN 10 port member, frames entering this port would be considered/treated as part of VLAN 10, and frames exiting this port, would only come from VLAN 10.
Again, given 4 access ports, two in VLAN X and two in VLAN Y, none with VLAN tagged frames, how does the switch "know" what VLAN they belong to, is accomplished by what access port they entered on. What access port the frame may exit on, is accomplished by access port being a member of the same VLAN. Lastly, how is the switch not confused by multiple VLAN frames, within it, we don't know, unless the manufacturer documents that. Whatever/however the switch does what it does, VLAN X and VLAN Y frames are "known" internally.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 03:43 PM
You ask "So why cannot a layer 2 switch forward frame between 2 VLANs (VLAN 5<>VLAN 10) as it has knowledge of all port mappings & VLAN ID?". There is a simple answer to this question and a more complicated answer.
First the simple answer: it is a basic limitation of layer 2 switch that it can not forward between vlans. To forward between vlans you need a layer 3 switch.
The more complicated answer: a vlan is a broadcast domain and a layer 2 switch can forward traffic to any destination in that broadcast domain but can not forward to a different broadcast domain. So if a switch has an access port in vlan 5 and receives a frame on that port with a destination that is in vlan 5 the switch can forward with no problem. But if that switch receives a frame on that access port and the destination is in vlan 10 then the layer 2 switch can not forward to that destination. (remember that right now we are dealing with mac actresses for layer 2 and not IP addresses for layer 3. note that the host device that originated that frame should not send the frame with a destination mac address in vlan 10).
If we have vlan 5 and vlan 10 and we want them to communicate then we need something that can operate between the vlans and that would be some layer 3 device (could be layer 3 switch, or could be router, or might be firewall). vlan 5 would have its own subnet (perhaps 10.5.5.0) and vlan 10 would have its own subnet (perhaps 10.10.10.0). If we think about the device that is connected to an access port in vlan 5 it would have an IP address in that subnet (perhaps 10.5.5.55). If it wanted to communicate with some device in vlan 10 (perhaps 10.10.10.10) its processing logic would recognize that it want to communicate with a "remote" device and would not look for a mac address of 10.10.10.10 but would look for a destination mac address of a device that can communicate between subnets (its default gateway).
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-26-2023 03:53 PM
Having sent that response it occurs to me that there is a different approach to the question "So why cannot a layer 2 switch forward frame between 2 VLANs (VLAN 5<>VLAN 10) as it has knowledge of all port mappings & VLAN ID?"." The question assumes that a frame has arrived on an access port in vlan 5 and the destination mac address is in vlan 10. But how/why would a frame be sent in vlan 5 whose destination is in vlan 10? That assumes a serious bug in the device that originated the frame.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-27-2023 12:13 AM
When we think about networking at layer 3 there is the concept of networking that is local and networking that is remote. So from vlan 5 to vlan 10 is possible. But at layer 2 there is not any concept of remote. The original post specifically asks about layer 2 switch. In a layer 2 switch if a frame enters in vlan 5 it can not be forwarded anywhere that is not in vlan 5. The switch may know about vlan 10 and be forwarding traffic in vlan 10 as well as in vlan 5. But a layer 2 switch can not forward from vlan 5 to vlan 10. To get between the vlans you need layer 3.
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-27-2023 02:48 AM
Yeah,
Yeah, the funny thing about all of this is that Host A will not even try to reach Host B using L2. Host A and Host B being in two different VLANs will be in two different networks. Host A will recognize that Host B is in a different network and it will send a L3 packet to its gateway for delivery to Host B.