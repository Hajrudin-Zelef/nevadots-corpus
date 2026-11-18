VLAN Membership Policy Server (VMPS) was a technology that, at a point in time, provided a way for organizations to control access to their networks. A few organizations embraced it and after a few years they found it to be an albatross around their neck. However, getting away from VMPS was not so easy and the longer they delay, the harder the breakup becomes.

VMPS is a Cisco proprietary solution for providing users access to a network based on their MAC address. VMPS is available on Cisco 4000/4500/5000/6000/6500 switches. As part of the configuration of VPMS a large table is constructed with MAC addresses and VLANs of valid network attached devices. This VMPS database is placed on a switch in the network environment that acts like a VMPS server for other switches to query. When computers turns on and activate their NICs the access switches in the environment use VLAN Query Protocol (VQP) that uses UDP port 1589 to determine the VLAN name for the end-user’s MAC address and then that user is assigned to that VLAN. The VMPS access system dynamically assigns an Ethernet switchport to a specific VLAN based on the user’s MAC address. This database is maintained manually and, based on the size of the organization, is often times updated anywhere from 1 to 10 times each day. The VMPS download server is a Cisco Ethernet switch and the vmps.cfg file is a file that is transmitted with TFTP to the VMPS servers. It is also possible to have a primary and a backup VMPS server for high availability.

If you are curious here is the Cisco guide for configuring VMPS on a 6500 running CatOS 8.7.

Here is a guide for Troubleshooting the Catalyst VMPS Switch.

Many others have written descriptions of VMPS. From Sean Convery’s Cisco Press Book here is what he says this about VMPS.

**Issues with VMPS:**

This system does have a couple of shortcomings. If an attacker knew what they were doing they could still assign a static IP address to their system with a locally administered MAC address to subvert this system. This system also has lots of historical MAC addresses entered into it that haven’t been removed. Therefore, an audit needs to be performed of the MAC addresses that are still valid. One way to do this would be to pull the CAM table today and then saying everything in the network today is permitted.

One issue with VMPS servers is that this protocol consumes a lot of processor power depending on the number of users and the size of the database. I have seen organizations with literally thousands of users and many thousands of entries in the database. That is because few organizations have the discipline to remove anything from the database once it is entered. If you are using VMPS and you are noticing these messages on your VMPS server then you may have a problem. The logs can show many messages related to VQP having issues. Messages like this can be abundant in the syslogs or on a CiscoWorks server.

2009 Jan 05 15:46:26 MDT -07:00 %IP-6-UDP_SOCKOVFL:UDP socket overflow from Source IP: 192.168.126.92, Destination port: 1589 The Cisco Error Message Decoder says that these messages are related to the following problems.

1. %IP-6-UDP_SOCKOVFL: UDP socket overflow from Source IP:[chars], Destination port:[dec] This message indicates that all buffers for a UDP socket on the Network Management Processor (NMP) have filled up due to excessive UDP traffic on the administrative VLAN and cannot store additional traffic. [chars] is the source IP address and [dec] is the destination port number.

Recommended Action: Remove or block the source of the UDP packets to prevent further UDP packet loss. Note Kernel messages do not indicate a problem with system performance but should be reported to your technical support representative.

Other issues with VMPS involve the vmps.txt file consuming space on the flash filesystem of the VMPS server. This experience is documented on Jonboys Blog .

The problem arises in the fact that in order to use VMPS you have to use CatOS. VMPS is not supported in Cat IOS. Therefore, any organization that has chosen VMPS has been stuck without an upgrade path. This technology obsolescence problem has prevented many organizations using VMPS from being able to use any newer feature in Cat IOS. Furthermore, VMPS has now been deprecated by Cisco and it is not recommended for customer use. In fact, organizations that use VMPS are frozen on the version of software currently used on their switches because newer versions don’t support VMPS. Therefore, organizations using VMPS are forced to choose between VMPS and security if a vulnerability is found on the current version of switch software.

**DISA has Dissed VMPS:**

The Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIGs) for Network Security doesn’t recommend its use for DOD organizations. The DISA STIG has several paragraphs on VPMS on pages 76-78. The concluding sentence ends with “For these reasons, the U.S. DOD believes that VMPS must not be used to provide port authentication or dynamic VLAN assignment.”

**What Options do VMPS Users Have?**

There are several other alternatives to VMPS and organizations are encouraged to explore other options that will have future support by Cisco and are also based on industry standards. What organizations are really looking for is a way to easily perform Network Access Control (NAC) and prevent unauthorized users from accessing the network. There are numerous styles and flavors of NAC solutions on the market. Because there are many different approaches it may be hard to differentiate them. Each system has a different way of assessing the security of the end-point and they also have different ways of containing security threats. The table below covers the handful of basic forms of policy enforcement that NAC products use.

**Enforcement Techniques:**

IEEE 802.1X The Ethernet switch only opens up the port if the end-point is properly authenticated and healthy. This is common for wireless LANs. The downside is that each end-point needs a supplicant.

VLAN Steering This technique assigns the user’s LAN switchport to a specific VLAN (guest, remediation, intranet,…). The command and control function of the NAC system must interact with the LAN switch.

DHCP Lease Management The NAC system controls the IP address that the end-point receives through DHCP

ARP Poisoning Uses ARP to control which hosts can communicate by modifying the binding of IP addresses to MAC addresses

DNS Redirection Redirects all DNS requests toward the web portal to guide a user to the authentication system

Inline Blocking A NAC system that is in-line between the computer and the core of the network can stop a specific client from communicating with the rest of the network. The closer the NAC system is to the end-point the more granular the control.

DHCP and ARP Poisoning are two of the techniques used to control which endpoints get access to the network. The NAC system first puts a host onto a private network so that it can be assessed, then changes the host’s IP as needed. DHCP control requires little change to the underlying infrastructure and is less invasive than switch-port manipulation, VLAN steering or dynamically updating router ACLs. ARP poisoning, on the other hand, uses ARP to manage the MAC-to-IP mapping used by network hosts to communicate within a single subnet. If a host sends out an ARP packet saying it’s the network router, for example, all endpoints on that segment will send it all packets bound for other segments (note that the concept is called ARP poisoning whether it’s used for good or evil).

Either method is easily defeated by knowledgeable attackers. Using a static IP address will bypass DHCP lease management handily. ARP poisoning is a bit stronger, but on Windows hosts, using the built-in “arp -a” command will create a static ARP mapping. The tricky part is getting the network peer–a router, for example–to know what your real MAC address is. Constantly sending out directed ARP responses is one solution. Therefore, while DHCP lease management and ARP poisoning have their uses as interim enforcement methods during a NAC pilot, the better enforcement methods such as VLAN steering or 802.1X should be preferred. Exceptions to this rule would be those cases where nothing else works well, such as when the infrastructure is unmanaged or it’s too costly to deploy in-line enforcement.

Cisco Layer-3 switches have DHCP snooping and Dynamic ARP Inspection features that can help keep tabs on these LAN abuses. I often recommend these to my customers that these features be enabled even if a NAC solution is not being deployed.

The best NAC approaches perform the assessment of the end-point and check that state before the client is allowed to connect to the network. The stronger NAC solutions provide integration with the LAN switching infrastructure and use 802.1X to achieve this tight control of the edge of the network. However, when organizations have hubs it makes 802.1X infrastructure solutions difficult to deploy. These hubs will need to be removed in order to be able to deploy a NAC solution.

NAC solutions that use 802.1X don’t typically have a lot of remediation capabilities and therefore must be combined with other techniques for remediation. IEEE 802.1X is merely just an access control methodology. Worst-case solutions require manual fixing of computers once on the guest VLAN. The better solutions can automate the remediation of the hosts.

NAC systems that are in-band can potentially suffer from the large amount of bandwidth coming from Gigabit Ethernet desktops. Some of the in-band NAC solutions use high performance ASICs and some even have 10GE interfaces for connection between distribution and core network switches. Since many organization’s network today consist of mostly 10/100/1000 access ports bandwidth is of less concern. Some of the in-band solutions operate with IEEE 802.1Q on the NAC layer-2 solutions. This could be a work solution because the aggregated bandwidth for an in-band solution would be acceptable.

While there are several solutions exist as viable alternatives for VMPS the most popular replacement options for VMPS is for organizations to use Identity Based Network Services (IBNS) 802.1X MAC Authentication Bypass (MAB) or a more complete NAC solution like Cisco’s NAC Appliance. Even Cisco recommends VMPS and Cisco Secure User Registration Tool (CSURT) users migrate to 802.1X MAB.

802.1X MAB allows devices without 802.1X supplicants that connect to the access switch to have their MAC address used in the RADIUS authentication request. The RADIUS server looks up the MAC address in its database and determines if the MAC address is allowed and what VLAN it should be assigned. The RADIUS server responds to the authentication request by the access switch with that information to allow the computer access to the network. The trick is getting the MAC addresses and VLAN mapping into the RADIUS server or into an LDAP database for the RADIUS server to query.

If you are curious here is the link to the Cisco 6500 CatOS 8.7 configuration guide for 802.1X. Furthermore, here is the Cisco 6500 Cat IOS 12.2(SXH) configuration guide for 802.1X.

Here is a sample configuration for an interface on a Cat IOS switch that has been placed into 802.1X MAB mode. interface FastEthernet1/6 switchport access vlan 20 switchport mode access dot1x mac-auth-bypass dot1x pae authenticator dot1x port-control auto spanning-tree portfast spanning-tree bpduguard enable

The configuration commands have changed to a new syntax using the “authentication” command in newer versions of 12.2 on 6500s. Here is an example of a newer MAB configuration for Cat IOS 12.2(33)SXI.

Router(config)# interface FastEthernet 1/6 Router(config-if)# authentication port-control auto Router(config-if)# authentication event no-response action authorize vlan 20 Router(config-if)# dot1x pae authenticator Router(config-if)# dot1x timeout supp-timeout 3 Router(config-if)# dot1x timeout tx-period 15 Router(config-if)# dot1x pae authenticator Router(config-if)# mab [eap]

Here are some useful Cisco IOS show commands for determining if your MAB configuration is working as you expect. show dot1x all [summary] show dot1x interface show dot1x interface fastethernet1/6 details show authentication [ registrations | interface |method ] show authentication sessions [handle handle] [interface interface] [mac mac] [method method] [session-id session-id] show vlan group [group-name group-name] clear dot1x interface fastethernet 1/6 clear authentication sessions interface fastethernet 1/6 method mab

To learn more about the newest features of 802.1X in Cat IOS 12.2(33)SXI check out Jamey Heary’s blog on the topic.

If you are a current user of VMPS I wish you all the best of luck migrating to 802.1X (with or without MAB) or a NAC appliance solution.

Scott