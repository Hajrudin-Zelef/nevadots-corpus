I have RouterOS v6.39.2 (stable)
- external IP: 134.249.116.246
- router IP: 192.168.88.1
- internal server IP: 192.168.88.245
I setup a regular port forwarding via
  ip firewall nat add chain=dstnat dst-address=134.249.116.246 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.88.245 to-ports=8080 
Attempt to setup a hairpin NAT
 ip firewall nat add chain=srcnat src-address=192.168.88.0/24 dst-address=192.168.88.245 protocol=tcp dst-port=8080 out-interface=LAN action=masquerade
failed due to:
input does not match any value of interface
I can't understand what is wrong