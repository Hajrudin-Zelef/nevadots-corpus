UniFi Remote Access: VPN and Port Forwarding
To securely access a web server, locally hosted application, or other internal service from outside your network, you need either a VPN or port forwarding. VPNs provide encrypted remote access, while port forwarding allows direct external connections to a service on your network. In this guide, we’ll cover when to use each method, how to configure them in UniFi, and key security considerations.
VPN Server
A Virtual Private Network (VPN) requires users to establish a secure connection before accessing internal resources. This is commonly used for employees accessing internal company resources. With a VPN Server:
- Clients must authenticate before gaining access.
- All data is encrypted, improving security.
- The internal network remains hidden from public exposure.
The easiest way to set up VPN is with our one-click VPN options UniFi Identity and Teleport.
UniFi currently supports manually setting up the following protocols:
Port Forwarding
Port forwarding allows external traffic to reach a specific device or service on your internal network, most commonly a web server, gaming server or other remote service. It works by forwarding all traffic that targets your public IP address on a certain port to a specific internal IP/port.
Key considerations for port forwarding:
- Security responsibility falls on the exposed device—proper configuration is crucial.
- Traffic is not encrypted by default, making it potentially vulnerable to attacks.
Port forwarding is a specific type of Destination NAT (DNAT), where traffic is forwarded to a single internal IP. For more details on NAT in UniFi, click here.
Ensure a Public, Static IP
For Port Forwarding and most VPNs (excluding Teleport), a Public IP is necessary for connectivity. Additionally, a Static IP can ensure stable connectivity over time. For more information on Public, Static IPs in UniFi, click here.