Can't get inter vlan routing to work
						
					
					
				
			
		
	
			
	
	
	
	
	
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 09:47 AM - edited 03-08-2019 09:36 AM
Hello,
I have a home lab that consists of 2 3550's and 1 1841 router. I'm learning about VLANs and trunking, and how to configure them. I've included a topology of the exact network that I've build to give you a better understanding of what i'm trying to do. The topology has the same interface numbers, ip address ranges and device names, that i used in the physical lab. But I cannot even get the hosts to ping their vlan interfaces, default gateways, or the other host. All the pings come back as "destination host unreachable" on the hosts, what am i missing? I've included all of the devices full configuration logs, and some show commands that i think are useful. If anyone wants a specific show command, or anything, leave a comment and I will update the post as soon as possible. Thanks!
SW1
SW1#show run
Building configuration...
Current configuration : 2364 bytes
!
version 12.1
no service pad
service timestamps debug uptime
service timestamps log uptime
no service password-encryption
!
hostname SW1
!
!
ip subnet-zero
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/2
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20
 switchport mode trunk
!
interface FastEthernet0/3
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20
 switchport mode trunk
!
interface FastEthernet0/4
 switchport mode dynamic desirable
!
interface FastEthernet0/5
 switchport mode dynamic desirable
!
interface FastEthernet0/6
 switchport mode dynamic desirable
!
interface FastEthernet0/7
 switchport mode dynamic desirable
!
interface FastEthernet0/8
 switchport mode dynamic desirable
!
interface FastEthernet0/9
 switchport mode dynamic desirable
!
interface FastEthernet0/10
 switchport mode dynamic desirable
!
interface FastEthernet0/11
 switchport mode dynamic desirable
!
interface FastEthernet0/12
 switchport mode dynamic desirable
!
interface FastEthernet0/13
 switchport mode dynamic desirable
!
interface FastEthernet0/14
 switchport mode dynamic desirable
!
interface FastEthernet0/15
 switchport mode dynamic desirable
!
interface FastEthernet0/16
 switchport mode dynamic desirable
!
interface FastEthernet0/17
 switchport mode dynamic desirable
!
interface FastEthernet0/18
 switchport mode dynamic desirable
!
interface FastEthernet0/19
 switchport mode dynamic desirable
!
interface FastEthernet0/20
 switchport mode dynamic desirable
!
interface FastEthernet0/21
 switchport mode dynamic desirable
!
interface FastEthernet0/22
 switchport mode dynamic desirable
!
interface FastEthernet0/23
 switchport mode dynamic desirable
!
interface FastEthernet0/24
 switchport mode dynamic desirable
!
interface GigabitEthernet0/1
 switchport mode dynamic desirable
!
interface GigabitEthernet0/2
 switchport mode dynamic desirable
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 description VLAN10
 ip address 192.168.10.2 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.2 255.255.255.0
!
ip classless
ip http server
!
!
line con 0
 logging synchronous
line vty 0 4
 logging synchronous
 login
line vty 5 15
 logging synchronous
 login
!
!
end
SW1#show vlan br
VLAN Name                          Status   Ports
---- -------------------------------- --------- -------------------------------
1      default                           active  Fa0/4, Fa0/5, Fa0/6, Fa0/7
                                                         Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                         Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                         Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                         Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                         Fa0/24, Gi0/1, Gi0/2
10  VLAN10                          active  Fa0/1
20  VLAN20                          active
1002 fddi-default                  act/unsup
1003 token-ring-default        act/unsup
1004 fddinet-default             act/unsup
1005 trnet-default               act/unsup
SW1#show int tru
Port    Mode               Encapsulation      Status     Native vlan
Fa0/2   on                      802.1q             trunking          1
Fa0/3   on                      802.1q             trunking          1
Port Vlans allowed on trunk
Fa0/2 1,10,20
Fa0/3 1,10,20
Port Vlans allowed and active in management domain
Fa0/2 1,10,20
Fa0/3 1,10,20
Port Vlans in spanning tree forwarding state and not pruned
Fa0/2 1,10,20
Fa0/3 1,10,20
SW1#show int f0/1 switch
Name: Fa0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: negotiate
Operational Trunking Encapsulation: native
Negotiation of Trunking: Off
Access Mode VLAN: 10 (VLAN10)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: ALL
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
SW1#show int f0/2 switch
Name: Fa0/2
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 1,10,20
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
SW1#show int f0/3 switch
Name: Fa0/3
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 1,10,20
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
SW2
SW2#show run
Building configuration...
Current configuration : 1783 bytes
!
version 12.1
no service pad
service timestamps debug uptime
service timestamps log uptime
no service password-encryption
!
hostname SW2
!
!
ip subnet-zero
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface FastEthernet0/1
 switchport access vlan 20
 switchport mode access
 no ip address
!
interface FastEthernet0/2
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1,10,20
 switchport mode trunk
 no ip address
!
interface FastEthernet0/3
 no ip address
!
interface FastEthernet0/4
 no ip address
!
interface FastEthernet0/5
 no ip address
!
interface FastEthernet0/6
 no ip address
!
interface FastEthernet0/7
 no ip address
!
interface FastEthernet0/8
 no ip address
!
interface FastEthernet0/9
 no ip address
!
interface FastEthernet0/10
 no ip address
!
interface FastEthernet0/11
 no ip address
!
interface FastEthernet0/12
 no ip address
!
interface FastEthernet0/13
 no ip address
!
interface FastEthernet0/14
 no ip address
!
interface FastEthernet0/15
 no ip address
!
interface FastEthernet0/16
 no ip address
!
interface FastEthernet0/17
 no ip address
!
interface FastEthernet0/18
 no ip address
!
interface FastEthernet0/19
 no ip address
!
interface FastEthernet0/20
 no ip address
!
interface FastEthernet0/21
 no ip address
!
interface FastEthernet0/22
 no ip address
!
interface FastEthernet0/23
 no ip address
!
interface FastEthernet0/24
 no ip address
!
interface GigabitEthernet0/1
 no ip address
!
interface GigabitEthernet0/2
 no ip address
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan10
 ip address 192.168.10.3 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.3 255.255.255.0
!
ip classless
ip http server
!
!
!
line con 0
 logging synchronous
line vty 0 4
 login
line vty 5 15
 login
!
end
SW2#show vlan br
VLAN Name                          Status  Ports
---- -------------------------------- --------- -------------------------------
1    defaultn                           active  Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                         Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                         Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                         Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                         Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                         Fa0/23, Fa0/24, Gi0/1, Gi0/2
10 VLAN10                            active
20 VLAN20                            active  Fa0/1
1002 fddi-default                    active
1003 token-ring-default         active
1004 fddinet-default               active
1005 trnet-default                  active
SW2#show int tru
Port    Mode      Encapsulation   Status     Native vlan
Fa0/2   on             802.1q           trunking          1
Port Vlans allowed on trunk
Fa0/2 1,10,20
Port Vlans allowed and active in management domain
Fa0/2 1,10,20
Port Vlans in spanning tree forwarding state and not pruned
Fa0/2 1,10,20
SW2#show int f0/1 switch
Name: Fa0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: negotiate
Operational Trunking Encapsulation: native
Negotiation of Trunking: Off
Access Mode VLAN: 20 (VLAN20)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Operational private-vlan: none
Trunking VLANs Enabled: ALL
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Voice VLAN: none (Inactive)
Appliance trust: none
SW2#show int f0/2 switch
Name: Fa0/2
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Operational private-vlan: none
Trunking VLANs Enabled: 1,10,20
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Voice VLAN: none (Inactive)
Appliance trust: none
R1
R1#show run
Building configuration...
Current configuration : 952 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
no aaa new-model
!
resource policy
!
ip cef
!
!
!
!
!
!
!
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no snmp trap link-status
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no snmp trap link-status
!
interface FastEthernet0/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface Serial0/0/0
 no ip address
 shutdown
 no fair-queue
 clock rate 2000000
!
!
!
ip http server
no ip http secure-server
!
!
!
!
!
control-plane
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 logging synchronous
 login
line vty 5 15
 logging synchronous
 login
!
scheduler allocate 20000 1000
end
R1#show int f0/0
FastEthernet0/0 is up, line protocol is up
 Hardware is Gt96k FE, address is 001f.ca8c.37ec (bia 001f.ca8c.37ec)
 MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
 reliability 255/255, txload 1/255, rxload 1/255
 Encapsulation 802.1Q Virtual LAN, Vlan ID 1., loopback not set
 Keepalive set (10 sec)
 Full-duplex, 100Mb/s, 100BaseTX/FX
 ARP type: ARPA, ARP Timeout 04:00:00
 Last input 00:00:02, output 00:00:01, output hang never
 Last clearing of "show interface" counters never
 Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
 Queueing strategy: fifo
 Output queue: 0/40 (size/max)
 5 minute input rate 0 bits/sec, 0 packets/sec
 5 minute output rate 0 bits/sec, 0 packets/sec
 1063 packets input, 188027 bytes
 Received 826 broadcasts, 0 runts, 0 giants, 0 throttles
 0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
 0 watchdog
 0 input packets with dribble condition detected
 991 packets output, 111728 bytes, 0 underruns
 0 output errors, 0 collisions, 7 interface resets
 0 babbles, 0 late collision, 0 deferred
 0 lost carrier, 0 no carrier
R1#show int f0/0.10
FastEthernet0/0.10 is up, line protocol is up
 Hardware is Gt96k FE, address is 001f.ca8c.37ec (bia 001f.ca8c.37ec)
 Internet address is 192.168.10.1/24
 MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
 reliability 255/255, txload 1/255, rxload 1/255
 Encapsulation 802.1Q Virtual LAN, Vlan ID 10.
 ARP type: ARPA, ARP Timeout 04:00:00
 Last clearing of "show interface" counters never
R1#show int f0/0.20
FastEthernet0/0.20 is up, line protocol is up
 Hardware is Gt96k FE, address is 001f.ca8c.37ec (bia 001f.ca8c.37ec)
 Internet address is 192.168.20.1/24
 MTU 1500 bytes, BW 100000 Kbit, DLY 100 usec,
 reliability 255/255, txload 1/255, rxload 1/255
 Encapsulation 802.1Q Virtual LAN, Vlan ID 20.
 ARP type: ARPA, ARP Timeout 04:00:00
 Last clearing of "show interface" counters never
- Labels:
- 
						
							
		
			LAN Switching
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 11:23 AM
Your configuration looks fine to me.
On sw2 can you ping 192.168.20.1 using 192.168.20.3 as the source IP (use an extended ping).
If you can suggest you look at PC settings.
Jon
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 11:49 AM
Hello Jon, thanks for your reply.
The ping was successful!
SW2#ping
Protocol [ip]:
Target IP address: 192.168.20.1
Repeat count [5]:
Datagram size [100]:
Timeout in seconds [2]:
Extended commands [n]: y
Source address or interface: 192.168.20.3
Type of service [0]:
Set DF bit in IP header? [no]:
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose[none]:
Sweep range of sizes [n]:
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.20.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/201/1000 ms
SW2#
And the PC settings are as follows:
VLAN10/PC
192.168.10.10
255.255.255.0
192.168.10.1
VLAN20/PC
192.168.20.10
255.255.255.0
192.168.20.1
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 12:01 PM
It looks like a problem with either the PCs or the ports on the switch that the PCs connect to.
Jon
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 11:54 AM
As Jon noted there doesn't appear to be anything wrong with you config; I would however make a best practice suggestion that will simplify your life when troubleshooting and save clutter on your network:
There really is no need to create a SVI on each switch for every VLAN in the network. Create a management VLAN that is independent of your data networks. Assign only one VLAN in each switch, that of the management VLAN. This would include creating a management VLAN sub-interface on fa0/0 of the 1841 and each of the 3550s. Removing VLAN 10 & 20 from each of the 3550s.
When you get the trunk connected correctly you will be able to ping the management VLAN of each device. Once this happens everything else will fall into place. Here is a good configuration example using management VLANs on the network equipment.
http://www.cisco.com/c/en/us/support/docs/switches/catalyst-3750-series-switches/45002-intervlan3750-45002.html
Cheers,
Sam
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 02:27 PM
Thanks for your reply.
That seems like a very good best practice suggestion, but I haven't understood it fully. For example, doesn't the port f0/1 on SW2 need to have the following config?
interface FastEthernet0/1
switchport access vlan 20
switchport mode access
So, should I remove the vlans that dont directly connect to the switches, and leave the ones that do?
And would I have to assign the management vlan to any ports?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 05:18 PM
You are correct in what you are saying; in that fa0/1 needs to be configured for access on VLAN 20. When you create the trunks between each device you can specify which VLANs are allowed through the trunk or if you do not specify switchport trunk allowed vlan command ALL VLANs are allowed through the trunk.
Any time you create interVLAN routing you have to have a Layer 3 device (router or L3 switch) that is assigned IP addresses for a given VLAN. Each VLAN must be assigned a Switch Virtual Interface (SVI). In your case this is the 1841, this must have a trunk interface for each VLAN you have throughout your network. The trunk between the 1841 and SW1 will carry whichever VLANs you have assigned in the 1841. Remember that trunking is a Layer 2 protocol and doesn't care what IP address is assigned to the SVI at this point. The only thing your trunk knows is that you currently have 2 VLANs (10 & 20, 3 if you add a management VLAN).
On the switch side of the trunk the only thing the switch sees are the VLANs that the router is sending, 10, 20 & management. In order to assign a client on the switch to a specific VLAN you simply issue the commands that you have on fa0/1 on SW1 & SW2. Of course you could have fa0/34 assigned to VLAN 10 and fa0/35 assigned to VLAN 20 without any problems. The trunk between your SW1 and SW2 simply mirrors what the router is sending to SW1 provided you allow the same VLANs through the trunk. One word of warning, when you assign to a switchport be sure that the VLAN is in the VLAN database. On the 3550 inter vlan data from the enable prompt and type add vlan x (x=vlan number). To review which VLANs are on a given switch issue the show vlan command.
The management VLAN is simply there for communication (telnet, SSH) to the individual switches. You do not need to assign any ports to the management VLAN. Simply make sure that the VLAN is in the vlan database. Once the SVI is created and the VLAN is in the database you should be able to ping each device in the management network. Finally assign the IP address of the management interface in the router as the gateway on both switches.
Regards,
Sam
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-05-2017 11:52 PM
Thanks for your clarification. I was under the impression, that if I gave a port access to a vlan, (e.g f0/1 on SW2) that the corresponding SVI was needed in the switch with a configured ip address. But instead, I should use the sub-interfaces f0/0.10 & f0/0.20 on the router as the vlan SVI's, am I correct in this assumption?
The design makes much more sense now that i understand it more. I will configure my network as soon as possible and let you know how everything works. Thanks again for taking the time to explain!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 02:58 AM
You are correct, using the example config referenced above you will see that the 2950 and 2948 have a sole VLAN (10). All other VLANs are defined on the L3 device, in this case the 3750 stack.
Regards,
Sam
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 01:33 PM
Hello Sam,
I believe I have configured the network correctly, but I am still unable to ping between the hosts. I can now ping my default gateway, 192.168.10.1, from VLAN10/PC, but no other sub interfaces on the router.
SW1
SW1#show run
Building configuration...
Current configuration : 2415 bytes
!
version 12.1
no service pad
service timestamps debug uptime
service timestamps log uptime
no service password-encryption
!
hostname SW1
!
!
ip subnet-zero
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
interface FastEthernet0/1
switchport access vlan 10
switchport mode access
!
interface FastEthernet0/2
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 1,10,20,99
switchport mode trunk
!
interface FastEthernet0/3
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 1,10,20,99
switchport mode trunk
!
-----Omitted lines-----
!
interface Vlan1
no ip address
shutdown
!
interface Vlan99
ip address 192.168.99.3 255.255.255.0
!
ip default-gateway 192.168.99.1
ip classless
ip http server
!
!
line con 0
logging synchronous
line vty 0 4
logging synchronous
login
line vty 5 15
logging synchronous
login
!
!
End
SW1#show vlan br
VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/4, Fa0/5, Fa0/6, Fa0/7
Fa0/8, Fa0/9, Fa0/10, Fa0/11
Fa0/12, Fa0/13, Fa0/14, Fa0/15
Fa0/16, Fa0/17, Fa0/18, Fa0/19
Fa0/20, Fa0/21, Fa0/22, Fa0/23
Fa0/24, Gi0/1, Gi0/2
10 VLAN10 active Fa0/1
99 MANAGEMENT active
1002 fddi-default act/unsup
1003 token-ring-default act/unsup
1004 fddinet-default act/unsup
1005 trnet-default act/unsup
SW2
SW2#show run
Building configuration...
Current configuration : 1760 bytes
!
version 12.1
no service pad
service timestamps debug uptime
service timestamps log uptime
no service password-encryption
!
hostname SW2
!
!
ip subnet-zero
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
interface FastEthernet0/1
switchport access vlan 20
switchport mode access
no ip address
!
interface FastEthernet0/2
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 1,10,20,99
switchport mode trunk
no ip address
!
-----Omitted lines-----
!
interface Vlan1
no ip address
shutdown
!
interface Vlan99
ip address 192.168.99.2 255.255.255.0
!
ip default-gateway 192.168.99.1
ip classless
ip http server
!
!
!
line con 0
logging synchronous
line vty 0 4
login
line vty 5 15
login
!
End
SW2#show vlan br
VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------
1 default active Fa0/3, Fa0/4, Fa0/5, Fa0/6
Fa0/7, Fa0/8, Fa0/9, Fa0/10
Fa0/11, Fa0/12, Fa0/13, Fa0/14
Fa0/15, Fa0/16, Fa0/17, Fa0/18
Fa0/19, Fa0/20, Fa0/21, Fa0/22
Fa0/23, Fa0/24, Gi0/1, Gi0/2
20 VLAN20 active Fa0/1
99 MANAGEMENT active
1002 fddi-default active
1003 token-ring-default active
1004 fddinet-default active
1005 trnet-default active
R1
R1#show run
Building configuration...
Current configuration : 1072 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
no aaa new-model
!
resource policy
!
ip cef
!
!
!
!
!
!
!
!
!
!
!
!
interface FastEthernet0/0
no ip address
duplex auto
speed auto
!
interface FastEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
no snmp trap link-status
!
interface FastEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
no snmp trap link-status
!
interface FastEthernet0/0.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
no snmp trap link-status
!
interface FastEthernet0/1
no ip address
shutdown
duplex auto
speed auto
!
interface Serial0/0/0
no ip address
shutdown
no fair-queue
clock rate 2000000
!
!
!
ip http server
no ip http secure-server
!
!
!
!
!
control-plane
!
!
!
line con 0
logging synchronous
line aux 0
line vty 0 4
logging synchronous
login
line vty 5 15
logging synchronous
login
!
scheduler allocate 20000 1000
end
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 01:33 PM
From the PC in vlan 10 can you ping 192.168.20.1 ?
If you can then from the same PC can you ping both 192.168.99.2 and .3.
If you can do that try pinging either PC from the router and let us know.
Jon
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 02:35 PM
From the PC in VLAN10 i can only ping 192.168.10.1. And from the pc in VLAN20 i cant ping anywhere.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 03:00 PM
While logged into the router can you ping 192.168.99.1, 192.168.99.2 and 192.168.99.3? Also could you post the show vlan-switch command from the router? Finally could you also post the show ip route command from the router?
Regards,
Sam
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 03:09 PM
Yes, I remember I could ping to 99.2 & 99.3 from 99.1. But I cant access my lab at the moment. I'll reply with the show commands asap.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-06-2017 03:17 PM
When you do get access can you do a ping from sw2 and ping both 192.168.10.1 and 192.168.20.1 which would test the routing between vlans.
Just trying to narrow down the problem.
Jon