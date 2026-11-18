Configuring HSRP 
    
    
 
    
 
  This chapter describes how to configure the Hot Standby Router Protocol ( HSRP) on the Cisco NX-OS device.
Information About HSRP
HSRP is a first-hop redundancy protocol ( FHRP) that allows a transparent failover of the first-hop IP router. HSRP provides first-hop routing redundancy for IP hosts on Ethernet networks configured with a default router IP address. You use HSRP in a group of routers for selecting an active router and a standby router. In a group of routers, the active router is the router that routes packets; the standby router is the router that takes over when the active router fails or when preset conditions are met.
Many host implementations do not support any dynamic router discovery mechanisms but can be configured with a default router. Running a dynamic router discovery mechanism on every host is not practical for many reasons, including administrative overhead, processing overhead, and security issues. HSRP provides failover services to these hosts.
This section includes the following topics:
HSRP Overview
When you use HSRP, you configure the HSRP virtual IP address as the host’s default router (instead of the IP address of the actual router). The virtual IP address is an IPv4 or IPv6 address that is shared among a group of routers that run HSRP.
When you configure HSRP on a network segment, you provide a virtual MAC address and a virtual IP address for the HSRP group. You configure the same virtual address on each HSRP-enabled interface in the group. You also configure a unique IP address and MAC address on each interface that acts as the real address. HSRP selects one of these interfaces to be the active router. The active router receives and routes packets destined for the virtual MAC address of the group.
HSRP detects when the designated active router fails. At that point, a selected standby router assumes control of the virtual MAC and IP addresses of the HSRP group. HSRP also selects a new standby router at that time.
HSRP uses a priority designator to determine which HSRP-configured interface becomes the default active router. To configure an interface as the active router, you assign it with a priority that is higher than the priority of all the other HSRP-configured interfaces in the group. The default priority is 100, so if you configure just one interface with a higher priority, that interface becomes the default active router.
Interfaces that run HSRP send and receive multicast User Datagram Protocol (UDP)-based hello messages to detect a failure and to designate active and standby routers. When the active router fails to send a hello message within a configurable period of time, the standby router with the highest priority becomes the active router. The transition of packet forwarding functions between the active and standby router is completely transparent to all hosts on the network.
You can configure multiple HSRP groups on an interface.
Figure 17-1 shows a network configured for HSRP. By sharing a virtual MAC address and a virtual IP address, two or more interfaces can act as a single virtual router.
Figure 17-1 HSRP Topology with Two Enabled Routers
The virtual router does not physically exist but represents the common default router for interfaces that are configured to provide backup to each other. You do not need to configure the hosts on the LAN with the IP address of the active router. Instead, you configure them with the IP address of the virtual router (virtual IP address) as their default router. If the active router fails to send a hello message within the configurable period of time, the standby router takes over, responds to the virtual addresses, and becomes the active router, assuming the active router duties. From the host perspective, the virtual router remains the same.
Note Packets received on a routed port destined for the HSRP virtual IP address terminate on the local router, regardless of whether that router is the active HSRP router or the standby HSRP router. This process includes ping and Telnet traffic. Packets received on a Layer 2 (VLAN) interface destined for the HSRP virtual IP address terminate on the active router.
HSRP Versions
Cisco NX-OS supports HSRP version 1 by default. You can configure an interface to use HSRP version 2.
HSRP version 2 has the following enhancements to HSRP version 1:
- Expands the group number range. HSRP version 1 supports group numbers from 0 to 255. HSRP version 2 supports group numbers from 0 to 4095.
- For IPv4, uses the IPv4 multicast address 224.0.0.102 or the IPv6 multicast address FF02::66 to send hello packets instead of the multicast address of 224.0.0.2, which is used by HSRP version 1.
- Uses the MAC address range from 0000.0C9F.F000 to 0000.0C9F.FFFF for IPv4 and 0005.73A0.0000 through 0005.73A0.0FFF for IPv6 addresses. HSRP version 1 uses the MAC address range 0000.0C07.AC00 to 0000.0C07.ACFF.
- Adds support for MD5 authentication.
When you change the HSRP version, Cisco NX-OS reinitializes the group because it now has a new virtual MAC address.
HSRP version 2 has a different packet format than HSRP version 1. The packet format uses a type-length-value (TLV) format. HSRP version 2 packets received by an HSRP version 1 router are ignored.
HSRP for IPv4
HSRP routers communicate with each other by exchanging HSRP hello packets. These packets are sent to the destination IP multicast address 224.0.0.2 (reserved multicast address used to communicate to all routers) on UDP port 1985. The active router sources hello packets from its configured IP address and the HSRP virtual MAC address while the standby router sources hellos from its configured IP address and the interface MAC address, which might be the burned-in address (BIA). The BIA is the last six bytes of the MAC address that is assigned by the manufacturer of the network interface card (NIC).
Because hosts are configured with their default router as the HSRP virtual IP address, hosts must communicate with the MAC address associated with the HSRP virtual IP address. This MAC address is a virtual MAC address, 0000.0C07.ACxy, where xy is the HSRP group number in hexadecimal based on the respective interface. For example, HSRP group 1 uses the HSRP virtual MAC address of 0000.0C07.AC01. Hosts on the adjoining LAN segment use the normal Address Resolution Protocol (ARP) process to resolve the associated MAC addresses.
HSRP version 2 uses the new IP multicast address 224.0.0.102 to send hello packets instead of the multicast address of 224.0.0.2, which is used by version 1. HSRP version 2 permits an expanded group number range of 0 to 4095 and uses a new MAC address range of 0000.0C9F.F000 to 0000.0C9F.FFFF.
HSRP for IPv6
IPv6 hosts learn of available IPv6 routers through IPv6 neighbor discovery (ND) router advertisement (RA) messages. These messages are multicast periodically, or might be solicited by hosts, but the time delay for detecting when a default route is down might be 30 seconds or more. HSRP for IPv6 provides a much faster switchover to an alternate default router than the IPv6 ND protocol provides, which is less than a second if the milliseconds timers are used. HSRP for IPv6 provides a virtual first hop for IPv6 hosts.
When you configure an IPv6 interface for HSRP, the periodic RAs for the interface link-local address stop after IPv6 ND sends a final RA with a router lifetime of zero. No restrictions occur for the interface IPv6 link-local address. Other protocols continue to receive and send packets to this address.
IPv6 ND sends periodic RAs for the HSRP virtual IPv6 link-local address when the HSRP group is active. These RAs stop after a final RA is sent with a router lifetime of 0 when the HSRP group leaves the active state. HSRP uses the virtual MAC address for active HSRP group messages only (hello, coup, and resign).
HSRP IPv6 Addresses
An HSRP IPv6 group has a virtual MAC address that is derived from the HSRP group number and a virtual IPv6 link-local address that is derived, by default, from the HSRP virtual MAC address. The default virtual MAC address for an HSRP IPv6 group is always used to form the virtual IPv6 link-local address, regardless of the actual virtual MAC address used by the group.
Table 17-1 shows the MAC and IP addresses used for IPv6 neighbor discovery packets and HSRP packets.
|  |  |  |  |  | 
|---|---|---|---|---|
| Neighbor solicitation (NS) | Interface MAC address | Interface IPv6 address | — | Interface MAC address | 
| Router solicitation (RS) | Interface MAC address | Interface IPv6 address | — | Interface MAC address | 
| Neighbor advertisement (NA) | Interface MAC address | Interface IPv6 address | Virtual IPv6 address | HSRP virtual MAC address | 
| Route advertisement (RA) | Interface MAC address | Virtual IPv6 address | — | HSRP virtual MAC address | 
| HSRP (inactive) | Interface MAC address | Interface IPv6 address | — | — | 
| HSRP (active) | Virtual MAC address | Interface IPv6 address | — | — | 
HSRP does not add IPv6 link-local addresses to the Unicast Routing Information Base (URIB). Link-local addresses have no secondary virtual IP addresses.
For global unicast addresses, HSRP adds the virtual IPv6 address to the URIB and IPv6.
HSRP Authentication
HSRP message digest 5 (MD5) algorithm authentication protects against HSRP-spoofing software and uses the industry-standard MD5 algorithm for improved reliability and security. HSRP includes the IPv4 or IPv6 address in the authentication TLVs.
HSRP Messages
Routers that are configured with HSRP exchange the following three types of multicast messages:
- Hello—The hello message conveys the HSRP priority and state information of the router to other HSRP routers.
- Coup—When a standby router wants to assume the function of the active router, it sends a coup message.
- Resign—A router that is the active router sends this message when it is about to shut down or when a router that has a higher priority sends a hello or coup message.
HSRP Load Sharing
HSRP allows you to configure multiple groups on an interface. You can configure two overlapping IPv4 HSRP groups to load share traffic from the connected hosts while providing the default router redundancy expected from HSRP. Figure 17-2 shows an example of a load-sharing HSRP IPv4 configuration.
Figure 17-2 shows two routers A and B and two HSRP groups. Router A is the active router for group A but is the standby router for group B. Similarly, router B is the active router for group B and the standby router for group A. If both routers remain active, HSRP load balances the traffic from the hosts across both routers. If either router fails, the remaining router continues to process traffic for both hosts.
Note HSRP for IPv6 load balances by default. If two HSRP IPv6 groups are on the subnet, hosts learn of both groups from their router advertisements and choose to use one so that the load is shared between the advertised routers.
Object Tracking and HSRP
You can use object tracking to modify the priority of an HSRP interface based on the operational state of another interface. Object tracking allows you to route to a standby router if the interface to the main network fails.
Two objects that you can track are the line protocol state of an interface or the reachability of an IP route. If the specified object goes down, Cisco NX-OS reduces the HSRP priority by the configured amount. For more information, see the “Configuring HSRP Object Tracking” section.
vPC and HSRP
HSRP interoperates with virtual port channels (vPCs). vPCs allow links that are physically connected to two different Cisco Nexus 9000 Series devices to appear as a single port channel by a third device. See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for more information on vPCs.
vPC forwards traffic through both the active HSRP router and the standby HSRP router. For more information, see the “Configuring the HSRP Priority” section and the “Configuration Examples for HSRP” section.
Note HSRP active can be distributed on both the primary and secondary vPC peers for different SVIs.
vPC Peer Gateway and HSRP
Some third-party devices can ignore the HSRP virtual MAC address and instead use the source MAC address of an HSRP router. In a vPC environment, the packets that use this source MAC address might be sent across the vPC peer link, causing a potential dropped packet. Configure the vPC peer gateway to enable the HSRP routers to directly handle packets sent to the local vPC peer MAC address and the remote vPC peer MAC address, as well as the HSRP virtual MAC address. See the Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide for more information on the vPC peer gateway.
BFD
This feature supports bidirectional forwarding detection (BFD). BFD is a detection protocol that provides fast-forwarding and path-failure detection times. BFD provides subsecond failure detection between two adjacent devices and can be less CPU-intensive than protocol hello messages because some of the BFD load can be distributed onto the data plane on supported modules. See the Cisco Nexus 9000 Series NX-OS Interfaces Configuration Guide for more information.
High Availability and Extended Nonstop Forwarding
HSRP supports stateful restarts and stateful switchovers. A stateful restart occurs when the HSRP process fails and is restarted. A stateful switchover occurs when the active supervisor switches to the standby supervisor. Cisco NX-OS applies the run-time configuration after the switchover.
If HSRP hold timers are configured for short time periods, these timers might expire during a controlled switchover. HSRP supports extended nonstop forwarding (NSF) to temporarily extend these HSRP hold timers during a controlled switchover.
With extended NSF configured, HSRP sends hello messages with the extended timers. HSRP peers update their hold timers with these new values. The extended timers prevent unnecessary HSRP state changes during the switchover. After the switchover, HSRP restores the hold timers to their original configured values. If the switchover fails, HSRP restores the hold timers after the extended hold timer values expire.
See the “Configuring Extended Hold Timers for HSRP” section for more information.
Virtualization Support
HSRP supports virtual routing and forwarding (VRF) instances.
Licensing Requirements for HSRP
The following table shows the licensing requirements for this feature:
Prerequisites for HSRP
Guidelines and Limitations for HSRP
HSRP has the following configuration guidelines and limitations:
- You must configure an IP address for the interface that you configure HSRP on and enable that interface before HSRP becomes active.
- You must configure HSRP version 2 when you configure an IPv6 interface for HSRP.
- For IPv4, the virtual IP address must be in the same subnet as the interface IP address.
- We recommend that you do not configure more than one first-hop redundancy protocol on the same interface.
- HSRP version 2 does not interoperate with HSRP version 1. An interface cannot operate both version 1 and version 2 because both versions are mutually exclusive. However, the different versions can be run on different physical interfaces of the same router.
- You cannot change from version 2 to version 1 if you have configured groups above the group number range allowed for version 1 (0 to 255).
- HSRP for IPv4 is supported with BFD. HSRP for IPv6 is not supported with BFD.
- If HSRP IPv4 and IPv6 use the same virtual MAC address on an SVI, the HSRP state should be the same for both HSRP IPv4 and IPv6. The priority and preemption should be configured to result in the same state after failovers.
- Cisco NX-OS removes all Layer 3 configurations on an interface when you change the interface VRF membership, port channel membership, or the port mode to Layer 2.
- If you configure virtual MAC addresses with vPC, you must configure the same virtual MAC address on both vPC peers.
- You cannot use the HSRP MAC address burned-in option on a VLAN interface that is a vPC member.
- Cisco NX-OS does not support having the same HSRP groups on all nodes in a double-sided vPC.
- If you have not configured authentication, the show hsrp command displays the following string:
The default behavior of HSRP is as defined in RFC 2281 :
value is 0x63 0x69 0x73 0x63 0x6F 0x00 0x00 0x00.
Default Settings
Table 17-2 lists the default settings for HSRP parameters.
Configuring HSRP
This section includes the following topics:
Note If you are familiar with the Cisco IOS CLI, be aware that the Cisco NX-OS commands for this feature might differ from the Cisco IOS commands that you would use.
Enabling HSRP
You must globally enable HSRP before you can configure and enable any HSRP groups.
DETAILED STEPS
To enable the HSRP feature, use the following command in global configuration mode:
To disable the HSRP feature and remove all associated configurations, use the following command in global configuration mode:
Configuring the HSRP Version
You can configure the HSRP version. If you change the version for existing groups, Cisco NX-OS reinitializes HSRP for those groups because the virtual MAC address changes. The HSRP version applies to all groups on the interface.
Note IPv6 HSRP groups must be configured as HSRP version 2.
To configure the HSRP version, use the following command in interface configuration mode:
Configuring an HSRP Group for IPv4
You can configure an HSRP group on an IPv4 interface and configure the virtual IP address and virtual MAC address for the HSRP group.
BEFORE YOU BEGIN
Ensure that you have enabled the HSRP feature (see the “Enabling HSRP” section).
Cisco NX-OS enables an HSRP group once you configure its virtual IP address. You should configure HSRP attributes such as authentication, timers, and priority before you enable the HSRP group.
SUMMARY STEPS
3. ip address ip-address/length
5. ip [ ip-address [ secondary ]]
DETAILED STEPS
|  |  |  | 
|---|---|---|
| Step 1 | configure terminal  Example: switch# configure terminal switch(config)# | Enters global configuration mode. | 
| Step 2 | interface type number  Example: switch(config)# interface ethernet 1/2 switch(config-if)# | Enters interface configuration mode. | 
| Step 3 | ip address ip-address/length  Example : switch(config-if)# ip 192.0.2.2/8 | Configures the IPv4 address of the interface. | 
| Step 4 | hsrp group-number [ ipv4 ]  Example : switch(config-if)# hsrp 2 switch(config-if-hsrp)# | Creates an HSRP group and enters HSRP configuration mode. The range for HSRP version 1 is from 0 to 255. The range is for HSRP version 2 is from 0 to 4095. The default value is 0. | 
| Step 5 | ip [ ip-address [ secondary ]]  Example : switch(config-if-hsrp)# ip 192.0.2.1 | Configures the virtual IP address for the HSRP group and enables the group. This address should be in the same subnet as the IPv4 address of the interface. | 
| Step 6 | exit  Example : switch(config-if-hsrp)# exit | Exits HSRP configuration mode. | 
| Step 7 | no shutdown  Example : switch(config-if)# no shutdown | Enables the interface. | 
| Step 8 | show hsrp [ group group-number ] [ ipv4 ]  Example : switch(config-if)# show hsrp group 2 | (Optional) Displays HSRP information. | 
| Step 9 | copy running-config startup-config  Example: switch(config-if)# copy running-config startup-config | (Optional) Copies the running configuration to the startup configuration. | 
Note You should use the no shutdown command to enable the interface after you finish the configuration.
This example shows how to configure an HSRP group on Ethernet 1/2:
switch(config)# interface ethernet 1/2
switch(config-if)# ip 192.0.2.2/8
switch(config-if-hsrp)# ip 192.0.2.1
Configuring an HSRP Group for IPv6
You can configure an HSRP group on an IPv6 interface and configure the virtual MAC address for the HSRP group.
When you configure an HSRP group for IPv6, HSRP generates a link-local address from the link-local prefix. HSRP also generates a modified EUI-64 format interface identifier in which the EUI-64 interface identifier is created from the relevant HSRP virtual MAC address.
BEFORE YOU BEGIN
You must enable HSRP (see the “Enabling HSRP” section).
Ensure that you have enabled HSRP version 2 on the interface that you want to configure an IPv6 HSRP group on.
Ensure that you have configured HSRP attributes such as authentication, timers, and priority before you enable the HSRP group.
SUMMARY STEPS
3. ipv6 address ipv6-address/length
DETAILED STEPS
|  |  |  | 
|---|---|---|
| Step 1 | configure terminal  Example: switch# configure terminal switch(config)# | Enters global configuration mode. | 
| Step 2 | interface type number  Example: switch(config)# interface ethernet 3/2 switch(config-if)# | Enters interface configuration mode. | 
| Step 3 | ipv6 address ipv6-address/length  Example : switch(config-if)# ipv6 address 2001:0DB8::0001:0001/64 | Configures the IPv6 address of the interface. | 
| Step 4 | hsrp version 2  Example : switch(config-if-hsrp)# hsrp version 2 | Configures this group for HSRP version 2. | 
| Step 5 | hsrp group-number ipv6  Example : switch(config-if)# hsrp 10 ipv6 switch(config-if-hsrp)# | Creates an IPv6 HSRP group and enters HSRP configuration mode. The range for HSRP version 2 is from 0 to 4095. The default value is 0. | 
| Step 6 | ip ipv6-address  Example : switch(config-if-hsrp)# ip 2001:DB8::1 | Configures the virtual IPv6 address for the HSRP group and enables the group. | 
| Step 7 | ip autoconfig  Example : switch(config-if-hsrp)# ip autoconfig | Autoconfigures the virtual IPv6 address for the HSRP group from the calculated link-local virtual IPv6 address and enables the group. | 
| Step 8 | exit  Example : switch(config-if-hsrp)# exit switch(config-if)# | Exits HSRP configuration mode. | 
| Step 9 | no shutdown  Example : switch(config-if)# no shutdown | Enables the interface. | 
| Step 10 | show hsrp [ group group-number ] [ ipv6 ]  Example : switch(config-if)# show hsrp group 10 | (Optional) Displays HSRP information. | 
| Step 11 | copy running-config startup-config  Example: switch(config-if)# copy running-config startup-config | (Optional) Copies the running configuration to the startup configuration. | 
Note You should use the no shutdown command to enable the interface after you finish the configuration.
This example shows how to configure an IPv6 HSRP group on Ethernet 3/2:
switch(config)# interface ethernet 3/2
switch(config-if)# ipv6 address 2001:0DB8::0001:0001/64
switch(config-if-hsrp)# hsrp version 2
switch(config-if)# hsrp 2 ipv6
switch(config-if-hsrp)# ip 2001:DB8::1
Configuring the HSRP Virtual MAC Address
You can override the default virtual MAC address that HSRP derives from the configured group number.
Note You must configure the same virtual MAC address on both vPC peers of a vPC link.
To manually configure the virtual MAC address for an HSRP group, use the following command in hsrp configuration mode:
To configure HSRP to use the burned-in MAC address of the interface for the virtual MAC address, use the following command in interface configuration mode:
|  |  | 
|---|---|
| hsrp use-bia [ scope interface ]  Example : switch(config-if)# hsrp use-bia | Configures HSRP to use the burned-in MAC address of the interface for the HSRP virtual MAC address. You can optionally configure HSRP to use the burned-in MAC address for all groups on this interface by using the scope interface keyword. | 
Authenticating HSRP
You can configure HSRP to authenticate the protocol using cleartext or MD5 digest authentication. MD5 authentication uses a key chain (see the Cisco Nexus 9000 Series NX-OS Security Configuration Guide).
BEFORE YOU BEGIN
You must enable HSRP (see the “Enabling HSRP” section).
You must configure the same authentication and keys on all members of the HSRP group.
Ensure that you have created the key chain if you are using MD5 authentication.
SUMMARY STEPS
2. interface interface- type slot/port
3. hsrp group- number [ ipv4 | ipv6 ]
authentication md5 { key-chain key-chain | key-string { 0 | 7 } text [ timeout seconds ]}
DETAILED STEPS
|  |  |  | 
|---|---|---|
| Step 1 | configure terminal  Example: switch# configure terminal switch(config)# | Enters global configuration mode. | 
| Step 2 | interface interface-type slot/port  Example: switch(config)# interface ethernet 1/2 switch(config-if)# | Enters interface configuration mode. | 
| Step 3 | hsrp group-number [ ipv4 \| ipv6 ]  Example : switch(config-if)# hsrp 2 switch(config-if-hsrp)# | Creates an HSRP group and enters HSRP configuration mode. | 
| Step 4 | authentication text string  Example : switch(config-if-hsrp)# authentication text mypassword | Configures cleartext authentication for HSRP on this interface. | 
|  | authentication md5 { key-chain key-chain \| key-string { 0 \| 7 } text [ timeout seconds ]}  Example : switch(config-if-hsrp)# authentication md5 key-chain hsrp-keys | Configures MD5 authentication for HSRP on this interface. You can use a key chain or key string. If you use a key string, you can optionally set the timeout for when HSRP only accepts a new key. The range is from 0 to 32767 seconds. | 
| Step 5 | show hsrp [ group group-number ]  Example : switch(config-if-hsrp)# show hsrp group 2 | (Optional) Displays HSRP information. | 
| Step 6 | copy running-config startup-config  Example: switch(config-if-hsrp)# copy running-config startup-config | (Optional) Copies the running configuration to the startup configuration. | 
This example shows how to configure MD5 authentication for HSRP on Ethernet 1/2 after creating the key chain:
switch(config-keychain-key)# interface ethernet 1/2
switch(config-if-hsrp)# authentication md5 key-chain hsrp-keys
Authenticating HSRP
You can configure HSRP to authenticate the protocol using cleartext or MD5 digest authentication. MD5 authentication uses a key chain (see the Cisco Nexus 9000 Series NX-OS Security Configuration Guide).
BEFORE YOU BEGIN
You must enable HSRP (see the “Enabling HSRP” section).
You must configure the same authentication and keys on all members of the HSRP group.
Ensure that you have created the key chain if you are using MD5 authentication.
SUMMARY STEPS
2. interface interface- type slot/port
3. hsrp group- number [ ipv4 | ipv6 ]
authentication md5 { key-chain key-chain | key-string { 0 | 7 } text [ timeout seconds ]}
DETAILED STEPS
|  |  |  | 
|---|---|---|
| Step 1 | configure terminal  Example: switch# configure terminal switch(config)# | Enters global configuration mode. | 
| Step 2 | interface interface-type slot/port  Example: switch(config)# interface ethernet 1/2 switch(config-if)# | Enters interface configuration mode. | 
| Step 3 | hsrp group-number [ ipv4 \| ipv6 ]  Example : switch(config-if)# hsrp 2 switch(config-if-hsrp)# | Creates an HSRP group and enters HSRP configuration mode. | 
| Step 4 | authentication text string  Example : switch(config-if-hsrp)# authentication text mypassword | Configures cleartext authentication for HSRP on this interface. | 
|  | authentication md5 { key-chain key-chain \| key-string { 0 \| 7 } text [ timeout seconds ]}  Example : switch(config-if-hsrp)# authentication md5 key-chain hsrp-keys | Configures MD5 authentication for HSRP on this interface. You can use a key chain or key string. If you use a key string, you can optionally set the timeout for when HSRP only accepts a new key. The range is from 0 to 32767 seconds. | 
| Step 5 | show hsrp [ group group-number ]  Example : switch(config-if-hsrp)# show hsrp group 2 | (Optional) Displays HSRP information. | 
| Step 6 | copy running-config startup-config  Example: switch(config-if-hsrp)# copy running-config startup-config | (Optional) Copies the running configuration to the startup configuration. | 
This example shows how to configure MD5 authentication for HSRP on Ethernet 1/2 after creating the key chain:
Configuring HSRP Object Tracking
You can configure an HSRP group to adjust its priority based on the availability of other interfaces or routes. The priority of an HSRP group can change dynamically if it has been configured for object tracking and the object that is being tracked goes down.
The tracking process periodically polls the tracked objects and notes any value change. The value change triggers HSRP to recalculate the priority. The HSRP interface with the higher priority becomes the active router if you configure the HSRP interface for preemption.
SUMMARY STEPS
2. track object-id interface interface-type slot/port { line-protocol | ip routing | ipv6 routing }
3. track object-id { ip | ipv6 } route ip-prefix/length reachability
5. interface interface- type slot/port
6. hsrp group- number [ ipv4 | ipv6 ]
8. track object-id [ decrement value ]
9. preempt [ delay [minimum seconds ] [reload seconds ] [sync seconds ]]
DETAILED STEPS
|  |  |  | 
|---|---|---|
| Step 1 | configure terminal  Example: switch# configure terminal switch(config)# | Enters global configuration mode. | 
| Step 2 | track object-id interface interface-type slot/port {line-protocol \| ip routing \| ipv6 routing}  Example: switch(config)# track 1 interface ethernet 2/2 line-protocol switch(config-track)# | Configures the interface that the track object tracks. Changes in the state of the interface affect the track object status as follows:  | 
| Step 3 | track o bject-id {ip \| ipv6} route ip-prefix / length reachability  Example: switch(config-track)# track 2 ip route 192.0.2.0/8 reachability | Creates a tracked object for a route and enters tracking configuration mode. The object-id range is from 1 to 500. | 
| Step 4 | exit  Example: switch(config-track)# exit switch(config)# | Exits track configuration mode. | 
| Step 5 | interface interface-type slot/port  Example: switch(config)# interface ethernet 1/2 switch(config-if)# | Enters interface configuration mode. | 
| Step 6 | hsrp group-number [ ipv4 \| ipv6 ]  Example : switch(config-if)# hsrp 2 switch(config-if-hsrp)# | Creates an HSRP group and enters HSRP configuration mode. | 
| Step 7 | priority [ value ]  Example : switch(config-if-hsrp)# priority 254 | Sets the priority level used to select the active router in an HSRP group. The range is from 0 to 255. The default is 100. | 
| Step 8 | track object-id [decrement value ]  Example: switch(config-if-hsrp)# track 1 decrement 20 | Specifies an object to be tracked that affects the weighting of an HSRP interface. The value argument specifies a reduction in the priority of an HSRP interface when a tracked object fails. The range is from 1 to 255. The default is 10. | 
| Step 9 | preempt [ delay [ minimum seconds ] [ reload seconds ] [ sync seconds ]]  Example : switch(config-if-hsrp)# preempt delay minimum 60 | Configures the router to take over as the active router for an HSRP group if it has a higher priority than the current active router. This command is disabled by default. Optionally, a delay can be configured that delays the HSRP group preemption by the configured time. The range is from 0 to 3600 seconds. | 
| Step 10 | show hsrp interface interface-type slot/port  Example : switch(config-if-hsrp)# show hsrp interface ethernet 1/2 | (Optional) Displays HSRP information for an interface. | 
| Step 11 | copy running-config startup-config  Example: switch(config-if-hsrp)# copy running-config startup-config | (Optional) Copies the running configuration to the startup configuration. | 
This example shows how to configure HSRP object tracking on Ethernet interface 1/2:
switch(config-track)# track 2 ip route 192.0.2.0/8 reachability
Configuring the HSRP Priority
You can configure the priority of an HSRP group. HSRP uses the priority to determine which HSRP group member acts as the active router. If you configure HSRP on a vPC-enabled interface, you can optionally configure the upper and lower threshold values to control when to fail over to the vPC trunk. If the standby router priority falls below the lower threshold, HSRP sends all standby router traffic across the vPC trunk to forward through the active HSRP router. HSRP maintains this scenario until the standby HSRP router priority increases above the upper threshold.
For IPv6 HSRP groups, if all group members have the same priority, HSRP selects the active router based on the IPv6 link-local address.
To configure the HSRP priority, use the following command in the HSRP group configuration mode:
|  |  | 
|---|---|
| priority level [ forwarding-threshold lower lower-value upper upper-value]  Example: switch(config-if-hsrp)# priority 60 forwarding-threshold lower 40 upper 50 | Sets the priority level used to select the active router in an HSRP group. The level range is from 0 to 255. The default is 100. Optionally, this command sets the upper and lower threshold values used by vPC to determine when to fail over to the vPC trunk. The lower-value range is from 1 to 255. The default is 1. The upper-value range is from 1 to 255. The default is 255. | 
Customizing HSRP
You can optionally customize the behavior of HSRP. Be aware that as soon as you enable an HSRP group by configuring a virtual IP address, that group is now operational. If you first enable an HSRP group before customizing HSRP, the router could take control over the group and become the active router before you finish customizing the feature. If you plan to customize HSRP, you should do so before you enable the HSRP group. To customize HSRP, use the following commands in HSRP configuration mode:
|  |  | 
|---|---|
| name string  Example: switch(config-if-hsrp)# name HSRP-1 | Specifies the IP redundancy name for an HSRP group. The string is from 1 to 255 characters. The default string has the following format: hsrp-interface short-name group-id. For example, hsrp-Eth2/1-1. | 
| preempt [ delay [ minimum seconds ] [ reload seconds ] [ sync seconds ]]  Example: switch(config-if-hsrp)# preempt delay minimum 60 | Configures the router to take over as an active router for an HSRP group if it has a higher priority than the current active router. This command is disabled by default. Optionally, a delay can be configured that delays the HSRP group preemption by the configured time. The range is from 0 to 3600 seconds. | 
| timers [ msec ] hellotime [ msec ] holdtime  Example: switch(config-if-hsrp)# timers 5 18 | Configures the hello and hold time for this HSRP member as follows:  The optional msec keyword specifies that the argument is expressed in milliseconds instead of the default seconds. The timer ranges for milliseconds are as follows: | 
To customize HSRP, use the following commands in interface configuration mode:
|  |  | 
|---|---|
| hsrp delay minimum seconds  Example: switch(config-if)# hsrp delay minimum 30 | Specifies the minimum amount of time that HSRP waits after a group is enabled before participating in the group. The range is from 0 to 10000 seconds. The default is 0. | 
| hsrp delay reload seconds  Example: switch(config-if)# hsrp delay reload 30 | Specifies the minimum amount of time that HSRP waits after a reload and before participating in the group. The range is from 0 to 10000 seconds. The default is 0. | 
Configuring Extended Hold Timers for HSRP
You can configure HSRP to use extended hold timers to support extended NSF during a controlled (graceful) switchover. You should configure extended hold timers on all HSRP routers (see the “High Availability and Extended Nonstop Forwarding” section).
Note You must configure extended hold timers on all HSRP routers if you configure extended hold timers. If you configure a nondefault hold timer, you should configure the same value on all HSRP routers when you configure HSRP extended hold timers.
Note HSRP extended hold timers are not applied if you configure millisecond hello and hold timers for HSRPv1. This statement does not apply to HSRPv2.
To configure HSRP extended hold timers, use the following command in global configuration mode:
Use the show hsrp command or the show running-config hsrp command to display the extended hold time.
Verifying the HSRP Configuration
To display HSRP configuration information, perform one of the following tasks:
|  |  | 
|---|---|
| show hsrp [group group-number] | Displays the HSRP status for all groups or one group. | 
| show hsrp delay [interface interface-type slot/port] | Displays the HSRP delay value for all interfaces or one interface. | 
| show hsrp [interface interface-type slot/port] | Displays the HSRP status for an interface. | 
| show hsrp [group group-number] [interface interface-type slot/port] [active] [all] [init] [learn] [listen] [speak] [standby] | Displays the HSRP status for a group or interface for virtual forwarders in the active, init, learn, listen, or standby state. Use the all keyword to see all states, including disabled. | 
| show hsrp [group group-number] [interface interface-type slot/port] active] [all] [init] [learn] [listen] [speak] [standby] brief | Displays a brief summary of the HSRP status for a group or interface for virtual forwarders in the active, init, learn, listen, or standby state. Use the all keyword to see all states, including disabled. | 
Configuration Examples for HSRP
This example shows how to enable HSRP on an interface with MD5 authentication and interface tracking:
track 2 interface ethernet 2/2 ip
authenticate md5 key-chain hsrp-keys
This example shows how to configure the HSRP priority on an interface:
Additional References
For additional information related to implementing HSRP, see the following sections:
Related Documents
|  |  | 
|---|---|
| Configuring the Virtual Router Redundancy Protocol | Chapter 18, “Configuring VRRP” | 
| Configuring high availability | Cisco Nexus 9000 Series NX-OS High Availability and Redundancy Guide | 
MIBs
|  |  | 
|---|---|
| MIBs related to HSRP | To locate and download supported MIBs, go to the following URL: ftp://ftp.cisco.com/pub/mibs/supportlists/nexus9000/Nexus9000MIBSupportList.html |