UniFi Gateway - WireGuard VPN Server
WireGuard is a high-performance VPN server found in your Network application's VPN section that allows you to connect to the UniFi network from a remote location. A UniFi Gateway or UniFi Cloud Gateway is required.
How Does it Work?
After enabling WireGuard and specifying a port (UDP 51820 by default), add a Client and share the configuration file with your desired recipient. Once the recipient has installed the WireGuard program or mobile app, they can import the configuration and easily remotely access the UniFi network at any time.
Note: On mobile devices, automatically add the WireGuard VPN configuration by scanning the QR code.
Frequently Asked Questions
Using Teleport or WireGuard is highly recommended. L2TP is a traditional VPN that is losing support on several different operating systems. In addition, L2TP has several caveats and encounters issues when the server is behind NAT.
WireGuard VPN encrypts your traffic and secures remote access connections. It also uses private and public keys.
Wireguard provides higher throughput than traditional VPNs such as L2TP. Wireguard can be used alongside other VPNs.
If the UniFi gateway is behind NAT, then the port used for Wireguard needs to be forwarded by the upstream router. The default port for WireGuard is UDP 51820 and this needs to be forwarded to the UniFi gateway's WAN IP address. For example:
- Protocol - UDP
- External Port - 51820
- Internal Port - 51820
- Internal IP - WAN IP address of the UniFi gateway
We recommend to use WireGuard on a UniFi gateway that has access to a public IP address. Any performance or port forwarding issues on the upstream router can cause the VPN to disconnect.
WireGuard is supported on many different clients. See the WireGuard installation page for more information.