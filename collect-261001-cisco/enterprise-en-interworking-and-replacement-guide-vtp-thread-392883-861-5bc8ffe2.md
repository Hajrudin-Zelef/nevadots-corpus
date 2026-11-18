Hi there, Community!
This post is about an Interworking and Replacement Guide VTP. Please see more details about the topic below.
The VLAN Central Management Protocol (VCMP), a Layer 2 protocol in the Open System Interconnection (OSI) model, transmits VLAN information and ensures consistent VLAN information on the Layer 2 network. VCMP is a Huawei proprietary protocol. Huawei S series switches support VCMP starting from V200R005.
Purpose
In most cases, switches on an enterprise network need to synchronize VLAN information with each other to ensure that they can correctly forward data. On a small-scale enterprise network, the network administrator can log in to each switch to configure and maintain VLANs. On a large-scale enterprise network, a lot of switches are deployed, so a large amount of VLAN information needs to be configured and maintained. If the network administrator manually configures and maintains all VLANs, the workload is heavy and VLAN information may be inconsistent.
To address the preceding issue, VCMP is used to implement centralized VLAN management. The network administrator needs to create and delete VLAN information only on one switch. The changes on the switch are automatically synchronized to other switches in a specified scope so that no manual operation is required on these switches. In this way, the configuration workload is reduced and VLAN information consistency is ensured.
VCMP can only help the network administrator synchronize VLAN information but not dynamically assign VLANs. VCMP is often used with Link-type Negotiation Protocol (LNP) to simplify user configurations.
VCMP that is configured on a switch of a Layer 2 network brings in the following benefits:
l Implements centralized VLAN management and maintenance, and reduces the network maintenance workload.
l Implements the plug-and-play function of access switches.
Basic Concepts
VCMP involves two important entities:
l VCMP domain
A VCMP domain is composed of switches that have the same VCMP domain name and are connected through trunk or hybrid interfaces. All switches in the VCMP domain must use the same domain name, and each switch can join only one VCMP domain. Switches in different VCMP domains cannot synchronize VLAN information.
A VCMP domain specifies the scope for the administrative switch and managed switches. Switches in a VCMP domain are managed by the administrative switch. There is only one administrative switch and multiple managed switches in a VCMP domain.
l VCMP roles
VCMP determines attributes of switches based on VCMP roles. Table 1-1 describes VCMP roles.
Table 1-1 VCMP roles
| VCMP Role | Description | 
| Server | The VCMP server synchronizes VLAN information to other switches in the local VCMP domain. The VLAN information that is created and deleted on the VCMP server is broadcast in a VCMP domain. | 
| Client | A VCMP client belongs to a specified VCMP domain and synchronizes VLAN information with the VCMP server. The VLAN information that is created and deleted on a VCMP client is not broadcast in a VCMP domain, but is overwritten by VLAN information sent by the VCMP server. Huawei S series switches are VCMP clients by default. | 
| Transparent | A VCMP transparent switch is not affected by VCMP management behaviors, and does not affect other switches in the local VCMP domain. The VCMP transparent switch transparently forwards VCMP packets to only trunk or hybrid links. The VLAN information that is created and deleted on a VCMP transparent switch is not affected by the VCMP server and is not broadcast in a VCMP domain. In this way, some switches that do not need to be managed by VCMP can forward VCMP packets. | 
| Silent | Deployed at the edge of a VCMP domain, a VCMP silent switch does not affect other switches in the local VCMP domain and is not affected by VCMP management behaviors. The VCMP silent switch prevents VCMP packets in a VCMP domain from being transmitted to other VCMP domains. A VCMP silent switch directly discards received VCMP packets but not forward them. The VLAN information that is created and deleted on a VCMP silent switch is not affected by the VCMP server and is not broadcast in a VCMP domain. | 
VCMP Packets
VCMP enables switches of different roles to exchange VCMP packets to implement centralized VLAN management. VCMP packets can be only transmitted in VLAN 1 on trunk or hybrid interfaces. To retain the same VLAN information on the VCMP server and clients, VCMP defines two types of multicast packets: Summary-Advert and Advert-Request. Table 1-2 describes the functions and applicable scenarios of the two types of packets.
Table 1-2 VCMP packets
| Packet Type | Function | Applicable Scenario | 
| Summary-Advert | The VCMP server sends Summary-Advert packets to other devices in the local VCMP domain to notify them of the domain name, device ID, configuration revision number, and VLAN information. | l The VCMP server sends a Summary-Advert packet every 5 minutes to ensure real-time synchronization of VLAN information on the VCMP server and clients and to prevent VLAN information loss due to packet loss. l The VCMP server configuration is changed. For example, VLANs are created or deleted, the VCMP domain name or device ID is changed, and the VCMP server restarts. l The VCMP server receives Advert-Request packets from VCMP clients in the same VCMP domain. | 
| Advert-Request | A VCMP client sends Advert-Request packets to the VCMP server to request VLAN information. | l A VCMP client is added. l A VCMP client restarts or a client interface becomes Up. l A VCMP client changed from a VCMP server, silent, or transparent sends Advert-Request packets to the VCMP server. If the VCMP server does not respond, the VCMP client sends 10 Advert-Request packets every 5 minutes. | 
l Figure 1-1 shows the format of a Summary-Advert packet.
− Code: indicates a Summary-Advert packet when the value is 0x01.
− Followers: is not used currently, and is set to 0.
− Updater Identity: indicates the VCMP server ID.
− Configuration Revision Number: determines whether VLAN information sent from the VCMP server is newer than the local VLAN information. The VCMP client determines whether to synchronize VLAN information with the VCMP server based on this field. The value is an 8-digit hexadecimal number. The four left-most bits indicate the change of the VCMP domain or device ID and the four right-most bits indicate the VLAN change. Upon a VLAN change on the VCMP server, the configuration revision number is automatically increased. When the VCMP domain name or device ID changes, the four left-most bits of the configuration revision number are recalculated and the four right-most bits are reset.
− Digest: indicates the packet digest. When a VCMP domain is configured with a password, the switch uses the password and bits 25 to 62 of packets to calculate the digest. When no password is configured, the switch uses bits 25 to 68 of packets to calculate the digest.
− VLAN bitmap: indicates the VLAN information on the VCMP server.
Figure 1-1 Format of a Summary-Advert packet
l Figure 1-2 shows the format of an Advert-Request packet.
− Code: indicates an Advert-Request packet when the value is 0x02.
− Reserved: indicates the reserved field that has a fixed value of 0.
Figure 1-2 Format of an Advert-Request packet
The VLAN Trunking protocol (VTP), a layer 2 protocol in the Open System Interconnection (OSI) model, manages VLAN creation, deletion, and renaming in a domain. VTP is a Cisco proprietary protocol.
VTP involves two important entities:
l VTP domain
A VTP domain is composed of switches that have the same VTP domain name and are connected through trunk links.
Switches in a VTP domain share VLAN information, and each switch can join only one VTP domain. Switches in different VTP domains cannot share VLAN information.
l Working mode
VTP supports three working modes: VTP server, VTP client, and VTP transparent. For details, see Table 1-3.
Table 1-3 Working mode of VTP
| Working Mode | Description | 
| Server | A VTP server maintains all VLAN lists in the local VTP domain. It can create, delete, and modify VLANs, send advertisement packets, and synchronize VLAN information to other switches in the local VTP domain. VLAN information is saved in the nonvolatitle RAM (NVRAM). By default, a Cisco switch is used as the VTP server. | 
| Client | A VTP client learns VTP information from a VTP server. It cannot create, delete, or modify VLANs, but can forward advertisement packets. VLAN information is not saved in NVRAM. | 
| Transparent | A VTP transparent switch is an independent switch that does not participate in VTP implementation or learn VLAN information from the VTP server. It only maintains local VLAN information. The VTP transparent switch can create, delete, and modify only the local VLAN information. When VTP version 1 is used, VTP transparent switches can only forward VTP packets of other switches in the same VTP domain. When VTP version 2 is used, VTP transparent switches can forward VTP packets of switches in a different VTP domain. | 
Advertisement Packets
Switches use VTP advertisement packets to transmit VLAN information. Table 1-4 describes three types of VTP advertisement packets.
Table 1-4 Format of advertisement packets
| Format | Applicable Scenario | 
| Summary Advertisement | l By default, a VTP server sends a Summary Advertisement packet every 300s to inform adjacent switches of the current VTP domain name and the configuration revision number. When a switch receives a summary advertisement packet, the following situations occur: 1. The switch compares the VTP domain name with its own VTP domain name. If the names are different, the switch ignores the packet. 2. If the names are the same, the switch compares the configuration revision number with its configuration revision number. 3. If its configuration revision number is higher than or equal to the configuration revision number in the Summary Advertisement packet, the switch ignores the packet. If its configuration revision number is lower than the configuration revision number in the Summary Advertisement packet, the switch sends an Advertisement Request packet. l When a switch receives an Advertisement Request packet, it sends a Summary Advertisement packet, and then sends one or several Subset Advertisement packets. | 
| Subset Advertisement | When you add, delete, or change a VLAN on a switch, the VTP server where the changes are made increments the configuration revision number and sends a Summary Advertisement packet. Then the VTP server sends one or more Subset Advertisement packets. A subset advertisement contains a list of VLAN information. If there are several VLANs, the VTP server needs to send more than one Subset Advertisement packet to advertise all the VLANs. | 
| Advertisement Request | A switch needs an Advertisement Request packet in the following situations: l The switch restarts. l The VTP domain name has been changed. l The switch has received a Summary Advertisement packet with a higher configuration revision number than its own. | 
VTP advertisement packets have the following characteristics:
l VTP advertisement packets are transmitted in multicast mode through trunk interfaces in VLAN 1.
l VTP advertisement packets are sent to the destination MAC address 01-00-0C-CC-CC-CC.
l VTP advertisement packets are sent in either Inter-Switch Link (ISL) or IEEE 802.1Q (dot1q) frames.
The differences between Cisco VTP and Huawei VCMP are as follows.
l Multiple servers can exist in a Cisco VTP domain, and any switch can function as a VTP server. VTP servers synchronize information to each other.
l Only one switch in a VCMP domain functions as the VCMP server to control all VLAN configurations in the domain.
Table 1-5 Differences in command formats
| Function | Command on Huawei S Series Switches | Command on Cisco Switches | Description | 
| Configure the device role or mode. | vcmp role { client \| server \| silent \| transparent } | vtp mode { client \| off \| server \| transparent } | A switch used as a VCMP silent in a Huawei VCMP domain is similar to the switch in off mode in a Cisco VTP domain, and directly discards received protocol packets. | 
| Configure the domain name. | vcmp domain domain-name | vtp domain domain-name vtp domain domain-name | - | 
| Configure the domain ID. | vcmp device-id device-id | Not supported | Cisco VTP does not support the configuration. | 
| Configure an authentication password for the domain. | vcmp authentication sha2-256 password password | vtp password password | - | 
| Configuring the protocol version number. | Not supported | vtp version number | Huawei VCMP does not support the configuration. | 
| Check the protocol status. | display vcmp status | show vtp status | - | 
VTP and VCMP are proprietary protocols, and cannot interwork. Huawei S series switches and Cisco switches can be used on the entire network. Configurations can be performed on the switch that is directly connected to Huawei and Cisco switches to implement interworking between Huawei and Cisco switches. The following describes three types of hybrid networking models.
l 1.5.1 Hybrid Networking 1: C-H Model
In the C-H model, a Cisco switch directly connects to a Huawei S series switch that has no downstream Cisco switch connected.
l 1.5.2 Hybrid Networking 2: C-H-C Model
In the C-H-C model, a Cisco switch directly connects to a Huawei S series switch that has a downstream Cisco switch connected.
l 1.5.3 Hybrid Networking 3: C-H-H-C Model
In the C-H-H-C model, a Cisco switch directly connects to a Huawei S series switch, and another edge switch of the VCMP network connects to a Cisco switch.
Huawei S series switches can replace switches in a Cisco VTP domain.
l Replacing the transparent switch
In Figure 1-3, a Huawei S series switch replaces the VTP transparent switch on a Cisco network. After the replacement, you only need to create a VLAN manually on the Huawei S series switch and add interfaces to the VLAN. For details, see Huawei S series switch configuration in 1.5.1 Hybrid Networking 1: C-H Model.
Figure 1-3 Networking for replacing the transparent switch
l Replacing the client
In Figure 1-4, a Huawei S series switch replaces the VTP client on a Cisco network. After the replacement, you need to configure the Huawei S series switch to transparently transmit VTP packets. For details, see Huawei S series switch configuration in 1.5.2 Hybrid Networking 2: C-H-C Model.
Figure 1-4 Networking for replacing the VTP client
l Replacing the server
In Figure 1-5, no VTP server exists in the VTP domain after a Huawei S series switch replace it. If the Cisco network runs VTP version 1 or 2, any Cisco switch can function as the VTP server. If the Cisco network runs VTP version 3 alone or with VTP version1, you need to find a switch running VTP version 3 and run the vtp primary vlan command to specify the switch as the VTP server to manage the VTP domain.
The Huawei S series switch only needs to transparently transmit VTP packets. For details, see Huawei S series switch configuration in 1.5.2 Hybrid Networking 2: C-H-C Model.
Figure 1-5 Networking for replacing the VTP server
Overview
When VTP is enabled on the Cisco switch to synchronize VLAN information, the Huawei S series switch cannot process VTP packets. Therefore, a VLAN needs to be configured manually on the Huawei S series switch.
Configuration Notes
l This example applies to Huawei S series switches of all versions.
l If switchport dynamic auto or switchport dynamic desirable is configured on the Cisco switch interface before the Cisco switch interface is directly connects to the Huawei S series switch, change it to switchport mode trunk to prevent DTP negotiation failure.
Networking Requirements
In Figure 1-6, a Huawei S series switch directly connects to a Cisco VTP server. The Cisco switch and user hosts connected to the Huawei S series switch need to communicate in VLAN 10.
Figure 1-6 Hybrid networking of the C-H model
Configuration Roadmap
The configuration roadmap is as follows:
1. Check the configuration of the Cisco switch.
2. Create a VLAN manually on the Huawei S series switch and add interfaces to the VLAN.
Procedure
Step 1 Check the Cisco VTP server configuration. The display depends on the device configuration.
# Run the show running-config command to check the interface configuration.
! 
hostname VTP_Sever 
! 
interface GigabitEthernet5/1 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
! 
interface GigabitEthernet5/2 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
! 
interface GigabitEthernet5/3 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
!
If the interface configuration is incorrect, perform the following operations to configure the interface.
VTP_Sever# configure terminal 
VTP_Sever(config)# interface gigabitethernet 5/1 
VTP_Sever(config-if)# switchport trunk encapsulation dot1q   
VTP_Sever(config-if)# switchport mode trunk   
VTP_Sever(config-if)# exit 
VTP_Sever(config)# interface gigabitethernet 5/2 
VTP_Sever(config-if)# switchport trunk encapsulation dot1q 
VTP_Sever(config-if)# switchport mode trunk 
VTP_Sever(config-if)# exit 
VTP_Sever(config)# interface gigabitethernet 5/3 
VTP_Sever(config-if)# switchport trunk encapsulation dot1q 
VTP_Sever(config-if)# switchport mode trunk 
VTP_Sever(config-if)# exit 
# Run the show vlan brief command to check whether VLAN 10 has been created. If VLAN 10 is created, perform the following operation to create VLAN 10.
VTP_Sever(config)# vlan 10
# Run the show vtp status command to check whether the VTP working mode is server and whether the domain name is the same as that on the client. Run the show vtp password command to check whether the password is the same as that on the client.
If the VTP configuration is different from that on the client, perform the following operations to configure the VTP server.
VTP_Sever(config)# vtp domain Cisco   
VTP_Sever(config)# vtp mode server   
VTP_Sever(config)# vtp password Cisco   
Step 2 Check the Cisco VTP client configuration. The configurations of two VTP clients are the same. The following information is used for reference only.
! 
hostname VTP_Client 
! 
interface GigabitEthernet0/1 
 switchport access vlan 10 
 switchport trunk encapsulation dot1q 
 switchport mode access 
! 
interface GigabitEthernet0/2 
 switchport access vlan 10 
 switchport trunk encapsulation dot1q 
 switchport mode access 
! 
interface GigabitEthernet0/48 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
!
VTP_Client# configure terminal 
VTP_Client(config)# interface gigabitethernet 0/1 
VTP_Client(config-if)# switchport mode access   
VTP_Client(config-if)# switchport access vlan 10   
VTP_Client(config-if)# exit 
VTP_Client(config)# interface gigabitethernet 0/2 
VTP_Client(config-if)# switchport mode access 
VTP_Client(config-if)# switchport access vlan 10 
VTP_Client(config-if)# exit 
VTP_Client(config)# interface gigabitethernet 0/48   
VTP_Client(config-if)# switchport trunk encapsulation dot1q   
VTP_Client(config-if)# switchport mode trunk 
VTP_Client(config-if)# exit 
# Run the show vtp status command to check whether the VTP working mode is client and whether the domain name is the same as that on the server. Run the show vtp password command to check whether the password is the same as that on the server.
If the VTP configuration is different from that on the server, perform the following operations to configure the VTP client.
VTP_Client(config)# vtp domain Cisco   
VTP_Client(config)# vtp mode client   
VTP_Client(config)# vtp password Cisco   
Step 3 Configure the Huawei S series switch.
<HUAWEI> system-view 
[HUAWEI] vlan 10   
[HUAWEI-vlan10] quit 
[HUAWEI] interface GigabitEthernet1/0/48 
[HUAWEI-GigabitEthernet1/0/48] port link-type trunk   
[HUAWEI-GigabitEthernet1/0/48] port trunk allow-pass vlan 2 to 4094   
[HUAWEI-GigabitEthernet1/0/48] quit 
[HUAWEI] interface GigabitEthernet1/0/1 
[HUAWEI-GigabitEthernet1/0/1] port link-type access   
[HUAWEI-GigabitEthernet1/0/1] port default vlan 10   
[HUAWEI-GigabitEthernet1/0/1] quit 
Step 4 Verify the configuration.
Run the display vlan 10 command to check whether interfaces on the Huawei S series switch have been added to VLAN 10.
----End
When VTP is enabled on a Cisco switch to synchronize VLAN information, a Huawei S series switch with a downstream Cisco switch connected cannot process VTP packets. Therefore, the Huawei S series switch needs to transparently transmit VTP packets.
l When a Huawei S series switch sets up a Layer 2 tunnel to transparently transmit VTP packets, the destination multicast address must map to the unused multicast address to prevent address conflicts.
l The VTP tunnel must be set up on an interface of the Huawei S series switch in VLAN 1 where VTP packets are transmitted.
In Figure 1-7, a Huawei S series switch is directly connected to the Cisco VTP server and client. The Huawei S series switch needs to transparently transmit VTP packets to the Cisco VTP client, and user hosts need to communicate in VLAN 10.
Figure 1-7 Hybrid networking of the C-H-C model
1. Check the configuration of Cisco switches.
2. Configure Layer 2 transparent transmission on the Huawei S series switch to transparently transmit VTP packets.
3. Create a VLAN manually on the Huawei S series switch and add interfaces to the VLAN.
! 
hostname VTP_Sever 
! 
interface GigabitEthernet5/1 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
! 
interface GigabitEthernet5/3 
 switchport trunk encapsulation dot1q 
 switchport mode trunk 
!
VTP_Sever# configure terminal 
VTP_Sever(config)# interface gigabitethernet 5/1 
VTP_Sever(config-if)# switchport trunk encapsulation dot1q   
VTP_Sever(config-if)# switchport mode trunk   
VTP_Sever(config-if)# exit 
VTP_Sever(config)# interface gigabitethernet 5/3 
VTP_Sever(config-if)# switchport trunk encapsulation dot1q 
VTP_Sever(config-if)# switchport mode trunk 
VTP_Sever(config-if)# exit 
If the VTP configuration is incorrect, perform the following operations to configure the VTP server.
If the VTP configuration is incorrect, perform the following operations to configure the VTP client.
# Configure Layer 2 transparent transmission on the Huawei S series switch.
<HUAWEI> system-view 
[HUAWEI] l2protocol-tunnel vtp group-mac 0100-5e00-0011   
[HUAWEI] interface GigabitEthernet1/0/48 
[HUAWEI-GigabitEthernet1/0/48] l2protocol-tunnel vtp vlan 1   
[HUAWEI-GigabitEthernet1/0/48] quit 
[HUAWEI] interface GigabitEthernet1/0/46 
[HUAWEI-GigabitEthernet1/0/46] l2protocol-tunnel vtp vlan 1   
[HUAWEI-GigabitEthernet1/0/46] quit 
# Add interfaces on the Huawei S series
[HUAWEI] vlan 10   
[HUAWEI-vlan10] quit 
[HUAWEI] interface GigabitEthernet1/0/48 
[HUAWEI-GigabitEthernet1/0/48] port link-type trunk   
[HUAWEI-GigabitEthernet1/0/48] port trunk allow-pass vlan 2 to 4094   
[HUAWEI-GigabitEthernet1/0/48] quit 
[HUAWEI] interface GigabitEthernet1/0/46 
[HUAWEI-GigabitEthernet1/0/46] port link-type trunk    
[HUAWEI-GigabitEthernet1/0/46] port trunk allow-pass vlan 2 to 4094   
[HUAWEI-GigabitEthernet1/0/46] quit 
l Run the display l2protocol-tunnel group-mac vtp command to check the Layer 2 transparent transmission configuration on the Huawei S series switch.
l Run the display vlan 10 command to check whether interfaces on the Huawei S series switch have been added to VLAN 10.
Furthermore, the Huawei S series switch uses VCMP to synchronize VLAN information, and interfaces on the switch use Link-type Negotiation Protocol (LNP), reducing the configuration and maintenance workload.
l This example applies to Huawei S series switches of V200R005 and later versions.
In Figure 1-8, a Huawei S series switch and a Cisco switch are deployed on a network. To reduce the configuration and maintenance workload, the Huawei S series switch uses VCMP and the Cisco switch uses VTP to synchronize VLAN information to other switches. The Cisco switch and user hosts connected to the Huawei S series switch need to communicate in VLAN 10.
Figure 1-8 Hybrid networking of the C-H-H-C model
3. Configure VCMP and LNP on the Huawei S series switch.
Step 3 Configure the VCMP server on the Huawei S series switch.
# Configure Layer 2 transparent transmission on the VCMP server.
<HUAWEI> system-view 
<HUAWEI> sysname Server 
[Server] l2protocol-tunnel vtp group-mac 0100-5e00-0011   
[Server] interface GigabitEthernet1/0/1 
[Server-GigabitEthernet1/0/1] l2protocol-tunnel vtp vlan 1   
[Server-GigabitEthernet1/0/1] quit 
[Server] interface GigabitEthernet1/0/2 
[Server-GigabitEthernet1/0/2] l2protocol-tunnel vtp vlan 1   
[Server-GigabitEthernet1/0/2] quit 
# Configure VCMP on the VCMP server.
[Server] vcmp domain huawei   
[Server] vcmp role server   
[Server] vcmp authentication sha2-256 password huawei   
[Server] vlan 10   
[Server-vlan10] quit
# Add interfaces on the VCMP server to the VLAN.
[Server] interface GigabitEthernet1/0/1 
[Server-GigabitEthernet1/0/48] port link-type trunk   
[Server-GigabitEthernet1/0/48] port trunk allow-pass vlan 2 to 4094   
[Server-GigabitEthernet1/0/48] quit 
[Server] interface GigabitEthernet1/0/2 
[Server-GigabitEthernet1/0/2] port link-type trunk   
[Server-GigabitEthernet1/0/2] port trunk allow-pass vlan 2 to 4094   
[Server-GigabitEthernet1/0/2] quit 
[Server] interface GigabitEthernet1/0/3 
[Server-GigabitEthernet1/0/3] port link-type negotiation-desirable   
[Server-GigabitEthernet1/0/3] port default vlan 10   
[Server-GigabitEthernet1/0/3] quit 
Step 4 Configure VCMP client 1 on the Huawei S series switch.
# Configure Layer 2 transparent transmission on VCMP client 1.
<HUAWEI> system-view 
<HUAWEI> sysname Client1 
[Client1] l2protocol-tunnel vtp group-mac 0100-5e00-0011   
[Client1] interface GigabitEthernet1/0/48 
[Client1-GigabitEthernet1/0/48] l2protocol-tunnel vtp vlan 1   
[Client1-GigabitEthernet1/0/48] quit 
[Client1] interface GigabitEthernet1/0/46 
[Client1-GigabitEthernet1/0/46] l2protocol-tunnel vtp vlan 1   
[Client1-GigabitEthernet1/0/46] quit 
# Configure VCMP on VCMP client 1.
[Client1] vcmp domain huawei   
[Client1] vcmp role client   
[Client1] vcmp authentication sha2-256 password huawei   
# Add interfaces on VCMP client 1 to the VLAN.
[Client1] interface GigabitEthernet1/0/48 
[Client1-GigabitEthernet1/0/48] port link-type trunk   
[Client1-GigabitEthernet1/0/48] port trunk allow-pass vlan 2 to 4094   
[Client1-GigabitEthernet1/0/48] quit 
[Client1] interface GigabitEthernet1/0/46 
[Client1-GigabitEthernet1/0/46] port link-type trunk   
[Client1-GigabitEthernet1/0/46] port trunk allow-pass vlan 2 to 4094   
[Client1-GigabitEthernet1/0/46] quit 
[Client1] interface GigabitEthernet1/0/1 
[Client1-GigabitEthernet1/0/1] port link-type access   
[Client1-GigabitEthernet1/0/1] port default vlan 10   
[Client1-GigabitEthernet1/0/1] quit 
Step 5 Configure VCMP client 2 on the Huawei S series switch.
# Configure VCMP on VCMP client 2.
<HUAWEI> system-view 
<HUAWEI> sysname Client2 
[Client2] vcmp domain huawei   
[Client2] vcmp role client    
[Client2] vcmp authentication sha2-256 password huawei   
# Add interfaces on VCMP client 2 to the VLAN.
[Client2] interface GigabitEthernet1/0/1 
[Client2-GigabitEthernet1/0/1] port link-type access   
[Client2-GigabitEthernet1/0/1] port default vlan 10   
[Client2-GigabitEthernet1/0/1] quit 
[Client2] interface GigabitEthernet1/0/2 
[Client2-GigabitEthernet1/0/2] port default vlan 10   
[Client2-GigabitEthernet1/0/2] quit 
Step 6 Verify the configuration.
l Run the display vcmp status command to check the VCMP configuration on the Huawei S series switch.