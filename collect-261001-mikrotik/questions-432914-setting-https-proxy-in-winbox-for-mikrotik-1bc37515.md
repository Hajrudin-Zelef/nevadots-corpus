Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
5
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I am using MikroTik router model RB750, I need to use HTTPS proxy method for some of my clients.
The Proxy Server (VPS) is in another country and i have the IP address and the Port Number and a username and password for connection, in the Proxy Server i have a CCPROXY program.
i have a VPN in router but i need th set HTTPS proxy in the same way to enable it for some client and disable it for some.
Can anyone tell me how to do it in Winbox software do?
Unfortunately Mikrotik RouterOS does not support acting as HTTPS proxy. Also if you check further it does not have any options for SSL certificates regarding to its proxy.
You can use Linux/squid or Microsoft ISA / TMG for HTTPS scenario.
If you want, Mikrotik has HTTP proxy with authentication in IP > Web Proxy.