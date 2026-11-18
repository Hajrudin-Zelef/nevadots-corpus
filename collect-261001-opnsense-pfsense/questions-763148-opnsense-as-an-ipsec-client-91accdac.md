# OPNsense as an IPsec client

*Score : 1 | Source : https://unix.stackexchange.com/questions/763148/opnsense-as-an-ipsec-client*

I want to setup a permanent VPN connection from one site to another. I already correctly set up an IPsec server on one site, reachable with a fixed IPv4 and IPv6, and domain.
What I want to do now, is to set up one of my OPNsense box interfaces (let's say OPT1) to be permanently connected to this IPsec VPN, so I don't have to configure anything VPN-related on the connected computers/clients inside the network of OPT1.
Is this possible? And if yes, how?
I already tried to find guidance in this type of VPN, but I couldn't find an exact guide for my use case. Most guides are about how to set up an IPsec SERVER in OPNsense, but what I need is an IPsec CLIENT, which will provide the connection to a physical interface.