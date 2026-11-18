Meraki Secure SD-WAN Microsoft SSE Configuration Guide
Microsoft’s SSE Meraki SD-WAN Configuration Guide
The integration of Cisco Meraki Secure SD-WAN with Microsoft's SSE solution facilitates inspection of north-south traffic originating from SD-WAN branches destined for the internet or Software-as-a-Service (SaaS) applications routed through Microsoft's SSE solution.
This guide details the process of securing Microsoft's SSE solution with Cisco Meraki Secure SD-WAN networks, specifically for internet and SaaS applications. The integration has undergone extensive testing and validation for deployment on Cisco Meraki version… in conjunction with the Microsoft's SSE solution cloud dashboard.
Microsoft Entra Internet Access traffic, alongside Microsoft Entra Access, are integral components of Microsoft's SSE solution: Global Secure Access. Microsoft Entra Internet Access ensures secure access to internet and SaaS apps, providing robust protection for users, devices, and data against internet-borne threats. This document focuses on the Internet Access use case.
There are multiple ways to connect remote networks to Global Secure Access. In a nutshell, you're creating an Internet Protocol Security (IPSec) tunnel between a core router, known as the customer premises equipment (CPE), at your remote network and the nearest Global Secure Access endpoint. All internet-bound traffic is routed through the core router of the remote network for security policy evaluation in the cloud. A key customer benefit is the seamless deployment of a comprehensive end-to-end SD-WAN and security solution where the installation of a client isn't required on individual devices. In addition, customer also benefit from enhanced security capabilities such as universal tenant restrictions, compliant network check and source IP restoration.
Prerequisites
- 
    Microsoft’s SSE solution account
- 
    Meraki MX/Z device (running MX19.1+ firmware)
- 
    BGP routing over IPsec (IKEv2 required)
- 
    BGP port TCP 179 is permitted on your VPN Firewall 
Caveats
- 
    This integration only applies to tunneling Microsoft 365 traffic
- 
    Only NULL encryption is supported for Phase 2 IPsec encryption settings
- 
    Configuration templates are not supported with this integration
- 
    User FQDN is not supported on Microsoft’s SSE solution
- 
    Zone Redundancy configuration via Microsoft’s SSE solution is not supported. To achieve Zone redundancy, see Redundant tunnel section
- 
    eBGP routes are redistributed into iBGP by default, this could lead to sub optimal routing for a remote AutoVPN peer. 
Configuration of Microsoft’s SSE solution
How to create remote networks - Global Secure Access | Microsoft Learn
- 
    Navigate to entra.microsoft.com and login with your credentials 
- 
    On the left pane, navigate to Global Secure Access > Connect > Remote Networks 
 
 
- 
    On the top left, click + Create remote Network 
 
 
- 
    Under Basics tab, configure a remote network Name and select a Region. Select a region closest to your remote network. Then click next: Connectivity. 
 
 
- 
    Under Connectivity tab, click + Add a link
5a. Under General - configure the following
|          |  | 
| 5b. Under Details - Configure the following    Phase 1 settings   Phase 2 Settings     |  | 
| 5c. Under Security - configure the following   5d. Save your configuration |  | 
5e. After your configuration has been saved, the created link will be listed under connectivity as seen below:
 
Click Next: Traffic profiles 
 
 
6. Under the Traffic profiles tab,  
Select the checkbox for Microsoft 365 traffic profile, and click Next: Review + create
 
7. Under the Review + create tab
Review your settings and click Create remote network
 
The newly created remote network will be listed on the Remote Network page. 
 
The ”view configuration link” under connectivity details can be used to view configurations needed to configure the Meraki Secure SD-WAN Firewall at my branch location.  
The local configuration is what I need to configure my Meraki Secure SD-WAN device. 
Meraki Secure SD-WAN Configuration
On the Meraki Network, Navigate to Site-to-site VPN settings through the Security & SD-WAN > Configure > Site-to-site VPN page.
There are three options for configuring the MX-Z's role in the Auto VPN topology:
- 
    Off: The MX-Z device will not participate in site-to-site VPN.
- 
    Hub (Mesh): The MX-Z device will establish VPN tunnels to all remote Meraki VPN peers that are also configured in this mode, as well as any MX-Z appliances in hub-and-spoke mode that have the MX-Z device configured as a hub.
- 
    Spoke: This MX-Z device (spoke) will establish direct tunnels only to the specified remote MX-Z devices (hubs). Other spokes will be reachable via their respective hubs unless blocked by site-to-site firewall rules. 
- 
    You can configure a VPN peering to Microsoft’s SSE solution by clicking "Add a peer" button
 
You can configure a VPN peering to Microsoft SSE by clicking "Add a peer" button enter the following information:
|   Peers   |  | 
|        |  | 
| IPsec policy Phase 1 settings      Phase 2 Settings      |  | 
Once all configurations' parameters have been filled, click Add, at the button right corner of the sider drawer.
Now your peer has been configured.
The corresponding eBGP peer will show up grayed out on the Security & SD-WAN > Routing page, and can only be removed when the peer is deleted via the Site-to-Site VPN page.
Verifying connectivity to Microsoft’s SSE solution
The connectivity to Microsoft SSE starts with an IPsec tunnel, and then an eBGP peering relationship. Hence, to verify connectivity, first verify that the IPsec tunnel is up. We can verify by navigating to Security & SD-WAN > VPN Status – IPsec VPN tab.
Here we can see the status of the IPsec tunnel.
| Indicator | Meaning | 
| Green | Phase 1 and phase 2 are up (IPsec connectivity is up) | 
| Amber | Phase 1 is up but phase 2 is down | 
| Red | Phase 1 and phase 2 are both down. To troubleshoot, see troubleshooting IPsec peer guide | 
A Green indicator means the IPsec tunnel is up.
Next, we navigate to the Dynamic protocol status page to see if the eBGP peering relationship with Microsoft SSE solution is up. An Established peer status indicates that the BGP neighbor relationship is established, and the routes column indicates how many routes have been learned from the BGP neighbor. 
The exact routes learned can viewed on the Security & SD-WAN > Route table page. 
Redundant tunnels
 
Meraki Secure SD-WAN native primary & backup tunnels are not supported with BGP over IPsec enabled tunnels, to achieve redundancy to Microsoft SSE, simply create two IPsec peering to a different Microsoft SSE zone and manipulate BGP attributes to give priority to one tunnel over the other. 
Redundant tunnels can be created to provide availability in the following failure scenarios.
Microsoft SSE Primary DC failure
 
    To avoid the impact of a potential failure of Microsoft’s SSE solution DC failure, it is recommended to create one connection for each two different     Microsoft’s SSE solution DCs. Then manipulate BGP attributes to give preference to one Microsoft SSE DC over the other. The Weight, MED (Multiple Exit     Discriminator) or Path prepend fields under the routing section of the IPsec peer configuration can be used to prefer BGP routes from one peer over     another.  
- 
    For example, the default Weight for BGP peers is 0, setting the Weight of one Microsoft’s SSE DC 10, gives a higher priority to the routes advertised by the zone A over another zone with a default Weight of 0.
- 
    For symmetric return traffic, use AS path prepending ensure Microsoft SSE knows the primary tunnel to return traffic. For example, the path prepending field is empty by default, on the backup tunnel, add e.g. 64550 64550 to increase hop count seen by Microsoft SSE peer, this will ensure the tunnel with the least hops will be preferred. 
| Primary Microsoft SSE tunnel | Backup Microsoft SSE tunnel | 
- 
    WAN connectivity failure 
To avoid the impact of a potential WAN failure, it is recommended to have dual WAN links on your Secure Meraki SD-WAN device and configure an additional link under the connectivity tab. If WAN1 fails, connectivity to Microsoft’s SSE solution is established on WAN2. The tunnel over the WAN2 link will not establish until WAN1 fails on the MX Secure SD-WAN device.
- 
    Device failure 
To avoid the impact of device failure, it is recommended to deploy your Meraki Secure SD-WAN in a High Availability configuration. See MX Warm Spare guide to learn more about High availability.
This Topology and configuration highlights how to deploy a fully redundant connection to Microsoft’s SSE solution.
 
 
WAN 1 Active primary and secondary tunnels are independent tunnels with BGP attributes configured to prefer one peer over the other. The WAN 2 inactive tunnels become active when WAN1 connection fails.  
The Single device Dual WAN topology addresses DC failure and WAN connectivity failure, while the High Availability with Dual WAN virtual IP addresses DC failure, WAN connectivity failure and device failure.
Single device Dual WAN topology configuration
On MSFT Entra,
- 
    Configure two remote networks
- 
    Configure with two links per remote network pointing to each WAN IP 
On Meraki Dashboard,
- 
    Configure higher BGP Weight for preferred MSFT DC
- 
    Increase AS path for backup tunnel to MSFT DC to ensure symmetric routing
High Availability with Dual WAN virtual IP configuration
On MSFT Entra,
- 
    configure two remote networks
- 
    Configure with two links per remote peer pointing to each WAN IP
On Meraki Dashboard,
- Configure MX in Warm spare mode
- Configure virtual IP
- Configure higher BGP Weight for preferred MSFT DC
- Configure lower MED value for preferred MSFT DC to ensure symmetric routing
IPsec VPN Firewall
BGP uses TCP port 179, this means that TCP for 179 needs to be permitted through the VPN firewall to allow BGP communication between configured peers.
You can add firewall rules to control what traffic is allowed to pass through the VPN tunnel. These rules will apply to outbound VPN traffic to/from all MX-Z appliances in the Organization that participate in site-to-site VPN. These rules are configured in the same manner as the Layer 3 firewall rules described on this documentation. Note that VPN Firewall rules will not apply to inbound traffic or to traffic that is not passing through the VPN.
Serviceability
Event Logs
If you have any issues or would like to know more about the Cisco Secure Access peering details, navigate to Network-wide > Monitor > Event log
- 
    Select Event type Include - Non-Meraki VPN Negotiation
Packet Captures
The following options are available for a packet capture on MX/Z platforms:
- 
    Appliance: The appliance the capture will run on.
- 
    Interface: Select the interface to run the capture on; the interface names will vary depending on the appliance configuration. A few examples of interfaces you may see are:
- 
    Internet 1 or Internet 2 - Capture traffic on one active WAN uplink. Internet 2 will only appear if there is a second WAN link.
- 
    LAN - Captures traffic from all LAN ports
- 
    Cellular - Captures cellular traffic from the integrated cellular interface. This does not apply to USB modems.
- 
    Site-to-Site VPN - Captures AutoVPN traffic (MX/Z to MX/Z only). This does not apply to IPsec VPN peers.
- 
    Output: Select how the capture should be displayed; view output or download .pcap.
- 
    Verbosity: Select the level of the packet capture (only available when viewing the output directly to Dashboard).
- 
    Ignore: Optionally ignore capturing broadcast/multicast traffic.
- 
    Filter expressions: Apply a capture filter.
To capture packets, select the WAN interface and use the filter expressions for UDP 500 for Phase 1 or UDP 4500 for Phase 2.
API
The Meraki dashboard API is an interface for software to interact directly with the Meraki cloud platform and Meraki-managed devices. The API contains a set of tools known as endpoints for building software and applications that communicate with the Meraki dashboard for use cases such as provisioning, bulk configuration changes, monitoring, and role-based access controls. The dashboard API is a modern, RESTful API using HTTPS requests to a URL and JSON as a human-readable format. The dashboard API is an open-ended tool that can be used for many purposes.
For more information, read here.
24/7 Support
Cisco Meraki Support is available 24/7 to Enterprise customers for assistance with resolving network issues and providing answers to questions not covered by the documentation. For more information, read here.