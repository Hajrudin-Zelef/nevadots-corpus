---
id: collect-261121-veille-ia/reseau-networking/arborescence-des-pages-31
title: "Arborescence des pages"
domain: reseau-networking
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["routing", "protocol", "network", "exchange", "information", "system", "route", "connection"]
source: https://help.mikrotik.com/docs/spaces/ROS/pages/328220/BGP
source_anchor: ""
source_lines: [1, 259]
section: "Gemini_Google"
---
# Summary

The Border Gateway Protocol (BGP) allows setting up an inter-domain dynamic routing system that automatically updates routing tables of devices running BGP in case of network topology changes.

BGP is an inter-autonomous system routing protocol based on the distance-vector algorithm. It is used to exchange routing information across the Internet and is the only protocol that is designed to deal with a network of the Internet's size and the only protocol that can deal well with having multiple connections to unrelated routing domains.

BGP is designed to allow for sophisticated administrative routing policies to be implemented. It does not exchange information about network topology but rather reachability information. As such, BGP is better suited to inter-AS environments and special cases like informational feeds. If you just need to enable dynamic routing in your network, consider OSPF instead.

The feature is not supported on SMIPS devices (hAP lite, hAP lite TC, and hAP mini).


Standards and Technologies:

- RFC 4271 Border Gateway Protocol 4
- RFC 4456 BGP Route Reflection
- RFC 5065 Autonomous System Confederations for BGP
- RFC 1997 BGP Communities Attribute
- RFC 8092 BGP Large Communities
- RFC 4360, 5668 BGP Extended Communities
- RFC 2385 TCP MD5 Authentication for BGPv4
- RFC 5492 Capabilities Advertisement with BGP-4
- RFC 2918 Route Refresh Capability
- RFC 4760 Multiprotocol Extensions for BGP-4
- RFC 2545 Use of BGP-4 Multiprotocol Extensions for IPv6 Inter-Domain Routing
- RFC 4893 BGP Support for Four-octet AS Number Space
- RFC 4364 BGP/MPLS IP Virtual Private Networks (VPNs)
- RFC 4761 Virtual Private LAN Service (VPLS) Using BGP for Auto-Discovery and Signalling
- RFC 6286 - AS-wide Unique BGP Identifier for BGP-4
- RFC 4273 - SNMP peer table monitoring (OID 1.3.6.1.2.1.15.3.1) (IPv4 only)
- RFC 6793 - 4-byte ASN support and Aggregator attribute.

# BGP Terminology

- AS - Autonomous System
- ASN - Autonomous System Number
- NLRI - Network Layer Reachability Information is what is being exchanged between BGP peers and represents how to reach the prefixes.
- IGP - Interior Gateway Protocol
- EGP - Exterior Gateway protocol
- RR - Route reflector is the router in the BGP network that reflects advertisements to all the neighbors, avoiding the requirement for full BGP mesh.
- Route server - is the BGP router that does not participate in traffic forwarding. Routes are typically not even installed in the FIB.
- loopback address - a /32 address configured on a dummy bridge interface, that can act as a loopback.

# BGP Basics

BGP routers exchange reachability information by means of a transport protocol, which in the case of BGP is TCP (port 179). Upon forming a TCP connection these routers exchange **OPEN** messages to negotiate and confirm supported capabilities.

After agreeing on capabilities to use, the session is considered to be established and peers can start to exchange NLRIs via **UPDATE** messages. This information contains an indication of what sequence of full paths (BGP AS numbers) the route should take in order to reach the destination network (NLRI prefix).

The peers initially exchange their full routing tables and after the initial exchange, incremental updates are sent as the routing tables change. Thus, BGP does not require a periodic refresh of the entire BGP routing table.

BGP maintains the routing table version number which must be the same between any two given peers for the duration of the connection.

**KEEPALIVE** messages are sent periodically to ensure that the connection is up and running, if **KEEPALIVE** messages are not received within the **Hold Time** interval, the connection will be closed.

To respond to errors or special conditions, **NOTIFICATION** messages can be generated and sent to the remote peer, notification message type also indicates whether the connection should be immediately closed.

There can be two types of BGP connections:

- **iBGP** - is an "internal" link connecting peers from the same AS
- **eBGP** - is an "external" link connecting peers belonging to two different AS-es

A particular AS might have multiple BGP speakers and provide transit service to other AS-es. This implies that BGP speakers must maintain a consistent view of routing within the AS. A consistent view of the routes exterior to the AS is provided by having all BGP routers within the AS establish direct iBGP connections with each other (full mesh) or by utilizing a Router Reflector setup.

Using a set of administrative policies BGP speakers within the AS come to an agreement as to which entry/exit point to use for a particular destination. This information is communicated to the interior routers of the AS using the interior routing protocol (IGP), for example, OSPF, RIP, or static routing. In certain setups, iBGP can take the IGP protocol role as well.

For certain BGP attributes handling behavior may change depending on what type of connection is set up, for example, the LOCAL-PREF attribute is not advertised to eBGP peers.

RouterOS divides configuration and session monitoring into four menus:

- instance menu (`/routing/bgp/instance` )

- connection menu (`/routing/bgp/connection` )
- sessions menu(`/routing/bgp/session` )
- template menu (`/routing/bgp/template` )

## Instance Menu

Starting from ROSv7.20, instead of auto detecting instance based on router-ids, BGP routing instances are now explicitly defined in instance menu.

BGP routing instance is necessary for best path route selection and other instance dependant features like VPN, EVPN and so on.

## Connection Menu

Let's look at a very basic eBGP configuration example assuming, that Router1 IP is 192.168.1.1, AS 65531 and Router2 IP 192.168.1.2, AS 65532:

The BGP connection menu defines BGP outgoing connections as well as acts as a template matcher for incoming BGP connections.

 `local.role` parameter is used to indicate that this connection will be the eBGP. Also, notice that the connection does not require a remote AS number to be specified, RouterOS can determine a remote AS number dynamically from the first received **OPEN** message.

The parameter equivalent to other vendors and older RouterOS "update-source" is "`local.address`". In most cases, it can be left unconfigured, and let the router determine the address.

When a local address is not specified, BGP will try to guess the local address depending on the current setup:

- if the peer is iBGP
  - if loopback available 
    - pick the highest loopback address
  - if loopback is not available
    - pick any highest IP address on the router
- if loopback available 
- if the peer is eBGP
  - if a remote peer's IP is not from a directly connected network:
    - and multihop is not set, then throw an error
    - and multihop is enabled:
      - if loopback available
        - pick the highest loopback address
      - if loopback is not available
        - pick any highest IP address on the router
    - if loopback available
  - if a remote peer's IP is from a directly connected network:
    - and multihop is not set:
      - pick the local routers IP address from that connected network
    - and multihop is set:
      - if loopback available
        - pick the highest loopback address
      - if loopback is not available
        - pick any highest IP address on the router
    - if loopback available
  - and multihop is not set:
- if a remote peer's IP is not from a directly connected network:

In addition to connection-specific parameters, template-specific parameters are also directly exposed in this menu, for easier configuration in simple scenarios (when templates are not necessary).

Listening on subnets should not be enabled in unsafe environments, denial of service is possible with such configuration. Firewall must be configured to protect the router.

See "listen" parameter for more details.

## Session Menu

To see the actual active sessions with selected template parameters and negotiated capabilities refer to the BGP sessions menu:

This menu shows read-only cached BGP session information. It will show the current status of the session, flags, last received notification, and negotiated session parameters.

Even if the BGP session is not active anymore, the cache can still be stored for some time. Routes received from a particular session are removed only if the cache expires, this allows mitigating extensive routing table recalculations if the BGP session is flapping.

## Template Menu

The template contains all BGP protocol-related configuration options. It can be used as a template for dynamic peers and to apply a similar configuration to a group of peers. Note that this is not the same as peer groups on Cisco devices, where the group is more than just a common configuration.

# Best-Path Selection

BGP routers can receive multiple copies of the global routing table from multiple providers.

There should be some way to compare those multiple BGP routing tables and select the best route to the destination, the solution is the BGP Best Path Selection Algorithm.

The route is evaluated by the algorithm only if it is valid. In general, the route is considered valid if:

- NEXT_HOP of the route is valid and reachable
- AS_PATH received from external peers does not contain the local AS
- the route is not rejected by routing filters

For more information read nexthop selection and validation.

The best path algorithm also compares routes received only by a **single BGP instance**. Routes installed by different BGP instances are compared by the general algorithm, i.e. route distances are compared and the route with a lower distance is preferred.

If all the criteria are met, then the following actions take place:

1. The first path received is automatically considered the 'best path'. Any further received paths are compared to the first received to determine if the new path is better.
2. Prefer the path with the highest **WEIGHT** .
This parameter is not a part of the BGP standard, it is invented to quickly locally select the best route. A parameter is local to the router (assigned with routing filters in the BGP input) and cannot be advertised. A route without assigned WEIGHT has a default value of 0.
3. Prefer the path with the highest **LOCAL_PREF** .
This attribute is used only within an AS. A path without the LOCAL_PREF attribute has a value of 100 by default.
4. Prefer the path with the shortest **AS_PATH** . (skipped if`input.ignore-as-path-len` set to**yes** ).
Each AS_SET counts as 1, regardless of the set size. The AS_CONFED_SEQUENCE and AS_CONFED_SET are not included in the AS_PATH length.
5. ~~Prefer the path that was locally originated via aggregate or BGP network~~
6. Prefer the path with the lowest **ORIGIN** type.Interior Gateway Protocol (IGP) is lower than Exterior Gateway Protocol (EGP), and EGP is lower than INCOMPLETE in other words**IGP < EGP < INCOMPLETE**
7. Prefer the path with the lowest **multi-exit discriminator (MED).**The router compares the MED attribute only for paths that have the same neighboring (leftmost) AS. 
Paths without explicit MED value are treated with MED of 0.
8. Prefer **eBGP** over**iBGP** paths
9. Prefer lowest **IGP metric** .
10. If `multipath` is enabled, break here and and consider paths equal. This will install BGP ECMP routes.
11. Prefer the route that comes from the BGP router with the lowest **router ID** . If a route carries the**ORIGINATOR_ID** attribute, then the**ORIGINATOR_ID** is used instead of the router ID.
12. Prefer the route with the shortest **route reflection cluster list** . Routes without a cluster list are considered to have a cluster list of length 0.
13. Prefer the path that comes from the lowest neighbor address

# Routing Filter Notes

On BGP output routing filters are executed before BGP itself is modifying attributes, for example, if `nexthop-choice` is set to `force-self`, then the gateway set in the routing filters will be overridden.

On BGP input routing filters are applied to the received attributes, which means that, for example, setting the gateway will work no matter what `nexhop-choice` value is set.

# Running More than One Instance

As we already know for best path selection to work properly, BGP routes must be received from the same instance. But in certain scenarios it is necessary to run multiple BGP instances with their own separate tables. 

BGP determines whether sessions belongs to the same instance by comparing configured local router IDs. 

For example config below will run each peer in its own BGP instance


When `router-id` is not specified BGP will pick the "default" ID from `/routing id`.

Starting from v7.20 instance is no longer determined by router-id.

# Route Distinguisher

Route Distinguisher is a 64-bit integer, which is divided into three parts:

- type (always 2 bytes),
- administrator subfield,
- value or service provider subfield.

Currently, there are three format types defined.

| **2bytes** | 2bytes | 2bytes | 2bytes | 
|---|---|---|---|
| **Type1** | ASN | 4byte value |  | 
| **Type2** | 4-byte IP |  | value | 
| Type3 | 4-byte ASN |  | value | 

# BGP Unnumbered

RouterOS is capable of establishing BGP session dynamically without global IPv6 address on an interface and without specifying remote link-local address (BGP Unnumbered connection). Mechanism relies on a IPv6 neighbor discovery (RFC 4861) to get neighbor's link-local address and RFC5549 to advertise IPv4 NLRIs with IPv6 nexthops.

Unnumbered connection is configured if `remote.address` is empty and `local.address` is an interface. In this mode only one connection per interface is accepted.

For router without configured global addresses to be able to send replies for RAs, following ND config is needed:

```
/ipv6/nd/prefix/add prefix=none interface=sfp-sfpplus1
```

 

```
[admin@CCR2004_2XS_111] /ipv6/neighbor> print 
Flags: D - DYNAMIC; R - ROUTER
Columns: ADDRESS, MAC-ADDRESS, INTERFACE, VRF
#    ADDRESS                    MAC-ADDRESS        INTERFACE     VRF 
0 DR fe80::de2c:6eff:fec5:a7ff  DC:2C:6E:C5:A7:FF  sfp-sfpplus1  main
```

 

Now we can add unnumbered connection config:

```
/routing bgp 
add instance=myInstance local.address=sfp-sfpplus1 .role=ibgp name=unnumbered_2
```

 

```
[admin@CCR2004_2XS_111] /routing/bgp/connection> print 
Flags: D - DYNAMIC, X - DISABLED, I - INACTIVE 
 0   name="unnumbered_2" instance=v6_test 
     local.address=sfp-sfpplus1 .default-address=fe80::de2c:6eff:fea4:b42f%sfp-sfpplus1 .role=ibgp 
     routing-table=main as=333 
[admin@CCR2004_2XS_111] /routing/bgp/session> print 
Flags: E - ESTABLISHED 
 0 E name="unnumbered_2-1" instance=v6_test 
     remote.address=fe80::de2c:6eff:fec5:a7ff%sfp-sfpplus1 .as=333 .id=203.0.113.2 .capabilities=mp,rr,enhe,gr,as4 .afi= 
     .messages=5181 .bytes=98439 .eor="" 
     local.role=ibgp .address=fe80::de2c:6eff:fea4:b42f%sfp-sfpplus1 .as=333 .id=203.0.113.1 .cluster-id=203.0.113.1 
     .capabilities=mp,rr,enhe,gr,as4 .afi= .messages=5181 .bytes=98439 .eor="" 
     output.procid=20 
     input.procid=20 ibgp 
     multihop=yes hold-time=3m keepalive-time=1m uptime=3d14h20m55s640ms last-started=2026-02-12 18:26:59 prefix-count=0 
```
