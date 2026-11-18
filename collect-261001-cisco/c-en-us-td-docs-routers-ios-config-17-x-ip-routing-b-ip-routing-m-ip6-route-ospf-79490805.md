The documentation set for this product strives to use bias-free language. For the purposes of this documentation set, bias-free is defined as language that does not imply discrimination based on age, disability, gender, racial identity, ethnic identity, sexual orientation, socioeconomic status, and intersectionality. Exceptions may be present in the documentation due to language that is hardcoded in the user interfaces of the product software, language used based on RFP documentation, or language that is used by a referenced third-party product. Learn more about how Cisco is using Inclusive Language.
Open Shortest Path First version 3 (OSPFv3) is an IPv4 and IPv6 link-state routing protocol that supports IPv6 and IPv4 unicast
address families (AFs).
Complete the OSPFv3 network strategy and planning for your IPv6 network. For example, you must decide whether multiple areas
are required.
Enable IPv6 unicast routing.
Enable IPv6 on the interface.
Restrictions for IPv6 Routing: OSPFv3
When running a dual-stack IP network with OSPF version 2 for IPv4 and OSPFv3, be careful when changing the defaults for commands
used to enable OSPFv3. Changing these defaults may affect your OSPFv3 network, possibly adversely.
OSPFv3 is a routing protocol for IPv4 and IPv6. It is a link-state protocol, as opposed to a distance-vector protocol. Think
of a link as being an interface on a networking device. A link-state protocol makes its routing decisions based on the states
of the links that connect source and destination machines. The state of a link is a description of that interface and its
relationship to its neighboring networking devices. The interface information includes the IPv6 prefix of the interface, the
network mask, the type of network it is connected to, the devices connected to that network, and so on. This information is
propagated in various type of link-state advertisements (LSAs).
A device’s collection of LSA data is stored in a link-state database. The contents of the database, when subjected to the
Dijkstra algorithm, result in the creation of the OSPF routing table. The difference between the database and the routing
table is that the database contains a complete collection of raw data; the routing table contains a list of shortest paths
to known destinations via specific device interface ports.
OSPFv3, which is described in RFC 5340, supports IPv6 and IPv4 unicast AFs.
Comparison of OSPFv3 and OSPF Version 2
Much of OSPF version 3 is the same as in OSPF version 2. OSPFv3, which is described in RFC 5340, expands on OSPF version
2 to provide support for IPv6 routing prefixes and the larger size of IPv6 addresses.
In OSPFv3, a routing process does not need to be explicitly created. Enabling OSPFv3 on an interface will cause a routing
process, and its associated configuration, to be created.
In OSPFv3, each interface must be enabled using commands in interface configuration mode. This feature is different from
OSPF version 2, in which interfaces are indirectly enabled using the device configuration mode.
When using a nonbroadcast multiaccess (NBMA) interface in OSPFv3, you must manually configure the device with the list of
neighbors. Neighboring devices are identified by their device ID.
In IPv6, you can configure many address prefixes on an interface. In OSPFv3, all address prefixes on an interface are included
by default. You cannot select some address prefixes to be imported into OSPFv3; either all address prefixes on an interface
are imported, or no address prefixes on an interface are imported.
Unlike OSPF version 2, multiple instances of OSPFv3 can be run on a link.
OSPF automatically prefers a loopback interface over any other kind, and it chooses the highest IP address among all loopback
interfaces. If no loopback interfaces are present, the highest IP address in the device is chosen. You cannot tell OSPF to
use any particular interface.
LSA Types for OSPFv3
The following list describes LSA types, each of which has a different purpose:
Device LSAs (Type 1)—Describes the link state and costs of a device’s links to the area. These LSAs are flooded within an
area only. The LSA indicates if the device is an Area Border Router (ABR) or Autonomous System Boundary Router (ASBR), and
if it is one end of a virtual link. Type 1 LSAs are also used to advertise stub networks. In OSPFv3, these LSAs have no address
information and are network-protocol-independent. In OSPFv3, device interface information may be spread across multiple device
LSAs. Receivers must concatenate all device LSAs originated by a given device when running the SPF calculation.
Network LSAs (Type 2)—Describes the link-state and cost information for all devices attached to the network. This LSA is
an aggregation of all the link-state and cost information in the network. Only a designated device tracks this information
and can generate a network LSA. In OSPFv3, network LSAs have no address information and are network-protocol-independent.
Interarea-prefix LSAs for ABRs (Type 3)—Advertises internal networks to devices in other areas (interarea routes). Type 3
LSAs may represent a single network or a set of networks summarized into one advertisement. Only ABRs generate summary LSAs.
In OSPFv3, addresses for these LSAs are expressed as
prefix,
prefix length instead of
address,
mask. The default route is expressed as a prefix with length 0.
Interarea-device LSAs for ASBRs (Type 4)—Advertises the location of an ASBR. Devices that are trying to reach an external
network use these advertisements to determine the best path to the next hop. Type 4 LSAs are generated by ABRs on behalf of
ASBRs.
Autonomous system external LSAs (Type 5)—Redistributes routes from another autonomous system, usually from a different routing
protocol into OSPFv3. In OSPFv3, addresses for these LSAs are expressed as
prefix,
prefixlength instead of
address,
mask. The default route is expressed as a prefix with length 0.
Link LSAs (Type 8)—Have local-link flooding scope and are never flooded beyond the link with which they are associated. Link
LSAs provide the link-local address of the device to all other devices attached to the link, inform other devices attached
to the link of a list of prefixes to associate with the link, and allow the device to assert a collection of Options bits
to associate with the network LSA that will be originated for the link.
Intra-Area-Prefix LSAs (Type 9)—A device can originate multiple intra-area-prefix LSAs for each device or transit network,
each with a unique link-state ID. The link-state ID for each intra-area-prefix LSA describes its association to either the
device LSA or the network LSA and contains prefixes for stub and transit networks.
An address prefix occurs in almost all newly defined LSAs. The prefix is represented by three fields: PrefixLength, PrefixOptions,
and Address Prefix. In OSPFv3, addresses for these LSAs are expressed as
prefix,
prefixlength instead of
address,
mask. The default route is expressed as a prefix with length 0. Type 3 and Type 9 LSAs carry all prefix (subnet) information that,
in OSPFv2, is included in device LSAs and network LSAs. The Options field in certain LSAs (device LSAs, network LSAs, interarea-device
LSAs, and link LSAs) has been expanded to 24 bits to provide support for OSPFv3.
In OSPFv3, the sole function of the link-state ID in interarea-prefix LSAs, interarea-device LSAs, and autonomous-system
external LSAs is to identify individual pieces of the link-state database. All addresses or device IDs that are expressed
by the link-state ID in OSPF version 2 are carried in the body of the LSA in OSPFv3.
The link-state ID in network LSAs and link LSAs is always the interface ID of the originating device on the link being described.
For this reason, network LSAs and link LSAs are now the only LSAs whose size cannot be limited. A network LSA must list all
devices connected to the link, and a link LSA must list all of the address prefixes of a device on the link.
Load Balancing in OSPFv3
When a device learns multiple routes to a specific network via multiple routing processes (or routing protocols), it installs
the route with the lowest administrative distance in the routing table. Sometimes the device must select a route from among
many learned via the same routing process with the same administrative distance. In this case, the device chooses the path
with the lowest cost (or metric) to the destination. Each routing process calculates its cost differently and the costs may
need to be manipulated in order to achieve load balancing.
OSPFv3 performs load balancing automatically in the following way. If OSPFv3 finds that it can reach a destination through
more than one interface and each path has the same cost, it installs each path in the routing table. The only restriction
on the number of paths to the same destination is controlled by the
maximum-paths command. The default maximum paths is 16, and the range is from 1 to 64.
Addresses Imported into OSPFv3
When importing the set of addresses specified on an interface on which OSPFv3 is running into OSPFv3, you cannot select specific
addresses to be imported. Either all addresses are imported, or no addresses are imported.
OSPFv3 Customization
You can customize OSPFv3 for your network, but you likely will not need to do so. The defaults for OSPFv3 are set to meet
the requirements of most customers and features. If you must change the defaults, refer to the IPv6 command reference to find
the appropriate syntax.
Caution
Be careful when changing the defaults. Changing defaults will affect your OSPFv3 network, possibly adversely.
Force SPF in OSPFv3
When the
process keyword is used with the
clearipv6ospf command, the OSPFv3 database is cleared and repopulated, and then the SPF algorithm is performed. When the
force-spf keyword is used with the
clearipv6ospf command, the OSPFv3 database is not cleared before the SPF algorithm is performed.
Once you have
completed step 3 and entered OSPFv3 router configuration mode, you can perform
any of the subsequent steps in this task as needed to configure OSPFv3 Device
configuration.
Enters router
configuration mode for the IPv4 or IPv6 address family.
Note
The previous syntax for enabling OSPF for IPv6 was:
ipv6 router ospf process-id
The new recommended syntax is:
router ospfv3process-id
It is recommended to use the new syntax for configuring OSPF for both IPv4 and IPv6. When migrating from older configurations,
enter the new router ospfv3process-id command in global configuration mode. This automatically converts all existing OSPFv3 configurations at global and interface-level,
including ipv6 ospf commands to the updated CLI syntax. You do not have to manually reconfigure each OSPFv3 enabled interface.
Device# show ospfv3 database
OSPFv3 Device with ID (172.16.4.4) (Process ID 1)
Device Link States (Area 0)
ADV Device Age Seq# Fragment ID Link count Bits
172.16.4.4 239 0x80000003 0 1 B
172.16.6.6 239 0x80000003 0 1 B
Inter Area Prefix Link States (Area 0)
ADV Device Age Seq# Prefix
172.16.4.4 249 0x80000001 FEC0:3344::/32
172.16.4.4 219 0x80000001 FEC0:3366::/32
172.16.6.6 247 0x80000001 FEC0:3366::/32
172.16.6.6 193 0x80000001 FEC0:3344::/32
172.16.6.6 82 0x80000001 FEC0::/32
Inter Area Device Link States (Area 0)
ADV Device Age Seq# Link ID Dest DevID
172.16.4.4 219 0x80000001 50529027 172.16.3.3
172.16.6.6 193 0x80000001 50529027 172.16.3.3
Link (Type-8) Link States (Area 0)
ADV Device Age Seq# Link ID Interface
172.16.4.4 242 0x80000002 14 PO4/0
172.16.6.6 252 0x80000002 14 PO4/0
Intra Area Prefix Link States (Area 0)
ADV Device Age Seq# Link ID Ref-lstype Ref-LSID
172.16.4.4 242 0x80000002 0 0x2001 0
172.16.6.6 252 0x80000002 0 0x2001 0
Device# show ospfv3 neighbor
OSPFv3 Device with ID (10.1.1.1) (Process ID 42)
Neighbor ID Pri State Dead Time Interface ID Interface
10.4.4.4 1 FULL/ - 00:00:39 12 vm1
OSPFv3 Device with ID (10.2.1.1) (Process ID 100)
Neighbor ID Pri State Dead Time Interface ID Interface
10.5.4.4 1 FULL/ - 00:00:35 12 vm1
Example: Forcing SPF Configuration
The following example shows how to trigger SPF to redo the SPF and repopulate the routing tables:
The Cisco Support and Documentation website provides online resources to download documentation, software, and tools. Use
these resources to install and configure the software and to troubleshoot and resolve technical issues with Cisco products
and technologies. Access to most tools on the Cisco Support and Documentation website requires a Cisco.com user ID and password.
The following table provides release information about the feature or features described in this module. This table lists
only the software release that introduced support for a given feature in a given software release train. Unless noted otherwise,
subsequent releases of that software release train also support that feature.
Use Cisco Feature Navigator to find information about platform support and Cisco software image support. To access Cisco Feature
Navigator, go to www.cisco.com/go/cfn. An account on Cisco.com is not required.
Table 1. Feature Information for IPv6 Routing: OSPFv3
Feature Name
Releases
Feature Information
IPv6 Routing: OSPFv3
Cisco IOS Release 15.2(6)E
OSPF version 3 for IPv6 expands on OSPF version 2 to provide support for IPv6 routing prefixes and the larger size of IPv6
addresses.
Table 2. Feature Information for IPv6 Routing: OSPFv3