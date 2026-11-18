Super User is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
1
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
As imposed by my government, my ISP blocks some sites, like Facebook and YouTube.
The ISP's MikroTik device redirects the blocked websites to a local IP address (192.168.222.66), which says that the site is blocked.
How can I get a list of all sites that are blocked? (I'm not trying to access the blocked sites; I know how to access them, but I just want to get a list.)
I tried some nslookup commands to get the domains given the local IP address, but to no avail.
Using a proxy server is always a good and would most likely resolve your issue. Another potential option is to change your DNS servers to 8.8.8.8 or 8.8.4.4 (Google Public DNS) and see if that works. If neither of those options work, most likely due to packet inspection from the provider, using a Encrypted VPN should work.
Most filters only block based on domain names and since the domain name is going over ssl the ISP's filter will only see an IP address pass through the system.
You can also use something like ultrasurf if you want to go the proxy/vpn route.