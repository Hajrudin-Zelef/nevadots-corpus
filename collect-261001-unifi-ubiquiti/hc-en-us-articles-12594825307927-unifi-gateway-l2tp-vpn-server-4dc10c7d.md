UniFi Gateway - L2TP VPN Server
L2TP is a traditional VPN server found in the VPN section of your Network application that allows you to connect to the UniFi network from a remote location. A UniFi Gateway or UniFi Cloud Gateway is required.
Should I Use L2TP?
On Next-Gen UniFi gateways, there are much better options available such as Teleport and Wireguard.
- For mobile users, we strongly recommend to use Teleport instead of L2TP. Teleport is faster, more secure, and requires zero configuration.
- For desktop and laptop users, we strongly recommend to use Wireguard instead of L2TP. Wireguard is faster, more secure, and requires less configuration.
How Does it Work?
After enabling L2TP, add a User and share the Pre-Shared Key and user credentials with your desired recipient. If available, the recipient can use the built-in L2TP VPN on their choice of operating system.
Compatibility and Limitations
There are several caveats associated with using L2TP. These are not specific to the UniFi gateway and associated with the protocol itself.
- L2TP is losing support on several different operating systems. Android versions that still support L2TP require the usage of the Weak Ciphers option.
- L2TP encounters issues when the UniFi gateway is behind NAT, even when forwarding the ports on the upstream router. On Windows clients, you must modify the registry.
- Windows clients must be configured to enable MS-CHAP v2. See here:
- MacOS clients must be configured to send all traffic over the VPN. See here:
- L2TP cannot push any routes to clients. Split tunneling requires static routes to be manually added on each client.
Frequently Asked Questions
We recommend to use L2TP on a UniFi gateway that has access to a public IP address. Any performance or port forwarding issues on the upstream router can cause the VPN to disconnect.