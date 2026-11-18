At my office I have a Unifi USG Gateway that currently has 3 IPSEC VPN Tunnels (Lan-To-Lan VPN) configured. I also have a L2TP Remote VPN Network configured so that when I'm outside of my office I can connect to the network.
When I'm at the office I can see and ping all the devices on all 3 IPSEC Networks. No problem at all. The issue that I'm having is that when I'm outside and I connect to my Office's network through the L2TP Remote VPN, I can see and ping my office's local devices and only the devices in 1 of the IPSEC Network. It seems that for some reason only 1 out of the 3 IPSEC network is exposed to the Remote VPN Network.
It does not seem to be an issue with the firewall settings.
Any ideas how I can expose all 3 networks to my Remote VPN Connection?
Any help would be greatly appreciated.
Regards.
Carlos