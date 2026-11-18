Spines not passing traffic: VxLAN EVPN Cisco NX-OS 
        
        
        
    
    
    
      This is driving my a little nuts. I have a spine/leaf configuration using Nexus C93180YC-FX3 in all roles. I'm trying to test a failure scenario (to be fair I haven't tested the steady state yet) and my spines appear to be eating traffic. My NVE interfaces transmit but never receive and ICMP traffic ingresses to the spines but never egresses anywhere. As such, host to host communication is not working.I have 5 vPC switch pairs (10 switches). Member 1 is connected to spine 1 and member 2 is connected to spine 2 for all of them. I'm testing this weird failure state. However, again, I have not actually tested the steady state. I only have two hosts connected. One on switch pair 507/508 and one on switch pair 503/504I'm using OSPF as my underlay and I can see and route to all loopbacks. Leaf switches are using Loopback 1 for NVE source interface and have the anycast IP secondary configured.
    
I added this monitor to spine 1 (coresw501)
monitor session 1  
  source interface Ethernet1/2 both   
  source interface Ethernet1/4 both  
  destination interface sup-eth0  
  no shut  
      Then I ran ethanalyzer and when ICMP goes from host 10.55.4.9 -> 10.55.2.9
    
coresw501(config)# ethanalyzer local interface inband mirror display-filter "icmp" limit-captured-frames 100
Capturing on 'ps-inb'
1     3 2023-09-20 21:45:26.901327766    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=1/256, ttl=64
2     4 2023-09-20 21:45:27.906902294    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=2/512, ttl=64
3     5 2023-09-20 21:45:28.930895980    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=3/768, ttl=64
4     7 2023-09-20 21:45:29.954889685    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=4/1024, ttl=64
5     8 2023-09-20 21:45:30.978902610    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=5/1280, ttl=64
6    10 2023-09-20 21:45:32.002921998    10.55.4.9 → 10.55.2.9    ICMP 148 Echo (ping) request  id=0x0029, seq=6/1536, ttl=64
I don't get any replies on eth1/2 where 10.55.2.9 is. If I start pinging from 10.55.2.9 -> 10.55.4.9 I will see the requests here, but never a reply. I've run tcpdump on my hosts and I can verify I get no ICMP echo request and, as such, there are no ICMP echo replies.
I have the l2routes on the two leaves:
555         6cfe.545d.e870 10.55.2.9                               BGP    --            0         10.254.50.201 (Label: 10555)
555         6cfe.545d.e4f4 10.55.4.9                               HMM    L,            0         Local
switch508#
555         6cfe.545d.e870 10.55.2.9                               HMM    L,            0         Local
555         6cfe.545d.e4f4 10.55.4.9                               BGP    --            0         10.254.50.203 (Label: 10555)
switch503#
The spines can see the routes
coresw501(config)# sho bgp l2vpn evpn 10.55.4.9
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.255.50.107:33322
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272, version 952
Paths: (1 available, best #1)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
  AS-Path: NONE, path sourced internal to AS
    10.254.50.203 (metric 2) from 10.255.50.107 (10.255.50.107)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8
          Router MAC:9088.555c.f31b
  Path-id 1 advertised to peers:
    10.255.50.101      10.255.50.102      10.255.50.103      10.255.50.104
    10.255.50.105      10.255.50.106      10.255.50.108      10.255.50.109
    10.255.50.110
Route Distinguisher: 10.255.50.108:33322
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272, version 955
Paths: (1 available, best #1)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
  AS-Path: NONE, path sourced internal to AS
    10.254.50.203 (metric 2) from 10.255.50.108 (10.255.50.108)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8
          Router MAC:9088.555d.2a93
  Path-id 1 advertised to peers:
    10.255.50.101      10.255.50.102      10.255.50.103      10.255.50.104
    10.255.50.105      10.255.50.106      10.255.50.107      10.255.50.109
    10.255.50.110
coresw501(config)# sho bgp l2vpn evpn 10.55.2.9
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.255.50.103:33322
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e870]:[32]:[10.55.2.9]/272, version 940
Paths: (1 available, best #1)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
  AS-Path: NONE, path sourced internal to AS
    10.254.50.201 (metric 2) from 10.255.50.103 (10.255.50.103)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.201:0 ENCAP:8
          Router MAC:9088.555d.2453
  Path-id 1 advertised to peers:
    10.255.50.101      10.255.50.102      10.255.50.104      10.255.50.105
    10.255.50.106      10.255.50.107      10.255.50.108      10.255.50.109
    10.255.50.110
Route Distinguisher: 10.255.50.104:33322
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e870]:[32]:[10.55.2.9]/272, version 943
Paths: (1 available, best #1)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
  AS-Path: NONE, path sourced internal to AS
    10.254.50.201 (metric 2) from 10.255.50.104 (10.255.50.104)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.201:0 ENCAP:8
          Router MAC:9088.555d.257f
  Path-id 1 advertised to peers:
    10.255.50.101      10.255.50.102      10.255.50.103      10.255.50.105
    10.255.50.106      10.255.50.107      10.255.50.108      10.255.50.109
    10.255.50.110
coresw501(config)#
The switch pairs each see l2 routes:
switch503# show bgp l2vpn evpn 10.55.4.9 vrf vxlan_prod
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.255.50.103:6    (L3VNI 50999)
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272, version 2030
Paths: (2 available, best #2)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Path type: internal, path is valid, not best reason: Router Id, no labeled nexthop
             Imported from 10.255.50.108:33322:[2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272
  AS-Path: NONE, path sourced internal to AS
    10.254.50.203 (metric 3) from 10.255.50.1 (1.1.1.1)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8
          Router MAC:9088.555d.2a93
      Originator: 10.255.50.108 Cluster list: 1.1.1.1
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
             Imported from 10.255.50.107:33322:[2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272
  AS-Path: NONE, path sourced internal to AS
    10.254.50.203 (metric 3) from 10.255.50.1 (1.1.1.1)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8
          Router MAC:9088.555c.f31b
      Originator: 10.255.50.107 Cluster list: 1.1.1.1
  Path-id 1 not advertised to any peer
switch503#
switch507# show bgp l2vpn evpn 10.55.2.9 vrf vxlan_prod
BGP routing table information for VRF default, address family L2VPN EVPN
Route Distinguisher: 10.255.50.107:6    (L3VNI 50999)
BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e870]:[32]:[10.55.2.9]/272, version 2056
Paths: (2 available, best #2)
Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW
  Path type: internal, path is valid, not best reason: Router Id, no labeled nexthop
             Imported from 10.255.50.104:33322:[2]:[0]:[0]:[48]:[6cfe.545d.e870]:[32]:[10.55.2.9]/272
  AS-Path: NONE, path sourced internal to AS
    10.254.50.201 (metric 3) from 10.255.50.1 (1.1.1.1)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.201:0 ENCAP:8
          Router MAC:9088.555d.257f
      Originator: 10.255.50.104 Cluster list: 1.1.1.1
  Advertised path-id 1
  Path type: internal, path is valid, is best path, no labeled nexthop
             Imported from 10.255.50.103:33322:[2]:[0]:[0]:[48]:[6cfe.545d.e870]:[32]:[10.55.2.9]/272
  AS-Path: NONE, path sourced internal to AS
    10.254.50.201 (metric 3) from 10.255.50.1 (1.1.1.1)
      Origin IGP, MED not set, localpref 100, weight 0
      Received label 10555 50999
      Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.201:0 ENCAP:8
          Router MAC:9088.555d.2453
      Originator: 10.255.50.103 Cluster list: 1.1.1.1
  Path-id 1 not advertised to any peer
switch507#
The NVE interface is clearly transmitting but NOT receiving anything:
switch507# sho int nve1
nve1 is up
admin state is up,  Hardware: NVE
  MTU 9216 bytes
  Encapsulation VXLAN
  Auto-mdix is turned off
  RX
    ucast: 0 pkts, 0 bytes - mcast: 103 pkts, 14098 bytes
  TX
    ucast: 29825 pkts, 4528536 bytes - mcast: 1851 pkts, 220684 bytes
switch507#
switch503# sh int nve1
nve1 is up
admin state is up,  Hardware: NVE
  MTU 9216 bytes
  Encapsulation VXLAN
  Auto-mdix is turned off
  RX
    ucast: 0 pkts, 0 bytes - mcast: 98 pkts, 13436 bytes
  TX
    ucast: 17998 pkts, 2735696 bytes - mcast: 1684 pkts, 199586 bytes
switch503#
I feel like I'm missing really simple. I believe I have the appropriate features enabled on the leaves and spines and the BGP configs are dead simple.
leaf:
switch507# sho run | grep overlay
nv overlay evpn
feature nv overlay
switch507# sho run | sec feature
feature vrrp
feature ospf
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature dhcp
feature vpc
feature nv overlay
switch507#
spine:
coresw501(config)# sho run | sec feature
feature ospf
feature bgp
feature fabric forwarding
feature nv overlay
coresw501(config)# sho run | grep overlay
nv overlay evpn
feature nv overlay
coresw501(config)#
I have far less hair than when I started this (especially since this same basic config is working in CML 2.0 with NX9000v.... *sigh*
Thanks for any and all help that you can provide!
EDIT: Formatting. Why is it so bad? It look right on the original post I was making.. GAH!
Section des commentaires
I Dumped the ICMP packet at the spine and I definitely see the VNI and expected src/dst.
Ethernet II, Src: 90:88:55:5d:24:53, Dst: 24:2a:04:c0:32:f3 Destination: 24:2a:04:c0:32:f3 Address: 24:2a:04:c0:32:f3 .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default) .... ...0 .... .... .... .... = IG bit: Individual address (unicast) Source: 90:88:55:5d:24:53 Address: 90:88:55:5d:24:53 .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default) .... ...0 .... .... .... .... = IG bit: Individual address (unicast) Type: IPv4 (0x0800) Internet Protocol Version 4, Src: 10.254.50.201, Dst: 10.254.50.203 0100 .... = Version: 4 .... 0101 = Header Length: 20 bytes (5) Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT) 0000 00.. = Differentiated Services Codepoint: Default (0) .... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0) Total Length: 134 Identification: 0x55aa (21930) Flags: 0x0000 0... .... .... .... = Reserved bit: Not set .0.. .... .... .... = Don't fragment: Not set ..0. .... .... .... = More fragments: Not set Fragment offset: 0 Time to live: 255 Protocol: UDP (17) Header checksum: 0xea2c [validation disabled] [Header checksum status: Unverified] Source: 10.254.50.201 Destination: 10.254.50.203 User Datagram Protocol, Src Port: 9834, Dst Port: 4789 Source Port: 9834 Destination Port: 4789 Length: 114 [Checksum: [missing]] [Checksum Status: Not present] [Stream index: 0] [Timestamps] [Time since first frame: 5.141114815 seconds] [Time since previous frame: 1.024004944 seconds] Virtual eXtensible Local Area Network Flags: 0x0800, VXLAN Network ID (VNI) 0... .... .... .... = GBP Extension: Not defined .... .... .0.. .... = Don't Learn: False .... 1... .... .... = VXLAN Network ID (VNI): True .... .... .... 0... = Policy Applied: False .000 .000 0.00 .000 = Reserved(R): 0x0000 Group Policy ID: 0 VXLAN Network Identifier (VNI): 10555 Reserved: 0 Ethernet II, Src: 6c:fe:54:5d:e8:70, Dst: 6c:fe:54:5d:e4:f4 Destination: 6c:fe:54:5d:e4:f4 Address: 6c:fe:54:5d:e4:f4 .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default) .... ...0 .... .... .... .... = IG bit: Individual address (unicast) Source: 6c:fe:54:5d:e8:70 Address: 6c:fe:54:5d:e8:70 .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default) .... ...0 .... .... .... .... = IG bit: Individual address (unicast) Type: IPv4 (0x0800)
Spine can definitely see routes to the src/dest on the outer IP header:
IP Route Table for VRF "default" '*' denotes best ucast next-hop '**' denotes best mcast next-hop '[x/y]' denotes [preference/metric] '%<string>' in via output denotes VRF <string> 10.254.50.203/32, ubest/mbest: 1/0 *via 10.255.50.107, Eth1/4.1530, [110/2], 1d07h, ospf-UNDERLAY, intra coresw501# show ip route 10.254.50.201 IP Route Table for VRF "default" '*' denotes best ucast next-hop '**' denotes best mcast next-hop '[x/y]' denotes [preference/metric] '%<string>' in via output denotes VRF <string> 10.254.50.201/32, ubest/mbest: 1/0 *via 10.255.50.103, Eth1/2.1530, [110/2], 1d07h, ospf-UNDERLAY, intra coresw501#
Src and Dest MACs in the Ethernet header under the VxLAN header are in BGP:
coresw501# show bgp l2vpn evpn 6c:fe:54:5d:e4:f4 BGP routing table information for VRF default, address family L2VPN EVPN Route Distinguisher: 10.255.50.107:33322 BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[0]:[0.0.0.0]/216, version 933 Paths: (1 available, best #1) Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW Advertised path-id 1 Path type: internal, path is valid, is best path, no labeled nexthop AS-Path: NONE, path sourced internal to AS 10.254.50.203 (metric 2) from 10.255.50.107 (10.255.50.107) Origin IGP, MED not set, localpref 100, weight 0 Received label 10555 Extcommunity: RT:64550:10555 SOO:10.254.50.203:0 ENCAP:8 Path-id 1 advertised to peers: 10.255.50.101 10.255.50.102 10.255.50.103 10.255.50.104 10.255.50.105 10.255.50.106 10.255.50.108 10.255.50.109 10.255.50.110 BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272, version 952 Paths: (1 available, best #1) Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW Advertised path-id 1 Path type: internal, path is valid, is best path, no labeled nexthop AS-Path: NONE, path sourced internal to AS 10.254.50.203 (metric 2) from 10.255.50.107 (10.255.50.107) Origin IGP, MED not set, localpref 100, weight 0 Received label 10555 50999 Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8 Router MAC:9088.555c.f31b Path-id 1 advertised to peers: 10.255.50.101 10.255.50.102 10.255.50.103 10.255.50.104 10.255.50.105 10.255.50.106 10.255.50.108 10.255.50.109 10.255.50.110 Route Distinguisher: 10.255.50.108:33322 BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[0]:[0.0.0.0]/216, version 934 Paths: (1 available, best #1) Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW Advertised path-id 1 Path type: internal, path is valid, is best path, no labeled nexthop AS-Path: NONE, path sourced internal to AS 10.254.50.203 (metric 2) from 10.255.50.108 (10.255.50.108) Origin IGP, MED not set, localpref 100, weight 0 Received label 10555 Extcommunity: RT:64550:10555 SOO:10.254.50.203:0 ENCAP:8 Path-id 1 advertised to peers: 10.255.50.101 10.255.50.102 10.255.50.103 10.255.50.104 10.255.50.105 10.255.50.106 10.255.50.107 10.255.50.109 10.255.50.110 BGP routing table entry for [2]:[0]:[0]:[48]:[6cfe.545d.e4f4]:[32]:[10.55.4.9]/272, version 955 Paths: (1 available, best #1) Flags: (0x000202) (high32 00000000) on xmit-list, is not in l2rib/evpn, is not in HW Advertised path-id 1 Path type: internal, path is valid, is best path, no labeled nexthop AS-Path: NONE, path sourced internal to AS 10.254.50.203 (metric 2) from 10.255.50.108 (10.255.50.108) Origin IGP, MED not set, localpref 100, weight 0 Received label 10555 50999 Extcommunity: RT:64550:10555 RT:64550:50999 SOO:10.254.50.203:0 ENCAP:8 Router MAC:9088.555d.2a93 Path-id 1 advertised to peers: 10.255.50.101 10.255.50.102 10.255.50.103 10.255.50.104 10.255.50.105 10.255.50.106 10.255.50.107 10.255.50.109 10.255.50.110 coresw501#
Packet has correct VNI value in the VxLAN header:
Virtual eXtensible Local Area Network Flags: 0x0800, VXLAN Network ID (VNI) 0... .... .... .... = GBP Extension: Not defined .... .... .0.. .... = Don't Learn: False .... 1... .... .... = VXLAN Network ID (VNI): True .... .... .... 0... = Policy Applied: False .000 .000 0.00 .000 = Reserved(R): 0x0000 Group Policy ID: 0 VXLAN Network Identifier (VNI): 10555 Reserved: 0
Srsly.. wtaf? It just ... never leaves the spine.
EDIT: formatting. again...
I should probably mention I'm using dot1q sub interfaces. I'm using NXOS version 10.2(5) which I *believe* supports VxLAN over subinterfaces. I'm pretty sure that was a limitation before NXOS version 9.3(5) based on this bit in the docs:
The following limitations apply to releases prior to Cisco NX-OS Release 9.3(5):
VTEPs do not support VXLAN-encapsulated traffic over subinterfaces, regardless of VRF participation or IEEE 802.1Q encapsulation.
VTEPs do not support VXLAN-encapsulated traffic over parent interfaces if subinterfaces are configured, regardless of VRF participation.
Mixing subinterfaces for VXLAN and non-VXLAN VLANs is not supported.
coresw501# show ip route 10.254.50.203 IP Route Table for VRF "default" '*' denotes best ucast next-hop '**' denotes best mcast next-hop '[x/y]' denotes [preference/metric] '%<string>' in via output denotes VRF <string> 10.254.50.203/32, ubest/mbest: 1/0 *via 10.255.50.107, Eth1/4.1530, [110/2], 1d07h, ospf-UNDERLAY, intra coresw501# show ip route 10.254.50.201 IP Route Table for VRF "default" '*' denotes best ucast next-hop '**' denotes best mcast next-hop '[x/y]' denotes [preference/metric] '%<string>' in via output denotes VRF <string> 10.254.50.201/32, ubest/mbest: 1/0 *via 10.255.50.103, Eth1/2.1530, [110/2], 1d07h, ospf-UNDERLAY, intra coresw501# sho run int eth1/4.1530 !Command: show running-config interface Ethernet1/4.1530 !Running configuration last done at: Wed Sep 20 21:42:55 2023 !Time: Thu Sep 21 01:34:39 2023 version 10.2(5) Bios:version 05.47 interface Ethernet1/4.1530 description switch507-eth1/50.1530 mtu 9216 encapsulation dot1q 1530 medium p2p ip unnumbered loopback0 ip ospf network point-to-point ip router ospf UNDERLAY area 0.0.0.0 no shutdown coresw501# sho run int eth1/2.1530 !Command: show running-config interface Ethernet1/2.1530 !Running configuration last done at: Wed Sep 20 21:42:55 2023 !Time: Thu Sep 21 01:34:46 2023 version 10.2(5) Bios:version 05.47 interface Ethernet1/2.1530 description switch503-eth1/50.1530 mtu 9216 encapsulation dot1q 1530 medium p2p ip unnumbered loopback0 ip ospf network point-to-point ip router ospf UNDERLAY area 0.0.0.0 no shutdown coresw501#
Hello!
Utilizing 802.1Q subinterfaces as a VXLAN uplink on VTEPs is still not supported on any NX-OS software release. This is supported by the following statement from the Cisco Nexus 9000 Series NX-OS VXLAN Configuration Guide, Release 10.2(x) document, with the relevant clause bolded:
"Starting from Cisco NX-OS Release 9.3(5), new VXLAN uplink capabilities are introduced:
A physical interface in default VRF is supported as VXLAN uplink.
A parent interface in default VRF, carrying subinterfaces with VRF and dot1q tags, is supported as VXLAN uplink.
A subinterface in any VRF and/or with dot1q tag remains not supported as VXLAN uplink.
An SVI in any VRF remains not supported as VXLAN uplink.
In vPC with physical peer-link, a SVI can be leveraged as backup underlay, default VRF only between the vPC members (infra-VLAN, system nve infra-vlans).
On a vPC pair, shutting down NVE or NVE loopback on one of the vPC nodes is not a supported configuration. This means that traffic failover on one-side NVE shut or one-side loopback shut is not supported.
FEX host interfaces remain not supported as VXLAN uplink and cannot have VTEPs connected (BUD node)."
This may not be the only root cause of your issues, but it will certainly cause you issues down the road. I would highly recommend you refactor your configuration such that subinterfaces are not used as a VXLAN uplink.
To give some background details, this is a hardware limitation with the Cloud Scale ASIC leveraged by modern Nexus 9000 series switches. The phrase I used to say is Cloud Scale can't "double-encap" - basically, the ASIC physically cannot perform two encapsulation actions on a frame/packet in a single pass through the ASIC, and the ASIC is not able to internally re-circulate the packet through the ASIC such that it can perform multiple passes on a single frame/packet. In this scenario, both the VXLAN header and the 802.1Q tag required by the subinterface count as two separate encapsulations.
I believe you see evidence of this in your SPAN-to-CPU. The detailed Ethanalyzer output you shared indicates that there is no 802.1Q header between the outer Ethernet header and the outer IPv4 header. This is because the ASIC physically cannot insert the 802.1Q header after having already encapsulated your native IPv4 packet with a VXLAN header. The spine is most likely receiving this packet and dropping it on ingress.
(Note that I believe this is the case - I'm 97.3% sure that a packet punted to the CPU via SPAN-to-CPU will retain the 802.1Q header [such that it's visible in detailed Ethanalyzer output], but I would need to test that just to be absolutely positive.)
Another key piece of evidence that suggests the spine is dropping this packet on ingress (meaning, it's not forwarding it out to the leaf) is in your non-detailed Ethanalyzer output:
Recall you have the following SPAN-to-CPU configuration:
With this configuration, if you have a packet ingress Ethernet1/2 and successfully egress Ethernet1/4, I would expect to see the same packet twice in the Ethanalyzer output - once as it ingresses Ethernet1/2, and once as it egresses Ethernet1/4. That's not the case here, because we see each ICMP Echo sequence number once, and the timestamps are very evenly spaced out with one second in between. I'd expect to see sub-millisecond differences in timestamps if the packet was being duplicated on ingress and egress, so it does feel like the spine is dropping this packet on ingress.
A "fun fact" - this same double-encap hardware limitation is also the root cause for a number of limitations with other features (e.g. why you cannot ERSPAN towards an IP where the egress interface is a tunnel, why you cannot configure Q-in-Q such that a frame ingresses with no 802.1Q headers and egresses with two 802.1Q headers, etc.)
As a side note, this likely worked just fine with Nexus 9000v because in 9000v, all of the forwarding is done in software. There's no ASIC/hardware for forwarding to be delegated to on a 9000v, and therefore, you cannot encounter hardware-specific limitations like this one when using 9000v (although interestingly, the lack of hardware/TCAM is the root cause of why some features aren't supported on 9000v).
Lastly, just to clear up any confusion, let's address the documentation you originally quoted:
This was taken from the Cisco Nexus 9000 Series NX-OS VXLAN Configuration Guide, Release 9.3(x) document. However, the bullet point immediately following this statement is as follows:
So, the intent of these two bullet points is to state that before 9.3(5), you could not have non-VXLAN traffic egress a subinterface if the parent interface was a VXLAN uplink. However, this capability was introduced starting with 9.3(5), but you still cannot forward VXLAN traffic over a subinterface due to the aforementioned hardware limitation.
I hope this helps clear things up - let me know if you have any questions!
This was, indeed, the issue. Thank you so much for this detailed response! So great!
VXLAN/EVPN is the biggest scam network vendors managed to sell to their customers.
What.. Why?
lol wut