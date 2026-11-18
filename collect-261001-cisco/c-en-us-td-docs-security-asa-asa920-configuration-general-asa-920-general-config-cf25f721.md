About Failover
Configuring failover requires two identical ASAs connected to each other through a dedicated failover link and, optionally, a state link. The health of the active units and interfaces is monitored to determine whether they meet the specific failover conditions. If those conditions are met, failover occurs.
Failover Modes
The ASA supports two failover modes, Active/Active failover and Active/Standby failover. Each failover mode has its own method for determining and performing failover.
- 
                                 In Active/Standby failover, one device functions as the Active unit and passes traffic. The second device, designated as the Standby unit, does not actively pass traffic. When a failover occurs, the Active unit fails over to the Standby unit, which then becomes Active. You can use Active/Standby failover for ASAs in single or multiple context mode.
- 
                                 In an Active/Active failover configuration, both ASAs can pass network traffic. Active/Active failover is only available to ASAs in multiple context mode. In Active/Active failover, you divide the security contexts on the ASA into 2 failover groups. A failover group is simply a logical group of one or more security contexts. One group is assigned to be Active on the primary ASA, and the other group is assigned to be active on the Secondary ASA. When a failover occurs, it occurs at the failover group level.
Both failover modes support stateful or stateless failover.
Failover system requirements
Failover system requirements are specifications that define the hardware, software, and license prerequisites for ASAs to function in a Failover configuration.
Hardware requirements
Hardware requirements are specifications that ensure two units in a Failover configuration can
- 
                                       
                                       maintain identical hardware configurations including model, interfaces, modules, and RAM
- 
                                       
                                       support proper synchronization and failover functionality, and
- 
                                       
                                       ensure reliable high availability operation.
Hardware requirement specifications
The two units in a Failover configuration must meet these requirements:
- 
                                       
                                       Be the same model. For the Firepower 9300, High Availability is only supported between same-type modules. However, the two chassis can include mixed modules. For example, each chassis has an SM-56, SM-48, and SM-40. You can create High Availability pairs between the SM-56 modules, between the SM-48 modules, and between the SM-40 modules.
- 
                                       
                                       Have the same number and types of interfaces. For the Firepower 2100 in Platform mode and Firepower 4100/9300 chassis, all interfaces must be preconfigured in FXOS identically before you enable Failover. If you change the interfaces after you enable Failover, make the interface changes in FXOS on the Standby unit, and then make the same changes on the Active unit. If you remove an interface in FXOS (for example, if you remove a network module, remove an EtherChannel, or reassign an interface to an EtherChannel), then the ASA configuration retains the original commands so that you can make any necessary adjustments; removing an interface from the configuration can have wide effects. You can manually remove the old interface configuration in the ASA OS.
- 
                                       
                                       Have the same modules installed (if any).
- 
                                       
                                       Have the same RAM installed.
If you are using units with different flash memory sizes in your Failover configuration, make sure the unit with the smaller flash memory has enough space to accommodate the software image files and the configuration files. If it does not, configuration synchronization from the unit with the larger flash memory to the unit with the smaller flash memory will fail.
Software requirements
Software requirements are configuration specifications that Failover units must meet to function properly in a failover configuration.
Required software specifications
The two units in a Failover configuration must meet these requirements:
- 
                                       
                                       Be in the same context mode (single or multiple).
- 
                                       
                                       For single mode: Be in the same firewall mode (routed or transparent). In multiple context mode, the firewall mode is set at the context-level, and you can use mixed modes.
- 
                                       
                                       Have the same major (first number) and minor (second number) software version. However, you can temporarily use different versions of the software during an upgrade process; for example, you can upgrade one unit from Version 8.3(1) to Version 8.3(2) and have failover remain active. We recommend upgrading both units to the same version to ensure long-term compatibility.
- 
                                       
                                       Have the same Secure Client images. If the failover pair has mismatched images when a hitless upgrade is performed, then the clientless SSL VPN connection terminates in the final reboot step of the upgrade process, the database shows an orphaned session, and the IP pool shows that the IP address assigned to the client is "in use."
- 
                                       
                                       Be in the same FIPS mode.
- 
                                       
                                       
                                       (Firepower 4100/9300) Have the same flow offload mode, either both enabled or both disabled.
License Requirements
The two units in a failover configuration do not need to have identical licenses; the licenses combine to make a failover cluster license.
Failover and stateful failover links
A failover link and stateful failover link are dedicated connections that
- 
                                    
                                    establish communication between the two units in a failover configuration
- 
                                    
                                    require the same interface to be used between corresponding devices for consistency, and
- 
                                    
                                    transmit critical failover and state information between the paired units.
Configuration recommendations
Cisco recommends to use the same interface between two devices in a failover link or a stateful failover link. For example, in a failover link, if you have used eth0 in device 1, use the same interface (eth0) in device 2 as well.
| Caution | All information sent over the failover and state links is sent in clear text unless you secure the communication with an IPsec tunnel or a failover key. If the ASA is used to terminate VPN tunnels, this information includes any usernames, passwords and preshared keys used for establishing the tunnels. Transmitting this sensitive data in clear text could pose a significant security risk. We recommend securing the failover communication with an IPsec tunnel or a failover key if you are using the ASA to terminate VPN tunnels. | 
Failover link
A failover link is a communication mechanism that enables two units in a failover pair to constantly communicate to determine the operating status of each unit.
Failover link data
This below information is communicated over the failover link:
- 
                                          
                                          The unit state (active or standby)
- 
                                          
                                          Hello messages (keep-alives)
- 
                                          
                                          Network link status
- 
                                          
                                          MAC address exchange
- 
                                          
                                          Configuration replication and synchronization
Interface for the failover link
A failover link interface is a dedicated communication channel that
- 
                                          
                                          exists solely for failover communication between high availability units
- 
                                          
                                          uses an unused data interface (physical, subinterface, or EtherChannel)
- 
                                          
                                          cannot be shared with normal networking interfaces or user data traffic, and
- 
                                          
                                          requires specific interface sizing based on the device model.
Interface restrictions and requirements
You can use an unused data interface (physical, subinterface, or EtherChannel) as the failover link; however, you cannot specify an interface that is currently configured with a name. The failover link interface is not configured as a normal networking interface; it exists for failover communication only. This interface can only be used for the failover link (and also for the state link). For most models, you cannot use a management interface for failover unless explicitly described below.
The ASA does not support sharing interfaces between user data and the failover link. You also cannot use separate subinterfaces on the same parent for the failover link and for data.
See these guidelines for the failover link:
- 
                                          
                                          5506-X through 5555-X—You cannot use the Management interface as the failover link; you must use a data interface. The only exception is for the 5506H-X, where you can use the management interface as the failover link.
- 
                                          
                                          5506H-X—You can use the Management 1/1 interface as the failover link. If you configure it for failover, you must reload the device for the change to take effect. In this case, you cannot also use the ASA Firepower module, because it requires the Management interface for management purposes.
- 
                                          
                                          Firepower 4100/9300—You cannot use the management-type interface for the failover link.
- 
                                          
                                          See these guidelines for sizing the link. Table 1. Failover link size Model Interface Size for Combined Failover and State Link Firepower 1010 1 Gbps Firepower 1100 1 Gbps Firepower 2100 1 Gbps Secure Firewall 3100 Secure Firewall 3105—1 Gbps Secure Firewall 3110—1 Gbps Secure Firewall 3120—1 Gbps Secure Firewall 3130—10 Gbps Secure Firewall 3140—10 Gbps Firepower 4100 10 Gbps Secure Firewall 4200 10 Gbps Firepower 9300 10 Gbps
The alternation frequency is equal to the unit hold time (the failover polltime unit command).
| Note | If you have a large configuration and a low unit hold time, alternating between the member interfaces can prevent the secondary unit from joining/re-joining. In this case, disable one of the member interfaces until after the secondary unit joins. | 
For an EtherChannel used as the failover link, to prevent out-of-order packets, only one interface in the EtherChannel is used. If that interface fails, then the next interface in the EtherChannel is used. You cannot alter the EtherChannel configuration while it is in use as a failover link.
Connect the failover link
The failover link enables communication and coordination between units in a failover configuration.
You need to establish a physical connection between the failover units to enable proper failover functionality.
Procedure
| Connect the failover link using one of the following methods:  If you do not use a switch between the units and the interface fails, the link is brought down on both peers. This condition may hamper troubleshooting efforts because you cannot easily determine which unit has the failed interface and caused the link to come down. The ASA supports Auto-MDI/MDIX on its copper Ethernet ports, so you can either use a crossover cable or a straight-through cable. If you use a straight-through cable, the interface automatically detects the cable and swaps one of the transmit/receive pairs to MDIX. | 
The failover link is physically connected and ready for failover configuration.
Stateful failover links
A stateful failover link is a network connection that
- 
                                       
                                       passes connection state information between failover units, and
- 
                                       
                                       must be configured to enable stateful failover functionality.
State link configuration requirements
To use Stateful Failover, you must configure a Stateful Failover link (also known as the state link) to pass connection state information.
Shared failover links
A shared failover link is an interface conservation method that enables multiple network functions to use the same failover connection, though dedicated interfaces for state and failover links should be considered for large configurations and high traffic networks.
Dedicated interface
A dedicated interface is a data interface configuration that uses a dedicated physical or EtherChannel interface specifically for the state link in failover deployments.
Performance considerations
You can use a dedicated data interface—such as a physical or EtherChannel interface—for the state link. For requirements about a dedicated state link, refer to Interface for the failover link. See the section on connecting the state link for additional information Connect the failover link.
For optimum performance with long-distance failover, the state link latency should be less than 10 milliseconds but no more than 250 milliseconds. If latency exceeds 10 milliseconds, performance might degrade due to retransmission of failover messages.
Avoiding Interrupted Failover and Data Links
We recommend that failover links and data interfaces travel through different paths to decrease the chance that all interfaces fail at the same time. If the failover link is down, the ASA can use the data interfaces to determine if a failover is required. Subsequently, the failover operation is suspended until the health of the failover link is restored.
See the following connection scenarios to design a resilient failover network.
Scenario 1—Not Recommended
If a single switch or a set of switches are used to connect both failover and data interfaces between two ASAs, then when a switch or inter-switch-link is down, both ASAs become active. Therefore, the following two connection methods shown in the following figures are NOT recommended.
Scenario 2—Recommended
We recommend that failover links NOT use the same switch as the data interfaces. Instead, use a different switch or use a direct cable to connect the failover link, as shown in the following figures.
Scenario 3—Recommended
If the ASA data interfaces are connected to more than one set of switches, then a failover link can be connected to one of the switches, preferably the switch on the secure (inside) side of network, as shown in the following figure.
MAC addresses and IP addresses in Failover
MAC addresses and IP addresses in Failover are network addressing mechanisms that
- 
                                    
                                    provide unique identification for network interfaces during failover scenarios
- 
                                    
                                    maintain network connectivity when primary devices become unavailable, and
- 
                                    
                                    ensure seamless traffic flow between active and standby configurations.
Address configuration types
When you configure your interfaces, you can specify an active IP address and a standby IP address on the same network. Generally, when a failover occurs, the new active unit takes over the active IP addresses and MAC addresses. Because network devices see no change in the MAC to IP address pairing, no ARP entries change or time out anywhere on the network.
| Note | Although recommended, the standby address is not required. Without a standby IP address, the active unit cannot perform network tests to check the standby interface health; it can only track the link state. You also cannot connect to the standby unit on that interface for management purposes. | 
The IP address and MAC address for the state link do not change at failover.
Active/standby configurations use specific address handling methods:
For Active/Standby Failover, see the following for IP address and MAC address usage during a failover event:
- 
                                       						
                                       The active unit always uses the primary unit's IP addresses and MAC addresses.
- 
                                       						
                                       When the active unit fails over, the standby unit assumes the IP addresses and MAC addresses of the failed unit and begins passing traffic.
- 
                                       						
                                       When the failed unit comes back online, it is now in a standby state and takes over the standby IP addresses and MAC addresses.
However, if the secondary unit boots without detecting the primary unit, then the secondary unit becomes the active unit and uses its own MAC addresses, because it does not know the primary unit MAC addresses. When the primary unit becomes available, the secondary (active) unit changes the MAC addresses to those of the primary unit, which can cause an interruption in your network traffic. Similarly, if you swap out the primary unit with new hardware, a new MAC address is used.
If you disable failover and set the failover configurations to a disabled state, you will need to manually resume failover, or reboot the device. It is recommended to use the command failover reset and resume the failover instead of rebooting the device. If you reload the standby unit with the failover configuration disabled, the standby unit boots up as the active unit and uses the primary unit's IP addresses and MAC addresses. This leads to duplicate IP addresses and causes network traffic disruptions. Use the command failover reset to enable failover and restore the traffic flow.
| Note | If you enable failover on a standalone device, the data interfaces go down at negotiation state of failover, interrupting traffic. | 
Virtual MAC addresses guard against this disruption, because the active MAC addresses are known to the secondary unit at startup, and remain the same in the case of new primary unit hardware. We recommend that you configure the virtual MAC address on both the primary and secondary units to ensure that the secondary unit uses the correct MAC addresses when it is the active unit, even if it comes online before the primary unit. If you do not configure virtual MAC addresses, you might need to clear the ARP tables on connected routers to restore traffic flow. The ASA does not send gratuitous ARPs for static NAT addresses when the MAC address changes, so connected routers do not learn of the MAC address change for these addresses.
For Active/Active failover, see the following for IP address and MAC address usage during a failover event:
- 
                                       						
                                       The primary unit autogenerates active and standby MAC addresses for all interfaces in failover group 1 and 2 contexts. You can also manually configure the MAC addresses if necessary, for example, if there are MAC address conflicts.
- 
                                       						
                                       Each unit uses the active IP addresses and MAC addresses for its active failover group, and the standby addresses for its standby failover group. For example, the primary unit is active for failover group 1, so it uses the active addresses for contexts in failover group 1. It is standby for the contexts in failover group 2, where it uses the standby addresses.
- 
                                       						
                                       When a unit fails over, the other unit assumes the active IP addresses and MAC addresses of the failed failover group and begins passing traffic.
- 
                                       						
                                       When the failed unit comes back online, and you enabled the preempt option, it resumes the failover group.
Active/active configurations implement additional address management:
Virtual MAC addresses provide specialized functionality:
The ASA has multiple methods to configure virtual MAC addresses. We recommend using only one method. If you set the MAC address using multiple methods, the MAC address used depends on many variables, and might not be predictable. Manual methods include the interface mode mac-address command, the failover mac address command, and for Active/Active failover, the failover group mode mac address command, in addition to autogeneration methods described below.
In multiple context mode, you can configure the ASA to generate virtual active and standby MAC addresses automatically for shared interfaces, and these assignments are synced to the secondary unit (see the mac-address auto command). For non-shared interfaces, you can manually set the MAC addresses for Active/Standby mode (Active/Active mode autogenerates MAC addresses for all interfaces).
For Active/Active failover, virtual MAC addresses are always used, either with default values or with values you can set per interface.
MAC address table updates occur during failover events:
During failover, the device that is designated as the new active device generates multicast packets for each MAC address entry in the MAC table and sends them to all the bridge group interfaces. This action prompts the upstream switches in the bridge group to update their routing tables with the new active device's interface to ensure accurate traffic forwarding.
The time taken to generate multicast packets and update the routing tables of the upstream switches depends on the number of entries in the MAC address table and the number of bridge group interfaces. Use the show failover statistics state-switch-delay command to display statistics related to the delays encountered during failover events.
Stateless and Stateful Failover
The ASA supports two types of failover, stateless and stateful for both the Active/Standby and Active/Active modes.
| Note | Some configuration elements for clientless SSL VPN (such as bookmarks and customization) use the VPN failover subsystem, which is part of Stateful Failover. You must use Stateful Failover to synchronize these elements between the members of the failover pair. Stateless failover is not recommended for clientless SSL VPN. | 
Stateless Failover
When a failover occurs, all active connections are dropped. Clients need to reestablish connections when the new active unit takes over.
| Note | Some configuration elements for clientless SSL VPN (such as bookmarks and customization) use the VPN failover subsystem, which is part of Stateful Failover. You must use Stateful Failover to synchronize these elements between the members of the failover pair. Stateless (regular) failover is not recommended for clientless SSL VPN. | 
Stateful failover
A stateful failover is a high availability mechanism that
- 
                                       
                                       continuously passes per-connection state information from the active unit to the standby unit
- 
                                       
                                       maintains connection information after a failover occurs, and
- 
                                       
                                       allows supported end-user applications to continue communication sessions without reconnecting.
Stateful failover operation
When Stateful Failover is enabled, the active unit continually passes per-connection state information to the standby unit, or in Active/Active failover, between the active and standby failover groups. After a failover occurs, the same connection information is available at the new active unit. Supported end-user applications are not required to reconnect to keep the same communication session.
Supported features
Supported features are high availability capabilities that
- 
                                          
                                          enable state information to be passed to the standby ASA device during Stateful Failover
- 
                                          
                                          maintain connection states and critical information for seamless traffic flow, and
- 
                                          
                                          ensure minimal disruption during failover events.
State information types
For Stateful Failover, these state information types are passed to the standby device:
- 
                                          
                                          NAT translation table.
- 
                                          
                                          TCP and UDP connections and states. Other types of IP protocols, and ICMP, are not parsed by the active unit, because they get established on the new active unit when a new packet arrives.
- 
                                          
                                          The HTTP connection table (unless you enable HTTP replication).
- 
                                          
                                          The HTTP connection states (if HTTP replication is enabled)—By default, the ASA does not replicate HTTP session information when Stateful Failover is enabled. We suggest that you enable HTTP replication.
- 
                                          
                                          SCTP connection states are included. SCTP inspection stateful failover is best effort. If any SACK packets are lost during failover, the new active unit drops all other out-of-order packets in the queue until the missing packet is received.
- 
                                          
                                          The ARP table
- 
                                          
                                          The Layer 2 bridge table (for bridge groups)
- 
                                          
                                          The ISAKMP and IPsec SA table
- 
                                          
                                          GTP PDP connection database
- 
                                          
                                          SIP signaling sessions and pin holes.
- 
                                          
                                          ICMP connection state—ICMP connection replication is enabled only if the respective interface is assigned to an asymmetric routing group.
- 
                                          
                                          Static and dynamic routing tables—Stateful Failover participates in dynamic routing protocols, like OSPF and EIGRP, so routes that are learned through dynamic routing protocols on the active unit are maintained in a Routing Information Base (RIB) table on the standby unit. Upon a failover event, packets travel normally with minimal disruption to traffic because the active secondary unit initially has rules that mirror the primary unit. Immediately after failover, the re-convergence timer starts on the newly active unit. Then the epoch number for the RIB table increments. During re-convergence, OSPF and EIGRP routes become updated with a new epoch number. Once the timer is expired, stale route entries (determined by the epoch number) are removed from the table. The RIB then contains the newest routing protocol forwarding information on the newly active unit. Note 
 Routes are synchronized only for link-up or link-down events on an active unit. If the link goes up or down on the standby unit, dynamic routes sent from the active unit may be lost. This is normal, expected behavior. 
- 
                                          
                                          DHCP Server—DHCP address leases are not replicated. However, a DHCP server configured on an interface will send a ping to make sure an address is not being used before granting the address to a DHCP client, so there is no impact to the service. State information is not relevant for DHCP relay or DDNS.
- 
                                          
                                          Cisco IP SoftPhone sessions—If a failover occurs during an active Cisco IP SoftPhone session, the call remains active because the call session state information is replicated to the standby unit. When the call is terminated, the IP SoftPhone client loses connection with the Cisco Call Manager. This connection loss occurs because there is no session information for the CTIQBE hangup message on the standby unit. When the IP SoftPhone client does not receive a response back from the Call Manager within a certain time period, it considers the Call Manager unreachable and unregisters itself.
- 
                                          
                                          RA VPN—Remote access VPN end users do not have to reauthenticate or reconnect the VPN session after a failover. However, applications operating over the VPN connection could lose packets during the failover process and not recover from the packet loss.
- 
                                          
                                          From all the connections, only established ones will be replicated on the Standby device.
Unsupported features
Unsupported features are device capabilities that
- 
                                          
                                          are not passed to the standby ASA in Stateful Failover
- 
                                          
                                          do not maintain state information during failover events, and
- 
                                          
                                          may require re-establishment after a failover occurs.
State information limitations
For Stateful Failover, the following state information is not passed to the standby ASA:
-  
                                          
                                          The user authentication (uauth) table
-  
                                          
                                          Multicast routing.
Bridge Group Requirements for Failover
There are special considerations for failover when using bridge groups.
Bridge Group Requirements for Appliances, ASAv
When the active unit fails over to the standby unit, the connected switch port running Spanning Tree Protocol (STP) can go into a blocking state for 30 to 50 seconds when it senses the topology change. To avoid traffic loss while the port is in a blocking state, you can configure one of the following workarounds depending on the switch port mode:
-  
                                    		  
                                    Access mode—Enable the STP PortFast feature on the switch: 
interface interface_id
  spanning-tree portfast
The PortFast feature immediately transitions the port into STP forwarding mode upon linkup. The port still participates in STP. So if the port is to be a part of the loop, the port eventually transitions into STP blocking mode.
-  
                                    		  
                                    Trunk mode—Block BPDUs on the ASA on a bridge group's member interfaces with an EtherType access rule. 
access-list id ethertype deny bpdu
access-group id in interface name1
access-group id in interface name2
Blocking BPDUs disables STP on the switch. Be sure not to have any loops involving the ASA in your network layout.
If neither of the above options are possible, then you can use one of the following less desirable workarounds that impacts failover functionality or STP stability:
-  
                                    		  
                                    Disable interface monitoring.
-  
                                    		  
                                    Increase interface holdtime to a high value that will allow STP to converge before the ASAs fail over.
-  
                                    		  
                                    Decrease STP timers to allow STP to converge faster than the interface holdtime.
Failover health monitoring
Failover health monitoring is a system capability that monitors each unit for overall health and interface health.
Health monitoring tests
The ASA performs tests to determine the state of each unit. This section includes information about how the ASA performs tests to determine the state of each unit.
Unit health monitoring
Unit health monitoring is a failover mechanism that
- 
                                       
                                       determines the health of peer units by monitoring the failover link with hello messages
- 
                                       
                                       sends LANTEST messages on each data interface when three consecutive hello messages are missed, and
- 
                                       
                                       initiates appropriate failover actions based on peer unit responsiveness.
Unit health monitoring behavior
The ASA determines the health of the other unit by monitoring the failover link with hello messages. If a unit does not receive three consecutive hello messages on the failover link, the sends LANTEST messages on each data interface, including the failover link, to validate whether the peer is responsive. For the Firepower 9300 and 4100 series, you can enable Bidirectional Forwarding Detection (BFD) monitoring, which is more reliable than hello messages. The action that the ASA takes depends on the response from the other unit. These are the possible actions:
- 
                                       
                                       If the ASA receives a response on the failover link, then it does not fail over.
- 
                                       
                                       If the ASA does not receive a response on the failover link, but it does receive a response on a data interface, then the unit does not failover. The failover link is marked as failed. You should restore the failover link as soon as possible because the unit cannot fail over to the standby while the failover link is down.
- 
                                       
                                       If the ASA does not receive a response on any interface, then the standby unit switches to active mode and classifies the other unit as failed.
Heartbeat module redundancy
Heartbeat module redundancy is a high availability feature that
- 
                                       
                                       sends heartbeat messages using the data plane transport infrastructure to supplement control plane heartbeat packets
- 
                                       
                                       increments a counter when the peer receives heartbeat packets in the data plane
- 
                                       
                                       prevents false failover or split-brain scenarios when the control plane is congested with traffic.
Heartbeat module redundancy behavior
Each unit in the HA periodically sends a broadcast keepalive heartbeat packet over the failover link. If the control plane is too busy handling traffic, sometimes the heartbeat packets do not reach the peers, or the peers do not process the heartbeat packets due to CPU overloading. When peers cannot communicate the keepalive status within the configurable timeout period, a false failover or split-brain scenario occurs.
The heartbeat module in the data plane helps to avoid the occurrence of false failover or split-brain due to traffic congestion in the control plane.
- 
                                       
                                       The additional heartbeat module works similarly to the control plane module but sends and receives heartbeat messages using the data plane transport infrastructure.
- 
                                       
                                       When the peer receives heartbeat packets in the data plane, a counter gets incremented.
- 
                                       
                                       If the heartbeat transfer in the control plane fails, the node checks the heartbeat counter in the data plane. If the counter is incrementing, then the peer is alive, and the cluster does not perform a failover in this situation.
| Note |  | 
Interface monitoring
Interface monitoring is a failover capability that
- 
                                       
                                       tracks the health of up to 1025 interfaces to detect interface failures
- 
                                       
                                       performs interface tests when hello messages are not received within 15 seconds, and
- 
                                       
                                       triggers failover when the number of failed interfaces meets the defined threshold.
Interface monitoring operation
You can monitor up to 1025 interfaces (in multiple context mode, divided between all contexts). You should monitor important interfaces. For example in multiple context mode, you might configure one context to monitor a shared interface: because the interface is shared, all contexts benefit from the monitoring.
When a unit does not receive hello messages on a monitored interface for 15 seconds (the default), it runs interface tests. (To change the period, see the failover polltime interface command, or for Active/Active failover, the polltime interface command) If one of the interface tests fails for an interface, but this same interface on the other unit continues to successfully pass traffic, then the interface is considered to be failed, and the ASA stops running tests.
If the threshold you define for the number of failed interfaces is met (see the failover interface-policy command, or for Active/Active failover, the interface-policy command), and the active unit has more failed interfaces than the standby unit, then a failover occurs. If an interface fails on both units, then both interfaces go into the "Unknown" state and do not count towards the failover limit defined by failover interface policy.
An interface becomes operational again if it receives any traffic. A failed ASA returns to standby mode if the interface failure threshold is no longer met.
If an interface has IPv4 and IPv6 addresses configured on it, the ASA uses the IPv4 addresses to perform the health monitoring. If an interface has only IPv6 addresses configured on it, then the ASA uses IPv6 neighbor discovery instead of ARP to perform the health monitoring tests. For the broadcast ping test, the ASA uses the IPv6 all nodes address (FE02::1).
| Note | If a failed unit does not recover and you believe it should not be failed, you can reset the state by entering the failover reset command. If the failover condition persists, however, the unit will fail again | 
Interface tests
Interface tests are monitoring mechanisms that:
- 
                                          
                                          evaluate interface operational status during failover scenarios
- 
                                          
                                          run sequentially with approximately 1.5 second duration per test by default, or 1/16 of the failover interface holdtime(see the failover polltime interface command, or for Active/Active failover, the interface-policy command), and
- 
                                          
                                          determine interface failure status on ASA units.
Interface test sequence
The ASA uses these interface tests in sequence:
- 
                                          
                                          Link Up/Down test—A test of the interface status. If the Link Up/Down test indicates that the interface is down, then the ASA considers it failed, and testing stops. If the status is Up, then the ASA performs the Network Activity test.
- 
                                          
                                          Network Activity test—A received network activity test. At the start of the test, each unit clears its received packet count for its interfaces. As soon as a unit receives any eligible packets during the test, then the interface is considered operational. If both units receive traffic, then testing stops. If one unit receives traffic and the other unit does not, then the interface on the unit that does not receive traffic is considered failed, and testing stops. If neither unit receives traffic, then the ASA starts the ARP test.
- 
                                          
                                          ARP test—A test for successful ARP replies. Each unit sends a single ARP request for the IP address in the most recent entry in its ARP table. If the unit receives an ARP reply or other network traffic during the test, then the interface is considered operational. If the unit does not receive an ARP reply, then the ASA sends a single ARP request for the IP address in the next entry in the ARP table. If the unit receives an ARP reply or other network traffic during the test, then the interface is considered operational. If both units receive traffic, then testing stops. If one unit receives traffic, and the other unit does not, then the interface on the unit that does not receive traffic is considered failed, and testing stops. If neither unit receives traffic, then the ASA starts the Broadcast Ping test.
- 
                                          
                                          Broadcast Ping test—A test for successful ping replies. Each unit sends a broadcast ping, and then counts all received packets. If the unit receives any packets during the test, then the interface is considered operational. If both units receive traffic, then testing stops. If one unit receives traffic, and the other unit does not, then the interface on the unit that does not receive traffic is considered failed, and testing stops. If neither unit receives traffic, then testing starts over again with the ARP test. If both units continue to receive no traffic from the ARP and Broadcast Ping tests, then these tests will continue running in perpetuity.
Interface status
Interface status is a monitoring indicator that
- 
                                          
                                          shows the current operational state of monitored interfaces in a failover configuration
- 
                                          
                                          tracks traffic reception and hello packet communication between peer units, and
- 
                                          
                                          determines whether an interface is actively monitored by the failover process.
Interface status types
Monitored interfaces can have these status types:
- 
                                          
                                          Unknown: Initial status. This status can also mean the status cannot be determined.
- 
                                          
                                          Normal: The interface is receiving traffic.
- 
                                          
                                          Testing: Hello messages are not heard on the interface for five poll times.
- 
                                          
                                          Link Down: The interface or VLAN is administratively down.
- 
                                          
                                          No Link: The physical link for the interface is down.
- 
                                          
                                          Failed: No traffic is received on the interface, yet traffic is heard on the peer interface.
Failover times
Failover times are high availability mechanisms that:
- 
                                    
                                    initiate automatic switchover when specific failure conditions occur on the active unit,
- 
                                    
                                    monitor system health through configurable detection thresholds and timing parameters, and
- 
                                    
                                    ensure service continuity by transferring control to the standby unit when failures exceed defined limits.
Failover triggering events and timing parameters
These events trigger failover in a Firepower high availability pair:
- 
                                    
                                    More than 50% of the Snort instances on the active unit are down.
- 
                                    
                                    Disk space on the active unit is more than 90% full.
- 
                                    
                                    The no failover active command is run on the active unit or the failover active command is run on the standby unit.
- 
                                    
                                    The active unit has more failed interfaces than the standby unit.
- 
                                    
                                    Interface failure on the active device exceeds the threshold configured. By default, failure of a single interface causes failover. You can change the default value by configuring a threshold for the number of interfaces or a percentage of monitored interfaces that must fail for the failover to occur. If the threshold is exceeded on the active device, failover occurs. If the threshold is exceeded on the standby device, the unit moves to Fail state. To change the default failover criteria, enter this command in global configuration mode: Table 2. Interface policy command Command Purpose failover interface-policy num [%] hostname (config)# failover interface-policy 20%Changes the default failover criteria. When specifying a specific number of interfaces, the num argument can be from 1 to 250. When specifying a percentage of interfaces, the num argument can be from 1 to 100.
| Note | If you manually fail over using the CLI or ASDM, or you reload the ASA, the failover starts immediately and is not subject to the timers listed below. | 
| Table 3. ASA |  |  |  | 
|---|---|---|---|
| Failover condition | Minimum | Default | Maximum | 
|---|---|---|---|
| Active unit loses power, hardware goes down, or the software reloads or crashes. When any of these occur, the monitored interfaces or failover link do not receives any hello message. | 800 milliseconds | 15 seconds | 45 seconds | 
| Active unit main board interface link down. | 500 milliseconds | 5 seconds | 15 seconds | 
| Active unit 4GE module interface link down. | 2 seconds | 5 seconds | 15 seconds | 
| Active unit interface up, but connection problem causes interface testing. | 5 seconds | 25 seconds | 75 seconds | 
Configuration Synchronization
Failover includes various types of configuration synchronization.
Running Configuration Replication
Running configuration replication occurs when any one or both of the devices in the failover pair boot.
In Active/Standby failover, configurations are always synchronized from the active unit to the standby unit.
In Active/Active failover, whichever unit boots second obtains the running configuration from the unit that boots first, regardless of the primary or secondary designation of the booting unit. After both units are up, commands entered in the system execution space are replicated from the unit on which failover group 1 is in the active state.
When the standby/second unit completes its initial startup, it clears its running configuration (except for the failover commands needed to communicate with the active unit), and the active unit sends its entire configuration to the standby/second unit. When the replication starts, the ASA console on the active unit displays the message “Beginning configuration replication: Sending to mate,” and when it is complete, the ASA displays the message “End Configuration Replication to mate.” Depending on the size of the configuration, replication can take from a few seconds to several minutes.
On the unit receiving the configuration, the configuration exists only in running memory. You should save the configuration to flash memory according to Save Configuration Changes. For example, in Active/Active failover, enter the write memory all command in the system execution space on the unit that has failover group 1 in the active state. The command is replicated to the peer unit, which proceeds to write its configuration to flash memory.
| Note | During replication, commands entered on the unit sending the configuration may not replicate properly to the peer unit, and commands entered on the unit receiving the configuration may be overwritten by the configuration being received. Avoid entering commands on either unit in the failover pair during the configuration replication process. | 
File Replication
Configuration syncing does not replicate the following files and configuration components, so you must copy these files manually so they match:
- 
                                    Secure Client images
- 
                                    CSD images
- 
                                    Secure Client profiles The ASA uses a cached file for the Secure Client profile stored in cache:/stc/profiles, and not the file stored in the flash file system. To replicate the Secure Client profile to the standby unit, perform one of the following: 
  - 
                                          Enter the write standby command on the active unit.
  - 
                                          Reapply the profile on the active unit.
  - 
                                          Reload the standby unit.
- 
                                          
- 
                                    Local Certificate Authorities (CAs)
- 
                                    ASA images
- 
                                    ASDM images
Command Replication
After startup, commands that you enter on the active unit are immediately replicated on the standby unit. You do not have to save the active configuration to flash memory to replicate the commands.
In Active/Active failover, commands entered in the system execution space are replicated from the unit on which failover group 1 is in the active state.
Failure to enter the commands on the appropriate unit for command replication to occur causes the configurations to be out of synchronization. Those changes may be lost the next time the initial configuration synchronization occurs.
The following commands are replicated to the standby ASA:
- 
                                    All configuration commands except for mode, firewall , and failover lan unit
- 
                                    copy running-config startup-config
- 
                                    delete
- 
                                    mkdir
- 
                                    rename
- 
                                    rmdir
- 
                                    write memory
The following commands are not replicated to the standby ASA:
- 
                                    All forms of the copy command except for copy running-config startup-config
- 
                                    All forms of the write command except for write memory
- 
                                    debug
- 
                                    failover lan unit
- 
                                    firewall
- 
                                    show
- 
                                    terminal pager and pager
Config-sync optimization
Config-sync optimization is a configuration synchronization enhancement that
- 
                                       
                                       compares configuration hash values between active and joining devices to determine if full synchronization is necessary
- 
                                       
                                       enables faster device rejoining by skipping full configuration synchronization when hash values match, and
- 
                                       
                                       reduces maintenance window and upgrade time during failover operations.
Guidelines and limitations of configuration sync optimization
When a device reboots or rejoins after a suspend or resume failover, the joining device clears its running configuration. The active device then sends its entire configuration to the joining device for a full configuration synchronization. If the active device has a large configuration, this process can take several minutes.
The configuration sync optimization functionality enables comparing the configuration of the joining device and the active device by exchanging configuration hash values. If the hash computed on both active and joining devices match, the joining device skips full configuration synchronization and rejoin the failover configuration. This functionality ensures faster peering and reduces maintenance window and upgrade time.
Guidelines and limitations:
- 
                                       
                                       The configuration sync optimization functionality is enabled by default.
- 
                                       
                                       ASA multiple context mode supports configuration sync optimization by sharing the context order during full configuration synchronization, allowing comparison of context order during subsequent node-rejoin.
- 
                                       
                                       If you configure passphrase and failover IPsec key, then configuration sync optimization is not effective as the hash value computed in the active and standby devices differs.
- 
                                       
                                       If you configure the device with dynamic ACL or SNMPv3, configuration sync optimization is not effective.
- 
                                       
                                       Active device synchronizes full configuration with flapping LAN links as default behavior. During failover flaps between active and standby devices, configuration sync optimization is not triggered and devices perform a full configuration synchronization.
- 
                                       
                                       Configuration sync optimization gets triggered when the failover configuration recovers from an interruption or loss of network communication between the active and standby devices.
Monitoring config-sync optimization:
- 
                                       
                                       Syslog messages are generated displaying whether the hash values computed on the active and joining unit match, does not match, or if the operation timeout expires.
- 
                                       
                                       The syslog message also displays the time elapsed, from the time of sending the hash request to the time of getting and comparing the hash response.
Monitoring commands for configuration sync optimization.
- 
                                       
                                       show failover config-sync checksum: Displays information about the device status and checksum.
- 
                                       
                                       show failover config-sync configuration: Displays information about the device configuration and checksum.
- 
                                       
                                       show failover config-sync status: Displays status of configuration sync optimization functionality.
| Note | The Configuration State in show failover state command displays the config sync state status during HA joining. This state does not reflect the later config deployments or changes, and replication on device until a resync has been initiated. | 
Active/standby failover
Active/standby failover is a high availability configuration that
- 
                                    
                                    lets you use a standby ASA device to take over the functionality of a failed unit,
- 
                                    
                                    enables the standby unit to become the active unit when the active unit fails, and
- 
                                    
                                    allows the failed unit to come back online as the standby unit if the problem is temporary.
The failed unit must be replaced if the problem is not temporary. Set the standby unit to primary before replacing the failed unit to retain the configuration of the secondary unit.
For multiple context mode, the ASA can fail over the entire unit (including all contexts) but cannot fail over individual contexts separately.
Primary/secondary roles and active/standby status
Primary/secondary roles and active/standby status are failover configuration concepts that
- 
                                       
                                       define unit hierarchy, with primary units taking precedence during simultaneous startup
- 
                                       
                                       determine IP address and MAC address usage patterns during failover operations, and
- 
                                       
                                       establish traffic flow responsibilities between paired units in Active/Standby failover configurations.
Primary and secondary unit differences
The main differences between the two units in a failover pair are related to which unit is active and which unit is standby, namely which IP addresses to use and which unit actively passes traffic.
However, a few differences exist between the units depending on which unit is primary (as specified in the configuration) and which unit is secondary:
- 
                                       
                                       The primary unit always becomes the active unit if both units start up at the same time (and are of equal operational health).
- 
                                       
                                       The primary unit MAC addresses are always coupled with the active IP addresses. The exception to this rule occurs when the secondary unit becomes active and cannot obtain the primary unit MAC addresses over the failover link. In this case, the secondary unit MAC addresses are used.
Active unit determination at startup
Active unit determination at startup is a high availability process that
- 
                                       
                                       sets a unit to standby if it boots and detects a peer already running as active,
- 
                                       
                                       sets a unit to active if it boots and does not detect a peer, and
- 
                                       
                                       sets the primary unit as active and secondary unit as standby if both units boot simultaneously.
Failover events
A failover event is a system condition that
- 
                                       
                                       triggers an active unit to transfer control to a standby unit in Active/Standby failover configurations
- 
                                       
                                       occurs on a unit basis and cannot fail over individual or groups of contexts even on systems running in multiple context mode, and
- 
                                       
                                       follows specific failover policies that determine whether failover occurs and what actions each unit takes.
Failover event policies and actions
This table shows the failover action for each failure event. For each failure event, the table shows the failover policy (failover or no failover), the action taken by the active unit, the action taken by the standby unit, and any special notes about the failover condition and actions.
| Table 4. Failover events |  |  |  |  | 
|---|---|---|---|---|
| Failure Event | Policy | Active Unit Action | Standby Unit Action | Notes | 
|---|---|---|---|---|
| Active unit failed (power or hardware) | Failover | n/a | Become active Mark active as failed | No hello messages are received on any monitored interface or the failover link. | 
| Formerly active unit recovers | No failover | Become standby | No action | None. | 
| Standby unit failed (power or hardware) | No failover | Mark standby as failed | n/a | When the standby unit is marked as failed, then the active unit does not attempt to fail over, even if the interface failure threshold is surpassed. | 
| Failover link failed during operation | No failover | Mark failover link as failed | Mark failover link as failed | You should restore the failover link as soon as possible because the unit cannot fail over to the standby unit while the failover link is down. | 
| Failover link failed at startup | No failover | Become active Mark failover link as failed | Become active Mark failover link as failed | If the failover link is down at startup, both units become active. | 
| State link failed | No failover | No action | No action | State information becomes out of date, and sessions are terminated if a failover occurs. | 
| Interface failure on active unit above threshold | Failover | Mark active as failed | Become active | None. | 
| Interface failure on standby unit above threshold | No failover | No action | Mark standby as failed | When the standby unit is marked as failed, then the active unit does not attempt to fail over even if the interface failure threshold is surpassed. | 
About Active/Active Failover
This section describes Active/Active failover.
Active/Active Failover Overview
In an Active/Active failover configuration, both ASAs can pass network traffic. Active/Active failover is only available to ASAs in multiple context mode. In Active/Active failover, you divide the security contexts on the ASA into a maximum of 2 failover groups.
A failover group is simply a logical group of one or more security contexts. You can assign failover group to be active on the primary ASA, and failover group 2 to be active on the secondary ASA. When a failover occurs, it occurs at the failover group level. For example, depending on interface failure patterns, it is possible for failover group 1 to fail over to the secondary ASA, and subsequently failover group 2 to fail over to the primary ASA. This event could occur if the interfaces in failover group 1 are down on the primary ASA but up on the secondary ASA, while the interfaces in failover group 2 are down on the secondary ASA but up on the primary ASA.
The admin context is always a member of failover group 1. Any unassigned security contexts are also members of failover group 1 by default. If you want Active/Active failover, but are otherwise uninterested in multiple contexts, the simplest configuration would be to add one additional context and assign it to failover group 2.
| Note | When configuring Active/Active failover, make sure that the combined traffic for both units is within the capacity of each unit. | 
| Note | You can assign both failover groups to one ASA if desired, but then you are not taking advantage of having two active ASAs. | 
Primary/Secondary Roles and Active/Standby Status for a Failover Group
As in Active/Standby failover, one unit in an Active/Active failover pair is designated the primary unit, and the other unit the secondary unit. Unlike Active/Standby failover, this designation does not indicate which unit becomes active when both units start simultaneously. Instead, the primary/secondary designation does two things:
-  
                                    		  
                                    The primary unit provides the running configuration to the pair when they boot simultaneously.
-  
                                    		  
                                    Each failover group in the configuration is configured with a primary or secondary unit preference. When used with preemption, this preference ensures that the failover group runs on the correct unit after it starts up. Without preemption, both groups run on the first unit to boot up.
Active Unit Determination for Failover Groups at Startup
The unit on which a failover group becomes active is determined as follows:
- 
                                    		  
                                    When a unit boots while the peer unit is not available, both failover groups become active on the unit.
- 
                                    		  
                                    When a unit boots while the peer unit is active (with both failover groups in the active state), the failover groups remain in the active state on the active unit regardless of the primary or secondary preference of the failover group until one of the following occurs: 
  - 
                                          				
                                          A failover occurs.
  - 
                                          				
                                          A failover is manually forced.
  - 
                                          				
                                          A preemption for the failover group is configured, which causes the failover group to automatically become active on the preferred unit when the unit becomes available.
- 
                                          				
                                          
Failover Events
In an Active/Active failover configuration, failover occurs on a failover group basis, not a system basis. For example, if you designate both failover groups as Active on the primary unit, and failover group 1 fails, then failover group 2 remains Active on the primary unit while failover group 1 becomes active on the secondary unit.
Because a failover group can contain multiple contexts, and each context can contain multiple interfaces, it is possible for all interfaces in a single context to fail without causing the associated failover group to fail.
The following table shows the failover action for each failure event. For each failure event, the policy (whether or not failover occurs), actions for the active failover group, and actions for the standby failover group are given.
| Table 5. Failover Events |  |  |  |  | 
|---|---|---|---|---|
| Failure Event | Policy | Active Group Action | Standby Group Action | Notes | 
|---|---|---|---|---|
| A unit experiences a power or software failure | Failover | Become standby Mark as failed | Become active Mark active as failed | When a unit in a failover pair fails, any active failover groups on that unit are marked as failed and become active on the peer unit. | 
| Interface failure on active failover group above threshold | Failover | Mark active group as failed | Become active | None. | 
| Interface failure on standby failover group above threshold | No failover | No action | Mark standby group as failed | When the standby failover group is marked as failed, the active failover group does not attempt to fail over, even if the interface failure threshold is surpassed. | 
| Formerly active failover group recovers | No failover | No action | No action | Unless failover group preemption is configured, the failover groups remain active on their current unit. | 
| Failover link failed at startup | No failover | Become active | Become active | If the failover link is down at startup, both failover groups on both units become active. | 
| State link failed | No failover | No action | No action | State information becomes out of date, and sessions are terminated if a failover occurs. | 
| Failover link failed during operation | No failover | n/a | n/a | Each unit marks the failover link as failed. You should restore the failover link as soon as possible because the unit cannot fail over to the standby unit while the failover link is down. |