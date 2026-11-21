---
id: collect-261121-veille-ia/actu-business/configuring-cisco-dynamic-multipoint-vpn-dmvpn-hub-spokes-mgre-protection-and-routing-dmvpn-configur
title: "Configuring Cisco Dynamic Multipoint VPN (DMVPN) - Hub, Spokes , mGRE Protection and Routing - DMVPN Configuration"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["dmvpn", "tunnel", "configuration", "spoke", "routing", "mgre", "nhrp", "spokes"]
source: https://www.firewall.cx/cisco/cisco-routers/cisco-router-dmvpn-configuration.html
source_anchor: ""
source_lines: [1, 337]
section: "Actu_Business"
---
Our DMVPN Introduction article covered the DMVPN concept and deployment designs. We explained how DMVPN combines a number of technologies that give it its flexibility, low administrative overhead and ease of configuration. This article will cover the configuration of a Cisco DMVPN including Hub, Spokes, Routing and Protecting the mGRE Tunnel.

It is highly advisable for those who haven’t read our Introduction to DMVPN to do so as it contains basic concepts and theory that are important to the configuration process.

Configuring DMVPN is simple, if you’ve worked with GRE tunnels before. If the GRE Tunnel concept is new to you, we would recommend reading through our Point-to-Point GRE IPSec Tunnel Configuration article before proceeding with DMVPN configuration.

DMVPN as a design concept is essentially the configuration combination of protected GRE Tunnel and Next Hop Routing Protocol (NHRP).

Before diving into the configuration of our routers, we’ll briefly explain how the DMVPN is expected to work. This will help in understanding how DMVPN operates in a network:

Each spoke has a permanent IPSec tunnel to the hub but not to the other spokes within the network.

Each spoke registers as a client of the NHRP server. The Hub router undertakes the role of the NHRP server.

When a spoke needs to send a packet to a destination (private) subnet on another spoke, it queries the NHRP server for the real (outside) address of the destination (target) spoke.

After the originating spoke learns the peer address of the target spoke, it can initiate a dynamic IPSec tunnel to the target spoke.

The spoke-to-spoke tunnel is built over the multipoint GRE (mGRE) interface.

The spoke-to-spoke links are established on demand whenever there is traffic between the spokes. Thereafter, packets are able to bypass the hub and use the spoke-to-spoke tunnel.

All data traversing the GRE tunnel is encrypted using IPSecurity (optional)

Our DMVPN Network

The diagram below depicts our DMVPN example network. Our goal is to connect the two remote networks (Remote 1 & 2) with the company headquarters. The headquarters router R1 is the central Hub router that will hold the NHRP database containing all spoke routers, their public IP addresses and LAN networks.

Four Steps To Fully Configure Cisco DMVPN

To help simplify the configuration of DMVPN we’ve split the process into 4 easy-to-follow steps. Each step is required to be completed before moving to the next one. These steps are:

Configure the DMVPN Hub

Configure the DMVPN Spoke(s)

Protect the mGRE tunnels with IPSecurity (optional)

Configure Routing Between DMVPN mGRE Tunnels (static routing or routing protocol)

Configuring The DMVPN Hub – R1 Router

Configuring the Hub router (R1) is simple. After configuring the router’s LAN and WAN interfaces we create our mGRE tunnel interface. Let's start with the router’s Ethernet interfaces:

interface FastEthernet0/0

description LAN-Network

ip address 192.168.1.1 255.255.255.0

duplex auto

speed auto

!

interface FastEthernet0/1

description WAN-Network

ip address 1.1.1.10 255.255.255.0

duplex auto

speed auto

Next, we configure the Tunnel0 interface. Notice this is an almost typical tunnel interface configuration with some minor but important changes that have been highlighted:

interface Tunnel0

description mGRE - DMVPN Tunnel

ip address 172.16.0.1 255.255.255.0

no ip redirects

ip nhrp authentication firewall

ip nhrp map multicast dynamic

ip nhrp network-id 1

tunnel source 1.1.1.10

tunnel mode gre multipoint

Engineers familiar with GRE Tunnels will immediately notice the absence of the tunnel destination command. It has been replaced with the tunnel mode gre multipointcommand, which designates this tunnel as a multipoint GRE tunnel.

The ip nhrp map multicast dynamic command enables the forwarding of multicast traffic across the tunnel to dynamic spokes. This is usually required by routing protocols such as OSPF and EIGRP. In most cases, DMVPN is accompanied by a routing protocol to send and receive dynamic updates about the private networks.

The ip nhrp network-id 1 command is used to identify this DMVPN cloud. All routers participating in this DMVPN cloud must have the same network-id configured in order for tunnels to form between them.

The ip nhrp authentication command is used to allow the authenticated updates and queries to the NHRP Database, ensuring unwanted queries are not provided with any information about the DMVPN network.

Configuring The DMVPN Spokes – R2 & R3 Routers

Spoke router configuration is similar to that of the hub. First configure the LAN and WAN interfaces:

interface FastEthernet0/0

description LAN-Network

ip address 192.168.2.1 255.255.255.0

duplex auto

speed auto

!

interface FastEthernet0/1

description WAN-Network

ip address 2.2.2.10 255.255.255.0

duplex auto

speed auto

Next, it’s time to build that mGRE tunnel:

interface Tunnel0

description R2 mGRE - DMVPN Tunnel

ip address 172.16.0.2 255.255.255.0

no ip redirects

ip nhrp authentication firewall

ip nhrp map multicast dynamic

ip nhrp map 172.16.0.1 1.1.1.10

ip nhrp map multicast 1.1.1.10

ip nhrp network-id 1

ip nhrp nhs 172.16.0.1

tunnel source FastEthernet0/1

tunnel mode gre multipoint

After a couple of seconds, we receive confirmation that our tunnel interface is up:

*Sep 9 21:27:29.774: %LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

The ip nhrp nhs 172.16.0.1 command tells our spoke router who the Next Hop Server (NHS) is, while the ip nhrp map 172.16.0.1 1.1.1.10 command maps the NHS address (172.16.0.1) to the Hub’s (R1) public IP address (1.1.1.10).

The ip nhrp map multicast 1.1.1.10 ensures multicast traffic is sent only from spokes to the hub and not from spoke to spoke. All multicast traffic should be received by the hub, processed and then updates are sent out to the spokes.

Lastly, notice that tunnel source FastEthernet0/1 command. All spokes with dynamic WAN IP address must be configured to bind the physical WAN interface as the tunnel source. This way, when the spoke’s WAN IP changes, it will be able to update the NHS server with its new WAN IP address.

Note: In R2’s configuration, we’ve configured a static IP address on its WAN interface FastEthernet0/1, but for the sake of this example, let us assume it was dynamically provided by the ISP.

R3’s configuration follows, similar to that of the R2 spoke router:

interface FastEthernet0/0

description LAN-Network

ip address 192.168.3.1 255.255.255.0

duplex auto

speed auto

!

interface FastEthernet0/1

description WAN-Network

ip address 3.3.3.10 255.255.255.0

duplex auto

speed auto

Next, our tunnel configuration:

interface Tunnel0

description R3 mGRE - DMVPN Tunnel

ip address 172.16.0.3 255.255.255.0

no ip redirects

ip nhrp authentication firewall

ip nhrp map multicast dynamic

ip nhrp map 172.16.0.1 1.1.1.10

ip nhrp map multicast 1.1.1.10

ip nhrp network-id 1

ip nhrp nhs 172.16.0.1

tunnel source FastEthernet0/1

tunnel mode gre multipoint

Note: In R3’s configuration, we’ve configured a static IP address on its WAN interface FastEthernet0/1, but for the sake of this example, let us assume it was dynamically provided by the ISP.

This completes the DMVPN configuration on our central hub and two spoke routers. It is now time to verify the DMVPNs are working correctly.

Verifying DMVPN Functionality At The R1 HUB Router

After completing our routers configuration, it’s time to verify everything is working as planned.

First we turn to our main hub, R1, and check the DMVPN by using the show dmvpn command:

R1# show dmvpn Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete N - NATed, L - Local, X - No Socket # Ent --> Number of NHRP entries with same NBMA peer NHS Status: E --> Expecting Replies, R --> Responding UpDn Time --> Up or Down Time for a Tunnel ==========================================================================

# Ent Peer NBMA Addr Peer Tunnel Add State UpDn Tm Attrb ----- ------------- --------------- ----- ------- ----- 1 2.2.2.10 172.16.0.2 UP 00:04:58 D 1 3.3.3.10 172.16.0.3 UP 00:04:12 D

The output of our command provides us with some valuable information. To start with, the router provides an explanation for each column presented (right under the show command) but we are still going to cover them so that we are not left with any unanswered questions.

The first column #Ent shows the number of entries that exist in the NHRP Database for the same spoke. Usually, we wouldn’t expect to see more than one for each spoke.

The second column Peer NBMA Addr presents the spoke’s public IP address, while the third column, Peer Tunnel Add, shows each spoke’s local Tunnel’s IP address.

Next, the State column shows the current state the tunnel is in. In our case, both tunnels are UP. Right next to the State is the UpDN Tm, which is the Up or Down Time of the current State. This is a very important bit of information as you can clearly see out how long your tunnel has been in its current state.

For our example, both spokes have been up for almost 5 minutes.

Lastly, the Attrib column shows the type of tunnels established by the spokes. D stands for Dynamic, S for Static and I for Incomplete. Usually dynamic spokes will create D type tunnels. Tunnels established from the spokes to the Hub router are expected to be S type, since the Hub remains static.

Verifying DMVPN Functionality At The R2 & R3 Spoke Router

Turning to R2 router, our first spoke, we can repeat the same show dmvpn command and obtain a list of dmvpns currently created:

R2# show dmvpn Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete N - NATed, L - Local, X - No Socket # Ent --> Number of NHRP entries with same NBMA peer NHS Status: E --> Expecting Replies, R --> Responding UpDn Time --> Up or Down Time for a Tunnel ==========================================================================

# Ent Peer NBMA Addr Peer Tunnel Add State UpDn Tm Attrb ----- ------------- -------------- ----- ------- ----- 1 1.1.1.10 172.16.0.1 UP 00:06:35 S

As expected, R2’s output shows one entry only. When traffic needs to be directed to R3, a second GRE tunnel will come up. We’ll try this soon. For now let’s check our third remote site, R3 spoke router

Using the same show dmvpn command we obtain the following similar output:

R3# show dmvpn Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete N - NATed, L - Local, X - No Socket # Ent --> Number of NHRP entries with same NBMA peer NHS Status: E --> Expecting Replies, R --> Responding UpDn Time --> Up or Down Time for a Tunnel ==========================================================================

# Ent Peer NBMA Addr Peer Tunnel Add State UpDn Tm Attrb ----- --------------- --------------- ----- -------- ----- 1 1.1.1.10 172.16.0.1 UP 00:06:55 S

Protecting - Encrypting DMVPN mGRE Tunnels With IPSec

Since we have our GRE tunnels up and running, we need to encrypt them using IPSec to ensure data confidentiality. Protecting GRE Tunnels is covered in great depth in our Protected GRE over IPSec article, so we are going to simply display the commands here without repeating the topic.

Notice the command crypto isakmp key firewall.cx address 0.0.0.0 0.0.0.0. The peer address for which the isakmp key is valid is 0.0.0.0 0.0.0.0, which means every possible host on the Internet. When our remote routers (spokes) have dynamic IP addresses, 0.0.0.0 0.0.0.0 must be used.

The following configuration applies to R2 & R3 spoke routers:

Again we’ve defined 0.0.0.0 0.0.0.0 as the isakmp peer address. While the hub’s public IP address is known we must keep in mind that R2 and R3 can build dynamic VPN tunnel between them. Taking into consideration that their public IP address is dynamic it is imperative to use 0.0.0.0 0.0.0.0 for the remote peer.

Verifying the DMVPN Crypto Tunnels

Once all routers are configured IPSec VPN tunnels are brought up. We can verify this by using the show crypto session command at our R1 hub router:

R1# show crypto session

Crypto session current status

Interface: Tunnel0

Session status: UP-ACTIVE

Peer: 2.2.2.10 port 500

IKE SA: local 1.1.1.10/500 remote 2.2.2.10/500 Active

IPSEC FLOW: permit 47 host 1.1.1.10 host 2.2.2.10

Active SAs: 2, origin: crypto map

Interface: Tunnel0

Session status: UP-ACTIVE

Peer: 3.3.3.10 port 500

IKE SA: local 1.1.1.10/500 remote 3.3.3.10/500 Active

IPSEC FLOW: permit 47 host 1.1.1.10 host 3.3.3.10

Active SAs: 2, origin: crypto map

Routing Between DMVPN mGRE Tunnels

Last step involves enabling routing in our DMVPN network. This is required so that the hub and spoke routers are aware which packets need to be sent via the VPN network.

There are two ways this can be achieved: 1) Static routes 2) Routing protocol.

For the sake of simplicity we are going to focus on static routes. DMVPN and routing protocol configuration will be covered in another article.

Configuring the necessary static routes is very simple. All that is required is a set of simply static routes on each router (hub and spoke), pointing to the other networks.

On the R1 hub router:

ip route 192.168.2.0 255.255.255.0 172.16.0.2 ip route 192.168.3.0 255.255.255.0 172.16.0.3

On R2 spoke router:

ip route 192.168.1.0 255.255.255.0 172.16.0.1 ip route 192.168.3.0 255.255.255.0 172.16.0.3

And finally on R3 spoke router:

ip route 192.168.1.0 255.255.255.0 172.16.0.1 ip route 192.168.2.0 255.255.255.0 172.16.0.2

Our DMVPN Network Is Ready!

At this point, our DMVPN network is ready and fully functional. All networks are connected between each other and dynamic VPN tunnels between spokes can be established. GRE tunnels are protected properly, providing data confidentiality and ip routing is enabled.

As a final step, we can try sending traffic between the spokes and verify the dynamic tunnel is being established:

From R2 spoke router, we try to ping R3’s LAN IP address:

R2# ping 192.168.3.1

Type escape sequence to abort. Sending 5, 100-byte ICMP Echos to 192.168.3.1, timeout is 2 seconds: .!!!! Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/4 ms

It is evident that the two spoke routers have established communication.

The DMVPN is up and routing is working perfectly:

R2# show dmvpn Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete N - NATed, L - Local, X - No Socket # Ent --> Number of NHRP entries with same NBMA peer NHS Status: E --> Expecting Replies, R --> Responding UpDn Time --> Up or Down Time for a Tunnel ==========================================================================

# Ent Peer NBMA Addr Peer Tunnel Add State UpDn Tm Attrb ----- ------------------ --------------- ----- ------- ----- 1 1.1.1.10 172.16.0.1 UP 00:39:05 S 1 3.3.3.10 172.16.0.3 UP 00:00:08 D

This concludes our DMVPN configuration article.

This article showed how to configure a DMVPN network between Cisco routers. We covered the configuration of a Cisco DMVPN including Hub, Spokes, Static Routing and Protecting the mGRE Tunnel. We also provided some useful show commands to help troubleshoot and debug the DMVPN network. More articles on VPN & DMVPN can be found in our Cisco Routers Section and Cisco Services & Technlogies Section.
