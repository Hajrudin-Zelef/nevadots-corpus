- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2024 12:50 PM - edited 10-30-2024 06:15 AM
Goal: Create 2 VLANs on a switch (for now, will expand to 4 - have 8 ports on the switch) for a test lab, not for production. Connect devices on these ports (direct connection) and #1. verify ping to VLAN IP, and other devices, and #2. verify ability to talk between devices on the same VLAN, and #3. verify ability to talk between devices on different VLANs. This switch will never connect to a router for Internet access, it will only act as a L3 Switch for a relatively small network
Device Info: Cisco Catalyst 3560-CX Series PD Cisco IOS Software Version 15.2(4)E8
show version
License level: ipbase
License Type: Default. No Valid License found
What has been done so far on this switch:
enable
config terminal
hostname no_internet_switch
vlan 10
interface Gi0/1 (and Gi0/2)
switchport mode access
switchport access vlan 10
interface vlan 10
ip address 192.168.10.1 subnet mask 255.255.255.0
<< Repeated the same seps for VLAN 20, with Gi0/3 and Gi0/4) >>
ip routing
show vlan
1   default                    active     Gi0/3, Gi0/4, Gi0/5, Gi0/6, Gi0/7, Gi0/8
10  vlan1                     active     Gi0/1, Gi0/2
20  vlan2                     active     Gi0/3, Gi0/4
show ip route
Gateway of last resort is not set
192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C   192.168.10.0/24 is directly connected, Vlan 10
L   192.168.10.1/32 is directly connected, Vlan 10
C   192.168.20.0/24 is directly connected, Vlan 20
L   192.168.20.1/32 is directly connected, Vlan 20
show ip interface brief
Vlan1                        unassigned         Yes   unset      up                     down
Vlan10                      192.168.10.1      Yes   unset      up                     down
Vlan20                      192.168.20.1     Yes   unset      up                     down
Here is the problem:
When I got the switch, did not check everything, set this up, same VLAN ping worked. Inter VLAN communications did not work. During the course of debugging, did a factory reset and now nothing works. Can someone please point out whats going on ? I am fairly new to this area, and have seen lots of questions like this here, but none where there is a requirement to not have a Router. I have the Vlan IPs as the default gateway on the Linux devices I have connected on the switch. Any help is greatly appreciated.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Catalyst 3000
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-30-2024 06:06 AM
Hi,
It all looks good, meaning you have ports Gi0/1, Gi0/2 in VLAN10 and Gi0/3, Gi0/4 in VLAN 20 (except that on STP output for VLAN 20, port Gi0/4 does not show up; did you not paste complete output or have disabled STP on that port via BPDFilter?); assuming hosts connected to these ports have IP addresses from the correct subnet, you should be able to have connectivity between PC's and default gateway which is the switch, while based on routing also between hosts in different VLAN's assuming you'v set the correct gateway IP address (the switch) on the hosts. Not sure what OS are the hosts running, however, try to ping the switch from hosts (the other way around, from switch to hosts may not work as maybe hosts have firewall turned on which filters ICMP packets). Based on the ARP and MAC table, you should at leat be able to ping from host 192.168.10.2 to switch which is 192.168.10.1.
ARP entries on the switch will show up only if there is IP communication between switch and host (if you ping the switch from all hosts, the switch should have all ARP entries); MAC entries will show up on switch only if the hosts sends any kind of traffic for switch to learn the MAC address.
Best,
Cristian.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2024 01:12 PM
In order to route between vlan, you need the command "ip routing", that's it. AS your switch have ipbase license, it is able to accept the command and do the needfull
Factory reset is not reason to stopping working. Can you share the show running-config?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2024 02:14 PM
Thank you for the response, greatly appreciated. I did run the "ip routing" command. Please see the show running-config output and please see if there are any mistakes in the cisco switch setup.
Building Configuration
Current Configuration
version 15.2
no service pad
no service password-encryption
!
hostname xxxxxx
!
boot-start-marker
boot-end-marker
!
!
no aaa new-model
system mtu routing 1500
!
!
!
!
!
ip routing
!
!
!
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
!
!
vlan internal allocation policy ascending
!
!
!
!
!
interface GigabitEthernet0/1
switchport access vlan 10
switchport mode access
!
interface GigabitEthernet0/2
switchport access vlan 10
switchport mode access
!
interface GigabitEthernet0/3
switchport access vlan 20
switchport mode access
!
interface GigabitEthernet0/4
switchport access vlan 20
switchport mode access
!
interface GigabitEthernet0/5
!
interface GigabitEthernet0/6
!
interface GigabitEthernet0/7
!
interface GigabitEthernet0/8
!
interface Vlan1
no ip address
!
interface Vlan10
ip address 192.168.10.1 255.255.255.0
!
interface Vlan20
ip address 192.168.20.1 255.255.255.0
!
ip forward-protocol nd
!
ip http server
ip http secure server
!
line con 0
line vty 5 15
!
!
end
-----------
Note about the host Linux PCs am testing with :
The hosts I am testing them with have some bridges setup for QEMU and maybe expecting tags - so if the switch configuration looks good, I can switch start looking for issues on the connected hosts. so far, all I have done on the hosts are these commands
sudo ip addr add 192.168.10.2/255.255.255.0 dev enp0s25 (a static ip in the 10 subnet)
sudo ip route add 0.0.0.0 via 192.168.10.1 --> which is Vlan 10's IP on the switch
Please let me know, thank you so much for the help!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2024 02:19 PM - edited 10-28-2024 02:40 PM
There is nothing need except
No shut down
Needed under vlan SVI
That all
MHM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 06:46 AM
thanks @MHM Cisco World can you please confirm if this is what you mean ?
#Assign Ports
interface GigabitEthernet0/<x>
no shutdown ?????
switchport mode access
switchport access vlan <vlan id>
or is this what you mean ?
#enable VLAN to VLAN communications
ip routing
no shutdown ?????
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 07:09 AM
I see this before
show ip interface brief
Vlan1                        unassigned         Yes   unset      up                     down
Vlan10                      192.168.10.1      Yes   unset      up                     down
Vlan20                      192.168.20.1     Yes   unset      up                     down
then you now share below
show ip interface brief
Vlan1 unassigned Yes unset up down
Vlan10 192.168.10.1 YES manual up up
Vlan20 192.168.20.1 YES manual up up
So the VLAN SVI are both UP not problem
only check if you connect correct PC to correct VLAN, i.e. PC with IP in subnet 192.168.10.x must connect to port assign to vlan 10 and PC with IP in subnet 192.168.20.x must connect to port assign to vlan20
MHM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2024 02:31 PM - edited 10-28-2024 02:37 PM
Send the commnad of "show ip int br" please
did you create the vlan on the switch with the command
conf t
vlan 10
exit
vlan 20
?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 06:43 AM
@Flavio Miranda here is the output - my apologies for the delay, I do not have remote access to this particular environment makes it hard to respond after evening.
show ip interface brief
Vlan1 unassigned Yes unset up down
Vlan10 192.168.10.1 YES manual up up
Vlan20 192.168.20.1 YES manual up up
GigabitEthernet0/1 unassigned YES unset up up
GigabitEthernet0/2 unassigned YES unset up up
GigabitEthernet0/3 unassigned YES unset up up
GigabitEthernet0/4 unassigned YES unset up up
GigabitEthernet0/5 unassigned YES unset down down
GigabitEthernet0/6 unassigned YES unset down down
GigabitEthernet0/7 unassigned YES unset down down
GigabitEthernet0/8 unassigned YES unset down down
GigabitEthernet0/9 unassigned YES unset down down
GigabitEthernet0/10 unassigned YES unset down down
For your question#2 this is the notes and sequence I used to create them
Config 1/4
----------
enable
config termninal
hostname <name>
show vlan
vlan <vlan id>
#Assign Ports
interface GigabitEthernet0/<x>
switchport mode access
switchport access vlan <vlan id>
#Assign IPs
interface vlan <vlan id>
ip address <match the subnet ids, start with 1, not zero> <subnet mask 255.255.255.0>
#to check everything
ip interface brief
#enable VLAN to VLAN communications
ip routing
Please let me know if you see something not right
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 07:40 AM
@bob0198labops dont worry
Something has change since your first post. The vlan were down
show ip interface brief
Vlan1 unassigned Yes unset up down
Vlan10 192.168.10.1 Yes unset up down
Vlan20 192.168.20.1 Yes unset up down
They are up now
show ip interface brief
Vlan1 unassigned Yes unset up down
Vlan10 192.168.10.1 YES manual up up
Vlan20 192.168.20.1 YES manual up up
Still can not communicate?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 08:41 AM
Yes, I just tried with 3 devices connected none of them are able to ping each other. I tried pinging the Vlan Ips, the other device ips, all come back with "Destination Host Unreachable"
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 09:25 AM
I might say the problem does not seems to be on the switch any more. If you have interface up, vlans UP and ip routing. there is no reason to not ping.
"The hosts I am testing them with have some bridges setup for QEMU and maybe expecting tags - so if the switch configuration looks good, I can switch start looking for issues on the connected hosts. so far, all I have done on the hosts are these commands
sudo ip addr add 192.168.10.2/255.255.255.0 dev enp0s25 (a static ip in the 10 subnet)
sudo ip route add 0.0.0.0 via 192.168.10.1 --> which is Vlan 10's IP on the switch"
As you are running linux, I think it is a good idea to check firewall on the host.
Eventually, test with windows os to make sure. The switch seems to be fine now to me.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 09:30 AM - edited 10-29-2024 10:55 AM
Hi,
Can you ping from the switch to any host in any of the VLAN's? Please provide confirmation. From switch side, provide following outputs: "show vlan brief", "show vlan id 10", "show vlan id 20", "show interfaces trunk", "show ip interface brief", "show spanning-tree vlan 10", "show spanning-tree vlan 20", "show ip route", "show ip cef", "show ip arp", "show mac address-table", "show version".
Best,
Cristian.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2024 12:29 PM - edited 10-30-2024 05:41 AM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-30-2024 06:06 AM
Hi,
It all looks good, meaning you have ports Gi0/1, Gi0/2 in VLAN10 and Gi0/3, Gi0/4 in VLAN 20 (except that on STP output for VLAN 20, port Gi0/4 does not show up; did you not paste complete output or have disabled STP on that port via BPDFilter?); assuming hosts connected to these ports have IP addresses from the correct subnet, you should be able to have connectivity between PC's and default gateway which is the switch, while based on routing also between hosts in different VLAN's assuming you'v set the correct gateway IP address (the switch) on the hosts. Not sure what OS are the hosts running, however, try to ping the switch from hosts (the other way around, from switch to hosts may not work as maybe hosts have firewall turned on which filters ICMP packets). Based on the ARP and MAC table, you should at leat be able to ping from host 192.168.10.2 to switch which is 192.168.10.1.
ARP entries on the switch will show up only if there is IP communication between switch and host (if you ping the switch from all hosts, the switch should have all ARP entries); MAC entries will show up on switch only if the hosts sends any kind of traffic for switch to learn the MAC address.
Best,
Cristian.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-30-2024 07:34 AM
@Cristian Matei Thank you so much, I believe I am one step closer to the solution at this point. my hosts are linux machines, and had someone make some changes to remove bridges running on both the hosts today, so now
the host connected to VLAN 10 is able to successfully ping the gateway IP - that was not working so far
the host connected to VLAN 20 is NOT able to successfully ping the gateway IP "Destination Host Unreachable" which means there is some issue on the switch. Here are the ONLY differences I saw today, with your list of commands (ran all of them again this morning)
192.168.10.0/24 attached Vlan10
192.168.10.0/32 receive Vlan10
192.168.10.1/32 receive Vlan10
192.168.10.2/32 attached Vlan10
192.168.10.255/32 receive Vlan10
192.168.20.0/24 attached Vlan20
192.168.20.0/32 receive Vlan20
192.168.20.1/32 receive Vlan20
192.168.20.2/32 attached Vlan20 <----- NEW LINE TODAY, AFTER CHANGES TO HOST CONNECTED ON VLAN 20
192.168.20.255/32 receive Vlan20
Protocol Address Age(min) Hardware Addr Type Interface
Internet 192.168.10.1 - 308b.b2e2.c3c1 ARPA Vlan10
Internet 192.168.10.2 44 20c5.eb9a.299c ARPA Vlan10
Internet 192.168.20.1 - 308b.b2e2.c3c2 ARPA Vlan20
Internet 192.168.20.2 47 20c5.eb9a.298d ARPA Vlan20 <----- NEW LINE TODAY, AFTER CHANGES TO HOST CONNECTED ON VLAN 20
Can you please explain "(except that on STP output for VLAN 20, port Gi0/4 does not show up; did you not paste complete output or have disabled STP on that port via BPDFilter?);" I did not have any hosts connected on Gi0/4 - can you explain this in a bit more detail ? One last step before I can 100% declare the Switch is good, mark yours as the right answer (helping with all the commands to make sure I can verify if the config was good). Or any other suggestions on why I would get a "Destination Host Unreachable" for a host on Vlan 20 and not on Vlan 10.
Once again, thanks much !