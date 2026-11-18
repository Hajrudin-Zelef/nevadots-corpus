Information About Rapid PVST+
| Note | See the Cisco Nexus 9000 Series NX-OS Interfaces Configuration Guide, for information on creating Layer 2 interfaces. | 
The Spanning Tree Protocol (STP) was implemented to provide a loop-free network at Layer 2 of the network. Rapid PVST+ is an updated implementation of STP that allows you to create one spanning tree topology for each VLAN. Rapid PVST+ is the default STP mode on the device.
| Note | Spanning tree is used to refer to IEEE 802.1w and IEEE 802.1s. If the IEEE 802.1D Spanning Tree Protocol is discussed in this publication, then 802.1D is stated specifically. | 
| Note | Rapid PVST+ is the default STP mode. | 
The Rapid PVST+ protocol is the IEEE 802.1w standard, Rapid Spanning Tree Protocol (RSTP), implemented on a per VLAN basis. Rapid PVST+ interoperates with the IEEE 802.1Q VLAN standard, which mandates a single STP instance for all VLANs, rather than per VLAN.
Rapid PVST+ is enabled by default on the default VLAN (VLAN1) and on all newly created VLANs on the device. Rapid PVST+ interoperates with devices that run legacy IEEE 802.1D STP.
RSTP is an improvement on the original STP standard, 802.1D, which allows faster convergence.
| Note | The device supports full nondisruptive upgrades for Rapid PVST+. See the Cisco Nexus 9000 Series NX-OS High Availability and Redundancy Guide, for complete information on nondisruptive upgrades. | 
STP
STP is a Layer 2 link-management protocol that provides path redundancy while preventing loops in the network.
Overview of STP
In order for a Layer 2 Ethernet network to function properly, only one active path can exist between any two stations. STP operation is transparent to end stations, which cannot detect whether they are connected to a single LAN segment or a switched LAN of multiple segments.
When you create fault-tolerant internetworks, you must have a loop-free path between all nodes in a network. The STP algorithm calculates the best loop-free path throughout a switched Layer 2 network. Layer 2 LAN ports send and receive STP frames, which are called Bridge Protocol Data Units (BPDUs), at regular intervals. Network devices do not forward these frames but use the frames to construct a loop-free path.
Multiple active paths between end stations cause loops in the network. If a loop exists in the network, end stations might receive duplicate messages and network devices might learn end station MAC addresses on multiple Layer 2 LAN ports.
STP defines a tree with a root bridge and a loop-free path from the root to all network devices in the Layer 2 network. STP forces redundant data paths into a blocked state. If a network segment in the spanning tree fails and a redundant path exists, the STP algorithm recalculates the spanning tree topology and activates the blocked path.
When two Layer 2 LAN ports on a network device are part of a loop, the STP port priority and port path-cost setting determine which port on the device is put in the forwarding state and which port is put in the blocking state. The STP port priority value is the efficiency with which that location allows the port to pass traffic. The STP port path-cost value is derived from the media speed.
How a Topology is Created
All devices in a LAN that participate in a spanning tree gather information about other switches in the network by exchanging BPDUs. This exchange of BPDUs results in the following actions:
-  
                                       			 
                                       The system elects a unique root switch for the spanning tree network topology.
-  
                                       			 
                                       The system elects a designated switch for each LAN segment.
-  
                                       			 
                                       The system eliminates any loops in the switched network by placing redundant switch ports in a backup state; all paths that are not needed to reach the root device from anywhere in the switched network are placed in an STP-blocked state.
The topology on an active switched network is determined by the following:
-  
                                       			 
                                       The unique device identifier Media Access Control (MAC) address of the device that is associated with each device
-  
                                       			 
                                       The path cost to the root that is associated with each switch port
-  
                                       			 
                                       The port identifier that is associated with each switch port
In a switched network, the root switch is the logical center of the spanning tree topology. STP uses BPDUs to elect the root switch and root port for the switched network.
| Note | The mac-address bpdu source version 2 command enables STP to use the new Cisco MAC address (00:26:0b:xx:xx:xx) as the source address of BPDUs generated on vPC ports. To apply this command, you must have identical configurations for both vPC peer switches or peers. Cisco strongly recommends that you disable ether channel guard on the edge devices before issuing this command to minimize traffic disruption from STP inconsistencies. Re-enable the ether channel guard after updating on both peers. | 
Bridge ID
Each VLAN on each network device has a unique 64-bit bridge ID that consists of a bridge priority value, an extended system ID (IEEE 802.1t), and an STP MAC address allocation.
Bridge Priority Value
The bridge priority is a 4-bit value when the extended system ID is enabled.
You can only specify a device bridge ID (used by the spanning tree algorithm to determine the identity of the root bridge; the lowest number is preferred) as a multiple of 4096.
| Note | In this device, the extended system ID is always enabled; you cannot disable the extended system ID. | 
Extended System ID
The device always uses the 12-bit extended system ID.
This table shows how the system ID extension combined with the bridge ID functions as the unique identifier for a VLAN.
| Table 1. Bridge Priority Value and Extended System ID                                              with the Extended System ID Enabled |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Bridge Priority Value |  |  |  |  | Extended System ID (Set Equal to the VLAN ID) |  |  |  |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Bit 16 | Bit 15 | Bit 14 | Bit 13 |  | Bit 12 | Bit 11 | Bit 10 | Bit 9 | Bit 8 | Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | 
| 32768 | 16384 | 8192 | 4096 |  | 2048 | 1024 | 512 | 256 | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | 
STP MAC Address Allocation
| Note | MAC address reduction is always enabled on the device. | 
Because MAC address reduction is always enabled on the device, you should also enable MAC address reduction on all other Layer 2 connected network devices to avoid undesirable root bridge election and spanning tree topology issues.
When MAC address reduction is enabled, the root bridge priority becomes a multiple of 4096 plus the VLAN ID. You can only specify a device bridge ID (used by the spanning tree algorithm to determine the identity of the root bridge; the lowest number is preferred) as a multiple of 4096. Only the following values are possible:
-  
                                          			 
                                          0
-  
                                          			 
                                          4096
-  
                                          			 
                                          8192
-  
                                          			 
                                          12288
-  
                                          			 
                                          16384
-  
                                          			 
                                          20480
-  
                                          			 
                                          24576
-  
                                          			 
                                          28672
-  
                                          			 
                                          32768
-  
                                          			 
                                          36864
-  
                                          			 
                                          40960
-  
                                          			 
                                          45056
-  
                                          			 
                                          49152
-  
                                          			 
                                          53248
-  
                                          			 
                                          57344
-  
                                          			 
                                          61440
STP uses the extended system ID plus a MAC address to make the bridge ID unique for each VLAN.
| Note | If another bridge in the same spanning tree domain does not run the MAC address reduction feature, it could win the root bridge ownership because of the finer granularity in the selection of its bridge ID. | 
BPDUs
Network devices transmit BPDUs throughout the STP instance. Each network device sends configuration BPDUs to communicate and compute the spanning tree topology. Each configuration BPDU contains the following minimal information:
-  
                                       			 
                                       The unique bridge ID of the network device that the transmitting network device believes to be the root bridge
-  
                                       			 
                                       The STP path cost to the root
-  
                                       			 
                                       The bridge ID of the transmitting bridge
-  
                                       			 
                                       The message age
-  
                                       			 
                                       The identifier of the transmitting port
-  
                                       			 
                                       Values for the hello, forward delay, and max-age protocol timer
-  
                                       			 
                                       Additional information for STP extension protocols
When a network device transmits a Rapid PVST+ BPDU frame, all network devices connected to the VLAN on which the frame is transmitted receive the BPDU. When a network device receives a BPDU, it does not forward the frame but instead uses the information in the frame to calculate a BPDU. If the topology changes, the device initiates a BPDU exchange.
A BPDU exchange results in the following:
-  
                                       			 
                                       One network device is elected as the root bridge.
-  
                                       			 
                                       The shortest distance to the root bridge is calculated for each network device based on the path cost.
-  
                                       			 
                                       A designated bridge for each LAN segment is selected. This network device is closest to the root bridge through which frames are forwarded to the root.
-  
                                       			 
                                       A root port is elected. This port provides the best path from the bridge to the root bridge.
-  
                                       			 
                                       Ports included in the spanning tree are selected.
Election of the Root Bridge
For each VLAN, the network device with the lowest numerical ID is elected as the root bridge. If all network devices are configured with the default priority (32768), the network device with the lowest MAC address in the VLAN becomes the root bridge. The bridge priority value occupies the most significant bits of the bridge ID.
When you change the bridge priority value, you change the probability that the device will be elected as the root bridge. Configuring a lower value increases the probability; a higher value decreases the probability.
The STP root bridge is the logical center of each spanning tree topology in a Layer 2 network. All paths that are not needed to reach the root bridge from anywhere in the Layer 2 network are placed in STP blocking mode.
BPDUs contain information about the transmitting bridge and its ports, including bridge and MAC addresses, bridge priority, port priority, and path cost. STP uses this information to elect the root bridge for the STP instance, to elect the root port that leads to the root bridge, and to determine the designated port for each Layer 2 segment.
Creating the Spanning Tree Topology
By lowering the numerical value of the ideal network device so that it becomes the root bridge, you force an STP recalculation to form a new spanning tree topology with the ideal network device as the root.
When the spanning tree topology is calculated based on default parameters, the path between the source and destination end stations in a switched network might not be ideal. For instance, connecting higher-speed links to a port that has a higher number than the current root port can cause a root-port change. The goal is to make the fastest link the root port.
For example, assume that one port on switch B is a fiber-optic link, and another port on switch B (an unshielded twisted-pair [UTP] link) is the root port. Network traffic might be more efficient over the high-speed fiber-optic link. By changing the STP port priority on the fiber-optic port to a higher priority (lower numerical value) than the root port, the fiber-optic port becomes the new root port.
Rapid PVST+
Rapid PVST+ is the default spanning tree mode for the software and is enabled by default on the default VLAN and all newly created VLANs.
A single instance, or topology, of RSTP runs on each configured VLAN, and each Rapid PVST+ instance on a VLAN has a single root device. You can enable and disable STP on a per-VLAN basis when you are running Rapid PVST+.
Overview of Rapid PVST+
Rapid PVST+ is the IEEE 802.1w (RSTP) standard implemented per VLAN. A single instance of STP runs on each configured VLAN (if you do not manually disable STP). Each Rapid PVST+ instance on a VLAN has a single root switch. You can enable and disable STP on a per-VLAN basis when you are running Rapid PVST+.
| Note | Rapid PVST+ is the default STP mode for the device. | 
Rapid PVST+ uses point-to-point wiring to provide rapid convergence of the spanning tree. The spanning tree reconfiguration can occur in less than 1 second with Rapid PVST+ (in contrast to 50 seconds with the default settings in the 802.1D STP). The device automatically checks the PVID.
| Note | Rapid PVST+ supports one STP instance for each VLAN. | 
Using Rapid PVST+, STP convergence occurs rapidly. By default, each designated port in the STP sends out a BPDU every 2 seconds. On a designated port in the topology, if hello messages are missed three consecutive times, or if the maximum age expires, the port immediately flushes all protocol information in the table. A port considers that it loses connectivity to its direct neighbor designated port if it misses three BPDUs or if the maximum age expires. This rapid aging of the protocol information allows quick failure detection.
Rapid PVST+ provides for rapid recovery of connectivity following the failure of a device, a device port, or a LAN. It provides rapid convergence for edge ports, new root ports, and ports connected through point-to-point links as follows:
-  
                                       			 
                                       Edge ports—When you configure a port as an edge port on an RSTP device, the edge port immediately transitions to the forwarding state. (This immediate transition was previously a Cisco-proprietary feature named PortFast.) You should only configure ports that connect to a single end station as edge ports. Edge ports do not generate topology changes when the link changes.
Enter the spanning-tree port type interface configuration command to configure a port as an STP edge port.
| Note | We recommend that you configure all ports connected to a Layer 2 host as edge ports. | 
-  
                                       			 
                                       Root port—If Rapid PVST+ selects a new root port, it blocks the old root port and immediately transitions the new root port to the forwarding state.
-  
                                       			 
                                       Point-to-point links—If you connect a port to another port through a point-to-point link and the local port becomes a designated port, it negotiates a rapid transition with the other port by using the proposal-agreement handshake to ensure a loop-free topology.
Rapid PVST+ achieves rapid transition to the forwarding state only on edge ports and point-to-point links. Although the link type is configurable, the system automatically derives the link type information from the duplex setting of the port. Full-duplex ports are assumed to be point-to-point ports, while half-duplex ports are assumed to be shared ports.
Edge ports do not generate topology changes, but all other designated and root ports generate a topology change (TC) BPDU when they either fail to receive three consecutive BPDUs from the directly connected neighbor or the maximum age times out. At this point, the designated or root port sends a BPDU with the TC flag set. The BPDUs continue to set the TC flag as long as the TC While timer runs on that port. The value of the TC While timer is the value set for the hello time plus 1 second. The initial detector of the topology change immediately floods this information throughout the entire topology.
When Rapid PVST+ detects a topology change, the protocol does the following:
-  
                                       			 
                                       Starts the TC While timer with a value equal to twice the hello time for all the nonedge root and designated ports, if necessary.
-  
                                       			 
                                       Flushes the MAC addresses associated with all these ports.
The topology change notification floods quickly across the entire topology. The system flushes dynamic entries immediately on a per-port basis when it receives a topology change.
| Note | The TCA flag is used only when the device is interacting with devices that are running legacy 802.1D STP. | 
The proposal and agreement sequence then quickly propagates toward the edge of the network and quickly restores connectivity after a topology change.
Rapid PVST+ BPDUs
Rapid PVST+ and 802.1w use all six bits of the flag byte to add the following:
-  
                                       			 
                                       The role and state of the port that originates the BPDU
-  
                                       			 
                                       The proposal and agreement handshake
Another important change is that the Rapid PVST+ BPDU is type 2, version 2, which makes it possible for the device to detect connected legacy (802.1D) bridges. The BPDU for 802.1D is type 0, version 0.
Proposal and Agreement Handshake
The switch learns the link type from the port duplex mode; a full-duplex port is considered to have a point-to-point connection and a half-duplex port is considered to have a shared connection. You can override the default setting that is controlled by the duplex setting by entering the spanning-tree link-type interface configuration command.
This proposal/agreement handshake is initiated only when a nonedge port moves from the blocking to the forwarding state. The handshaking process then proliferates step-by-step throughout the topology.
Protocol Timers
This table describes the protocol timers that affect the Rapid PVST+ performance.
| Table 2. Rapid PVST+                                           		  Protocol Timers |  | 
|---|---|
| Variable | Description | 
|---|---|
| Hello timer | Determines how often each device broadcasts BPDUs to other network devices. The default is 2 seconds, and the range is from 1 to 10. | 
| Forward delay timer | Determines how long each of the listening and learning states last before the port begins forwarding. This timer is generally not used by the protocol, but it is used when interoperating with the 802.1D spanning tree. The default is 15 seconds, and the range is from 4 to 30 seconds. | 
| Maximum age timer | Determines the amount of time that protocol information received on a port is stored by the network device. This timer is generally not used by the protocol, but it is used when interoperating with the 802.1D spanning tree. The default is 20 seconds; the range is from 6 to 40 seconds. | 
Port Roles
Rapid PVST+ provides rapid convergence of the spanning tree by assigning port roles and learning the active topology. Rapid PVST+ builds upon the 802.1D STP to select the device with the highest switch priority (lowest numerical priority value) as the root bridge. Rapid PVST+ assigns one of these port roles to individual ports:
- 
                                       					
                                       Root port—Provides the best path (lowest cost) when the device forwards packets to the root bridge.
- 
                                       					
                                       Designated port—Connects to the designated device that has the lowest path cost when forwarding packets from that LAN to the root bridge. The port through which the designated device is attached to the LAN is called the designated port.
- 
                                       					
                                       Alternate port—Offers an alternate path toward the root bridge to the path provided by the current root port. An alternate port provides a path to another device in the topology.
- 
                                       					
                                       Backup port—Acts as a backup for the path provided by a designated port toward the leaves of the spanning tree. A backup port can exist only when two ports are connected in a loopback by a point-to-point link or when a device has two or more connections to a shared LAN segment. A backup port provides another path in the topology to the device.
- 
                                       					
                                       Disabled port—Has no role within the operation of the spanning tree.
In a stable topology with consistent port roles throughout the network, Rapid PVST+ ensures that every root port and designated port immediately transition to the forwarding state while all alternate and backup ports are always in the blocking state. Designated ports start in the blocking state. The port state controls the operation of the forwarding and learning processes.
Rapid PVST+ Port State Overview
Propagation delays can occur when protocol information passes through a switched LAN. As a result, topology changes can take place at different times and at different places in a switched network. When a Layer 2 LAN port transitions directly from nonparticipation in the spanning tree topology to the forwarding state, it can create temporary data loops. Ports must wait for new topology information to propagate through the switched LAN before starting to forward frames.
Each Layer 2 LAN port on the device that uses Rapid PVST+ or MST exists in one of the following four states:
-  
                                       			 
                                       Blocking—The Layer 2 LAN port does not participate in frame forwarding.
-  
                                       			 
                                       Learning—The Layer 2 LAN port prepares to participate in frame forwarding.
-  
                                       			 
                                       Forwarding—The Layer 2 LAN port forwards frames.
-  
                                       			 
                                       Disabled—The Layer 2 LAN port does not participate in STP and is not forwarding frames.
When you enable Rapid PVST+, every port in the device, VLAN, and network goes through the blocking state and the transitory states of learning at power up. If properly configured, each Layer 2 LAN port stabilizes to the forwarding or blocking state.
When the STP algorithm places a Layer 2 LAN port in the forwarding state, the following process occurs:
-  
                                       			 
                                       The Layer 2 LAN port is put into the blocking state while it waits for protocol information that suggests it should go to the learning state.
-  
                                       			 
                                       The Layer 2 LAN port waits for the forward delay timer to expire, moves the Layer 2 LAN port to the learning state, and restarts the forward delay timer.
-  
                                       			 
                                       In the learning state, the Layer 2 LAN port continues to block frame forwarding as it learns the end station location information for the forwarding database.
-  
                                       			 
                                       The Layer 2 LAN port waits for the forward delay timer to expire and then moves the Layer 2 LAN port to the forwarding state, where both learning and frame forwarding are enabled.
Blocking State
A Layer 2 LAN port in the blocking state does not participate in frame forwarding.
A Layer 2 LAN port in the blocking state performs as follows:
- 
                                          
                                          
                                          
                                          Discards frames received from the attached segment.
- 
                                          
                                          
                                          
                                          Discards frames switched from another port for forwarding.
- 
                                          
                                          
                                          
                                          Does not incorporate the end station location into its address database. (There is no learning on a blocking Layer 2 LAN port, so there is no address database update.)
- 
                                          
                                          
                                          
                                          Receives BPDUs and directs them to the system module.
- 
                                          
                                          
                                          
                                          Receives, processes, and transmits BPDUs received from the system module.
- 
                                          
                                          
                                          
                                          Receives and responds to control plane messages.
Learning State
A Layer 2 LAN port in the learning state prepares to participate in frame forwarding by learning the MAC addresses for the frames. The Layer 2 LAN port enters the learning state from the blocking state.
A Layer 2 LAN port in the learning state performs as follows:
- 
                                          
                                          
                                          
                                          Discards frames received from the attached segment.
- 
                                          
                                          
                                          
                                          Discards frames switched from another port for forwarding.
- 
                                          
                                          
                                          
                                          Incorporates the end station location into its address database.
- 
                                          
                                          
                                          
                                          Receives BPDUs and directs them to the system module.
- 
                                          
                                          
                                          
                                          Receives, processes, and transmits BPDUs received from the system module.
- 
                                          
                                          
                                          
                                          Receives and responds to control plane messages.
Forwarding State
A Layer 2 LAN port in the forwarding state forwards frames. The Layer 2 LAN port enters the forwarding state from the learning state.
A Layer 2 LAN port in the forwarding state performs as follows:
- 
                                          
                                          
                                          
                                          Forwards frames received from the attached segment.
- 
                                          
                                          
                                          
                                          Forwards frames switched from another port for forwarding.
- 
                                          
                                          
                                          
                                          Incorporates the end station location information into its address database.
- 
                                          
                                          
                                          
                                          Receives BPDUs and directs them to the system module.
- 
                                          
                                          
                                          
                                          Processes BPDUs received from the system module.
- 
                                          
                                          
                                          
                                          Receives and responds to control plane messages.
Disabled State
A Layer 2 LAN port in the disabled state does not participate in frame forwarding or STP. A Layer 2 LAN port in the disabled state is virtually nonoperational.
A disabled Layer 2 LAN port performs as follows:
- 
                                          
                                          
                                          
                                          Discards frames received from the attached segment.
- 
                                          
                                          
                                          
                                          Discards frames switched from another port for forwarding.
- 
                                          
                                          
                                          
                                          Does not incorporate the end station location into its address database. (There is no learning, so there is no address database update.)
- 
                                          
                                          
                                          
                                          Does not receive BPDUs from neighbors.
- 
                                          
                                          
                                          
                                          Does not receive BPDUs for transmission from the system module.
Summary of Port States
This table lists the possible operational and Rapid PVST+ states for ports and whether the port is included in the active topology.
| Table 3.                                                                                                                                                                                                                                                                                     Port State Active Topology |  |  | 
|---|---|---|
| Operational Status | Port State | Is Port Included in the Active Topology? | 
|---|---|---|
| Enabled | Blocking | No | 
| Enabled | Learning | Yes | 
| Enabled | Forwarding | Yes | 
| Disabled | Disabled | No | 
Synchronization of Port Roles
When the device receives a proposal message on one of its ports and that port is selected as the new root port, Rapid PVST+ forces all other ports to synchronize with the new root information.
The device is synchronized with superior root information received on the root port if all other ports are synchronized. An individual port on the device is synchronized if either of the following applies:
-  
                                       			 
                                       That port is in the blocking state.
-  
                                       			 
                                       It is an edge port (a port configured to be at the edge of the network).
If a designated port is in the forwarding state and is not configured as an edge port, it transitions to the blocking state when the Rapid PVST+ forces it to synchronize with new root information. In general, when the Rapid PVST+ forces a port to synchronize with root information and the port does not satisfy any of the above conditions, its port state is set to blocking.
After ensuring that all of the ports are synchronized, the device sends an agreement message to the designated device that corresponds to its root port. When the devices connected by a point-to-point link are in agreement about their port roles, Rapid PVST+ immediately transitions the port states to the forwarding state.
Processing Superior BPDU Information
A superior BPDU is a BPDU with root information (such as a lower switch ID or lower path cost) that is superior to what is currently stored for the port.
If a port receives a superior BPDU, Rapid PVST+ triggers a reconfiguration. If the port is proposed and is selected as the new root port, Rapid PVST+ forces all the designated, nonedge ports to synchronize.
If the received BPDU is a Rapid PVST+ BPDU with the proposal flag set, the device sends an agreement message after all of the other ports are synchronized. The new root port transitions to the forwarding state as soon as the previous port reaches the blocking state.
If the superior information received on the port causes the port to become a backup port or an alternate port, Rapid PVST+ sets the port to the blocking state and sends an agreement message. The designated port continues sending BPDUs with the proposal flag set until the forward-delay timer expires. At that time, the port transitions to the forwarding state.
Processing Inferior BPDU Information
An inferior BPDU is a BPDU with root information (such as a higher switch ID or higher path cost) that is inferior to what is currently stored for the port.
If a designated port receives an inferior BPDU, it immediately replies with its own information.
Detecting Unidirectional Link Failure:Rapid PVST+
The software checks the consistency of the port role and state in the received BPDUs to detect unidirectional link failures that could cause bridging loops using the Unidirectional Link Detection (UDLD) feature. This feature is based on the dispute mechanism.
See the Cisco Nexus 9000 Series NX-OS Interfaces Configuration Guide, for information on UDLD.
When a designated port detects a conflict, it keeps its role, but reverts to a discarding state because disrupting connectivity in case of inconsistency is preferable to opening a bridging loop.
Port Cost
| Note | Rapid PVST+ uses the short (16-bit) path-cost method to calculate the cost by default. With the short path-cost method, you can assign any value in the range of 1 to 65535. However, you can configure the device to use the long (32-bit) path-cost method, which allows you to assign any value in the range of 1 to 200,000,000. You configure the path-cost calculation method globally. | 
This table shows how the STP port path-cost default value is determined from the media speed and path-cost calculation method of a LAN interface.
| Table 4. Default Port                                           		  Cost |  |  | 
|---|---|---|
| Bandwidth | Short Path-Cost Method of Port Cost | Long Path-Cost Method of Port Cost | 
|---|---|---|
| 10 Mbps | 100 | 2,000,000 | 
| 100 Mbps | 19 | 200,000 | 
| 1 Gbps | 4 | 20,000 | 
| 10 Gbps | 2 | 2,000 | 
| 40 Gbps | 1 | 500 | 
| 100 Gbps | 1 | 200 | 
| 400 Gbps | 1 | 50 | 
If a loop occurs, STP considers the port cost when selecting a LAN interface to put into the forwarding state.
You can assign the lower cost values to LAN interfaces that you want STP to select first and higher cost values to LAN interfaces that you want STP to select last. If all LAN interfaces have the same cost value, STP puts the LAN interface with the lowest LAN interface number in the forwarding state and blocks other LAN interfaces.
On access ports, you assign the port cost by the port. On trunk ports, you assign the port cost by the VLAN; you can configure the same port cost to all the VLANs on a trunk port.
Port Priority
If a redundant path occurs and multiple ports have the same path cost, Rapid PVST+ considers the port priority when selecting which LAN port to put into the forwarding state. You can assign lower priority values to LAN ports that you want Rapid PVST+ to select first and higher priority values to LAN ports that you want Rapid PVST+ to select last.
If all LAN ports have the same priority value, Rapid PVST+ puts the LAN port with the lowest LAN port number in the forwarding state and blocks other LAN ports. The possible priority range is from 0 through 224 (the default is 128), configurable in increments of 32. The device uses the port priority value when the LAN port is configured as an access port and uses the VLAN port priority values when the LAN port is configured as a trunk port.
Rapid PVST+ and IEEE 802.1Q Trunks
The 802.1Q trunks impose some limitations on the STP strategy for a network. In a network of Cisco network devices connected through 802.1Q trunks, the network devices maintain one instance of STP for each VLAN allowed on the trunks. However, non-Cisco 802.1Q network devices maintain only one instance of STP for all VLANs allowed on the trunks, which is the Common Spanning Tree (CST).
When you connect a Cisco network device to a non-Cisco device through an 802.1Q trunk, the Cisco network device combines the STP instance of the 802.1Q VLAN of the trunk with the STP instance of the non-Cisco 802.1Q network device. However, all per-VLAN STP information that is maintained by Cisco network devices is separated by a cloud of non-Cisco 802.1Q network devices. The non-Cisco 802.1Q cloud that separates the Cisco network devices is treated as a single trunk link between the network devices.
For more information on 802.1Q trunks, see the Cisco Nexus 9000 Series NX-OS Interfaces Configuration Guide.
Rapid PVST+ Interoperation with Legacy 802.1D STP
Rapid PVST+ can interoperate with devices that are running the legacy 802.1D protocol. The device knows that it is interoperating with equipment running 802.1D when it receives a BPDU version 0. The BPDUs for Rapid PVST+ are version 2. If the BPDU received is an 802.1w BPDU version 2 with the proposal flag set, the device sends an agreement message after all of the other ports are synchronized. If the BPDU is an 802.1D BPDU version 0, the device does not set the proposal flag and starts the forward-delay timer for the port. The new root port requires twice the forward-delay time to transition to the forwarding state.
The device interoperates with legacy 802.1D devices as follows:
-  
                                    			 
                                    Notification—Unlike 802.1D BPDUs, 802.1w does not use TCN BPDUs. However, for interoperability with 802.1D devices, the device processes and generates TCN BPDUs.
-  
                                    			 
                                    Acknowledgment—When an 802.1w device receives a TCN message on a designated port from an 802.1D device, it replies with an 802.1D configuration BPDU with the TCA bit set. However, if the TC-while timer (the same as the TC timer in 802.1D) is active on a root port connected to an 802.1D device and a configuration BPDU with the TCA set is received, the TC-while timer is reset. This method of operation is required only for 802.1D devices. The 802.1w BPDUs do not have the TCA bit set.
-  
                                    			 
                                    Protocol migration—For backward compatibility with 802.1D devices, 802.1w selectively sends 802.1D configuration BPDUs and TCN BPDUs on a per-port basis. When a port is initialized, the migrate-delay timer is started (specifies the minimum time during which 802.1w BPDUs are sent), and 802.1w BPDUs are sent. While this timer is active, the device processes all BPDUs received on that port and ignores the protocol type. If the device receives an 802.1D BPDU after the port migration-delay timer has expired, it assumes that it is connected to an 802.1D device and starts using only 802.1D BPDUs. However, if the 802.1w device is using 802.1D BPDUs on a port and receives an 802.1w BPDU after the timer has expired, it restarts the timer and starts using 802.1w BPDUs on that port.
| Note | If you want all devices on the same LAN segment to reinitialize the protocol on each interface, you must reinitialize Rapid PVST+. | 
Rapid PVST+ Interoperation with 802.1s MST
Rapid PVST+ interoperates seamlessly with the IEEE 802.1s Multiple Spanning Tree (MST) standard. No user configuration is needed. To disable this seamless interoperation, you can use PVST Simulation.
High Availability for Rapid PVST+
The software supports high availability for Rapid PVST+. However, the statistics and timers are not restored when Rapid PVST+ restarts. The timers start again and the statistics begin from 0.
| Note | See the Cisco Nexus 9000 Series NX-OS High Availability and Redundancy Guide, for complete information on high-availability features. |