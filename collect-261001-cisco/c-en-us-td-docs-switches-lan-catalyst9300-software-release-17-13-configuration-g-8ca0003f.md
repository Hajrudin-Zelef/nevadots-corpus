Information About IPv6 Unicast Routing
This chapter describes how to configure IPv6 unicast routing on the switch.
| Note | To use all IPv6 features in this chapter, the switch or active switch must be running the Network Advantage license. Switches running the Network Essentials license support IPv6 static routing and RIP for IPv6. Switches running the Network Advantage license support OSPF, EIGRP, and BGP for IPv6. | 
Understanding IPv6
IPv4 users can move to IPv6 and receive services such as end-to-end security, quality of service (QoS), and globally unique addresses. The IPv6 address space reduces the need for private addresses and Network Address Translation (NAT) processing by border routers at network edges.
For information about how Cisco Systems implements IPv6, go to Networking Software (IOS & NX-OS)
For information about IPv6 and other features in this chapter
- 
                                 
                                 See the Cisco IOS IPv6 Configuration Library.
- 
                                 
                                 Use the Search field on Cisco.com to locate the Cisco IOS software documentation. For example, if you want information about static routes, you can enter Implementing Static Routes for IPv6 in the search field to learn about static routes.
Static Routes for IPv6
Static routes are manually configured and define an explicit route between two networking devices. Static routes are useful for smaller networks with only one path to an outside network or to provide security for certain types of traffic in a larger network.
Configuring Static Routing for IPv6 (CLI)
For configuring static routes for IPv6, see the Configuring Static Routing for IPv6 section.
For more information about static routes, see the “Implementing Static Routes for IPv6” chapter in the Cisco IOS IPv6 Configuration Library on Cisco.com.
Path MTU Discovery for IPv6 Unicast
The switch supports advertising the system maximum transmission unit (MTU) to IPv6 nodes and path MTU discovery. Path MTU discovery allows a host to dynamically discover and adjust to differences in the MTU size of every link along a given data path. In IPv6, if a link along the path is not large enough to accommodate the packet size, the source of the packet handles the fragmentation.
ICMPv6
The Internet Control Message Protocol (ICMP) in IPv6 generates error messages, such as ICMP destination unreachable messages, to report errors during processing and other diagnostic functions. In IPv6, ICMP packets are also used in the neighbor discovery protocol and path MTU discovery.
Neighbor Discovery
The switch supports NDP for IPv6, a protocol running on top of ICMPv6, and static neighbor entries for IPv6 stations that do not support NDP. The IPv6 neighbor discovery process uses ICMP messages and solicited-node multicast addresses to determine the link-layer address of a neighbor on the same network (local link), to verify the reachability of the neighbor, and to keep track of neighboring routers.
The switch supports ICMPv6 redirect for routes with mask lengths less than 64 bits. ICMP redirect is not supported for host routes or for summarized routes with mask lengths greater than 64 bits.
Neighbor discovery throttling ensures that the switch CPU is not unnecessarily burdened while it is in the process of obtaining the next hop forwarding information to route an IPv6 packet. The switch drops any additional IPv6 packets whose next hop is the same neighbor that the switch is actively trying to resolve. This drop avoids further load on the CPU.
IPv6 Router Advertisement Options for DNS Configuration
Most of the internet services are identified by a Domain Name Server (DNS) name. IPv6 Router Advertisement (RA) provides the following two options to allow IPv6 hosts to perform automatic DNS configuration:
- 
                                    
                                    Recursive DNS Server (RDNSS)
- 
                                    
                                    DNS Search List (DNSSL)
RDNSS contains the address of recursive DNS servers that help in DNS name resolution in IPv6 hosts. DNS Search List is a list of DNS suffix domain names that are used by IPv6 hosts when they perform DNS query searches.
For more information on RA options for DNS configuration, refer IETF RFC 6106.
For configuring DNSSL, see the Configuring DNS Search List Using IPv6 Router Advertisement Options section of the IP Addressing Services Configuration Guide.
Default Router Preference
The switch supports IPv6 default router preference (DRP), an extension in router advertisement messages. DRP improves the ability of a host to select an appropriate router, especially when the host is multihomed and the routers are on different links. The switch does not support the Route Information Option in RFC 4191.
An IPv6 host maintains a default router list from which it selects a router for traffic to offlink destinations. The selected router for a destination is then cached in the destination cache. NDP for IPv6 specifies that routers that are reachable or probably reachable are preferred over routers whose reachability is unknown or suspect. For reachable or probably reachable routers, NDP can either select the same router every time or cycle through the router list. By using DRP, you can configure an IPv6 host to prefer one router over another, provided both are reachable or probably reachable.
For configuring DRP for IPv6, see the Configuring Default Router Preference section.
For more information about DRP for IPv6, see the Cisco IOS IPv6 Configuration Library on Cisco.com.
Policy-Based Routing for IPv6
Policy-based routing (PBR) gives you a flexible means of routing packets by allowing you to configure a defined policy for traffic flows, which lessens reliance on routes that are derived from routing protocols. Therefore, PBR gives you more control over routing by extending and complementing the existing mechanisms that are provided by routing protocols. PBR allows you to set the IPv6 precedence. For a simple policy, you can use any one of these tasks; for a complex policy, you can use all of them. It also allows you to specify a path for certain traffic, such as priority traffic over a high-cost link.
PBR for IPv6 may be applied to both forwarded and originated IPv6 packets. For forwarded packets, PBR for IPv6 will be implemented as an IPv6 input interface feature, supported in the following forwarding paths:
- 
                                    				
                                    Process
- 
                                    				
                                    Cisco Express Forwarding (formerly known as CEF)
- 
                                    				
                                    Distributed Cisco Express Forwarding
Policies can be based on the IPv6 address, port numbers, protocols, or packet size.
PBR allows you to perform the following tasks:
- 
                                    				
                                    Classify traffic based on extended access list criteria. Access lists, then, establish the match criteria.
- 
                                    				
                                    Set IPv6 precedence bits, giving the network the ability to enable differentiated classes of service.
- 
                                    				
                                    Route packets to specific traffic-engineered paths; you might need to route them to allow a specific quality of service (QoS) through the network.
PBR allows you to classify and mark packets at the edge of the network. PBR marks a packet by setting precedence value. The precedence value can be used directly by devices in the network core to apply the appropriate QoS to a packet, which keeps packet classification at your network edge.
For enabling PBR for IPv6, see the Enabling Local PBR for IPv6 section.
For enabling IPv6 PBR for an interface, see the Enabling IPv6 PBR on an Interface section.
Unsupported IPv6 Unicast Routing Features
The switch does not support these IPv6 features:
- 
                                    				
                                    IPv6 packets that are destined to site-local addresses.
- 
                                    				
                                    Tunneling protocols, such as IPv4-to-IPv6 or IPv6-to-IPv4.
- 
                                    				
                                    The switch as a tunnel endpoint supporting IPv4-to-IPv6 or IPv6-to-IPv4 tunneling protocols.
- 
                                    				
                                    IPv6 Web Cache Communication Protocol (WCCP).
IPv6 Feature Limitations
Because IPv6 is implemented in switch hardware, some limitations occurs due to the IPv6 compressed addresses in the hardware memory. This hardware limitation result in some loss of functionality and limits some features. For example, the switch cannot apply QoS classification on source-routed IPv6 packets in hardware.
IPv6 and Switch Stacks
The switch supports IPv6 forwarding across the stack and IPv6 host functionality on the active switch. The active switch runs the IPv6 unicast routing protocols and computes the routing tables. They receive the tables and create hardware IPv6 routes for forwarding. The active switch also runs all IPv6 applications.
If a new switch becomes the active switch, it recomputes the IPv6 routing tables and distributes them to the member switches. While the new active switch is being elected and is resetting, the switch stack does not forward IPv6 packets. The stack MAC address changes, which also change the IPv6 address. When you specify the stack IPv6 address with an extended unique identifier (EUI) by using the ipv6 address ipv6-prefix/prefix length eui-64 interface configuration command, the address is based on the interface MAC address. See the Configuring IPv6 Addressing and Enabling IPv6 Routing section.
If you configure the persistent MAC address feature on the stack and the active switch changes, the stack MAC address does not change for approximately 4 minutes.
These are the functions of IPv6 active switch and members:
- 
                                    				
                                    Active switch: 
  - 
                                          						
                                          runs IPv6 routing protocols
  - 
                                          						
                                          generates routing tables
  - 
                                          						
                                          distributes routing tables to member switches that use distributed Cisco Express Forwarding for IPv6
  - 
                                          						
                                          runs IPv6 host functionality and IPv6 applications
- 
                                          						
                                          
- 
                                    				
                                    Member switch: 
  - 
                                          						
                                          receives Cisco Express Forwarding for IPv6 routing tables from the active switch
  - 
                                          						
                                          programs the routes into hardware
 Note 
 IPv6 packets are routed in hardware across the stack if the packet does not have exceptions (IPv6 Options) and the switches in the stack have not run out of hardware resources. 
 
  - 
                                          						
                                          flushes the Cisco Express Forwarding for IPv6 tables on active switch re-election
- 
                                          						
                                          
Default IPv6 Configuration
| Table 1. Default IPv6 Configuration |  | 
|---|---|
| Feature | Default Setting | 
|---|---|
| SDM template | Default is advance template | 
| IPv6 routing | Disabled globally and on all interfaces | 
| Cisco Express Forwarding for IPv6 or distributed Cisco Express Forwarding for IPv6 | Disabled (IPv4 Cisco Express Forwarding and distributed Cisco Express Forwarding are enabled by default) | 
| IPv6 addresses | None configured | 
| Note | When IPv6 routing is enabled, Cisco Express Forwarding for IPv6 and distributed Cisco Express Forwarding for IPv6 are automatically enabled. |