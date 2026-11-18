At a loss, unsure how to troubleshoot any further 
        
        
        
    
    
    Good afternoon,
I come to you r/Meraki in search of help. I (and multiple Meraki engineers) are at a complete loss, and I feel we're grasping at straws as to how proceed.
To keep things as simple as possible, there are (2) Meraki devices. Head Office MX84 (10.0.0.0/16) and Satellite office (10.20.0.0/16). Head Office has a 100M/100M fiber connection, Satellite Office has a 120M/20M cable connection. Both have static IP's -- direct connection to the internet (cable modem is bridged). Both offices maintain a hub/spoke configuration.
VLAN configuration is the exact same both all locations:
- 
      VLAN 10 = x.x.10.x/24 = LAN
- 
      VLAN 20 = x.x.20.x/24 = Voice
VLAN 10 has "VPN mode" enabled; internet traffic is routed the local MX.
VLAN 20 has "VPN mode" disabled. Phones are 100% cloud based and do not need to interact with anything on either side of the tunnel. DHCP is handled by the Meraki, pushing Google DNS, and has a Group Policy applied on the VLAN blocking any RFC1918 traffic.
No inter-VLAN rules, and very basic configuration on both sides. Very simple stuff. Maybe 2-3 users maximum at the Satellite Office.
I can sit at the Head Office, at Home, or at another customers site and successfully ping the Satellite Office modem and Meraki MX67W WAN interface without issue. It does not miss a beat. However, whenever the Meraki decides to execute the chaos monkey protocol -- all heck breaks loose. If I'm sitting on VLAN10 in the Head Office and ping VLAN10 in the Satellite Office -- I will randomly just start dropping packets. When this happens, it's almost as if the internal software bridge just dies for [random interval] until a service restarts, as when I drop packets on the other side of the tunnel, the users on VLAN20 will have their phones disconnect (which should not be possible).
I've tried everything, new cables, new power adapters, RMA'd MX's, factory resets, creating a new site, upgrading to various firmware builds (currently on 16.16.3 as the 17.8 beta randomly broke everything), moving to different registration servers and nothing helps.
Whilst I was waiting for my RMA I had to provide a workaround to keep the packets flowing, so I dug out some old Cisco 881's and set them up at both locations without a single issue for over a month straight. They both just ran without any drops, disconnects, or issues of any kind so I have an incredibly difficult time pointing the finger at the ISP -- however, I honestly just don't understand what to do next short of just calling it and leaving the 881's in place -- but it seems silly when I've paid for an MX67W (with advanced security license) and have to treat it as a doorstop.
Any help or next steps would be appreciated. I'm truly at a loss -- it just doesn't make sense.
Cheers
Edit: Throwing some additional random bits of info here
- 
      I have another office (in a different city) with the same hardware as this satellite office -- no issues
- 
      There is a APC UPS connected correctly
- 
      The room is air conditioned -- hardware (should) not be overheating
Section des commentaires
Were there any issues with the setup on MX versions prior to 16?
I had a few customers who had to revert due to application weirdness once the MX 16.x firmwares were released.
Yes. 15.44 (and below) were absolutely the most stable -- but even then drops would still happen. Part of the Meraki troubleshooting was to upgrade everything to the latest build, multiple times -- hence why I'm almost completely on 17.8.
Does the packet loss happen when you start the ping from the satellite office to the head office?
Is the VPN set as full mesh or spoke to hub?
Im probably sure youve tried this but just want to verify. Disable all advanced security settings on both MX units and test from both directions to see if it drops again.
Support has the ability to take Pcaps on the VPN interface so I would have them take that capture while pinging across the tunnel from both a client device behind each MX and also from the MX units themselves to help eliminate some variables.
I vaguely remember something similar I worked on back in 2018/2019 with MX84s doing weird things with the VPN interface due to how that unit was made. Basically they threw a switch interface in the MX unit that had a ton of software to hardware issues like the VPN interface dropping when you make any DHCP changes.
My best guess its something with the MX84 itself
Yes. It is seemingly random. I can go a day, week, month without issue. However when chaos monkey is let loose, watch out.
Hub and spoke.
Correct. Same thing.
Unfortunately when chaos monkey is released and/or I start losing any type of connectivity the "internal" side of the MX just dies. No VPN traffic, nothing. The WAN is the only thing that responds oddly enough.
MX84 has a hardware issue that will always exist -- depending on the change you make, you can drop a couple packets on any/all interfaces (it's super fun to work around)
I considered that, but I do have another Satellite Office (not outlined in this scenario) which has been rock solid (up until 17.8 -- which required me to do a factory reset and has been fine since). It uses the 10.10.0.0/16 range and actually uses the same ISP/service (Rogers Cable - 120/20 w/ 1 static IP). Just different city.
Ty for the response! I'm glad it wasn't something stupid right off the hop.
Just to confirm no other DNS servers other then Google’s? What’s your lease time and when do your start dropping packets? Do you drop packets when you ping every device or is it only a single device? Are you specifying packet size in your ping commands?
Nope. I originally had phones proxy to upstream DNS (Meraki is configured to use Google DNS) so figured it was prettier. Part of my troubleshooting process, I removed that from the equation and just pass "Google DNS" directly -- on the phones, I can see 8.8.8.8/8.8.4.4 being used.
DHCP lease time is 1 week across the board. (Guest WiFi is 1 hour, but that's out of scope).
No -- default ping sizes were used; but that's not really the issue; rather just one of the tools I would use to initially detect the outage. I would see the agents drop offline in the call center and have all (3) pings going -- modem external IP, WAN external IP or Meraki, and Meraki VLAN IP -- and without fail, when the agents would disappear so would my pings to the Meraki VLAN IP (Data VLAN -- VLAN 10).
And I’m guessing the issue occurs one a week or so? Do yourself a favour and go to that site and double check all of your phones only have a single Ethernet connection. I’ve run into this issue with Meraki so many times. If you have a switch that has spanning tree enabled and isn’t configured for port isolation and one of your phones is double connected your Meraki will flip out and seemingly shutdown at lease renewal time.
Hi
Sorry youre going through so much grief.
I had not yet read the whole thread, so if its solved, please disregard.
BUT, regarding the use of Google as your DNS, and sending ping (IGMP) traffic. We have done extensive work with our enterprise ISP and they informed us that google is constantly hammered by ping traffic. Therefore they tend to have high loss rates. Im not saying this is the cause of your issue, but you might consider using a different DNS and see if it helps, such as your ISPs, or Cloudflare (1.1.1.1, or 4.2.2.2 ) These have a much friendlier public DNS. Google is great, but not if your sending IGMP traffic.
Check the monthly or weekly resolution graph in the SD-WAN, Appliance Status, Uplink tab.
Secondly, another area to check would be your SD-WAN & Traffic shaping. Just for kicks, in the uplink configuration, if that number is set much higher than your ISP WAN speed, then the extra packets can get lost. Meraki Support wont discover those two. Unless you get someone on the line that is OG Meraki and ISP. That was a painful lesson to learn, fortunately in our case, our Fiber ISP has some great engineers to work with. Tweaking that is really helpful.
Id check that and see if that helps. Of course, you would need to restart all connected devices to get the new DNS info piped back out to them.
I have questions regarding the VLAN and LAN connections, which I will read up on the rest of the threads first... sounds like a peculiar issue.
Had something similar happen on a T1 years ago and it was the “clocking” on the T1 circuit which had to be set to External/Internal on the T1 Routers, Cisco by the way. Was this all working before that update? I recently had to roll back a switch update because it destroyed the Trunking on the network.
Unfortunately this location has "always" been a little hit or miss, but I will say that 15.x was by far the most stable build.
I just have such a hard time pointing the finger at the ISP here -- rock solid connection, and I can swap it out with an 881 and have zero issues -- but then the Meraki gets dumped in place and issues appear.
I think u/Able-Stretch9223 was leading me towards MTU issues (if it's not always DNS, it's MTU) -- but I would suspect the Meraki magic happens and figures that out automatically/would report in the logs if there was an MTU issue. Additionally, the other office with the exact same service has (basically) zero issues -- so if it were an MTU issue, I would expect to experience this.
What kind of link is it? Ethernet? MPLS? I did have s bad MTU issue on an MPLS link years ago there was an MTU mismatch upstream.
Have you opened a ticket with the ISP? Could it be a routing issue with the ISP and how it’s handling their Meraki cloud traffic?
No. I just have such a difficult time pointing the finger at the ISP (even with it being Rogers). It doesn't stand to reason to me that the Cisco 881 would perform perfectly fine, but a Meraki doesn't.
The Meraki also shows perfect uptime/connectivity to the cloud (remember, I don't lose pings to the modem or the Meraki WAN interface -- just internal tunnels).
AFAIK, the Meraki registration servers come into play to establish/maintain a tunnel, but once established the connection is direct -- so even if the ISP was taking a sub-optimal route to Meraki, that process should be fairly straight forward.
Ever figure anything out with this? I'm having the same issue with two satellite offices in an eerily similar configuration to yours. Specifically a Rogers business static IP with a Hitron CODA-4680 bridged to a Meraki Z3. Has been a major issue for these offices since...June-ish?
I'm actually running a Hitron CODA-4582 -- but eerily similar indeed.
So -- there's multiple ways I can answer this. The most problematic site (the one I referenced in this scenario) started largely having these issues after a power failure/APC battery depletion/failure. I went back on my e-mail chain with APC about it, and it looks like it was back in March of 2022.
Before then, once in a blue moon there would be issues -- but largely stable. After, it was a complete crapshoot. Some days/weeks -- it'd be perfectly fine, others not.
Oddly enough, it (and the other office) have been stable since I posted this issue.
Just based on experience, I would say the following items are what seem to have "fixed" it:
Upgrade the hub to 17.8 beta (had to anyway due to work around a WAN1/WAN2 locking up until one of the WAN connections is physically pulled -- other issue I've ranted about on here)
Upgrade the spokes to 17.8 beta
Factory reset spokes -- pin hole push, manually reconfigure static IP's, etc.
Added a laptop cooling pad for both the modem + MX67W -- this one, to be exact: https://www.amazon.ca/gp/product/B08B38LQZ6/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1
The MX67W has a USB port, so I used that for the power. The one site annoyed me, so I left the RGB on, just because.
Is your modem static IP and WAN interface IP staying up though? When I first had Rogers install the lines -- the Hitrons were complete trash. I ended up having to have them downgrade me to a specific version that seemed most stable.
I haven't been able to dig into it as deep as you have. What I know is this:
2 locations with Rogers Hitron modems had similar symptoms that started a few weeks before the giant outage, possibly coincidental
at first thought it was an internal equipment issue at one of the locations, so swapped out an Aruba switch as well as the MX at that location - but symptoms still continue
VPN will be fine for a random amount of time, but then will see flapping and dropped packets in our monitoring, a power cycle of the Rogers equipment sometimes fixes but have also had to pinhole reset the Rogers modem to fix for a longer stretch
We do have other locations with Rogers connections that aren't seeing this sort of behaviour, with identical Meraki equipment. I can't be sure of what Rogers modem they're using, however.