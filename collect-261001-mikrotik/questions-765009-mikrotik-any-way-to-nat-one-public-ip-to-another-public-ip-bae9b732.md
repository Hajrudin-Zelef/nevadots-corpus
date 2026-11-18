You just do a dst-nat to the new IP and then an src-nat to the IP of the router.
You don't need rinetd for this, IP > Firewall > NAT can support this directly.
Here are some example rules:
/ip firewall nat add chain=dst-nat dst-address=OLD_PUBLIC_IP protocol=tcp dst-port=80 \
action=dst-nat to-addresses=NEW_PUBLIC_IP to-ports=80
/ip firewall nat add chain=src-nat dst-address=NEW_PUBLIC_IP protocol=tcp dst-port=80 \
action=masquerade 
First you do a Destination NAT for all packets coming to OLD_PUBLIC_IP to TCP Port 80, to NEW_PUBLIC_IP to port 80.
Then you do a Source NAT (masquerade) for all packets destined to NEW_PUBLIC_IP to TCP Port 80 so that the server running on the new IP knows where to return the packets.
This - as well as rinetd - will cause all redirected connections to the new IP to have the source address of the MikroTik router instead of the original Client's IP.
Edit:
Rereading your question, I am not sure if I understood it correctly. Probably my answer is offtopic?