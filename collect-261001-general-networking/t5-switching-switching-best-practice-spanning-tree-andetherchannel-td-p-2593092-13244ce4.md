- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-22-2014 11:21 AM - edited 03-07-2019 09:37 PM
Dear All,
Regarding best practice related to Spanning Tree and Etherchannel, we have decided to configure following.
1. Manually configure STP Root Bridge.
2. On end ports, enable portfast and bpduguard.
3. On ports connecting to other switches enable root guard.
In etherchannel config, we have kept mode on on both side, need to change to Active and desirable as I have read that mode on may create loops? Please let me know if this is OK and suggest if something missing.
Thank You,
Abhisar.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Other Switching
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-22-2014 12:12 PM
Hi Abhisar,
Regarding your individual decisions: Manually configuring the Root Bridge is a natural thing to do. You should never leave your network just pick up a root switch based on default switch settings.
On end ports, using PortFast and BPDU Guard is a must especially if you are running Rapid PVST+ or MSTP.
Regarding the Root Guard on ports to other switches - this is something I do not recommend. The Root Guard is a protective mechanism in situations when your network and the network of your customer need to form a single STP domain, yet you want to have the STP Root Bridge in your network part and you do not want your customer to take over this root switch selection. In these cases, you would put the Root Guard on ports toward the customer. However, inside your own network, using Root Guard is a questionable practice. Your network can be considered trustworthy and there is no rogue root switch to protect against. Using Root Guard in your own network could cause your network to be unable to converge on a new workable spanning tree if any of the primary links failed, and it would also prevent your network from converging to a secondary root switch if the primary root switch failed entirely. Therefore, I personally see no reason to use Root Guard inside your own network - on the contrary, I am concerned that it would basically remove the possibility of your network to actually utilize the redundant links and switches.
Regarding EtherChannels - yes, you are right, using the on mode can, under circumstances, lead to permanent switching loops. EtherChannel is one of few technologies in which I wholeheartedly recommend on relying on a signalling protocol to set it up, as opposed to configuring it manually. The active mode is my preferred mode, as it utilizes the open LACP to signal the creation of an EtherChannel, and setting both ends of a link to active helps to bring up the EtherChannel somewhat faster.
If you are using fiber links between switches, I recommend running UDLD on them to be protected against issues caused by uni-directional links. UDLD is not helpful on copper ports and is not recommended to be run on them. However, I strongly recommend running Loop Guard configured globally with the spanning-tree loopguard default. Loop Guard can, and should, be run regardless of UDLD, and they can be used both as they nicely complement each other.
My $0.02...
Best regards,
Peter
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-22-2014 12:12 PM
Hi Abhisar,
Regarding your individual decisions: Manually configuring the Root Bridge is a natural thing to do. You should never leave your network just pick up a root switch based on default switch settings.
On end ports, using PortFast and BPDU Guard is a must especially if you are running Rapid PVST+ or MSTP.
Regarding the Root Guard on ports to other switches - this is something I do not recommend. The Root Guard is a protective mechanism in situations when your network and the network of your customer need to form a single STP domain, yet you want to have the STP Root Bridge in your network part and you do not want your customer to take over this root switch selection. In these cases, you would put the Root Guard on ports toward the customer. However, inside your own network, using Root Guard is a questionable practice. Your network can be considered trustworthy and there is no rogue root switch to protect against. Using Root Guard in your own network could cause your network to be unable to converge on a new workable spanning tree if any of the primary links failed, and it would also prevent your network from converging to a secondary root switch if the primary root switch failed entirely. Therefore, I personally see no reason to use Root Guard inside your own network - on the contrary, I am concerned that it would basically remove the possibility of your network to actually utilize the redundant links and switches.
Regarding EtherChannels - yes, you are right, using the on mode can, under circumstances, lead to permanent switching loops. EtherChannel is one of few technologies in which I wholeheartedly recommend on relying on a signalling protocol to set it up, as opposed to configuring it manually. The active mode is my preferred mode, as it utilizes the open LACP to signal the creation of an EtherChannel, and setting both ends of a link to active helps to bring up the EtherChannel somewhat faster.
If you are using fiber links between switches, I recommend running UDLD on them to be protected against issues caused by uni-directional links. UDLD is not helpful on copper ports and is not recommended to be run on them. However, I strongly recommend running Loop Guard configured globally with the spanning-tree loopguard default. Loop Guard can, and should, be run regardless of UDLD, and they can be used both as they nicely complement each other.
My $0.02...
Best regards,
Peter
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-23-2014 02:15 AM
Dear Peter,
Thank you for the valuable suggestions, I will consider during configuration.
Just one clarification,
Last week I faced small outage while creating portchannel, the setup as follows
Cisco 6506 VSS (Root Bridge) connected to other switches in the network.
Parallely, I was working on creating one more separate setup with
Cisco 6807 VSS (Root Bridge) connected to 7 stacks of each two 3850s.
I connected trunk link between 6506 and 6807XL and link came up with 6506 as Root. But when creating Etherchannel (mode on) on one of the Stack of 3850s, outage came on whole network. I got following syslog on 6807
*Nov 18 09:23:42.545: %PM-SW1-4-ERR_DISABLE: channel-misconfig error detected on                                                                                         Po11, putting Te1/5/5 in err-disable state
*Nov 18 09:23:42.621: %PM-SW1-4-ERR_DISABLE: channel-misconfig error detected on                                                                                         Po11, putting Te2/5/5 in err-disable state
*Nov 18 09:23:42.689: %PM-SW1-4-ERR_DISABLE: channel-misconfig error detected on Po11, putting Po11 in err-disable state
*Nov 18 09:23:42.689: %PM-SW2_STBY-4-ERR_DISABLE: channel-misconfig error detected on Te1/5/5, putting Te1/5/5 in err-disable state
*Nov 18 09:23:42.781: %PM-SW2_STBY-4-ERR_DISABLE: channel-misconfig error detected on Te2/5/5, putting Te2/5/5 in err-disable state
*Nov 18 09:23:42.981: %PM-SW2_STBY-4-ERR_DISABLE: channel-misconfig error detected on Po11, putting Te1/5/5 in err-disable state
*Nov 18 09:23:42.981: %PM-SW2_STBY-4-ERR_DISABLE: channel-misconfig error detected on Po11, putting Te2/5/5 in err-disable state
*Nov 18 09:23:42.981: %PM-SW2_STBY-4-ERR_DISABLE: channel-misconfig error detected on Po11, putting Po11 in err-disable state
Can you please help me to find out the reason behind it, as I want to connect the trunk link back.
Thank You,
Abhisar.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-23-2014 07:57 AM
Hi Abhisar it seems like the configuration on the portchannel and physical interface does not match
te1/5/5
te2/5/5
po11
this will cause the interfaces to transition to err-disable state. If you could kindly make sure configs are same, or paste them here if you have doubt, we can peer review for you.
hope this helps
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-23-2014 10:47 AM
Dear Bilal,
I dont think its a config issue. I had applied the config on 6807 but did not applied on 3850s and suddenly error occured.
interface TenGigabitEthernet1/5/5
 switchport
 switchport mode trunk
 channel-group 11 mode on
interface TenGigabitEthernet2/5/5
 switchport
 switchport mode trunk
 channel-group 11 mode on
interface Port-channel11
 switchport
 switchport mode trunk
As I checked about the error, I found that if you config mode on on one side and dont config other side withing some time then there are chances of loops.
So I removed mode on and configured as active. Also, my VSL portchannel are on mode on and etherchannel is up, this will create any issue?
Thank You,
Abhisar.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-24-2014 08:16 AM
Hello Abhisar, your VSL requires mode ON, this is by design to my recollection and by default. Please do not change this otherwise your VSS will break.
hth
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-24-2014 11:58 AM
Hi Bilal,
You are correct in that a VSL link should use the unconditional EtherChannel mode on configuration because the links run a different protocol called Link Management Protocol:
http://www.cisco.com/c/en/us/td/docs/solutions/Enterprise/Campus/VSS30dg/campusVSS_DG/VSS-dg_ch2.html#wp1056015
However, Abhisar has not indicated whether the EtherChannel he was creating was related to the VSL link, and according to the outputs and descriptions he has provided, it does not seem that the issue was related to the VSL.
In general, what Abhisar's outputs show is a simple EtherChannel Misconfiguration Guard kicking into action. This mechanism is triggered whenever physical ports of a single Port-channel receive BPDUs sourced from different MAC addresses. In most cases, this is an indication that while this switch is already considering the physical ports to be bundled in a single Port-channel, the switch at the other end of these links is still treating the ports as individual ports. In fact, I am not surprised that the EtherChannel Misconfig Guard has been triggered in Abhisar's case: he has configured the Port-channel interface on one of the switches into the static mode on and before he could configure the other end for the mode on as well, the EtherChannel Misconfig Guard has brought down the entire Port-channel as it was receiving BPDUs from the other switch sourced from different MAC addresses.
There is a general rule for configuring the mode on Port-channels: first, switch off the ports you want to bundle on both switches, then configure both switches for the mode on, and only then turn the ports back on. Never configure the Port-channel in such a way that would allow one switch to treat the links as already bundled while the other is still unconfigured, no matter how short the time would be.
Best regards,
Peter
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-24-2014 11:43 PM
Dear Guys,
Thank you for your comments. As per Peter, yes it happened due to the Etherchannel mode on.
I was configuring etherchannel between Cisco 6807 and Stack of Two 3850, and this Stack of 3850 was root and when I configured mode on on 6807 both links went to forwarding and got looped. Now, I made 6807 as root and Etherchannel is on LACP.
Guys, request if you can help to sort out one more issue.
Now, I have two separate setup.
(Attached file)
One is 6506(VSS) connected to other access switches(5 4506 and 2 3750s) and this is working fine. The VTP domain is CISCO.
Other is Cisco 6807(VSS) connected to other switches(7 stacks of two 3850s and 3 4506) and this connected to each other via etherchannel. There is no traffic on this setup. There is no VTP domain configured. I guess this should work.
Now, when I connected the trunk link (Etherchannel LACP of 2 1G SFP)between both VSS the network gets little slow, specially browsing traffic as ping to some servers is 1ms. Why this has happened? After connecting both the setup Cisco 6506 is root as priority defined for this is 24576 and for 6807 is 28672.
Is this because 6807 bacame secondary root? Or if I keep 6807 priority to default then Stack of 3850s will become root and will connect the link between both VSS then 6506 will become as root, this can help?
If I configure VTP domain on new setup as CISCO and all switches as client, does this affect anything on network?
Can you please help what might be reason for slow.
Thank You,
Abhisar.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-25-2014 01:06 AM
Hi Abhisar,
Let me go over some of your statements as I need more clarification.
Other is Cisco 6807(VSS) connected to other switches(7 stacks of two 3850s and 3 4506) and this connected to each other via etherchannel. There is no traffic on this setup. There is no VTP domain configured. I guess this should work.
Regarding VTP, is this part of network configured for VTP Transparent or VTP Off mode, or are the VTP settings left on default values? I am asking because leaving the VTP settings entirely unconfigured and then connecting the 6807 via a trunk link to an existing switch that runs VTP may cause the 6807 to adopt the VTP domain name and download the VLAN database, and subsequently propagate it to the attached 3850s and 4506s. This will certainly happen if the VTP settings on the 6087 were left at their default values, and you are not using VTP password protection in your existing VTP domain.
Is this because 6807 bacame secondary root? Or if I keep 6807 priority to default then Stack of 3850s will become root and will connect the link between both VSS then 6506 will become as root, this can help?
This is certainly not an issue of the secondary root bridge. In fact, there is no such role in STP. The name "secondary root bridge" is just a saying that tells which switch is going to become the root switch if - and only if - the current root switch fails. However, until that happens, the switch configured as a "secondary root bridge" is just an ordinary switch having no special role with respect to STP.
In fact, I am surprised that by connecting your two VSS pods, the latency in your networks gets increased. I am afraid that this requires further investigation. I personally suspect some kind of flooding present in your network that potentially results into increased delays. However, to investigate this issue, you should try monitoring the traffic and the traffic loads on all ports to see if there is indeed any kind of unexpected load present. With the information we have up to now, no conclusion can be drawn yet.
Best regards,
Peter
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-25-2014 01:47 AM
Dear Peter,
Thank for the comments.
I will only connect the Cisco 6807 to 6506. I will not connect any stack connected to 6807, I will shutdown the all the etherchannels.
I am not sure about the traffic, I will troubleshoot if something is wrong.
Thank You,
Abhisar.