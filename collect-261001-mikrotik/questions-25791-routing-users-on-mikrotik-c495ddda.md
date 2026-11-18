On one MikroTik router, I want to divide my users to two groups and assign each group to a separate network (two networks). How do I do that?
2 Answers 2
Assuming you have separated them into two vlans, or in two parts of a subnet, or some other criteria that is comprehensible at the network level, you will most likely need to setup a Policy Route, i.e. a rule that routes based on something more than just the packet's destination.
For example, if you have user group A in subnet 192.168.1.0/24 and user group B in subnet 192.168.2.0/24 you could create a Policy Route such that if the packet comes from subnet 192.168.1.0/24 it is sent out through line 1, and if the packet comes from the other subnet it is sent out through line 2.
See here : http://wiki.mikrotik.com/wiki/Policy_Base_Routing
It is pretty easy to separate users in different networks on MikroTiks with RouterOS.
Let say you need to separate Wi-Fi clients from Wi-Fi guests. All you need to do is to add a new virtual Wi-Fi interface, add an IP address to the interface, create a new DHCP server, and setup routes for traffic to flow.
Here is a good explanation how is done. It was really helpful for me: