Hairpin NAT in UniFi
Hairpin NAT is a feature of NAT that is implemented in UniFi. It allows devices on the internal network to access a local server using the network's public IP address. This is particularly useful for environments where a service is hosted internally but needs to be accessed using the same domain name from both inside and outside the network.
How Hairpin NAT Works in UniFi
By default, UniFi's gateway configuration enables Hairpin NAT. When a device on the local network attempts to connect to the public IP address of the UniFi gateway, the traffic is redirected internally, ensuring that port forwarding rules apply as they would for external requests. This makes it seamless for users to access internal services without requiring separate configurations for internal and external access.
Use Cases for Hairpin NAT
- Hosting a local web or application server that must be accessible via a public domain name.
- Enabling internal devices to connect to externally accessible services without modifying DNS settings.
- Simplifying configurations for remote access to internal resources.