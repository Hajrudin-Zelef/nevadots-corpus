Super User is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
2
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
My UniFi log shows this:
Multiple devices are using the same IP address: xxx. Please check each device's configuration to ensure none are communicating with a rogue DHCP server.
I can also see that an iPad connected to the WiFi and received that IP address xxx two minutes prior.
But I can't find any other mention of IP address xxx in the logs, except for earlier connections from the same iPad on that IP address.
Is there any other way to find exactly which two devices clashed on that IP address?
Run arping for the IP address, from any device that's directly attached to the subnet. (It's present in UniFi UAP firmware via SSH or "Debug Console" if you don't have any other Linux system.)
arping -b -I eth0 <the_address> for the iputils and busybox versions, the -b is necessary when hunting for duplicates
arping -i eth0 <the_address> for the Thomas Habets version
(Before doing that, get the correct interface name from ip addr.)
If there are two devices with the same address, at some point the output will show two ARP responses from different MAC addresses.