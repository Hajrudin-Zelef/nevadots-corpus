- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-11-2011 08:44 AM - edited 03-07-2019 02:44 AM
I have a strange issue with HSRP on my Nexus7000 resulting in a Active/Active-State.
Does anyone see where the problem is founded or where I should look next?
Thx in advance and Greetings from Berne,
Stefan Mueller
Layout
- 2 Nexus 7000 with NX-OS 5.1(3) as Distribution-Switch, with all the Access-Switches attached to each Nexus, bundled with vPC.
- N7K Providing L3 with SVIs on 49 Vlans. Nexus1 always takes the IP x.11, Nexus2 is x.12. Default Gateway is x.10, provided via HSRP. 48 Vlans work out fine. 1 Vlan (with identical Configuration) has a Problem:
Issue
- Both Nexus think that they are HSRP Active on Vl 783. Standby-Router is unknown.
Config-Snippet Nexus 1
interface Vlan783
ip address 10.34.195.11/25
ip router eigrp 41
ip passive-interface eigrp 41
hsrp 1
authentication text somethingelse
preempt
priority 150
timers msec 300 msec 1000
ip 10.34.195.10
no shutdown
Config-Snippet Nexus 2
interface Vlan783
ip address 10.34.195.12/25
ip router eigrp 41
ip passive-interface eigrp 41
hsrp 1
authentication text somethingelse
preempt
priority 130
timers msec 300 msec 1000
ip 10.34.195.10
no shutdown
debug hsrp engine packet hello interface vlan 783
=> on N2 (which should be Standby. IP: .12), only the following lines are repeating:
2011 Oct 11 16:58:36.880624 hsrp: Vlan783[1/V4]: Hello out Active pri 130 ip 10.34.195.10
2011 Oct 11 16:58:36.880651 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 16:58:37.184802 hsrp: Vlan783[1/V4]: Hello out Active pri 130 ip 10.34.195.10
2011 Oct 11 16:58:37.184827 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
=> on N1 (which should be Active. IP: .11), I receive two Hellos for each Hello sent:
2011 Oct 11 17:07:56.405711 hsrp: Vlan783[1/V4]: Hello out Active pri 150 ip 10.34.195.10
2011 Oct 11 17:07:56.405735 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 17:07:56.491349 hsrp: Vlan783[1/V4]: Hello in from 10.34.195.12 State Active pri 130 ip 10.34.195.10
2011 Oct 11 17:07:56.491450 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 17:07:56.491546 hsrp: Vlan783[1/V4]: Hello in from 10.34.195.12 State Active pri 130 ip 10.34.195.10
2011 Oct 11 17:07:56.491559 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 17:07:56.705691 hsrp: Vlan783[1/V4]: Hello out Active pri 150 ip 10.34.195.10
2011 Oct 11 17:07:56.705715 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 17:07:56.791414 hsrp: Vlan783[1/V4]: Hello in from 10.34.195.12 State Active pri 130 ip 10.34.195.10
2011 Oct 11 17:07:56.791437 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
2011 Oct 11 17:07:56.791532 hsrp: Vlan783[1/V4]: Hello in from 10.34.195.12 State Active pri 130 ip 10.34.195.10
2011 Oct 11 17:07:56.791546 hsrp: Vlan783[1/V4]: hel 0 hol 0 auth somethingelse
Further Observations:
- sh ip arp: N1 sees the SVI-address of N2 and vice-versa. Both of course have a ARP-Entry for the HSRP-address
- sh mac add: N1 sees the N2-SVI-MAC on the vPC Peer-Link and vice-versa
- Both N1 and N2 can ping all involved Addresses 10.34.195.10, 10.34.195.11 and 10.34.195.12 (and all Host-addresses as well)
- Previously this morning, N1 could not ping SVI of N2 and Vice-Versa, although they could see each-other in the mac address-table (don't remember about arp-table). This also caused issues for End-Host-Traffic, notably DHCP. I then deleted hsrp-group 1, created hsrp-group 2 without authentication and with default-timers. This led to the same situation as above (Ping possible, HSRP both active), so I changed back to our standard-configuration.
- The Vlan used to work at least three weeks ago. We are not aware of any relevant changes since then (we did attach more Access-Switches via vPC-Uplinks, though).
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Other Switching
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-01-2012 04:39 AM
Hi Shane
5.1(3)N2(1) is a Nexus 5000-Release, the above Problem occured on a Nexus 7000, where 5.1(3) was another NX-OS-Generation.
AFAIK, 5.1(3)N2(1) still is considered a good Release. The bug which stood behind my issue never was present on this release.
As mentioned in my last post, I resolved my issue not with upgrading, but with disallowing/allowing the one faulty VLAN on the vPC-Link between both Nexus. Please be aware that this would not help if a) you have Dual-Active-HSRP on multiple Vlans or b) if neither of both Nexus receive HSRP-Packets from the other (debug hsrp engine packet hello)
Greetings from Berne, Switzerland
Stefan Mueller
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-02-2012 07:06 AM
Thanks Stefan, I see the error of my ways, this does not relate to 5K as you have pointed out.
My issue turned out to be related to LANBase license not installed on L3 daughter cards out of the box. Once installed, my issues were resolved.
Kind regards.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-15-2013 12:59 AM
Hi,
We encouner similar issue that 2 x N7K w/ SUP1/M1/FAB1 claim itself HSRP active after NXOS upgrade from 6.1.x to 6.2.x. Anyone has clue?
Tks
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-10-2013 06:57 PM
Leon....I just had this same issue (active/active HSRP) today on my two 7010's running 6.2.2a code. To resolve, I made sure that my VPC role priority for my vpc domain was such that my 'by design' active HSRP device and the Primary VPC role were in fact one in the same device. I then bounced my peer link port channel.
If you have a case where your hsrp config is set to a high priority on switch 'A' and your vpc role is primary on switch 'B', you may experience issues with the HSRP multicast traffic traversing the peer link due to the loop prevention methodology within vpc. At least that is my hypothesis from reading miscellaneous articles and troubleshooting threads. I did not have time to open a TAC case for a root cause but the above steps resolved my issue on 6.2.x code.
HTH
JW
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-10-2013 09:31 PM
Hi John,
Do you mean that HSRP master must be VPC primary device?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-10-2013 10:12 PM
It appears so. I also employed the peer-gateway component under my vpc domain but I don't think it was the root cause of the active/active HSRP scenario that I saw.
Take a look at this doc and I carved out the relevant quotes below:
"Features That You Must Manually Configure on the Primary and Secondary Devices
You must manually configure the following features to conform to the primary/secondary mapping of each of the vPC peer devices:
•HSRP active—If you want to use Hot Standby Router Protocol (HSRP) and VLAN interfaces on the vPC peer devices, configure the primary vPC peer device with the HSRP active highest priority. Configure the secondary device to be the HSRP standby and ensure that you have VLAN interfaces on each vPC device that are in the same administrative and operational mode. (See the "vPC Peer Links and Routing" section for more information on vPC and HSRP.)
vPC Peer Links and Routing
To simplify initial configuration verification and vPC/HSRP troubleshooting, you can configure the primary vPC peer device with the FHRP active router highest priority.
JW
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
01-18-2015 02:02 AM
I hit the same on
show version internal build-identifier
Kickstart image file: bootflash:///n7000-s1-kickstart.6.0.2.bin :  S31
System image file: bootflash:///n7000-s1-dk9.6.0.2.bin :  S31
Removing and adding the vlans solved the issue
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
12-03-2021 05:57 AM
i have/had this same issue, on a Nexus 9k, NXOS: version 7.0(3)I7(2)
no hsrp hellos were being sent to the other host.
no vpc involved here, just a regular port-channel to an identical 9k.
removing the vlan from the port-channel and then re-adding, kicked hsrp into life and the hellos immediately started sending again, thus resolving the hsrp active/active issue.
it was only affecting one specific vlan, others were unaffected.
thanks to Stefan for providing the answer.
this bug still seems to be lurking in NX-OS.
- « Previous
- Next »