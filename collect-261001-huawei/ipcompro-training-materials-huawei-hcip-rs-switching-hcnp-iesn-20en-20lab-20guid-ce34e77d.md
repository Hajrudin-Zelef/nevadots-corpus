Huawei Certification 
 
HCDP-IESN 
 
Implementing Enterprise Switching Networks 
Lab Guide 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
Huawei Technologies Co.,Ltd

)UV_XOMNZj.[G]KO:KINTURUMOKY)U2ZJ'RRXOMNZYXKYKX\KJ
No part of this document may be repr oduced or transmitted in any form or 
by any means without prior written consent of Huawei Technologies Co., Ltd. 
:XGJKSGXQYGTJ6KXSOYYOUTY
 and other Huawei trademarks are trademarks of Huawei Technologies 
Co., Ltd. All other trademarks and trade names mentioned in this document 
the property of their respective holders.
4UZOIK
The information in this document is subject to change without notice. Every 
effort has been made in the preparation of this document to ensure accuracy of 
the contents, but all statements, information, and recommendations in this 
document do not constitute the warranty of any kind, expressed or implied. 
 
 
 
.[G]KO)KXZOLOIGZOUT
.)*6/+94/SVRKSKTZOTM+TZKXVXOYK9]OZINOTM
2GH-[OJK

+JOZOUT


Huawei Certification System 
Relying on its strong technical and professional training system, in accordance  
with different customers at differ ent levels of ICT technology, Huawei 
certification is committed to provide customs with authentic, professional 
certification.
Based on characteristics of ICT technologies and customersÿneeds at different 
levels, Huawei certification provides customers with certification system of four 
levels.  
HCDA (Huawei Certification Datacom Associate) is primary for IP network 
maintenance engineers, and any others who want to build an understanding of 
the IP network. HCDA certification covers the TCP/IP basics, routing, switching 
and other common foundational knowledge of IP networks, together with 
Huawei communications products, versatile routing platform VRP 
characteristics and basic maintenance. 
HCDP-Enterprise (Huawei Certification Datacom Professional-Enterprise) is 
aimed at enterprise-class network ma intenance engineers, network design 
engineers, and any others who want to   grasp in depth routing, switching, 
network adjustment and optimization technologies. HCDP-Enterprise consists 
of IESN (Implementing Enterprise Switch Networks), IERN (Implementing 
Enterprise Routing Networks), and IENP (Improving Enterprise Network 
performance), which includes advanced IPv4 routing and switching technology 
principles,  network security, high av ailability and QoS, as well as the 
configuration of  Huawei products. 
HCIE-Enterprise (Huawei Certified Internetwork Expert-Enterprise) is designed 
to endue engineers with a variety of IP  technologies and proficiency in the 
maintenance, diagnostics and troubl eshooting of Huawei products, which 
equips  engineers with competence in planning, design and optimization of 
large-scale IP networks.

HCIE- 
R&S 
 
UC&C VC Cloud Storage Wireless Transmission Security 
ICT Career Certification 
Expert 
HCNA- 
Design 
HCNP- 
Design 
HCNA(HCDA) 
HCAr 
HCNA-
WLAN 
HCNA- 
UC 
HCNA- 
VC 
HCNA-
Cloud 
HCNA- 
LTE 
HCNA- 
Transmission 
HCNA-
Security 
HCNA- 
CC 
HCNP-Carrier 
(HCDP-Carrier) 
HCNP-
WLAN 
HCNP- 
UC 
HCNP- 
VC 
HCNP- 
Cloud 
HCNP- 
LTE 
HCNP-
Transmission 
HCNP-R&S 
(HCDP) 
HCNP- 
Security 
HCNP-
Storage 
HCNP- 
CC 
HCNA-
Storage 
H
Associate Professional 
HCIE- 
Design 
Proposed Advanced 
relationship 
Necessary advanced 
relationship 
 
HCIE- 
Carrier 
 
HCIE- 
LTE 
HCIE- 
WLAN 
HCIE-
Security 
HCIE-
Transmissio
n 
HCIE- 
CC 
HCIE- 
UC 
HCIE- 
Cloud 
HCIE- 
VC 
HCIE- 
Storage 
Architect 
Routing & Switching WLAN ICT Convergence 
Design

Referenced icon 


  
8U[ZKX 29]OZIN 29]OZIN ,OXK]GRR 4KZIRU[J
+ZNKXTKZROTK  9KXOGRROTK

Lab environment specification 
:NK2GHKT\OXUTSKTZOYY[MMKYZKJHKRU] 

/JKTZOLOKX *K\OIK 59\KXYOUT
8 '8 <KXYOUT<8)96) 
8 '8 <KXYOUT<8)96) 
8 '8 <KXYOUT<8)96) 
8 '8 <KXYOUT<8)96) 
8 '8 <KXYOUT<8)96) 
9 9)+/9 <KXYOUT<8)96)
9 9)+/9 <KXYOUT<8)96)
9 9:6+/') <KXYOUT<8)96)
9 9:6+/') <KXYOUT<8)96)
,= ;9- <KXYOUT<8)96) 
,= ;9- <KXYOUT<8)96) 

HCDP-IESN Content   
 
HC Series HUAWEI TECHNOLOGIES Page1 
 
)54:+4:9
Chapter 1 Implementing VLAN features ............................................................................................. 1 
Lab 1-1 VLAN Configuration ........................................................................................................... 1 
Lab 1-2 MUX VLAN Configuration and GVRP Configuration (Optional) ......................................... 17 
Lab 1-3 Inter-VLAN Communication ............................................................................................. 32 
Lab 1-4 SEP and Smart Link .......................................................................................................... 46 
Chapter 2 STP and SEP ..................................................................................................................... 59 
Lab 2-1 STP, RSTP, and MSTP Configuration ................................................................................. 59 
Lab 2-2 Compatibility Between Multi-Region MSTP and STP (Optional) ....................................... 82 
Chapter 3 Implementing MPLS technologies .................................................................................. 104 
Lab 3-1 MPLS LDP Configuration ................................................................................................ 104

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page1 
 
Chapter 1 Implementing VLAN features 
Lab 1-1 VLAN Configuration 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Configure VLANs. 
/g120 Configure Eth-trunks. 
/g120 Use hybrid interfaces. 
Topology 
 
Figure 1-1 VLAN configuration 
Scenario 
Assume that you are a network administrator of a company. The company 
uses an Ethernet that has two switch es. In the preceding figure, R1 and R2 
represent computers on the network and R3 is a server. To optimize the 
network, the transmission speed and reliab ility must be improved for the links 
between S1 and S2. Two VLANs are configured to isolate broadcast domains.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page2 HUAWEI TECHNOLOGIES HC Series 
 
R2 and R3 are in the same VLAN. R1 needs to communicate with R3. 
Tasks 
Step 1 Perform basic configurations and IP addressing. 
Configure IP addresses and subnet masks for all routers. 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R1 
[R1]interface GigabitEthernet 0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.10.1 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R2 
[R2]interface GigabitEthernet 0/0/1 
[R2-GigabitEthernet0/0/1]ip address 10.0.10.2 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R3 
[R3]interface GigabitEthernet 0/0/2 
[R3-GigabitEthernet0/0/2]ip address 10.0.10.3 24 
 
Configure names for the two switches,shutdown some unused interfaces. 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S1 
[S1]interface GigabitEthernet 0/0/13 
[S1-GigabitEthernet0/0/13]shutdown 
[S1-GigabitEthernet0/0/13]interface GigabitEthernet 0/0/14 
[S1-GigabitEthernet0/0/14]shutdown 
 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S2

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page3 
 
Step 2 Configure link aggregation with Eth-trunk. 
The Eth-trunk technology bundles two or more links into a single link to 
increase the bandwidth and improve reliability. Add the G0/0/9 and G0/0/10 
interfaces of S1 and S2 to an Eth-trunk. 
Create an Eth-trunk. 
[S1]interface Eth-Trunk 1 
[S1-Eth-Trunk1] 
 
[S2]interface Eth-Trunk 1 
[S2-Eth-Trunk1] 
 
Configure the Eth-trunk to work in static Link Aggregation Control Protocol 
(LACP) mode. 
[S1-Eth-Trunk1]bpdu enable 
[S1-Eth-Trunk1]mode lacp-static 
 
[S2-Eth-Trunk1]bpdu enable 
[S2-Eth-Trunk1]mode lacp-static 
 
Add the G0/0/9 and G0/0/10 interfaces of S1 and S2 to the Eth-trunk. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]eth-trunk 1 
[S1-GigabitEthernet0/0/9]quit 
[S1]interface GigabitEthernet 0/0/10 
[S1-GigabitEthernet0/0/10]eth-trunk 1 
 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]eth-trunk 1 
[S2-GigabitEthernet0/0/9]quit 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]eth-trunk 1 
 
Run the display eth-trunk command to view the configuration result. 
[S1]display eth-trunk 1 
Eth-Trunk1's state information is: 
Local: 
LAG ID: 1                   WorkingMode: STATIC

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page4 HUAWEI TECHNOLOGIES HC Series 
 
Preempt Delay: Disabled     Hash arithmetic: According to SA-XOR-DA            
System Priority: 32768      System ID: 4c1f-cc45-aace                          
Least Active-linknumber: 1  Max Active-linknumber: 8                           
Operate status: up        Number Of Up Port In Trunk: 2                     
---------------------------------------------------------------------------- 
ActorPortName          Status   PortType PortPri PortNo PortKey PortState Weight 
GigabitEthernet0/0/9   Selected 1GE      32768   9      305     10100010  1      
GigabitEthernet0/0/10  Selected 1GE      32768   10     305     10100010  1      
 
Partner: 
---------------------------------------------------------------------------- 
ActorPortName          SysPri    SystemID  PortPri PortNo  PortKey   PortState 
GigabitEthernet0/0/9   32768      4c1f-cc45-aace  32768   9      305     10100010 
GigabitEthernet0/0/10  32768      4c1f-cc45-aace 32768   10     305     10100010 
 
The preceding information shows that the Eth-trunk link works in static 
LACP mode and the maximum number of active interfaces is 8. In addition, the 
G0/0/9 and G0/0/10 interfaces are active. 
Change the maximum number of active interfaces. 
[S1-Eth-Trunk1]max active-linknumber 1 
 
[S2-Eth-Trunk1]max active-linknumber 1 
 
View the configurations of the Eth-trunk link. 
[S1]display eth-trunk 1 
Eth-Trunk1's state information is: 
Local: 
LAG ID: 1                   WorkingMode: STATIC 
Preempt Delay: Disabled     Hash arithmetic: According to SA-XOR-DA 
System Priority: 32768      System ID: 4c1f-cc45-aace 
Least Active-linknumber: 1  Max Active-linknumber: 1 
Operate status: up          Number Of Up Port In Trunk: 1 
---------------------------------------------------------------------------- 
ActorPortName          Status   PortType PortPri PortNo PortKey PortState Weight 
GigabitEthernet0/0/9   Selected 1GE      32768   9      305     10111100  1 
GigabitEthernet0/0/10  Unselect 1GE      32768   10     305     10100000  1 
 
Partner: 
---------------------------------------------------------------------------- 
ActorPortName          SysPri    SystemID  PortPri PortNo  PortKey   PortState

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page5 
 
GigabitEthernet0/0/9   32768  4c1f-cc45-aacc  32768  9     305       10111100 
GigabitEthernet0/0/10  32768  4c1f-cc45-aacc  32768  10    305       10100000 
 
The preceding information shows that the G0/0/10 interface changes to the 
Unselect state. In this way, one link in the Eth-trunk link group transmits data 
and the other is a backup, improving network reliability. 
Disable the G0/0/9 interface of S1 to verify the link backup function. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]shutdown 
 
View the information about the Eth-trunk link. 
[S1]display eth-trunk 1 
Eth-Trunk1's state information is: 
Local: 
LAG ID: 1                   WorkingMode: STATIC 
Preempt Delay: Disabled     Hash arithmetic: According to SA-XOR-DA 
System Priority: 32768      System ID: 4c1f-cc45-aace 
Least Active-linknumber: 1  Max Active-linknumber: 1 
Operate status: up          Number Of Up Port In Trunk: 1 
---------------------------------------------------------------------------- 
ActorPortName          Status   PortType PortPri PortNo PortKey PortState Weight 
GigabitEthernet0/0/9   Unselect 1GE      32768   9      305     10100010  1 
GigabitEthernet0/0/10  Selected 1GE      32768   10     305     10111100  1 
 
Partner: 
---------------------------------------------------------------------------- 
ActorPortName          SysPri    SystemID  PortPri PortNo  PortKey   PortState 
GigabitEthernet0/0/9   0      0000-0000-0000  0      0     0         10100011 
GigabitEthernet0/0/10  32768  4c1f-cc45-aacc  32768  10    305       10111100 
 
The preceding information shows that the G0/0/9 interface in the Eth-trunk 
link changes to the Unselect state, and G0/0/10 changes from the Unselect 
state to the Selected state to transmit data. This proves the link backup 
function. 
Step 3 Configure VLANs. 
Create two VLANs on S1 and S2 to isolate R1 from R2 and R3. Configure 
the Eth-trunk link between S1 and S2 to work in Trunk mode so that the routers 
in the two VLANs can communicate with each other.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page6 HUAWEI TECHNOLOGIES HC Series 
 
Create VLAN 10 and VLAN 20 on S1 and S2. 
[S1]vlan batch 10 20 
 
[S2]vlan batch 10 20 
 
Configure the G0/0/1 and G0/0/2 interfaces of S1 to work in Access mode, 
and add the two interfaces to VLAN 10. 
[S1]interface GigabitEthernet 0/0/1 
[S1-GigabitEthernet0/0/1]port link-type access 
[S1-GigabitEthernet0/0/1]port default vlan 10 
[S1]interface GigabitEthernet 0/0/2 
[S1-GigabitEthernet0/0/2]port link-type access 
[S1-GigabitEthernet0/0/2]port default vlan 20 
 
Configure the G0/0/3 interface of S2 to work in Access mode, and add this 
interface to VLAN 20. 
[S2]interface GigabitEthernet 0/0/3 
[S2-GigabitEthernet0/0/3]port link-type access 
[S2-GigabitEthernet0/0/3]port default vlan 20 
 
Configure the Eth-trunk link between S1 and S2 to work in Trunk mode. By 
default, the trunk interface allows only packets from VLAN 1 to pass through. 
Therefore, configure the trunk interf ace to allow packets from VLAN 10 and 
VLAN 20 to pass through. 
[S1]interface Eth-Trunk 1 
[S1-Eth-Trunk1]port link-type trunk 
[S1-Eth-Trunk1]port trunk allow-pass vlan 10 20 
 
[S2]inter Eth-Trunk 1 
[S2-Eth-Trunk1]port link-type trunk 
[S2-Eth-Trunk1]port trunk allow-pass vlan 10 20 
 
Run the display port vlan  command to view the status of the trunk 
interface. 
[S2]display port vlan  
Port                    Link Type    PVID  Trunk VLAN List 
---------------------------------------------------------------------------- 
Eth-Trunk1              trunk        1     1 10 20 
GigabitEthernet0/0/1    hybrid       1     -

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page7 
 
GigabitEthernet0/0/2    hybrid       1    -                                    
GigabitEthernet0/0/3    access       20     -                                    
GigabitEthernet0/0/4    hybrid       1     - 
GigabitEthernet0/0/5    hybrid       1     - 
GigabitEthernet0/0/6    hybrid       1     - 
GigabitEthernet0/0/7    hybrid       1     - 
GigabitEthernet0/0/8    hybrid       1     - 
GigabitEthernet0/0/9    hybrid       0     - 
GigabitEthernet0/0/10   hybrid       0     - 
GigabitEthernet0/0/11   hybrid       1     - 
GigabitEthernet0/0/12   hybrid       1     - 
GigabitEthernet0/0/13   hybrid       1     - 
GigabitEthernet0/0/14   hybrid       1     - 
GigabitEthernet0/0/15   hybrid       1     - 
GigabitEthernet0/0/16   hybrid       1     - 
GigabitEthernet0/0/17   hybrid       1     - 
GigabitEthernet0/0/18   hybrid       1     - 
GigabitEthernet0/0/19   hybrid       1     - 
GigabitEthernet0/0/20   hybrid       1     - 
GigabitEthernet0/0/21   hybrid       1     - 
GigabitEthernet0/0/22   hybrid       1     - 
GigabitEthernet0/0/23   hybrid       1     - 
GigabitEthernet0/0/24   hybrid       1     - 
Eth-Trunk1              trunk        1     1 10 20 
 
The preceding information shows that  the Eth-trunk interface works in 
Trunk mode and allows VLAN 10 to communicate with VLAN 20. 
Run the ping command to test whether the routes from R2 to R1 and R3 are 
reachable. 
[R2]ping -c 1 10.0.10.1 
  PING 10.0.10.1: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.1 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R2]ping -c 1 10.0.10.3 
  PING 10.0.10.3: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.3: bytes=56 Sequence=1 ttl=255 time=3 ms

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page8 HUAWEI TECHNOLOGIES HC Series 
 
 
  --- 10.0.10.3 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 3/3/3 ms 
 
The preceding information shows that computers in different VLANs on two 
switches that are interconnected cannot communicate with each 
other,computers in the same VLAN can;but computers in different VLANs on 
two switches that are interconnected ov er trunk can communicate with each 
other. 
Step 4 Configure hybrid interfaces. 
The hybrid interface allows R1 to communicate with R3 in Layer 2 mode. 
Compare the hybrid interface with the trunk interface to learn the 
configurations and features of the hybrid interface. 
Configuration 1: The link between S1 and S2 works in Trunk mode. 
Configure the G0/0/1 interface of S1 to work in Hybrid mode, and add this 
interface to VLAN 10. Configure the function of removing VLAN tags of 
packets from VLAN 20. 
[S1]interface GigabitEthernet 0/0/1 
[S1-GigabitEthernet0/0/1]port default vlan 1 
[S1-GigabitEthernet0/0/1]port link-type hybrid 
[S1-GigabitEthernet0/0/1]port hybrid pvid vlan 10 
[S1-GigabitEthernet0/0/1]port hybrid tagged vlan 10 
[S1-GigabitEthernet0/0/1]port hybrid untagged vlan  20 
 
Configure the G0/0/3 interface of S2 to work in Hybrid mode, and add this 
interface to VLAN 20. Enable the G0/0/3 interface to forward packets from 
VLAN 10 in untagged mode. 
[S2]interface GigabitEthernet 0/0/3 
[S2-GigabitEthernet0/0/3]port default vlan 1 
[S2-GigabitEthernet0/0/3]port link-type hybrid 
[S2-GigabitEthernet0/0/3]port hybrid pvid vlan 20 
[S2-GigabitEthernet0/0/3]port hybrid untagged vlan 10  
 
Run the display vlan and display port vlan commands to view interface 
status.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page9 
 
[S1]display vlan 20 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports                                                           
---------------------------------------------------------------------------- 
20   common  UT:GE0/0/1(U)      GE0/0/2(U)                                       
              TG:Eth-Trunk1(U)                                                    
VID  Status  Property      MAC-LRN Statistics Description       
---------------------------------------------------------------------------- 
20   enable  default       enable  disable    VLAN 0020                   
 
[S1]display port vlan 
Port                    Link Type    PVID  Trunk VLAN List 
---------------------------------------------------------------------------- 
Eth-Trunk1                trunk        1     1 10 20 
GigabitEthernet0/0/1    hybrid       10    10                                   
GigabitEthernet0/0/2    access       20    -                                    
GigabitEthernet0/0/3    hybrid       1     -                                    
GigabitEthernet0/0/4    hybrid       1     -                                    
GigabitEthernet0/0/5    hybrid       1     -                                    
GigabitEthernet0/0/6    hybrid       1     -                                    
GigabitEthernet0/0/7    hybrid       1     -                                    
GigabitEthernet0/0/8    hybrid       1     -                                    
GigabitEthernet0/0/9    hybrid       0     -                                    
GigabitEthernet0/0/10   hybrid       0     -                                    
GigabitEthernet0/0/11   hybrid       1     -                                    
GigabitEthernet0/0/12   hybrid       1     -                                    
GigabitEthernet0/0/13   hybrid       1     -                                    
GigabitEthernet0/0/14   hybrid       1     -                                    
GigabitEthernet0/0/15   hybrid       1     -                                    
GigabitEthernet0/0/16   hybrid       1     -                                    
GigabitEthernet0/0/17   hybrid       1     -                                    
GigabitEthernet0/0/18   hybrid       1     -                                    
GigabitEthernet0/0/19   hybrid       1     -                                    
GigabitEthernet0/0/20   hybrid       1     -                                    
GigabitEthernet0/0/21   hybrid       1     -                                    
GigabitEthernet0/0/22   hybrid       1     -                                    
GigabitEthernet0/0/23   hybrid       1     -                                    
GigabitEthernet0/0/24   hybrid       1     -

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page10 HUAWEI TECHNOLOGIES HC Series 
 
 
[S2]display vlan 10    
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports                                                           
---------------------------------------------------------------------------- 
10   common  UT:GE0/0/3(U)                                                       
              TG:Eth-Trunk1(U)                                                    
VID  Status  Property      MAC-LRN Statistics Description       
---------------------------------------------------------------------------- 
10   enable  default       enable  disable    VLAN 0010   
                  
[S2]display port vlan 
Port                    Link Type    PVID  Trunk VLAN List 
---------------------------------------------------------------------------- 
Eth-Trunk1                trunk        1     1 10 20 
GigabitEthernet0/0/1    hybrid       1     -                                    
GigabitEthernet0/0/2    hybrid       1     -                                    
GigabitEthernet0/0/3    hybrid       20   20                                    
GigabitEthernet0/0/4    hybrid       1     -                                    
GigabitEthernet0/0/5    hybrid       1     -                                    
GigabitEthernet0/0/6    hybrid       1     -                                    
GigabitEthernet0/0/7    hybrid       1     -                                    
GigabitEthernet0/0/8    hybrid       1     -                                    
GigabitEthernet0/0/9    hybrid       0     -                                    
GigabitEthernet0/0/10   hybrid       0     -                                    
GigabitEthernet0/0/11   hybrid       1     -                                    
GigabitEthernet0/0/12   hybrid       1     -                                    
GigabitEthernet0/0/13   hybrid       1     -                                    
GigabitEthernet0/0/14   hybrid       1     -                                    
GigabitEthernet0/0/15   hybrid       1     -                                    
GigabitEthernet0/0/16   hybrid       1     -                                    
GigabitEthernet0/0/17   hybrid       1     -                                    
GigabitEthernet0/0/18   hybrid       1     -                                    
GigabitEthernet0/0/19   hybrid       1     -                                    
GigabitEthernet0/0/20   hybrid       1     -                                    
GigabitEthernet0/0/21   hybrid       1     -                                    
GigabitEthernet0/0/22   hybrid       1     -                                    
GigabitEthernet0/0/23   hybrid       1     -

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page11 
 
GigabitEthernet0/0/24   hybrid       1     -                                    
 
The preceding information shows that th e G0/0/1 interface of S1 forwards 
packets from VLAN 20 in untagged mode. The port VLAN ID (PVID) of G0/0/1 
belongs to VLAN 10. The G0/0/3 interface of S2 forwards packets from VLAN 
10 in untagged mode. The PVID of G0/0/3 belongs to VLAN 20. 
 
Run the ping command to test whether the routes from R1 to R2 and R3 
are reachable. 
[R1]ping -c 1 10.0.10.2 
  PING 10.0.10.2: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.2 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R1]ping -c 1 10.0.10.3 
  PING 10.0.10.3: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.3: bytes=56 Sequence=1 ttl=255 time=13 ms 
 
  --- 10.0.10.3 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 13/13/13 ms 
 
Configuration 2: The link between S1 and S2 works in Hybrid mode. 
Configure the Eth-trunk interfaces on S1 and S2 to work in Hybrid mode. 
Run Untagged VLAN 10 on the Eth-trunk interface of S1 and Untagged 
VLAN 20 on the Eth-trunk interface of S2. 
[S1]interface Eth-Trunk 1 
[S1-Eth-Trunk1]undo port trunk allow-pass vlan 10 20 
[S1-Eth-Trunk1]port link-type hybrid 
[S1-Eth-Trunk1]port hybrid untagged vlan 10 
 
[S2]interface Eth-Trunk 1 
[S2-Eth-Trunk1]undo port trunk allow-pass vlan 10 20 
[S2-Eth-Trunk1]port link-type hybrid

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page12 HUAWEI TECHNOLOGIES HC Series 
 
[S2-Eth-Trunk1]port hybrid untagged vlan 20 
 
View the configurations of the Eth-trunk 1 interface. 
[S1]display port vlan  
Port                    Link Type    PVID  Trunk VLAN List 
---------------------------------------------------------------------------- 
Eth-Trunk1              hybrid       1      - 
GigabitEthernet0/0/1    hybrid       10   - 
GigabitEthernet0/0/2    access       20    -                                    
GigabitEthernet0/0/3    hybrid       1     -                                    
GigabitEthernet0/0/4    hybrid       1     -                                    
GigabitEthernet0/0/5    hybrid       1     -                                    
GigabitEthernet0/0/6    hybrid       1     -                                    
GigabitEthernet0/0/7    hybrid       1     -                                    
GigabitEthernet0/0/8    hybrid       1     -                                    
GigabitEthernet0/0/9    hybrid       0     -                                    
GigabitEthernet0/0/10   hybrid       0     -                                    
GigabitEthernet0/0/11   hybrid       1     -                                    
GigabitEthernet0/0/12   hybrid       1     -                                    
GigabitEthernet0/0/13   hybrid       1     -                                    
GigabitEthernet0/0/14   hybrid       1     -                                    
GigabitEthernet0/0/15   hybrid       1     -                                    
GigabitEthernet0/0/16   hybrid       1     -                                    
GigabitEthernet0/0/17   hybrid       1     -                                    
GigabitEthernet0/0/18   hybrid       1     -                                    
GigabitEthernet0/0/19   hybrid       1     -                                    
GigabitEthernet0/0/20   hybrid       1     -                                    
GigabitEthernet0/0/21   hybrid       1     -                                    
GigabitEthernet0/0/22   hybrid       1     -                                    
GigabitEthernet0/0/23   hybrid       1     -                                    
GigabitEthernet0/0/24   hybrid       1     -                                    
 
[S1]display vlan 10 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports                                                           
---------------------------------------------------------------------------- 
10   common  UT:GE0/0/1(U)      Eth-Trunk1(U)

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page13 
 
VID  Status  Property      MAC-LRN Statistics Description       
---------------------------------------------------------------------------- 
10   enable  default       enable  disable    VLAN 0010 
The preceding information shows that the Eth-trunk 1 interface of S1 works 
in Hybrid mode and forwards packets from VLAN 10 in untagged mode. The 
Eth-trunk 1 interface of S2 works in Hybrid mode and forwards packets from 
VLAN 20 in untagged mode. 
Run the ping command to test whether the routes from R1 to R2 and R3 
are reachable. 
[R1]ping -c 1 10.0.10.2 
  PING 10.0.10.2: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.2 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R1]ping -c 1 10.0.10.3 
  PING 10.0.10.3: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.3: bytes=56 Sequence=1 ttl=255 time=4 ms 
 
  --- 10.0.10.3 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
0% packet loss 
R1 can communicate with R3. 
Additional Exercises: Analyzing and Verifying 
In step 4, figure out the differences in data transmission when the 
Eth-trunk interfaces on S1 and S2 work in Trunk mode and Hybrid mode. 
Final Configurations 
[S1]display current-configuration 
# 
!Software Version V100R005C01SPC100 
 sysname S1

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page14 HUAWEI TECHNOLOGIES HC Series 
 
# 
 vlan batch 10 20 
# 
 stp mode rstp 
 stp enable 
# 
 undo http server enable 
# 
interface Eth-Trunk1 
 port hybrid untagged vlan 10 
 mode lacp-static 
 max active-linknumber 1 
 bpdu enable 
# 
interface GigabitEthernet0/0/1 
 port hybrid pvid vlan 10 
 port hybrid tagged vlan 10 
# 
interface GigabitEthernet0/0/2 
 port link-type access 
 port default vlan 20 
# 
interface GigabitEthernet0/0/3 
 shutdown 
# 
interface GigabitEthernet0/0/4 
 shutdown 
# 
interface GigabitEthernet0/0/9 
 shutdown 
 eth-trunk 1 
# 
interface GigabitEthernet0/0/10 
 eth-trunk 1 
# 
interface GigabitEthernet0/0/13 
 shutdown 
# 
interface GigabitEthernet0/0/14 
 shutdown 
# 
interface GigabitEthernet0/0/21 
 shutdown

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page15 
 
# 
interface GigabitEthernet0/0/22 
 shutdown 
# 
interface GigabitEthernet0/0/23 
 shutdown 
# 
Return 
 
[S2]display current-configuration 
# 
!Software Version V100R005C01SPC100 
 sysname S2 
# 
 vlan batch 10 20 
# 
 stp mode rstp 
 stp enable 
# 
interface Eth-Trunk1 
 port hybrid untagged vlan 20 
 mode lacp-static 
 max active-linknumber 1 
 bpdu enable 
# 
interface GigabitEthernet0/0/1 
 shutdown 
# 
interface GigabitEthernet0/0/2 
 shutdown 
# 
interface GigabitEthernet0/0/3 
 port hybrid pvid vlan 20 
 port hybrid tagged vlan 20 
# 
interface GigabitEthernet0/0/4 
 shutdown 
# 
interface GigabitEthernet0/0/5 
 shutdown 
# 
interface GigabitEthernet0/0/9 
 eth-trunk 1

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page16 HUAWEI TECHNOLOGIES HC Series 
 
# 
interface GigabitEthernet0/0/10 
 eth-trunk 1 
# 
interface GigabitEthernet0/0/11 
 shutdown 
# 
interface GigabitEthernet0/0/12 
 shutdown 
# 
interface GigabitEthernet0/0/13 
 shutdown 
# 
interface GigabitEthernet0/0/23 
 shutdown 
# 
interface GigabitEthernet0/0/24 
 shutdown 
# 
return

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page17 
 
Lab 1-2 MUX VLAN Configuration and GVRP Configuration 
(Optional) 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Configure GARP VLAN Registration Protocol (GVRP). 
/g120 Configure multiplexer (MUX) VLANs. 
/g120 Configure voice VLANs in automatic mode. 
Topology 
 
Figure 1-2 MUX VLAN configuration and GVRP configuration 
Scenario 
Assume that you are a network administrator of a company. The company 
uses an Ethernet that has two switche s. In the preceding figure, the routers 
represent computers on the network. To optimize the network, broadcast 
domains must be isolated from each other. R1 and R2 are in the same VLAN, 
and R3 and R4 are in the same VLAN. As required by the company, all

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page18 HUAWEI TECHNOLOGIES HC Series 
 
computers can access R4, but R3 and R4 cannot communicate with each 
other, with neither R1 nor R2. Voice devices will be used on the G0/0/24 
interface of S2. Therefore, plan a voic e VLAN and related configurations in 
advance. 
Tasks 
Step 1 Perform basic configurations and IP addressing. 
Configure IP addresses and subnet masks for all routers. 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R1 
[R1]interface g0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.10.1 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R2 
[R2]interface g0/0/1 
[R2-GigabitEthernet0/0/1]ip address 10.0.10.2 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R3 
[R3]interface g0/0/1 
[R3-GigabitEthernet0/0/1]ip address 10.0.10.3 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R4 
[R4]interface g0/0/2 
[R4-GigabitEthernet0/0/2]ip address 10.0.10.4 24 
 
<Huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R5 
[R5]interface g0/0/2 
[R5-GigabitEthernet0/0/2]ip address 10.0.10.5 24 
 
<Quidway>system-view

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page19 
 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S1 
[S1] 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S2 
[S2] 
 
Test whether the routes from R1 to R2, R3, R4, and R5 are reachable. 
[R1]ping -c 1 10.0.10.2 
  PING 10.0.10.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.2: bytes=56 Sequence=1 ttl=255 time=14 ms 
 
  --- 10.0.10.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 14/14/14 ms 
 
[R1]ping -c 1 10.0.10.3 
  PING 10.0.10.3: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.3: bytes=56 Sequence=1 ttl=255 time=5 ms 
 
  --- 10.0.10.3 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 5/5/5 ms 
 
[R1]ping -c 1 10.0.10.4 
  PING 10.0.10.4: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.4: bytes=56 Sequence=1 ttl=255 time=15 ms 
 
  --- 10.0.10.4 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 15/15/15 ms 
 
[R1]ping -c 1 10.0.10.5 
  PING 10.0.10.5: 56  data bytes, press CTRL_C to break

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page20 HUAWEI TECHNOLOGIES HC Series 
 
    Reply from 10.0.10.5: bytes=56 Sequence=1 ttl=255 time=6 ms 
 
  --- 10.0.10.5 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 6/6/6 ms 
 
Step 2 Configure GVRP. 
GVRP transmits VLAN information across the network, improving the 
VLAN configuration efficiency. Configure the G0/0/9 interface of S2 to work in 
Fixed mode and that of S1 to work in Normal mode so that S1 can learn the 
VLAN configurations on S2. 
Configure the G0/0/9 interfaces of S1 and S2 to work in Trunk mode and 
allow the access from all VLANs. 
[S1]interface g0/0/9 
[S1-GigabitEthernet0/0/9]port link-type trunk 
[S1-GigabitEthernet0/0/9]port trunk allow-pass vlan all 
 
[S2]interface g0/0/9 
[S2-GigabitEthernet0/0/9]port link-type trunk 
[S2-GigabitEthernet0/0/9]port trunk allow-pass vlan all 
 
Enable GVRP on S1 and S2. 
[S1]gvrp 
 
[S2]gvrp 
 
Run the display gvrp status command to view GVRP status information. 
[S1]display gvrp status 
 GVRP is enabled 
 
[S2]display gvrp status 
 GVRP is enabled 
 
Configure the G0/0/9 interface of S1 to work in Normal mode. 
[S1]interface g0/0/9 
[S1-GigabitEthernet0/0/9]gvrp

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page21 
 
[S1-GigabitEthernet0/0/9]gvrp registration normal 
[S1-GigabitEthernet0/0/9]bpdu enable 
 
Configure the G0/0/9 interface of S2 to work in Fixed mode. 
[S2]interface g0/0/9 
[S2-GigabitEthernet0/0/9]gvrp 
[S2-GigabitEthernet0/0/9]gvrp registration fixed 
[S2-GigabitEthernet0/0/9]bpdu enable 
 
Run the display gvrp statistics command to view GVRP statistics. 
[S1]display gvrp statistics 
 
  GVRP statistics on port GigabitEthernet0/0/9 
    GVRP status                         : Enabled 
    GVRP registrations failed           : 0 
    GVRP last PDU origin                        : 4c1f-cc45-aacc 
GVRP registration type              : Normal 
 
Run the display vlan command to view VLAN information about S1 in the 
current state. 
[S1]display vlan 
The total number of vlans is : 1 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports 
---------------------------------------------------------------------------- 
1    common  UT:GE0/0/1(U)      GE0/0/2(U)      GE0/0/3(U)      GE0/0/4(U) 
                GE0/0/5(U)      GE0/0/6(D)      GE0/0/7(D)      GE0/0/8(D) 
                GE0/0/9(U)      GE0/0/10(D)     GE0/0/11(D)     GE0/0/12(D) 
                GE0/0/13(D)     GE0/0/14(D)     GE0/0/15(D)     GE0/0/16(D) 
                GE0/0/17(D)     GE0/0/18(D)     GE0/0/19(D)     GE0/0/20(D) 
                GE0/0/21(D)     GE0/0/22(D)     GE0/0/23(D)     GE0/0/24(D) 
 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
1    enable  default       enable  disable    VLAN 0001 
 
Create VLAN 10, VLAN 20, and VLAN 100 on S2.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page22 HUAWEI TECHNOLOGIES HC Series 
 
[S2]vlan batch 10 20 100 
Info: This operation may take a few seconds. Please wait for a moment...done. 
 
View VLAN information about S1 again. 
[S1]display vlan 
The total number of vlans is : 4 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports 
---------------------------------------------------------------------------- 
1    common  UT:GE0/0/1(U)      GE0/0/2(U)      GE0/0/3(U)      GE0/0/4(U) 
                GE0/0/5(U)      GE0/0/6(D)      GE0/0/7(D)      GE0/0/8(D) 
                GE0/0/9(U)      GE0/0/10(D)     GE0/0/11(D)     GE0/0/12(D) 
                GE0/0/13(D)     GE0/0/14(D)     GE0/0/15(D)     GE0/0/16(D) 
                GE0/0/17(D)     GE0/0/18(D)     GE0/0/19(D)     GE0/0/20(D) 
                GE0/0/21(D)     GE0/0/22(D)     GE0/0/23(D)     GE0/0/24(D) 
10   dynamic TG:GE0/0/9(U) 
20   dynamic TG:GE0/0/9(U) 
100  dynamic TG:GE0/0/9(U) 
 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
1    enable  default       enable  disable    VLAN 0001 
10   enable  default       enable  disable    VLAN 0010 
20   enable  default       enable  disable    VLAN 0020 
100  enable  default       enable  disable    VLAN 0100 
 
The preceding information shows that S1 learns the VLAN information 
about S2. 
Create VLAN 30 on S1 and observe the changes of the VLAN information 
about S1 and S2. 
[S1]vlan 30 
[S1-vlan30]quit 
[S1]display vlan 
The total number of vlans is : 5 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking;

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page23 
 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
 
VID  Type    Ports 
---------------------------------------------------------------------------- 
1    common  UT:GE0/0/1(U)      GE0/0/2(U)      GE0/0/3(U)      GE0/0/4(U) 
                GE0/0/5(U)      GE0/0/6(D)      GE0/0/7(D)      GE0/0/8(D) 
                GE0/0/9(U)      GE0/0/10(D)     GE0/0/11(D)     GE0/0/12(D) 
                GE0/0/13(D)     GE0/0/14(D)     GE0/0/15(D)     GE0/0/16(D) 
                GE0/0/17(D)     GE0/0/18(D)     GE0/0/19(D)     GE0/0/20(D) 
                GE0/0/21(D)     GE0/0/22(D)     GE0/0/23(D)     GE0/0/24(D) 
10   dynamic TG:GE0/0/9(U) 
20   dynamic TG:GE0/0/9(U) 
30   common  TG:GE0/0/9(U) 
100  dynamic TG:GE0/0/9(U) 
 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
1    enable  default       enable  disable    VLAN 0001 
10   enable  default       enable  disable    VLAN 0010 
20   enable  default       enable  disable    VLAN 0020 
30   enable  default       enable  disable    VLAN 0030 
100  enable  default       enable  disable    VLAN 0100 
 
[S2]display vlan 
The total number of vlans is : 4 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports 
---------------------------------------------------------------------------- 
1    common  UT:GE0/0/1(U)      GE0/0/2(U)      GE0/0/3(U)      GE0/0/4(U) 
                GE0/0/5(U)      GE0/0/6(D)      GE0/0/7(D)      GE0/0/8(D) 
                GE0/0/9(U)      GE0/0/10(D)     GE0/0/11(D)     GE0/0/12(D) 
                GE0/0/13(D)     GE0/0/14(D)     GE0/0/15(D)     GE0/0/16(D) 
                GE0/0/17(D)     GE0/0/18(D)     GE0/0/19(D)     GE0/0/20(D) 
                GE0/0/21(D)     GE0/0/22(D)     GE0/0/23(D)     GE0/0/24(D) 
10   common  TG:GE0/0/9(U) 
20   common  TG:GE0/0/9(U) 
100  common  TG:GE0/0/9(U)

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page24 HUAWEI TECHNOLOGIES HC Series 
 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
1    enable  default       enable  disable    VLAN 0001 
10   enable  default       enable  disable    VLAN 0010 
20   enable  default       enable  disable    VLAN 0020 
100  enable  default       enable  disable    VLAN 0100 
 
The preceding information shows that S2 does not learn the VLAN 
information about S1. 
Step 3 Configure MUX VLANs. 
After devices on the same network segment are added to different VLANs, 
Layer 2 communication is isolated, that is, routers in different VLANs cannot 
communicate with each other. However, the MUX VLAN can allow these 
routers to communicate with a specified VLAN. In addition, the MUX VLAN can 
restrict devices in the same VLAN from communicating with each other. 
Configure VLAN 100 as the primary VLAN of the MUX VLAN, and VLAN 
10 and VLAN 20 as the secondary VLANs. 
Configure the types of the interfaces on the computers that connect to the 
switches to ensure that: all comput ers can communicate with R4; R3 and R4 
cannot communicate with each other or with routers in other VLANs. 
Configure VLAN 100 as a secondary VLAN and add related configurations. 
[S1]vlan 10 
[S1-vlan10]quit 
[S1]vlan 20 
[S1-vlan20]quit 
[S1]vlan 100 
[S1-vlan100]mux-vlan 
[S1-vlan100]subordinate group 10 
[S1-vlan100]subordinate separate 20 
 
[S2]vlan 100 
[S2-vlan100]mux-vlan 
[S2-vlan100]subordinate group 10 
[S2-vlan100]subordinate separate 20 
 
Add the G0/0/5 interface on R5 that connects to S2 to VLAN 100 and 
enable the MUX VLAN function.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page25 
 
[S2]interface GigabitEthernet 0/0/5 
[S2-GigabitEthernet0/0/5]port link-type access 
[S2-GigabitEthernet0/0/5]port default vlan 100 
[S2-GigabitEthernet0/0/5]port mux-vlan enable 
 
Add the G0/0/1 interface on R1 that  connects to S1 and the G0/0/2 
interface on R2 that connects to S1 to VLAN 10 and enable the MUX VLAN 
function. 
[S1]interface GigabitEthernet 0/0/1 
[S1-GigabitEthernet0/0/1]port link-type access 
[S1-GigabitEthernet0/0/1]port default vlan 10 
[S1-GigabitEthernet0/0/1]port mux-vlan enable 
[S1-GigabitEthernet0/0/1]interface GigabitEthernet 0/0/2 
[S1-GigabitEthernet0/0/2]port link-type access 
[S1-GigabitEthernet0/0/2]port default vlan 10 
[S1-GigabitEthernet0/0/2]port mux-vlan enable 
 
Add the G0/0/3 interface on R3 that  connects to S1 and the G0/0/4 
interface on R4 that connects to S2 to VLAN 20 and enable the MUX VLAN 
function. 
[S1]interface GigabitEthernet 0/0/3 
[S1-GigabitEthernet0/0/3]port link-type access 
[S1-GigabitEthernet0/0/3]port default vlan 20 
[S1-GigabitEthernet0/0/3]port mux-vlan enable 
 
[S2]interface GigabitEthernet 0/0/4  
[S2-GigabitEthernet0/0/4]port link-type access  
[S2-GigabitEthernet0/0/4]port default vlan 20  
[S2-GigabitEthernet0/0/4]port mux-vlan enable 
Run the display mux-vlan command to view information about all MUX 
VLANs. 
[S1]display mux-vlan 
Principal Subordinate Type         Interface 
---------------------------------------------------------------------------- 
100       -           principal 
100       20          separate     GigabitEthernet0/0/3 
100       10          group        GigabitEthernet0/0/1 GigabitEthernet0/0/2 
 
[S2]display mux-vlan 
Principal Subordinate Type         Interface 
----------------------------------------------------------------------------

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page26 HUAWEI TECHNOLOGIES HC Series 
 
100       -           principal    GigabitEthernet0/0/5 
100       20          separate     GigabitEthernet0/0/4 
100       10          group 
---------------------------------------------------------------------------- 
 
Run the ping command to test whether the routes from R1 to R2, R3, R4, 
and R5 are reachable. 
[R1]ping -c 1 10.0.10.2 
  PING 10.0.10.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.2: bytes=56 Sequence=1 ttl=255 time=3 ms 
 
  --- 10.0.10.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 3/3/3 ms 
 
[R1]ping -c 1 10.0.10.3 
  PING 10.0.10.3: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.3 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R1]ping -c 1 10.0.10.4 
  PING 10.0.10.4: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.4 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R1]ping -c 1 10.0.10.5 
  PING 10.0.10.5: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.5: bytes=56 Sequence=1 ttl=255 time=3 ms 
 
  --- 10.0.10.5 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page27 
 
    0.00% packet loss 
    round-trip min/avg/max = 3/3/3 ms 
 
Run the ping command to test whether the routes from R3 to R2, R4, and 
R5 are reachable. 
[R3]ping -c 1 10.0.10.2 
  PING 10.0.10.2: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.2 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R3]ping -c 1 10.0.10.4 
  PING 10.0.10.4: 56  data bytes, press CTRL_C to break 
    Request time out 
 
  --- 10.0.10.4 ping statistics --- 
    1 packet(s) transmitted 
    0 packet(s) received 
    100.00% packet loss 
 
[R3]ping -c 1 10.0.10.5 
  PING 10.0.10.5: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.10.5: bytes=56 Sequence=1 ttl=255 time=3 ms 
 
  --- 10.0.10.5 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 3/3/3 ms 
 
The preceding information shows that: R1 and R2 in VLAN 10 can 
communicate with each other, as well as with R5; R3 and R4 in VLAN 10 can 
communicate only with R5. 
Step 4 Configure voice VLANs. 
To meet service development requirements, voice VLANs are configured 
on S2 and related configuration is completed on the G0/0/24 interface.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page28 HUAWEI TECHNOLOGIES HC Series 
 
Create VLAN 30 and VLAN 200 on S2. VLAN 200 functions as a voice 
VLAN. 
[S2]vlan batch 30 200 
Info: This operation may take a few seconds. Please wait for a moment...done. 
 
Configure the interface type and default VLAN for the G0/0/24 interface of 
S2. Assume that the voice device belongs to VLAN 30. 
[S2]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]port hybrid pvid vlan 30 
[S2-GigabitEthernet0/0/24]port hybrid untagged vlan 30 
 
Configure an OUI address on S2. Assume  that the media access control 
(MAC) address of the voice device is 0011-2200-0000 and the subnet mask is 
ffff-ff00-0000. 
[S2]voice-vlan mac-address 0011-2200-0000 mask ffff-ff00-0000 
 
Enable the voice VLAN function and configure an automatic voice VLAN 
on the G0/0/24 interface of S2. 
[S2]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]voice-vlan 200 enable 
[S2-GigabitEthernet0/0/24]voice-vlan mode auto 
[S2-GigabitEthernet0/0/24]voice-vlan security enable 
 
Run the display voice-vlan oui command to view the OUI address of the 
voice VLAN. 
[S2]display voice-vlan oui 
--------------------------------------------------- 
OuiAddress       Mask             Description 
--------------------------------------------------- 
0011-2200-0000  ffff-ff00-0000 
 
Run the display voice-vlan 200 status  command to view the 
configurations of the voice VLAN. 
[S2]display voice-vlan 200 status 
Voice VLAN Configurations: 
----------------------------------------------------------- 
Voice VLAN ID            : 200 
Voice VLAN status        : Enable 
Voice VLAN aging time    : 1440(minutes)

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page29 
 
Voice VLAN 8021p remark  : 6 
Voice VLAN dscp remark   : 46 
----------------------------------------------------------- 
Port Information: 
----------------------------------------------------------- 
Port                     Add-Mode  Security-Mode  Legacy 
----------------------------------------------------------- 
GigabitEthernet0/0/24    Auto      Security       Disable 
Additional Exercises: Analyzing and Verifying 
Figure out whether computers in different MUX VLANs can communicate 
with each other. 
Final Configurations 
[S1]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S1 
# 
 vlan batch 10 20 30 100 
# 
 gvrp 
# 
vlan 100 
 mux-vlan 
 subordinate separate 20 
 subordinate group 10 
# 
interface GigabitEthernet0/0/1 
 port link-type access 
 port default vlan 10 
 port mux-vlan enable 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/2 
 port link-type access 
 port default vlan 10 
 port mux-vlan enable

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page30 HUAWEI TECHNOLOGIES HC Series 
 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/3 
 port link-type access 
 port default vlan 20 
 port mux-vlan enable 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
 undo ntdp enable 
 undo ndp enable 
 gvrp 
# 
interface GigabitEthernet0/0/10 
 shutdown 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
Return 
 
[S2]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S2 
# 
 voice-vlan mac-address 0011-2200-0000 mask ffff-ff00-0000 
# 
 vlan batch 10 20 30 100 200 
# 
 gvrp 
# 
vlan 100 
 mux-vlan 
 subordinate separate 20 
 subordinate group 10 
#

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page31 
 
interface GigabitEthernet0/0/4 
 port link-type access 
 port default vlan 20 
 port mux-vlan enable 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/5 
 port link-type access 
 port default vlan 100 
 port mux-vlan enable 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
 undo ntdp enable 
 undo ndp enable 
 gvrp 
 gvrp registration fixed 
# 
interface GigabitEthernet0/0/10 
 shutdown 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/24 
voice-vlan 200 enable 
 port hybrid pvid vlan 30 
 port hybrid untagged vlan 30 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
Return

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page32 HUAWEI TECHNOLOGIES HC Series 
 
Lab 1-3 Inter-VLAN Communication 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Configure multi-arm routing. 
/g120 Configure single-arm routing. 
/g120 Configure inter-VLAN communication. 
/g120 Configure VLAN aggregation. 
Topology 
 
Figure 1-3 Inter-VLAN communication 
Scenario 
Assume that you are a network administrator of a company. The company 
uses an Ethernet that has two switches and one router. In the preceding figure, 
R1 and R2 represent two computers in different departments and are added to 
two VLANs. As required by the compan y, R1 needs to communicate with R2.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page33 
 
Multi-arm routing is used first and later replaced with single-arm routing to 
reduce the costs. 
Then multi-layer switching is used because traffic is mostly transmitted 
between VLANs as the network architecture changes. Finally the VLAN 
aggregation technology is used to facilitate network management. 
Tasks 
Step 1 Perform basic configurations and IP addressing. 
Disable G0/0/9 of S1 to avoid affecting the experiment. 
Configure IP addresses and subnet masks for all routers. 
<huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[huawei]sysname R1 
[R1]interface GigabitEthernet 0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.2.2 24 
 
<huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[huawei]sysname R2 
[R2]interface GigabitEthernet 0/0/2 
[R2-GigabitEthernet0/0/2]ip address 10.0.3.2 24 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S1 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/10]undo shutdown 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S2 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/10]undo shutdown 
 
<huawei>system-view 
Enter system view, return user view with Ctrl+Z. 
[huawei]sysname R4 
[R4]interface GigabitEthernet 0/0/1

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page34 HUAWEI TECHNOLOGIES HC Series 
 
[R4-GigabitEthernet0/0/1]ip address 10.0.2.1 24 
[R4-GigabitEthernet0/0/1]interface GigabitEthernet 0/0/2 
[R4-GigabitEthernet0/0/2]ip address 10.0.3.1 24 
 
Run the ping command to test whether the r oute from R1 to the G0/0/1 
interface of R4 is reachable. 
[R1]ping -c 1 10.0.2.1 
  PING 10.0.2.1: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.2.1: bytes=56 Sequence=1 ttl=255 time=4 ms 
 
  --- 10.0.2.1 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 4/4/4 ms 
 
Run the ping command to test whether the r oute from R2 to the G0/0/2 
interface of R4 is reachable. 
[R2]ping -c 1 10.0.3.1 
  PING 10.0.3.1: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.3.1: bytes=56 Sequence=1 ttl=255 time=3 ms 
 
  --- 10.0.3.1 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 3/3/3 ms 
 
Step 2 Configure multi-arm routing. 
R1 and R2 are in different VLANs. 
The gateway of R1 uses the IP address of the G0/0/1 interface of R4, and 
that of R2 uses the IP address of the G0/0/2 interface of R4. 
Multiple interfaces of R4 support communication between VLANs. This is 
multi-arm routing. 
Create VLAN 2 and VLAN 3 on S1 and S2. 
[S1]vlan batch 2 3 
Info: This operation may take a few seconds. Please wait for a moment...done.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page35 
 
[S2]vlan batch 2 3 
Info: This operation may take a few seconds. Please wait for a moment...done. 
 
Add R1 and the G0/0/1 interface of R4 to VLAN 2, and R2 and the G0/0/2 
interface of R4 to VLAN 3. 
[S1]interface GigabitEthernet 0/0/1 
[S1-GigabitEthernet0/0/1]port link-type access 
[S1-GigabitEthernet0/0/1]port default vlan 2 
[S1-GigabitEthernet0/0/1]inter g0/0/4 
[S1-GigabitEthernet0/0/4]port link-type access 
[S1-GigabitEthernet0/0/4]port default vlan 2 
 
[S2]interface GigabitEthernet 0/0/2 
[S2-GigabitEthernet0/0/2]port link-type access 
[S2-GigabitEthernet0/0/2]port default vlan 3 
[S2-GigabitEthernet0/0/2]inter g0/0/4 
[S2-GigabitEthernet0/0/4]port link-type access 
[S2-GigabitEthernet0/0/4]port default vlan 3 
 
Configure a gateway on R1 and R2. The gateway of R1 uses the IP 
address of the G0/0/1 interface of R4, and  that of R2 uses the IP address of 
the G0/0/2 interface of R4. 
[R1]ip route-static 0.0.0.0 0 10.0.2.1 
 
[R2]ip route-static 0.0.0.0 0 10.0.3.1 
 
Run the display vlan command to check the configurations. 
[S1]display vlan 2 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports 
---------------------------------------------------------------------------- 
2    common  UT:GE0/0/1(U)      GE0/0/4(U) 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
2    enable  default       enable  disable    VLAN 0002 
 
[S2]display vlan 3

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page36 HUAWEI TECHNOLOGIES HC Series 
 
---------------------------------------------------------------------------- 
U: Up;         D: Down;         TG: Tagged;         UT: Untagged; 
MP: Vlan-mapping;               ST: Vlan-stacking; 
#: ProtocolTransparent-vlan;    *: Management-vlan; 
---------------------------------------------------------------------------- 
VID  Type    Ports 
---------------------------------------------------------------------------- 
3    common  UT:GE0/0/2(U)      GE0/0/4(U) 
VID  Status  Property      MAC-LRN Statistics Description 
---------------------------------------------------------------------------- 
3    enable  default       enable  disable    VLAN 0003 
 
Test whether the route from R1 to R2 is reachable. 
[R1]ping -c 1 10.0.3.2 
  PING 10.0.3.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.3.2: bytes=56 Sequence=1 ttl=254 time=3 ms 
 
  --- 10.0.3.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 3/3/3 ms 
 
[R2]ping -c 1 10.0.2.2 
  PING 10.0.2.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.2.2: bytes=56 Sequence=1 ttl=254 time=3 ms 
 
  --- 10.0.2.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 3/3/3 ms 
Step 3 Configure single-arm routing. 
Create two subinterfaces on a physical interface of R4. Inter-VLAN 
communication is implemented on relevant subinterfaces. 
This is single-alarm routing. 
Disable the G0/0/4 interface of S2. 
[S2]interface GigabitEthernet 0/0/4

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page37 
 
[S2-GigabitEthernet0/0/4]shutdown 
 
Add the G0/0/9 interfaces of S1 and S2 to VLAN 3. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]undo shutdown 
[S2-GigabitEthernet0/0/9]port link-type access 
[S2-GigabitEthernet0/0/9]port default vlan 3 
 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]undo shutdown 
[S1-GigabitEthernet0/0/9]port link-type access 
[S1-GigabitEthernet0/0/9]port default vlan 3 
 
Configure the G0/0/4 interface of S1 to work in Trunk mode and allow the 
access from VLAN 2 and VLAN 3. 
[S1]interface GigabitEthernet 0/0/4 
[S1-GigabitEthernet0/0/4]port default vlan 1 
[S1-GigabitEthernet0/0/4]port link-type trunk 
[S1-GigabitEthernet0/0/4]port trunk allow-pass vlan 2 3 
 
Create two subinterfaces for the G0/0 /1 interface of R4. Configure IP 
addresses for the two subinterfaces and configure them as Dot1q termination 
Ethernet subinterfaces. 
[R4]interface GigabitEthernet 0/0/1.2 
[R4-GigabitEthernet0/0/1.2]control-vid 20 dot1q-termination 
[R4-GigabitEthernet0/0/1.2]dot1q termination vid 2 
[R4-GigabitEthernet0/0/1.2]arp broadcast enable 
[R4-GigabitEthernet0/0/1.2]ip address 10.0.20.1 24 
[R4-GigabitEthernet0/0/1.2]interface GigabitEthernet 0/0/1.3 
[R4-GigabitEthernet0/0/1.3]control-vid 30 dot1q-termination 
[R4-GigabitEthernet0/0/1.3]dot1q termination vid 3 
[R4-GigabitEthernet0/0/1.3]arp broadcast enable 
[R4-GigabitEthernet0/0/1.3]ip address 10.0.30.1 24 
Run the display ip interface brief command to view subinterface 
configurations on R4. 
[R4]display ip interface brief 
*down: administratively down 
(l): loopback 
(s): spoofing 
 
The number of interface that is UP in Physical is 6

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page38 HUAWEI TECHNOLOGIES HC Series 
 
The number of interface that is DOWN in Physical is 4 
The number of interface that is UP in Protocol is 3 
The number of interface that is DOWN in Protocol is 7 
 
Interface                         IP Address/Mask      Physical   Protocol 
Cellular0/0/0                     unassigned           down       down 
Cellular0/0/1                     unassigned           down       down 
Ethernet2/0/0                     unassigned           down       down 
Ethernet2/0/1                     10.0.3.1/24          down       down 
GigabitEthernet0/0/0              unassigned           up         down 
GigabitEthernet0/0/1              10.0.2.1/24          up         up 
GigabitEthernet0/0/1.2            10.0.20.1/24        up         down 
GigabitEthernet0/0/1.3            10.0.30.1/24        up         down 
NULL0                             unassigned           up         up(s) 
Serial1/0/0                       unassigned           up         up 
 
Change the IP addresses and gateways of R1 and R2. 
[R1]inter GigabitEthernet 0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.20.2 24 
[R1-GigabitEthernet0/0/1]quit 
[R1]undo ip route-static 0.0.0.0 0 10.0.2.1 
[R1]ip route-static 0.0.0.0 0 10.0.20.1 
 
[R2]interface GigabitEthernet 0/0/2 
[R2-GigabitEthernet0/0/2]ip address 10.0.30.2 24 
[R2-GigabitEthernet0/0/2]quit 
[R2]undo ip route-static 0.0.0.0 0 10.0.3.1 
[R2]ip route-static 0.0.0.0 0 10.0.30.1 
 
Test whether the route from R1 to R2 is reachable. 
 
[R1]ping -c 1 10.0.30.2 
  PING 10.0.30.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.30.2: bytes=56 Sequence=1 ttl=254 time=3 ms 
 
  --- 10.0.30.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 3/3/3 ms

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page39 
 
The preceding information shows that  computers in VLAN 2 and VLAN 3 
can communicate with each other. 
Compared with the multi-arm scheme, this scheme reduces the costs in 
purchasing router interfaces. 
In single-arm routing, all data is transmitted over a single interface. W hen 
the number of VLANs increases, the bandwidth of this link may be insufficient. 
Moreover, this link is likely to caus e a single-point failure and then the entire 
network fails. 
Step 4 Configure Layer 3 switching. 
In Layer 3 switching, no router is required and each VLAN has a VLANIF 
interface for communicating with other VLANs. 
Disable the G0/0/4 interface of S1. 
[S1]interface GigabitEthernet 0/0/4 
[S1-GigabitEthernet0/0/4]shutdown 
 
Configure the G0/0/9 interfaces of S1 and S2 to work in Trunk mode and 
allow the access from VLAN 2 and VLAN 3. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]port default vlan 1 
[S1-GigabitEthernet0/0/9]port link-type trunk 
[S1-GigabitEthernet0/0/9]port trunk allow-pass vlan 2 3 
 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]port default vlan 1 
[S2-GigabitEthernet0/0/9]port link-type trunk 
[S2-GigabitEthernet0/0/9]port trunk allow-pass vlan 2 3 
 
Create the VLANIF 2 and VLANIF 3 interfaces on S1 and configure IP 
addresses for the interfaces. 
[S1]interface Vlanif 2 
[S1-Vlanif2]ip address 10.0.20.1 24 
[S1]interface Vlanif 3 
[S1-Vlanif3]ip address 10.0.30.1 24 
 
Test whether the route from R1 to R2 is reachable. 
[R1]ping -c 1 10.0.30.2 
  PING 10.0.30.2: 56  data bytes, press CTRL_C to break

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page40 HUAWEI TECHNOLOGIES HC Series 
 
    Reply from 10.0.30.2: bytes=56 Sequence=1 ttl=254 time=2 ms 
 
  --- 10.0.30.2 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 2/2/2 ms 
 
The preceding information shows that Layer 3 communication between 
computers in VLAN 2 and those in VLAN 3 is implemented using the two 
VLANIF interfaces of S1. 
Layer 3 switching delivers higher scalability than single-arm routing and 
can withstand the addition of VLANs. 
Layer 3 switching can effectively se rve a network where most traffic is 
generated by inter-VLAN communication. 
Step 5 Configure VLAN aggregation. 
Similar to Layer 3 switching, VLAN aggregation implements inter-VLAN 
communication on a switch. In VLAN aggr egation, all VLANs are on the same 
network segment to reduce the number of required IP network segments and 
unify gateway configuration. 
Create VLAN 10, VLAN 20, and VLAN 100 on S1 and S2. 
[S1]vlan batch 10 20 100 
Info: This operation may take a few seconds. Please wait for a moment...done. 
 
[S2]vlan batch 10 20 100 
Info: This operation may take a few seconds. Please wait for a moment...done. 
 
Configure the G0/0/9 interfaces of S1 and S2 to allow the access from 
VLAN 10 and VLAN 20. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]port trunk allow-pass vlan 10 20 
 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]port trunk allow-pass vlan 10 20 
 
Add the G0/0/1 interface of S1 to VL AN 10 and the G0/0/2 interface of S2

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page41 
 
to VLAN 20. 
[S1]interface GigabitEthernet 0/0/1 
[S1-GigabitEthernet0/0/1]port default vlan 10 
 
[S2]interface GigabitEthernet 0/0/2 
[S2-GigabitEthernet0/0/1]port default vlan 20 
 
Configure VLAN 100 as a Super-VLAN, and add VLAN 10 and VLAN 20 to 
the Super-VLAN. Then VLAN 10 and VLAN 20 are Sub-VLANs of this 
Super-VLAN. 
[S1]vlan 100 
[S1-vlan100]aggregate-vlan 
[S1-vlan100]access-vlan 10 20 
 
Configure a VLANIF interface for VLAN 100 and enable the ARP proxy 
function. 
[S1]interface Vlanif 100 
[S1-Vlanif100]ip address 10.0.100.1 24 
[S1-Vlanif100]arp-proxy inter-sub-vlan-proxy enable 
 
Change the IP addresses of R1 and R2 to  ensure that they are on the 
same network segment as the VLANIF 100 interface. Configure the IP address 
of the VLANIF 100 interface as the gateway IP address. 
[R1]interface GigabitEthernet 0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.100.2 24 
[R1-GigabitEthernet0/0/1]quit 
[R1]ip route-static 0.0.0.0 0 10.0.100.1 
 
[R2]interface GigabitEthernet 0/0/2 
[R2-GigabitEthernet0/0/2]ip address 10.0.100.3 24 
[R2-GigabitEthernet0/0/2]quit 
[R2]ip route-static 0.0.0.0 0 10.0.100.1 
 
Test whether the routes from R1 and R2 to the VLANIF 100 interface of S1 
are reachable. 
[R1]ping -c 1 10.0.100.1 
  PING 10.0.100.1: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.100.1: bytes=56 Sequence=1 ttl=254 time=3 ms 
 
  --- 10.0.100.1 ping statistics ---

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page42 HUAWEI TECHNOLOGIES HC Series 
 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 3/3/3 ms 
 
[R1]ping -c 1 10.0.100.3 
  PING 10.0.100.3: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.100.3: bytes=56 Sequence=1 ttl=254 time=2 ms 
 
  --- 10.0.100.3 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 2/2/2 ms 
 
[R2]pin -c 1 10.0.100.1 
  PING 10.0.100.1: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.100.1: bytes=56 Sequence=1 ttl=254 time=3 ms 
 
  --- 10.0.100.1 ping statistics --- 
    1 packet(s) transmitted 
    1 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 3/3/3 ms 
 
The preceding information shows that R1 and R2 can communicate with 
the VLANIF 100 interface of S1. Compared with Layer 3 switching, VLAN 
aggregation allows different VLANs to use the same gateway for 
communication. This reduces the number of required IP addresses and 
improves management efficiency. Howev er, computers on the same network 
segment communicate with each other over  the same VLANIF interface, this 
challenges the robustness of the interface. 
Additional Exercises: Analyzing and Verifying 
Figure out the features, strong points, drawbacks, and application 
scenarios of multi-arm routing, single-arm routing, inter-VLAN communication, 
and VLAN aggregation.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page43 
 
Final Configurations 
[S1]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S1 
# 
 vlan batch 2 to 3 10 20 100 
# 
vlan 100 
 aggregate-vlan 
 access-vlan 10 20 
# 
interface Vlanif2 
 ip address 10.0.20.1 255.255.255.0 
# 
interface Vlanif3 
 ip address 10.0.30.1 255.255.255.0 
# 
interface Vlanif100 
 ip address 10.0.100.1 255.255.255.0 
 arp-proxy inter-sub-vlan-proxy enable 
# 
interface GigabitEthernet0/0/1 
 port link-type access 
 port default vlan 10 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/4 
 shutdown 
port link-type trunk 
 port trunk allow-pass vlan 2 to 3 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 3 10 20 
 undo ntdp enable

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page44 HUAWEI TECHNOLOGIES HC Series 
 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/10 
 shutdown 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
Return 
 
[S2]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S2 
# 
 vlan batch 2 to 3 10 20 100 
# 
interface GigabitEthernet0/0/2 
 port link-type access 
 port default vlan 20 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/4 
 shutdown 
 port link-type access 
 port default vlan 3 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 3 10 20 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
return 
 
[R4]display current-configuration

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page45 
 
[V200R001C00SPC500] 
# 
 sysname R4 
# 
interface GigabitEthernet0/0/1 
 ip address 10.0.2.1 255.255.255.0 
# 
interface GigabitEthernet0/0/1.2 
 control-vid 20 dot1q-termination 
 dot1q termination vid 2 
 ip address 10.0.20.1 255.255.255.0 
 arp broadcast enable 
# 
interface GigabitEthernet0/0/1.3 
 control-vid 30 dot1q-termination 
 dot1q termination vid 3 
 ip address 10.0.30.1 255.255.255.0 
 arp broadcast enable 
# 
interface GigabitEthernet0/0/0 
ip address 10.0.3.1 255.255.255.0 
# 
Return

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page46 HUAWEI TECHNOLOGIES HC Series 
 
Lab 1-4 SEP and Smart Link 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Configure SEP. 
/g120 Configure Smart Link. 
/g120 Configure hybrid networking of SEP and Smart Lin k.
Topology 
 
Figure 1-4 SEP configuration 
Scenario 
Assume that you are a network administrator of a company. The company

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page47 
 
uses an Ethernet that has four switches. S1, S2, and S3 comprise the core 
network. The backup design is used to improve the robustness of the network. 
That is, SEP is used for loop protection. Configure two upstream links on the 
access layer switch (S4). The Smart Link technology provides master and 
slave links for a network, ensuring network reliability. 
Tasks 
Step 1 Perform basic configurations. 
Configure names for all devices and disa ble the E0/0/1 interface of S4 to 
avoid affecting the experiment. 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S1 
[S1] 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S2 
[S2] 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S3 
[S3] 
 
<Quidway>system-view 
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S4 
[S4]interface Ethernet 0/0/1 
[S4-Ethernet0/0/1]shutdown 
 
Step 2 Configure SEP. 
Redundant connections are created between S1, S2, and S3 to improve 
the robustness of the network. Two network loops exist. 
The G0/0/9 and G0/0/10 interfaces of S1 and S2 form a closed loop. The

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page48 HUAWEI TECHNOLOGIES HC Series 
 
G0/0/13 interface of S1, G0/0/13 and G0 /0/23 of S3, and G0/0/23 of S2 form 
an open loop. SEP is used to  provide redundancy protection for the two 
network loops. 
Create a SEP network segment, configure a control VLAN, and specify 
protection instances. 
[S1]sep segment 1 
[S1-sep-segment1]control-vlan 10 
[S1-sep-segment1]protected-instance all 
[S1-sep-segment1]quit 
[S1]sep segment 2 
[S1-sep-segment2]control-vlan 20 
[S1-sep-segment2]protected-instance all 
 
[S2]sep segment 1 
[S2-sep-segment1]control-vlan 10 
[S2-sep-segment1]protected-instance all 
[S2-sep-segment1]quit 
[S2]sep segment 2 
[S2-sep-segment2]control-vlan 20 
[S2-sep-segment2]protected-instance all 
 
[S3]sep segment 2 
[S3-sep-segment2]control-vlan 20 
[S3-sep-segment2]protected-instance all 
 
[S4]sep segment 2 
[S4-sep-segment2]control-vlan 20 
[S4-sep-segment2]protected-instance all 
 
Add interfaces to the SEP network se gment and configure roles for the 
interfaces. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]stp disable 
[S1-GigabitEthernet0/0/9]sep segment 1 edge primary 
[S1-GigabitEthernet0/0/9]interface GigabitEthernet 0/0/10 
[S1-GigabitEthernet0/0/10]stp disable 
[S1-GigabitEthernet0/0/10]sep segment 1 edge secondary 
[S1-GigabitEthernet0/0/10]interface GigabitEthernet 0/0/13 
[S1-GigabitEthernet0/0/13]stp disable 
[S1-GigabitEthernet0/0/13]sep segment 2 edge primary

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page49 
 
[S2]interface GigabitEthernet 0/0/23 
[S2-GigabitEthernet0/0/23]stp disable 
[S2-GigabitEthernet0/0/23]sep segment 2 edge secondary 
[S2-GigabitEthernet0/0/23]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]stp disable 
[S2-GigabitEthernet0/0/9]sep segment 1 
[S2-GigabitEthernet0/0/9]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]stp disable 
[S2-GigabitEthernet0/0/10]sep segment 1 
 
[S3]interface Ethernet 0/0/13 
[S3-Ethernet0/0/13]stp disable 
[S3-Ethernet0/0/13]sep segment 2 
[S3-Ethernet0/0/13]interface ethernet 0/0/23 
[S3-Ethernet0/0/23]stp disable 
[S3-Ethernet0/0/23]sep segment 2 
 
Configure the blocking and preemption modes. 
[S1]sep segment 1 
[S1-sep-segment1]block port optimal 
[S1-sep-segment1]quit 
[S1]sep segment 2 
[S1-sep-segment2]block port 
[S1-sep-segment2]block port optimal 
 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]sep segment 1 priority 128 
 
[S3]inter Ethernet 0/0/23 
[S3-Ethernet0/0/23]sep segment 2 priority 128 
 
[S1]sep segment 1 
[S1-sep-segment1]preempt delay 30 
[S1-sep-segment1]quit 
[S1]sep segment 2 
[S1-sep-segment2]preempt delay 30 
 
Enable the function of advertising topology changes. 
[S1]sep segment 2 
[S1-sep-segment2]tc-notify segment 1 
 
[S2]sep segment 2

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page50 HUAWEI TECHNOLOGIES HC Series 
 
[S2-sep-segment2]tc-notify segment 1 
 
Run the display sep topology command to view SEP information. 
[S1]display sep topology 
SEP segment 1 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/9          primary         forwarding 
S2                   GE0/0/9          common          forwarding 
S2                   GE0/0/10         common          discarding 
S1                   GE0/0/10         secondary       forwarding 
 
SEP segment 2 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/13         primary         forwarding 
S3                   Eth0/0/13        common          forwarding 
S3                   Eth0/0/23        common          forwarding 
S2                   GE0/0/23         secondary       discarding 
 
Disable the G0/0/9 interface of S2 to check the running of SEP. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]shutdown 
[S2-GigabitEthernet0/0/9]quit 
[S2]display sep topology 
SEP segment 1 
SEP detects a segment failure that may be caused by an incomplete topology 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/9          secondary       discarding 
S1                   GE0/0/10         secondary       forwarding 
S2                   GE0/0/10         common          forwarding 
S2                   GE0/0/9          common          discarding 
 
SEP segment 2 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/13         primary         forwarding

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page51 
 
S3                   Eth0/0/13        common          forwarding 
S3                   Eth0/0/23        common          forwarding 
S2                   GE0/0/23         secondary       discarding 
 
Enable the G0/0/9 interface of S2 and disable the E0/0/13 interface of S3 
to check the running of SEP. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]undo shutdown 
 
[S3]interface Ethernet 0/0/13 
[S3-Ethernet0/0/13]shutdown 
[S3]display sep topology 
SEP segment 2 
SEP detects a segment failure that may be caused by an incomplete topology 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S2                   GE0/0/23         secondary       forwarding 
S3                   Eth0/0/23        common          forwarding 
S3                   Eth0/0/13        common          discarding 
 
Enable the E0/0/13 interface of S3to check the running of SEP. 
[S3]interface Ethernet 0/0/13 
[S3-Ethernet0/0/13]undo shutdown 
 
[S1]display sep topology 
SEP segment 1 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/9          primary         forwarding 
S2                   GE0/0/9          common          forwarding 
S2                   GE0/0/10         common          discarding 
S1                   GE0/0/10         secondary       forwarding 
 
SEP segment 2 
----------------------------------------------------------------- 
System Name          Port Name        Port Role       Port Status 
----------------------------------------------------------------- 
S1                   GE0/0/13         primary         forwarding 
S3                   Eth0/0/13        common          forwarding 
S3                   Eth0/0/23        common          discarding

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page52 HUAWEI TECHNOLOGIES HC Series 
 
S2                   GE0/0/23         secondary       forwarding 
 
The preceding information shows that SEP changes the network topology 
when a fault occurs. After the fault is rectified , SEP blo cks the specified 
interface. 
Step 3 Configure Smart Link. 
The Smart Link technology provides  redundant upstream connections for 
users. The Smart Link technology sets the E0/0/14 interface of S4 as the 
master interface and the E0/0/24 interface  as the slave interface. When the 
master interface is faulty, S4 switches to the slave interface quickly, ensuring 
service continuity for users. 
Configure a control VLAN on S4 and add the interfaces to the control 
VLAN. 
[S4]vlan 100 
[S4-vlan100]quit 
[S4]interface Ethernet 0/0/14 
[S4-Ethernet0/0/14]port link-type trunk 
[S4-Ethernet0/0/14]port trunk allow-pass vlan 100 
[S4-Ethernet0/0/14]interface ethe0/0/24 
[S4-Ethernet0/0/24]port link-type trunk 
[S4-Ethernet0/0/24]port trunk allow-pass vlan 100 
 
Configure a control VLAN on S1 and S2. Add the G0/0/14 interfaces of S1 
and S2 to this VLAN. 
[S1]vlan 100 
[S1-vlan100]quit 
[S1]interface GigabitEthernet 0/0/14 
[S1-GigabitEthernet0/0/14]port link-type trunk 
[S1-GigabitEthernet0/0/14]port trunk allow-pass vlan 100 
 
[S2]vlan 100 
[S2-vlan100]quit 
[S2]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]port link-type trunk 
[S2-GigabitEthernet0/0/24]port trunk allow-pass vlan 100 
 
Disable Spanning Tree Protocol (STP) for the E0/0/14 and E0/0/24 
interfaces of S4.

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page53 
 
[S4]interface Ethernet 0/0/14 
[S4-Ethernet0/0/14]stp disable 
[S4-Ethernet0/0/14]interface ethe0/0/24 
[S4-Ethernet0/0/24]stp disable 
 
Add the E0/0/14 and E0/0/24 interfaces  of S4 to the Smart Link group, 
E0/0/14 as the master interface and E0/0/24 as the slave interface. 
[S4]smart-link group 1 
[S4-smlk-group1]port Ethernet 0/0/14 master 
[S4-smlk-group1]port Ethernet 0/0/24 slave 
 
Enable the switchover function on S4 and set the switchover time. 
[S4]smart-link group 1 
[S4-smlk-group1]restore enable 
[S4-smlk-group1]timer wtr 30 
 
Enable the function of sending Flush packets on S4, and the function of 
receiving Flush packets on S1 and S2. 
[S4]smart-link group 1 
[S4-smlk-group1]flush send control-vlan 100 password simple huawei 
 
[S1]interface GigabitEthernet 0/0/14 
[S1-GigabitEthernet0/0/14]smart-link flush receive control-vlan 100 password 
simple huawei 
 
[S2]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]smart-link flush receive control-vlan 100 password 
simple huawei 
 
Enable the Smart Link function on S4. 
[S4]smart-link group 1 
[S4-smlk-group1]smart-link enable 
 
Run the display smart-link group command to view the information about 
the Smart Link group on S4. 
[S4]display smart-link group 1 
Smart Link group 1 information : 
  Smart Link group was enabled 
  Wtr-time is: 30 sec. 
  There is no Load-Balance

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page54 HUAWEI TECHNOLOGIES HC Series 
 
  There is no protected-vlan reference-instance 
  DeviceID: 5489-98ec-f012  Control-vlan ID: 100 
      Member              Role   State    Flush Count Last-Flush-Time 
  ---------------------------------------------------------------------- 
  Ethernet0/0/14          Master Active   1           2008/01/05 03:11:18 
UTC-05:13 
  Ethernet0/0/24          Slave  Inactive 0           0000/00/00 00:00:00 
UTC+00:00 
 
Disable the E0/0/14 interface of S4 to verify the Smart Link function. 
[S4]interface Ethernet 0/0/14 
[S4-Ethernet0/0/14]shutdown 
[S4]display smart-link group 1 
Smart Link group 1 information : 
  Smart Link group was enabled 
  Wtr-time is: 30 sec. 
  There is no Load-Balance 
  There is no protected-vlan reference-instance 
  DeviceID: 5489-98ec-f012  Control-vlan ID: 100 
      Member              Role   State    Flush Count Last-Flush-Time 
  ---------------------------------------------------------------------- 
 
  Ethernet0/0/14          Master Inactive 1           2008/01/05 03:11:18 UTC-05:13 
  Ethernet0/0/24          Slave  Active   1          2008/01/05 03:14:57 UTC-05:13 
 
The preceding information shows that the Smart Link function enables S4 
to switch over to the slave interface when the master interface is faulty. 
Step 4 Configure hybrid networking of SEP and Smart Link. 
When Smart Link is configured on the lower-layer network, SEP must learn 
the conditions of the lower-layer net work to meet network changes. Enable 
SEP and Smart Link on the network. 
On S1 and S2, enable the function of processing Smart Link Flush 
packets. 
[S1]sep segment 1 
[S1-sep-segment1]deal smart-link-flush 
 
[S2]sep segment 1 
[S2-sep-segment1]deal smart-link-flush

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page55 
 
Additional Exercises: Analyzing and Verifying 
Compare SEP and STP, and figure out their respective strong points and 
drawbacks. 
Final Configurations 
[S1]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S1 
# 
 vlan batch 10 20 100 
# 
sep segment 1 
 control-vlan 10 
 block port optimal 
 preempt delay 30 
 protected-instance 0 to 48 
 deal smart-link-flush 
sep segment 2 
 control-vlan 20 
 block port optimal 
 preempt delay 30 
 tc-notify segment 1 
 protected-instance 0 to 48 
# 
interface GigabitEthernet0/0/9 
 port hybrid tagged vlan 10 
 stp disable 
 sep segment 1 edge primary 
# 
interface GigabitEthernet0/0/10 
 port hybrid tagged vlan 10 
 stp disable 
 sep segment 1 edge secondary 
# 
interface GigabitEthernet0/0/13 
 port hybrid tagged vlan 20 
 stp disable 
 sep segment 2 edge primary 
#

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page56 HUAWEI TECHNOLOGIES HC Series 
 
interface GigabitEthernet0/0/14 
 port link-type trunk 
 port trunk allow-pass vlan 100 
 smart-link flush receive control-vlan 100 password simple huawei 
# 
Return 
 
[S2]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S2 
# 
 vlan batch 10 20 100 
# 
sep segment 1 
 control-vlan 10 
 protected-instance 0 to 48 
 deal smart-link-flush 
sep segment 2 
 control-vlan 20 
 tc-notify segment 1 
 protected-instance 0 to 48 
# 
interface GigabitEthernet0/0/9 
 port hybrid tagged vlan 10 
 stp disable 
 sep segment 1 
# 
interface GigabitEthernet0/0/10 
 port hybrid tagged vlan 10 
 stp disable 
 sep segment 1 
 sep segment 1 priority 128 
# 
interface GigabitEthernet0/0/23 
 port hybrid tagged vlan 20 
 stp disable 
 sep segment 2 edge secondary 
# 
interface GigabitEthernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 100 
 smart-link flush receive control-vlan 100 password simple huawei

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
HC Series HUAWEI TECHNOLOGIES     Page57 
 
# 
return 
 
[S3]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S3 
# 
 vlan batch 20 
# 
sep segment 2 
 control-vlan 20 
 protected-instance 0 to 48 
# 
# 
interface Ethernet0/0/13 
 port hybrid tagged vlan 20 
 stp disable 
 sep segment 2 
# 
interface Ethernet0/0/23 
 port hybrid tagged vlan 20 
 stp disable 
 sep segment 2 
 sep segment 2 priority 128 
# 
Return 
 
[S4]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S4 
# 
 vlan batch 20 100 
# 
sep segment 2 
 control-vlan 20 
 protected-instance 0 to 48 
# 
interface Ethernet0/0/1 
 shutdown 
# 
interface Ethernet0/0/14

HCDP-IESN  Chapter 1 Implementing VLAN features 
 
Page58 HUAWEI TECHNOLOGIES HC Series 
 
 shutdown 
 port link-type trunk 
 port trunk allow-pass vlan 100 
 stp disable 
# 
interface Ethernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 100 
 stp disable 
# 
smart-link group 1 
 restore enable 
 smart-link enable 
 port Ethernet0/0/14 master 
 port Ethernet0/0/24 slave 
 timer wtr 30 
 flush send control-vlan 100 password simple huawei 
# 
Return

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page59 
 
Chapter 2 STP and SEP 
Lab 2-1 STP, RSTP, and MSTP Configuration 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Learn the differences among Spanning Tree Protocol (STP), Rapid 
Spanning Tree Protocol (RSTP), and Multiple Spanning Tree Protocol 
(MSTP). 
/g120 Modify bridge priorities to control root bridge election. 
/g120 Modify interface priorities to control root interface and designated 
interface election. 
/g120 Configure RSTP and conduct compatibility configuration between STP 
and RSTP. 
/g120 Configure MSTP to implement load balancing among VLANs. 
Topology

HCDP-IESN  Chapter 2 STP and SEP 
 
Page60 HUAWEI TECHNOLOGIES HC Series 
 
Figure 2-1 STP, RSTP, and MSTP topology 
Scenario 
Assume that you are a network administrator of a company. The company 
uses a backup network. STP is used to avoid loops. By default ,STP 
convergence takes a long period of time. You can use RSTP to speed up 
network convergence. By default, all VLANs share a spanning tree. If you want 
to load balancing among VLANs,you can use MSTP. 
Tasks 
Step 1 Configure STP and verify the configuration. 
If the device default spanning tree is not turned on, use the following 
command to open. Configuration using the STP: 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S1 
[S1]stp enable 
[S1]stp mode stp 
 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S2 
[S2]stp enable 
[S2]stp mode stp 
 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S3 
[S3]stp enable 
[S3]stp mode stp 
 
<Quidway>system-view  
Enter system view, return user view with Ctrl+Z. 
[Quidway]sysname S4 
[S4]stp enable 
[S4]stp mode stp

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page61 
 
View STP status information. 
[S1]display stp 
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :32768.4c1f-cc45-aadc 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :32768.4c1f-cc45-aac1 / 20000 
CIST RegRoot/IRPC   :32768.4c1f-cc45-aadc / 0 
CIST RootPortId     :128.9 
BPDU-Protection     :Disabled 
TC or TCN received  :36 
TC count per hello  :2 
STP Converge Mode   :Normal 
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:0m:1s 
……output omit…… 
 
[S2]display stp 
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :32768.4c1f-cc45-aac1 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :32768.4c1f-cc45-aac1 / 0 
CIST RegRoot/IRPC   :32768.4c1f-cc45-aac1 / 0 
CIST RootPortId     :0.0 
BPDU-Protection     :Disabled 
TC or TCN received  :20 
TC count per hello  :0 
STP Converge Mode   :Normal 
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:1m:4s 
……output omit…… 
 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/10       ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page62 HUAWEI TECHNOLOGIES HC Series 
 
   0    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
S2 is the root bridge and all interfaces are designated interfaces. 
The actual results of the experim ent may be different due to the 
uncertainty of the MAC addresses of switches. 
Step 2 Control root bridge election. 
Configure S1 as the primary root br idge and S2 as the secondary root 
bridge. 
[S1]stp root primary 
 
[S2]stp root secondary 
 
View STP configuration information. 
[S1]display stp 
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :0    .4c1f-cc45-aadc 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :0    .4c1f-cc45-aadc / 0 
CIST RegRoot/IRPC   :0    .4c1f-cc45-aadc / 0 
CIST RootPortId     :0.0 
BPDU-Protection     :Disabled 
CIST Root Type      :Primary root 
TC or TCN received  :67 
TC count per hello  :0 
STP Converge Mode   :Normal 
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:0m:15s  
……output omit…… 
 
[S2]display stp 
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :4096 .4c1f-cc45-aac1 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :0    .4c1f-cc45-aadc / 20000 
CIST RegRoot/IRPC   :4096 .4c1f-cc45-aac1 / 0 
CIST RootPortId     :128.9 
BPDU-Protection     :Disabled

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page63 
 
CIST Root Type      :Secondary root 
TC or TCN received  :26 
TC count per hello  :0 
STP Converge Mode   :Normal 
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:0m:1s 
……output omit…… 
 
Configure S1 as the primary root br idge and S2 as the secondary root 
bridge. 
The smaller the priority value, the higher the priority. Change the priority 
value to 8129 for S1 and to 4096 for S2. 
[S1]undo stp root 
[S1]stp priority 8192 
 
[S2]undo stp root 
[S2]stp priority 4096 
 
View STP information. 
[S1]display stp 
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :8192 .4c1f-cc45-aadc 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :4096 .4c1f-cc45-aac1 / 20000 
CIST RegRoot/IRPC   :8192 .4c1f-cc45-aadc / 0 
CIST RootPortId     :128.9 
BPDU-Protection     :Disabled 
TC or TCN received  :79 
TC count per hello  :1 
STP Converge Mode   :Normal  
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:0m:0s 
ĂĂoutput omitĂĂ 
 
[S2]display stp  
-------[CIST Global Info][Mode STP]------- 
CIST Bridge         :4096 .4c1f-cc45-aac1 
Bridge Times        :Hello 2s MaxAge 20s FwDly 15s MaxHop 20 
CIST Root/ERPC      :4096 .4c1f-cc45-aac1 / 0 
CIST RegRoot/IRPC   :4096 .4c1f-cc45-aac1 / 0 
CIST RootPortId     :0.0

HCDP-IESN  Chapter 2 STP and SEP 
 
Page64 HUAWEI TECHNOLOGIES HC Series 
 
BPDU-Protection     :Disabled 
TC or TCN received  :88 
TC count per hello  :0 
STP Converge Mode   :Normal  
Share region-configuration :Enabled 
Time since last TC  :0 days 0h:0m:9s 
ĂĂoutput omitĂĂ 
 
S2 becomes the root bridge as it has a higher priority than S1. 
Step 3 Control root interface election. 
View role information about interfaces of S1. 
[S1]display stp  brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/10       ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/9 interface of S1 is the root interface. 
The default priority is 128. The lowe r the priority value, the larger the 
priority. 
S1 interconnects with S2 over the G0/0/9 and G0/0/10 interfaces. 
Set the priority to 32 for the G0/0/9  interface of S2 and to 16 for the 
G0/0/10 interface. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]stp port priority 32 
[S2-GigabitEthernet0/0/9]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]stp port priority 16 
 
Note: The priorities of the interfaces of S2, instead of S1, are changed. 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page65 
 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/10 interface of S1 becomes the root interface. 
Step 4 Control designated port election. 
View status information about the direct interfaces of S3 and S4. 
 [S3]display stp interface Ethernet 0/0/1 
----[CIST][Port1(Ethernet0/0/1)][DISCARDING]---- 
 Port Protocol       :Enabled 
 Port Role           :Alternate Port 
 Port Priority       :128 
 Port Cost(Dot1T )   :Config=auto / Active=199999 
 Designated Bridge/Port   :32768.5489-98ec-f00a / 128.1 
 Port Edged          :Config=default / Active=disabled 
 Point-to-point      :Config=auto / Active=true 
 Transit Limit       :147 packets/hello-time 
 Protection Type     :None 
 Port STP Mode       :STP 
 Port Protocol Type  :Config=auto / Active=dot1s 
 PortTimes           :Hello 2s MaxAge 20s FwDly 15s RemHop 0 
 TC or TCN send      :17 
 TC or TCN received  :52 
 BPDU Sent           :172 
          TCN: 0, Config: 172, RST: 0, MST: 0 
 BPDU Received       :206 
          TCN: 0, Config: 206, RST: 0, MST: 0 
 
[S4]display stp interface Ethernet 0/0/1  
----[CIST][Port1(Ethernet0/0/1)][FORWARDING]----  
 Port Protocol       :Enabled 
 Port Role           :Designated Port 
 Port Priority       :128 
 Port Cost(Dot1T )   :Config=auto / Active=199999 
 Designated Bridge/Port   :32768.5489-98ec-f00a / 128.1 
 Port Edged          :Config=default / Active=disabled 
 Point-to-point      :Config=auto / Active=true 
 Transit Limit       :147 packets/hello-time 
 Protection Type     :None 
 Port STP Mode       :STP  
 Port Protocol Type  :Config=auto / Active=dot1s

HCDP-IESN  Chapter 2 STP and SEP 
 
Page66 HUAWEI TECHNOLOGIES HC Series 
 
 PortTimes           :Hello 2s MaxAge 20s FwDly 15s RemHop 20 
 TC or TCN send      :37 
 TC or TCN received  :17 
 BPDU Sent           :181              
          TCN: 0, Config: 181, RST: 0, MST: 0 
 BPDU Received       :172              
          TCN: 0, Config: 172, RST: 0, MST: 0 
 
The Ethernet 0/0/1 interface of S3 is an alternate interface and that of S4 is 
a designated interface. Change the path cost to 2000000 for the E0/0/24 
interface of S4. 
[S4-Ethernet0/0/24]stp cost 2000000 
 
View role information about interfaces. 
[S3]display stp interface Ethernet 0/0/1 
----[CIST][Port1(Ethernet0/0/1)][FORWARDING]---- 
 Port Protocol       :Enabled 
 Port Role           :Designated Port 
 Port Priority       :128 
 Port Cost(Dot1T )   :Config=auto / Active=199999 
 Designated Bridge/Port   :32768.5489-98ec-f022 / 128.1 
 Port Edged          :Config=default / Active=disabled 
 Point-to-point      :Config=auto / Active=true 
 Transit Limit       :147 packets/hello-time 
 Protection Type     :None 
 Port STP Mode       :STP 
 Port Protocol Type  :Config=auto / Active=dot1s 
 PortTimes           :Hello 2s MaxAge 20s FwDly 15s RemHop 20 
 TC or TCN send      :52 
 TC or TCN received  :52 
 BPDU Sent           :284 
          TCN: 0, Config: 284, RST: 0, MST: 0 
 BPDU Received       :380 
          TCN: 0, Config: 380, RST: 0, MST: 0 
 
[S4]display stp interface Ethernet 0/0/1  
----[CIST][Port1(Ethernet0/0/1)][DISCARDING]---- 
 Port Protocol       :Enabled 
 Port Role           :Alternate Port 
 Port Priority       :128 
 Port Cost(Dot1T )   :Config=auto / Active=199999

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page67 
 
 Designated Bridge/Port   :4096.4c1f-cc45-aac1 / 128.30 
 Port Edged          :Config=default / Active=disabled 
 Point-to-point      :Config=auto / Active=true 
 Transit Limit       :147 packets/hello-time 
 Protection Type     :None 
 Port STP Mode       :STP  
 Port Protocol Type  :Config=auto / Active=dot1s 
 PortTimes           :Hello 2s MaxAge 20s FwDly 15s RemHop 0 
 TC or TCN send      :7 
 TC or TCN received  :162 
 BPDU Sent           :8              
          TCN: 7, Config: 1, RST: 0, MST: 0 
 BPDU Received       :1891              
          TCN: 0, Config: 1891, RST: 0, MST: 0 
 
The Ethernet 0/0/1 interface of S3 becomes a designated interface and 
that of S4 becomes an alternate interface. 
Step 5 Configure RSTP and verify the configuration. 
Configure IP addresses for the VLANIF interfaces of S1 and S2. Test 
whether the route from S1 to S2 is reachable. 
[S1]interface Vlanif 1 
[S1-Vlanif1]ip address 10.0.1.1 24 
 
[S2]interface Vlanif 1 
[S2-Vlanif1]ip address 10.0.1.2 24 
 
[S1]ping 10.0.1.2 
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=9 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=254 time=1 ms 
 
  --- 10.0.1.2 ping statistics --- 
    5 packet(s) transmitted 
    5 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 1/2/9 ms

HCDP-IESN  Chapter 2 STP and SEP 
 
Page68 HUAWEI TECHNOLOGIES HC Series 
 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/10 interface of  S1 is the root interface. Run the 
ping command to test whether the route from S1 to S2 is reachable for 20 
times. 
Note: Disable the GigabitEthernet 0/0/10 interface of S2 immediately after 
running the ping command on S1. 
 [S1]ping -c 20 10.0.1.2 
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=254 time=1 ms 
Dec 21 2011 16:20:44-05:13 S1 %%01IFNET/4/IF_STATE(l)[5]:Interface 
GigabitEthernet0/0/10 has turned into DOWN state. 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Reply from 10.0.1.2: bytes=56 Sequence=18 ttl=255 time=15 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=19 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=20 ttl=254 time=1 ms 
 
  --- 10.0.1.2 ping statistics --- 
    20 packet(s) transmitted

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page69 
 
    5 packet(s) received 
    75.00% packet loss 
    round-trip min/avg/max = 1/3/15 ms 
 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]shutdown 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/9 interface of  S1 becomes the root interface and 
enters the FORWARDING state. There are 15 expired packets and the 
network convergence takes 30 seconds. 
Enable the GigabitEthernet 0/0/10 interface of S2. 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]undo shutdown 
 
Configure RSTP. 
[S1]stp mode rstp 
 
[S2]stp mode rstp 
 
[S3]stp mode rstp 
 
[S4]stp mode rstp 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID      Port                      Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/10 interface of S1 becomes the root interface. 
Run the ping command to test whether the route from S1 to S2 is reachable

HCDP-IESN  Chapter 2 STP and SEP 
 
Page70 HUAWEI TECHNOLOGIES HC Series 
 
for 20 times. 
Note: Disable the GigabitEthernet 0/0/10 interface of S2 immediately after 
running the ping command on S1. 
[S1]ping -c 20 10.0.1.2 
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=254 time=1 ms 
Dec 21 2011 16:37:10-05:13 S1 %%01IFNET/4/IF_STATE(l)[7]:Interface 
GigabitEthernet0/0/10 has turned into DOWN state. 
    Request time out 
    Reply from 10.0.1.2: bytes=56 Sequence=7 ttl=255 time=10 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=8 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=9 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=10 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=11 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=12 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=13 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=14 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=15 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=16 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=17 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=18 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=19 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=20 ttl=254 time=1 ms 
 
  --- 10.0.1.2 ping statistics --- 
    20 packet(s) transmitted 
    19 packet(s) received 
    5.00% packet loss 
round-trip min/avg/max = 1/1/10 ms 
 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]shutdown 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page71 
 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/9 interface of  S1 becomes the root interface and 
enters the FORWARDING state. There is one expired packet and the network 
convergence takes 2 seconds. 
Enable the GigabitEthernet 0/0/10 interface of S2. 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]undo shutdown 
 
Step 6 Perform compatibility configuration between RSTP and 
STP . 
Enable STP on S1 and retain other configurations. 
[S1]stp mode stp 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ALTE  DISCARDING      NONE 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/10 interface of  S1 is the root interface. Run the 
ping command to test whether the route from S1 to S2 is reachable for 20 
times. 
Note: Disable the GigabitEthernet 0/0/10 interface of S2 immediately after 
running the ping command on S1. 
 [S1]ping -c 20 10.0.1.2 
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=254 time=1 ms 
Dec 21 2011 16:20:44-05:13 S1 %%01IFNET/4/IF_STATE(l)[5]:Interface 
GigabitEthernet0/0/10 has turned into DOWN state. 
    Request time out 
    Request time out

HCDP-IESN  Chapter 2 STP and SEP 
 
Page72 HUAWEI TECHNOLOGIES HC Series 
 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Request time out 
    Reply from 10.0.1.2: bytes=56 Sequence=18 ttl=255 time=15 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=19 ttl=254 time=1 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=20 ttl=254 time=1 ms 
 
  --- 10.0.1.2 ping statistics --- 
    20 packet(s) transmitted 
    5 packet(s) received 
    75.00% packet loss 
    round-trip min/avg/max = 1/3/15 ms 
 
[S2]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]shutdown 
 
View role information about interfaces of S1. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
The GigabitEthernet 0/0/9 interface of  S1 becomes the root interface and 
enters the FORWARDING state. There are 15 expired packets and the 
network convergence takes 30 seconds. 
RSTP is compatible with STP but it still uses the convergence mechanism 
of STP. 
Enable the GigabitEthernet 0/0/10 interface of S2. 
[S2]interface GigabitEthernet 0/0/10

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page73 
 
[S2-GigabitEthernet0/0/10]undo shutdown 
 
Step 7 Configure MSTP and verify the configuration. 
Create VLANs 2 to 20 and add interfaces to relevant VLANs. 
[S1]vlan batch 2 to 20 
Info: This operation may take a few seconds. Please wait for a moment...done. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]port link-type trunk 
[S1-GigabitEthernet0/0/9]port trunk allow-pass vlan 1 TO 20 
[S1-GigabitEthernet0/0/9]interface GigabitEthernet 0/0/10 
[S1-GigabitEthernet0/0/10]port link-type trunk 
[S1-GigabitEthernet0/0/10]port trunk allow-pass vlan 1 TO 20 
[S1-GigabitEthernet0/0/10]interface GigabitEthernet 0/0/13 
[S1-GigabitEthernet0/0/13]port link-type trunk 
[S1-GigabitEthernet0/0/13]port trunk allow-pass vlan 1 TO 20 
[S1-GigabitEthernet0/0/13]interface GigabitEthernet 0/0/14 
[S1-GigabitEthernet0/0/14]port link-type trunk 
[S1-GigabitEthernet0/0/14]port trunk allow-pass vlan 1 TO 20 
 
[S2]vlan batch 1 to 20 
Info: This operation may take a few seconds. Please wait for a moment...done. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]port link-type trunk 
[S2-GigabitEthernet0/0/9]port trunk allow-pass vlan 1 TO 20 
[S2-GigabitEthernet0/0/9]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]port link-type trunk 
[S2-GigabitEthernet0/0/10]port trunk allow-pass vlan 1 TO 20 
[S2-GigabitEthernet0/0/10]interface GigabitEthernet 0/0/23 
[S2-GigabitEthernet0/0/23]port link-type trunk 
[S2-GigabitEthernet0/0/23]port trunk allow-pass vlan 1 TO 20 
[S2-GigabitEthernet0/0/23]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]port link-type trunk 
[S2-GigabitEthernet0/0/24]port trunk allow-pass vlan 1 TO 20 
 
[S3]vlan batch 1 to 20 
Info: This operation may take a few seconds. Please wait for a moment...done. 
[S3]interface Ethernet0/0/1 
[S3-Ethernet0/0/1]port link-type trunk 
[S3-Ethernet0/0/1]port trunk allow-pass vlan 1 TO 20 
[S3-Ethernet0/0/1]interface Ethernet0/0/13

HCDP-IESN  Chapter 2 STP and SEP 
 
Page74 HUAWEI TECHNOLOGIES HC Series 
 
[S3-Ethernet0/0/13]port link-type trunk 
[S3-Ethernet0/0/13]port trunk allow-pass vlan 1 TO 20 
[S3-Ethernet0/0/13]interface Ethernet0/0/23 
[S3-Ethernet0/0/23]port link-type trunk 
[S3-Ethernet0/0/23]port trunk allow-pass vlan 1 TO 20 
 
[S4]vlan batch 1 to 20 
Info: This operation may take a few seconds. Please wait for a moment...done. 
[S4]interface Ethernet0/0/1 
[S4-Ethernet0/0/1]port link-type trunk 
[S4-Ethernet0/0/1]port trunk allow-pass vlan 1 TO 20 
[S4-Ethernet0/0/1]interface Ethernet0/0/14 
[S4-Ethernet0/0/14]port link-type trunk 
[S4-Ethernet0/0/14]port trunk allow-pass vlan 1 TO 20 
[S4-Ethernet0/0/14]interface Ethernet0/0/24 
[S4-Ethernet0/0/24]port link-type trunk 
[S4-Ethernet0/0/24]port trunk allow-pass vlan 1 TO 20 
 
Configure MSTP. 
Configure VLANs 110 to belong to instance 1 and VLANs 1120 to belong 
to instance 2. 
[S1]stp mode mstp 
[S1]stp region-configuration 
[S1-mst-region]region-name RG1 
[S1-mst-region]instance 1 vlan 1 TO 10 
[S1-mst-region]instance 2 vlan 11 to 20 
[S1-mst-region]active region-configuration 
Info: This operation may take a few seconds. Please wait for a moment....done. 
 
[S2]stp mode mstp 
[S2]stp region-configuration 
[S2-mst-region]region-name RG1 
[S2-mst-region]instance 1 vlan 1 TO 10 
[S2-mst-region]instance 2 vlan 11 to 20 
[S2-mst-region]active region-configuration 
Info: This operation may take a few seconds. Please wait for a moment....done. 
 
[S3]STP mode mstp 
Info: This operation may take a few seconds. Please wait for a moment.....done. 
[S3]stp region-configuration 
[S3-mst-region]region-name RG1 
[S3-mst-region]instance 1 vlan 1 to 10

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page75 
 
[S3-mst-region]instance 2 vlan 11 to 20 
 
[S4]STP mode mstp 
Info: This operation may take a few seconds. Please wait for a moment.....done. 
[S4]stp region-configuration 
[S4-mst-region]region-name RG1 
[S4-mst-region]instance 1 vlan 1 to 10 
[S4-mst-region]instance 2 vlan 11 to 20 
 
View the mapping between MSTP instances and VLANs. 
[S1]display stp region-configuration 
 Oper configuration 
   Format selector    :0 
   Region name        :RG1 
   Revision level     :0 
   Instance   VLANs Mapped 
      0       21 to 4094 
      1       1 to 10 
      2       11 to 20 
 
Set the priority of S1 to 4096 in instance 1 and to 8192 in instance 2. 
Set the priority of S2 to 4096 in instance 2 and to 8192 in instance 1. 
[S1]stp instance 1 priority 4096 
[S1]stp instance 2 priority 8192 
 
[S2]stp instance 2 priority 4096 
[S2]stp instance 1 priority 8192 
 
View status information about instance 1 and instance 2. 
[S1]display stp instance 1 
-------[MSTI 1 Global Info]------- 
MSTI Bridge ID      :4096.4c1f-cc45-aadc 
MSTI RegRoot/IRPC   :4096.4c1f-cc45-aadc / 0 
MSTI RootPortId     :0.0 
Master Bridge       :4096.4c1f-cc45-aac1 
Cost to Master      :20000 
TC received         :20 
TC count per hello  :0 
ĂĂoutput omitĂĂ 
 
[S2]display stp instance 2

HCDP-IESN  Chapter 2 STP and SEP 
 
Page76 HUAWEI TECHNOLOGIES HC Series 
 
-------[MSTI 2 Global Info]------- 
MSTI Bridge ID      :4096.4c1f-cc45-aac1 
MSTI RegRoot/IRPC   :4096.4c1f-cc45-aac1 / 0 
MSTI RootPortId     :0.0 
Master Bridge       :4096.4c1f-cc45-aac1 
Cost to Master      :0 
TC received         :16 
TC count per hello  :0 
ĂĂoutput omitĂĂ 
 
S1 is the root bridge of instance 1, and S2 is the root bridge of instance 2. 
View the role information about interfaces in instance 1. 
[S1]display stp instance 1 brief 
 MSTID  Port                        Role  STP State     Protection 
   1    GigabitEthernet0/0/9        DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/14       DESI  FORWARDING      NONE 
 
[S2]display stp instance 1 brief 
 MSTID  Port                        Role  STP State     Protection 
   1    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ALTE  DISCARDING      NONE 
   1    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp instance 1 brief 
 MSTID  Port                        Role  STP State     Protection 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/23              ALTE  DISCARDING      NONE 
 
[S4]display stp instance 1 brief 
 MSTID  Port                        Role  STP State     Protection 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/14              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/24              ALTE  DISCARDING      NONE 
 
In instance 1, S1 is the root bridge. Users in VLAN 1 to VLAN 10 of S3 
communicate with users in VLAN 1 to VLAN 10 of S1, S2, and S4 over the 
Ethernet 0/0/13 interface.

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page77 
 
View the role information about interfaces in instance 2. 
[S1]display stp instance 2 brief 
 MSTID  Port                        Role  STP State     Protection 
   2    GigabitEthernet0/0/9        ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ALTE  DISCARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp instance 2 brief 
 MSTID  Port                        Role  STP State     Protection 
   2    GigabitEthernet0/0/9        DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp instance 2 brief 
 MSTID  Port                        Role  STP State     Protection 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   2    Ethernet0/0/13              ALTE  DISCARDING      NONE 
   2    Ethernet0/0/23              ROOT  FORWARDING      NONE 
 
[S4]display stp instance 2 brief 
 MSTID  Port                        Role  STP State     Protection 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/14              DESI  FORWARDING      NONE 
   2    Ethernet0/0/24              ROOT  FORWARDING      NONE 
 
In instance 2, S2 is the root bridge. Users in VLAN 11 to VLAN 20 of S3 
communicate with users in VLAN 11 to VLAN 20 of S1, S2, and S4 over the 
Ethernet 0/0/23 interface. 
Additional Exercises: Analyzing and Verifying 
Figure out how MSTP balances data transmission load among VLANs in 
different areas. 
Figure out the reasons why RSTP can forward data quickly. 
Final Configurations 
[S1]display current-configuration 
# 
!Software Version V100R006C00SPC800

HCDP-IESN  Chapter 2 STP and SEP 
 
Page78 HUAWEI TECHNOLOGIES HC Series 
 
 sysname S1 
# 
vlan batch 2 to 20 
# 
 stp instance 0 priority 8192 
 stp instance 1 priority 4096 
stp instance 2 priority 8192 
# 
 stp region-configuration 
  region-name RG1 
  instance 1 vlan 1 to 10 
  instance 2 vlan 11 to 20 
  active region-configuration 
# 
interface Vlanif1 
 ip address 10.0.1.1 255.255.255.0 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface GigabitEthernet0/0/10 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface GigabitEthernet0/0/13 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
Return 
 
[S2]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S2 
# 
 vlan batch 2 to 20 
# 
stp instance 1 priority 8192 
 stp instance 2 priority 4096 
 stp instance 0 root secondary 
# 
 stp region-configuration

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page79 
 
  region-name RG1 
  instance 1 vlan 1 to 10 
  instance 2 vlan 11 to 20 
  active region-configuration 
# 
interface Vlanif1 
 ip address 10.0.1.2 255.255.255.0 
# 
interface GigabitEthernet0/0/9 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
 stp instance 0 port priority 32 
# 
interface GigabitEthernet0/0/10 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
 stp instance 0 port priority 16 
# 
interface GigabitEthernet0/0/23 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface GigabitEthernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
Return 
 
[S3]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S3 
# 
vlan batch 2 to 20 
# 
stp region-configuration 
  region-name RG1 
  instance 1 vlan 1 to 10 
  instance 2 vlan 11 to 20 
  active region-configuration 
# 
interface Ethernet0/0/1 
 port link-type trunk

HCDP-IESN  Chapter 2 STP and SEP 
 
Page80 HUAWEI TECHNOLOGIES HC Series 
 
 port trunk allow-pass vlan 2 to 20 
# 
interface Ethernet0/0/13 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface Ethernet0/0/23 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
Return 
 
[S4]display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S4 
# 
 vlan batch 2 to 20 
# 
 stp region-configuration 
  region-name RG1 
  instance 1 vlan 1 to 10 
  instance 2 vlan 11 to 20 
  active region-configuration 
# 
interface Ethernet0/0/1 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface Ethernet0/0/14 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
# 
interface Ethernet0/0/23 
# 
interface Ethernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 20 
 stp instance 0 cost 2000000 
# 
Return

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page81

HCDP-IESN  Chapter 2 STP and SEP 
 
Page82 HUAWEI TECHNOLOGIES HC Series 
 
Lab 2-2 Compatibility Between Multi-Region MSTP and STP 
(Optional) 
Learning Objectives 
The objectives of this lab are to learn and understand how to perform the 
following operations: 
/g120 Configure multi-instance Multiple Spanning Tree Protocol (MSTP) and 
multi-region MSTP. 
/g120 Configure MSTP and Spanning Tree Protocol (STP) to be compatible.  
/g120 Configure MSTP edge interface protection, designated interface 
protection, loop protection, and TC-BPDU protection. 
Topology 
 
Figure 2-2 Compatibility between multi-region MSTP and STP 
Scenario 
Assume that you are a network administrator of a company. In the Layer 2 
networking architecture, MSTP is deployed to: 
Avoid sub-optimal routes. 
Resolve the problem that some virtual local area network (VLAN) paths

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page83 
 
are unreachable if a single spanning tree instance is used. 
Implement load balancing. 
Interoperate with traditional spanning trees. 
Tasks 
Step 1 Perform basic configurations. 
Disable unused interfaces before carrying out the experiment. 
<S1>system-view 
Enter system view, return user view with Ctrl+Z. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]shutdown 
 
<S3>system-view 
Enter system view, return user view with Ctrl+Z. 
[S3]interface Ethernet 0/0/23 
[S3-Ethernet0/0/23]shutdown 
 
<S4>system-view 
Enter system view, return user view with Ctrl+Z. 
[S4]interface Ethernet 0/0/14 
[S4-Ethernet0/0/14]shutdown 
 
Create VLANs 3, 4, 5, 6, 7, and 8 on all switches. 
[S1]vlan batch 3 to 8 
 
[S2]vlan batch 3 to 8 
 
[S3]vlan batch 3 to 8 
 
[S4]vlan batch 3 to 8 
 
View information about the created VLANs. 
[S1]display vlan 
* : management-vlan 
--------------------- 
The total number of vlans is : 7 
VLAN ID Type         Status   MAC Learning Broadcast/Multicast/Unicast Property

HCDP-IESN  Chapter 2 STP and SEP 
 
Page84 HUAWEI TECHNOLOGIES HC Series 
 
---------------------------------------------------------------------------- 
1       common       enable   enable       forward   forward   forward default 
3       common       enable   enable       forward   forward   forward default 
4       common       enable   enable       forward   forward   forward default 
5       common       enable   enable       forward   forward   forward default 
6       common       enable   enable       forward   forward   forward default 
7       common       enable   enable       forward   forward   forward default 
8       common       enable   enable       forward   forward   forward default 
 
[S2]display vlan 
* : management-vlan 
--------------------- 
The total number of vlans is : 7 
VLAN ID Type         Status   MAC Learning Broadcast/Multicast/Unicast Property 
---------------------------------------------------------------------------- 
1       common       enable   enable       forward   forward   forward default 
3       common       enable   enable       forward   forward   forward default 
4       common       enable   enable       forward   forward   forward default 
5       common       enable   enable       forward   forward   forward default 
6       common       enable   enable       forward   forward   forward default 
7       common       enable   enable       forward   forward   forward default 
8       common       enable   enable       forward   forward   forward default 
 
[S3]display vlan 
* : management-vlan 
--------------------- 
The total number of vlans is : 7 
VLAN ID Type         Status   MAC Learning Broadcast/Multicast/Unicast Property 
---------------------------------------------------------------------------- 
1       common       enable   enable       forward   forward   forward default 
3       common       enable   enable       forward   forward   forward default 
4       common       enable   enable       forward   forward   forward default 
5       common       enable   enable       forward   forward   forward default 
6       common       enable   enable       forward   forward   forward default 
7       common       enable   enable       forward   forward   forward default 
8       common       enable   enable       forward   forward   forward default 
 
[S4]display vlan 
* : management-vlan 
--------------------- 
The total number of vlans is : 7 
VLAN ID Type         Status   MAC Learning Broadcast/Multicast/Unicast Property 
----------------------------------------------------------------------------

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page85 
 
1       common       enable   enable       forward   forward   forward default 
3       common       enable   enable       forward   forward   forward default 
4       common       enable   enable       forward   forward   forward default 
5       common       enable   enable       forward   forward   forward default 
6       common       enable   enable       forward   forward   forward default 
7       common       enable   enable       forward   forward   forward default 
8       common       enable   enable       forward   forward   forward default 
 
Set the type to Trunk for the links between switches. Enable the links to 
receive bridge protocol data unit (BDPU) packets and to allow the access from 
all VLANs. Note: The direct link between S2 and S3 remains unchanged. 
[S1]interface GigabitEthernet 0/0/13 
[S1-GigabitEthernet0/0/13]port link-type trunk 
[S1-GigabitEthernet0/0/13]port trunk allow-pass vlan all 
[S1-GigabitEthernet0/0/13]bpdu enable 
[S1-GigabitEthernet0/0/13]interface GigabitEthernet 0/0/10 
[S1-GigabitEthernet0/0/10]port link-type trunk 
[S1-GigabitEthernet0/0/10]port trunk allow-pass vlan all 
[S1-GigabitEthernet0/0/10]bpdu enable 
 
[S2]interface GigabitEthernet 0/0/24 
[S2-GigabitEthernet0/0/24]port link-type trunk 
[S2-GigabitEthernet0/0/24]port trunk allow-pass vlan all 
[S2-GigabitEthernet0/0/24]bpdu enable 
[S2-GigabitEthernet0/0/24]interface GigabitEthernet 0/0/10 
[S2-GigabitEthernet0/0/10]port link-type trunk 
[S2-GigabitEthernet0/0/10]port trunk allow-pass vlan all 
[S2-GigabitEthernet0/0/10]bpdu enable 
 
[S3]interface  Ethernet0/0/1 
[S3-Ethernet0/0/1]port link-type trunk 
[S3-Ethernet0/0/1]port trunk allow-pass vlan all 
[S3-Ethernet0/0/1]bpdu enable 
[S3-Ethernet0/0/1]interface Ethernet0/0/13 
[S3-Ethernet0/0/13]port link-type trunk 
[S3-Ethernet0/0/13]port trunk allow-pass vlan all 
[S3-Ethernet0/0/13]bpdu enable 
 
[S4]interface  Ethernet0/0/1 
[S4-Ethernet0/0/1]port link-type trunk 
[S4-Ethernet0/0/1]port trunk allow-pass vlan all 
[S4-Ethernet0/0/1]bpdu enable

HCDP-IESN  Chapter 2 STP and SEP 
 
Page86 HUAWEI TECHNOLOGIES HC Series 
 
[S4-Ethernet0/0/1]interface Ethernet0/0/24 
[S4-Ethernet0/0/24]port link-type trunk 
[S4-Ethernet0/0/24]port trunk allow-pass vlan all 
[S4-Ethernet0/0/24]bpdu enable 
 
Step 2 Configure multi-instance MSTP . 
Enable MSTP in the system view. 
[S1]stp enable 
[S1]stp mode mstp 
 
[S2]stp enable 
[S2]stp mode mstp 
 
[S3]stp enable 
[S3]stp mode mstp 
 
[S4]stp enable 
[S4]stp mode mstp 
 
Configure all switches to belong to RG1 and set the revision level to 1. 
Create instance 1 and map it to VLANs 3, 4 and 5. Create instance 2 and map 
it to VLANs 6, 7, and 8. Then activate the region configurations. 
[S1]stp region-configuration 
[S1-mst-region]region-name RG1 
[S1-mst-region]revision-level 1 
[S1-mst-region]instance 1 vlan 3 4 5 
[S1-mst-region]instance 2 vlan 6 7 8 
[S1-mst-region]active region-configuration 
 
[S2]stp  region-configuration 
[S2-mst-region]region-name RG1 
[S2-mst-region]revision-level 1 
[S2-mst-region]instance 1 vlan 3 4 5 
[S2-mst-region]instance 2 vlan 6 7 8 
[S2-mst-region]active region-configuration 
 
[S3]stp  region-configuration 
[S3-mst-region]region-name RG1 
[S3-mst-region]revision-level 1

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page87 
 
[S3-mst-region]instance 1 vlan 3 4 5 
[S3-mst-region]instance 2 vlan 6 7 8 
[S3-mst-region]active region-configuration 
 
[S4]stp  region-configuration 
[S4-mst-region]region-name RG1 
[S4-mst-region]revision-level 1 
[S4-mst-region]instance 1 vlan 3 4 5 
[S4-mst-region]instance 2 vlan 6 7 8 
[S4-mst-region]active region-configuration 
 
View MSTP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ROOT  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page88 HUAWEI TECHNOLOGIES HC Series 
 
   0    Ethernet0/0/24              ALTE  DISCARDING      NONE 
   1    Ethernet0/0/1               ROOT  FORWARDING      NONE 
   1    Ethernet0/0/24              ALTE  DISCARDING      NONE 
   2    Ethernet0/0/1               ROOT  FORWARDING      NONE 
   2    Ethernet0/0/24              ALTE  DISCARDING      NONE 
 
S1 is the root switch and the E0/0/24 interface of S4 is the alternate 
interface for all MSTP processes. 
In instance 2, set the priority to 0 for S2, to 4096 for S1, and to 8192 for S4 
so that S2 becomes the root switch in instance 2. 
[S2]stp instance 2 priority 0 
 
[S1]stp instance 2 priority 4096 
 
[S4]stp instance 2 priority 8192 
 
View MSTP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page89 
 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   0    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   1    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/24              ROOT  FORWARDING      NONE 
 
S2 becomes the root switch in instance 2 and the E0/0/1 interface of S3 
becomes the alternate interface. The status of switches does not change in 
instance 1. This proves that MSTP instances calculate spanning tree instances 
independently. 
Step 3 Configure multi-region MSTP. 
Delete the MSTP region and priority configurations made in step 2 for all 
switches. 
[S1]undo stp region-configuration 
[S1]undo stp instance 2 priority 
 
[S2]undo stp region-configuration 
[S2]undo stp instance 2 priority 
 
[S3]undo stp region-configuration 
 
[S4]undo stp region-configuration 
[S4]undo stp instance 2 priority 
 
Configure S1 and S3 to belong to RG1 and set the revision level to 1. 
Create instance 1 and map it to VLANs 3, 4, and 5. 
Create instance 2 and map it to VLANs 6, 7, and 8. 
[S1]stp region-configuration 
[S1-mst-region]region-name RG1 
[S1-mst-region]revision-level 1 
[S1-mst-region]instance 1 vlan 3 4 5

HCDP-IESN  Chapter 2 STP and SEP 
 
Page90 HUAWEI TECHNOLOGIES HC Series 
 
[S1-mst-region]instance 2 vlan 6 7 8 
[S1-mst-region]active region-configuration 
 
[S3]stp region-configuration 
[S3-mst-region]region-name RG1 
[S3-mst-region]revision-level 1 
[S3-mst-region]instance 1 vlan 3 4 5 
[S3-mst-region]instance 2 vlan 6 7 8 
[S3-mst-region]active region-configuration 
 
Configure S2 and S4 to belong to RG2 and set the revision level to 2. 
Create instance 1 and map it to VLANs 3, 4, and 5. 
Create instance 2 and map it to VLANs 6, 7, and 8. Then activate the 
region configurations. 
[S2]stp region-configuration 
[S2-mst-region]region-name RG2 
[S2-mst-region]revision-level 2 
[S2-mst-region]instance 1 vlan 3 4 5 
[S2-mst-region]instance 2 vlan 6 7 8 
[S2-mst-region]active region-configuration 
 
[S4]stp region-configuration 
[S4-mst-region]region-name RG2 
[S4-mst-region]revision-level 2 
[S4-mst-region]instance 1 vlan 3 4 5 
[S4-mst-region]instance 2 vlan 6 7 8 
[S4-mst-region]active region-configuration 
 
View MSTP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page91 
 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   0    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   1    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   2    Ethernet0/0/24              ROOT  FORWARDING      NONE 
 
S1 is the root switch and the E0/0/1 interface of S4 is the alternate 
interface. 
Set the priority to 0 for S3 in instance 0 so that S3 is the Common and 
Internal Spanning Tree (CIST) root, and to 0 in instance 1 so that S3 is the 
regional root in instance 1. Set the priority to 0 for S4 in instance 1 so that S4 is 
the regional root in instance 1. 
[S3]stp instance 0 priority 0 
[S3]stp instance 1 priority 0 
 
[S4]stp instance 1 priority 0 
 
View MSTP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page92 HUAWEI TECHNOLOGIES HC Series 
 
   1    GigabitEthernet0/0/13       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              DESI  FORWARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              DESI  FORWARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   0    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   1    Ethernet0/0/24              DESI  FORWARDING      NONE 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   2    Ethernet0/0/24              ROOT  FORWARDING      NONE 
 
Delete the MSTP configurations from S2 and S4. Configure S2 and S4 to 
belong to RG2 and set the revision level to 2. Create instance 1 and map it to 
VLANs 6, 7, and 8. Create instance 2 a nd map it to VLANs 3, 4 and 5. Then 
activate the region configurations. 
[S2]undo stp region-configuration 
 
[S3]undo stp instance 0 priority 
[S3]undo stp instance 1 priority 
 
[S4]undo stp region-configuration

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page93 
 
[S4]undo stp instance 1 priority 
 
[S2]stp region-configuration 
[S2-mst-region]region-name RG2 
[S2-mst-region]revision-level 2 
[S2-mst-region]instance 1 vlan 6 7 8 
[S2-mst-region]instance 2 vlan 3 4 5 
[S2-mst-region]active region-configuration 
 
[S4]stp region-configuration 
[S4-mst-region]region-name RG2 
[S4-mst-region]revision-level 2 
[S4-mst-region]instance 1 vlan 6 7 8 
[S4-mst-region]instance 2 vlan 3 4 5 
[S4-mst-region]active region-configuration 
 
View MSTP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       MAST  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page94 HUAWEI TECHNOLOGIES HC Series 
 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   0    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   1    Ethernet0/0/24              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
 
Except instance 0, the MSTP instances in other regions calculate spanning 
trees independently, regardless of whether the instances contain or map the 
same VLAN. That is, the spanning tree processes in the same region run 
independent of each other. 
Step 4 Configure MSTP and STP to be compatible. 
Configure S1, S2, and S3 to belong to the same MSTP region. Enable 
STP on S4. 
Delete the MSTP configurations from S2 and then create RG1 on S2. 
Create instance 1 and map it to VLANs 3, 4, and 5. Create instance 2 and map 
it to VLANs 6, 7, and 8. Then activate the region configurations. 
[S2]undo stp region-configuration 
[S2]stp region-configuration 
[S2-mst-region]region-name RG1 
[S2-mst-region]revision-level 1 
[S2-mst-region]instance 1 vlan 3 4 5 
[S2-mst-region]instance 2 vlan  6 7 8 
[S2-mst-region]active region-configuration 
 
Enable the S0/0/23 interfaces of S2 and S3. 
Configure the direct link between S2 and S3 to work in Trunk mode, 
receive bridge protocol data unit (BPDU) packets, and allow the access from 
all VLANs. 
[S2]int GigabitEthernet 0/0/23 
[S2-GigabitEthernet0/0/23]undo shutdown 
[S2-GigabitEthernet0/0/23]port link-type trunk 
[S2-GigabitEthernet0/0/23]port trunk all vlan all 
[S2-GigabitEthernet0/0/23]bpdu enable

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page95 
 
[S3]int Ethernet0/0/23 
[S3-Ethernet0/0/23]undo shutdown 
[S3-Ethernet0/0/23]port link-type trunk 
[S3-Ethernet0/0/23]port trunk allow-pass vlan all 
[S3-Ethernet0/0/23]bpdu enable 
 
Delete the MSTP configurations from S4 and enable STP on S4. 
[S4]undo stp region-configuration 
[S4]stp mode stp 
 
View STP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       DESI  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   0    Ethernet0/0/23              ALTE  DISCARDING      NONE 
   1    Ethernet0/0/1               DESI  FORWARDING      NONE 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/23              ALTE  DISCARDING      NONE 
   2    Ethernet0/0/1               DESI  FORWARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page96 HUAWEI TECHNOLOGIES HC Series 
 
   2    Ethernet0/0/23              ALTE  DISCARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ROOT  FORWARDING      NONE 
   0    Ethernet0/0/24              ALTE  DISCARDING      NONE 
 
STP-enabled S4 and instance 0 in MSTP-enabled S1, S2, and S3 
calculate the CIST together. S1 is the CIST root. 
Set the priority to 4096 for S4 so that it becomes the CIST root. 
[S4]stp priority 4096 
 
View STP information. 
[S1]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   0    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/13       DESI  FORWARDING      NONE 
 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       MAST  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       MAST  FORWARDING      NONE 
 
[S3]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   0    Ethernet0/0/13              ALTE  DISCARDING      NONE 
   0    Ethernet0/0/23              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/1               ALTE  DISCARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page97 
 
   1    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   1    Ethernet0/0/23              ALTE  DISCARDING      NONE 
   2    Ethernet0/0/1               ALTE  DISCARDING      NONE 
   2    Ethernet0/0/13              ROOT  FORWARDING      NONE 
   2    Ethernet0/0/23              ALTE  DISCARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      NONE 
   0    Ethernet0/0/24              DESI  FORWARDING      NONE 
 
S4 becomes the CIST root and all interfaces on S4 become designated 
interfaces. 
Step 5 Configure designated interface protection. 
Configure designated interface protection for the E0/0/1 and E0/0/24 
interfaces of S4. 
[S4]int Ethernet0/0/1 
[S4-Ethernet0/0/1]stp root-protection 
[S4-Ethernet0/0/1]int Ethernet0/0/24 
[S4-Ethernet0/0/24]stp root-protection 
 
View STP information about S4. 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      ROOT 
   0    Ethernet0/0/24              DESI  DISCARDING      ROOT 
 
Set the priority to 0 for S2 in instance 0 to simulate preemption of the CIST 
root. 
[S2]stp instance 0 priority 0 
 
View STP information about S2 and S4. 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       DESI  LEARNING        NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE

HCDP-IESN  Chapter 2 STP and SEP 
 
Page98 HUAWEI TECHNOLOGIES HC Series 
 
   1    GigabitEthernet0/0/23       DESI  LEARNING        NONE 
   1    GigabitEthernet0/0/24       DESI  LEARNING        NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/23       DESI  LEARNING        NONE 
   2    GigabitEthernet0/0/24       DESI  LEARNING        NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  DISCARDING      ROOT 
   0    Ethernet0/0/24              DESI  DISCARDING      ROOT 
 
The interfaces of S4 enter the DISCARDING state and do not forward 
packets. This indicates that the roles of these interfaces do not change and S4 
is still the root switch. 
Delete the priority configurations of instance 0 from S2. 
[S2]undo stp instance 0 priority 
 
View STP information about S2 and S4. 
[S2]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/10       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   0    GigabitEthernet0/0/24       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   1    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   1    GigabitEthernet0/0/24       MAST  FORWARDING      NONE 
   2    GigabitEthernet0/0/10       ROOT  FORWARDING      NONE 
   2    GigabitEthernet0/0/23       DESI  FORWARDING      NONE 
   2    GigabitEthernet0/0/24       MAST  FORWARDING      NONE 
 
[S4]display stp brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/1               DESI  FORWARDING      ROOT 
   0    Ethernet0/0/24              DESI  FORWARDING      ROOT 
 
The interfaces restore to the original state (FORWARDING) if they do not 
receive configuration messages with higher priorities within the period 
specified by Max Age. The default period is 20 seconds.

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     Page99 
 
Step 6 Configure edge interface protection. 
Enable the G0/0/9 interface of S2. 
[S2]interface GigabitEthernet 0/0/9 
[S2-GigabitEthernet0/0/9]undo shutdown 
 
Configure the G0/0/9 interface of S1 as an edge interface and enable edge 
interface protection in global mode. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]undo shutdown 
[S1-GigabitEthernet0/0/9]stp edged-port enable 
[S1-GigabitEthernet0/0/9]quit 
[S1]stp bpdu-protection 
 
View STP information about S1. 
[S1]display stp interface GigabitEthernet 0/0/9 brief 
 MSTID  Port                        Role  STP State     Protection 
   0    GigabitEthernet0/0/9        DESI  FORWARDING      BPDU 
 
Enable the G0/0/9 interface of S1 so that it can receive BPDU packets and 
simulate an attack to S1. 
[S1]interface GigabitEthernet 0/0/9 
[S1-GigabitEthernet0/0/9]undo shutdown 
 
Observe S1. 
Dec 21 2011 08:39:51-05:13 S1 %%01IFNET/4/IF_STATE(l)[3]:Interface 
GigabitEthernet0/0/9 has turned into UP state. 
Dec 21 2011 08:39:51-05:13 S1 %%01MSTP/4/BPDU_PROTECTION(l)[4]:This edged-port 
GigabitEthernet0/0/9 that enabled BPDU-Protection will be shutdown, because it 
received BPDU packet! 
Dec 21 2011 08:39:52-05:13 S1 %%01IFNET/4/IF_STATE(l)[5]:Interface 
GigabitEthernet0/0/9 has turned into DOWN state. 
 
After edge interface protection is configured, the edge interface receives 
BPDU packets once being enabled, and then this interface is disabled 
automatically.

HCDP-IESN  Chapter 2 STP and SEP 
 
Page100 HUAWEI TECHNOLOGIES HC Series 
 
Step 7 Configure loop protection. 
Configure loop protection on the E0/0/23 interface of S3. 
[S3]interface Ethernet0/0/23 
[S3-Ethernet0/0/23]stp loop-protection 
 
View STP information about the E0/0/23 interface of S3. 
[S3]display stp interface Ethernet 0/0/23 brief 
 MSTID  Port                        Role  STP State     Protection 
   0    Ethernet0/0/23              ROOT  FORWARDING      LOOP 
   1    Ethernet0/0/23              ALTE  DISCARDING      LOOP 
   2    Ethernet0/0/23              ALTE  DISCARDING      LOOP 
Step 8 Configure TC-BPDU protection. 
Enable the protection function for TC-BPDU packets. 
[S1]stp tc-protection 
 
Additional Exercises: Analyzing and Verifying 
Figure out the impact if the MSTP region names are the same but the 
revision levels are different on switches. 
In step 4, figure out the status changes  of the interfaces of the four 
switches if the priority is changed to 0 for S3 in instance 1. 
Final Configurations 
<S1>display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S1 
# 
 vlan batch 3 to 8 
# 
 stp bpdu-protection 
 stp tc-protection 
#

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     
Page101 
 
 stp region-configuration 
  region-name RG1 
  revision-level 1 
  instance 1 vlan 3 to 5 
  instance 2 vlan 6 to 8 
  active region-configuration 
# 
interface GigabitEthernet0/0/9 
 shutdown 
stp edged-port enable 
# 
interface GigabitEthernet0/0/10 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
interface GigabitEthernet0/0/13 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
return 
<S2>display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S2 
# 
 vlan batch 3 to 8 
# 
 stp region-configuration 
  region-name RG1 
  revision-level 1 
  instance 1 vlan 3 to 5 
  instance 2 vlan 6 to 8 
  active region-configuration 
# 
interface GigabitEthernet0/0/9 
# 
interface GigabitEthernet0/0/10 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
interface GigabitEthernet0/0/23

HCDP-IESN  Chapter 2 STP and SEP 
 
Page102 HUAWEI TECHNOLOGIES HC Series 
 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
interface GigabitEthernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
return 
<S3>display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S3 
# 
 vlan batch 3 to 8 
# 
 stp region-configuration 
  region-name RG1 
  revision-level 1 
  instance 1 vlan 3 to 5 
  instance 2 vlan 6 to 8 
  active region-configuration 
# 
interface Ethernet0/0/1 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
interface Ethernet0/0/13 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
# 
interface Ethernet0/0/23 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
 stp loop-protection 
# 
return 
<S4>display current-configuration 
# 
!Software Version V100R006C00SPC800 
 sysname S4 
#

HCDP-IESN  Chapter 2 STP and SEP 
 
HC Series HUAWEI TECHNOLOGIES     
Page103 
 
 vlan batch 3 to 8 30 
# 
 stp mode stp 
 stp instance 0 priority 4096 
# 
interface Vlanif30 
 ip address 100.100.100.8 255.255.255.0 
# 
interface Ethernet0/0/1 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
 stp root-protection 
 undo ntdp enable 
 undo ndp enable 
# 
interface Ethernet0/0/14 
 shutdown 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface Ethernet0/0/23 
 port link-type access 
 port default vlan 30 
 undo ntdp enable 
 undo ndp enable 
 bpdu disable 
# 
interface Ethernet0/0/24 
 port link-type trunk 
 port trunk allow-pass vlan 2 to 4094 
 stp root-protection 
 undo ntdp enable 
 undo ndp enable 
# 
Return

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page104 HUAWEI TECHNOLOGIES HC Series 
 
Chapter 3 Implementing MPLS technologies 
Lab 3-1 MPLS LDP Configuration 
Learning Objectives 
The objectives of this lab are to learn and understand: 
/g120 Methods used to enable and disable MPLS 
/g120 MPLS LDP Configuration 
/g120 Methods used to configure LSP sessions using MPLS LDP 
/g120 Methods used to modify the LDP LSP trigger policy on each LSR 
Topology 
 
Figure 3-1 MPLS LDP topology 
Scenario 
Assume that you are a network administrator of an enterprise. Your 
enterprise uses an IP network with poor forwarding performance. You need to 
use MPLS to improve the forwarding rate of routers. Static LSPs are 
configured manually, while LDP is a protocol developed for label distribution. 
To perform flexible configuration, use LDP to set up MPLS LSPs.

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page105 
 
Tasks 
Step 1 Configure IP addresses. 
Configure IP addresses and masks for all routers. 
<Huawei>system-view  
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname S1 
[S1]interface Vlanif 1 
[S1-Vlanif1]ip address 10.0.1.2 24 
 
<Huawei>system-view  
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R1 
[R1]interface GigabitEthernet 0/0/1 
[R1-GigabitEthernet0/0/1]ip address 10.0.1.1 24 
[R1-GigabitEthernet0/0/1]quit 
[R1]interface s1/0/0 
[R1-Serial1/0/0]ip address 10.0.12.1 24 
[R1-Serial1/0/0]quit 
[R1]interface loopback 0 
[R1-LoopBack0]ip address 2.2.2.2 24 
 
<Huawei>system-view  
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R2 
[R2]interface s1/0/0 
[R2-Serial1/0/0]ip address 10.0.12.2 24 
[R2-Serial1/0/0]quit 
[R2]interface s2/0/0 
[R2-Serial2/0/0]ip address 10.0.23.2 24 
[R2-Serial2/0/0]quit 
[R2]interface loopback 0 
[R2-LoopBack0]ip address 3.3.3.3 24 
 
<Huawei>system-view  
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname R3 
[R3]interface GigabitEthernet 0/0/2 
[R3-GigabitEthernet0/0/2]ip address 10.0.2.1 24

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page106 HUAWEI TECHNOLOGIES HC Series 
 
[R3-GigabitEthernet0/0/2]quit 
[R3]interface s2/0/0 
[R3-Serial2/0/0]ip address 10.0.23.3 24 
[R3-Serial2/0/0]quit 
[R3]interface loopback 0 
[R3-LoopBack0]ip address 4.4.4.4 24 
 
<Huawei>system-view  
Enter system view, return user view with Ctrl+Z. 
[Huawei]sysname S2 
[S2]interface Vlanif 1 
[S2-Vlanif1]ip address 10.0.2.2 24 
 
After the configurations are complete, test the connectivity of direct links. 
Step 2 Configure single-area OSPF. 
Configure network segments including 10.0.12.0/24, 10.0.23.0/24, 
10.0.1.0/24, and 10.0.2.0/24 to belong to OSPF area 0.
[S1]ospf 1 router-id 1.1.1.1 
[S1-ospf-1]area 0  
[S1-ospf-1-area-0.0.0.0]network 10.0.1.0 0.0.0.255 
 
[R1]ospf 1 router-id 2.2.2.2 
[R1-ospf-1]area 0  
[R1-ospf-1-area-0.0.0.0]network 10.0.1.0 0.0.0.255 
[R1-ospf-1-area-0.0.0.0]network 10.0.12.0 0.0.0.255 
[R1-ospf-1-area-0.0.0.0]network 2.2.2.0 0.0.0.255 
 
[R2]ospf 1 router-id 3.3.3.3 
[R2-ospf-1]area 0  
[R2-ospf-1-area-0.0.0.0]network 10.0.12.0 0.0.0.255 
[R2-ospf-1-area-0.0.0.0]network 10.0.23.0 0.0.0.255 
[R2-ospf-1-area-0.0.0.0]network 3.3.3.0 0.0.0.255   
 
[R3]ospf 1 router-id 4.4.4.4 
[R3-ospf-1]area 0 
[R3-ospf-1-area-0.0.0.0]net 
[R3-ospf-1-area-0.0.0.0]network 10.0.23.0 0.0.0.255 
[R3-ospf-1-area-0.0.0.0]network 10.0.2.0 0.0.0.255 
[R3-ospf-1-area-0.0.0.0]network 4.4.4.0 0.0.0.255

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page107 
 
 
[S2]ospf 1 router-id 5.5.5.5 
[S2-ospf-1]area 0 
[S2-ospf-1-area-0.0.0.0]network 10.0.2.0 0.0.0.255 
 
After the configurations are complete , check routing tables and test the 
network connectivity. 
[R2]ping 10.0.1.2 
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=253 time=36 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=253 time=31 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=253 time=31 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=253 time=31 ms 
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=253 time=31 ms 
 
  --- 10.0.1.2 ping statistics --- 
    5 packet(s) transmitted 
    5 packet(s) received 
    0.00% packet loss 
    round-trip min/avg/max = 31/32/36 ms 
 
[R2]ping 10.0.2.2 
  PING 10.0.2.2: 56  data bytes, press CTRL_C to break 
    Reply from 10.0.2.2: bytes=56 Sequence=1 ttl=253 time=38 ms 
    Reply from 10.0.2.2: bytes=56 Sequence=2 ttl=253 time=33 ms 
    Reply from 10.0.2.2: bytes=56 Sequence=3 ttl=253 time=33 ms 
    Reply from 10.0.2.2: bytes=56 Sequence=4 ttl=253 time=33 ms 
    Reply from 10.0.2.2: bytes=56 Sequence=5 ttl=253 time=33 ms 
 
  --- 10.0.2.2 ping statistics --- 
    5 packet(s) transmitted 
    5 packet(s) received 
    0.00% packet loss 
round-trip min/avg/max = 33/34/38 ms 
 
Run the display ip routing-table  command to view the OSPF routing 
tables of each router.  
[R2]display ip routing-table  
Route Flags: R - relay, D - download to fib 
---------------------------------------------------------------------------- 
Routing Tables: Public

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page108 HUAWEI TECHNOLOGIES HC Series 
 
         Destinations : 14       Routes : 14        
 
Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface 
 
       10.0.1.0/24   OSPF    10   1563        D   10.0.12.1       Serial1/0/0 
       10.0.2.0/24   OSPF    10   1563        D   10.0.23.3       Serial2/0/0 
      10.0.12.0/24   Direct  0    0           D   10.0.12.2       Serial1/0/0 
      10.0.12.1/32   Direct  0    0           D   10.0.12.1       Serial1/0/0 
      10.0.12.2/32   Direct  0    0           D   127.0.0.1       InLoopBack0 
    10.0.12.255/32   Direct  0    0           D   127.0.0.1       InLoopBack0 
      10.0.23.0/24   Direct  0    0           D   10.0.23.2       Serial2/0/0 
      10.0.23.2/32   Direct  0    0           D   127.0.0.1       InLoopBack0 
      10.0.23.3/32   Direct  0    0           D   10.0.23.3       Serial2/0/0 
    10.0.23.255/32   Direct  0   0           D   127.0.0.1       InLoopBack0 
      127.0.0.0/8    Direct  0    0           D   127.0.0.1       InLoopBack0 
      127.0.0.1/32   Direct  0    0           D   127.0.0.1       InLoopBack0 
127.255.255.255/32  Direct  0   0           D   127.0.0.1       InLoopBack0 
255.255.255.255/32  Direct  0   0           D   127.0.0.1       InLoopBack0 
 
Step 3 Configure MPLS LDP. 
Configure global MPLS and LDP on each LSR. 
[R1]mpls lsr-id 2.2.2.2 
[R1]mpls 
Info: Mpls starting, please wait... OK! 
[R1-mpls]mpls ldp 
 
[R2]mpls lsr-id 3.3.3.3 
[R2]mpls 
Info: Mpls starting, please wait... OK! 
[R2-mpls]mpls ldp 
 
[R3]mpls lsr-id 4.4.4.4 
[R3]mpls 
Info: Mpls starting, please wait... OK! 
[R3-mpls]mpls ldp 
 
Configure MPLS and LDP on interfaces of each LSR. 
[R1]interface Serial 1/0/0 
[R1-Serial1/0/0]mpls

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page109 
 
[R1-Serial1/0/0]mpls ldp 
 
[R2]interface Serial 1/0/0 
[R2-Serial1/0/0]mpls 
[R2-Serial1/0/0]mpls ldp 
[R2-Serial1/0/0]interface Serial 2/0/0 
[R2-Serial2/0/0]mpls 
[R2-Serial2/0/0]mpls ldp 
 
[R3]interface Serial 2/0/0  
[R3-Serial2/0/0]mpls 
[R3-Serial2/0/0]mpls ldp 
 
After the configurations are complete, run the display mpls ldp session  
command on R1, R2, and R3. The status of the local LDP session between R1, 
R2 and R3 is Operational. 
[R1]display mpls ldp session 
 LDP Session(s) in Public Network  
 Codes: LAM(Label Advertisement Mode), SsnAge Unit(DDDD:HH:MM) 
 A '*' before a session means the session is being deleted. 
 ---------------------------------------------------------------------------- 
 PeerID             Status      LAM  SsnRole  SsnAge      KASent/Rcv 
 ---------------------------------------------------------------------------- 
 3.3.3.3:0          Operational DU   Passive  0000:00:10  41/41 
 ---------------------------------------------------------------------------- 
TOTAL: 1 session(s) Found. 
 
[R2]display mpls ldp session  
 LDP Session(s) in Public Network  
 Codes: LAM(Label Advertisement Mode), SsnAge Unit(DDDD:HH:MM) 
 A '*' before a session means the session is being deleted. 
 ---------------------------------------------------------------------------- 
 PeerID             Status      LAM  SsnRole  SsnAge      KASent/Rcv 
 ---------------------------------------------------------------------------- 
 2.2.2.2:0          Operational DU   Active   0000:00:11  46/46 
 4.4.4.4:0          Operational DU   Passive  0000:00:10  43/43 
 ---------------------------------------------------------------------------- 
 TOTAL: 2 session(s) Found. 
 
[R3]display mpls ldp session 
 LDP Session(s) in Public Network

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page110 HUAWEI TECHNOLOGIES HC Series 
 
 Codes: LAM(Label Advertisement Mode), SsnAge Unit(DDDD:HH:MM) 
 A '*' before a session means the session is being deleted. 
 ---------------------------------------------------------------------------- 
 PeerID             Status      LAM  SsnRole  SsnAge      KASent/Rcv 
 ---------------------------------------------------------------------------- 
 3.3.3.3:0          Operational DU   Active   0000:00:11  46/46 
 ---------------------------------------------------------------------------- 
 TOTAL: 1 session(s) Found. 
 
Step 4 Set up LSPs using LDP. 
All LSRs are triggered to establis h LDP LSPs based on the host route, 
which is the default trigger policy. 
Run the display mpls ldp lsp  command on LSRs. All host routes are 
triggered to establish LDP LSPs.
[R1]display mpls ldp lsp 
  LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface  
 ---------------------------------------------------------------------------- 
 2.2.2.2/32         3/NULL         3.3.3.3         127.0.0.1       InLoop0 
*2.2.2.2/32         Liberal/1024                   DS/3.3.3.3  
 3.3.3.3/32         NULL/3         -               10.0.12.2       S1/0/0 
 3.3.3.3/32         1024/3         3.3.3.3         10.0.12.2       S1/0/0 
 4.4.4.4/32         NULL/1025      -               10.0.12.2       S1/0/0 
 4.4.4.4/32         1025/1025      3.3.3.3         10.0.12.2       S1/0/0 
 ---------------------------------------------------------------------------- 
 TOTAL: 5 Normal LSP(s) Found. 
 TOTAL: 1 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state  
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP 
 
[R2]display mpls ldp lsp 
 LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page111 
 
 ---------------------------------------------------------------------------- 
 2.2.2.2/32         NULL/3         -               10.0.12.1       S1/0/0 
 2.2.2.2/32         1024/3         2.2.2.2         10.0.12.1       S1/0/0 
 2.2.2.2/32         1024/3         4.4.4.4         10.0.12.1       S1/0/0 
*2.2.2.2/32         Liberal/1024                   DS/4.4.4.4  
 3.3.3.3/32         3/NULL         2.2.2.2         127.0.0.1       InLoop0 
 3.3.3.3/32         3/NULL         4.4.4.4         127.0.0.1       InLoop0 
*3.3.3.3/32         Liberal/1024                   DS/2.2.2.2  
*3.3.3.3/32         Liberal/1025                   DS/4.4.4.4  
 4.4.4.4/32         NULL/3         -               10.0.23.3       S2/0/0 
 4.4.4.4/32         1025/3         2.2.2.2         10.0.23.3       S2/0/0 
 4.4.4.4/32         1025/3         4.4.4.4         10.0.23.3       S2/0/0 
*4.4.4.4/32         Liberal/1025                   DS/2.2.2.2  
 ---------------------------------------------------------------------------- 
 TOTAL: 8 Normal LSP(s) Found. 
 TOTAL: 4 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state  
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP 
 
[R3]display mpls ldp lsp 
  LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface  
 ---------------------------------------------------------------------------- 
 2.2.2.2/32         NULL/1024      -               10.0.23.2       S2/0/0 
 2.2.2.2/32         1024/1024      3.3.3.3         10.0.23.2       S2/0/0 
 3.3.3.3/32         NULL/3         -               10.0.23.2       S2/0/0 
 3.3.3.3/32         1025/3         3.3.3.3         10.0.23.2       S2/0/0 
 4.4.4.4/32         3/NULL         3.3.3.3         127.0.0.1       InLoop0 
*4.4.4.4/32         Liberal/1025                   DS/3.3.3.3  
 ---------------------------------------------------------------------------- 
 TOTAL: 5 Normal LSP(s) Found. 
 TOTAL: 1 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page112 HUAWEI TECHNOLOGIES HC Series 
 
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP 
 
In most cases, the default trigger policy is used. The establishment of an 
LDP LSP is triggered in Host mode. 
Change the trigger policy to All on LSRs so that all static routes and IGP 
entries can trigger the establishment of the LDP LSPs.
[R1]mpls  
[R1-mpls]lsp-trigger all 
 
[R2]mpls 
[R2-mpls]lsp-trigger all 
 
[R3]mpls 
[R3-mpls]lsp-trigger all 
 
Run the display mpls ldp lsp  command. Information about the 
established LDP LSPs is displayed. 
[R1]display mpls ldp lsp 
  LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface  
 ---------------------------------------------------------------------------- 
 2.2.2.0/24         3/NULL         3.3.3.3         2.2.2.2         Loop0 
 2.2.2.2/32         3/NULL         3.3.3.3         127.0.0.1       InLoop0 
*2.2.2.2/32         Liberal/1024                   DS/3.3.3.3  
*3.3.3.0/24         Liberal/3                      DS/3.3.3.3  
 3.3.3.3/32         NULL/3         -               10.0.12.2       S1/0/0 
 3.3.3.3/32         1024/3         3.3.3.3         10.0.12.2       S1/0/0 
 4.4.4.4/32         NULL/1025      -               10.0.12.2       S1/0/0 
 4.4.4.4/32         1025/1025      3.3.3.3         10.0.12.2       S1/0/0 
 10.0.1.0/24        3/NULL         3.3.3.3         10.0.1.1        GE0/0/1 
*10.0.1.0/24        Liberal/1026                   DS/3.3.3.3  
 10.0.2.0/24        NULL/1027      -               10.0.12.2       S1/0/0 
 10.0.2.0/24        1027/1027      3.3.3.3         10.0.12.2       S1/0/0 
 10.0.12.0/24       3/NULL         3.3.3.3         10.0.12.1       S1/0/0 
*10.0.12.0/24       Liberal/3                      DS/3.3.3.3  
 10.0.23.0/24       NULL/3         -               10.0.12.2       S1/0/0 
 10.0.23.0/24       1026/3         3.3.3.3         10.0.12.2       S1/0/0 
 ---------------------------------------------------------------------------- 
 TOTAL: 12 Normal LSP(s) Found.

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page113 
 
 TOTAL: 4 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state  
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP 
 
[R2]display mpls ldp lsp 
  LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface  
 ---------------------------------------------------------------------------- 
*2.2.2.0/24         Liberal/3                      DS/2.2.2.2  
 2.2.2.2/32         NULL/3         -               10.0.12.1       S1/0/0 
 2.2.2.2/32         1024/3         2.2.2.2         10.0.12.1       S1/0/0 
 2.2.2.2/32         1024/3         4.4.4.4         10.0.12.1       S1/0/0 
*2.2.2.2/32         Liberal/1024                   DS/4.4.4.4  
 3.3.3.0/24         3/NULL         2.2.2.2         3.3.3.3         Loop0 
 3.3.3.0/24         3/NULL         4.4.4.4         3.3.3.3         Loop0 
 3.3.3.3/32         3/NULL         2.2.2.2         127.0.0.1       InLoop0 
 3.3.3.3/32         3/NULL         4.4.4.4         127.0.0.1       InLoop0 
*3.3.3.3/32         Liberal/1024                   DS/2.2.2.2  
*3.3.3.3/32         Liberal/1025                   DS/4.4.4.4  
*4.4.4.0/24         Liberal/3                      DS/4.4.4.4  
 4.4.4.4/32         NULL/3         -               10.0.23.3       S2/0/0 
 4.4.4.4/32         1025/3         2.2.2.2         10.0.23.3       S2/0/0 
 4.4.4.4/32         1025/3         4.4.4.4         10.0.23.3       S2/0/0 
*4.4.4.4/32         Liberal/1025                   DS/2.2.2.2  
 10.0.1.0/24        NULL/3         -               10.0.12.1       S1/0/0 
 10.0.1.0/24        1026/3         2.2.2.2         10.0.12.1       S1/0/0 
 10.0.1.0/24        1026/3         4.4.4.4         10.0.12.1       S1/0/0 
*10.0.1.0/24        Liberal/1026                   DS/4.4.4.4  
 10.0.2.0/24        NULL/3         -               10.0.23.3       S2/0/0 
 10.0.2.0/24        1027/3         2.2.2.2         10.0.23.3       S2/0/0 
 10.0.2.0/24        1027/3         4.4.4.4         10.0.23.3       S2/0/0 
*10.0.2.0/24        Liberal/1027                   DS/2.2.2.2  
 10.0.12.0/24       3/NULL         2.2.2.2         10.0.12.2       S1/0/0 
 10.0.12.0/24       3/NULL         4.4.4.4         10.0.12.2       S1/0/0 
*10.0.12.0/24       Liberal/3                      DS/2.2.2.2  
*10.0.12.0/24       Liberal/1027                   DS/4.4.4.4  
 10.0.23.0/24       3/NULL         2.2.2.2         10.0.23.2       S2/0/0

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page114 HUAWEI TECHNOLOGIES HC Series 
 
 10.0.23.0/24       3/NULL         4.4.4.4         10.0.23.2       S2/0/0 
*10.0.23.0/24       Liberal/1026                   DS/2.2.2.2  
*10.0.23.0/24       Liberal/3                      DS/4.4.4.4  
 ---------------------------------------------------------------------------- 
 TOTAL: 20 Normal LSP(s) Found. 
 TOTAL: 12 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state  
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP 
 
[R3]display mpls ldp lsp 
  LDP LSP Information   
 ---------------------------------------------------------------------------- 
 DestAddress/Mask   In/OutLabel    UpstreamPeer    NextHop         OutInterface  
 ---------------------------------------------------------------------------- 
 2.2.2.2/32         NULL/1024      -               10.0.23.2       S2/0/0 
 2.2.2.2/32         1024/1024      3.3.3.3         10.0.23.2       S2/0/0 
*3.3.3.0/24         Liberal/3                      DS/3.3.3.3  
 3.3.3.3/32         NULL/3         -               10.0.23.2       S2/0/0 
 3.3.3.3/32         1025/3         3.3.3.3         10.0.23.2       S2/0/0 
 4.4.4.0/24         3/NULL         3.3.3.3         4.4.4.4         Loop0 
 4.4.4.4/32         3/NULL         3.3.3.3         127.0.0.1       InLoop0 
*4.4.4.4/32         Liberal/1025                   DS/3.3.3.3  
 10.0.1.0/24        NULL/1026      -               10.0.23.2       S2/0/0 
 10.0.1.0/24        1026/1026      3.3.3.3         10.0.23.2       S2/0/0 
 10.0.2.0/24        3/NULL         3.3.3.3         10.0.2.1        GE0/0/2 
*10.0.2.0/24        Liberal/1027                   DS/3.3.3.3  
 10.0.12.0/24       NULL/3         -               10.0.23.2       S2/0/0 
 10.0.12.0/24       1027/3         3.3.3.3         10.0.23.2       S2/0/0 
 10.0.23.0/24       3/NULL         3.3.3.3         10.0.23.3       S2/0/0 
*10.0.23.0/24       Liberal/3                      DS/3.3.3.3  
 ---------------------------------------------------------------------------- 
 TOTAL: 12 Normal LSP(s) Found. 
 TOTAL: 4 Liberal LSP(s) Found. 
 TOTAL: 0 Frr LSP(s) Found. 
 A '*' before an LSP means the LSP is not established  
 A '*' before a Label means the USCB or DSCB is stale  
 A '*' before a UpstreamPeer means the session is in GR state  
 A '*' before a DS means the session is in GR state  
 A '*' before a NextHop means the LSP is FRR LSP

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page115 
 
 
Step 5 Configure an inbound LDP policy. 
If labels received on R1 are not controlled, R1 will establish a large number 
of LSPs, consuming large memory. 
After an inbound LDP policy is configured, R1 receives label mapping 
messages only from R2 and establishes LSPs to R2, saving resources. 
Run the display mpls lsp command on R1. Information about established 
LSPs is displayed.
[R1]display mpls lsp 
---------------------------------------------------------------------------- 
                 LSP Information: LDP LSP 
---------------------------------------------------------------------------- 
FEC                In/Out Label  In/Out IF                      Vrf Name        
3.3.3.3/32         NULL/3        -/S1/0/0                                       
3.3.3.3/32         1024/3        -/S1/0/0                                       
2.2.2.2/32         3/NULL        -/-                                            
4.4.4.4/32         NULL/1025     -/S1/0/0                                       
4.4.4.4/32         1025/1025     -/S1/0/0                                       
10.0.12.0/24       3/NULL        -/-                                            
10.0.1.0/24        3/NULL        -/-                                            
2.2.2.0/24         3/NULL        -/-                                            
10.0.23.0/24       NULL/3        -/S1/0/0                                       
10.0.23.0/24       1026/3        -/S1/0/0                                       
10.0.2.0/24        NULL/1027     -/S1/0/0                                       
10.0.2.0/24        1027/1027     -/S1/0/0    
 
LSPs on R1 to R2 and R3 are displayed. If the inbound policy is configured 
on R1, only routes to R2 are allowed. 
[R1]ip ip-prefix prefix1 permit 10.0.12.0 24 
[R1]mpls ldp 
[R1-mpls-ldp]inbound peer 3.3.3.3 fec ip-prefix prefix1 
[R1-mpls-ldp]quit 
[R1]display mpls lsp 
---------------------------------------------------------------------------- 
                 LSP Information: LDP LSP 
---------------------------------------------------------------------------- 
FEC                In/Out Label  In/Out IF                      Vrf Name

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page116 HUAWEI TECHNOLOGIES HC Series 
 
2.2.2.2/32         3/NULL        -/-                                            
10.0.12.0/24       3/NULL        -/-                                            
10.0.1.0/24        3/NULL        -/-                                            
2.2.2.0/24         3/NULL        -/-      
 
Additional Exercises: Analyzing and Verifying 
In which way can only R1 receive label mapping messages from R3? 
Final Configurations
<R1>display current-configuration
[V200R001C00SPC200] 
# 
 sysname R1 
# 
mpls lsr-id 2.2.2.2 
 mpls 
  lsp-trigger all  
# 
mpls ldp 
 inbound peer 3.3.3.3 fec ip-prefix prefix1 
# 
interface Serial1/0/0 
 link-protocol ppp 
 ip address 10.0.12.1 255.255.255.0  
 mpls 
 mpls ldp 
# 
interface GigabitEthernet0/0/1             
 ip address 10.0.1.1 255.255.255.0  
# 
interface LoopBack0 
 ip address 2.2.2.2 255.255.255.0  
# 
ospf 1 router-id 2.2.2.2  
 area 0.0.0.0  
  network 10.0.1.0 0.0.0.255  
  network 10.0.12.0 0.0.0.255  
  network 2.2.2.0 0.0.0.255  
#

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
HC Series HUAWEI TECHNOLOGIES     
Page117 
 
 ip ip-prefix prefix1 index 10 permit 10.0.12.0 24 
# 
return 
 
[R2]display current-configuration  
[V200R001C00SPC200] 
# 
 sysname R2 
# 
mpls lsr-id 3.3.3.3 
 mpls 
  lsp-trigger all  
# 
mpls ldp 
# 
interface Serial1/0/0 
 link-protocol ppp 
 ip address 10.0.12.2 255.255.255.0  
 mpls 
 mpls ldp 
# 
interface Serial2/0/0 
 link-protocol ppp 
 ip address 10.0.23.2 255.255.255.0  
 mpls 
 mpls ldp 
# 
interface LoopBack0 
 ip address 3.3.3.3 255.255.255.0  
# 
ospf 1 router-id 3.3.3.3  
 area 0.0.0.0  
  network 10.0.12.0 0.0.0.255  
  network 10.0.23.0 0.0.0.255  
  network 3.3.3.0 0.0.0.255  
# 
return 
 
[R3]display current-configuration  
[V200R001C00SPC200] 
# 
 sysname R3

HCDP-IESN  Chapter 3 Implementing MPLS technologies 
 
Page118 HUAWEI TECHNOLOGIES HC Series 
 
# 
mpls lsr-id 4.4.4.4 
 mpls 
  lsp-trigger all  
# 
mpls ldp 
# 
interface Serial2/0/0 
 link-protocol ppp 
 ip address 10.0.23.3 255.255.255.0  
 mpls 
 mpls ldp 
# 
interface GigabitEthernet0/0/2 
 ip address 10.0.2.1 255.255.255.0  
# 
interface LoopBack0 
 ip address 4.4.4.4 255.255.255.0  
# 
ospf 1 router-id 4.4.4.4  
 area 0.0.0.0  
  network 10.0.2.0 0.0.0.255  
  network 10.0.23.0 0.0.0.255  
  network 4.4.4.0 0.0.0.255  
# 
Return