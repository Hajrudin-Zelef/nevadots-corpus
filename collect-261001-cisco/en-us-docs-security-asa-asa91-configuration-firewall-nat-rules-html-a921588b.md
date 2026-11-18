Configuring Twice NAT
Twice NAT lets you identify both the source and destination address in a single rule. This chapter shows you how to configure twice NAT and includes the following sections:
Note For detailed information about how NAT works, see Chapter3, “Information About NAT”
Information About Twice NAT
Twice NAT lets you identify both the source and destination address in a single rule. Specifying both the source and destination addresses lets you specify that a source address should be translated to A when going to destination X, but be translated to B when going to destination Y, for example.
Note For static NAT, the rule is bidirectional, so be aware that “source” and “destination” are used in commands and descriptions throughout this guide even though a given connection might originate at the “destination” address. For example, if you configure static NAT with port address translation, and specify the source address as a Telnet server, and you want all traffic going to that Telnet server to have the port translated from 2323 to 23, then in the command, you must specify the source ports to be translated (real: 23, mapped: 2323). You specify the source ports because you specified the Telnet server address as the source address.
The destination address is optional. If you specify the destination address, you can either map it to itself (identity NAT), or you can map it to a different address. The destination mapping is always a static mapping.
Twice NAT also lets you use service objects for static NAT-with-port-translation; network object NAT only accepts inline definition.
For detailed information about the differences between twice NAT and network object NAT, see the “How NAT is Implemented” section.
Twice NAT rules are added to section 1 of the NAT rules table, or if specified, section 3. For more information about NAT ordering, see the “NAT Rule Order” section.
Prerequisites for Twice NAT
- For both the real and mapped addresses, configure network objects or network object groups (the object network or object-group network command). Network object groups are particularly useful for creating a mapped address pool with discontinuous IP address ranges or multiple hosts or subnets. To create a network object or group, see the general operations configuration guide.
- For static NAT-with-port-translation, configure TCP or UDP service objects (the object service command). To create a service object, see the general operations configuration guide.
For specific guidelines for objects and groups, see the configuration section for the NAT type you want to configure. See also the “Guidelines and Limitations” section.
Guidelines and Limitations
This section includes the guidelines and limitations for this feature.
Supported in single and multiple context mode.
- Supported in routed and transparent firewall mode.
- In transparent mode, you must specify the real and mapped interfaces; you cannot use any .
- In transparent mode, you cannot configure interface PAT, because the transparent mode interfaces do not have IP addresses. You also cannot use the management IP address as a mapped address.
- In transparent mode, translating between IPv4 and IPv6 networks is not supported. Translating between two IPv6 networks, or between two IPv4 networks is supported.
- Supports IPv6.
- For routed mode, you can also translate between IPv4 and IPv6.
- For transparent mode, translating between IPv4 and IPv6 networks is not supported. Translating between two IPv6 networks, or between two IPv4 networks is supported.
- For transparent mode, a PAT pool is not supported for IPv6.
- For static NAT, you can specify an IPv6 subnet up to /64. Larger subnets are not supported.
- When using FTP with NAT46, when an IPv4 FTP client connects to an IPv6 FTP server, the client must use either the extended passive mode (EPSV) or extended port mode (EPRT); PASV and PORT commands are not supported with IPv6.
- (This limitation is for 9.1.0 to 9.1.5; this limitation was removed in 9.1.6 and following maintenance releases.) You cannot configure FTP destination port translation when the source IP address is a subnet (or any other application that uses a secondary connection); the FTP data channel establishment does not succeed. For example, the following configuration does not work:
- If you change the NAT configuration, and you do not want to wait for existing translations to time out before the new NAT information is used, you can clear the translation table using the clear xlate command. However, clearing the translation table disconnects all current connections that use translations.
Note If you remove a dynamic NAT or PAT rule, and then add a new rule with mapped addresses that overlap the addresses in the removed rule, then the new rule will not be used until all connections associated with the removed rule time out or are cleared using the clear xlate command. This safeguard ensures that the same address is not assigned to multiple hosts.
- You cannot use an object group with both IPv4 and IPv6 addresses; the object group must include only one type of address.
- When using the any keyword in a NAT rule, the definition of “any” traffic (IPv4 vs. IPv6) depends on the rule. Before the ASA performs NAT on a packet, the packet must be IPv6-to-IPv6 or IPv4-to-IPv4; with this prerequisite, the ASA can determine the value of any in a NAT rule. For example, if you configure a rule from “any” to an IPv6 server, and that server was mapped from an IPv4 address, then any means “any IPv6 traffic.” If you configure a rule from “any” to “any,” and you map the source to the interface IPv4 address, then any means “any IPv4 traffic” because the mapped interface address implies that the destination is also IPv4.
- Objects and object groups used in NAT cannot be undefined; they must include IP addresses.
- You can use the same objects in multiple rules.
- The mapped IP address pool cannot include:
– The mapped interface IP address. If you specify any interface for the rule, then all interface IP addresses are disallowed. For interface PAT (routed mode only), use the interface keyword instead of the IP address.
– (Transparent mode) The management IP address.
– (Dynamic NAT) The standby interface IP address when VPN is enabled.
Default Settings
- By default, the rule is added to the end of section 1 of the NAT table.
- (Routed mode) The default real and mapped interface is Any, which applies the rule to all interfaces.
- If you specify an optional interface, then the ASA uses the NAT configuration to determine the egress interface, but you have the option to always use a route lookup instead.
Configuring Twice NAT
This section describes how to configure twice NAT. This section includes the following topics:
Adding Network Objects for Real and Mapped Addresses
For each NAT rule, configure up to four network objects or groups for:
Objects are required unless you specify the any keyword inline to represent all traffic, or for some types of NAT, the interface keyword to represent the interface address. For more information about configuring a network object or group, see the general operations configuration guide.
Guidelines
- A network object group can contain objects and/or inline addresses of either IPv4 or IPv6 addresses. The group cannot contain both IPv4 and IPv6 addresses; it must contain one type only.
- See the “Guidelines and Limitations” section for information about disallowed mapped IP addresses.
- Source Dynamic NAT:
– You typically configure a larger group of real addresses to be mapped to a smaller group.
– The mapped object or group cannot contain a subnet; the object must define a range; the group can include hosts and ranges.
– If a mapped network object contains both ranges and host IP addresses, then the ranges are used for dynamic NAT, and the host IP addresses are used as a PAT fallback.
– The mapped object or group cannot contain a subnet; a network object must define a host, or for a PAT pool, a range; a network object group (for a PAT pool) can include hosts and ranges.
– The mapped object or group can contain a host, range, or subnet.
– The static mapping is typically one-to-one, so the real addresses have the same quantity as the mapped addresses. You can, however, have different quantities if desired. For more information, see the “Static NAT” section.
– The real and mapped objects must match; you can use the same object for both, or you can create separate objects that contain the same IP addresses.
- Destination Static NAT or Static NAT with port translation (the destination translation is always static):
– Although the main feature of twice NAT is the inclusion of the destination IP address, the destination address is optional. If you do specify the destination address, you can configure static translation for that address or just use identity NAT for it. You might want to configure twice NAT without a destination address to take advantage of some of the other qualities of twice NAT, including the use of network object groups for real addresses, or manually ordering of rules. For more information, see the “Main Differences Between Network Object NAT and Twice NAT” section.
– For identity NAT, the real and mapped objects must match; you can use the same object for both, or you can create separate objects that contain the same IP addresses.
– The static mapping is typically one-to-one, so the real addresses have the same quantity as the mapped addresses. You can, however, have different quantities if desired. For more information, see the “Static NAT” section.
– For static interface NAT with port translation (routed mode only), you can specify the interface keyword instead of a network object/group for the mapped address. For more information, see the “Static Interface NAT with Port Translation” section.
Detailed Steps
| object network obj_name ciscoasa(config)# object network MyInsNet ciscoasa(config-network-object)# subnet 10.1.1.0 255.255.255.0 | Adds a network object, either IPv4 or IPv6. | 
| object-group network grp_name  { network-object  { object   net_obj_name  \|  subnet_address netmask  \|  host   ip_address } \|  group-object   grp_obj_name } ciscoasa(config)# object network TEST ciscoasa(config-network-object)# range 10.1.1.1 10.1.1.70  ciscoasa(config)# object network TEST2 ciscoasa(config-network-object)# range 10.1.2.1 10.1.2.70  ciscoasa(config-network-object)# object-group network MAPPED_IPS ciscoasa(config-network)# network-object object TEST ciscoasa(config-network)# network-object object TEST2 ciscoasa(config-network)# network-object host 10.1.2.79 | Adds a network object group, either IPv4 or IPv6. | 
(Optional) Adding Service Objects for Real and Mapped Ports
Configure service objects for:
- Source real port (Static only) or Destination real port
- Source mapped port (Static only) or Destination mapped port
For more information about configuring a service object, see the general operations configuration guide.
Guidelines
- NAT only supports TCP or UDP. When translating a port, be sure the protocols in the real and mapped service objects are identical (both TCP or both UDP).
- The “not equal” ( neq ) operator is not supported.
- For identity port translation, you can use the same service object for both the real and mapped ports.
- Source Dynamic NAT—Source Dynamic NAT does not support port translation.
- Source Dynamic PAT (Hide)—Source Dynamic PAT does not support port translation.
- Source Static NAT or Static NAT with port translation—A service object can contain both a source and destination port; however, you should specify either the source or the destination port for both service objects. You should only specify both the source and destination ports if your application uses a fixed source port (such as some DNS servers); but fixed source ports are rare. For example, if you want to translate the port for the source host, then configure the source service.
- Source Identity NAT—A service object can contain both a source and destination port; however, you should specify either the source or the destination port for both service objects. You should only specify both the source and destination ports if your application uses a fixed source port (such as some DNS servers); but fixed source ports are rare. For example, if you want to translate the port for the source host, then configure the source service.
- Destination Static NAT or Static NAT with port translation (the destination translation is always static)—For non-static source NAT, you can only perform port translation on the destination. A service object can contain both a source and destination port, but only the destination port is used in this case. If you specify the source port, it will be ignored.
Detailed Steps
Configuring Dynamic NAT
This section describes how to configure twice NAT for dynamic NAT. For more information, see the “Dynamic NAT” section.
Detailed Steps
| Step 1 | Create network objects or groups for the: | See the “Adding Network Objects for Real and Mapped Addresses” section. If you want to translate all source traffic, you can skip adding an object for the source real addresses, and instead specify the any keyword in the nat command. If you want to configure destination static interface NAT with port translation only, you can skip adding an object for the destination mapped addresses, and instead specify the interface keyword in the nat command. | 
| Step 2 | (Optional) Create service objects for the: | See the “(Optional) Adding Service Objects for Real and Mapped Ports” section. | 
| Step 3 | nat [ ( real_ifc , mapped_ifc ) ] [ line \| { after-auto [ line ]}] source dynamic { real_obj \| any } { mapped_obj [ interface [ ipv6 ]]} [ destination static { mapped_obj \| interface [ ipv6 ]} real_obj ] [ service mapped_dest_svc_obj real_dest_svc_obj ] [ dns ] [ unidirectional ] [ inactive ] [ description desc ] ciscoasa(config)# nat (inside,outside) source dynamic MyInsNet NAT_POOL destination static Server1_mapped Server1 service MAPPED_SVC REAL_SVC | Configure dynamic NAT . See the following guidelines:  – Real—Specify a network object, group, or the any keyword. – Mapped—Specify a different network object or group. You can optionally configure the following fallback method: Interface PAT fallback—(Routed mode only) The interface keyword enables interface PAT fallback. If you specify ipv6 , then the IPv6 address of the interface is used. After the mapped IP addresses are used up, then the IP address of the mapped interface is used. For this option, you must configure a specific interface for the mapped_ifc . | 
|  |  | (Continued) – Mapped—Specify a network object or group, or for static interface NAT with port translation only, specify the interface keyword. If you specify ipv6 , then the IPv6 address of the interface is used. If you specify interface , be sure to also configure the service keyword. For this option, you must configure a specific interface for the real_ifc . See the “Static Interface NAT with Port Translation” section for more information. – Real—Specify a network object or group. For identity NAT, simply use the same object or group for both the real and mapped addresses.  | 
Examples
The following example configures dynamic NAT for inside network 10.1.1.0/24 when accessing servers on the 209.165.201.1/27 network as well as servers on the 203.0.113.0/24 network:
ciscoasa(config)# object network INSIDE_NW
ciscoasa(config-network-object)# subnet 10.1.1.0 255.255.255.0
ciscoasa(config)# object network MAPPED_1
ciscoasa(config-network-object)# range 209.165.200.225 209.165.200.254
ciscoasa(config)# object network MAPPED_2
ciscoasa(config-network-object)# range 209.165.202.129 209.165.200.158
ciscoasa(config)# object network SERVERS_1
ciscoasa(config-network-object)# subnet 209.165.201.0 255.255.255.224
ciscoasa(config)# object network SERVERS_2
ciscoasa(config-network-object)# subnet 203.0.113.0 255.255.255.0
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW MAPPED_1 destination static SERVERS_1 SERVERS_1
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW MAPPED_2 destination static SERVERS_2 SERVERS_2
The following example configures dynamic NAT for an IPv6 inside network 2001:DB8:AAAA::/96 when accessing servers on the IPv4 209.165.201.1/27 network as well as servers on the 203.0.113.0/24 network:
ciscoasa(config)# object network INSIDE_NW
ciscoasa(config-network-object)# subnet 2001:DB8:AAAA::/96
ciscoasa(config)# object network MAPPED_1
ciscoasa(config-network-object)# range 209.165.200.225 209.165.200.254
ciscoasa(config)# object network MAPPED_2
ciscoasa(config-network-object)# range 209.165.202.129 209.165.200.158
ciscoasa(config)# object network SERVERS_1
ciscoasa(config-network-object)# subnet 209.165.201.0 255.255.255.224
ciscoasa(config)# object network SERVERS_2
ciscoasa(config-network-object)# subnet 203.0.113.0 255.255.255.0
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW MAPPED_1 destination static SERVERS_1 SERVERS_1
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW MAPPED_2 destination static SERVERS_2 SERVERS_2
Configuring Dynamic PAT (Hide)
This section describes how to configure twice NAT for dynamic PAT (hide). For more information, see the “Dynamic PAT” section.
Guidelines
- If available, the real source port number is used for the mapped port. However, if the real port is not available, by default the mapped ports are chosen from the same range of ports as the real port number: 0 to 511, 512 to 1023, and 1024 to 65535. Therefore, ports below 1024 have only a small PAT pool that can be used. (8.4(3) and later, not including 8.5(1) or 8.6(1)) If you have a lot of traffic that uses the lower port ranges, you can now specify a flat range of ports to be used instead of the three unequal-sized tiers: either 1024 to 65535, or 1 to 65535.
- If you use the same PAT pool object in two separate rules, then be sure to specify the same options for each rule. For example, if one rule specifies extended PAT and a flat range, then the other rule must also specify extended PAT and a flat range.
For extended PAT for a PAT pool:
- Many application inspections do not support extended PAT. See the “Default Settings and NAT Limitations” section in “Getting Started with Application Layer Protocol Inspection,” for a complete list of unsupported inspections.
- If you enable extended PAT for a dynamic PAT rule, then you cannot also use an address in the PAT pool as the PAT address in a separate static NAT-with-port-translation rule. For example, if the PAT pool includes 10.1.1.1, then you cannot create a static NAT-with-port-translation rule using 10.1.1.1 as the PAT address.
- If you use a PAT pool and specify an interface for fallback, you cannot specify extended PAT.
- For VoIP deployments that use ICE or TURN, do not use extended PAT. ICE and TURN rely on the PAT binding to be the same for all destinations.
For round robin for a PAT pool:
- If a host has an existing connection, then subsequent connections from that host will use the same PAT IP address if ports are available. Note : This “stickiness” does not survive a failover. If the ASA fails over, then subsequent connections from a host may not use the initial IP address.
- Round robin, especially when combined with extended PAT, can consume a large amount of memory. Because NAT pools are created for every mapped protocol/IP address/port range, round robin results in a large number of concurrent NAT pools, which use memory. Extended PAT results in an even larger number of concurrent NAT pools.
Detailed Steps
| Step 1 | Create network objects or groups for the: | See the “Adding Network Objects for Real and Mapped Addresses” section. If you want to translate all source traffic, you can skip adding an object for the source real addresses, and instead specify the any keyword in the nat command. If you want to use the interface address as the mapped address, you can skip adding an object for the source mapped addresses, and instead specify the interface keyword in the nat command. If you want to configure destination static interface NAT with port translation only, you can skip adding an object for the destination mapped addresses, and instead specify the interface keyword in the nat command. | 
| Step 2 | (Optional) Create service objects for the: | See the “(Optional) Adding Service Objects for Real and Mapped Ports” section. | 
| Step 3 | nat [ ( real_ifc , mapped_ifc ) ] [ line \| { after-auto [ line ]}] source dynamic { real-obj \| any } { mapped_obj [ interface [ ipv6 ]] \| [ pat-pool mapped_obj [ round-robin ] [ extended ] [ flat [ include-reserve ]] [ interface [ ipv6 ]] \| interface [ ipv6 ]} [ destination static { mapped_obj \| interface [ ipv6 ]} real_obj ] [ service mapped_dest_svc_obj real_dest_svc_obj ] [ dns ] [ unidirectional ] [ inactive ] [ description desc ] ciscoasa(config)# nat (inside,outside) source dynamic MyInsNet interface destination static Server1 Server1 description Interface PAT for inside addresses when going to server 1 | Configures dynamic PAT (hide) . See the following guidelines:  – Real—Specify a network object, group, or the any keyword. Use the any keyword if you want to translate all traffic from the real interface to the mapped interface. – Mapped—Configure one of the following: - Network object—Specify a network object that contains a host address. - pat-pool —Specify the pat-pool keyword and a network object or group that contains multiple addresses. - interface —(Routed mode only) Specify the interface keyword alone to only use interface PAT. If you specify ipv6 , then the IPv6 address of the interface is used. When specified with a PAT pool or network object, the interface keyword enables interface PAT fallback. After the PAT IP addresses are used up, then the IP address of the mapped interface is used. For this option, you must configure a specific interface for the mapped_ifc . (continued) | 
|  |  | (continued) For a PAT pool, you can specify one or more of the following options: -- Round robin—The round-robin keyword enables round-robin address allocation for a PAT pool. Without round robin, by default all ports for a PAT address will be allocated before the next PAT address is used. The round-robin method assigns an address/port from each PAT address in the pool before returning to use the first address again, and then the second address, and so on. -- Extended PAT—The extended keyword enables extended PAT. Extended PAT uses 65535 ports per service , as opposed to per IP address, by including the destination address and port in the translation information. Normally, the destination port and address are not considered when creating PAT translations, so you are limited to 65535 ports per PAT address. For example, with extended PAT, you can create a translation of 10.1.1.1:1027 when going to 192.168.1.7:23 as well as a translation of 10.1.1.1:1027 when going to 192.168.1.7:80. -- Flat range—The flat keyword enables use of the entire 1024 to 65535 port range when allocating ports. When choosing the mapped port number for a translation, the ASA uses the real source port number if it is available. However, without this option, if the real port is not available, by default the mapped ports are chosen from the same range of ports as the real port number: 1 to 511, 512 to 1023, and 1024 to 65535. To avoid running out of ports at the low ranges, configure this setting. To use the entire range of 1 to 65535, also specify the include-reserve keyword. (continued) | 
|  |  | (continued) – Mapped—Specify a network object or group, or for static interface NAT with port translation only (routed mode), specify the interface keyword. If you specify ipv6 , then the IPv6 address of the interface is used. If you specify interface , be sure to also configure the service keyword. For this option, you must configure a specific interface for the real_ifc . See the “Static Interface NAT with Port Translation” section for more information. – Real—Specify a network object or group. For identity NAT, simply use the same object or group for both the real and mapped addresses.  | 
Examples
The following example configures interface PAT for inside network 192.168.1.0/24 when accessing outside Telnet server 209.165.201.23, and Dynamic PAT using a PAT pool when accessing any server on the 203.0.113.0/24 network.
ciscoasa(config)# object network INSIDE_NW
ciscoasa(config-network-object)# subnet 192.168.1.0 255.255.255.0
ciscoasa(config)# object network PAT_POOL
ciscoasa(config-network-object)# range 209.165.200.225 209.165.200.254
ciscoasa(config)# object network TELNET_SVR
ciscoasa(config-network-object)# host 209.165.201.23
ciscoasa(config)# object service TELNET
ciscoasa(config-service-object)# service tcp destination eq 23
ciscoasa(config)# object network SERVERS
ciscoasa(config-network-object)# subnet 203.0.113.0 255.255.255.0
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW interface destination static TELNET_SVR TELNET_SVR service TELNET TELNET
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW pat-pool PAT_POOL destination static SERVERS SERVERS
The following example configures interface PAT for inside network 192.168.1.0/24 when accessing outside IPv6 Telnet server 2001:DB8::23, and Dynamic PAT using a PAT pool when accessing any server on the 2001:DB8:AAAA::/96 network.
Configuring Static NAT or Static NAT-with-Port-Translation
This section describes how to configure a static NAT rule using twice NAT. For more information about static NAT, see the “Static NAT” section.
Detailed Steps
| Step 1 | Create network objects or groups for the: | See the “Adding Network Objects for Real and Mapped Addresses” section. If you want to configure source static interface NAT with port translation only, you can skip adding an object for the source mapped addresses, and instead specify the interface keyword in the nat command. If you want to configure destination static interface NAT with port translation only, you can skip adding an object for the destination mapped addresses, and instead specify the interface keyword in the nat command. | 
| Step 2 | (Optional) Create service objects for the: | See the “(Optional) Adding Service Objects for Real and Mapped Ports” section. | 
| Step 3 | nat [ ( real_ifc , mapped_ifc ) ] [ line \| { after-object [ line ]}] source static real_ob [ mapped_obj \| interface [ ipv6 ]] [ destination static { mapped_obj \| interface [ ipv6 ]} real_obj ] [ service real_src_mapped_dest_svc_obj mapped_src_real_dest_svc_obj ][ net-to-net ] [ dns ] [ unidirectional \| no-proxy-arp ] [ inactive ] [ description desc ] ciscoasa(config)# nat (inside,dmz) source static MyInsNet MyInsNet_mapped destination static Server1 Server1 service REAL_SRC_SVC MAPPED_SRC_SVC | Configures static NAT . See the following guidelines:  – Real—Specify a network object or group. – Mapped—Specify a different network object or group. For static interface NAT with port translation only, you can specify the interface keyword (routed mode only). If you specify ipv6 , then the IPv6 address of the interface is used. If you specify interface , be sure to also configure the service keyword (in this case, the service objects should include only the source port). For this option, you must configure a specific interface for the mapped_ifc . See the “Static Interface NAT with Port Translation” section for more information. – Mapped—Specify a network object or group, or for static interface NAT with port translation only, specify the interface keyword. If you specify ipv6 , then the IPv6 address of the interface is used. If you specify interface , be sure to also configure the service keyword (in this case, the service objects should include only the destination port). For this option, you must configure a specific interface for the real_ifc . – Real—Specify a network object or group. For identity NAT, simply use the same object or group for both the real and mapped addresses. | 
|  |  | (Continued)  | 
Examples
The following example shows the use of static interface NAT with port translation. Hosts on the outside access an FTP server on the inside by connecting to the outside interface IP address with destination port 65000 through 65004. The traffic is untranslated to the internal FTP server at 192.168.10.100:6500 through :65004. Note that you specify the source port range in the service object (and not the destination port) because you want to translate the source address and port as identified in the command; the destination port is “any.” Because static NAT is bidirectional, “source” and “destination” refers primarily to the command keywords; the actual source and destination address and port in a packet depends on which host sent the packet. In this example, connections are originated from outside to inside, so the “source” address and port of the FTP server is actually the destination address and port in the originating packet.
ciscoasa(config)# object service FTP_PASV_PORT_RANGE
ciscoasa(config-service-object)# service tcp source range 65000 65004
ciscoasa(config)# object network HOST_FTP_SERVER
ciscoasa(config-network-object)# host 192.168.10.100
ciscoasa(config)# nat (inside,outside) source static HOST_FTP_SERVER interface service FTP_PASV_PORT_RANGE FTP_PASV_PORT_RANGE
The following example shows a static translation of one IPv6 network to another IPv6 when accessing an IPv6 network, and the dynamic PAT translation to an IPv4 PAT pool when accessing the IPv4 network:
ciscoasa(config)# nat (inside,outside) source static INSIDE_NW MAPPED_IPv6_NW destination static OUTSIDE_IPv6_NW OUTSIDE_IPv6_NW
ciscoasa(config)# nat (inside,outside) source dynamic INSIDE_NW pat-pool MAPPED_IPv4_POOL destination static OUTSIDE_IPv4_NW OUTSIDE_IPv4_NW
Configuring Identity NAT
This section describes how to configure an identity NAT rule using twice NAT. For more information about identity NAT, see the “Identity NAT” section.
Detailed Steps
| Step 1 | Create network objects or groups for the: | See the “Adding Network Objects for Real and Mapped Addresses” section. If you want to perform identity NAT for all addresses, you can skip creating an object for the the source real addresses and instead use the keywords any any in the nat command. If you want to configure destination static interface NAT with port translation only, you can skip adding an object for the destination mapped addresses, and instead specify the interface keyword in the nat command. | 
| Step 2 | (Optional) Create service objects for the: | See the “(Optional) Adding Service Objects for Real and Mapped Ports” section. | 
| Step 3 | nat [ ( real_ifc , mapped_ifc ) ] [ line \| { after-object [ line ]}] source static { nw_obj nw_obj \| any any } [ destination static { mapped_obj \| interface [ ipv6 ]} real_obj ] [ service real_src_mapped_dest_svc_obj mapped_src_real_dest_svc_obj ] [ no-proxy-arp ] [ route-lookup ] [ inactive ] [ description desc ] ciscoasa(config)# nat (inside,outside) source static MyInsNet MyInsNet destination static Server1 Server1 | Configures identity NAT . See the following guidelines:  – Mapped—Specify a network object or group, or for static interface NAT with port translation only, specify the interface keyword (routed mode only).If you specify ipv6 , then the IPv6 address of the interface is used. If you specify interface , be sure to also configure the service keyword (in this case, the service objects should include only the destination port). For this option, you must configure a specific interface for the real_ifc . See the “Static Interface NAT with Port Translation” section for more information. – Real—Specify a network object or group. For identity NAT, simply use the same object or group for both the real and mapped addresses.  | 
|  |  | (Continued)  | 
Configuring Per-Session PAT Rules
By default, all TCP PAT traffic and all UDP DNS traffic uses per-session PAT. To use multi-session PAT for traffic, you can configure per-session PAT rules: a permit rule uses per-session PAT, and a deny rule uses multi-session PAT. For more information about per-session vs. multi-session PAT, see the “Per-Session PAT vs. Multi-Session PAT” section.
Detailed Steps
To configure a per-session PAT rule, see the “Configuring Per-Session PAT Rules” section.
Monitoring Twice NAT
To monitor twice NAT, enter one of the following commands:
| show nat | Shows NAT statistics, including hits for each NAT rule. | 
| show nat pool | Shows NAT pool statistics, including the addresses and ports allocated, and how many times they were allocated. | 
| show xlate | Shows current NAT session information. | 
| show nat divert-table | All NAT rules build an entry in the NAT divert table. If the NAT divert field is set to ignore=yes NAT on the matching rule, the ASA stops the lookup and does a route lookup based on the destination IP to determine the egress interface. If the NAT divert field is set to ignore=no on the matching rule, walk the NAT table based on the found input_ifc and output_ifc and do the necessary translation. Egress interface will be output_ifc. | 
Configuration Examples for Twice NAT
This section includes the following configuration examples:
- Different Translation Depending on the Destination (Dynamic PAT)
- Different Translation Depending on the Destination Address and Port (Dynamic PAT)
Different Translation Depending on the Destination (Dynamic PAT)
Figure 5-1 shows a host on the 10.1.2.0/24 network accessing two different servers. When the host accesses the server at 209.165.201.11, the real address is translated to 209.165.202.129: port . When the host accesses the server at 209.165.200.225, the real address is translated to 209.165.202.130: port .
Figure 5-1 Twice NAT with Different Destination Addresses
Step 1 Add a network object for the inside network:
Step 2 Add a network object for the DMZ network 1:
Step 3 Add a network object for the PAT address:
Step 4 Configure the first twice NAT rule:
ciscoasa(config)# nat (inside,dmz) source dynamic myInsideNetwork PATaddress1 destination static DMZnetwork1 DMZnetwork1
Because you do not want to translate the destination address, you need to configure identity NAT for it by specifying the same address for the real and mapped destination addresses.
By default, the NAT rule is added to the end of section 1 of the NAT table, See the “Configuring Dynamic PAT (Hide)” section for more information about specifying the section and line number for the NAT rule.
Step 5 Add a network object for the DMZ network 2:
Step 6 Add a network object for the PAT address:
Step 7 Configure the second twice NAT rule:
ciscoasa(config)# nat (inside,dmz) source dynamic myInsideNetwork PATaddress2 destination static DMZnetwork2 DMZnetwork2
Different Translation Depending on the Destination Address and Port (Dynamic PAT)
Figure 5-2 shows the use of source and destination ports. The host on the 10.1.2.0/24 network accesses a single host for both web services and Telnet services. When the host accesses the server for Telnet services, the real address is translated to 209.165.202.129: port . When the host accesses the same server for web services, the real address is translated to 209.165.202.130: port .
Figure 5-2 Twice NAT with Different Destination Ports
Step 1 Add a network object for the inside network:
Step 2 Add a network object for the Telnet/Web server:
Step 3 Add a network object for the PAT address when using Telnet:
Step 4 Add a service object for Telnet:
Step 5 Configure the first twice NAT rule:
ciscoasa(config)# nat (inside,outside) source dynamic myInsideNetwork PATaddress1 destination static TelnetWebServer TelnetWebServer service TelnetObj TelnetObj
Because you do not want to translate the destination address or port, you need to configure identity NAT for them by specifying the same address for the real and mapped destination addresses, and the same port for the real and mapped service.
By default, the NAT rule is added to the end of section 1 of the NAT table, See the “Configuring Dynamic PAT (Hide)” section for more information about specifying the section and line number for the NAT rule.
Step 6 Add a network object for the PAT address when using HTTP:
Step 7 Add a service object for HTTP:
Step 8 Configure the second twice NAT rule:
ciscoasa(config)# nat (inside,outside) source dynamic myInsideNetwork PATaddress2 destination static TelnetWebServer TelnetWebServer service HTTPObj HTTPObj
Feature History for Twice NAT
Table 5-1 lists each feature change and the platform release in which it was implemented.
| Twice NAT | 8.3(1) | Twice NAT lets you identify both the source and destination address in a single rule. We modified or introduced the following commands: nat , show nat , show xlate , show nat pool .  | 
| Identity NAT configurable proxy ARP and route lookup | 8.4(2)/8.5(1) | In earlier releases for identity NAT, proxy ARP was disabled, and a route lookup was always used to determine the egress interface. You could not configure these settings. In 8.4(2) and later, the default behavior for identity NAT was changed to match the behavior of other static NAT configurations: proxy ARP is enabled, and the NAT configuration determines the egress interface (if specified) by default. You can leave these settings as is, or you can enable or disable them discretely. Note that you can now also disable proxy ARP for regular static NAT. For pre-8.3 configurations, the migration of NAT exempt rules (the nat 0 access-list command) to 8.4(2) and later now includes the following keywords to disable proxy ARP and to use a route lookup: no-proxy-arp and route-lookup . The unidirectional keyword that was used for migrating to 8.3(2) and 8.4(1) is no longer used for migration. When upgrading to 8.4(2) from 8.3(1), 8.3(2), and 8.4(1), all identity NAT configurations will now include the no-proxy-arp and route-lookup keywords, to maintain existing functionality. The unidirectional keyword is removed. We modified the following command: nat source static [ no-proxy-arp ] [ route-lookup ].  | 
| PAT pool and round robin address assignment | 8.4(2)/8.5(1) | You can now specify a pool of PAT addresses instead of a single address. You can also optionally enable round-robin assignment of PAT addresses instead of first using all ports on a PAT address before using the next address in the pool. These features help prevent a large number of connections from a single PAT address from appearing to be part of a DoS attack and makes configuration of large numbers of PAT addresses easy. We modified the following command: nat source dynamic [ pat-pool mapped_object [ round-robin ]].  | 
| Round robin PAT pool allocation uses the same IP address for existing hosts | 8.4(3) | When using a PAT pool with round robin allocation, if a host has an existing connection, then subsequent connections from that host will use the same PAT IP address if ports are available. We did not modify any commands. This feature is not available in 8.5(1) or 8.6(1). | 
| Flat range of PAT ports for a PAT pool | 8.4(3) | If available, the real source port number is used for the mapped port. However, if the real port is not available, by default the mapped ports are chosen from the same range of ports as the real port number: 0 to 511, 512 to 1023, and 1024 to 65535. Therefore, ports below 1024 have only a small PAT pool. If you have a lot of traffic that uses the lower port ranges, when using a PAT pool, you can now specify a flat range of ports to be used instead of the three unequal-sized tiers: either 1024 to 65535, or 1 to 65535. We modified the following command: nat source dynamic [ pat-pool mapped_object [ flat [ include-reserve ]]]. This feature is not available in 8.5(1) or 8.6(1). | 
| Extended PAT for a PAT pool | 8.4(3) | Each PAT IP address allows up to 65535 ports. If 65535 ports do not provide enough translations, you can now enable extended PAT for a PAT pool. Extended PAT uses 65535 ports per service , as opposed to per IP address, by including the destination address and port in the translation information. We modified the following command: nat source dynamic [ pat-pool mapped_object [ extended ]].  This feature is not available in 8.5(1) or 8.6(1). | 
| Automatic NAT rules to translate a VPN peer’s local IP address back to the peer’s real IP address | 8.4(3) | In rare situations, you might want to use a VPN peer’s real IP address on the inside network instead of an assigned local IP address. Normally with VPN, the peer is given an assigned local IP address to access the inside network. However, you might want to translate the local IP address back to the peer’s real public IP address if, for example, your inside servers and network security is based on the peer’s real IP address. You can enable this feature on one interface per tunnel group. Object NAT rules are dynamically added and deleted when the VPN session is established or disconnected. You can view the rules using the show nat command. Note Because of routing issues, we do not recommend using this feature unless you know you need this feature; contact Cisco TAC to confirm feature compatibility with your network. See the following limitations:  We introduced the following command: nat-assigned-to-public-ip interface (tunnel-group general-attributes configuration mode).  | 
| NAT support for IPv6 | 9.0(1) | NAT now supports IPv6 traffic, as well as translating between IPv4 and IPv6. Translating between IPv4 and IPv6 is not supported in transparent mode. We modified the following commands: nat (global configuration mode), show nat , show nat pool , show xlate .  | 
| NAT support for reverse DNS lookups | 9.0(1) | NAT now supports translation of the DNS PTR record for reverse DNS lookups when using IPv4 NAT, IPv6 NAT, and NAT64 with DNS inspection enabled for the NAT rule. | 
| Per-session PAT | 9.0(1) | The per-session PAT feature improves the scalability of PAT and, for clustering, allows each member unit to own PAT connections; multi-session PAT connections have to be forwarded to and owned by the master unit. At the end of a per-session PAT session, the ASA sends a reset and immediately removes the xlate. This reset causes the end node to immediately release the connection, avoiding the TIME_WAIT state. Multi-session PAT, on the other hand, uses the PAT timeout, by default 30 seconds. For “hit-and-run” traffic, such as HTTP or HTTPS, the per-session feature can dramatically increase the connection rate supported by one address. Without the per-session feature, the maximum connection rate for one address for an IP protocol is approximately 2000 per second. With the per-session feature, the connection rate for one address for an IP protocol is 65535/ average-lifetime . By default, all TCP traffic and UDP DNS traffic use a per-session PAT xlate. For traffic that requires multi-session PAT, such as H.323, SIP, or Skinny, you can disable per-session PAT by creating a per-session deny rule. We introduced the following commands: xlate per-session , show nat pool .  |