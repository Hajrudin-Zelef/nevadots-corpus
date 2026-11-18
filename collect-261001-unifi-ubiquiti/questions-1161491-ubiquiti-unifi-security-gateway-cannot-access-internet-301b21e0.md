Super User is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
2
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I have a Security Gateway with the WAN port connected to my BT Homehub, and then the LAN on the Gateway going to a switch to which everything else is connected including two Access Points.
The Gateway will not connect to the internet. It connects fine when I run a cable from the router to the switch then it connects but I don't understand what is stopping it connect straight from the router to the WAN port. I've tried DHCP, setting IP manually and even trying the PPPoE settings from the router on the Gateway.
What am I missing?
I've found this Unifi forum thread that describes someone else fighting to set up a Unifi Security Gateway with a HomeHub and the solution suggested is to go back to the white PPPoE unit or buy a third party router. It doesn't answer why the set up doesn't/isn't working.
Unifi Security Gateways are quite painful to setup, but once done they work great.
I experienced best setup following the next steps:
Configure a network in the controller having 192.168.1.0/24 as IP/subnet (192.168.1.1 is the factory default IP of the USG)
Confgure your ISP router to an IP range different to 192.168.1.1; for example set it up to be 192.168.2.1 and enable DHCP server on it, so it will give the USG a IP like 192.168.2.2 on the USG WAN interface.
First plug in a factory default set USG to your switch on the LAN1 interface. It will have IP 192.168.1.1 here and you can adopt it to your controller.
Once adopted, plug in the WAN interface of your USG to any port on your ISP router and wait until it get and IP from the DHCP server of your ISP router.
Normally once this done, adopted, and getting internet over WAN you can change the IP of the USG. Usually the problem comes because default IP of USG is 192.168.1.1 and most ISP routers also use this IP.