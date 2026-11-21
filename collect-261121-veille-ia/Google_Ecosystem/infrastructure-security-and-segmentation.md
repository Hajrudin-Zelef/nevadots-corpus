---
id: collect-261121-veille-ia/google-ecosystem/infrastructure-security-and-segmentation
title: "Infrastructure Security and Segmentation"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["security", "table", "data", "plane", "switch", "address", "source", "frame"]
source: https://www.ciscopress.com/articles/article.asp?p=2928193&seqNum=4
source_anchor: ""
source_lines: [1, 621]
section: "Google_Ecosystem"
---
## Securing the Data Plane

The purpose of a network is to move data from its source to its destination. Network devices are geared toward that one function alone, and the data plane is where it is executed. Hence, it should not be surprising that most attacks are targeted toward this plane, and most security is also implemented here.

The difference between the data plane of a Layer 2 device and that of a Layer 3 device is far more distinct than in any other plane. The security objectives, challenges, and solutions are very different between those two layers.

Layer 2 security is primarily focused on unauthorized or spoofed access, segmentation, and resource protection. On the other hand, Layer 3 security is primarily focused on the data itself and ensuring that the right kind from the right source is allowed. Even with the differences, some general security considerations apply to the data plane of both layers:

- **Zero trust and minimum required access:** This principle applies to the data plane also. No traffic or its source should be trusted, and each device should be given the minimum required access. Access control and segmentation of traffic play a crucial role in this.
- **Protocol validation:** Most of the common protocols used in the data plane have absolutely no security built in. They are usually easy to spoof and exploit. In fact, some of the most effective attacks are executed by exploiting transport protocols. Measures should be taken to prevent attacks using these protocols.
- **Audit trail collection:** Cisco devices provide various methods to generate and export audit trails in the form of logs. Common examples of these are access lists with logging and NetFlow. These should be enabled, exported to central collectors, and analyzed for incidence response and policy correction.

### Security at the Layer 2 Data Plane

The data plane on a switch is focused on switching frames in the local network segment. To accomplish this, it needs to know where a given MAC address is located and, sometimes, what IP address it is mapped to. To keep track of these two important details, a switch uses some protocols and tables, such as the CAM and ARP tables. The next few sections discuss the inherent vulnerabilities in the switching mechanism and the security features used to protect them.

#### The CAM Table and Port Security

The content addressable memory (CAM) table is a list of all MAC addresses known to the switch, along with their physical location and VLAN ID. When an endpoint sends a frame, its MAC address, source interface, and VLAN ID are recorded in the CAM table. The switch searches this table to find the location of a MAC address when it needs to deliver a frame. If the destination MAC address is not found in the CAM table, the frame is flooded out all interfaces except the one where the frame was received.

The process of learning a MAC address and its location has no security built into it. Anyone can send a frame with a spoofed source MAC address to cause the switch to update its CAM table. This can be used for a MITM attack such that frames are switched to the spoofed location instead of the intended destination. The traffic continues to flow to the attacking host until the original endpoint sends a frame, causing the switch to update the CAM table again. This is called a *MAC spoofing attack*.

Another problem with the CAM table is its limited size. An attacker can send hundreds of thousands of frames with difference source MAC addresses within a short period of time. This causes the switch to add these spoofed MAC addresses in the CAM table. Once the CAM table has reached its limits, it stops learning new MAC addresses and starts flooding frames with unknown MAC addresses out all its interfaces. This can cause severe degradation of the network to the point of a DoS situation. This attack is called *CAM flooding*. Each switch has a different CAM table size limit. The size of the CAM table on a Cisco IOS switch can be found by using the **show mac address-table count** command, as shown in Example 2-38.

#### **Example 2-38** *MAC Address Table Limit*

`SW1#**sh mac address-table count**
Mac Entries for Vlan 1:
---------------------------
Dynamic Address Count  : 17
Static  Address Count  : 37
Total Mac Addresses    : 54
Total Mac Address Space Available: 16303`

CAM table attacks can be prevented with the port security feature in Cisco switches. With port security, an interface can be configured to learn only a limited number of MAC addresses. The addresses an interface can learn can be dynamic, static, or a combination of these. Port security can be enabled on an interface with the **switchport port-security** command.

The following MAC address learning restrictions can be configured with port security:

- **Static Secure MAC Addresses:** With this option, an interface is configured to learn only the manually configured MAC addresses. This option limits the devices that can connect to a given interface and is best used for static devices such as printers, cameras, servers, and other devices that do not change locations. This also prevents MAC spoofing by preventing the configured MAC address from connecting to another switch port. Static secure MAC addresses can be configured on an interface with the**switchport port-security mac-address***mac-address* command.
- **Dynamic Secure Addresses:** This option configures the maximum addresses that an interface will learn. The addresses are learned dynamically as devices connect to the interface. This option can be used to prevent CAM flooding attacks and the addition of unauthorized switches or hubs. The number of maximum allowed addresses can be configured on an interface with the**switchport port-security maximum***number* command.
- **Sticky Secure MAC Addresses:** Configuring each interface to allow a static MAC address is administratively prohibitive and restrictive to users. The Sticky Secure MAC Addresses for port security solves this by dynamically learning the MAC addresses that connect to it and storing them in the configuration as static. Sticky secure learning can be enabled on an interface with the**switchport port-security mac-address sticky** command.

The three options can be combined to meet different requirements. For example, an interface can be configured to learn a maximum of two addresses, while one is statically defined, as shown in Example 2-39. Such a configuration allows the interface to learn one dynamic address along with the statically configured one.

#### **Example 2-39** *Port Security with Static and Dynamic Address Learning*

`SW1(config)#**interface Gi0/5**
SW1(config-if)#**switchport port-security mac-address 1001.1001.1001**
SW1(config-if)#**switchport port-security maximum 2**`

Example 2-40 shows another combination in which an interface is configured for a maximum of three addresses with sticky learning enabled. This means the switch will learn and save the MAC addresses of the first three devices that connect to it. After that, only those three devices will be able to connect to that interface.

#### **Example 2-40** *Three Sticky Secure Addresses*

`SW1(config)#**interface Gi0/9**
SW1(config-if)#**switchport port-security maximum 3**
SW1(config-if)#**switchport port-security mac-address sticky**`

Port security defines three actions that can be taken when a violation occurs. An interface can be configured to take one of these actions:

- **Protect:** When the number of secure MAC addresses exceeds the maximum allowed limit on an interface, packets with unknown source addresses are dropped silently. The switch continues to drop frames until a sufficient number of addresses have been removed or the maximum number of allowed addresses has been increased in the configuration. The switch does not generate any alerts or logs in this mode. This mode can be enabled on an interface with the**switchport port-security violation protect** command.
- **Restrict:** Like the Protect mode, this mode also drops frames, but it generates alerts in the form of SNMP traps and syslog, and it also increases the violation counter for that interface. This mode can be enabled on an interface with the**switchport port-security violation restrict** command.
- **Shutdown:** An interface in this mode enters an error-disabled status and shuts down as soon as a port security violation occurs. An interface can be brought out of this state with the**errdisable recovery cause psecure-violation** command in the global configuration mode or with the**shutdown** command followed by the**no shutdown command** on the interface. While this mode is the default, it can be reenabled on an interface with the**switchport port-security violation shutdown** command.

With dynamic secure addressing, it is important to allow the learned addresses to age out of the CAM table. Without proper aging, new devices will not be able to connect on the interface. The aging timer can be configured with the **switchport port-security aging time** *minutes* command.

The aging timer can be configured to be absolute so that it starts as soon as the address is learned, or it can be configured to start when a period of inactivity begins. The type of timer can be configured with the **switchport port-security aging type** {**absolute**| **inactivity**} command.

Example 2-41 shows an interface configured to learn four addresses, out of which one is configured statically and three can be learned dynamically. The interface is also configured to remove the dynamically learned addresses after 10 minutes of inactivity.

#### **Example 2-41** *Port Security Aging*

`SW1(config)#**interface Gi0/9**
SW1(config-if)#**switchport port-security**
SW1(config-if)#**switchport port-security mac-address 1001.1001.1001**
SW1(config-if)#**switchport port-security aging time 10**
SW1(config-if)#**switchport port-security aging type inactivity**`

Port security configuration of an interface can be verified with the **show port-security interface** *interface* command, as shown in Example 2-42.

#### **Example 2-42** *Verifying Port Security Configuration*

`SW1#**show port-security int Gi0/9**
Port Security              : Enabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 10 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 1
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 1001.1001.1001:1
Security Violation Count   : 0`

#### DHCP Snooping

One of the key protocols in modern networks is Dynamic Host Configuration Protocol (DHCP). It provides IP addressing, default gateway, and other information to endpoints as they connect to the network and enables them to communicate. When an endpoint connects, it broadcasts a DHCP request, and any DHCP server in the network can respond to it. That is where the problem with DHCP lies. As you can imagine, the DHCP communication has no security built into it. Any host can claim to be a DHCP server and respond to requests, while any endpoint can forge DHCP requests to get an address assigned. This situation can be exploited to carry two attacks:

- **DHCP spoofing MITM:** An attacker can set up a rogue DHCP server to respond to DHCP requests. This DHCP server responds to requests with its own IP address as the default gateway. This causes the victims to send their traffic through the rogue gateway, where it can be read, stored, and modified before being sent to the real gateway, resulting in a MITM attack.
- **DHCP starvation DoS:** Because a DHCP server responds to any client request, it is trivial to forge thousands of DHCP requests with different MAC addresses and cause the server to exhaust its address pool. When the pool is exhausted, legitimate clients do not receive IP addresses, resulting in a DoS situation. This attack is often a precursor to a DHCP spoofing MITM attack, just described. If the legitimate DHCP pool is exhausted, a larger number of endpoints receive IP addresses from the rogue DHCP server.

While a DHCP starvation attack can be prevented with port security, as described earlier, mitigating DHCP spoofing attacks requires the use of DHCP snooping on Cisco switches.

*DHCP snooping* is a security feature that acts as a filtering mechanism between DHCP servers and clients. It works by defining trusted and untrusted interfaces. Trusted interfaces are those from which DHCP server messages can be expected, while all other interfaces are untrusted. This prevents rogue DHCP servers from responding to requests.

In addition to filtering rogue DHCP server packets, DHCP snooping also builds a database of clients connected to untrusted interfaces, along with the IP address, MAC address, and VLAN ID of each one. When a DHCP packet is received from an untrusted interface, its content is validated against the database. This prevents spoofed DHCP requests from being sent out.

Finally, DHCP snooping has a rate-limiting function that limits the number of DHCP packets allowed on an untrusted interface. This helps prevent starvation attacks against the DHCP server.

DHCP snooping configuration can be broken down into four steps:

- **Step 1.** Enable DHCP snooping on VLANs that require it with the**ip dhcp snooping vlan***vlan-id* command. Multiple VLANs can be specified, separated by commas, or a range of VLANs can be specified by using a dash.
- **Step 2.** Configure trusted interfaces from which DHCP server messages are expected with the**ip dhcp snooping trust** command. All interfaces are untrusted by default.
- **Step 3.** Configure DHCP rate limiting on untrusted interfaces with the**ip dhcp limit rate***limit* command.
- **Step 4.** Enable DHCP snooping globally with the**ip dhcp snooping** command.

Example 2-43 shows DHCP snooping configuration on a switch. In this example, interface GigabitEthernet0/10 is the trunk interface toward the DHCP server, and interfaces GigabitEthernet0/1 to 9 are access ports for endpoints. Interface Gi0/10 is configured as trusted, while others have rate limits applied to them.

#### **Example 2-43** *Configuring DHCP Snooping*

`SW1(config)#**ip dhcp snooping vlan 1,4**
SW1(config)#**interface Gi0/10**
SW1(config-if)#**ip dhcp snooping trust**
SW1(config-if)#**exit**
SW1(config)#**interface range Gi0/1-9**
SW1(config-if-range)#**ip dhcp snooping limit rate 5**
SW1(config-if)#**exit**
SW1(config)#**ip dhcp snooping**`

DHCP snooping can be verified with the **show ip dhcp snooping** command, as shown in Example 2-44.

#### **Example 2-44** *Verifying DHCP Snooping*

`SW1#**show ip dhcp snooping**
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
1,4
DHCP snooping is operational on following VLANs:
1
— removed for brevity -
DHCP snooping trust/rate is configured on the following Interfaces:
Interface                  Trusted    Allow option    Rate limit (pps)
----------------------     --------   ------------    ----------------
GigabitEthernet0/1         no         no              5
— remove for brevity —
GigabitEthernet0/10        yes        yes             unlimited`

#### The ARP Table and Dynamic ARP Inspection (DAI)

While forcing an association change in the CAM table by spoofing the source MAC address is effective, it is not very practical. The device that actually owns the MAC address may send a frame any time and update the table. ARP poisoning, on the other hand, is easier to execute and lasts a longer time.

With ARP poisoning, the ARP table of a router, switch, or target host is changed to map an IP address to the MAC address of a different host. This causes IP packets to be switched to a rogue device instead of the intended destination.

Two methods can be used to poison the ARP table. The first involves using a forged ARP response to a broadcasted query. The second involves using a Gratuitous ARP (GARP) packet to announce a false IP-to-MAC address mapping. The new mapping announced with the GARP packet overwrites the existing one and causes the packets to be delivered to the newly mapped MAC address.

ARP poisoning attacks can be prevented with the DAI security feature of a Cisco switch. When this feature is enabled, the switch checks all ARP packets against the DHCP snooping database to validate mappings being announced. If a mapping does not match the information in the database, it is dropped. Just as with DHCP snooping, interfaces can be configured as trusted or untrusted for DAI. Only ARP packets from untrusted interfaces are inspected. If the network segment contains hosts that do not use DHCP, an ARP access list must be configured for DAI to use.

DAI can be enabled for a VLAN with the **ip arp inspection vlan** *vlan-ID* command. *VLAN-ID* can be a single VLAN, a comma-separated list, or a range.

An ARP access list is created using the **arp access-list** *acl-name* command. Within the access list, individual entries are defined using the **permit ip host** *ip-address* **mac host** *mac-address* command. The access list can be applied to a VLAN with the **ip arp inspection filter** *filter-name* **vlan** *vlan-ID* command.

Example 2-45 shows DAI configuration on a switch. In this example, DAI is enabled on VLAN 1, an ARP inspection access list is applied for a static IP address, and Gi0/10 uplink is configured as a trusted interface.

#### **Example 2-45** *Configuring Dynamic ARP Inspection*

`SW1(config)#**ip arp inspection vlan 1**
SW1(config)#**arp access-list static-map**
SW1(config-arp-nacl)#**permit ip host 192.168.1.24 mac host 1001.1001.1001**
SW1(config-arp-nacl)#**exit**
SW1(config)#**ip arp inspection filter static-map vlan 1**
SW1(config)#**interface Gi0/10**
SW1(config-if)#**ip arp inspection trust**`

DAI configuration and operation can be verified with the **show ip arp inspection** command, as shown in Example 2-46.

#### **Example 2-46** *Verifying Dynamic ARP Inspection*

`SW1#**show ip arp inspection**
—removed for brevity—
 Vlan     Configuration    Operation   ACL Match         Static ACL
 ----     -------------    ---------   ---------         -----------
    1     Enabled          Active      static-map         No
 Vlan     ACL Logging      DHCP Logging      Probe Logging
 ----     -----------      ------------      -------------
    1     Deny             Deny              Off
 Vlan     Forwarded        Dropped     DHCP Drops      ACL Drops
 ----     ---------        -------     ----------      ---------
    1            19              6              6              0
—removed for brevity—`

#### Segmentation

Flat networks provide a high degree of flexibility because all resources and users are connected to the same network and can access anything. With the increasing popularity of wireless networks, accessing networks is even easier. The operational and administrative ease make flat networks very popular. You are probably wondering why we are talking about segmentation. Flat networks are not secure! With a flat network, anybody who has access can try to move laterally and may compromise the whole network. In fact, most worms do this.

The most common form of network architecture places critical assets in a central location such as a data center and segments that off while the rest of the network is kept flat. The problem with this approach is that it does not take into consideration the potential to escalate privilege with a lateral attack.

With the increasing numbers and types of devices, such as the IoT and mobile, it is more important than ever before to segment traffic even at the access layer. Such segmentation provides a few key benefits:

- **Network performance:** Segmentation divides broadcast domains and improves network performance. A very large and flat network has degraded performance due to the number of broadcast packets.
- **Reduced area of attack:** When an endpoint is compromised, an attacker or an agent such as a worm attempts to move laterally to compromise other endpoints. Segmentation limits the number of devices that can be targeted.
- **Compliance:** Certain standards, such as the Payment Card Industry Data Security Standard (PCI DSS), require relevant traffic to be kept separate. Segmentation helps achieve that while using the same physical infrastructure.
- **Secure mobility and guest access:** Users are increasingly mobile within and outside offices. On the other hand, visitors and employees require Internet access for their devices. Segmentation allows traffic from guests, non-corporate devices and remote users to be kept separate from normal business traffic.
- **Monitoring and analytics:** The difficulty involved in monitoring and analyzing traffic increases with the size of the network. With segmentation of traffic, monitoring can also be divided into smaller pieces while also adding context to analytics.

Segmentation at the Layer 2 data plane can be achieved with either VLANs or Cisco TrustSec. VLANs work by placing endpoints into logical broadcast domains, while TrustSec works by tagging traffic with scalable (formerly security) group tags (SGTs). TrustSec and SGTs are covered in detail in *Integrated Security Technologies and Solutions, Volume II*. While configuring and managing VLANs is beyond the scope of this book, this section looks at private VLANs (PVLANs).

Typically, devices within a VLAN belong to the same broadcast domain and can communicate directly without going through a Layer 3 device. Each VLAN is assigned a separate IP subnet, and inter-VLAN traffic is routed through a Layer 3 device. In a multitenant environment or segments with few devices, using multiple VLANs and subnets is not always feasible. In such situations, PVLANs can be used to provide segmentation within a VLAN.

PVLANs treat the existing VLAN domain as the *primary VLAN* and then create subdomains called *secondary VLANs*. Traffic within the secondary VLANs is kept separate and has to be routed by a Layer 3 device to reach another secondary VLAN. The traffic between secondary VLANs can be filtered at Layer 3 to provide additional security. PVLANs work by assigning switch ports into three types:

- **Promiscuous ports:** These ports are part of the primary VLAN and can communicate with all other ports in that VLAN, including all ports in secondary VLANs. These ports generally connect to the VLAN gateway to allow traffic to be routed between different secondary VLANs and other primary VLANs.
- **Isolated ports:** These ports are part of a secondary isolated VLAN. PVLANs block all traffic to isolated ports except traffic from promiscuous ports. Similarly, traffic received from an isolated port is forwarded only to promiscuous ports. Isolated ports provide an absolute separation for the hosts connected to them.
- **Community ports:** These ports are part of a secondary community VLAN. Community ports communicate with other ports in the same community VLAN and with promiscuous ports. These interfaces are isolated at Layer 2 from all other interfaces in other communities and from isolated ports within their private VLAN.

A PVLAN can work across multiple switches, and VTPv3 can be used to carry PVLAN information across the domain. Configuring PVLAN can be broken down into four steps:

- **Step 1. Defining the secondary VLANs:** Each secondary VLAN should be configured as required. The**private-vlan** {**community** |**isolated** } command is used in VLAN configuration mode for this.
- **Step 2. Defining the primary VLAN:** The primary VLAN is configured for the PVLAN, and the secondary VLANs are associated with it. A VLAN can be declared primary with the**private-vlan primary** command in VLAN configuration mode. Secondary VLAN associations are also declared in that mode, using the**private-vlan association***vlan-ID* command. Multiple VLANs can be specified as a comma-separated list.
- **Step 3. Configuring a promiscuous port:** A switch interface can be configured as a promiscuous port with the**switchport mode private-vlan promiscuous** command. The PVLANs should then be mapped to the interface with the**switchport private-vlan mapping***primary-vlan-ID* ,*secondary-vlan-list* command, where*secondary-vlan-list* is a comma-separated list of all secondary VLANs of that PVLAN.
- **Step 4. Configuring member ports:** The PVLAN can be enabled on each participating interface with the**switchport mode private-vlan host** command. The primary and secondary VLANs can be mapped to the interface with the**switchport private-vlan host-association***primary-vlan-ID secondary- vlan-ID* command.

Example 2-47 shows the configuration of private VLANs with interface Gi0/10 configured as the promiscuous port in primary VLAN 10, interface Gi0/11 configured as an isolated port in secondary VLAN 20, and interface Gi0/12 configured as a community port in secondary VLAN 30.

#### **Example 2-47** *Configuring Private VLANs*

`SW1(config)#**vlan 20**
SW1(config-vlan)#**private-vlan isolated**
SW1(config-vlan)#**exit**
SW1(config)#**vlan 30**
SW1(config-vlan)#**private-vlan community**
SW1(config-vlan)#**exit**
SW1(config)#**vlan 10**
SW1(config-vlan)#**private-vlan primary**
SW1(config-vlan)#**private-vlan association 20,30**
SW1(config-vlan)#**exit**
SW1(config)#**interface Gi0/10**
SW1(config-if)#**switchport mode private-vlan promiscuous**
SW1(config-if)#**switchport private-vlan mapping 10 20,30**
SW1(config-if)#**exit**
SW1(config)#**interface Gi0/11**
SW1(config-if)#**switchport mode private-vlan host**
SW1(config-if)#**switchport private-vlan host-association 10 20**
SW1(config-if)#**exit**
SW1(config)#**interface Gi0/12**
SW1(config-if)#**switchport mode private-vlan host**
SW1(config-if)#**switchport private-vlan host-association 10 30**`

#### Attacks Against Segmentation

VLANs and PVLANs are both subject to some attacks. The primary motivation behind the attacks is to send traffic outside the segment that the attacker belongs to without going through a Layer 3 device and any filtering configured there. Whereas VLANs are subject to *VLAN hopping attacks*, PVLANs are suspect to the unimaginatively named *private VLAN attacks*.

In a VLAN hopping attack, two methods can be used to send traffic outside the VLAN without going through a router:

- **Basic VLAN hopping:** To execute a basic VLAN hopping attack, the attacker establishes a trunk link with the switch and is then able to tag frames with any VLAN. As mentioned earlier, Cisco switch interfaces have DTP enabled by default, which allows trunk negotiation on any interface if not disabled.
- **Double tagging:** In this type of attack, the attacker sends a frame with two 802.1q tags. The first tag specifies a VLAN that the attacker’s host actually belongs to, and the second frame specifies a VLAN of the destination host. This attack attempts to exploit the fact that most trunks have their native VLANs set to the same one as the hosts, and they allow frames in native VLANs to be sent without any VLAN tags. This results in the first tag being stripped at the source switch and being delivered across a trunk to the destination switch, where the second tag is read and the frame is delivered to the destination.

Basic VLAN hopping attacks can easily be mitigated by disabling DTP negotiation on interfaces and by configuring non-trunk interfaces in access mode.

Double tagging attacks can be mitigated by either configuring the native VLAN on trunk links to be an unused VLAN or by forcing trunks to tag frames in native VLANs also. The native VLAN of a trunk interface can be changed with the **switchport trunk native vlan** *vlan-ID* command, and native VLAN tagging can be enabled with the **vlan dot1q tag native** global configuration command.

In a PVLAN attack, the attacker attempts to send a packet to a host in another isolated or community VLAN. This is done by sending a crafted IP packet with the following:

- Real source MAC and IP addresses
- A real destination IP address
- The destination MAC address of the gateway router instead of the destination host

Because the destination MAC address belongs to the gateway router connected to a promiscuous port, the switch delivers it. Because the router only looks at the destination IP address, it routes the packet to the destination. This results in the packet being delivered outside the PVLAN’s secondary VLAN.

PVLAN attacks can be mitigated by applying an ACL on the router interface, connected to the promiscuous port, to drop packets that originate from and are destined to the same IP subnet.

#### Traffic Filtering at Layer 2

While filtering and access lists are generally associated with routers and firewalls, they can also be applied at Layer 2 interfaces and to VLANs to provide granular security. The following are some of the benefits of using access lists at Layer 2:

- **Contextual filtering:** Filtering at Layer 3 is generally based on subnets of the source traffic. A subnet can have multiple types of devices, such as IP phones, workstations, printers, and such. This context of the device is lost when filtering is done at that level. On the other hand, the context of the device is known at the switch interface it connects to, and filtering can be designed based on that. For example, an IP phone only needs to communicate with a certain set of services, so filtering can be applied to drop traffic destined to any other service.
- **Containing lateral attacks:** In most cases, devices in a subnet do not need to communicate with each other. Most services are centrally located, and there is little need for endpoints in a subnet to communicate with each other. For example, in many cases workstation-to-workstation or printer-to-printer communication is not required. Restricting intra-subnet communication with Layer 2 ACLs reduces the chance for lateral movement from a compromised host.
- **Reduced load on Layer 3:** When filtering is applied right at source, the rest of the network has to do less. This improves network performance and simplifies configuration

Filtering on a Cisco switch can be configured with port access control lists (PACLs) or VLAN access control lists (VACLs).

PACLs are standard, extended, or named IP ACLs, and named MAC address ACLs applied to a switch interface. The syntax for creating PACLs is the same as the syntax for creating ACLs on any Cisco IOS router.

When the PACL is applied, it filters incoming traffic on an interface. A few restrictions apply to PACL:

- Log, reflect, and evaluate keywords cannot be used.
- Physical and logical link protocols such as CDP, STP, DTP, and VTP cannot be filtered with a PACL.
- Ingress traffic is evaluated against PACLs before any other ACLs, such as a VACL.
- PACLs take up Ternary Content-Addressable Memory (TCAM) space and should be kept as small as possible. Generally, 20 to 30 Access Control Entries (ACEs) per PACL is an acceptable value.

A PACL is applied to an interface with the **ip access-group** *access-list* **in** command. Example 2-48 shows a PACL applied to interface Gi0/5 to block RDP and Telnet traffic.

#### **Example 2-48** *Applying a PACL*

`SW1(config)#**ip access-list extended pacl-5**
SW1(config-ext-nacl)#**deny tcp any any eq 3389**
SW1(config-ext-nacl)#**deny udp any any eq 3389**
SW1(config-ext-nacl)#**deny tcp any any eq 23**
SW1(config-ext-nacl)#**permit ip any any**
SW1(config-ext-nacl)#**exit**
SW1(config)#**int Gi0/5**
SW1(config-if)#**ip access-group pacl-5 in**`

A PACL can be further augmented with the IP Source Guard (IPSG) feature, which uses information from DHCP snooping to dynamically configure a port such that traffic is allowed only if it is sourced from an IP address bound to the interface. This is an effective method for blocking traffic with spoofed IP addresses. IPSG can be enabled with the **ip verify source** command on the interface and verified with the **show ip verify source** exec mode command. Example 2-49 shows IPSG enabled on the Gi0/9 interface, and only packets with source IP address 192.168.1.10 will be allowed out.

#### **Example 2-49** *Verifying IP Source Guard*

`SW1#**show ip verify source**
Interface   Filter-type   Filter-mode   IP-address    Mac-address   Vlan  Log
---------   -----------   -----------   ----------    -----------   ----  ---
Gi0/9      ip             active       192.168.1.10                 1     disabled`

The second method for filtering traffic at Layer 2 is to use VACLs. VACLs filter traffic that enters the VLAN from any source, including hosts in the VLAN. This makes it an effective tool for filtering traffic between hosts in the same VLAN as well as traffic being received from outside.

VACLs are configured using a VLAN access map. An access map is a series of **match** and **action** sets that define interesting traffic and action to be taken on them. Interesting traffic is defined by matching an IPv4, IPv6, or MAC access list. For each set of matched traffic, two actions can be defined: **forward** or **drop**. Optionally, the **log** keyword can be used with the **drop** action.

Access maps are defined with the **vlan access-map** *name sequence* command. Each map can have multiple sequences, with each sequence defining a **match** and **action** set. The access map can be applied to VLANs with the **vlan filter** *map-name* **vlan-list** *vlan-list* command, where *vlan-list* can be a single VLAN, a range of VLANs, or multiple VLANs as a comma-separated list.

Example 2-50 shows a VLAN access map applied for VLAN 1 to drop RDP and Telnet traffic. Note that the named ACLs, **rdp-traffic** and **telnet-traffic**, include a **permit** statement for the interesting traffic. Interesting traffic is always defined with a **permit** statement in the ACL so that it matches a VACL sequence. The VACL itself, though, is configured to drop the matched traffic.

#### **Example 2-50** *Creating and Applying a VACL*

`SW1(config)#**ip access-list extended rdp-traffic**
SW1(config-ext-nacl)#**permit tcp any any eq 3389**
SW1(config-ext-nacl)#**permit udp any any eq 3389**
SW1(config-ext-nacl)#**exit**
SW1(config)#**ip access-list extended telnet-traffic**
SW1(config-ext-nacl)#**permit tcp any any eq 23**
SW1(config-ext-nacl)#**exit**
SW1(config)#**ip access-list extended other-traffic**
SW1(config-ext-nacl)#**permit ip any any**
SW1(config-ext-nacl)#**exit**
SW1(config)#**vlan access-map vacl1 10**
SW1(config-access-map)#**match ip address rdp-traffic**
SW1(config-access-map)#**action drop log**
SW1(config-access-map)#**exit**
SW1(config)#**vlan access-map vacl1 20**
SW1(config-access-map)#**match ip address telnet-traffic**
SW1(config-access-map)#**action drop log**
SW1(config-access-map)#**exit**
SW1(config)#**vlan access-map vacl1 30**
SW1(config-access-map)#**match ip address other-traffic**
SW1(config-access-map)#**action forward**
SW1(config)#**vlan filter vacl1 vlan-list 1**`

### Security at the Layer 3 Data Plane

The data plane of a Layer 3 device uses information learned from the control plane protocols to route traffic between subnets. It uses the IP headers to determine where the intended destination is and routes the packet to the next hop. Given that a router works on a subnet level, it is easy to apply broad controls such as filtering and QoS. This section looks at some of the most common security features applied at the data layer of a Layer 3 device.

#### Traffic Filtering at Layer 3

The primary method of filtering traffic at Layer 3 is using access control lists (ACLs). ACLs are the Swiss Army knife of security with various uses. From broad traffic filtering based on source or destination address to granular filtering based on ports, protocol characteristics, or time, ACLs can be used in various ways. As mentioned before, they are even used to classify traffic for other security and non-security features.

ACLs are sequential lists of **permit** or **deny** statements, called access control entries (ACEs), that packets are evaluated against until the first match. When a packet matches an ACE, the specified action is taken. Using ACLs consists of two steps:

- **Step 1. Creating ACLs:** The first step in using ACLs is to create them. The type of ACL and its content determine the steps required to create it. While there are many variations of access lists, sometimes based on their usage, the five most common types—standard, extended, named, time-based, and reflexive—are discussed in the following sections.
- **Step 2. Applying ACLs:** ACLs need to be applied to interfaces, in the path of traffic, before they can be used. In addition to the interface, the direction in which the ACL needs to be applied has to be specified. Cisco routers allow one ACL per interface per direction.

Before looking into specific ACL types, it is important to know that ACLs on Cisco routers use something called inverse masks or wildcard masks instead of subnet masks to define source and destination traffic. As the name implies, an inverse mask is an inversed subnet mask. When broken down into binary numbers, each 0 bit in an inverse mask indicates that the corresponding address bit has to match exactly, while a 1 indicates that the corresponding address bit can be anything. For example, an IP address of 10.1.2.0 with an inverse mask of 0.0.0.255 means all bits of the first three octets must match exactly, while all bits of the last octet can be anything. Any IP address from 10.1.2.0 to 10.1.2.255 will match such an inverse mask.

An easy way to determine the inverse mask for a given subnet mask is to subtract it from 255.255.255.255. Example 2-51 shows a few examples of this.

#### **Example 2-51** *Finding the Inverse Mask from a Subnet Mask*

`255.255.255.255 - 255.255.255.0 = 0.0.0.255
255.255.255.255 - 255.255.255.128 = 0.0.0.127
255.255.255.255 - 255.255.240.0 = 0.0.15.255
255.255.255.255 - 255.128.0.0 = 0.127.255.255`

One important thing to remember about ACLs is that each of them has an implicit deny at the end. Traffic that is not permitted by any ACE in the ACL will be denied by the implicit deny at the end.

#### Standard ACLs

Standard ACLs are the simplest form of ACLs, and they filter based on only the source IP address of a packet. These ACLs can be numbered between 1 and 99 or 1300 and 1999. The source address can be a single host address, a subnet defined with an inverse mask, or simply all hosts defined with the *any* keyword. The syntax for creating a standard ACL is as follows:

`**access-list** *access-list-number* {**permit**|**deny**}{**host** *source*|*source inverse-mask*|**any**}[**log**]`

The optional **log** keyword at the end of the command causes the router to generate a syslog every time a packet matches that ACL. The log message includes the ACL number, the action taken on the packet, the source IP address of the packet, and the number of matches from a source within a five-minute period. The **log** keyword should be used sparingly to reduce CPU load. Typically, this keyword is used on ACEs that deny certain traffic to monitor for suspicious activity.

The **access-list** command can be repeated with the same *access-list-number* as often as required to add more ACEs in the ACL. Each subsequent command adds the new ACE at the end of the existing list. The order of the ACEs in an ACL is important because the ACL will be evaluated from the top, and the first match will be applied to a given packet. So, if there is a **deny** statement before a **permit** statement for the same source, the packet will be dropped.

The access list can be applied to an interface with the **ip access-group** *access-list-number* {**in**|**out**} command. The **in** and **out** keywords indicate the direction in which the access list is applied.

Example 2-52 shows how an access list is created and applied to deny traffic from the 10.1.1.0/24 subnet and from host 10.2.1.1, while all other traffic is permitted coming into interface Gi1. In the example, note the last ACE that explicitly permits all traffic. If this ACE is not added, all traffic will be dropped because of the implicit deny.

#### **Example 2-52** *Creating and Applying a Standard ACL*

`Router(config)#**access-list 10 deny 10.1.1.0 0.0.0.255**
Router(config)#**access-list 10 deny host 10.2.1.1**
Router(config)#**access-list 10 permit any**
Router(config)#**interface Gi1**
Router(config-if)#**ip access-group 10 in**`

#### Extended ACLs

While standard ACLs are simple, they have limited functionality because they can only filter based on the source address. Most security policies require more granular filtering based on various fields, such as source address, destination address, protocols, and ports. Extended ACLs can be used for such filtering requirements.

Extended ACLs are created and applied in the same way as standard ACLs and follow similar rules of sequential ACEs and implicit deny. The difference begins with the ACL numbers: Extended ACLs are numbered from 100 to 199 and 2000 to 2699. The syntax of extended ACLs is also different, as shown here:

`**access-list** *access-list-number* {**deny**|**permit**} **protocol** *source destination*
[*protocol-options*] [**log|log-input**]`

As with standard ACLs, *source* and *destination* can be defined as a single host with the **host** keyword or a subnet with inverse mask or as all hosts with the **any** keyword.

The optional *protocol-options* part of the command differs based on the protocol specified in the ACE. For example, when TCP or UDP protocols are defined, **eq**, **lt**, and **gt** (equal to, less than, and greater than) keywords are available to specify ports to be matched. In addition to ports, various bits in the TCP headers, such as SYN and ACK, can also be matched with available keywords.

Example 2-53 shows access list 101 created and applied to block all Telnet and RDP traffic coming into interface Gi1. In addition, the access list blocks all communication between hosts 10.1.1.1 and 10.2.1.1. Note the use of the **eq** keyword with TCP in the first two lines to deny Telnet and RDP traffic.

#### **Example 2-53** *Creating and Applying Extended ACLs*

`Router(config)#**access-list 101 deny tcp any any eq 23**
Router(config)#**access-list 101 deny tcp any any eq 3389**
Router(config)#**access-list 101 deny ip host 10.1.1.1 host 10.2.1.1**
Router(config)#**access-list 101 permit ip any any**
Router(config)#**interface Gi1**
Router(config-if)#**ip access-group 101 in**`

#### Named ACLs

During the course of normal operations, it is not uncommon to see tens or hundreds of ACLs created on a device. Eventually, when ACLs are identified with numbers only, it becomes difficult to keep track of the reason an ACL was created. To make administering ACLs easy, you can give them names instead of numbers; in this case, they are called named ACLs. Both standard and extended ACLs can be created as named ACLs, and all the previous discussed rules apply. The difference is in the commands used to create the ACL. Named ACLs are created using the **ip access-list** command, shown here:

`**ip access-list** {**standard**|**extended**} *name*`

This command creates an ACL and brings you to the **nacl** prompt, denoted **config-std-nacl** for a standard ACL or **config-ext-nacl** for an extended ACL. At this prompt, you can add ACEs as usual, starting with a **permit** or **deny** command. The rest of the command follows the syntax discussed earlier for standard and extended ACLs.

The command to apply a named ACL is the same command that is used to apply the standard and extended ACLs except that a name is used instead of a number in the **ip access-group** command, as shown here:

`**ip access-group** *name {in*|*out}*`

To illustrate the differences and similarities between creating numbered and named ACLs, Example 2-54 re-creates the standard ACL from Example 2-52 as a named ACL, and Example 2-55 does the same for the extended ACL shown in Example 2-53.

#### **Example 2-54** *Creating and Applying Standard Named ACLs*

`Router(config)#**ip access-list standard bad-hosts**
Router(config-std-nacl)#**deny 10.1.1.0 0.0.0.255**
Router(config-std-nacl)#**deny host 10.2.1.1**
Router(config-std-nacl)#**permit any**
Router(config-std-nacl)#**exit**
Router(config)#**interface Gi1**
Router(config-if)#**ip access-group bad-hosts in**`

#### **Example 2-55** *Creating and Applying Extended Named ACLs*

`Router(config)#**ip access-list extended bad-traffic**
Router(config-ext-nacl)#**deny tcp any any eq 23**
Router(config-ext-nacl)#**deny tcp any any eq 3389**
Router(config-ext-nacl)#**deny ip host 10.1.1.1 host 10.2.1.1**
Router(config-ext-nacl)#**permit ip any any**
Router(config-ext-nacl)#**exit**
Router(config)#**interface Gi1**
Router(config-if)#**ip access-group bad-traffic in**`

#### Time Based ACLs

ACEs in an extended ACL can be configured to be enforced during certain times only. To do this, you specify a time range at the end of an ACE with the **time-range** *time-range-name* command. *time-range-name* references an object in IOS that defines a time period. The object can be created with the **time-range** *name* global configuration command. Within the object, a time period is defined as recurring or absolute. A recurring, or periodic, range starts and ends at the same time on certain days of the week, while an absolute range starts and ends at a specific date and time.

A recurring, or periodic, range can be defined with the **periodic** {*day-of-the-week*|**daily**|**weekdays**|**weekends**} *start-time* **to** *end-time* command. *start-time* and *end-time* are defined in *hh*:*mm* format.

An absolute range can be defined with the **absolute** {**start**|**end**} *hh*:*mm day month year* command. The *month* option is specified with the first three letters of a month.

In Example 2-56, two time ranges are created. The first range, called *daily*, is a periodic range that starts at 00:00 hours and ends at 02:00 hours. The second range is an absolute range that begins on December 1 at 00:00 hours and ends on December 31 at 21:59 hours.

#### **Example 2-56** *Recurring Time Range*

`Router(config)#**time-range daily**
Router(config-time-range)#**periodic daily 00:00 to 02:00**
Router(config-time-range)#**exit**
Router(config)#**time-range december**
Router(config-time-range)#**absolute start 00:00 01 December 2017 end 21:59  31 December 2017**
Router(config-time-range)#**exit**`

Example 2-57 shows the ACL from Example 2-50 modified to allow RDP traffic every day between midnight and 2:00 a.m. It uses the recurring time range shown in Example 2-53 to accomplish this. Note how the second ACE in the example is changed from **deny** to **permit** with the time range, while a new line is added below it to deny RDP traffic during other times. If the new line is not added, RDP traffic is permitted by the **permit ip any any** ACE at all times.

#### **Example 2-57** *Using a Time Range in an ACL*

`Router(config)#**access-list 101 deny tcp any any eq 23**
Router(config)#**access-list 101 permit tcp any any eq 3389 time-range daily**
Router(config)#**access-list 101 deny tcp any any eq 3389**
Router(config)#**access-list 101 deny ip host 10.1.1.1 host 10.2.1.1**
Router(config)#**access-list 101 permit ip any any**
Router(config)#**interface Gi1**
Router(config-if)#**ip access-group 101 in**`

#### Reflexive ACLs

It is difficult to keep track of all traffic that should be allowed between various networks connected to a router. This is especially true when one of those segments is a public network, such as the Internet or an untrusted network. For example, when a router connects to the Internet, traffic in response to queries sent from inside is expected and should be allowed, whereas any other traffic not in response to an explicit query should not be allowed.

Extended ACLs in IOS allow tracking of sessions in one direction while allowing return traffic in the other. This is done by marking each ACE that needs session tracking in an outbound ACL with the **reflect** keyword. Every time a new session matching that ACE is encountered, a reflexive ACE entry is created in an inbound ACL that is marked with the **evaluate** keyword.

Each reflexive ACE is a temporary entry in the inbound ACL that matches the IP address pair, protocol, and port pair of the original outbound session that triggered the reflect. The temporary ACE is removed after the original session ends. The end of a session is determined by TCP packets with FIN or RST bits set or when a certain configurable time has passed without any packets seen for that session. The latter option is always used for non-TCP sessions.

To further understand how reflexive ACLs work, consider the network diagram shown in Figure 2-4. The figure shows router R1 connected to the internal network 10.1.1.0/24 on interface Gi1 and to the Internet on interface Gi3. On Interface Gi3, an outbound ACL called **allow-out** and an inbound ACL called **allow-in** are configured. The contents of the two ACLs are shown in Example 2-58.

#### **Example 2-58** *Initial Configuration for a Reflexive ACL*

`R1#**show running-config**
—removed for brevity—
ip access-list extended allow-in
 permit tcp any eq www any
 permit tcp any eq 443 any
 deny   ip any any
ip access-list extended allow-out
 permit tcp any any eq www
 permit tcp any any eq 443
 deny   ip any any
!
interface GigabitEthernet3
 ip address dhcp
 ip nat outside
 ip access-group allow-in in
 ip access-group allow-out out
 negotiation auto
—removed for brevity—`

Notice that the inbound ACL allows all traffic sourced from ports 80 and 443. While this permits responses from web servers, it also permits an attacker to reach any inside destination with a packet sourced from port 80 or 443. Hence, this is not a very secure or desirable configuration.

With reflexive ACL, the router in Figure 2-4 can be configured such that all web traffic going out of Gi3 will be evaluated, and a reflexive ACE will be created in the **allow-in** ACL to allow only return traffic to come in from the Internet.

To configure this, the two ACLs on R1 are changed as shown in Example 2-59. Notice that the **allow-in** ACL contains a single **evaluate** command pointing to the reflected ACL **racl**. IOS creates temporary ACEs in the **allow-in** ACL as sessions, matching the two permit statements in the **allow-out** ACL, and exits the Gi3 interface toward the Internet.

#### **Example 2-59** *Configuring Reflexive ACL*

`R1#**show running-config**
—removed for brevity—
ip access-list extended allow-in
 evaluate racl
ip access-list extended allow-out
 permit tcp any any eq www reflect racl
 permit tcp any any eq 443 reflect racl
!
interface GigabitEthernet3
 ip address dhcp
 ip nat outside
 ip access-group allow-in in
 ip access-group allow-out out
 negotiation auto
—removed for brevity—`

#### Unicast Reverse Path Forwarding

One of the key uses of traffic filtering is to prevent traffic with spoofed sources. While ACLs can be used to do this, creating and maintaining ACLs for this purpose can be cumbersome. IOS provides a nifty security feature called Unicast Reverse Path Forwarding (URPF) to simplify detection of spoofed IP packets.

When Unicast RPF is enabled on an interface, the router examines all packets received on that interface to see if the packet was received at an interface that is the best return path back to the source of the packet. In other words, it does a reverse lookup, or a “look back,” on each packet to see if the source of the packet appears in its forwarding table and is connected to the same interface where the packet was received.

If the router determines that the source of the packet cannot be reached through the interface where the packet was received, it drops the packet.

URPF can be used in any “single-homed” environment, where there is essentially only one access point out of the network (that is, one upstream connection). Networks having one access point offer the best example of symmetric routing, which means the interface where a packet enters the network is also the best return path to the source of the IP packet. URPF is best used at the network perimeter for Internet or other untrusted networks or in ISP environments for customer network terminations.

URPF can be configured on an interface with the **ip verify unicast reverse-path** command and can be verified with the **show ip interface** *interface* command.

#### Network Based Application Recognition (NBAR)

While filtering traffic with IP access lists is a good security practice, it has its shortcomings. Lot of applications and protocols, such as Real Time Protocol (RTP), use dynamic port ranges and are difficult to block with ACLs. For a better security posture, it is important to understand and limit traffic based on applications and protocols. This is where the Network Based Application Recognition (NBAR) feature of IOS helps.

NBAR is a classification engine that recognizes and classifies a wide variety of protocols and applications, including web-based and other difficult-to-classify applications and protocols that use dynamic TCP/UDP port assignments.

As traffic flows through interfaces configured for NBAR discovery, it recognizes and classifies protocols or applications that the packets belong to. The router can be configured to apply QoS policies based on the NBAR classifications, including dropping packets belonging to unwanted applications and protocols. The following are some of the key uses of NBAR:

- **Classification by HTTP header:** NBAR can be used to classify traffic based on HTTP header information, including URL, host, MIME, and user agent. This classification can be used to filter traffic going to known malicious sites or coming from unknown clients.
- **Classification of peer-to-peer (P2P) file-sharing traffic:** NBAR can be used to identify and block traffic belonging to P2P file-sharing applications such as BitTorrent to prevent data loss.
- **Classification of real-time traffic:** NBAR can be used to classify and filter or rate limit RTP or Real Time Streaming Protocol (RTSP) to prevent misuse of network resources.

NBAR uses its Protocol Discovery function to classify traffic. With Protocol Discovery, you can discover any protocol or application traffic that is supported by NBAR and obtain statistics that are associated with that protocol. Protocol Discovery maintains the following per-protocol statistics for enabled interfaces:

- Total number of input packets and bytes
- Total number of output packets and bytes
- Input bit rates
- Output bit rates

To use NBAR in QoS policies, Protocol Discovery needs to be enabled on router interfaces with the **ip nbar protocol-discovery** command. The collected data from Protocol Discovery can be verified with the **show ip nbar protocol-discovery interface** *interface* command.

While an in-depth discussion on QoS is beyond the scope of this book, as a quick refresher, QoS is configured using the Modular QoS CLI (MQC). Earlier in this chapter, you saw MQC used to configure CoPP. The MQC is a CLI that allows you to define traffic classes (class maps), create and configure traffic policies (policy maps), and attach the traffic policies to interfaces. Using MQC to configure NBAR consists of the following steps:

- **Step 1. Defining a traffic class with a class map:** Class maps are created using the**class-map***name* command and contain a**match protocol** statement to classify traffic using NBAR. Hundreds of applications and protocols are available, with numerous sub-options to create granular classification.
- **Step 2. Defining a traffic policy:** A traffic policy or policy map defines what action to take on defined traffic classes. Policy maps are created using the**policy-map***name* command. Within a policy map, multiple class maps can be associated with actions such as**drop, police** , and**shape** .
- **Step 3. Attaching the traffic policy to an interface:** A traffic policy or policy map needs to be attached to an interface in order to be effective. This can be done using the**service-policy** {**input** |**output** }*policy-map-name* command.

Example 2-60 shows how NBAR is used with MQC to classify and drop P2P file-sharing traffic. This example is based on the network shown in Figure 2-4. Interface Gi1 on R1 is configured for NBAR Protocol Discovery, and a policy map is applied to that interface to drop P2P file-sharing traffic.

#### **Example 2-60** *Configuring NBAR to Drop P2P File-Sharing Traffic*

`R1(config)#**interface Gi1**
R1(config-if)#**ip nbar protocol-discovery**
R1(config-if)#**exit**
R1(config)#**class-map p2p-traffic**
R1(config-cmap)#**match protocol bittorrent**
R1(config-cmap)#**match protocol fasttrack**
R1(config-cmap)#**match protocol gnutella**
R1(config-cmap)#**match protocol kazaa2**
R1(config-cmap)#**exit**
R1(config)#**policy-map drop-p2p**
R1(config-pmap)#**class p2p-traffic**
R1(config-pmap-c)#**drop**
R1(config-pmap-c)#**exit**
R1(config-pmap)#**exit**
R1(config)#**interface Gi1**
R1(config-if)#**service-policy output drop-p2p**
R1(config-if)#**service-policy input drop-p2p**`

#### TCP Intercept

TCP Intercept is an important security feature on Cisco routers that is used to protect TCP servers from SYN-flooding attacks. A SYN-flooding attack occurs when a hacker floods a server with requests for TCP connections sourced from spoofed addresses. Because these messages come from spoofed or unavailable addresses, the connections do not get established, and the server is forced to keep them open for a while. The resulting volume of unresolved open connections eventually overwhelms the server and can cause it to deny service to valid requests.

The TCP Intercept feature protects the servers, such as web servers, in a network from such SYN-flooding attacks. It does so by intercepting and validating TCP connection requests. In intercept mode, the router intercepts TCP synchronization (SYN) packets from clients to servers. It establishes a connection with the client on behalf of the destination server, and if that connection is successful, it establishes a connection with the server on behalf of the client and knits the two half-connections together transparently.

With the router intercepting all TCP connection requests, a SYN-flooding attack never reaches the servers itself. To protect itself from being overwhelmed in case of an attack, the router uses aggressive timeouts of half-open or embryonic connections.

Another way to protect the server and the router both is to use TCP Intercept in *watch* mode. In this mode, the router does not intercept TCP connections but passively watches each connection request. If the request is not completed within a configured time interval, the router intervenes and terminates the connection.

Before configuring TCP Intercept, an extended access list containing a list of servers to protect needs to be defined. You can choose to allow the whole inside network to be protected, but that may cause the router to be overwhelmed. It is recommended that you define the critical servers that require the protection.

After defining the access list, TCP Intercept can be enabled with the **ip tcp intercept list** *access-list* global configuration command. The TCP Intercept mode can be configured with the **ip tcp intercept mode** {**intercept**|**watch**} command. Example 2-61 shows how TCP Intercept is enabled to protect three servers in watch mode.

#### **Example 2-61** *Configuring TCP Intercept*

`R1(config)#**access-list 105 permit tcp any host 10.1.1.10**
R1(config)#**access-list 105 permit tcp any host 10.1.1.11**
R1(config)#**access-list 105 permit tcp any host 10.1.1.12**
R1(config)#**ip tcp intercept list 105**
R1(config)#**ip tcp intercept watch**`
