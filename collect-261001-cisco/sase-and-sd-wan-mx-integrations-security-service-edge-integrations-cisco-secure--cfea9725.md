Cisco Secure Access Meraki SD-WAN Configuration Guide
Cisco Secure Internet Access Configuration Guide
Cisco Secure Access offers a security stack solution from the cloud for internet, SaaS, ZTNA, Remote access connections, etc. Cisco Secure Access acts as a security gateway where 0.0.0.0/0 traffic will be routed for inspection and enforcement prior to internet or site-to-site, private cloud termination.
This document covers configuration for Secure Internet access with Primary & Secondary static tunnels.
Meraki SDWAN + Secure Access Design Guide
Private Access BGP Configuration Guide
Prerequisites
- 
    Cisco Secure Access account
- 
    Meraki MX/Z device (running MX19.1.6+ firmware)
- 
    Meraki MX/Z Site-to-site VPN enabled
Caveats and Considerations
- 
    ECMP/Load balancing is not supported with this integration
Cisco Secure Access Configuration
Go to sse.cisco.com and login with your credentials and follow the steps outlined below.
1. Add a Network Tunnel Group - From the Secure Access console, navigate to Connect > Network Connections.
 
- 
    Select Network Tunnel Groups
- 
    Click Add. 
- 
    Enter the General Settings for your tunnel group:
- 
    Give your tunnel group a meaningful name.
- 
    Choose a Region.
- 
    Choose a Device Type.
- 
    Click Next.
- 
    Enter the Tunnel ID and Passphrase for your tunnel group:
- 
    Tunnel ID Format: Email.
- 
    Enter Passphrase
- 
    Re Enter Passphrase.
- 
    Click Next.
Note: The passphrase must be between 16 and 64 characters in length and contain at least one upper case letter, one lower case letter, and one number. The passphrase cannot include any special characters. 
 
4. Routing - choose Enable NAT / Outbound Only (for Secure Internet Access use case)
Static Routing configuration in Secure Access is not supported with Meraki SD-WAN. In Secure Access set Routing to "Enable NAT / Outbound Only" for Secure Internet Access use cases, and Dynamic Routing for Private Access use cases. Refer to the Design guide for supported Design topologies and configurations.
- 
    Click Save.
- 
    On the Data for Tunnel Setup page, review the network tunnel information for completeness. Click the Download CSV button to save configuration information needed for your Meraki Secure SD-WAN device.
Note: This is the only time that your passphrase is displayed.
- 
    Click Done.
A Network Tunnel Group will be configured but the Primary and Secondary Hub will be down until a tunnel is established to the Hubs.
Meraki Secure SD-WAN Configuration
In an AutoVPN and IPsec VPN environment, when configuration changes are applied on a Hub to a subnet that participates in the VPN, VPN connections will reset on every site connected to the Hub to update the VPN configuration.
Gather Details from Cisco Secure Access Portal
Information can be taken from the CSV downloaded from step 5 above or from the Data for Tunnel Setup page seen below:
On the Meraki Network, Navigate to Site-to-site VPN settings through the Security & SD-WAN > Configure > Site-to-site VPN page.
There are three options for configuring the MX-Z's role in the Auto VPN topology:
- 
    Off: The MX-Z device will not participate in site-to-site VPN.
- 
    Hub (Mesh): The MX-Z device will establish VPN tunnels to all remote Meraki VPN peers that are also configured in this mode, as well as any MX-Z appliances in hub-and-spoke mode that have the MX-Z device configured as a hub.
- 
    Spoke: This MX-Z device (spoke) will establish direct tunnels only to the specified remote MX-Z devices (hubs). Other spokes will be reachable via their respective hubs unless blocked by site-to-site firewall rules.
Select Hub (Mesh) or Spoke depending on your AutoVPN requirements, to enable VPN.
Configure Layer 7 Health Checks
Click on the Configure health check button as shown in the following image to start the process.
Next, configure a health check name and endpoint. Meraki Secure SD-WAN will probe the configured endpoint to track connectivity over the tunnel. The endpoint hostname will be resolved over the local WAN uplink with the DNS server IP configured on the WAN interface.
Next, create a new IPsec peer by clicking on the "Add peer" button on the IPsec VPN peers table
You can create Site-to-site VPN tunnels between a Security Appliance or a Teleworker Gateway and Cisco Secure Access under the IPsec VPN peers section on the Security & SD-WAN > Configure > Site-to-site VPN page.
Enter the following information:
- 
    Name—Provide a meaningful name for the tunnel.
- 
    IKE Version—Select IKEv2.
- 
    IPsec policies—Choose the predefined Umbrella configuration, see Supported IPsec Parameters.
- 
    Public IP—IP address to connect to Secure Access Network Tunnel Group Primary Data Center IP
- 
    Local ID—The Primary Tunnel ID for the Network Tunnel Group.
- 
    Remote ID—Leave this blank
- 
    Preshared secret—The Passphrase for the Network Tunnel Group created in Secure Access
- 
    Routing — Static, configure a default route 0.0.0.0/0
- 
    Availability— Add network tag for the MX network appliance that should build the tunnel to Secure Access. "All Networks" tag will configure all MXs in your Dashboard Organization to establish a tunnel to Secure Access
- 
    Tunnel Monitoring 
  - 
        Health check - select previously configured heath check. This endpoint will be used to monitor connectivity through the tunnel
- 
        
For IPSec configurations utilizing static routing (0.0.0.0/0) and health checks, health checks are currently limited to MX-to-single-headend architectures. This means that health checks will not influence routing failover as only one MX-to-headend health check will work as expected. We are expecting to address this in future firmware releases. Please note that this limitation does not apply to Meraki AutoVPN Secure Access deployments, which fully support these configurations.
Please see here for further detail: https://securitydocs.cisco.com/docs/...lh/121311.dita 
- 
    
  - 
        Failover directly to Internet - Enable if you prefer traffic to failover to the local Internet uplink when both primary and secondary tunnels are marked as failed.
- 
        
- 
    Click Update & Save
Creating a Secondary or Backup Tunnel
Secondary tunnels can be enabled to provide redundancy if the primary tunnel fails.
To create a Secondary tunnel, navigate to the Primary tunnel you want to create a secondary tunnel for, and click on the dotted … menu to right of the peer, and click on Add option under Secondary. This will open a dedicated side drawer for the Secondary peer.
Configuring a secondary peer is the same as configuring a primary one. The inherit primary peer configuration makes it easy to inherit primary settings and minimizes error during configuration. IKE version, routing, private subnets and availability will be automatically inherited from the primary and set to read-only.
Save configuration.
Multi-Uplink Active-Active IPsec
This feature is mainly for Secure Internet use case with SASE/SSE providers. Considering traffic will be load balanced across all healthy tunnels, it’s important to ensure that the remote SASE/SSE peer sends the return traffic back through the tunnel it was received.
When Multi-Uplink IPsec is enabled, IPsec tunnels are established on all available uplinks, and traffic intelligently load-balanced across all healthy tunnels, providing both performance gains and built-in redundancy. If any tunnel becomes unavailable, traffic seamlessly redistributes across the remaining active connections.
To enable Multi Uplink IPsec:
- Create or edit an IPsec peer
- Enable Mutli-Uplink IPsec by toggling on Multi-Uplink IPsec VPN option on the peer side drawer as shown below.
Verification and Troubleshooting
In an AutoVPN and IPsec VPN environment, when configuration changes are applied on a Hub to a subnet that participates in the VPN, VPN connections will reset on every site connected to the Hub to update the VPN configuration.
The Network Tunnel Group will move from Disconnected Status to Connected. This change could take a few minutes.
- 
    Run ping tests from a tunnel enabled VLAN to the internet. For more information, see Using the Ping Live Tool.
- 
    Check the status of the VPN tunnel. For more information, see VPN Status Page.
- 
    Follow the VPN troubleshooting procedures. For more information, see Site-to-site VPN Troubleshooting.
IPsec VPN Firewall
You can add firewall rules to control what traffic is allowed to pass through the VPN tunnel. These rules will apply to outbound VPN traffic to/from all MX-Z appliances in the Organization that participate in site-to-site VPN. These rules are configured in the same manner as the Layer 3 firewall rules described on the Firewall Settings page of this documentation. Note that VPN Firewall rules will not apply to inbound traffic or to traffic that is not passing through the VPN.
Serviceability
Event Logs
If you have any issues or would like to know more about the Cisco Secure Access peering details, navigate to Network-wide > Monitor > Event log
- Select Event type Include - IPsec VPN Negotiation
- IPsec VPN Healthcheck event type can be used to identify the recently reported healthcheck status of the tunnels
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