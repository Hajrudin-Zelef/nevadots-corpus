# How to block unknown MAC addresses in OpnSense

*Score : 1 | Source : https://networkengineering.stackexchange.com/questions/67091/how-to-block-unknown-mac-addresses-in-opnsense*

I'm dealing with an OPNSense router, at the moment I already configured the DHCP server to provide IPv4 addresses only to a legitimate list of fixed MAC addresses and I want to block every unknown MAC, because right now anyone can connect to the server via the etheret interface and can give himself a static IP and the route in order to enter into the subnet.
I know that the MAC can be spoofed and changed, but i want to mitigate this by denying any connection to unknown MAC addresses. Is this possible? Are other approaches available?
I'm on :