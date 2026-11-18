White Paper
Air Marshal 
SEPTEMBER 2013
This document discusses potential security threats in a WiFi 
environment, and outlines how enterprises can use a best-in-class 
Wireless Intrusion Prevention System (WIPS) such as Meraki’s Air 
Marshal to create a secure network with preemptive protection policies 
and security alerts.

Copyright 
© 2013 Cisco Systems, Inc. All rights reserved
Trademarks 
Meraki® is a registered trademark of Cisco Systems, Inc. 
Table of Contents Introduction     3  
Wireless Threats     5  
Threat Remediation     7  
Configuration      10  
Conclusion      13  
 
 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
2

Introduction
Wireless Security Threats in an Enterprise Environment
Secure WiFi access has become a critical component of enterprise networking. WiFi Internet 
access is critical for corporate communication in verticals including financial services, retail, and 
distributed enterprise. Due to the widespread use of WiFi and variety of use cases (e.g., point-of-
sale (POS) communications, corporate access, warehouse inventory, asset tracking, WiFi services 
for targeted advertising), the wealth of information transmitted across the wireless medium 
has skyrocketed. Data transmitted over wireless increasingly contains sensitive personal and 
financial data. Unfortunately, the tremendous growth in wireless has been accompanied with 
an increasingly widespread ability to obtain open-source hacking tools that can compromise a 
wireless network through impersonation of client devices and access points.
Examples of common threats in a modern WiFi environment include:
Network impersonation: achievable by purchasing any consumer-grade access point and 
copying an SSID, “tricking” clients into thinking that this SSID is available and snooping on their 
information transactions. 
Figure 1: Example of SSID spoofing threat in a retail environment
Legitimate SSID Malicious SSID Unsuspecting user  
connects to malicious 
SSID
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
3

Wired network compromise: achieved by an unsuspecting employee or student plugging in a 
consumer-grade access point into the wired infrastructure and exposing the LAN to hackers.
To successfully protect an enterprise network, a Wireless Intrusion Prevention System (WIPS) 
should provide powerful wireless intrusion scanning capabilities, enabling detection and 
classification of different types of wireless threats, including rogue access points and wireless 
hackers. Access points can be put in either dedicated scanning mode or sensor mode for 
real-time intrusion detection and threat remediation. Additionally, a WIPS system should be 
configurable with intuitive auto-containment policies to facilitate pre-emptive action against rogue 
devices. Once a threat has been detected, the WIPS platform should kick into gear to enact 
powerful policies, including intelligent auto-disablement of APs matching a pre-defined criteria 
and generating different tiers of e-mail alarms based on the type of threat in your airspace. 
In addition to protecting airspace against hackers with malicious intent, it is valuable to be able 
to mark or group high-value ‘VIP’ wireless clients into a special category, where they can be 
tracked, to ensure they never leave the wireless network. Examples of VIP clients include high-
value corporate assets — these devices belong to the organization and should never associate 
to a wireless network other than your own (e.g., corporate issued laptops, point-of-sale registers 
or barcode scanners in a retail environment, etc.) These VIP clients should only associate to the 
corporate network. If they do stray to a foreign network, it would be classified as an ‘accidental 
association.’ The ability to detect and generate alerts when a VIP client strays over to a rogue 
infrastructure can be invaluable in security conscious environments.
Cisco Meraki’s Air Marshal mode allows network administrators to meet these requirements and 
design an airtight network architecture that provides an industry-leading WIPS platform in order to 
completely protect the airspace from wireless attacks. The remainder of this document describes 
in greater depth wireless threats and the necessary security measures required to remediate 
against these threats; the conclusion then summarizes the setup and configuration process for 
Meraki’s Air Marshal WIPS platform in order to achieve the highest security protection possible. 
 
Figure 2: Example of accidental wired LAN compromise in corporate environment
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
4
Internet
Corporate network
Unsuspecting employee 
plugs in home AP to create 
wireless access
User gains access to 
corporate LAN resources

Wireless Threats
Understanding the wireless airspace around you can help to take effective measures, both 
preventive and reactive, to ensure that the wireless airspace is secure and interference-free 
from other wireless networks. A number of different threats exist in the modern enterprise 
environment, facilitated by easy access to cheap consumer-grade 802.11 equipment, along with 
open-source hacking tools that can be used to simulate and spoof devices and generate traffic 
floods. Leading enterprise WLAN providers such as Cisco provide built-in WIPS features to ensure 
detection and remediation against these threats. 
Threat classifications
Visibility and classification of potential wireless threats is an important first step in securing the 
wireless network and network infrastructure as a whole. Once classified, remediation can be 
taken against confirmed threats and innocuous alerts can be dismissed. Cisco Meraki Air Marshal 
automatically classifies threats into the following categories to provide the greatest visibility and 
overall protection for your network.
Rogue SSIDs
SSID and AP spoofs: the malicious impersonation of a legitimate AP by either spoofing the SSID 
name or, even worse, the SSID name and the BSSID (the wireless MAC address, which makes it 
indistinguishable from the original AP).
Rogue SSID seen on LAN: SSIDs that are broadcast by rogue APs and seen on wired LAN; this 
could suggest compromise of the wired network.
Other SSIDs
Interfering SSIDs: wireless networks that are broadcasting and could be causing RF interference, 
as well as attracting accidental associations from clients who are supposed to be connecting to 
your own network. 
Ad-Hoc SSIDs: modern smartphones and mobile devices are capable of associating to WiFi 
networks and then re-broadcasting the SSID, essentially acting as a wireless bridge. Devices in 
ad-hoc mode can connect to a client AP and create a gateway for wireless hackers.
Malicious Broadcasts
Denial of Service (DoS) attacks are attempts to prevent clients from associating to the legitimate 
AP by sending an excessive number of broadcast messages to clients. DoS attacks could be from 
malicious clients, APs, or even another WIPS system in the area that considers the corporate 
network a threat and is attempting to remediate.
Packet Floods
Clients or APs that are sending an excessive number of packets to your AP. Packets are 
monitored and classified based on multiple categories including beacon, authentication and 
association frames. An excessive number of any category of packets seen within a short time 
interval will be marked in Air Marshal as a packet flood. 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
5

Client Straying Threats
Accidental associations: client devices that belong to your infrastructure associating to a wireless 
network in your airspace that has not been sanctioned by your corporation. Straying clients could 
accidentally connect to Rogue SSIDs or spoofed SSIDs if proper action is not taken to protect the 
wireless airspace.  
PCI Compliance
Understanding and remediating against wireless threats is also a requirement under the Payment 
Card Industry Data Security Standard (PCI DSS), a standard required for retailers to follow when 
processing credit card data over WLAN networks. Examples of WIPS requirements under PCI DSS 
include:
Section 9.1.3 Physical Security: Restrict physical access to known wireless devices.
Section 10.5.4 Wireless Logs: Archive wireless access centrally using a WIPS for 1 year.
Section 11.1 Quarterly Wireless Scan: Scan all sites with card dataholder environments (CDE) 
whether or not they have known WLAN APs in the CDE. Sampling of sites is not allowed. A WIPS 
is recommended for large organizations since it is not possible to manually scan or conduct a 
walk-around wireless security audit of all sites on a quarterly basis
Section 11.4 Monitor Alerts: Enable automatic WIPS alerts to instantly notify personnel of rogue 
devices and unauthorized wireless connections into the CDE.
Section 12.9 Eliminate Threats: Prepare an incident response plan to monitor and respond to 
alerts from the WIPS. Enable automatic containment mechanism on WIPS to block rogues and 
unauthorized wireless connections.
Figure 3: Security threats in an enterprise environment
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
6
Site A
Client AP
Rogue AP Client Laptop 
‘Accidental association’  
High-value asset associations 
to wrong network
Client AP
Ad Hoc 
Devices in ad-hoc mode can 
connect to a client AP and 
create a gateway for wireless 
hackers
Wireless Hacker
Rogue AP 
Rogue APs on  
the wired LAN can  
compromise your 
entire wired and 
wireless network
Site B

Threat Remediation using Meraki’s  
Air Marshal WIPS platform
A careful study of the common wireless security threats has led to the development of Meraki’s 
Air Marshal platform, which allows access points to be turned into dedicated WIPS sensors called 
‘Air Marshal’ APs. Air Marshal is a WIPS platform which comes equipped with security alerting and 
threat remediation mechanisms. This includes the following:
a. Monitoring and alerting: a robust and intuitive display of all of the threats for a particular 
network, including auto-alerting based on the network administrator’s preferences. Monitoring 
techniques include:
i. Rogue AP monitoring: Meraki APs scan across all 2.4 GHz and 5 GHz channels to build 
a list of rogue access points in the nearby vicinity. In addition, further mechanisms are in 
place to track APs on the wired LAN network by inspecting traffic on the wired port of the 
Meraki AP, and using this to build a list of rogue APs that may be on the wired LAN. E-mail 
alerts will be triggered and sent based on parameters predefined by the network admin. 
ii. Tracking ‘client straying’ of VIP clients: Air Marshal allows tagging of VIP clients and 
an alert is sent if those clients connect to a unsanctioned SSID. Air Marshal does this 
by monitoring traffic with the source MAC address of the VIP clients. Wireless devices 
communicate with three types of 802.11 frames: management frames are used during the 
probing and association process. Control and data frames are used when the client is 
actually connected. If Air Marshal sees data frames originating from VIP clients which are 
not connected to the corporate wireless network, an alert can be sent to administrators for 
remediation.  
b. Remediation mechanisms: Air Marshal APs come equipped with the ability to automatically 
‘contain’ rogue APs and alert on rogue APs and accidental associations, allowing for 
administrators to take physical action to remove rogue APs and recover straying devices. 
Figure 4: Tracking accidental associations 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
7
Client accidentally  
associates to Rogue AP
Air Marshal AP detects 
data frames exchanged
Email alert sent to  
network administrator

What is containment?
‘Containment’ is a common mechanism that calls for the Air Marshal AP to impersonate or spoof 
the rogue AP in order to render it ineffective. Air Marshal does this by generating a large number 
of 802.11 packets and using the BSSID of the rogue AP as the the source MAC address. Air 
Marshal APs also provides more sophisticated containment methods including spoofing clients 
attempting to associate to the rogue by generating packets with the source MAC of the clients; 
this allows for a ‘two-way’ spoof and ensures a fool-proof shutdown of the rogue AP. 
Packet types generated by WIPS during containment: 
1. 802.11 Broadcast deauthorizations with source = Rogue AP, destination = broadcast 
2. 802.11 Deauthorization messages with source = Rogue AP, destination MAC = client  
3. 802.11 Deauthorization and disassociate messages with source = client, destination = Rogue AP 
#3 ensures that more sophisticated 802.11 clients with battery-saving capabilities are also unable 
to connect to the rogue, as they may ignore deauthorization messages from the Rogue AP if they 
are ‘sleeping’ in order to save battery life. 
2As containment renders any standard 802.11 network completely ineffective, containment 
measures should taken in your airspace. Extreme caution should be taken to ensure that 
containment is not being performed on a legitimate network nearby and, action should only be 
taken as a last resort. Unauthorized containment is prosecutable by law (subject to the FCC’s 
Communications Act of 1934, Section 333, ‘Willful or Malicious Interference’).  
http://transition.fcc.gov/Reports/1934new.pdf
Figure 5. Rogue AP ‘Containment’ explained 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
8
802.11 packets
Packet Types
1. Rogue AP sending broadcast 
deauthorizations
2. Rogue AP deauthorizing client
3. Client deauthorizing rogue AP
Air Marshal AP
Rogue AP thinks client is 
requesting deauthorization
Client thinks rogue AP is  
forcing deauthorizing

Figure 6 - Example of containment technique - 802.11 Deauthentication packet
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
9
iPhone client accidentally 
associating to Rogue AP
802.11n packet type is 
deauthentication, source is 
Rogue AP and destination 
is client
Repeated deauthentications  
sent within short time period
Dummy packet is generated 
by our WIPS sensor, looks 
like it came from Rogue AP

Configuring Meraki’s Air Marshal  
WIPS platform
Dual-radio Meraki APs will run wireless scans opportunistically while also serving clients; this 
means they will scan the channel on which they are serving clients. It is possible to schedule 
‘mandatory’ scans to be run at pre-specified time intervals that can be set as frequently as once 
a day. For users requiring more accurate and real-time wireless threat assessments, it is possible 
to place an AP in Air Marshal mode. While acting as an Air Marshal, an AP will use its radios as 
dedicated scanners to monitor its surrounding environment in real-time. For the dual-radio APs, 
this includes both the 2.4GHz and 5GHz frequencies. Newer Meraki APs include a third radio 
which comes pre-configured for permanent Air Marshal scanning. These APs do not require any 
Air Marshal configuration and will scan and remediate against threats in real-time. 
Air Marshal mode can be switched on by selecting the relevant APs on the Access Points page. 
By clicking the relevant AP and selecting ‘On’ under the Air Marshal scanning section, it is 
possible to designate this AP as a dedicated WIPS scanner. This Air Marshal AP will now be a 
dedicated sensor performing scans of the surrounding environments for threats, the results of 
which will be displayed on the WIPS page in real-time. 
Figure 7 - Configuring Air Marshal APs on the Wireless > Access points page
A note on hybrid vs. dedicated scanners
APs with two radios running in client-serving mode will only scan the airspace opportunisti-
cally; this means they will scan the client-serving channel in real-time, and will scan across 
all channels either once a day or when no clients are being served. Most WLAN vendors 
recommend having dedicated scanning sensors (with no clients being served) in security-
conscious environments, to ensure real-time security alerting and protection. Some vendors 
offer ‘time slicing’ which allows cross-channel scans while serving clients, but this sacrifices 
performance of latency-sensitive applications such as VoIP and is generally not recom-
mended in the industry. For this reason, Meraki recommends placing an AP in dedicated  
Air Marshal mode (or utilizing newer 3-radio APs) for real-time scanning.
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
10
iPhone client accidentally 
associating to Rogue AP
iPhone client accidentally 
iPhone client accidentally 
Select an AP and tag it  
‘airmarshal’
3-radio APs come  
pre-equipped with  
Air Marshal radio
Shield icon indicates 
Air Marshal status

Figure 8 - Monitoring Air Marshal Page
On the Wireless > Air Marshal page, a number of manual actions of automated policies can be 
set as a response to the detection of certain types of wireless threats based on administrator 
preferences:
1. Manual rogue containment: when choosing to ‘contain’ a rogue SSID, the Meraki AP will 
perform containment (as described in ‘Threat Remediation’ section of this document) to 
render the rogue AP ineffective. Certain rogue SSIDs known as friendly APs can also be 
whitelisted to avoid confusion in the future. 
2. Automated LAN and Keyword Containment: automated policies can also be set to contain 
rogues seen on the wired network, as well as rogue APs matching a certain keyword. For 
example, if “Acme” is specified as a keyword and a Rogue SSID begins broadcasting an 
SSID named “AcmeCorp”, it will automatically be contained and clients will not be able to 
associate with it. This can be helpful in detecting people who are trying to copy the network 
with similar names and ‘trick’ clients into associating with their own AP.
3. Mandatory scan schedule: set time and days of the week where non-Air Marshal APs should 
scan all channels to ensure daily scanning. 
4. On the Wireless > Group policies page, a special policy can be created to track accidental 
associations by VIP clients; simply select the ‘track clients straying’ policy attribute and save 
the policy. The policy can then be applied to specific clients on the Clients page, and devices 
can also be pre-staged with their MAC addresses to have this policy automatically applied 
upon association by using the ‘Add devices’ function on the page. 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
11
Configure LAN  
containment
Manual Containment/
Whitelist
Specify exact or key-
word matches
Add data on rogues: 
-VLAN 
-Manufacturer 
-Wired/wireless MAC 
-RSSI 
-Encryption type
Outlines # of APs in Air Marshal 
mode and with 3rd scanning radio

Figure 9 - Creating a Client tracking group policy
5. Generic alerts for Rogue APs can be set on the ‘Alerts and Administration’ page, allowing 
administrators to receive automatic alerts when rogue APs are detected that either match 
specified keywords or are seen to be on the wired LAN. 
Creating a WIPS response plan
By configuring alerts and utilizing Meraki’s Air Marshal view to monitor these threats retroactively 
and in real-time, it is possible to build a robust security plan that can be enforced. An example of 
a complete security methodology is as follows:
1. Create a WIPS plan as per your company’s security policies 
(i) Configure mandatory scanning intervals or designate APs to run in Air Marshal mode 
(ii) Configure auto-containment policies for rogue SSID keyword matches or rogues on the 
wired LAN 
(iii) Configure client straying policies to track batches of VIP clients 
(iv) Configure WIPS alerts
2. Proactive monitoring of Air Marshal 
(i) Visit Air Marshal page weekly or quarterly and mark known rogues as ‘whitelisted’, contain 
dangerous rogues 
(ii) Physically contain rogues that may be a threat
3. Reactive monitoring of Air Marshal alerts 
(i) Receive alert and react accordingly (set containment, find and contain rogue, etc). 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
12
Create and apply a group  
policy to track specific clients

Conclusion
By understanding the spectrum of wireless security threats in today’s environment and creating 
a comprehensive response plan, network administrators can preclude the possibility of a serious 
compromise of critical network assets — including access to secure network devices that belong 
to the enterprise. A best-in-class WIPS platform should be capable of delivering intuitive reporting 
and monitoring, along with a robust suite of tools allowing for automatic alerts and security 
enforncement.
Meraki’s Air Marshal system includes real-time detection, remediation and alerting capabilities, 
including the ability to define pre-emptive policies that will intelligently take action to shoot down 
rogue APs using sophisticated containment mechanisms. Meraki’s wireless portfolio contains 
both dual-radio APs which can be converted into full-time sensors running in Air Marshal mode, 
and three-radio APs with dedicated scanning radios permanently running as Air Marshal scanners. 
By utilizing Meraki access points and Meraki’s intuitive web-based Dashboard interface, network 
administrators can create a robust WIPS policy plan, and easily deploy an airtight network to 
deliver enterprise-grade security in a WLAN environment. 
Cisco Systems, Inc.  |  500 Terry A. Francois Blvd, San Francisco, CA 94158  |  (415) 432-1000  |  sales@meraki.com
13