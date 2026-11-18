Has anyone had issues with Stacks and LACP after going from 14 to 15 code?
						
					
					
				
			
		
	
			
	
	
	
	
	
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-30-2023 06:55 AM
I'm working on a challenging issue with support right now. We have a somewhat complex network, with about 65 switches, multiple stacks in 30+ buildings all interconnected by 10G single mode fiber. We have a pair of MS425-32's at the core, and every link out to the and MS350-48FP stacks in the buildings is LACP spanning the two core switches. All inter-vlan routing is done at the core.
We were running 14.33.1 code and we went to 15.21.1 code because it attained the "Stable" rating. I upgraded remote sites first because they are simple with only one or two switches. Everything went smoothly so I let our main campus upgrade.
That one went so bad. Half of our switches went down and never came back, stacks were half alive with some switches working and others not. The core looked green, but Core 1 turned out to not be working once the network was put under load. I tried to reboot it and it went down and didn't come back. Unplugging the stack completely and plugging it back in got it to boot again. That became the fix for every switch we have.
Once force-rebooted I thought we were good, but now I have a couple switches that have dropped out of their stacks, then hours later they just pop back in. From the local status pages other switches in the stack show them as there, and the ports appear to be up, but the switch is gone and the light is red. In most cases the ports in the switch don't work. Also if I reboot the core some of my stacks will not reconnect to it until they are also rebooted. My other weird issue is that I'm now suddenly getting UDLD errors on my LACP links heading back specifically to Core 1, Core 2 never has an issue. This could be the stack acting up, or LACP acting up, I'm just not sure.
I'm trying to revert to 14.33.1 code now, but I'm curious if anyone else has had issues anything like this on 15.21.1 or any 15 code?
Thank you,
James
- Labels:
- 
						
							
		
			Meraki
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-30-2023 04:23 PM
I've not upgraded too many switch stacks from 14.x to 15.x but the ones I have done have gone smoothly.
Without digging too deeply into your issue, it's feasible that you hit one or multiple of the known issues around LACP and stacks on upgrade/reboot. To name a few from the release notes:
- Cross-stack LACP bundles experiencing a switch reboot will cause the remaining online port to experience an outage for up to 30 seconds. The same is seen again when the switch comes back online (present since MS 10)
- Loops can be seen when rebooting a stack member containing a cross-stack lag port (always present)
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-31-2023 10:31 AM
Thanks for the reply Brash!
I had considered those known issues, but we were on 14 code before and didn't have this, I would have expected it there as well since one was always present and the other was from code 10. The other thing that is puzzling is that these errors aren't coming up when something is changed or rebooted. The switch is running just fine for a few days then I get a short burst of these errors from across campus, the errors come up and clear in a short time, but it seems to come in waves.
Currently I haven't had a UDLD error since May 24th, but I had it on 6 completely different switch stacks, it occurred and cleared on all of them all within 10 minutes. Then on May 22nd I had another wave of these errors, all happening and clearing within about 25 minutes. and this was in 14 completely independent stacks and buildings.
I'm not sure, I'm hoping it stops happening though. A big part of me was starting to wonder if one of my core switches didn't upgrade correctly.
Yellow Tang
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-01-2023 02:46 AM
I have a somewhat big setup and the upgrade went terribly wrong, nothing I'm not used when it comes to Meraki switches (sad, but true) - this will be a long story
I have 4x MS425-32 switches in a stack + a number of MS225-24/48 in different combinations (some are single, some are 2pcs stacks and some are 3pcs or more which is very important in the Meraki world) - overall ~40 Meraki switches + ~10 WS2960X.
All "remote" switches are connected to 2 of the core switches using LACP, so no SPOF.
I set up a staged upgrade where I first wanted to upgrade the core switches (they are handling all L3 as in your setup), then 2 pure management switches in server rooms and then the rest of the remote switches.
Here is what happened:
- cores upgraded nice, everything green, all ports up, BUT
- as in your story I had something between 10-20 switches behind the cores offline and not handling traffic despite all ports being up and showing no issues
- so the upgrade got stuck and I needed to react
- first I rebooted all cores from the dashboard - didn't change a thing
- then, since I have a long story with the Meraki switches I started disabling all redundant connections towards the remote switches (so basically all ports on core 02 and 04 disabled, only stacking was up)
- after that almost all switches started regaining connectivity and started upgrading
- by saying almost I mean that there is a story if you have more than 2 switches in a stack and a cross stack LACP connection to the cores they tend to "break", some of the switches in a stack will be offline, some online, the best resolution to that is remove all redundant connections (I disabled that from core side) and then reboot one of the switches that is online, if this doesn't help you pick the next one and next one and this will eventually kick in. If you are onsite, then just remove redundant connections and reboot the whole stack. I assume that rebooting the master switch would help right away, but I didn't know that the dashboard shows the info, at least on the latest firmware
- I waited some time and observed the current firmware version on the dashboard (it will show you if its "not current" or MS 15.21.1)
- after everything upgraded I rebooted the cores once again and then I started enabling the redundant ports and everything is working as expected since then (two weeks ago)
So this was a journey which took me over 2 hours to fix remotely, I even didn't bother to raise a case because I knew that playing with ports and rebooting will work and also I planned to do a full reboot anyway on the cores because I have low trust in the firmware upgrade process (bad experience).
If you have any questions - go ahead
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-03-2023 01:03 AM
@jacek.jagielka what release were you coming from? We did have a similar issue with a much earlier version of 15, but not generally from one 15 version to another. The only thing we do differently is we upgrade the L2 edge first, wat a bit and then the L3 core last.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-04-2023 11:59 PM
We were upgrading from MS14.33.1.
Is this something we should be doing? First upgrade all L2 remote switches and then the cores?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-03-2023 12:18 PM
Yes, we've seen similar issues going from 14.33.1 to 15.21.1 with out MS250-48FP switches. I've done troubleshooting with support, full stack reboot, etc., I'm at the point where I'm looking to revert to 14.33.1
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-05-2023 12:30 AM
Are you still experiencing issues? What is happening?
Curious why you are thinking about an rollback.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-04-2023 03:31 PM
Thank you everyone for posting your experiences here! Unfortunately I will miss the 14 day rollback window due to travel, but if you go forward with that South Paw, I'd love to hear how it goes. Meraki support said there is absolutely no way to roll back after 14 days.
I have not removed LACP, and I've noticed that I'm getting almost no UDLD errors now (none since the 24th actually), plus the switches have been fairly stable (Knock on Wood).  I have my fingers crossed that this all works itself out.  I've never heard of a firmware upgrade needing to break in,.. but maybe something still wasn't finished?  I can only guess and hope lol.
Thanks again!  And if anyone else has experience with this please let me know,
Yellow Tang
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-12-2023 08:44 AM
So,.. this got allot worse. I started noticing that some internal IP addresses would randomly not be reachable from certain machines on the network, but other machines connected to the same switch were just fine. I could even unplug a machine from a port, plug a different computer in, and it would start working. It was very intermittent, and the same machine that wasn't working yesterday would suddenly be working today, but another one wouldn't be. It's worth noting that the Cohesity VIP might not be pingable, but half the node addresses on the same subnet would respond, and half wouldn't. Which ones did and didn't was not consistent across different workstations either though. The only thing that was consistent was that all virtual computers always had a perfect connection to everything. I suspect that is because LACP is not setup between that switch and the core.
As you can imagine this was causing some bizarre and difficult helpdesk tickets with glitchy behavior like this, and those poor guys have enough to do.
I ended up nailing down a computer that couldn't ping our Cohesity. I got a persistent ping going then I disabled the cross/stack LACP by disabling one of the ports in the 2 port aggregate link. It immediately fixed that computer and a couple others that were having trouble in the same stack.
Every one of our buildings has at least one LACP link to an IDF over SMF which spans 2 switches at the core, and 2 switches in most IDF's. Right now to keep the lights on I'm working on disabling LACP everywhere.
I'd love to hear from people if LACP into a single switch is working?  Or is LACP bad in this version?  
Thank you,
Yellow_Tang
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
06-12-2023 11:08 PM
I'm running LACP between access switches and cores in different configurations, cross stack on both sides, sometimes one single access switch with 2 LACP uplinks to 2 cores and sometimes a single switch with one uplink going to one core and a backup link (blocked by RSTP) going via an other switch to an other core.
After things settled after the upgrade I didn't notice any LACP issues.
We also experienced things not being reachable in a random way and it was either the LACP links misbehaving or L3 on the cores, but in the end disabling all redundant ports (not unconfiguring LACP) helped and that is what I would try in your situation.
Since we have all links from access switches spanned across 2 cores I went into one of them and disabled all ports (not the uplink and stacking ofc), after waiting a minute or two things went back to normal, then I started enabling ports and the issues never came back.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-30-2024 08:46 AM
Could you provide some more detail on "disable" all ports. Is this on MS425? Did you disable / re-enable both unused ports and also active LCAP ports? We have major connectivity issues between MS390 v C16.7 up link to core MS425 v16.7.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-31-2024 12:29 PM
It's on the MS425 (my core), I don't have MS390's, I have MS225 downstream.
What I mean is that I would disable all redundant links towards given downstream switches from the MS425 perspective.
If I have a LACP connection to some downstream switch (or downstream stack) consisting of 2 ports, I would manually disable one of them and leave only one running (if I would have 5, I would disable 4 and leave one running).
So basically, what I'm doing is killing all redundancy in my network during upgrades, just in case.
And since I'm doing this only on the core switches, if something is going wrong I can easily turn the ports back on (not disabling these links on both sides).
In a perfect world, lets say you have 2 core switches, both of them having connections to all downstream switches (LACP, 2 ports, each ending up in one of the cores), then it would mean that you just need to disable all ports on one of the core switches (assuming you have them stacked, the stack you leave enabled) - that's what I'm doing more/less.
If you read the release notes for the firmwares you will see that there is a lot going on around switches creating loops during reboots on LACP ports and so on.
Its my way of preventing these since some firmware versions ago.
BTW, maybe this known issue is hitting you?
- Switches move LACP ports to an active forwarding state if configured. This can cause loops when connecting to an MS390 or other Catalyst switches unless the bundles are configured on the MS390/Catalyst switches first. All non-catalyst ports are configured in passive LACP mode so that loops do not occur between Meraki switches (always present)
Sorry for the lengthy post and repeating myself, just ask if you have any doubts
And by any means - that is not what Meraki would suggest, its only my way of working around the issues that I kinda got used to during the years.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-01-2024 12:51 PM
I didn't catch that you were doing this ONLY during upgrades. I disabled one downstream port on every switch so we no longer have redundancy at all and it fixed the issue. I cannot reenable LACP though, or the problem will come back. I'm waiting for an upgrade past 15.21.1 to be stable in hopes that they address cross stack LACP. Of interesting note, I think if one side is Cross Stack, and the other goes to a single switch it's still okay. It's only if the redundant links are split across stack members on both sides.
Thank you!
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-02-2024 01:14 AM
Yes, when updating or I think when rebooting the devices to be more specific.
But on a daily basis I'm not having any issues, so what you are experiencing is very weird.
Have you contacted support?
Can you share all settings from the ports on both sides here? Maybe there is something that you are just missing?