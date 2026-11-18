AR611-S, AR611W-S, AR611E-S, AR611-LTE4EA, AR611, AR611W, AR611W-LTE4CN, AR611W-LTE6EA, AR617VW, AR617VW-LTE4, AR617VW-LTE4EA, and AR651C do not support this function.
The RU-5G-101 does not support this function.
An ISP network provides the L2VPN service for users. Many users connect to the MPLS network through PE1 and PE2, and users on the PEs change frequently. A proper VPN solution is required to provide secure VPN services for users and to simplify configuration when new users connect to the network.
A Martini VLL connection can be set up between CE1 and CE2 to meet these requirements. By default, the system uses Label Switched Paths (LSPs) for Martini VLL, and does not perform load balancing. When the P does not provide MPLS functions, VLL cannot be implemented.
To solve the problem, apply a tunnel policy to Martini VLL to specify that VLL services are transmitted over a GRE tunnel.
The configuration roadmap is as follows:
Configure a routing protocol on the PE and P devices on the backbone network to ensure reachability between them.
Enable MPLS and MPLS LDP on PEs. Set up a remote LDP session between the PEs to exchange VC labels between the PEs.
Enable MPLS L2VPN on PEs. Enabling MPLS L2VPN is the prerequisite for VLL configuration.
Create GRE tunnel interfaces on PEs and establish a GRE tunnel between PEs.
Create VC connections on PEs. Because the P does not support MPLS functions, configure a tunnel policy and apply it when you create VC connections so that VLL services can be transmitted over a GRE tunnel.
# Configure PE1. The configurations of PE2 and P are similar to the configuration of PE1, and are not mentioned here.
<Huawei> system-view
[Huawei] sysname PE1
[PE1] interface gigabitethernet 2/0/0
[PE1-GigabitEthernet2/0/0] ip address 172.1.1.1 255.255.255.0
[PE1-GigabitEthernet2/0/0] quit
[PE1] interface loopback 1
[PE1-LoopBack1] ip address 10.10.1.1 255.255.255.255
[PE1-LoopBack1] quit
[PE1] ospf 1
[PE1-ospf-1] area 0
[PE1-ospf-1-area-0.0.0.0] network 172.1.1.0 0.0.0.255
[PE1-ospf-1-area-0.0.0.0] network 10.10.1.1 0.0.0.0
[PE1-ospf-1-area-0.0.0.0] quit
[PE1-ospf-1] quit
After the configurations are complete, OSPF neighbor relationships can be set up between PE1, P, and PE2. Run the display ospf peer command. You can see that the neighbor status is Full. Run the display ip routing-table command. You can see that PEs have learnt the routes to Loopback1 of each other.
# Configure PE1.
[PE1] mpls lsr-id 10.10.1.1
[PE1] mpls
[PE1-mpls] quit
[PE1] mpls ldp
[PE1-mpls-ldp] quit
[PE1] mpls ldp remote-peer 10.10.2.1
[PE1-mpls-ldp-remote-10.10.2.1] remote-ip 10.10.2.1
[PE1-mpls-ldp-remote-10.10.2.1] quit
# Configure PE2.
[PE2] mpls lsr-id 10.10.2.1
[PE2] mpls
[PE2-mpls] quit
[PE2] mpls ldp
[PE2-mpls-ldp] quit
[PE2] mpls ldp remote-peer 10.10.1.1
[PE2-mpls-ldp-remote-10.10.1.1] remote-ip 10.10.1.1
[PE2-mpls-ldp-remote-10.10.1.1] quit
After the configurations are complete, run the display mpls ldp session command on PE1 to view the LDP session status. You can see that an LDP session is set up between PE1 and PE2.
The display on PE1 is used as an example.
[PE1] display mpls ldp session
                                                                                
 LDP Session(s) in Public Network                                               
 Codes: LAM(Label Advertisement Mode), SsnAge Unit(DDDD:HH:MM)                  
 A '*' before a session means the session is being deleted.                     
 ------------------------------------------------------------------------------ 
 PeerID             Status      LAM  SsnRole  SsnAge      KASent/Rcv            
 ------------------------------------------------------------------------------ 
 10.10.2.1:0          Operational DU   Passive  0000:00:01  1/1                   
 ------------------------------------------------------------------------------ 
 TOTAL: 1 session(s) Found.                                                     
[PE1] mpls l2vpn
[PE1-l2vpn] quit
[PE2] mpls l2vpn
[PE2-l2vpn] quit
[PE1] interface tunnel 0/0/1
[PE1-Tunnel0/0/1] ip address 10.2.1.1 255.255.255.0 
[PE1-Tunnel0/0/1] tunnel-protocol gre
[PE1-Tunnel0/0/1] source 10.10.1.1
[PE1-Tunnel0/0/1] destination 10.10.2.1
[PE1-Tunnel0/0/1] quit
[PE2] interface tunnel 0/0/1
[PE2-Tunnel0/0/1] ip address 10.2.1.2 255.255.255.0 
[PE2-Tunnel0/0/1] tunnel-protocol gre
[PE2-Tunnel0/0/1] source 10.10.2.1
[PE2-Tunnel0/0/1] destination 10.10.1.1
[PE2-Tunnel0/0/1] quit
After the configurations are complete, the tunnel interfaces go Up and can ping each other.
[PE1] ping -a 10.2.1.1 10.2.1.2
  PING 10.2.1.2: 56  data bytes, press CTRL_C to break        
    Reply from 10.2.1.2: bytes=56 Sequence=1 ttl=255 time=1 ms
    Reply from 10.2.1.2: bytes=56 Sequence=2 ttl=255 time=1 ms
    Reply from 10.2.1.2: bytes=56 Sequence=3 ttl=255 time=1 ms
    Reply from 10.2.1.2: bytes=56 Sequence=4 ttl=255 time=1 ms
    Reply from 10.2.1.2: bytes=56 Sequence=5 ttl=255 time=1 ms
                                                              
  --- 10.2.1.2 ping statistics ---                            
    5 packet(s) transmitted                                   
    5 packet(s) received                                      
    0.00% packet loss                                         
    round-trip min/avg/max = 1/1/1 ms
[PE1] tunnel-policy gre1
[PE1-tunnel-policy-gre1] tunnel select-seq gre load-balance-number 1
[PE1-tunnel-policy-gre1] quit
[PE1] interface gigabitethernet 1/0/0
[PE1-GigabitEthernet1/0/0] mpls l2vc 10.10.2.1 39 tunnel-policy gre1
[PE1-GigabitEthernet1/0/0] quit
[PE2] tunnel-policy gre1
[PE2-tunnel-policy-gre1] tunnel select-seq gre load-balance-number 1
[PE2-tunnel-policy-gre1] quit
[PE2] interface gigabitethernet 2/0/0
[PE2-GigabitEthernet2/0/0] mpls l2vc 10.10.1.1 39 tunnel-policy gre1
[PE2-GigabitEthernet2/0/0] quit
# After the configurations are complete, check the L2VPN connection on PEs. You can see that an L2VC connection has been set up and is in Up state.
[PE1] display mpls l2vc interface gigabitethernet 1/0/0
 *client interface       : GigabitEthernet1/0/0 is up
  Administrator PW       : no
  session state          : up
  AC status              : up
  Ignore AC state        : disable
  VC state               : up
  Label state            : 0
  Token state            : 0
  VC ID                  : 39
  VC type                : Ethernet
  destination            : 10.10.2.1
  local group ID         : 0            remote group ID      : 0
  local VC label         : 1025         remote VC label      : 1024
  local AC OAM State     : up
  local PSN OAM State    : up
  local forwarding state : forwarding
  local status code      : 0x0
  remote AC OAM state    : up
  remote PSN OAM state   : up
  remote forwarding state: forwarding
  remote status code     : 0x0
  ignore standby state   : no
  BFD for PW             : unavailable
  VCCV State             : up
  manual fault           : not set
  active state           : active
  forwarding entry       : exist
  link state             : up
  local VC MTU           : 1500         remote VC MTU        : 1500
  local VCCV             : alert ttl lsp-ping bfd 
  remote VCCV            : alert ttl lsp-ping bfd 
  local control word     : disable      remote control word  : disable
  tunnel policy name     : gre1
  PW template name       : --
  primary or secondary   : primary
  load balance type      : flow
  Access-port            : false
  Switchover Flag        : false
  VC tunnel/token info   : 1 tunnels/tokens
    NO.0  TNL type       : gre   , TNL ID : 0x2
    Backup TNL type      : lsp   , TNL ID : 0x0
  create time            : 0 days, 2 hours, 37 minutes, 1 seconds
  up time                : 0 days, 0 hours, 2 minutes, 11 seconds
  last change time       : 0 days, 0 hours, 2 minutes, 11 seconds
  VC last up time        : 2013/02/20 18:58:24
  VC total up time       : 0 days, 2 hours, 35 minutes, 58 seconds
  CKey                   : 2
  NKey                   : 1
  PW redundancy mode     : frr
  AdminPw interface      : --
  AdminPw link state     : --
  Diffserv Mode          : uniform
  Service Class          : --
  Color                  : --
  DomainId               : --
  Domain Name            : --
# Run the display tunnel-info tunnel-id command on PEs according to the tunnel ID in the preceding command output. You can view details of the specified tunnel ID.
[PE1] display tunnel-info tunnel-id 2
Tunnel ID:                    0x2
Tunnel Token:                 2
Type:                         gre
Destination:                  10.10.2.1
Out Slot:                     0
Instance ID:                  0
Interface:                    Tunnel0/0/1
# CE1 and CE2 can ping each other successfully.
# The display on CE1 is used as an example.
[CE1] ping 10.1.1.2
  PING 10.1.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.1.1.2: bytes=56 Sequence=1 ttl=255 time=31 ms
    Reply from 10.1.1.2: bytes=56 Sequence=2 ttl=255 time=10 ms
    Reply from 10.1.1.2: bytes=56 Sequence=3 ttl=255 time=5 ms
    Reply from 10.1.1.2: bytes=56 Sequence=4 ttl=255 time=2 ms
    Reply from 10.1.1.2: bytes=56 Sequence=5 ttl=255 time=28 ms
  --- 10.1.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 2/15/31 ms 
Configuration file of CE1
#
 sysname CE1
#
interface GigabitEthernet1/0/0
 ip address 10.1.1.1 255.255.255.0
#
return
Configuration file of PE1
#
 sysname PE1
#
mpls lsr-id 10.10.1.1
mpls
#
mpls l2vpn
#
mpls ldp
#
mpls ldp remote-peer 10.10.2.1
 remote-ip 10.10.2.1
#
interface GigabitEthernet1/0/0
 mpls l2vc 10.10.2.1 39 tunnel-policy gre1
#
interface GigabitEthernet2/0/0
 ip address 172.1.1.1 255.255.255.0
#
interface LoopBack1
 ip address 10.10.1.1 255.255.255.255
#
interface Tunnel0/0/1
 ip address 10.2.1.1 255.255.255.0
 tunnel-protocol gre
 source 10.10.1.1
 destination 10.10.2.1
# 
ospf 1
 area 0.0.0.0
  network 10.10.1.1 0.0.0.0
  network 172.1.1.0 0.0.0.255
#
tunnel-policy gre1 
 tunnel select-seq gre load-balance-number 1
#
return
Configuration file of P
#
 sysname P
#
interface GigabitEthernet2/0/0
 ip address 172.1.1.2 255.255.255.0
#
interface GigabitEthernet1/0/0
 ip address 172.2.1.2 255.255.255.0
#
ospf 1
 area 0.0.0.0
  network 172.1.1.0 0.0.0.255
  network 172.2.1.0 0.0.0.255
#
return
Configuration file of PE2
#
 sysname PE2
#
mpls lsr-id 10.10.2.1
mpls
#
mpls l2vpn
#
mpls ldp
#
mpls ldp remote-peer 10.10.1.1
 remote-ip 10.10.1.1
#
interface GigabitEthernet1/0/0
 ip address 172.2.1.1 255.255.255.0
#
interface GigabitEthernet2/0/0
 mpls l2vc 10.10.1.1 39 tunnel-policy gre1
#
interface LoopBack1
 ip address 10.10.2.1 255.255.255.255
#
interface Tunnel0/0/1
 ip address 10.2.1.2 255.255.255.0
 tunnel-protocol gre
 source 10.10.2.1
 destination 10.10.1.1
#
ospf 1
 area 0.0.0.0
  network 10.10.2.1 0.0.0.0
  network 172.2.1.0 0.0.0.255
#
tunnel-policy gre1 
 tunnel select-seq gre load-balance-number 1
#
return
Configuration file of CE2
#
 sysname CE2
#
interface GigabitEthernet1/0/0
 ip address 10.1.1.2 255.255.255.0
#
return