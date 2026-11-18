- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 12:56 AM
Hi,
I'm experiencing an issue on my Meraki-based network, where a certain set of switches are almost constantly reporting STP BPDU sender conflicts.
Here is some info about my network:
We are situated on a large farm, therefore we have multiple points in different locations on the same network. I have no redundant links between any of my switches; there are only daisy-chains between some of them. Some of the locations are connected wirelessly, using Ubiquiti and Mimosa links.
The Mimosa links are merely two Point to Point links connecting to a facility, with two switches daisy-chained on the other side. Then I have a Ubiquiti Rocket M5 set up as an Access Point. Three different locations connect to them, each using a Ubiquiti Nanobeam M5. There is a Meraki switch on the other side of each of those stations. The Rocket M5 and Mimosa each connects directly to my Root Bridge.
Since I am only getting errors on the switches connected to the Rocket AP, I will refer only to them by name: Admin, Grape Office, Workshop and Controlroom.
Admin connects via a Nanobeam M5 to the Rocket M5.
Grape Office connects via a Nonobeam M5 to the Rocket M5.
Workshop connects via a Nanobeam M5 to the Rocket M5.
Controlroom connects via two point to point Nanobeam M5s to Switch C.
The following events occur frequently in the Event log:
| Apr 24 09:28:07 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from ROOT BRIDGE, 19; expected Grape Office, 1 | 
| Apr 24 09:28:06 | Admin |  | STP BPDU sender conflict | Port 1 received BPDU from Grape Office, 1; expected ROOT BRIDGE, 19 | 
| Apr 24 09:28:06 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from Grape Office, 1; expected ROOT BRIDGE, 19 | 
| Apr 24 09:28:05 | Admin |  | STP BPDU sender conflict | Port 1 received BPDU from ROOT BRIDGE, 19; expected Grape Office, 1 | 
| Apr 24 09:28:05 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from ROOT BRIDGE, 19; expected Grape Office, 1 | 
| Apr 24 09:28:04 | Admin |  | STP BPDU sender conflict | Port 1 received BPDU from Grape Office, 1; expected ROOT BRIDGE, 19 | 
| Apr 24 09:28:04 | Grape Office |  | Port STP change | Port 1 root→designated | 
| Apr 24 09:28:04 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from Grape Office, 1; expected ROOT BRIDGE 19 | 
| Apr 24 09:28:03 | Admin |  | STP BPDU sender conflict | Port 1 received BPDU from ROOT BRIDGE, 19; expected Grape Office, 1 | 
| Apr 24 09:28:01 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from ROOT BRIDGE, 19; expected Grape Office, 1 | 
| Apr 24 09:28:00 | Admin |  | STP BPDU sender conflict | Port 1 received BPDU from Grape Office, 1; expected ROOT BRIDGE, 19 | 
| Apr 24 09:28:00 | Workshop |  | STP BPDU sender conflict | Port 6 received BPDU from Grape Office, 1; expected ROOT BRIDGE, 19 | 
| Apr 24 09:27:59 | Grape Office |  | Port STP change | Port 1 designated→root | 
It most commonly occurs that the Grape Office switch sends BPDUs to the other switches, but it is not limited to that. Sometimes Admin will send a BPDU to Workshop, Workshop to Grape Office, etc. The Port STP change, however, only ever happens on the Grape Office switch. The Port STP Changes happen 2-6 times each minute, where the BPDU errors happen every 1-3 seconds.
The Grape Office switch is a small 8-port switch. It uses 4 ports: Its Uplink via a Nanobeam M5; A VoIP phone; A PC; A Ubiquiti Unifi that provides WiFi in the office.
When I temporarily disconnect the Grape Office switch remotely, I get no more BPDU errors on the network for the entire time it is disconnected. I do not know why this switch would misbehave, as it is configured the exact same way as all other switches on the network.
Although the network does not come to a complete standstill when it happens, my users frequently complain about disconnecting from my main server or slow internet connections in general.
Any help regarding this issue would be greatly appreciated.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Meraki
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 02:56 AM
Is the grape office switch a Meraki switch?
Next thought; if you can be confident the network is loop free, it may be worthwhile simply disabling spanning tree.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 01:34 AM
The 10.x firmware had a lot of spanning tree improvements. Try going to 10.x if you don't make progress.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 02:08 AM
@Philip D'Ath wrote:
Have you set the spanning tree priority on your "core" switch to force it to be the root? If not do this first.
The 10.x firmware had a lot of spanning tree improvements. Try going to 10.x if you don't make progress.
Yes, the core switch's bridge priority is set to 0 to force it to be the root. It is the "Root Bridge" switch I refer to throughout the question, which connects to the Rocket AP.
All of the switches are currently already on the 10.6 firmware.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 02:41 AM
10.6! I would go to 10.22 promptly.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 02:56 AM
Is the grape office switch a Meraki switch?
Next thought; if you can be confident the network is loop free, it may be worthwhile simply disabling spanning tree.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-24-2018 10:13 PM
@Philip D'Ath wrote:
Is the grape office switch a Meraki switch?
Next thought; if you can be confident the network is loop free, it may be worthwhile simply disabling spanning tree.
Yes, it's an 8-port MS220.
I will disable it for the time being and see how it goes. I may begin using redundant links sometime in the future as the network grows, so I would eventually have to enable STP again.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-25-2018 03:33 AM
You can do redundant links with port aggregation. Then you don´t need STP.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-10-2019 03:23 PM
Hi redsector.
STP is just not used for redundancy, it is for find and stop loop in network as well .
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-13-2020 06:40 AM
I realize that this is an old thread but thought I would update with information that would be relevant in today's environment. I had similar issue and the solution was to enable VLAN 1 on the core switch and walk it down to the other switches. After doing so the switch status will update to point to the Core Switch as being the Root Switch in the topology. Here is an article that was provided to me for further reading.
Identifying Root Switch:
Configuring STP on Meraki Switch:
Advance Setting of STP on Meraki Switches:
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-25-2018 09:36 AM
Hey Piet,
These warnings are actually ones you don't want to ignore, particularly if you are planning to introduce redundant links down the road.
Your network must be on the MS 10.x firmware release, as this is the version where BPDU conflict logging was introduced, as part of overall enhancements to anomaly detection.
If I understand correctly your topology is something like this:
You've got a switch, ROOT BRIDGE, to which one port has a connection with a Ubiquity AP.
Then you have three Meraki switches:
Workshop, Admin and Grape Office
Each of these three switches has another Ubiquity wireless bridge connected, and all three of them wirelessly bridge back to the AP connected to your root switch. The outcome is that from the perspective of your root switch, all three of these Meraki switches are downstream from the single port with the wireless bridge.
This is where your problem is stemming from, and what the logging has identified. The wireless bridge solution is for all intents and purposes acting as a dumb L2 switch. This dumb/unmanaged switch effectively interconnects four of your switches in a star topology:
(A) ROOT BRIDGE
(B) Workshop
(C) Admin
(D) Grape Office
In addition, you have a 5th "pseudo-switch", the unmanaged L2 switch formed by the wireless bridging, I'll refer to as:
(E) Unmanaged Switch
For the remainder of this post I will refer to these switches by A, B, C, D and E per the above list. The switches A through D must be running RSTP, not legacy STP, as the warnings you see logged are only produced from segments operating in RSTP mode.
Now here's the problem:
When any of the four switches A through D transmits a BPDU, the BPDU will be received by switch E (unmanaged switch / wireless bridge). BPDUs are sent to a special destination of 01:80:C2:00:00:00. This is the well-known address used by the IEEE STP/RSTP protocols.
If a switch supports STP, when it recevies a BPDU with this special destination address, it does not forward the BPDU out other ports. It just uses the data from the BPDU for its own STP calculations, and then may generate its own BPDUs to send out other ports.
But in the case of switch E, (R)STP is not supported, it's just an unmanaged L2 switch. So when E recevies a BPDU from any of the switches A through D, it will just flood the BPDU out all its other "ports" (wireless links in this case).
Example:
(1) Switch A transmits a BPDU.
(2) Switches B, C, and D all receive this same BPDU.
This is the root of the problem. It would actually be fine using legacy STP, in which a port won't become forwarding until the expiration of a long timer, which can be around 30 seconds. But with RSTP, ports become forwarding rapidly (hence the name RSTP!).
The mechanism RSTP uses to rapidly transition a port into the forwarding state and bypass the 30 second delay of the old standard is through the use of a proposal/agreement negotiation between ports. Instead of wating for timers to expire, a port will send out an initial BPDU with a "proposal" flag set. The other port that receives this "proposal" BPDU will send out a responding BPDU with an "agreement" flag set.
This affirmative proposal/agreement process is the primary mechanism used by RSTP to converge faster than the legacy standard which exclusively relies on waiting for timer expirations.
The problem:
This proposal/agreement mechanism is exlcusively point-to-point, it only works between explicit pairs of ports. It does not work in your scenario where a proposal BPDU from A is received by B, C, and D, with all of them potentially sending their own agreement responses (and then these agreements would again get flooded to all switches!).
In this scenario, RSTP convergence becomes undefined and may be unstable and/or introduce temporary loops that could come and go. Now, with your current topology you don't actually have any physical loop, so even with the RSTP convergence imstability, there is no risk of an actual loop forming, as it's physically impossible.
However, if you add a redundant link down the road such that there is a real physical loop that relies on spanning tree to be handled, now the door will be open to encounter more impactful problems.
Personally, I would recommend not using the single AP on the root switch to wirelessly bridge down to your three other switches. If you add two additional APs and have your three switches wirelessly link up to dedicated APs on the root bridge, then you will avoid this problem scenario.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-29-2018 10:23 PM
@test108 wrote:
Hey Piet,
These warnings are actually ones you don't want to ignore, particularly if you are planning to introduce redundant links down the road.
Your network must be on the MS 10.x firmware release, as this is the version where BPDU conflict logging was introduced, as part of overall enhancements to anomaly detection.
If I understand correctly your topology is something like this:
You've got a switch, ROOT BRIDGE, to which one port has a connection with a Ubiquity AP.
Then you have three Meraki switches:
Workshop, Admin and Grape Office
Each of these three switches has another Ubiquity wireless bridge connected, and all three of them wirelessly bridge back to the AP connected to your root switch. The outcome is that from the perspective of your root switch, all three of these Meraki switches are downstream from the single port with the wireless bridge.
This is where your problem is stemming from, and what the logging has identified. The wireless bridge solution is for all intents and purposes acting as a dumb L2 switch. This dumb/unmanaged switch effectively interconnects four of your switches in a star topology:
(A) ROOT BRIDGE
(B) Workshop
(C) Admin
(D) Grape Office
In addition, you have a 5th "pseudo-switch", the unmanaged L2 switch formed by the wireless bridging, I'll refer to as:
(E) Unmanaged Switch
For the remainder of this post I will refer to these switches by A, B, C, D and E per the above list. The switches A through D must be running RSTP, not legacy STP, as the warnings you see logged are only produced from segments operating in RSTP mode.
Now here's the problem:
When any of the four switches A through D transmits a BPDU, the BPDU will be received by switch E (unmanaged switch / wireless bridge). BPDUs are sent to a special destination of 01:80:C2:00:00:00. This is the well-known address used by the IEEE STP/RSTP protocols.
If a switch supports STP, when it recevies a BPDU with this special destination address, it does not forward the BPDU out other ports. It just uses the data from the BPDU for its own STP calculations, and then may generate its own BPDUs to send out other ports.
But in the case of switch E, (R)STP is not supported, it's just an unmanaged L2 switch. So when E recevies a BPDU from any of the switches A through D, it will just flood the BPDU out all its other "ports" (wireless links in this case).
Example:
(1) Switch A transmits a BPDU.
(2) Switches B, C, and D all receive this same BPDU.
This is the root of the problem. It would actually be fine using legacy STP, in which a port won't become forwarding until the expiration of a long timer, which can be around 30 seconds. But with RSTP, ports become forwarding rapidly (hence the name RSTP!).
The mechanism RSTP uses to rapidly transition a port into the forwarding state and bypass the 30 second delay of the old standard is through the use of a proposal/agreement negotiation between ports. Instead of wating for timers to expire, a port will send out an initial BPDU with a "proposal" flag set. The other port that receives this "proposal" BPDU will send out a responding BPDU with an "agreement" flag set.
This affirmative proposal/agreement process is the primary mechanism used by RSTP to converge faster than the legacy standard which exclusively relies on waiting for timer expirations.
The problem:
This proposal/agreement mechanism is exlcusively point-to-point, it only works between explicit pairs of ports. It does not work in your scenario where a proposal BPDU from A is received by B, C, and D, with all of them potentially sending their own agreement responses (and then these agreements would again get flooded to all switches!).
In this scenario, RSTP convergence becomes undefined and may be unstable and/or introduce temporary loops that could come and go. Now, with your current topology you don't actually have any physical loop, so even with the RSTP convergence imstability, there is no risk of an actual loop forming, as it's physically impossible.
However, if you add a redundant link down the road such that there is a real physical loop that relies on spanning tree to be handled, now the door will be open to encounter more impactful problems.
Personally, I would recommend not using the single AP on the root switch to wirelessly bridge down to your three other switches. If you add two additional APs and have your three switches wirelessly link up to dedicated APs on the root bridge, then you will avoid this problem scenario.
Hi Andrew,
Thank you for all of the info!
I will take what you have said into consideration when deciding on adding redundant links in the future. Perhaps I will then, as you have suggested, rather replace the Rocket AP with three separate PtP links. For now disabling RSTP seems to have solved the problem, as I am no longer getting any errors.