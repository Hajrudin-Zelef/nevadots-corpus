The documentation set for this product strives to use bias-free language. For the purposes of this documentation set, bias-free is defined as language that does not imply discrimination based on age, disability, gender, racial identity, ethnic identity, sexual orientation, socioeconomic status, and intersectionality. Exceptions may be present in the documentation due to language that is hardcoded in the user interfaces of the product software, language used based on RFP documentation, or language that is used by a referenced third-party product. Learn more about how Cisco is using Inclusive Language.
All ports in an EtherChannel must be assigned to the same VLAN or they must be configured as trunk port.
The LACP 1:1 redundancy feature is supported on port channel interfaces only.
On
EtherChannel member port selection is software based in Layer 2 and Layer 3 multicast route. This means that all multicast
traffic under a group will be routed via the same physical port of the EtherChannel. As a result, the distribution of multicast
traffic load balance over etherchannels might not evenly spread across member ports.
Information About EtherChannels
The following sections provide information about EtherChannels and the various modes to configure EtherChannels.
EtherChannel provides fault-tolerant high-speed links between switches, routers, and servers. You can use the EtherChannel
to increase the bandwidth between the wiring closets and the data center, and you can deploy it anywhere in the network where
bottlenecks are likely to occur. EtherChannel provides automatic recovery for the loss of a link by redistributing the load
across the remaining links. If a link fails, EtherChannel redirects traffic from the failed link to the remaining links in
the channel without intervention.
An EtherChannel consists of individual Ethernet links bundled into a single logical link.
Figure 1. Typical EtherChannel Configuration
Each EtherChannel can consist of up to eight compatibly configured Ethernet ports.
Channel Groups and Port-Channel Interfaces
An EtherChannel comprises a channel group and a port-channel interface. The channel group binds physical ports to the port-channel
interface. Configuration changes applied to the port-channel interface apply to all the physical ports bound together in the
channel group.
Figure 2. Relationship Between Physical Ports, a Channel Group, and a Port-Channel Interface
The channel-group command binds the physical port and the port-channel interface together. Each EtherChannel has a port-channel logical interface
numbered from 1 to 128. This port-channel interface number corresponds to the one specified with the channel-group interface configuration command.
With Layer 2 ports, use the channel-group interface configuration command to dynamically create the port-channel interface.
You also can use the interface port-channelport-channel-number global configuration command to manually create the port-channel interface, but then you must use the channel-groupchannel-group-numbercommand to bind the logical interface to a physical port. The channel-group-number can be the same as the port-channel-number, or you can use a new number. If you use a new number, the channel-group command dynamically creates a new port channel.
With Layer 3 ports, you should manually create the logical interface by using the interface port-channel global configuration command followed by the no switchport interface configuration command. You then manually assign an interface to the EtherChannel by using the channel-group interface configuration command.
With Layer 3 ports, use the no switchport interface command to configure the interface as a Layer 3 interface, and then use the channel-group interface configuration command to dynamically create the port-channel interface.
Port Aggregation Protocol
The Port Aggregation Protocol (PAgP) is a Cisco-proprietary protocol that can be run only on Cisco devices and on those devices
that are licensed by vendors to support PAgP. PAgP facilitates the automatic creation of EtherChannels by exchanging PAgP
packets between Ethernet ports. PAgP can be enabled on cross-stack EtherChannels.
By using PAgP, the switch or switch stack learns the identity of partners capable of supporting PAgP and the capabilities
of each port. It then dynamically groups similarly configured ports (on a single device in the stack) into a single logical
link (channel or aggregate port). Similarly configured ports are grouped based on hardware, administrative, and port parameter
constraints. For example, PAgP groups the ports with the same speed, duplex mode, native VLAN, VLAN range, and trunking status
and type. After grouping the links into an EtherChannel, PAgP adds the group to the spanning tree as a single device port.
PAgP modes specify whether a port can send PAgP packets, which start PAgP negotiations, or only respond to PAgP packets received.
Table 1. EtherChannel PAgP Modes
Mode
Description
auto
Places a port into a passive negotiating state, in which the port responds to PAgP packets it receives but does not start
PAgP packet negotiation. This setting minimizes the transmission of PAgP packets.
desirable
Places a port into an active negotiating state, in which the port starts negotiations with other ports by sending PAgP packets.
Switch ports exchange PAgP packets only with partner ports that are configured in the auto or desirable modes. Ports that are configured in the on mode do not exchange PAgP packets.
Both the auto and desirable modes enable ports to negotiate with partner ports to form an EtherChannel based on criteria such as port speed. and for
Layer 2 EtherChannels, based on trunk state and VLAN numbers.
Ports can form an EtherChannel when they are in different PAgP modes as long as the modes are compatible. For example:
A port in the desirable mode can form an EtherChannel with another port that is in the desirable or auto mode.
A port in the auto mode can form an EtherChannel with another port in the desirable mode.
A port in the auto mode cannot form an EtherChannel with another port that is also in the auto mode because neither port starts PAgP negotiation.
If your switch is connected to a partner that is PAgP-capable, you can configure the switch port for nonsilent operation by
using the non-silent keyword. If you do not specify non-silent with the auto or desirable mode, silent mode is assumed.
Use the silent mode when the switch is connected to a device that is not PAgP-capable and seldom, if ever, sends packets.
An example of a silent partner is a file server or a packet analyzer that is not generating traffic. In this case, running
PAgP on a physical port that is connected to a silent partner prevents that switch port from ever becoming operational. However,
the silent setting allows PAgP to operate, to attach the port to a channel group, and to use the port for transmission.
Port Aggregation Protocol Learn Method and Priority
Network devices are classified as PAgP physical learners or aggregate-port learners. A device is a physical learner if it
learns addresses by physical ports and directs transmissions based on that knowledge. A device is an aggregate-port learner
if it learns addresses by aggregate (logical) ports. The learn method must be configured the same at both ends of the link.
When a device and its partner are both aggregate-port learners, they learn the address on the logical port-channel. The device
sends packets to the source by using any of the ports in the EtherChannel. With aggregate-port learning, it is not important
on which physical port the packet arrives.
PAgP cannot automatically detect when the partner device is a physical learner and when the local device is an aggregate-port
learner. Therefore, you must manually set the learning method on the local device to learn addresses by physical ports. You
also must set the load-distribution method to source-based distribution, so that any given source MAC address is always sent
on the same physical port.
You also can configure a single port within the group for all transmissions and use other ports for hot-standby. The unused
ports in the group can be swapped into operation in just a few seconds if the selected single port loses hardware-signal detection.
You can configure which port is always selected for packet transmission by changing its priority with the pagp port-priority interface configuration command. The higher the priority, the more likely that the port will be selected.
Note
The device supports address learning only on aggregate ports even though the physical-port keyword is provided in the CLI. The pagp learn-method command and the pagp port-priority command have no effect on the device hardware, but they are required for PAgP interoperability with devices that only support
address learning by physical ports, such as the Catalyst 1900 switch.
When the link partner of the device is a physical learner, we recommend that you configure the device as a physical-port learner
by using the pagp learn-method physical-port interface configuration command. Set the load-distribution method based on the source MAC address by using the port-channel load-balance src-mac global configuration command. The device then sends packets to the physical learner using the same port in the EtherChannel
from which it learned the source address. Only use the pagp learn-method command in this situation.
Port Aggregation Protocol Interaction with Other Features
The Dynamic Trunking Protocol (DTP) and the Cisco Discovery Protocol (CDP) send and receive packets over the physical ports
in the EtherChannel. Trunk ports send and receive PAgP protocol data units (PDUs) on the lowest numbered VLAN.
In Layer 2 EtherChannels, the first port in the channel that comes up provides its MAC address to the EtherChannel. If this
port is removed from the bundle, one of the remaining ports in the bundle provides its MAC address to the EtherChannel. For Layer 3 EtherChannels, the MAC address is allocated by the active device as soon as the interface is created (through
the interface port-channel global configuration command).
PAgP sends and receives PAgP PDUs only from ports that are up and have PAgP enabled for the auto or desirable mode.
Link Aggregation Control Protocol
The LACP is defined in IEEE 802.3ad and enables Cisco devices to manage Ethernet channels between devices that conform to
the IEEE 802.3ad protocol. LACP facilitates the automatic creation of EtherChannels by exchanging LACP packets between Ethernet
ports.
By using LACP, the switch or switch stack learns the identity of partners capable of supporting LACP and the capabilities
of each port. It then dynamically groups similarly configured ports into a single logical link (channel or aggregate port).
Similarly configured ports are grouped based on hardware, administrative, and port parameter constraints. For example, LACP
groups the ports with the same speed, duplex mode, native VLAN, VLAN range, and trunking status and type. After grouping the
links into an EtherChannel, LACP adds the group to the spanning tree as a single device port.
The independent mode behavior of ports in a port channel is changed. With CSCtn96950, by default, standalone mode is enabled.
When no response is received from an LACP peer, ports in the port channel are moved to suspended state.
LACP modes specify whether a port can send LACP packets or only receive LACP packets.
Table 2. EtherChannel LACP Modes
Mode
Description
active
Places a port into an active negotiating state in which the port starts negotiations with other ports by sending LACP packets.
passive
Places a port into a passive negotiating state in which the port responds to LACP packets that it receives, but does not start
LACP packet negotiation. This setting minimizes the transmission of LACP packets.
Both the active and passive LACP modes enable ports to negotiate with partner ports to an EtherChannel based on criteria such as port speed, and for Layer 2
EtherChannels, based on trunk state and VLAN numbers.
Ports can form an EtherChannel when they are in different LACP modes as long as the modes are compatible. For example:
A port in the active mode can form an EtherChannel with another port that is in the active or passive mode.
A port in the passive mode cannot form an EtherChannel with another port that is also in the passive mode because neither port starts LACP negotiation.
Link Aggregation Control Protocol Standalone Mode on Ethernet Channel
When one end of an EtherChannel has more members than the other, the unmatched ports enter the standalone state. The standalone
mode is also called the independent mode. In the standalone mode the port is not bundled in an EtherChannel. The port functions
as a standalone data port and it can send and receive BPDUs and data traffic.
In a topology that is not protected from Layer 2 loops by the spanning tree protocol
(STP), a port in the standalone state can cause significant network errors. You can
enter the port-channel standalone-disable command in the
interface configuration mode to put ports into the suspended state instead of the
standalone state.
The standalone mode is particularly relevant when a port (A) in a Layer 2 LACP EtherChannel is connected to an unresponsive
port (B) on the peer. When LACP standalone is disabled on the EtherChannel, all traffic arriving on A is blocked (the default
behavior on a switch). In some scenarios, you might want to allow management traffic on such ports. You can do this by enabling
LACP standalone (or independent) mode. To enable the standalone mode on a Layer 2 LACP Etherchannel, use the no port-channel standalone disable command in the interface configuration mode. To disable the Standalone mode and revert to the default use the port-channel standalone disable command in the interface configuration mode.
Note
LACP standalone mode is disabled by default.
Starting with the Cisco IOS XE Dublin 17.10.1 release, you can configure the LACP standalone mode on a Layer 3 EtherChannel. To configure the standalone mode use the no port-channel standalone disable command in the interface configuration mode. To disable the Standalone mode and revert to the default use the port-channel standalone disable command in the interface configuration mode.
Link Aggregation Control Protocol and Link Redundancy
LACP port-channel operation, bandwidth availability, and link redundancy can be further refined with the LACP port-channel
min-links and the LACP max-bundle features.
The LACP port-channel min-links feature:
Configures the minimum number of ports that must be linked up and bundled in the LACP port channel.
Prevents a low-bandwidth LACP port channel from becoming active.
Causes an LACP port channel to become inactive if there are too few active members ports to supply the required minimum bandwidth.
The LACP max-bundle feature:
Defines an upper limit on the number of bundled ports in an LACP port channel.
Allows hot-standby ports with fewer bundled ports. For example, in an LACP port channel with five ports, you can specify
a max-bundle of three, and the two remaining ports are designated as hot-standby ports.
Link Aggregation Control Protocol Interaction with Other Features
The DTP and the CDP send and receive packets over the physical ports in the EtherChannel. Trunk ports send and receive LACP
PDUs on the lowest numbered VLAN.
In Layer 2 EtherChannels, the first port in the channel that comes up provides its MAC address to the EtherChannel. If this
port is removed from the bundle, one of the remaining ports in the bundle provides its MAC address to the EtherChannel. For Layer 3 EtherChannels, the MAC address is allocated by the active device as soon as the interface is created through the
interface port-channel global configuration command.
LACP sends and receives LACP PDUs only from ports that are up and have LACP enabled for the active or passive mode.
Link Aggregation Control Protocol Interaction with Other Features 1:1 Redundancy
The LACP 1:1 Redundancy feature supports an EtherChannel configuration with one active link, and fast switchover to a hot-standby
link. The link that is connected to the port with the lower port priority number (and therefore, of a higher priority) will
be the active link, and the other link will be in a hot-standby state. If the active link goes down, LACP performs a fast
switchover to the hot-standby link to keep the EtherChannel up. When the failed link becomes operational again, LACP performs
another fast switchover to revert to the original active link.
To allow the higher priority port to stabilize when it becomes active again after a higher-priority to lower-priority switchover,
the LACP 1:1 Hot Standby Dampening feature configures a timer that delays switchover back to the higher priority port after
higher priority port becomes active.
EtherChannel On Mode
EtherChannel on mode can be used to manually configure an EtherChannel. The on mode forces a port to join an EtherChannel without negotiations. The on mode can be useful if the remote device does not support PAgP or LACP. In the on mode, a usable EtherChannel exists only when the devices at both ends of the link are configured in the on mode.
Ports that are configured in the on mode in the same channel group must have compatible port characteristics, such as speed and duplex. Ports that are not compatible
are suspended, even though they are configured in the on mode.
Caution
You should use care when using the on mode. This is a manual configuration, and ports on both ends of the EtherChannel must have the same configuration. If the
group is misconfigured, packet loss or spanning-tree loops can occur.
Load-Balancing and Forwarding Methods
EtherChannel balances the traffic load across the links in a channel by reducing part of the binary pattern that is formed
from the addresses in the frame to a numerical value that selects one of the links in the channel. You can specify one of
several different load-balancing modes, including load distribution based on MAC addresses, IP addresses, source addresses,
destination addresses, or both source and destination addresses. The selected mode applies to all EtherChannels configured
on the device.
Note
Layer 3 Equal-cost multi path (ECMP) load balancing is based on source IP address, destination IP address, source port, destination
port, and layer 4 protocol. Fragmented packets will be treated on two different links based on the algorithm that is calculated
using these parameters. Any changes in one of these parameters result in load balancing.
With source-MAC address forwarding, when packets are forwarded to an EtherChannel, they are distributed across the ports in
the channel based on the source-MAC address of the incoming packet. Therefore, to provide load-balancing, packets from different
hosts use different ports in the channel, but packets from the same host use the same port in the channel.
With destination-MAC address forwarding, when packets are forwarded to an EtherChannel, they are distributed across the ports
in the channel based on the destination host’s MAC address of the incoming packet. Therefore, packets to the same destination
are forwarded over the same port, and packets to a different destination are sent on a different port in the channel.
With source-and-destination MAC address forwarding, when packets are forwarded to an EtherChannel, they are distributed across
the ports in the channel based on both the source and destination MAC addresses. This forwarding method, a combination source-MAC
and destination-MAC address forwarding methods of load distribution, can be used if it is not clear whether source-MAC or
destination-MAC address forwarding is better suited on a particular device. With source-and-destination MAC-address forwarding,
packets sent from host A to host B, host A to host C, and host C to host B could all use different ports in the channel.
IP Address Forwarding
With source-IP address-based forwarding, packets are distributed across the ports in the EtherChannel based on the source-IP
address of the incoming packet. To provide load balancing, packets from different IP addresses use different ports in the
channel, and packets from the same IP address use the same port in the channel.
With destination-IP address-based forwarding, packets are distributed across the ports in the EtherChannel based on the destination-IP
address of the incoming packet. To provide load balancing, packets from the same IP source address that is sent to different
IP destination addresses could be sent on different ports in the channel. Packets sent from different source IP addresses
to the same destination IP address are always sent on the same port in the channel.
With source-and-destination IP address-based forwarding, packets are distributed across the ports in the EtherChannel based
on both the source and destination IP addresses of the incoming packet. This forwarding method, a combination of source-IP
and destination-IP address-based forwarding, can be used if it is not clear whether source-IP or destination-IP address-based
forwarding is better suited on a particular device. In this method, packets sent from the IP address A to IP address B, from
IP address A to IP address C, and from IP address C to IP address B could all use different ports in the channel.
Load-Balancing Advantages
Different load-balancing methods have different advantages, and the choice of a particular load-balancing method should be
based on the position of the device in the network and the kind of traffic that needs to be load-distributed.
Figure 3. Load Distribution and Forwarding Methods. In the following figure, an EtherChannel of four workstations communicates with a router. Because the router is a single
MAC-address device, source-based forwarding on the switch EtherChannel ensures that the switch uses all available bandwidth
to the router. The router is configured for destination-based forwarding because the large number of workstations ensures
that the traffic is evenly distributed from the router EtherChannel.
Use the option that provides the greatest variety in your configuration. For example, if the traffic on a channel is going
only to a single MAC address, using the destination-MAC address always chooses the same link in the channel. Using source
addresses or IP addresses might result in better load-balancing.
EtherChannel and Switch Stacks
If a stack member that has ports participating in an EtherChannel fails or leaves the stack, the active switch removes the
failed stack member switch ports from the EtherChannel. The remaining ports of the EtherChannel, if any, continue to provide
connectivity.
When a switch is added to an existing stack, the new switch receives the running configuration from the active switch and
updates itself with the EtherChannel-related stack configuration. The stack member also receives the operational information
(the list of ports that are up and are members of a channel).
When two stacks merge that have EtherChannels configured between them, self-looped ports result. Spanning tree detects this
condition and acts accordingly. Any PAgP or LACP configuration on a winning switch stack is not affected, but the PAgP or
LACP configuration on the losing switch stack is lost after the stack reboots.
With PAgP, if the active switch fails or leaves the stack, the standby switch becomes the new active switch. The new active
switch synchronizes the configuration of the stack members to that of the active switch. The PAgP configuration is not affected
after an active switch change unless the EtherChannel has ports residing on the old active switch.
Switch Stacks and Link Aggregation Control Protocol
With LACP, the system ID uses the stack MAC address from the active switch. When an active switch fails or leaves the stack
and the standby switch becomes the new active switch, the LACP system ID is unchanged. By default, the LACP configuration
is not affected after the active switch changes.
Default EtherChannel Configuration
The default EtherChannel configuration is described in this table.
Table 3. Default EtherChannel Configuration
Feature
Default Setting
Channel groups
None assigned.
Port-channel logical interface
None defined.
PAgP mode
No default.
PAgP learn method
Aggregate-port learning on all ports.
PAgP priority
128 on all ports.
LACP mode
No default.
LACP learn method
Aggregate-port learning on all ports.
LACP port priority
32768 on all ports.
LACP system priority
32768.
LACP system ID
LACP system priority and the switch or stack MAC address.
Load-balancing
Load distribution on the switch is based on the source-MAC address of the incoming packet.
The source-MAC address is src-mac.
EtherChannel Configuration Guidelines
If improperly configured, some EtherChannel ports are automatically disabled to avoid network loops and other problems. Follow
these guidelines to avoid configuration problems:
A maximum of 128 EtherChannels are supported on a switch or switch stack.
Configure all ports in an EtherChannel to operate at the same speeds and duplex modes.
Enable all ports in an EtherChannel. A port in an EtherChannel that is disabled by using the shutdown interface configuration command is treated as a link failure, and its traffic is transferred to one of the remaining ports
in the EtherChannel.
When a group is first created, all ports follow the parameters set for the first port to be added to the group. If you change
the configuration of one of these parameters, you must also make the changes to all ports in the group:
Allowed-VLAN list
Spanning-tree path cost for each VLAN
Spanning-tree port priority for each VLAN
Spanning-tree Port Fast setting
Do not configure a port to be a member of more than one EtherChannel group.
Do not configure an EtherChannel in both the PAgP and LACP modes. EtherChannel groups running PAgP and LACP can coexist on
the same switch or on different switches in the stack. Individual EtherChannel groups can run either PAgP or LACP, but they
cannot interoperate.
Do not configure a port that is an active or a not-yet-active member of an EtherChannel as an IEEE 802.1x port. If you try
to enable IEEE 802.1x on an EtherChannel port, an error message appears, and IEEE 802.1x is not enabled.
If EtherChannels are configured on device interfaces, remove the EtherChannel configuration from the interfaces before globally
enabling IEEE 802.1x on a device by using the dot1x system-auth-control global configuration command.
When configuring Layer 2 EtherChannels, follow these guidelines:
Assign all ports in the EtherChannel to the same VLAN, or configure them as trunks. Ports with different native VLANs cannot
form an EtherChannel.
An EtherChannel supports the same allowed range of VLANs on all the ports in a trunking Layer 2 EtherChannel. If the allowed
range of VLANs is not the same, the ports do not form an EtherChannel even when PAgP is set to the auto or desirable mode.
Ports with different spanning-tree path costs can form an EtherChannel if they are otherwise compatibly configured. Setting
different spanning-tree path costs does not, by itself, make ports incompatible for the formation of an EtherChannel.
Layer 3 EtherChannel Configuration Guidelines
For Layer 3 EtherChannels, assign the Layer 3 address to the port-channel logical interface, not to the physical ports in
the channel.
Auto-LAG
The auto-LAG feature provides the ability to auto create EtherChannels on ports that are connected to a switch. By default,
auto-LAG is disabled globally and is enabled on all port interfaces. The auto-LAG applies to a switch only when it is enabled
globally.
On enabling auto-LAG globally, the following scenarios are possible:
All port interfaces participate in creation of auto EtherChannels provided the partner port interfaces have EtherChannel
configured on them. For more information, see the "The supported auto-LAG configurations between the actor and partner devices" table below.
Ports that are already part of manual EtherChannels cannot participate in creation of auto EtherChannels.
When auto-LAG is disabled on a port interface that is already a part of an auto created EtherChannel, the port interface unbundles
from the auto EtherChannel.
The following table shows the supported auto-LAG configurations between the actor and partner devices:
Table 4. The supported auto-LAG configurations between the actor and partner devices
Actor/Partner
Active
Passive
Auto
Active
Yes
Yes
Yes
Passive
Yes
No
Yes
Auto
Yes
Yes
Yes
On disabling auto-LAG globally, all auto created Etherchannels become manual EtherChannels.
You cannot add any configurations in an existing auto created EtherChannel. To add, you should first convert it into a manual
EtherChannel by executing the port-channel<channel-number>persistent.
Note
Auto-LAG uses the LACP protocol to create auto EtherChannel. Only one EtherChannel can be automatically created with the
unique partner devices.
Follow these guidelines when configuring the auto-LAG feature.
When auto-LAG is enabled globally and on the port interface, and if you do not want the port interface to become a member
of the auto EtherChannel, disable the auto-LAG on the port interface.
A port interface will not bundle to an auto EtherChannel when it is already a member of a manual EtherChannel. To allow it
to bundle with the auto EtherChannel, first unbundle the manual EtherChannel on the port interface.
When auto-LAG is enabled and auto EtherChannel is created, you can create multiple EtherChannels manually with the same partner
device. But by default, the port tries to create auto EtherChannel with the partner device.
The auto-LAG is supported only on Layer 2 EtherChannel. It is not supported on Layer 3 interface and Layer 3 EtherChannel.
The auto-LAG is supported on cross-stack EtherChannel.
How to Configure EtherChannels
After you configure an EtherChannel, configuration changes applied to the port-channel interface apply to all the physical
ports assigned to the port-channel interface, and configuration changes that are applied to the physical port affect only
the port where you apply the configuration. When bundling physical ports into an EtherChannel, configurine settings on the EtherChannel itself, rather than directly on
its member ports.
The following sections provide various configuration information for EtherChannels:
Configure Layer 2 EtherChannels by assigning ports to a channel group with the channel-group command in interface configuration mode. This command automatically creates the port-channel logical interface.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interface interface-id
Example:
Device(config)# interface gigabitethernet 1/0/1
Specifies a physical port, and enters interface configuration mode.
Valid interfaces are physical ports.
For a PAgP EtherChannel, you can configure up to eight ports of the same type and speed for the same group.
For a LACP EtherChannel, you can configure up to 16 Ethernet ports of the same type. Up to eight ports can be active, and
up to eight ports can be in standby mode.
Step 4
switchport mode {access | trunk}
Example:
Device(config-if)# switchport mode access
Assigns all ports as static-access ports in the same VLAN, or configure them as trunks.
If you configure the port as a static-access port, assign it to only one VLAN. The range is 1 to 4094.
Step 5
switchport access vlanvlan-id
Example:
Device(config-if)# switchport access vlan 22
(Optional) If you configure the port as a static-access port, assign it to only one VLAN. The range is 1 to 4094.
Step 6
channel-groupchannel-group-numbermode {auto [non-silent] | desirable [non-silent ] | on } | { active | passive}
Example:
Device(config-if)# channel-group 5 mode auto
Assigns the port to a channel group, and specifies the PAgP or the LACP mode.
For mode, select one of these keywords:
auto— Enables PAgP only if a PAgP device is detected. It places the port into a passive negotiating state, in which the port responds
to PAgP packets it receives but does not start PAgP packet negotiation.
desirable— Unconditionally enables PAgP. It places the port into an active negotiating state, in which the port starts negotiations with
other ports by sending PAgP packets.
on— Forces the port to channel without PAgP or LACP. In the on mode, an EtherChannel exists only when a port group in the on mode is connected to another port group in the on mode.
non-silent— (Optional) If your device is connected to a partner that is PAgP-capable, configures the device port for nonsilent operation
when the port is in the auto or desirable mode. If you do not specify non-silent, silent is assumed. The silent setting is for connections to file servers or packet analyzers. This setting allows PAgP to
operate, to attach the port to a channel group, and to use the port for transmission.
active—Enables LACP only if a LACP device is detected. It places the port into an active negotiating state in which the port starts
negotiations with other ports by sending LACP packets.
passive— Enables LACP on the port and places it into a passive negotiating state in which the port responds to LACP packets that it
receives, but does not start LACP packet negotiation.
Step 7
end
Example:
Device(config-if)# end
Returns to privileged EXEC mode.
Configuring Layer 3 EtherChannels
Follow these steps to assign an Ethernet port to a Layer 3 EtherChannel. This procedure is required.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interfaceinterface-id
Example:
Device(config)# interface gigabitethernet 1/0/2
Specifies a physical port, and enters interface configuration mode.
Valid interfaces include physical ports.
For a PAgP EtherChannel, you can configure up to eight ports of the same type and speed for the same group.
For a LACP EtherChannel, you can configure up to 16 Ethernet ports of the same type. Up to eight ports can be active, and
up to eight ports can be in standby mode.
Step 4
no ip address
Example:
Device(config-if)# no ip address
Ensures that there is no IP address assigned to the physical port.
Step 5
noswitchport
Example:
Device(config-if)# no switchport
Puts the port into Layer 3 mode.
Step 6
channel-groupchannel-group-numbermode{ auto [ non-silent] | desirable [ non-silent] | on} | { active | passive}
Example:
Device(config-if)# channel-group 5 mode auto
Assigns the port to a channel group, and specifies the PAgP or the LACP mode.
For mode, select one of these keywords:
auto—Enables PAgP only if a PAgP device is detected. It places the port into a passive negotiating state, in which the port responds
to PAgP packets it receives but does not start PAgP packet negotiation. This keyword is not supported when EtherChannel members
are from different switches in the switch stack.
desirable—Unconditionally enables PAgP. It places the port into an active negotiating state, in which the port starts negotiations
with other ports by sending PAgP packets. This keyword is not supported when EtherChannel members are from different switches
in the switch stack.
on—Forces the port to channel without PAgP or LACP. In the on mode, an EtherChannel exists only when a port group in the on mode is connected to another port group in the on mode.
non-silent—(Optional) If your device is connected to a partner that is PAgP capable, configures the device port for nonsilent operation
when the port is in the auto or desirable mode. If you do not specify non-silent, silent is assumed. The silent setting is for connections to file servers or packet analyzers. This setting allows PAgP to
operate, to attach the port to a channel group, and to use the port for transmission.
active—Enables LACP only if a LACP device is detected. It places the port into an active negotiating state in which the port starts
negotiations with other ports by sending LACP packets.
passive— Enables LACP on the port and places it into a passive negotiating state in which the port responds to LACP packets that it
receives, but does not start LACP packet negotiation.
Configures an EtherChannel extended load-balancing method.
The default is src-mac.
Select one of these load-distribution methods:
dst-ip—Specifies destination-host IP address.
dst-mac—Specifies the destination-host MAC address of the incoming packet.
dst-port—Specifies the destination TCP/UDP port.
ipv6-label—Specifies the IPv6 flow label.
l3-proto—Specifies the Layer 3 protocol.
src-ip—Specifies the source host IP address.
src-mac—Specifies the source MAC address of the incoming packet.
src-port—Specifies the source TCP/UDP port.
Step 4
end
Example:
Device(config)# end
Returns to privileged EXEC mode.
(Optional) Configuring the Port Aggregation Protocol Learn Method and Priority
To configure the PAgP learn method and priority, perform this procedure:
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interfaceinterface-id
Example:
Device(config)# interface gigabitethernet 1/0/2
Specifies the port for transmission, and enters interface configuration mode.
Step 4
pagp learn-methodphysical-port
Example:
Device(config-if)# pagp learn-method physical port
Selects the PAgP learning method.
By default, aggregation-port learning is selected, which means the device sends packets to the source by using any of the ports in the EtherChannel. With aggregate-port
learning, it is not important on which physical port the packet arrives.
Selects physical-port to connect with another device that is a physical learner.
Make sure to configure the port-channel load-balance global configuration command to src-mac.
The learning method must be configured the same at both ends of the link.
Step 5
pagp port-prioritypriority
Example:
Device(config-if)# pagp port-priority 200
Assigns a priority so that the selected port is chosen for packet transmission.
For priority, the range is 0 to 255. The default is 128. The higher the priority, the more likely that the port will be used for PAgP
transmission.
Step 6
end
Example:
Device(config-if)# end
Returns to privileged EXEC mode.
Configuring Link Aggregation Control Protocol Hot-Standby Ports
When LACP is enabled, the software, by default, tries to configure the maximum number of LACP-compatible ports in a channel,
up to a maximum of 16 ports. Only eight LACP links can be active at one time; the remaining eight links are placed in hot-standby
mode. If one of the active links becomes inactive, a link that is in the hot-standby mode becomes active in its place.
You can override the default behavior by specifying the maximum number of active ports in a channel, in which case, the remaining
ports become hot-standby ports. For example, if you specify a maximum of five ports in a channel, up to 11 ports become hot-standby
ports.
If you configure more than eight links for an EtherChannel group, the software automatically decides which of the hot-standby
ports to make active based on the LACP priority. To every link between systems that operate LACP, the software assigns a unique
priority that is made up of these elements (in priority order):
LACP system priority
System ID (the device MAC address)
LACP port priority
Port number
In priority comparisons, numerically lower values have higher priority. The priority decides which ports should be put in
standby mode when there is a hardware limitation that prevents all compatible ports from aggregating.
Determining which ports are active and which are hot standby is a two-step procedure. First the system with a numerically
lower system priority and system ID is placed in charge of the decision. Next, that system decides which ports are active
and which are hot standby, based on its values for port priority and port number. The port priority and port number values
for the other system are not used.
You can change the default values of the LACP system priority and the LACP port priority to affect how the software selects
active and standby links.
(Optional) Configuring the Link Aggregation Control Protocol Max Bundle
When you specify the maximum number of bundled LACP ports allowed in a port channel, the remaining ports in the port channel
are designated as hot-standby ports.
Beginning in privileged EXEC mode, follow these steps to configure the maximum number of LACP ports in a port channel.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interface port-channelchannel-number
Example:
Device(config)# interface port-channel 2
Enters interface configuration mode for a port channel.
For channel-number, the range is 1 to 128.
Step 4
lacp max-bundlemax-bundle-number
Example:
Device(config-if)# lacp max-bundle 3
Specifies the maximum number of LACP ports in the port-channel bundle.
The range is 1 to 8.
Step 5
end
Example:
Device(config-if)# end
Returns to privileged EXEC mode.
Configuring Link Aggregation Control Protocol Port-Channel Standalone Disable
To disable the standalone EtherChannel member port state on a port channel, perform this task on the port channel interface:
Configuring the Link Aggregation Control Protocol Port Channel Min-Links
You can specify the minimum number of active ports that must be in the link-up state and bundled in an EtherChannel for the
port channel interface to transition to the link-up state. Using EtherChannel min-links, you can prevent low-bandwidth LACP
EtherChannels from becoming active. Port channel min-links also cause LACP EtherChannels to become inactive if they have too
few active member ports to supply the required minimum bandwidth.
To configure the minimum number of links that are required for a port channel. Perform the following tasks.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configure terminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interface port-channelchannel-number
Example:
Device(config)# interface port-channel 2
Enters interface configuration mode for a port-channel.
For channel-number, the range is 1 to 128.
Step 4
port-channel min-linksmin-links-number
Example:
Device(config-if)# port-channel min-links 3
Specifies the minimum number of member ports that must be in the link-up state and bundled in the EtherChannel for the port
channel interface to transition to the link-up state.
For min-links-number, the range is 2 to 8.
Step 5
end
Example:
Device(config)# end
Returns to privileged EXEC mode.
(Optional) Configuring the Link Aggregation Control Protocol System Priority
You can configure the system priority for all the EtherChannels that are enabled for LACP by using the lacp system-priority command in global configuration mode. You cannot configure a system priority for each LACP-configured channel. By changing
this value from the default, you can affect how the software selects active and standby links.
You can use the show etherchannel summary command in privileged EXEC mode to see which ports are in the hot-standby mode (denoted with an H port-state flag).
Follow these steps to configure the LACP system priority.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
lacp system-prioritypriority
Example:
Device(config)# lacp system-priority 32000
Configures the LACP system priority.
The range is 1 to 65535. The default is 32768.
The lower the value, the higher the system priority.
Step 4
end
Example:
Device(config)# end
Returns to privileged EXEC mode.
(Optional) Configuring the Link Aggregation Control Protocol Port Priority
By default, all ports use the same port priority. If the local system has a lower value for the system priority and the system
ID than the remote system, you can affect which of the hot-standby links become active first by changing the port priority
of LACP EtherChannel ports to a lower value than the default. The hot-standby ports that have lower port numbers become active
in the channel first. You can use the show etherchannel summary privileged EXEC command to see which ports are in the hot-standby mode (denoted with an H port-state flag).
Note
If LACP is not able to aggregate all the ports that are compatible (for example, the remote system might have more restrictive
hardware limitations), all the ports that cannot be actively included in the EtherChannel are put in the hot-standby state
and are used only if one of the channeled ports fails.
Follow these steps to configure the LACP port priority.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configure terminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interface interface-id
Example:
Device(config)# interface gigabitethernet 1/0/2
Specifies the port to be configured, and enters interface configuration mode.
Step 4
lacp port-prioritypriority
Example:
Device(config-if)# lacp port-priority 32000
Configures the LACP port priority.
The range is 1 to 65535. The default is 32768. The lower the value, the more likely that the port will be used for LACP transmission.
Step 5
end
Example:
Device(config-if)# end
Returns to privileged EXEC mode.
Configuring Link Aggregation Control Protocol 1:1 Redundancy
Note
LACP 1:1 redundancy must be enabled at both ends of the LACP EtherChannel.
For the LACP 1:1 Redundancy feature to work, the lacp max-bundle 1 command must be configured along with the lacp fast-switchover command.
For the LACP 1:1 Hot Standby Dampening feature to work, the lacp max-bundle 1 and lacp fast-switchover commands must be configured before the lacp fast-switchover dampening command is configured.
To configure LACP 1:1 redundancy, perform this procedure:
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password, if prompted.
Step 2
configure terminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interface port-channelgroup_number
Example:
Device(config)# interface port-channel 40
Selects an LACP port channel interface and enters interface configuration mode.
Step 4
lacp fast-switchover
Example:
Device(config-if)# lacp fast-switchover
Enables the LACP 1:1 Redundancy feature on the EtherChannel.
Step 5
lacp max-bundle 1
Example:
Device(config-if)# lacp max-bundle 1
Sets the maximum number of active member ports to be one. The only value that is supported with LACP 1:1 redundancy is 1.
(Optional) Enables the LACP 1:1 Hot Standby Dampening feature for this EtherChannel. The range for the time parameter is from
30 to 180 seconds.
Step 7
end
Example:
Device(config-if)# end
Exits interface configuration mode and returns to privileged EXEC mode.
Configuring Link Aggregation Control Protocol 1:1 Redundancy Fast Rate Timer
You can change the LACP timer rate to modify the duration of the LACP timeout. Use the lacp rate command to set the rate at which LACP control packets are received by an LACP-supported interface. You can change the timeout
rate from the default rate (30 seconds) to the fast rate (1 second). This command is supported only on LACP-enabled interfaces.
To configure LACP 1:1 redundancy fast rate timer, perform this procedure:
Configures an interface and enters interface configuration mode.
Step 4
lacp rate{ normal | fast}
Example:
Device(config-if)# lacp rate fast
Configures the rate at which LACP control packets are received by an LACP-supported interface.
To reset the timeout rate to its default, use the no lacp rate command.
Step 5
end
Example:
Device(config)# end
Returns to privileged EXEC mode.
Step 6
show lacp internal
Example:
Device# show lacp internal
Device# show lacp counters
Verifies your configuration.
Configuring Auto-LAG Globally
To configure Auto-LAG globally, perform this procedure:
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
[no] port-channel auto
Example:
Device(config)# port-channel auto
Enables the auto-LAG feature on a switch globally. Use the no form of this command to disable the auto-LAG feature on the
switch globally.
Note
By default, the auto-LAG feature is enabled on the port.
Step 4
end
Example:
Device(config)# end
Returns to privileged EXEC mode.
Step 5
show etherchannel auto
Example:
Device# show etherchannel auto
Displays that EtherChannel is created automatically.
Configuring Auto-LAG on a Port Interface
To configure Auto-LAG on a port interface, perform this procedure:
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
interfaceinterface-id
Example:
Device(config)# interface gigabitethernet 1/0/1
Specifies the port interface to be enabled for auto-LAG, and enters interface configuration mode.
Step 4
[no] channel-group auto
Example:
Device(config-if)# channel-group auto
(Optional) Enables auto-LAG feature on individual port interface. Use the no form of this command to disable the auto-LAG
feature on individual port interface.
Note
By default, the auto-LAG feature is enabled on the port.
Step 5
end
Example:
Device(config-if)# end
Returns to privileged EXEC mode.
Step 6
show etherchannel auto
Example:
Device# show etherchannel auto
Displays that EtherChannel is created automatically.
Configuring Persistence with Auto-LAG
You use the persistence command to convert the auto created EtherChannel into a manual one and allow you to add configuration
on the existing EtherChannel.
To configure persistence with Auto-LAG, perform this procedure:
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
port-channelchannel-numberpersistent
Example:
Device# port-channel 1 persistent
Converts the auto created EtherChannel into a manual one and allows you to add configuration on the EtherChannel.
Step 3
show etherchannel summary
Example:
Device# show etherchannel summary
Displays the EtherChannel information.
Monitoring EtherChannel, Port Aggregation Protocol, and Link Aggregation Control Protocol Status
You can display EtherChannel, PAgP, and LACP status using the commands listed in this table.
Table 5. Commands for Monitoring EtherChannel, PAgP, and LACP Status
Clears PAgP channel-group information and traffic counters.
show etherchannel[ channel-group-number { detail | load-balance | port | port-channel | protocol| summary}] [ detail | load-balance | port | port-channel | protocol | auto | summary]
Displays EtherChannel information in a brief, detailed, and one-line summary form. Also displays the load-balance or frame-distribution
scheme, port, port-channel, protocol, and Auto-LAG information.
show pagp [ channel-group-number] { counters | internal | neighbor}
Displays PAgP information such as traffic information, the internal PAgP configuration, and neighbor information.
show pagp [channel-group-number] dual-active
Displays the dual-active detection status.
show lacp [channel-group-number] {counters | internal | neighbor | sys-id}
Displays LACP information such as traffic information, the internal LACP configuration, and neighbor information.
show running-config
Verifies your configuration entries.
show etherchannel load-balance
Displays the load balance or frame distribution scheme among ports in the port channel.
Configuration Examples for EtherChannels
The following sections provide various configuration examples for EtherChannels:
This example shows how to configure an EtherChannel on a single switch in the stack. It assigns two ports as static-access ports in VLAN 10 to channel 5 with the PAgP mode desirable:
This example shows how to configure an EtherChannel on a single switch in the stack. It assigns two ports as static-access ports in VLAN 10 to channel 5 with the LACP mode active:
Device# configure terminal
Device(config)# interface range gigabitethernet2/0/1 -2
Device(config-if-range)# switchport mode access
Device(config-if-range)# switchport access vlan 10
Device(config-if-range)# channel-group 5 mode active
Device(config-if-range)# end
This example shows how to configure a cross-stack EtherChannel. It uses LACP passive mode and assigns two ports on stack member
1 and one port on stack member 2 as static-access ports in VLAN 10 to channel 5:
PoE or LACP negotiation errors may occur if you configure two ports from switch to the access point (AP). This scenario can
be avoided if the port channel configuration is on the switch side. For more details, see the following example:
If the port reports LACP errors on port flap, you should include the following command as well: no errdisable detect cause pagp-flap
Example: Configuring Layer 3 EtherChannels
This example shows how to configure a Layer 3 EtherChannel. It assigns two ports to channel 5 with the LACP mode active:
Device# configure terminal
Device(config)# interface range gigabitethernet2/0/1 -2
Device(config-if-range)# no ip address
Device(config-if-range)# no switchport
Device(config-if-range)# channel-group 5 mode active
Device(config-if-range)# end
This example shows how to configure a cross-stack Layer 3 EtherChannel. It assigns two ports on stack member 2 and one port
on stack member 3 to channel 7 using LACP active mode:
Device# configure terminal
Device(config)# interface range gigabitethernet2/0/4 -5
Device(config-if-range)# no ip address
Device(config-if-range)# no switchport
Device(config-if-range)# channel-group 7 mode active
Device(config-if-range)# exit
Device(config)# interface gigabitethernet3/0/3
Device(config-if)# no ip address
Device(config-if)# no switchport
Device(config-if)# channel-group 7 mode active
Device(config-if)# exit
Example: Configuring Link Aggregation Control Protocol Hot-Standby Ports
This example shows how to configure an EtherChannel (port channel 2) that will be active when there are at least three active
ports, will comprise up to seven active ports and the remaining ports (up to nine) as hot-standby ports:
This is a sample output from the show lacp internal command:
Device# show lacp 1 internal
Flags: S - Device is requesting Slow LACPDUs
F - Device is requesting Fast LACPDUs
A - Device is in Active mode
P - Device is in Passive mode
Channel group 1,[146 s left to exit dampening state]
LACP port Admin Oper Port Port
Port Flags State Priority Key Key Number State
Fa1/1 FA hot-sby 30000* 0x1 0x1 0x103 0x7
Fa1/2 SA bndl 32768 0x1 0x1 0x102 0x3D
Example: Configuring Standalone Mode on EtherChannel
This example shows how to configure the Standalone mode or Independent mode on an Port channel:
Device(config)# interface port-channel 1
Device(config-if)# no port-channel standalone-disable
Device(config-if)# end
This example shows how to verify the configuration of the Standalone mode on a Port
Channel interface:
Device# show running-config interface port-channel 1
Building configuration...
Current configuration:
!
interface Port-channel1
no ip address
no switchport
no port-channel standalone-disable
end
Example: Configuring Auto LAG
This example shows how to configure Auto-LAG on a switch
Device> enable
Device# configure terminal
Device(config)# port-channel auto
Device(config-if)# end
Device# show etherchannel auto
This example shows the summary of EtherChannel that was created automatically.
Device# show etherchannel auto
Flags: D - down P - bundled in port-channel
I - stand-alone s - suspended
H - Hot-standby (LACP only)
R - Layer3 S - Layer2
U - in use f - failed to allocate aggregator
M - not in use, minimum links not met
u - unsuitable for bundling
w - waiting to be aggregated
d - default port
A - formed by Auto LAG
Number of channel-groups in use: 1
Number of aggregators: 1
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SUA) LACP Gi1/0/45(P) Gi2/0/21(P) Gi3/0/21(P)
This example shows the summary of auto EtherChannel after executing the port-channel 1 persistent command .
Device# port-channel 1 persistent
Device# show etherchannel summary
Switch# show etherchannel summary
Flags: D - down P - bundled in port-channel
I - stand-alone s - suspended
H - Hot-standby (LACP only)
R - Layer3 S - Layer2
U - in use f - failed to allocate aggregator
M - not in use, minimum links not met
u - unsuitable for bundling
w - waiting to be aggregated
d - default port
A - formed by Auto LAG
Number of channel-groups in use: 1
Number of aggregators: 1
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) LACP Gi1/0/45(P) Gi2/0/21(P) Gi3/0/21(P)
Additional References for EtherChannels
Related Documents
Related Topic
Document Title
For complete syntax and usage information for the commands used in this chapter.
See the Layer 2/3 Commands section of theCommand Reference (Catalyst 9300 Series Switches)
Feature History for EtherChannels
This table provides release and related information for features explained in this module.
These features are available on all releases subsequent to the one they were introduced in, unless noted otherwise.
Release
Feature
Feature Information
Cisco IOS XE Everest 16.5.1a
EtherChannels
EtherChannel provides fault-tolerant high-speed links between switches, routers, and servers.
Cisco IOS XE Amsterdam 17.3.1
LACP 1:1 Redundancy and Dampening
The LACP 1:1 Redundancy feature supports an EtherChannel configuration with one active link and fast switchover to a hot-standby
link.
The LACP 1:1 Hot Standby Dampening feature configures a timer that delays switchover back to the higher priority port after
it becomes active.
Cisco IOS XE Dublin 17.10.1
Standalone Mode on Layer 3 Etherchannels
Support for configuring Standalone mode or Independent mode on Layer 3 EtherChannels was introduced in this release.
Use Cisco Feature Navigator to find information about platform and software image support. To access Cisco Feature Navigator,
go to http://www.cisco.com/go/cfn.