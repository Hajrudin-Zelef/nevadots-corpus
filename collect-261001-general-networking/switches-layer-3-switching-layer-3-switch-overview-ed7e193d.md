MS Layer 3 Switching and Routing
Click 日本語 for Japanese
Layer 3 (L3) SVI Routing Overview
A typical network design segments/divides the network based on the group that a device belongs or the function it performs. For example, a Corporate or a Production VLAN only has devices that belong to your own Organization; on the other hand, a Guest-WIFI VLAN only has visitor devices that are connected to a Guest wireless network (Guest SSID).
In such use-cases, VLANs separate devices into different broadcast domains and Layer 3 (L3) subnets. Network switches typically forward data to its destination based on Layer 2 (L2) packet attributes like VLAN-ID and MAC Address. Network switches that perform this forwarding operation are known as L2 switches. Devices within the same VLAN can communicate with each other without the need for routing as they live within the same broadcast domain and L3 subnet.
As a result, devices in separate VLANs require a L3 forwarding device (e.g., a router) to communicate with each other. The L3 device can be external to the switch or it can be a feature in the same switch. Layer 2-only switches require an external L3 routing device to provide communication between VLANs as they don't have L3 routing functionality i.e., they don't forward data to destination based on L3 attributes like destination IP address.
Many Cisco Meraki switches have L3 routing capability within the switch itself. E.g., a switch receives a packet, determines that the packet belongs to another VLAN, and sends the packet to the appropriate port within the destination VLAN. L3 routing is possible due to an internal interface called a "Switched Virtual Interface" or SVI for short.
When L3 routing is enabled on the switch, SVIs per VLAN/subnet are created which will forward data based on the destination IP address. Afterwards, devices living within separate VLANs can talk to one another without the need to be in the same broadcast domain or to have a router acting as a gateway.
Learn more with this free online training course on the Meraki Learning Hub:
Layer 3 (L3) Routed Ports Overview
A routed port is a physical port on a switch or router that is configured to act as a Layer 3 interface. It is used for routing IP packets instead of switching layer 2 frames. Unlike regular switch ports, a routed port is not associated with a specific VLAN and does not participate in Layer 2 operations such as STP. Routed ports are commonly used for connections between switches, routers, firewalls, or other network devices where routing is needed. Routed ports are a great option for connecting switches in core/distribution network for inter-switch routing and are ideal for point-to-point links. Routed ports, being physical interfaces, have a more straightforward and faster process for coming up and down compared to SVIs. SVIs need a Layer 2 port to be active before the SVI itself can be enabled as well as all ports using that same VLAN to be down prior to bringing the SVI down. Routed ports are ideal for point-to-point links, where you don't want to spread a VLAN across multiple devices. They can also be easier to manage when using features like ECMP (Equal Cost Multi-Path). Routed ports allow you to reuse the same VLAN ID on another L3 port or from an L2 VLAN without the risk of L2 traffic in between. Routed ports create separate Layer 3 segments, which means that broadcast traffic is confined to that specific subnet and doesn't affect other VLANs or routed ports.
Layer 3 routing capabilities are available on most Cisco Meraki switches. This allows the switches to route traffic between VLANs in a network without the need for an additional layer 3 device. This feature also allows you to control traffic between VLANs using Access Control Lists (ACLs) while reducing workload on your Router or Firewall. In addition, the Cisco Meraki L3 switch can also offer simple DHCP services through its SVIs or Routed Ports.
This feature is supported in IOS XE 17.18.1 and later on MS390 and Cloud Managed Catalyst switches.
Note: The configuration of routed ports is only available on the New Version of the UI
Supported Models
In order to enable and configure layer 3 routing on MS switches, a layer 3 capable switch is required.
The alert, "This switch is routing for too many hosts. Performance may be affected" will be displayed if the current number of routed clients exceeds the values listed in the table below.
| Model | Layer 3 Interfaces | Routes | Maximum Routable Clients | Features | 
|---|---|---|---|---|
| MS150 | 16 | 16 Static routes | 8192 | SVI Static Routing 17.2.1+ Required* DHCP Relay | 
| MS210 | 16 | 16 static routes | 8192 | SVI Static Routing DHCP Relay | 
| MS225 | 16 | 16 static routes | 8192 | SVI Static Routing DHCP Relay | 
| MS250 | 256 | 1024 1 (256 static routes) | 8192 | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS350 | 256 | 16384 1 (256 static routes) | 24K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS350X | 256 | 8192 (256 static routes) | 45K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS355 | 256 | 8192 (256 static routes) | 68K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS390 | 256 | 8192 (256 static routes) | 24K | Routed Port SVI Static Routing OSPFv2 DHCP Server + Relay Multicast Routing (PIM-SM) IPv6 Layer-3 Interfaces & Static Routing 2 BGP3 | 
| C9300(-M) | 256 | 8192 (256 static routes) | 24K | Routed Port SVI Static Routing OSPFv2 DHCP Server + Relay Multicast Routing (PIM-SM) IPv6 Layer-3 Interfaces & Static Routing 2 BGP3 | 
| C9350 | 1.9K | 96K | 64K | Routed Port SVI Static Routing OSPFv2 DHCP Server + Relay Multicast Routing (PIM-SM) IPv6 Layer-3 Interfaces & Static Routing 2 BGP4 | 
| Cloud managed C9200, C9200L(-M), C9500H, C9200CX | 256 | 8192 (256 static routes) | 24K | Routed Port SVI Static Routing OSPFv2 DHCP Server + Relay Multicast Routing (PIM-SM) IPv6 Layer-3 Interfaces & Static Routing 2 BGP3 | 
| MS410 | 256 | 16384 1 (256 static routes) | 24K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS425 | 256 | 8192 (256 static routes) | 212K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
| MS450 | 256 | 8192 (256 static routes) | 68K | SVI Static Routing OSPFv2 DHCP Server + Relay Warm Spare (VRRP) Multicast Routing (PIM-SM) | 
1 To prevent hardware TCAM exhaustion, the following platform limitations are enforced on the number of dynamically (OSPF) learned routes
MS250: 900
MS350, MS410: 15000
If the limit is reached, routes will be rejected indiscriminately and may result in erratic routing behavior. To minimize the impact of this, the default route will not be affected by the limit and will be accepted regardless.
2 Supported only on the specified switches series, on firmware versions CS 15.21.1 and higher.
3 Supported on IOS XE 17.18.1 and higher.
4. C9350 BGP support on IOS XE 26.1.1 and higher
Initializing Layer 3 Routing
You must create Layer 3 Interfaces in order to route traffic between VLANs. These special interfaces are called "Switched Virtual Interface" or SVI for short or routed ports. Only VLANs that have an SVI configured will be able to route traffic on the switch. An SVI is a kind of Layer 3 Routing interface and the term Layer 3 / L3 interface and SVI / SVI interface are used interchangeably. A routed port is a physical port on a switch or router that is configured to act as a Layer 3 interface.
Note: only clients/devices configured to use the correct SVI IP address as their gateway or next-hop will have its packets routed by your switch SVI. If a client/device within a VLAN use another IP address as their gateway, then your switch won't be doing the Layer 3 packet forwarding decision.
SVI MTU size is 1500
To start using layer 3 routing, navigate to the Switching > Configure > Routing & DHCP page. Alternatively, you could go to Switching > Monitor > Switches and click on the switch to be configured. Under L3 routing tab, click Configure - which takes you to the same Routing & DHCP page as above.
On the Routing & DHCP page, you will have the option to either "create interface" or to add an interface, if any L3 interfaces (SVI) or routed ports already exist in the network. Clicking on the available option will bring up the Interface Editor UI. Use the Interface Editor to configure your SVI or routed port.
Configuring an IPv4 L3 SVI Interface
- Interface name: A friendly name/description for the interface/VLAN.
- VLAN: The VLAN this L3 interface is in.
- Subnet: The network that this L3 interface is in, in CIDR notation (ex. 10.1.1.0/24).
- Interface IP: The IP address this switch will use for L3 routing on this VLAN/subnet. This cannot be the same as the switch's management IP.
- Multicast support: Enable multicast support if multicast routing between VLANs is required.
- Default gateway: When creating the first IPv4 interface on a switch, you will be prompted to enter a default gateway address. This is the next hop IPv4 address of another device on the network, used for any traffic that isn't going to a directly connected subnet or over a static route. This IP address must exist in a subnet with a L3 interface, and will be used for the default route next hop IP address.
- DHCP settings: If DHCP on this VLAN should be handled by the switch or forwarded to a server, make the appropriate selections. See the article on Configuring DHCP Services for more details.
- OSPF settings: This VLAN can be distributed via OSPF. See the MS OSPF Overview article for more details.
When complete, click Save or Save and add another to configure additional L3 interfaces.
 
L3 configuration changes on MS210, MS225, MS250, MS350, MS355, MS410, MS425, MS450 require the flushing and rebuilding of L3 hardware tables. As such, momentary service disruption may occur. We recommend making such changes only during scheduled downtime/maintenance window.
Configuring an IPv4 L3 Routed Port Interface
Note: This feature is only supported on the MS390 and Cloud Managed Catalyst switches.
- Interface mode: Select "Routed port".
- Switch or switch stack: Select the switch or switch stack for the routed port.
- Select Module (optional): Select if you have a modular Catalyst switch. If fixed then this will be greyed out.
- Switch ports: Select the routed port interface number.
- Name: A friendly name/description for the interface/routed port.
- VRF: Select the VRF if other than the default.
- IP toggle: Select if this routed port will be IPv4, IPv6, or Both
- Uplink: Select if this routed port will be the uplink interface to Dashboard.
- Subnet: The network that this L3 interface is in, in CIDR notation (ex. 10.1.1.0/24).
- Interface IP: The IP address this switch will use for layer 3 routing on this VLAN/subnet. This cannot be the same as the switch's management IP.
- Multicast support: Enable multicast support if multicast routing between VLANs is required.
- Default gateway (IPv4): When creating the first IPv4 interface on a switch, you will be prompted to enter a default gateway address. This is the next hop IPv4 address of another device on the network, used for any traffic that isn't going to a directly connected subnet or over a static route. This IP address must exist in a subnet with a L3 interface, and will be used for the default route next hop IP address.
- DNS Server: If this interface is selected as uplink, configure DNS servers.
- High Availibility (VRRP): Select if runing VRRP on this routed port.
- DHCP settings: If DHCP on this VLAN should be handled by the switch or forwarded to a server, make the appropriate selections. See the article on Configuring DHCP Services for more details.
- OSPF settings: This VLAN can be distributed via OSPF. See the MS OSPF Overview article for more details.
When complete, click Save changes.
Note: VRRP is not currently supported on routed ports.
Configuring an IPv6 L3 SVI Interface
Note: This feature is only supported on the MS390 and Cloud Managed Catalyst switches.
- Interface name: A friendly name/description for the interface/VLAN.
- VLAN: The VLAN this L3 interface is in.
- Prefix: The IPv6 subnet that this L3 interface is in, in CIDR notation (ex. 2001:db8::/32).
- IPv6 EUI64: Option to use EUI (extended unique identifier) allowing the switch to automatically dervice the interface IPv6 address from the switch's MAC address. This option can only be used if the prefix length is /64.
- Interface IPv6: The IPv6 address this switch will use for L3 routing on this VLAN/subnet. This cannot be the same as the switch's management IPv6 address. If the interace is configure to use EUI64, this option will be disabled.
- Default gateway: When creating the first IPv6 interface on a switch, you will be prompted to enter a default gateway address. This is the next hop IPv6 of a another device on the network, used address for any traffic that isn't going to a directly connected subnet or over a static route. This IP address must exist in a subnet with a L3 interface, and will be used for the default route next hop IP address.
Once created, any layer 3 interfaces or static routes will appear under Switching > Configure > Routing & DHCP.
Note: Each switch can only have a single L3 interface per VLAN.
Configuring an IPv6 L3 Routed Port Interface
Note: This feature is only supported on the MS390 and C9300-M models.
- Interface mode: Select "Routed port".
- Switch or switch stack: Select the switch or switch stack for the routed port.
- Select Module (optional): Select if you have a modular Catalyst switch. If fixed then this will be greyed out.
- Switch ports: Select the routed port interface number.
- Name: A friendly name/description for the interface/routed port.
- VRF: Select the VRF if other than the default.
- IP toggle: Select if this routed port will be IPv4, IPv6, or Both
- Uplink: Select if this routed port will be the uplink interface to Dashboard.
- Prefix: The network that this L3 interface is in, in CIDR notation (ex. 10.1.1.0/24).
- Interface IPv6: The IP address this switch will use for L3 routing on this VLAN/subnet. This cannot be the same as the switch's management IP.
- Multicast support: Enable multicast support if multicast routing between VLANs is required.
- Default gateway (IPv6): When creating the first IPv4 interface on a switch, you will be prompted to enter a default gateway address. This is the next hop IPv4 address of another device on the network, used for any traffic that isn't going to a directly connected subnet or over a static route. This IP address must exist in a subnet with a L3 interface, and will be used for the default route next hop IP address.
- DNS Server: If this interface is selected as uplink, configure DNS servers.
- High Availibility (VRRP): Select if runing VRRP on this routed port.
When complete, click Save changes.
Configuring an L3 Aggregate Routed Port Interface
- 
    Navigate to the Switch Summary page.
- 
    Click on Ports
- 
    Select the ports you want to convert into a L3 aggregate interface and click Aggregate
- 
    Select the Aggregate interface you just created and click Edit
- 
    Select Routed Port for Interace mode and then click Update (this will bring you to the Interface Editor in the Routing & DHCP page to begin configuring the L3 Aggregate interface.
- 
    Name: A friendly name/description for the interface/routed port.
- 
    VRF: Select the VRF if other than the default.
- 
    IP toggle: Select if this routed port will be IPv4, IPv6, or Both
- 
    Uplink: Select if this routed port will be the uplink interface to Dashboard.
- 
    Subnet: The network that this L3 interface is in, in CIDR notation (ex. 10.1.1.0/24).
- 
    Interface IP: The IP address this switch will use for L3 routing on this VLAN/subnet. This cannot be the same as the switch's management IP.
- 
    Multicast support: Enable multicast support if multicast routing between VLANs is required.
- 
    Default gateway (IPv4): When creating the first IPv4 interface on a switch, you will be prompted to enter a default gateway address. This is the next hop IPv4 address of another device on the network, used for any traffic that isn't going to a directly connected subnet or over a static route. This IP address must exist in a subnet with a L3 interface, and will be used for the default route next hop IP address.
- 
    DNS Server: If this interface is selected as uplink, configure DNS servers.
- 
    High Availibility (VRRP): Select if runing VRRP on this routed port.
- 
    DHCP settings: If DHCP on this VLAN should be handled by the switch or forwarded to a server, make the appropriate selections. See the article on Configuring DHCP Services for more details.
- 
    OSPF settings: This VLAN can be distributed via OSPF. See the MS OSPF Overview article for more details.
When complete, click Save changes.
Note: When deleting an L3 aggregate routed port interface from the Routing and DHCP page, the interface and its member ports are converted to an L2 aggregate. You can then convert the aggregate back to an L2 interface, split the member ports to remove the aggregation, or reconfigure the interface as a routed port.
Configuring Static Routes
In order to route traffic elsewhere in the network, static routes must be configured for subnets that are not being routed by the switch or would not be using the default route already configured, such as if another portion of the network was located behind a router or another L3 switch is downstream from the Cisco Meraki L3 switch being configured.
To create a new static route:
- Navigate to Switching > Configure > Routing & DHCP.
- Click Add a static route.
- Select the Switch it should be applied to.
- Provide the following information:
    
  - Name: A friendly name/description for the static route.
  - Subnet: The network that this static route is for, in CIDR notation (ex. 10.1.1.0/24 or 2001:db8::/32).
  - Next hop IP: The IP address of the next L3 device along the path to this network. This address must exist in a subnet with a L3 interface. On switches that support IPv6 static routing, an IPv6 global unicast address can be entered as the next hop IP.
- Click Save or Save and add another if additional static routes are needed.
Note: A default route cannot point to an SVI configured on that same switch. Likewise, avoid configuring any static route with a Next-Hop which is the IP address of another SVI in the same switch. Doing this can create a Layer 3 routing loop and consequent network outages.
Editing an Existing Layer 3 SVI, Routed Port, or Static Route
To modify an existing layer 3 interface or static route on a specific switch:
- Navigate to Switching > Configure > Routing & DHCP.
- Click on the desired Interface or Route.
- Make any desired changes.
- Click Save.
Moving a Layer 3 SVI or Routed Port to Another Switch
To move a layer 3 interface from one switch to another:
- Navigate to Switching > Configure > Routing & DHCP.
- Select the layer 3 interfaces that will be moved.
- Click Edit > Move...
- Select destination switch or switch stack, then click Submit.
Deleting a Layer 3 SVI, Routed Port, or Static Route
In order to delete a layer 3 interface or static route:
- Navigate to Switching > Configure > Routing & DHCP.
- Click on the desired Interface or Route.
- Click Delete Interface/Route, then click Confirm delete.
Note: A switch must retain at least one layer 3 interface and the default route. The default route cannot be manually deleted.
Disabling Layer 3 Routing
In order to disable layer 3 routing, any configured static routes and layer 3 interfaces must be deleted in a specific order.
- Navigate to Switching > Configure > Routing & DHCP.
- Delete any static routes other than the Default route for the desired switch.
- Delete any layer 3 interfaces other than the one which contains the next hop IP for the default route on the desired switch.
- Delete the last layer 3 interface to disable layer 3 routing.
Performing these steps out of order will result in an error and will not allow the route/interface to be deleted.
Layer 3 Interface (SVI) Caveats
Switch Management IP and Layer 3 Interfaces (SVIs)
The management IP is treated entirely different from the SVIs and must be a different IP address. It can be placed on a routed or non-routed VLAN (e.g.: a management VLAN independent from client traffic). Traffic from the management IP address to the Cisco Meraki Cloud Controller will not use the layer 3 routing settings; instead, it will be using its configured default gateway. Therefore, it is important that the IP address, VLAN, and default gateway configured in your switch management IP can still provide connectivity to the internet independently from the switch's own L3 routing settings.
The Switch (or Stack) management IP configuration cannot have Gateway address defined as one of its own SVI address when it is performing Layer 3 routing. It will not be able to check in using the assigned management IP when the gateway is pointed to itself. For example, if 192.168.1.1 is one of the L3 interfaces (SVI) on a switch (or stack), you cannot have 192.168.1.1 as the gateway for its management IP (Switching > Switches > LAN IP).
For switch stacks performing L3 routing, ensure that the management IP subnet does not overlap with the subnet of any of it's own configured L3 interfaces. Overlapping subnets on the management IP and L3 interfaces can result in packet loss when pinging or polling (via SNMP) the management IP of stack members.
Note: The overlapping subnet limitation does not apply to Catalyst switches (MS390/C9300-M) running IOS XE firmware version.
Pings Destined for a Layer 3 Interface
MS Switches with Layer 3 enabled will prioritize forwarding traffic over responding to pings. As a result, packet loss and/or latency may be observed for pings destined to an SVI address. Therefore, it's recommended to ping another device in a given subnet to check network stability and reachability.
Note: Meraki MS classic switches (excluding MS390) are unable to ping their own Layer 3 Interface.