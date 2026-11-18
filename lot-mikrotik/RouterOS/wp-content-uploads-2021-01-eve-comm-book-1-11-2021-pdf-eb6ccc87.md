EVE-NG Community 
Cookbook 
 
 
Version 1.11  
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
© EVE-NG LTD 
The information contained in this document is the property of EVE-NG Limited 
The contents of the document must not be reproduced or disclosed wholly or in part or used 
for purposes other than that for which it is supplied without the prior written permission of 
EVE-NG Limited. 
Author:  
Uldis Dzerkals 
 
Editors:  
Michael Doe 
Christopher Lim

EVE-NG Community Cookbook 
Version 1.11 
Page 2 of 165 © EVE-NG LTD 
 
Table of Contents 
 
PREFACE .................................................................................................................................. 7 
1 INTRODUCTION ................................................................................................................ 8 
1.1 WHAT IS EVE-NG? ....................................................................................................... 8 
1.2 WHAT IS EVE-NG USED FOR? ....................................................................................... 8 
1.3 WHO IS EVE-NG FOR? ................................................................................................. 8 
2 SYSTEM REQUIREMENTS............................................................................................... 9 
2.1 HARDWARE REQUIREMENTS ........................................................................................... 9 
2.1.1 Minimal Laptop/PC Desktop system requirements .............................................. 9 
2.1.2 Recommended Laptop/PC Desktop system requirements ................................ 10 
2.1.3 Virtual Server system requirements ................................................................... 10 
2.1.4 Dedicated Server (bare) system requirements .................................................. 11 
2.1.5 Nodes per lab calculator .................................................................................... 11 
2.2 SUPPORTED VIRTUALIZATION PLATFORMS AND SOFTWARE ............................................. 11 
2.3 UNSUPPORTED HARDWARE AND SYSTEMS .................................................................... 12 
3 INSTALLATION ............................................................................................................... 13 
3.1 VMWARE WORKSTATION OR VM PLAYER ..................................................................... 13 
3.1.1 VMware workstation EVE VM installation using ISO image (preferred)  ............ 13 
3.1.1.1 EVE VM Setup and Settings ....................................................................................................... 13 
3.1.1.2 EVE-NG VM Installation steps ................................................................................................... 16 
3.1.2 VMware workstation OVF deployment .............................................................. 22 
3.1.2.1 Deployment and VM machine settings ..................................................................................... 22 
3.1.2.2 OVF VM update to the latest EVE version ................................................................................. 24 
3.1.2.3 OVF VM HDD Size expansion ..................................................................................................... 24 
3.2 VMWARE ESXI ........................................................................................................... 24 
3.2.1 VMware ESXi EVE installation using ISO image (preferred)............................. 24 
3.2.1.1 EVE-NG ESXi VM Setup and Settings ......................................................................................... 24 
3.2.1.2 EVE-NG ESXi VM Installation steps ............................................................................................ 26 
3.2.2 VMware ESXi OVF deployment ......................................................................... 32 
3.2.2.1 ESXi OVF VM Setup and Settings ............................................................................................... 32 
3.2.2.2 ESXi OVF VM update to the latest EVE version ......................................................................... 34 
3.2.2.3 ESXi OVF VM HDD Size expansion ............................................................................................. 34 
3.3 BARE HARDWARE SERVER EVE INSTALLATION .............................................................. 34 
3.3.1 Ubuntu Server Installation Phase 1 ................................................................... 34 
3.3.2 EVE Community Installation Phase 2 ................................................................ 42 
3.3.3 EVE Community Installation Phase 3 ................................................................ 42 
3.4 GOOGLE CLOUD PLATFORM ......................................................................................... 43 
3.4.1 Google account .................................................................................................. 43 
3.4.2 Goggle Cloud project ......................................................................................... 43 
3.4.3 Preparing Ubuntu boot disk template ................................................................ 45 
3.4.4 Creating VM ....................................................................................................... 46 
3.4.5 EVE-NG-Community installation ........................................................................ 48 
3.4.6 Access to Google Cloud EVE-COMM ............................................................... 50 
3.4.7 Optional: GCP Firewall rules for native console use ......................................... 50 
3.5 EVE MANAGEMENT IP ADDRESS SETUP ....................................................................... 53 
3.5.1 Management static IP address setup (preferred) .............................................. 53

EVE-NG Community Cookbook 
Version 1.11 
Page 3 of 165 © EVE-NG LTD 
3.5.2 EVE Management IP address setup via DHCP ................................................. 56 
3.5.3 EVE Management IP address reset .................................................................. 58 
3.6 EVE-NG COMMUNITY UPGRADE TO EVE-NG PROFESSIONAL ....................................... 58 
3.6.1 Mandatory Prerequisites .................................................................................... 58 
3.6.1.1 EVE Community disk space ....................................................................................................... 58 
3.6.1.2 Verify current EVE Community version ..................................................................................... 58 
3.6.1.3 Steps to upgrade to the latest EVE Community version ............................................................ 59 
3.6.2 Upgrading EVE Community to EVE-NG Professional ....................................... 59 
3.7 NATIVE TELNET CONSOLE MANAGEMENT SETUP ............................................................ 60 
3.7.1 Windows Native Console ................................................................................... 60 
3.7.2 Linux Native Console ......................................................................................... 61 
3.7.3 MAC OSX Native Console ................................................................................. 62 
3.8 LOGIN TO THE EVE WEB GUI ..................................................................................... 64 
4 EVE-NG COMMUNITY UPDATE & UPGRADE .............................................................. 66 
4.1 EVE-NG COMMUNITY UPDATE .................................................................................... 66 
4.2 EVE-NG COMMUNITY UPGRADE .................................................................................. 67 
5 TYPES OF EVE MANAGEMENT CONSOLES ............................................................... 69 
5.1 NATIVE CONSOLE ........................................................................................................ 69 
5.1.1 Native Console: telnet ........................................................................................ 69 
5.1.2 Native Console: Wireshark ................................................................................ 70 
5.1.3 Native Console: VNC ......................................................................................... 72 
5.1.4 Native Console: RDP ......................................................................................... 73 
5.2 HTML5 CONSOLE ........................................................................................................ 74 
5.2.1 HTML5 Console: Telnet ..................................................................................... 74 
5.2.2 HTML5 Console: VNC ....................................................................................... 75 
5.2.3 HTML5 Console: RDP for Windows .................................................................. 75 
6 EVE WEB GUI MANAGEMENT ...................................................................................... 77 
6.1 EVE MANAGEMENT PAGE ............................................................................................ 77 
6.1.1 Management buttons ......................................................................................... 77 
6.1.2 Management tabs .............................................................................................. 78 
6.2 FOLDERS AND LAB FILES MANAGEMENT ........................................................................ 79 
6.2.1 Folders Management ......................................................................................... 79 
6.2.1.1 Create folder ............................................................................................................................. 79 
6.2.1.2 Delete folder ............................................................................................................................. 79 
6.2.1.3 Move Folder .............................................................................................................................. 79 
6.2.1.4 Export Folder ............................................................................................................................. 80 
6.2.1.5 Import Folder ............................................................................................................................ 80 
6.2.2 Lab files Management........................................................................................ 81 
6.2.2.1 Create Lab ................................................................................................................................. 82 
6.2.2.2 Delete Lab ................................................................................................................................. 82 
6.2.2.3 Clone Lab ................................................................................................................................... 82 
6.2.2.4 Move Lab ................................................................................................................................... 83 
6.2.2.5 Export Lab ................................................................................................................................. 83 
6.2.2.6 Import Labs ............................................................................................................................... 84 
6.3 EVE MANAGEMENT DROPDOWN MENU ........................................................................ 85 
6.3.1 EVE User management ..................................................................................... 85 
6.3.1.1 Creating a new EVE User ........................................................................................................... 85 
6.3.1.2 Edit EVE User ............................................................................................................................. 86 
6.3.1.3 User monitoring ........................................................................................................................ 87 
6.4 EVE SYSTEM DROPDOWN MENU .................................................................................. 87 
6.4.1 System status .................................................................................................... 88 
6.4.2 System logs ....................................................................................................... 88

EVE-NG Community Cookbook 
Version 1.11 
Page 4 of 165 © EVE-NG LTD 
6.4.3 Stop All Nodes ................................................................................................... 89 
6.5 EVE INFORMATION DROPDOWN MENU .......................................................................... 89 
6.6 OTHER TAB LINE INFO .................................................................................................. 89 
6.7 LAB PREVIEW AND GLOBAL SETTINGS ............................................................................ 89 
6.7.1 Lab preview window ........................................................................................... 90 
6.7.2 Lab preview buttons ........................................................................................... 90 
6.7.3 Lab preview information ..................................................................................... 91 
6.7.4 Lab Global Settings ............................................................................................ 91 
7 EVE WEB TOPOLOGY PAGE ........................................................................................ 93 
7.1 SIDE BAR FUNCTIONS ................................................................................................... 93 
7.1.1 Add an object ..................................................................................................... 94 
7.1.1.1 Node object ............................................................................................................................... 94 
7.1.1.2 Network object.......................................................................................................................... 94 
7.1.1.3 Picture object ............................................................................................................................ 95 
7.1.1.4 Custom shape object ................................................................................................................. 95 
7.1.1.5 Text object ................................................................................................................................ 96 
7.1.2 Nodes ................................................................................................................. 96 
7.1.3 Networks ............................................................................................................ 97 
7.1.4 Startup-configs ................................................................................................... 98 
7.1.5 Logical Maps ...................................................................................................... 98 
7.1.6 Configured Objects ............................................................................................ 99 
7.1.7 More actions ...................................................................................................... 99 
7.1.7.1 Start all nodes ........................................................................................................................... 99 
7.1.7.2 Stop all nodes ............................................................................................................................ 99 
7.1.7.3 Wipe all nodes ......................................................................................................................... 100 
7.1.7.4 Console to All Nodes ............................................................................................................... 100 
7.1.7.5 Export all CFGs ........................................................................................................................ 100 
7.1.7.6 Edit lab .................................................................................................................................... 100 
7.1.7.7 Set node’s startup-cfg to default configset ............................................................................. 101 
7.1.7.8 Set node’s startup-cfg to none ................................................................................................ 101 
7.1.7.9 Delete default startup-cfgs...................................................................................................... 101 
7.1.8 Refresh Topology ............................................................................................. 101 
7.1.9 Lab page zoom/unzoom .................................................................................. 101 
7.1.10 Status ............................................................................................................... 102 
7.1.11 Lab details ........................................................................................................ 102 
7.1.12 Lock Lab with password................................................................................... 103 
7.1.13 Dark mode or Light mode ................................................................................ 103 
7.1.14 Close lab .......................................................................................................... 103 
7.1.15 Logout .............................................................................................................. 103 
7.2 EVE LAB TOPOLOGY MENUS ...................................................................................... 104 
7.2.1 Lab topology menu .......................................................................................... 104 
7.2.2 Connection menu ............................................................................................. 104 
7.2.3 Cloud or Bridge network menu ........................................................................ 104 
7.2.4 Stopped node menu......................................................................................... 105 
7.2.5 Running node menu......................................................................................... 105 
7.2.6 Selected nodes menu and features ................................................................. 106 
7.3 EVE LAB NODE STATES AND SYMBOLS........................................................................ 109 
7.3.1 Stopped (non-running) nodes .......................................................................... 109 
7.3.2 Running nodes ................................................................................................. 110 
7.3.3 Node connector symbol ................................................................................... 110 
7.4 OTHER ...................................................................................................................... 111 
7.4.1 Notifications area ............................................................................................. 111 
8 WORKING WITH EVE LABS ........................................................................................ 112

EVE-NG Community Cookbook 
Version 1.11 
Page 5 of 165 © EVE-NG LTD 
8.1 CREATING A LAB ........................................................................................................ 112 
8.1.1 Adding nodes to the lab ................................................................................... 112 
8.1.1.1 Node values Table ................................................................................................................... 115 
8.1.2 Edit node .......................................................................................................... 117 
8.1.2.1 Edit nodes globally .................................................................................................................. 118 
8.1.2.2 Edit node individually. ............................................................................................................. 118 
8.1.3 Wipe Node ....................................................................................................... 119 
8.1.4 Interconnecting nodes...................................................................................... 120 
8.1.5 Delete connection between nodes ................................................................... 120 
8.1.6 Delete Node ..................................................................................................... 121 
8.2 RUNNING LABS .......................................................................................................... 121 
8.2.1 Starting lab ....................................................................................................... 121 
8.3 SAVING LABS ............................................................................................................. 121 
8.4 STOPPING LABS ......................................................................................................... 122 
8.5 START SAVED LAB...................................................................................................... 122 
8.6 IMPORTING LABS ........................................................................................................ 122 
8.7 EXPORTING LABS ....................................................................................................... 122 
8.8 DELETING LABS ......................................................................................................... 123 
8.9 MOVING LABS ............................................................................................................ 123 
9 EVE CLOUDS AND NETWORKS ................................................................................. 124 
9.1 BRIDGE NETWORK ..................................................................................................... 124 
9.2 MANAGEMENT CLOUD0 INTERFACE ............................................................................ 125 
9.3 OTHER CLOUD INTERFACES ........................................................................................ 127 
9.4 CONNECTING EXTERNAL VM MACHINES TO THE EVE LAB ............................................ 129 
9.4.1 ESXi VM machines .......................................................................................... 129 
9.4.2 VMWare workstation machines ....................................................................... 131 
9.5 CONNECTING EVE LAB TO A PHYSICAL DEVICE ........................................................... 133 
9.5.1 ESXi EVE ......................................................................................................... 133 
9.5.2 VMWare workstation EVE ............................................................................... 136 
9.5.3 Bare metal server EVE .................................................................................... 138 
10 ADVANCED EVE LAB FEATURES .......................................................................... 139 
10.1 LAB DESIGN OBJECTS ............................................................................................. 139 
10.1.1 Custom shape .................................................................................................. 139 
10.1.2 Resize square or circle objects ........................................................................ 140 
10.1.3 Text object ....................................................................................................... 140 
10.1.4 Add custom picture on the Lab using Text object feature ............................... 141 
10.1.5 Cloning objects and overlay positions ............................................................. 142 
10.1.6 Objects Editing ................................................................................................. 142 
10.1.7 Lock objects movement ................................................................................... 143 
10.2 CUSTOM DESIGN LOGICAL TOPOLOGY ..................................................................... 143 
10.2.1 Custom design upload ..................................................................................... 144 
10.2.2 Custom topology mapping ............................................................................... 145 
10.2.3 Delete topology or mapping ............................................................................. 146 
10.3 CONFIGURATION EXPORT FEATURE ......................................................................... 146 
10.3.1 Supported nodes for configuration exports ...................................................... 147 
10.3.2 Startup config management ............................................................................. 147 
10.3.2.1 Global commands ............................................................................................................... 147 
10.3.2.2 Individual node commands ................................................................................................. 148 
10.3.2.3 Multiple selected nodes commands ................................................................................... 148 
10.3.2.4 Startup-configuration window ............................................................................................ 149 
10.3.2.5 Startup-config window information ................................................................................... 149 
10.3.3 Export configuration ......................................................................................... 150

EVE-NG Community Cookbook 
Version 1.11 
Page 6 of 165 © EVE-NG LTD 
10.3.4 Boot nodes from exported config set ............................................................... 150 
10.3.5 Edit exported configurations ............................................................................ 151 
10.3.6 Set lab to boot from none ................................................................................ 151 
10.3.7 Lab config script timeout .................................................................................. 152 
11 EVE TROUBLESHOOTING ....................................................................................... 153 
11.1 CLI DIAGNOSTIC INFORMATION DISPLAY COMMANDS ................................................ 153 
11.1.1 Display full EVE Community diagnostic ........................................................... 153 
11.1.2 Display the currently installed EVE Community version: ................................. 153 
11.1.3 Display if EVEs Intel VT-x/EPT option on/off: .................................................. 153 
11.1.4 Display EVEs CPU INFO: ................................................................................ 153 
11.1.5 Display EVEs HDD utilization. ......................................................................... 153 
11.1.6 Display EVEs Bridge interface status .............................................................. 154 
11.1.7 Display EVEs system services status .............................................................. 154 
11.2 EXPAND EVES SYSTEM HDD................................................................................. 154 
11.2.1 Expand HDD on VMware Workstation ............................................................. 155 
11.2.2 Expand your HDD on ESXi .............................................................................. 155 
11.2.3 Expand your HDD on a Bare Metal EVE Server ............................................. 156 
11.3 RESET MANAGEMENT IP ........................................................................................ 156 
11.4 EVE COMMUNITY SQL DATABASE RECOVERY ........................................................ 156 
11.5 EVE LOG FILES ..................................................................................................... 156 
11.6 EVE CLI DIAGNOSTIC INFO ...................................................................................... 157 
12 IMAGES FOR EVE ..................................................................................................... 158 
12.1 QEMU IMAGE NAMING TABLE ................................................................................... 158 
12.2 HOW TO PREPARE IMAGES FOR EVE ....................................................................... 159 
12.3 HOW TO ADD CUSTOM IMAGE TEMPLATE .................................................................. 159 
12.3.1 Templates folder choice ................................................................................... 159 
12.3.2 Prepare template file ........................................................................................ 159 
12.3.3 Prepare interface format and name lines......................................................... 160 
12.3.4 Edit your new template file: .............................................................................. 162 
12.3.5 Prepare new icon for your template: ................................................................ 163 
12.3.6 Template use ................................................................................................... 163 
12.4 HOW TO HIDE UNUSED IMAGES IN THE NODE LIST ..................................................... 163 
12.4.1 Creating new config.php file ............................................................................ 163 
12.4.2 Edit config.php file ............................................................................................ 164 
13 EVE RESOURCES ..................................................................................................... 165

EVE-NG Community Cookbook 
Version 1.11 
Page 7 of 165 © EVE-NG LTD 
Preface 
When I first heard about EVE-NG I was skeptical. Back then I used to Lab mainly with ESX by 
deploying many virtual Devices and connecting them manually by separate vSwitches for Point-
to-Point connections. The Problem with that was, that it was extremely time-consuming and did 
not scale - for every new Device I had to create multiple vSwitches to interconnect them with 
the virtual Machines - a Nightmare. I was in the middle of my JNCIE-Exam-Prep when I first 
saw EVE-NG on Twitter - I downloaded the Community Edition, which was the only Edition 
back then and I was amazed how easy Labbing all of a sudden was. No more deploying of 
vSwitches to interconnect nodes and boy did it Scale… 
If you follow me on Twitter you know, that I'm one of the hardest Juniper Fanboys and of course 
my Goal was to "Juniperize" EVE. I started to get in touch with Uldis and Alain and found myself 
into the Position as one of the Juniper Test Guys. Meanwhile I added nearly all Juniper related 
Devices (including cSRX and JATP) and I still test a Lot - but now on EVE-Pro. 
The Pro-Edition was a big step forward for the Project. It added some nice Features like "hot -
add-interconnect" and the Ability to use EVE-NG with multiple Users. Especially Companies 
will love EVE as it is THE Solution for Labs and PoC's. I have succe ssfully run over 30 PoC's 
in EVE and over 100 Labs (Job -Related and Personal Labs) - and I still enjoy it every day 
thanks to EVE and the a mazing Team behind it. When the Guys asked me to write the 
Introduction I was of course honored and now this Book is finally coming out to help you on 
your Quest to Setup, Run and Manage EVE-NG in a lot of possible ways. 
 
Well - enough from my Side. I hope you enjoy this Cookbook and use it wisely for your Everyday 
EVE Work. If you have Problems there is always the EVE -Forum and Live-Helpdesk - you will 
also find me there from time to time ;) 
I wish you happy reading and if you think, that this Product is amazing feel free to support it by 
buying the PRO-Edition or Donating a bit – it helps to expand this already cool Pro duct even 
more and it also honors all the work that the Guys spent in it. 
 
Christian Scholz 
@chsjuniper

EVE-NG Community Cookbook 
Version 1.11 
Page 8 of 165 © EVE-NG LTD 
1 Introduction 
1.1 What is EVE-NG? 
To describe what Emulated Virtual Environment – Next Generation (EVE-NG) is without solely 
stating dry facts about features, we need to elaborate more on what EVE -NG can be used for 
and whom it would be useful for. 
In some trivial dry words, EVE -NG gives you tools to use around virtual devices and 
interconnect them with other virtual or physical devices. Many of its features greatly simplify the 
usabilities, re-usability, manageability, interconnectivity, distribution and therefore the ability to 
understand and share topologies, work, ideas, concepts or simply “labs”. This can simply mean 
it will reduce the cost and time to s et up what you need or it might enable you to do tasks you 
would not have thought could be done this simple. 
1.2 What is EVE-NG used for? 
This is the real question but there is no finite answer, the possibilities are almost limitless and 
depends on what you want to use it for. 
It can be used for studying all kinds of technologies. You can learn about general technologies 
or vendor specific topics. You can test new technologies like network automation, SDN, etc. 
It can be used to recreate corporate networks and test changes before putting them i nto 
production. You can create proof of concepts for clients . You can troubleshoot network issues 
by recreating them and e.g. use Wireshark to inspect packets. 
It is most definitely not just for networking, it can be used to test software in simulated networks, 
test out security vulnerabilities of any kind, system engineering like LDAP and AD servers and 
many more areas. 
You could set it up to automate sandboxing unknown files/software and use software to analyse 
short and long term behaviour for malicious intent much simpler than without EVE -NG. 
The list of what EVE -NG can be used for co uld go on indefinitely, possibilities are limited by 
knowledge and imagination only. Both of which can be improved with EVE-NG. 
To get a ve ry small idea of what can be done with EVE -NG, check out the tested/ supported 
images (many have not been tested, almost everything virtual should run on EVE-NG) and refer 
to section 12. 
EVE-NG helps you achieve what you want to and more. 
1.3 Who is EVE-NG for? 
EVE-NG is for everyone working in the Information Technology Sector, period. 
It is for very large enterprise companies, training facilities, service providers, consultants, 
people who want to train themselves; it is for everyone, it is for YOU! 
Use-cases that are more than worth it, almost priceless even, can be found everywhere.  
 
The EVE-NG community version is free for everyone; while the paid professional version adds 
a few things that make your life easier. Almost everything can still be done with the free version, 
just less conveniently and therefore more time-consuming. 
However, with the free version, the possibility to train yourself with  technologies, hone your 
skills and become an expert even with very no monetary possibilities. For some this is and has 
been life changing.

EVE-NG Community Cookbook 
Version 1.11 
Page 9 of 165 © EVE-NG LTD 
2 System requirements 
EVE-NG is available in the OVF or ISO file format. The Open Virtualization Format (OVF) is an 
open standard for packaging and distributing virtual appliances. It can be used to deploy a VM 
in hypervisors like VMware Workstation, Player and ESXi. Please note that installing EVE as a 
Virtual Machine (VM) will mean any nodes deployed within EVE will be nested. Nested 
virtualization causes degraded performance in deployed nodes . This should be fine for  lab 
purposes as long as the host meets or exceed s the resource requirements for the deployed 
nodes.  
EVE-NG can also be installed directly on physical hard ware, without a hypervisor, using the 
provided ISO image. This is referred to as a “bare metal” install and is the most recommended 
method of installing EVE-NG. 
2.1 Hardware requirements 
2.1.1 Minimal Laptop/PC Desktop system requirements 
Prerequisites: 
CPU: Intel CPU supporting Intel® VT-x /EPT virtualization 
Operating System: Windows 7, 8, 10 or Linux Desktop 
VMware Workstation 12.5 or later 
VMware Player 12.5 or later 
 
PC/Laptop HW requirements 
CPU Intel i5/i7 (4 Logical processors), Enabled Intel virtualization 
in BIOS 
RAM 8Gb 
HDD Space 40Gb 
Network LAN/WLAN 
EVE Virtual machine requirements 
CPU 4/1 (Number of processors/Number of cores per processor) 
Enabled Intel VT-x/EPT virtualization engine 
 
RAM 6Gb or more 
HDD 40Gb or more 
Network VMware NAT or Bridged network adapter 
Note: Minimal PC Desktop/Laptop will be able to run small Labs. The performance and quantity 
of nodes per lab depend on the types of nodes deployed in the lab. 
Example:  
IOL image-based nodes: up to 40-50 nodes per lab 
Dynamips image-based nodes: up to 20-25 nodes per lab 
vIOS image-based nodes: up to 8-10 nodes per lab 
CSRv1000 or XRv image-based nodes: up to 2-3 per lab

EVE-NG Community Cookbook 
Version 1.11 
Page 10 of 165 © EVE-NG LTD 
 
2.1.2 Recommended Laptop/PC Desktop system requirements 
Prerequisites: 
CPU: Intel CPU supporting Intel® VT-x /EPT virtualization  
Operation System: Windows 7, 8, 10 or Linux Desktop 
VMware Workstation 12.5 or later 
VW Ware Player 12.5 or later 
 
PC/Laptop HW requirements 
CPU Intel i7 (8 Logical processors), Enabled Intel virtualization in 
BIOS 
RAM 32Gb 
HDD Space 200Gb 
Network LAN/WLAN 
EVE Virtual machine requirements 
CPU 8/1 (Number of processors/Number of cores per processor) 
Enabled Intel VT-x/EPT virtualization engine 
 
RAM 24Gb or more 
HDD 200Gb or more 
Network VMware NAT or Bridged network adapter 
 
Note: PC Desktops/Laptops will be able to run small to medium Labs. Performance and quantity 
of nodes per lab depend on the type of nodes deployed in the lab. 
Example:  
IOL image-based nodes: up to 120 nodes per lab 
vIOS image-based nodes: up to 20-40 nodes per lab 
CSR image-based nodes: up to 10 per lab 
2.1.3 Virtual Server system requirements 
Prerequisites: 
CPU: Intel Xeon CPU supporting Intel® VT-x with Extended Page Tables (EPT) 
Operation System: ESXi 6.0 or later 
 
Server HW requirements 
CPU Recommended CPU 2x Intel E5-2650v3 (40 Logical processors) 
or better supporting Intel® VT -x with Extended Page Tables 
(EPT) 
Minimum CPU is any Intel Xeon CPU supporting Intel® VT -x 
with Extended Page Tables (EPT) 
RAM 128Gb 
HDD Space 2Tb 
Network LAN Ethernet

EVE-NG Community Cookbook 
Version 1.11 
Page 11 of 165 © EVE-NG LTD 
EVE Virtual machine requirements 
CPU 32/1 (Number of processors/Number of cores per processor) 
Enabled Intel VT-x/EPT virtualization engine 
 
RAM 64Gb or more 
HDD 800Gb or more 
Network vSwitch/VMnet 
Note: Performance and quantity of nodes per lab depends from the type of nodes used in the 
lab.  
Example:  
120 IOL image-based lab 
20 CSRv1000 image-based nodes per lab 
2.1.4 Dedicated Server (bare) system requirements 
Prerequisites: 
CPU: Intel Xeon CPU supporting Intel® VT-x with Extended Page Tables (EPT) 
Operation System: Ubuntu Server 16.04.4 LTS x64 
 
Server HW requirements 
CPU Recommended CPU Intel E5-2650v3 (40 Logical processors) or 
better supporting Intel® VT-x with Extended Page Tables (EPT) 
Minimum CPU is any Intel Xeon CPU supporting Intel® VT -x 
with Extended Page Tables (EPT) 
RAM 128Gb 
HDD Space 2Tb 
Network LAN Ethernet 
 
Note: Performance and quantity of nodes per lab depends from type of nodes used in the lab.  
2.1.5 Nodes per lab calculator 
It is recommended to use the “nodes per lab calculator” to achieve best performance and avoid 
overloading your EVE system. 
https://drive.google.com/file/d/1Rbu7KDNSNuWiv_AphWx0vCek8CKVB1WI/view  
2.2 Supported virtualization platforms and software 
• VMware Workstation 12.5 or later 
• VMware Player 12.5 or later 
• VMware ESXi 6.0 or later

EVE-NG Community Cookbook 
Version 1.11 
Page 12 of 165 © EVE-NG LTD 
• Ubuntu Server 16.04 LTS as platform for bare metal 
• Google Cloud Platform 
2.3 Unsupported hardware and systems 
The following are currently not supported: 
• VirtualBox virtualization 
• Citrix XenServer 
• Microsoft HyperV 
• Ubuntu 17.X or 18.x as platform

EVE-NG Community Cookbook 
Version 1.11 
Page 13 of 165 © EVE-NG LTD 
3 Installation 
3.1 VMware Workstation or VM Player 
3.1.1 VMware workstation EVE  VM installation using ISO image 
(preferred) 
Download EVE-NG Community ISO distribution image: 
https://www.eve-ng.net/downloads/eve-ng-2 
3.1.1.1 EVE VM Setup and Settings 
Step 1: Create a New Virtual machine 
 
Step 2: Select “I will install the operating 
system later”

EVE-NG Community Cookbook 
Version 1.11 
Page 14 of 165 © EVE-NG LTD 
Step 3: Select a Guest Operating system: 
Linux and select the version: Ubuntu 64-bit 
 
 
Step 4: Enter the name for your EVE-
COMM VM and select Location where your 
EVE VM will be stored on the host PC. 
 
 
Step 5: Type your desirable HDD size and 
select “Store virtual disk as single file”. 
 
Step 6: Press Customize Hardware

EVE-NG Community Cookbook 
Version 1.11 
Page 15 of 165 © EVE-NG LTD 
Step 7: Assign desirable memory 
 
Step 8: Set Processors “Number of 
processors” and “Number of cores per 
processor”. Set Intel VT-x/EPT Virtualization 
engine to ON (checked).  
NOTE: VMware Player will display only one 
CPU option: Number of processors. 
 
 
Step 9a: Select your desirable Network 
Adapter. Laptop PC 
NOTE: It is recommended to choose the 
NAT adapter option for Laptops to avoid 
EVE management interface IP changes. 
This can happen anytime the laptop is 
connected to a different SSID 
  
Step 9b: Select your desirable Network 
Adapter. Desktop PC 
NOTE: Desktop PC EVE management 
interface can be either NAT or Bridged to 
home LAN subnet.

EVE-NG Community Cookbook 
Version 1.11 
Page 16 of 165 © EVE-NG LTD 
 
Step 10: Select CD/DVD Option: “use ISO 
image file.” Browse to your downloaded 
EVE-Community.iso (actual name can be 
different) file 
 
Step 11: Confirm VM Settings. 
 
3.1.1.2 EVE-NG VM Installation steps 
 Mandatory Prerequisites: Internet must be reachable from your PC and VMware. EVE ISO 
installation requires internet access to get updates and install the latest EVE-PRO version from 
the EVE-NG repository. DNS must work as well, to check it, do a named ping, for example ping 
www.google.com 
EVE VM Installation from ISO has 3 Phases 
Phase 1 (Ubuntu installation) 
Step 1: Power ON EVE VM. Chose English 
and confirm with Enter. 
Step 2: Be sure that “Install EVE VM” is 
highlighted. Confirm with Enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 17 of 165 © EVE-NG LTD 
  
 
Step 3: Make sure that English is selected 
and confirm with Enter. 
 
Step 4: You can select your own Location, 
or later, after management IP assignment, 
location will be set automatically. You can 
leave United States. Confirm with Enter. 
 
 
Step 5: DHCP ENABLED, EVEs hostname 
by default is eve-ng. You can change it if 
you wish. Using the Tab key select continue 
and confirm with Enter. Continue to Step 14 
Step 6: DHCP DISABLED/Static IP setup. If 
you have not enabled DHCP in the network, 
you must assign an IP address manually. 
Confirm Continue with Enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 18 of 165 © EVE-NG LTD 
  
 
Step 7: Confirm selection “Configure network 
manually” with Enter 
 
Step 8: Enter your desirable EVE 
management IP, using the Tab key select 
“Continue” and confirm with Enter 
 
 
Step 9: Enter your subnet mask, using the 
Tab key select “Continue” and confirm with 
Enter 
Step 10: Enter your Gateway IP, using the 
Tab key select “Continue” and confirm with 
Enter

EVE-NG Community Cookbook 
Version 1.11 
Page 19 of 165 © EVE-NG LTD 
  
 
Step 11: IMPORTANT. The name server 
must be able to resolve public DNS entries 
and will be used during the next install 
steps. Enter your name server IP, using the 
Tab key select “Continue” and confirm with 
Enter 
 
Step 12: EVEs hostname by default is eve-
ng. It can be changed if you wish, using the 
Tab key select continue and confirm with 
Enter

EVE-NG Community Cookbook 
Version 1.11 
Page 20 of 165 © EVE-NG LTD 
Step 13: Enter your networks domain name. 
You are free to use anything you like, for 
example: eve-ng.net 
Using the Tab key select continue and 
confirm with Enter 
 
Step 14: If your DNS IP settings are correct, 
Ubuntu will detect your location automatically 
by conn ecting to Ubuntu servers . Confirm 
with Enter.  
 
 
Step 15: If you have a proxy in use for your 
internet access, enter your netwo rk proxy 
settings. If no proxy is used, select Continue 
with the Tab key and confirm with Enter. 
 
Step 16:  Select no automatic  updates and 
confirm with Enter. Security updates can 
later be run manually from EVE cli. 
 
EVE VM Installation Phase 2 (EVE installation) 
Step 17:  After the “Finish the installation”  
screen appeared, DO NOT remove CD ISO 
from the VM or hit Enter continue. First, we 
have to verify that EVE is ready for the 
installation phase 2. 
Step 18: Without powering off your EVE VM, 
open the  EVE VM settings and make sure  
that CD/DVD ISO “Device status connected” 
and “Connect at power on ” is checked. 
Confirm with OK.

EVE-NG Community Cookbook 
Version 1.11 
Page 21 of 165 © EVE-NG LTD 
  
 
Step 19: Return to the EVE console screen 
and continue with Enter , the EVE VM will 
reboot and finish the installation phase 2 
 
Step 20: Once the EVE login screen appears, 
login to the  CLI with root/eve and continue 
with installation phase 3 
 
EVE VM Installation Phase 3 (Management IP setup and updates) 
Step 21: Setup EVEs Management IP 
address. A Static IP address setup is 
preferred.  
Follow steps in section: 
3.5.1 for static IP, 3.5.2 for DHCP IP 
Step 22:  After your EVE is rebooted,  
Login to EVE CLI and type:  
apt update 
apt upgrade 
 
 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12

EVE-NG Community Cookbook 
Version 1.11 
Page 22 of 165 © EVE-NG LTD 
3.1.2 VMware workstation OVF deployment 
Download EVE-NG Community OVF image zip file, place it in the dedicated HDD storage for 
EVE VM and unzip it: 
https://www.eve-ng.net/index.php/download/#DL-COMM 
3.1.2.1 Deployment and VM machine settings 
Step 1: VMware workstation or VM Player, 
Menu File/Open 
 
Step 2:  Browse your downloaded and 
unzipped EVE -COMM, EVE -COMM-VM.ovf, 
followed by Open 
 
 
Step 3: Browse your desired EVE VM store 
destination followed by Import 
 
Step 4: Open your EVE VM Settings and set 
the desired RAM.

EVE-NG Community Cookbook 
Version 1.11 
Page 23 of 165 © EVE-NG LTD 
Step 5: IMPORTANT Set CPU Number of 
Cores and number of cores per processor. 
Set Intel VT-x/EPT Virtualization engine to 
ON (checked).  
NOTE: VMware Player will display only one 
CPU option: Number of processors.  
 
Step 6: Laptop PC Select your desirable 
Network Adapter. 
NOTE: It is recommended to choose the 
NAT adapter option for Laptops to avoid 
EVE management interface IP changes. 
This can happen anytime the laptop is 
connected to a different SSID. 
 
 
Step 7: Desktop PC Select your desirable 
Network Adapter.  
NOTE: Desktop PC EVE management 
interface can be either NAT or Bridged to 
home LAN subnet. 
 
Step 8: Power ON your EVE VM and follow 
Management IP setup instructions described 
in section 3.5.1 for Static IP or 3.5.2 for 
DHCP IP.

EVE-NG Community Cookbook 
Version 1.11 
Page 24 of 165 © EVE-NG LTD 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12 
3.1.2.2 OVF VM update to the latest EVE version 
Step 9: Make sure if your EVE OVF VM is up to date to the newest EVE version. 
Follow the steps described in section 4. 
 
3.1.2.3 OVF VM HDD Size expansion 
 IMPORTANT NOTE:  DO NOT expand the current EVE OV F HDD. To expand your EVE 
system size, please follow Troubleshooting section 11.2 
3.2 VMware ESXi 
3.2.1 VMware ESXi EVE installation using ISO image (preferred) 
Download EVE-NG Community ISO installation image: 
http://www.eve-ng.net/downloads/eve-ng-2 
3.2.1.1 EVE-NG ESXi VM Setup and Settings 
Step 1: Upload EVE ISO image to the ESXi 
store. 
 
Step 2: Create NEW VM

EVE-NG Community Cookbook 
Version 1.11 
Page 25 of 165 © EVE-NG LTD 
Step 3: Enter the name for your EVE-PRO 
VM and select Guest Operating system 
Linux and version: Ubuntu 64-bit 
 
Step 4: Select Location where your EVE VM 
will be stored in HDD. 
 
 
Step 5: IMPORTANT Customize your EVE 
VM CPU Settings. Set CPU Number of 
Cores and number of cores per processor. 
Set Intel VT-x/EPT Virtualization to ON 
(checked). 
 
Step 6: Assig desirable RAM for your EVE 
 
 
Step 7: Set the size of HDD for your new 
EVE VM. It is recommended to set “Thick 
Provisioned eagerly provisioned”. Server 
EVE HDD is recommended to set at least 
500Gb 
Step 8: Set your Management network. 
Adapter type VMXNET3

EVE-NG Community Cookbook 
Version 1.11 
Page 26 of 165 © EVE-NG LTD 
 
NOTE: Additional Network A dapters c an be 
added for further use. 
 
Step 9:  Add new device to your EVE VM, 
CD/DVD 
 
Step 10:  Set DVD drive to “Datastore ISO 
File” and browse your uploaded EVE -
PRO.iso. Make sure that Status is checked 
ON, “Connect at power on” 
 
3.2.1.2 EVE-NG ESXi VM Installation steps 
 Mandatory Prerequisites: Internet must be reachable from your PC and VMware. EVE 
ISO installation requires internet access to get updates and install the latest EVE-PRO 
version from the EVE-NG repository. DNS must work as well, to check it, do  a named 
ping, for example ping www.google.com  
EVE ESXi VM Installation from ISO has 3 Phases 
Phase 1 (Ubuntu installation) 
Step 1: Power ON EVE VM. Chose English 
and confirm with Enter. 
Step 2: Be sure if “Install EVE VM” is 
highlighted. Confirm with Enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 27 of 165 © EVE-NG LTD 
  
 
Step 3: Make sure if English is selected and 
confirm with Enter. 
 
Step 4: You can select your own Location, 
or later, after management IP assignment, 
location will be set automatically. You can 
leave United States. Confirm with Enter 
 
 
Step 5: DHCP ENABLED, EVEs hostname 
by default is eve-ng. You can change it if 
you wish. Using the Tab key select continue 
and confirm with Enter. Continue to Step 14 
Step 6: DHCP DISABLED/Static IP setup. If 
you have not enabled DHCP in the network, 
you must assign an IP address manually. 
Confirm Continue with Enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 28 of 165 © EVE-NG LTD 
  
 
Step 7: Confirm selection “Configure network 
manually” with Enter 
 
Step 8: Enter your desirable EVE 
management IP, using the Tab key select 
“Continue” and confirm with Enter 
 
 
Step 9: Correct your subnet mask, using the 
Tab key select “Continue” and confirm with 
Step 10: Correct your Gateway IP, using 
the Tab key select “Continue” and confirm

EVE-NG Community Cookbook 
Version 1.11 
Page 29 of 165 © EVE-NG LTD 
Enter 
 
with Enter
 
 
Step 11: IMPORTANT. Name server must 
respond to the Internet and will be used 
during the next install steps. Enter your 
name server IP.  Using the Tab key select 
“Continue” and confirm with Enter 
 
Step 12: EVE hostname by default is eve-
ng. It can be changed if you wish. Using the 
Tab key select continue and confirm with 
Enter 
 
 
Step 13: Enter your network domain name. 
You are free to use any, for example:  
eve-ng.net 
Step 14: If your DNS IP settings are correct, 
Ubuntu will detect your location from 
Internet. Confirm with Enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 30 of 165 © EVE-NG LTD 
Using the Tab key select continue and 
confirm with Enter 
 
 
 
Step 15:  If you have proxy in use for your 
internet, assign your network proxy settings. 
If no proxy in use, with Tab key select 
Continue and confirm with Enter. 
 
Step 16:  Select no automatic updates and 
confirm with Enter. Security updates ca n be 
run later manually from EVE cli. 
 
EVE VM Installation Phase 2 (EVE installation) 
Step 17:  After the “Finish the installation” 
screen appears, DO NOT remove CD ISO 
from VM or hit Enter continue. We have to 
verify settings for EVE installation Phase 2 . 
Follow step 9. 
Step 18: Without powering off the EVE VM, 
open the  EVE VM settings and make sure 
that CD/DVD ISO “Device status connected” 
and “Connect at power on” is checked. 
Confirm with OK.

EVE-NG Community Cookbook 
Version 1.11 
Page 31 of 165 © EVE-NG LTD 
 
 
 
Step 19: Return back to EVE console screen 
and confirm Continue with Enter , EVE VM 
will reboot and continue Phase 2 installation 
 
Step 20:  Once EVE login screen appeared, 
login in CLI with root/eve and follow 
installation Phase 3 
 
EVE VM Installation Phase 3 (Management IP setup and updates) 
Step 21:  Setup EVE Management IP 
address. A Static IP address setup is 
preferred  
Follow steps in section : 
3.5.1 for static IP, 3.5.2 for DHCP IP 
Step 22:  After your EVE is rebooted,  
Login to EVE CLI and type:  
apt update 
apt upgrade

EVE-NG Community Cookbook 
Version 1.11 
Page 32 of 165 © EVE-NG LTD 
Step 2 3:  On the EVE CLI prompt, reboot 
EVE by typing  
reboot 
 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12 
3.2.2 VMware ESXi OVF deployment 
Download EVE-NG Community OVF image zip file, place it in the dedicated HDD storage for 
EVE VM and unzip it: 
https://www.eve-ng.net/index.php/download/#DL-COMM 
3.2.2.1 ESXi OVF VM Setup and Settings 
Step 1: ESXi Host, Create/Register VM 
 
Step 2: Set option Deploy a virtual machine 
from an OVF or OVA file 
 
 
Step 3: Type the name for your new EVE 
VM and browse to select your all 
downloaded EVE OVF files 
Step 4: Select the storage where your EVE 
VM will be deployed.

EVE-NG Community Cookbook 
Version 1.11 
Page 33 of 165 © EVE-NG LTD 
  
 
Step 5: Select your Management network 
and Thick Disk provisioning. EVE OVF 
HDD is only 40Gb large. It is recommended 
after installation to add extra HDD. Section 
11.2 
 
Step 6:  IMPORTANT Open VM Settings. 
Set the quantity of CPUs and number of 
cores per socket. Set Intel VT-x/EPT 
Hardware Virtualization engine to ON 
(checked).  
 
 
Step 7: Set desirable RAM for your EVE. 
 
Step 8: Power ON your EVE VM and follow 
Management IP setup instructions described 
in section 3.5.1 for Static IP or 3.5.2 for 
DHCP IP.

EVE-NG Community Cookbook 
Version 1.11 
Page 34 of 165 © EVE-NG LTD 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12 
3.2.2.2 ESXi OVF VM update to the latest EVE version 
Make sure that your EVE OVF VM is up to date with the newest EVE version. 
Follow the steps described in section 4 for upgrade instructions 
 
3.2.2.3 ESXi OVF VM HDD Size expansion 
 NOTE: IMPORTANT! DO NOT expand the current EVE OVF HDD. To expand your EVEs 
system disk size, please follow the troubleshooting section 11.2 
3.3 Bare hardware server EVE installation 
Download Ubuntu Server 16.04.6 LTS ISO image: 
http://tw.archive.ubuntu.com/ubuntu-cd/16.04/ubuntu-16.04.6-server-amd64.iso 
 
 Mandatory Prerequisites: Internet must be reachable from your PC and VMware. EVE ISO 
installation requires internet access to get updates and install the latest EVE-COMM version 
from the EVE-NG repository. DNS must work as well, to check it, do a named ping, for example 
ping www.google.com 
3.3.1 Ubuntu Server Installation Phase 1 
Step 1: Create a bootable DVD disk or USB 
flash drive with an Ubuntu server image. 
Boot your server from ISO . Make sure that 
English is selected, Confirm with Enter 
 
 
Step 2:  Select the first Option “Install Ubuntu 
Server” Confirm with Enter 
 
 
 
Step 3:  Make sure that English is selected 
and confirm with Enter 
Step 4: You can select your own Location, 
or later, after management IP assignment,

EVE-NG Community Cookbook 
Version 1.11 
Page 35 of 165 © EVE-NG LTD 
 
location will be set automatically. You can 
leave United States. Confirm with Enter. 
 
 
Step 5: Configure the keyboard, leave “No” 
selected and confirm with enter 
 
Step 6:  Leave English (US)  as selection , 
confirm with Enter 
 
 
Step 7: Leave English (US) as selection and 
confirm with Enter 
 
Step 8: Select your management network 
adapter and confirm with Enter

EVE-NG Community Cookbook 
Version 1.11 
Page 36 of 165 © EVE-NG LTD 
Step 9: DHCP ENABLED 
Continue with Step 16 
 
Step 10: DHCP DISABLED/Static IP setup. 
If have not enabled DHCP in the network, 
you must assign an IP address manually. 
Continue with Enter. 
 
 
 
Step 11: Select “Configure network 
manually” and confirm with Enter
 
Step 12: Enter your desirable EVE 
management IP, using the Tab key select 
“Continue” and confirm with Enter
 
 
 
Step 13: Enter your subnet mask, using the 
Tab key select “Continue” and confirm with 
Enter 
Step 14:  Enter your Gateway IP, using the 
Tab key select “Continue” and confirm with 
Enter

EVE-NG Community Cookbook 
Version 1.11 
Page 37 of 165 © EVE-NG LTD 
  
 
 
Step 15: IMPORTANT: The name server 
must be able to resolve public DNS entries 
and will be used during the next install 
steps. Enter your name server IP,  using the 
Tab key select “Continue” and confirm with 
Enter 
 
 
Step 16:  Type your EVE server hostname, 
Example: eve-ng 
 
 
Step 17: Type your domain name. You are 
free to use any. Example: eve-ng.net 
 
Step 18: Type your Ubuntu username, 
Example: user

EVE-NG Community Cookbook 
Version 1.11 
Page 38 of 165 © EVE-NG LTD 
 
Step 19: Select a username (e.g. “user”) for  
your account and Continue 
 
Step 20: Enter a password for your new 
user
 
 
 
 
Step 21:  Re-enter your password and 
continue 
 
Step 22: If you want to use a weak password, 
click “Yes” on this screen. 
 
 
Step 23: Encrypt your Home directory, “No” 
 
Step 24: If your DNS and internet are 
working properly, Ubuntu will automatically 
detect your location and timezone. Confirm 
your timezone and continue with enter

EVE-NG Community Cookbook 
Version 1.11 
Page 39 of 165 © EVE-NG LTD 
 
 
Step 25: Select HDD partitioning method 
“Guided – use entire disk and set up LVM” 
 
Step 26: Select your disk partition, and 
confirm with enter
 
 
Step 27: Confirm write changes to disk with 
“Yes” and hit enter to continue 
 
Step 28: Select the volume size and continue 
 
 
Step 29: Confirm write the changes to disk 
with “Yes” and continue 
Step 30: If you have a proxy in use for your 
internet, enter your network proxy settings. If 
no proxy i s used, use the tab key to select 
Continue and confirm with enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 40 of 165 © EVE-NG LTD 
  
 
Step 31: Select “No automatic updates” and 
Continue 
 
Step 32:  Using the Arrow keys select 
“OpenSSH server” for installation  and 
confirm with the Space key (*), continue with 
enter 
 
 
Step 33: Confirm “Install the GRUB 
bootloader to the master boot record” with 
“Yes” and continue with enter 
 
Step 34: REMOVE CD/DVD installation 
media and continue with enter

EVE-NG Community Cookbook 
Version 1.11 
Page 41 of 165 © EVE-NG LTD 
Step 35: Login in to your Ubuntu with the 
username created above (user/Test123 was 
the example)
 
 
Step 36:  Continue as root user. Enter the 
commands below, each followed by the enter 
key. 
 
sudo su 
 
Test123 
 
cd 
 
 
 
 
 
Step 37: Create root password 
 
sudo passwd root 
 
Repeat your desirable password twice; 
Example: eve 
 
 
 
Step 38:  Verify and set your hostname if you 
haven’t set it before 
 
nano /etc/hostname 
 
Edit it if necessary: eve-ng 
 
Confirm edit with ctrl+o followed by Enter 
And ctrl+x for Exit 
 
Step 39: Verify your host settings 
 
nano /etc/hosts 
 
Your assigned static IP will be bound to your 
server hostname and domain 
 
NOTE: in case if DHCP IP address is used, 
you will see 127.0.0.1 IP vs hostname 
 
Confirm edit with ctrl+o followed by enter 
And ctrl+x for Exit 
Step 40:  Edit permissions for root user to 
allow SSH access to EVE server 
 
nano /etc/ssh/sshd_config 
 
Find and edit PermitRootLogin to “yes” 
 
Confirm edit with ctrl+o followed by enter 
And ctrl+x for Exit 
 
Restart ssh service: 
 
sudo service ssh restart 
 
Step 41:  IMPORTANT 
SSH as root to your EVE server with Putty or any other telnet client program. 
Update the Ubuntu grub CMD Line with the following customized command. Make sure you 
enter this command below in a single line and confirm it with the enter key.

EVE-NG Community Cookbook 
Version 1.11 
Page 42 of 165 © EVE-NG LTD 
sed -i -e 's/GRUB_CMDLINE_LINUX_DEFAULT =.*/GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0 
noquiet"/' /etc/default/grub 
 
Update GRUB, Followed by Enter 
 
update-grub 
 
 WARNING: DO NOT REBOOT your Ubuntu/EVE yet, proceed to step 42! 
 
Step 42: IMPORTANT 
Rename your Server interface name to eth0 
 
nano /etc/network/interfaces 
 
Before edit: 
 
After edit: 
 
 
Confirm your edit with ctrl+o followed by enter 
And ctrl+x to exit 
 
Reboot the EVE server 
 
reboot 
 
3.3.2 EVE Community Installation Phase 2 
Step 43: Start EVE Community installation with the following one-line command and hit enter 
 
wget -O - http://www.eve-ng.net/repo/install-eve.sh | bash -i 
 
Step 44: Reboot EVE 
reboot 
3.3.3 EVE Community Installation Phase 3 
 
Step 45: After the installation is completed, 
reboot EVE and follow the Management IP 
setup instructions in section 3.5.1. It is 
strongly recommended for bare-metal 
Step 46:  After your EVE is rebooted,

EVE-NG Community Cookbook 
Version 1.11 
Page 43 of 165 © EVE-NG LTD 
installations to use a static IP address. After 
the IP address setup, continue with Step 46 
 
Login to the EVE CLI and type:  
apt update 
apt upgrade 
reboot 
 
 
 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer section 12 
3.4 Google Cloud Platform 
3.4.1 Google account 
Step 1: Connect to Google Cloud Platform (GCP 
https://console.cloud.google.com/getting-started 
 
 
 
Step 2: Sign into GCP. Create a new GCP account if you do not already have one.  
3.4.2 Goggle Cloud project 
Create new project. By default, GCP will offer you a project named “My First Project”. It can be 
used as well. 
 
Step 1. GCP top bar, click on “My First Project” 
 
 
 
Step 2. Next pop up window, click “NEW PROJECT”

EVE-NG Community Cookbook 
Version 1.11 
Page 44 of 165 © EVE-NG LTD 
Step 3. Enter your project name, and confirm “CREATE” 
 
This will take some time.  
 
Step 4. Navigate: Navigation Menu/Compute Engine/VM Instances 
 
 
 
Step 5. Navigate: top bar and select your newly created Project

EVE-NG Community Cookbook 
Version 1.11 
Page 45 of 165 © EVE-NG LTD 
 
Preparation of your Project can take some time. Wait until the VM Instance window finishes 
deployment and then press the “Create button.” 
 
3.4.3 Preparing Ubuntu boot disk template 
Step 1: Open the google cloud shell and press: “START CLOUD SHELL”

EVE-NG Community Cookbook 
Version 1.11 
Page 46 of 165 © EVE-NG LTD 
Step 2: create a nested Ubuntu 16.04 image model. Copy and paste the below command into 
the shell. Use copy/paste. crtl +c/ctrl +v. It is single line command. Confirm with “enter”: 
 
gcloud compute images create nested-ubuntu-xenial --source-image-
project=ubuntu-os-cloud --source-image-family=ubuntu-1604-lts --
licenses="https://www.google.com/compute/v1/projects/vm-
options/global/licenses/enable-vmx" 
 
 
You will get the following output when your image is ready: 
 
 
3.4.4 Creating VM 
Step 1: Navigate: Navigation Menu/Compute Engine/VM Instances and press “Create” 
 
 
 
 
Step 2: Assign the name for your VM 
 
Step 3: Set your own region and zone 
 
Step 4: Edit your Machine Configuration. General-Purpose. Choose the series of CPU platform, 
Preferred are Intel CPUs Skylake or Cascade. 
 
Step 5: Choose your desirable CPU and RAM settings. 
IMPORTANT: “Deploy a container image” must be UNCHECKED.

EVE-NG Community Cookbook 
Version 1.11 
Page 47 of 165 © EVE-NG LTD 
 
 
 
Step 6: Select Boot disk. Press Change 
 
  
 
Step 7. Select Custom images and the custom boot images you created previously. Choose 
HDD disk type and size. HDD size can vary depends of your needs.

EVE-NG Community Cookbook 
Version 1.11 
Page 48 of 165 © EVE-NG LTD 
 
 
 
Step 7: Allow http traffic and create VM 
 
3.4.5 EVE-NG-Community installation 
Step 1: Connect to the VM with the first option “Open in browser window”

EVE-NG Community Cookbook 
Version 1.11 
Page 49 of 165 © EVE-NG LTD 
 
 
Step 2: Launch installation with:  
 
Type the below command to become root: 
sudo -i 
 
Start EVE-COMM installation 
wget -O - http://www.eve-ng.net/repo/install-eve.sh | bash -i 
 
 
Step 3:  Update and upgrade your new EVE-COMM 
apt update 
 
apt upgrade 
Confirm with Y 
 
Step 4. Reboot EVE. Allow some time for reboot and then press “Reconnect”  
 
 
Step 5: IMPORTANT: Setup IP 
Once the IP wizard screen appears, press ctrl +c and type the below command to become root: 
sudo -i 
 
 
 
Now follow the IP setup wizard.

EVE-NG Community Cookbook 
Version 1.11 
Page 50 of 165 © EVE-NG LTD 
IMPORTANT: set IP as DHCP! 
 
Step 6: Dockers installation. After EVE is rebooted, reconnect the SSH session:  
 
Type command to become root: 
sudo -i 
 
Type command to update EVE 
apt update 
 
 
3.4.6 Access to Google Cloud EVE-COMM 
Use your public IP for accessing EVE via http. 
 
 
 
 
Default web login: admin/eve 
3.4.7 Optional: GCP Firewall rules for native console use 
Step 1: Navigate: Navigation menu/VPC Network/Firewall rules

EVE-NG Community Cookbook 
Version 1.11 
Page 51 of 165 © EVE-NG LTD 
 
 
Step 2: Create new firewall rule 
 
 
 
 
 
Step 3: Create an ingress FW rule; allow TCP ports 0-65535

EVE-NG Community Cookbook 
Version 1.11 
Page 52 of 165 © EVE-NG LTD 
 
Step 4: Create an egress FW rule; allow TCP ports 0-65535

EVE-NG Community Cookbook 
Version 1.11 
Page 53 of 165 © EVE-NG LTD 
 
Summary FW rules. 
 
 
3.5 EVE Management IP Address setup 
3.5.1 Management static IP address setup (preferred) 
The steps below will walk you through the network setup and a ssign a static management IP 
for EVE. 
Step 1: Log into the EVE CLI using the default 
login root/eve After login, type your preferred 
root password  for EVE , default is eve. 
Remember it for further use. Confirm with 
enter 
NOTE: Typed characters in the passwo rd 
field are not visible. 
Step 2:  Retype your root password again 
and confirm with enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 54 of 165 © EVE-NG LTD 
 
 
 
 
Step 3: Choose your EVE VMs hostname. By 
default, it is eve-ng. You can leave it as it is. 
Confirm with enter 
 
Step 4: Type your domain name for your 
EVE VM. By default, it is example.com. The 
default value can be used as well. 
Confirm with enter
 
 
Step 5: Using the arrow keys, select the 
option “static”, confirm your selection with 
the space key, followed by enter 
 
Step 6: Type your desirable EVE 
management IP. Confirm with enter.

EVE-NG Community Cookbook 
Version 1.11 
Page 55 of 165 © EVE-NG LTD 
Step 7: Type the subnet mask of your EVE 
management network. Confirm with enter. 
 
Step 8: Type your networks gateway IP. 
Confirm with enter. 
 
 
Step 9: Type your networks primary DNS 
IP. Confirm with enter. 
IMPORTANT: DNS must be reachable and 
resolve public addresses. 
.  
Step 10: Type your network Secondary 
DNS IP. Confirm with Enter. 
IMPORTANT: DNS must be reachable and 
resolve public addresses. 
 
 
Step 11: Type your preferred NTP server IP. 
It can be left empty as well; in this case, your 
EVE VM will auto matically assign the time 
from its host. 
Step 12: If you have a proxy in use for your 
Internet, select the respective  proxy option 
and configure your proxy settings. By default, 
it is direct connection  (no proxy) . Confirm 
your selection with enter. EVE will reboot 
automatically.

EVE-NG Community Cookbook 
Version 1.11 
Page 56 of 165 © EVE-NG LTD 
  
 IMPORTANT NOTE: If you are setting up your management IP for the first time (fresh 
EVE installation), please return to the install section and complete installation phase 3. 
3.5.2 EVE Management IP address setup via DHCP 
The steps below will walk you through the network setup and assign a management IP for EVE 
via DHCP. 
Step 1: Log into the EVE CLI using the default 
login root/eve After login, type your preferred 
root password for EVE, default is eve. 
Remember it for further use. Confirm with 
enter 
NOTE: Typed characters in the password 
field are not visible. 
 
Step 2: Retype your root password again 
and confirm with enter. 
 
 
Step 3: Choose your EVE VMs hostname. By 
default, it is eve-ng. You can leave it as it is. 
Confirm with enter 
Step 4: Type your domain name for your 
EVE VM. By default, it is example.com. The 
default value can be used as well. 
Confirm with enter

EVE-NG Community Cookbook 
Version 1.11 
Page 57 of 165 © EVE-NG LTD 
  
 
Step 5: Using the arrow keys, select the 
option “dhcp”, confirm your selection with the 
space key, followed by enter 
 
Step 6: Type your preferred NTP server IP. 
It can be left empty as well; in this case, your 
EVE VM will automatically assign the time 
from its host. 
 
 
Step 7: If you have a proxy in use for your 
Internet, select the  respective proxy option 
and configure your proxy settings. By default, 
it is direct connection (no proxy). Confirm 
your selection with enter. EVE will reboot 
automatically.

EVE-NG Community Cookbook 
Version 1.11 
Page 58 of 165 © EVE-NG LTD 
 IMPORTANT NOTE: If you are setting up your management IP for the first time (fresh 
EVE installation), please return to the install section and complete installation phase 3. 
3.5.3 EVE Management IP address reset 
If for any reason you need to change these settings after the installation, you can rerun the IP 
setup wizard. Type the following command in the CLI and hit enter: 
rm -f /opt/ovf/.configured 
Then reboot. Once you log  into the CLI again, EVE will go t hrough the network setup again . 
Please follow the steps in section 3.5.1 for Static IP or 3.5.2 for DHCP IP. 
3.6 EVE-NG Community upgrade to EVE-NG Professional 
3.6.1 Mandatory Prerequisites  
 Mandatory Prerequisites: Internet must be reachable from your PC and VMware. EVE 
ISO installation requires internet access to get updates and install the latest EVE-PRO 
version from the EVE-NG repository. DNS must work as well, to check it, do a named 
ping, for example ping www.google.com  
3.6.1.1 EVE Community disk space 
You must have enough HDD space available. The main eve--ng--vg-root partition must have 
at least 10GByte free space while the boot partition must have at least 50Mbyte. To check how 
much space is available on your HDD, enter the following command into the CLI of EVE: 
df -h 
 
 
To free up space on the /boot, enter the following command, hit enter and confirm with “y” 
apt autoremove 
3.6.1.2 Verify current EVE Community version 
You have to make sure that your EVE Community Edition is of version (v2.0.3-86) or later. You 
must be able to reach the internet from your PC, VMware or Server.  
 
To check your current EVE-NG version, enter the following command

EVE-NG Community Cookbook 
Version 1.11 
Page 59 of 165 © EVE-NG LTD 
dpkg -l eve-ng 
 
You can also verify your current EVE version from the WEB GUI.  Top menu bar,  System, 
System status. 
 
You can check the version number of the newest currently available Community version on the 
EVE-NG Community site: http://www.eve-ng.net/community. 
3.6.1.3 Steps to upgrade to the latest EVE Community version 
Type the following commands below and hit enter after each. 
 apt update 
In case of any Y/N prompt, answer Yes. 
apt upgrade 
In case of any Y/N prompt, answer Yes. 
reboot 
3.6.2 Upgrading EVE Community to EVE-NG Professional 
 WARNING: Please be ready to purchase a license when upgrading, as you will not be 
able to start any nodes until a valid license has been activated on your EVE. 
To upgrade to EVE -NG Pro, issue the following commands into the CLI of EVE follow ed by 
enter. 
apt update

EVE-NG Community Cookbook 
Version 1.11 
Page 60 of 165 © EVE-NG LTD 
apt install eve-ng-pro 
 
reboot 
 
After the reboot continue with the below commands, followed by enter 
apt update 
 
apt install eve-ng-dockers 
 
reboot 
 
Continue to the EVE-NG Pro license purchase section of the website and follow the remaining 
instructions. 
3.7 Native telnet console management setup 
If you prefer to use a natively installed telnet client to manage nodes inside EVE, follow the 
steps below: 
3.7.1 Windows Native Console 
Step 1: Download the EVE Windows Client 
integration pack: 
http://www.eve-ng.net/downloads/windows-
client-side-pack 
 
Step 2: Install it as administrator 
 
Step 3: Leave the option for UltraVNC 
checked. UltraVNC is very tiny and the 
preferred VNC client for Windows by EVE. 
 
Step 4: Continue with Next. When it ask s to 
choose Ultra VNC Options, only leave the 
UltraVNC Viewer  checked, the rest is not 
needed.

EVE-NG Community Cookbook 
Version 1.11 
Page 61 of 165 © EVE-NG LTD 
Step 5: Continue with Next and finish the 
installation. 
 
By default, EVE Windows Client Integration will install Putty as your Telnet Client. The default 
location for the  EVE Win dows Client Integration  software and .re g files is: “C:\Program 
Files\EVE-NG” 
Set the default telnet program manually in Windows 10. Example: SecureCRT 
Step 1: Go to: Windows Settings/Apps/Default Apps/Choose Default Apps by Protocol  
Step 2: Set your default Telnet program: 
 
 NOTE: The first time click on the type of link that is used to access a running node 
inside EVE via telnet, the  browser will ask to choose the telnet program. If you ha ve 
prepared your default telnet program with the instructions above, you have to choose 
your default Telnet program. 
Example: Firefox browser: 
 
Set your default application, check the box “Remember my choice telnet links” and click Open 
link 
3.7.2 Linux Native Console 
The steps below will show how to setup the native consoles pack for Linux Mint 18 (Ubuntu): 
Step 1: Go to the EVE Linux Side 
integration pack download page: 
http://www.eve-ng.net/downloads/linux-
client-side 
Step 2: Open the link to GitHub 
https://github.com/SmartFinn/eve-ng-
integration

EVE-NG Community Cookbook 
Version 1.11 
Page 62 of 165 © EVE-NG LTD 
Step 3: Scroll down to the installation part 
 
 
 
Step 4: Login as root to your Linux system and enter the commands below: 
NOTE: An internet connection is required. Enter each command line below one after the 
other 
sudo add-apt-repository ppa:smartfinn/eve-ng-integration 
 
sudo apt-get update 
 
sudo apt-get install eve-ng-integration 
 For other Linux native console setup options please refer to: 
https://github.com/SmartFinn/eve-ng-integration 
 
3.7.3 MAC OSX Native Console 
Telnet Protocol: 
OSX Sierra (and older releases) is ready to use for the telnet protocol.  
 
 
 
For High Sierra, a telnet binary must be added (Apple decided to remove it and it is not present 
anymore on the latest OSX releases).

EVE-NG Community Cookbook 
Version 1.11 
Page 63 of 165 © EVE-NG LTD 
 
Procedure to install a previous telnet binary: 
Download telnet and ftp binaries from eve:  
http://your_eve_ip/files/osx.zip  (to be updated) Please contact to EVE Live chat for this 
package. 
Step 1: Reboot the Mac and hold down the “Command” and “R” key simultaneously after you 
hear the start-up chime, this will boot OSX into Recovery Mode 
Step 2: When the “OSX Utilities” screen appears, pull down the ‘Utilities’ menu at the top of the 
screen instead, and choose “Terminal” 
Step 3: Type the following command into the terminal then hit enter: 
 
crutil disable; reboot 
 
Step 4: When the OSX reboot is done, extract the osx.zip to your home directory 
Step 5:  Copy the files to /usr/bin and set the permissions using the terminal utility:  
 
sudo –i 
 
cp telnet ftp /usr/bin ; chmod 555 /usr/bin/telnet; chmod 555 /usr/bin/ftp 
 
chown root:wheel /usr/bin/telnet /usr/bin/ftp

EVE-NG Community Cookbook 
Version 1.11 
Page 64 of 165 © EVE-NG LTD 
1. Reboot the Mac and hold down Command + R keys simultaneously after you hear 
the startup chime, this will boot OSX into Recovery Mode 
2. When the “OSX Utilities” screen appears, pull down the ‘Utilities’ menu at the top of 
the screen instead, and choose “Terminal” 
Type the following command into the terminal then hit enter: 
crutil enable; reboot 
 
VNC Protocol: 
Download Chicken of VNC at: https://sourceforge.net/projects/chicken/files/Chicken-
2.2b2.dmg/download  
Install and use it as default VNC Client 
RDP Protocol: 
Download and install the Microsoft Remote Desktop on the App Store: 
 
 
3.8 Login to the EVE WEB GUI 
Login to the EVE management UI: 
http://<your_eve_ip>/ 
Default user access:

EVE-NG Community Cookbook 
Version 1.11 
Page 65 of 165 © EVE-NG LTD 
User: admin 
Password: eve 
 NOTE: You can change your EVE WEB Admin passw ord, please refer to section  
6.3.1.2 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12

EVE-NG Community Cookbook 
Version 1.11 
Page 66 of 165 © EVE-NG LTD 
4 EVE-NG Community Update & Upgrade 
 Prerequisites: Internet access and working DNS on your EVE-NG is required. 
Verify your internet reachability with named ping. Example: ping www.google.com  
ping www.google.com 
 
If your ping is success, follow next step for update. If named ping has no success, please verify 
your DNS IP assigned for EVE or firewall. Some cases ping can be blocked by FW, but Internet 
and DNS are capable to make update/upgrade. 
OPTION for bare EVE installations which has bnx2x Broadcom Ethernet  drivers, please 
rewrite your driver to the newest linux-firmware: 
sudo apt-get -o Dpkg::Options::="--force-overwrite" install linux-firmware 
 IMPORTANT NOTE: before you start your EVE Community update & upgrade, please free 
up your EVE Community from older kernel packages: 
apt autoremove 
 
4.1 EVE-NG Community Update 
It is strongly recommended to keep your EVE-NG up to date. To update and upgrade, SSH to 
your EVE CLI.  
To verify your current EVE -NG version, please follow “ CLI diagnostic information display 
commands” in section 11.1.1. You can verify your current EVE version from the System/System 
Status tab on the top menu of the WEB GUI as well.

EVE-NG Community Cookbook 
Version 1.11 
Page 67 of 165 © EVE-NG LTD 
The newest version of EVE-NG can be verified by checking the official website: http://www.eve-
ng.net/community/community-2. The main page will display the latest EVE -NG version and 
correct steps to update. 
 
 
Type the below commands followed by Enter 
 apt update 
In case the prompt asks to confirm with Y/N, answer Yes. 
4.2 EVE-NG Community Upgrade 
Type commands followed by Enter 
 apt upgrade 
In case the prompt asks to confirm with Y/N, answer Yes. 
 IMPORTANT NOTE:  If you are upgrading EVE Community from older version, the 
installation may ask you to confirm additional! Information: 
 
Answer for prompt above is “N”

EVE-NG Community Cookbook 
Version 1.11 
Page 68 of 165 © EVE-NG LTD 
 
Answer for grub-pc version is: “Keep the local version currently installed” 
After the completion of the update and upgrade, reboot your EVE  Server. Type the following 
command and hit enter. 
reboot

EVE-NG Community Cookbook 
Version 1.11 
Page 69 of 165 © EVE-NG LTD 
5 Types of EVE management consoles 
 IMPORTANT NOTE: EVE Console TCP ports. EVE Community uses a static port 
range between 32678-40000.  
Formula is = 32768+128*POD+1 -> 32768+128*POD+128    POD: user id ( admin = 0 ) 
Exemple: you got admin (POD 0) + 2 users ( POD 1, POD 2 )  
32768+128*0+1(First port for POD0)   -> 32768+128*2+128(Last port of POD 2 ) = 32769 -> 
33152 
Port per user pod: 
POD First Port Last Port 
0 32769 32896 
1 32897 33024 
2 33025 33152 
3 33153 33280 
4 33281 33408 
5 33409 33536 
6 33537 33664 
7 33665 33792 
8 33793 33920 
9 33921 34048 
10 34049 34176 
EVE Community supports two different console types. 
5.1 Native console 
EVE Native console option requires locally installed software 
to access your lab nodes. To use the Native console option, 
you must have Administrator rights on your PC and ensure 
the TCP port range 32768-40000 is not blocked by a firewall 
or antivirus software. (See table above) 
 
 
 
5.1.1 Native Console: telnet 
Windows OS: You can use your preferred telnet program like Putty, SecureCRT or others. 
Example: Putty as native telnet client on Windows. 
To setup Windows native telnet client please follow section 3.7.1

EVE-NG Community Cookbook 
Version 1.11 
Page 70 of 165 © EVE-NG LTD 
 
 
Linux OS: You can use your preferred telnet program like the Native Terminal, SecureCRT, 
or others. 
Example: Telnet client from the native terminal on Linux Mint.  To setup Linux native telnet 
client please follow section 3.7.2 
 
MAC OSX: You can use your preferred telnet program like the native Terminal, SecureCRT, 
or others. 
Example: Telnet client from the native terminal on MAC OSX.  To setup MAC OSX native 
telnet client please follow section 3.7.3 
5.1.2 Native Console: Wireshark 
EVE Community has an integrated connection with natively installed Wireshark software on 
your PC. This allows live captures with Wireshark installed on the client machine. The EVE 
will capture natively installed Wireshark session. 
 
 IMPORTANT NOTE: Make sure you have installed Wireshark and EVE-NG client 
pack. It is strongly recommended if your Wireshark software is installed at your PC 
default location.

EVE-NG Community Cookbook 
Version 1.11 
Page 71 of 165 © EVE-NG LTD 
 
 IMPORTANT NOTE: The Wireshark wrapper located in your PC station must match 
your EVE root password.  Edit your EVE root password in the wireshark_wrapper.bat, 
if you had changed it during install. 
 
 
 
Example: Fortinet live interface port1 capture.

EVE-NG Community Cookbook 
Version 1.11 
Page 72 of 165 © EVE-NG LTD 
 
 
5.1.3 Native Console: VNC 
Windows OS: Recommended and tested is UltraVNC but any other compatible one can be 
used. 
Example: UltraVNC as Native VNC client on Windows.  To setup Windows native VNC client 
please follow section 3.7.1 
 
Linux OS: Remote Desktop Viewer for VNC Sessions. 
Example: Remote Desktop Viewer for VNC sessions on Linux Mint.  To setup Linux native 
Remote Desktop Viewer please follow section 3.7.2

EVE-NG Community Cookbook 
Version 1.11 
Page 73 of 165 © EVE-NG LTD 
 
MAC OSX: Preferred VNC program: Chicken VNC 
Example: Chicken VNC as Native VNC client on MAC OSX.  To setup MAC OSX native RDP 
Viewer client please follow section 3.7.3 
5.1.4 Native Console: RDP 
Windows OS: Windows Native RDP. 
Example: Windows RDP session to Win10 host in the lab.   
 
Linux OS: Remote Desktop Viewer as RDP session to lab Win10 host. 
Example: RDP session to Win10 host in the lab. To setup Linux native Remote Desktop 
Viewer please follow section 3.7.2

EVE-NG Community Cookbook 
Version 1.11 
Page 74 of 165 © EVE-NG LTD 
MAC OSX: Remote Desktop Viewer as RDP session to lab Win10 host. 
Example: RDP session to Win10 host in the lab.   
To setup MAC OSX native RDP Viewer client please follow section 3.7.3 
 
5.2 HTML5 console 
The EVE Community HTML5 console 
provides a clientless solution for 
managing labs and node sessions. 
Management is achieved directly through 
the browser by opening new browser 
window. It is very convenient for 
Corporate users with restricted 
Workstation permissions (Locked Telnet, 
vnc, rdp). 
 
 
 
 
 
5.2.1 HTML5 Console: Telnet 
HTML5 Telnet console opens telnet sessions in the new browser window.

EVE-NG Community Cookbook 
Version 1.11 
Page 75 of 165 © EVE-NG LTD 
5.2.2 HTML5 Console: VNC 
HTML5 VNC opens VNC sessions in the new browser window. 
 
5.2.3 HTML5 Console: RDP for Windows 
HTML5 RDP console opens RDP session s in the new browser window. For Windows  7, 
Windows Server 2008. 
During Windows machine image installation, you can allow RDP sessions to be used for access 
to Windows host. If your Windows host has enabled RDP session, edit windows node settings 
and set RDP console. Give time to boot this node and RDP session will opens in new browser 
tab.

EVE-NG Community Cookbook 
Version 1.11 
Page 76 of 165 © EVE-NG LTD

EVE-NG Community Cookbook 
Version 1.11 
Page 77 of 165 © EVE-NG LTD 
6 EVE WEB GUI Management 
6.1 EVE Management Page 
The Main EVE management window 
 
6.1.1 Management buttons 
 
Button Description 
 
Select All or Deselect All folders or labs in the EVE tree 
 
Create/Add new Lab 
 
Change selected item name. To use this option, please select the folder or lab 
that you want to rename. You must not rename the Shared folder, the Users 
folder or any folder inside the Users folder. 
 
Move selected item(s) to a different location. To use this option, please select 
the folder(s) or lab(s) that you want to move.

EVE-NG Community Cookbook 
Version 1.11 
Page 78 of 165 © EVE-NG LTD 
 
Delete selected folders or labs. You must not d elete the Shared folder, the 
Users folder or any folder inside the Users folder. 
 
Import an EVE lab or lab folder  from a previous export . Import file must be in 
.zip format 
 
Export EVE lab or folder. Select folder(s) and /or labs you wish to export and 
select this option. The export is saved to your local PC in .zip format and is 
ready to import to another EVE. 
 
Toggle the s orting folders and labs b etween alphabetical and last edit date  
(ascending/descending cannot be changed currently). 
 
Refresh current folder content 
6.1.2 Management tabs 
 
 
Tab Description 
 
Returns back to the EVE Home Management screen. 
 
 
 
Management dropdown, opening the management submenu. 
 
Management submenu, refer to sections: 6.3 
 
System dropdown. 
 
System submenu, refer to section 6.4

EVE-NG Community Cookbook 
Version 1.11 
Page 79 of 165 © EVE-NG LTD 
 
 
 
Information dropdown 
 
Information submenu, for details see section 6.5 
6.2 Folders and Lab files management 
This section will explain how to manage folders and labs on the EVE management page. 
6.2.1 Folders Management 
6.2.1.1 Create folder 
Type the new folder name and click “Add Folder” 
6.2.1.2 Delete folder 
Select the f older you wish to delete and press 
Delete.  
 NOTE: All folder content will be deleted as 
well. 
 
 
6.2.1.3 Move Folder 
Select the f older you wish to move and press the 
Move to button.

EVE-NG Community Cookbook 
Version 1.11 
Page 80 of 165 © EVE-NG LTD 
 
 
Type and select the target destination for your folder and 
confirm by clicking on Move. 
 
 
6.2.1.4 Export Folder 
Select the folder(s) you wish to export from your EVE and press Export.  
 
 
Save the exported file as .zip to your local PC. The exported zip file is ready to import to another 
EVE instance. 
 
If your browser is set to save downloaded files to a default directory, your exported file will be 
saved in the browsers default downloads directory. 
6.2.1.5 Import Folder 
 IMPORTANT: Importable file MUST be in .zip format, do NOT unzip the file. 
Step 1: Press the Import button.

EVE-NG Community Cookbook 
Version 1.11 
Page 81 of 165 © EVE-NG LTD 
 
Step 2: Choose the zipped file that contains EVE folders with labs. 
 
Step 3: Press the Upload Button 
 
Step 4: After you made sure your folder is imported and has all its content (labs), you can close 
the upload session. 
 
6.2.2 Lab files Management 
You can manage created labs from the main EVE file manager window

EVE-NG Community Cookbook 
Version 1.11 
Page 82 of 165 © EVE-NG LTD 
 
6.2.2.1 Create Lab 
Click on the New Lab button and refer to section 8.1 
 
6.2.2.2 Delete Lab 
Select the lab or labs you wish to delete and then press the Delete button 
 
6.2.2.3 Clone Lab 
The cloning feature provides a very convenient way to duplicate original labs to share with 
others or base another lab on it. 
Cloned labs will copy exported configs (on supported nodes ) but  will not copy saved 
states/configurations in Qemu nodes like Windows hosts, Cisco ISE, or other Qemu nodes that 
are not supported by the export config feature. Please refer to section 10.1 for more information 
on configuration export for labs. 
Step 1: Select the lab you wish to clone and move the mouse pointer (blue) to that lab, an extra 
option will appear. Click on Clone.

EVE-NG Community Cookbook 
Version 1.11 
Page 83 of 165 © EVE-NG LTD 
 
Step 2: Your lab will be cloned with all you r exported configurations or configuration sets with 
a new name. 
 
Step 3: The lab has been cloned lab and can be renamed to your liking. Move the mouse pointer 
to the cloned lab and choose Rename. 
 
Step 4: Rename it, and click OK to confirm 
 
6.2.2.4 Move Lab 
Step 1: Select the lab you wish to Move and move the mouse pointer (blue) to that lab, an extra 
option will appear. Choose Move to. 
 
Step 2:  Type the path to the new destination and 
confirm by clicking Move 
   
 
 
6.2.2.5 Export Lab 
Select the Lab(s) you wish to export from your EVE Server and press Export.

EVE-NG Community Cookbook 
Version 1.11 
Page 84 of 165 © EVE-NG LTD 
 
Save exported file as .zip to your local PC. The exported zip file is ready to import into another 
EVE. 
 
If your browser is set to save downloaded files to default directory, your exported file will be 
saved in the browsers default downloads directory. 
6.2.2.6 Import Labs 
 IMPORTANT: Importable file MUST be in .zip format, do NOT unzip the file.  
Step 1: Press the Import button. 
 
Step 2: Choose the zipped file which contains the EVE labs.

EVE-NG Community Cookbook 
Version 1.11 
Page 85 of 165 © EVE-NG LTD 
 
Step 3: Press the Upload Button 
 
Step 4: After you made sure your lab is imported, you can close the upload session. 
 
6.3 EVE Management Dropdown Menu 
6.3.1 EVE User management 
The User Management page, under the 
Management dropdown, will allow Admin 
accounts to manage other user accounts. 
 
6.3.1.1 Creating a new EVE User 
Step 1: Open the User management submenu. Management>User management and click Add 
user

EVE-NG Community Cookbook 
Version 1.11 
Page 86 of 165 © EVE-NG LTD 
 
Step 2: The Add New User management window will pop up. Fill in the main information about 
your EVE user 
 
Step 3: The POD number is a value assigned to user accounts automatically. POD number s 
are like user profiles inside of EVE and are a unique value for every user Think of PODs like a 
virtual rack of equipment for each user. Admins can assign a preferred number between 1-128. 
Please keep POD numbers unique between users! 
Step 4: Press ADD 
  
6.3.1.2 Edit EVE User 
Step 1: Open the User management submenu. Management -> User management and choose 
which user you want to edit.

EVE-NG Community Cookbook 
Version 1.11 
Page 87 of 165 © EVE-NG LTD 
Step 2:  The Edit user management window will pop up. Now you can edit necessa ry user 
information, roles , or access time . Confirm settings by pressing Edit at the bottom of the 
window. 
 
6.3.1.3 User monitoring 
There is a dropdown menu next to “Add User” called “More Info” that can provide additional 
information about your users. Click the checkbox next to the relevant information that you would 
like displayed. Additional columns will be added for each checkbox that is chosen. 
 
6.4 EVE System Dropdown menu 
 The EVE System dropdown contains the system utilization status, log 
files, and an option to stop all running nodes on the server.

EVE-NG Community Cookbook 
Version 1.11 
Page 88 of 165 © EVE-NG LTD 
6.4.1 System status 
The System Status page, under the System Dropdown, will show EVE 
server resource utilization, the number of  running nodes per template,  
current running versions of EVE and Qemu , and the current sta tus of the 
UKSM and CPU Limit options. 
 
UKSM – “Ultra KSM (kernel same-page merging) is a Linux kernel feature that allows the KVM 
hypervisor to share identical memory pages among different process or virtual machines on the 
same server.” It can be disabl ed globally for EVE on this page. It is recommended to keep 
UKSM enabled.  
CPU Limit – CPU limit is used to limit CPU 
overloads during the nodes run time. It acts 
like a smart CPU usage option. If a running 
node reaches 80% CPU u tilization, the CPU 
Limit feature throttles CPU use for this node 
to 50% until process usage drop s under 30% 
for a period of 1 minute. 
It is recommended to keep the Global CPU 
Limit option enabled. 
CPU Limit can be turned for individual nodes 
in a lab. EVE node templates are set , by 
default, with the recommended CPU limit 
settings. An Unchecked CPU Limit option 
means that this node will boot without CPU 
limit. 
Reference: 
https://searchservervirtualization.techtarget.com/definition/KSM-kernel-samepage-merging  
6.4.2 System logs 
The System logs page, under the System Dropdown, will display EVE 
server log information

EVE-NG Community Cookbook 
Version 1.11 
Page 89 of 165 © EVE-NG LTD 
 
In the menu you can select a specific log file for inspection. 
 
6.4.3 Stop All Nodes 
The Stop All Nodes option, under the System Dropdown, is an 
option that stops all running nodes on the EVE server.  This 
option is accessible only by Admin users. 
  
6.5 EVE Information Dropdown menu 
The Eve Information Dropdown contains links to  the EVE 
Website, EVE forum, EVE YouTube channel , and the web-
based EVE Live Help chat. 
To join the EVE Forum, in order to make posts or download 
materials, a forum user account must be created.  
To join the EVE Live Chat for support, please use your Google account for access, or create a 
new user account for this chat. Please note the forum and live chat use separate user accounts. 
6.6 Other Tab line info 
 
Other items on the top menu are: Real-time clock, a shortcut to edit the currently logged in user, 
and a sign-out button. 
6.7 Lab preview and global settings 
Once you click on a lab in the folder tree, a main window on the right side will display schematic 
content of the lab as well as lab management options like open, edit, and delete.

EVE-NG Community Cookbook 
Version 1.11 
Page 90 of 165 © EVE-NG LTD 
 
6.7.1 Lab preview window 
The lab preview window display s the schematic position of nodes and their connectivity . The 
Scale option allows you change the lab preview size. 
 
6.7.2 Lab preview buttons 
In the lab preview, these buttons allow you to manage the selected lab. 
Button Description 
 Opens the Lab to the Topology Canvas  
 Opens the Labs Global Settings. Refer to section 6.7.4 for more info. 
 Deletes the lab

EVE-NG Community Cookbook 
Version 1.11 
Page 91 of 165 © EVE-NG LTD 
6.7.3 Lab preview information 
Description, version, UUID etc. 
 
6.7.4 Lab Global Settings 
Lab Global Setting s Page is opened when you click on the Edit button below the Lab 
preview window or from the Topology page Side bar: 
 
This page allows you to fill out important information about the lab. The red numbers in the 
picture correlate with the numbers listed below 
1. Lab name. 
2. Version: Version numbers allow a lab author to assign a value to a unique state of a 
lab. Increase the number to correspond to new developments in the lab. If left unfilled, 
EVE will assign a value of 1 automatically. 
3. Author: You can add a lab author name in this field 
4. Config Script Timeout: It is the value in seconds used for the “Configuration Export” 
and “Boot from exported configs ” operations. Refer to section 10.3 for more 
information.

EVE-NG Community Cookbook 
Version 1.11 
Page 92 of 165 © EVE-NG LTD 
5. Description: In the Description field you can write a short description of the lab. 
6. Tasks: In the Tasks field you can write the task for your lab. 
The Lab details window can be opened from the Topology Canvas page 
sidebar during labbing, to read the Tasks for the lab.

EVE-NG Community Cookbook 
Version 1.11 
Page 93 of 165 © EVE-NG LTD 
7 EVE WEB Topology page 
Once you open a lab, the topology page for that lab will open. 
 
7.1 Side bar functions 
Move your mouse pointer over to the left on top of the minimized sidebar to expand the 
interactive sidebar as shown in below screenshot

EVE-NG Community Cookbook 
Version 1.11 
Page 94 of 165 © EVE-NG LTD 
7.1.1 Add an object 
The “Add an object” menu can be accessed in two different ways, from the sidebar and by right-
clicking on the Topology Page 
  
7.1.1.1 Node object 
The Node object opens the “Add a new node ” window. Only nodes that appear blue in the 
dropdown menu can be added. A grey image name signifies that you have not yet properly 
uploaded an image to the proper folder. A blue image name means that at least one i mage 
exists in the proper folder for this template.  
 
7.1.1.2 Network object 
The Network object opens the “Add a new network” window. This function is used to add any 
kind of network (Cloud, Bridge). For details on these, please refer to section 9

EVE-NG Community Cookbook 
Version 1.11 
Page 95 of 165 © EVE-NG LTD 
 
7.1.1.3 Picture object 
The picture object opens the “Add Picture” window and allows you to upload custom topologies 
in jpg or png format. After uploading, you can edit these pictures and map selected areas to 
nodes from the topology to use your own designs as a lab topology from which you can directly 
connect to the nodes. For details, refer to section 10.2 
 
7.1.1.4 Custom shape object 
The Custom shape object allows you to add shape elements onto the topology; these currently 
include squares, round squares and circles. For details, refer to section 10.1

EVE-NG Community Cookbook 
Version 1.11 
Page 96 of 165 © EVE-NG LTD 
7.1.1.5 Text object 
The Text object allows you to add Text elements onto the topology. For details, refer to section 
10.1.3 
 
7.1.2 Nodes 
The Nodes object in the sidebar opens the “Configured Nodes” window.  
 
In this window,  you can m ake changes for nodes that are on the lab topology. More options 
can be found in the detailed node specific menu, for details refer to section 8.1.2. 
 NOTE: Running nodes are highlighted in Blue, their settings c annot be changed. You 
can only change settings of nodes that are not currently running. 
You can change the following values: 
• Node Name 
• Boot image 
• Number of CPUs for the node 
• Enable or disable CPU Limit (Refer to section 6.4.1) 
• IDLE PC for Dynamips node 
• NVRAM in Kbyte 
• RAM in Mbyte 
• Ethernet quantity. NOTE: The Node must be disconnected from any other nodes to 
make this change. You cannot change the interface quantit y if the node is connected 
to any other node. 
• Serial interface quantity, IOL nodes only. You cannot change Serial interface quantity 
if the node is connected to any other node. 
• Type of Console 
• Node Icon that appears on the Topology 
• Startup configuration to boot from

EVE-NG Community Cookbook 
Version 1.11 
Page 97 of 165 © EVE-NG LTD 
Actions Buttons (Stopped node): 
 
• Start node 
• Stop node 
• Wipe node 
• Export the nodes config 
• Networks 
• Edit node 
• Delete Node 
 
Actions Buttons (Running node): 
 
• Console to the node 
• Stop node 
• Wipe node 
• Export the nodes config 
• Edit node 
• Delete Node 
 
7.1.3 Networks 
 The Networks object in the sidebar will open the “Configured 
Networks” window. 
The “Configured Networks” window will only show networks that were specifically added to the 
topology; it will not show node interconnections. The example below is showing information for 
networks on the Topology. For Cloud networks  and how to connect EVE labs to a network 
external to EVE, please refer to section 9

EVE-NG Community Cookbook 
Version 1.11 
Page 98 of 165 © EVE-NG LTD 
 
 
• Edit Network 
• Delete Network 
7.1.4 Startup-configs 
The Startup-configs object in the sidebar opens the “Startup-configs” 
window. 
This window will show you startup -config for each node and if the node is set to boot from it 
(ON) or not (OFF). 
 
7.1.5 Logical Maps 
NOTE: The Logical Maps object will only appear in the sidebar after 
you have uploaded a custom topology picture to the lab EV E lab 
(Please refer to section 7.1.1.3). The  Pictures object in the sidebar opens the “Picture 
Management” window.  
For details on the Picture / custom topology feature, refer to section 10.2

EVE-NG Community Cookbook 
Version 1.11 
Page 99 of 165 © EVE-NG LTD 
7.1.6 Configured Objects 
The “Configured Objects” window will display a list of all objects that 
are added onto the topology. For details on different objects, refer to 
section 10.1 
NOTE: You will not see any objects in this window if none have been added to the lab yet. 
 
7.1.7 More actions 
The More actions menu in the sidebar has a submenu with the following functions.  
 
7.1.7.1 Start all nodes 
The “Start all nodes” action will start all nodes on your topology, taking 
the (configurable) startup delay of each node into consideration.  
 IMPORTANT. Starting many nodes at once can seriously spike your CPU utilization. 
Please make sure that you are not using the “Start all nodes” option for heavy labs or 
that you have configured a proper de lay between the nodes. For heavy nodes and 
large quantities, it is recommended to start them in smaller groups, wait for them to 
finish booting and then start another small group of nodes. 
7.1.7.2 Stop all nodes 
  Stopping all nodes will power off all nodes on your topology. 
 NOTE: It is recommended to save your (running) config urations on the nodes in your 
lab before you stop the lab if you want to continue where you left off the next time. 
Stopping the nodes will leave the images in a temporary folder and will ta ke up space 
on your drive until they have been wiped.

EVE-NG Community Cookbook 
Version 1.11 
Page 100 of 165 © EVE-NG LTD 
7.1.7.3 Wipe all nodes 
 The “Wipe all nodes” action will wipe the NVRAM or currently saved 
image of all your nodes in the current lab. 
Example: You have saved the nodes configuration by saving the running conf iguration to the 
startup configuration. The Wipe command will delete the saved NVRAM startup configuration 
and on the next boot it will boot from factory defaults.  
The same applies to images without configurations, e.g. a linux node. If you make modifications 
to the system and afterwards wipe this node, the next time it will boot from the original base 
image again as the modified image was deleted. 
The “Wipe node” action is commonly used with initial  startup configuration modifications. The 
Wipe node actio n does not delete configured startup configurations or sets. Please refer to 
section 10.3 
7.1.7.4 Console to All Nodes 
“Console to all nodes ” will open a console to all of your running 
nodes in the current lab. This includes all diffe rent kinds of 
configured console types for lab nodes like VNC, Telnet and RDP. 
7.1.7.5 Export all CFGs 
The “Export all configurations” action will export current configs to 
the EVE startup-configs. 
Export configurations are supported for: 
Cisco Dynamips all nodes 
Cisco IOL (IOS on Linux) 
Cisco ASA 
Cisco ASAv 
Cisco CSR1000v 
Cisco Nexus 9K 
Cisco Nexus Titanium 
Cisco vIOS L3 
Cisco vIOS L2 
Cisco XRv 
Cisco XRv9K 
Juniper VRR 
Juniper VMX 
Juniper vMX-NG 
Juniper vQFX 
Juniper vSRX 
Juniper vSRX-NG 
Mikrotik 
PFsense FW 
Timos Alcatel 
vEOS Arista 
For a full explanation of exporting configurations, please refer to section 10.3 
7.1.7.6 Edit lab 
Opens the Edit lab window. Refer to section: 6.7.4

EVE-NG Community Cookbook 
Version 1.11 
Page 101 of 165 © EVE-NG LTD 
 
7.1.7.7 Set node’s startup-cfg to default configset 
Sets nodes to the d efault startup -config. NOTE: If you have 
nothing saved in the default config set for any node, that node 
will boot from factory default instead. This is commonly used with the wipe nodes function so 
the node will boot from the  configured startup-config on next boot and not from the startup -
config in its NVRAM in case the node was started before already. 
Please refer to section 10.3 
7.1.7.8 Set node’s startup-cfg to none 
Setting all lab nodes to boot from f actory default. Used 
commonly with the wipe nodes function. The example below 
shows the steps to set a lab to boot from factory default. 
Step 1: Wipe all nodes 
Step 2: Set all nodes to startup-cfg none 
 
Please refer to section 10.3 
7.1.7.9 Delete default startup-cfgs 
 
 WARNING: this action will delete all configurations saved to your saved default config 
set. Please make sure that is what you want to do before you execute this.  
7.1.8 Refresh Topology 
Sometimes it is necessary to refresh the topology if many objects are 
added on the topology. 
7.1.9 Lab page zoom/unzoom 
This action is used to zoom or unzoom a large topology in EVE.

EVE-NG Community Cookbook 
Version 1.11 
Page 102 of 165 © EVE-NG LTD 
 
7.1.10 Status 
Opens the EVE Status window.  
Especially useful while working with labs to monitor your EVE ’s resource utiliz ation. It shows 
EVEs CPU, RAM and disk utilization in real time. You can also see the number of running 
nodes per node type. For details on UKSM and CPU Limit, please refer to section 6.4.1 
 
7.1.11 Lab details 
Lab details display information about a lab, its UUID, description and 
lab tasks. To edit the lab description and lab tasks, please refer to 
section 6.7.4 and 7.1.7.6

EVE-NG Community Cookbook 
Version 1.11 
Page 103 of 165 © EVE-NG LTD 
7.1.12 Lock Lab with password 
“Lock Lab” disables some of the functions on the lab topology. If the lab is locked, you cannot 
move any node or object nor edit any node settings. Basically, the whole lab will be in read -
only mode except for the lab settings itself , which you can st ill edit as Administrator from the 
main menu.  
 
Lab is unlocked and all ope rations are 
working 
 
Enter and confirm your lab lock password 
To unlock a Lab, simply press on the red “Unlock Lab” button with an Administrator account. 
 
Lab is locked and all operations are 
restricted 
 
Enter lab unlock password to unlock lab. 
Warning: Please remember your Lab lock password. In case of a lost password, you will not be 
able to recover it. Unlocking a lab / removal of password can be done by EVE-NG support only. 
7.1.13 Dark mode or Light mode 
 
Sets your lab background to the dark mode 
 
Sets your lab background to light mode 
7.1.14 Close lab 
Closes the lab topology. The lab can be closed while the nodes in 
the lab nodes are stopped. 
7.1.15 Logout 
Log out from the EVE WEB GUI session.

EVE-NG Community Cookbook 
Version 1.11 
Page 104 of 165 © EVE-NG LTD 
 
7.2 EVE Lab topology menus 
Right-clicking within the EVE topology can open new menus with various functions and options 
for managing nodes. 
7.2.1 Lab topology menu 
Right-clicking on the (free/unused) canvas of the EVE topology opens a new 
menu. (Add-) Node, Network, Picture, Custom Shape  and Text are the same 
functions referred to in section 7.1.1. 
Auto Align.  This function will help align object s on the topology. The l ab 
creator does not need to worry about small  displacements of object s. Auto  
Align will align all object s to a virtual grid  with a single click and can make 
neatly arranged labs look even neater. 
7.2.2 Connection menu 
Right-clicking on the connection between nodes allows you 
to delete this connection. 
 
7.2.3 Cloud or Bridge network menu 
Right-clicking on a Cloud or Bridge network allows you to edit or delete it. 
  
If you have chosen Edit, the Network edit window will 
open a window where you can change the placement, 
network type or name/prefix. 
 
For details on  how to operate EVE Cloud networks and 
external connections, please refer to section 9

EVE-NG Community Cookbook 
Version 1.11 
Page 105 of 165 © EVE-NG LTD 
 
 
7.2.4 Stopped node menu 
Right-clicking on a stopped node also opens a menu: 
 
Start node: This will start the selected node in this lab 
Wipe node: Wiping a node will erase the NVRAM 
(running config) or the temporary image snapshot 
depending on the type of node. This option is used to 
clean up a node in order to boot it from factory 
defaults or a custom set of configurations. 
Edit node: Opens the Edit node window (picture on 
the right). For details please refer to section 8.1.2 
Delete node. Deletes the node from the lab. It is 
recommended to disconnect (delete connections to it) 
the node before you delete it. 
 
 
 
 
7.2.5 Running node menu 
Right-clicking on a running node also opens a menu:

EVE-NG Community Cookbook 
Version 1.11 
Page 106 of 165 © EVE-NG LTD 
Wipe node: Wiping a node will erase the NVRAM (running config) or the temporary image 
snapshot depending on the type of node. This option is used to clean up a node in order to 
boot it from factory defaults or a custom set of configurations. 
Export CFG: This function is used to export the saved running configuration to the EVE 
startup configuration sets. Reference section 10.3 
Capture. Wireshark capture. Select the interface which you wish 
to capture. Reference section 5.1.2 
 
 
 
7.2.6 Selected nodes menu and features 
It is possible to select many objects or nodes at once in EVE. Using your mouse, you can select 
an area which will cover your nodes and/or you can click on nodes while holding the CTRL key 
on your keyboard. 
 
A right-click on any of the selected nodes opens a group menu: 
 
Start Selected: This will start the selected nodes in this lab. 
Stop Selected: This will stop the selected nodes in this lab

EVE-NG Community Cookbook 
Version 1.11 
Page 107 of 165 © EVE-NG LTD 
Wipe Selected: The Wipe Selected nodes action will wipe the NVRAM or currently saved 
image of the selected nodes in the current lab. 
Example: You have saved the nodes configuration by sav ing the running configuration to the 
startup configuration. The Wipe comm and will delete the saved NVRAM startup configuration 
and on the next boot it will boot from factory defaults.  
The same applies to images without configurations, e.g. a linux node. If you make modifications 
to the system and afterwards wipe this node, the  next time it will boot from the original base 
image again as the modified image was deleted. 
The Wipe node action is commonly used with initial startup configuration modifications. T he 
Wipe node action does not delete configured startup configurations or sets. Please refer to 
section 10.3 
Console To Selected Nodes: Console To Selected Nodes will open a console to all selected 
running nodes in the current l ab. This includes all different kinds of configured console types 
for lab nodes like VNC, Telnet and RDP  
Export all CFG s: The Export all configurations action wil l export current configs of selected 
nodes to the EVE startup-configs. 
For a full explanation of exporting configurations, please refer to section 10.3 
Set nodes s tartup-cfg to default configset:  Sets nodes to Default startup config, used 
commonly with the wipe nodes function. NOTE: If you have nothing saved in the default config 
set for any node, that node will boot from factory default instead. This is commonly us ed with 
the wipe nodes function so the node will boot from the configured startup -config on next boot 
and not from the startup-config in its NVRAM in case the node was started before already. 
Please refer to section 10.3 
Set nodes startup-cfg to none. Setting selected lab nodes to boot from factory default. Used 
commonly with the wipe nodes function. The example below shows the ste ps to set selected 
nodes to boot from factory default. 
Step 1: Wipe selected nodes 
Step 2: Set nodes startup-cfg to none 
 
Please refer to section 10.3 
Horizontal Align. Aligns the selected nodes in one horizontal line.  
Step 1: Select the nodes you wish to align. 
Step 2: Right click on one of the selected nodes and choose Horizontal align, this will align all 
nodes to the selected node. 
Picture before:

EVE-NG Community Cookbook 
Version 1.11 
Page 108 of 165 © EVE-NG LTD 
 
Picture after: 
 
Vertical Align: Aligns the nodes in one vertical line. 
Step 1: Select the nodes you wish to align. 
Step 2: Right click on one of the selected nodes and choose Vertical align , this will align all 
nodes to the selected node. 
Picture before  Picture after 
  
Circular Align: Aligns the nodes in a circle.  
Step 1: Select the nodes you wish to align. 
Step 2: Right click on one of the selected nodes and choose Circular Align, this will align all 
nodes in a circle, the midpoint of the circle will be at the coordinates the selected node was at 
before. 
Picture Before    Picture After

EVE-NG Community Cookbook 
Version 1.11 
Page 109 of 165 © EVE-NG LTD 
  
Delete nodes startup-config.  
 WARNING, this action will delete the configurations of the selected nodes that are 
saved to your Default config set. Please make sure that is what you want to do before 
you execute this. 
Delete selected: This will delete the selected nodes from your current lab. 
Selected nodes can be moved as a group across the topology. 
Example: You can select nodes and objects to better position them on the Topology.  
 
 
7.3 EVE Lab node states and symbols 
7.3.1 Stopped (non-running) nodes 
Grey colour and a square symbol below a node means that the node is stopped 
and not running. Once you will start it, the node will change to one of the running 
states below.

EVE-NG Community Cookbook 
Version 1.11 
Page 110 of 165 © EVE-NG LTD 
A grey node with an exclamation mark inside a triangle below the node means 
that there was a problem during the boot process, this could be a corrupted boot 
image, insufficient resources or problems with the initial configuration. A node in 
this state cannot be started again. 
Workaround: Right-click on the node and wipe it, the symbol will then change to a grey colour 
with a square symbol below it. Then edit the node and make sure you have configured sufficient 
resources and the correct settings for this node, if it has startup-configs you can check them as 
well. Afterwards start the node again. 
7.3.2 Running nodes 
The blue colour and black Play triangle symbol means that the node is started and 
running, the node is in a working/functional state. 
A running node with a clock symbol below the node means that the node is waiting 
to finish loading from the set exported/startup configuration. Once the configuration 
has been successfully applied, the node symbol will change to a Play triangle 
symbol. If the node has finished booting but the clock symbol does not change to 
the Play triangle symbol, the problem could be in the uploaded startup configuration. For how 
to use exported configurations and boot nodes from them, please refer to section 10.1 
A running node with a turning red gear symbol means that the node is either in the 
process of hibernating the node or it has sent the shutdown signal to the node and 
is waiting for it to turn off. Once this process has successfully finished, the symbol 
will turn into a grey node with a black square symbol below it (stopped state). 
 NOTE: If the node does not support a system shutdown or does not recognize the 
shutdown signal (example: Cisco router), after clicking on Shutdown, the node can 
stay with a turning red gear symbol below it indefinitely.  
Workaround: Use Stop or Stop/PowerOff to stop the node. 
Example nodes where Stop/Shutdown is supported: Microsoft Windows and most Linux nodes 
as well as a lot of appliances based on linux. 
7.3.3 Node connector symbol 
Connector symbol: If you move your mouse pointer on top of a running 
or stopped node, an orange connector symbol appears. It is used to 
connect nodes on the topology in a drag and drop style. Drag the 
symbol from one node and release the mouse pointer on th e second 
node. A new window will appear where you can select the 
interfaces the link should connect to.

EVE-NG Community Cookbook 
Version 1.11 
Page 111 of 165 © EVE-NG LTD 
7.4 Other 
7.4.1 Notifications area 
 The Notification area in the top right is displaying 
informational or error messages.

EVE-NG Community Cookbook 
Version 1.11 
Page 112 of 165 © EVE-NG LTD 
8 Working with EVE labs 
 IMPORTANT NOTE: You must prepare and upload at least a couple of images to start 
building your labs. Refer to section 12 
8.1 Creating a lab 
Step 1: Click Add new lab. For more information on creating new labs, please refer to section 
6.2.2.1 
 
Step 2:  
Fill out the  lab information . Name and Ver sion are required fields. Next hit  Save. Refer to 
section 6.7.4 for more information about the different fields in the Edit lab window. 
 
8.1.1 Adding nodes to the lab 
The new Topology page will open. There are two different ways to add nodes to the topology 
canvas: 
Step 1: Object/Add Node 
Left Side Bar > Add object > node. Refer to 
section 7.1.1.1 for more information. 
Right click on a free a rea of the t opology 
page and click on “Node” to add a new node. 
Refer to section 7.2.1 for more information.

EVE-NG Community Cookbook 
Version 1.11 
Page 113 of 165 © EVE-NG LTD 
  
 
Step 2: The Add new node window will appear. You can scroll down to choose which node you 
wish to add to the lab topology, or you can type the node name to filter through the node list. 
 NOTE: It will only be possible to select and add nodes that have images preloaded in 
EVE. These nodes will be displayed in  a blue font. To prepare images for EVE, refer  
to section

EVE-NG Community Cookbook 
Version 1.11 
Page 114 of 165 © EVE-NG LTD 
Step 3: Edit “Add a new node” settings. Please refer to the picture and table below.

EVE-NG Community Cookbook 
Version 1.11 
Page 115 of 165 © EVE-NG LTD 
8.1.1.1 Node values Table 
Number Description 
1.  
Template menu. Choose which node template to add 
to the topology 
2.   Chose the number of nodes of this type you want to add 
to the topology 
3.  
Choose your preferred version from preloaded images 
list (if you have more than one image loaded fo r a 
single template). 
4.  
Type your preferred 
node name. If you are 
adding more than one, EVE will automatically append numbers to the nodes 
name.  
Example. We are adding 5 CSR nodes with the name R. On the topology they 
will appear as R1, R2, R3, R4, R5. Later using the Nodes window, you can edit 
the node names per your needs. Refer to section 7.1.2 or edit the node 
individually, refer to section 8.1.2. 
5.  
Node icons can be changed from the 
default per your preference, simply choose 
the preferred icon from the dropdown list. 
Node icons can be changed later per your 
needs. Refer to section 7.1.2 
6.  
The UUID number is 
assigned automatically after 
a node is created. You may 
also set it manually in case you are using a license that is tied to a particular 
UUID.

EVE-NG Community Cookbook 
Version 1.11 
Page 116 of 165 © EVE-NG LTD 
7.  
CPU limit per node. This option is already set 
(checked/unchecked) per EVE recommendations. Refer to 
section 6.4.1 
8.  
Each node template has a pre-set CPU value that aligns 
with vendor requirements. This value can be changed per 
your needs. 
9.  
Each node template has a pre-set RAM value that aligns 
with vendor requirements. This value is displayed in MB 
and may be changed per your needs. 
10.  
The number of ethernets interfaces. 
 NOTE for IOL nodes: 
Ethernet interfaces for IOL nodes are placed into groups of 4. A value of 1 for 
Ethernet means your node will have 4 interfaces. 
The serial interface option is available for IOL nodes only and follows the same 
grouping structure as ethernet interfaces. A value of 1 for Serial means your 
node will have 4 serial 
interfaces. 
 
11.  
Custom MAC address for Qemu nodes only. You can define your own MAC 
address for first interface: 
 
12.  
EVE will pre-set the best recommended QEMU version 
for each node template. This value can be changed per 
your needs. 
13.  
Qemu architecture is pre-set per image vendor 
recommendations. This value can be changed per your 
needs

EVE-NG Community Cookbook 
Version 1.11 
Page 117 of 165 © EVE-NG LTD 
14.  
Type of Qemu NIC is pre-set per image vendor 
recommendations. This value can be changed per your 
needs. 
15.  
Qemu custom options are pre-set 
per image vendor 
recommendations. This value can be changed per your needs 
16.  
Startup configuration: Value can 
be changed to set your node  to 
boot from saved configurations. Refer to section 10.3 for more details. 
17.  
The Delay value is set in seconds and can 
be used to delay a node from booting after 
it is started. Example: if the value is set to 30, the node will wait 30 seconds 
before processing its boot sequense. This feature is useful in conjunction with 
the “Start all nodes” function if your lab requires certain nodes to start up before 
others or to avoid a mass-start of very heavy nodes. 
18.  
Console types for each 
template are pre-set with 
recommended settings.   
The setting can be changes per your needs.  
 NOTE:  The Docker template contains a wide variety of images, 
therefore, please refer to section 14.1.3 for recommended console 
types for each docker image. Windows nodes can use either RDP or 
VNC but RDP needs to be enabled in Windows itself. 
19.  
OPTIONAL: Templates for 
Cisco FirePower, F5, Linux, 
and Citrix have the option to 
manually set the MAC address for the first ethernet interface. This will enable 
the use of licenses that are tied to a particular MAC address. 
MAC Address format must be like: 00:50:0a:00:0b:00 
8.1.2 Edit node 
EVE provides two ways to edit nodes after being added to the topology canvas.

EVE-NG Community Cookbook 
Version 1.11 
Page 118 of 165 © EVE-NG LTD 
 NOTE: A node must be wiped each time an image or startup configuration has been 
changed. 
8.1.2.1 Edit nodes globally 
From the Topology page. Click “Nodes” from the left sidebar to bring up the nodes list. Refer to 
section 7.1.2 for more details. 
 
 
8.1.2.2 Edit node individually. 
Right click on the node and click Edit 
 
 
The “Edit node” window will appear. It is very similar to the window that is displayed when you 
add a new node . To change values for the node, refer to the nodes value table in section 
8.1.1.1.

EVE-NG Community Cookbook 
Version 1.11 
Page 119 of 165 © EVE-NG LTD 
 
 
8.1.3 Wipe Node 
The “Wipe node” function will clear the NVRAM of the node. Each time 
a node setting  is changed ( CPU, RAM, boot image or startup 
configuration) a wipe must be issued on that node. For more information 
refer to section 10.3

EVE-NG Community Cookbook 
Version 1.11 
Page 120 of 165 © EVE-NG LTD 
8.1.4 Interconnecting nodes 
To connect nodes on the lab, use the drag and drop style method 
Connector symbol: Moving the mouse over a node will make an orange male plug 
appear. The male plug is used to connect nodes on the topology , drag and drop 
style. Release the mouse pointer on the second node. 
 
 
 
 
The connection window will appear. Choose the interface you want to use to  interconnect the 
nodes. Click Save when finished. 
 
 
 
 
 
 
8.1.5 Delete connection between nodes 
To delete a connection, right click on it and hit “Delete.”

EVE-NG Community Cookbook 
Version 1.11 
Page 121 of 165 © EVE-NG LTD 
8.1.6 Delete Node 
To delete a node, right click it and hit “Delete.” This is a non -reversable 
function 
NOTE: It is strongly recommended to delete connections from a node 
before deleting the node itself.  
 
8.2 Running labs 
8.2.1 Starting lab 
Nodes inside a lab may be started individually, in groups, or all at once. 
The Start all nodes option will start all nodes on your topology.  
 IMPORTANT. Starting all the nodes at once can  result in major  spikes in CPU 
utilization. Please make sure you are not using the “Start all nodes” option for heavy 
labs. Instead, it is recommended to start nodes in small groups. 
Starting a node or group of nodes: 
Right click on single node or node group and hit “Start.” 
 
 
 
 
Running nodes will turn blue. Refer to section 7.3 for node states 
 
8.3 Saving labs 
To save a running lab, refer to the vendor recommended save commands for each node.   
Example:  
Cisco: “copy run start” 
Juniper “commit”

EVE-NG Community Cookbook 
Version 1.11 
Page 122 of 165 © EVE-NG LTD 
 
Your current work will be saved in the nodes ’ NVRAM and the lab can be stopped safely . 
Starting the lab again will allow you to pick up from where you left off. 
 WARNING: Using the wipe action on a node will clear its  NVRAM. This is similar to 
doing a factory reset on a device. 
The configurations of nodes can be  exported and used as initial or startup configurations for 
your labs. To export configurations and configuration sets for labs refer to section 10.1 
8.4 Stopping labs 
  The Stop all nodes option will stop all nodes on your topology. 
NOTE: It is recommended to save your running configurations before you stop your nodes. 
Stopping a node or group of nodes: 
Right click on single node or node group and hit “Stop.” 
For individual node Stop options refer to section 7.2.5 
8.5 Start saved lab 
Select the lab you want to start and click “Open”. To start Lab refer section 8.2.1 
 
8.6 Importing labs 
Refer to section 6.2.2.6 
8.7 Exporting labs 
Refer to section 6.2.2.5

EVE-NG Community Cookbook 
Version 1.11 
Page 123 of 165 © EVE-NG LTD 
8.8 Deleting labs 
Refer to section 6.2.2.2 
8.9 Moving labs 
Refer to section 6.2.2.4

EVE-NG Community Cookbook 
Version 1.11 
Page 124 of 165 © EVE-NG LTD 
9  EVE Clouds and Networks 
9.1 Bridge Network  
The EVE Bridge interface act s like an unmanaged Switch. It supports passing along tagged 
dot1q packets. 
Example: We have to connect many nodes in a flat (dot1q) network 
Step 1: Add a Bridge Network onto the topol ogy. There are two ways to do this: Right -clicking 
on the topology area and selecting “Add Network” or in the sidebar  click “Add an Object” and 
then select “Network.” Please refer to sections 7.2.3 and 7.1.1.2 
  
Step 2: Name/prefix can be changed in order to rename your Bridge network. Make sure your 
network type is set to bridge. 
 
Step 3: Connect your nodes using the drag and drop connector. Re fer to sections 8.1.4 and 
7.2.3

EVE-NG Community Cookbook 
Version 1.11 
Page 125 of 165 © EVE-NG LTD 
9.2 Management Cloud0 interface 
EVE management interface is also known  as the Cloud0 
network for labs. The Cloud0 interface is bridged with your 
EVEs first NIC. “Cloud” is used as an alias to pnet. Pnet is 
the bridge interface name inside of EVE. 
Cloud0 is commonly used inside EVE labs to get management access to nodes running inside 
EVE from a host machine external to EVE. 
 IMPORTANT NOTE: For EVE VMs running  on ESXi, make sure your management 
interface bridged with the vSwitch (Port group) has the security settings for 
Promiscuous Mode set to Accept. Any port group or vSwitch used to connect an 
external network to an EVE Cloud network need s to have the Promis cuous mode set 
to “Accept”! 
vSwitch Settings 
 
Portgroup Settings

EVE-NG Community Cookbook 
Version 1.11 
Page 126 of 165 © EVE-NG LTD 
EVE Cloud0 bridging table. 
Lab name 
EVE 
interface 
name (inside) 
Type Notes 
Cloud0 pnet0 Bridged 
Cloud0/pnet0 is bridged with your primary 
EVE ethernet port. It is assigned a 
management I P address used for WEB 
GUI access. The  EVE management  
subnet can be used as a management 
network in labs. 
 Question: How can I obtain my Cloud0 subnet and gateway IP. Many EVE VMs only 
have a DHCP address assigned on the pnet0 interface. 
Answer: SSH to EVE and type the following from the CLI: 
route 
 
Example: We want to use Cloud0 as a management network for an ASAv node in an EVE lab. 
From the above-obtained information, we know that our Cloud management subnet is 
192.168.90.0 with a mask of 255.255.255.0 and the Gateway IP is 192.168.90.1. 
Step 1:  Add A New Network onto the topology. 
There are two ways to do this: Right -clicking on 
topology area and selecting “Network” or in the 
sidebar, “Add an Object” and then select “Network.” 
Step 2:  Name/prefix can b e changed in order to 
rename your Cloud0 network. Make sure your 
network type is set to Management(Cloud0). 
Step 3: Connect your ASAv using the drag and drop 
connector to the Cloud0 network. Refer to sections 
8.1.4 and 7.2.3 
Step 4: Start the node and configure the interface connected to Cloud0 with an IP address from 
the management subnet (192.168.90.0/24 in this example). Make sure you do not assign 
duplicate IPs.

EVE-NG Community Cookbook 
Version 1.11 
Page 127 of 165 © EVE-NG LTD 
 
NOTE: Cloud interfaces can be used to connect multiple nodes to a single cloud instance on 
the topology. 
 
9.3 Other cloud interfaces 
Other cloud interfaces can be used to extend a lab connection inside of EVE or bridged with 
other EVE interfaces to connect external networks or devices. 
EVE Cloud bridging table. 
Lab cloud 
name 
EVE 
interface 
name 
(inside) 
Type 
ESXi VM 
corresponding 
interface 
VMware 
Workstation 
corresponding 
interface 
Bare HW 
Server Notes 
Cloud0 Pnet0 bridged Network 
adapter 1 
Network 
Adapter 
First 
ethernet 
Eth0 
Cloud0/pnet0 is bridged with 
your primary EVE ethernet port. 
It is assigned a management IP 
address used for WEB GUI 
access. The EVE management 
subnet can be used as

EVE-NG Community Cookbook 
Version 1.11 
Page 128 of 165 © EVE-NG LTD 
management network in the 
labs. 
Cloud1 Pnet1 bridged Network 
adapter 2 
Network 
Adapter 2 
Second 
ethernet 
Eth1 
Cloud1 can be bridged with your 
EVE second ethernet port to 
achieve connection to another 
network or device. The IP 
address is not required to be 
configured on it. It will act like a 
pure bridge your external 
connection with EVE lab node. 
Cloud2 Pnet2 bridged Network 
adapter 3 
Network 
Adapter 3 
Third 
ethernet 
Eth2 
Same as Cloud1 
Cloud3 Pnet3 bridged Network 
adapter 4 
Network 
Adapter 4 
Fourth 
ethernet 
Eth3 
Same as Cloud1 
Cloud4-9 Pnet4-9 bridged Network 
adapter 5-10 
Network 
Adapter 5-10  Same as Cloud1 
Example: Cloud7 network is used as an extended connector between nodes: 
Step 1: Add two Cloud7 networks onto the topology. 
 
Step 2: Connect your lab nodes to Cloud7. Your 
configured nodes will work like being connected to 
the same switch (or the same bridge in EVE). 
Even CDP works. It is convenient if it is necessary 
to have connections across the lab and you don’t 
want to have connections going from one end of 
the lab to the other.

EVE-NG Community Cookbook 
Version 1.11 
Page 129 of 165 © EVE-NG LTD 
 
If some of the clouds (e.g. Cloud1 ) are bridged to another ethernet (VMnet) you can connect 
your EVE lab to an external VM or physical device (like e.g. a switch, IP phone or access point). 
 For ESXi make sure that you have set Promiscuous mode security setting s on the 
vSwitch and Port group to Accept. Please refer to section 9.2 
The next sections will explain how you can use Cloud networks in EVE to connect to other 
external (e.g. VMWare) VMs or physical devices. 
9.4 Connecting external VM machines to the EVE Lab 
9.4.1 ESXi VM machines 
External ESXi VM machines can be connected to EVE labs using cloud interfaces.  
 NOTE: A single Cloud interface can be used to connect more than one external VM to 
the EVE lab. 
Example:  Connecting a Web Security Appliance (WSA) to the lab using the Cloud1 interface. 
Step 1: Create a new or use an existing portgroup on your ESXi and assign it to EVE and WSA 
VMs as shown below. Make sure you have set Promiscuous mode on the vSwitch (portgroup 
WSA-MGMT) to Accept. 
 NOTE: VM machines must be in a powered off state to assign network interfaces. 
Portgroup WSA-MGMT (with vSwitch5 as parent) settings:

EVE-NG Community Cookbook 
Version 1.11 
Page 130 of 165 © EVE-NG LTD 
 
Parent vSwitch5 settings: 
 
EVE and WSA VMs settings 
EVE VM, second port is assigned to 
portgroup WSA-MGMT. It is Cloud1 on the 
EVE topology.  
 
Cisco Web security appliance (WSA), 
Management port is assigned in portgroup 
WSA-MGMT.  
 
 
EVE Lab connected to the WSA (Cloud1) 
 NOTE: ESXi WSA VM obtained the IP 192.168.10.3 from the DHCP pool on the lab 
switch. The gateway is 192.168.10.1 
 NOTE: The Firefox Docker node user for management obtained the IP 192.168.10.2 
from the DHCP pool configured on the lab switch.

EVE-NG Community Cookbook 
Version 1.11 
Page 131 of 165 © EVE-NG LTD 
 
 
 
 
 
 
 
 
 
 
 
 
9.4.2 VMWare workstation machines 
External (meaning not running inside EVE) VMWare workstation machines can be connected 
to EVE labs using cloud interfaces. 
 NOTE: A single Cloud interface can be used to connect more than one external VM to 
the EVE lab. 
Example:  Connecting Web security Appliance (WSA) to the lab using Cloud2 interface. 
 NOTE: VMs must be in a powered off state to assign network interfaces. 
Step 1:  Open your VMWare Workstation Virtual Network Editor and configure the VMnet 
interface for the Cloud and WSA VMs. If necessary, add a new VMnet. The example below is 
showing VMnet2 Settings in VMWare workstation. DHCP must be disabled for VMnet2. 
Virtual Network Editor settings:

EVE-NG Community Cookbook 
Version 1.11 
Page 132 of 165 © EVE-NG LTD 
 
EVE and WSA VMs settings 
EVE VM, the third port (Network adapter 3) is 
assigned to VMnet2. This is Cloud2 inside your 
EVE labs.  
 
Cisco Web Security Appliance (WSA), 
Management port is assigned to VMnet2 
 
EVE Lab connected to the WSA (Cloud2) 
 NOTE: ESXi WSA VM obtained the IP 192.168.10.3 from the DHCP pool on the lab 
switch. The gateway is 192.168.10.1 
 NOTE: The Firefox Docker node user for management obtained the IP 192.168.10.2 
from the DHCP pool configured on the lab switch.

EVE-NG Community Cookbook 
Version 1.11 
Page 133 of 165 © EVE-NG LTD 
 
9.5 Connecting EVE Lab to a physical device 
 IMPORTANT NOTE : To bypass MAC addressing over pnet/cloud interface please 
SSH to your EVE and type: 
for i in /sys/class/net/pnet*/bridge/group_fwd_mask  ; do echo 8 > $i ; done 
 
9.5.1 ESXi EVE 
To connect a physical device (e.g. router, switch) to an EVE lab over a cloud interface, we have 
to bridge the ESXi NICs ethernet port to a VMnet interface. 
 IMPORTANT NOTE: Make sure that you have set Promiscuous mode security settings 
on the vSwitch and Port group to Accept. 
 IMPORTANT NOTE: If you are building trunk between EVE lab node to real Switch, 
please make sure you have set your ESXi vSwitch interface to accept all vlans.  
Reference: https://kb.vmware.com/s/article/1004074 
The Example below is showing ESXi Server settings of the virtual network bridged to the 
physical interface.

EVE-NG Community Cookbook 
Version 1.11 
Page 134 of 165 © EVE-NG LTD 
Logical chain of the networking bridge: 
EVE Lab Cloud0 →Portgroup “Management 90 UD”→vSwitch 1→Physical Adapter eth1 
 
vSwitch1 settings bridged with Server Ethernet port vmnic1 (physical adapter) 
 
Portgroup “Management 90 UD” Settings associated with vSwitch1 
 
EVE VM Settings 
EVE VM Cloud0 is connected to Portgroup “Management 90 UD”

EVE-NG Community Cookbook 
Version 1.11 
Page 135 of 165 © EVE-NG LTD 
 
 
EVE Lab Connected to a physical device 
Physical Topology 
Cisco 887M device port Fastethernet 3 is physically connected to Server port eth1.  
 
EVE Lab Topology 
EVE lab switch port G0/0 is configured as trunk and connected to Cloud0 over bridged chain 
to the physical Cisco 887M Router switchport Fastethernet 3

EVE-NG Community Cookbook 
Version 1.11 
Page 136 of 165 © EVE-NG LTD 
9.5.2 VMWare workstation EVE 
Similar to the ESXi connection, it is recommended to have a second ethernet interface on your 
PC. It can be a USB ethernet extender as well. Not all ethernet adapters fully support a layer2 
connectivity over it. MS Windows OS itself strips off any tags added to the packet. Even if your 
NIC supports 802.1q VLAN tagging, Windows 10 strips these tags off. The example below will 
show a Windows 10 host connected to a physical 3750G-24 switch. The Windows 10 Host has 
an Intel (R) PRO/1000 PT Dual port server adapter and is bridged with VMWare workstation 
(version 14) VMnets. 
Virtual Network Editor Settings, Bridged VMnet interfaces with Real NIC Ports 
 
EVE VM Settings. Network adapter is bridged to VMnet0 (ethernet Intel Pro 1), and Network 
adapter 2 is bridged to VMnet1 (ethernet Intel Pro 2).  
Responding cloud interfaces on EVE VM: 
Cloud0→Network Adapter→VMnet0→IntelPro 
Cloud1→Network Adapter 2→VMnet1→IntelPro#2

EVE-NG Community Cookbook 
Version 1.11 
Page 137 of 165 © EVE-NG LTD 
 
Physical connection scheme and VMware bridging. 
 
EVE Lab scheme.

EVE-NG Community Cookbook 
Version 1.11 
Page 138 of 165 © EVE-NG LTD 
The following solution allows Windows hosts to transmit tagged packets over ethernet. This 
has been used in the example above. 
 Warning. You are making changes to your Windows registry files! This is at your own 
risk. 
https://www.intel.co.uk/content/www/uk/en/support/articles/000005498/network -and-i-
o/ethernet-products.html 
9.5.3 Bare metal server EVE 
A physical server usually has more than one ethernet port, free ports 
can be bridged with EVE clouds and used for external connections. 
EVEs internal interface settings are already bridged in order, pnet0 -9 
are mapped to eth0-9. Refer to the bridging table in section 9.3 
cat /etc/network/interfaces 
Basically, your servers physical port eth0 is bridged to pnet0 which is Cloud0 in your labs, eth1 
is bridged to pnet1 which is Cloud1 in your labs (and so on). Refer to the bridgin g table in 
section 9.3 
The example below shows how to connect a bare -metal EVE server with a physical Cisco 
3750E switch. 
Physical connection topology: 
 
The EVE lab switch’s CDP neighbor is the 3750E switch’s p ort Gig 1/0/25: A trunk has been 
configured between the EVE lab switch and the physical 3750E switch.

EVE-NG Community Cookbook 
Version 1.11 
Page 139 of 165 © EVE-NG LTD 
10  Advanced EVE Lab features 
10.1 Lab design objects 
EVE Community has drawing elements integrates to add drawings and text information to the 
lab topology. Objects can be placed on the topology in two ways. 
Example below, EVE lab with design elements: 
 
Option 1: Side bar -> Add an object 
 
Option 2: Right -click on a fr ee area on the 
topology canvas to add an object. 
 
10.1.1 Custom shape  
There are three custom sha pes that can be added to the topology: square, round square and 
circle (sphere). 
Type: Square, round square or circle

EVE-NG Community Cookbook 
Version 1.11 
Page 140 of 165 © EVE-NG LTD 
Name: This field can be filled with your preferred shape’s name. If the field is left empty, EVE 
will generate a name for the shape. 
Border type: Two options: line or dashed 
Border width : Increase or decrease the width of the 
border. This can be edited later in the “Shape Edit” menu. 
Border colour: Allows you to choose a colour for the 
shape’s border. This can be edited later in the “Shape 
Edit” menu. 
Background colour: Allows you to choose a colour to fill 
your shape with. This can be edited later in the “Shape 
Edit” menu. 
 
Example: Added a circle and square on the topology. Shapes can be moved 
around the topology drag and drop style (click and move with mouse). 
 
10.1.2 Resize square or circle objects 
Move your mouse over the right bottom corner of the object until a 
corner symbol appears. Left click and drag your mouse to change 
object size or style (rectangle, sphere)  
10.1.3 Text object 
It is also possible to add text to your EVE topology.

EVE-NG Community Cookbook 
Version 1.11 
Page 141 of 165 © EVE-NG LTD 
 
Example: text objects added to the topology. 
 
 
 
10.1.4 Add custom picture on the Lab using Text object feature 
Sometimes you may have to add pictures, like logos on your topology. It is possible but you 
need to convert your png or jpg to html format. We have tested this one as the best to achieve 
result. Load your image in the web, and convert to html format. 
https://www.askapache.com/online-tools/base64-image-converter/ 
Step 1: Load your picture jpg or png format and encode it. 
 
Step 2: Scroll down to find HTML format 
 
Step 3: Set your desirable size of picture. 
 
Step 4: Mark and copy all content from HTML window above

EVE-NG Community Cookbook 
Version 1.11 
Page 142 of 165 © EVE-NG LTD 
Step 4: Copy content to EVE text object 
 
Step 5: Move and place your picture to the Lab. 
 
10.1.5 Cloning objects and overlay positions 
Right click on the object y ou want to clone and choose “Duplicate”. You can also change the 
object’s overlay position using the “Send to Back” or “Send to front” options. 
 
 
 
10.1.6 Objects Editing 
Right click the object and choose “Edit” for additional options.

EVE-NG Community Cookbook 
Version 1.11 
Page 143 of 165 © EVE-NG LTD 
 
At the bottom of the “Topology Canvas” page, additional object options will appear  
 
Z-index: Used to change the object’s overlay pos ition on the “Topology Canvas.” An object 
with a higher numerically valued z -index will cover an object with a lower 
numerically valued z-indexed.   
Example: The blue object has a z-index of -1 and the orange object’s z-index is 0. 
Orange object is top over blue object. 
Border width: Used to change the object’s border width. 
Border type: Used to change the border style of the object between solid and dashed. 
Border colour: Used to change the colour of the object’s border 
Background colour: Used to change the background colour of the object 
Transparent: Turns off background colour (filling) and makes the 
object transparent. 
Rotate: Used to rotate the object on the topology.  
 
Name: Used to change the object’s name. 
To save the object, press Save (green button).  
 
10.1.7 Lock objects movement 
The “Lock Lab” feature prevents objects from being moved around on the canvas (among other 
things). For more information about this feature, refer to section 7.1.12. 
10.2 Custom design logical topology 
EVE Community includes a feature to upload your own custom topology picture and map nodes 
to it for easy access.

EVE-NG Community Cookbook 
Version 1.11 
Page 144 of 165 © EVE-NG LTD 
10.2.1 Custom design upload  
Before you upload a custom picture in the lab, make sure it is 
in .png or jpg format with resolution 130-150x130-150 pixels. 
TIP: It is best is to create a topology in the MS Visio and after 
convert it to the .png picture format with resolution 140x140. 
 
Step 1: Open “Add an Object” and then “Pictures” from the left sidebar or 
right click on a free area on topology canvas and hit “Add Picture.” 
 
 
 
 
 
 
 
 
Step 2:  Browse your PC for a .png or .jpg file and hit “Add”.  
 
 
 
Once the picture is added to the topology canvas, the sidebar will display a new option: “Logical 
maps” 
 
 
Step 3: Open the “Logical maps” menu item.  
 
 
 
Pictures window management  
 Delete uploaded picture from the lab 
 
Image Map: Map nodes to places in the 
picture 
 
Display uploaded picture. Work with lab and 
custom topology 
 
Zoom/unzoom uploaded custom topology 
 
Makes the window transparent to see  the 
“Topology Canvas” behind it. Clicking again 
returns to the normal view.

EVE-NG Community Cookbook 
Version 1.11 
Page 145 of 165 © EVE-NG LTD 
 
Close “Pictures” window. 
10.2.2 Custom topology mapping 
This feature allows you to map the lab nodes to your custom topology picture.  
Step 1: Open the Image Map window: 
 
Step 2: Select a node, from the dropdown menu, that you want to map to the topology. 
 
Step 3: Move your mouse over a node icon on the “Image Map” and click to map it. The grey 
circle means that the node is mapped. 
 
Step 4: Continue mapping the rest of the nodes. 
 
Step 5: OPTIONAL. You can also add a mapping for a device external to your EVE server in 
order to telnet, VNC, or RDP to it. This way you can open sessions to all your devices (whether 
external or internal) in one place. 
Select from menu:  
And map with node on topology.  
 
Change image map adding protocol, IP and port.

EVE-NG Community Cookbook 
Version 1.11 
Page 146 of 165 © EVE-NG LTD 
 
 
Step 6: Save your mapping and refresh the browser with F5.  
10.2.3 Delete topology or mapping  
To delete a single node mapping, right click on node mapping circle and click  “Delete.”  
 
To delete the entire custom topology, click delete.  
 
10.3 Configuration export feature 
EVE Community includes an export configuration feature that allows you to save  and manage 
configurations in a lab. The "Configuration Export" and “Startup-configs” features will allow you 
to set these saved configurations as startup configs for your nodes when they boot.  
 IMPORTANT NOTE : Before you start us ing the  “Configuration export ” feature, you 
must complete at least one configuration export. 
Nodes will be greyed out withou t the option to enable “Startup-
configs” until you complete at least one configuration export for 
each node. 
 
 
Node boot order:

EVE-NG Community Cookbook 
Version 1.11 
Page 147 of 165 © EVE-NG LTD 
NVRAM: NVRAM is used as writable permanent storage for the startup configuration . During 
the boot process, the node will alway s check NVRAM for a saved configuration. Saving the 
configuration to NVRAM requires a vendor specific command. Cisco: copy run startup (wr), 
Juniper: commit, etc. It is MANDATORY to save a node’s configuration before you can export 
it. 
Exported configuration: A node configuration that has been exported from the node. It can 
be used to backup configurations or to set them as startup-configs.  
Wipe node: Wiping a node will erase the NVRAM (running config) or the t emporary image 
snapshot, depending on the type of node. Upon a successful wipe, the node will boot with the 
factory default configuration or the configuration included in the base image you are using.  If 
you have the “Startup -config” feature enabled for th e node, then it will boot with the chosen 
config set. You must wipe a node after changing certain node template settings like the image 
or startup-config. You also must wipe the node the first time you want to enable the “Startup -
config” feature. 
Factory default configuration: The base configuration that is applied from the manufacturer.  
 
10.3.1 Supported nodes for configuration exports 
Cisco Dynamips all nodes 
Cisco IOL (IOS on Linux) 
Cisco ASA 
Cisco ASAv 
Cisco CSR1000v 
Cisco Nexus 9K 
Cisco Nexus Titanium 
Cisco vIOS L3 
Cisco vIOS L2 
Cisco XRv 
Cisco XRv9K 
Juniper VRR 
Juniper VMX 
Juniper vMX-NG 
JunipervQFX 
JunipervSRX 
Juniper vSRX-NG 
Mikrotik 
PFsense FW 
Timos Alcatel 
vEOS Arista 
10.3.2 Startup config management  
10.3.2.1 Global commands 
Configurations can be managed via the “Startup-configs window which can 
be accessed from the sidebar menu while on the Topology page.

EVE-NG Community Cookbook 
Version 1.11 
Page 148 of 165 © EVE-NG LTD 
 
Topology page, More Options: 
Export all CFGs – Exports all supported node configurations. 
Set nodes startup-cfg to default configset- Sets all 
supported nodes to boot from the default configuration set. 
Set nodes startup-cfg to none - Sets all supported nodes to 
boot from NVRAM configuration. 
Delete default configuration set. Warning, this will delete 
your exported default configuration set for all nodes. 
 
10.3.2.2 Individual node commands 
Select node, right click 
 
Wipe: Wipes the NVRAM for a single node 
Export CFG: Exports the configuration for a single node 
 
10.3.2.3 Multiple selected nodes commands 
 
Wipe Selected: Wipes the NVRAM for selected nodes 
Export all CFGs: Exports the configuration for selected nodes 
Set nodes startup-cfg to default configs set: Set selected nodes to the default config set 
Set nodes startup -cfg to none : Set nodes to boot from NVRAM or from factory default if 
wiped.

EVE-NG Community Cookbook 
Version 1.11 
Page 149 of 165 © EVE-NG LTD 
Delete nodes startup cfg: Delete selected node’s startup cfg. (clean default set) 
10.3.2.4 Startup-configuration window 
No configuration exports or manual configs loaded for nodes 
 
Startup-configs are exported and the “Configuration Export” feature can be used. 
 
10.3.2.5 Startup-config window information 
 Config set menu 
 No configuration is available for node. Grey 
node 
 
Configuration is available and can be used. 
Blue node. Exported configuration persist 
 Configuration persist but it is disabled. Node 
will boot from NVRAM or factory default if it 
is wiped  
 Configuration persists and node will boot 
from the configuration after being wiped 
 Ace Editor. Different vendor configuration 
edit option. Just Text visual format.

EVE-NG Community Cookbook 
Version 1.11 
Page 150 of 165 © EVE-NG LTD 
10.3.3 Export configuration 
Example: 
 
Step 1: MANDATORY: Configure your nodes and make sure you applied the vendor specific 
command to save the running configuration to NVRAM. If you do not save the configuration, it 
will not be exported and in the notification area, you will receive an error message stating the 
node cannot be exported. 
In this example the nodes have been configured with hostnames only and  the configurations 
have been saved to NVRAM. 
Step 2: In the example below a group of nodes were selected to export configurations. 
 
Step 3: Use “Export all CFGs” for selected nodes. Export configuration is 
completed. The notification area will display “Export All: done” when complete. 
10.3.4 Boot nodes from exported config set 
Step 1: Stop all nodes 
Step 2: Open sidebar and click Startup-configs. Make sure your config is set to ON and the 
nodes config switch is green  (switch on/off beside n ode). Press the green “Save” button (on 
the bottom) and all your nodes will boot with the exported config set after wiping them.

EVE-NG Community Cookbook 
Version 1.11 
Page 151 of 165 © EVE-NG LTD 
 
Step 3: Wipe nodes. For more information refer to section 8.1.3 
Step 4: Start nodes 
10.3.5 Edit exported configurations 
It is possible to edit your configurations for the nodes manually. 
Step 1: Select the node you want to edit the configuration o f and make your changes.  Click 
“Save” when you are finished. 
  
Step 2: Save the config for nodes with the green “Save” button on the bottom.  
 NOTE: you can manually copy/paste any configuration into the config set editor and 
apply it to  your node. Make sure your configuration interfaces match  the lab node’s 
interface names. 
10.3.6 Set lab to boot from none 
To reset your lab nodes’ configuration to factory default, follow the steps below: 
Step 1: Wipe nodes. Refer to section 10.3 for information about wiping nodes and the order of 
operations during boot.

EVE-NG Community Cookbook 
Version 1.11 
Page 152 of 165 © EVE-NG LTD 
Step 2: Open sidebar and click Startup -configs. Make sure your config is set to OFF and the 
nodes config switch is red (switch on/off beside node). Press the green “Save” button (on the 
bottom) and all your nodes will boot with no config/factory default after wiping them. 
 
Step 3: Start nodes 
10.3.7 Lab config script timeout 
Lab config script timeout is used when nodes are w aiting to boot from a config set. The node 
will literally wait during boot until the configuration is applied from the config set. 
Hit “More actions” and then “Edit lab” from the sidebar. Set the config script timeout in seconds. 
By default, this timer is set to 300 seconds for new labs.  
 NOTE: For heavy labs and nodes with 
long configurations, you can raise this 
timer to 600 seconds or higher.

EVE-NG Community Cookbook 
Version 1.11 
Page 153 of 165 © EVE-NG LTD 
11 EVE Troubleshooting 
11.1 CLI diagnostic information display commands 
11.1.1 Display full EVE Community diagnostic 
eve-info 
11.1.2 Display the currently installed EVE Community version: 
dpkg -l eve-ng 
 
11.1.3 Display if EVEs Intel VT-x/EPT option on/off: 
kvm-ok 
 
11.1.4 Display EVEs CPU INFO: 
lscpu 
 
11.1.5 Display EVEs HDD utilization.  
If the /boot only has a little space left you can refer to section 3.6.1.1. If the eve—ng—vg—root 
reaches 99% or 100% then you will need to expand the HDD in order to continue using EVE. 
The Solution to expand your HDD is described in section 11.1

EVE-NG Community Cookbook 
Version 1.11 
Page 154 of 165 © EVE-NG LTD 
df -h 
 
11.1.6 Display EVEs Bridge interface status 
brctl show 
 
11.1.7 Display EVEs system services status 
systemctl list-unit-files --state=enabled 
 
11.2 Expand EVEs System HDD 
 IMPORTANT NOTE: DO NOT expand your current/existing HDD on your EVE VM!

EVE-NG Community Cookbook 
Version 1.11 
Page 155 of 165 © EVE-NG LTD 
11.2.1 Expand HDD on VMware Workstation 
Expanding your EVEs system HDD is achieved by adding an additional HDD to your EVE VM. 
Step 1: Stop all your labs and shutdown EVE. 
Use EVE CLI command: shutdown -h now 
Step 2: Go to edit VM settings and add a new Hard driv e. 
Then click Next. 
Step 3: Leave the recommended SCSI HDD option and then 
click Next 
Step 4: Make sure you have selected the option “Create a 
new Virtual disk.”  
Step 5: Set your desirable HDD Size; example 200GB. 
Step 6: Make sure you have set  the option “Store Virtual disk as a single file” and then click 
Next  
Step 7: Optional: Specify the location of where your new HDD will be stored, then click Finish. 
Step 8:  Boot your EVE VM, HDD siz e will be expanded automatically. To verify, use the 
command to verify HDD utilization referenced in section 11.1.5 
11.2.2 Expand your HDD on ESXi 
Expanding your EVEs system HDD is achieved by adding an additional HDD to your EVE VM. 
Step 1: Stop all your labs and shutdown EVE. 
Use EVE CLI command: shutdown -h now 
Step 2:  Go to edit VM settings and add a new Hard 
drive. Then click Next  
Step 3: Make sure you have selected the option “Create 
a new Virtual disk.” Then click Next 
Step 4: Set your desirable HDD Size; example 200GB. 
Step 5: It is recommended to set the Thick Provision Lazy Zeroed HDD option. 
Step 6: Specify the location of where your new HDD will be stored and then click Next 
Step 7: Leave the recommended SCSI HDD option as is and click Finish. 
Step 8: Boot your EVE VM, the HDD size will be expanded automatically. To verify, use the 
command to verify HDD utilization referenced in section 11.1.5

EVE-NG Community Cookbook 
Version 1.11 
Page 156 of 165 © EVE-NG LTD 
11.2.3 Expand your HDD on a Bare Metal EVE Server 
It is a complicated process to expand a HDD for a bare metal EVE server. Please open a ticket 
in our Live chat support for advice. 
http://www.eve-ng.net/live-helpdesk 
Use a google account to join in the Live Chat or create new chat account. 
11.3 Reset Management IP 
Type the following commands into the CLI followed by enter: 
rm -f /opt/ovf/.configured 
 
su – 
 
http://www.eve-ng.net/documentation/installation/bare-installIP address setup wiza rd. Please 
follow the steps in section 3.5.1 for Static IP or 3.5.2  for DHCP IP setup. 
11.4 EVE Community SQL Database recovery 
Starting from EVE Community version 2.0.3-95, you can recover SQL user database in case 
of disaster: 
unl_wrapper -a restoredb 
 
11.5 EVE Log files 
EVE log Files can be obtained from the System Logs page under the System dropdown menu
 
Use the menu to collect log file data you are interested in.

EVE-NG Community Cookbook 
Version 1.11 
Page 157 of 165 © EVE-NG LTD 
 
11.6 EVE cli diagnostic info 
Use EVE cli to obtain your EVE information: 
eve-info

EVE-NG Community Cookbook 
Version 1.11 
Page 158 of 165 © EVE-NG LTD 
12 Images for EVE 
Images must be uploaded and prepared before they can be used in labs . The best way to 
upload images is to use the  WinSCP tool for Windows environment or FileZilla for MAC  OSX 
and Linux. 
Link to download WinSCP: 
https://winscp.net/eng/download.php 
Link to download FileZilla: 
https://filezilla-project.org/ 
To access EVE, use SSH protocol (port 22).  
Supported images for EVE are stored in the three locations: 
• IOL (IOS on Linux), /opt/unetlab/addons/iol/bin/ 
• Dynamips images, /opt/unetlab/addons/dynamips 
• Qemu images, /opt/unetlab/addons/qemu 
12.1 Qemu image naming table 
 IMPORTANT NOTE: Intel VT-X/EPT must be enabled to run Qemu nodes in EVE. For 
information on how to enable this option, Refer to section 3: EVE Installation. 
The directory names used for QEMU images are very sensitive and must match the table below 
exactly in order to work. 
 
Ensure your image folder name starts as per the table. After the " -" you can add whatever you 
like to label the image. We recommend using the version of your image.  
 
Folder name examples: 
 
firepower6-FTD-6.2.1 
acs-5.8.1.4 
 
The image hdd inside the folder must be named correctly: 
Example: hda.qcow2 or virtioa.qcow2 
 
Full path Example: opt/unetlab/addons/qemu/acs-5.8.1.4/hda.qcow2 
The table of proper folder names is provided in our website: 
 
https://www.eve-ng.net/index.php/documentation/qemu-image-namings/ 
 
Supported HDD formats for the EVE images: 
lsi([a-z]+).qcow  lsia.qcow

EVE-NG Community Cookbook 
Version 1.11 
Page 159 of 165 © EVE-NG LTD 
hd([a-z]+).qcow  hda.qcow 
virtide([a-z]+).qcow  virtidea.qcow 
virtio([a-z]+).qcow  virtioa.qcow 
scsi([a-z]+).qcow  scsia.qcow 
sata([a-z]+).qcow  sataa.qcow 
12.2 How to prepare images for EVE 
How to add EVE-NG images please refer to: 
https://www.eve-ng.net/index.php/documentation/howtos/ 
12.3 How to add custom image template 
12.3.1 Templates folder choice 
 IMPORTANT NOTE: Starting from EVE-Community Version 2.0.3-107, EVE installation is 
autodetecting what kind of CPU manufacturer has your server: Intel or AMD, to choose proper 
templates set. You can check it manually on EVE cli: example below, showing that EVE has 
Intel CPU. 
root@eve-ng:~# lsmod | grep ^kvm_ 
kvm_intel             212992  74 
root@eve-ng:~# 
 
• If you have Intel CPU, then your template files are in 
"/opt/unetlab/html/templates/intel/" 
• If you have AMD CPU, then your template files are in 
"/opt/unetlab/html/templates/amd/" 
12.3.2 Prepare template file 
 NOTE: For templates development use templates folder which is matching your EVE server 
CPU manufacturer. 
Example below will be based for Intel CPU EVE custom image template. Use EVE cli or 
WinSCP/Filezilla to create template. 
Step 1:  Navigate to EVE location: /opt/unetlab/html/templates/intel/ 
Step 2: Choose your most suitable template from which you want to create your own image 
template. (example: newimage.yml)

EVE-NG Community Cookbook 
Version 1.11 
Page 160 of 165 © EVE-NG LTD 
 
Step 3: Make a copy from source template newimage.yml. Example: Using CLI create template 
and name it ngips.yml. 
cp /opt/unetlab/html/templates/intel/newimage.yml /opt/unetlab/html/templates/intel/ngips.yml 
You can create new template using WinSCP or Filezilla as well. 
 
IMOPRTANT: The new name of your template will be related to your image foldername. You r 
image foldername must start with prefix “ngips- “ 
Example: image foldername under /opt/unetlab/addons/qemu/ngips-6.5.0-115 
 
12.3.3 Prepare interface format and name lines 
EVE Community has included option to create various interface names, sequences and 
numbering. Please refer table below.  
Formula Template line format 
example 
Will 
produce

EVE-NG Community Cookbook 
Version 1.11 
Page 161 of 165 © EVE-NG LTD 
eth_format: <prefix>{<first value for slot: example 
1>}<separator>{<first value for port>-<number of 
port per slot: example 8>} 
eth_format: Gi{1}/{0-8} 
Gi1/0 
Gi1/1 
Gi1/2 
Gi1/3 
Gi1/4 
Gi1/5 
Gi1/6 
Gi1/7 
Gi2/0 
Gi2/1 
.... 
eth_format: <prefix>{<first value for slot:  example 
0>}<separator>{<first value for port> -<number of 
port per slot: example 4>} 
eth_format: Ge{0}/{0-4} 
Ge0/0 
Ge0/1 
Ge0/2 
Ge0/3 
Ge1/0 
Ge1/2 
Ge1/3 
Ge2/0 
Ge2/1 
Ge2/2 
.... 
eth_format: <prefix>{<first value>} eth_format: Gi{0} 
Gi0  
Gi1 
Gi2 
Gi3 
... 
eth_format: <prefix>{<first value>} eth_format: G0/{0} 
G0/0  
G0/1 
G0/2 
G0/3 
... 
eth_name: <prefix: Interface custom name> 
eth_name: 
- M1 
- T1 
- T2 
M1 
T1 
T2 
eth_name: <prefix: Interface custom name> 
eth_name: 
- MGMT 
- DATA 
- TRAFFIC 
MGMT 
DATA 
TRAFFIC 
 
Combined first named interface following by formatted interfaces Example: We have to 
set first node interface name “eth0/mgmt” and next following interfaces must start from eth1 
and change sequence accordingly. eth1, eth2,….,ethx 
As your node first interface will  be custom named (eth0/mgmt), therefore in the template 
“eth_name:” must be added before “eth_format:”

EVE-NG Community Cookbook 
Version 1.11 
Page 162 of 165 © EVE-NG LTD 
eth_name: 
- eth0/mgmt 
eth_format: eth{1} 
 
This adding will produce Node interfaces. 
 
12.3.4 Edit your new template file: 
For edit newly created template you ca n use WinSCP, FileZilla or cli. Example below shows 
template edit using cli and nano editor 
cd /opt/unetlab/html/templates/intel/ 
nano ngips.yml 
 
Change content , setting for various images can vary depends of vendor re quirements. The 
interface name lines please refer Section: 12.3.1 
 
 
# Copyright (c) 2016, Andrea Dainese 
# Copyright (c) 2018, Alain Degreffe 
# All rights reserved. 
# 
# Redistribution and use in source and binary forms, with or without 
# modification, are permitted provided that the following conditions are met: 
#     * Redistributions of source code must retain the above copyright 
#       notice, this list of conditions and the following disclaimer. 
#     * Redistributions in binary form must reproduce the above copyright 
#       notice, this list of conditions and the following disclaimer in the 
#       documentation and/or other materials provided with the distribution. 
#     * Neither the name of the UNetLab Ltd nor  the name of EVE-NG Ltd nor the 
#       names of its contributors may be used to endorse or promote products 
#       derived from this software without specific prior written permission. 
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND 
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED 
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE 
# DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY 
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; 
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND 
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT 
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS 
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

EVE-NG Community Cookbook 
Version 1.11 
Page 163 of 165 © EVE-NG LTD 
--- 
type: qemu 
name: NGIPS 
description: Cisco FirePower NGIPS 
cpulimit: 1 
icon: IPS.png 
cpu: 4 
ram: 8192 
ethernet: 3 
eth_name: 
- eth0/mgmt 
eth_format: eth{1} 
console: vnc 
shutdown: 1 
qemu_arch: x86_64 
qemu_version: 2.4.0 
qemu_nic: e1000 
qemu_options: -machine type=pc,accel=kvm -serial none -nographic -no-user-config  
  -nodefaults -display none -vga std -rtc base=utc -cpu host 
... 
 
Note: Qemu options in the line may vary per image requirements. Please check manufacturer 
advice how to run KVM image 
12.3.5 Prepare new icon for your template: 
Step 1 Use Filezilla or Winscp to copy your custom icon IPS.png (icon  filename IPS.png used 
in ngips.yml) 
 
This icon should be about 30-60 x 30-60 in the png format (switch.png is for example 65 x 33, 
8-bit/color RGBA) 
 
Step 2 Copy this new icon into /opt/unetlab/html/images/icons/ 
12.3.6 Template use 
 
Step 1 Create directory /opt/unetlab/addons/qemu/ngips-6.2.83  
 
mkdir /opt/unetlab/addons/qemu/ngips-6.2.83  
 
Step 4.2 Upload image NGIPS, Refer Section: 
  
 
 
12.4 How to hide unused images in the node list 
12.4.1 Creating new config.php file 
If your EVE Serve r does not have the config.php file in the /opt/unetlab/html/includes/ 
directory, then it must be created. 
Step 1. Use the EVE CLI. Make sure you are in the following EVE directory: 
/opt/unetlab/html/includes/ 
Step 2. Rename config.php.distributed (the template) to config.php.

EVE-NG Community Cookbook 
Version 1.11 
Page 164 of 165 © EVE-NG LTD 
mv config.php.distribution config.php 
 
12.4.2 Edit config.php file 
Step 1. Use vi or nano file editor to edit your config.php file. 
nano config.php 
 
Step 2. Edit the config.php file, uncomment and adjust to your TEMPLATE _DISABLED settings 
(see screenshot below). 
“hided” will remove unloaded/empty image templates from nodes list in WEB GUI  
“missed” will show you all available templates in EVE WEB nodes list 
Example below will give you result: 
<?php 
// TEMPLATE MODE .missing or .hided 
DEFINE('TEMPLATE_DISABLED','.hided') ; 
?> 
 
You are seeing only templates with loaded images.

EVE-NG Community Cookbook 
Version 1.11 
Page 165 of 165 © EVE-NG LTD 
13 EVE Resources 
For additional updated information please follow our web site: https://www.eve-ng.net 
How to updates: https://www.eve-ng.net/index.php/documentation/howtos/ 
How to videos: https://www.eve-ng.net/index.php/documentation/howtos-video/ 
FAQ: https://www.eve-ng.net/index.php/faq/ 
Live support chat: https://www.eve-ng.net/index.php/live-helpdesk/ 
For access to live chat use your Google account or create new chat account. 
EVE forum: https://www.eve-ng.net/forum/ 
To access forum resources, please create a new forum account. 
EVE YouTube channel: 
https://www.youtube.com/playlist?list=PLF8yvsYkPZQ0myW7aVMZ80k8FU04UUgjV  
EVE Professional downloads: https://www.eve-ng.net/index.php/download/ 
EVE Community version downloads, free: https://www.eve-ng.net/index.php/community/ 
EVE Supported images: https://www.eve-ng.net/index.php/documentation/supported-images/