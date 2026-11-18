FORTIGATE WORKBOOK 
FORTIGATE ADMINISTRATOR 
VERSION 1 
MARYAM ABOOTORABI 
Last Update: February 2026

i 
Workbook License and Copyright Agreement 
This workbook, with all text and graphics, is protected by copyright, with all rights reserved by SMenode Labs. 
It is provided exclusively for personal or educational use. Any sale, rental, modification, creation of derivative 
works, or sharing of this material online is strictly prohibited without prior written consent. Limited reproduction 
for personal or educational purposes is permitted, provided that all copyright notices remain intact. Proper 
credit must be given to the author(s) when quoting or referencing content, along with the copyright notices. 
Disclaimer 
This workbook is offered solely for informational and educational purposes. The designs and content within are 
not validated for production and should not be implemented in a production environment without 
comprehensive case studies and risk analysis. Any violation of the terms outlined herein may result in the 
immediate termination of the license. Upon termination, all copies of the workbook must be permanently 
destroyed. For permissions, licensing requests, or further inquiries, please contact the author(s) at 
info@smenode-labs.com.

COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 
1-
1 
 
 
Table of Contents ............................................................................................................................................................... 1-1 
1 FortiGate Course Materials ....................................................................................................................................... 1-5 
1.1 Files and Documents ........................................................................................................................................... 1-5 
1.2 Overview .............................................................................................................................................................. 1-5 
2 FortiGate LAB ............................................................................................................................................................. 2-7 
2.1 Topology ............................................................................................................................................................... 2-8 
2.2 IP Addressing ....................................................................................................................................................... 2-8 
2.3 LAB Topology Setup ............................................................................................................................................ 2-9 
2.3.1 DMZ Zone ........................................................................................................................................................ 2-9 
2.3.2 Data Center Zone ........................................................................................................................................... 2-10 
2.3.3 LAN Zone ....................................................................................................................................................... 2-11 
2.3.4 OOB................................................................................................................................................................ 2-12 
2.3.5 Core Infrastructure ......................................................................................................................................... 2-13 
2.4 Basic Setup from factory default ........................................................................................................................ 2-13 
2.4.1 Getting Familiar with CLI ................................................................................................................................ 2-13 
2.4.2 Getting Familiar with GUI ............................................................................................................................... 2-14 
2.4.3 Static Route .................................................................................................................................................... 2-21 
2.4.4 Cloud Network ................................................................................................................................................ 2-22 
2.4.5 Back Up ............................................................................................................... Error! Bookmark not defined. 
2.4.6 Reset Factory ...................................................................................................... Error! Bookmark not defined. 
2.4.7 restore ................................................................................................................. Error! Bookmark not defined. 
2.4.8 Viewing and Managing Revisions ....................................................................... Error! Bookmark not defined. 
2.4.9 User Management ............................................................................................... Error! Bookmark not defined. 
2.4.10 Aggregated Interface ....................................................................................... Error! Bookmark not defined. 
2.4.11 VLAN Interfaces .............................................................................................. Error! Bookmark not defined. 
2.4.12 Zones .............................................................................................................. Error! Bookmark not defined. 
2.5 Firewall Policy ......................................................................................................... Error! Bookmark not defined. 
2.5.1 Exercise ............................................................................................................... Error! Bookmark not defined.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 1-2 
 
2.5.2 Concept and Procedure ...................................................................................... Error! Bookmark not defined. 
2.5.3 Creating Objects .................................................................................................. Error! Bookmark not defined. 
2.5.4 Services ............................................................................................................... Error! Bookmark not defined. 
2.5.5 Schedule ............................................................................................................. Error! Bookmark not defined. 
2.6 User Identification.................................................................................................... Error! Bookmark not defined. 
2.7 NAT ......................................................................................................................... Error! Bookmark not defined. 
2.7.1 Exercise ........................................................................................................... Error! Bookmark not defined. 
2.7.2 Concept and Procedure .................................................................................. Error! Bookmark not defined. 
2.7.3 Source NAT (SNAT) ............................................................................................ Error! Bookmark not defined. 
2.7.4 Destination NAT (DNAT) ................................................................................. Error! Bookmark not defined. 
2.8 Software Switch ....................................................................................................... Error! Bookmark not defined. 
2.9 Hardware Switch ..................................................................................................... Error! Bookmark not defined. 
2.10 Virtual Wire pair ................................................................................................... Error! Bookmark not defined. 
2.11 Load Balancing .................................................................................................... Error! Bookmark not defined. 
2.11.1 Virtual Server ................................................................................................... Error! Bookmark not defined. 
2.11.2 Health Check ................................................................................................... Error! Bookmark not defined. 
2.12 Transparent mode ................................................................................................... Error! Bookmark not defined. 
2.13 Security Profiles ...................................................................................................... Error! Bookmark not defined. 
2.13.1 Inspection Methods ......................................................................................... Error! Bookmark not defined. 
2.13.2 Security Profile Prerequisites .......................................................................... Error! Bookmark not defined. 
2.13.3 Anti-Virus ......................................................................................................... Error! Bookmark not defined. 
2.13.4 SSH/SSL Inspection ........................................................................................ Error! Bookmark not defined. 
2.13.5 Web Filter ........................................................................................................ Error! Bookmark not defined. 
2.13.6 DNS Filter ........................................................................................................ Error! Bookmark not defined. 
2.13.7 Application Control .......................................................................................... Error! Bookmark not defined. 
2.13.8 File Filter .......................................................................................................... Error! Bookmark not defined. 
2.13.9 IPS ................................................................................................................... Error! Bookmark not defined. 
3 VPN LAB ............................................................................................................................. Error! Bookmark not defined. 
3.1 Site to site VPN ....................................................................................................... Error! Bookmark not defined. 
3.1.1 Topology .......................................................................................................... Error! Bookmark not defined. 
3.1.2 IP Addressing ...................................................................................................... Error! Bookmark not defined. 
3.1.3 LAB Setup ........................................................................................................... Error! Bookmark not defined.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 1-3 
 
3.1.4 IP Sec .................................................................................................................. Error! Bookmark not defined. 
3.1.5 GRE ................................................................................................................. Error! Bookmark not defined. 
3.2 Remote Access VPN ............................................................................................... Error! Bookmark not defined. 
3.2.1 IPsec VPN ........................................................................................................... Error! Bookmark not defined. 
3.2.2 SSL VPN ............................................................................................................. Error! Bookmark not defined. 
4 HA LAB ............................................................................................................................... Error! Bookmark not defined. 
4.1 Topology .................................................................................................................. Error! Bookmark not defined. 
4.2 IP addressing .......................................................................................................... Error! Bookmark not defined. 
4.3 LAB Setup ............................................................................................................... Error! Bookmark not defined. 
4.4 Verification ............................................................................................................... Error! Bookmark not defined. 
5 SD-WAN LAB ..................................................................................................................... Error! Bookmark not defined. 
5.1 Topology .................................................................................................................. Error! Bookmark not defined. 
5.2 IP Addressing .......................................................................................................... Error! Bookmark not defined. 
5.3 LAB Setup ............................................................................................................... Error! Bookmark not defined. 
5.3.1 DNS-Root ............................................................................................................ Error! Bookmark not defined. 
5.3.2 DNS-Google ........................................................................................................ Error! Bookmark not defined. 
5.3.3 DNS-CloudFlare .................................................................................................. Error! Bookmark not defined. 
5.3.4 ISP1 ..................................................................................................................... Error! Bookmark not defined. 
5.3.5 ISP2 ..................................................................................................................... Error! Bookmark not defined. 
5.3.6 Verification Commands on FortiGate: ................................................................. Error! Bookmark not defined. 
5.3.7 FortiGate BGP Configuration .............................................................................. Error! Bookmark not defined. 
5.4 SD WAN Zone ......................................................................................................... Error! Bookmark not defined. 
5.5 Performance SLA .................................................................................................... Error! Bookmark not defined. 
5.6 SD-WAN Rules ........................................................................................................ Error! Bookmark not defined. 
5.7 verification ............................................................................................................... Error! Bookmark not defined. 
6 VDOM .................................................................................................................................. Error! Bookmark not defined. 
6.1 Enable VDOM mode ............................................................................................... Error! Bookmark not defined. 
6.2 Verify VDOM Status ................................................................................................ Error! Bookmark not defined. 
6.3 Create New VDOMs ................................................................................................ Error! Bookmark not defined.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 1-4 
 
6.4 Assign Interfaces to VDOMs ................................................................................... Error! Bookmark not defined. 
6.5 Configure Interfaces Within a VDOM ...................................................................... Error! Bookmark not defined. 
6.6 Create Inter-VDOM Links (VDOM Links) ................................................................ Error! Bookmark not defined. 
6.7 Configure Firewall Policies in Specific VDOM ........................................................ Error! Bookmark not defined. 
6.8 View Current VDOM Context .................................................................................. Error! Bookmark not defined. 
6.9 Disable VDOM Mode (FortiOS 7.4) ......................................................................... Error! Bookmark not defined. 
7 Logging and Monitoring ................................................................................................... Error! Bookmark not defined. 
7.1 Traffic Logs .............................................................................................................. Error! Bookmark not defined. 
7.2 Log & Report ........................................................................................................... Error! Bookmark not defined. 
7.2.1 Log Settings ........................................................................................................ Error! Bookmark not defined. 
7.2.2 Forward Traffic .................................................................................................... Error! Bookmark not defined. 
7.2.3 Security Events ................................................................................................... Error! Bookmark not defined. 
7.2.4 System Events .................................................................................................... Error! Bookmark not defined. 
7.3 FortiView ................................................................................................................. Error! Bookmark not defined.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 1-5 
 
 
1.1 Files and Documents 
The table below shows the files and materials that come with this workbook 
File or Folder Name Comments 
FortiGateAdministrator7.4.pdf This current file that you are reading 
FortiGate-LAB1.unl Should be imported into eVe-NG 
FORTIGATE-HA  
FortiGate-LAB-SDwan  
FortiGate-LAB-IPsec-VPN  
FortiGate (directory) 
This folder contains a virtioa.qcow2 file. 
Should already be placed on your eVe-NG server at 
/opt/unetlab/addons/qemu/ 
 
1. ensure that these files have been imported into the respective directories on your eVe-NG server as indicated 
in the table above. 
 x86_64_crb_linux-adventerprisek9-ms.bin 
 asav-922-1/virtioa.qcow2 
2. Access the Lab in eVe-NG 
 Log in to your eVe-NG web interface. 
 Navigate to “File manager”. 
 Select the “Import” icon. 
 Choose the  unl then hit open. 
 Once the file is uploaded, locate the lab in the GUI and cli ck it to open the lab. 
 The lab topology and configurations will load automatically. 
1.2 Overview 
This document provides a comprehensive, hands-on guide to the most common FortiGate features, structured 
across four progressive laboratories: FortiGate LAB, VPN Lab, HA Lab, and SD-WAN Lab, covering concepts 
from basic setup to advanced enterprise features.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 1-6 
 
The VPN Lab is based on the FortiGate Lab topology. The only difference is the addition of a small branch 
network and a single IP address change, introduced at the beginning of the VPN Lab section in IP addressing 
table. 
The HA (High Availability) and SD-WAN labs have different structures and are implemented in different labs. 
Each lab uses its own topology, and the IP addressing is configured separately for each lab. In the last section, 
the VDOM (Virtual Domain) feature is explained. Since, VDOM cannot be implemented in the lab environment, 
only the related CLI commands are mentioned. This topic is not included in the FortiGate Administrator 
syllabus and is added to this document as a bonus section.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-7 
 
 
In FortiGate LAB, main topics are described, configured, and tested step-by-step in the E VE-NG environment. 
The LAB begins with the definition of the IP addressing scheme, followed by a detai led explanation of the lab 
structure and setup process. 
After the lab environment is fully built, the FortiGate configuration is performed from factory default settings. The 
lab then proceeds through the remaining topics in a structured order. Each se ction includes verification steps 
a n d  s c r e e n s h o t s  t o  c l e a r l y  i l l u s t r a t e  t h e  c o n f i g u r a t i o n  a n d  r e s u l t s .

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-8 
 
2.1 Topology 
 
2.2 IP Addressing 
CI OS Image Interface Zone 
IPv4 
Address/Prefix 
Sw-core 
x86_64_crb_linux_l2-adventerprisek9-
ms.bin 
E0/0 LAN 192.168.22.0/30 
E0/1 LAN 192.168.33.0/24 
E0/2 LAN 192.168.44.0/24 
Sw1 E0/0 LAN Trunk

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-9 
 
x86_64_crb_linux_l2-adventerprisek9-
ms.bin 
E0/1 LAN 192.168.33.0/24 
Sw2 
x86_64_crb_linux_l2-adventerprisek9-
ms.bin 
E0/0 LAN Trunk 
E0/1 LAN 192.168.44.0/24 
Sw-dmz 
x86_64_crb_linux_l2-adventerprisek9-
ms.bin 
E0/0 DMZ 172.16.1.0/24 
E0/1 DMZ 172.16.2.0/24 
po/1 DMZ Trunk 
Sw-dc 
x86_64_crb_linux_l2-adventerprisek9-
ms.bin 
E0/0 
DATA 
CENTER 
10.10.11.0/24 
E0/1 
DATA 
CENTER 
10.10.10.0/24 
po/1 
DATA 
CENTER 
trunk 
2.3 LAB Topology Setup 
In this part, a multi-zone enterprise environment is built with VLAN segmentation for security and efficient traffic 
management. In the following sections, lab setup explained, starting with the DMZ and continu ing through the 
Data Center, LAN zone, Out-of-Band (OOB) management environment and Core Infrastr ucture. Each 
component is configured step by step, and all zones are interconnected through switches, with a Fort iGate 
firewall acting as the central security gateway for internal traffic control and external connectivity. 
2.3.1 DMZ Zone 
This zone, hosts web servers that are exposed to external traffic. It consists of Web-Server1 and Web-Server2 
and a switch that is connecting these servers to the main firewall. 
DMZ Subnets PC IP Address Gateway IP Address Servers 
VLAN 161 172.16.1.0/24 172.16.1.100 172.16.1.1 Blue-Web Server 
VLAN 162 172.16.2.0/24 172.16.2.100 172.16.2.1 Green-Web Server 
Copy the initial configuration and paste it into global configuration mode on SW-DMZ switch.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-10 
 
configure terminal 
hostname SW-DMZ 
interface Ethernet0/0 
switchport access vlan 161 
switchport mode access 
interface Ethernet0/1 
switchport access vlan 162 
switchport mode access 
interface ethernet0/2 
switchport trunk encapsulation dot1q 
 switchport trunk allowed vlan 161,162 
 switchport mode trunk 
interface ethernet0/3 
switchport trunk encapsulation dot1q 
 switchport trunk allowed vlan 161,162 
 switchport mode trunk 
end 
wr 
2.3.2 Data Center Zone 
This zone hosts essential services and servers within the internal network. Components are, NTP Server that 
provides time synchronization in VLAN101, a server as a domain controller in VLAN 100 and a switch that is 
connecting servers in this zone to the core infrastructure. 
Data Center Subnets PC IP Address Gateway IP Address Servers 
VLAN 101 10.10.11.0/24 10.10.11.100 10.10.11.1 NTP 
VLAN 100 10.10.10.0/24 10.10.10.100 10.10.10.1 Active Directory & CA 
Copy the initial configuration and paste it into global configuration mode on SW-DC switch. 
configure terminal 
hostname SW-DC 
interface ethernet0/0 
switchport mode access

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-11 
 
switchport access vlan 101 
interface ethernet0/1 
switchport mode access 
switchport access vlan 100 
interface ethernet0/2 
switchport trunk encapsulation dot1q 
 switchport trunk allowed vlan 100,101 
 switchport mode trunk 
interface ethernet0/3 
switchport trunk encapsulation dot1q 
 switchport trunk allowed vlan 100,101 
 switchport mode trunk 
end 
wr 
2.3.3 LAN Zone 
The LAN Zone is the local network for client access, comprising SW1, SW2 and SW-Core. 
LAN Subnets PC IP Address Gateway IP Address Clients 
VLAN 33 192.168.33.0/24 192.168.33.100 192.168.33.1 Client1 
VLAN 44 192.168.44.0/24 192.168.44.100 192.168.44.1 Client2 
Copy the initial configuration and paste it into global configuration mode on SW1. 
Configure terminal 
interface ethernet0/0 
switchport mode trunk 
switchport trunk encapsulation dot1q 
interface ethernet0/1 
switchport mode access 
switchport access vlan 33 
end 
wr 
Copy the initial configuration and paste it into global configuration mode on SW2.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-12 
 
Configure terminal 
interface ethernet0/0 
switchport mode trunk 
switchport trunk encapsulation dot1q 
interface ethernet0/1 
switchport mode access 
switchport access vlan 44 
end 
wr 
The core switch is interconnecting sw1 and sw2 switches. Copy the initial confi guration and paste it into global 
configuration mode on SW-core switch. 
configure terminal 
int ethernet0/0 
switchport mode access 
switchport access vlan 22 
int ethernet0/1 
switchport mode trunk 
switchport trunk encapsulation dot1q 
int ethernet0/2 
switchport mode trunk 
switchport trunk encapsulation dot1q 
int vlan 22 
ip address 192.168.22.2 255.255.255.252 
vlan 33 
int vlan 33 
ip address 192.168.33.1 255.255.255.0 
vlan 44 
int vlan 44 
ip address 192.168.44.1 255.255.255.252 
end 
wr 
2.3.4 OOB 
This zone is dedicated to Out-of-Band (OOB) management and includes a Wind ows system in the 
172.20.20.0/24 subnet. It is connected to the network through a standalone switch (SW-OOB). No configuration 
is required for the switch. FW-Core is connected to SW-OOB using port 1.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-13 
 
OOB Subnets PC IP Address Gateway IP Address Clients 
172.20.20.0/24 172.20.20.100 172.20.20.1 OOB Client 
2.3.5 Core Infrastructure  
In this LAB, Fortinet Firewall acts as the gateway for traffic betw een zones and the external network. We will 
discuss zones in detail later in session 2.4.12. Port2 is assigned for connection to LAN zone using Static routing 
in the subnet 192.168.22.0/30. Port3 to Port4 connect FW-core to DMZ an d port5 to port6 connect FW-core to 
Data Center zone. 
2.4 Basic Setup from factory default 
2.4.1 Getting Familiar with CLI 
In this lab, we will log in to the CLI and configure the device's manage ment IP address so we can access the 
graphical interface. Start by logging in to the FortiGate for the first  time, then explore its CLI. Double-click the 
FortiGate icon to open the CLI console. The default username is admin with a blank password. Upon first login, 
FortiGate prompts you to set a new password. 
FortiGate-VM64-KVM login: admin 
Password:  
You are forced to change your password. Please input a new password. 
New Password:  
Confirm Password:  
Welcome! 
As you can see in the configuration panel, port1 is currently set to DHCP. Since we don’t have a DHCP server 
in this lab, change the mode to static and assign the IP address 172.20.20.1 to the management port. 
FortiGate-VM64-KVM #show system interface port1  
config system interface 
    edit "port1" 
        set vdom "root" 
        set mode dhcp 
        set allowaccess ping 
        set type physical 
        set snmp-index 1 
    next 
end

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-14 
 
FortiGate-VM64-KVM #config system interface  
 
FortiGate-VM64-KVM (interface) #edit port1  
FortiGate-VM64-KVM (port1) #set mode static 
FortiGate-VM64-KVM (port1) #set ip 172.20.20.1 255.255.255.0 
FortiGate-VM64-KVM (port1) #set allowaccess ping http https 
FortiGate-VM64-KVM (port1) #end 
To view the graphical user interface, enter "http://172.20.20.1" in OOB-client browser. Please note that activating 
the HTTP service using the highlighted command is required to access the GUI. The end command in FortiGate 
is used to exit the current configuration menu and save any changes, while the next command is used to move 
to the next entry or object within the current configuration menu. 
2.4.2 Getting Familiar with GUI 
After activating the web service, the GUI loads for the first time, as shown in the following figure. The login 
page is displayed, enter your username and password, then press Enter. 
 
Afterward, next figure will appear.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-15 
 
 
Click "Begin," then on the next screen press "Later."

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-16 
 
 
Press Save and continue. 
 
Click OK in the next few figures.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-17 
 
 
 
Upon first login to the FortiGate GUI, the dashboard is displayed. Configuration will be performed step-by-step. 
The FortiGate dashboard provides an overview of system information, licensing, allocated vCPU and RAM. The 
firewall runs firmware v7.4.1 in NAT mode, with an uptime of over 9 days. It has 1% vCPU usage and 55% RAM 
usage.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-18 
 
 
From the left-hand menu, select Network, then navigate to the Interfaces menu, ports, 1 to 7 are visible in the 
figure. We can configure several items such as IP address, subnet mask, role ( WAN, LAN, or undefined), and 
administrative access for each port. Double-click the port1 which was configured in the CLI part. 
 
Port1 configuration is shown in the following figure.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-19 
 
 
As displayed, the addressing mode is Manual. Choose a name for the interface and enter it in the Alias field, 
then click OK. 
As shown in previous figure, administrative access protocols such as PING, HTTPS, SSH,  and HTTP can be 
enabled or disabled on a per-interface basis to allow connections from specific trusted  hosts or subnets. In the 
previous section, we enabled HTTP, PING, and HTTPS access through the command-line interface (CLI).  
For the next step, click Network > Interfaces in the left-hand menu, then double-click Port 2, which is 
assigned to the LAN. Enter the address 192.168.22.1/255.255.255.0 in the IP/Netmask field, as shown in the 
following figure.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-20 
 
 
Different interface roles on a FortiGate device, such as LAN, WAN, or DMZ, dictate which settings are displayed 
on the GUI, and prevent accidental misconfigurations. When an interface is confi gured as WAN, for example, 
settings for a DHCP server and device detection are not available, as these a re typically used for internal LAN 
configurations. If an unusual case requires the use of options hidden by the current role, the role can be switched 
to Undefined which will display all options. 
With all the configuration we have done so far, we can ping sw-core from the firewall using the following 
command. The FortiGate firewall and sw-core can ping each other because they have connected interfaces. 
However, can we ping the clients? 
FortiGate-VM64-KVM #execute ping 192.168.22.2 
PING 192.168.22.2 (192.168.22.2): 56 data bytes 
64 bytes from 192.168.22.2: icmp_seq=0 ttl=255 time=1.2 ms 
64 bytes from 192.168.22.2: icmp_seq=1 ttl=255 time=0.7 ms 
FortiGate-VM64-KVM # execute ping 192.168.33.100 
PING 192.168.33.100 (192.168.33.100): 56 data bytes 
sendto failed

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-21 
 
 
2.4.3 Static Route 
Since the firewall has no route to the core switch, a static route is required. Configure it as shown in the following 
figure.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-22 
 
 
 
Now, the ping is successful. 
2.4.4 Cloud Network 
Based on the topology, we can observe that the firewall is connected to the cloud. For this scenario, we assume 
that the cloud represents the internet. The cloud network has an IP address range  of 172.24.47.0/24, with the 
gateway set to 172.24.47.254.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-23 
 
Ideally, we should assign a public IP address to the internet-facing interface. However, since this is a lab 
environment, we will assume that the firewall is connected to the cloud using a public IP address. 
In fact, since it is a lab environment, we are using a private IP address (172 .24.47.100) to simulate a public IP 
for testing purposes. 
Configure Port 7 as shown in the following figure. 
 
To connect to the cloud, we need a static route like the one shown in the following figure.

FortiGate Workbook 
 
COPYRIGHT SMENODE LABS. ALL RIGHTS RESERVED. 2-24 
 
 
Now, we can ping the global DNS servers from the FortiGate CLI, confirming Internet access.