We have a product that considers installing on a client’s room a Mikrotik RouterBoard 951G 2HnD hosting a wifi network with WPA2 PSK security connected to our client’s network and an Android tablet connected to the Mikrotik’s wifi network. This solution has been installed on dozens of our clients’ sites and we have never had any issues before.
Two weeks ago the all the rooms (four) inside one of our client’s building started to have wifi problems. After working great for several hours, each tablet would disconnect (¿or be dropped?) from the RouterBoard 951G 2HnD’s wifi network and then immediately connect. After the first disconnection (or being dropped) and reconnection the tablet would keep disconnecting (or being dropped) and reconnecting from the wifi for several minutes, which would end in a final disconnection and then no more reconnection intents.
The solution worked great for months but after we changed the tablet this started happening. At the beginning we thought this was because we had changed the tablet but after trying with another Android tablets and an Android cell phone even, the problem would still occur. We also replaced the RouterBoard 951G 2HnD with one that was working great at another office and it also started to show this unusual behaviour.
Finally, we took one of the four room’s Android tablet and RouterBoard 951G 2HnD, and installed them on one of our laboratories without modifying any configuration on both of them. After almost 24 hours the problem has not replicated.
I thought activating the Wireless Debug Logs would us show more relevant information and help us focus our investigation on the right direction but it didn’t. Next are the log records:
jan/01/1970 20:03:17 interface,info ether1_Conexion_Hacia_LAN link up (speed 100M, full duplex) 
jan/01/1970 20:03:17 interface,info ether2_Conexion_Hacia_terminal_VC link up (speed 100M, full duplex) 
jan/01/1970 20:04:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jan/01/1970 20:04:01 dhcp,info dhcp1_My_Companys_Name assigned 192.168.89.2 to 0C:B3:19:29:08:13 
jan/01/1970 20:04:09 system,error,critical router was rebooted without proper shutdown 
jan/01/1970 20:04:20 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: sending station leaving (3) 
jan/01/1970 20:04:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jan/01/1970 20:07:28 interface,info ether4_Conexion_Hacia_dispositivos_internos link up (speed 100M, full duplex) 
jan/01/1970 20:07:41 system,info,account user admin logged in via winbox 
jan/01/1970 20:17:48 system,info,account user admin logged in via winbox 
jan/01/1970 20:17:50 system,info,account user admin logged out via winbox 
jan/01/1970 20:19:01 system,info,account user admin logged in via local 
jan/01/1970 20:19:28 system,info nat rule added by admin 
jan/01/1970 20:19:31 system,info nat rule added by admin 
jan/01/1970 20:19:31 system,info nat rule added by admin 
jan/01/1970 20:19:31 system,info nat rule added by admin 
jan/01/1970 20:22:51 interface,info ether4_Conexion_Hacia_dispositivos_internos link down 
jan/01/1970 20:22:52 system,info,account user admin logged out via winbox 
jan/01/1970 20:22:52 system,info,account user admin logged out via local 
jan/01/1970 20:33:20 interface,info ether2_Conexion_Hacia_terminal_VC link down 
jan/01/1970 20:33:23 interface,info ether2_Conexion_Hacia_terminal_VC link up (speed 100M, full duplex) 
jan/01/1970 23:59:08 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jan/01/1970 23:59:08 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jan/01/1970 23:59:08 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jan/02/1970 00:02:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jan/02/1970 00:02:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jan/02/1970 00:02:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jan/02/1970 00:05:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jan/04/1970 15:20:45 system,info,account user admin logged in from 10.252.163.33 via winbox 
jan/04/1970 15:37:52 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jan/04/1970 23:03:37 system,info,account user admin logged in from 10.252.163.33 via winbox 
jan/04/1970 23:03:48 system,info log rule added by admin 
jan/04/1970 23:04:07 system,info,account user admin logged out from 10.252.163.33 via winbox 
jan/04/1970 23:07:23 system,info SNTP client configuration changed by admin 
jun/15 15:53:43 system,info SNTP client configuration changed by admin 
jun/15 18:09:25 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:09:41 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:09:41 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:09:41 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:15:29 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:15:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:15:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:15:29 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:15:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:17:11 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:17:15 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:17:15 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:17:15 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:17:20 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:17:20 wireless,info wlan1_My_Companys_Name: data from unknown device 0C:B3:19:29:08:13, sent deauth 
jun/15 18:17:20 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:17:20 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:17:20 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:23:12 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:23:12 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:23:12 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:23:12 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:23:12 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:23:37 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:23:37 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:23:37 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:23:37 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:23:37 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:25:42 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:25:42 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:25:42 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:25:42 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:25:42 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:27:00 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:27:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:27:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:27:00 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:27:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:27:58 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:27:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:27:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:27:58 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:27:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:28:04 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:28:07 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:28:07 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:28:07 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:32:53 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:32:57 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:32:57 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:32:57 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:33:36 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:33:36 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:33:36 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:33:36 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:33:36 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:33:46 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:33:50 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:33:50 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:33:50 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:33:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, extensive data loss 
jun/15 18:33:56 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:33:56 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:33:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:34:15 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:34:15 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:34:15 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:34:15 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:34:15 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:34:54 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:34:54 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:34:54 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:34:54 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:34:54 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:35:16 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:35:16 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:35:16 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:35:16 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:35:16 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:37:17 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:37:17 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:37:17 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:37:17 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:37:17 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:37:23 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:37:27 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:37:27 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:37:27 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:38:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:38:04 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:38:04 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:38:04 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:38:35 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:38:35 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:38:35 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:38:35 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:38:35 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:38:41 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:38:41 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:38:41 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:38:41 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:41:52 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:41:52 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:41:52 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:41:52 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:41:52 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:41:58 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:41:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:41:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:41:58 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:41:58 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:43:05 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:43:06 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:43:06 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:43:06 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:44:32 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:44:32 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:44:32 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:44:32 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:44:32 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:45:10 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:45:10 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:45:10 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:45:10 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:45:10 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:47:24 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:47:24 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:47:24 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:47:24 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:47:24 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:47:25 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:47:25 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:47:25 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:47:25 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:47:25 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:47:25 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 2 frame received (6) 
jun/15 18:47:29 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:47:29 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:47:29 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:48:13 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:48:13 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:48:13 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:48:13 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:48:13 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:48:33 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:48:33 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:48:33 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:48:33 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:48:33 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:48:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:49:00 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:49:00 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:49:00 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:50:45 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:50:45 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:50:45 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:50:45 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:50:45 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:51:28 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:51:28 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:51:28 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:51:28 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:51:28 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:51:35 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:51:39 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:51:39 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:51:39 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:52:02 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:52:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:52:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:52:02 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:52:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:52:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:52:09 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:52:09 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:52:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:52:34 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:52:38 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:52:38 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:52:38 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:52:57 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:52:57 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:52:57 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:52:57 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:52:57 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:53:56 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:53:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:53:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:53:56 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:53:56 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:54:08 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:54:09 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:54:09 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:54:09 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:55:14 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:55:14 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:55:14 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:55:14 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:55:14 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:55:19 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:55:19 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:55:19 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:55:19 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:56:43 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
jun/15 18:56:47 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:56:47 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:56:47 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:57:02 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:57:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:57:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:57:02 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:57:02 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:57:03 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 attempts to associate 
jun/15 18:57:03 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: reassociating 
jun/15 18:57:03 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, ok 
jun/15 18:57:03 wireless,debug wlan1_My_Companys_Name: 0C:B3:19:29:08:13 in local ACL, accept 
jun/15 18:57:03 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: connected 
jun/15 18:57:03 wireless,info 0C:B3:19:29:08:13@wlan1_My_Companys_Name: disconnected, received deauth: class 3 frame received (7) 
Also, we did a spectrum analisis showing the usage of each channel (but only with networks with visible SSIDs) and the result was the following:
As the image shows we are currently working on channel 4 but previously when the problem started happening we were on channel 1.
We have no idea where to keep looking and why is this problem happening. Any help or guidance will be deeply appreciated.
Upate
I would like to add more detailed information about the usage of the wifi spectrum. At the beggining we were on channel one. Then we changed to channel 3 (see image on first post) but after using InSSIDer we ended changing to channel 10.
Next is the image of the test results when channel 3 was being used:
Next is the image of the test results after selecting the best wifi channel (which was channel 10):
I would love to hear more comments and thought. Everything that makes us do something we have not yet done.
EVEN MORE UPDATE
The next image presents the DHCP packets sent between the tablets and the Mikrotik router installed on one room:
The DHCP Discover and DHCP Request packets are sent from the tablets to the DHCP server (supposedly the Microtik's), which are not beeing answered with a DHCP Offer or a DHCP ACK. One could get confuzed and think that the packets #997, #995, #999 y #1004 respond to the requests that appear before but their transaction ID shows that they are from another set of requests. Same happens with the DHCP NAK. On the DHCP protocol the different messages from one communication flow will keep the transaction ID as shown in the next image:
Looking deeper on the DHCP Request messages sent by the tablets, we found one (packet #785) asking for the IP 172.22.198.134 to the DHCP server with the IP address 152.141.217.7, which is very weird because the only Wi-Fi network registrated on the test devices was the one with SSID CVNET62710 owned by the Mikrotik.
Additionally, the ARP packets were analized and we found that the device with a MAC address 00:00:0c:07:ac:64, that appears as All-HSRP-routers_64, reports that he has the IP address 172.22.198.1 and the IP address 192.168.89.1, being this last one the IP address of the Mikrotik's Wi-Fi interface that holds the DHCP server. Next you can see the evidence:
I'm thinking this duplicated IP is the reason the problem of the disconnection and reconnection on of the tablet of this room. Sadly, I haven't been able to take captures on the other rooms so I can't assure or discard that this could be happening on every one of them.
Also, today in the morning when we went to configure the IP address of the tablet (of the current room) as a static one, when we selected manual IP address we found that it was set to 192.168.1.128, which is not from the ranged delivered by the Mikrotik (192.168.89.X). It seems that it took the IP address of another network even though the only registrated Wi-Fi network is CVNET62710.
I would like to hear your thoughts on the matter and see if you think this could be causing the recurrent disconnection and reconnection of the devices.