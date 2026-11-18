- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-27-2018 07:47 AM
I want to set this router up in a live environment. Is it possible to get it on the network, add the settings and lastly change the gateway configuration at the end, without it affecting the current router settings? thoughts?
What's the easiest way to do this?
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Meraki
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-27-2018 08:08 AM
Yes I do this. Just plug my MX into a regular access port. It'll get DHCP and checkin to the dashboard so you can start configuring it. Then my last step is to configure the static IP on the WAN interface before putting it into production.
If this was helpful click the Kudo button below
If my reply solved your issue, please mark it as a solution.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-27-2018 08:08 AM
Yes I do this. Just plug my MX into a regular access port. It'll get DHCP and checkin to the dashboard so you can start configuring it. Then my last step is to configure the static IP on the WAN interface before putting it into production.
If this was helpful click the Kudo button below
If my reply solved your issue, please mark it as a solution.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-27-2018 08:10 AM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-27-2018 08:25 AM
And another thing that could happen is that you'll need to reboot the next device up the line when you replace the firewall. I've had a couple times replacing a firewall and had to reboot the modem in front of it as it was caching arp and wasn't happy.