I have a mikrotik rb4011igs with following connections:
- WAN on port1
- Bridge with DHCP for lan on ports2-9
- Other LAN on port10 (I cant make any changes to this networks router, so cant change the default gateway for this LANs devices)
Now I want to be able to access the other LAN devices with their addresses from my LAN(other LANs devices dont need to access the mikrotiks LAN).
As far as I understand, I need to create some NAT rules with some kind of masquerading, but unfortunately I'm not sure what to do exactly.