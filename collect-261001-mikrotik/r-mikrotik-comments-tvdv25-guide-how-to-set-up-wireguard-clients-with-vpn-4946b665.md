Guide - how to set up WireGuard clients with VPN service 
        
        
        
    
    
    Edit (8/5/2022) Added dst-address-type=!local to Mark Routings in mangles as per changes to rOS. That small change will make the entire setup valid.
Edit (4/4/2022): Small changes to improve clarity and give credits to people who helped me by answering gazillion questions and providing their knowledge.
[TL;DR] – How to set up wireguard VPN connections to VPN provider on MikroTik RouterOS v.7
Hi all,
First of all, I need to say that this would not be possible without user: Sob from https://forum.mikrotik.com . Without his help, there would not be this guide.
Here is a hopefully simple guide on how to create a wireguard VPN tunnel(s) on MT router. There will be several scenarios so you may pick and choose :)
I will not be using WebFig/WinBox just Terminal as it is much easier.
Sidenote – I am based in the US so my tunnels (4) will be exploring other countries. Please adjust your situation accordingly.
#1 Get your WireGuard connection information from your VPN provider. Here I will be using KeepSolidVPN. Note: if you want to create multiple tunnels please choose a different device for each. If you will get info for tunnel X on device A, and then you create tunnel Y on device A then tunnel X will be deleted by your provider. IPSec in this case is easier.
I have 4 files from VPN provider (each looks like this)
[Interface]PrivateKey = [private key here]ListenPort = 51820Address = [IPaddress]/32DNS = [DNS-IP]
[Peer]PublicKey = [public key here]PresharedKey = [PSK key here]AllowedIPs = 0.0.0.0/0Endpoint = [enpointIP]:51820PersistentKeepalive = 25
#2 Lets set up interface on MT
/interface wireguard add listen-port=51821 mtu=1420 name=KeepSolidVPN-Germany private-key="[private key here – tunnel DE]"
Note: Please use a different ListenPort number than you received from your VPN provider. You can’t have multiple interfaces with same port working at the same time
/interface wireguard add listen-port=51822 mtu=1420 name=KeepSolidVPN-France private-key="[private key here – tunnel FR]"
/interface wireguard add listen-port=51823 mtu=1420 name=KeepSolidVPN-Poland private-key="[private key here – tunnel PL]"
/interface wireguard add listen-port=51824 mtu=1420 name=KeepSolidVPN-UK private-key="[private key here – tunnel UK]"
#3 Lets set up peers on MT
/interface wireguard peers add allowed-address=0.0.0.0/0 endpoint-address=[enpointIP – tunnel DE] endpoint-port=51820 interface=KeepSolidVPN-Germany persistent-keepalive=25s preshared-key="[PSK key here – tunnel DE]" public-key="[public key here – tunnel DE]"
/interface wireguard peers add allowed-address=0.0.0.0/0 endpoint-address=[enpointIP – tunnel PL] endpoint-port=51820 interface=KeepSolidVPN-Poland persistent-keepalive=25s preshared-key="[PSK key here – tunnel PL]" public-key="[public key here – tunnel PL]"
/interface wireguard peers add allowed-address=0.0.0.0/0 endpoint-address=[enpointIP – tunnel UK] endpoint-port=51820 interface=KeepSolidVPN-UK persistent-keepalive=25s preshared-key="[PSK key here – tunnel UK]" public-key="[public key here – tunnel UK]"
/interface wireguard peers add allowed-address=0.0.0.0/0 endpoint-address=[enpointIP – tunnel FR] endpoint-port=51820 interface=KeepSolidVPN-France persistent-keepalive=25s preshared-key="[PSK key here – tunnel FR]" public-key="[public key here – tunnel FR]"
#4 Lets set up IP addresses for each tunnel on MT
/ip address add address=[IPaddress – tunnel DE]/32 interface=KeepSolidVPN-Germany network=[IPaddress – tunnel DE]
/ip address add address=[IPaddress – tunnel PL]/32 interface=KeepSolidVPN-Poland network=[IPaddress – tunnel PL]
/ip address add address[IPaddress – tunnel UK]/32 interface=KeepSolidVPN-UK network=[IPaddress – tunnel UK]
/ip address add address=[IPaddress – tunnel FR]/32 interface=KeepSolidVPN-France network=[IPaddress – tunnel FR]
#5 Create routing tables on MT
/routing table add comment="Table for WireGuard - Poland" disabled=no fib name=wg-pl
/routing table add comment="Table for WireGuard - Germany" disabled=no fib name=wg-de
/routing table add comment="Table for WireGuard - UK" disabled=no fib name=wg-uk
/routing table add comment="Table for WireGuard - France" disabled=no fib name=wg-fr
#6 Lets create routes on MT
/ip route add dst-address=0.0.0.0/0 gateway=KeepSolidVPN-UK routing-table=wg-uk
/ip route add dst-address=0.0.0.0/0 gateway=KeepSolidVPN-France routing-table=wg-fr
/ip route add dst-address=0.0.0.0/0 gateway=KeepSolidVPN-Germany routing-table=wg-de
/ip route add dst-address=0.0.0.0/0 gateway=KeepSolidVPN-Poland routing-table=wg-pl
#7 Lets create masquerades on MT
/ip firewall nat add action=masquerade chain=srcnat out-interface=KeepSolidVPN-Poland
/ip firewall nat add action=masquerade chain=srcnat out-interface=KeepSolidVPN-Germany
/ip firewall nat add action=masquerade chain=srcnat out-interface=KeepSolidVPN-UK
/ip firewall nat add action=masquerade chain=srcnat out-interface=KeepSolidVPN-France
We are ready now for different scenarios
Scenario A – Specific computers are using tunnels exclusively (i.e. Computer X with IP-A is using tunnel-X)
/routing rule add action=lookup disabled=no src-address=IP-A/32 table=wg-uk (Computer with IP-A is sending all its traffic via UK tunnel)
/routing rule add action=lookup disabled=no src-address=IP-B/32 table=wg-de (Computer with IP-B is sending all its traffic via Germany tunnel)
/routing rule add action=lookup disabled=no src-address=IP-C/32 table=wg-fr (Computer with IP-C is sending all its traffic via France tunnel)
/routing rule add action=lookup disabled=no src-address=IP-D/32 table=wg-pl (Computer with IP-D is sending all its traffic via Poland tunnel)
Scenario B – Entire network is using ONE specific tunnel
/routing rule add action=lookup disabled=no src-address=Local-IP(Subnet)/NetSize table=wg-uk
Entire network Local-IP(Subnet)/NetSize (i.e. 192.168.0.0/24 if you have subnet 192.168.0.0 netmask 255.255.255.0) is sending all its traffic via UK tunnel). If you know segmentation with NetSizes you can play it pushing parts of your network to different tunnels. The sky is the limit here.
Scenario C – Same as A but using lists (will be important with Scenario E) (What is good it is much easier to add/remove computers in the lists (rather than create/delete routing rules), also you could disable IPs from the lists and when needed just enable it – good for scripts). Note: LAN is my bridge for all LAN traffic, you can be interface-specific here
/ip firewall address-list add address=IP-A list=local-uk
/ip firewall address-list add address=IP-B list=local-de
/ip firewall address-list add address=IP-C list=local-fr
/ip firewall address-list add address=IP-D list=local-pl
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-PL passthrough=yes src-address-list=local-pl
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-UK passthrough=yes src-address-list=local-uk
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-FR passthrough=yes src-address-list=local-fr
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-DE passthrough=yes src-address-list=local-de
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-DE dst-address-type=!local in-interface=LAN new-routing-mark=wg-de passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-UK dst-address-type=!local in-interface=LAN new-routing-mark=wg-uk passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-FR dst-address-type=!local in-interface=LAN new-routing-mark=wg-fr passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-PL dst-address-type=!local in-interface=LAN new-routing-mark=wg-pl passthrough=no
Scenario D – Traffic to the countries based on their IP addresses. You can use https://mikrotikconfig.com/firewall/ to download IP ranges. Then you need to change list names to be different for each country. Note: LAN is my bridge for all LAN traffic, you can be interface-specific here
Upload file(s) to MT
/import IP-Poland.rsc
/import IP-Germany.rsc
/import IP-France.rsc
/import IP-UK.rsc
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-Poland new-connection-mark=VPN-IP-PL passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-Germany new-connection-mark=VPN-IP-DE passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-UK new-connection-mark=VPN-IP-UK passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-France new-connection-mark=VPN-IP-FR passthrough=yes
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-DE dst-address-type=!local in-interface=LAN new-routing-mark=wg-de passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-UK dst-address-type=!local in-interface=LAN new-routing-mark=wg-uk passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-FR dst-address-type=!local in-interface=LAN new-routing-mark=wg-fr passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-PL dst-address-type=!local in-interface=LAN new-routing-mark=wg-pl passthrough=no
Scenario E – Combination of Scenarios C & D. The way I am doing this here is first there are computers in the network that will use tunnels for their all traffic and then the rest will use traffic based on destination address i.e. computer with IP-A will use exclusively tunnel to the UK, IP-B to Germany, IP-C to France, IP-D to Poland. So if IP is not in the local-xx list then it checks the destination address and route to proper tunnels. If IP is outside any of your lists it will be routed to your Internet connection without using VPN (i.e. in my case it is WAN). Please note that you can't do it any other way (destination and then source) as it does not make sense and would create more issues with proper routing) Note: LAN is my bridge for all LAN traffic, you can be interface-specific here
/ip firewall address-list add address=IP-A list=local-uk
/ip firewall address-list add address=IP-B list=local-de
/ip firewall address-list add address=IP-C list=local-fr
/ip firewall address-list add address=IP-D list=local-pl
Upload file(s) to MT
/import IP-Poland.rsc
/import IP-Germany.rsc
/import IP-France.rsc
/import IP-UK.rsc
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-PL passthrough=yes src-address-list=local-pl
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-UK passthrough=yes src-address-list=local-uk
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-FR passthrough=yes src-address-list=local-fr
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark new-connection-mark=VPN-IP-DE passthrough=yes src-address-list=local-de
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-Poland new-connection-mark=VPN-IP-PL passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-Germany new-connection-mark=VPN-IP-DE passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-UK new-connection-mark=VPN-IP-UK passthrough=yes
/ip firewall mangle add action=mark-connection chain=prerouting connection-mark=no-mark dst-address-list=IP-France new-connection-mark=VPN-IP-FR passthrough=yes
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-DE dst-address-type=!local in-interface=LAN new-routing-mark=wg-de passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-UK dst-address-type=!local in-interface=LAN new-routing-mark=wg-uk passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-FR dst-address-type=!local in-interface=LAN new-routing-mark=wg-fr passthrough=no
/ip firewall mangle add action=mark-routing chain=prerouting connection-mark=VPN-IP-PL dst-address-type=!local in-interface=LAN new-routing-mark=wg-pl passthrough=no
Hope that helps!!!
Section des commentaires
For surfshark customers, in case of scenario B, the following 2 lines are needed:
/ip firewall mangle add action=mark-connection chain=prerouting new-connection-mark=VPN passthrough=yes
/ip firewall mangle add action=change-mss chain=forward new-mss=1360 protocol=tcp tcp-flags=syn tcp-mss=1453-65535
I hope i was helpful...
My surfshark instructions and info aren't providing a PSK for this connection. Am I missing something?
No, if your connection is not using PSK then disregard anything that has PSK, do not use preshared-key option
DUDE (or "Dudette", as required)!
I've been trying to do something similar for days now to route a local subnet from another MT (that's successfully working for other peers) to my MT and couldn't get it to work! Everything was set up properly- peers were handshaking, interfaces were set up, routes were in place- but NO traffic; I was pulling my hair out!
I'm pretty sure it was these lines of yours that made the difference:
As this box is also a WG server of its own, and I'm guessing that having only one routing table was preventing my remote MT's local subnet from being routed.
I also needed this:
... which I'd had before, but was missing some parameters.
Again, thanks for this!
Thank you for the gold but this is truly unneccesarry
I treat my writeups on Reddit a bit like a backup. Usually, my setup is far from usual hence I have to do a lot of research to create a single, cohesive, stupid-proof setup (see scenario E). I like also to create solutions that may be complex in a single rule rather than creating 10 simple rules as I see the possibility of unwanted interactions between rules. Once the complex rule is fully tested (and I spend a lot of time testing) I know it will work as designed as long I need that rule to be.
Regardless - thank you and happy WGing :)
I can now- I'd been beating my head against this one final issue for an embarrassingly long time!
Greetings, super grateful with your manual, I am using your case D, which I separate the traffic of some IP depending on some countries, but it gives me error when I want that by default everything that is not marked goes through a specific VPN, is it possible?
So you want to commingle scenario D with B? So the logic (for entire network) is that if the destination IP belongs to country A then it goes to VPN-A, country B then it goes to VPN-B, etc, and then rest to VPN-Z?
Or D with A? So the logic (for specific IP addresses) is that if the destination IP belongs to country A then it goes to VPN-A, country B then it goes to VPN-B, etc, and then rest to VPN-Z?
Yes, both are possible.
You can even create rules that Country A -> VPN-A, Country B -> VPN-B,…, specific sites -> No VPN [like connecting remotely to work], everything else -> VPN-Z
Hello Thanks for your answer, yes i want this "Country A -> VPN-A, Country B -> VPN-B,…, specific sites -> No VPN [like connecting remotely to work], everything else -> VPN-Z", i try usign Case D with B. but i failed, because my D sites are block (UK, FR sites).
Thank you for this great tutorial.
When trying to follow it, I noticed one thing that maybe you could clarify:
The file I got, just like yours but from another VPN provider, had as you said:
Address = [IPaddress]/32
However when I tried to do
/ip address add address=[IPaddress – tunnel DE]/32 interface=KeepSolidVPN-Germany network=[IPaddress – tunnel DE]
with my VPN's parameters, I noticed that MikroTik ROS does not accept /32 subnets.
How did you work around this problem?
Please double-check. ROS accepts IP address with /32 subnet but you can't put subnet in network
Line should look like this: /ip address add address=10.200.14.35/32 interface=vpn-interface network=10.200.14.35
10.200.14.35 is bogus address
True, that goes through.
But when I open the GUI Adress List, there is no /32 any more. If I use /31 or /30, then these /31 or /30 stay after the IP address.
That just may be a quirk.
Did you use /32 in your real world usage and does it work? My address list entry is red right now.
Thanks for this! i got a tunnel working to the outside. traffic goes out no problem. using scenario A and my specific VPN provider.
Only issue I'm running into is port forwarding from the VPN provider back the client using scenario A. I'm a novice with MT so any pointers would be great.
I will say only this - I don’t know.
1 - I don’t have MT anymore
2 - as I am thinking how I would do this it would require:
a) that you have your own IP address from VPN provider. Most of them would put you behind NAT, therefore inbound traffic would be restricted. Some providers are allowing you to have open port for you - this is where you would need to funnel your traffic b) opening port in the VPN interface to allow traffic to go it. I assume similarly to opening port to allow inbound WG connections if you have road warrior setup.
Thats my $0.02 :) Best
Sorry for resurrecting this thread, but was curious if I'm reading correctly that wireguard support in RouterOS wasn't added until v7.xx?
Correct
Appreciate the response. I was able to update the router to 7.12.1 from 6.48. I was worried it wouldn't be available, but all went smoothly. :)
this is more insightful than the mikrotik wiki. thanks, I'm sure this will come in handy somewhere in the future.
great job, thanks mate! I was having troubles configuring WireGuard client on my MikroTik using YouTube videos and MikroTik wiki, but your solution finally helped me to sort it out :)
My configuration is:
- private VPS with custom WireGuard setup inside Docker Container
- MikroTik at home
Initially your setup didn't work for me as well, but then I noticed you used /32 netmask, so I changed it to /24 and it worked like a charm.
Thank you so much! This really helped! However, I have run into a small issue. I am able to route all network traffic through my wireguard server on the internet (scenario B), but response times are rather poor and sometimes the connection fails. I suspect that my routes are not configured properly.
IP wg interface: 10.11.10.2
IP main gateway: 10.11.10.1
DHCP address space: 10.110.10.50-100
DNS: set via DHCP to 8.8.8.8
Is it a problem to have the main gateway, the dhcp address range and the wg interface on the same subnet?
Thanks!
Peter
I was able to follow this and do it in winbox. Super quick to do. Thanks for this!
Thank you so much for this guide. I’ve read so many forum posts that are very similar to this but not right. This article was just what I needed to tweak my current Wireguard setup and get it working. Thank you.
Hey, could someone make a tutorial video and upload it to youtube?
That's a great guide, simple and most importantly works like a charm! Thanks a bunch mate.
Cheers for this. Worked great!