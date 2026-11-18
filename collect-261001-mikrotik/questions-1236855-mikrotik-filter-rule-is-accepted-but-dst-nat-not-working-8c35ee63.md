I have an NAT issue with mikrotik CCR1016-12G - namely I can't see any packets going to my dst-nat rules and dst-nat rules are not working.
I have tried disabling filtering rules that might drop any packets, and I've also tried to make a specific rule to accept a specific port with no luck:
chain=input action=accept connection-state=new protocol=tcp 
src-address=0.0.0.0/0 in-interface=ether01-gateway dst-port=8088 
I can see the packets coming in to the filter rules and assume that they get accepted(as I have the previous rule in place), but no packets are going to the dst-nat part:
chain=dstnat action=dst-nat to-addresses=192.168.1.88 to-ports=80 
protocol=tcp src-address=0.0.0.0 dst-address="Outer-IP" dst port=8088   
What could be the issue that I don't see any packets going to NAT?