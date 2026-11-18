Hi, everyone!
Today I’m going to introduce the interworking and replacement guide of spanning-tree protocols on Huawei switches and Cisco switches.
Huawei S series switches support the following spanning tree protocols: Spanning Tree Protocol (STP), Rapid Spanning Tree Protocol (RSTP), Multiple Spanning Tree Protocol (MSTP), and VLAN-Based Spanning Tree (VBST). VBST is supported by V200R005 and later versions.
STP is used on a LAN to prevent loops. Devices running STP discover loops on a network by exchanging information, and block some ports to eliminate loops. Both STP and RSTP (which is an evolution of STP and allows for fast network topology convergence) suffer from a significant limitation: neither can implement VLAN-based load balancing because all VLANs on a LAN use one spanning tree. When a link is blocked, it no longer transmits traffic, which wastes bandwidth and prevents certain VLAN packets from being forwarded. Based on STP and RSTP, MSTP allows fast convergence and provides multiple paths to load balance VLAN traffic. VBST, a Huawei spanning tree protocol, constructs a spanning tree in each VLAN so that traffic from different VLANs is forwarded through different spanning trees. Traffic is therefore load balanced.
Huawei MSTP and Cisco Multiple Spanning Tree (MST) are based on IEEE standards, but their implementations are different. VBST is a Huawei proprietary protocol. The following describes working mechanisms of Huawei MSTP and VBST.
MSTP is a new spanning tree protocol defined in IEEE 802.1s. MSTP uses Multiple Spanning Tree Instances (MSTIs) and Multiple Spanning Tree (MST) regions. An MSTI is a collection of VLANs. Binding multiple VLANs to a single MSTI reduces communication costs and resource usage. The topology of each MSTI is calculated independently, and traffic can be balanced among MSTIs. Multiple VLANs with the same topology can be mapped to a single MSTI. The forwarding state of the VLANs for a port is determined by the port state in the MSTI.
MST region: consists of the configuration name, revision level, configuration identifier format selector, and mapping between VLANs and MSTIs. The configuration name, configuration identifier format selector, and revision level have corresponding fields in a Bridge Protocol Data Unit (BPDU). The mapping between VLANs and MSTIs is the configuration digest in the BPDU, which is a 16-byte signature calculated depending on the mapping. All switches in an MST region must have the same MST region configuration. By default, the configuration name is the first MAC address of a switch, the revision level is 0, the configuration identifier format selector is 0, and all VLANs are mapped to MSTI 0.
MSTI: Each MSTI corresponds to one VLAN or a group of VLANs, whereas each VLAN corresponds only to one MSTI. Each switch can run multiple MSTIs. When the mapping between VLANs and MSTIs is not configured, all VLANs are mapped to MSTI 0.
CIST: The Common Spanning Tree (CST) and the Internal Spanning Tree (IST) construct a CIST. The IST provides connectivity for an MST region. The IST is a segment of the CIST in an MST region and is a special MSTI with an MSTI ID of 0. The CST is a segment of the CIST (each MST region is considered as a single node).
CIST root and regional root: Compared with STP and RSTP, MSTP uses the CIST root and regional root. For all connected switches running STP/RSTP/MSTP, there is only one CIST root. Each MSTI in each MST region has a regional root. There is only one CIST root on the switching network, whereas the number of regional roots in each region depends on the number of MSTIs.
External path cost and internal path cost: Compared with STP and RSTP, MSTP uses the external path cost and internal path cost. The external path cost corresponds to the CIST and is consistent in an MST region. Each MSTI in each MST region has an internal path cost. Different internal path costs correspond to different MSTIs on the same port.
Edge port, master port, and alternate port: Compared with STP and RSTP, MSTP uses the edge port and master port. The regional edge port connects ports in different MST regions, MST regions and regions running STP, or MST regions and regions running RSTP, and is located at the edge of an MST region. Among all edge ports in an MST region, the master port has the smallest cost to the CIST root. The master port is located on the shortest path connecting MST regions to the CIST root. The alternate port is the backup port of the master port. If the master port is blocked, the alternate port becomes the new master port.
Figure 1-1 shows the format of an MST BPDU.
Figure 1-1 Format of an MST BPDU
VBST, a Huawei spanning tree protocol, constructs a spanning tree in each VLAN so that traffic from different VLANs is forwarded through different spanning trees. VBST is equivalent to STP or RSTP running in each VLAN. Spanning trees in different VLANs are independent of each other.
VBST transmits VBST BPDUs in VLANs but not VLAN 1 to determine the network topology. VBST BPDUs are protocol packets of VBST based on STP or RSTP BPDUs. Compared with STP or RSTP BPDUs, a 4-byte 802.1Q tag is added between the source MAC address field and the protocol length field of a VBST BPDU. Figure 1-2 compares encapsulation formats between an STP or RSTP BPDU and a VBST BPDU.
Figure 1-2 Comparisons between encapsulation formats of an STP or RSTP BPDU and a VBST BPDU
Table 1-1 compares VBST, STP, RSTP, and MSTP of Huawei S series switches in convergence speed, traffic forwarding, applicable scenario, and configuration complexity.
Table 1-1 Comparison among four spanning tree protocols
| Spanning Tree Protocol | Similarity | Difference |  |  |  | 
|  |  | Convergence Speed | Traffic Forwarding | Applicable Scenario | Configuration Complexity | 
| STP | Forms a loop-free tree to prevent broadcast storms and implements redundancy. | Slowest | All VLANs share one spanning tree, and traffic from all VLANs is transmitted along the same path. | Service traffic does not need to be differentiated. | Low | 
| RSTP |  | RSTP, MSTP, and VBST provide the same topology convergence speed and offer faster topology convergence than STP. |  |  | Low | 
| MSTP |  |  | MSTP provides mappings between MSTIs and VLANs so that traffic from different VLANs is forwarded through different spanning trees that are independent of each other. | Service traffic needs to be differentiated and load balanced. | Medium | 
| VBST |  |  | A spanning tree is formed in each VLAN, so that traffic from different VLANs is forwarded through different spanning trees that are independent of each other. | l Service traffic needs to be differentiated and load balanced. l VBST can interwork with the Per-VLAN Spanning Tree (PVST), Per-VLAN Spanning Tree Plus (PVST+), and Rapid PVST+. | High | 
Cisco switches support the following spanning tree protocols: Per VLAN Spanning Tree (PVST), Per VLAN Spanning Tree Plus (PVST+), Rapid PVST+, and Multiple Spanning Tree (MST).
Cisco Catalyst series switches of IOS 12.2 and later versions support PVST, PVST+, Rapid PVST+, and MST. Some BPDUs of these spanning tree protocols use Cisco proprietary BPDU formats, which are different from the BPDU format defined by the IEEE. Trunk interfaces on Cisco switches enabled with PVST+ or Rapid PVST+ send Cisco proprietary BPDUs in VLANs but not VLAN 1. The source MAC address of these BPDUs is the MAC address of the trunk interface, and the destination MAC address is Cisco's reserved MAC address 01-00-0C-CC-CC-CD.
PVST can be considered as STP running in each VLAN. Each VLAN has an independent STP status and a spanning tree calculated. Although PVST does not define the instances used in MSTP, PVST can load balance traffic from different VLANs. PVST BPDUs carry VLAN information in data frames. The destination MAC address of PVST BPDUs is 01-00-0C-CC-CC-CD; therefore, PVST cannot interwork with standard IEEE spanning tree protocols.
To interwork with standard IEEE spanning tree protocols, Cisco develops PVST+ based on PVST. PVST+ provides interoperability with standard spanning tree protocols, which is an improvement made to PVST.
On an access interface, PVST+ sends standard STP BPDUs in its native VLAN. On a trunk interface, PVST+ sends standard STP BPDUs with the destination MAC address of 01-80-C2-00-00-00 only in VLAN 1, and sends Cisco proprietary BPDUs with the destination MAC address of 01-00-0C-CC-CC-CD in other VLANs allowed by the trunk interface.
Huawei S series switches support standard IEEE spanning tree protocols, and can process standard STP BPDUs from Cisco switches. However, Huawei S series switches forward Cisco proprietary BPDUs as multicast packets but not process them.
Rapid PVST+ is an extension of PVST+. Compared with PVST+, Rapid PVST+ uses the Rapid Spanning Tree Protocol (RSTP) mechanism to implement rapid transition.
Cisco MST supports VLAN-instance mapping and defines the region; therefore, it can be considered as a standard MSTP protocol. MST BPDUs use the standard format defined by the IEEE. Huawei and Cisco switches use different keys to generate MSTP digests in BPDUs, so the digests in BPDUs are different. By default, MSTP and Cisco MST can implement only inter-region interworking because Huawei and Cisco switches generate different digests. To enable MSTP and Cisco MST to interwork within an MST region, enable digest snooping on a Huawei S series switch connected to a Cisco switch and the Huawei S series switch's interface connected to the Cisco switch.
Processing mode of Cisco PVST+ BPDUs (the implementation is similar to that of Rapid PVST+ BPDUs)
On a trunk interface:
− In VLAN 1, a PVST+ device sends standard STP BPDUs and untagged PVST BPDUs to negotiate with the remote device.
− In the native VLAN but not VLAN 1, a PVST+ device sends untagged PVST BPDUs to negotiate with the remote device.
− In other VLANs, a PVST+ device sends PVST BPDUs to negotiate with the remote device.
− A PVST+ device sends standard STP BPDUs to negotiate with the remote device in VLAN 1 after the no spanning-tree vlan 1 command is configured globally.
On an access interface:
In all VLANs, a PVST+ device sends standard STP BPDUs to negotiate with the remote device.
By default, VLAN 1 is the native VLAN on a Cisco switch.
Processing of Huawei VBST BPDUs
− In VLAN 1, a VBST-enabled device sends standard STP or RSTP BPDUs and VBST BPDUs to negotiate with the remote device.
− In other VLANs, a VBST-enabled device sends VBST BPDUs to negotiate with the remote device.
l On an access interface:
A VBST-enabled device sends standard STP or RSTP BPDUs to negotiate with the remote device only in the VLAN where the access interface is located.
The Data field of VBST BPDUs and selection of packets of a standard protocol depend on the remote device connected to the Huawei S series switch. By default, standard RSTP BPDUs are used.
Table 1-2 Differences in command formats
| Function | Command on Huawei S Series Switches | Command on Cisco Switches | 
| Configure a spanning tree mode. | stp mode | spanning-tree mode | 
| Configure a path cost algorithm. | stp pathcost-standard | spanning-tree pathcost method | 
| Configure a fast transition mode on an interface. | stp no-agreement-check | - NOTE No such command is available on Cisco switches. Cisco switches support different convergence modes depending on the product model. For details, see Cisco product manuals. | 
| Enable digest snooping. | stp config-digest-snoop | - NOTE No such command is available on Cisco switches. Cisco switches do not support digest snooping. | 
Table 1-3 Differences between path cost algorithms
| Path Cost Algorithm | Command on Huawei S Series Switches |  | Command on Cisco Switches |  | 
|  | Query Command | Configuration Command | Query Command | Configuration Command | 
| IEEE 802.1t | display stp | stp pathcost-standard dot1t | show spanning-tree detail | spanning-tree pathcost method long | 
| IEEE 802.1d-1998 |  | stp pathcost-standard dot1d-1998 |  | spanning-tree pathcost method short | 
Before the 802.1s standard (MSTP) is released, vendors use different formats of digest fields in MSTP BPDUs. When devices from different vendors interwork with each other, negotiation may fail.
When a Huawei S series switch is connected to a Cisco switch, the two devices may fail to communicate because of different keys in BPDUs even though they have the same domain name, revision level, and VLAN mapping table. To solve this problem, enable digest snooping on the interface of the Huawei S series switch connected to the remote device. This function enables the Huawei switch to use the same key as the remote device, so that the Huawei S series switch can negotiate with the remote device.
Table 1-4 Comparison between digest commands
| Function | Command on Huawei S Series Switches | Command on Cisco Switches | 
| Check the digest information. | display stp region-configuration digest | show spanning-tree mst digest | 
| Enable digest snooping. | stp config-digest-snoop | - NOTE No such command is available on Cisco switches. Cisco switches do not support digest snooping. | 
There are three interworking and replacement solutions.
Huawei S series switches transparently transmit PVST BPDUs, and Cisco switches remove loops through negotiation.
Huawei S series switches running VBST interwork with Cisco switches running PVST, PVST+, or Rapid PVST+.
Huawei S series switches running MSTP interwork with Cisco switches running MST.
All models and versions of Huawei S series switches support MSTP, whereas VBST is supported in V200R005 and later versions.
A Huawei S series switch running VBST interworks with a Cisco switch running PVST, PVST+, or Rapid PVST+. They process protocol packets using the same mechanism, identify packets of each other, and use the same multicast MAC address 01-00-0C-CC-CC-CD. That is, their communication is similar to the communication between Huawei S series switches running VBST.
When a Huawei S series switch enabled with MSTP interworks with a Cisco switch running MST, digest snooping needs to be enabled on the Huawei S series switch because their digest formats are different. Their other implementations are the same.
A Cisco switch running PVST+ or Rapid PVST+ sends both PVST BPDUs and STP or RSTP BPDUs to negotiate with the remote device, so a Huawei S series switch running STP or RSTP can interwork with the Cisco switch. STP or RSTP convergence on the Huawei S series switch is based on ports, whereas PVST+ or Rapid PVST+ convergence on the Cisco switch is based on VLANs. The convergence results are as follows:
When a blocked port is located on the Huawei S series switch, data packets of all VLANs including PVST BPDUs of the Cisco switch are discarded on the blocked port. Therefore, the port is blocked in any VLAN.
When a blocked port is located on the Cisco switch, the Cisco switch running PVST+ or Rapid PVST+ only sends standard STP or RSTP BPDUs to negotiate with the remote device in VLAN 1. In this case, the blocked port only blocks packets from VLAN 1. It normally processes and forwards PVST BPDUs of other VLANs, and calculates the spanning tree of the VLAN where the port is located. The Huawei S series switch running STP or RSTP does not process PVST BPDUs, so blocked ports in other VLANs must be located on the Cisco switch.
Huawei S series switches transparently transmit Cisco PVST BPDUs to remove loops between Cisco switches or on themselves.
In Figure 1-3, all switches are Cisco switches. Layer 3 switches establish a virtual switching system (VSS) to implement connectivity. Two aggregation switches establish a port channel in manual mode to provide link redundancy. They are configured with OSPF and establish OSPF relationships with core switches to receive and transmit routes, and are configured with the Hot Standby Router Protocol (HSRP) to implement virtual gateway backup. Switches are enabled with PVST to remove loops.
Huawei S series switches need to replace the two aggregation switches, without changing the network plan.
Figure 1-3 Networking where Huawei S series switches transparently transmit Cisco PVST BPDUs
The configuration roadmap is as follows:
1. Configure OSPF on Huawei S series switches to establish OSPF relationships with core switches to receive and transmit routes.
2. Configure link aggregation in manual mode on Huawei S series switches to implement load balancing.
3. Configure VRRP on Huawei S series switches to interwork with and replace HSRP on Cisco switches. VRRP implements virtual gateway backup.
4. Configure Huawei S series switches to transparently transmit Cisco PVST BPDUs to remove loops between Cisco switches or on themselves.
a. Disable STP on Huawei S series switches.
b. Configure Huawei S series switches to transparently transmit Cisco PVST BPDUs.
5. Configure service forwarding on Huawei S series switches based on the original network plan.
During migration, connect Huawei switches in bypass mode and establish OSPF routes. Migrate services on access switches to Huawei S series switches one by one.
1. Check the configuration of Cisco switches before the replacement.
a. Run the show running-config command to check the spanning tree configuration on Cisco switches.
b. Run the show spanning-tree summary command to check spanning tree parameters and status information on Cisco switches.
Cisco switches use PVST to calculate spanning trees.
2. Power on two Huawei S series switches, and connect links between them and their uplinks. Configure addresses for downlink interfaces of core switches, and configure addresses for uplink interfaces and loopback addresses on Huawei S series switches. Complete the configuration on Huawei S series switches, and shut down VLANIF 10 and VLANIF 20 on HuaweiA and HuaweiB. Retain the configuration of Cisco switches.
a. Disable STP on HuaweiA and HuaweiB.
# Configure HuaweiA.
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiA 
[HuaweiA] stp disable 
# Configure HuaweiB.
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiB 
[HuaweiB] stp disable 
[HuaweiA] interface eth-trunk 1 
[HuaweiA-Eth-Trunk1] l2protocol-tunnel PVST+ enable 
[HuaweiA-Eth-Trunk1] quit 
[HuaweiA] interface gigabitethernet 0/0/1 
[HuaweiA-GigabitEthernet0/0/1] l2protocol-tunnel PVST+ enable 
[HuaweiA-GigabitEthernet0/0/1] quit 
[HuaweiA] interface gigabitethernet 0/0/2 
[HuaweiA-GigabitEthernet0/0/2] l2protocol-tunnel PVST+ enable 
[HuaweiA-GigabitEthernet0/0/2] quit 
[HuaweiB] interface eth-trunk 1 
[HuaweiB-Eth-Trunk1] l2protocol-tunnel PVST+ enable 
[HuaweiB-Eth-Trunk1] quit 
[HuaweiB] interface gigabitethernet 0/0/1 
[HuaweiB-GigabitEthernet0/0/1] l2protocol-tunnel PVST+ enable 
[HuaweiB-GigabitEthernet0/0/1] quit 
[HuaweiB] interface gigabitethernet 0/0/2 
[HuaweiB-GigabitEthernet0/0/2] l2protocol-tunnel PVST+ enable 
[HuaweiB-GigabitEthernet0/0/2] quit 
3. Migrate services of the backup uplink of CiscoD to HuaweiB and shut down VLANIF 20, as shown in Figure 1-4.
Figure 1-4 Migration process 1
4. Disconnect the cable between CiscoA and CiscoD, shut down VLANIF 20 on CiscoA and CiscoB, and enable VLANIF 20 on Huawei S series switches.
5. Test services on CiscoD. When verifying that services on CiscoD are normal, migrate services on the link between CiscoD and CiscoA to Huawei A. The migration of the access switch is completed, as shown in Figure 1-5.
Figure 1-5 Migration process 2
6. Perform the preceding steps to migrate services on downstream access switches one by one. Figure 1-6 shows the network where migration is completed.
Figure 1-6 Network where migration is completed
7. Check the configuration of Huawei S series switches after the replacement.
a. Run the display l2protocol-tunnel group-mac { all | protocol-type | user-defined-protocol protocol-name } command to check whether Huawei S series switches can transparently transmit Cisco PVST BPDUs.
b. Run the show spanning-tree summary command to check spanning tree status information on Cisco switches.
c. Verify services on user-side devices and check whether the replacement is successful.
Huawei S series switches transparently transmit Cisco PVST BPDUs to implement spanning tree negotiation between Cisco switches, so Huawei S series switches broadcast received PVST BPDUs in VLANs. As a result, P2P negotiation between two switches is changed to P2MP negotiation, affecting spanning tree convergence. Solution 1 causes slow spanning tree convergence and easily results in temporary loops.
Huawei S series switches are configured with VBST to interwork with Cisco PVST switches to remove loops.
In Figure 1-7, all switches are Cisco switches. Layer 3 switches establish a virtual switching system (VSS) to implement connectivity. Two aggregation switches establish a port channel in manual mode to provide link redundancy. They are configured with OSPF and establish OSPF relationships with core switches to receive and transmit routes, and are configured with the Hot Standby Router Protocol (HSRP) to implement virtual gateway backup. Switches are enabled with Rapid PVST+ to remove loops.
Cisco switches use the short algorithm to calculate the path cost. The short algorithm corresponds to the dot1d-1998 algorithm on Huawei S series switches. Cisco switches do not support fast transition in enhanced mode, whereas Huawei S series switches use fast transition in enhanced mode by default. You must run the stp no-agreement-check command to configure fast transition in common mode on the interfaces that do not support fast transition in enhanced mode.
Figure 1-7 Networking where Huawei S series switches use VBST to interwork with Cisco PVST switches
4. Configure VBST on Huawei S series switches to interwork with Rapid PVST+ on Cisco switches.
a. Configure Huawei S series switches to work in VBST mode.
b. Configure Huawei S series switches to use the dot1d-1998 algorithm to calculate the path cost.
c. Configure fast transition in common mode on Huawei S series switches.
Cisco switches use Rapid PVST+ to calculate spanning trees and use the short algorithm to calculate the path cost. In addition, they do not support fast transition in enhanced mode.
Configure VBST on Huawei S series switches.
a. Configure HuaweiA and HuaweiB to work in VBST mode.
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiA 
[HuaweiA] stp mode vbst 
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiB 
[HuaweiB] stp mode vbst 
[HuaweiA] stp pathcost-standard dot1d-1998 
[HuaweiB] stp pathcost-standard dot1d-1998 
[HuaweiA] interface gigabitethernet 0/0/1 
[HuaweiA-GigabitEthernet0/0/1] stp no-agreement-check 
[HuaweiA-GigabitEthernet0/0/1] quit 
[HuaweiA] interface gigabitethernet 0/0/2 
[HuaweiA-GigabitEthernet0/0/2] stp no-agreement-check 
[HuaweiA-GigabitEthernet0/0/2] quit 
[HuaweiB] interface gigabitethernet 0/0/1 
[HuaweiB-GigabitEthernet0/0/1] stp no-agreement-check 
[HuaweiB-GigabitEthernet0/0/1] quit 
[HuaweiB] interface gigabitethernet 0/0/2 
[HuaweiB-GigabitEthernet0/0/2] stp no-agreement-check 
[HuaweiB-GigabitEthernet0/0/2] quit 
3. Migrate services of the backup uplink of CiscoD to HuaweiB and shut down VLANIF 20, as shown in Figure 1-8.
Figure 1-8 Migration process 1
5. Test services on CiscoD. When verifying that services on CiscoD are normal, migrate services on the link between CiscoD and CiscoA to Huawei A. The migration of the access switch is completed, as shown in Figure 1-9.
Figure 1-9 Migration process 2
6. Perform the preceding steps to migrate services on downstream access switches one by one. Figure 1-10 shows the network where migration is completed.
Figure 1-10 Network where migration is completed
a. Run the display stp [ vlan vlan-id ] [ interface interface-type interface-number | slot slot-id ] [ brief ] command to check the spanning tree status and statistics on Huawei S series switches.
PVST on Cisco switches is changed to MST so that Cisco switches can interwork with Huawei S series switches running MSTP.
In Figure 1-11, all switches are Cisco switches. Layer 3 switches establish a virtual switching system (VSS) to implement connectivity. Two aggregation switches establish a port channel in manual mode to provide link redundancy. They are configured with OSPF and establish OSPF relationships with core switches to receive and transmit routes, and are configured with the Hot Standby Router Protocol (HSRP) to implement virtual gateway backup. Switches are enabled with PVST to remove loops.
Cisco switches use the short algorithm to calculate the path cost. The short algorithm corresponds to the dot1d-1998 algorithm on Huawei S series switches. Cisco switches do not support fast transition in enhanced mode, whereas Huawei S series switches use fast transition in enhanced mode by default. You must run the stp no-agreement-check command to configure fast transition in common mode on the interfaces that do not support fast transition in enhanced mode. The format of the digest on a Cisco MST switch is different from that defined by the IEEE. CiscoA is the root bridge and CiscoB is the secondary root bridge in VLAN 10, and GE0/2 on CiscoC is the blocked port. CiscoB is the root bridge and CiscoA is the secondary root bridge in VLAN 20, and GE0/1 on CiscoD is the blocked port.
Figure 1-11 Networking where Cisco switches use MST to replace PVST to interwork with Huawei S series switches running MSTP
4. Configure MST on Cisco switches. Configure MSTP on Huawei S series switches and configure the path cost calculation algorithm and fast transmission mode so that Huawei S series switches can interwork with Cisco MST switches.
a. Configure MST on Cisco switches based on the original network plan.
i. Configure an MST region, create multiple MSTIs, and map VLAN 10 to MSTI 1 and VLAN 20 to MSTI 2.
ii. Configure the root bridge and secondary root bridge of each MSTI in each MST region.
iii. Configure the path cost of a port in each instance so that the port can be blocked.
iv. Enable MST.
b. Configure MSTP on Huawei S series switches based on the original network plan.
iii. Configure the path cost calculation algorithm on Huawei S series switches to be consistent with that on Cisco switches.
iv. Configure digest snooping on interfaces of Huawei S series switches connected to Cisco access switches.
v. Enable MSTP.
2. Power on two Huawei S series switches, and connect links between them and their uplinks. Configure addresses for downlink interfaces of core switches, and configure addresses for uplink interfaces and loopback addresses on Huawei S series switches. Change the spanning tree protocol to MST on Cisco switches and set parameters based on the original network plan. Complete the configuration on Huawei S series switches, and shut down VLANIF 10 and VLANIF 20 on Huawei A and Huawei B.
a. Configure MST on CiscoA and CiscoB to be replaced.
# Configure CiscoA.
CiscoA# configure terminal 
CiscoA(config)# spanning-tree mst configuration 
CiscoA(config)# spanning-tree extend system-id 
CiscoA(config-mst)# instance 1 vlan 10 
CiscoA(config-mst)# instance 2 vlan 20 
CiscoA(config-mst)# spanning-tree mst 1 priority 0 
CiscoA(config-mst)# spanning-tree mst 2 priority 24576 
CiscoA(config-mst)# name BG1 
CiscoA(config-mst)# revision 0 
CiscoA(config-mst)# exit 
CiscoA(config)# spanning-tree mode mst 
CiscoA(config)# end 
# Configure CiscoB.
CiscoB# configure terminal 
CiscoB(config)# spanning-tree mst configuration 
CiscoB(config)# spanning-tree extend system-id 
CiscoB(config-mst)# instance 1 vlan 10 
CiscoB(config-mst)# instance 2 vlan 20 
CiscoB(config-mst)# spanning-tree mst 1 priority 24576 
CiscoB(config-mst)# spanning-tree mst 2 priority 0 
CiscoB(config-mst)# name BG1 
CiscoB(config-mst)# revision 0 
CiscoB(config-mst)# exit 
CiscoB(config)# spanning-tree mode mst 
CiscoB(config)# end 
b. Configure MST on CiscoC and CiscoD (access switches).
# Configure CiscoC.
CiscoC# configure terminal 
CiscoC(config)# spanning-tree mst configuration 
CiscoC(config)# spanning-tree extend system-id 
CiscoC(config-mst)# instance 1 vlan 10 
CiscoC(config-mst)# instance 2 vlan 20 
CiscoC(config-mst)# name BG1 
CiscoC(config-mst)# revision 0 
CiscoC(config-mst)# exit 
CiscoC(config)# spanning-tree mode mst 
CiscoC(config)# interface gigabitethernet 0/2 
CiscoC(config-if)# spanning-tree mst 1 cost 20000 
CiscoC(config-if)# exit 
CiscoC(config)# end 
# Configure CiscoD.
CiscoC# configure terminal 
CiscoC(config)# spanning-tree mst configuration 
CiscoC(config)# spanning-tree extend system-id 
CiscoC(config-mst)# instance 1 vlan 10 
CiscoC(config-mst)# instance 2 vlan 20 
CiscoC(config-mst)# name BG1 
CiscoC(config-mst)# revision 0 
CiscoC(config-mst)# exit 
CiscoC(config)# spanning-tree mode mst 
CiscoC(config)# interface gigabitethernet 0/1 
CiscoC(config-if)# spanning-tree mst 2 cost 20000 
CiscoC(config-if)# exit 
CiscoC(config)# end 
c. Configure MSTP on HuaweiA and HuaweiB.
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiA 
[HuaweiA] stp region-configuration 
[HuaweiA-mst-region] region-name RG1 
[HuaweiA-mst-region] instance 1 vlan 10 
[HuaweiA-mst-region] instance 2 vlan 20 
[HuaweiA-mst-region] active region-configuration 
[HuaweiA-mst-region] quit 
[HuaweiA] stp pathcost-standard dot1d-1998 
[HuaweiA] stp instance 1 root primary 
[HuaweiA] stp instance 2 root secondary 
[HuaweiA] interface gigabitethernet 0/0/1 
[HuaweiA-GigabitEthernet0/0/1] stp no-agreement-check 
[HuaweiA-GigabitEthernet0/0/1] stp config-digest-snoop 
[HuaweiA-GigabitEthernet0/0/1] quit 
[HuaweiA] interface gigabitethernet 0/0/2 
[HuaweiA-GigabitEthernet0/0/2] stp no-agreement-check 
[HuaweiA-GigabitEthernet0/0/2] stp config-digest-snoop 
[HuaweiA-GigabitEthernet0/0/2] quit 
<HUAWEI> system-view 
[HUAWEI] syaname HuaweiB 
[HuaweiB] stp region-configuration 
[HuaweiB-mst-region] region-name RG1 
[HuaweiB-mst-region] instance 1 vlan 10 
[HuaweiB-mst-region] instance 2 vlan 20 
[HuaweiB-mst-region] active region-configuration 
[HuaweiB-mst-region] quit 
[HuaweiB] stp pathcost-standard dot1d-1998 
[HuaweiB] stp instance 1 root secondary 
[HuaweiB] stp instance 2 root primary 
[HuaweiB] interface gigabitethernet 0/0/1 
[HuaweiB-GigabitEthernet0/0/1] stp no-agreement-check 
[HuaweiB-GigabitEthernet0/0/1] stp config-digest-snoop 
[HuaweiB-GigabitEthernet0/0/1] quit 
[HuaweiB] interface gigabitethernet 0/0/2 
[HuaweiB-GigabitEthernet0/0/2] stp no-agreement-check 
[HuaweiB-GigabitEthernet0/0/2] stp config-digest-snoop 
[HuaweiB-GigabitEthernet0/0/2] quit 
3. Migrate services of the backup uplink of CiscoD to HuaweiB and shut down VLANIF 20, as shown in Figure 1-12.
Figure 1-12 Migration process 1
5. Test services on CiscoD. When verifying that services on CiscoD are normal, migrate services on the link between CiscoD and CiscoA to HuaweiA. The migration of the access switch is completed, as shown in Figure 1-13.
Figure 1-13 Migration process 2
6. Perform the preceding steps to migrate services on downstream access switches one by one. Figure 1-14 shows the network where migration is completed.
Figure 1-14 Network where migration is completed
If you have any problems, please post them in our Community. We are happy to solve them for you!