Securing Reolink IP Camera Systems: NVR VLAN Isolation with UniFi OS 4.4.9
This article was written with the help of Sonnet 4.5 on Claude.ai. All outputs were reviewed for accuracy and confirmed working on the UniFi OS and Reolink NVR systems.
Overview
IP camera systems and Network Video Recorders (NVRs) represent a unique security challenge in home and small office networks. These devices require internet access for firmware updates, remote viewing, and cloud features, yet they handle sensitive video footage and often have security vulnerabilities. This guide demonstrates how to isolate a Reolink NVR system on a dedicated VLAN using UniFi OS 4.4.9, allowing necessary functionality while maintaining network security through controlled access.
Hardware Used:
- Cloud Gateway Ultra (running UniFi OS 4.4.9)
- USW Flex Mini
- PoE+ Switch
- U6+ Access Point
Camera System:
- Reolink NVR (hardwired to switch)
- IP cameras (connected to NVR via PoE or WiFi)
Key Principle: NVRs need internet access for updates and remote viewing, but should be isolated from other network devices to prevent unauthorized access to video feeds and limit the impact of potential security vulnerabilities.
Understanding the NVR Security Model
Why NVRs Need Special Treatment
Sensitive Data: NVR systems record video footage of your property, potentially capturing sensitive information about your daily routines, when you’re home, security vulnerabilities, and private activities.
Internet Access Requirements: Unlike printers or some IoT devices, NVRs legitimately need internet access for:
- Firmware updates from the manufacturer
- Remote viewing via mobile apps and web portals
- Cloud recording and backup features
- Time synchronization (NTP)
- Push notifications for motion detection events
Security Vulnerabilities: Camera systems and NVRs have a poor security track record:
- Outdated firmware with known vulnerabilities
- Weak default credentials
- Unnecessary services enabled by default (UPnP, P2P)
- Poor encryption implementations
- Lack of timely security updates
Attack Vectors: Compromised camera systems can be used to:
- Access and exfiltrate video footage
- Use as pivot points to attack other network devices
- Participate in botnets (Mirai botnet famously targeted IP cameras)
- Serve as reconnaissance tools for physical security breaches
The Balance: Isolation with Functionality
The challenge is maintaining functionality while implementing security:
Must Allow:
- Internet access for updates and remote viewing
- Controlled access from specific networks (laptops, phones, tablets)
- Communication between NVR and its cameras
Must Prevent:
- Unauthorized access to video feeds
- Lateral movement to other network segments if compromised
- Unnecessary exposure of the entire camera network
The solution: Place the NVR on an isolated VLAN with internet access, but use IP-based firewall policies that target only the NVR itself. This provides necessary functionality while limiting the attack surface.
Understanding Reolink NVR Architecture
How Reolink Systems Work
NVR as Central Hub: The Reolink NVR acts as the central management and recording system for all cameras. You don’t access individual cameras directly — all management, viewing, and recording happens through the NVR interface.
Camera Connection Methods:
PoE Cameras (Most Common):
- Connect directly to NVR’s built-in PoE ports
- Receive both power and data from NVR
- Operate on NVR’s internal network
- No network configuration needed
WiFi Cameras:
- Connect to your WiFi network
- Must be on the same VLAN as the NVR for discovery
- Managed through NVR console once added
- Still record to NVR storage
Part 1: Initial NVR Configuration
Important: Complete these NVR configuration steps BEFORE creating VLANs or changing network assignments in UniFi OS. This ensures you have access to configure the NVR and can verify settings before isolation.
Step 1: Initial NVR Setup
- Connect the Reolink NVR to your network (temporarily on your main network)
- Power on the NVR and complete initial setup wizard
- Set a strong admin password
- Configure basic settings (time zone, date/time)
- Add any cameras that will be connected
Step 2: Configure NVR Network Ports
Access the Reolink NVR through the client software:
- Open Reolink Client
- Navigate to Device Settings → Network → Advanced → Server Settings
- Configure the following ports:
- Basic Service Port: 9000 (default, can be changed if desired)
- Check HTTPS to enable secure connections
- HTTPS Port: 443 (default, can be changed if desired)
4. Click Save or Apply
5. Note these port numbers for the firewall policy configuration
Why Configure Ports First:
- Ensures you know exactly which ports to open in the firewall policy
- Allows you to enable HTTPS before network isolation
- Verifies NVR is functioning properly before moving to isolated VLAN
- Makes troubleshooting easier if issues arise
Step 3: Test Basic Connectivity
Before proceeding with VLAN isolation:
- Verify you can access NVR web interface
- Confirm all cameras are visible and recording
- Test live view and playback
- Verify motion detection is working
- Note the NVR’s current IP address
Once the NVR is properly configured and tested, proceed with VLAN isolation.
Part 2: Creating the Camera VLAN
Step 1: Create the Camera Network
- Click the Settings gear icon on the left sidebar
- Navigate to Networks
- Click New Virtual Network
- Configure the network:
- Name: Camera
- Router: Default (leave as is)
- Zone: Internal
- Protocol: IPv4
- Auto-scale network: Enabled
- Advanced: Manual
- Check Isolate Network
- Check Allow Internet Access
5. Click Add
Understanding the Configuration
Isolate Network (Checked): Prevents devices on this VLAN from communicating with devices on other VLANs. This ensures the NVR and any WiFi cameras cannot access or be accessed by devices on your other networks except through explicit firewall policies.
Allow Internet Access (Checked): Unlike printers, NVRs legitimately need internet access for:
- Manufacturer firmware updates
- Remote viewing through Reolink cloud or app
- Time synchronization via NTP servers
- Push notifications to mobile devices
- Cloud backup features (if enabled)
Part 3: Assigning NVR to Camera VLAN
Step 1: Configure Switch Port
Since your Reolink NVR is hardwired to your network:
- Click UniFi Devices on the left sidebar
- Select your switch (the one the NVR is connected to)
- Click Port Manager on the right-hand side
- Select the port that your NVR is connected to
- Under Native VLAN / Network, select Camera
- Click Apply
Step 2: Set Fixed IP Address for NVR
Setting a static IP is critical for the IP-based firewall policy approach:
- Click Client Devices on the left sidebar
- Find and select your Reolink NVR
- Click Settings in the right-hand menu
- Under IP Settings:
- Check Virtual Network Override
- Set Network to Camera
- Check Fixed IP Address (required for security)
- Assign a static IP address
- Check Local DNS Record (optional)
- If using DNS, enter hostname (e.g., nvr.local )
5. Click Apply
Why Static IP is Required:
With our security approach, the firewall policy targets the NVR’s specific IP address rather than the entire Camera network. If the IP changes:
- The firewall policy breaks
- You lose access to the NVR
- Security is compromised if the old IP is reused
Static IP ensures the firewall policy remains effective and access stays consistent.
Step 3: Restart NVR
Power cycle the Reolink NVR to ensure it picks up the new network configuration:
- Disconnect power from the NVR
- Wait 10 seconds
- Reconnect power
- Wait 2–3 minutes for full boot
Step 4: Verify NVR Network Assignment
- In UniFi, go to Client Devices
- Find your Reolink NVR
- Verify it shows:
- Connected to Camera VLAN
- Has the static IP you assigned
- Shows as “Online”
Part 4: Adding WiFi Cameras (If Applicable)
Most Reolink cameras connect via PoE directly to the NVR and require no additional configuration. However, if you have WiFi-enabled cameras (battery-powered, plug-in WiFi, or PoE cameras used wirelessly):
Step 1: Initial Camera Setup
Initialize your WiFi camera on your network first:
- Follow Reolink’s initial setup process for your camera
- Connect the camera to your WiFi network
- Set a strong password for the camera
- Verify the camera is functioning properly
For detailed setup instructions, refer to: How to Initially Set up Reolink Devices
Step 2: Add Camera to NVR via LAN
Important: Both the camera and NVR must be on the same network for discovery. Since your NVR is now on the Camera VLAN, you need to temporarily assign the camera to the Camera VLAN as well.
- Click Client Devices on the left sidebar in UniFi
- Find and select the WiFi camera
- Click Settings in the right-hand menu
- Under IP Settings:
- Check Virtual Network Override
- Set Network to Camera
- Check Fixed IP Address (recommended)
- Assign a static IP address
5. Click Apply
Step 3: Auto-Add Camera to NVR
- Log in to your Reolink NVR
- Navigate to Channel → Channel Management
- Turn on Auto Add
- All cameras on the same LAN (Camera VLAN) will automatically appear in the list
- You may see “Incorrect account name or password” — this is normal
Step 4: Enter Camera Credentials
- Click Modify next to the camera
- Enter the login password you set for the camera during initial setup
- Click OK
- The camera should now appear with live view on the NVR
Step 5: Assign to Channel
- Select the camera you want to add
- Choose which channel to assign it to
- Click Next to complete the addition
Note: Each camera must be on a unique channel. You cannot have two cameras on the same channel.
Important Considerations for Battery-Powered WiFi Cameras
NVR Takes Over Management: Once a battery-powered WiFi camera is added to the NVR:
- The NVR takes full control of the camera
- Standalone access through the Reolink App/Client is no longer possible
- All viewing and management must be done through the NVR
To Restore Standalone Access: If you need standalone access to the battery-powered camera again (without NVR):
- A hard reset of the camera is required
- The camera will need to be set up from scratch
- You’ll lose the NVR integration
For complete instructions on adding WiFi cameras to Reolink NVRs, see: How to Add Reolink IP Cameras to Reolink WiFi NVR
Part 5: Configuring Firewall Policy
Create IP-Based Policy for NVR Access
- Navigate to Settings → Policy Engine
- Click Create New Policy
- Select Firewall
- Configure:
- Name: Internal to Reolink NVR
- Source Zone: Internal
- Source Networks: Select all networks that need access (Apple, Windows VLANs, etc.)
- Source Port: Any
- Action: Allow
- Auto Allow Return Traffic: Checked
- Destination Zone: IP Address
- Destination IP: Your NVR’s static IP address
- Destination Port: 443,9000
- Protocol: All
5. Click Save
Understanding the Ports
We specify only the ports configured in the NVR settings:
Get Lazro’s stories in your inbox
Join Medium for free to get updates from this writer.
Port 443 (TCP) — HTTPS:
- Secure web interface access
- Required for browser-based management
- Must be enabled in NVR settings (Part 1, Step 2)
Port 9000 (TCP) — Basic Service Port:
- Reolink client software connection
- Default port for client application access
- Configured in NVR settings (Part 1, Step 2)
Note: If you changed the default ports in the NVR configuration (Part 1, Step 2), update the port numbers in this policy to match your custom settings. For example, if you set HTTPS to port 8443 instead of 443, use 8443,9000 in the policy.
Still IP-Based for Maximum Security
We’re targeting the NVR’s specific IP address rather than the entire Camera network. This means:
- Only the NVR is accessible from approved VLANs
- WiFi cameras on the Camera VLAN remain isolated
- Adding cameras doesn’t expand the accessible surface
- Clear, maintainable security posture
Part 6: Testing and Verification
Verify NVR Access from Approved Networks
From Windows PC (on Windows VLAN):
- Open web browser
- Navigate to: https://nvr.local (or static IP address)
- Should see Reolink login page
- Log in with NVR credentials
- Test live view from cameras
- Test playback of recorded footage
From macOS/iOS (on Apple VLAN):
- Open Reolink app on iPhone/iPad
- NVR should be accessible
- Test live view
- Test playback
- Test remote notifications (if enabled)
From Any Approved Network:
- Access NVR web interface
- Verify all cameras appear and are recording
- Test playback controls
- Check motion detection settings
- Verify recording schedules
Verify Internet Connectivity
Check NVR Can Reach Internet:
- Log into NVR web interface
- Navigate to Settings → System → About
- Check for firmware updates — should be able to check for updates
- If available, test downloading an update
Test Remote Access (If Using Reolink Cloud):
- Disconnect from your home network (use cellular data)
- Open Reolink app
- NVR should be accessible remotely
- Test live view over internet
- Verify push notifications work
Verify Time Synchronization:
- Check NVR system time
- Should match actual time (synced via NTP)
- Important for accurate recording timestamps
Verify Isolation
Test That Other VLANs Cannot Access NVR:
- Devices on VLANs NOT listed in the Source Networks should be unable to reach the NVR
- Try accessing NVR from a guest network or other isolated VLAN
- Should timeout or be blocked
Check Camera VLAN Cannot Access Other Networks:
- The NVR should not be able to access devices on your main networks
- Isolation prevents the NVR from scanning or accessing other network resources
- This is automatic due to the “Isolate Network” setting
Part 7: Troubleshooting
Cannot Access NVR Web Interface
Possible Causes:
- Firewall policy not active or misconfigured
- Wrong IP address in policy
- NVR didn’t pick up static IP
- Source network not included in policy
Solutions:
- Verify required ports are in firewall policy (443,9000 if using defaults)
- Check if you changed default ports in NVR — update policy to match
- Confirm policy destination IP matches NVR’s actual IP
- Check NVR IP in UniFi Client Devices
- Verify your device’s VLAN is listed in Source Networks
- Try accessing from a different device on an approved VLAN
NVR Shows as Offline in App
Possible Causes:
- NVR doesn’t have internet access
- DNS resolution issues
- Reolink cloud service disruption
- Port forwarding issues (if manually configured)
Solutions:
- Verify “Allow Internet Access” is checked on Camera VLAN
- Check NVR can ping internet addresses (8.8.8.8)
- Test web interface access locally (should work even if cloud is down)
- Check Reolink service status on their website
- Restart NVR and test again
Cameras Not Appearing in NVR
For PoE Cameras:
- Verify camera is connected to NVR’s PoE ports
- Check cable connections
- Try different PoE port on NVR
- Power cycle NVR
- Check camera power indicator lights
For WiFi Cameras:
- Verify camera is on Camera VLAN (check UniFi Client Devices)
- Confirm camera has static IP assigned
- Check WiFi signal strength
- Camera and NVR must be on same VLAN for discovery
- Try re-adding camera through NVR interface
Part 8: Security Best Practices
NVR-Specific Security
- Change Default Password Immediately:
- Access NVR web interface
- Change the admin password from default
- Use strong, unique password (20+ characters)
- Store in password manager
- Never use default credentials like “admin/admin”
2. Keep Firmware Updated:
- Enable Auto Update in NVR settings
- Updates often include security patches
- NVR will automatically check for and install updates
- Test functionality after automatic updates if needed
3. Disable Unnecessary Features:
- Turn off UPnP if not needed for remote access
- Disable P2P if using VPN for remote access
- Turn off cloud features if not used
- Disable RTSP if not streaming to third-party systems
- Review and disable any unused network services
4. Configure Motion Detection Properly:
- Set up detection zones (avoid triggering on trees, flags)
- Adjust sensitivity to reduce false alerts
- Configure useful notifications
- Use privacy masks for sensitive areas
- Test detection before relying on it
5. Manage Recording Storage:
- Use appropriate retention periods (7–30 days typical)
- Enable overwrite when storage full
- Regularly check storage health
- Consider backup for critical recordings
- Don’t store recordings on network shares (security risk)
Network-Level Security
- Monitor NVR Traffic:
- Use UniFi traffic analytics to watch NVR activity
- Check for unusual outbound connections
- Monitor bandwidth usage
- Look for connections to unexpected IPs or countries
- Set up alerts for unusual traffic patterns
2. Review Access Regularly:
- Quarterly review which VLANs have NVR access
- Remove access for networks that don’t need it
- Verify firewall policy is still appropriate
- Check for any unauthorized devices on Camera VLAN
- Review NVR access logs
3. Consider VPN for Remote Access:
- More secure than cloud-based access
- UniFi Teleport VPN provides easy setup
- No ports exposed to internet
- Works even if Reolink cloud is down
- Trade-off: Requires VPN connection before access
4. Physical Security:
- Place NVR in secure location
- Limit physical access to NVR
- Protect power supply and network connections
- Consider locking network rack/cabinet
- Document camera and cable locations
What NOT to Do
❌ Don’t expose NVR directly to internet without proper security — Use Reolink cloud service or VPN
❌ Don’t use the same password for NVR and other systems — Unique credentials for each system
❌ Don’t ignore firmware updates — Security patches are critical
❌ Don’t put NVR on your main network — Defeats the purpose of segmentation
❌ Don’t create policies that open the entire Camera network — Use IP-based targeting for security
❌ Don’t rely solely on cloud access — Have local access method as backup
Understanding Remote Access Options
Reolink Cloud/P2P (Built-in)
How it works:
- NVR connects to Reolink cloud servers
- Mobile app connects to cloud servers
- Cloud servers relay connection to your NVR
- Uses P2P technology for video streaming
Advantages:
- Easy setup (usually automatic)
- Works from anywhere
- No port forwarding needed
- Handles dynamic IP addresses
Disadvantages:
- Relies on Reolink’s servers
- Less control over security
- May have bandwidth limitations
- Cloud service could be discontinued
Security considerations:
- Enable only if you need remote access
- Use strong UID password
- Enable two-factor authentication if available
- Monitor for unauthorized access attempts
Manual Port Forwarding
How it works:
- Forward specific ports from WAN to NVR IP
- Access NVR directly via your public IP
- No reliance on third-party services
Advantages:
- Direct connection (no intermediary)
- Full control
- No cloud dependency
- Often better performance
Disadvantages:
- Exposes NVR directly to internet
- Requires static IP or DDNS
- More complex setup
- Must secure NVR carefully
If you choose this approach:
- Use non-standard ports (not 80/443)
- Enable HTTPS only
- Implement IP whitelist if possible
- Use very strong credentials
- Monitor access logs religiously
UniFi Teleport VPN (Most Secure)
How it works:
- UniFi Cloud Gateway provides built-in Teleport VPN
- Install Teleport app on your phone or computer
- Connect to VPN from remote location
- Access NVR as if on local network
Advantages:
- Most secure option
- No NVR ports exposed to internet
- Can access entire network securely
- Works even without Reolink cloud
- Easy setup through UniFi interface
- No additional VPN server needed
Disadvantages:
- Requires UniFi Teleport app on remote devices
- Need to connect to VPN before accessing NVR
- Slightly more steps than direct cloud access
Recommendation: If you’re using UniFi equipment (Cloud Gateway Ultra), Teleport VPN is the gold standard for remote camera access. It’s built-in, easy to set up, and provides secure access to your entire network including the NVR.
Conclusion
Isolating your Reolink NVR system on a dedicated Camera VLAN represents a significant security improvement while maintaining full functionality. By implementing this configuration, you’ve achieved several critical security objectives:
Contained Video Access: Your surveillance footage is now protected behind network segmentation. Even if the NVR has a security vulnerability, an attacker cannot use it as a launching point to access your computers, phones, NAS, or other sensitive systems on different VLANs.
Controlled Access Points: Only explicitly authorized networks (Apple, Windows VLANs) can access the NVR through targeted firewall policies. Guest networks, IoT devices, and other segments have no ability to view your camera feeds or modify NVR settings.
Maintained Internet Connectivity: Unlike printers and other IoT devices which benefit from complete internet isolation, NVRs legitimately need internet access for firmware updates, remote viewing, and time synchronization. The Camera VLAN allows this while preventing lateral movement to other network segments.
Simplified Camera Management: Understanding that Reolink cameras are managed through the NVR means you only need one firewall policy. PoE cameras are inherently isolated on the NVR’s internal network, and WiFi cameras on the Camera VLAN are only accessible through the NVR interface. This architectural understanding simplifies security without compromising protection.
IP-Based Security: By targeting the NVR’s specific IP address rather than the entire Camera network, you’ve created a precise security model. If you add WiFi cameras to the Camera VLAN in the future, they won’t be automatically accessible — only the NVR remains accessible. This is defense-in-depth in action.
The combination of VLAN isolation and IP-based firewall policies creates a security architecture that respects the realities of modern camera systems while implementing enterprise-grade segmentation principles. Your camera system now operates in a security sandbox that limits potential damage from vulnerabilities while preserving the functionality that makes these systems valuable.
As you expand your surveillance infrastructure with additional cameras or NVRs, the pattern remains consistent: Camera VLAN with internet access, static IP addressing, and targeted firewall policies. Whether adding WiFi cameras or deploying a second NVR, you can replicate this secure architecture with minimal additional configuration.
The techniques demonstrated here extend beyond camera systems to any network device that requires internet access but should remain isolated from your core infrastructure. Network-attached storage with cloud sync, smart TVs with streaming services, and game consoles all benefit from similar VLAN isolation strategies.
Your surveillance system is now properly secured, accessible where needed, and isolated from potential compromise vectors — all while maintaining the remote access and management features that make modern NVR systems practical and useful.
References and Resources
- UniFi Documentation: https://help.ui.com/
- Reolink Support: https://support.reolink.com/
- Network Segmentation Best Practices: NIST Special Publication 800–125B