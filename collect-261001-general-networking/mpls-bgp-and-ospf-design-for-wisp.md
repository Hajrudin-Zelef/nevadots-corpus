I am working on a new design for our WISP. We currently have around 800 customers and around 50 towers. We are getting ready to order 2 Mikrotik CCR1072’s for our core routers and upgrade from a bridged network to MPLS.

Our needs are as follows

Clients get a static NATed IPv4 address or static Public

IPv6 support

Ability to create tunnels for clients with multiple sites

Redundant connections for backhauls at different points of entry

Redundant upstream providers at different locations. Our main 10G connections comes from 1 locations while we have a couple 200M connections at different locations. redundancy will need to be set on a site (tower) level.

I would like to run iBGP for our public/nated ip addresses on the internal side and we will be using eBGP on the upstream side. We have plans to get 2 10G connections dropped to our cabinet and use eBGP to create failover between the CCR1072’s.

I have created a test lab using 4 Mikrotik routers joined as a ring using MPLS and OSPF. I configured a unique loopback address per router and /30s between the routers. Failover when breaking on of the links takes anywhere from 1 second to 30 seconds. Currently iBGP is not working in the lab, each peer is stuck at open sent. I am not sure where I went wrong on the setup. Any help or suggestions would be welcomed.


```
Core Router
/interface bridge
add name=Loopback
/interface ethernet
set [ find default-name=combo1 ] mtu=1526
set [ find default-name=ether1 ] l2mtu=1800 mtu=1800
set [ find default-name=ether2 ] l2mtu=1800 mtu=1800
/interface vlan
add interface=combo1 mtu=1526 name=vlan205 vlan-id=205
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=100 router-id=10.255.0.1
/routing ospf instance
set [ find default=yes ] distribute-default=always-as-type-1 mpls-te-area=backbone mpls-te-router-id=Loopback router-id=10.255.0.1
/ip neighbor discovery-settings
set discover-interface-list=!dynamic
/ip address
add address=192.168.33.1/30 interface=ether1 network=192.168.33.0
add address=192.168.33.14/30 interface=ether2 network=192.168.33.12
add address=10.255.0.1 interface=Loopback network=10.255.0.1
add address=192.168.205.200/24 interface=vlan205 network=192.168.205.0
/ip route
add distance=1 gateway=192.168.205.1
/mpls ldp
set enabled=yes lsr-id=10.255.0.1 transport-address=10.255.0.1
/mpls ldp interface
add interface=ether1
add interface=ether2
/routing bgp peer
add name=Core-R2 remote-address=10.255.0.2 remote-as=100 ttl=default
add name=Core-R3 remote-address=10.255.0.3 remote-as=100 ttl=default
add name=Core-R4 remote-address=10.255.0.4 remote-as=100 ttl=default
/routing ospf interface
add interface=ether1 network-type=broadcast use-bfd=yes
add interface=ether2 network-type=broadcast use-bfd=yes
/routing ospf network
add area=backbone network=192.168.33.0/24
add area=backbone network=10.255.0.1/32
/system identity
set name=Core
Router 2
/interface bridge
add name=Loopback
/interface ethernet
set [ find default-name=ether1 ] l2mtu=1800 mtu=1800
set [ find default-name=ether2 ] l2mtu=1800 mtu=1800
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=100 router-id=10.255.0.2
/routing ospf instance
set [ find default=yes ] mpls-te-area=backbone mpls-te-router-id=Loopback router-id=10.255.0.2
/ip neighbor discovery-settings
set discover-interface-list=!dynamic
/ip address
add address=192.168.33.2/30 interface=ether1 network=192.168.33.0
add address=192.168.33.5/30 interface=ether2 network=192.168.33.4
add address=10.255.0.2 interface=Loopback network=10.255.0.2
/mpls ldp
set enabled=yes lsr-id=10.255.0.2 transport-address=10.255.0.2
/mpls ldp interface
add interface=ether1
add interface=ether2
/routing bgp peer
add name=R2-Core remote-address=10.255.0.1 remote-as=100 ttl=default
add name=R2-R3 remote-address=10.255.0.3 remote-as=100 ttl=default
add name=R2-R4 remote-address=10.255.0.4 remote-as=100 ttl=default
/routing ospf interface
add interface=ether1 network-type=broadcast use-bfd=yes
add interface=ether2 network-type=broadcast use-bfd=yes
/routing ospf network
add area=backbone network=192.168.33.0/24
add area=backbone network=10.255.0.2/32
/system identity
set name=R2
Router 3
/interface bridge
add name=Loopback
/interface ethernet
set [ find default-name=ether1 ] l2mtu=1800 mtu=1800
set [ find default-name=ether2 ] l2mtu=1800 mtu=1800
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=100 router-id=10.255.0.3
/routing ospf instance
set [ find default=yes ] mpls-te-area=backbone mpls-te-router-id=Loopback redistribute-connected=as-type-1 router-id=10.255.0.3
/ip neighbor discovery-settings
set discover-interface-list=!dynamic
/ip address
add address=192.168.33.6/30 interface=ether1 network=192.168.33.4
add address=192.168.33.9/30 interface=ether2 network=192.168.33.8
add address=10.255.0.3 interface=Loopback network=10.255.0.3
add address=192.168.10.1/24 interface=ether3 network=192.168.10.0
/mpls ldp
set enabled=yes lsr-id=10.255.0.3 transport-address=10.255.0.3
/mpls ldp interface
add interface=ether1
add interface=ether2
/routing bgp network
add network=192.168.10.0/24
/routing bgp peer
add name=R3-Core remote-address=10.255.0.1 remote-as=100 ttl=default
add name=R3-R2 remote-address=10.255.0.2 remote-as=100 ttl=default
add name=R3-R4 remote-address=10.255.0.4 remote-as=100 ttl=default
/routing ospf interface
add interface=ether1 network-type=broadcast use-bfd=yes
add interface=ether2 network-type=broadcast use-bfd=yes
/routing ospf network
add area=backbone network=192.168.33.0/24
add area=backbone network=10.255.0.3/32
/system identity
set name=R3
Router 4
/interface bridge
add name=Loopback
/interface ethernet
set [ find default-name=ether1 ] l2mtu=1800 mtu=1800
set [ find default-name=ether2 ] l2mtu=1800 mtu=1800
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default as=100 router-id=10.255.0.4
/routing ospf instance
set [ find default=yes ] mpls-te-area=backbone mpls-te-router-id=Loopback redistribute-connected=as-type-1 router-id=10.255.0.4
/ip neighbor discovery-settings
set discover-interface-list=!dynamic
/ip address
add address=192.168.33.10/30 interface=ether1 network=192.168.33.8
add address=192.168.33.13/30 interface=ether2 network=192.168.33.12
add address=10.255.0.4 interface=Loopback network=10.255.0.4
/mpls ldp
set enabled=yes lsr-id=10.255.0.4 transport-address=10.255.0.4
/mpls ldp interface
add interface=ether1
add interface=ether2
/routing bgp peer
add name=R4-Core remote-address=10.255.0.1 remote-as=100 ttl=default
add name=R4-R2 remote-address=10.255.0.2 remote-as=100 ttl=default
add name=R4-R3 remote-address=10.255.0.3 remote-as=100 ttl=default
/routing ospf interface
add interface=ether1 network-type=broadcast use-bfd=yes
add interface=ether2 network-type=broadcast use-bfd=yes
/routing ospf network
add area=backbone network=192.168.33.0/24
add area=backbone network=10.255.0.4/32
/system identity
set name=R4
```

             
            
           
          
            
            
              Your BGP peer remote-addresses are wrong.  These should be the peer’s address on the /30 links.

             
            
           
          
            
            
              Here are a couple of presentations I’ve done at different MUMs that may help you out with design for this type of network. Hope this helps!

**BGP as an IGP for Carrier/Enterprise Networks**

https://mum.mikrotik.com//presentations/US13/kevin.pdf

**ISP Architecture – MPLS Overview, Design and Implementation for WISPs.**

https://mum.mikrotik.com//presentations/US16/presentation_3327_1462279781.pdf

Also…you need to add a source interface to the BGP peering when using Loopbacks and it looks like you don’t have that set…otherwise, it will inherit the transit subnet address as the source for the peering.

             
            
           
          
            
            
              
Not true…in an iBGP design you want the peering address to use loopbacks that are advertised by OSPF so that if an interface goes down and another path is available, the peering will stay online.

             
            
           
          
            
            
              I set the source interface to the loopback and that fixed the peering issue. Setting syncronize=no fixed the router advertisement. I went through the second presentation and will watch the first one tomorrow. Thanks for the help.

             
            
           
          
            
            
              Hi Kevin.

Can you explain us more about, you know i’ve watched your presentation about

“Using eBGP and OSPF transit fabric for traffic engineering”.

Nice one but would you like to point us about BGP Communities,and how can we build up this kind of configuration, any example or maybe part of that configuration will be appreciated

Thanks

             
            
           
          
            
            
              BGP communities allow you to change the route processing by tagging prefixes that get flooded throughout your BGP network. What you want to use them for is up to you, but I use them for setting local pref and MED values.

For example, I set a particular community for a customer route depending on which gateway I want it to use. Both gateways are configured to look for the communities and set the MED to the core network accordingly. By simply changing the BGP community, I can dictate the path that traffic takes back from the core, without having to update routing filters everywhere.  Within the core, I set different community values based on whether the source of the update is from transit or peering. Then the core BGP routers can set the local pref based on this community string.

There are also well known communities such as no-export, which stops update outside of the local AS, and prevents you from leaking routes outside your network.

             
            
           
          
            
            
              if i have OSPF cost on one way and BGP communities on other way, which way will prefer?

and also thanks for explanation but do you mind to show us how do you configure?

Thanks

             
            
           
          
            
            
              OSPF is an IGP, BGP is an EGP, they have different use cases.

By default, eBGP > OSPF > iBGP in terms of administrative distance, but BGP communities are not attributes that directly affect the best path algorithm. Communities are used simply to tag a prefix that another peer can check to see if it needs to do any special processing of that prefix. For example, most Tier 1 ISPs have a list of communities that you can set to tell them what you want to do with those prefixes. This one is from NTT https://us.ntt.net/support/policy/routing.cfm#communities

Maybe you are dual-homed to a provider and want to tell them which link to prefer. By using the NTT example, you would configure community 2914:490 on your primary and 2914:480 on your backup link. When the NTT routers receive these, they then set the local pref of those prefixes accordingly.

```
/routing filter
add chain=NTT-primary-out action=accept set-bgp-communities=2914:490
add chain=NTT-backup-out action=accept set-bgp-communities=2914:480
```

The NTT side might be

```
/routing filter
add chain=BGP-in bgp-communities=2914:490 action=accept set-bgp-local-pref=120
add chain=BGP-in bgp-communities=2914:480 action=accept set-bgp-local-pref=110
```

             
            
           
          
            
            
              i true it doesn’t go i do not understand two thing.

1. 
Why have you done on each side diferent AS?
2. 
see the picture:

 
            
           
          
            
            
              The goal of this design is to use OSPF only for EQMP load balancing between the sites, but BGP as the overall routing protocol.

EBGP allows routing policy to be modified and advertised at each tower site. iBGP basically considers the entire AS with a more or less single unified routing policy for egress, but internally it will use the IGP for final routing decisions.

             
            
           
          
            
            
              i have a question as we are trying to do the same with mpls and vpls. if you are using vpls tunnels back to your core everywhere, how is bgp doing anything with your traffic ip addresses as everything is supposed to be traveling over the tunnel. I also noticed there were no traffic networks entered for bgp. Are they supposed to be and do the get entered on overy router in your network.

             
            
           
          
            
            
              This depends on your use case.

Some ISPs may use LDP signalled VPLS for private transport circuits. Other ISPs may use BGP signalled VPLS.

In most cases, it’s helpful to have iBGP to advertise public subjects and /32 loopbacks even if the majority of traffic is in VPLS.

In short, having BGP on the interior of the network provides deployment flexiblity.

Add to that MikroTik’s issues with large OSPF routing tables and BGP becomes a solid choice for internal routing while relying on OSPF for reachability.

             
            
           
          
            
            
              I can get everything connected but not carrying traffic properly.  Would you by chance have an example config of say a tower router set up with a couple of AP’s and backhaul, and one of your core router that you are routing it to using private subnets that we could see as an example to compare and learn from.  We can find bits an pieces out there on different parts but not a complete working config of both ends of a real world use case to see.  I think it would be most beneficial to a lot of WISPs as we hear more and more about moving to an MPLS/VPLS/BGP/OSPF network.

             
            
           
          
            
            
              Here’s an example of OSPF/MPLS/VPLS for a WISP with HA DCs and with configs…I’ll see what I can dig up for BGP 

https://www.stubarea51.net/2018/04/23/wisp-design-building-highly-available-vpls-for-public-subnets/

             
            
           
          
            
            
              Thanks.

On the MTU size, I see some people set it to 1530 for MPLS, some 1580, 1600, and 2000.  Is there any downside to setting it to 2000 across the board?

Also I am having issues getting the MPLS working out in the field through the various wireless links even though on my lab it works fine.  I have all the MTU on all equipment between both ends set to 2000.  If I try to ping anything larger than 1500 with do-not-fragment including the router I’m working on it fails.  So why can’t I ping larger than 1500 to all the equipment in between to find if there is an MTU issue somewhere along the path.

             
            
           
          
            
            
              
Hi,

Regarding the MTU setting, some devices or ISPs may have stricter limits on MTU, and it is best to use the same MPLS MTU on all devices if possible. We went with 1550 because it allows us several nested VLAN tags and also allows the customer several nested VLAN tags within their VPLS tunnel, while allowing us also to use RFC4638 with PPPoE.

Before we enable MPLS at a site, we set up a test VPLS tunnel across the link with temporary IP addresses on both ends (in the same /30 or /24), and we try pinging with size 1500 do-not-fragment from one address to the other. If we want to provide more than 1500 MTU (ex. 1508 for RFC 4638 PPPoE) then we temporarily change the IP MTU on the VPLS tunnel to 1508, and test the ping with 1508. That way, we can verify that all of the devices in between are properly passing the jumbo frames before we put customers on the service.

             
            
           
          
            
            
              
This will help you some on MTU sizing.

https://mum.mikrotik.com//presentations/US16/presentation_3327_1462279781.pdf

             
            
           
          
            
            
              How much horsepower does it take to run MPLS/VPLS.  We set up a test bed of 4 CRS112-BP-4S-IN switches which is what we might normally use at a smaller micro pop site where we have small 14-16" boxes and just need POE to the radio and UPS and they failed terribly.  They could only do 50-60 Mbps before the CPU got maxed out which is terrible when they can do handle a ton more otherwise in a normal configuration.  At normal tower sites where we have rack space we typically run CCR routers.  But even with these as I don’t have a test bed I would be very concerned of the ability to handle it, even more so at your core where you might have 40-100 VPLS tunnels terminating on one CCR

             
            
           
          
            
            
              You definitely don’t want to do it with a small CRS. Look at using a 3011 at smaller sites and CCR at others.