- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-27-2011 02:29 PM
Hi Team,
I have a strange problem with a VPN L2L between an ASA on my side and a CheckPoint as the peer.
The IPsec tunnel works fine, but from time to time, traffic stops passing through the tunnel.
Scenario:
172.31.250.0/28 -- ASA --- Internet --- CheckPoint --- 200.122.x.y/32
I've done plenty tunnels between ASAs and CheckPoints but this time we've found this:
access-list outside_1_cryptomap extended permit ip 172.31.250.0 255.255.255.240 host 200.122.164.165
local ident (addr/mask/prot/port): (172.31.250.0/255.255.255.240/0/0)
remote ident (addr/mask/prot/port): (200.122.164.165/255.255.255.255/0/0)
#pkts encaps: 0, #pkts encrypt: 0, #pkts digest: 0
#pkts decaps: 1148, #pkts decrypt: 1148, #pkts verify: 1148
local ident (addr/mask/prot/port): (172.31.250.8/255.255.255.248/0/0)
remote ident (addr/mask/prot/port): (200.122.164.0/255.255.255.0/0/0)
#pkts encaps: 27682, #pkts encrypt: 27683, #pkts digest: 27683
#pkts decaps: 27683, #pkts decrypt: 27683, #pkts verify: 27683
local ident (addr/mask/prot/port): (172.31.250.8/255.255.255.248/0/0)
remote ident (addr/mask/prot/port): (200.122.164.165/255.255.255.255/0/0)
#pkts encaps: 3579, #pkts encrypt: 3579, #pkts digest: 3579
#pkts decaps: 10443, #pkts decrypt: 10443, #pkts verify: 10443
Traffic is defined between 172.31.250.0/28 and a single host, but I see three SAs:
1. 172.31.250.0/28 - 200.122.164.165/32
2. 172.31.250.8/32 - 200.122.164.0/24
3. 172.31.250.8/32 - 200.122.164.165/32
What is the reason for this??
The reason I paste the above is because the CheckPoint defines the ''interesting traffic'' as two rules (one in each direction).
On CheckPoint:
Rule 1: Traffic from 200.122.164.165/32 to 172.31.250.0/28
Rule 2: Traffic from 172.31.250.0/28 to 200.122.164.165/32
So, I believe the problem happens because we define the phase 2 SAs as bidirectional rules (crypto ACLs), and CheckPoint defines the phase 2 SAs as unidirectional rules. Even though the traffic matches, I see the above output.
I think it means that the ASA receives some traffic in one SA and send it via another, and I'm not sure if that causes the problem, and if so, how to fix it?
The problem is totally random. We lowered the rekey time to 2 minutes on phase 2 and 5 minutes on phase 1 and there's no problem during rekey.
We hadn't been able to capture the log at the exact moment of the problem. Then the tunnel suddenly comes up again and start working.
ASA 5510 version 8.2(5)
Any help is appreciated!
Federico.
Solved! Go to Solution.
- Labels:
- 
						
							
		
			VPN
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-30-2011 02:27 AM
Federico,
Installing new SAs doesn't conincide with rekey, it consicides with one peer assuming it matches new traffic and thus need to inititale a new SA.
Now when we have static crypto map, this new SA's traffic selector needs to match what we defined in ACL.
Usually you would get an error if there is absolutely no match and tunnel would fail at phase 2.
I just want to make sure we're on the same page. When terminating on a dynamic crypto map, we don't know (or rarely know) what the remote SA will look like so we accept everything.
I'm not saying that checkpoint was half match here half matched there. I'm saying that it most likely (for a reason I might not be aware of, or a bug) matched the ACL under static crypto map.
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2011 08:14 AM
Federico,
This is bad. There should be no overlap in proxy IDs.
Who's pushing those proxyIDs? What is the configuration on ASA side?
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2011 10:42 AM
Marcin,
Thank you for jumping in!
The configuration on the ASA side is the following:
access-list outside_1_cryptomap extended permit ip 172.31.250.0 255.255.255.240 host 200.122.164.165
Here's the output of ''sh cry ipse sa''
ASA(config)# sh cry ips sa
local ident (addr/mask/prot/port): (172.31.250.8/255.255.255.248/0/0)
remote ident (addr/mask/prot/port): (200.122.164.0/255.255.255.0/0/0
#pkts encaps: 5261, #pkts encrypt: 5261, #pkts digest: 5261
#pkts decaps: 5263, #pkts decrypt: 5263, #pkts verify: 5263
inbound esp sas:
spi: 0xE1A215FB (3785496059)
transform: esp-3des esp-sha-hmac no compression
in use settings ={L2L, Tunnel, }
slot: 0, conn_id: 90112, crypto-map: outside_map
sa timing: remaining key lifetime (kB/sec): (4373883/26808)
IV size: 8 bytes
replay detection support: Y
Anti replay bitmap:
0xFFFFFFFF 0xFFFFFFFF
outbound esp sas:
spi: 0x3080AC5D (813739101)
transform: esp-3des esp-sha-hmac no compression
in use settings ={L2L, Tunnel, }
slot: 0, conn_id: 90112, crypto-map: outside_map
sa timing: remaining key lifetime (kB/sec): (4373883/26808)
IV size: 8 bytes
replay detection support: Y
Anti replay bitmap:
0x00000000 0x00000001
access-list outside_1_cryptomap extended permit ip 172.31.250.0 255.255.255.240 host 200.122.164.165
local ident (addr/mask/prot/port): (172.31.250.8/255.255.255.248/0/0)
remote ident (addr/mask/prot/port): (200.122.164.165/255.255.255.255/0/0)
#pkts encaps: 32877, #pkts encrypt: 32878, #pkts digest: 32878
#pkts decaps: 26746, #pkts decrypt: 26746, #pkts verify: 26746
path mtu 1500, ipsec overhead 58, media mtu 1500
current outbound spi: EEA4A57F
current inbound spi : A7164A99
inbound esp sas:
spi: 0xA7164A99 (2803255961)
transform: esp-3des esp-sha-hmac no compression
in use settings ={L2L, Tunnel, }
slot: 0, conn_id: 90112, crypto-map: outside_map
sa timing: remaining key lifetime (kB/sec): (4372807/1609)
IV size: 8 bytes
replay detection support: Y
Anti replay bitmap:
0xFFFFFFFF 0xFFFFFFFF
outbound esp sas:
spi: 0xEEA4A57F (4003767679)
transform: esp-3des esp-sha-hmac no compression
in use settings ={L2L, Tunnel, }
slot: 0, conn_id: 90112, crypto-map: outside_map
sa timing: remaining key lifetime (kB/sec): (4372496/1608)
IV size: 8 bytes
replay detection support: Y
Anti replay bitmap:
0x00000000 0x00000001
Why would I see those proxy IDs when the crypto ACL is defined as above as if the only entry.
Also, is the only VPN configured on this ASA.
Thank you again.
Federico. 
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2011 11:17 AM
Federico,
You need to debug isakmp and ipsec to see who's sending those proxy IDs.
I've never seen out equipment initiating SAs which are not configured, but it would be interesting to get to the bottom of this.
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-28-2011 12:29 PM
Marcin,
We were having this problem before because the CheckPoint side was summarizing and sending the 200.122.164.0/255.255.255.0 instead of 200.122.164.165/255.255.255.255 to us. I saw this on the debugs.
However, after changes on the CheckPoint I don't see the /24 coming from the CheckPoint side anymore, but perhaps I've overlooked. Has to be the only explanation right? That the CheckPoint is summarizing and sending the /24 to use, maybe besides the /32 which is the one we need?
Federico.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2011 11:00 AM
Federico,
[According to the best of my knowledge] We should not allow the remote end to negotiate ANYTHING else that is confgured, in case of static L2L with "match", unlike dynamic tunnels.
1) This looks like a bug on Checkpoint.
2) In my opinion ASA should not allow to create those SAs (unless checkpoint is terminating on a dyanmic crypto map).
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-29-2011 05:14 PM
So, the tunnel works fine, but something happens and traffic stops passing through (the tunnel fixes itself and start working again).
I thought this happened during rekey but does not.
What I don't get is why the ASA creates the SAs for the remote /24 (since it does not have that network defined in the crypto ACL), unless the CheckPoint is terminating at that point in the dynamic crypto map and therefore creating the SA.
Interestingly, the traffic works fine when there's the SA to the remote /24 created and passing traffic.
I guess I will try to see if the ASA still receives the remote /24 from the CheckPoint (as it was summarizing the network in the beginning), but I haven't seen that anymore.
So Marcin.... If the ASA receives the summarized /24 instead of the host, it will not match the static L2L, will then match the dynamic crypto map and create that SA, and pass traffic. But this causes the problem.
I will try to see if this is what's going on.... (supposedly we fixed this already).
Federico.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
10-30-2011 02:27 AM
Federico,
Installing new SAs doesn't conincide with rekey, it consicides with one peer assuming it matches new traffic and thus need to inititale a new SA.
Now when we have static crypto map, this new SA's traffic selector needs to match what we defined in ACL.
Usually you would get an error if there is absolutely no match and tunnel would fail at phase 2.
I just want to make sure we're on the same page. When terminating on a dynamic crypto map, we don't know (or rarely know) what the remote SA will look like so we accept everything.
I'm not saying that checkpoint was half match here half matched there. I'm saying that it most likely (for a reason I might not be aware of, or a bug) matched the ACL under static crypto map.
Marcin
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-03-2011 02:45 PM
Marcin,
We haven't seen the problem so far....
I will let you know what happens
Federico.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
04-08-2016 06:52 AM
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-04-2011 05:49 AM
where are your evidence that it is a Checkpoint bug? It could be a checkpoint bug but unless you can show it, pure speculation.
Federico, I have another suggestion, why don't you turn on "vpn debug ikeon" on the checkpoint side to see what going on during phase I and II. Checkpoint will tell you exactly how and why it failed, whether it is sending or receiving to and from the ASA.
You then can view the debug file with IKEView.exe. It will tell you exactly where things go wrong.
By the way, what version of checkpoint? Checkpoint on Nokia IPSO or Secureplatform? can you share the output "uname -a" and "fw ver"? Furthermore, are you setting this up in Simplified mode (VPN community) or traditional mode?
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-16-2011 08:58 AM
Hi,
Check Point does supernetting by default.
If you have a host in your check point rulebase but the network is defined in the Check Point encryption domain then it will try to establish the SA with the encryption domain network instead of the host.
I suggest you use network to network between check point and ASA then do the filtering in the rulebase.
This way the SA is up between the 2 networks but the rulebase in Check Point will only allow the host.
You can have a VPN Filter ACL in the ASA to acheive the same security. The filter ACL permits only the host and is different from the interesting traffic ACL which is applied to the crypto map and used to establish the SA...
You can also disable supernetting in Check Point but that would affect all Check Point VPNs so be very careful!
You have to do that through the GUI db edit tool not smartdashboard...
Hope this helps
Patrick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-16-2011 09:11 AM
That's why I keep saying you need to turn on "vpn debug ikeon" and look at the file ike.elg file with IKEView.exe and it will tell you where things go wrong.
In Checkpoint VPN community, you have the option to do network or hosts. Keep in mind that if you do hosts instead of network, it will require more processing power because of the SA. That's the beauty of Checkpoint VPN simplify mode.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-16-2011 10:26 AM
Yes I agree, the debugs should tell you what exactly is happening. Same thing on the Cisco side, you should see if you receive a supernetted subnet or a host in the IPSEC phase 2 negotiations...
I think Check Point recommends network to network when establishing an IPSEC tunnel with a non-Check Point gateway.
Patrick