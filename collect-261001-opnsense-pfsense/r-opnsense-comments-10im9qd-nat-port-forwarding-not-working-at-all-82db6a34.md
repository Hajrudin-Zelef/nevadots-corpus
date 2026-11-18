# NAT Port Forwarding not working at all

I am on the edge of desperation

I have now read quite a few posts and how:

but still do not get to the goal.

The internal traffic works as desired over all the VLANS, etc.. But when I want to share my gaming server, I just can not get it myself and according to opn-ports tools, the port is still shown as closed.

Now I ask myself, what am I missing?

I have a router from my ISP and there DMZ mode enabled, so it should forward everything to the opnsense. In the Opnsense I have entered the NAT port forwarding as in the forum above, from this was directly set up a rule in the WAN.

Under Firewall->Settings-> Advanced I have set the marks for Reflection for port forwards and Automatic outbound NAT for Reflection.

Despite this, I can not access it via my ext. IP.

Opnsense is still quite new territory for me, maybe that's why :P