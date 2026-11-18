My target is to route: 192.0.2.1 (public IP) to 192.0.2.2 (another public IP out of my network). I want to route all the ports and protocols, so Mikrotik will just send all the packets there.
I have tried this:
/ip firewall nat add chain=dst-nat dst-address=192.0.2.1 \ action=dst-nat to-addresses=192.0.2.2
/ip firewall nat add chain=src-nat dst-address=192.0.2.2 \ action=masquerade 
However it didn't work. May I please know what I'm doing wrong? I should point out that 192.0.2.1 is not connected to any server out there, however 192.0.2.1/24 is in use on the router and some IP's are in use on the servers.
192.0.2.0/24,198.51.100.0/24, and203.0.113.0/24), and you should use those so that people know you are faking addresses.