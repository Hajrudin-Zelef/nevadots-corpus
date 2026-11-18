How to Configure Meraki Device Reporting via Syslog, SNMP, and API
Click 日本語 for Japanese
Learn more with these free online training courses on the Meraki Learning Hub:
Overview
This article explains how to configure the three device reporting methods available on the Cisco Meraki dashboard: Syslog, API (including Webhooks), and SNMP. Aside from the Meraki Event Log available on the dashboard, these methods let you gather device information and events for reporting and monitoring purposes.
Each method offers distinct benefits for network and device reporting. Use the comparison below to choose the reporting method that best fits your use case.
Choosing a reporting method
The following list compares the capabilities supported by each reporting method.
| Network Event Information | Syslog | API / Webhooks | SNMP | 
|---|---|---|---|
| Device Flows | X |  |  | 
| Client Connectivity |  | X | X | 
| Configuration Changes |  | X | X | 
| Real-time information gathering | X | X | X | 
| Real-time network statistics | X | X | X | 
| Device information gathering | X | X | X | 
| Detailed device statistics |  | X | X | 
| Network-wide information gathering | X | X | X | 
| Organization-wide information gathering |  | X | X | 
| Proactive alerts for critical events |  | X | X | 
| Automation capabilities |  | X | X | 
| Application integration |  | X |  | 
| Cloud-centric |  | X |  | 
| Scalability |  | X |  | 
Step-by-step instructions
Configure a Syslog server
A syslog server stores messages for reporting from MX WAN appliances, MR access points, and MS switches.
Supported message roles by device:
- MX WAN Appliance supports four roles: Event Log, IDS Alerts, URLs, and Flows.
- MR access points support the same roles except IDS Alerts.
- MS switches currently support Event Log messages only.
Add the syslog server
- 
    Navigate to Network-Wide > Configure > General.
- 
    Locate the Reporting section, which contains the Syslog server configuration.
- 
    Select Add a syslog server to define a new server.
- 
    Configure the IP address of your syslog server, the UDP port the server listens on, and the roles you want reported to the server.
In Appliance-only, Switch-only, and Wireless-only dashboard networks, syslog servers appear under the Logging section instead of Reporting.
Encrypted (TLS) syslog will be available for configuration at a future date.
Enable firewall rule logging (optional)
If you enable the Appliance Flows role for Meraki MX reporting, you can enable or disable logging for individual firewall rules.
- 
    Navigate to Security & SD-WAN > Configure > Firewall.
- 
    Set logging under the Logging column.
Configure API reporting
Meraki devices support API calls to gather statistics and other information from your networks. The dashboard API is a powerful, flexible, open-ended tool for many reporting use cases.
Generate a Dashboard API key
The Meraki dashboard API is enabled by default on all organizations. The API key associates with the dashboard administrator account that generates it and inherits that account's permissions. You can generate, revoke, and regenerate your API key on your profile.
- 
    Select the avatar icon in the top right-hand corner of the dashboard, then open the My Profile page.
- 
    Select Generate new API key. A window displays your unique API key.
- 
    Record the API key immediately. Once this window closes, you cannot view the full key on the Meraki dashboard again.
- 
    After copying the key, select the I have stored my new API key checkbox.
- 
    Select Done.
The page refreshes. Under the API access section, only the last 4 digits of the key display for security.
Only you can view your unique API key. No one else with access to the Meraki dashboard—including Meraki Support—can view your API key.
API endpoints for device and network reporting
The following endpoints support device and network reporting:
| API Endpoint | API Call | Description | 
| Client Security Events | Get Network Appliance Client Security Events | Gathers data on all security events for a specified dashboard network | 
| Clients | Get Device Clients | Gathers data on all client devices connected to specific Meraki device, up to a maximum of one month | 
| Devices | Get Network Device Loss and Latency History | Displays data on uplink loss percentage and latency (in milliseconds) for Meraki security appliance | 
| Devices | Get Network Device Performance | Returns the performance score for a specified device. (Only MX security appliances are supported for this API call at this time) | 
| Devices | Get Network Device Uplink | Returns the current uplink information of a specified device. | 
| Devices | Get Network Devices | Lists all devices in the specified network | 
| MV Sense | Get Device Camera Analytics Live | Returns live state form camera of analytics zones configured | 
| MV Sense | Get Device Camera Analytics Overview | Returns overview of aggregate analytics data for timespan specified | 
| MV Sense | Get Device Camera Analytics Recent | Returns the most recent record for analytics zones configured | 
| MV Sense | Get Device Camera Analytics Zone History | Returns historical records for analytic zones configured | 
| MV Sense | Get Device Camera Analytics Zones | Returns all configured analytic zones for specified camera | 
| Networks | Get Network Air Marshal | Lists the Air Marshal scan results for the specified network | 
| Networks | Get Network Traffic | Gathers traffic analysis data for the specified network | 
| Organizations | Get Organization Device Statuses | Lists the current status is every Meraki device in the specified Organization | 
| Organizations | Get Organization License State | Returns the license state for the specified Organization | 
| Organizations | Get Organization Uplinks Loss and Latency | Returns the uplink loss and latency measurements for every MX in the Organization. (From 2 - 7 minutes ago) | 
| Splash Login Attempts | Get Network Splash Login Attempts | Lists the number of login attempts for a splashpage on the specified network | 
| Wireless Health | Get Network Clients Connection Stats | Gathers aggregated connectivity information for wireless clients in the specified network | 
| Wireless Health | Get Network Clients Latency Stats | Gathers aggregated wireless latency for clients on the specified network | 
| Wireless Health | Get Network Connection Stats | Gathers aggregated connectivity data for the specified network | 
| Wireless Health | Get Network Devices Connection Status | Lists client connectivity data on a per-AP basis for the specified network | 
| Wireless Health | Get Network Devices Latency Stats | Lists the amount of wireless latency data on a per-AP basis for the specified network | 
| Wireless Health | Get Network Failed Connections | Lists all failed client connection events on the specified network based on time frame given | 
| Wireless Health | Get Network Latency Stats | Lists aggregated wireless latency information for the entirety of the specified network | 
To learn more about using API with the Meraki dashboard, please visit Cisco Meraki DevNet at https://developer.cisco.com/meraki/
Configure Meraki Dashboard Webhooks
Meraki Webhooks are a lightweight way to subscribe to alerts sent from the Meraki Cloud when an event triggers a configured dashboard alert. They include a JSON-formatted message sent to a unique URL, where you can process, store, or use them to trigger automations. This solution provides a rapid way to set up a hosted service that receives and stores webhook alert data.
Webhooks support all configurable alert types available under Network-wide > Configure > Alerts, including alerts for all product types you own or operate. The webhooks architecture consists of the Meraki cloud and a cloud-accessible HTTP or HTTPS receiver (server). Several standalone cloud webhook services (for example, hook.io and Zapier) can receive or forward webhooks.
Set up Webhooks on the dashboard
- 
    Navigate to Network-Wide > Configure > Alerts.
- 
    Locate the Webhooks section.
- 
    Select Add an HTTP server and configure the server URLs.
- 
    Specify the HTTP servers as recipients for dashboard alerts.
Zapier and Built.io can integrate and automate API applications. Either can be used to set up Meraki dashboard Webhooks.
For more information about setting up and using Webhooks, refer to Cisco Meraki Webhooks.
Configure SNMP
Simple Network Management Protocol (SNMP) lets network administrators query devices for various types of information. Meraki allows SNMP polling to gather information from the dashboard or directly from Meraki devices, including MR access points, MS switches, and MX security appliances. Third-party network monitoring tools can use SNMP to monitor certain parameters on Meraki devices.
SNMP device information
You can poll the following information from the Meraki dashboard:
- Device MAC address
- Device serial number
- Device name
- Device status (online or offline)
- Device last contacted—date and time
- Mesh status (gateway or repeater)
- Public IP address
- Product code (for example, MR18-HW)
- Product description (for example, Meraki Cloud-controller 802.11n AP)
- Name of the network the device resides in (dashboard network)
- Packets/bytes in/out on each physical interface
Standard MIB support
SNMP servers use a Management Information Base (MIB), a database of SNMP Object Identifiers (OIDs). OIDs identify the managed objects that inform the SNMP server which values to poll from the device.
The Meraki dashboard and SNMP-capable Meraki devices support the following standard MIBs:
- SNMPv2-MIB .1.3.6.1.2.1.1
- IF-MIB .1.3.6.1.2.1
Meraki proprietary MIB
The Meraki dashboard has its own proprietary MIB that lets SNMP servers pull data and information from the dashboard.
- 
    Navigate to Organization > Configure > Settings > SNMP to locate and download the MIB document.
The Meraki Cloud MIB cannot poll information from Meraki devices directly, because it is designed strictly for dashboard information.
Configure dashboard SNMP polling
- 
    Navigate to Organization > Configure > Settings > SNMP.
- 
    Choose an SNMP configuration option: SNMP Version 2C or Version 3.
- 
    After you enable SNMP, send SNMP requests to the host defined directly under the enable setting. The community string and a sample command to extract information via SNMP requests display as well.
There are three versions available for configuration for SNMP. These versions are: Version 1, Version 2c, and Version 3
- Versions 1 and 2c allow for a simple community string to be defined. This string will be used between the SNMP server and reporting devices to validate the connection
- Version 3 includes authentication and encryption for added security. Version 3 requires that a username and password be defined
Cisco strongly recommends disabling SNMP V2C and only using SNMP V3 with Authentication mode SHA and Privacy mode AES128 for the highest level of security.
Configure IP restrictions to restrict SNMP access to particular IP addresses in your environment when using v2c. SNMP versions 1 and 2 send the community string in clear text, so IP restrictions prevent unauthorized SNMP access if another party intercepts or learns the community string. IP restrictions are recommended for v3 but not required.
Configure local device SNMP polling
You can poll individual Meraki devices locally. In this scenario, SNMP traffic stays within the local network, and each device is polled from the network management system.
- 
    Navigate to Network-wide > Configure > General > SNMP.
- 
    Configure the local SNMP settings.
Cisco strongly recommends using SNMP V3 with Privacy mode AES128 for the highest level of security.
Configure SNMP traps
SNMP traps are proactive SNMP messages sent when specific networking events take place. They are useful for real-time alerting in your network environment and can be sent from the Meraki cloud. SNMP traps are closely related to the alerts you can configure for your network. SNMP traps use SHA1 for authentication and AES for privacy.
Consider using webhooks instead of SNMP traps if your network environment allows, because webhooks have greater coverage.
Enable SNMP traps
- 
    Navigate to Network-Wide > Configure > Alerts.
- 
    Locate the SNMP Traps section, which is set to Disabled.
- 
    Enable SNMP Version 2c or Version 3.
- 
    To use V3 (username/password), select Add an SNMP user.
- 
    Input the desired username and password, which must also be configured on the SNMP server.
- 
    Enter the Receiving server IP address, which must be a public IP address.
- 
    Configure the receiving server ports with UDP port 161 or 162 (the default ports SNMP servers listen on).
Cisco strongly recommends using SNMP v3 for the highest level of security.
If the SNMP server is behind a NAT device, configure a port forwarding rule to allow the SNMP traffic through. This is due to SNMP traps being sent from the Meraki cloud controller. Specify the correct LAN IP address of the SNMP server and the UDP ports it listens on.
Define SNMP traps to be sent
SNMP traps are closely tied to the alerts configured under Network-Wide > Configure > Alerts. To generate SNMP traps, configure SNMP as a recipient for the alert. You can:
- Input SNMP as a default recipient so all enabled alerts generate a trap, or
- Configure SNMP on a per-alert basis.
Once you enable and configure SNMP traps to send, enable and set up the alerts that trigger them. The following list maps each SNMP trap to its dashboard alert and alert category:
| SNMP Trap Sent | Meraki Dashboard Alert | Alert Category | 
| Settings Changed | Configuration settings are changed | Network-Wide | 
| VPN Connectivity Change | A VPN connection comes up or goes down | Network-Wide | 
| Foreign AP Detected | A rogue access point is detected | Network-Wide | 
| Device Goes Down/Device Comes Online | A gateway goes offline | Wireless | 
| Device Goes Down/Device Comes Online | A repeater goes offline | Wireless | 
| Gateway to Repeater | A gateway becomes a repeater | Wireless | 
| Device Goes Down/Device Comes Online | A WAN appliance goes offline | WAN Appliance | 
| Uplink Status Changed | Primary uplink status changes | WAN Appliance | 
| No DHCP leases | The DHCP lease pool is exhausted | WAN Appliance | 
| IP Conflict | An IP conflict is detected | WAN Appliance | 
| Cellular Network Up/Cellular Network Down | Cellular connection state changes | WAN Appliance | 
| Rogue DHCP Server | A rogue DHCP is detected | WAN Appliance | 
| Warm Spare Failover Detected | A warm spare failover occurs | WAN Appliance | 
| Malware Blocked | Malware is blocked | WAN Appliance | 
| Malware Detected | Malware is downloaded | WAN Appliance | 
| Device Goes Down/Device Comes Online | A switch goes offline | Switch | 
| New DHCP Server Alert | A new DHCP server is detected on the network | Switch | 
| Port Disconnected/Port Connected | Any port goes down | Switch | 
| Port Cable Error | Any port detects a cable error | Switch | 
| Port Speed Change | Any port changes link speed | Switch | 
| Power Supply Down/Power Supply Up | A power supply goes down | Switch | 
| Redundant Power Supply Backup/Redundant Power Supply Back to Primary | A redundant power supply is powering a switch | Switch | 
| UDLD Error | Unidirectional link detection (UDLD) errors exist on a port | Switch | 
| Critical Temperature | A switch is operating at critical temperature | Switch | 
| Device Goes Down/Device Comes Online | A camera goes offline | Camera | 
| Device Goes Down/Device Comes Online | A cellular gateway goes offline | Cellular Gateway | 
Troubleshooting
Additional considerations for Syslog
If the syslog server is across a VPN, the source IP will be 6.X.X.X, provided no VLANs are in VPN mode, the MX is in passthrough mode, or the MX is in Routed/NAT Single LAN mode.
Storage allocation
Syslog messages can take up a large amount of disk space, especially when collecting flows. When choosing a host to run the syslog server, ensure enough storage space to hold the logs. Consult the syslog-ng man page for information on keeping logs for only a certain amount of time.
Expected traffic flow
Syslog traffic flows to the server in one of three scenarios, depending on the route type used to reach the server.
Scenario 1 – Reachable via LAN: The MX sources traffic from the VLAN interface where the server resides if the syslog server is on the LAN of the MX. The transit VLAN interface is used if the device is only accessible via static route.
Scenario 2 – Reachable via public interface: The MX sources traffic from the public interface (WAN) if the syslog server is accessible via the WAN link.
Scenario 3 – Reachable via AutoVPN: The MX sources traffic from the interface of the highest VLAN participating in AutoVPN if the syslog server is accessible via AutoVPN. If the traffic passes through the site-to-site AutoVPN connection, the traffic is subject to the Site-to-site outbound firewall rules, so you may need an allow rule.
To configure this allow rule:
- 
    Navigate to Security & SD-WAN > Configure > Site-to-site VPN > Organization-wide settings.
- 
    Select Add a rule.
Additional Webhooks considerations
Refer to Additional Alerting Considerations for event types that are rate limited.
SNMP trap limitations
- SNMP traps cannot be sent for any Systems Manager alerts.
- If a trap and its associated alert are not listed in Section 5.2, Meraki devices do not send traps for it.
- If devices go down and are not communicating with the dashboard, the trap is sent after the amount of time configured for the alert.