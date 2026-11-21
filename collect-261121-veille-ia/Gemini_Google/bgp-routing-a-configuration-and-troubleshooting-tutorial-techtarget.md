---
id: collect-261121-veille-ia/gemini-google/bgp-routing-a-configuration-and-troubleshooting-tutorial-techtarget
title: "BGP routing: A configuration and troubleshooting tutorial | TechTarget"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["routing", "network", "isps", "path", "protocol", "between", "internet", "routers"]
source: https://www.techtarget.com/it-infrastructure/feature/BGP-routing-A-configuration-and-troubleshooting-tutorial
source_anchor: ""
source_lines: [1, 220]
section: "Gemini_Google"
---
Getty Images/iStockphoto

Service providers and network professionals working with IP networks agree that Border Gateway Protocol is one of the most complex and difficult-to-configure internet protocols. However, its emphasis on security and scalability makes it essential to the internet.

This tutorial examines BGP functionality and offers configuration and troubleshooting options, especially for administrators at ISPs or large regional or global network deployments. Use these steps and commands to manage BGP-enabled routers so they can exchange information efficiently and securely within the context of large networks or ISPs.

BGP is the routing protocol that makes the internet work. The protocol exchanges routing table information among BGP-speaking routers, enabling traffic across autonomous and diverse network environments.

Because IP address allocation lacks a structured assignment hierarchy, most service providers' core network routers must exchange information about several hundred thousand IP prefixes. BGP can manage this, as it's a highly scalable and security-focused routing protocol.

ISPs exchange BGP routing information between business entities on the public internet. They use peering agreements to define the necessary security conditions that enable routing exchanges between neighboring ISPs. Administrators configure BGP to adhere to these agreements. All adjacent routers must also satisfy the routing policies. Practical BGP implementations provide a rich set of route filters that enable ISPs to defend their networks and control what they advertise to their competitors.

### Autonomous systems

In BGP terminology, an independent routing domain -- which almost always means an ISP network -- is called an *autonomous system* (AS). ISPs use BGP as the routing protocol of choice in two scenarios:

1. **External BGP.** Exchanges routing tables between different ISPs on the internet.
2. **Internal BGP.** Exchanges routing tables within an ISP's own network.

BGP also evaluates routing tables and information along multiple routes between ISPs, using a selection algorithm that determines the best path to direct traffic based on specified commands and attributes. However, the best path isn't always the shortest path.

### BGP attributes

Many other routing protocols focus only on finding the optimal path toward all known destinations. BGP can't take this simplistic approach because the peering agreements between ISPs often result in complex routing policies. To help network administrators implement these policies, BGP carries many attributes with each IP prefix, including the following:

- **AS path.** The complete path documenting the ASes through which a packet must travel to reach the destination.
- **Local preference.** The internal cost of a destination, which ensures AS-wide consistency.
- **Multi-exit discriminator.** A BGP attribute that gives adjacent ISPs the ability to prefer one peering point over another.
- **Communities.** A set of generic tags that can signal various administrative policies between BGP routers.

Because BGP considers these factors, it might direct traffic to take one path from the destination and another path on its return trip, resulting in asymmetric routing. This isn't usually problematic for basic routing, but could complicate routes with firewalls and VPNs.

### BGP convergence

BGP's design and implementation focus on security and scalability, which makes it harder to configure than other routing protocols. This complexity is even more apparent when configuring other routing policies and network commands. BGP is also one of the slowest converging routing protocols.

The slow BGP convergence dictates a two-protocol design of an ISP network:

- Network administrators often use an internal routing protocol, such as Open Shortest Path First (OSPF) or Intermediate System to Intermediate System (IS-IS), to achieve faster convergence for internal routes, including IP addresses of BGP routers.
- Administrators configure BGP on routers to exchange internet routes.

The fast convergence of OSPF or IS-IS helps bypass a failure within the core network. At the same time, BGP on top of an internal routing protocol meets scalability, security and policy requirements. In addition, if you migrate customer routes into BGP, the customer problems don't affect the stability of your core network. For example, link flaps between your router and a customer's router don't affect your infrastructure.

Because of BGP's inherent complexity, customers and small ISPs often deploy BGP only where needed, such as on peering points. They typically configure it on a minimal subset of core routers, the ones between the peering points, as shown here.

The BGP-speaking routers must also generate a default route into the internal routing protocol to attract the traffic for internet destinations unknown to other routers in your network.

Any organization that wants to achieve redundant internet access must have its own AS and exchange BGP information with its ISPs. You'll likely deploy BGP on core and edge routers, so plan to include BGP on those routers as part of your initial network design, as shown here.

Although you might not rely on BGP everywhere in the initial network deployment, a blueprint could help when you need to scale the BGP-speaking part of the network.

In addition, BGP requires a full mesh of internal BGP sessions between routers in the same AS. It's possible to use BGP route reflectors or confederations to make the network scalable.

Another reason to deploy BGP throughout your network is that MPLS-based VPNs, large-scale quality of service deployments or large-scale differentiated web caching implementations rely on BGP to transport the information they need.

BGP commands and configurations vary by router vendor, but the overall process is similar across devices. Before you begin, ensure you have accurate documentation of all IP addresses, subnet masks, AS references and other network settings. Configuring BGP is challenging enough without adding inaccurate network information. Ensure basic connectivity between all involved devices, too. Don't let a pesky packet filter disrupt the process.

Begin by setting up the router. Enable secure administrative access, update the firmware and review the peering agreements before beginning the configuration. You must specify the BGP neighbors, define routing policies and advertise your router's networks.

### A basic BGP configuration example

An example of the basic BGP configuration steps on a Cisco router is as follows:

1. Enter the BGP configuration mode: router bgp {AS}.
2. Define the router's BGP neighbors using IP addresses and the remote AS: neighbor {IP} remote-as {AS}.
3. Advertise your router's networks to neighbors: network {network-address} mask {subnet-mask}.
4. Verify connectivity using tools like ping.
5. Set a router ID: bgp router-id {router-ID}.
6. Confirm the settings: show ip bgp summary and show ip bgp.

You've now configured BGP between routers in two separate AS environments.

Review the documentation for your specific network devices for additional options and settings. However, the general information should be similar across vendors.

Potential pitfalls include the following:

- BGP flapping -- a router or interface status going up and down -- due to hardware or media issues.
- Network congestion.
- Older, out-of-date router OSes or configurations.
- Authentication issues.

Remember to configure logging and monitoring. Also, test any failover or redundancy configurations to ensure they function as intended.

A structured approach to BGP troubleshooting can lead you from the initial problem diagnosis to a fix. Here is a simple scenario with a single BGP-speaking router in the network.

Multihomed customers and small ISPs use similar designs that don't offer BGP connectivity to their customers.

First, identify the source of the connectivity problem you're debugging. BGP might be involved if a customer reports they have limited or no internet connectivity beyond your network. Follow these quick steps to find the source:

1. Perform a traceroute from a workstation on the problematic LAN. If the trace reaches the first BGP-speaking router or gets beyond your network's edge, it's likely a BGP issue.
2. Check whether the BGP-speaking router advertises a default route into your network. Without a default route, other routers in your network can't reach internet destinations.

If you can't access a LAN-attached workstation, perform the traceroute from the customer-premises router. Be sure to use the router's LAN address as the source IP address in the traceroute packets.

### Adjacent BGP router issue troubleshooting

BGP must establish a TCP session between adjacent BGP routers before they can exchange routes. The first step is to check the status of the BGP sessions between the routers.

Administrators configure the BGP neighbors manually. Potential configuration errors include the following:

- **Neighbor IP address mismatch.** The destination IP address configured on one BGP neighbor must match the source IP address -- or the IP address of the directly connected interface -- configured on the other.
- **AS number mismatch.** The neighbor AS number configured on one side of the BGP session must match the neighbor's actual AS number.

You could also have a problem with packet filters deployed on the BGP-speaking router. These filters must send packets to and from TCP port 179.

### BGP route propagation troubleshooting

If your users want to receive traffic from the internet, the IP prefix assigned to your network must be visible throughout the internet. The following steps outline how to create a visible IP prefix:

1. Network administrators insert the IP prefix into their BGP routing table.
2. The BGP router advertises the IP prefix to its BGP neighbors.
3. Other networks propagate the IP prefix throughout the internet.

**Is the route inserted into BGP?**

Most routing protocols automatically insert directly connected IP subnets into their routing tables or databases. Due to security requirements, BGP is an exception. It originates an IP prefix only if you manually configure it. For example, Cisco routers configure advertised IP prefixes with the network statement.

Another option is route redistribution, which enables the network to direct traffic from a different protocol. However, this is highly discouraged in the internet environment.

Furthermore, BGP announces a configured IP prefix only if the IP routing table has a matching route to avoid attracting unrouteable traffic. You can generate the matching IP route through route summarization, but it's usually best to configure a static route that points to a null interface or its equivalent.

To check whether your IP prefix is in your BGP routing table, use a BGP show command. These commands vary depending on the type of routers you use. For example, type show ip bgp prefix mask on a Cisco router.

**Is the route advertised to your neighbors?**

The BGP router announces IP prefixes in the BGP table to all neighbors by default. You must implement output and input filters to modify this default behavior to comply with security and routing policy requirements.

If you have applied output filters toward your BGP neighbors, you must check whether these filters enable the BGP router to propagate the IP prefix to the external neighbors. The command to display routes advertised to a BGP neighbor on a Cisco router is show ip bgp neighbor ip-address advertised.

**Is the route visible throughout the internet?**

Even if you successfully announce your IP prefix to your BGP neighbors, the router might not propagate it throughout the internet. It's hard to figure out what the BGP router propagates beyond the boundaries of your network, but BGP looking glasses tools can help with this process. With these tools, you can inspect BGP tables at various points throughout the internet and check whether your IP prefix has made it to those destinations.

A few factors could block your IP prefix from the internet. The most common one is BGP route flap dampening: If an IP prefix flaps -- meaning it disappears and reappears -- too often in a short period, the prefix gets blocked for an extended period of time, up to an hour by default. This might happen if you clear your BGP sessions or change a configuration. If your IP prefix is dampened, you must wait it out.

You could also have an invalid or missing entry in the IP routing registries. Another possibility is that the upstream ISPs might have inbound filters. These problems are beyond the scope of technical BGP troubleshooting, so your upstream ISP can help you fix the problem.

### Advanced BGP troubleshooting

So far, this article has addressed identifying whether a routing problem is a BGP problem, troubleshooting BGP sessions, and troubleshooting IP route origination and propagation.

Next, let's focus on a more advanced scenario: transit ISP networks.

To establish end-to-end connectivity across a service provider network, the ISP must receive customers' IP prefixes via BGP and announce them to other ISPs. The exact process must happen in reverse; the ISP must announce the default route to the customer.

Network-wide BGP troubleshooting includes the following steps:

1. Receive the IP prefix.
2. Propagate the IP prefix across your network.
3. Send the IP prefix to external BGP neighbors at the other edge of the network.

**Have you received the prefix?**

Identifying inbound BGP problems can be the most difficult part of troubleshooting. Potential reasons an IP prefix isn't in your BGP table are as follows:

- The neighbor is not sending the prefix.
- Your inbound filters are blocking the prefix.

The debugging facility on your edge router is the only tool to help you identify the problem. You typically don't have access to the other BGP neighbor. When debugging, you must recognize that a BGP neighbor can send several hundred thousand routes. Ensure the debugging output produced by the troubleshooting session doesn't overwhelm the router.

Furthermore, the BGP router sends prefixes only when they change, not periodically like with Routing Information Protocol updates or OSPF link-state advertisement floods. Your debugging tool does not show you an IP prefix until it has changed or you've cleared the BGP session with your neighbor.

Some BGP routers can store a separate copy of all routes a neighbor sends in a parallel BGP table. To enable this functionality on Cisco Internetworking Operating System, configure soft-reconfiguration in for a BGP neighbor.

With the parallel per-neighbor table, you can pinpoint what the neighbor has sent or the content of the parallel table. You can also see the routes that have passed your input filters or the contents of the main BGP table. However, the parallel per-neighbor table consumes a large amount of memory.

**Is the IP prefix propagated across your network?**

Even when an edge router receives an IP prefix via BGP, it might not propagate to the other end of your network. First, an internal BGP -- or BGP within a single AS -- requires a full mesh of BGP sessions among all routers. Every router between every pair of edge routers must run BGP, or the network might drop the traffic. This means the number of BGP sessions could become excessively large.

This illustrates the BGP sessions needed in a small four-router network.

Two tools -- BGP route reflectors and BGP confederations -- can help you keep the number of sessions to a reasonable level. Route reflectors are most commonly used.

BGP route reflector rules are as follows:

- The route reflector sends to every other peer what it receives from a route reflector client or an external BGP peer.
- The route reflector only sends what it receives from non-route reflector clients to other clients and external BGP peers.

Once you've established these rules, it's time to examine the BGP sessions in your network. Check every BGP router on the way, and ensure they don't violate the route reflector rules. The BGP prefixes propagate from every edge router to all other routers using these rules. This is where thorough network documentation is essential.

Another common reason an IP prefix doesn't propagate across your network is that the external subnets on the edge of your network are not advertised to your core routers.

The IP address of the next-hop router doesn't change when a BGP router sends an IP prefix to an internal BGP neighbor. Thus, the IP next hop of an external route is always the IP address of a router one hop beyond the edge of your AS.

Network administrators must insert the IP subnets that connect edge routers to their external neighbors into their internal routing protocol, such as OSPF or IS-IS. Otherwise, some internal BGP routers decide if the BGP next hop is unreachable and ignore the IP prefix. It appears in the BGP table, but the router can't use or propagate it to other BGP peers.

**Is the prefix sent to external neighbors?**

As the last step in troubleshooting BGP route propagation, check whether the IP prefixes transported across your network are announced to your external BGP peers. This article explains the techniques for troubleshooting outbound BGP route propagation.

**Is the traffic traversing the network?**

Even if your BGP route propagation works well, the IP packets might not be able to traverse your network. These are pure IP networks, so the process could change if you add MPLS.

The most common cause of a black hole in your network is a router in the transit path that doesn't run BGP and doesn't know how to route the received IP packet toward the destination network. IP routing works hop by hop. Even though the ingress edge router knows which egress edge router to use and how to get there, it can't pass that information to the intermediate routers. All of them must also run BGP.

To identify a black hole in your network, perform a traceroute from your customer's network to an internet destination. The last router that responds to the traceroute is one hop before the black hole.

Even though all core routers in your network must run BGP, the internal BGP sessions don't have to follow the network's physical structure. For example, you could have a few central routers acting as route reflectors for all BGP routers in your network.

BGP is a critical but complex protocol. Many factors complicate its configuration and troubleshooting, including the following:

- Peering agreements that define the routing policies and security settings for route exchanges between separate networks.
- Security settings, including filters, authentication or other controls.
- Areas of the network that are beyond your control, such as other ISPs or partner networks.
- Custom maximum transmission unit settings that don't match.

Begin working with BGP after determining it's necessary. It's a common protocol for large, routed networks and ISPs, but most small organizations never need BGP. If your organization establishes a need for BGP, address some prerequisites before you begin configuration:

- Create an accurate network map.
- Confirm you have the correct identifiers, including IP addresses, router IDs and AS numbers. You need these values to configure BGP, so document them beforehand.
- Address any basic connectivity, router security or network congestion challenges.
- Confirm you have the correct authentication information for all devices.

Don't underestimate the complexity of deploying and managing a BGP-enabled network. BGP is a complex protocol, but you can grasp this essential internet routing protocol by understanding the configuration steps and creating trustworthy network documentation.

**Editor's note:** *This article was originally written by Ivan Pepelnjak and expanded by Damon Garn to include more BGP troubleshooting information.*

*Damon Garn owns Cogspinner Coaction and provides freelance IT writing and editing services. He has written multiple CompTIA study guides, including the Linux+, Cloud Essentials+ and Server+ guides, and contributes extensively to Informa TechTarget Editorial, The New Stack and CompTIA Blogs.*
