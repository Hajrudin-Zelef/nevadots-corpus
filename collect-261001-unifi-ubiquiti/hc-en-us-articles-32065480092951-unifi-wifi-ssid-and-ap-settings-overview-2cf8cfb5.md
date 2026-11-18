UniFi WiFi SSID and AP Settings Overview
The WiFi settings page in UniFi Network lets you control how your wireless networks operate at the SSID level. You can access it by navigating to Settings > WiFi. This includes basic settings like SSID and password, as well as advanced options for performance, roaming, and security.
This article explains each setting, what it does, and when to use it—covering both SSID-level options and access point–level radio and IP settings.
SSID Level Settings
Basic Configuration
- Network Name (SSID)
The SSID is the name of your WiFi network shown to nearby devices. Choose a unique and easily recognizable name, especially if you manage multiple networks.
- Password
Defines the WiFi password users must enter to join the network. It must be at least 8 characters long. Strong passwords with letters, numbers, and symbols are recommended for better security.
- Broadcasting APs
Select which Access Points (APs) will broadcast this WiFi network:
- All – Broadcasts the network from all APs.
- Specific – Manually choose which APs broadcast this network.
- Groups – Broadcast based on defined AP groups.
Tip: Use the Specific or Groups option for separating guest networks or IoT devices by physical location.
Advanced WiFi Settings
Connectivity Options
These settings control how devices connect to the network and how UniFi handles different connection types.
- Private Pre-Shared Keys (PPSK)
Enables you to assign multiple unique passwords to the same SSID, with each key mapped to a specific VLAN or user group. This is especially useful when you want to segment users—like guests, staff, or devices—on the same network name but route their traffic differently based on the password they use. Note that PPSK requires the use of WPA2 encryption (link to WPA2 section)
- Hotspot Mode
Enables UniFi’s Captive Portal or Passpoint (Hotspot 2.0) features, allowing you to present a splash page or require guest authentication before users can access the network. This is ideal for public or guest WiFi networks where controlled access and branding are important.
- Enhanced IoT Connectivity
Enables specialized AP functions to improve compatibility with certain smart home and IoT devices, particularly those limited to networking capabilities. This setting is generally not needed in modern networks but can help resolve connectivity issues with legacy or low-power devices that struggle to stay connected.
Frequency Band Selection
Control which frequency bands your WiFi network uses. Each band has different range and performance characteristics.
- WiFi Band (2.4 GHz, 5 GHz, 6 GHz)
Choose which bands the SSID will operate on.
- 2.4 GHz – Longer range, lower speed.
- 5 GHz – Shorter range, higher speed.
- 6 GHz – Available only on WiFi 6E/7 devices; offers high speed and less interference.
We generally recommend using multi-band SSIDs to ensure broad device compatibility and optimal performance—especially when using 6 GHz. Creating 6 GHz–only SSIDs can lead to discovery issues, as many client devices rely on 2.4 or 5 GHz for scanning and initial connection.
Multi-Link Operation (MLO)
A WiFi 7 feature that allows supported client devices to connect over multiple frequency bands simultaneously, improving throughput and connection stability. Enable this setting only if your network includes WiFi 7 clients that can take advantage of it. This setting also enables WPA3 security, which may limit connectivity with IoT clients.
Band Steering
Encourages client devices connected to 2.4 GHz instead to move to the higher performance 5 GHz band using BSS transition frames. This standardized methodology replaces the old AP level band steering, which is fully deprecated. This helps reduce interference, improves overall speed, and ensures better performance for devices that support higher bands. We generally recommend leaving this setting enabled.
Network Behavior & Visibility
These settings control how your network is advertised and how connected clients interact with one another.
Hide WiFi Name (SSID broadcast)
Disables the public broadcasting of an SSID's name, meaning devices won't see the network name in standard WiFi scans. Clients must manually enter the SSID to connect. This is sometimes used as a basic security measure to reduce visibility, though it does not prevent detection by more advanced over-the-air scanning tools.
Client Device Isolation
Prevents devices connected to the same access point from communicating with each other (also known as east-west traffic). This is ideal for guest or IoT networks where devices don’t need to talk to each other, and it can significantly reduce airtime usage and improve overall wireless performance by limiting unnecessary traffic.
Proxy ARP
Allows the access point to respond to ARP (IPv4) and NDP (IPv6) requests on behalf of connected clients, which helps reduce broadcast traffic on the network. This can improve airtime efficiency and lower latency, especially in large networks. However, it may increase resource usage on the AP and is generally done for broadcast minimization and airtime improvements.
Roaming & Transition
These settings help client devices switch between APs more efficiently, improving user experience in environments with multiple access points.
BSS Transition
Encourages client devices to roam away from access points with weak signals or high load and connect to a better-performing AP nearby. This improves roaming behavior in networks with multiple APs, helping maintain stable performance as clients move. However, some very old devices may not support this feature properly and could experience roaming issues when it's enabled. We generally recommend leaving this setting enabled.
UAPSD (Unscheduled Automatic Power Save Delivery)
Allows compatible client devices—such as phones and tablets—to stay in low-power mode longer by waking only when there’s real-time traffic like calls, messages, or push notifications. This helps extend battery life and reduce unnecessary wake cycles. However, some legacy or non-compliant clients may experience connectivity issues when this feature is enabled.
Fast Roaming (802.11r)
Speeds up the handoff process between access points by allowing supported client devices to maintain their session as they move throughout the network. This improves the roaming experience for mobile devices such as phones and laptops, reducing lag or connection drops during transitions. It’s generally recommended in environments with frequent movement, but note that Fast Roaming may conflict with Switch Port Isolation—for best results, enable only one of these features at a time. Our strong recommendation is to have this feature turned on.
Performance Optimization
These settings can help improve throughput, reduce interference, and prioritize critical traffic in busy networks.
WiFi Speed Limit
Allows you to cap the maximum wireless speed for clients connected to this SSID. This is especially useful when you want to deprioritize traffic from guest users or IoT devices, ensuring more bandwidth is available for critical or high-performance clients. The maximum limit per client is 100 Mbps.
Multicast Enhancement
Converts multicast traffic—such as media streaming or device discovery protocols—into unicast packets to improve reliability and efficiency. This setting is especially helpful in networks with devices like Chromecast, Apple TV, or Sonos, where frequent multicast traffic can impact performance. 
Multicast and Broadcast Control
Restricts the forwarding of multicast and broadcast traffic so that only selected clients capable of receiving these packet types are allowed to process them. This helps reduce airtime usage and interference caused by unnecessary traffic, especially in dense environments or networks with many IoT devices.
Data Rate and Timing
These settings affect how frequently devices communicate and how quickly clients are forced to roam.
802.11 DTIM Period
Controls how frequently the access point sends buffered multicast and broadcast traffic, and can be configured separately for each WiFi band or left on Auto. Raising the DTIM interval can improve fast roaming performance in some cases, but it may negatively affect airtime efficiency, battery life, and connectivity for IoT devices.
This is an advanced setting and should only be adjusted if you're targeting specific roaming behavior or troubleshooting timing-related issues. Our defaults of 1 and 3 for 2.4Ghz and 5/6Ghz respectively have been tuned to balance connectivity and device performance.
Minimum Data Rate Control
Defines the lowest WiFi data rate that clients are allowed to use when connecting to the SSID. Increasing the minimum rate helps reduce airtime usage and encourages clients to roam sooner, improving overall efficiency—especially in large or high-density networks. It can also block legacy or low-speed IoT devices that rely on outdated data rates. Before adjusting this setting, consider the types of devices connecting to your network, as even high-performance clients may fall back to lower rates when signal strength is weak.
Security Settings
These options control how clients authenticate and how secure the WiFi connection is.
Security Protocol (WPA2/WPA3)
Defines the encryption and authentication standard for the network.
Use case:
- Use WPA2/WPA3 for most environments — this provides a good compromise between modern security and legacy device compatibility. It allows newer devices to use WPA3 while falling back to WPA2 for those that don’t support it.
- Use WPA3 only when enabling MLO.
- Use Enterprise for RADIUS-based authentication.
- WPA2 only may be necessary for legacy IoT devices—or when using features like PPSK, which currently require WPA2 compatibility.
- For guest networks, use OWE with an Open network - this gives modern clients the security of WPA3 without a password. Do note that all 6 GHz networks will be secured with OWE or WPA3, as required by standards - the OWE toggle in UniFi Network will enable it for 2.4 and 5 GHz on your SSID as well.
Protected Management Frames (PMF)
Adds encryption to WiFi management traffic—such as association and disassociation messages—to prevent spoofing and disassociation attacks. PMF is a required component of WPA3 and Fast Roaming, and can be configured with three options:
- Optional (default): Enables PMF when supported by the client, while maintaining compatibility with older devices that don’t support it.
- Required: Enforces PMF for all clients, providing the highest level of security. This is required for WPA3-only networks and OWE networks.
- Disabled: Turns off PMF entirely. This is only recommended when creating a separate WPA2 SSID for legacy or incompatible devices.
PMF is essential for securing modern networks, but disabling it should only be considered in cases where specific clients can’t connect otherwise.
Group Rekey Interval
Defines how often encryption keys are refreshed for broadcast traffic. We strongly recommend keeping the default setting.
MAC Address Filter
Restricts access to the network by allowing only client devices with specific MAC addresses to connect. This provides a basic layer of access control through allowlists managed within the UniFi interface. While useful for limiting which devices can join a network, it’s generally better suited for simple control scenarios with clients that do not rotate or randomize their MAC addresses. For more flexible or secure segmentation—such as assigning VLANs per device—PPSK is usually a more robust alternative.
RADIUS MAC Authentication
Authenticates client devices by verifying their MAC addresses against an external RADIUS server before allowing them to join the network. This method is commonly used in enterprise environments where centralized, policy-driven control over device access is required.
Unlike MAC Address Filter, which relies on a locally managed allowlist, RADIUS MAC Authentication offers centralized enforcement, integration with identity systems, and the ability to dynamically assign VLANs or policies based on MAC identity. Learn more here
SAE Anti-clogging
This is a required feature for WPA3 authentication by preventing denial-of-service (DoS) attacks that attempt to overwhelm the AP with handshake requests. This feature is rarely needed in typical deployments and is intended for high-security environments or cases where targeted attacks are a concern. It can safely be left disabled in most networks unless there is a specific threat model that requires it. We highly recommend not to adjust the default value of it.
SAE Sync Time
Configures the timeout window for WPA3's handshake retries, which determines how long the system waits for a response before dropping the attempt. This setting is designed to help mitigate denial-of-service (DoS) attacks that exploit handshake timing. Like SAE Anti-clogging, it is on by default and should only be adjusted in high-security environments or when specifically addressing targeted attack scenarios.
WiFi Blackout Scheduler
Allows you to automatically disable an SSID during specific times of the day or week. This is particularly useful for limiting access to guest or IoT networks outside of business hours, reducing unnecessary network activity, or enforcing screen time rules in home environments. Each SSID can have its own schedule, offering flexible control across different network types
Individual AP settings
Minimum RSSI
Tells the AP to disconnect clients based on signal strength (measured in dBm). This is helpful when attempting to keep client data rates up by enforcing strict limits to AP cell size (signal range) and control for sticky clients. However, this can have implications for general roaming if improperly tuned; some devices may refuse to connect to an AP from which they have been kicked multiple times. This is generally not recommended unless running a high density deployment.
Interference Blocker
An extension of Minimum RSSI which treats all connections below the minimum signal strength as noise - the AP will not waste airtime attempting to complete the association process with these clients. Recommended for high density deployments.
Roaming Assistant
New in Network 9.2, this uses BSS transition frames to inform clients that they will be dissociated from the AP once they drop below a certain signal strength. Unlike the “hard kick” of Minimum RSSI, this “soft kick” is tolerated much better by modern clients and is generally recommended to be set to a value below -70 dBm.