Super User is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
1
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I have two Mikrotik routers (951g-2hnd) connected to each other via ethernet.
As I understand if I will be using WDS for extending WiFi coverage, my routers will be using WiFi for communicate between themselves, but I want to use ethernet link instead of WiFi, could someone provide example configuration?
WDS don't do anything special. I just allow bridging APs without wires.
Just bridge all wireless cards to same LAN. And ensure, this LAN has only one DHCP server enabled.
For example interconnect this RB951 by wires between 2 - 5 ports. Look for not build loops or enable RSTP in bridges. And disable dhcp server on all except one.
Also is good to set different IP for every device.
You can configure wireless in dewices with same identical SSID and Security profile to form one ESSID.