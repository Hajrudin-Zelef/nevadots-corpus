             Recommendations
       Huawei Learning Website
            http://learning.huawei.com/en

       Huawei e-Learning
            https://ilearningx.huawei.com/portal/#/portal/ebg/51



       Huawei Certification
            http://support.huawei.com/learning/NavigationAction!createNavi?navId=_31
             &lang=en

       Find Training
            http://support.huawei.com/learning/NavigationAction!createNavi?navId=_trai
             ningsearch&lang=en


             More Information
   Huawei learning APP




版权所有© 2019 华为技术有限公司
            Huawei Certification




               HCIA-
  Routing&Switching
              ENTRY

Huawei Networking Technology and Device
                 Lab Guide




           Huawei Technologies Co.,Ltd.
Copyright © Huawei Technologies Co., Ltd. 2019.



 All rights reserved.
 Huawei owns all copyrights, except for references to other parties. No part of this
 document may be reproduced or transmitted in any form or by any means without
 prior written consent of Huawei Technologies Co., Ltd.


 Trademarks and Permissions

        and other Huawei trademarks are trademarks of Huawei Technologies Co., Ltd.

All other trademarks and trade names mentioned in this document are the property of
their respective holders.



 Notice


The information in this manual is subject to change without notice. Every effort has
been made in the preparation of this manual to ensure accuracy of the contents, but all
statements, information, and recommendations in this manual do not constitute the
warranty of any kind, express or implied.




                            Huawei Certification

HCIA-Routing&SwitchingHuawei Networking Technology

                                   and Device

                                Entry Lab Guide

                                   Version 2.5
                     Huawei Certification System

Relying on its strong technical and professional training and certification system
and in accordance with customers of different ICT technology levels, Huawei
certification is committed to providing customers with authentic, professional
certification, and addresses the need for the development of quality engineers that
are capable of supporting Enterprise networks in the face of an ever changing ICT
industry. The Huawei certification portfolio for routing and switching (R&S) is
comprised of three levels to support and validate the growth and value of customer
skills and knowledge in routing and switching technologies.


The Huawei Certified Network Associate (HCIA) certification level validates the skills
and knowledge of IP network engineers to implement and support small to
medium-sized enterprise networks. The HCIA certification provides a rich
foundation of skills and knowledge for the establishment of such enterprise
networks, along with the capability to implement services and features within
existing enterprise networks, to effectively support true industry operations.


HCIA certification covers fundamentals skills for TCP/IP, routing, switching and
related IP network technologies, together with Huawei data communications
products,   and skills for versatile routing platform (VRP)          operation and
management.


The Huawei Certified Network Professional (HCIP-R&S) certification is aimed at
enterprise network engineers involved in design and maintenance, as well as
professionals who wish to develop an in depth knowledge of routing, switching,
network efficiency and optimization technologies. HCIP-R&S consists of three units
including Implementing Enterprise Routing and Switching Network (IERS),
Improving Enterprise Network Performance (IENP), and Implementing Enterprise
Network Engineering Project (IEEP), which includes advanced IPv4 routing and
switching technology principles, network security, high availability and QoS, as well
as application of the covered technologies in Huawei products.


The Huawei Certified Internet Expert (HCIE-R&S) certification is designed to imbue
engineers with a variety of IP network technologies and proficiency in maintenance,
for the diagnosis and troubleshooting of Huawei products, to equip engineers with
in-depth competency in the planning, design and optimization of large-scale IP
networks.
Reference Icons
                                                       CONTENTS

MODULE 1 ESTABLISHING BASIC NETWORKS WITH ENSP ......................................... 1


  LAB 1-1 BUILDING BASIC IP NETWORKS ..................................................................................................1


MODULE 2 BASIC DEVICE NAVIGATION AND CONFIGURATION .............................14


  LAB 2-1 BASIC DEVICE NAVIGATION AND CONFIGURATION ............................................................... 14


MODULE 3 STP AND RSTP..............................................................................................25


  LAB 3-1 CONFIGURING STP ................................................................................................................... 25


  LAB 3-2 CONFIGURING RSTP ................................................................................................................ 42


MODULE 4 ROUTING CONFIGURATION .......................................................................51


  LAB 4-1 CONFIGURING STATIC ROUTES AND DEFAULT ROUTES ........................................................ 51


  LAB 4-2 OSPF SINGLE-AREA CONFIGURATION ................................................................................... 68


MODULE 5 FTP AND DHCP ............................................................................................84


  LAB 5-1 CONFIGURING FTP SERVICES .................................................................................................. 84


  LAB 5-2 IMPLEMENTING DHCP ............................................................................................................. 92
       Module 1 Establishing Basic Networks with eNSP

Lab 1-1 Building Basic IP Networks


Learning Objectives


As a result of this lab section, you should achieve the following tasks:

      Set up and navigate the eNSP simulator application.

      Establish a simple peer-to-peer network in eNSP.

      Perform capture of IP packets using Wireshark within eNSP.




                                      HUAWEI TECHNOLOGIES                  Page1
The fundamental network behavior can be understood through the application of
packet capture tools to the network. The use of Huawei’s simulator platform eNSP
is capable of supporting both the implementation of technologies and the capture
of packets within the network to provide a comprehensive knowledge of IP
networks.


Tasks


Step 1 Install eNSP


1. Login website of eNSP：

   https://support.huawei.com/enterprise/en/tool/ensp-TL1000000015/23917110




2. Download the latest version of eNSP




3. Please refer to the software installation guide below to install eNSP in local PC.




   Then engineer can practice lab with AR, Router, S57, S37, USG5500, AC, AP .



                                      HUAWEI TECHNOLOGIES                    Page2
  If the engineer want to practice lab with USG6000V, CE, NE40, NE5000E, NE9000,

  CX, please follow Step4




4. Enable USG6000V, CE, NE40, NE5000E, NE9000, CX devices in eNSP:

  1） For example, if you want to enable USG6000V in eNSP, you should download

       the corresponding mirror file.




  2） Select USG6000V into new project of eNSP, then right click “start”of

       USG6000V :




  3） The dialog box of “import package” will show up:



                                    HUAWEI TECHNOLOGIES               Page3
   4） Click "Browse" - and import the downloaded mirror files, then engineer can

       practice lab with USG6000V.




   5） If the engineer want to practice CE, NE40, NE5000E, NE9000, CX, please

       repeat step 4-1) --- 4).




Step 2 Initiate eNSP

This step introduces how to start and navigate the eNSP simulator application for
rapid development of TCP/IP knowledge and familiarity with network operation. If
eNSP is not available, please inform the course instructor
After launching eNSP, the following application user interface will be presented. The
left panel houses the icons that represent the various products and devices that are
supported within eNSP, while the central panel provides lab examples for practice
scenarios.



                                     HUAWEI TECHNOLOGIES                   Page4
After launching eNSP, users should select the New operator in the top left corner of
the application window to begin a new lab session.
The user will be presented with a canvas on which to establish a network topology
for practice and analysis of network behavior. In this example a simple peer-to-peer
network using two end systems is to be established.



Step 3 Build a Topology

Select the End Devce icon in the top left panel to reveal a list of end devices that can
be applied. Select the Laptop icon and drag it to the canvas, release the icon to place
it on the canvas.




                                      HUAWEI TECHNOLOGIES                     Page5
The same action should be taken to position a second laptop on the canvas for
establishing the peer-to-peer network topology.




                                  HUAWEI TECHNOLOGIES               Page6
The devices on the canvas represent simulated end systems that can be used to
emulate real world operations.



Step 4 Establish a physical medium

Select the connections icon from the upper left panel to reveal a list of media that
can be applied to the topology. Select the copper (Ethernet) medium from the list.
Once the icon has been clicked, the cursor will represent a connector to show the
current role of the cursor as a connector. Click on the client device to reveal a list of
port interfaces supported by the simulated device. For the client click the option
forGigabitEthernet 0/0/1 to apply the connection.




Once this has been achieved, click on the peering device to apply the opposite end
of the medium to the end system. Again select the interfaceGigabitEthernet 0/0/1 to
establish the medium between the two devices and complete the construction of a
peer-to-peer topology.




                                       HUAWEI TECHNOLOGIES                    Page7
The establishment of a point-to-point network reveals a connection with two red
dots on the medium that represent the current state of the interfaces to which the
medium connects as down.



Step 5 Access the end system settings

Select the end system and use the right click option to display a properties menu.
The settings option should be selected in order to display the current system
settings for the end system devices.




                                    HUAWEI TECHNOLOGIES                 Page8
The settings option in the properties window reveals a set of four tabs for
establishment of basic configuration, the device command line interface, multicast
traffic generator configuration, and UDP packet generator configuration.



Step 6 Configure the end system

Ensure the Basic Config tab is selected and enter a host name in the Host Name field
window. Ensure the IPv4 configuration is currently set to static and configure an IP
address in the IP address window. It is recommended that the address (together with
the subnet mask) be configured as shown in the below example. Once this has been
configured, click the Apply button in the bottom left corner of the window before
closing with the x in the top left corner of the CLIENT 1 window.




The same process is required for CLIENT2. It is recommended that initially the IP
address 192.168.1.2 be configured, with a subnet mask of 255.255.255.0.
The basic configuration enables peer-to-peer communication to be supported


                                     HUAWEI TECHNOLOGIES                  Page9
between the two end systems.



Step 7 Initiate the end system devices

The devices can be activated using one of two methods. The first involves using the
right click option to open the properties menu and select start for the individual
icons. The alternative involves dragging the cursor over the icons (as shown) to
highlight multiple devices and using the right click settings option start multiple
devices simultaneously.




Once the devices are online and active, it is common to notice a change in the status
of the connectors through a switch in the colour of the red dot on the medium to
green, highlighting that the status of the connectors is now up.




                                     HUAWEI TECHNOLOGIES                  Page10
Once the devices within the network topology are operational, it is possible to begin
to monitor the flow of traffic that is carried over the medium and the interfaces via
which the devices have established a physical peering.




Step 8 Implement the capture of packets on an interface

Select the device to for whose interface is to be monitored and use the right click
option to display the settings menu. Highlight the capture data option to reveal a list
of interfaces that belong to the device and are available for observation by the
packet capture tool. Select the interface from the list that is to be monitored.




The selection of an interface will result in the activation of the Wireshark packet
capture tool for the selected interface. If additional interfaces are to be monitored,
separate instances of the same packet capture tool will be activated.




                                      HUAWEI TECHNOLOGIES                   Page11
Depending on the devices being monitored, the packet capture tool may or may not
begin to generate packet capture results for all traffic that passes through the
selected interface. In the case of the peer-to-peer relationship, it will be necessary to
generate some traffic.




Step 9 Generate traffic on the interface

Open the command window on the client by either double clicking the client icon
and selecting the Command tab, or alternatively use the right click option to enter
the properties menu and select settings from which point the Command tab can be
selected.
The most basic means for generating traffic is through the ping command. This can
be achieved by entering ping <ip address> where the IP address refers to the
address of the peer.




The generation of traffic will be confirmed by the resulting output in which case the
number of packets transmitted are shown to also be received.



                                       HUAWEI TECHNOLOGIES                    Page12
Following the generation of traffic, the resulting traffic flow shall be captured by the
packet capture tool and can be used for observation of the behavior of protocols
within the IP network along with details of the various layers as referenced in the OSI
reference model.



Step 10    Observe the captured traffic flow

An instance of the Wireshark packet capture tool should currently be active
following the action to capture data on the client interface. Maximize the active
window to observe the results of the packet capture process.




The Wireshark application contains many functions for management of the packet
capture process. One of the more common functions includes the filter function to
isolate the packet capture display to a select group of packets or protocols. This can
be achieved using the filter field below the menu bar. The simplest filter method
involves entering the protocol name (in lower case) and pressing Enter. In the given
example packets for two protocols have been captured, entering either icmp, or arp
into the filter window will result in only the protocol entered in the filter field being
displayed in the output.
The packet capture tool consists of three panels, to show the list of packets, a
breakdown of the content of each packet and finally display the equivalent data
format of the packet. The breakdown is invaluable for understanding the format of
protocol packets and displays the details for protocols as referenced at each layer of
the OSI reference model.



                                       HUAWEI TECHNOLOGIES                    Page13
       Module 2 Basic Device Navigation and Configuration

Lab 2-1 Basic Device Navigation and Configuration


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

       Configure device system parameters including device name, the system time,

        and the system time zone.

       Configure the console port idle timeout duration.

       Configure the login information.

       Configure the login password.

       Save configuration files.

       Configure IP addresses for router interfaces.

       Test the connectivity between two directly connected routers.

       Restart a device using VRP.


Topology




               Figure 2.1 Lab topology for basic VRP navigation and operation.




                                        HUAWEI TECHNOLOGIES                      Page14
Scenario

A company has purchased two AR G3 routers that require commissioning before
they can be used in the enterprise network. Items to be commissioned include
setting device names, the system time, and password management.




Tasks


Step 1 View the system information

Run the display version command to view the software version and hardware
information for the system.
<Huawei>display version
Huawei Versatile Routing Platform Software
VRP (R) software, Version 5.160 (AR2200 V200R007C00SPC600)
Copyright (C) 2011-2013 HUAWEI TECH CO., LTD
Huawei AR2220E Router uptime is 0 week, 3 days, 21 hours, 43 minutes
BKP 0 version information:
......output omitted......


The command output includes the VRP operating system version, device model, and
startup time.

Step 2 Change the system time parameter

The system automatically saves the time. If the time is incorrect, run the clock
timezone and clock datetime commands in the user view to change the system
time.
<Huawei>clock timezone Local add 08:00:00
<Huawei>clock datetime 12:00:00 2016-03-11



The keyword Local can be exchanged with the current regional timezone name, and

add replaced with minus where the timezone is west of UTC+0.


Run the display clock command to check that the new system time has taken effect.


                                               HUAWEI TECHNOLOGIES       Page15
<Huawei>display clock
  2016-03-11 12:00:10
  Friday
  Time Zone(Local) : UTC+08:00




Step 3 Help features & Auto-completion functions

The question mark (?) is a wildcard, and the Tab is used as a shortcut to enter
commands.
<Huawei>display ?
    Cellular               Cellular interface
    aaa                          AAA
    access-user                  User access
    accounting-scheme            Accounting scheme
    acl                          <Group> acl command group
    actual                       Current actual
    adp-ipv4               Ipv4 information
    adp-mpls               Adp-mpls module
    alarm                        Alarm
    antenna                      Current antenna that outputting radio
    anti-attack                  Specify anti-attack configurations
    ap                           <Group> ap command group
    ap-auth-mode           Display AP authentication mode
 ......output omit......


To display all the commands that start with a specific letter or string of letters, enter
the desired letters and the question mark (?). The system displays all the commands
that start with the letters entered. For example, if the string dis? is entered, the
system displays all the commands that start with dis.

If a space exists between the character string and the question mark (?), the system
will identify the commands corresponding to the string and display the parameters
of the command. For example, if the string dis ? is entered and only the display
command matches the dis string, the system displays the parameters of the display
command. If multiple commands start with dis, the system displays an error.
The Tab key can also be pressed to complete a command. For example, if dis is
entered followed by Tab, the system completes the display command. If multiple
commands start with dis, the appropriate command can be selected.
If there are no other commands starting with the same letters, dis or disp can be


                                                  HUAWEI TECHNOLOGIES         Page16
entered to indicate display, and int or inter to indicate interface.



Step 4 Access the system view

Run the system-view command to access the system view to configure interfaces
and protocols.
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]



Step 5 Change device names

To more easily identify devices, set device names during the device configuration.
Change device names based on the lab topology, as shown below:
Change the name of the R1 router to R1.
[Huawei]sysname R1
[R1]


Change the name of the R3 router to R3.

[Huawei]sysname R3
[R3]



Step 6 Configure the login information

Configure the login information to indicate the login result.
[R1]header shell information "Welcome to the Huawei certification lab."



Run the preceding command to configure the login information. To check whether
the login information has been changed, exit from the router command line
interface, and log back in to view the login information.
[R1]quit
<R1>quit


  Configuration console exit, please press any key to log on




                                                   HUAWEI TECHNOLOGIES    Page17
Welcome to the Huawei certification lab.
<R1>



Step 7 Configure console port parameters

The console port by default does not have a login password. Users must configure a
password for the console port before logging in to the device.
The password can be changed in the password authentication mode to huawei in
plain text.
If there is no activity on the console port for the period of time specified by the
timeout interval, the system will automatically log out the user. When this occurs, log
in to the system again using the configured password.
The default timeout interval is set to 10 minutes. If a 10 minutes idle period is not a
reasonable amount of time for the timeout interval, change the timeout interval to a
more suitable duration, here this is set to 20 minutes.
[R1]user-interface console 0
[R1-ui-console0]authentication-mode password
[R1-ui-console0]set authentication password cipher
Warning: The "password" authentication mode is not secure, and it is strongly recommended to use "aaa"
authentication mode.
Enter Password(<8-128>):
Confirm password:
[R1-ui-console0] idle-timeout 20 0


Run the display this command to check the configuration results.
[R1-ui-console0]display this
[V200R007C00SPC600]
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$fIn'6>NZ6*~as(#J:WU%,#72Uy8cVlN^NXkT51E ^RX;>#75,%$%$
 idle-timeout 20 0


Log out of the system and log back in, using the password set. It should be noted
that this password is required to be set when the router is first initialized.
[R1-ui-console0]return
    <R1>quit




                                               HUAWEI TECHNOLOGIES                             Page18
      Configuration console exit, please press any key to log on

    Login authentication

    Password:

    Welcome to Huawei certification lab
    <R1>



Step 8 Configure interface IP addresses and descriptions

Configure an IP address for the GigabitEthernet 0/0/0 interface of R1. The subnet
mask can be configured using a dotted decimal format (255.255.255.0), or based on
the subnet mask prefix length.
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ip address 10.0.13.1 24
[R1-GigabitEthernet0/0/0]description This interface connects to R3-G0/0/0


Run the display this command to check the configuration results at the current
interface view.
[R1-GigabitEthernet0/0/0]display this
[V200R007C00SPC600]
#
interface GigabitEthernet0/0/0
 description This interface connects to R3-G0/0/0
 ip address 10.0.13.1 255.255.255.0
#
return


Run the display interface command to view the interface description.
[R1]display interface GigabitEthernet0/0/0
GigabitEthernet0/0/0 current state : UP
Line protocol current state : UP
Last line protocol up time : 2016-03-11 04:13:09
Description:This interface connects to R3-G0/0/0
Route Port,The Maximum Transmit Unit is 1500
Internet Address is 10.0.13.1/24
IP Sending Frames' Format is PKTFMT_ETHNT_2, Hardware address is 5489-9876-830b
Last physical up time      :   2016-03-10 03:24:01
Last physical down time :      2016-03-10 03:25:29
Current system time: 2016-03-11 04:15:30



                                                   HUAWEI TECHNOLOGIES            Page19
Port Mode: FORCE COPPER
Speed : 100, Loopback: NONE
Duplex: FULL, Negotiation: ENABLE
Mdi      : AUTO, Clock    :-
Last 300 seconds input rate 2296 bits/sec, 1 packets/sec
Last 300 seconds output rate 88 bits/sec, 0 packets/sec
Input peak rate 7392 bits/sec,Record time: 2016-03-10 04:08:41
Output peak rate 1120 bits/sec,Record time: 2016-03-10 03:27:56
Input: 3192 packets, 895019 bytes
  Unicast:                     0,        Multicast:              1592
  Broadcast:             1600,      Jumbo:                       0
  Discard:                     0,        Total Error:            0
  CRC:                         0,        Giants:                     0
  Jabbers:                     0,        Throttles:              0
  Runts:                       0,        Symbols:                0
  Ignoreds:                    0,        Frames:                     0
Output: 181 packets, 63244 bytes
  Unicast:                     0,        Multicast:              0
  Broadcast:             181, Jumbo:                         0
  Discard:                     0,        Total Error:            0
  Collisions:            0,         ExcessiveCollisions: 0
  Late Collisions:       0,         Deferreds:                   0
      Input bandwidth utilization threshold : 100.00%
      Output bandwidth utilization threshold: 100.00%
      Input bandwidth utilization : 0.01%
Output bandwidth utilization :      0%


The command output shows that the physical status and protocol status of the
interface are UP, and the corresponding physical layer and data link layer are
functional.
Once the status has been verified, configure the IP address and description for the
interface of R3.
[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ip address 10.0.13.3 255.255.255.0 [R3-GigabitEthernet0/0/0]description This interface
connects to R1-G0/0/0

After completing the configuration, run the ping command to test the connection
between R1 and R3.
<R1>ping 10.0.13.3
  PING 10.0.13.3: 56 data bytes, press CTRL_C to break
      Reply from 10.0.13.3: bytes=56 Sequence=1 ttl=255 time=35 ms



                                                      HUAWEI TECHNOLOGIES                        Page20
    Reply from 10.0.13.3: bytes=56 Sequence=2 ttl=255 time=32 ms
    Reply from 10.0.13.3: bytes=56 Sequence=3 ttl=255 time=32 ms
    Reply from 10.0.13.3: bytes=56 Sequence=4 ttl=255 time=32 ms
    Reply from 10.0.13.3: bytes=56 Sequence=5 ttl=255 time=32 ms
  --- 10.0.13.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
round-trip min/avg/max = 32/32/35 ms

Step 9 View the file list stored on the current device

Run the dir command in the user view to display the list of files in the current
directory.
<R1>dir
Directory of flash:/
  Idx Attr     Size(Byte)       Date     Time(LMT)      FileName
    0 -rw-       1,738,816      Mar 10 2016 11:50:24 web.zip
1 -rw- 68,288,896          Mar 10 2016 14:17:5          ar2220E-v200r007c00spc600.cc
2 -rw-                 739      Mar 10 2016 16:01:17         vrpcfg.zip
1,927,476 KB total (1,856,548 KB free)


<R3>dir
Directory of flash:/
  Idx Attr     Size(Byte)       Date         Time(LMT)        FileName
    0 -rw-     1,738,816 Mar 10 2016 11:50:58 web.zip
1 -rw-     68,288,896      Mar 10 2016 14:19:0          ar2220E-v200r007c00spc600.cc
2 -rw-                 739 Mar 10 2016 16:03:04        vrpcfg.zip
1,927,476 KB total (1,855,076 KB free)



Step 10        Manage device configuration files

Attempt to display the saved-configuration file.
<R1>display saved-configuration
     There is no correct configuration file in FLASH



Since no save-configuration file exists, save the current configuration file.
<R1>save
  The current configuration will be written to the device.



                                                  HUAWEI TECHNOLOGIES                  Page21
    Are you sure to continue? (y/n)[n]:y
    It will take several minutes to save configuration file, please wait............
    Configuration file had been saved successfully
    Note: The configuration file will take effect after being activated




Run the following command again to view the saved configuration information:
<R1>display saved-configuration
[V200R007C00SPC600]
#
sysname R1
header shell information "Welcome to Huawei certification lab"
#
board add 0/1 1SA
board add 0/2 1SA
……output omit……

Run the following command to view the current configuration information:
<R1>display current-configuration
[V200R007C00SPC600]
#
sysname R1
header shell information "Welcome to Huawei certification lab"
#
board add 0/1 1SA
board add 0/2 1SA
board add 0/3 2FE
……output omit……

A router can store multiple configuration files. Run the following command to view
the configuration file to currently be used after the next startup:
<R3>display startup
MainBoard:
    Startup system software:                              flash:/ar2220E-V200R007C00SPC600.cc
    Next startup system software:                         flash:/ar2220E-V200R007C00SPC600.cc
    Backup system software for next startup: null
    Startup saved-configuration file:                     null
    Next startup saved-configuration file:         flash:/vrpcfg.zip
    Startup license file:                                 null
    Next startup license file:                     null
    Startup patch package:                                null



                                                          HUAWEI TECHNOLOGIES                   Page22
  Next startup patch package:                     null
  Startup voice-files:                            null
  Next startup voice-files:                       null



Delete configuration files from the flash memory.
<R1>reset saved-configuration
This will delete the configuration in the flash memory.
The device configurations will be erased to reconfigure.
Are you sure? (y/n)[n]:y
 Clear the configuration in the device successfully.


<R3>reset saved-configuration
This will delete the configuration in the flash memory.
The device configurations will be erased to reconfigure.
Are you sure? (y/n)[n]:y
 Clear the configuration in the device successfully.



Step 11        Device restart procedure

Use the reboot command to restart the router.
<R1>reboot
Info: The system is now comparing the configuration, please wait.
Warning: All the configuration will be saved to the next startup configuration. Continue ? [y/n]:n
System will reboot! Continue ? [y/n]:y
Info: system is rebooting ,please wait...


<R3>reboot
Info: The system is now comparing the configuration, please wait.
Warning: All the configuration will be saved to the next startup configuration. Continue ? [y/n]:n
System will reboot! Continue ? [y/n]:y


The system asks to save the current configuration. It is necessary to determine
whether the current configuration should be saved based on the requirements for
the lab. If unsure as to whether the current configuration should be saved, do not
save.


Final Configuration

[R1]display current-configuration



                                                   HUAWEI TECHNOLOGIES                               Page23
[V200R007C00SPC600]
#
 sysname R1
 header shell information "Welcome to Huawei certification lab"
#
interface GigabitEthernet0/0/0
 description This interface connects to R3-G0/0/0
 ip address 10.0.13.1 255.255.255.0
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$4D0K*-E"t/I7[{HD~kgW,%dgkQQ!&|;XTDq9SFQJ.27M%dj,%$%$
 idle-timeout 20 0
#
return


[R3]display current-configuration
[V200R007C00SPC600]
#
 sysname R3
#
interface GigabitEthernet0/0/0
 description This interface connect to R1-G0/0/0
 ip address 10.0.13.3 255.255.255.0
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$M8\HO3:72:ERQ8JLoHU8,%t+lE:$9=a7"8%yMoARB]$B%t.,%$%$
user-interface vty 0 4
#
return




                                                   HUAWEI TECHNOLOGIES             Page24
                          Module 3 STP and RSTP

Lab 3-1 Configuring STP


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

      Enable and disable STP

      Change the STP mode that is used by a switch

      Change the bridge priority to control root bridge election

      Change the port priority to control election of the root port and designated

       port

      Change the port cost to control election of the root port and designated port

      Configure an edge port


Topology




                                Figure 3.1 STP topology



Scenario

Assume that you are a network administrator of a company. The company network
consists of two layers: core layer and access layer. The network uses a design that
supports network redundancy. STP will be used to prevent loops. The STP network


                                      HUAWEI TECHNOLOGIES                  Page25
should include setting the bridge priority to control STP root bridge election, and
configuration of features to speed up STP route convergence.


Tasks


    Step 1 Configure STP and verify the STP configuration.

Irrelevant interfaces must be disabled to ensure test result accuracy.
Shut down port interfacesGigabitEthernet 0/0/1 on S3,GigabitEthernet 0/0/13 and
Ethernet 0/0/7 on S3; GigabitEthernet 0/0/1, GigabitEthernet 0/0/2, GigabitEthernet
0/0/3, GigabitEthernet 0/0/13, GigabitEthernet 0/0/14 on S1; GigabitEthernet 0/0/1,
GigabitEthernet 0/0/2, GigabitEthernet 0/0/3, GigabitEthernet 0/0/6, GigabitEthernet
0/0/7 on S2; as well as GigabitEthernet 0/0/1, GigabitEthernet 0/0/14 and
GigabitEthernet 0/0/6 on S4 before starting STP configuration. Ensure that the
devices start without any configuration files. If STP is disabled, run the stp enable
command to enable STP.
<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S1
[S1]interface GigabitEthernet 0/0/1
[S1-GigabitEthernet0/0/1]shutdown
[S1-GigabitEthernet0/0/1]quit
[S1]interface GigabitEthernet 0/0/2
[S1-GigabitEthernet0/0/2]shutdown
[S1-GigabitEthernet0/0/2]quit
[S1]interface GigabitEthernet 0/0/3
[S1-GigabitEthernet0/0/3]shutdown
[S1-GigabitEthernet0/0/3]quit
[S1]interface GigabitEthernet 0/0/13
[S1-GigabitEthernet0/0/13]shutdown
[S1-GigabitEthernet0/0/13]quit
[S1]interface GigabitEthernet 0/0/14
[S1-GigabitEthernet0/0/14]shutdown
[S1-GigabitEthernet0/0/14]quit


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S2
[S2]interface GigabitEthernet 0/0/1




                                                   HUAWEI TECHNOLOGIES    Page26
[S2-GigabitEthernet0/0/1]shutdown
[S2-GigabitEthernet0/0/1]quit
[S2]interface GigabitEthernet 0/0/2
[S2-GigabitEthernet0/0/2]shutdown
[S2-GigabitEthernet0/0/2]quit
[S2]interface GigabitEthernet 0/0/3
[S2-GigabitEthernet0/0/3]shutdown
[S2-GigabitEthernet0/0/3]quit
[S2]interface GigabitEthernet 0/0/6
[S2-GigabitEthernet0/0/6]shutdown
[S2-GigabitEthernet0/0/6]quit
[S2]interface GigabitEthernet 0/0/7
[S2-GigabitEthernet0/0/7]shutdown
[S2-GigabitEthernet0/0/7]quit


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S3
[S3]interface GigabitEthernet 0/0/1
[S3-GigabitEthernet0/0/1]shutdown
[S3-GigabitEthernet0/0/1]quit
[S3]interface GigabitEthernet 0/0/13
[S3-GigabitEthernet0/0/13]shutdown
[S3-GigabitEthernet0/0/13]quit
[S3]interface GigabitEthernet 0/0/7
[S3-GigabitEthernet0/0/7]shutdown


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S4
[S4]inter GigabitEthernet 0/0/1
[S4-GigabitEthernet 0/0/1]shutdown
[S4-GigabitEthernet 0/0/1]quit
[S4]inter GigabitEthernet 0/0/14
[S4-GigabitEthernet 0/0/14]shutdown
[S4-GigabitEthernet 0/0/14]quit
[S4]interface GigabitEthernet 0/0/6
[S4-GigabitEthernet0/0/6]shutdown


In the lab, S1 and S2 are connected through two links, and STP is used. Enable STP
on S1 and S2 and set S1 as the root.
[S1]stp mode stp


                                                   HUAWEI TECHNOLOGIES   Page27
Info: This operation may take a few seconds. Please wait for a moment...done.
[S1]stp root primary


[S2]stp mode stp
Info: This operation may take a few seconds. Please wait for a moment...done.
[S2]stp root secondary


Run the display stp brief command to view brief information about STP.
<S1>display stp brief
MSTID         Port                             Role   STP State Protection
    0         GigabitEthernet0/0/9      DESI      FORWARDING        NONE
    0         GigabitEthernet0/0/10            DESI   FORWARDING         NONE


<S2>display stp brief
MSTID    Port                           Role      STP State Protection
     0        GigabitEthernet0/0/9      ROOT FORWARDING            NONE
     0        GigabitEthernet0/0/10 ALTE          DISCARDING       NONE



Run the display stp interface command to view the STP status of a port.



<S1>display stp interface GigabitEthernet 0/0/10
-------[CIST Global Info][Mode STP]-------
CIST Bridge            :0     .d0d0-4ba6-aab0
Config Times           :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times           :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC          :0     .d0d0-4ba6-aab0 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :0      .d0d0-4ba6-aab0 / 0
CIST RootPortId        :0.0
BPDU-Protection         :Disabled
CIST Root Type          :Primary root
TC or TCN received :11
TC count per hello :0
STP Converge Mode           :Normal
Share region-configuration :Enabled
Time since last TC :0 days 1h:43m:55s
Number of TC            :29
Last TC occurred       :GigabitEthernet0/0/9
----[Port10(GigabitEthernet0/0/10)][FORWARDING]----
 Port Protocol          :Enabled
 Port Role              :Designated Port


                                                       HUAWEI TECHNOLOGIES      Page28
 Port Priority         :128
 Port Cost(Dot1T )      :Config=auto / Active=20000
 Designated Bridge/Port          :0.d0d0-4ba6-aab0 / 128.10
 Port Edged              :Config=default / Active=disabled
 Point-to-point         :Config=auto / Active=true
 Transit Limit         :6 packets/s
 Protection Type         :None
 Port STP Mode            :STP
 Port Protocol Type     :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes               :Hello 2s MaxAge 20s FwDly 15s RemHop 20
 TC or TCN send          :52
 TC or TCN received :0
 BPDU Sent                :3189
             TCN: 0, Config: 3189, RST: 0, MST: 0
 BPDU Received            :5
             TCN: 0, Config: 5, RST: 0, MST: 0
 Last forwarding time: 2016/11/21 14:55:11 UTC




<S2>display stp interface GigabitEthernet 0/0/10
-------[CIST Global Info][Mode STP]-------
CIST Bridge            :4096 .d0d0-4ba6-ac20
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times           :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :0      .d0d0-4ba6-aab0 / 20000
CIST RegRoot/IRPC        :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId         :128.9 (GigabitEthernet0/0/9)
BPDU-Protection          :Disabled
CIST Root Type          :Secondary root
TC or TCN received :122
TC count per hello :0
STP Converge Mode         :Normal
Share region-configuration :Enabled
Time since last TC :0 days 1h:50m:0s
Number of TC             :17
Last TC occurred       :GigabitEthernet0/0/9
----[Port10(GigabitEthernet0/0/10)][DISCARDING]----
 Port Protocol          :Enabled
 Port Role              :Alternate Port
 Port Priority         :128



                                                     HUAWEI TECHNOLOGIES   Page29
 Port Cost(Dot1T )    :Config=auto / Active=20000
 Designated Bridge/Port           :0.d0d0-4ba6-aab0 / 128.10
 Port Edged             :Config=default / Active=disabled
 Point-to-point        :Config=auto / Active=true
 Transit Limit        :6 packets/s
 Protection Type       :None
 Port STP Mode             :STP
 Port Protocol Type    :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes              :Hello 2s MaxAge 20s FwDly 15s RemHop 0
 TC or TCN send            :0
 TC or TCN received :18
 BPDU Sent                 :2
          TCN: 0, Config: 2, RST: 0, MST: 0
 BPDU Received             :3317
          TCN: 0, Config: 3317, RST: 0, MST: 0




Step 2 Control root bridge election.

Run the display stp command to view information about the root bridge.
<S1>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge           :0        .d0d0-4ba6-aab0
Config Times          :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times          :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC         :0         .d0d0-4ba6-aab0 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :0        .d0d0-4ba6-aab0 / 0
CIST RootPortId       :0.0
BPDU-Protection        :Disabled
CIST Root Type        :Primary root
TC or TCN received :11
TC count per hello :0
STP Converge Mode          :Normal
Share region-configuration :Enabled
Time since last TC :0 days 2h:32m:25s
……output omit……




<S2>display stp
-------[CIST Global Info][Mode STP]-------


                                                        HUAWEI TECHNOLOGIES       Page30
CIST Bridge             :4096 .d0d0-4ba6-ac20
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :0   .d0d0-4ba6-aab0 / 20000
CIST RegRoot/IRPC        :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId         :128.9 (GigabitEthernet0/0/9)
BPDU-Protection          :Disabled
CIST Root Type          :Secondary root
TC or TCN received :122
TC count per hello :0
STP Converge Mode         :Normal
Share region-configuration :Enabled
Time since last TC :0 days 2h:35m:57s
……output omit……


Configure S2 as the root bridge and S1 as the backup root bridge using priority
values. The device with the same value for the CIST Bridge and CIST Root/ERPC is
the root bridge. A smaller bridge priority value indicates a higher bridge priority.
Change the priorities of S1 and S2 to 8192 and 4096 respectively so that S2 becomes
the root bridge.
[S1]undo stp root
[S1]stp priority 8192


[S2]undo stp root
[S2]stp priority 4096


Run the display stp command to view information about the new root bridge.
<S1>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge             :8192 .d0d0-4ba6-aab0
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :4096 .d0d0-4ba6-ac20 / 20000
CIST RegRoot/IRPC        :8192 .d0d0-4ba6-aab0 / 0
CIST RootPortId         :128.9 (GigabitEthernet0/0/9)
BPDU-Protection          :Disabled
TC or TCN received :47
TC count per hello :0
STP Converge Mode         :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:6m:55s



                                                     HUAWEI TECHNOLOGIES   Page31
……output omit……


<S2>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge          :4096 .d0d0-4ba6-ac20
Config Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC        :4096 .d0d0-4ba6-ac20 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId      :0.0
BPDU-Protection       :Disabled
TC or TCN received :135
TC count per hello :0
STP Converge Mode        :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:8m:4s
……output omit……


The highlighted lines in the preceding information indicate that S2 has become the
new root bridge.
Shut down interfaces Gigabit Ethernet 0/0/9 and GigabitGigabitEthernet 0/0/10 on
S2 to isolate S2.
[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]shutdown
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]shutdown


<S1>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge          :8192 .d0d0-4ba6-aab0
Config Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC        :8192 .d0d0-4ba6-aab0 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :8192 .d0d0-4ba6-aab0 / 0
CIST RootPortId      :0.0
BPDU-Protection       :Disabled
TC or TCN received :174
TC count per hello :0
STP Converge Mode        :Normal
Share region-configuration :Enabled



                                                    HUAWEI TECHNOLOGIES     Page32
Time since last TC :0 days 0h:12m:51s
……output omit……


The highlighted lines in the preceding information indicate that S1 becomes the root
bridge when S2 is faulty.
Re-enable the interfaces that have been disabled on S2.
[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]undo shutdown
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]undo shutdown


<S1>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge          :8192 .d0d0-4ba6-aab0
Config Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC        :4096 .d0d0-4ba6-ac20 / 20000
CIST RegRoot/IRPC       :8192 .d0d0-4ba6-aab0 / 0
CIST RootPortId      :128.9 (GigabitEthernet0/0/9)
BPDU-Protection       :Disabled
TC or TCN received :47
TC count per hello :0
STP Converge Mode        :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:6m:55s
……output omit……


<S2>display stp
-------[CIST Global Info][Mode STP]-------
CIST Bridge          :4096 .d0d0-4ba6-ac20
Config Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times         :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC        :4096 .d0d0-4ba6-ac20 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId      :0.0
BPDU-Protection       :Disabled
TC or TCN received :135
TC count per hello :0
STP Converge Mode        :Normal
Share region-configuration :Enabled



                                                    HUAWEI TECHNOLOGIES     Page33
Time since last TC :0 days 0h:8m:4s
……output omit……


The highlighted lines in the preceding information indicate that S2 has been
restored and has become the root bridge once again.

Step 3 Control root port election.

Run the display stp brief command on S1 to view the roles of the interfaces.
<S1>display stp brief
    MSTID      Port                     Role STP State Protection
    0          GigabitEthernet0/0/9     ROOT      FORWARDINGNONE
    0          GigabitEthernet0/0/10 ALTE         DISCARDING NONE


The preceding information shows that G0/0/9 is the root port and G0/0/10 is the
alternate port. You can change port priorities so that port interface G0/0/10 will
become the root port and G0/0/9 will become the alternate port.
Change priorities of G0/0/9 and G0/0/10 on S2.
The default port priority is 128. A larger port priority value indicates a lower priority.
The priorities of G0/0/9 and G0/0/10 on S2 are set to 32 and 16; therefore, G0/0/10
on S1 becomes the root port.
[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]stp port priority 32
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]stp port priority 16


Note that the port priorities are changed on S2, not S1.
<S2>display stp interface GigabitEthernet 0/0/9
-------[CIST Global Info][Mode STP]-------
CIST Bridge           :4096 .d0d0-4ba6-ac20
Config Times          :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times          :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC          :4096 .d0d0-4ba6-ac20 / 0 (This bridge is the root)
CIST RegRoot/IRPC       :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId       :0.0
BPDU-Protection         :Disabled
TC or TCN received :147
TC count per hello :0



                                                    HUAWEI TECHNOLOGIES        Page34
STP Converge Mode            :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:7m:35s
Number of TC             :41
Last TC occurred       :GigabitEthernet0/0/10
----[Port34(GigabitEthernet0/0/9)][FORWARDING]----
 Port Protocol          :Enabled
 Port Role              :Designated Port
 Port Priority         :32
 Port Cost(Dot1T )      :Config=auto / Active=20000
 Designated Bridge/Port             :4096.d0d0-4ba6-ac20 / 32.34
 Port Edged              :Config=default / Active=disabled
 Point-to-point         :Config=auto / Active=true
 Transit Limit         :6 packets/s
 Protection Type         :None
 Port STP Mode               :STP
 Port Protocol Type     :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes               :Hello 2s MaxAge 20s FwDly 15s RemHop 20
 TC or TCN send           :35
 TC or TCN received :2
 BPDU Sent                   :1013
             TCN: 0, Config: 1013, RST: 0, MST: 0
 BPDU Received               :2
             TCN: 2, Config: 0, RST: 0, MST: 0
 Last forwarding time: 2016/11/22 10:00:00 UTC


<S2>display stp interface GigabitEthernet 0/0/10
-------[CIST Global Info][Mode STP]-------
CIST Bridge            :4096 .d0d0-4ba6-ac20
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times           :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :4096 .d0d0-4ba6-ac20 / 0 (This bridge is the root)
CIST RegRootIRPC        :4096 .d0d0-4ba6-ac20 / 0
CIST RootPortId         :0.0
BPDU-Protection          :Disabled
TC or TCN received :147
TC count per hello :0
STP Converge Mode            :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:8m:19s
Number of TC             :41



                                                        HUAWEI TECHNOLOGIES    Page35
Last TC occurred        :GigabitEthernet0/0/10
----[Port35(GigabitEthernet0/0/10)][FORWARDING]----
 Port Protocol           :Enabled
 Port Role               :Designated Port
 Port Priority          :16
 Port Cost(Dot1T )       :Config=auto / Active=20000
 Designated Bridge/Port              :4096.d0d0-4ba6-ac20 / 16.35
 Port Edged               :Config=default / Active=disabled
 Point-to-point          :Config=auto / Active=true
 Transit Limit          :6 packets/s
 Protection Type         :None
 Port STP Mode                :STP
 Port Protocol Type      :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes                :Hello 2s MaxAge 20s FwDly 15s RemHop 20
 TC or TCN send           :35
 TC or TCN received :1
 BPDU Sent                    :1032
             TCN: 0, Config: 1032, RST: 0, MST: 0
 BPDU Received                :2
             TCN: 1, Config: 1, RST: 0, MST: 0
 Last forwarding time: 2016/11/22 10:00:11 UTC


Run the display stp brief command on S1 to view the role of the interfaces.
<S1>display stp brief
     MSTID       Port                          Role STP State Protection
     0           GigabitEthernet0/0/9          ALTE     DISCARDING NONE
     0           GigabitEthernet0/0/10 ROOT             FORWARDINGNONE


The highlighted lines in the preceding information indicate that G0/0/10 on S1 has
become the root port and G0/0/9 has become the alternate port.
Shut down G0/0/10 on S1 and view the port roles.
[S1]interface GigabitEthernet 0/0/10
[S1-GigabitEthernet0/0/10]shutdown
<S1>display stp brief
     MSTID       Port                          Role STP State Protection
     0           GigabitEthernet0/0/9          ROOT     FORWARDINGNONE


The highlighted line in the preceding information indicates that G0/0/9 has become
the root port. Resume the default priorities of G0/0/9 and G0/0/10 on S2 and


                                                         HUAWEI TECHNOLOGIES   Page36
re-enable the shutdown interfaces on S1.
[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]undo stp port priority
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]undo stp port priority



[S1]interface GigabitEthernet 0/0/10
[S1-GigabitEthernet0/0/10]undo shutdown


Run the display stp brief and display stp interface command on S1 to view the
roles of interfaces.
<S1>display stp brief
     MSTID       Port                       Role STP State Protection
     0           GigabitEthernet0/0/9       ROOT     FORWARDINGNONE
     0           GigabitEthernet0/0/10 ALTE          DISCARDING NONE



[S1]display stp interface GigabitEthernet 0/0/9
----[CIST][Port9(GigabitEthernet0/0/9)][FORWARDING]----
 Port Protocol           :Enabled
 Port Role               :Root Port
 Port Priority          :128
 Port Cost(Dot1T )       :Config=auto / Active=20000
 Designated Bridge/Port          :4096.4c1f-cc45-aacc / 128.9
 Port Edged               :Config=default / Active=disabled
 Point-to-point          :Config=auto / Active=true
 Transit Limit          :147 packets/hello-time
 Protection Type         :None
 Port STP Mode            :STP
 Port Protocol Type      :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes                :Hello 2s MaxAge 20s FwDly 15s RemHop 0
 TC or TCN send           :4
 TC or TCN received :90
 BPDU Sent                :5
             TCN: 4, Config: 1, RST: 0, MST: 0
 BPDU Received             :622
             TCN: 0, Config: 622, RST: 0, MST: 0


[S1]display stp interface GigabitEthernet 0/0/10



                                                      HUAWEI TECHNOLOGIES   Page37
----[CIST][Port10(GigabitEthernet0/0/10)][DISCARDING]----
 Port Protocol          :Enabled
 Port Role              :Alternate Port
 Port Priority         :128
 Port Cost(Dot1T )      :Config=auto / Active=20000
 Designated Bridge/Port          :4096.4c1f-cc45-aacc / 128.10
 Port Edged              :Config=default / Active=disabled
 Point-to-point         :Config=auto / Active=true
 Transit Limit         :147 packets/hello-time
 Protection Type         :None
 Port STP Mode            :STP
 Port Protocol Type     :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes               :Hello 2s MaxAge 20s FwDly 15s RemHop 0
 TC or TCN send          :3
 TC or TCN received :90
 BPDU Sent                :4
             TCN: 3, Config: 1, RST: 0, MST: 0
 BPDU Received            :637
             TCN: 0, Config: 637, RST: 0, MST: 0


The greyed line in the preceding information indicates that G0/0/9 and G0/0/10 cost
is 20000 by default.
Change the cost of G0/0/9 to 200000 on S1.
[S1]interface GigabitEthernet 0/0/9
[S1-GigabitEthernet0/0/9]stp cost 200000


Run the display stp brief and display stp interface command on S1 to view the
roles of interfaces.
<S1>display stp interface GigabitEthernet 0/0/9
----[CIST][Port9(GigabitEthernet0/0/9)][DISCARDING]----
 Port Protocol          :Enabled
 Port Role              :Alternate Port
 Port Priority         :128
 Port Cost(Dot1T )      :Config=200000 / Active=200000
 Designated Bridge/Port          :4096.4c1f-cc45-aacc / 128.9
 Port Edged              :Config=default / Active=disabled
 Point-to-point         :Config=auto / Active=true
 Transit Limit         :147 packets/hello-time
 Protection Type         :None




                                                      HUAWEI TECHNOLOGIES   Page38
 Port STP Mode           :STP
 Port Protocol Type     :Config=auto / Active=dot1s
 BPDU Encapsulation :Config=stp / Active=stp
 PortTimes              :Hello 2s MaxAge 20s FwDly 15s RemHop 0
 TC or TCN send          :4
 TC or TCN received :108
 BPDU Sent               :5
          TCN: 4, Config: 1, RST: 0, MST: 0
 BPDU Received           :818
          TCN: 0, Config: 818, RST: 0, MST: 0


<S1>display stp brief
    MSTID       Port                    Role STP State Protection
    0           GigabitEthernet0/0/9    ALTE     DISCARDING NONE
    0           GigabitEthernet0/0/10 ROOT       FORWARDINGNONE


The highlighted lines in the preceding information indicates that G0/0/10 has
become the root port.


Final Configuration

<S1>display current-configuration
#
!Software Version V200R008C00SPC500
 sysname S1
#
 stp mode stp
 stp instance 0 priority 8192
#
interface GigabitEthernet0/0/1
 shutdown
#
interface GigabitEthernet0/0/2
 shutdown
#
interface GigabitEthernet0/0/3
 shutdown
#
interface GigabitEthernet0/0/9
 stp instance 0 cost 200000
#
interface GigabitEthernet0/0/10


                                                  HUAWEI TECHNOLOGIES   Page39
#
interface GigabitEthernet0/0/13
 shutdown
#
interface GigabitEthernet0/0/14
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return


<S2>display current-configuration
#
!Software Version V200R008C00SPC500
 sysname S2
#
 stp mode stp
 stp instance 0 priority 4096
#
interface GigabitEthernet0/0/1
 shutdown
#
interface GigabitEthernet0/0/2
 shutdown
#
interface GigabitEthernet0/0/3
 shutdown
#
interface GigabitEthernet0/0/6
 shutdown
#
interface GigabitEthernet0/0/7
 shutdown
#
interface GigabitEthernet0/0/9
#
interface GigabitEthernet0/0/10
#
user-interface con 0
user-interface vty 0 4
#



                                      HUAWEI TECHNOLOGIES   Page40
return


<S3>display current-configuration
#
!Software Version V100R006C05
 sysname S3
#
interface GigabitEthernet0/0/1
 shutdown
#
interface GigabitEthernet0/0/13
 shutdown
#
interface GigabitEthernet0/0/7
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return


<S4>display current-configuration
#
!Software Version V100R006C05
 sysname S4
#
interface GigabitEthernet0/0/14
 shutdown
#
interface Gigabit
Ethernet0/0/1
 shutdown
#
interface GigabitEthernet0/0/6
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return




                                    HUAWEI TECHNOLOGIES   Page41
Lab 3-2 Configuring RSTP


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

       Enable and disable RSTP .

       Configuration of an edge port.

       Configuration of RSTP BPDU protection.

       Configuration of RSTP loop protection


Topology




                                Figure 3.2 RSTP topology



Scenario

Assume that you are a network administrator of a company. The company network
consists of two layers: core layer and access layer. The network uses a redundancy
design. RSTP will be used to prevent loops. You can configure features to speed up
RSTP route convergence at the edge network and configure RSTP protection
function.


Tasks


Step 1 Preparing the environment

If you are starting this section with a non-configured device, begin here and then


                                      HUAWEI TECHNOLOGIES                  Page42
move to step 3. For those continuing from previous labs, begin at step 2.

Irrelevant interfaces must be disabled to ensure test result accuracy.
Shut down port interfacesGigabitEthernet 0/0/1 on S3,GigabitEthernet 0/0/13 and
Ethernet 0/0/7 on S3; GigabitEthernet 0/0/1, GigabitEthernet 0/0/2, GigabitEthernet
0/0/3, GigabitEthernet 0/0/13, GigabitEthernet 0/0/14 on S1; GigabitEthernet 0/0/1,
GigabitEthernet 0/0/2, GigabitEthernet 0/0/3, GigabitEthernet 0/0/6, GigabitEthernet
0/0/7 on S2;       as well asGigabitEthernet 0/0/1,GigabitEthernet 0/0/14 and
GigabitEthernet 0/0/6 on S4 before starting STP configuration. Ensure that the
devices start without any configuration files. If STP is disabled, run the stp enable
command to enable STP.


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S1
[S1]interface GigabitEthernet 0/0/1
[S1-GigabitEthernet0/0/1]shutdown
[S1-GigabitEthernet0/0/1]quit
[S1]interface GigabitEthernet 0/0/2
[S1-GigabitEthernet0/0/2]shutdown
[S1-GigabitEthernet0/0/2]quit
[S1]interface GigabitEthernet 0/0/3
[S1-GigabitEthernet0/0/3]shutdown
[S1-GigabitEthernet0/0/3]quit
[S1]interface GigabitEthernet 0/0/13
[S1-GigabitEthernet0/0/13]shutdown
[S1-GigabitEthernet0/0/13]quit
[S1]interface GigabitEthernet 0/0/14
[S1-GigabitEthernet0/0/14]shutdown
[S1-GigabitEthernet0/0/14]quit


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S2
[S2]interface GigabitEthernet 0/0/1
[S2-GigabitEthernet0/0/1]shutdown
[S2-GigabitEthernet0/0/1]quit
[S2]interface GigabitEthernet 0/0/2
[S2-GigabitEthernet0/0/2]shutdown
[S2-GigabitEthernet0/0/2]quit
[S2]interface GigabitEthernet 0/0/3



                                                   HUAWEI TECHNOLOGIES      Page43
[S2-GigabitEthernet0/0/3]shutdown
[S2-GigabitEthernet0/0/3]quit
[S2]interface GigabitEthernet 0/0/6
[S2-GigabitEthernet0/0/6]shutdown
[S2-GigabitEthernet0/0/6]quit
[S2]interface GigabitEthernet 0/0/7
[S2-GigabitEthernet0/0/7]shutdown
[S2-GigabitEthernet0/0/7]quit


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S3
[S3]interface GigabitEthernet 0/0/1
[S3-GigabitEthernet 0/0/1]shutdown
[S3-GigabitEthernet 0/0/1]quit
[S3]interface GigabitEthernet 0/0/13
[S3-GigabitEthernet 0/0/13]shutdown
[S3-GigabitEthernet 0/0/13]quit
[S3]interface GigabitEthernet 0/0/7
[S3-GigabitEthernet0/0/7]shutdown


<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S4
[S4]interface GigabitEthernet 0/0/1
[S4-GigabitEthernet 0/0/1]shutdown
[S4-GigabitEthernet 0/0/1]quit
[S4]interface GigabitEthernet 0/0/14
[S4-GigabitEthernet 0/0/14]shutdown
[S4-GigabitEthernet 0/0/14]quit
[S4]interface GigabitEthernet 0/0/6
[S4-GigabitEthernet0/0/6]shutdown




Step 2 Clean up the previous configuration


Remove the configured STP priority from S1 and S2, and assigned cost on S1.

[S1]undo stp priority
[S1]inter GigabitEthernet 0/0/9




                                                   HUAWEI TECHNOLOGIES   Page44
[S1-GigabitEthernet0/0/9]undo stp cost


[S2]undo stp priority

Step 3 Configure RSTP and verify the RSTP configuration.

Configure S1 and S2 to use RSTP as the spanning tree protocol.
 [S1]stp mode rstp


 [S2]stp mode rstp


Run the display stp command to view brief information about RSTP.
[S1]display stp
-------[CIST Global Info][Mode RSTP]-------
CIST Bridge             :32768.d0d0-4ba6-aab0
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :32768.d0d0-4ba6-aab0 / 0 (This bridge is the root)
CIST RegRoot/IRPC        :32768.d0d0-4ba6-aab0 / 0
CIST RootPortId         :0.0
BPDU-Protection          :Disabled
TC or TCN received :362
TC count per hello :0
STP Converge Mode         :Normal
Share region-configuration :Enabled
Time since last TC :0 days 0h:0m:45s
……output omit……


[S2]display stp
-------[CIST Global Info][Mode RSTP]-------
CIST Bridge             :32768.d0d0-4ba6-ac20
Config Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
Active Times            :Hello 2s MaxAge 20s FwDly 15s MaxHop 20
CIST Root/ERPC           :32768.d0d0-4ba6-aab0 / 20000
CIST RegRoot/IRPC        :32768.d0d0-4ba6-ac20 / 0
CIST RootPortId         :128.34 (GigabitEthernet0/0/9)
BPDU-Protection          :Disabled
TC or TCN received :186
TC count per hello :0
STP Converge Mode         :Normal
Share region-configuration :Enabled



                                                    HUAWEI TECHNOLOGIES        Page45
Time since last TC :0 days 0h:3m:55s
……output omit……



Step 4 Configure an edge port.

Configure ports connected to the user terminals as edge ports. An edge port can
transition to the forwarding state without participating in the RSTP calculation. In
this example, interface GigabitEthernet 0/0/1 on S1 and S2 connect to a router and
can be configured as edge ports.
[S1]interface GigabitEthernet 0/0/1
[S1-GigabitEthernet0/0/1]undo shutdown
[S1-GigabitEthernet0/0/1]stp edged-port enable


[S2]interface GigabitEthernet 0/0/1
[S2-GigabitEthernet0/0/1]undo shutdown
[S2-GigabitEthernet0/0/1]stp edged-port enable



Step 5 Configure BPDU protection.

Edge ports are directly connected to user terminal and will not receive BPDUs.
Attackers may send pseudo BPDUs to attack the switching device. If the edge ports
receive the BPDUs, the switching device configures the edge ports as non-edge
ports and triggers a new spanning tree calculation. Network flapping then occurs.
BPDU protection can be used to protect switching devices against malicious attacks.
Configure BPDU protection on both S1 and S2.
[S1]stp bpdu-protection


[S2]stp bpdu-protection


Run the display stp brief command to view the port protection.
<S1>display stp brief
 MSTID Port                            Role STP State    Protection
   0    GigabitEthernet0/0/1           DESI FORWARDING        BPDU
   0    GigabitEthernet0/0/9           DESI FORWARDING        NONE
   0    GigabitEthernet0/0/10          DESI FORWARDING        NONE


<S2>display stp brief
 MSTID Port                            Role STP State    Protection



                                                 HUAWEI TECHNOLOGIES     Page46
   0    GigabitEthernet0/0/1           DESI FORWARDING        BPDU
   0    GigabitEthernet0/0/9           ROOT    FORWARDING      NONE
   0    GigabitEthernet0/0/10          ALTE DISCARDING       NONE


After the configuration is complete, interface GigabitGigabitEthernet 0/0/1 on S1
and S2 shows as supporting BPDU protection.



Step 6 Configure Loop protection

On a network running RSTP, a switching device maintains the root port status and
status of alternate ports by receiving BPDUs from an upstream switching device. If
the switching device cannot receive BPDUs from the upstream device because of link
congestion or unidirectional-link failure, the switching device re-selects a root port.
The original root port becomes a designated port and the original discarding ports
change to the Forwarding state. This switching may cause network loops, which can
be mitigated by configuring loop protection.
Configure loop protection on both the root port and the alternate port.
[S2]display stp brief
 MSTID Port                            Role STP State    Protection
   0    GigabitEthernet0/0/1           DESI FORWARDING        BPDU
   0    GigabitEthernet0/0/9           ROOT    FORWARDING      NONE
   0    GigabitEthernet0/0/10          ALTE DISCARDING       NONE


G0/0/9 and G0/0/10 on S2 are now the root port and alternate port. Configure loop
protection on these two ports.
[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]stp loop-protection
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]stp loop-protection



Run the display stp brief command to view the port protection.
<S2>display stp brief
 MSTID Port                            Role STP State    Protection
   0    GigabitEthernet0/0/1           DESI FORWARDING        BPDU
   0    GigabitEthernet0/0/9           ROOT    FORWARDING      LOOP
   0    GigabitEthernet0/0/10          ALTE DISCARDING       LOOP




                                                 HUAWEI TECHNOLOGIES        Page47
Since S1 is root, all the ports are designated ports and therefore do not need to
configure loop protection. After completing the configuration, you may wish to set
S2 as the root, and configure loop protection on the root port and alternate port of
S1 using the same process as with S2.


Final Configuration

<S1>display current-configuration
#
!Software Version V200R008C00SPC500
 sysname S1
#
 stp mode rstp
 stp bpdu-protection
#
interface GigabitEthernet0/0/1
undo shutdown
 stp edged-port enable
#
interface GigabitEthernet0/0/2
 shutdown
#
interface GigabitEthernet0/0/3
 shutdown
#
interface GigabitEthernet0/0/13
 shutdown
#
interface GigabitEthernet0/0/14
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return


<S2>display current-configuration
#
!Software Version V200R008C00SPC500
 sysname S2




                                      HUAWEI TECHNOLOGIES                Page48
#
 stp mode rstp
 stp bpdu-protection
#
interface GigabitEthernet0/0/1
undo shutdown
 stp edged-port enable
#
interface GigabitEthernet0/0/2
 shutdown
#
interface GigabitEthernet0/0/3
 shutdown
#
interface GigabitEthernet0/0/6
 shutdown
#
interface GigabitEthernet0/0/7
 shutdown
#
interface GigabitEthernet0/0/9
 stp loop-protection
#
interface GigabitEthernet0/0/10
 stp loop-protection
#
user-interface con 0
user-interface vty 0 4
#
return


<S3>display current-configuration
#
!Software Version V100R006C05
 sysname S3
#
interface GigabitEthernet0/0/1
 shutdown
#
interface GigabitEthernet0/0/13
 shutdown
#



                                    HUAWEI TECHNOLOGIES   Page49
interface GigabitEthernet0/0/7
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return


<S4>display current-configuration
#
!Software Version V100R006C05
 sysname S4
#
interface GigabitEthernet0/0/14
 shutdown
#
interface GigabitEthernet0/0/1
 shutdown
#


interface GigabitEthernet0/0/6
 shutdown
#
user-interface con 0
user-interface vty 0 4
#
return




                                    HUAWEI TECHNOLOGIES   Page50
                   Module 4 Routing Configuration

Lab 4-1 Configuring Static Routes and Default Routes


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

      Configuration of a static route using an interface and an IP address as the

       next hop.

      Verification of static route operation.

      Implementation of the interconnection between a local and external network

       using a default route.

      Configuration of a backup static route on a router.


Topology




                   Figure 4.1 Lab topology for static and default routes


                                       HUAWEI TECHNOLOGIES                 Page51
Scenario

Assume that you are a network administrator of a company that contains a single
administrative domain and within the administrative domain, multiple networks
have been defined, for which currently no method of routing exists.
Since the network scale is small, with only a few networks, static routes and default
routes are to be used to implement interwork communication. The network
addressing is to be applied as shown in Figure 4.1.
If a password is requested, and unless otherwise stated, please use the password:
huawei


Tasks


Step 1 Perform basic system and IP address configuration.

Configure the device names and IP addresses for R1, R2, and R3.
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R1
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ip address 10.0.13.1 24
[R1-GigabitEthernet0/0/0]quit
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]ip address 10.0.12.1 24
[R1-GigabitEthernet0/0/1]quit
[R1]interface LoopBack 0
[R1-LoopBack0]ip address 10.0.1.1 24




Run the display current-configuration command to check the configuration.
<R1>display ip interface brief
Interface                          IP Address/Mask       Physical   Protocol
......output omitted......
GigabitEthernet0/0/0              10.0.13.1/24           up          up
GigabitEthernet0/0/1              10.0.12.1/24           up          up
GigabitEthernet0/0/2              unassigned             up          down
LoopBack0                         10.0.1.1/24            up          up(s)


                                                   HUAWEI TECHNOLOGIES         Page52
......output omitted......


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R2
[R2]interface GigabitEthernet 0/0/1
[R2-GigabitEthernet0/0/1]ip address 10.0.12.2 24
[R2-GigabitEthernet0/0/1]quit
[R2]interface GigabitEthernet0/0/2
[R2-GigabitEthernet0/0/2]ip add 10.0.23.2 24
[R2-GigabitEthernet0/0/2]quit
[R2]interface LoopBack0
[R2-LoopBack0]ip address 10.0.2.2 24




<R2>display ip interface brief
Interface                            IP Address/Mask        Physical    Protocol
......output omitted......
GigabitEthernet0/0/0              unassigned                up           down
GigabitEthernet0/0/1              10.0.12.2/24              up           up
GigabitEthernet0/0/2              10.0.23.2/24              up           up
LoopBack0                         10.0.2.2/24               up           up(s)
......output omitted......


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R3
[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ip address 10.0.13.3 24
[R3-GigabitEthernet0/0/0]quit
[R3]interface GigabitEthernet0/0/2
[R3-GigabitEthernet0/0/2]ip address 10.0.23.3 24
[R3-GigabitEthernet0/0/2]quit
[R3]interface LoopBack 0
[R3-LoopBack0]ip address 10.0.3.3 24


<R3>display ip interface brief
Interface                            IP Address/Mask        Physical    Protocol
......output omitted......
GigabitEthernet0/0/0              10.0.13.3/24         up          up
GigabitEthernet0/0/1              unassigned           up          down
GigabitEthernet0/0/2              10.0.23.3/24         up          up



                                                   HUAWEI TECHNOLOGIES             Page53
LoopBack0                             10.0.3.3/24         up        up(s)
......output omitted......


Use the ping command to test network connectivity from R1.
<R1>ping 10.0.12.2
  PING 10.0.12.2: 56 data bytes, press CTRL_C to break
     Reply from 10.0.12.2: bytes=56 Sequence=1 ttl=255 time=30 ms
     Reply from 10.0.12.2: bytes=56 Sequence=2 ttl=255 time=30 ms
     Reply from 10.0.12.2: bytes=56 Sequence=3 ttl=255 time=30 ms
     Reply from 10.0.12.2: bytes=56 Sequence=4 ttl=255 time=30 ms
     Reply from 10.0.12.2: bytes=56 Sequence=5 ttl=255 time=30 ms


  --- 10.0.12.2 ping statistics ---
     5 packet(s) transmitted
     5 packet(s) received
     0.00% packet loss
     round-trip min/avg/max = 30/30/30 ms


<R1>ping 10.0.13.3
  PING 10.0.13.2: 56 data bytes, press CTRL_C to break
     Reply from 10.0.13.3: bytes=56 Sequence=1 ttl=255 time=6 ms
     Reply from 10.0.13.3: bytes=56 Sequence=2 ttl=255 time=2 ms
     Reply from 10.0.13.3: bytes=56 Sequence=3 ttl=255 time=2 ms
     Reply from 10.0.13.3: bytes=56 Sequence=4 ttl=255 time=2 ms
     Reply from 10.0.13.3: bytes=56 Sequence=5 ttl=255 time=2 ms


  --- 10.0.13.3 ping statistics ---
     5 packet(s) transmitted
     5 packet(s) received
     0.00% packet loss
     round-trip min/avg/max = 2/2/6 ms


Use the ping command to test network connectivity from R2


<R2>ping 10.0.23.3
  PING 10.0.23.3: 56 data bytes, press CTRL_C to break
     Reply from 10.0.23.3: bytes=56 Sequence=1 ttl=255 time=31 ms
     Reply from 10.0.23.3: bytes=56 Sequence=2 ttl=255 time=31 ms
     Reply from 10.0.23.3: bytes=56 Sequence=3 ttl=255 time=41 ms
     Reply from 10.0.23.3: bytes=56 Sequence=4 ttl=255 time=31 ms
     Reply from 10.0.23.3: bytes=56 Sequence=5 ttl=255 time=41 ms



                                                    HUAWEI TECHNOLOGIES     Page54
 --- 10.0.23.3 ping statistics ---
   5 packet(s) transmitted
   5 packet(s) received
   0.00% packet loss
   round-trip min/avg/max = 31/35/41 ms




Step 2 Test connectivity

Use the ping command to test network connectivity from R2 to neworks
10.0.13.0/24 and 10.0.3.0/24
<R2>ping 10.0.13.3
 PING 10.0.13.3: 56 data bytes, press CTRL_C to break
   Request time out
   Request time out
   Request time out
   Request time out
   Request time out


 --- 10.0.13.3 ping statistics ---
   5 packet(s) transmitted
   0 packet(s) received
   100.00% packet loss


<R2>ping 10.0.3.3
 PING 10.0.3.3: 56 data bytes, press CTRL_C to break
   Request time out
   Request time out
   Request time out
   Request time out
   Request time out


 --- 10.0.3.3 ping statistics ---
   5 packet(s) transmitted
   0 packet(s) received
100.00% packet loss


If R2 wishes to communicate with the network segment 10.0.3.0, a route destined for
this network segment must be configured on R2, and routes destined for the R2
interface must be configured on R3.


                                              HUAWEI TECHNOLOGIES        Page55
The preceding test result shows that R2 cannot communicate with 10.0.3.3 and
10.0.13.3.


Run the display ip routing-table command to view the routing table of R2. The
routing table does not contain the routes of the two networks.
<R2>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 13         Routes : 13
Destination/Mask        Proto    Pre Cost Flags NextHop               Interface
     10.0.2.0/24           Direct 0      0            D    10.0.2.2 LoopBack0
     10.0.2.2/32           Direct 0      0            D    127.0.0.1 LoopBack0
     10.0.2.255/32         Direct 0      0            D    127.0.0.1 LoopBack0
     10.0.12.0/24          Direct 0      0            D    10.0.12.2 GigabitEthernet0/0/1
     10.0.12.2/32          Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/1
     10.0.23.0/24          Direct 0      0            D    10.0.23.2 GigabitEthernet0/0/2
     10.0.23.2/32          Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/2
     10.0.23.255/32        Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/2
     127.0.0.0/8           Direct 0      0            D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct 0      0            D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0



Step 3 Configure static routes on R2.

Configure a static route for destination networks 10.0.13.0/24 and 10.0.3.0/24, with
the next hop set as the IP address 10.0.23.3 of R3, a preference value of 60 is the
default and need not be set.
[R2]ip route-static 10.0.13.0 24 10.0.23.3
[R2]ip route-static 10.0.3.0 24 10.0.23.3


Note: In the ip route-static command, 24 indicates the subnet mask length, which
can also be expressed using the decimal format 255.255.255.0.
<R2>display ip routing-table

Route Flags: R - relay, D - download to fib




                                                      HUAWEI TECHNOLOGIES                   Page56
Destination/Mask        Proto       Pre Cost Flags NextHop           Interface

   10.0.3.0/24             Static      60   0       RD 10.0.23.3           GigabitEthernet0/0/2
   10.0.12.0/24            Direct      0    0       D    10.0.12.2         GigabitEthernet0/0/1
   10.0.12.2/32            Direct      0    0       D    127.0.0.1         GigabitEthernet0/0/1
   10.0.12.255/32          Direct      0    0       D    127.0.0.1         GigabitEthernet0/0/1
   10.0.13.0/24            Static      60   0       RD 10.0.23.3           GigabitEthernet0/0/2
   10.0.23.0/24            Direct      0    0       D    10.0.23.2         GigabitEthernet0/0/2
   10.0.23.2/32            Direct      0    0       D    127.0.0.1         GigabitEthernet0/0/2



Step 4 Configure backup static routes.

The data exchanged between R2 and 10.0.13.3 and 10.0.3.3 is transmitted through
the link between R2 and R3. R2 fails to communicate with 10.0.13.3 and 10.0.3.3 if
the link between R2 and R3 is faulty.
According to the topology, R2 can communicate with R3 through R1 if the link
between R2 and R3 fails. A backup static route can be configured to enable this
redundancy. Backup static routes do not take effect in normal cases. If the link
between R2 and R3 fails, backup static routes are used to transfer data.
Amend th preferences for on the backup static routes to ensure that the routes are
used only when the primary link fails. In this example, the preference of the backup
static route is set to 80.
[R1]ip route-static 10.0.3.0 24 10.0.13.3


[R2]ip route-static 10.0.13.0 255.255.255.0 10.0.12.1 preference 80
[R2]ip route-static 10.0.3.0 24 10.0.12.1 preference 80


[R3]ip route-static 10.0.12.0 24 10.0.13.1




Step 5 Test the static routes.

View the current static route configuration in the routing table of R2.
<R2>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 15           Routes : 15
  Destination/Mask      Proto       Pre Cost    Flags NextHop        Interface



                                                        HUAWEI TECHNOLOGIES                       Page57
       10.0.2.0/24        Direct 0    0       D   10.0.2.2 LoopBack0
       10.0.2.2/32        Direct 0    0       D   127.0.0.1 LoopBack0
     10.0.2.255/32        Direct 0    0       D   127.0.0.1 LoopBack0
       10.0.3.0/24        Static 60   0   RD 10.0.23.3 GigabitEthernet0/0/2
      10.0.12.0/24        Direct 0    0       D   10.0.12.2 GigabitEthernet0/0/1
      10.0.12.2/32        Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/1
    10.0.12.255/32        Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/1
      10.0.13.0/24        Static 60   0   RD 10.0.23.3 GigabitEthernet0/0/2
      10.0.23.0/24        Direct 0    0       D   10.0.23.2 GigabitEthernet0/0/2
      10.0.23.2/32        Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/2
    10.0.23.255/32        Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/2
       127.0.0.0/8        Direct 0    0       D   127.0.0.1 InLoopBack0
      127.0.0.1/32        Direct 0    0       D   127.0.0.1 InLoopBack0
127.255.255.255/32        Direct 0    0       D   127.0.0.1 InLoopBack0
255.255.255.255/32        Direct 0    0       D   127.0.0.1 InLoopBack0


The routing table contains two static routes that were configured in step 3. The value
of the Protocol field is Static, indicating a static route. The value of the Preference
field is 60, indicating the default preference is used for the route.
Test network connectivity to ensure the route between R2 and R3 exists.
<R2>ping 10.0.13.3
 PING 10.0.13.3: 56 data bytes, press CTRL_C to break
   Reply from 10.0.13.3: bytes=56 Sequence=1 ttl=255 time=34 ms
   Reply from 10.0.13.3: bytes=56 Sequence=2 ttl=255 time=34 ms
   Reply from 10.0.13.3: bytes=56 Sequence=3 ttl=255 time=34 ms
   Reply from 10.0.13.3: bytes=56 Sequence=4 ttl=255 time=34 ms
   Reply from 10.0.13.3: bytes=56 Sequence=5 ttl=255 time=34 ms


 --- 10.0.13.3 ping statistics ---
   5 packet(s) transmitted
   5 packet(s) received
   0.00% packet loss
   round-trip min/avg/max = 34/34/34 ms


<R2>ping 10.0.3.3
 PING 10.0.3.3: 56 data bytes, press CTRL_C to break
   Reply from 10.0.3.3: bytes=56 Sequence=1 ttl=255 time=41 ms
   Reply from 10.0.3.3: bytes=56 Sequence=2 ttl=255 time=41 ms
   Reply from 10.0.3.3: bytes=56 Sequence=3 ttl=255 time=41 ms
   Reply from 10.0.3.3: bytes=56 Sequence=4 ttl=255 time=41 ms
   Reply from 10.0.3.3: bytes=56 Sequence=5 ttl=255 time=41 ms



                                               HUAWEI TECHNOLOGIES                 Page58
  --- 10.0.3.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
round-trip min/avg/max = 41/41/41 ms


The command output shows that the route is functioning normally. The tracert
command can also be run to view the path over which the data is transferred.
<R2>tracert 10.0.13.3
 traceroute to 10.0.13.3(10.0.13.3), max hops: 30 ,packet length: 40,
 press CTRL_C to break
 1 10.0.23.3 40 ms    31 ms 30 ms


<R2>tracert 10.0.3.3
 traceroute to 10.0.3.3(10.0.3.3), max hops: 30 ,packet length: 40,
 press CTRL_C to break
 1 10.0.23.3 40 ms    30 ms 30 ms



The command output verifies that R2 directly sends data to R3.

Step 6 Test the backup static routes.

Disable the path to 10.0.23.3 via GigabitEthernet0/0/2 on R2 and observe the
changes in the IP routing tables.
[R2]interface GigabitEthernet0/0/2
[R2-GigabitEthernet0/0/2]shutdown
[R2-GigabitEthernet0/0/2]quit


Compare the routing tables with the previous routing tables before Gigabit Ethernet
0/0/2 was disabled.
<R2>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 12          Routes : 12


Destination/Mask        Proto    Pre Cost Flags NextHop               Interface


     10.0.2.0/24           Direct 0      0           D     10.0.2.2        LoopBack0


                                                      HUAWEI TECHNOLOGIES              Page59
    10.0.2.2/32           Direct 0    0       D   127.0.0.1 LoopBack0
    10.0.2.255/32         Direct 0    0       D   127.0.0.1 LoopBack0
    10.0.3.0/24           Static 80   0      RD    10.0.12.1   GigabitEthernet0/0/1
    10.0.12.0/24          Direct 0    0       D   10.0.12.2 GigabitEthernet0/0/1
    10.0.12.2/32          Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/1
    10.0.12.255/32        Direct 0    0       D   127.0.0.1 GigabitEthernet0/0/1
    10.0.13.0/24          Static 80   0      RD    10.0.12.1 GigabitEthernet0/0/1
    127.0.0.0/8           Direct 0    0       D   127.0.0.1 InLoopBack0
    127.0.0.1/32          Direct 0    0       D   127.0.0.1 InLoopBack0
127.255.255.255/32        Direct 0    0       D   127.0.0.1 InLoopBack0
255.255.255.255/32        Direct 0    0       D   127.0.0.1 InLoopBack0



The next hops and preferences of the two routes as shown in the preceding routing
table for R2 have changed.
Test connectivity between R2 and the destination addresses 10.0.13.3 and 10.0.3.3
on R2.
<R2>ping 10.0.3.3
 PING 10.0.3.3: 56 data bytes, press CTRL_C to break
   Reply from 10.0.3.3: bytes=56 Sequence=1 ttl=255 time=3 ms
   Reply from 10.0.3.3: bytes=56 Sequence=2 ttl=255 time=2 ms
   Reply from 10.0.3.3: bytes=56 Sequence=3 ttl=255 time=2 ms
   Reply from 10.0.3.3: bytes=56 Sequence=4 ttl=255 time=2 ms
   Reply from 10.0.3.3: bytes=56 Sequence=5 ttl=255 time=2 ms


 --- 10.0.3.3 ping statistics ---
   5 packet(s) transmitted
   5 packet(s) received
   0.00% packet loss
   round-trip min/avg/max = 2/2/3 ms


<R2>ping 10.0.13.3
 PING 10.0.13.3: 56 data bytes, press CTRL_C to break
   Reply from 10.0.13.3: bytes=56 Sequence=1 ttl=255 time=3 ms
   Reply from 10.0.13.3: bytes=56 Sequence=2 ttl=255 time=2 ms
   Reply from 10.0.13.3: bytes=56 Sequence=3 ttl=255 time=2 ms
   Reply from 10.0.13.3: bytes=56 Sequence=4 ttl=255 time=2 ms
   Reply from 10.0.13.3: bytes=56 Sequence=5 ttl=255 time=2 ms


 --- 10.0.13.3 ping statistics ---
   5 packet(s) transmitted




                                              HUAWEI TECHNOLOGIES                     Page60
    5 packet(s) received
    0.00% packet loss
round-trip min/avg/max = 2/2/3 ms


The network is not disconnected when the link between R2 and R3 is shut down.


The tracert command can also be run to view through over which path the data is
being forwarded.
<R2>tracert 10.0.13.3
 traceroute to 10.0.13.3(10.0.13.3), max hops: 30 ,packet length: 40,press CTRL_C to break
 1 10.0.12.1 40 ms    21 ms 21 ms
 2 10.0.13.3 30 ms    21 ms 21 ms


<R2>tracert 10.0.3.3
 traceroute to 10.0.3.3(10.0.3.3), max hops: 30 ,packet length: 40,press CTRL_C to break
 1 10.0.12.1 40 ms    21 ms 21 ms
 2 10.0.13.3 30 ms    21 ms 21 ms


The command output shows that the data sent by R2 reaches R3 via the 10.0.12.0
and 10.0.13.0 networks connected to R1.

Step 7 Using default routes to implement network connectivity.

Enable the interface that was disabled in step 6 on R2.
[R2]interface GigabitEthernet 0/0/2
[R2-GigabitEthernet0/0/2]undo shutdown



Verify connectivity to the network 10.0.23.0 from R1.
[R1]ping 10.0.23.3
  PING 10.0.23.3: 56 data bytes, press CTRL_C to break
    Request time out
    Request time out
    Request time out
    Request time out
    Request time out


  --- 10.0.23.3 ping statistics ---
    5 packet(s) transmitted




                                                 HUAWEI TECHNOLOGIES                         Page61
    0 packet(s) received
100.00% packet loss


R3 cannot be reached because the route destined for 10.0.23.3 is not configured on
R1.


<R1>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 14           Routes : 14
Destination/Mask        Proto       Pre Cost Flags NextHop            Interface


     10.0.1.0/24           Direct     0     0         D    10.0.1.1 LoopBack0
     10.0.1.1/32           Direct     0     0         D    127.0.0.1 LoopBack0
     10.0.1.255/32         Direct     0     0         D    127.0.0.1 LoopBack0
     10.0.3.0/24           Static     60    0         RD 10.0.13.3 GigabitEthernet0/0/0
     10.0.12.0/24          Direct     0     0         D    10.0.12.1 GigabitEthernet0/0/1
     10.0.12.1/32          Direct     0     0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct     0     0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          Direct     0     0         D    10.0.13.1 GigabitEthernet0/0/0
     10.0.13.1/32          Direct     0     0         D    127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct     0     0         D    127.0.0.1 GigabitEthernet0/0/0
     127.0.0.0/8           Direct     0     0         D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct     0     0         D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct     0     0         D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct     0     0         D    127.0.0.1 InLoopBack0


A default route can be configured on R1 to implement network connectivity via a
next hop of 10.0.13.3.
[R1]ip route-static 0.0.0.0 0.0.0.0 10.0.13.3


After the configuration is complete, test connectivity between R1 and 10.0.23.3.
<R1>ping 10.0.23.3
  PING 10.0.23.3: 56 data bytes, press CTRL_C to break
    Reply from 10.0.23.3: bytes=56 Sequence=1 ttl=255 time=3 ms
    Reply from 10.0.23.3: bytes=56 Sequence=2 ttl=255 time=2 ms
    Reply from 10.0.23.3: bytes=56 Sequence=3 ttl=255 time=2 ms
    Reply from 10.0.23.3: bytes=56 Sequence=4 ttl=255 time=2 ms
    Reply from 10.0.23.3: bytes=56 Sequence=5 ttl=255 time=2 ms




                                                      HUAWEI TECHNOLOGIES                   Page62
  --- 10.0.23.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
round-trip min/avg/max = 2/2/3 ms
The default route forwards traffic destined for 10.0.23.3 to the next hop of 10.0.13.3
on R3. R3 is directly connected to the 10.0.23.0 network.

Step 8 Configure a backup default route.

If the link between R1 and R3 fails, a backup default route can be used to
communicate with 10.0.23.3 and 10.0.3.3 via the 10.0.12.0 network.
However, R1 is not directly connected to these networks and therefore a backup
route (in both directions) must be configured to provide a forwarding path.
[R1]ip route-static 0.0.0.0 0.0.0.0 10.0.12.2 preference 80


[R3]ip route-static 10.0.12.0 24 10.0.23.2 preference 80



Step 9 Test the backup default route.

View the routes of R1 when the link between R1 and R3 is operational.
<R1>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 15           Routes : 15
Destination/Mask        Proto    Pre Cost Flags NextHop               Interface
     0.0.0.0/0             Static 60      0           RD 10.0.13.3 GigabitEthernet0/0/0
     10.0.1.0/24           Direct 0       0           D    10.0.1.1 LoopBack0
     10.0.1.1/32           Direct 0       0           D    127.0.0.1 LoopBack0
     10.0.1.255/32         Direct 0       0           D    127.0.0.1 LoopBack0
     10.0.3.0/24           Static 60      0           RD 10.0.13.3 GigabitEthernet0/0/0
     10.0.12.0/24          Direct 0       0           D    10.0.12.1 GigabitEthernet0/0/1
     10.0.12.1/32          Direct 0       0           D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct 0       0           D    127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          Direct 0       0           D    10.0.13.1 GigabitEthernet0/0/0
     10.0.13.1/32          Direct 0       0           D    127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct 0       0           D    127.0.0.1 GigabitEthernet0/0/0



                                                      HUAWEI TECHNOLOGIES                   Page63
     127.0.0.0/8           Direct 0      0            D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct 0      0            D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0



Disable Gigabit Ethernet 0/0/0 on R1 and disable interface Gigabit Ethernet 0/0/0 on
R3 to simulate a link failure, and then view the routes of R1. Compare the current
routes with the routes before Gigabit Ethernet 0/0/0 was disabled.
[R1]interface GigabitEthernet0/0/0
[R1-GigabitEthernet0/0/0]shutdown
[R1-GigabitEthernet0/0/0]quit


[R3]interface GigabitEthernet0/0/0
[R3-GigabitEthernet0/0/0]shutdown
[R3-GigabitEthernet0/0/0]quit


<R1>display ip routing-table
Route Flags: R - relay, D - download to fib
-------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 11         Routes : 11
Destination/Mask           Proto    Pre Cost Flags NextHop            Interface
     0.0.0.0/0             Static 80     0      RD 10.0.12.2 GigabitEthernet0/0/1
     10.0.1.0/24           Direct 0      0            D    10.0.1.1 LoopBack0
     10.0.1.1/32           Direct 0      0            D    127.0.0.1 LoopBack0
     10.0.1.255/32         Direct 0      0            D    127.0.0.1 LoopBack0
     10.0.12.0/24          Direct 0      0            D    10.0.12.1 GigabitEthernet0/0/1
     10.0.12.1/32          Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct 0      0            D    127.0.0.1 GigabitEthernet0/0/1
     127.0.0.0/8           Direct 0      0            D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct 0      0            D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct 0      0            D    127.0.0.1 InLoopBack0



According to the preceding routing table, the value of 80 in the Preference column
indicates that the backup default route 0.0.0.0 is actively forwarding traffic to the
next hop of 10.0.23.3.
Test network connectivity on R1.
<R1>ping 10.0.23.3



                                                      HUAWEI TECHNOLOGIES                   Page64
    PING 10.0.23.3: 56 data bytes, press CTRL_C to break
      Reply from 10.0.23.3: bytes=56 Sequence=1 ttl=254 time=76 ms
      Reply from 10.0.23.3: bytes=56 Sequence=2 ttl=254 time=250 ms
      Reply from 10.0.23.3: bytes=56 Sequence=3 ttl=254 time=76 ms
      Reply from 10.0.23.3: bytes=56 Sequence=4 ttl=254 time=76 ms
      Reply from 10.0.23.3: bytes=56 Sequence=5 ttl=254 time=76 ms
    --- 10.0.23.3 ping statistics ---
      5 packet(s) transmitted
      5 packet(s) received
      0.00% packet loss
      round-trip min/avg/max = 76/110/250 ms
<R1>tracert 10.0.23.3
traceroute to 10.0.23.3(10.0.23.2), max hops: 30 ,packet length: 40,press CTRL_C to break
 1 10.0.12.2 30 ms      26 ms 26 ms
 2 10.0.23.3 60 ms      53 ms 56 ms


The IP packets are reaching R3 (10.0.23.3) via the next hop 10.0.12.2 of R2.


Final Configuration

<R1>dis current-configuration
[V200R007C00SPC600]
#
 sysname R1
#
interface GigabitEthernet0/0/0
 shutdown
 ip address 10.0.13.1 255.255.255.0
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.1 255.255.255.0
#
interface LoopBack0
 ip address 10.0.1.1 255.255.255.0
#
ip route-static 0.0.0.0 0.0.0.0 10.0.13.3
ip route-static 0.0.0.0 0.0.0.0 10.0.12.2 preference 80
ip route-static 10.0.3.0 255.255.255.0 10.0.13.3
#
user-interface con 0
 authentication-mode password




                                                    HUAWEI TECHNOLOGIES                     Page65
 set authentication password cipher %$%$+L'YR&IZt'4,)>-*#lH",}%K-oJ_M9+'lOU~bD (\WTqB}%N,%$%$
user-interface vty 0 4
#
return
<R2>display current-configuration
[V200R007C00SPC600]
#
 sysname R2
interface GigabitEthernet0/0/1
 ip address 10.0.12.2 255.255.255.0
#
interface GigabitEthernet0/0/2
 ip address 10.0.23.2 255.255.255.0
#
interface LoopBack0
 ip address 10.0.2.2 255.255.255.0
#
ip route-static 10.0.3.0 255.255.255.0 10.0.23.3
ip route-static 10.0.3.0 255.255.255.0 10.0.12.1 preference 80
ip route-static 10.0.13.0 255.255.255.0 10.0.23.3
ip route-static 10.0.13.0 255.255.255.0 10.0.12.1 preference 80
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$1=cd%b%/O%Id-8X:by1N,+s}'4wD6TvO<I|/pd# #44C@+s#,%$%$
user-interface vty 0 4
#
return


<R3>display current-configuration
[V200R007C00SPC600]
#
 sysname R3
#
interface GigabitEthernet0/0/0
 shutdown
 ip address 10.0.13.3 255.255.255.0
#
interface GigabitEthernet0/0/2
 ip address 10.0.23.3 255.255.255.0
#
interface LoopBack0



                                                    HUAWEI TECHNOLOGIES              Page66
 ip address 10.0.3.3 255.255.255.0
#
ip route-static 10.0.12.0 255.255.255.0 10.0.13.1
ip route-static 10.0.12.0 255.255.255.0 10.0.23.2 preference 80
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$ksXDMg7Ry6yUU:63:DQ),#/sQg"@*S\U#.s.bHW xQ,y%#/v,%$%$
user-interface vty 0 4
#
return




                                                    HUAWEI TECHNOLOGIES              Page67
Lab 4-2 OSPF Single-Area Configuration


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

      Configuration of the Router-ID for OSPF.

      Establish OSPF on a specified interface or network.

      View OSPF operations using display commands.

      Advertisement of default routes in OSPF.

      Change of the OSPF hello interval and dead interval.

      Familiarization with DR or BDR election on multi-access networks.

      Change of the OSPF route priority to manipulate DR election.


Topology




                          Figure 4.2 OSPF single area topology




                                      HUAWEI TECHNOLOGIES                  Page68
Scenario

As the network administrator of an establishing small enterprise, it is required that a
network be implemented using OSPF. Then network is to support a single area and
with consideration for future expansion it is requested that this area be set as area 0.
OSPF is required to advertise default routes and also elect both a DR and BDR for
network resiliency.


Tasks


Step 1 Prepare the environment

If you are starting this section with a non-configured device, begin here and then
move to step 3. For those continuing from previous labs, begin at step 2.

Establish the basic system configuration and addressing for the lab.


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R1
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet 0/0/1]ip address 10.0.12.1 24
[R1-GigabitEthernet 0/0/1]quit
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ip address 10.0.13.1 24
[R1-GigabitEthernet0/0/0]quit
[R1]interface LoopBack 0
[R1-LoopBack0]ip address 10.0.1.1 24


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R2
[R2]interface GigabitEthernet 0/0/1
[R2-GigabitEthernet 0/0/1]ip address 10.0.12.2 24
[R2-GigabitEthernet 0/0/1]quit
[R2]interface LoopBack 0
[R2-LoopBack0]ip address 10.0.2.2 24




                                                   HUAWEI TECHNOLOGIES       Page69
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R3
[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ip address 10.0.13.3 24
[R3-GigabitEthernet0/0/0]quit
[R3]interface LoopBack 0
[R3-LoopBack0]ip address 10.0.3.3 24
[R3-LoopBack0]quit
[R3]interface LoopBack 2
[R3-LoopBack2]ip address 172.16.0.1 24



Step 2 Configure OSPF.

Assign the value 10.0.1.1 (as used on logical interface loopback 0 for simplicity) as
the router ID. Use OSPF process 1 (the default process), and specify network
segments 10.0.1.0/24, 10.0.12.0/24, and 10.0.13.0/24 as part of OSPF area 0.
[R1]ospf 1 router-id 10.0.1.1
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 10.0.1.0 0.0.0.255
[R1-ospf-1-area-0.0.0.0]network 10.0.13.0 0.0.0.255
[R1-ospf-1-area-0.0.0.0]network 10.0.12.0 0.0.0.255


Different process ID's will generate multiple link state databases, therefore ensure
that all routers use the same OSPF process ID. The wildcard mask must be specified
as part of the network command.

Manually assign the value 10.0.2.2 as the router ID. Use OSPF process 1, and
advertise network segments 10.0.12.0/24 and 10.0.2.0/24 into OSPF area 0.
[R2]ospf 1 router-id 10.0.2.2
[R2-ospf-1]area 0
[R2-ospf-1-area-0.0.0.0]network 10.0.2.0 0.0.0.255
[R2-ospf-1-area-0.0.0.0]network 10.0.12.0 0.0.0.255




…output omitted…
Mar 30 2016 09:41:39+00:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[5]:Neighbor changes event: neighbor status changed.
(ProcessId=1, NeighborAddress=10.0.12.1, NeighborEvent=LoadingDone, NeighborPreviousState=Loading,
NeighborCurrentState=Full)



                                                   HUAWEI TECHNOLOGIES                               Page70
Adjacency is attained when “NeighborCurrentState=Full”. For R3, Manually assign
the value 10.0.3.3 as the router ID. Use OSPF process 1, and advertise network
segments 10.0.3.0/24 and 10.0.13.0/24 into OSPF area 0.
[R3]ospf 1 router-id 10.0.3.3
[R3-ospf-1]area 0
[R3-ospf-1-area-0.0.0.0]network 10.0.3.0 0.0.0.255
[R3-ospf-1-area-0.0.0.0]network 10.0.13.0 0.0.0.255
…output omitted…
Mar 30 2016 16:05:34+00:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[5]:Neighbor changes event: neighbor status changed.
(ProcessId=1, NeighborAddress=10.0.13.1, NeighborEvent=LoadingDone, NeighborPreviousState=Loading,
NeighborCurrentState=Full)




Step 3 Verify the OSPF configuration.

After OSPF route convergence is complete, view routing tables of R1, R2, and R3.
<R1>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 15           Routes : 15


Destination/Mask        Proto       Pre Cost Flags    NextHop         Interface


     10.0.1.0/24           Direct      0    0         D    10.0.1.1 LoopBack0
     10.0.1.1/32           Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.1.255/32         Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.2.2/32           OSPF        10   1         D    10.0.12.2 GigabitEthernet0/0/1
     10.0.3.3/32           OSPF        10   1         D    10.0.13.3 GigabitEthernet0/0/0
     10.0.12.0/24          Direct      0    0         D    10.0.12.1 GigabitEthernet0/0/1
     10.0.12.1/32          Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          Direct      0    0         D    10.0.13.1 GigabitEthernet0/0/0
     10.0.13.1/32          Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/0
     127.0.0.0/8           Direct      0    0         D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct      0    0         D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0         D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct      0    0         D    127.0.0.1 InLoopBack0




                                                      HUAWEI TECHNOLOGIES                            Page71
<R2>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 13           Routes : 13


Destination/Mask        Proto       Pre Cost        Flags NextHop              Interface


     10.0.1.1/32           OSPF        10   1         D    10.0.12.1 GigabitEthernet0/0/1
     10.0.2.0/24           Direct      0    0         D    10.0.2.2        LoopBack0
     10.0.2.2/32           Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.2.255/32         Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.3.3/32           OSPF        10   2         D    10.0.12.1 GigabitEthernet0/0/1
     10.0.12.0/24          Direct      0    0         D    10.0.12.2 GigabitEthernet0/0/1
     10.0.12.2/32          Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          OSPF        10   2         D    10.0.12.1 GigabitEthernet0/0/1
     127.0.0.0/8           Direct      0    0         D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct      0    0         D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0         D    127.0.0.1 InLoopBack0
255.255.255.255/32         Direct      0    0         D    127.0.0.1 InLoopBack0


<R3>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 16           Routes : 16


Destination/Mask        Proto       Pre Cost Flags NextHop            Interface


     10.0.1.1/32           OSPF        10   1         D    10.0.13.1 GigabitEthernet0/0/0
     10.0.2.2/32           OSPF        10   2         D    10.0.13.1 GigabitEthernet0/0/0
     10.0.3.0/24           Direct      0    0         D    10.0.3.3 LoopBack0
     10.0.3.3/32           Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.3.255/32         Direct      0    0         D    127.0.0.1 LoopBack0
     10.0.12.0/24          OSPF        10   2         D    10.0.13.1 GigabitEthernet0/0/0
     10.0.13.0/24          Direct      0    0         D    10.0.13.3 GigabitEthernet0/0/0
     10.0.13.3/32          Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct      0    0         D    127.0.0.1 GigabitEthernet0/0/0
     127.0.0.0/8           Direct      0    0         D    127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct      0    0         D    127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0         D    127.0.0.1 InLoopBack0



                                                      HUAWEI TECHNOLOGIES                   Page72
     172.16.0.0/24         Direct   0   0          D   172.16.0.1   LoopBack2
     172.16.0.1/32         Direct   0   0          D   127.0.0.1 LoopBack2
     172.16.0.255/32       Direct   0   0          D   127.0.0.1 LoopBack2
255.255.255.255/32         Direct   0   0          D   127.0.0.1 InLoopBack0



Test network connectivity between R2 and R1 at 10.0.1.1 and between R2 and R3 at
10.0.3.3.
<R2>ping 10.0.1.1
  PING 10.0.1.1: 56 data bytes, press CTRL_C to break
    Reply from 10.0.1.1: bytes=56 Sequence=1 ttl=255 time=37 ms
    Reply from 10.0.1.1: bytes=56 Sequence=2 ttl=255 time=42 ms
    Reply from 10.0.1.1: bytes=56 Sequence=3 ttl=255 time=42 ms
    Reply from 10.0.1.1: bytes=56 Sequence=4 ttl=255 time=45 ms
    Reply from 10.0.1.1: bytes=56 Sequence=5 ttl=255 time=42 ms


--- 10.0.1.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 37/41/45 ms


<R2>ping 10.0.3.3
  PING 10.0.3.3: 56 data bytes, press CTRL_C to break
    Reply from 10.0.3.3: bytes=56 Sequence=1 ttl=254 time=37 ms
    Reply from 10.0.3.3: bytes=56 Sequence=2 ttl=254 time=42 ms
    Reply from 10.0.3.3: bytes=56 Sequence=3 ttl=254 time=42 ms
    Reply from 10.0.3.3: bytes=56 Sequence=4 ttl=254 time=42 ms
    Reply from 10.0.3.3: bytes=56 Sequence=5 ttl=254 time=42 ms


 --- 10.0.3.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 37/41/42 ms




Run the display ospf peer command to view the OSPF neighbor status.
<R1>display ospf peer


          OSPF Process 1 with Router ID 10.0.1.1



                                                   HUAWEI TECHNOLOGIES          Page73
                   Neighbors


 Area 0.0.0.0 interface 10.0.12.1(GigabitEthernet0/0/1)'s neighbors
 Router ID: 10.0.2.2            Address: 10.0.12.2
   State: Full Mode:Nbr is Master         Priority: 1
   DR: 10.0.12.1 BDR: 10.0.12.2       MTU: 0
   Dead timer due in 32 sec
   Retrans timer interval: 5
   Neighbor is up for 00:47:59
   Authentication Sequence: [ 0 ]


                   Neighbors


 Area 0.0.0.0 interface 10.0.13.1(GigabitEthernet0/0/0)'s neighbors
 Router ID: 10.0.3.3            Address: 10.0.13.3
   State: Full Mode:Nbr is Master         Priority: 1
   DR: 10.0.13.1 BDR: 10.0.13.3       MTU: 0
   Dead timer due in 34 sec
   Retrans timer interval: 5
   Neighbor is up for 00:41:44
   Authentication Sequence: [ 0 ]


The display ospf peer command displays detailed information about any peering
neighbors. In the example given, the link 10.0.13.1 of R1 shows to be the DR. The DR
election is non pre-emptive, meaning that the link of R3 will not take over the role of
DR from R1 unless the OSPF process is reset.

The display ospf peer brief command can also be used to display a condensed
version of the OSPF peer information.
<R1>display ospf peer brief


           OSPF Process 1 with Router ID 10.0.1.1
                    Peer Statistic Information
 ----------------------------------------------------------------------------
 Area Id            Interface                             Neighbor id            State
 0.0.0.0           GigabitEthernet0/0/0                   10.0.3.3              Full
 0.0.0.0           GigabitEthernet0/0/1                   10.0.2.2              Full
----------------------------------------------------------------------------
<R2>display ospf peer brief


           OSPF Process 1 with Router ID 10.0.2.2
                    Peer Statistic Information



                                                        HUAWEI TECHNOLOGIES              Page74
 ----------------------------------------------------------------------------
 Area Id            Interface                            Neighbor id             State
 0.0.0.0          GigabitEthernet0/0/1                    10.0.1.1              Full
 ----------------------------------------------------------------------------


<R3>display ospf peer brief
           OSPF Process 1 with Router ID 10.0.3.3
                    Peer Statistic Information
 ----------------------------------------------------------------------------
 Area Id            Interface                            Neighbor id             State
 0.0.0.0           GigabitEthernet0/0/0                   10.0.1.1              Full
 ----------------------------------------------------------------------------

Step 4 Change the OSPF hello interval and dead interval.

Run the display ospf interface GigabitEthernet 0/0/0 command on R1 to view the
default OSPF hello interval and dead interval.
<R1>display ospf interface GigabitEthernet 0/0/0


           OSPF Process 1 with Router ID 10.0.1.1
                   Interfaces




 Interface: 10.0.13.1 (GigabitEthernet0/0/0)
 Cost: 1        State: DR           Type: Broadcast       MTU: 1500
 Priority: 1
 Designated Router: 10.0.13.1
 Backup Designated Router: 10.0.13.3
 Timers: Hello 10 , Dead 40 , Poll 120 , Retransmit 5 , Transmit Delay 1


Run the ospf timer command to change the OSPF hello interval and dead interval
on GE0/0/0 of R1 to 15s and 60s respectively.
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ospf timer hello 15
[R1-GigabitEthernet0/0/0]ospf timer dead 60
Mar 30 2016 16:58:39+00:00 R1 %%01OSPF/3/NBR_DOWN_REASON(l)[1]:Neighbor state leaves full or changed
to Down. (ProcessId=1, NeighborRouterId=10.0.3.3, NeighborAreaId=0,
NeighborInterface=GigabitEthernet0/0/0,NeighborDownImmediate reason=Neighbor Down Due to Inactivity,
NeighborDownPrimeReason=Interface Parameter Mismatch, NeighborChangeTime=2016-03-30 16:58:39)


<R1>display ospf interface GigabitEthernet 0/0/0



                                                      HUAWEI TECHNOLOGIES                Page75
           OSPF Process 1 with Router ID 10.0.1.1
                   Interfaces




 Interface: 10.0.13.1 (GigabitEthernet0/0/0)
 Cost: 1        State: DR           Type: Broadcast       MTU: 1500
 Priority: 1
 Designated Router: 10.0.13.1
 Backup Designated Router: 10.0.13.3
 Timers: Hello 15 , Dead 60 , Poll 120 , Retransmit 5 , Transmit Delay 1


Check the OSPF neighbor status on R1.
<R1>display ospf peer brief


           OSPF Process 1 with Router ID 10.0.1.1
                    Peer Statistic Information
 ----------------------------------------------------------------------------
 Area Id            Interface                            Neighbor id             State
 0.0.0.0          GigabitEthernet0/0/1                    10.0.2.2              Full
 ----------------------------------------------------------------------------


The preceding information shows that R1 has only one neighbor, R2. Since the OSPF
hello intervals and dead intervals on R1 and R3 are different, R1 and R3 will fail to
establish an OSPF neighbor relationship.
Run the ospf timer command to change the OSPF hello interval and dead interval
on GE0/0/0 of R3 to 15s and 60s respectively.
[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ospf timer hello 15
[R3-GigabitEthernet0/0/0]ospf timer dead 60
…output omitted…
Mar 30 2016 17:03:33+00:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[4]:Neighbor changes event: neighbor status
changed. (ProcessId=1, NeighborAddress=10.0.13.1, NeighborEvent=LoadingDone,
NeighborPreviousState=Loading, NeighborCurrentState=Full)


<R3>display ospf interface GigabitEthernet 0/0/0


           OSPF Process 1 with Router ID 10.0.3.3
                   Interfaces




                                                      HUAWEI TECHNOLOGIES                Page76
 Interface: 10.0.13.3 (GigabitEthernet0/0/0)
 Cost: 1         State: DR             Type: Broadcast       MTU: 1500
 Priority: 1
 Designated Router: 10.0.13.3
 Backup Designated Router: 10.0.13.1
 Timers: Hello 15 , Dead 60 , Poll 120 , Retransmit 5 , Transmit Delay 1



Check the OSPF neighbor status on R1 again.
<R1>display ospf peer brief
           OSPF Process 1 with Router ID 10.0.1.1
                     Peer Statistic Information
 ----------------------------------------------------------------------------
 Area Id             Interface                               Neighbor id         State
 0.0.0.0           GigabitEthernet0/0/0                      10.0.3.3           Full
 0.0.0.0           GigabitEthernet0/0/1                      10.0.2.2           Full
----------------------------------------------------------------------------



Step 5 Advertise default routes in OSPF.

Configure OSPF to advertise default routes on R3.
[R3]ip route-static 0.0.0.0 0.0.0.0 LoopBack 2
[R3]ospf 1
[R3-ospf-1]default-route-advertise



View routing tables of R1 and R2. You can see that R1 and R2 have learned the
default routes advertised by R3.
<R1>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
           Destinations : 16            Routes : 16


Destination/Mask        Proto         Pre Cost Flags     NextHop        Interface


     0.0.0.0/0               O_ASE       150 1           D    10.0.13.3 GigabitEthernet0/0/0
     10.0.1.0/24             Direct      0    0          D    10.0.1.1 LoopBack0
     10.0.1.1/32             Direct      0    0          D    127.0.0.1 LoopBack0
     10.0.1.255/32           Direct      0    0          D    127.0.0.1 LoopBack0
     10.0.2.2/32             OSPF        10   1          D    10.0.12.2 GigabitEthernet0/0/1


                                                         HUAWEI TECHNOLOGIES                   Page77
     10.0.3.3/32           OSPF        10   1           D   10.0.13.3 GigabitEthernet0/0/0
     10.0.12.0/24          Direct      0    0           D   10.0.12.1 GigabitEthernet0/0/1
     10.0.12.1/32          Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          Direct      0    0           D   10.0.13.1 GigabitEthernet0/0/0
     10.0.13.1/32          Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/0
     127.0.0.0/8           Direct      0    0           D   127.0.0.1 InLoopBack0
     127.0.0.1/32               Direct      0       0       D     127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0           D   127.0.0.1 InLoopBack0
255.255.255.255/32         Direct      0    0           D   127.0.0.1 InLoopBack0




<R2>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------
Routing Tables: Public
          Destinations : 14           Routes : 14


Destination/Mask        Proto       Pre Cost Flags      NextHop       Interface


     0.0.0.0/0             O_ASE       150 1            D   10.0.12.1 GigabitEthernet0/0/1
     10.0.1.1/32           OSPF1       0    1           D   10.0.12.1 GigabitEthernet0/0/1
     10.0.2.0/24           Direct      0    0           D   10.0.2.2 LoopBack0
     10.0.2.2/32           Direct      0    0           D   127.0.0.1 LoopBack0
     10.0.2.255/32         Direct      0    0           D   127.0.0.1 LoopBack0
     10.0.3.3/32           OSPF        10   2           D   10.0.12.1 GigabitEthernet0/0/1
     10.0.12.0/24          Direct      0    0           D   10.0.12.2 GigabitEthernet0/0/1
     10.0.12.2/32          Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/1
     10.0.12.255/32        Direct      0    0           D   127.0.0.1 GigabitEthernet0/0/1
     10.0.13.0/24          OSPF        10   2           D   10.0.12.1 GigabitEthernet0/0/1
     127.0.0.0/8           Direct      0    0           D   127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct      0    0           D   127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0           D   127.0.0.1 InLoopBack0
255.255.255.255/32         Direct      0    0           D   127.0.0.1 InLoopBack0




<R3>display ip routing-table
Route Flags: R - relay, D - download to fib
----------------------------------------------------------------------------



                                                        HUAWEI TECHNOLOGIES                  Page78
Routing Tables: Public
          Destinations : 17           Routes : 17


Destination/Mask       Proto        Pre Cost Flags   NextHop        Interface


     0.0.0.0/0             Static      60   0        D   172.16.0.1     LoopBack2
     10.0.1.1/32           OSPF        10   1        D   10.0.13.1 GigabitEthernet0/0/0
     10.0.2.2/32           OSPF        10   2        D   10.0.13.1 GigabitEthernet0/0/0
     10.0.3.0/24           Direct      0    0        D   10.0.3.3       LoopBack0
     10.0.3.3/32           Direct      0    0        D   127.0.0.1 LoopBack0
     10.0.3.255/32         Direct      0    0        D   127.0.0.1 LoopBack0
     10.0.12.0/24          OSPF        10   2        D   10.0.13.1 GigabitEthernet0/0/0
     10.0.13.0/24          Direct      0    0        D   10.0.13.3 GigabitEthernet0/0/0
     10.0.13.3/32          Direct      0    0        D   127.0.0.1 GigabitEthernet0/0/0
     10.0.13.255/32        Direct      0    0        D   127.0.0.1 GigabitEthernet0/0/0
     127.0.0.0/8           Direct      0    0        D   127.0.0.1 InLoopBack0
     127.0.0.1/32          Direct      0    0        D   127.0.0.1 InLoopBack0
127.255.255.255/32         Direct      0    0        D   127.0.0.1 InLoopBack0
     172.16.0.0/24         Direct      0    0        D   172.16.0.1     LoopBack2
     172.16.0.1/32         Direct      0    0        D   127.0.0.1 LoopBack2
     172.16.0.255/32       Direct      0    0        D   127.0.0.1 LoopBack2
255.255.255.255/32         Direct      0    0        D   127.0.0.1 InLoopBack0


Run the ping command to test connectivity between R2 and Loopback2 at
172.16.0.1.
<R2>ping 172.16.0.1
  PING 172.16.0.1: 56 data bytes, press CTRL_C to break
    Reply from 172.16.0.1: bytes=56 Sequence=1 ttl=254 time=47 ms
    Reply from 172.16.0.1: bytes=56 Sequence=2 ttl=254 time=37 ms
    Reply from 172.16.0.1: bytes=56 Sequence=3 ttl=254 time=37 ms
    Reply from 172.16.0.1: bytes=56 Sequence=4 ttl=254 time=37 ms
    Reply from 172.16.0.1: bytes=56 Sequence=5 ttl=254 time=37 ms


 --- 172.16.0.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 37/39/47 ms




                                                     HUAWEI TECHNOLOGIES                  Page79
Step 6 Control OSPF DR or BDR election.

Run the display ospf peer command to view the DR and BDR of R1 and R3.
<R1>display ospf peer 10.0.3.3


         OSPF Process 1 with Router ID 10.0.1.1
                  Neighbors


 Area 0.0.0.0 interface 10.0.13.1(GigabitEthernet0/0/0)'s neighbors
 Router ID: 10.0.3.3           Address: 10.0.13.3
   State: Full Mode:Nbr is Master       Priority: 1
   DR: 10.0.13.3 BDR: 10.0.13.1 MTU: 0
   Dead timer due in 49 sec
   Retrans timer interval: 5
   Neighbor is up for 00:17:40
   Authentication Sequence: [ 0 ]


The preceding information shows that R3 is the DR and R1 is the BDR. This is
because R3's router ID 10.0.3.3 is greater than R1's router ID 10.0.1.1. R1 and R3 use
the default priority of 1, so their router IDs are used for DR or BDR election.
Run the ospf dr-priority command to change DR priorities of R1 and R3.
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ospf dr-priority 200


[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ospf dr-priority 100


A DR or BDR is elected in non-preemption mode, by default. After router priorities
are changed, a DR is not re-elected, so you must reset the OSPF neighbor
relationship between R1 and R3.
Shut down and re-enable Gigabit Ethernet 0/0/0 interfaces on R1 and R3 to reset the
OSPF neighbor relationship between R1 and R3.
[R3]interface GigabitEthernet0/0/0
[R3-GigabitEthernet0/0/0]shutdown


[R1]interface GigabitEthernet0/0/0
[R1-GigabitEthernet0/0/0]shutdown




                                                      HUAWEI TECHNOLOGIES   Page80
[R1-GigabitEthernet0/0/0]undo shutdown


[R3-GigabitEthernet0/0/0]undo shutdown


Run the display ospf peer command to view the DR and BDR of R1 and R3.
[R1]display ospf peer 10.0.3.3


          OSPF Process 1 with Router ID 10.0.1.1
                   Neighbors


 Area 0.0.0.0 interface 10.0.13.1(GigabitEthernet0/0/0)'s neighbors
 Router ID: 10.0.3.3            Address: 10.0.13.3
    State: Full Mode:Nbr is Master       Priority: 100
    DR: 10.0.13.1 BDR: 10.0.13.3 MTU: 0
    Dead timer due in 52 sec
    Retrans timer interval: 5
    Neighbor is up for 00:00:25
    Authentication Sequence: [ 0 ]


According to the preceding information, R1's priority is higher than R3's priority, so
R1 becomes DR and R3 becomes the BDR.


Final Configuration

<R1>display current-configuration
[V200R007C00SPC600]
#
 sysname R1
#
interface GigabitEthernet0/0/0
 ip address 10.0.13.1 255.255.255.0
 ospf dr-priority 200
 ospf timer hello 15
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.1 255.255.255.0
#
interface LoopBack0
 ip address 10.0.1.1 255.255.255.0
#
ospf 1 router-id 10.0.1.1



                                                     HUAWEI TECHNOLOGIES    Page81
 area 0.0.0.0
    network 10.0.1.0 0.0.0.255
    network 10.0.12.0 0.0.0.255
    network 10.0.13.0 0.0.0.255
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$+L'YR&IZt'4,)>-*#lH",}%K-oJ_M9+'lOU~bD (\WTqB}%N,%$%$
user-interface vty 0 4
#
return




<R2>display current-configuration
[V200R007C00SPC600]
#
 sysname R2
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.2 255.255.255.0
#
interface LoopBack0
 ip address 10.0.2.2 255.255.255.0
#
ospf 1 router-id 10.0.2.2
 area 0.0.0.0
    network 10.0.2.0 0.0.0.255
    network 10.0.12.0 0.0.0.255
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$1=cd%b%/O%Id-8X:by1N,+s}'4wD6TvO<I|/pd# #44C@+s#,%$%$
user-interface vty 0 4
#
return




<R3>display current-configuration
[V200R007C00SPC600]



                                          HUAWEI TECHNOLOGIES                        Page82
#
 sysname R3
#
interface GigabitEthernet0/0/0
 ip address 10.0.13.3 255.255.255.0
 ospf dr-priority 100
 ospf timer hello 15
#
interface LoopBack0
 ip address 10.0.3.3 255.255.255.0
#
interface LoopBack2
 ip address 172.16.0.1 255.255.255.0
#
ospf 1 router-id 10.0.3.3
 default-route-advertise
 area 0.0.0.0
    network 10.0.3.0 0.0.0.255
    network 10.0.13.0 0.0.0.255
#
ip route-static 0.0.0.0 0.0.0.0 LoopBack2
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$ksXDMg7Ry6yUU:63:DQ),#/sQg"@*S\U#.s.bHW xQ,y%#/v,%$%$
user-interface vty 0 4
#
return




                                            HUAWEI TECHNOLOGIES                    Page83
                         Module 5 FTP and DHCP

Lab 5-1 Configuring FTP Services


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

      Establishment of the FTP service.

      Configuration of FTP server parameters.

      Successful file transfer from an FTP server.


Topology




                                Figure 5.1 FTP topology



Scenario

As a network administrator of a company, you have been tasked with implementing
FTP services on the network. You need to implement the FTP service on a router
assigned to be an FTP server. The router should allow clients to successfully establish
a TCP session to the FTP application and transfer files.




                                      HUAWEI TECHNOLOGIES                   Page84
Tasks


Step 1 Preparing the environment.

If you are starting this section with a non-configured device, begin here and then
move to step 2. For those continuing from previous labs, begin at step 2.


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R1
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]ip address 10.0.12.1 24


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R2
[R2]interface GigabitEthernet 0/0/1
[R2-GigabitEthernet0/0/1]ip address 10.0.12.2 24


Verify that R1 can reach R2, and vice versa..
 [R1]ping 10.0.12.2
  PING 10.0.12.2: 56 data bytes, press CTRL_C to break
    Reply from 10.0.12.2: bytes=56 Sequence=1 ttl=255 time=10 ms
    Reply from 10.0.12.2: bytes=56 Sequence=2 ttl=255 time=1 ms
    Reply from 10.0.12.2: bytes=56 Sequence=3 ttl=255 time=1 ms
    Reply from 10.0.12.2: bytes=56 Sequence=4 ttl=255 time=10 ms
    Reply from 10.0.12.2: bytes=56 Sequence=5 ttl=255 time=1 ms


  --- 10.0.12.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 1/4/10 ms



Step 2 Enable the FTP service on the router.

The FTP service is disabled by default on the router. It must be enabled before FTP
can be used. Configure an FTP server using R1 with R2 as the client. The same steps


                                                   HUAWEI TECHNOLOGIES    Page85
can be reversed to enable R2 to also act as an FTP server.
[R1]ftp server enable
Info: Succeeded in starting the FTP server
[R1]set default ftp-directory flash:/


Configure user authorization for FTP users to access the server. Unauthorized users
will not be able to access the FTP server, reducing security risks.
[R1]aaa
[R1-aaa]local-user huawei password cipher         huawei123
Info: Add a new user.

[R1-aaa]local-user huawei service-type ftp

Info: The cipher password has been changed to an irreversible-cipher password.

Warning: The user access modes include Telnet, FTP or HTTP, and so security risks exist.

Info: After you change the rights (including the password, access type, FTP directory, and level) of a local user, the
rights of users already online do not change. The change takes effect to users who go online after the change.

[R1-aaa]local-user huawei privilege level 15

Info: After you change the rights (including the password, access type, FTP directory, and level) of a local user, the
rights of users already online do not change. The change takes effect to users who go online after the change.

[R1-aaa]local-user huawei ftp-directory flash:

Info: After you change the rights (including the password, access type, FTP directory, and level) of a local user, the
rights of users already online do not change. The change takes effect to users who go online after the change.




[R1]display ftp-server

   FTP server is running

   Max user number                           5

   User count                            0

   Timeout value(in minute)             30

   Listening port                       21

   Acl number                             0

   FTP server's source address          0.0.0.0



The FTP server is running on R1 and listens on TCP port 21 by default.


                                                    HUAWEI TECHNOLOGIES                                Page86
Step 3 Establish an FTP client connection

Establish a connection to the FTP Server from R2.
<R2>ftp 10.0.12.1
Trying 10.0.12.1 ...
Press CTRL+K to abort
Connected to 10.0.12.1.
220 FTP service ready.
User(10.0.12.1:(none)):huawei
331 Password required for huawei.
Enter password:
230 User logged in.


[R2-ftp]


Following entry of the correct user name and password, the FTP server can be
successfully logged into.

Run the dir command before downloading a file or after uploading a file to view the
detailed information of the file.
[R2-ftp]dir
200 Port command okay.
150 Opening ASCII mode data connection for *.
drwxrwxrwx      1 noone    nogroup            0 May 03 18:03 .
-rwxrwxrwx      1 noone    nogroup 114552448 Jan 19 2012 AR2220E-V200R006C10SPC300.cc
-rwxrwxrwx      1 noone    nogroup      159858 May 03 17:59 mon_file.txt
-rwxrwxrwx      1 noone    nogroup      304700 Mar 03 11:11 sacrule.dat
-rwxrwxrwx      1 noone    nogroup         783 Mar 03 11:12 default_local.cer
-rwxrwxrwx      1 noone    nogroup           0 Dec 20 2015 brdxpon_snmp_cfg.efs
-rwxrwxrwx      1 noone    nogroup         777 May 03 18:03 vrpcfg.zip
drwxrwxrwx      1 noone    nogroup            0 Mar 10 11:14 update
drwxrwxrwx      1 noone    nogroup            0 May 03 18:03 localuser
drwxrwxrwx      1 noone    nogroup            0 Mar 17 10:45 dhcp
-rwxrwxrwx      1 noone    nogroup         460 May 03 18:03 private-data.txt
-rwxrwxrwx      1 noone    nogroup 126352896 Mar 10 11:09 AR2220E-V200R007C00SPC600.cc
drwxrwxrwx      1 noone    nogroup            0 Mar 10 11:15 shelldir
-rwxrwxrwx      1 noone    nogroup       11606 May 03 18:00 mon_lpu_file.txt
drwxrwxrwx      1 noone    nogroup            0 Mar 18 14:45 huawei
-rwxrwxrwx      1 noone    nogroup         120 Mar 18 15:02 text.txt226 Transfer complete.
FTP: 836 byte(s) received in 0.976 second(s) 856.55byte(s)/sec.




                                                  HUAWEI TECHNOLOGIES                        Page87
Set the transfer mode for the files to be transferred.


[R2-ftp]binary
200 Type set to I.



Retrieve a file from the FTP server. Note: If the vrpcfg.zip file is not present in the sd1:

directory of R1, use the save command on R1 to create it.


[R2-ftp]get vrpcfg.zip vrpnew.zip
200 Port command okay.
150 Opening BINARY mode data connection for vrpcfg.zip.
226 Transfer complete.
FTP: 120 byte(s) received in 0.678 second(s) 176.99byte(s)/sec.



After downloading the file from FTP server, use the bye command to close the

connection


[R2-ftp]bye
221 Server closing.


<R2>dir
Directory of flash:/


  Idx Attr       Size(Byte) Date          Time(LMT) FileName
    0 -rw-       114,552,448 Jan 19 2012 15:32:52     AR2220E-V200R006C10SPC300.cc
    1 -rw-             270,176 Apr 30 2016 03:17:08    mon_file.txt
    2 -rw-             304,700 Mar 03 2016 11:11:44    sacrule.dat
    3 -rw-                783 Mar 03 2016 11:12:22     default_local.cer
    4 -rw-                  0 Dec 20 2015 00:06:14     brdxpon_snmp_cfg.efs
    5 -rw-                775 Apr 29 2016 17:51:48     vrpcfg.zip
    6 drw-                   - Mar 10 2016 11:28:46    update
    7 drw-                   - Apr 23 2016 17:33:38   localuser
    8 drw-                   - Mar 21 2016 20:59:46    dhcp
    9 -rw-                394 Apr 29 2016 17:51:50     private-data.txt
   10 -rw-       126,352,896 Mar 10 2016 11:14:40      AR2220E-V200R007C00SPC600.cc
   11 drw-                   - Mar 10 2016 11:29:20    shelldir
   12 -rw-              23,950 Apr 27 2016 16:06:06    mon_lpu_file.txt



                                                  HUAWEI TECHNOLOGIES                 Page88
   13 -rw-                120 Mar 24 2016 11:45:44      huawei.zip
   14 -rw-                777 May 10 2016 14:23:43      vrpnew.zip
A file can be uploaded to the FTP server by using the command put, for which a new
file name can also be assigned.
[R2-ftp]put vrpnew.zip vrpnew2.zip
200 Port command okay.
150 Opening BINARY mode data connection for vrpnew2.zip.
226 Transfer complete.
FTP: 120 byte(s) sent in 0.443 second(s) 270.88byte(s)/sec.


After uploading the file, check for the presence of the file on FTP server.
<R1>dir
Directory of flash:/


  Idx Attr       Size(Byte) Date            Time(LMT) FileName
    0 -rw-             286,620 Mar 14 2016 09:22:20    sacrule.dat
    1 -rw-             512,000 Mar 28 2016 14:39:16    mon_file.txt
    2 -rw-         1,738,816 Mar 17 2016 12:05:36      web.zip
    3 -rw-             48,128 Mar 10 2016 14:16:56     ar2220E_v200r001sph001.pat
    4 -rw-                120 Mar 28 2016 10:09:50     iascfg.zip
    5 -rw-                699 Mar 28 2016 17:52:38     vrpcfg.zip
    6 -rw-        93,871,872 Mar 14 2016 09:13:26      ar2220-V200R007C00SPC600.cc
    7 -rw-             512,000 Mar 28 2016 14:40:20    mon_lpu_file.txt
    8 -rw-                699 Mar 02 2016 15:44:16     vrpnew2.zip



Remove the created vrpnew.zip and vrpnew2.zip files on R1 and R2.
<R1>delete flash:/vrpnew2.zip
Delete flash:/vrpnew2.zip? (y/n)[n]:y
Info: Deleting file flash:/vrpnew2.zip...succeed.


<R2>delete flash:/vrpnew.zip
Delete flash:/vrpnew.zip? (y/n)[n]:y
Info: Deleting file flash:/vrpnew.zip...succeed.


Note: Please take extreme care when deleting the configuration files so to ensure
that the entire flash:/ directory of R1 and R2 is not erased.




                                                    HUAWEI TECHNOLOGIES              Page89
Final Configuration

<R1>display current-configuration
[V200R007C00SPC600]
#
 sysname R1
 ftp server enable
 set default ftp-directory flash:
#
aaa
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default
 domain default_admin
 local-user admin password cipher %$%$=i~>Xp&aY+*2cEVcS-A23Uwe%$%$
 local-user admin service-type http
 local-user huawei password cipher %$%$f+~&ZkCn]NUX7m.t;tF9R48s%$%$
 local-user huawei privilege level 15
 local-user huawei ftp-directory flash:/
 local-user huawei service-type ftp
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.1 255.255.255.0
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$+L'YR&IZt'4,)>-*#lH",}%K-oJ_M9+'lOU~bD (\WTqB}%N,%$%$
user-interface vty 0 4
#
return


<R2>display current-configuration
[V200R007C00SPC600]
#
 sysname R2
 ftp server enable
 set default ftp-directory flash:
#
aaa
 authentication-scheme default



                                           HUAWEI TECHNOLOGIES                       Page90
 authorization-scheme default
 accounting-scheme default
 domain default
 domain default_admin
 local-user admin password cipher %$%$=i~>Xp&aY+*2cEVcS-A23Uwe%$%$
 local-user admin service-type http
 local-user huawei password cipher %$%$<;qM3D/O;ZLqy/"&6wEESdg$%$%$
 local-user huawei privilege level 15
 local-user huawei ftp-directory flash:/
 local-user huawei service-type ftp
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.2 255.255.255.0
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$1=cd%b%/O%Id-8X:by1N,+s}'4wD6TvO<I|/pd# #44C@+s#,%$%$
user-interface vty 0 4
#
return




                                           HUAWEI TECHNOLOGIES                    Page91
Lab 5-2 Implementing DHCP


Learning Objectives

As a result of this lab section, you should achieve the following tasks:

      Configuration of a global DHCP pool.

      Configuration of an interface based DHCP pool.

      Enable DHCP discovery and IP allocation for switch interfaces.

      Method of global address pool configuration.

      Method of interface address pool configuration.


Topology




                               Figure 5.2 DHCP topology



Scenario

As the administrator of an enterprise you have been tasked with implementing
DHCP application services within the network. The gateway router in the company
network is to be configured as a DHCP server. IP addressing from an address pool
are to be offered by the gateway(s) (R1 and R3) to respective access layer devices.



                                      HUAWEI TECHNOLOGIES                  Page92
Tasks


Step 1 Preparing the environment

If you are starting this section with a non-configured device, begin here and then
move to step 3. For those continuing from previous labs, begin at step 2.
Establish the addressing for the lab and temporarily shut down the interfaces Gigabit
Ethernet 0/0/2 of R1 and GigabitGigabitEthernet 0/0/1 of R3.
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R1
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]ip address 10.0.12.1 24
[R1-GigabitEthernet0/0/1]quit


<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R3
[R3]interface GigabitEthernet 0/0/1
[R3-GigabitEthernet0/0/1]ip address 10.0.12.3 24
[R3-GigabitEthernet0/0/1]shutdown
[R3-GigabitEthernet0/0/1]quit
[R3]interface GigabitEthernet 0/0/2
[R3-GigabitEthernet0/0/2]ip address 10.0.23.3 24



<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S1



<Quidway>system-view
Enter system view, return user view with Ctrl+Z.
[Quidway]sysname S2




                                                   HUAWEI TECHNOLOGIES    Page93
Step 2 Cleaning up the previous configuration

Re-enable to Gigabit Ethernet 0/0/2 interface on R3.


[R3]interface GigabitEthernet 0/0/2
[R3-GigabitEthernet0/0/2]undo shutdown



Step 3 Additional configuration

Disable the port interfaces between S1 and S2 as well as other interfaces to prevent

interference from other devices.



[S1]interface GigabitEthernet 0/0/9
[S1-GigabitEthernet0/0/9]shutdown
[S1-GigabitEthernet0/0/9]quit
[S1]interface GigabitEthernet 0/0/10
[S1-GigabitEthernet0/0/10]shutdown
[S1-GigabitEthernet0/0/10]quit
[S1]interface GigabitEthernet 0/0/13
[S1-GigabitEthernet0/0/13]shutdown
[S1-GigabitEthernet0/0/13]quit
[S1]interface GigabitEthernet 0/0/14
[S1-GigabitEthernet0/0/14]shutdown


[S2]interface GigabitEthernet 0/0/9
[S2-GigabitEthernet0/0/9]shutdown
[S2-GigabitEthernet0/0/9]quit
[S2]interface GigabitEthernet 0/0/10
[S2-GigabitEthernet0/0/10]shutdown
[S2-GigabitEthernet0/0/10]quit
[S2]interface GigabitEthernet 0/0/7
[S2-GigabitEthernet0/0/7]shutdown
[S2-GigabitEthernet0/0/23]quit
[S2]interface GigabitEthernet 0/0/6
[S2-GigabitEthernet0/0/6]shutdown


[R1]interface GigabitEthernet 0/0/2



                                         HUAWEI TECHNOLOGIES              Page94
[R1-GigabitEthernet0/0/2]ip address 10.0.23.1 24
[R1-GigabitEthernet0/0/2]shutdown




Verify that Gigabit Ethernet interfaces 0/0/9, 0/0/10, 0/0/13 and 0/0/14, have been

shut down on S1 and that Gigabit Ethernet interfaces 0/09, 0/0/10, 0/0/6 and 0/0/7

have been shut down on S2.


<S1>display interface brief
…output omitted…
Interface               PHY     Protocol InUti OutUti    inErrors   outErrors
GigabitEthernet0/0/1    up         up       0.01%       0.01%         0         0
GigabitEthernet0/0/2    up         up       0.01%       0.01%         0         0
GigabitEthernet0/0/3    down       down        0%         0%          0         0
GigabitEthernet0/0/4    up         up          0%       0.01%         0         0
GigabitEthernet0/0/5    up         up          0%       0.01%         0         0
GigabitEthernet0/0/6    down       down        0%         0%          0         0
GigabitEthernet0/0/7    down       down        0%         0%          0         0
GigabitEthernet0/0/8    down       down        0%         0%          0         0
GigabitEthernet0/0/9    *down      down        0%         0%          0         0
GigabitEthernet0/0/10 *down        down        0%         0%          0         0
GigabitEthernet0/0/11 down         down        0%         0%          0         0
GigabitEthernet0/0/12 down         down        0%         0%          0         0
GigabitEthernet0/0/13 *down        down        0%         0%          0         0
GigabitEthernet0/0/14 *down        down        0%         0%          0         0
…output omitted…


<S2>display interface brief
…output omit…
GigabitEthernet0/0/1          up    up              0% 4.06%              0         0
GigabitEthernet0/0/2          up    up              0% 4.06%              0         0
GigabitEthernet0/0/3          up    up              0% 4.06%              0         0
GigabitEthernet0/0/4          up    up              0% 20.40%             0         0
GigabitEthernet0/0/5          up    up              0% 20.40%             0         0
GigabitEthernet0/0/6          *down down            0% 2.04%              0         0
GigabitEthernet0/0/7          *down down        2.03% 2.03%               0         0



                                                   HUAWEI TECHNOLOGIES                  Page95
GigabitEthernet0/0/8           down down                0%       0%            0       0
GigabitEthernet0/0/9           *down down             1.91% 1.91%          0       0
GigabitEthernet0/0/10          *down down             3.95% 0.12%          0       0
GigabitEthernet0/0/11          up      up              0% 4.06%            0       0
GigabitEthernet0/0/12          up      up              0% 4.06%            0       0
…output omit…




Verify that only interface Gigabit Ethernet 0/0/2 is disabled on R1 and that only

interface GigabitGigabitEthernet 0/0/1 is disabled on R3.


<R1>display ip interface brief
…output omitted…
GigabitEthernet0/0/1                10.0.12.1/24         up           up
GigabitEthernet0/0/2                10.0.23.1/24         *down        down
…output omitted…


<R3>display ip interface brief
…output omitted…
GigabitEthernet0/0/1                10.0.12.3/24         *down        down
GigabitEthernet0/0/2                10.0.23.3/24         up           up
…output omitted…



Step 4 Enable the DHCP function.

The DHCP service is not enabled by default, enable the DHCP service on the
router(s).
[R1]dhcp enable


[R3]dhcp enable

Step 5 Create a global IP address pool

Create an address pool named pool1 for R1 and pool2 for R3. Configure attributes
for pool1 and pool2, including address range, egress gateway, and IP address lease
period.
[R1]ip pool pool1
Info: It's successful to create an IP address pool.
[R1-ip-pool-pool1]network 10.0.12.0 mask 24


                                                      HUAWEI TECHNOLOGIES                  Page96
[R1-ip-pool-pool1]gateway-list 10.0.12.1
[R1-ip-pool-pool1]lease day 1 hour 12
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]dhcp select global


[R3]ip pool pool2
Info: It's successful to create an IP address pool.
[R3-ip-pool-pool2]network 10.0.23.0 mask 24
[R3-ip-pool-pool2]gateway-list 10.0.23.3
[R3-ip-pool-pool2]lease day 1 hour 12
[R3]interface GigabitEthernet 0/0/2
[R3-GigabitEthernet0/0/2]dhcp select global


Run the display ip pool name <name> command on the router to view the
assigned IP address pool configuration parameters.
<R1>display ip pool name pool1
  Pool-name          : pool1
  Pool-No            :0
  Lease              : 1 Days 12 Hours 0 Minutes
  Domain-name        :-
  DNS-server0        :-
  NBNS-server0       :-
  Netbios-type       :-
  Position           : Local             Status             : Unlocked
  Gateway-0          : 10.0.12.1
  Network            : 10.0.12.0
  Mask               : 255.255.255.0
  VPN instance       : --


----------------------------------------------------------------------------       Start   End       Total
Used Idle(Expired) Conflict Disable
----------------------------------------------------------------------------
   10.0.12.1         10.0.12.254     253       0          253(0)               0     0
----------------------------------------------------------------------------




Configure the default management interface for S1 to request an IP address from

the DHCP server (R1). Perform the same steps on S2 for R3.


[S1]dhcp enable


                                                      HUAWEI TECHNOLOGIES                        Page97
[S1]interface Vlanif 1
[S1-Vlanif1]ip address dhcp-alloc


<S1>display ip interface brief
…output omitted…
Interface                      IP Address/Mask      Physical     Protocol
MEth0/0/1                              unassigned                 down         down
NULL0                                  unassigned                 up           up(s)
Vlanif1                                10.0.12.254/24             up           up




Verify that this address was taken from the DHCP pool named pool1 on R1,

and for S2, from the DHCP pool named pool2 on R3.



<R1>display ip pool name pool1
  Pool-name          : pool1
  Pool-No            :0
  Lease              : 1 Days 12 Hours 0 Minutes
  Domain-name        :-
  DNS-server0        :-
  NBNS-server0       :-
  Netbios-type       :-
  Position         : Local                Status           : Unlocked
  Gateway-0          : 10.0.12.1
  Network            : 10.0.12.0
  Mask               : 255.255.255.0
  VPN instance       : --


----------------------------------------------------------------------------
     Start           End           Total Used Idle(Expired)       Conflict Disable
----------------------------------------------------------------------------
   10.0.12.1     10.0.12.254        253       1         252(0)           0          0
----------------------------------------------------------------------------




 <R3>display ip pool name pool2
  Pool-name          : pool2
  Pool-No            :0
  Lease              : 1 Days 12 Hours 0 Minutes



                                                        HUAWEI TECHNOLOGIES             Page98
  Domain-name        :-
  DNS-server0        :-
  NBNS-server0       :-
  Netbios-type       :-
  Position           : Local             Status             : Unlocked
  Gateway-0          : 10.0.23.3
  Network           : 10.0.23.0
  Mask               : 255.255.255.0
  VPN instance       : --
 ----------------------------------------------------------------------------
          Start             End        Total Used Idle(Expired)        Conflict Disable
 ----------------------------------------------------------------------------
       10.0.23.1     10.0.23.254     253       1         252(0)                 0         0
 ----------------------------------------------------------------------------



Ensure that global pool configuration has been completed for both R1 and R3
before continuing!

Step 6 Create an interface based IP address pool

Disable the interface GigabitGigabitEthernet 0/0/1 R1. For R3 disable interface
Gigabit Ethernet 0/0/2.
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]shutdown


[R3]interface GigabitEthernet 0/0/2
[R3-GigabitEthernet0/0/2]shutdown


Configure an interface address pool to allow the clients connected via Gigabit
Ethernet 0/0/2 of R1 to obtain IP addresses. Perform the same operation for
GigabitGigabitEthernet 0/0/1 of R3. Do not enable these interfaces, as we do not yet
wish to activate the DHCP service on the network.
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]dhcp select interface




[R3]interface GigabitEthernet 0/0/1
[R3-GigabitEthernet0/0/1]dhcp select interface
Isolate addresses from the pool GigabitEthernet0/0/2 for R1, and the pool
GigabitEthernet0/0/1 for R3, for DNS services. Additionally, set the IP address lease


                                                      HUAWEI TECHNOLOGIES                     Page99
period for the interface address pool.
[R1-GigabitEthernet0/0/2]dhcp server dns-list 10.0.23.254
[R1-GigabitEthernet0/0/2]dhcp server excluded-ip-address 10.0.23.254
[R1-GigabitEthernet0/0/2]dhcp server lease day        1 hour 12


[R3-GigabitEthernet0/0/1]dhcp server dns-list 10.0.12.254
[R3-GigabitEthernet0/0/1]dhcp server excluded-ip-address 10.0.12.254
[R3-GigabitEthernet0/0/1]dhcp server lease day 1 hour 12


Run the display ip pool interface command on the router to view the configured
parameters of the interface address pool. For R3 the interface is
GigabitGigabitEthernet 0/0/1.


<R1>display ip pool interface GigabitEthernet0/0/2
  Pool-name          : GigabitEthernet0/0/2
  Pool-No            :1
  Lease              : 1 Days 12 Hours 0 Minutes
  Domain-name        :-
  DNS-server0        : 10.0.23.254
  NBNS-server0       :-
  Netbios-type       :-
  Position           : Interface        Status              : Unlocked
  Gateway-0          : 10.0.23.1
  Network           : 10.0.23.0
  Mask               : 255.255.255.0
  VPN instance       : --
 ----------------------------------------------------------------------------
          Start           End        Total Used Idle(Expired)        Conflict Disable
 ----------------------------------------------------------------------------
       10.0.23.1          10.0.23.254   253      0          252(0)              0       1
 ----------------------------------------------------------------------------



Flush the existing Vlanif1 address from S2 to allow for dynamic allocation of a new

IP address from the interface GigabitEthernet0/0/2 pool.


[S2]interface Vlanif 1
[S2-Vlanif1]shutdown
[S2-Vlanif1]undo shutdown




                                                      HUAWEI TECHNOLOGIES                   Page100
Enable interface Gigabit Ethernet 0/0/2 to allow the DHCP server to become active
on the network and to begin sending DHCP discover messages.
[R1]interface GigabitEthernet0/0/2
[R1-GigabitEthernet0/0/2]undo shutdown


<R1>display ip pool interface GigabitEthernet0/0/2
  Pool-name          : GigabitEthernet0/0/2
  Pool-No            :1
  Lease              : 1 Days 12 Hours 0 Minutes
  Domain-name        :-
  DNS-server0        : 10.0.23.254
  NBNS-server0       :-
  Netbios-type       :-
  Position           : Interface           Status             : Unlocked
  Gateway-0          : 10.0.23.1
  Network            : 10.0.23.0
  Mask               : 255.255.255.0
  VPN instance       : --
 ----------------------------------------------------------------------------
          Start             End        Total Used Idle(Expired)       Conflict Disable
 ----------------------------------------------------------------------------
      10.0.23.1     10.0.23.254      253       1          251(0)           0               1
 ----------------------------------------------------------------------------
<S2>display ip interface brief
…output omitted…
Interface                              IP Address/Mask          Physical        Protocol
MEth0/0/1                                    unassigned               down             down
NULL0                                        unassigned               up               up(s)
Vlanif1                                      10.0.23.253/24           up               up



The interface Vlanif1 shows to have been allocated an address from the
GigabitEthernet0/0/2 address pool of R1.
Flush the existing Vlanif1 address from S1 to allow for dynamic allocation of a new
IP address from the interface GigabitEther0/0/1 pool.


[S1]interface Vlanif 1
[S1-Vlanif1]shutdown
[S1-Vlanif1]undo shutdown


Enable interface GigabitGigabitEthernet 0/0/1 to allow the DHCP server to become


                                                      HUAWEI TECHNOLOGIES                      Page101
active on the network and to begin sending DHCP discover messages.


[R3]interface GigabitEthernet 0/0/1
[R3-GigabitEthernet0/0/1]undo shutdown



Verify that the new IP address as been allocated from the interface pool.


<R3>display ip pool interface GigabitEthernet0/0/1
    Pool-name        : GigabitEthernet0/0/1
    Pool-No          :1
    Lease            : 1 Days 12 Hours 0 Minutes
    Domain-name      :-
    DNS-server0      : 10.0.12.254
    NBNS-server0     :-
    Netbios-type     :-
    Position         : Interface           Status             : Unlocked
    Gateway-0        : 10.0.12.3
    Network         : 10.0.12.0
    Mask             : 255.255.255.0
    VPN instance     : --
 ----------------------------------------------------------------------------
            Start           End        Total Used Idle(Expired)         Conflict Disable
 ----------------------------------------------------------------------------
        10.0.12.1   10.0.12.254      253       1          251(0)              0              1
 ----------------------------------------------------------------------------
<S1>display ip interface brief
…output omitted…
Interface                              IP Address/Mask             Physical       Protocol
MEth0/0/1                                    unassigned                 down             down
NULL0                                        unassigned                 up               up(s)
Vlanif1                                      10.0.12.253/24             up               up


It should also be noted that a default static route pointing to the DHCP server is
automatically generated by the switch, as seen in the final configuration below.


Final Configuration

[R1]display current-configuration
[V200R007C00SPC600]
#



                                                      HUAWEI TECHNOLOGIES                        Page102
 sysname R1
#
dhcp enable
#
ip pool pool1
 gateway-list 10.0.12.1
 network 10.0.12.0 mask 255.255.255.0
 lease day 1 hour 12 minute 0
#
interface GigabitEthernet0/0/1
 shutdown
 ip address 10.0.12.1 255.255.255.0
 dhcp select global
#
interface GigabitEthernet0/0/2
 ip address 10.0.23.1 255.255.255.0
 dhcp select interface
 dhcp server excluded-ip-address 10.0.23.254
 dhcp server lease day 1 hour 12 minute 0
 dhcp server dns-list 10.0.23.254
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$+L'YR&IZt'4,)>-*#lH",}%K-oJ_M9+'lOU~bD(\WTqB}%N,%$%$user-i
nterface vty 0 4
#
return


[R3]dis current-configuration
[V200R007C00SPC600]
#
 sysname R3
#
dhcp enable
#
ip pool pool2
 gateway-list 10.0.23.3
 network 10.0.23.0 mask 255.255.255.0
 lease day 1 hour 12 minute 0
#
interface GigabitEthernet0/0/1
 ip address 10.0.12.3 255.255.255.0



                                               HUAWEI TECHNOLOGIES                   Page103
 dhcp select interface
 dhcp server excluded-ip-address 10.0.12.254
 dhcp server lease day 1 hour 12 minute 0
 dhcp server dns-list 10.0.12.254
#
interface GigabitEthernet0/0/2
 shutdown
 ip address 10.0.23.3 255.255.255.0
 dhcp select global
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$ksXDMg7Ry6yUU:63:DQ),#/sQg"@*S\U#.s.bHWxQ,y%#/v,%$%$
user-interface vty 0 4
#
return


<S1>dis current-configuration
#
!Software Version V200R008C00SPC500
 sysname S1
#
 dhcp enable
#
interface Vlanif1
 ip address dhcp-alloc
#
 ip route-static 0.0.0.0 0.0.0.0 10.0.12.3
#
user-interface con 0
user-interface vty 0 4
#
return


<S2>display current-configuration
#
!Software Version V200R008C00SPC500
 sysname S2
#
 dhcp enable
#
interface Vlanif1



                                               HUAWEI TECHNOLOGIES                Page104
 ip address dhcp-alloc
#
 ip route-static 0.0.0.0 0.0.0.0 10.0.23.1
#
user-interface con 0
user-interface vty 0 4
#
return




                                             HUAWEI TECHNOLOGIES   Page105
             Recommendations
       Huawei Learning Website
            http://learning.huawei.com/en

       Huawei e-Learning
            https://ilearningx.huawei.com/portal/#/portal/ebg/51



       Huawei Certification
            http://support.huawei.com/learning/NavigationAction!createNavi?navId=_31
             &lang=en

       Find Training
            http://support.huawei.com/learning/NavigationAction!createNavi?navId=_trai
             ningsearch&lang=en


             More Information
   Huawei learning APP




版权所有© 2019 华为技术有限公司
