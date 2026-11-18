UniFi Gateway - OpenVPN Server
OpenVPN is a VPN server found in the VPN section of your Network application that allows you to connect to the UniFi network from a remote location.
Requirements
- A Next-Gen UniFi gateway or UniFi Cloud Gateway
- UniFi Network application version 7.4 or newer.
How does it work?
After enabling OpenVPN and specifying a port (default OpenVPN port is 1194), add a User and share the configuration file with your desired recipient. Once the recipient has installed the OpenVPN program or mobile app, they can import the configuration and easily remotely access the UniFi network at any time.
Frequently Asked Questions
1. Should I use Teleport, Wireguard or OpenVPN?
For mobile client devices, it is recommended to use Teleport.
For desktop or laptop clients, Teleport and Wireguard are recommended over OpenVPN.
2. Is OpenVPN secure?
OpenVPN encrypts your traffic and secures remote access connections. It also uses private and public keys.
3. How does OpenVPN compare with other VPNs, and can you use them simultaneously?
OpenVPN provides lower throughput than Wireguard. OpenVPN can be used alongside other VPNs.
4. Can OpenVPN be used when the UniFi gateway is behind NAT?
If the UniFi gateway is behind NAT, then the port used for OpenVPN needs to be forwarded by the upstream router.
We recommend using OpenVPN on a UniFi gateway that has access to a public IP address. Any performance or port forwarding issues on the upstream router can cause the VPN to disconnect.
5. Which clients support OpenVPN?
OpenVPN is supported by many different clients. See the OpenVPN installation page for more information.