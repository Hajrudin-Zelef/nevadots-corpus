# How to allow responding to broadcast pings on Mikrotik RouterOS?

*Source : https://serverfault.com/questions/955423/how-to-allow-responding-to-broadcast-pings-on-mikrotik-routeros | Site : serverfault.com | Score : 1*

By default, most systems don't respond to broadcast ping requests. However, there exist ways to explicitly allow responding to broadcast ICMP requests on multiple systems, such as Linux.

How can I enable responding to broadcast ping requests on Mikrotik?

Using /tool sniffer start and /tool sniffer packet print I was able to verify that the broadcast packets do indeed reach the Mikrotik routers, but a response is never sent.