vPCs
vPCs — concept overview.
vPCs
A virtual port channel (vPC) allows links that are physically connected to two Cisco Nexus 9000 Series devices to appear as a single port channel by a third device (see figure). The third device can be a switch, server, or any other networking device that supports port channels. A vPC can provide Layer 2 multipathing, which allows you to create redundancy and increase the bisectional bandwidth by enabling multiple parallel paths between nodes and allowing load balancing traffic.
- 
                                    
                                    Allows a single device to use a port channel across two upstream devices
- 
                                    
                                    Eliminates Spanning Tree Protocol (STP) blocked ports
- 
                                    
                                    Provides a loop-free topology
- 
                                    
                                    Uses all available uplink bandwidth
- 
                                    
                                    Provides fast convergence if either the link or a device fails
- 
                                    
                                    Provides link-level resiliency
- 
                                    
                                    Assures high availability
The virtual port channel (vPC) is a technology that allows a single downstream device to connect to two upstream devices as though they were one logical device.
- 
                                    
                                    Layer 2 port channel support
- 
                                    
                                    Link Aggregation Control Protocol (LACP) optional
- 
                                    
                                    Enables redundancy and load balancing
vPC supports trunk mode port channels with or without LACP, and improves network stability and convergence.
Protocol Details and Recommendations
You can use only Layer 2 port channels in the vPC. You configure the port channels by using one of the following:
- 
                                    
                                    No protocol
- 
                                    
                                    Link Aggregation Control Protocol (LACP)
When you configure the port channels in a vPC—including the vPC Peer-Link channel—without using LACP, each device can have up to 32 active links in a single port channel. When using LACP, each device can have 32 active links and eight standby links.
| Note | You must enable the vPC feature before you can configure or run the vPC functionality. | 
The system automatically takes a checkpoint prior to disabling the feature, and you can roll back to this checkpoint.
After you enable the vPC functionality, you create the peer-keepalive link, which sends heartbeat messages between the two vPC peer devices.
To ensure that you have the correct hardware to enable and run a vPC, enter the show hardware feature-capability command. If you see an X across from the vPC in your command output, your hardware cannot enable the vPC feature.
| Note | Devices attached to a vPC domain using port channels should be connected to both of vPC peers. | 
Peer-Link Creation Example
You can create a vPC Peer-Link by configuring a port channel on one Cisco Nexus 9000 Series chassis by using two or more Ethernet ports higher speed than 1-Gigabit Ethernet.
We recommend that you configure the vPC Peer-Link Layer 2 port channels as trunks. On another Cisco Nexus 9000 Series chassis, you configure another port channel again using two or more Ethernet ports with speed higher than 1-Gigabit in the dedicated port mode.
Connecting these two port channels creates a vPC Peer-Link in which the two linked Cisco Nexus devices appear as one device to a third device.
Incorrect Hardware or Module Usage
If you are not using the correct module, the system displays an error message.
Once you configure this feature and if the primary vPC peer device fails, the system automatically suspends all the vPC links on the primary vPC peer device.
Track Object Recommendation
You can create a track object and apply that object to all links on the primary vPC peer device that connect to the core and to the vPC Peer-Link.
If you must configure all the vPC Peer-Links and core-facing interfaces on a single module, you should configure a track object.
Hitless vPC Role Changes
A virtual port channel (vPC) allows links that are physically connected to two different Cisco Nexus 9000 Series devices to appear as a single port channel. The vPC role change feature enables you switch vPC roles between vPC peers without impacting traffic flow. The vPC role switching is done based on the role priority value of the device under the vPC domain. A vPC peer device with lower role priority is selected as the primary vPC device during the vPC Role switch. You can use the vpc role preempt command to switch vPC role between peers.
Additional Information
For information about how to configure Hitless vPC Role Change, see Configure the Hitless vPC Role Change.
vPC Terminology
The terminology used in vPCs is as follows:
- 
                                       						
                                       vPC—The combined port channel between the vPC peer devices and the downstream device.
- 
                                       						
                                       vPC peer device—One of a pair of devices that are connected with the special port channel known as the vPC Peer-Link.
- 
                                       						
                                       vPC Peer-Link—The link used to synchronize state between the vPC peer devices. This link must use a 10-Gigabit Ethernet interface at a minimum. Higher-bandwidth interfaces (such as 25-Gigabit Ethernet, 40-Gigabit Ethernet, 100-Gigabit Ethernet, and so on) may also be used.
- 
                                       						
                                       vPC member port—An interface that belongs to a vPC.
- 
                                       						
                                       Host vPC port—A Fabric Extender host interface that belongs to a vPC.
- 
                                       						
                                       vPC domain—This domain includes both vPC peer devices, the vPC peer-keepalive link, and all of the port channels in the vPC connected to the downstream devices. It is also associated to the configuration mode that you must use to assign vPC global parameters.
- 
                                       						
                                       vPC peer-keepalive link—The peer-keepalive link monitors the vitality of a vPC peer Cisco Nexus 9000 Series device. The peer-keepalive link sends configurable, periodic keepalive messages between vPC peer devices. We recommend that you associate a peer-keepalive link to a separate virtual routing and forwarding (VRF) instance that is mapped to a Layer 3 interface in each vPC peer device. If you do not configure a separate VRF, the system uses the management VRF by default. However, if you use the management interfaces for the peer-keepalive link, you must put a management switch connected to both the active and standby management ports on each vPC peer device (see figure). No data or synchronization traffic moves over the vPC peer-keepalive link; the only traffic on this link is a message that indicates that the originating switch is operating and running a vPC.
- 
                                       						
                                       Dual-active—Both vPC peers act as primary. This situation occurs when the peer-keepalive and vPC Peer-Link go down while both peers are still active. In this case, the secondary vPC assumes that the primary vPC is inactive and acts as the primary vPC.
- 
                                       						
                                       Recovery—When the peer-keepalive and the vPC Peer-Link come up, one switch becomes the secondary vPC. On the switch that becomes the secondary vPC, the vPC links go down and come back up.
vPC Peer-Links
You can have only two devices as vPC peers; each device can serve as a vPC peer to only one other vPC peer. The vPC peer devices can also have non-vPC links to other devices.
Reference Information
See the following figure for invalid vPC peer configurations.
To make a valid configuration, you first configure a port channel on each device and then configure the vPC domain. You assign the port channel on each device as a vPC Peer-Link, using the same vPC domain ID. For redundancy, we recommend that you should configure at least two of the dedicated ports into the port channel because if one of the interfaces in the vPC Peer-Link fails, the device automatically falls back to use another interface in the vPC Peer-Link.
| Note | We recommend that you configure the Layer 2 port channels in trunk mode. | 
Compatibility and Configuration Consistency
Many operational parameters and configuration parameters must be the same in each device connected by a vPC Peer-Link (see the Compatibility Parameters for vPC Interfaces section). Because each device is completely independent on the management plane, you must ensure that the devices are compatible on the critical parameters. vPC peer devices have separate control planes. After configuring the vPC Peer-Link, you should display the configuration on each vPC peer device to ensure that the configurations are compatible.
| Note | You must ensure that the two devices connected by the vPC Peer-Link have certain identical operational and configuration parameters. For more information on required configuration consistency, see the Compatibility Parameters for vPC Interfaces section. | 
Primary and Secondary Device Roles
When you configure the vPC Peer-Link, the vPC peer devices negotiate that one of the connected devices is the primary device and the other connected device is the secondary device (see the “Configuring vPCs” section). By default, the Cisco NX-OS software uses the lowest MAC address to elect the primary device. However, if the role priority is set, then the device with the lowest priority will be elected as the primary device. The software takes different actions on each device—that is, the primary and secondary—only in certain failover conditions. If the primary device fails, the secondary device becomes the new primary device when the system recovers, and the previously primary device is now the secondary device.
You can also configure which of the vPC devices is the primary device. Changing the priority of the vPC peer devices can cause the interfaces in your network to go up and down. If you want to configure the role priority again to make one vPC device the primary device, configure the role priority on both the primary vPC device with a lower priority value and the secondary vPC device with the higher value. Then, shut down the port channel that is the vPC Peer-Link on both devices by entering the shutdown command, and finally reenable the port channel on both devices by entering the no shutdown command.
| Note | We recommend that you use two different modules for redundancy on each vPC peer device on each vPC Peer-Link. | 
Traffic Flow and Load Balancing
The software keeps all traffic that forwards across the vPC peer devices as local traffic. A packet that ingresses the port channel uses one of the local links rather than moving across the vPC Peer-Link. Unknown unicast, multicast, and broadcast traffic (including STP BPDUs) are flooded across the vPC Peer-Link. The software keeps the multicast forwarding state synchronized on both of the vPC peer devices.
You can configure any of the standard load-balancing schemes on both the vPC Peer-Link devices and the downstream device (see the Configuring Port Channels chapter for information about load balancing).
Configuration and MAC Address Synchronization
Configuration information flows across the vPC Peer-Links using the Cisco Fabric Services over Ethernet (CFSoE) protocol. (See the section for more information about CFSoE.)
All MAC addresses for those VLANs configured on both devices are synchronized between vPC peer devices. The software uses CFSoE for this synchronization. (See the section for information about CFSoE.)
vPC Peer-Link Failure and Peer-Keepalive
If the vPC Peer-Link fails, the software checks the status of the remote vPC peer device using the peer-keepalive link, which is a link between vPC peer devices that ensures that both devices are up. If the vPC peer device is up, the secondary vPC device disables all vPC ports on its device, to prevent loops and disappearing or flooding traffic. The data then forwards down the remaining active links of the port channel.
The software learns of a vPC peer device failure when the keepalive messages are not returned over the peer-keepalive link.
Use a separate link (vPC peer-keepalive link) to send configurable keepalive messages between the vPC peer devices. The keepalive messages on the vPC peer-keepalive link determines whether a failure is on the vPC Peer-Link only or on the vPC peer device. The keepalive messages are used only when all the links in the vPC Peer-Link fail. See the “Peer-Keepalive Link and Messages” section for information about the keepalive message.
Features That You Must Manually Configure on the Primary and Secondary Devices
You must manually configure the following features to conform to the primary/secondary mapping of each of the vPC peer devices.
STP Root Configuration
STP root—Configure the primary vPC peer device as the STP primary root device and configure the vPC secondary device to be the STP secondary root device. See the “vPC Peer-Links and STP” section for more information about vPCs and STP.
- 
                                       					
                                       We recommend that you configure the vPC Peer-Link interfaces as STP network ports so that Bridge Assurance is enabled on all vPC Peer-Links.
- 
                                       					
                                       We recommend that you configure Rapid per VLAN Spanning Tree plus (PVST+) so that the primary device is the root for all VLANs and configure Multiple Spanning Tree (MST) so that the primary device is the root for all instances.
Layer 3 VLAN Network Interface Configuration
Layer 3 VLAN network interface—Configure Layer 3 connectivity from each vPC peer device by configuring a VLAN network interface for the same VLAN from both devices.
HSRP Active Configuration
HSRP active—If you want to use Hot Standby Router Protocol (HSRP) and VLAN interfaces on the vPC peer devices, configure the primary vPC peer device with the HSRP active highest priority. Configure the secondary device to be the HSRP standby and ensure that you have VLAN interfaces on each vPC device that are in the same administrative and operational mode. (See the “vPC Peer-Links and Routing” section for more information on vPC and HSRP.)
UDLD Configuration Recommendations
While you configure Unidirectional Link Detection (UDLD), note the following recommendations:
- 
                                       					
                                       If LACP is used as port-channel aggregation protocol, UDLD is not required in a vPC domain.
- 
                                       					
                                       If LACP is not used as the port-channel aggregation protocol (static port-channel), use UDLD in normal mode on vPC member ports.
- 
                                       					
                                       If STP is used without Bridge Assurance and if LACP is not used, use UDLD in normal mode on vPC orphan ports.
Layer 3 Backup Routes
You can use VLAN network interfaces on the vPC peer devices to link to Layer 3 of the network for such applications as HSRP and PIM. Ensure that you have a VLAN network interface configured on each peer device and that the interface is connected to the same VLAN on each device. Also, each VLAN interface must be in the same administrative and operational mode. For more information about configuring VLAN network interfaces, see the “Configuring Layer 3 Interfaces” chapter.
If a failover occurs on the vPC Peer-Link, the VLAN interfaces on the vPC peer devices are also affected. If a vPC Peer-Link fails, the system brings down associated VLAN interfaces on the secondary vPC peer device.
You can ensure that specified VLAN interfaces do not go down on the vPC secondary device when the vPC Peer-Link fails.
Peer-Keepalive Links and Messages
The Cisco NX-OS software uses the peer-keepalive link between the vPC peers to transmit periodic, configurable keepalive messages. You must have Layer 3 connectivity between the peer devices to transmit these messages; the system cannot bring up the vPC Peer-Link unless the peer-keepalive link is already up and running.
| Note | We recommend that you associate the vPC peer-keepalive link to a separate VRF mapped to a Layer 3 interface in each vPC peer device. If you do not configure a separate VRF, the system uses the management VRF and management ports by default. Do not use the vPC Peer-Link itself to send and receive vPC peer-keepalive messages. | 
Failure Detection and Keepalive Timers
If one of the vPC peer devices fails, the vPC peer device on the other side of the vPC Peer-Link senses the failure by not receiving any peer-keepalive messages. The default interval time for the vPC peer-keepalive message is 1 second, and you can configure the interval between 400 milliseconds and 10 seconds.
You can configure a hold-timeout value with a range of 3 to 10 seconds; the default hold-timeout value is 3 seconds. This timer starts when the vPC Peer-Link goes down. During this hold-timeout period, the secondary vPC peer device ignores vPC peer-keepalive messages, which ensures that network convergence occurs before a vPC action takes place. The purpose of the hold-timeout period is to prevent false-positive cases.
You can also configure a timeout value with a range of 3 to 20 seconds; the default timeout value is 5 seconds. This timer starts at the end of the hold-timeout interval. During the timeout period, the secondary vPC peer device checks for vPC peer-keepalive hello messages from the primary vPC peer device. If the secondary vPC peer device receives a single hello message, that device disables all vPC interfaces on the secondary vPC peer device.
Hold-Timeout vs. Timeout Parameters
The difference between the hold-timeout and the timeout parameters is as follows:
- 
                                    					
                                    During the hold-timeout, the vPC secondary device does not take any action based on any keepalive messages received, which prevents the system taking action when the keepalive might be received just temporarily, such as if a supervisor fails a few seconds after the vPC Peer-Link goes down.
- 
                                    					
                                    During the timeout, the vPC secondary device takes action to become the vPC primary device if no keepalive message is received by the end of the configured interval.
See the “Configuring vPC Keepalive Link and Messages” section for information about configuring the timer for the keepalive messages.
| Note | Ensure that both the source and destination IP addresses used for the peer-keepalive messages are unique in your network and these IP addresses are reachable from the VRF associated with the vPC peer-keepalive link. Peer-keepalive IP addresses must be global unicast addresses. Link-local addresses are not supported. | 
Configuring Trusted Ports for Peer-Keepalive
Use the command-line interface (CLI) to configure the interfaces you are using the vPC peer-keepalive messages as trusted ports. Leave the precedence at the default (6) or configure it higher.
vPC Peer-Gateways
You can configure vPC peer devices to act as the gateway even for packets that are destined to the vPC peer device’s MAC address.
Use the peer-gateway command to configure this feature.
| Note |  | 
Some network-attached storage (NAS) devices or load balancers might have features that help to optimize the performances of particular applications. These features enable the device to avoid a routing-table lookup when responding to a request that originated from a host that is not locally attached to the same subnet. Such devices might reply to traffic using the MAC address of the sender Cisco Nexus 9000 Series device rather than the common HSRP gateway. This behavior is noncomplaint with some basic Ethernet RFC standards. Packets that reach a vPC device for the nonlocal router MAC address are sent across the vPC Peer-Link and could be dropped by the built in vPC loop avoidance mechanism if the final destination is behind another vPC.
The vPC peer-gateway capability allows a vPC switch to act as the active gateway for packets that are addressed to the router MAC address of the vPC peer. This feature enables local forwarding of packets without the need to cross the vPC Peer-Link. In this scenario, the feature optimizes use of the vPC Peer-Link and avoids potential traffic loss.
Configuring the peer-gateway feature must be done on both primary and secondary vPC peers and is nondisruptive to the operations of the device or to the vPC traffic. The vPC peer-gateway feature can be configured globally under the vPC domain submode.
When you enable this feature, Cisco NX-OS automatically disables IP redirects on all interface VLANs mapped over a vPC VLAN to avoid generation of IP redirect messages for packets switched through the peer gateway router.
Packets that arrive at the peer-gateway vPC device have their Time to Live (TTL) decremented, so that packets carrying a TTL of 1 might get dropped in transit due to TTL expiration. You should take this situation into account when the peer-gateway feature is enabled and particular network protocols that source packets with a TTL of 1 operate on a vPC VLAN.
vPC Domains
You can use the vPC domain ID to identify the vPC Peer-Links and the ports that are connected to the vPC downstream devices.
The vPC domain is also a configuration mode that you use to configure the keepalive messages and other vPC Peer-Link parameters rather than accept the default values. See the “Configuring vPCs” section for more information about configuring these parameters.
vPC Domain Creation and Peer-Link Configuration
To create a vPC domain, you must first create a vPC domain ID on each vPC peer device using a number from 1 to 1000. You can have only one vPC domain per vPC peer.
You must explicitly configure the port channel that you want to act as the vPC Peer-Link on each device. You associate the port channel that you made a vPC Peer-Link on each device with the same vPC domain ID to form a single vPC domain. Within this domain, the system provides a loop-free topology and Layer 2 multipathing.
You can only configure these port channels and vPC Peer-Links statically. You can configure the port channels and vPC Peer-Links either using LACP or no protocol. We recommend that you use LACP with the interfaces in active mode to configure port channels in each vPC, which ensures an optimized, graceful recovery in a port-channel failover scenario and provides configuration checks against configuration mismatches among the port channels themselves.
vPC System MAC Address Assignment
The vPC peer devices use the vPC domain ID that you configure to automatically assign a unique vPC system MAC address. Each vPC domain has a unique MAC address that is used as a unique identifier for the specific vPC-related operations, although the devices use the vPC system MAC addresses only for link-scope operations, such as LACP. We recommend that you create each vPC domain within the contiguous Layer 2 network with a unique domain ID. You can also configure a specific MAC address for the vPC domain, rather than having the Cisco NX-OS software assign the address.
See the “vPC and Orphan Ports” section for more information about displaying the vPC MAC table.
vPC Domain System Priority
After you create a vPC domain, the Cisco NX-OS software creates a system priority for the vPC domain. You can also configure a specific system priority for the vPC domain.
| Note | When manually configuring the system priority, you must ensure that you assign the same priority value on both vPC peer devices. If the vPC peer devices have different system priority values, vPC does not come up. | 
How VPC Topologies Work
In both topologies, port channels P020 and P0200 must be configured identically on the peer switches and configuration synchronization is used to synchronize the configurations of the vPC switches.
Summary
This document describes two common vPC topologies: a basic configuration with directly connected Cisco Nexus 9000 Series devices and a configuration involving Fabric Extenders (FEXs) for host vPC.
Workflow
These stages describe how vPC topologies work.
- The first topology shows a basic configuration in which the Cisco Nexus 9000 Series device ports are directly connected to another switch or host and are configured as part of a port channel that becomes part of a vPC. In this configuration, vPC 20 is configured on port channel 20, which has Eth1/10 on the first device and Eth2/1 on the second as member ports.
- The second topology illustrates how to configure a vPC from the peer devices through Fabric Extenders (FEXs). In this FEX straight-through topology, each FEX is single-homed with a Cisco Nexus 9000 Series device. The host interfaces on this FEX are configured as port channels, and those port channels are configured as vPCs. For example, Eth101/1/1 and Eth102/1/5 are configured as members of PO200, and PO200 is configured for vPC 200.
What’s next
See the Cisco Nexus 2000 Series NX-OS Fabric Extender Configuration Guide for Cisco Nexus 9000 Series Switches for more information about configuring FEX ports.
Compatibility Parameters
Many configuration and operational parameters must be identical on all interfaces in the vPC. We recommend that you configure the Layer 2 port channels that you use for the vPC Peer-Link in trunk mode.
After you enable the vPC feature and configure the vPC Peer-Link on both vPC peer devices, Cisco Fabric Services (CFS) messages provide a copy of the configuration on the local vPC peer device configuration to the remote vPC peer device. The system then determines whether any of the crucial configuration parameters differ on the two devices. (See the “vPC and Orphan Ports” section for more information about CFS.)
The vPC Peer-Link is a core component of vPC functionality, requiring consistent configuration across both peer devices.
- 
                                    					
                                    Layer 2 port channels for vPC Peer-Link must be configured in trunk mode.
- 
                                    					
                                    Compatibility parameters must be identical across all interfaces in the vPC.
For example, the compatibility check process differs for vPCs compared to regular port channels.
Configuration and Guidelines
After enabling the vPC feature and configuring the vPC Peer-Link, Cisco Fabric Services (CFS) ensures configuration consistency between the local and remote vPC peer devices.
| Note | Enter the show vpc consistency-parameters command to display the configured values on all interfaces in the vPC. The displayed configurations are only those that would limit the vPC Peer-Link and vPC from coming up. | 
| Note | The port channel compatibility parameters must be the same for all the port channel members on the physical switch. You cannot configure shared interfaces to be part of a vPC. | 
See the “Configuring Port Channels” chapter for more details about regular port channels.
Configuration Parameters That Must Be Identical
The configuration parameters in this section must be configured identically on both devices of the vPC Peer-Link; otherwise, the vPC moves fully or partially into a suspended mode.
- 
                                          						
                                          You must ensure that all interfaces in the vPC have the identical operational and configuration parameters listed in this section.
- 
                                          						
                                          Enter the show vpc consistency-parameters command to display the configured values on all interfaces in the vPC. The displayed configurations are only those configurations that would limit the vPC Peer-Link and vPC from coming up.
The devices automatically check for compatibility for some of these parameters on the vPC interfaces. The per-interface parameters must be consistent per interface, and the global parameters must be consistent globally.
- 
                                          						
                                          Port-channel mode: on, off, or active (port-channel mode can, however, be active/passive on each side of the vPC peer)
- 
                                          						
                                          Link speed per channel
- 
                                          						
                                          Duplex mode per channel
- 
                                          						
                                          Trunk mode per channel: 
  - 
                                                								
                                                Native VLAN
  - 
                                                								
                                                VLANs allowed on trunk
  - 
                                                								
                                                Tagging of native VLAN traffic
- 
                                                								
                                                
- 
                                          						
                                          Spanning Tree Protocol (STP) mode
- 
                                          						
                                          STP region configuration for Multiple Spanning Tree
- 
                                          						
                                          Enable/disable state per VLAN
- 
                                          						
                                          STP global settings: 
  - 
                                                								
                                                Bridge Assurance setting
  - 
                                                								
                                                Port type setting
  - 
                                                								
                                                Loop Guard settings
- 
                                                								
                                                
- 
                                          						
                                          STP interface settings: 
  - 
                                                								
                                                Port type setting
  - 
                                                								
                                                Loop Guard
  - 
                                                								
                                                Root Guard
- 
                                                								
                                                
- 
                                          						
                                          Maximum Transmission Unit (MTU)
If any of these parameters are not enabled or defined on either device, the vPC consistency check ignores those parameters.
- 
                                          						
                                          To ensure that none of the vPC interfaces are in the suspend mode, enter the show vpc brief and show vpc consistency-parameters commands and check the syslog messages. In the output of show vpc or show vpc brief command, after every 50th configured vPC port-channel the following message will be displayed: Please check "show vpc consistency-parameters vpc <vpc-num>" for the consistency reason of down vpc and for type-2 consistency reasons for any vpc.
Configuration Parameters That Should Be Identical
Configure the following parameters identically on both vPC peer devices to prevent misconfiguration and undesirable traffic behavior.
- 
                                          						
                                          MAC aging timers
- 
                                          						
                                          Static MAC entries
- 
                                          						
                                          VLAN interface—Each device on the end of the vPC Peer-Link must have a VLAN interface configured for the same VLAN on both ends and they must be in the same administrative and operational mode. Those VLANs configured on only one device of the vPC Peer-Link do not pass traffic using the vPC or vPC Peer-Link. You must create all VLANs on both the primary and secondary vPC devices, or the VLAN will be suspended.
- 
                                          						
                                          All ACL configurations and parameters
- 
                                          						
                                          Quality of Service (QoS) configuration and parameters
- 
                                          						
                                          STP interface settings: 
  - 
                                                								
                                                BPDU Filter
  - 
                                                								
                                                BPDU Guard
  - 
                                                								
                                                Cost
  - 
                                                								
                                                Link type
  - 
                                                								
                                                Priority
  - 
                                                								
                                                VLANs (Rapid PVST+)
- 
                                                								
                                                
- 
                                          						
                                          Port security
- 
                                          						
                                          Cisco Trusted Security (CTS)
- 
                                          						
                                          Dynamic Host Configuration Protocol (DHCP) snooping
- 
                                          						
                                          Network Access Control (NAC)
- 
                                          						
                                          Dynamic ARP Inspection (DAI)
- 
                                          						
                                          IP source guard (IPSG)
- 
                                          						
                                          Internet Group Management Protocol (IGMP) snooping
- 
                                          						
                                          Hot Standby Routing Protocol (HSRP)
- 
                                          						
                                          Protocol Independent Multicast (PIM)
- 
                                          						
                                          All routing protocol configurations
After configuring the vPC, display the configurations for each vPC peer device to ensure all parameters are compatible.
Consequences of Parameter Mismatches
You can configure the graceful consistency check feature, which suspends only the links on the secondary peer device when a mismatch is introduced in a working vPC. This feature is configurable only in the CLI and is enabled by default.
Consistency Check Behavior
The graceful consistency-check command is configured by default.
As part of the consistency check of all parameters from the list of parameters that must be identical, the system checks the consistency of all VLANs.
The vPC remains operational, and only the inconsistent VLANs are brought down. This per-VLAN consistency check feature cannot be disabled and does not apply to Multiple Spanning Tree (MST) VLANs.
Deleting the vPC port-channel on the switch results in the suspension of the allowed VLANs on the corresponding vPC port-channel on the peer switch, regardless of the vPC role.
vPC Numbers
Once you have created the vPC domain ID and the vPC Peer-Link, you create port channels to attach the downstream device to each vPC peer device. That is, you create one port channel to the downstream device from the primary vPC peer device and you create another port channel to the downstream device from the secondary peer device.
| Note | We recommend that you configure the ports on the downstream devices that connect to a host or a network device that is not functioning as a switch or a bridge as STP edge ports. | 
On each vPC peer device, you assign a vPC number to the port channel that connects to the downstream device. You will experience minimal traffic disruption when you are creating vPCs. To simplify the configuration, you can assign the vPC ID number to every port channel to be the same as the port channel itself (that is, vPC ID 10 for port channel 10).
| Note | The vPC number that you assign to the port channel that connects to the downstream device from the vPC peer device must be identical on both vPC peer devices. | 
Moving Other Port Channels into vPCs
Moving Other Port Channels into vPCs — concept overview.
| Note | You must attach a downstream device using a port channel to both vPC peer devices. | 
To connect to the downstream device, you create a port channel to the downstream device from the primary vPC peer device and you create another port channel to the downstream device from the secondary peer device. On each vPC peer device, you assign a vPC number to the port channel that connects to the downstream device. You will experience minimal traffic disruption when you are creating vPCs.
vPC Object Trackings
vPC Object Trackings — concept overview.
vPC Object Tracking
| Note | We recommend that you configure the vPC Peer-Links on dedicated ports of different modules on Cisco Nexus 9500 devices. This is recommended to reduce the possibility of a failure. For the best resiliency scenario, use at least two modules. | 
vPC object tracking is used to prevent traffic black-holing in case of failure of a module where both vPC Peer-Link and uplinks to the core resides. By tracking interface feature can suspend vPC on affected switch and prevent traffic black-holing.
If you must configure all the vPC Peer-Links and core-facing interfaces on a single module, you should configure, using the command-line interface, a track object and a track list that is associated with the Layer 3 link to the core and on all vPC Peer-Links on both vPC peer devices. You use this configuration to avoid dropping traffic if that particular module goes down because when all the tracked objects on the track list go down, the system does the following:
- 
                                    					
                                    Stops the vPC primary peer device sending peer-keepalive messages, which forces the vPC secondary peer device to take over.
- 
                                    					
                                    Brings down all the downstream vPCs on that vPC peer device, which forces all the traffic to be rerouted in the access switch toward the other vPC peer device.
Once you configure this feature and if the module fails, the system automatically suspends all the vPC links on the primary vPC peer device and stops the peer-keepalive messages. This action forces the vPC secondary device to take over the primary role and all the vPC traffic to go to this new vPC primary device until the system stabilizes.
You should create a track list that contains all the links to the core and all the vPC Peer-Links as its object. Enable tracking for the specified vPC domain for this track list. Apply this same configuration to the other vPC peer device. See the Cisco Nexus 9000 Series NX-OS Unicast Routing Configuration Guide for information about configuring object tracking and track lists.
| Note | This example uses Boolean OR in the track list and forces all traffic to the vPC peer device only for a complete module failure. If you want to trigger a switchover when any core interface or vPC Peer-Link goes down, use a Boolean AND in the torack list below. | 
To configure a track list to switch over a vPC to the remote peer when all related interfaces on a single module fail, follow these steps:
- 
                                    					
                                    Configure track objects on an interface (Layer 3 to core) and on a port channel (vPC Peer-Link). 
switch(config-if)# track 35 interface ethernet 8/35 line-protocol
switch(config-track)# track 23 interface ethernet 8/33 line-protocol
switch(config)# track 55 interface port-channel 100 line-protocol
- 
                                    					
                                    Create a track list that contains all the interfaces in the track list using the Boolean OR to trigger when all objects fail. 
switch(config)# track 44 list boolean OR
switch(config-track)# object 23
switch(config-track)# object 35
switch(config-track)# object 55
switch(config-track)# end
- 
                                    					
                                    Add this track object to the vPC domain: 
switch(config)# vpc domain 1
switch(config-vpc-domain)# track 44
- 
                                    					
                                    Display the track object: 
switch# show vpc brief
Legend:
(*) - local vPC is down, forwarding via vPC peer-link
vPC domain id : 1
Peer status : peer adjacency formed ok
vPC keep-alive status : peer is alive
Configuration consistency status: success
vPC role : secondary
Number of vPCs configured : 52
Track object : 44
vPC Peer-link status
---------------------------------------------------------------------
id Port Status Active vlans
-- ---- ------ --------------------------------------------------
1 Po100 up 1-5,140
vPC status
----------------------------------------------------------------------
id Port Status Consistency Reason Active vlans
-- ---- ------ ----------- -------------------------- ------------
1 Po1 up success success 1-5,140
This example shows how to display information about the track objects:
switch# show track brief
Track Type Instance Parameter State Last
Change
23 Interface Ethernet8/33 Line Protocol UP 00:03:05
35 Interface Ethernet8/35 Line Protocol UP 00:03:15
44 List ----- Boolean
or UP 00:01:19
55 Interface port-channel100 Line Protocol UP 00:00:34
vPC Interactions with Other Features
vPC Interactions with Other Features — concept overview.
vPC and LACP
LACP uses the system MAC address of the vPC domain to form the LACP Aggregation Group (LAG) ID for the vPC. (See the “Configuring Port Channels” chapter for information about LAG-ID and LACP.)
You can use LACP on all the vPC port channels, including those channels from the downstream device. We recommend that you configure LACP with active mode on the interfaces on each port channel on the vPC peer devices. This configuration allows you to more easily detect compatibility between devices, unidirectional links, and multihop connection, and provides dynamic reaction to run-time changes and link failures.
We recommend that you manually configure the system priority on the vPC Peer-Link devices to ensure that the vPC Peer-Link devices have a higher LACP priority than the downstream connected devices. A lower numerical value system priority means a higher LACP priority.
| Note | When manually configuring the system priority, you must ensure that you assign the same priority value on both vPC peer devices. If the vPC peer devices have different system priority values, vPC does not come up. | 
vPC Peer-Links and STP
Although vPCs provide a loop-free Layer 2 topology, STP is still required to provide a fail-safe mechanism to protect against any incorrect or defective cabling or possible misconfiguration. When you first bring up a vPC, STP reconverges. STP treats the vPC Peer-Link as a special link and always includes the vPC Peer-Link in the STP active topology.
We recommend that you set all the vPC Peer-Link interfaces to the STP network port type so that Bridge Assurance is automatically enabled on all vPC Peer-Links. We also recommend that you do not enable any of the STP enhancement features on vPC Peer-Links. If the STP enhancements are already configured, they do not cause any problems for the vPC Peer-Links..
When you are running both MST and Rapid PVST+, ensure that the PVST simulation feature is correctly configured.
See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for information about STP enhancement features and PVST simulation.
| Note | You must configure a list of parameters to be identical on the vPC peer devices on both sides of the vPC Peer-Link. See the “Compatibility Parameters for vPC Interfaces” section for information about these required matched settings. | 
STP is distributed; that is, the protocol continues running on both vPC peer devices. However, the configuration on the vPC peer device elected as the primary device controls the STP process for the vPC interfaces on the secondary vPC peer device.
The primary vPC device synchronizes the STP state on the vPC secondary peer device using Cisco Fabric Services over Ethernet (CFSoE). See the “vPC and Orphan Ports” section for information about CFSoE.
The STP process for vPC also relies on the periodic keepalive messages to determine when one of the connected devices on the vPC Peer-Link fails. See the “Peer-Keepalive Link and Messages” section for information about these messages.
The vPC manager performs a proposal/handshake agreement between the vPC peer devices that set the primary and secondary devices and coordinates the two devices for STP. The primary vPC peer device then controls the STP protocol on both the primary and secondary devices. We recommend that you configure the primary vPC peer device as the STP primary root device and configure the secondary VPC device to be the STP secondary root device.
If the primary vPC peer device fails over to the secondary vPC peer device, there is no change in the STP topology.
The BPDUs uses the MAC address set for the vPC for the STP bridge ID in the designated bridge ID field. The vPC primary device sends these BPDUs on the vPC interfaces.
STP Configuration Parameters
You must configure both ends of vPC Peer-Link with the identical STP configuration for the following parameters:
- 
                                       					
                                       STP global settings: 
  - 
                                             							
                                             STP mode
  - 
                                             							
                                             STP region configuration for MST
  - 
                                             							
                                             Enable/disable state per VLAN
  - 
                                             							
                                             Bridge Assurance setting
  - 
                                             							
                                             Port type setting
  - 
                                             							
                                             Loop Guard settings
- 
                                             							
                                             
- 
                                       					
                                       STP interface settings: 
  - 
                                             							
                                             Port type setting
  - 
                                             							
                                             Loop Guard
  - 
                                             							
                                             Root Guard
- 
                                             							
                                             
| Note | If any of these parameters are misconfigured, the Cisco NX-OS software suspends all interfaces in the vPC. Check the syslog and enter the show vpc brief command to see if the vPC interfaces are suspended. | 
Ensure that the following STP interface configurations are identical on both sides of the vPC Peer-Links or you may see unpredictable behavior in the traffic flow:
- 
                                       					
                                       BPDU Filter
- 
                                       					
                                       BPDU Guard
- 
                                       					
                                       Cost
- 
                                       					
                                       Link type
- 
                                       					
                                       Priority
- 
                                       					
                                       VLANs (PVRST+)
| Note | Display the configuration on both sides of the vPC Peer-Link to ensure that the settings are identical. | 
You can use the show spanning-tree command to display information about the vPC when that feature is enabled. See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for an example.
| Note | We recommend that you configure the ports on the downstream devices as STP edge ports. You should configure all host ports connected to a switch as STP edge ports. See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for more information about STP port types. | 
vPC Peer Switches
The vPC peer switch feature was added to Cisco NX-OS to address performance concerns around STP convergence. This feature allows a pair of Cisco Nexus 9000 Series devices to appear as a single STP root in the Layer 2 topology. This feature eliminates the need to pin the STP root to the vPC primary switch and improves vPC convergence if the vPC primary switch fails.
To avoid loops, the vPC Peer-Link is excluded from the STP computation. In vPC peer switch mode, STP BPDUs are sent from both vPC peer devices to avoid issues related to STP BPDU timeout on the downstream switches, which can cause traffic disruption.
This feature can be used with the pure peer switch topology in which the devices all belong to the vPC.
Additional Information
| Note | Peer-switch feature is supported on networks that use vPC and STP-based redundancy is not supported. If the vPC Peer-Link fail in a hybrid peer-switch configuration, you can lose traffic. In this scenario, the vPC peers use the same STP root ID as well as the same bridge ID. The access switch traffic is split in two with half going to the first vPC peer and the other half to the second vPC peer. With vPC Peer-Link failure, there is no impact to the north/south traffic but the east/west traffic is lost. | 
See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for information about STP enhancement features and Rapid PVST+.
vPC and ARP or ND
A feature was added to Cisco NX-OS to address table synchronization across vPC peers using the reliable transport mechanism of the Cisco Fabric Service over Ethernet (CFSoE) protocol. You must enable the ip arp synchronize and ipv6 nd synchronize commands to support faster convergence of address tables between the vPC peers. This convergence overcomes the delay that occurs in ARP table restoration for IPv4 or ND table restoration for IPv6 when the vPC Peer-Link port channel flaps or when a vPC peer comes back online.
vPC Multicast—PIM, IGMP, and IGMP Snooping
The Cisco NX-OS software for the Nexus 9000 Series devices supports the following on a vPC:
- 
                                       					
                                       PIM Any Source Multicast (ASM).
- 
                                       					
                                       					
                                       PIM Source-Specific Multicast (SSM) (7.0(3)I4(1) and later).
Multicast Synchronization and Traffic Flow
The software keeps the multicast forwarding state synchronized on both of the vPC peer devices. The IGMP snooping process on a vPC peer device shares the learned group information with the other vPC peer device through the vPC Peer-Link; the multicast states are always synchronized on both vPC peer devices. The PIM process in vPC mode ensures that only one of the vPC peer devices forwards the multicast traffic to the receivers.
Each vPC peer is a Layer 2 or Layer 3 device. Multicast traffic flows from only one of the vPC peer devices. You might see duplicate packets in the following scenarios:
- 
                                       					
                                       Orphan hosts
- 
                                       					
                                       When the source and receivers are in the Layer 2 vPC cloud in different VLANs with multicast routing enabled and a vPC member link goes down.
You might see negligible traffic loss in the following scenarios:
- 
                                       					
                                       When you reload the vPC peer device that is forwarding the traffic.
- 
                                       					
                                       When you restart PIM on the vPC peer device that is forwarding the traffic.
| Note | The Cisco NX-OS software does not support Bidirectional (BIDR) on a vPC. | 
Overall multicast convergence times are scale and vPC role change / PIM restart duration dependent.
Ensure that you dual-attach all Layer 3 devices to both vPC peer devices. If one vPC peer device goes down, the other vPC peer device continues to forward all multicast traffic normally.
The following outlines vPC PIM and vPC IGMP/IGMP snooping:
- 
                                       					
                                       vPC PIM—The PIM process in vPC mode ensures that only one vPC peer device forwards multicast traffic. The PIM process in vPC mode synchronizes the source state with both vPC peer devices and elects which vPC peer device forwards the traffic.
- 
                                       					
                                       vPC IGMP/IGMP snooping—The IGMP process in vPC mode synchronizes the designated router (DR) information on both vPC peer devices. Dual DRs are available for IGMP when you are in vPC mode. Dual DRs are not available when you are not in vPC mode, because both vPC peer devices maintain the multicast group information between the peers.
| Note | A PIM adjacency between a Switched Virtual Interface (SVI) on a vPC VLAN (a VLAN that is carried on a vPC Peer-Link) and a downstream device is not supported; this configuration can result in dropped multicast packets. If a PIM neighbor relationship is required with a downstream device, a physical Layer 3 interface must be used on the Nexus switches instead of a vPC SVI. For SVIs on vPC VLANs, only one PIM adjacency is supported, which is with the vPC peer switch. PIM adjacencies over the vPC Peer-Link with devices other than the vPC peer switch for the vPC-SVI are not supported. | 
You should enable or disable IGMP snooping identically on both vPC peer devices, and all the feature configurations should be identical. IGMP snooping is on by default.
| Note | The following commands are not supported in vPC mode:  | 
See the Cisco Nexus 9000 Series NX-OS Multicast Routing Configuration Guide for more information about multicasting.
Multicast PIM Dual DRs
By default, a multicast router sends PIM joins upstream only if it has interested receivers. These interested receivers can either be IGMP hosts (they communicate through IGMP reports) or other multicast routers (they communicate through PIM joins).
In the Cisco NX-OS vPC implementation, PIM works in dual designated router (DR) mode. That is, if a vPC device is a DR on a vPC SVI outgoing interface (OIF), its peer automatically assumes the proxy DR role. IGMP adds an OIF (the report is learned on that OIF) to the forwarding if the OIF is a DR. With dual DRs, both vPC devices have an identical (*,G) entry with respect to the vPC SVI OIFs as shown in this example:
VPC Device1:
------------
(*,G)
oif1 (igmp)
VPC Device2:
------------
(*,G)
oif1 (igmp)
IP PIM Pre-Build SPTs
When the multicast source is in a Layer 3 cloud (outside the vPC domain), one vPC peer is elected as the forwarder for the source. This forwarder election is based on the metrics to reach the source. If there is a tie, the vPC primary is chosen as the forwarder. Only the forwarder has the vPC OIFs in its associated (S,G) and the nonforwarder (S,G) has 0 OIFs. Therefore, only the forwarder sends PIM (S,G) joins toward the source as shown in this example:
VPC Device1 (say this is Forwarder for Source 'S'):
------------
(*,G)
oif1 (igmp)
(S,G)
oif1 (mrib)
VPC Device2:
------------
(*,G)
oif1 (igmp)
(S,G)
NULL
In the case of a failure (for example, a Layer 3 Reverse Path Forwarding (RPF) link on the forwarder becomes inoperable or the forwarder gets reloaded), if the current nonforwarder ends up becoming the forwarder, it has to start sending PIM joins for (S,G) toward the source to pull the traffic. Depending upon the number of hops to reach the source, this operation might take some time (PIM is a hop-by-hop protocol).
To eliminate this issue and get better convergence, use the ip pim pre-build-spt command. This command enables PIM send joins even if the multicast route has 0 OIFs. In a vPC device, the nonforwarder sends PIM (S,G) joins upstream toward the source. The downside is that the link bandwidth upstream from the nonforwarder gets used for the traffic that is ultimately dropped by it. The benefits that result with better convergence far outweigh the link bandwidth usage. Therefore, we recommend that you use this command if you use vPCs.
vPC Peer-Links and Routing
The First Hop Redundancy Protocols (FHRPs) interoperate with vPCs. The Hot Standby Routing Protocol (HSRP), and Virtual Router Redundancy Protocol (VRRP) all interoperate with vPCs. We recommend that you dual-attach all Layer 3 devices to both vPC peer devices.
The primary FHRP device responds to ARP requests, even though the secondary vPC device forwards the data traffic.
To simplify initial configuration verification and vPC/HSRP troubleshooting, you can configure the primary vPC peer device with the FHRP active router highest priority.
In addition, you can use the priority command in the if-hsrp configuration mode to configure failover thresholds for when a group state enabled on a vPC Peer-Link is in standby or in listen state. You can configure lower and upper thresholds to prevent the interface from going up and down.
VRRP acts similarly to HSRP when running on vPC peer devices. You should configure VRRP the same way that you configure HSRP.
When the primary vPC peer device fails over to the secondary vPC peer device, the FHRP traffic continues to flow seamlessly.
We recommend that you configure routing adjacency between the two vPC peer devices to act as a backup routing path. If one vPC peer device loses Layer 3 uplinks, the vPC can redirect the routed traffic to the other vPC peer device and leverage its active Layer 3 uplinks.
You can configure the inter-switch link for a backup routing path in the following ways:
- 
                                       					
                                       Create a Layer 3 link between the two vPC peer devices.
- 
                                       					
                                       Use the non-VPC VLAN trunk with a dedicated VLAN interface.
- 
                                       					
                                       Use a vPC Peer-Link with a dedicated VLAN interface.
We do not recommend that you configure the burnt-in MAC address option (use-bia) for HSRP or manually configure virtual MAC addresses for any FHRP protocol in a vPC environment because these configurations can adversely affect vPC load balancing. The HSRP use-bia option is not supported on vPCs. When you are configuring custom MAC addresses, you must configure the same MAC address on both vPC peer devices.
You can use the delay restore command to configure a restore timer that delays the vPC coming back up until after the peer adjacency forms and the VLAN interfaces are back up. This feature enables you to avoid packet drops when the routing tables might not be converged before the vPC is once again passing traffic. Use the delay restore command to configure this feature.
To delay the VLAN interfaces on the restored vPC peer device from coming up, use the interfaces-vlan option of the delay restore command.
See the Cisco Nexus 9000 Series NX-OS Unicast Routing Configuration Guide for more information about FHRPs and routing.
CFSoE
The Cisco Fabric Services over Ethernet (CFSoE) is a reliable state transport mechanism that is used to synchronize the actions of the vPC peer devices. CFSoE carries messages and packets for many features linked with vPC, such as STP and IGMP. Information is carried in CFS/CFSoE protocol data units (PDUs).
When you enable the vPC feature, the device automatically enables CFSoE, and you do not have to configure anything. CFSoE distributions for vPCs do not need the capabilities to distribute over IP or the CFS regions. You do not need to configure anything for the CFSoE feature to work correctly on vPCs.
The CFSoE transport is local to each VDC.
Reference Information
You can use the show mac address-table command to display the MAC addresses that CFSoE synchronizes for the vPC Peer-Link.
| Note | Do not enter the no cfs eth distribute or the no cfs distribute command. You must enable CFSoE for vPC functionality. If you do enter either of these commands with vPC enabled, the system displays an error message. | 
When you enter the show cfs application command, the output displays “Physical-eth,” which shows the applications that are using CFSoE.
CFS also transports data over TCP/IP. See the Cisco Nexus 9000 Series NX-OS System Management Configuration Guide for more information about CFS over IP.
| Note | The software does not support CFS regions. | 
vPC and Orphan Ports
When a device that is not vPC-capable connects to each peer, the connected ports are known as orphan ports because they are not members of a vPC. The device’s link to one peer will be active (forwarding) and the other link will be standby (blocking) due to STP.
If a vPC Peer-Link failure or restoration occurs, an orphan port’s connectivity might be bound to the vPC failure or restoration process. For example, if a device’s active orphan port connects to the secondary vPC peer, the device loses any connections through the primary peer if a vPC Peer-Link failure occurs and the vPC ports are suspended by the secondary peer. If the secondary peer were to also suspend the active orphan port, the device’s standby port becomes active, provides a connection to the primary peer, and restores connectivity. You can configure in the CLI that specific orphan ports are suspended by the secondary peer when it suspends its vPC ports and are restored when the vPC is restored.
Virtualization Supports
All ports in a given vPC must be in the same VDC. This version of the software supports only one vPC domain per VDC. You can use the numbers from 1 to 4096 in each VDC to number the vPC.
vPC Recovery After an Outage
In a data center outage, both the vPC peer in vPC domain get reloaded. Occasionally only one peer can be restored. With no functioning peer-keepalive or vPC Peer-Link, the vPC cannot function normally, a method might be available to allow vPC services to use only the local ports of the functional peer.
Autorecoveries
You can configure the Cisco Nexus 9000 Series device to restore vPC services when its peer fails to come online by using the auto-recovery command. You must save this setting in the startup configuration. On reload, if the vPC Peer-Link is down and three consecutive peer-keepalive messages are lost, the secondary device assumes the primary STP role and the primary LACP role. The software reinitializes the vPCs, bringing up its local ports. Because there are no peers, the consistency check is bypassed for the local vPC ports. The device elects itself to be the STP primary regardless of its role priority and also acts as the primary device for LACP port roles.
vPC Peer Roles After a Recovery
When the other peer device completes its reload and adjacency forms, the following process occurs:
- 
                                       					
                                       The first vPC peer maintains its current role to avoid any transition reset to other protocols. The peer accepts the other available role.
- 
                                       					
                                       When an adjacency forms, consistency checks are performed and appropriate actions are taken.
High Availability
During an In-Service Software Upgrade (ISSU), the software reload process on the first vPC device locks its vPC peer device by using CFS messaging over the vPC communications channel. Only one device at a time is upgraded. When the first device completes its upgrade, it unlocks its peer device. The second device then performs the upgrade process, locking the first device as it does so. During the upgrade, the two vPC devices temporarily run different releases of Cisco NX-OS, however the system functions correctly because of its backward compatibility support.
| Note | See the Cisco Nexus 9000 Series NX-OS High Availability and Redundancy Guide for complete information about high-availability features. | 
vPC Forklift Upgrade Scenario
The following procedure describes a scenario of migrating a pair of Cisco Nexus 9500 switches in a vPC domain to a different pair of Cisco Nexus 9500 switches with the same type of line cards. Migrating from Cisco Nexus 9504 switches to Cisco Nexus 9508 switches for the need of more interfaces is a typical example of such migration. The following migration scenarios are not supported:
- 
                                       						
                                       Migration of Cisco Nexus 9500 switches with a different set of line cards. For example, from a Cisco Nexus 9500 switches with N9K-X94xx line card to Cisco Nexus 9500 switches with N9K-X97xx line card.
- 
                                       						
                                       Migration between different generations of Cisco Nexus 9300 switches. For example, migration from Cisco Nexus N9K-C9372PX to Cisco Nexus N9K-93180YC-EX switches.
- 
                                       						
                                       Having different generations of Cisco Nexus 9000 switches in a vPC domain is not supported.
Considerations for a vPC forklift upgrade:
- 
                                       						
                                       vPC Role Election and Sticky-bit By default, the Cisco NX-OS software uses the lowest MAC address to elect the primary device. However, if the role priority is set, then the device with the lowest priority will be elected as the primary device. When the primary device is reloaded, the system comes back online and connectivity to the vPC secondary device (now the operational primary) is restored. The operational role of the secondary device (operational primary) does not change (to avoid unnecessary disruptions). This behavior is achieved with a sticky-bit, where the sticky information is not saved in the startup configuration. This method makes the device that is up and running win over the reloaded device. Hence, the vPC primary becomes the vPC operational secondary. Sticky-bit is also set when a vPC node comes up with vPC Peer-Link and peer-keepalive down and it becomes primary after the auto recovery period.
- 
                                       						
                                       vPC Delay Restore The delay restore timer is used to delay the vPC from coming up on the restored vPC peer device after a reload when the peer adjacency is already established. To delay the VLAN interfaces on the restored vPC peer device from coming up, use the interfaces-vlan option of the delay restore command.
- 
                                       						
                                       vPC Auto-Recovery During a data center power outage when both vPC peer switches go down, if only one switch is restored, the auto-recovery feature allows that switch to assume the role of the primary switch and the vPC links come up after the auto-recovery time period. The default auto-recovery period is 240 seconds.
| Table 1. Migration Steps and Expected Behavior |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
|  | Migration Step | Expected Behavior | Node1 Configured Role (Ex: role priority 100) | Node1 Operational Role | Node2 Configured Role (Ex: role priority 200) | Node2 Operational Role | 
|---|---|---|---|---|---|---|
| 1 | Initial state | Traffic is forwarded by both vPC peers – Node1 and Node2. Node1 is primary and Node2 is secondary. | primary | Primary Sticky bit: False | secondary | Secondary Sticky bit: False | 
| 2 | Node2 replacement – Shut all vPCs and uplinks on Node2. vPC Peer-Link and vPC peer-keepalive are in administrative up state. | Traffic converged on Primary vPC peer Node1. | primary | Primary Sticky bit: False | secondary | Secondary Sticky bit: False | 
| 3 | Remove Node2. | Node1 will continue to forward traffic. | primary | Primary Sticky bit: False | n/a | n/a | 
| 4 | Configure New_Node2. Copy the configuration to startup config. vPC Peer-Link and peer-keepalive in administrative up state. Power off New_Node2. Make all connections. Power on New_Node2. | New_Node2 will come up as secondary. Node1 continue to be primary. Traffic will continue to be forwarded on Node1. | primary | Primary Sticky bit: False | secondary | Secondary Sticky bit: False | 
| 5 | Bring up all vPCs and uplink ports on New_Node2. | Traffic will be forwarded by both Node1 and New_Node2. | primary | Primary Sticky bit: False | secondary | Secondary Sticky bit: False | 
| 6 | Node1 replacement - Shut vPCs and uplinks on Node1. | Traffic will converge on New_Node2. | primary | Primary Sticky bit: False | secondary | Secondary Sticky bit: False | 
| 7 | Remove Node1. | New_Node2 will become secondary, operational primary and sticky bit will be set to True. | n/a | n/a | secondary | Primary Sticky bit: True | 
| 8 | Configure New_Node1. Copy running to startup. Power off the new Node1. Make all connections. Power on New_Node1. | New_Node1 will come up as primary, operational secondary. | primary | Secondary Sticky bit: False | secondary | Primary Sticky bit: True | 
| 9 | Bring up all vPCs and uplink ports on New_Node1. | Traffic will be forwarded by both New_Node1 and New_Node2. | primary | Secondary Sticky bit: False | secondary | Primary Sticky bit: True | 
| Note | If you prefer to have the configured secondary node as the operational secondary and the configured primary as the operational primary, then Node2 can be reloaded at the end of the migration. This is optional and does not have any functional impact. |