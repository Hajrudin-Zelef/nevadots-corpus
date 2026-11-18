The documentation set for this product strives to use bias-free language. For the purposes of this documentation set, bias-free is defined as language that does not imply discrimination based on age, disability, gender, racial identity, ethnic identity, sexual orientation, socioeconomic status, and intersectionality. Exceptions may be present in the documentation due to language that is hardcoded in the user interfaces of the product software, language used based on RFP documentation, or language that is used by a referenced third-party product. Learn more about how Cisco is using Inclusive Language.
The Virtual Router Redundancy Protocol (VRRP) is an election protocol that dynamically assigns responsibility for one or
more virtual routers to the VRRP routers on a LAN, allowing several routers on a multiaccess link to utilize the same virtual
IP address. A VRRP router is configured to run the VRRP protocol in conjunction with one or more other routers attached to
a LAN. In a VRRP configuration, one router is elected as the virtual primary router, with the other routers acting as backups
in case the virtual primary router fails.
This module explains the concepts related to VRRP and describes how to configure VRRP in a network.
Restrictions for VRRP
VRRP is designed for use over multiaccess, multicast, or broadcast capable Ethernet LANs. VRRP is not intended as a replacement
for existing dynamic protocols.
VRRP is supported on Ethernet, Fast Ethernet, Bridge Group Virtual Interface (BVI), and Gigabit Ethernet interfaces, and
on Multiprotocol Label Switching (MPLS) Virtual Private Networks (VPNs), VRF-aware MPLS VPNs, and VLANs.
Because of the forwarding delay that is associated with the initialization of a BVI interface, you must configure the VRRP
advertise timer to a value equal to or greater than the forwarding delay on the BVI interface. This setting prevents a VRRP
router on a recently initialized BVI interface from unconditionally taking over the primary role. Use the bridgeforward-time command to set the forwarding delay on the BVI interface. Use the vrrptimersadvertise command to set the VRRP advertisement timer.
Information About VRRP
VRRP Operation
There are several ways a LAN client can determine which router should be the first hop to a particular remote destination.
The client can use a dynamic process or static configuration. Examples of dynamic router discovery are as follows:
Proxy ARP—The client uses Address Resolution Protocol (ARP) to get the destination it wants to reach, and a router will respond
to the ARP request with its own MAC address.
Routing protocol—The client listens to dynamic routing protocol updates (for example, from Routing Information Protocol [RIP])
and forms its own routing table.
ICMP Router Discovery Protocol (IRDP) client—The client runs an Internet Control Message Protocol (ICMP) router discovery
client.
The drawback to dynamic discovery protocols is that they incur some configuration and processing overhead on the LAN client.
Also, in the event of a router failure, the process of switching to another router can be slow.
An alternative to dynamic discovery protocols is to statically configure a default router on the client. This approach simplifies
client configuration and processing, but creates a single point of failure. If the default gateway fails, the LAN client is
limited to communicating only on the local IP network segment and is cut off from the rest of the network.
VRRP can solve the static configuration problem. VRRP enables a group of routers to form a single
virtualrouter. The LAN clients can then be configured with the virtual router as their default gateway. The virtual router, representing
a group of routers, is also known as a VRRP group.
VRRP is supported on Ethernet, Fast Ethernet, BVI, and Gigabit Ethernet interfaces, and on MPLS VPNs, VRF-aware MPLS VPNs,
and VLANs.
The figure below shows a LAN topology in which VRRP is configured. In this example, Routers A, B, and C are VRRP routers
(routers running VRRP) that comprise a virtual router. The IP address of the virtual router is the same as that configured
for the Ethernet interface of Router A (10.0.0.1).
Because the virtual router uses the IP address of the physical Ethernet interface of Router A, Router A assumes the role
of the virtual primary router and is also known as the IP address owner. As the virtual primary router, Router A controls
the IP address of the virtual router and is responsible for forwarding packets sent to this IP address. Clients 1 through
3 are configured with the default gateway IP address of 10.0.0.1.
Routers B and C function as virtual router backups. If the virtual primary router fails, the router configured with the higher
priority will become the virtual primary router and provide uninterrupted service for the LAN hosts. When Router A recovers,
it becomes the virtual primary router again. For more detail on the roles that VRRP routers play and what happens if the virtual
primary router fails, see the VRRP Router Priority and Preemption section.
The figure below shows a LAN topology in which VRRP is configured so that Routers A and B share the traffic to and from clients
1 through 4 and that Routers A and B act as virtual router backups to each other if either router fails.
In this topology, two virtual routers are configured. (For more information, see the Multiple Virtual Router Support section.) For virtual router 1, Router A is the owner of IP address 10.0.0.1 and virtual primary router, and Router B is
the virtual router backup to Router A. Clients 1 and 2 are configured with the default gateway IP address of 10.0.0.1.
For virtual router 2, Router B is the owner of IP address 10.0.0.2 and virtual primary router, and Router A is the virtual
router backup to Router B. Clients 3 and 4 are configured with the default gateway IP address of 10.0.0.2.
VRRP Benefits
Redundancy
VRRP enables you to
configure multiple routers as the default gateway router, which reduces the
possibility of a single point of failure in a network.
Load Sharing
You can configure
VRRP in such a way that traffic to and from LAN clients can be shared by
multiple routers, thereby sharing the traffic load more equitably among
available routers.
Multiple Virtual
Routers
Multiple IP Addresses
The virtual router
can manage multiple IP addresses, including secondary IP addresses. Therefore,
if you have multiple subnets configured on an Ethernet interface, you can
configure VRRP on each subnet.
Preemption
The redundancy scheme of VRRP enables you to preempt a virtual router backup that has taken over for a failing virtual primary
router with a higher priority virtual router backup that has become available.
Authentication
VRRP message digest
5 (MD5) algorithm authentication protects against VRRP-spoofing software and
uses the industry-standard MD5 algorithm for improved reliability and security.
Advertisement
Protocol
VRRP uses a
dedicated Internet Assigned Numbers Authority (IANA) standard multicast address
(224.0.0.18) for VRRP advertisements. This addressing scheme minimizes the
number of routers that must service the multicasts and allows test equipment to
accurately identify VRRP packets on a segment. The IANA assigned VRRP the IP
protocol number 112.
VRRP Object Tracking
VRRP object tracking provides a way to ensure the best VRRP router is the virtual primary router for the group by altering
VRRP priorities to the status of tracked objects such as the interface or IP route states.
Multiple Virtual Router
Support
Router processing
capability
Router memory
capability
Router interface
support of multiple MAC addresses
In a topology where multiple virtual routers are configured on a router interface, the interface can act as primary for one
virtual router and as a backup for one or more virtual routers.
VRRP Router Priority and Preemption
An important aspect of the VRRP redundancy scheme is VRRP router priority. Priority determines the role that each VRRP router
plays and what happens if the virtual primary router fails.
If a VRRP router owns the IP address of the virtual router and the IP address of the physical interface, this router will
function as a virtual primary router.
Priority also determines if a VRRP router functions as a virtual router backup and the order of ascendancy to becoming virtual
primary router if the virtual primary router fails. You can configure the priority of each virtual router backup with a value
of 1 through 254 using the vrrppriority command.
For example, if Router A, the virtual primary router in a LAN topology, fails, an election process takes place to determine
if virtual router backups B or C should take over. If Routers B and C are configured with the priorities of 101 and 100, respectively,
Router B is elected to become virtual primary router because it has the higher priority. If Routers B and C are both configured
with the priority of 100, the virtual router backup with the higher IP address is elected to become the virtual primary router.
By default, a preemptive scheme is enabled whereby a higher priority virtual router backup that becomes available takes over
for the virtual router backup that was elected to become virtual primary router. You can disable this preemptive scheme using
the novrrppreempt command. If preemption is disabled, the virtual router backup that is elected to become virtual primary router remains as
the primary until the original virtual primary router recovers and becomes the primary again.
VRRP Advertisements
The virtual primary router sends VRRP advertisements to other VRRP routers in the same group. The advertisements communicate
the priority and state of the virtual primary router. The VRRP advertisements are encapsulated in IP packets and sent to the
IP Version 4 multicast address assigned to the VRRP group. The advertisements are sent every second by default; the interval
is configurable.
Although the VRRP protocol as per RFC 3768 does not support millisecond timers, Cisco routers allow you to configure millisecond
timers. You need to manually configure the millisecond timer values on both the primary and the backup routers. The primary
advertisement value displayed in the showvrrp command output on the backup routers is always 1 second because the packets on the backup routers do not accept millisecond
values.
Note
Use millisecond timers where absolutely necessary, with careful consideration and testing.
Millisecond values work only under favorable circumstances, and you must be aware that the use of the millisecond timer values
restricts VRRP operation to Cisco devices only.
Be cautious while using millisecond timers when SNMP traps are enabled for VRRP.
VRRP Object Tracking
Object tracking is an
independent process that manages creating, monitoring, and removing tracked
objects such as the state of the line protocol of an interface. Clients such as
the Hot Standby Router Protocol (HSRP), Gateway Load Balancing Protocol (GLBP),
and VRRP register their interest with specific tracked objects and act when the
state of an object changes.
Each tracked object
is identified by a unique number that is specified on the tracking CLI. Client
processes such as VRRP use this number to track a specific object.
The tracking process
periodically polls the tracked objects and notes any change of value. The
changes in the tracked object are communicated to interested client processes,
either immediately or after a specified delay. The object values are reported
as either up or down.
VRRP object tracking
gives VRRP access to all the objects available through the tracking process.
The tracking process allows you to track individual objects such as a the state
of an interface line protocol, state of an IP route, or the reachability of a
route.
VRRP provides an
interface to the tracking process. Each VRRP group can track multiple objects
that may affect the priority of the VRRP device. You specify the object number
to be tracked and VRRP is notified of any change to the object. VRRP increments
(or decrements) the priority of the virtual device based on the state of the
object being tracked.
How VRRP Object Tracking
Affects the Priority of a Device
The priority of a device can change dynamically if it has been configured for object tracking and the object that is being
tracked goes down. The tracking process periodically polls the tracked objects and notes any change of value. The changes
in the tracked object are communicated to VRRP, either immediately or after a specified delay. The object values are reported
as either up or down. Examples of objects that can be tracked are the line protocol state of an interface or the reachability
of an IP route. If the specified object goes down, the VRRP priority is reduced. The VRRP device with the higher priority
can now become the virtual primary device if it has the vrrppreempt command configured. See the “VRRP Object Tracking” section for more information on object tracking.
In Service Software Upgrade--VRRP
VRRP supports In Service Software Upgrade (ISSU). In Service Software Upgrade (ISSU) allows a high-availability (HA) system
to run in stateful switchover (SSO) mode even when different versions of Cisco IOS XE software are running on the active and
standby Route Processors (RPs) or line cards.
ISSU provides the ability to upgrade or downgrade from one supported Cisco IOS XE release to another while continuing to
forward packets and maintain sessions, thereby reducing planned outage time. The ability to upgrade or downgrade is achieved
by running different software versions on the active RP and standby RP for a short period of time to maintain state information
between RPs. This feature allows the system to switch over to a secondary RP running upgraded (or downgraded) software and
continue forwarding packets without session loss and with minimal or no packet loss. This feature is enabled by default.
For detailed information about ISSU, see the Cisco IOS XE In Service Software Upgrade Process document in the Cisco IOS XE High Availability Configuration Guide.
VRRP Support for Stateful
Switchover
With the introduction
of the VRRP Support for Stateful Switchover feature, VRRP is SSO aware. VRRP
can detect when a router is failing over to the secondary RP and continue in
its current group state.
SSO functions in
networking devices (usually edge devices) that support dual Route Processors
(RPs). SSO provides RP redundancy by establishing one of the RPs as the active
processor and the other RP as the standby processor. SSO also synchronizes
critical state information between the RPs so that network state information is
dynamically maintained between RPs.
Prior to being SSO
aware, if VRRP was deployed on a router with redundant RPs, a switchover of
roles between the active RP and the standby RP would result in the router
relinquishing its activity as a VRRP group member and then rejoining the group
as if it had been reloaded. The SSO--VRRP feature enables VRRP to continue its
activities as a group member during a switchover. VRRP state information
between redundant RPs is maintained so that the standby RP can continue the
router’s activities within the VRRP during and after a switchover.
This feature is
enabled by default.
To disable this feature, use the
novrrpsso command in global configuration mode.
For more information, see the Stateful Switchover
document.
How to Configure VRRP
VRRP
Customizing the behavior of VRRP is optional. Be aware that as soon as you enable a VRRP group, that group is operating.
It is possible that if you first enable a VRRP group before customizing VRRP, the router could take over control of the group
and become the virtual primary router before you have finished customizing the feature. Therefore, if you plan to customize
VRRP, it is a good idea to do so before enabling VRRP.
Procedure
Step 1
enable
Example:
Router> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Router# configure terminal
Enters global configuration mode.
Step 3
interfacetypenumber
Example:
Router(config)#GigabitEthernet 0/0/0
Enters interface configuration mode.
Step 4
ipaddressip-addressmask
Example:
Router(config-if)# ip address 172.16.6.5 255.255.255.0
Configures the router to take over as virtual primary router for a VRRP group if it has a higher priority than the current
virtual primary router.
The default delay period is 0 seconds.
The router that is IP address owner will preempt, regardless of the setting of this command.
Step 8
vrrpgrouptimerslearn
Example:
Router(config-if)# vrrp 10 timers learn
Configures the router, when it is acting as virtual router backup for a VRRP group, to learn the advertisement interval used
by the virtual primary router.
Step 9
exit
Example:
Router(config-if)# exit
Exits interface configuration mode.
Step 10
novrrpsso
Example:
Router(config)# no vrrp sso
(Optional) Disables VRRP support of SSO.
VRRP support of SSO is enabled by default.
EnablingVerifying VRRP
Procedure
Step 1
enable
Example:
Router> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Router# configure terminal
Enters global configuration mode.
Step 3
interfacetypenumber
Example:
Router(config)# interfaceGigabitEthernet 0/0/0
Enters interface configuration mode.
Step 4
ipaddressip-addressmask
Example:
Router(config-if)# ip address 172.16.6.5 255.255.255.0
Configures an IP address for an interface.
Step 5
vrrpgroupipip-address [secondary]
Example:
Router(config-if)# vrrp 10 ip 172.16.6.1
Enables VRRP on an interface.
After you identify a primary IP address, you can use the vrrpip command again with the secondary keyword to indicate additional IP addresses supported by this group.
Note
All routers in the VRRP group must be configured with the same primary address and a matching list of secondary addresses
for the virtual router. If different primary or secondary addresses are configured, the routers in the VRRP group will not
communicate with each other and any misconfigured router will change its state to primary.
Step 6
showvrrp [briefall] | interface]
Example:
Router(config-if)#show vrrp brief
Interface Grp Pri Time Own Pre State Master addr Group addr
BD10 1 100 9609 Y Backup 10.1.0.2 10.1.0.10
BD10 5 200 90218 Y Master 10.1.0.1 10.1.0.50
BD10 100 100 3609 Backup 10.1.0.2 10.1.0.100
(Optional) Displays a brief or detailed status of one or all VRRP groups on the router.
Step 7
showvrrpinterfacetypenumber [brief]
Example:
Router(config)# interfaceGigabitEthernet 0/0/0
Router)config-if)#show vrrp interface bdi10
BDI10 - Group 10
G1
State is Master
Virtual IP address is 10.0.0.5
Virtual MAC address is 0000.5e00.010a
Advertisement interval is 10.000 sec
Preemption enabled, delay min 380 secs
Priority is 110
Master Router is 10.0.0.2 (local), priority is 110
Master Advertisement interval is 10.000 sec
Master Down interval is 30.570 sec
FLAGS: 1/1
(Optional) Displays the VRRP groups and their status on a specified interface.
Step 8
end
Example:
Router(config-if)# end
Returns to privileged EXEC mode.
Configuring VRRP Object Tracking
Note
If a VRRP group is the IP address owner, its priority is fixed at 255 and cannot be reduced through object tracking.
Router(config)# track 2 interface serial 6 line-protocol
Configures an interface to be tracked where changes in the state of the interface affect the priority of a VRRP group.
This command configures the interface and corresponding object number to be used with the
vrrptrack command.
The
line-protocol keyword tracks whether the interface is up. The
iprouting keyword also checks that IP routing is enabled and active on the interface.
You can also use the
trackiproute command to track the reachability of an IP route or a metric type object.
Step 4
interfacetypenumber
Example:
Router(config)# interface Ethernet 2
Enters interface configuration mode.
Step 5
vrrpgroupipip-address
Example:
Router(config-if)# vrrp 1 ip 10.0.1.20
Enables VRRP on an interface and identifies the IP address of the virtual router.
Step 6
vrrpgroupprioritylevel
Example:
Router(config-if)# vrrp 1 priority 120
Sets the priority level of the router within a VRRP group.
Step 7
vrrpgrouptrackobject-number [decrementpriority]
Example:
Router(config-if)# vrrp 1 track 2 decrement 15
Configures VRRP to track an object.
Step 8
end
Example:
Router(config-if)# end
Returns to privileged EXEC mode.
Step 9
showtrack [object-number]
Example:
Router# show track 1
Displays tracking information.
Configuring VRRP Text Authentication
Before you begin
Interoperability with vendors that may have implemented the RFC 2338 method is not enabled.
Text authentication cannot be combined with MD5 authentication for a VRRP group at any one time. When MD5 authentication
is configured, the text authentication field in VRRP hello messages is set to all zeros on transmit and ignored on receipt,
provided the receiving router also has MD5 authentication enabled.
Configures an interface type and enters interface configuration mode.
Step 4
ipaddressip-addressmask [secondary]
Example:
Router(config-if)# ip address 10.0.0.1 255.255.255.0
Specifies a primary or secondary IP address for an interface.
Step 5
vrrpgroupauthenticationtexttext-string
Example:
Router(config-if)# vrrp 1 authentication text textstring1
Authenticates VRRP packets received from other routers in the group.
If you configure authentication, all routers within the VRRP group must use the same authentication string.
The default string is cisco.
Note
All routers within the VRRP group must be configured with the same authentication string. If the same authentication string
is not configured, the routers in the VRRP group will not communicate with each other and any misconfigured router will change
its state to primary.
Step 6
vrrpgroupipip-address
Example:
Router(config-if)# vrrp 1 ip 10.0.1.20
Enables VRRP on an interface and identifies the IP address of the virtual router.
Step 7
Repeat Steps 1 through 6 on each router that will communicate.
—
Step 8
end
Example:
Router(config-if)# end
Returns to privileged EXEC mode.
Configuration Examples for VRRPv2
Example: Configuring VRRP
In the following example, Router A and Router B each belong to three VRRP groups.
In the configuration, each group has the following properties:
Group 1:
Virtual IP address is 10.1.0.10.
Router A will become the primary for this group with priority 120.
Advertising interval is 3 seconds.
Preemption is enabled.
Group 5:
Router B will become the primary for this group with priority 200.
Advertising interval is 30 seconds.
Preemption is enabled.
Group 100:
Router A will become the primary for this group first because it has a higher IP address (10.1.0.2).
In the following example, the tracking process is configured to track the state of the line protocol on serial interface
0/1. VRRP on Ethernet interface 1/0 then registers with the tracking process to be informed of any changes to the line protocol
state of serial interface 0/1. If the line protocol state on serial interface 0/1 goes down, then the priority of the VRRP
group is reduced by 15.
Router# show vrrp
Ethernet1/0 - Group 1
State is Master
Virtual IP address is 10.0.0.3
Virtual MAC address is 0000.5e00.0101
Advertisement interval is 1.000 sec
Preemption is enabled
min delay is 0.000 sec
Priority is 105
Track object 1 state Down decrement 15
Master Router is 10.0.0.2 (local), priority is 105
Master Advertisement interval is 1.000 sec
Master Down interval is 3.531 sec
Router# show track
Track 1
Interface Serial0/1 line-protocol
Line protocol is Down (hw down)
1 change, last change 00:06:53
Tracked by:
VRRP Ethernet1/0 1
Example: VRRP Text Authentication
The following example shows how to configure VRRP text authentication using a text string:
Router(config)#GigabitEthernet 0/0/0interface GigabitEthernet 0/0/0
Router(config)# ip address 10.21.8.32 255.255.255.0
Router(config-if)# vrrp 10 authentication text stringxyz
Router(config-if)# vrrp 10 ip 10.21.8.10
The
Cisco Support and Documentation website provides online resources to download
documentation, software, and tools. Use these resources to install and
configure the software and to troubleshoot and resolve technical issues with
Cisco products and technologies. Access to most tools on the Cisco Support and
Documentation website requires a Cisco.com user ID and password.
The following table provides release information about the feature or features described in this module. This table lists
only the software release that introduced support for a given feature in a given software release train. Unless noted otherwise,
subsequent releases of that software release train also support that feature.
Use Cisco Feature Navigator to find information about platform support and Cisco software image support. To access Cisco
Feature Navigator, go to www.cisco.com/go/cfn. An account on Cisco.com is not required.
Glossary
virtualIPaddressowner—The VRRP router that owns the IP address of the virtual router. The owner is the router that has the virtual router address
as its physical interface address.
virtualrouter—One or more VRRP routers that form a group. The virtual router acts as the default gateway router for LAN clients. Also known
as a VRRP group.
virtualrouterbackup—One or more VRRP routers that are available to assume the role of forwarding packets if the virtual primary router fails.
virtualprimaryrouter—The VRRP router that is currently responsible for forwarding packets sent to the IP addresses of the virtual router. Usually
the virtual primary router also functions as the IP address owner.