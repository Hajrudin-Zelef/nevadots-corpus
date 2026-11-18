- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 07:37 AM - edited 02-21-2020 06:48 AM
Im setting up a home lab and having a real time with natting. My global statement seems to override my static statements causing a drop in my inbound traffic to a plex server.
Right now in the below outputs i kinda have the static statements reversed i think just because i tried to place them on the outside interface to separate them from the global statement but im pretty sure its all wrong. Any and all advise is very much appreciated.
Below are some show and packet trace inputs, a couple of notes I currently have a permit ip any any for troubleshooting reasons and I included a couple of packet trace inputs, one that has my publick ip as the source and one that just has any public IP.
access-list OUTSIDE_access_in extended permit ip any any
access-list OUTSIDE_access_in extended permit udp any host 10.10.X.X eq 20122 log
access-list OUTSIDE_access_in extended permit tcp any host 10.10.X.X eq 5050 log
access-list OUTSIDE_access_in extended permit tcp any host 10.10.X.X eq 8989 log
access-list OUTSIDE_access_in extended permit tcp any host 10.10.X.X eq 8080 log
access-list OUTSIDE_access_in extended permit tcp any host 10.10.X.X eq 32400 log
access-list OUTSIDE_access_in extended deny ip any any
access-list LAB_access_in extended permit ip any any
access-list LAB_access_in extended deny ip any any
access-list WIRELESS_access_in extended permit ip any any
access-list WIRELESS_access_in extended deny ip any any
access-list plex_access_in extended permit ip any any
access-list plex_access_in extended deny ip any any
access-list OUTSIDE_acess_in extended permit udp any any
pager lines 24
mtu Outside 1500
mtu WIRELESS 1500
mtu LAB 1500
mtu Plex 1500
ip local pool vpn_users 10.10.X.2-10.10.X.254 mask 255.255.255.0
no failover
icmp unreachable rate-limit 1 burst-size 1
no asdm history enable
arp timeout 14400
global (Outside) 10 interface
nat (Plex) 10 10.10.X.0 255.255.255.0
static (Outside,Plex) tcp 10.10.X.X 32400 159.118.X.X 32400 netmask 255.255.255.255
static (Outside,Plex) tcp 10.10.X.X 8989 159.118.X.X 8989 netmask 255.255.255.255
static (Outside,Plex) tcp 10.10.X.X 8080 159.118.X.X 8080 netmask 255.255.255.255
static (Outside,Plex) tcp 10.10.X.X 5050 159.118.X.X 5050 netmask 255.255.255.255
static (Outside,Plex) udp 10.10.X.X 22 159.118.X.X 20122 netmask 255.255.255.255
access-group OUTSIDE_access_in in interface Outside
access-group WIRELESS_access_in in interface WIRELESS
access-group LAB_access_in in interface LAB
access-group plex_access_in in interface Plex
timeout xlate 3:00:00
NAT policies on Interface Outside:
 match tcp Outside host 159.118.X.X eq 32400 Plex any
 static translation to 10.10.X.X/32400
 translate_hits = 0, untranslate_hits = 17
 match tcp Outside host 159.118.X.X eq 8989 Plex any
 static translation to 10.10.X.X/8989
 translate_hits = 0, untranslate_hits = 0
 match tcp Outside host 159.118.X.X eq 8080 Plex any
 static translation to 10.10.X.X/8080
 translate_hits = 0, untranslate_hits = 0
 match tcp Outside host 159.118.X.X eq 5050 Plex any
 static translation to 10.10.X.X/5050
 translate_hits = 0, untranslate_hits = 0
 match udp Outside host 159.118.X.X eq 20122 Plex any
 static translation to 10.10.X.X/22
 translate_hits = 0, untranslate_hits = 0
NAT policies on Interface Plex:
 match ip Plex 10.10.X.X255.255.255.0 Outside any
 dynamic translation to pool 10 (159.118.X.X [Interface PAT])
 translate_hits = 538, untranslate_hits = 43
 match ip Plex 10.10.X.X 255.255.255.0 WIRELESS any
 dynamic translation to pool 10 (No matching global)
 translate_hits = 0, untranslate_hits = 0
 match ip Plex 10.10.X.X 255.255.255.0 LAB any
 dynamic translation to pool 10 (No matching global)
 translate_hits = 0, untranslate_hits = 0
 match ip Plex 10.10.X.X 255.255.255.0 Plex any
 dynamic translation to pool 10 (No matching global)
 translate_hits = 0, untranslate_hits = 0
Scott-ASA5510-EDGE(config)# packet-tracer input outside udp 159.118.X.X 20122 10.1$
Phase: 1
Type: ACCESS-LIST
Subtype:
Result: ALLOW
Config:
Implicit Rule
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xab90ae00, priority=1, domain=permit, deny=false
 hits=11924721, user_data=0x0, cs_id=0x0, l3_type=0x8
 src mac=0000.0000.0000, mask=0000.0000.0000
 dst mac=0000.0000.0000, mask=0100.0000.0000
Phase: 2
Type: ROUTE-LOOKUP
Subtype: input
Result: ALLOW
Config:
Additional Information:
in 10.10.X.X 255.255.255.0 Plex
Phase: 3
Type: ACCESS-LIST
Subtype:
Result: DROP
Config:
Implicit Rule
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xab928108, priority=500, domain=permit, deny=true
 hits=18, user_data=0x6, cs_id=0x0, reverse, flags=0x0, protocol=0
 src ip=159.118.X.X, mask=255.255.255.255, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Result:
input-interface: Outside
input-status: up
input-line-status: up
output-interface: Plex
output-status: up
output-line-status: up
Action: drop
Drop-reason: (acl-drop) Flow is denied by configured rule
Scott-ASA5510-EDGE(config)# packet-tracer input outside udp 1.1.1.1 20122 10.1$
Phase: 1
Type: ACCESS-LIST
Subtype:
Result: ALLOW
Config:
Implicit Rule
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xab90ae00, priority=1, domain=permit, deny=false
 hits=11946670, user_data=0x0, cs_id=0x0, l3_type=0x8
 src mac=0000.0000.0000, mask=0000.0000.0000
 dst mac=0000.0000.0000, mask=0100.0000.0000
Phase: 2
Type: ROUTE-LOOKUP
Subtype: input
Result: ALLOW
Config:
Additional Information:
in 10.10.X.X 255.255.255.0 Plex
Phase: 3
Type: ACCESS-LIST
Subtype: log
Result: ALLOW
Config:
access-group OUTSIDE_access_in in interface Outside
access-list OUTSIDE_access_in extended permit ip any any
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xacbc0a38, priority=12, domain=permit, deny=false
 hits=3838, user_data=0xa8b3fb80, cs_id=0x0, flags=0x0, protocol=0
 src ip=0.0.0.0, mask=0.0.0.0, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Phase: 4
Type: IP-OPTIONS
Subtype:
Result: ALLOW
Config:
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xab90d538, priority=0, domain=inspect-ip-options, deny=true
 hits=40865, user_data=0x0, cs_id=0x0, reverse, flags=0x0, protocol=0
 src ip=0.0.0.0, mask=0.0.0.0, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Phase: 5
Type: NAT
Subtype: rpf-check
Result: DROP
Config:
nat (Plex) 10 10.10.X.X 255.255.255.0
 match ip Plex 10.10.X.X 255.255.255.0 Outside any
 dynamic translation to pool 10 (159.118.X.X [Interface PAT])
 translate_hits = 701, untranslate_hits = 48
Additional Information:
 Forward Flow based lookup yields rule:
 out id=0xacb91d98, priority=1, domain=nat-reverse, deny=false
 hits=49, user_data=0xaba3dd70, cs_id=0x0, flags=0x0, protocol=0
 src ip=0.0.0.0, mask=0.0.0.0, port=0
 dst ip=10.10.30.0, mask=255.255.255.0, port=0, dscp=0x0
Result:
input-interface: Outside
input-status: up
input-line-status: up
output-interface: Plex
output-status: up
output-line-status: up
Action: drop
Drop-reason: (acl-drop) Flow is denied by configured rule
Solved! Go to Solution.
- Labels:
- 
						
							
		
			NGFW Firewalls
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 06:29 PM
Hi Scott,
The Packet tracer is not properly written.
I would first recommend you to write the nats from inside to outside.... just to be more organized.
so do
no static (Outside,Plex) tcp 10.10.X.X 32400 159.118.X.X 32400 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 8989 159.118.X.X 8989 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 8080 159.118.X.X 8080 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 5050 159.118.X.X 5050 netmask 255.255.255.255
no static (Outside,Plex) udp 10.10.X.X 22 159.118.X.X 20122 netmask 255.255.255.255
static (Plex,Outside) tcp 159.118.x.x 32400 10.10.x.x 32400
static (Plex,Outside) tcp 159.118.x.x 8989 10.10.x.x 8989
static (Plex,Outside) tcp 159.118.x.x 8080 10.10.x.x 8080
static (Plex,Outside) tcp 159.118.x.x 5050 10.10.x.x 5050
Then run the following packet tracer
packet-tracer input outside tcp 11.10.9.8 1025 159.118.x.x 32400
Please provide us the output as we might need to run captures depending on the result.
Regards,
Julio Carvajal
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 11:24 AM
Looks like you are using local address on the access list, in 8.2 you need to use the global ip address.
This changed in after 8.3 release of ASA nat.
br, Micke
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 01:44 PM
Can you give me an example of what you are referring to? The access list is set to allow IP any any, Global or internal.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 01:52 PM
I was referring to these:
access-list OUTSIDE_access_in extended permit udp any host 10.10.X.X eq 20122 log
10.10.x.x should be a 159.118.X.X address if it is access from internet.
Trying to figure out, what the problem is.
br, Micke
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 02:19 PM
Looks like you are using wrong destination ip on the packet tracer. "udp 1.1.1.1 20122 10.1$"
That should at least be 159.1$ at the end.
br, Micke
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 03:44 PM
yes, none of my outside to inside traffic is passing, hence the troubleshooting with packet tracer. Ive seen post with any public ip used and ive seen post with the outside interface ip used... i just included both.
if you look at the PT you notice the rpf check is using the global nat rather than the static nat... I believe this is where the problem is i just dont know how to correct the behavior.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-21-2017 06:29 PM
Hi Scott,
The Packet tracer is not properly written.
I would first recommend you to write the nats from inside to outside.... just to be more organized.
so do
no static (Outside,Plex) tcp 10.10.X.X 32400 159.118.X.X 32400 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 8989 159.118.X.X 8989 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 8080 159.118.X.X 8080 netmask 255.255.255.255
no static (Outside,Plex) tcp 10.10.X.X 5050 159.118.X.X 5050 netmask 255.255.255.255
no static (Outside,Plex) udp 10.10.X.X 22 159.118.X.X 20122 netmask 255.255.255.255
static (Plex,Outside) tcp 159.118.x.x 32400 10.10.x.x 32400
static (Plex,Outside) tcp 159.118.x.x 8989 10.10.x.x 8989
static (Plex,Outside) tcp 159.118.x.x 8080 10.10.x.x 8080
static (Plex,Outside) tcp 159.118.x.x 5050 10.10.x.x 5050
Then run the following packet tracer
packet-tracer input outside tcp 11.10.9.8 1025 159.118.x.x 32400
Please provide us the output as we might need to run captures depending on the result.
Regards,
Julio Carvajal
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-22-2017 01:03 PM
thank you so much .. this worked like a champ!!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-22-2017 01:22 PM
Sweet!
Glad to know that I could help mate
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-24-2017 07:42 PM
I hate to post in the same thread but its still a natting issue and same config.
Ive enabled the other interfaces I need on the ASA but Im not getting traffic between the interfaces as its being dropped by natting restrictions and I need wireless to be able to access the Plex interface. same security level was enabled but its still hitting my global rules.
configs and PT listed below:
same-security-traffic permit inter-interface
global (Outside) 10 interface
nat (WIRELESS) 10 10.10.x.x 255.255.255.0
nat (LAB) 10 10.10.x.x 255.255.255.0
nat (Plex) 10 10.10.x.x 255.255.255.0
Scott-ASA5510-EDGE(config)# packet-tracer input wireless tcp 10.10.x.x 1025 1$
Phase: 1
Type: ROUTE-LOOKUP
Subtype: input
Result: ALLOW
Config:
Additional Information:
in 10.10.x.x 255.255.255.0 Plex
Phase: 2
Type: ACCESS-LIST
Subtype: log
Result: ALLOW
Config:
access-group WIRELESS_access_in in interface WIRELESS
access-list WIRELESS_access_in extended permit ip any any
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xaba4cfc0, priority=12, domain=permit, deny=false
 hits=29142, user_data=0xa8b3f800, cs_id=0x0, flags=0x0, protocol=0
 src ip=0.0.0.0, mask=0.0.0.0, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Phase: 3
Type: IP-OPTIONS
Subtype:
Result: ALLOW
Config:
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xab96b848, priority=0, domain=inspect-ip-options, deny=true
 hits=29873, user_data=0x0, cs_id=0x0, reverse, flags=0x0, protocol=0
 src ip=0.0.0.0, mask=0.0.0.0, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Phase: 4
Type: NAT
Subtype:
Result: DROP
Config:
nat (WIRELESS) 10 10.10.x.x 255.255.255.0
 match ip WIRELESS 10.10.x.x 255.255.255.0 Plex any
 dynamic translation to pool 10 (No matching global)
 translate_hits = 2708, untranslate_hits = 0
Additional Information:
 Forward Flow based lookup yields rule:
 in id=0xacb9cb08, priority=1, domain=nat, deny=false
 hits=2704, user_data=0xacb9ca48, cs_id=0x0, flags=0x0, protocol=0
 src ip=10.10.x.x, mask=255.255.255.0, port=0
 dst ip=0.0.0.0, mask=0.0.0.0, port=0, dscp=0x0
Result:
input-interface: WIRELESS
input-status: up
input-line-status: up
output-interface: Plex
output-status: up
output-line-status: up
Action: drop
Drop-reason: (acl-drop) Flow is denied by configured rule