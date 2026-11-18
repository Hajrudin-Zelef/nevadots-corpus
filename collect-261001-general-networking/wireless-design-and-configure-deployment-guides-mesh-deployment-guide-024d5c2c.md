Mesh Deployment Guide
A key factor when designing a wireless network is whether Cisco Meraki’s unique mesh networking capabilities will need to be used. Mesh networks allow several access points to wirelessly share a single Internet connection. There are several key terms and guidelines that should be understood to ensure successful deployment of a Meraki mesh network. Wireless Mesh Networking is good a reference outlining the main components, algorithm and the monitoring tools available.
Mesh Network Components
Site Preparation and Planning
Site Survey
We recommend that you perform a radio site survey before installing the equipment. A site survey reveals characteristics such as interference, the Fresnel zone, and logistics problems. A proper site survey involves temporarily setting up mesh links and taking measurements to determine whether your antenna calculations are accurate. A site survey helps determine the correct location and antenna to use before drilling holes, routing cables, and mounting equipment.
We recommend the use of an uninterruptible power supply (UPS) to temporarily power the mesh link when power is not readily available.
Antenna Selection
Selecting the appropriate external antenna for each access point in the mesh design is an important part of the design process. There are a number of external antenna options available from Meraki. In most cases, antennas with very narrow beam-widths are ideal for backhaul connections spanning long distances. The datasheet for the antenna will define the Half Power Beam-Width (HPBW) and the gains of the antenna. The information on the datasheet will help identify the best antenna for the application, however, it is recommended to have a Meraki Partner assist and verify the RF design of the mesh network.
Maximize RF Line of Sight
A wireless signal travels most effectively through open space. As such, an AP with an omni-directional antenna should be positioned to maximize its line of sight both to wireless users and to all necessary coverage areas. "RF line of sight" is different than "visual line of sight" because of how RF propagates though the air.
Fresnel Zones
A Fresnel zone is an imaginary ellipse encompassing the visual line of sight between the transmitter and receiver. As radio signals travel through free space to their intended target, signals could encounter an obstruction in the Fresnel zone, degrading the signal. Best performance and range are attained when there is no obstruction in the Fresnel zone. Fresnel zone, free space loss, antenna gain, cable loss, data rate, link distance, transmitter power, receiver sensitivity, and other variables play a role in determining the throughput and reach of your mesh link. Functional links can still be made as long as 60% to 70% of the Fresnel zone is unobstructed, as illustrated in the figure below. A Meraki Partner can help assess a network design for appropriate clearance of the Fresnel zone.
Weather
Weather can degrade a mesh link along with free space loss and line of sight. Rain, snow, fog, and any high humidity condition can slightly obstruct or affect the line of sight. Weather may introduce a small loss (sometimes referred to as rain fade or fade margin) which can have a small effect on the mesh link. If you have established a stable mesh link, then the weather should not be a problem. If the link is poor to begin with, however, bad weather can degrade performance or cause loss of link.
For instance, a white-out snow storm does not allow for line of sight between gateway and repeater access points. Rain or snow itself may appear to be the main problem during a storm, however, other conditions caused by the adverse weather may cause issues too. For example, if an antenna is on a mast pipe and the storm is blowing the mast pipe or antenna structure, that movement could cause the link to come and go. As another example, there could be a large build-up of ice or snow on the antenna causing link problems.
Typical Deployment Models
Point-to-Point
In the point-to-point mesh scenario, one gateway access point provides access to one repeater access point. By default, the repeater access point will broadcast the same SSIDs as the gateway access point. Access point tags can be used to disable specific SSIDs on the repeater access point; Using Tags to Broadcast SSIDs from Specifc APs has more information about this. The repeater access point can extend an Ethernet segment from the gateway access point at the same time as providing wireless access to clients. Further discussion on Ethernet bridging can be found in Ethernet Bridging below.
Applications for which this deployment scenario is appropriate include:
- 
    Bridge link between a main office and a remote temporary office
- 
    Extend the reach of infrastructure-based wireless network at a campus to less critical, hard-to-reach areas like sports fields or parking lots
- 
    Extend a secure network from an industrial core to a non-mission-critical temporary roadhouse or satellite building
Point to Multi-Point / Multi-Point to Multi-Point
In the point to multi-point scenario, one or more gateways serve multiple repeaters. All gateways and repeaters may broadcast the same SSIDs and serve wireless clients.
Applications for which this deployment scenario is appropriate include:
- 
    Provide backhaul for multiple temporary buildings
- 
    Reach remote wireless access point clusters
- 
    Cover low bandwidth subscribers over a large area
Any gateway may be used by a mesh repeater in the same Meraki dashboard network. A separate network may be used if more control is required.
Access Point Capabilities
All Meraki access points support wireless meshing without any additional configuration or licensing. The most successful mesh deployments undergo both pre-deployment design and post-design testing phases.
Each access point has unique mesh characteristics independent of general technology specifications listed on the datasheet. A Meraki Systems Engineer is the best resource to ensure that an access point's capabilities match the desired design targets. The Meraki Trial Program allows customers to run a real world proof of concept to test the design. Reaching out to a Meraki account representative to discuss the design with a Meraki Systems Engineer and to arrange a trial for a proof of concept is considered best practice.
Staging access points before deployment is recommended to ensure that they update to the latest firmware and download the proper configuration from dashboard. Firmware and configuration updates may occur over the mesh network after deployment.
Deployment Considerations
The guidelines below should be followed to ensure a successful mesh deployment.
Wireless Client Handling on a Repeater
VLANs and other SSID options (such as firewall rules and traffic shaping) will be maintained on SSIDs that are broadcast by repeater access points.
Gateway-to-Repeater Ratio
In general, having as many gateway access points as possible to maximize overall network performance and reliability is desired. It is recommended to plan for no more than two repeater access points attached to each gateway access point.
Maximum Mesh Hops
There will be a throughput reduction (about 50%) with each “hop” in a mesh. It is recommended that a mesh network be designed for no more than one mesh hop from the gateway access point to client device.
Multi-Path Mesh
It is recommended that each mesh access point has at least three strong “neighbors,” or other access points in the mesh that the access point can “see,” so that multiple failover paths exist. Wireless Mesh Networking gives an overview of the monitoring tools available on dashboard, including information about the mesh neighbors table found on individual access point detail pages.
Ethernet Bridging
The gateway access point may be connected to a trunk port and trunk SSIDs to different VLANs. Repeater access points will broadcast SSIDs trunked on different VLANs too. Only one SSID and associated VLAN, however, may be configured to bridge wired clients across a mesh link on a repeater access point's Ethernet port. A mix of wired clients and Meraki access points attached to one repeater access point Ethernet port is not a supported deployment configuration. Meraki access points use auto detection mechanisms to infer when they should function as a gateway or a repeater, which is why a mix of wired clients and Meraki access points is not allowed.
Mesh wired access may be treated like a traditional point-to-point link with a router on the remote site. Meraki access points may be connected to the repeater side when a Layer 3 device is separating the broadcast domains. For further discussion on this design, see Extending the LAN with a Wireless Mesh Link.
Wired clients are not subject to the same authentication requirements that wireless clients are subject to. Wired clients will bypass authentication methods such as PSK and RADIUS and gain network connectivity as though they had associated to the SSID.
Wireless Mesh Data Rate
Wireless data rate selection is an important mechanism for effective use of the available RF spectrum. The data rate can affect the throughput of clients. Throughput is an important metric used by industry publications to evaluate vendor devices.
Dynamic Rate Adaptation (DRA) introduces a process to estimate the optimal rate for packet transmissions. Correctly selecting rates is important as a too high rate leads to packet transmissions failing, which leads to communication failure. If the rate is too low, the available channel bandwidth is not used efficiently, creating the potential for network congestion and collapsed links. Meraki access points use a customized, fully-automatic DRA algorithm when establishing mesh links.
Frequency/Radio Usage
Any channel that a Meraki access point is permitted to operate on can be used for a mesh link. The channel availability for a particular access point model is subject to regulatory domain restrictions and certification. Meraki access points do not prefer one band over another, as described in the Wireless Mesh Networking "Meraki Mesh Algorithm" section. Both radios on a Meraki access point may be used concurrently for meshing while also serving wireless clients.
Any given mesh backhaul link will only use one radio, either 2.4 or 5 GHz, but not both concurrently. The gateway and/or repeater access point can serve clients and provide mesh connectivity. It is recommended not to use mesh links/radios to serve clients but rather have them remain dedicated for mesh to help maximize performance.
An access point in repeater mode may not always honor manual channel settings. More details on manually influencing the mesh channel can be found in Manually Changing Channels in a Mesh Network.
DFS Recommendation
It is recommended to avoid Dynamic Frequency Selection (DFS) channels using the "Exclude DFS channels" option on the Wireless > Configure > Radio settings page for greatest reliability. A DFS event will cause access points to silence communication on the affected channel and temporarily move to another channel as described in Dynamic Frequency Selection.
Mesh Convergence Time
The Meraki mesh algorithm is designed for stationary Meraki access points with variable links. The algorithm sends out periodic discovery frames of varying sizes on the 2.4 GHz and 5 GHz data radios to discover mesh neighbors. More discussion on neighbor discovery and gateway selection can be found in Wireless Mesh Networking. After one neighbor is selected as the gateway, new routes are passively identified on the same operating channel. A mesh link may take a couple minutes to establish and may not always be used for every data flow if a new route with better link metrics becomes available.
The Meraki mesh algorithm is not optimized for access points that are moving.
Multi-VLAN Support Over Mesh
Meraki APs will allow traffic from multiple VLANs over mesh links. This feature can be enabled by contacting support. There are a couple of conditions that must be met to support this functionality:
1. Clients wired directly into Meraki access points needs to be enabled and configured for a specific SSID where multiple VLANs are used. This option is found on the Network Wide > Configure > General page.
2. SSID configuration has to use Bridge mode. This option is found on the Wireless > Configure > Access Control page, Client IP assignment section.
Multi-VLAN support over Mesh is supported with MR 28.1 and higher firmware versions.
If you plan to have one or more Meraki access points behind a repeater access point, a layer 3 device needs to separate them as stated in Extending the LAN with a Wireless Mesh Link.
Multicast Over Mesh
Multicast protocols, including CDP, LLDP, VTP, etc., are not supported over a wireless mesh link.