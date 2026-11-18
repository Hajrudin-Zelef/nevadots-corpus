Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
2
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
Here is the setup:
SonicWALL NSA3600 has WAN on X1 and DMZ on X2
Netgear FS728TP has port 28 plugged into X2 on SonicWALL and port 27 plugged into a production switch
Ubiquiti AP's are plugged into ports 1-6 on FS728TP
VLAN's are set on the Netgear switch as follows:
VLAN 1 is production VLAN and is marked "U" on all ports except 28 which is blank. Port 27 is marked "T" on VLAN1.
VLAN 99 is for guest wifi and is marked "U" on ports 1-6. It is marked as "T" on port 28.
All ports have PVID of VLAN 1 except port 28 which is PVID 99.
When users connect to the guest wireless, they get assigned the proper IP address but cannot get out to the internet.
Production wireless works perfectly.
Any thoughts are much appreciated. Please let me know if more info is needed to help diagnose. THANK YOU!
If you've got VLAN 99 "T"agged on Netgear port 28, you need to have it tagged on the SonicWall X2 as well - alternatively, "U"ntag it on port 28 and it'll most probably work. As it is, the VLAN 99 tagged frames are dropped on the SonicWall X2 port and the WiFi clients can't get to the router.
6 on FS728TP VLAN's are set on the Netgear switch as follows: VLAN 1 is production VLAN and is marked "U" on all ports except 28 which is blank. Port 27 is marked "T" on VLAN1. VLAN 99 is for guest wifi and is marked "U" on ports 1-6. It is marked as "T" on port 28. All ports have PVID of VLAN 1 except port 28 which is PVID 99.
So you have both production and guest Untagged on the UniFi ports? That won't fly... The guest network should be Tagged as it goes to the UniFi, and the UniFi system should know what VLAN the Guest network is on.