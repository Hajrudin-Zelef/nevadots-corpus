I have attempted and followed many guides and am still having issues allowing traffic from my VPN to access devices on the LAN.
https://rbgeek.wordpress.com/2014/08/26/pptp-server-setup-on-mikrotik/ Is an example of one guide I have followed, and although I can connect, and ping from LAN > VPN I am unable to ping from VPN > LAN
I have the following setup:
Routerboard 750 WebFig v6.35.1 (stable)
LAN 192.168.88.0/24
PPTP Pool 192.168.200.10-192.168.200.20
PPTP Server: Enabled
PPTP Profile created using the PPTP IP Pool for both internal and external addresses
/ip pool
add name=dhcp ranges=192.168.88.100-192.168.88.254
add name=pptp-pool ranges=192.168.200.1-192.168.200.10<
# may/31/2016 23:02:50 by RouterOS 6.35.1
# software id = 8RIQ-2NZU
#
/ip firewall filter
add chain=input comment="default configuration" protocol=icmp
add chain=input comment="VPN PPTP ACCEPT" dst-port=1723 log=yes protocol=tcp
add chain=input comment="GRE ACCEPT" log=yes protocol=gre
add chain=input comment="default configuration" connection-state=established
add chain=input comment="default configuration" connection-state=related
add chain=input comment="allow l2tp" dst-port=1701 protocol=udp
add chain=input comment="allow sstp" dst-port=443 protocol=tcp
add chain=input comment="web access for config" dst-port=80 in-interface=ether1-gateway log=yes log-prefix=remote-access protocol=tcp
add action=drop chain=input comment="default configuration" in-interface=ether1-gateway
/ip firewall nat
add action=masquerade chain=srcnat comment="default configuration" out-interface=ether1-gateway to-addresses=0.0.0.0
# may/31/2016 23:03:52 by RouterOS 6.35.1
# software id = 8RIQ-2NZU
#
/ppp profile
add local-address=pptp-pool name=pptp-profile remote-address=pptp-pool
set *FFFFFFFE dns-server=0.0.0.0 use-compression=yes
/ppp secret
add name=USERNAME password=PASSWORD profile=pptp-profile service=pptp
From the logs
20:20:35 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:35 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 220
20:20:35 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:35 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 76
20:20:35 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 60
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 59
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 98
20:20:36 pptp,ppp,info,account USERNAME logged in, 192.168.200.10
20:20:36 pptp,ppp,info <pptp-USERNAME>: authenticated
20:20:36 pptp,ppp,info <pptp-USERNAME>: terminating...
20:20:36 pptp,ppp,info,account USERNAME logged out, 1 18 28 3 4
20:20:36 pptp,ppp,info <pptp-USERNAME>: disconnected
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 44
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 44
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 74
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,FIN), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,FIN), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:36 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14320->82.XXX.XXX.177:1723, len 52
20:20:41 system,info PPTP Server settings changed by admin
20:20:41 system,info PPTP Server settings changed by admin
20:20:43 pptp,info TCP connection established from 82.132.216.62
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 64
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 208
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 220
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 76
20:20:43 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 60
20:20:44 pptp,ppp,info,account USERNAME logged in, 192.168.200.10
20:20:44 pptp,ppp,info <pptp-USERNAME>: authenticated
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 59
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 98
20:20:44 pptp,ppp,info <pptp-USERNAME>: using encoding - MPPE128 stateless
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 44
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 62
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 54
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 56
20:20:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 62
20:20:45 pptp,ppp,info <pptp-USERNAME>: connected
20:20:45 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 50
20:20:45 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 334
20:20:48 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 334
20:20:51 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 334
20:20:54 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 334
20:20:57 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 334
20:21:04 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:21:04 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 40
20:21:24 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:21:25 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 40
20:21:44 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:21:45 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 40
20:22:04 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:22:06 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 40
20:22:24 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 48
20:22:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 40
20:22:32 pptp,ppp,info <pptp-USERNAME>: terminating...
20:22:32 pptp,ppp,info,account USERNAME logged out, 109 1568 98 14 8
20:22:32 pptp,ppp,info <pptp-USERNAME>: disconnected
20:22:32 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 57
20:22:32 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto 47, 82.132.216.62->82.XXX.XXX.177, len 57
20:22:32 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,FIN), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:22:32 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,FIN), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
20:22:32 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 82.132.216.62:14321->82.XXX.XXX.177:1723, len 52
22:00:44 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 93.174.93.94:47264->82.XXX.XXX.177:80, len 40
22:01:09 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 61.240.144.64:48406->82.XXX.XXX.177:80, len 40
22:09:42 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 94.102.49.54:22->82.XXX.XXX.177:80, len 40
22:57:26 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 123.151.149.222:22200->82.XXX.XXX.177:1723, len 40
22:57:26 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (RST), 123.151.149.222:22200->82.XXX.XXX.177:1723, len 40
22:57:26 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (RST), 123.151.149.222:22200->82.XXX.XXX.177:1723, len 40
22:57:26 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 48
22:57:27 pptp,info TCP connection established from 123.151.42.61
22:57:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 40
22:57:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,PSH), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 196
22:57:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 40
22:57:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK,FIN), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 40
22:57:27 firewall,info input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (ACK), 123.151.42.61:17122->82.XXX.XXX.177:1723, len 40
22:59:34 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 141.212.122.151:47113->82.XXX.XXX.177:80, len 40
22:59:34 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 141.212.122.152:38568->82.XXX.XXX.177:80, len 40
22:59:35 firewall,info remote-access input: in:ether1-gateway out:(none), src-mac 00:01:5c:82:ee:47, proto TCP (SYN), 141.212.122.145:45406->82.XXX.XXX.177:80, len 60
I have also enabled proxy-arp on the LAN connections.
I don't know why I cant get traffic to route from VPN > LAN.