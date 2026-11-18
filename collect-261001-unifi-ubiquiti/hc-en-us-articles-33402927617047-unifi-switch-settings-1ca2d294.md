UniFi Switch Settings
UniFi Switch Settings let you configure how your network switches handle traffic, VLANs, port behavior, and Layer 3 isolation. These settings are critical for managing both basic Layer 2 switching and advanced Layer 3 routing features in UniFi deployments. Some settings apply to individual VLANs, others to switches globally, and some are specific to devices or ports.
Per-VLAN Settings
Location: Settings > Networks
These settings apply to individual VLANs and may be dependent on whether a UniFi Gateway or L3-capable switch is present.
When creating or editing a VLAN, you can also choose which UniFi Gateway or routing device is assigned to route traffic for that network. This determines which gateway handles inter-VLAN routing and other L3 functions.
mDNS
Forwards Multicast DNS (MDNS) traffic (e.g., AirPlay, AirPrint, Bonjour, Chromecast, media servers, etc) between selected networks so MDNS-enabled devices and services can be discovered across VLANs. Requires a UniFi Gateway.
mDNS Proxy
Defines which mDNS services are relayed between VLANs. By default, all mDNS traffic is forwarded. You can restrict this to reduce traffic or improve privacy.
- All: Forwards all mDNS traffic.
- 
Auto: Forwards a predefined list of commonly used services. The following table lists common service types included in Auto mode:Service mDNS Types Apple AirDrop _airdrop._tcp.local Apple AirPlay _airplay._tcp.local; _companion-link._tcp.local; _raop._tcp.local; _appletv-v2._tcp.local Apple File Sharing _afpovertcp._tcp.local Apple iChat _presence._tcp.local; _ichat._tcp.local Apple iTunes _daap._tcp.local; _atc._tcp.local; _dacp._tcp.local; _home-sharing._tcp.local; _apple-mobdev._tcp.local; _apple-mobdev2._tcp.local Aqara _aqara-setup._tcp.local; _aqara._tcp.local Bose _bose._tcp.local DNS Service Discovery _dns-sd._udp.local FTP Servers _ftp._tcp.local; _sftp-ssh._tcp.local Google Chromecast _googlecast._tcp.local; _googlezone._tcp.local HomeKit _homekit._tcp.local; _hap._tcp.local Matter Network _matterc._udp; _matterd._udp Philips Hue _philipshue._tcp.local Printers _printer._tcp.local;_ipp._tcp.local;_ippusb._tcp.local;_ipps._tcp.local;_scan-target._tcp.local;_pdl-datastream._tcp.local; _scanner._tcp.local; _http._tcp.local; _http_alt._tcp.local; _ipp-tls._tcp.local; _fax-ipp._tcp.local; _riousbprint._tcp.local; _ica-networking._tcp.local; _ica-networking2._tcp.local; _ptp._tcp.local; _canon-bjnp1._tcp.local Roku _roku._tcp.local, _rsp._tcp.local Scanners _scanner._tcp.local; _uscan._tcp.local; _uscans._tcp.local; _scan-target._tcp.local Sonos _sonos._tcp.local Spotify Connect _spotify-connect._tcp.local SSH Servers _ssh._tcp.local Time Capsule _adisk._tcp.local Web Servers _http._tcp.local; _https._tcp.local Windows File Sharing / Samba _smb._tcp.local; _smbdirect._tcp.local
- Custom: Forwards only selected or custom-defined services.
IGMP Snooping
Reduces multicast flooding by forwarding traffic only to switch ports with active multicast group members. This feature works without a UniFi Gateway. Some related features require UniFi switch firmware 7.2 or later.
Once enabled, there are several additional configuration options:
- 
Forward Unknown Multicast Traffic
 Forwards multicast traffic that does not match any IGMP membership reports to either:
  - Multicast Router Ports as configured below
  - All ports on the specified VLANs (Flood)
  - Drop (default)
 Multicast traffic in the224.0.0.0/24 range is always flooded.
Note: Which option you choose is client- and protocol-dependent. Some devices or applications—especially in AV and IoT environments—may expect multicast to be flooded regardless of group membership.
- 
Flood Known Protocols
Floods multicast traffic for known protocols — including PTP, H.323, NTP, SLP, and SSDP — to all ports. For PTP, this ensures clock synchronization messages reach all devices without requiring IGMP membership reports. Essential for AV and ProAV deployments.
- 
Fast Leave
Immediately removes ports from the multicast forwarding table when an IGMP leave message is received for a multicast group. Reduces latency when clients leave multicast groups. Recommended for video and high-turnover multicast environments.
- 
Create New Querier Switch
Allows you to manually specify a switch to be the IGMP Querier, instead of relying on default election. This may be required by Dante or other AV multicast systems. Refer to their documentation for guidance. If multiple Querier switches are created on the same VLAN, the switch with the lowest IP address is elected.
- 
Configure Multicast Router Ports
Opens the Port Manager to designate ports that should forward multicast group reports. Required for ports facing routers, upstream switches, or multicast sources.
See Pro AV Traffic Optimization on UniFi Switches for more information.
L3 Network Isolation (ACL)
Blocks all IPv4 traffic between devices in different networks using switch-level access control lists.
For more information, see UniFi Switches and ACLs.
Device Isolation (ACL)
Prevents devices within the selected VLAN from communicating with each other.
For more information, see UniFi Switches and ACLs.
Global Switch Settings
Location: Settings > Networks
These settings act as default behaviors applied to all UniFi switches handling the selected VLAN, unless explicitly overridden per device.
- 
Spanning Tree Protocol
Prevents network loops by blocking redundant paths. RSTP is enabled by default for enhanced scalability and convergence time. See Understand and Mitigate Network Loops (STP) for more information.
- 
Rogue DHCP Server Detection (DHCP Guarding)
Blocks unauthorized DHCP servers and is enabled by default. Rogue DHCP servers can cause network instability.
- 
Jumbo Frames
Enables Ethernet frames larger than 1500 bytes. Can improve network throughput in some cases, especially involving file transfers at multi-gigabit speeds, assuming all clients and switches along the path support jumbo frames.
- 
Flow Control (on supported switches only)
Temporarily pauses traffic to reduce congestion. Especially helpful when connecting devices and clients with mismatched speeds (e.g., 1GbE and 10GbE).
- 
802.1X Control
Uses RADIUS-based authentication to control port access. Most deployments use MAC-based authentication, which does not require user interaction. See MAC-Based VLAN Assignment Using 802.1x for more information.
Port Settings
Location: Devices > [Switch] > Ports > Port Manager
These settings control behavior on a per-port basis. Note that some settings are only available on specific switch models.
- State: Active, disabled, restricted, or ProAV. The latter option enables a port and configures optimal QoS settings for that type of client.
- Native VLAN: The VLAN assigned to traffic that arrives untagged.
- Tagged VLAN Management: Controls which tagged VLANs are allowed on the port. Useful for trunk links or VLAN-aware clients.
- PoE: Controls whether a port provides PoE power to a connected client.
- Multicast Router Port: Defines a port as connecting to an upstream multicast router. Important for multicast traffic spanning switches.
Advanced Options
- Operation: Switching (default), mirroring (for PCAP/security), or aggregation (supports non-consecutive ports).
- Link Speed: Manually set port speed and duplex mode. Auto negotiation is on by default.
- Ethernet Port Profile: Applies a pre-defined set of advanced settings, including VLANs, rate limits, and isolation behavior.
- Flow Control: Temporarily pauses traffic to reduce congestion. This will be enabled for all ports if enabled globally.
- Port Isolation: Prevents this port from communicating with other ports on the same switch, even if they share the same VLAN.
- Storm Control: Limits the rate of unicast, broadcast, or multicast traffic through the port. After enabling, packet-per-second limits can be set.
- Loop Protection: Disables the port automatically if a switching loop is detected.
- Spanning Tree Protocol: Enables STP/RSTP on a specific port. Inherits global setting if not overridden.
- Egress Rate Limit: Caps outbound bandwidth from the port.
- LLDP-MED: Extends LLDP info for devices like VoIP phones. Enabled by default and recommended to leave on.
- Voice VLAN: Assigns a VLAN to VoIP phones via LLDP-MED while defaulting other traffic to the port’s native VLAN.
- QoS: Assigns traffic to specific switch queues for prioritization. Automatically set by ProAV profiles, or can be customized manually.
Device Settings
Location: Devices > [Switch] > Settings tab
These settings apply to the switch hardware itself. Availability may vary by model.
- IP Settings: Assign a static IP or use DHCP for management.
- Network Override: Manually set the management VLAN for a device. Ensure the correct VLAN is tagged or native on the upstream port; misconfiguration may require a physical reset.
- Override Global Switch Settings: Configure Jumbo Frames, Flow Control, 802.1X Control, and Spanning Tree Protocol independently for this switch.
- Priority: Set STP/RSTP bridge priority. This is important for loop prevention and performance.
- SNMP: Set SNMP Location and Contact. SNMP monitoring is enabled elsewhere in Settings.
Additional Management Actions
- Adjust LCM display settings, brightness, and night mode.
- Enable or disable Rack Multi-Screen Synchronization.
- Replace, locate, restart, or remove the switch.
- Load saved configuration, set a replacement device, or manually update firmware.
Best Practices
- Use VLANs to logically separate traffic, then configure switch behavior per VLAN.
- Enable DHCP Guarding to block unauthorized DHCP servers.
- Leave STP or RSTP enabled and assign STP priorities to manage loops.
- Only enable Jumbo Frames if all devices in the traffic path support them.
- Use Port Isolation for untrusted ports (e.g., IoT or guest VLANs).
- Monitor performance using the Insights tab.
- Use Port Profiles to apply consistent configurations across multiple switches.