---
id: collect-261121-veille-ia/cisco/configuring-link-aggregation-with-etherchannel-ciscozine
title: "Configuring Link Aggregation with EtherChannel – CiscoZine"
domain: cisco
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["etherchannel", "pagp", "lacp", "link", "mode", "channel", "links", "interface"]
source: https://www.ciscozine.com/configuring-link-aggregation-with-etherchannel/
source_anchor: ""
source_lines: [1, 143]
section: "Claude_Anthropic"
---
EtherChannel bundles individual Ethernet links into a single logical link that provides bandwidth up to **1600 Mbps** (Fast EtherChannel, full duplex) or **16 Gbps** (Gigabit EtherChannel) between two Cisco Catalyst switches. All interfaces in each EtherChannel must be the same speed and duplex, and both ends of the channel must be configured as either a Layer 2 or Layer 3 interface.

If a link within the EtherChannel bundle fails, traffic previously carried over the failed link is carried over the remaining links within the EtherChannel.

There are two protocols used for the link aggregation:

- Cisco’s proprietary Port Aggregation Protocol (**PAgP** ).
- IEEE standard Link Aggregation Protocol (**LACP** )


PAgP packets are sent between Fast EtherChannel-capable ports to negotiate the forming of a channel. When PAgP identifies matched Ethernet links, it groups the links into an EtherChannel. The EtherChannel is then added to the spanning tree as a single bridge port.

| **Mode (PAgP)** | **Description** | 
| Auto | Places an interface into a passive negotiating state, in which the interface responds to PAgP packets it receives but does not start PAgP packet negotiation. This setting minimizes the transmission of PAgP packets and is the default. | 
| Desirable | Places an interface into an active negotiating state, in which the interface starts negotiations with other interfaces by sending PAgP packets. | 
| On | Forces the interface to channel without PAgP. With the **on** mode, a usable**EtherChannel** exists only when an interface group in the**on** mode is connected to another interface group in the**on** mode. | 


LACP is part of an IEEE specification (802.3ad) that allows several physical ports to be bundled together to form a single logical channel. LACP allows a switch to negotiate an automatic bundle by sending LACP packets to the peer. It performs a similar function as PAgP with Cisco EtherChannel. Because LACP is an IEEE standard, it can be used to facilitate EtherChannels in mixed-switch environments.

| **Mode (LACP)** | **Description** | 
| Passive | The switch does not initiate the channel, but does understand incoming LACP packets. The peer (in active state) initiates negotiation (by sending out an LACP packet) which we receive and reply to, eventually forming the aggregation channel with the peer. This is similar to the auto mode in PAgP. | 
| Active | We are willing to form an aggregate link, and initiate the negotiation. The link aggregate will be formed if the other end is running in LACP active or passive mode. This is similar to the desirable mode of PAgP. | 
| On | The link aggregation is forced to be formed without any LACP negotiation .In other words, the switch will neither send the LACP packet nor process any incoming LACP packet. This is similar to the on state for PAgP. | 



**Etherchannel** Negotiation Protocols

| **Negotiation Mode**  |  | **Packets Sent?**  | **Charateristics** | 
| **PAgP** | **LACP** |  |  | 
| on | on | No | All ports channeling | 
| auto | passive | Yes | Waits to channel until asked | 
| desirable | active | Yes | Actively asks to form a channel | 


**Matrix of Load Balancing Methods** Cisco EtherChannel technology is composed of several Fast Ethernet links and is capable of load balancing traffic across those links. Unicast, broadcast, and multicast traffic is evenly distributed across the links, providing higher performance and redundant parallel paths. When a link fails, traffic is redirected to the remaining links within the channel without user intervention and with minimal packet loss. To 

**define the load-balance method**, enter in the global configuration mode and

**use the command “port-channel load-balance ?”**to see the load-distribution method availables.

This matrix consolidates the load balancing methods:

| **Platform** | **Address Used in XOR** | **Source-Based?** | **Destination-Based?** | **Source-Destination-Based?** | **Load Balancing Method—Configurable/Fixed?** | 
| 6500/6000 | Layer 2, Layer 3 addresses, Layer 4 information, or MPLS information ² | Yes | Yes | Yes | Configurable | 
| 5500/5000 | Layer 2 address only | — | — | Yes | Cannot change the method | 
| 4500/4000 | Layer 2, Layer 3 addresses, or Layer 4 information | Yes | Yes | Yes | Configurable | 
| 2900XL/3500XL | Layer 2 address only | Yes | Yes | — | Configurable | 
| 3750/3560 | Layer 2 or Layer 3 address only | Yes | Yes | Yes | Configurable | 
| 2950/2955/3550 | Layer 2 address only ¹ | Yes | Yes | —1 | Configurable | 
| 1900/2820 | These platforms use a special method of load balancing. See the Catalyst 1900/2820 section for details. |  |  |  |  | 
| 8500 | Layer 3 address only | — | — | Yes | Cannot change the method | 

¹ For the 3550 series switch, when source-MAC address forwarding is used, load distribution based on the source and destination IP address is also enabled for routed IP traffic. All routed IP traffic chooses a port based on the source and destination IP address.

² For the 6500 series switches that run Cisco IOS, MPLS layer 2 information can also be used for load balancing MPLS packets.


**Example #1: Configuring Layer2 Etherchannel**

In this example, the FastEthernet0/0-0/1-0/2 (on Ciscozine_SW1 and Ciscozine_SW2) must belong to vlan 10; it is required to create a Layer2 etherchannel using LACP with desirable mode on Ciscozine_SW1 and passive mode on the Ciscozine_SW2. The configuration is:

**Ciscozine_SW1 etherchannel L2 configuration**

Ciscozine_SW1# configure terminal
Ciscozine_SW1(config)# interface range fastethernet0/0 -2
Ciscozine_SW1(config-if-range)# switchport mode access
Ciscozine_SW1(config-if-range)# switchport access vlan 10
Ciscozine_SW1(config-if-range)# channel-protocol lacp
Ciscozine_SW1(config-if-range)# channel-group 1 mode active


**Ciscozine_SW2 etherchannel L2 configuration**

Ciscozine_SW2# configure terminal
Ciscozine_SW2(config)# interface range fastethernet0/0 -2
Ciscozine_SW2(config-if-range)# switchport mode access
Ciscozine_SW2(config-if-range)# switchport access vlan 10
Ciscozine_SW1(config-if-range)# channel-protocol lacp
Ciscozine_SW2(config-if-range)# channel-group 1 mode passive



**Example #2: Configuring Layer3 Etherchannel**

In this example, the FastEthernet0/0-0/1-0/2 (on Ciscozine_SW1 and Ciscozine_SW2) must be aggregated; it is required to create a Layer3 etherchannel using PAgP with desirable mode on Ciscozine_SW1 and auto mode on the Ciscozine_SW2. The configuration is:

**Ciscozine_SW1 etherchannel L3 configuration**

Ciscozine_SW1# configure terminal
Ciscozine_SW1(config)# interface port-channel 1
Ciscozine_SW1(config-if)# no switchport
Ciscozine_SW1(config-if)# ip address 172.16.1.11 255.255.255.0
Ciscozine_SW1(config-if)# end
Ciscozine_SW1# configure terminal
Ciscozine_SW1(config)# interface range fastethernet0/0 -2
Ciscozine_SW1(config-if-range)# no switchport
Ciscozine_SW1(config-if-range)# no ip address
Ciscozine_SW1(config-if-range)# channel-group 1 mode desirable
Ciscozine_SW1(config-if-range)# end


**Ciscozine_SW2 etherchannel L3 configuration**

Ciscozine_SW2# configure terminal
Ciscozine_SW2(config)# interface port-channel 1
Ciscozine_SW2(config-if)# no switchport
Ciscozine_SW2(config-if)# ip address 172.16.1.12 255.255.255.0
Ciscozine_SW2(config-if)# end
Ciscozine_SW2# configure terminal
Ciscozine_SW2(config)# interface range fastethernet0/0 -2
Ciscozine_SW2(config-if-range)# no switchport
Ciscozine_SW2(config-if-range)# no ip address
Ciscozine_SW2(config-if-range)# channel-group 1 mode auto
Ciscozine_SW2(config-if-range)# end


**REMEMBER**: The “no switchport” command is required to change interface from layer2 to layer3 mode.

Use the “show etherchannel” command to display port-channel information after configuration and remember to save the configuration!

The third table, comparing PAgP and LACP, should say “No” below “Packets Sent?”.

Which? 1600 Mbps (Fast EtherChannel, full duplex) or 16 Gbps (Gigabit EtherChannel)

1600 Mbps is 1.6 Gbps! Probably 16000 Mbps.

Is it possible to have one end configured as l2 while the other end as l3 port-channel? Does this kind of configuration (mix of l2&l3) work?

Many thanx

No, the interface on each sides must be the same: L2 – L2 or L3 – L3

Nice

I think Fabio means when using fast ethernet, you can have up to 1.6Gbps, but when using gigabit ethernet, you can have 16Gbps. Correct me if I am wrong, I am learninbg also :)

When PagP or LACP are ‘on’, no packet will be sent to make a channel. Indeed: “The link aggregation is forced to be formed without any LACP negotiation .In other words, the switch will neither send the LACP packet nor process any incoming LACP packet. This is similar to the on state for PAgP.”

Ugh! :) Please excuse my avatar. I must have been having a bad day!

So, if I have 2 cables linking 2 switches together, and the purpose I am aiming for is to double the bandwidth between the 2, should I be using channel-group [#] mode on, as opposed to mode active or passive, or does it make any difference? I am trying to get double the bandwidth – I am not aiming for fault tolerance. So I am trying to understand if the mode I am using has any effect on that. Like, let’s say I have 2 buildings connected by fiber. Let’s say each building has 2 stacked switches, so each stack is one logical switch. My understanding is, if I connect one cable between switch A in the stack (in building A), and the other end of this same cable to switch A in building B, and then connect the second cable to switch B in building A and the other end of this cable to switch B in building B, and then create a port-channel with mode active, this will give me fault-tolerance. But I don’t know if this will give me more bandwidth. So let’s say I want more bandwidth and don’t care about fault-tolerance. So if I connect both cables to switch A in both buildings, and use mode active, will that give me twice the bandwidth of what only one cable between the building would give me? Or do I need to use mode ON in the port-channel? Does the physical connection determine the outcome, or the mode used in the port-channel, or both? Many thanks!
