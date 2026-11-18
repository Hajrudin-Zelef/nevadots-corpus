In Figure 1, Router1 and Router2 are the branch and headquarters gateways of an enterprise. As users in the headquarters and branch have different service requirements, they are planned in different network segments. PC_1 in the branch and PC_2 in the headquarters belong to VLAN 10 and VLAN 20, respectively. The enterprise requires that users in the headquarters and branch can communicate over a VXLAN tunnel dynamically established using BGP EVPN.
On AR routers, the ingress replication list cannot be dynamically discovered using BGP EVPN. To implement VXLAN using BGP EVPN on AR routers, you can only run the vni vni-id head-end peer-list ip-address &<1-10> command in the NVE interface view to configure a VXLAN network identifier (VNI) ingress replication list.
The configuration roadmap is as follows:
# Configure Router1. The configurations of Router2 and Router3 are similar to that of Router1, and are not mentioned here. When OSPF is used, the 32-bit loopback address of each router must be advertised.
<Huawei> system-view
[Huawei] sysname Router1
[Router1] interface loopback 1
[Router1-LoopBack1] ip address 10.1.1.2 32
[Router1-LoopBack1] quit
[Router1] interface ethernet 2/0/0
[Router1-Ethernet2/0/0] ip address 192.168.2.1 24
[Router1-Ethernet2/0/0] quit
[Router1] ospf 1 router-id 10.1.1.2
[Router1-ospf-1] area 0
[Router1-ospf-1-area-0.0.0.0] network 10.1.1.2 0.0.0.0
[Router1-ospf-1-area-0.0.0.0] network 192.168.2.0 0.0.0.255
[Router1-ospf-1-area-0.0.0.0] quit
[Router1-ospf-1] quit
# After OSPF is configured, the routers can learn the loopback interface address of each other and successfully ping each other. The following shows the ping result from Router1 to Router2.
[Router1] ping 10.2.2.2
  PING 10.2.2.2: 56  data bytes, press CTRL_C to break                     
    Reply from 10.2.2.2: bytes=56 Sequence=1 ttl=255 time=1 ms             
    Reply from 10.2.2.2: bytes=56 Sequence=2 ttl=255 time=5 ms             
    Reply from 10.2.2.2: bytes=56 Sequence=3 ttl=255 time=5 ms             
    Reply from 10.2.2.2: bytes=56 Sequence=4 ttl=255 time=2 ms             
    Reply from 10.2.2.2: bytes=56 Sequence=5 ttl=255 time=2 ms             
                                                                                
  --- 10.2.2.2 ping statistics ---                                         
    5 packet(s) transmitted                                                     
    5 packet(s) received                                                        
    0.00% packet loss                                                           
    round-trip min/avg/max = 1/3/5 ms                                           
                                                                                
# Configure Router1. The configuration of Router2 is similar to that of Router1, and is not mentioned here.
[Router1] bridge-domain 10
[Router1-bd10] quit
[Router1] interface ethernet 2/0/1.1 mode l2
[Router1-Ethernet2/0/1.1] encapsulation dot1q vid 10
[Router1-Ethernet2/0/1.1] bridge-domain 10
[Router1-Ethernet2/0/1.1] quit
# Establish a BGP EVPN peer relationship on Router1. The configuration of Router2 is similar to that of Router1, and is not mentioned here.
[Router1] bgp 100
[Router1-bgp] router-id 10.1.1.2
[Router1-bgp] peer 10.2.2.2 as-number 100
[Router1-bgp] peer 10.2.2.2 connect-interface LoopBack1
[Router1-bgp] l2vpn-family evpn
[Router1-bgp-af-evpn] peer 10.2.2.2 enable
[Router1-bgp-af-evpn] quit
[Router1-bgp] quit
[Router1] interface nve 1
[Router1-Nve1] source 10.1.1.2
[Router1-Nve1] vni 2010 head-end peer-list 10.2.2.2
[Router1-Nve1] quit
[Router1] ip vpn-instance vpn1
[Router1-vpn-instance-vpn1] ipv4-family
[Router1-vpn-instance-vpn1-af-ipv4] route-distinguisher 100:1
[Router1-vpn-instance-vpn1-af-ipv4] vpn-target 1:1 evpn
[Router1-vpn-instance-vpn1-af-ipv4] quit
[Router1-vpn-instance-vpn1] vxlan vni 5010
[Router1-vpn-instance-vpn1] quit
[Router1] bridge-domain 10
[Router1-bd10] vxlan vni 2010
[Router1-bd10] quit
[Router1] interface vbdif 10
[Router1-Vbdif10] ip binding vpn-instance vpn1
[Router1-Vbdif10] ip address 192.168.10.10 24
[Router1-Vbdif10] quit
[Router1] bgp 100
[Router1-bgp] ipv4-family vpn-instance vpn1
[Router1-bgp-vpn1] import-route direct
[Router1-bgp-vpn1] advertise l2vpn evpn
[Router1-bgp-vpn1] quit
[Router1-bgp] quit
# After the configuration is complete, run the display vxlan tunnel command on Router1 and Router2 to view VXLAN tunnel information. The command output on Router1 is used as an example.
[Router1] display vxlan tunnel
 Tunnel ID       Source              Destination         State     Type         
 ----------------------------------------------------------------------------   
 4               10.1.1.2            10.2.2.2            up        l3 dynamic      
  ----------------------------------------------------------------------------   
 Number of vxlan tunnel : 2  
# After the configuration is complete, run the display vxlan vni command on Router1 and Router2, view the VNI status is up. The command output on Router1 is used as an example.
<Router> display vxlan vni 2010 verbose  
BD ID               :10                                                          
State               :up                                                          
Source              :10.1.1.2     
Source IPv6 Address :-                                                 
UDP Port            :4789                                                        
Peer List           :10.2.2.2     
IPv6 Peer List      :-                    
Router1 configuration file
#
sysname Router1
#                                                                               
ip vpn-instance vpn1                                                            
 ipv4-family                                                                    
  route-distinguisher 100:1                                                    
  vpn-target 1:1 export-extcommunity evpn                                       
  vpn-target 1:1 import-extcommunity evpn                                       
 vxlan vni 5010                                                                 
#
bridge-domain 10                                                                
 vxlan vni 2010
#                                                                               
interface Ethernet2/0/0                                                                                                                         
 ip address 192.168.2.1 255.255.255.0                                           
#                                                                               
interface Ethernet2/0/1.1 mode l2                                               
 encapsulation dot1q vid 10                                                     
 bridge-domain 10
#                                                                               
interface LoopBack1                                                             
 ip address 10.1.1.2 255.255.255.255  
#                                                                               
interface Vbdif10                                                               
 ip binding vpn-instance vpn1                                                   
 ip address 192.168.10.10 255.255.255.0                                         
#                                                                               
interface Nve1                                                                  
 source 10.1.1.2   
 vni 2010 head-end peer-list 10.2.2.2                                                             
#                                                                               
bgp 100  
 router-id 10.1.1.2                                                                       
 peer 10.2.2.2 as-number 100                                                     
 peer 10.2.2.2 connect-interface LoopBack1                                       
 #                                                                              
 ipv4-family unicast                                                            
  undo synchronization                                                          
  peer 10.2.2.2 enable                                                           
 #                                                                              
 l2vpn-family evpn                                                              
  policy vpn-target                                                             
  peer 10.2.2.2 enable                                                           
 #                                                                              
 ipv4-family vpn-instance vpn1                                                  
  import-route direct                                                           
  advertise l2vpn evpn                                                          
#  
ospf 1 router-id 10.1.1.2                                                                          
 area 0.0.0.0                                                                   
  network 10.1.1.2 0.0.0.0                                                       
  network 192.168.2.0 0.0.0.255 
#                                                                               
return 
Router2 configuration file
#
sysname Router2
#                                                                               
ip vpn-instance vpn1                                                            
 ipv4-family                                                                    
  route-distinguisher 100:1                                                    
  vpn-target 1:1 export-extcommunity evpn                                       
  vpn-target 1:1 import-extcommunity evpn                                       
 vxlan vni 5020                                                                 
#
bridge-domain 20                                                                
 vxlan vni 2020
#                                                                               
interface Ethernet2/0/0                                                                                                                        
 ip address 192.168.3.1 255.255.255.0                                           
#                                                                               
interface Ethernet2/0/1.1 mode l2                                               
 encapsulation dot1q vid 20                                                     
 bridge-domain 20
#                                                                               
interface LoopBack1                                                             
 ip address 10.2.2.2 255.255.255.255  
#                                                                               
interface Vbdif20                                                               
 ip binding vpn-instance vpn1                                                   
 ip address 192.168.20.10 255.255.255.0                                         
#                                                                               
interface Nve1                                                                  
 source 10.2.2.2  
 vni 2020 head-end peer-list 10.1.1.2                                                                
#                                                                               
bgp 100    
 router-id 10.2.2.2                                                                     
 peer 10.1.1.2 as-number 100                                                     
 peer 10.1.1.2 connect-interface LoopBack1                                       
 #                                                                              
 ipv4-family unicast                                                            
  undo synchronization                                                          
  peer 10.1.1.2 enable                                                           
 #                                                                              
 l2vpn-family evpn                                                              
  policy vpn-target                                                             
  peer 10.1.1.2 enable                                                           
 #                                                                              
 ipv4-family vpn-instance vpn1                                                  
  import-route direct                                                           
  advertise l2vpn evpn                                                          
#  
ospf 1 router-id 10.2.2.2                                                                           
 area 0.0.0.0                                                                   
  network 10.2.2.2 0.0.0.0                                                       
  network 192.168.3.0 0.0.0.255 
#                                                                               
return 
Router3 configuration file
#
sysname Router3
#                                                                               
interface Ethernet2/0/1                                                                                              
 ip address 192.168.2.2 255.255.255.0                                           
#                                                                               
interface Ethernet2/0/2                                                                                           
 ip address 192.168.3.2 255.255.255.0                                           
#                                                                               
interface LoopBack1                                                             
 ip address 10.3.3.2 255.255.255.255  
#  
ospf 1 router-id 10.3.3.2                                                                         
 area 0.0.0.0                                                                   
  network 10.3.3.2 0.0.0.0                                                       
  network 192.168.2.0 0.0.0.255 
  network 192.168.3.0 0.0.0.255 
#                                                                               
return