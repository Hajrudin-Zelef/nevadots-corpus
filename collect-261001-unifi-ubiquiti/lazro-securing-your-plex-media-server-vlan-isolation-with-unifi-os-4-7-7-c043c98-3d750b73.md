Securing Your Plex Media Server: VLAN Isolation with UniFi OS 4.4.9
This article was written with the help of Sonnet 4.5 on Claude.ai. All outputs were reviewed for accuracy and confirmed working on the UniFi OS and Plex system.
Overview
This guide walks through securing a Plex media server using VLAN isolation in UniFi OS 4.4.9. By isolating your media server and related services (like Deluge) on separate VLANs, you create network segmentation that limits potential security risks while maintaining controlled access from your other devices.
Hardware Used:
- Cloud Gateway Ultra (running UniFi OS 4.4.9)
- USW Flex Mini
- PoE+ Switch
- U6+ Access Point
Services Covered:
- Plex Media Server (Ubuntu 24.04.3 LTS)
- Deluge BitTorrent Client
- UniFi OS
Part 1: Creating Isolated VLANs
Access your Unifi Site Manager at https://unifi.ui.com/. You can select the site listed in the manager.
Creating a New VLAN
- Click the Settings gear icon on the left sidebar
- Navigate to Networks
- Click New Virtual Network
- Configure the network:
- Name: Plex (or your preferred name)
- Router: Default (leave as is)
- Zone: Internal
- Protocol: IPv4
- Auto-scale network: Enabled
- Advanced: Manual
- Check Isolate Network
- Check Allow Internet Access
5. Click Add
Repeat this process for any additional isolated networks you need (Apple TV network, etc.).
Assigning Devices to VLANs
For Wired Devices (Connected to Switch)
- Click UniFi Devices on the left sidebar
- Select your PoE+ Switch
- Click Port Manager on the right-hand side
- Select the port that your device is connected to (e.g., the port for Plex server)
- Under Native VLAN / Network, select your VLAN (e.g., Plex)
- Click Apply
Setting Fixed IP Address
- Click Client Devices on the left sidebar
- Select the device you want to configure (e.g., Plex server)
- Click Settings in the right-hand menu
- Under IP Settings:
- Check Virtual Network Override
- Set Network to your VLAN (e.g., Plex)
- Check Fixed IP Address (recommended for servers)
- Assign your desired static IP address
5. Click Apply
Forcing IP Update on Ubuntu
If your Ubuntu server doesn’t immediately pick up the new IP:
Restart NetworkManager:
sudo systemctl restart NetworkManager
Alternative — GUI Method:
- Disable the network adapter in Ubuntu’s network settings
- Re-enable it
- The system should request a new DHCP lease
Verify the new IP:
ip a
Part 2: Configuring Firewall Policies
UniFi OS 4.4.9 uses a Zone-based Policy Engine rather than traditional firewall rules.
Understanding Policy Components
- Source Zone: Where traffic originates (e.g., Client VLAN, User network)
- Source Port: Port on the originating device (almost always “Any”)
- Destination Zone: Where traffic is going (e.g., Plex VLAN)
- Destination Port: The service port you’re allowing (e.g., 32400 for Plex)
- Action: Block, Allow, or Reject
Critical Note: Source ports should almost always be set to “Any” because client devices use random high ports when initiating connections. Only the destination port should be restricted.
Creating Internal Access Policies
Policy 1: Allow Multiple Networks to Access Plex
- Navigate to Settings → Policy Engine
- Click Create New Policy
- Select Firewall
- Configure:
- Name: Internal to Plex
- Source Zone: Internal
- Source Networks: Select all networks that need Plex access
- Source Port: Any
- Action: Allow
- Auto Allow Return Traffic: Checked
- Destination Zone: Internal
- Destination Network: Plex
- Destination Port: 32400
- Protocol: All
5. Click Save
Policy 2: Allow Access to Deluge (if applicable)
- Navigate to Settings → Policy Engine
- Click Create New Policy
- Select Firewall
- Configure:
- Name: Internal to Deluge
- Source Zone: Internal
- Source Networks: Select networks that need Deluge access
- Source Port: Any
- Action: Allow
- Auto Allow Return Traffic: Checked
- Destination Zone: Internal
- Destination Network: Plex
- Destination Port: 8112
- Protocol: All
5. Click Save
Part 3: External Access Configuration
Security Considerations
Important: Opening ports to the internet increases your attack surface and should only be done when necessary. Before proceeding with external access configuration, consider whether you truly need it.
Alternative to External Access: Plex supports downloading media directly to mobile devices through the Plex app when you’re connected to your local network. You can sync content to your phone or tablet for offline viewing, eliminating the need to expose your server to the internet for many use cases. This is a much more secure option if your primary goal is accessing media while away from home.
Get Lazro’s stories in your inbox
Join Medium for free to get updates from this writer.
If you decide external access is necessary, proceed with the following steps to configure it as securely as possible.
Setting Up Remote Access for Plex
Using a non-standard port (32401) provides security through obscurity.
Step 1: Create Port Forwarding Rule
- Navigate to Settings → Policy Engine
- Click Create New Policy
- Select Port Forwarding
- Configure:
- Name: Plex External
- WAN Interface: WAN1 (Default)
- WAN Port: 32401 (external port)
- From: Any
- Forward IP Address: Your Plex server’s IP address (Select Device)
- Forward Port: 32400 (Plex’s internal listening port)
- Protocol: TCP/UDP
5. Click Add
Key Concept: Port translation happens here:
- External clients connect to port 32401
- Traffic is forwarded to internal port 32400 where Plex actually listens
Step 2: Create External Firewall Policy
- Navigate to Settings → Policy Engine
- Click Create New Policy
- Select Firewall
- Configure:
- Name: Internet to Plex
- Source Zone: External
- Source Region: Country of residence (or restrict to specific countries)
- Source Port: Any
- Action: Allow
- Auto Allow Return Traffic: Checked
- Destination Zone: Internal
- Destination Network: Plex
- Destination Port: 32400 (internal port after forwarding)
- IP Version: Both
- Protocol: All
5. Click Save
Step 3: Configure Plex Server
- Open Plex web interface
- Go to Settings → Remote Access
- Check “Manually specify public port”
- Enter 32401
- Click Apply
Important: Plex always listens internally on port 32400. The manual port setting only tells Plex what external port to advertise.
In Plex:
- Settings → Remote Access should show “Fully accessible outside your network”
Step 4: Verify External Access
- Visit https://www.canyouseeme.org/
- Enter port 32401
- Click Check Port
- Should show “Success”
Step 5: Fix Mobile App Relay Connection
If the Plex mobile app shows “Relay” instead of direct connection:
- Open Plex app
- Go to Profile (top right) → Settings
- Navigate to Advanced
- Tap Reset Cache
- Restart the app
- Verify: Libraries → Library (top) → See All Libraries → Should show “Remote Connection”
Part 4: Local DNS Configuration
Creating friendly hostnames makes accessing your services easier without remembering IP addresses.
Creating DNS Records
- Click Client Devices on the left sidebar
- Select your device (e.g., Plex server)
- Click Settings in the right-hand menu
- Under IP Settings:
- Check Local DNS Record
- Enter your desired hostname (e.g., plex.local )
5. Click Apply
Accessing Services
Note: DNS only resolves to IP addresses, not ports. You must include the port in the URL.
- Plex: http://plex.local:32400/web
- Deluge: http://plex.local:8112
Recommendation: Create browser bookmarks for these URLs for quick access.
Part 5: Troubleshooting Common Issues
Device Not Getting Correct IP
Problem: Device shows old IP after VLAN assignment
Solutions:
- Disable and re-enable network adapter on the device
- Restart NetworkManager: sudo systemctl restart NetworkManager
- Reboot the device
- Check that VLAN assignment is correct in UniFi
Cannot Access Service Despite Policy
Problem: Firewall policy exists but connection times out
Common Causes:
- Source port is restricted: Should be “Any”, not a specific port
- Wrong destination port: Verify the service is actually listening on that port
- Policy order: Deny rules above Allow rules will block traffic
- Zone assignment: Device not actually on the expected VLAN/zone
External Access Shows Relay Connection
Problem: Plex accessible but mobile app uses relay
Solutions:
- Reset app cache (Profile → Settings → Advanced → Reset Cache)
- Force quit and restart the app
- Sign out and back into Plex account
- Test on cellular data (not home WiFi)
- Verify port forwarding shows success on canyouseeme.org
- Check Plex Remote Access settings show correct public port
Conclusion
Network segmentation through VLAN isolation represents a fundamental shift in how we approach home network security. Rather than treating your entire home network as a single trusted zone, this guide has demonstrated how to implement defense-in-depth principles that are commonly used in enterprise environments.
By isolating your Plex media server and related services on dedicated VLANs, you’ve accomplished several critical security objectives:
Containment: If your media server were ever compromised — whether through a software vulnerability, weak credentials, or malicious media files — the attacker’s lateral movement across your network is severely restricted. They cannot freely pivot to other devices on different VLANs without first breaching your carefully constructed firewall policies.
Principle of Least Privilege: Each firewall policy you’ve created embodies this security principle. Your Apple TV can access Plex on port 32400, but nothing more. Your client networks can reach Deluge on port 8112, but are blocked from everything else on that VLAN. This granular control means that even legitimate devices only have the minimum access necessary to function.
Visibility and Control: With traffic now flowing through explicit policy definitions, you have unprecedented visibility into what’s happening on your network. UniFi’s traffic analytics can show you exactly which devices are communicating with your media server, how much bandwidth they’re consuming, and whether any unexpected connection attempts are occurring.
External Access Without Compromise: The port forwarding configuration demonstrates how to safely expose services to the internet. By using port translation (external 32401 → internal 32400) and region-based restrictions, you’ve added multiple layers of protection that go beyond simple port forwarding. The non-standard external port reduces automated scanning exposure, while the internal isolation ensures that even if someone gains access to Plex, they haven’t gained access to your entire network.
The techniques covered in this guide — VLAN creation, zone-based firewall policies, port forwarding with translation, and local DNS configuration — are all transferable skills that extend beyond just securing a media server. These same principles can be applied to IoT devices, home automation systems, guest networks, security cameras, and any other services that benefit from isolation.
As you continue to expand your home network, remember that security is not a destination but an ongoing process. Regularly review your firewall policies, keep your services updated, monitor your network traffic for anomalies, and adjust your security posture as new threats emerge and your network requirements evolve.
The investment you’ve made in properly segmenting your network today will pay dividends in both security and peace of mind for years to come.
References and Resources
- UniFi Documentation: https://help.ui.com/
- Plex Support: https://support.plex.tv/
- Network Segmentation Best Practices: NIST Special Publication 800–125B
- CanYouSeeMe Port Checker: https://www.canyouseeme.org/