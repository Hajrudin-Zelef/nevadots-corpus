# Overview

This document defines **Quality of Service (QoS)** usage in RouterOS based on **Marvell Prestera switch chips**.

QoS is a set of features in network switches that allow network administrators to prioritize traffic and allocate network resources to ensure that important data flows smoothly and with low latency.

The primary function of QoS in network switches is to manage network traffic in a way that meets the specific requirements of different types of network applications. For example, voice and video data require low latency and minimal packet loss to ensure high-quality communication, while file transfers and other data applications can tolerate higher levels of latency and packet loss.

QoS works by identifying the type of traffic flowing through the switch and assigning it a priority level based on its requirements. The switch can then use this information to alter packet headers and prioritize the flow of traffic, ensuring that higher-priority traffic is given preferential treatment over lower-priority traffic.

RouterOS v7.15+ is required to support all QoS features:

1. **QoS Marking.** QoS profile matching by ingress packet headers, then egress header alternation according to the assigned QoS profiles.
2. **QoS Enforcement** . Avoid or resolve congestion based on the assigned QoS profiles and traffic shaping.
3. **QoS Policy** . Assign QoS profiles via ACL rules.
4. Active Queue Management: **WRED** (Weighted Random Early Detection),**ECN** notification, and processing,**PFC** (Priority-based Flow Control).
5. Traffic shaping.

## QoS Changes in RouterOS v7.23

RouterOS version 7.23 introduced significant changes in Quality of Service offloading, simplifying the configuration process and introducing "**lossless**" traffic classes. While hardware resources are finite, an active queue management with the help of ECN and/or PFC prevents packet loss if all parties support the features and are configured properly. Prior RouterOS versions supported ECN and PFC as well, but in v7.23, the configuration process was streamlined.

Not all devices support lossless traffic. Check the QoS Device Support table.

### Configuration Change Summary

- QoS Settings: most changes were set to "auto" by default, allowing RouterOS to pick the best known settings for the setup.
- QoS Settings: added `lossless-traffic-class` and`lossless-buffers` , allowing explicit specification of lossless traffic and the reservation of queue resources for it. The user can leave those fields to "auto" as well.
- QoS Monitor: displays the shared pool usage for lossy and lossless separately.
- Tx Queue: removed shared pool index. Router OS automatically selects the shared pool based on the traffic type (lossy or lossless).
- QoS Profile: added automapping to the specified PCP and DSCP values (enabled by default). For example, adding a QoS profile with DSCP=46 automatically applies the profile to traffic received from trusted ports.

# QoS Terminology

These terms will be used throughout the article.

- **QoS** - Quality of Service.
- **ACL** - Access Control List, a set of switch rules used to filter network traffic based on specified criteria.
- **AQM** - Active Queue Management.
- **DSCP** - Differentiated Services Code Point, a 6-bit field in the IP header used to prioritize network traffic.
- **ECN** - Explicit Congestion Notification.
- **ETS** - Enhanced Transmission Selection.
- **PCP** - Priority Code Point, a 3-bit field in the VLAN header used to prioritize traffic within a VLAN.
- **PFC** - Priority-based Flow Control (IEEE 802.1Qbb).
- **RoCE** - RDMA over Converged Ethernet.
- **WRED** - Weighted Random Early Detection.
**/in/eth/sw/**
**/interface/ethernet/switch/**

# QoS Enhancements in new generation Marvell Prestera Switch Chips

MikroTik devices with new generation Marvell Prestera switch chips (e.g. CRS8xx series running the switch-marvell package) offer a new approach in QoS enforcement. Previous models required making a tradeoff between guaranteed and shared Tx queue buffers. By increasing the shared buffer percentage, the device can absorb larger traffic bursts, but at the same time, one congested port can occupy all shared buffers, limiting the QoS capabilities of other ports. Using the new generation switch chips enable the best of both worlds by introducing **Dynamic Buffers**. A switch chip dynamically adjusts port and queue buffer limits based on the current congestion level. If there is no congestion within the device, a single port can absorb a large burst of traffic, preventing packet loss. When the Tx queue size increases across multiple ports, the device reduces per-port and per-queue limits, enforcing fair use of shared resources while ensuring enough buffers for non-congested queues to continue forwarding traffic. The entire process is automated, requiring zero configuration from the user.

Another improvement in new generation Marvell Prestera switch chips is eliminating the shortage of enqueued packet descriptors for storing control data. Previous generation devices had Packet Cap and Use stats. When a device received a burst of small packets, it could start tail-dropping packets due to hitting Packet Cap, even though it had enough queue buffers to store the payload. New generation devices ensure there is always enough memory to store control data, leaving the Dynamic Buffer size as the only limiting factor. That's why there are no Packet Cap/Use stats in new generation devices.

New generation devices require QoS HW Offloading to be enabled at all times (**qos-hw-offloading=yes**). RouterOS ignores user requests to disable it while keeping the field for backward compatibility.

| Switch Chip | Models | QoS Profiles | QoS Maps | Tx Managers | WRED | ECN | PFC | Lossless Buffers | Dynamic Buffers | Port/Queue Usage Stats | 
|---|---|---|---|---|---|---|---|---|---|---|
| **98DX3236** | CRS305-1G-4S+IN CRS326-24G-2S+ (RM/IN) CRS328-24P-4S+RM CRS328-4C-20S-4S+RM | 128 | 1 | 8 |  |  |  |  |  | Current values | 
| **98DX226S** | CRS305-1G-4S+OUT (FiberBox Plus) CRS310-1G-5S-4S+ (netFiber 9/IN) CRS310-8G+2S+IN CRS318-16P-2S+OUT (netPower 16P) CRS320-8P-8B-4S+RM CRS418-8P-8G-2S+RM CRS418-8P-8G-2S+5axQ2axQ-RM | 128 | 1 | 8 |  |  |  |  |  | Current values | 
| **98DX224S** | CRS318-1Fi-15Fr-2S-OUT (netPower 15FR) | 128 | 1 | 8 |  |  |  |  |  | Current values | 
| **98DX2528** | CRS304-4XG-IN | 128 | 1 | 8 |  |  |  |  |  | Current values | 
| **98DX8525** | CCR2216-1G-12XS-2XQ CRS518-16XS-2XQ-RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Max fill <sup>1</sup> | 
| **98DX4310** | CRS504-4XQ (IN/OUT) CRS510-8XS-2XQ-IN RDS2216-2XG-4S+4XS-2XQ | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Max fill <sup>1</sup> | 
| **98DX8208** | CRS309-1G-8S+IN | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98DX8212** | CRS312-4C+8XG-RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98DX8216** | CRS317-1G-16S+RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98DX8332** | CRS326-24S+2Q+RM CRS326-4C+20G+2Q+RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98DX3257**  | CRS354-48G-4S+2Q+RM CRS354-48P-4S+2Q+RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98DX3255** | CCR2116-12G-4S+ | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Current values <sup>2</sup> | 
| **98CX8410** | CRS520-4XS-16XQ-RM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ |  | Unavailable <sup>3</sup> | 
| **98DX7335** | CRS812-8DS-2DQ-2DDQ-RM CRS804-4DDQ-hRM | 1024 | 12 | 15 | ✔ | ✔ | ✔ | ✔ | ✔ | Current values + Max fill | 

<sup>1</sup> The device gathers max queue fill statistics instead of displaying the current usage values. Use the **reset-counters** command to reset those stats.

<sup>2</sup> Due to hardware limitations, some switch chip models may break traffic flow while accessing QoS port/queue usage data.

<sup>3</sup> Usage data for individual queues on a port are unavailable, only the total usage for the entire port can be accessed.

# Applications and Usage Examples

## Basic Configuration Example

In this example, we define just one QoS level - VoIP (IP Telephony) on top of the standard "Best Effort" class. Let's imagine that we have a CRS326-24G-2S+ device where:

- all ports are bridged and using **vlan-filtering** ;
- sfp-sfpplus1 is a VLAN trunk connected to another switch;
- ether1-ether9 are dedicated ports for IP phones;
- ether10-ether24 are standard ports for host connection;

First, we need to define QoS profiles. Defined `dscp` and `pcp` values that will be used in forwarded packets on egress:

Port-based QoS profile assignment on dedicated ports for IP phones applies to ingress traffic. Other Ethernet ports will use the default `profile` (where `dscp=0` and `pcp=0`):

The trunk port receives both types of QoS traffic, we need to enable `trust-l3` and `trust-l2` to differentiate them:

If you are running RouterOS version 7.23 or later, the VLAN priority and IP DSCP mapping is created automatically, so this step is not required. If you are using an earlier RouterOS version, you must manually create the VLAN priority and IP DSCP mapping with the QoS profile:

Finally, enable QoS hardware offloading for the above settings to start working:

It is possible to verify the port QoS settings with `print` command:

Now incoming packets on ports ether1-ether9 are marked with a Priority Code Point (PCP) value of 5 and a Differentiated Services Code Point (DSCP) value of 46, and incoming packets on ports ether10-ether24 are marked with PCP and DSCP values of 0. When packets are incoming to sfp-sfpplus1 port, any packets with a PCP value of 5 will retain their PCP value of 5 and DSCP value of 46, while all other packets will be marked with PCP and DSCP values of 0.

## Dante

Starting from RouterOS v7.15, all MikroTik QoS-Capable devices are compatible with Dante**.** 

Dante hardware use the following DSCP / Diffserv priority values for traffic prioritization.

| Dante Priority | Usage | DSCP Label | DSCP Value | 
|---|---|---|---|
| High | Time critical PTP events | CS7 | 56 | 
| Medium | Audio, PTP | EF | 46 | 
| None | Other traffic | BE | 0 | 

The example assumes that the switch is using its default configuration, which includes a default "bridge" interface and all Ethernet interfaces added as bridge ports, and any of these interfaces could be used for Dante.

First, create QoS Profiles to match Dante traffic classes, there is already a pre-existing "default" profile that corresponds to Dante's None priority.

If you are running RouterOS version 7.23 or later, the IP DSCP mapping is created automatically, so this step is not required. If you are using an earlier RouterOS version, create a QoS mapping to match QoS profiles based on DSCP values. 

Configure hardware queues to enforce QoS on Dante traffic.

Dante's High and Medium priority traffic is scheduled in strict order. The devices transmit time-critical PTP packets until queue7 gets empty, then proceed with audio (queue5). Other traffic gets transmitted only when PTP and audio queues are empty.

The next step is to enable trust mode for incoming Layer3 packets (IP DSCP field):

Finally, enable QoS hardware offloading for the above settings to start working:

When using Dante in multicast mode, it is beneficial to enable IGMP snooping on the switch. This feature directs traffic only to ports with subscribed devices, preventing unnecessary flooding. Additionally, enabling an IGMP querier (if not already enabled on another device in the same LAN), adjusting query intervals, and activating fast-leave can further optimize multicast performance.

## RDMA over Converged Ethernet (RoCE)

RoCE allows you to directly access memory on remote storage systems using Ethernet networks without involving the host CPU. This capability significantly reduces latency and CPU overhead, making RoCE ideal for high-performance computing and data center environments. RoCE also enables a converged network, where various services (such as data storage, networking, and multimedia) run over a single Ethernet infrastructure. This simplifies network management and reduces the cost and complexity of maintaining separate networks.

RoCE achieves this through the use of **ECN** and **PFC** mechanisms. These features help prevent network congestion and packet loss, ensuring reliable, lossless communication. See the device feature table for compatible switches. Although switches can support RoCE environments, the end hosts must also be compatible with the RoCE protocol and equipped with RDMA-capable network interface cards (NICs).

There are two main versions of RoCE. RoCEv1 operates as an Ethernet link layer protocol and uses Ethertype 0x8915. RoCEv2 works over standard IP networks, using UDP destination port number 4791. ECN bits in the IP header are marked to signal network congestion, and a Congestion Notification Packet (CNP) is used to acknowledge congestion to the sender. For traffic prioritization, DSCP 26 is used for RoCEv2 traffic, while DSCP 48 for CNPs.

The following example can be used for lossless RoCEv2 with PFC and ECN and it assumes that the switch is using its default configuration, which includes a default "bridge" interface and all Ethernet interfaces added as bridge ports. The minimal recommended RouterOS version is 7.17.

First, configure additional profiles. Non-RoCE traffic will be assigned to already existing "default" profile with traffic-class 1, RoCEv2 to traffic-class 3, and CNP to traffic-class 6.

If you are running RouterOS version 7.23 or later, the IP DSCP mapping is created automatically, so this step is not required. If you are using an earlier RouterOS version, create a QoS mapping to match QoS profiles based on DSCP values.

Configure hardware queues and scheduler. We are using ETS (`schedule=high-priority-group`) for traffic-class 1 and traffic-class 3 with 50% bandwith assigment each (`weight=1`), and strict priority scheduling for traffic-class 6. Additionally, enable ECN (`ecn=yes`) for traffic-class 3 to mark IP packets in the switch that experience congestion.

Although using `schedule=low-priority-group` allows you to create separate ETS scheduling and bandwidth allocation for a different set of traffic-classes, it is not recommended to use this setting together with `lldp-dcbx=yes`. The reason is that the ETS Configuration/Recommendation TLVs are designed to handle a single bandwidth allocation across traffic classes, thus `schedule=high-priority-group` should be used instead.

Configure PFC profile for traffic-class 3 to ensure a lossless environment for RoCEv2 traffic.

Set Layer3 trust mode (`trust-l3=keep`) on switch ports where RoCEv2 traffic is expected, set PFC (`pfc=pfc-tc3`) and egress-rate for queue3 to comply with PFC requirements (`egress-rate-queue3=10.0Gbps`). In this example, 10Gbps SFP+ interfaces are used, and the egress rate can be set to match the physical speed of the interface. Change this property depending on your interface speeds.

Enable QoS hardware offloading for the above settings to start working.

Enable the LLDP Data Center Bridging Capability Exchange Protocol (DCBX) to share QoS settings and capabilities with other neighboring devices.

As an optional step, increase the L2MTU to accommodate larger data packets.

# QoS Marking

## Understanding Map ranges

In order to avoid defining all possible PCP and DSCP mappings, RouterOS allows setting multiple values and ranges for PCP and DSCP values for QoS Profile mapping.

In the following example, PCP values 0 and 2 use the default QoS profile, 1, 3-4 - streaming, 5 - voip, and 6-7 - control.

## Understanding Port, Profile, and Map relation

Each switch port has Layer2 and Layer3 trust settings that will change how ingress packets are classified into QoS profiles and what PCP and DSCP values will be used. Below are tables that describe all possible options:

| **qos-trust-l2** | **qos-trust-l3** | Behavior | 
|---|---|---|
| **ignore** | **ignore** | The port is considered untrusted. Both headers are ignored, and the port's **profile** is forced to all ingress packets. This is the default setting. | 
| **ignore** | **trust** | Trust the Layer 3 header. Use the DSCP field from the IP header of ingress packets for QoS profile lookup (see `/in/eth/sw/qos/map/ip` ). If the lookup fails (no QoS profiles are mapped to the given DSCP value), the**default** QoS profile is used (not the switch port's QoS profile). The switch port's**profile** field is used only for non-IP traffic. | 
| **ignore** | **keep** | Trust the Layer 3 header. Use the DSCP field from the IP header of ingress packets for QoS profile lookup (see `/in/eth/sw/qos/map/ip` ). If the lookup fails, the**default** QoS profile is used. The switch port's**profile** field is used only for non-IP traffic. If the forwarded/routed packet is VLAN-tagged, its PCP value is set from the selected QoS profile. However, the original DSCP value of the packet is kept intact. | 
| **trust** | **ignore** | Trust the Layer 2 header, but ignore L3. If an ingress packet is VLAN-tagged, use the PCP field from the VLAN header for QoS profile lookup (see `/in/eth/sw/qos/map/vlan` ). If the lookup fails (no QoS profiles are mapped to the given PCP value), the**default** QoS profile is used. The switch port's**profile** field is used only for untagged traffic. | 
| **trust** | **trust** | Trust both headers, but Layer 3 has higher precedence. In the case of an IP packet, use the DSCP field for QoS profile lookup (see `/in/eth/sw/qos/map/ip` ). If the DSCP-to-QoS lookup fails, use the**default** profile. If the packet is not an IP packet but is VLAN-tagged, use the PCP field from the VLAN header for QoS profile lookup (see`/in/eth/sw/qos/map/vlan` ).  If the VLAN-to-QoS lookup fails, use the**default** QoS profile. Non-IP untagged packets use the switch port's**profile** . | 
| **trust** | **keep** | The same as **trust+trust** , but the original DSCP value is preserved in forwarded/routed packets. | 
| **keep** | **ignore** | Trust the Layer 2 header but ignore L3. If an ingress packet is VLAN-tagged, use the PCP field from the VLAN header for QoS profile lookup (see `/in/eth/sw/qos/map/vlan` ). If the lookup fails (no QoS profiles are mapped to the given PCP value), the**default** QoS profile is used. The switch port's**profile** field is used only for untagged traffic. If the packet is VLAN-tagged on both ingress and egress, the original PCP value is kept. | 
| **keep** | **trust** | Trust both headers, but Layer 3 has higher precedence. In the case of an IP packet, use the DSCP field for QoS profile lookup (see `/in/eth/sw/qos/map/ip` ). If the DSCP-to-QoS lookup fails, use the**default** profile. If the packet is not an IP packet but is VLAN-tagged, use the PCP field from the VLAN header for QoS profile lookup (see`/in/eth/sw/qos/map/vlan` ).  If the VLAN-to-QoS lookup fails, use the**default** QoS profile. Non-IP untagged packets use the switch port's**profile** . If the packet is VLAN-tagged on both ingress and egress, the original PCP value is kept. The DSCP value in forwarded/routed packets is set from the selected QoS profile. | 
| **keep** | **keep** | Trust both headers, but Layer 3 has higher precedence. In the case of an IP packet, use the DSCP field for QoS profile lookup (see `/in/eth/sw/qos/map/ip` ). If the DSCP-to-QoS lookup fails, use the**default** profile. If the packet is not an IP packet but is VLAN-tagged, use the PCP field from the VLAN header for QoS profile lookup (see`/in/eth/sw/qos/map/vlan` ).  If the VLAN-to-QoS lookup fails, use the**default** QoS profile. Non-IP untagged packets use the switch port's**profile** . Keep both the original PCP and/or DSCP values intact in cases of VLAN-tagged and/or IP packets, respectively. | 

| **Port settings**  |  | The selected QoS profile and the source for PCP / DSCP field values in forwarded/routed packets |  |  |  |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **qos-trust-l2**  | **qos-trust-l3**  | VLAN-Tagged IP |  |  | Untagged IP |  |  | VLAN-Tagged Non-IP |  |  | Untagged Non-IP |  |  | 
|  |  | QoS Profile | PCP | DSCP | QoS Profile | PCP *<sup>1</sup>* | DSCP | QoS Profile | PCP | DSCP | QoS Profile | PCP *<sup>1</sup>* | DSCP | 
| ignore | ignore | profile | profile | profile | profile | profile | profile | profile | profile | - | profile | profile | - | 
| ignore | trust | map/ip | map/ip | map/ip | map/ip | map/ip | map/ip | profile | profile | - | profile | profile | - | 
| ignore | keep | map/ip | map/ip | original | map/ip | map/ip | original | profile | profile | - | profile | profile | - | 
| trust | ignore | map/vlan | map/vlan | map/vlan | profile | profile | profile | map/vlan | map/vlan | - | profile | profile | - | 
| trust | trust | map/ip | map/ip | map/ip | map/ip | map/ip | map/ip | map/vlan | map/vlan | - | profile | profile | - | 
| trust | keep | map/ip | map/ip | original | map/ip | map/ip | original | map/vlan | map/vlan | - | profile | profile | - | 
| keep | ignore | map/vlan | original | map/vlan | profile | profile | profile | map/vlan | original | - | profile | profile | - | 
| keep | trust | map/ip | original | map/ip | map/ip | profile | map/ip | map/vlan | original | - | profile | profile | - | 
| keep | keep | map/ip | original | original | map/ip | profile | original | map/vlan | original | - | profile | profile | - | 

<sup>1</sup> applies only when ingress traffic is untagged, but the egress needs to be VLAN-tagged.

Starting from **RouterOS v7.15**, it is possible to assign QoS profiles via Switch Rules (ACL).

**Sub-menu:** `/interface/ethernet/switch/rule`

| New/Changed Properties | Description | 
|---|---|
| **new-qos-profile** (*name* ) | The name of the QoS profile to assign to the matched packets. | 
| **keep-qos-fields** (*yes \| no* ; Default:**no** ) | Should the original values of QoS fields (PCP, DSCP) be kept ( *yes* ), or replace them with the ones from the assigned QoS profile (*no* )? Relevant only if**new-qos-profile** is set. | 
| **new-vlan-priority** (*0..7* ) | Deprecated and should be replaced with the respective **new-qos-profile** . Kept for backward compatibility. Relevant only if qos-hw-offloading=no. | 

The following example assigns a QoS profile based on the source MAC address.

# QoS Enforcement

## Hardware Queues

Each switch port has eight hardware transmission (tx) queues (queue0..queue7). Each queue corresponds to a traffic class (tc0..tc7) set by a QoS profile. Each ingress packet gets assigned to a QoS profile, which, in turn, determines the traffic class for tx queue selection on the egress port.

Hardware queues are of variable size - set by the Transmission Manager. Moreover, multiple ports and/or queues can share resources with each other (so-called *Shared Buffers*). For example, a device with 25 ports has memory (buffers) to queue 1200 packets in total. If we split the resources equally, each port gets 48 exclusive buffers with a maximum of 6 packets per queue (48/8) - which is usually insufficient to absorb even a short burst of traffic. However, choosing to share 50% of the buffers leaves each port with 24 exclusive buffers (3 per queue), but at the same time, a single queue can grow up to 603 buffers (3 exclusive + 600 shared).

RouterOS allows enabling/disabling the shared pool for each queue individually - for example, to prevent low-priority traffic from consuming the entire hardware memory. In addition, port buffer limits may prevent a single low-speed port from consuming the entire shared pool. See QoS Settings and Transmission Manager for details.

The default, best-effort (PCP=0, DSCP=0) traffic class is 1, while the lowest priority (PCP=1) has traffic class 0.

## Hardware Resources

The hardware (switch chips) has limited resources (memory). There are two main hardware resources that are relevant to QoS:

- *Packet descriptors* - contain packet control information (target port, header alternation, etc).
- *Data buffers* - memory chunks containing the actual payload. Buffer size depends on the switch chip model. Usually - 256 bytes.

One packet descriptor may use multiple buffers (depending on the payload size); buffers may be shared by multiple descriptors - in cases of multicast/broadcast. If the hardware does not have enough free descriptors or buffers, the packet gets dropped (*tail-drop*).

Hardware resources can be limited per destination type (multicast/unicast), per port, and per each tx queue. If any limits are reached, no more packets can be enqueued for transmission, and further packets get dropped.

RouterOS obscures low-level hardware information, allowing to set resource limits either in terms of packets or a percentage of the total amount. RouterOS automatically calculates the required hardware descriptor and buffer count based on the user-specified packet limit and port's MTU. Moreover, RouterOS comes with preconfigured hardware resources, so there is no need to do a manual configuration in common QoS environments.

Changing any hardware resource allocation parameter in runtime results in a temporary device halt when no packets can be enqueued nor transmitted. Temporary packet loss is expected while the device is forwarding traffic.

## Resource Saving (Previous Generation Devices)

This section does not apply to devices that support **Dynamic Buffers**.

Since reallocating hardware resources in runtime is not an option, RouterOS cannot automatically free queue buffers reserved for inactive ports. Those buffers remain unused. However, if the user knows that the specific ports will *never* be used (e.g., stay physically disconnected), the respective queue resources can be manually freed by using the built-in "offline" tx-manager with minimum resources, for example:

Avoid configuring the "offline" tx-manager on **switch-cpu** port.

When configuring `tx-manager` setting to QSFP+ or QSFP28 interfaces, you must apply the same configuration to all four sub-interfaces of a port. For example, if the interface qsfp28-1-1 is active and linked at 100Gbps, while sub-interfaces (qsfp28-1-2, qsfp28-1-3, qsfp28-1-4) are showing a non-running flag, **do not assign the "offline" tx-manager to thouse non-running sub-interfaces**. Doing so will impact the 100Gbps link as well. However, if none of the four sub-interfaces are running, it is safe to assign the "offline" tx-manager setting.

## Traffic Prioritization

The hardware provides two types of traffic transmission prioritization:

- **Strict Priority** - traffic from higher queues is always transmitted first;
- **Enhanced Transmission Selection (ETS)** - multiple queues participate in packet transmission scheduling at the same time.

**Strict priority** queues are straightforward. If the highest priority queue (Q7) has packets, those are transmitted first. When Q7 is empty, packets from Q6 get transmitted, and so on. The packets from the lowest priority queue (Q0) are transmitted only if all other queues are empty.

The downside of strict prioritization is increased latency in lower queues while "overprioritizing" higher queues. Suppose the acceptable latency of TC5 is 20ms, TC3 - 50ms. Traffic appearing in Q5 gets immediately transmitted due to the strict priority of the queue, adding extra latency to every packet in the lower queues (Q4..Q0). A packet burst in Q5 (e.g., a start of a voice call) may temporarily "paralyze" Q3, increasing TC3 latencies over the acceptable 50ms (or even causing packet drops due to full queue) while TC5 packets get transmitted at <1ms (way below the 20ms limit). Slightly sacrificing TC5 latency by transmitting TC3 packets in between would make everybody happy. That **ETS** is for.

**Enhanced Transmission Selection (ETS)** schedule traffic for transmission from multiple queues (group members) in a weighted round-robin manner. A queue's weight sets the number of packets transmitted from the queue in each round. For example, if Q2, Q1, and Q0 are the group members, and their weights are 3, 2, and 1, respectively, the scheduler transmits 3 packets from Q2, 2 - from Q1, and 1 - from Q0. The actual Tx order is "Q2, Q1, Q0, Q2, Q1, Q2" - for even fairer scheduling.

There are two hardware groups: `low-priority-group` and `high-priority-group`. There is a strict priority ordering between the two groups: the low-priority-group is transmitting only when *all* queues in the high-priority-group are empty. However, it is possible to use only one group for all queues.

The default (built-in) RouterOS queue setup is listed below. Q3-Q5 share the bandwidth within the high-priority group, where packets are transmitted while Q6 and Q7 are empty. Q0-Q2 are the members of the low-priority-group, where packets are transmitted while Q3-Q7 are empty.

It is recommended that all group members are adjacent to each other.

# Active Queue Management (AQM)

WRED is a per-queue congestion control mechanism that signals congestion events to the end-points by dropping packets. WRED relies on the existence of rate throttling mechanisms in the end-points that react to packet loss, such as TCP/IP. WRED uses a randomized packet drop algorithm in an attempt to anticipate congestion events and respond to them by throttling traffic rates before the congestion actually happens. The randomness property of WRED prevents throughput collapse related to the global synchronization of TCP flows.

WRED can be enabled/disabled per each queue in each Tx Manager. Disable WRED for lossless traffic! Also, there is no reason to enable WRED on high-speed ports where congestion should not happen in the first place.

The behavior is controlled via *WRED threshold*. WRED threshold is the maximum number of packets/bytes that can exceed the queue shared buffer limit (cap). A random packet drop begins when queue usage exceeds their respective capacities:

- `queueX-packet-use > queueX-shared-packet-cap` or
- `queueX-byte-use > queueX-shared-byte-cap` .

The more usage exceeds capacity, the higher the packet drop chance, reaching 100% at `queueX-shared-packet-cap + wred-packet-threshold` (or byte).

RouterOS automatically chooses the actual WRED threshold values according to queue or shared pool capacities. The user may shift the thresholds in one way or another via QoS Settings.

WRED requires the respective Tx queues to use shared buffers (**use-shared-buffers=yes**).

Choosing a WRED threshold value is a tradeoff between congestion anticipation and burst absorption. Setting a higher WRED threshold may lead to earlier traffic rate throttling and, therefore, resolve congestion. On the other hand, a high threshold leads to packet drops in limited traffic bursts that could be absorbed by the queue buffers and transformed losslessly if WRED didn't kick in. For instance, initiating a remote database connection usually starts with heavier traffic ("packet burst") at the initialization phase; then, the traffic rate drops down to a "reasonable" level. Any packet drop during the initialization phase leads to nothing but a slower database connection due to the need for retransmission. Hence, lowering the WRED threshold or entirely disabling WRED on such traffic is advised. The opposite case is video streaming. Early congestion detection helps select a comfortable streaming rate without losing too much bandwidth on retransmission or/and "overshooting" by sacrificing the quality level by too much.

Use Switch Rules (ACL) or other QoS Marking techniques to differentiate traffic and put packets into queues with desired WRED settings.

The following script only applies WRED to TCP/IP traffic by redirecting it to queue2. UDP and other packets are left in queue1 - since their end-points usually cannot respond to early drops. Queue1 and queue2 are scheduled equally - without prioritizing one queue over another.

## (ECN)

Some switch chips can perform ECN marking of IP packets on the hardware level, according to RFC 3168. Hardware ECN marking is based on the WRED mechanism, but instead of dropping IP packets, they are marked with CE (Congestion Experienced, binary 11) in the ECN field (two least significant bits in IPv4/TOS or IPv6/TrafficClass octet). Only ECN-Capable IP packets may be marked - those with the ECN field value of ECT(1) or ECT(0) (binary 01 or 10, respectively). Not ECN-Capable Transport packets (ECN=00) never get marked. If a packet already has the CE mark (ECN=11), it never gets cleared, even if the device does not experience congestion.

Set **ecn=yes** on Tx Manager Queue to enable ECN marking.

ECN marking mechanism requires the respective Tx queues to use shared buffers (**use-shared-buffers****=yes)**.

The packet receives the CE mark if all conditions below are met:

1. The packet is either IPv4 or IPv6.
2. The ECN field value in IP header is either ECT(1) or ECT(0).
3. Egress port's Tx Queue has **ecn=yes** and uses shared buffers (**use-shared-buffers=yes** ).
4. `queueX-packet-use > queueX-shared-packet-cap` or`queueX-byte-use > queueX-shared-byte-cap` .

Since enabling ECN (ecn=yes) prevents ECN-capable packet drop, queue usage may exceed WRED thresholds if the traffic sender doesn't react to congestion notification in time.

Priority-Based Flow Control (PFC) provides lossless operation for up to eight traffic classes, so that congestion in one traffic class does not pause other traffic classes. In addition, PFC enables co-existence of loss-sensitive traffic types with loss tolerant traffic type in the same network.

PFC-capable switch chips are complaint with IEEE 802.1Qbb PFC, meaning that the respective devices are capable of generating and responding to PFC frames. On the triggering part, the PFC frame is sent by the source port and traffic class experiencing the congestion. The timer values of the generated PFC frames are 0xFFFF for pause (XOFF) and 0x0 for resume (XON), and the appropriate bit in the priority enable vector is set. On the response part, the received PFC frame pauses the specific priority queues on the port that received the PFC frame for the duration specified by the PFC frame.

In RouterOS, PFC configuration is organized in profiles, where each port can be assigned to a specific profile. A PFC profile defines the traffic classes to enable PFC on, pause/resume thresholds to send XOFF/XON PFC frames, respectively, and whenever the assigned ports should transmit or/and receive PFC frames.

While congestion occurs on egress ports, PFC is triggered on the ingress port. Shared buffers must be used to associate the amount of ingressed traffic with the respective packets waiting in Tx queues. For each PFC-enabled traffic class, set **use-shared-buffers=yes** to the respective Tx Queues.

RouterOS implements 1:1 mapping between traffic classes and Tx queues. Packets with assigned traffic class 0 get enqueued in queue0, TC1 - queue1, etc., up to TC7-Q7. Hence, the terms "traffic class" and "tx queue" are used interchangeably in this text.

When choosing pause and resume thresholds, consider a delay in transmitting a PFC frame and processing it by the other side. For example, device A experienced congestion at time T, transmitted a PFC pause frame to device B, and B processed the frame and halted transmission at time T+D. During the delta time D, device B still kept sending traffic. If device A has configured the pause threshold to 100%, it has no free buffers available, and, therefore, packets may drop, which is unacceptable for lossless traffic classes. Lowering the pause threshold, let's say, down to 80% issues a PFC pause frame while still having free memory to accumulate traffic during the delta time D. The same applies to resume threshold. Setting it to 0% keeps the device idle during the delta time, lowering the overall throughput.

PFC Rx requires setting the egress rate to all associated queues to calculate pause time, even if it matches the wire speed. For example, if PFC runs on traffic class 3, the assigned ports require the `egress-rate-queue3` setting.

# Property Reference

## Switch settings

**Sub-menu:** `/interface/ethernet/switch`

Switch QoS settings (in addition to the existing ones).

| Property | Description | 
|---|---|
| **qos-hw-offloading** (*yes \| no* ; Default:**no** ) | Allows enabling QoS for the given switch chip (if the latter supports QoS). New generation devices force **qos-hw-offloading=yes** at all times. | 

When you enable QoS, turning off the **qos-hw-offloading** setting will not completely revert to the previous functionality. It is recommended to reboot the device after disabling it.

## Port settings

**Sub-menu:** `/interface/ethernet/switch/qos/port`

Switch port QoS settings. Assigns a QoS profile to ingress packets on the given port. The assigned profile can be changed via match rules if the port is considered trusted.

By default, ports are untrusted and receive the default QoS profile (Best-Effort, PCP=0, DSCP=0), where priority fields are cleared from the egress packets.

| Property | Description | 
|---|---|
| **egress-rate-queue0 .. egress-rate-queue7** (*integer: 0..18446744073709551615* ; Default**!egress-rate-queuex** ) | Sets egress traffic limitation (bits per second) for specific output queue. It is possible to specify the limit using suffixes like k, M, or G to represent kbps, Mbps, or Gbps. This setting can be combined with the overall per-port limit **egress-rate** (see ).**/in/eth/sw/port** | 
| **map**  (*name* ; Default:**default** ) | Allows user-defined QoS priority-to-profile mapping in the case of a trusted port or host (see **`/in/eth/sw/qos/map`** ). | 
| **pfc** (*name* ; Default:**disabled** )  | The name of the PFC profile to control ingress priority-based traffic flow (see **`/in/eth/sw/qos/priority-flow-control`**`).` | 
| **profile** (*name* ; Default:**default** ) | The name of the QoS profile to assign to the ingress packets by default (see **`/in/eth/sw/qos/profile`** ). | 
| **trust-l2** (*ignore \| trust \| keep* ; Default:**ignore** ) | Whenever to trust the Layer 2 headers of the incoming packets (802.1p PCP field):  | 
| **trust-l3** (*ignore \| trust \| keep* ; Default:**ignore** ) | Whenever to trust the Layer 3 headers of the incoming packets (IP DSCP field):  | 
| **tx-manager**  (*name* ; Default:**default** ) | The name of the Transmission Manager that is responsible for enqueuing and transmitting packets *from* the given port (see**`/in/eth/sw/qos/tx-manager`** ). | 

L3 trust mode has higher precedence than L2 unless *trust-l3=ignore* or the packet does not have an IP header.

Forwarded/routed packets obtain priority field values (PCP, DSCP) from the selected QoS profile, overwriting the original values unless the respective trust mode is set to **keep**.

Commands.

| Command | Description | 
|---|---|
| **print** | Print the above properties in a human-friendly format. | 
| **print stats** | Print port statistics: total and per-queue transmitted/dropped packets/bytes. | 
| **reset-counters** | Reset all counters in port statistics to zero. | 
| **print usage** | Print queue usage/resources. | 
| **print pfc**  | Pring Priority Flow Control stats | 
| **print rates**  | Print per-queue egress traffic limitation (set by **egress-rate-queueX** ) | 

### Port Stats

| Property | Description | 
|---|---|
| **name** | Port name. | 
| **tx-packet** | The total number of packets transmitted via this port. | 
| **tx-byte** | The total number of bytes transmitted via this port. | 
| **drop-packet** | The total number of packets should have been transmitted via this port but were dropped due to a lack of resources (e.g., queue buffers) or QoS Enforcement. | 
| **drop-byte** | The total number of bytes should have been transmitted via this port but were dropped. | 
| **tx-queue0-packet** .. **tx-queue7-packet** | The number of packets transmitted via this port from the respective queue. | 
| **tx-queue0-byte** ..**tx-queue7-byte** | The number of bytes transmitted via this port from the respective queue. | 
| **drop-queue0-packet** ..**drop-queue7-packet** | The number of packets dropped from the respective queue (or not enqueued at all due to lack of resources). | 
| **drop-queue0-byte** ..**drop-queue7-byte** | The number of bytes dropped from the respective queue. | 

### Port Resources/Usage

Due to hardware limitations, some switch chip models may break traffic flow while accessing QoS port `usage` data. Use port `usage` for diagnostics/troubleshooting only. For monitoring, use QoS `monitor` or Port `stats` instead.

| Property | Description | 
|---|---|
| **name** | Port name. | 
| **packet-cap** | Port's packet capacity. The maximum number of packets that can be enqueued for transmission via the port. | 
| **packet-use***<sup>1</sup>* | Port's packet usage. The number of packets that are currently enqueued in all port's queues. | 
| **byte-cap** | Port's byte capacity (buffer size). The maximum number of bytes that can be enqueued for transmission via the port. | 
| **byte-use***<sup>1</sup>* | Port's byte usage. The size of hardware buffers (in bytes) that are currently allocated for packets the enqueued packets. Since the buffers are allocated by blocks (usually - 256B each), the allocated buffer size can be bigger than the actual payload. | 
| **queue0-packet-cap** .. **queue7-packet-cap** <sup>*2*</sup> | Individual queue capacity. The maximum number of packets that can be enqueued in the respective queues (unless the **Shared Buffers** are enabled). | 
| **queue0-shared-packet-cap .. queue7-shared-packet-cap** <sup>*2*</sup> | Shared queue capacity (individual queue capacity + shared buffers). The maximum number of packets that can be enqueued in the respective queues. | 
| **queue0-packet-use** .. **queue7-packet-use** <sup>*2*</sup> | Queue packet usage. The number of enqueued packets in the respective queues. | 
| **queue0-byte-cap** .. **queue7-byte-cap** <sup>*2*</sup> | Individual queue capacity. The maximum number of bytes that can be enqueued in the respective queues (unless the **Shared Buffers** are enabled). | 
| **queue0-shared-byte-cap .. queue7-shared-byte-cap** <sup>*2*</sup> | Shared queue capacity (individual queue capacity + shared buffers). The maximum number of bytes that can be enqueued in the respective queues. | 
| **queue0-byte-use** .. **queue7-byte-use** <sup>*2*</sup> | Queue buffer usage (in bytes). The size of hardware buffers (in bytes) that are currently allocated for packets in the respective queues. | 
| **queue0-byte-max .. queue7-byte-max** <sup>*2*</sup> | Maximum queue buffer fill level (in bytes). Available only on devices that provide the queue statistics service. Use the **reset-counters** command to reset values. | 

<sup>1</sup> Port's packet/byte usage can exceed the capacity if **Shared Buffers** are enabled.

<sup>2</sup> Only the queues in use are printed.

### Port PFC Stats (Previous Generations)

| Property | Description | 
|---|---|
| **name** | Port name. | 
| **pfc** | PFC profile name. | 
| **pfc-rx**  | Received PFC frame count. | 
| **pfc-tx** | Transmitted PFC frame count. | 
| **pfc-paused-tc** | The list of traffic classes should be paused (from the sender's perspective). PFC pause frames (XOFF) are periodically sent with the listed timers set from this port. | 
| **pfc0-pause-threshold .. pfc7-pause-threshold**  | Pause thresholds of the respective traffic classes. Only PFC-enabled traffic classes are displayed. | 
| **pfc0-resume-threshold .. pfc7-resume-threshold** | Resume thresholds of the respective traffic classes. Only PFC-enabled traffic classes are displayed. | 
| **pfc0-use .. pfc7-use** | The current buffer usage of the respective traffic classes (in bytes). In other words, it is the total size of all queued packets on all ports that were received from this port. Only PFC-enabled traffic classes are displayed. | 

### Port PFC Stats (New Generations)

| Property | Description | 
|---|---|
| **name** | Port name. | 
| **pfc** | PFC profile name. | 
| **pfc0-use .. pfc7-use** | The current buffer usage of the respective traffic classes (in bytes). In other words, it is the total size of all queued packets on all ports that were received from this port. Only PFC-enabled traffic classes are displayed. | 
| **rx-pause** | Received pause frame count. | 
| **tx-pause** | Transmitted pause frame count. | 

## QoS Menu

**Sub-menu:** `/interface/ethernet/switch/qos`

Almost the entire QoS HW configuration is located under **`/in/eth/sw/qos`**. Such an approach allows storing all QoS-related configuration items in one place, easy monitoring and exporting (`/in/eth/sw/qos/export`).

QoS entries have two major flags:

- **H** - Hardware-offloaded.
- **I** - Inactive.

**Sub-menu:** `/interface/ethernet/switch/qos/settings`

| Property | Description | 
|---|---|
| **mirror-buffers** (*percent**: 1..90* ; Default:**auto** ) | Maximum amount of packet buffers for mirrored traffic (% of the total buffer memory). | 
| **mirror-profile** (*name* ; Default:**default** ) | The name of the QoS profile to assign to the mirrored packets (see **`/in/eth/sw/qos/profile`** ). | 
| **multicast-buffers** (*percent**: 1..90* ; Default:**auto** ) | Maximum amount of packet buffers for multicast/broadcast traffic (% of the total buffer memory). | 
| **shared-buffers** (*percent**: 0..90* ; Default:**auto** ) | Maximum amount of packet buffers that are shared between ports (% of the total buffer memory). Setting it to 0 disables buffer sharing. The remaining buffer memory is split between the ports. All buffers are treated as shared on new generation switch chips that use **Dynamic Buffers** . The switch chip automatically adjusts port and queue buffer limits based on the current congestion level. Using the auto value allows the device to utilize 100% of the available buffer memory, which is the recommended setting for most scenarios. In specific use cases where latency is more important than avoiding packet drops, the buffer limit can be manually reduced. | 
| **lossless-buffers**  (*percent**: 0..100* ; Default:**auto** ) | If the device supports multiple shared buffer pools, this setting allows adjusting the size of the lossless pool (% of the *shared* buffer memory, where 100% means all shared buffers allocated by the**shared-buffers** setting). For example, if shared-buffers=50 and lossless-buffers=80, the lossless pool receives 40% of the total buffer memory (80% of 50% or "0.8 * 0.5 = 0.4"), and the lossy pool receives the remaining 10% of shared buffers. | 
| **lossless-traffic-class**  (*integer array: 0..7* ; Default:**auto** ) | The list of lossless traffic classes. | 
| **wred-threshold**  (*low \| medium \| high;*  Default:**medium** )  | A relative amount of packets above a shared queue cap (" `queueX-shared-packet-cap` " or "`queueX-shared-byte-cap` ") where random drops take place. This threshold is applied only to queues with enabled Weighed Random Early Detection (**wred=yes** ) that use shared buffers (**use-shared-buffers=yes)** . The higher the queue buffer fill level, the higher the packet drop chance. The*low* threshold means the random tail drop starts later; the*high* - sooner. | 

## QoS Monitor

**Command:** `/interface/ethernet/switch/qos/monitor`

Monitors hardware QoS resources.

| Property | Description | 
|---|---|
| **total-packet-cap***(integer)* | Total packet capacity. The maximum number of hardware packet descriptors that the device can store is all queues. | 
| **total-packet-use***(integer)* | Total packet usage. The current number of packet descriptors residing in the hardware memory. | 
| **total-byte-cap***(byte)* | Total tx memory capacity. | 
| **total-byte-use***(byte)* | Total tx memory usage. The current number of bytes occupied by the packets in all tx queues. | 
| **multicast-packet-cap** *(integer)* | Multicast packet capacity. The maximum number of hardware packet descriptors that can be used by multicast/broadcast traffic. Depends on the **multicast-buffers** setting. | 
| **multicast-packet-use** *(integer)* | Multicast packet usage. The hardware makes a copy of the packet descriptor for each multicast destination. | 
| **mirror-ingress-packet-cap** *(integer)* | Ingress mirror packet capacity. The maximum number of hardware packet descriptors that can be used by ingress mirrored traffic. Depends on the **mirror-buffers** setting. | 
| **mirror-ingress-packet-use** *(integer)* | Ingress mirror packet usage. | 
| **mirror-ingress-byte-cap***(byte)* | Ingress mirror byte capacity. Depends on the **mirror-buffers** setting. | 
| **mirror-ingress-byte-use***(byte)* | Ingress mirror byte usage. | 
| **mirror-egress-packet-cap** *(integer)* | Egress mirror packet capacity. The maximum number of hardware packet descriptors that can be used by egress mirrored traffic. Depends on the **mirror-buffers** setting. | 
| **mirror-egress-packet-use** *(integer)* | Egress mirror packet usage. | 
| **mirror-egress-byte-cap***(byte)* | Egress mirror byte capacity. Depends on the **mirror-buffers** setting. | 
| **mirror-egress-byte-use***(byte)* | Egress mirror byte usage. | 
| **shared-packet-cap***(integer)* | Shared packet capacity. The maximum number of hardware packet descriptors that can be shared between ports and tx queues. Depends on the **shared-buffers** setting. | 
| **shared-packet-use***(integer)* | Shared packet usage. The current number of shared packet descriptors used by all tx queues. | 
| **shared-byte-cap***(byte)* | Shared tx memory capacity. Depends on the **shared-buffers** setting. | 
| **shared-byte-use***(byte)* | Shared tx memory usage. The current number of shared buffers occupied by the packets in all tx queues. | 
| **lossy-pool-packet-cap**  (integer) | Shared packet capacity of the lossy pool. The field is omitted if the device does not support multiple shared pools. | 
| **lossy-pool-packet-use (integer)** | Shared packet usage of the lossy pool. The field is omitted if the device does not support multiple shared pools. | 
| **lossless-pool-packet-cap (integer)** | Shared packet capacity of the lossless pool. The field is omitted if the device does not support multiple shared pools. | 
| **lossless-pool-packet-use (integer)** | Shared packet usage of the lossless pool. The field is omitted if the device does not support multiple shared pools. | 
| **wred-packet-cap***(integer)* | The maximum packet count that a queue can use above the shared cap (" `queueX-shared-packet-cap` " in "`/in/eth/sw/qos/port print usage` ") to trigger a random tail drop. For example, if "`queue1-shared-packet-cap=3072` " and "`wred-packet-cap=512` ", WRED triggers when`queue1-packet-use` exceeds 3072, reaching 100% drop rate at 3072+512=3584 packets. | 
| **wred-byte-cap***(integer)* | The maximum byte count that a queue can use above the shared cap (" `queueX-shared-byte-cap` ") to trigger a random tail drop. For example, if "`queue1-shared-byte-cap=768KiB` " and "`wred-byte-cap=128KiB` ", WRED triggers when`queue1-packet-use` exceeds 768KiB, reaching 100% drop rate at 768+128=896KiB. | 

**Sub-menu:** `/interface/ethernet/switch/qos/profile`

QoS profiles determine priority field values (PCP, DSCP) for the forwarded/routed packets. Congestion avoidance/resolution is based on QoS profiles. Each packet gets a QoS profile assigned based on the ingress switch port QoS settings (see `/in/eth/sw/port`).

| Property | Description | 
|---|---|
| **color** (*green \| yellow \| red* ; Default:**green** ) | Traffic color for color-aware drop precedence management. Leave the default value (green) for color-blind drop precedence management. | 
| **dscp** (*integer: 0..63* ; Default:**0** ) | IPv4/IPv6 DSCP field value for the egress packets assigned to the QoS profile. | 
| **name** (*string* ; Default: ) | The user-defined name of the QoS profile. | 
| **pcp** (*integer: 0..7* ; Default:**0** ) | VLAN priority value (IEEE 802.1q PCP - Priority Code Point). Used only if the egress packets assigned to the QoS profile are VLAN-tagged (have the 802.1q header). The value can be further altered via the QoS Egress Map. | 
| **traffic-class** (*integer: 0..7* ; Default:**0** ) | The traffic class determines the packet priority and the egress queue (see **tx-manager** ). The queue number is usually the same as the traffic class (packets with tc0 go into queue0, tc1 - queue1, ... tc7 - queue7). Unlike pcp, where 0 means the default priority but 1 - the lowest one (and further customizable), traffic classes are strictly ordered. TC0 always selects the lowest priority, etc. | 
| **automap**  (yes *\| no* ; Default:**yes** ) | Automatically map packets with matching PCP or DSCP values to this QoS profile. Only applies to **trusted** ports. | 

**Sub-menu:** `/interface/ethernet/switch/qos/map`

Priority-to-profile mapping table(-s) for trusted packets. All switch chips have one built-in map - **default**. In addition, some models allow the user to define custom mapping tables and assign different maps to various switch ports via the **qos-map** property:

- devices based on **Marvell Prestera **98DX224S, 98DX226S**** , or**98DX3236** switch chip models support only one map - default.
- devices based on **Marvell Prestera 98DX8xxx** ,**98DX4xxx** switch chips, or**98DX325x** model devices support up to 12 maps (the default + 11 user-defined).

| Property | Description | 
|---|---|
| **name** (*string* ; Default: ) | The user-defined name of the mapping table. | 

### `VLAN Map`

**Sub-menu:** `/interface/ethernet/switch/qos/map/vlan`

Matches VLAN priorities (802.1p PCP/DEI fields) to QoS profiles. By default, all values are matched to the default QoS profile.

| Property | Description | 
|---|---|
| **dei-only** (yes *\| no* ; Default:**no** ) | Map only packets with DEI (formerly CFI) bit set in the VLAN header. | 
| **map** (*name* ; Default:**default** ) | The name of the mapping table. | 
| **profile** (*name* ; Default: ) | The name of the QoS profile to assign to the matched packets. | 
| **pcp** (*range: 0..7* ; Default:**0** ) | VLAN priority (PCP) value(-s) for the lookup. | 

### DSCP Map

**Sub-menu:** `/interface/ethernet/switch/qos/map/ip`

Matches DSCP values to QoS profiles.

| Property | Description | 
|---|---|
| **dscp** (*range: 0..63* ; Default:**0** ) | DSCP value(-s) for the lookup. | 
| **map** (*name* ; Default:**default** ) | The name of the mapping table. If not set, the standard (built-in) mapping table gets altered. | 
| **profile** (*name* ; Default: ) | The name of the QoS profile to assign to the matched packets. | 

**Sub-menu:** `/interface/ethernet/switch/qos/tx-manager`

Transmission (Tx) Manager controls packet enqueuing for transmission and packet tx order. Different switch ports can be assigned to different Tx managers. The maximum number of hardware Tx managers depends on the switch chip model.

| Property | Description | 
|---|---|
| **name** (*string* ; Default: ) | The user-defined name of the Tx Manager | 
| **queue-buffers** *(percent: 0%..100% \| bytes \| auto;*  Default: **auto** ) | The total amount of hardware Tx buffers allocated to all ports linked to this Tx Manager. Any value but **auto** is NOT scaled by the number of ports. For example, if queue-buffers=30%, and there are 3 ports using this Tx Manager, each respective port receives 10% of total available resources. Adding two more ports to the Tx Manager drops per-port buffers down to 6% (30/5). | 

Port status has not effect on the allocated resources. Running ports receive the same amount of queue buffers as disconnected or disabled ones if all of them are assigned to the same Tx Manager.

### Transmission Queue Scheduler

**Sub-menu:** `/interface/ethernet/switch/qos/tx-manager/queue`

Each port has eight Tx queues. The assigned Tx Manager controls packet enqueuing and schedules transmission orders. Each queue can have either strict priority (where packets with the highest traffic class are always transmitted first) or grouped together for a weighted round-robin tx schedule.

Creating a Tx Manager automatically creates all eight respective queue schedulers.

Changing any properties of Tx manager or queues completely halts traffic enqueueing and transmission during the offload process. Temporary packet loss is expected while the device is forwarding traffic.

| Property | Description | 
|---|---|
| **tx-manager** (*name* ;*read-only* ) | The linked Tx Manager | 
| **traffic-class** (*integer: 0..7* ; *read-only* ) | The traffic class (tc0..tc7) and the respective port queue (queue0..queue7) that the scheduler controls. | 
| **schedule**  (*strict-priority \| high-priority-group \| low-priority-group* ) |  | 
| **weight** *(integer: 0..255;* Default:**1** ) | The weight value for the traffic class if it is a member of a schedule group. The field is not used in the case of strict priority schedule. | 
| **queue-buffers** *(percent: 0%..100% \| bytes \| auto;*  Default: **auto** ) | The amount of hardware Tx buffers allocated to this queue. Any value but **auto** is NOT scaled by the number of ports, i.e., the value gets split on ports linked to the Tx Manager. When given in percent, it means percentage of the tx-manager's queue-buffers value. | 
| **use-shared-buffers** *(yes \| no)* | Allow the queue to use the shared buffer pool when **queue-buffers**  are full. If the queue is full and the shared buffers are disabled, the packet gets dropped. If the shared buffers are enabled, the queue may use up to**shared-packet-cap** or**shared-poolX-packet-cap** (see QoS Settings for details) packets from the shared pool. | 
| **wred** (*yes \| no* ; Default:**no)** | Enables/disables Weighted Random Early Detection for the given queue. | 
| ***ecn (****yes \| no; Default: **no**)* | Enables/disables ECN marking of the transmitted packets. | 
| **wred-actual**  (*yes \| no* ;  read-only)  | The actual WRED value. | 
| **ecn-actual** (*yes \| no* ;  read-only) | The actual ECN value. | 

On some device models, due to hardware limitations, enabling ECN on one queue turns on CE marking of ECN-capable packets on all queues. In such cases, `ecn-actual=yes` despite `ecn=no`.

**Sub-menu:** `/interface/ethernet/switch/qos/priority-flow-control`

PFC configuration is organized in profiles. Different switch ports can be assigned to different PFC profiles. The maximum number of hardware Tx managers depends on the switch chip model. The builtin profile named "**disabled**" cannot be changed.

| Property | Description | 
|---|---|
| **name** (*string* ; Default: ) | The user-defined name of the PFC profile | 
| **pause-threshold** (*percent: 0%..100% \| bytes \| auto;* Default:**auto)** | Transmits a pause frame (XOFF) when the total size of enqueued packets reaches this threshold. Enqueued packets are counted per ingress port. Applies only when **tx=yes** . The value can be given either explicitly in bytes or percent of the respective shared pool size (**shared-poolX-byte-cap** ). | 
| **resume-threshold** (*percent: 0%..100% \| bytes \| auto;* Default:**auto)** | Transmits a resume frame (XON) when the total size of enqueued packets drops down to this threshold. Enqueued packets are counted per ingress port. Applies only when **tx=yes** . The value can be given either explicitly in bytes or percent of the respective shared pool size (**shared-poolX-byte-cap** ). | 
| **rx** (*yes \| no* ; Default:**no)** | Enables receiving of PFC frames. The received PFC frame pauses the specific priority queues on the port that received the PFC frame for the duration specified by the PFC frame. Disabling rx disables queue pausing. | 
| **traffic-class** (*integer array: 0..7* )  | The list of PFC-enabled traffic classes. | 
| **tx** (*yes \| no* ; Default:**no)** | Enables transmition of PFC frames. |