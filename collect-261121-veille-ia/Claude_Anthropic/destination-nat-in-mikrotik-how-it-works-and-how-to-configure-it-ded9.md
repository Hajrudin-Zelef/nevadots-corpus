---
id: collect-261121-veille-ia/claude-anthropic/destination-nat-in-mikrotik-how-it-works-and-how-to-configure-it-ded9
title: "Destination NAT in MikroTik: How It Works and How to Configure It - DED9"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["address", "destination", "private", "mikrotik", "public", "addresses", "technology", "same"]
source: https://ded9.com/destination-nat-in-mikrotik/
source_anchor: ""
source_lines: [1, 170]
section: "Claude_Anthropic"
---
Mikrotik is the designer of the Mikrotik operating system and also manufactures hardware products called Routerboards and products such as bandwidth controllers. One of the useful commands in Mikrotik routers is Destination NAT, which is used to change the public IP address to a private internal IP address. In this article, from the IT assistant, we will learn about NAT technology, Destination NAT, and Destination NAT training in Mikrotik.

## What is NAT technology?

NAT stands for Network Address Translation. It is a method of routing information in the Internet space that translates several local private addresses to public addresses before transferring the information. Organizations that want multiple devices to use the same IP address in their internal environment use NAT. Home routers work based on this technology. NAT technology is one of the most important technologies in computer networks.

## How does NAT work?

Suppose a laptop is connected to a home router. A person uses the laptop to search for directions to their favorite restaurant. The laptop sends this request in a data packet to the router, which forwards it to the web. But before sending the information, the router first changes the outgoing IP address from a private local address to a public address.

If the intended data packet was sent with the same private IP address, the receiving server would not know where to send the data. This issue is similar to sending physical letters and requesting return or reply services. If the IP address is sent to the receiving server, it is the same as providing an anonymous address instead of our address on the mail or leaving it blank. With NAT, information is routed back to the sender’s laptop using the router’s public address, not the laptop’s private address.

## The difference between Source NAT and Destination NAT

NAT routing technology works in the third layer of the OSI model, or the Network layer. This is why NAT works with IP addresses. As mentioned earlier, the basic job of NAT is to convert a local private IP address to a public IP address and vice versa. The same route and round-trip create two types of NAT operations. The first case is when the private IP is converted to a public IP, called the Source NAT operation. The second case is when the public IP is converted or translated into a private IP called Destination NAT.

- Destination NAT operation and technology change the destination address of the packets passing through the router. This method provides a solution for port translation in TCP/UDP headers.
- Destination NAT usually directs incoming packets with an external address or port destination to an internal IP address or port within the network.
- Different applications of Destination NAT

Destination NAT is performed, destination IP addresses are configured and translated according to Destination NAT rules, and security policies are applied. Destination NAT is commonly used to perform the following actions:

- Translate a single IP address to another: for example, to allow a device on the Internet to connect to a host on a private network
- Translate a contiguous block of IP addresses into another block of addresses of the same size: for example, to allow access to a group of servers.
- Translate the destination IP address and port to another destination IP address and port: for example, to allow access to multiple services using the same IP address but different ports.

## Destination NAT address pool definition

A NAT Pool is a set of user-defined IP addresses used for translation. Unlike static NAT, where there is one-to-one routing and IP translation, group-to-group IP translation is possible here. Meanwhile, in this case, the original destination IP address is translated to an IP address from a user-defined pool. Therefore, if the original destination IP address range is larger than the address range in the user-defined address pool, any untranslated packets are dropped. This feature is one of the applications of Destination NAT.

You can configure a NAT Pool to exist in the default routing instance. A configuration option specifies whether a NAT pool exists in the default routing instance. As a result, the NAT pool is discoverable and reachable from zones in the default routing instance and from zones in other routing instances.

Introduction of Mikrotik company

MikroTik is a network equipment manufacturer based in Latvia, Europe. It develops and sells wired and wireless network routers, switches, operating systems, and supporting software.

Mikrotik was founded in 1996 to sell network equipment in emerging markets. As of August 2019, the company’s website reported the number of employees to be around 280. In 2015, Mikrotik was the 20th largest company in Latvia, with revenues of 202 million euros. Mikrotik was originally a PC software company. In 2002, the company began producing its hardware.

## What is the Mikrotik router operating system?

MikroTik router operating system, MikroTik RouterOS, is an independent Linux operating system used in MikroTik network equipment. Of course, this is more than just an operating system for routers. This software can even be installed on regular PCs to turn them into dedicated routers.

Mikrotik’s operating system increases the efficiency of network equipment manufactured by Mikrotik. This operating system is also highly flexible, allowing it to be installed on computers and turned into routers. The Mikrotik operating system has routing, Firewall, bandwidth management, creation of wireless access points, backhaul link, spot ports, and a VPN server.

RouterOS operating system can be used in networks in two ways: in the first case, this operating system can be installed on a personal computer or virtual machine; The second mode is when this operating system is installed on the RouterBoard hardware in Mikrotik’s physical equipment. In both cases, we have created a Mikrotik router. Routerboards are Mikrotik’s exclusive hardware and RouterOS operating system with several network ports.

## Advantages of using the Mikrotik router operating system

Companies can use Mikrotik’s operating system as a trial solution during the R&D process. Practical results have shown that this operating system’s research and development phase has succeeded. Many companies have extensively implemented it to serve their customers as part of their development solution. Today, this operating system can be converted into a cloud management suite since it adds many improvements and functions. Let’s review some of the advantages of using this operating system:

- It is affordable
- It is easy to deploy
- It is powerful and practical
- Compared to other brands at the same price, it has more features
- It is widely available
- Today, it can be easily obtained
- It is very adjustable and flexible
- You can use it to write a powerful script to improve performance
- You can create a different configuration approach based on your needs

User management training with Mikrotik – click

## Training destination NAT in Mikrotik for more safety

Network address translation in the Destination NAT method is possible by changing the network address information in the IP header of packets. Let’s look at a common setup where a network administrator wants to access an administrative server from the Internet. We want to allow connections from the Internet to the administration server, whose local IP is 10.0.0.3. In this case, we need to configure a destination address translation rule on the administrative gateway router as follows:

The above rule translates to: When an incoming connection requests TCP port 22 with destination address 172.16.16.1, use the Destination NAT or DST-nat function and forward the packets to the device with local IP address 10.0.0.3 and port 22 transfer.

Note: To allow access, but only from the home PC, we can improve our DST-nat rule with “src-address=192.168.88.1,” which is a public IP address of the home PC for added security. You can choose and participate in this training course to fully familiarize yourself with topics related to network security:

Training destination NAT in Mikrotik for port forwarding operations

As we explained at the beginning of the article, Destination NAT is used to change the IP address and port. Port forwarding is a daily need for internet users, website administrators, and server and host management. In the following, we will learn about this concept, the use of Destination NAT, and how to set it up in Mikrotik.

## What is port forwarding?

Port forwarding intercepts and routes data traffic to a computer’s IP/port combination and redirects it to another IP or port. This process can be easily done using a MikroTik router or any system running RouterOS. It also creates a better user experience and optimal user management when using the Mikrotik router.

### How to use and configure port forwarding in Mikrotik

To understand the necessity and application of the port forwarding process, suppose you are an IT manager. You have created a large network, and someone wants to remotely connect to your VPS or dedicated server to work remotely. You cannot share the server IP with that person for security reasons. What should you do?

In this situation, you must use port forwarding in the Mikrotik router to handle all requests, and this operation is performed based on the Destination NAT feature. To configure port forwarding in MikroTik, you must first make sure that you have installed the latest version of MikroTik RouterOS, and then do these things step by step:

- Step 1: Log in to your MikroTik server with admin privileges
- Step 2: Click on IP from the left panel
- Step 3: In the submenu that opens, click on Firewall
- Step 4: Go to the NAT tab in the Firewall window
- Step 5: Click the + button to create a new rule. Note that in this scenario, the router is assumed to be connected to the IP (10.10.10.10), and we want to forward all requests from (10.10.10.10:5847) to (20.20.20.20:4324).
- Step 6: Click the General tab and select distant from the drop-down list
- Step 7: In the Dst field. Address: Type the IP from which you want to forward all requests
- Step 8: From the protocol list, select the connection protocol, such as TCP
- Step 9: In the Dst field. Port Type the port you want to forward requests to
- Step 10: Now, go to the Action tab
- Step 11: From the Action drop-down list, select DST-nat
- Step 12: In the To Addresses field, type the desired IP to send all requests to
- Step 13: In the To Ports field, type the port code to which you want to forward requests.
- Step 14: Click Apply and then OK to save and add the new rule

This way, you have successfully configured your first port forwarding rule in MikroTik. To add new port forwarding rules, follow the same steps for new ports or IPs.

## Source NAT training in Mikrotik

Suppose you want to hide your local devices behind the public IP address received from your Internet Service Provider (ISP). In that case, you must configure your MikroTik router’s source network address translation or masquerading feature. Suppose you want to hide the office computer and server behind the public IP 172.16.16.1. The rule required for this work in the operating system will be as follows:

By setting this rule, your ISP will see all requests sent to the 172.16.16.1 IP address, not your LAN IP addresses.

MikroTik routers and switches are affordable and reliable products. Due to their low cost, they are very suitable for small and medium-sized businesses. In some cases, even large companies use these products because they offer many features at a very low cost. However, they are unsuitable for supporting large volumes of data traffic like the main tier of ISPs. If you are interested in learning about the Mikrotik routing system, the following course is the best option:

Managers of one of the companies that have used MikroTik routers and switches in their development process, especially the CCR-1036 and CCR-1072 models, said about their experience: “What we like is the ease of use and how they work. It is data processing. Likewise, we have deployed RB devices as the customer’s CPE, and the results have been satisfactory.” Let’s review some features of Mikrotik routers:

- Support for Destination NAT to access servers inside the network from outside
- Support MPLS, BGP, and OSPF routing protocols
- The possibility of setting up a hotspot
- The possibility of network accounting
- An IP address supports version 6
- High boot speed on the router
- Support for the routing system based on PBR or Policy-Based Routing
- The possibility of remote access to a remote network through various VPN protocols
- VRF support
- Support for multiple virtual routers
- Ability to perform network traffic control operations
- Apply a speed limit for users
- The possibility of connecting to several Internet service providers
- The possibility of distributing users’ Internet traffic over several Internet links
- Ability to set up DHCP on the network to configure network clients
- The possibility of implementing quality of service on network packets
- The ability to use the MAC address for the initial configuration of the device without the need for an IP address at first

### Mikrotik router category and some of its most commonly used types

Mikrotik routers can be divided into four basic groups. This classification is based on several factors, including the connection technologies, the number of ports, the size, and the field of work of the devices. The four groups of these routers are:

- 3DIGIT routers
- 4DIGIT routers
- Naming routers
- Cloud Core routers

### Some of the famous models of these routers are

- RB133C: Simple Mikrotik router for subscriber-side wireless service
- RB450: from the Router Banel series with 300MHz processor, 32MB memory, and 5 Ethernet ports
- RB411: This router is used in links with medium bandwidth and high distances
- RB493: suitable for routing and wireless servicing of offices and centers connected to multiple Sowers
- RB433: suitable for wireless links with high bandwidth and long distances
- 433AH: Very powerful router with many features
- RB600A: It has high processing power and is similar to the 433AH router
- RB1000: It has a powerful 13333MHZ processor and 512MB memory

To fully familiarize yourself with Mikrotik company’s routers and their application and features, we suggest that all those interested participate in this course:

Learning and teaching destination NAT in Mikrotik: A factor for the development of companies

Mikrotik is one of the top companies producing network equipment and software required for it. Among its most important products are Mikrotik routers, as well as the company’s proprietary operating system and hardware called RouterOS. One useful command in this operating system is the destination NAT, which works based on the network address translation method.

This command and other functional commands in this operating system allow you to increase the efficiency of your equipment based on individual or corporate needs. Therefore, we suggest learning the skills of working with the Mikrotik operating system and those related to network security to all people who intend to learn the optimal and practical management of the network, server, and related equipment, or to teach others how to work with them.

## FAQ

### What is Destination NAT in MikroTik?

Destination NAT (dst-nat) changes the destination IP and/or port of incoming packets to forward them from a public address to an internal private address.

### Why use Destination NAT on a MikroTik router?

It enables access from outside your network—such as the internet—to servers or services inside your LAN by translating external requests to internal hosts and ports.

### How do you configure a simple DST-NAT rule?

In RouterOS, create a firewall NAT rule on the dstnat chain specifying the external destination IP/port and set action=dst-nat with to-addresses and optional to-ports for the internal service.
