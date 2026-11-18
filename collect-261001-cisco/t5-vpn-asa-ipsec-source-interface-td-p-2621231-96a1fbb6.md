- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-23-2015 08:45 AM - edited 02-21-2020 08:05 PM
Hi...
There is a way to configure an IPSEC VPN with a source-interface like in a router,? This is for a site to site VPN. I want to use a loopback interface.
When I configured one VPN, the only option is the IP from the interface where the traffic is going out.
Thanks.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			IPSEC
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-23-2015 09:06 AM
Whatever interface you enable ipsec on is the source interface.
crypto map MyMap interface [interface name]
ASA's don't support loopbacks so that is not possible.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
02-23-2015 09:06 AM
Whatever interface you enable ipsec on is the source interface.
crypto map MyMap interface [interface name]
ASA's don't support loopbacks so that is not possible.