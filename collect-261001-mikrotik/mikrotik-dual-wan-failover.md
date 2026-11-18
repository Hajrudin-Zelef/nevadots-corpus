Hello guys,

I’ve recently bough a mikrotik router and the model I chose as let’s say my teaching router thanks to the help of some colleagues from the forum is hAP ac2.  If someone is interested of something about the need of the router - here is the thread I made http://forum.mikrotik.com/t/mikrotik-routers-question/122624/1

However, one of the critical things I needed was the dual WAN support. I need it only as a fail-over without load balance. This weekend I had few hours to play with the new router and  to try to make a simple setup.

Here is the current situation about the ISPs:

1. Main link - 100Mbps PPPoE  directly to the hAP
2. Back up link - 50 Mbps ADSL  - phone line->Modem->hAP
3. Static local IPs from both providers.

Here is what I’ve done so far:

1. 
I stepped on the default configuration of the hAP and from there I’ve tried to build up what I need. Firstly I’ve changed rotuer’s IP  192.168.0.1 and created a new DHCP server wit primary DNS 192.168.0.1 and secondary 8.8.8.8, added pool in the needed range.
2. 
Excluded the ether2 port from the bridge and added it to the list of WANs so other rules can apply to it.
3. 
I’ve created a ppoe client for port1 with the needed user and password while using peer DNS and default route / ppoe-out  was also added to the WAN list.
4. 
Static IP for the port2 - 192.168.x.x, added route with gateway (the ADSL modem)
5. 
Static DNS - same as the adsl modem
6. 
Distance of the main link is 1 and distance of the backup - 2

So in this configuration everything seems to work for now even if I don’t know if there is something missed and the test was done only by disconnecting the WAN ports. However this configurations is apart from the PCC tutorial and I’m not sure if the PCC could be applied to this case when there is one static and one pppoe? Could I use as gateway the whole pppoe-out in the PCC wiki scenario? I’ve read several posts and wikis about the dual WAN scenario but it seems that most of them are using load balance and they are mostly for static addresses. There are also the mangle rules which I haven’t had the time to study more carefully. I saw that the preferred way of dual WAN fail-over is the PCC but what about the mangle rules, they seems to use it too?  What would be the best way to configure dual WAN fail-over in my case and is my configuration by far worth a dime?

I’m using the default settings for the firewall applied to both WAN ports via the WAN list, same for NAT.

Thanks for the help in advance guys. It’s a great device with plenty of settings and I like it. Still it would be great if they’ve added something as the quick config for dual WAN options as it’s a common thing now days. This router would be used for experiments for now until I start to feel a bit more comfortable with the OS and I get my knowledge together.

             
                
            
           
          
            
            
              PCC is for load balancing, from your description, you do not need that.

Then I would also change the ADSL Modem to bridge mode and configure ADSL PPPoE on the Mikrotik.

The do not use the "Add default Gateway"in the PPPoE settings, instead create static default routes with a distance of 1 and 2, 2 for the adsl and use "check gateway " on the static routes

             
            
           
          
            
            
              
As @CZFan has said already: if so, don’t bother about PCC, as PCC is here first for load distribution, and only as a side effect it provides some kind of failover. Leaving out PCC will relieve you from having to understand the mangle rules for the moment.




Which is also the weakest point of that configuration. Even though the handover interface of WAN1 is Ethernet, there may still be something between your Ethernet port and the actual Internet which may fail without your Ethernet interface going down, and in such case the route via that interface will stay up so no failover will happen. So here I recommend this article explaining how to monitor that accesss to internet is really possible via each WAN. The Mikrotik wiki describes the same coniguration but in a much less explanatory way.




I agree it would be great - but a great waste of developers’ efforts that could be better spent on features which cannot be achieved by configuration. Every user has a different environment, so WAN1 may be anything out of (PPPoE, static IP configuration, DHCP) on anything out of (ethernet, wireless), leaving aside LTE with its two modes (serial or Ethernet emulation) and so can be the WAN2, and every user has different requirements, e.g. a mere failover in your case and load distribution in someone else’s case. Others may want one of those basic approaches for most of the traffic but some services to be accessed solely via one of the WANs. So I personally like the current approach where QuickSet is for people who have bought Mikrotik by chance and the real configuration interface is for those who have chosen it for its flexibility. Flexibility means a lot of things can be configured, and without an understanding what each setting is necessary for it is close to impossible to answer properly all what a configuration wizard would have to ask.

             
            
           
          
            
            
              
I though about something like this but if I set the ADSL modem to bridge I’d need the password so I’ll be able to create the pppoe from the mikrotik. Sadly I don’t have this information and the ISP won’t give it to me if requested. Normally I won’t even have access to their device and would be forced to manage it by their limited web but however I have access to the modem. I’d probably be able even to recover the password for the pppoe but it may be too much.  Other problem that will result directly from that is the DVR which needs some ports forwarding so I’d have to configure it too.

According to the Add default gateway - I’m a bit confused as the local address is static but in the default settings it takes the remote address. I’ll have to try this otherwise the pppoe route is still set to 1 and the adsl to 0 - that’s for the 0.0.0.0/0 dest.

Thanks for the link, I’m going to check it for sure and I’ll try to make this work. According to the quick settings - it is true that it’s a bit of a hard work to implement it. Still even if it’s a bit tricky to set things up and you’ll need a lot of reading and network knowledge I really like this product. You can learn a lot from it.

At the bottom line it turns out that I can use the distance difference and monitoring to realize the setup. It’s great as I was just preparing to read about the PCC and mangle rules for the next time I have free time.

             
            
           
          
            
            
              Just a note when keeping ADSL modem in router mode, you must not use nat / masquerade between else you will have a double NAT situation than can cause issues

             
            
           
          
            
            
              @CZFan, sorry… although it is true that multiple NAT does cause issues in rare cases (in 99,9% situations it is just as bad as a single NAT), you cannot just disable src-nat (masquerade) between Mikrotik and the ADSL modem, but you also have to add route(s) to Mikrotik’s LAN subnet(s) to the ADSL modem. Otherwise the modem would send packets for these subnets back up the WAN (which is its default gateway).

             
            
           
          
            
            
              Hello guys,

I haven’t had much time recently to play with the fail-over but today I had some time and I decided to test the fail-over scenario from the article **sindy** posted here. I think that I’m facing a problem and I’m not exactly sure where it comes from.

First of all I want to say that I’m continuing with the article after I have set my settings for both ISPs. Here are the things I’ve done before continuing with the first method from the article.

1. Port 1&2 are set as WAN ports.
2. Port 3-5 are in a LAN bridge.
3. DHCP server for the bridge is set with the needed pool and the router used as DNS.
4. PPPoE-out for the main link is set with the needed credentials (Use peer DNS = true Add default route = false), Ethernet port 1.
5. Created route  Dest. Add. 0.0.0.0/0, GW PPPoE-out.

At this point I’m having internet through the ppoe  and everything works fine.

1. Add new address to the address list for the second ISP 192.169.1.2/24, network 192.168.1.0, Ethernet port 2.
2. Created route  Dest. Add. 0.0.0.0/0, GW 192.168.1.1 via Ethernet port 2.
3. DNS server set to 192.168.1.1 (the ADSL modem), Allow remote request = true.

WAN1 and WAN2 alongside with pppoe are added to the WAN list so the NAT and firewall rules can apply to all of them.

At this point I want to continue with the monitoring of the gateways explained in the article (its method one)

/ip route

add dst-address=8.8.8.8 gateway=PPPoE-out scope=10

add dst-address=8.8.4.4 gateway=192.168.1.1 scope=10

At this point both addresses are reachable

/ip route

add distance=1 gateway=8.8.8.8 check-gateway=ping

add distance=2 gateway=8.8.4.4 check-gateway=ping

And here comes the problem, once I set these two routes they are both unreachable. I know that I’m missing something and probably I don’t need the dest in point 5 and 7 but I’m not able to figure it out. Probably there is some kind of a conflict but I’ve tried everything I could imagine for now and it seems not to work. Could you please give me an advice about where I’m in fact messing the things up.

             
            
           
          
            
            
              First of all, the recursive routing on which the scriptless failover is based does not work if a route’s *gateway* is set to anything else than an IP number anywhere in the recursive chain. So you cannot use the interface name (*PPPoE-out*) as a *gateway* for *dst-address=8.8.8.8*, you have to use the IP address provided by the PPPoE server.

             
            
           
          
            
            
              
Fair enough but it isn’t working even with the static address of the second ISP. I mean the 0.0.0.0/0 with GW 8.8.8.8 is still unreachable.

             
            
           
          
            
            
              When I say “you must use as gateway the IP address provided by the PPPoE server”, I have in mind the address which that PPPoE server provides as a gateway, not the one it assigns to you. Is it what you mean by “static address of the second ISP”?

Normally, where you are a PPPoE client, the server assigns you your own address and indicates its own IP address which you may use as a gateway for anything you want to send via that server. But in most cases, you can use the interface name as well; recursive routing on Mikrotik is one of the exceptions where you can’t. I have seen you have set *add-default-route* in */interface pppoe-client* to *no*, but when you do that, you won’t learn the gateway address. So you have to set it to *yes* for a while to learn the address “manually”, or keep it on *yes* and set *default-route-distance* to e.g. *10* and add a blackhole route with a lower distance, so the final set of default routes would be

```
dst-address=0.0.0.0/0 gateway=8.8.8.8 distance=1
dst-address=0.0.0.0/0 gateway=8.8.4.4 distance=2
dst-address=0.0.0.0/0 type=blackhole distance=9
dst-address=0.0.0.0/0 gateway=the.ip.from.isp distance=10
```

Then, you would use an *on-up* script from a */ppp profile* attached to the */interface pppoe-client* to copy the gateway IP from the route with *distance=10* to the route with *dst-address=8.8.8.8/32*. But it only makes sense to do it this complex way if the PPPoE server doesn’t provide the same gateway IP address each time.

Where you are a DHCP client, you must use the IP address provided by the DHCP server as a default gateway (or use the routing table provided by the DHCP server as Option 121 but that’s out of scope of this).

             
            
           
          
            
            
              There’s also the trick with locally set remote address. Simply put one in PPP profile used by PPPoE client and then use it as gateway. I found it some time ago in this forum and although it looks completely wrong at first (how can I set remote address when I don’t control remote side, right?) it works. The used address is not actually used by anything by default, no packets are sent to it, so it don’t matter what you put there. Importatnt part is that it’s static. And I think it was possible to go even one step further and use 8.8.8.8 as this remote address/gateway and check-gateway=ping with it. I don’t remember the details, it probably had to be done with routing filter to add check-gateway option.

             
            
           
          
            
            
              
So what you are saying is that

- you don’t need to retrieve the real “remote” address from the PPPoE client, so *add-default-gateway* may stay at*no*
- you can assign different “remote” addresses to different PPPoE clients, which makes it possible to use the recursive next-hop search and thus scriptless failover even on several PPPoE connections even if the servers assign the same remote addresses to them

?

             
            
           
          
            
            
              Yes. In other words, if ISP would be giving you random 10.x.y.z every time you connect, you can set static 10.1.1.1 in PPP profile and use that. And it will work, because it’s PPP, a tunnel where you just feed everything into. On ethernet, gateway IP address is used by ARP, but with PPP it’s just a local hint where it is.

             
            
           
          
            
            
              
Ah I was afraid it won’t  be so straight forword with the PPPoE…

I meant that when I’m using the second ISP settings, everything is static i.e.  the adress is 192.168.1.2 and the GW is the ADS modem at 192.168.1.1. Thus with these settings while I set the route with dest 8.8.8.8 throught GW 192.168.1.1 it is reachable and when afterwords I set the dest 0.0.0.0/0 with GW 8.8.8.8 its unreachable.

Sadly I’m afraid that the remote ip is not the same and it may variate (I’ll double check it) which seems to make the things even more complicated as it’s obvoius that if the remote adress is changing the set up won’t work if this adress is not monitored. However I have static IP adresses from both ISPs.

             
            
           
          
            
            
              There is no point in **monitoring** the remote IP, it may even not be up at all on the remote end. For the purpose of identifying a local PPPoE tunnel to use by a gateway IP address, you may assign the local alias to the tunnel’s *remote-address* as per @Sob’s suggestion. For the purpose of monitoring the WAN link transparency, the monitored addresses should be some immortal addresses further in the internet, so instead of checking just the hop between your router and ISP’s PPPoE server, you check the whole path through the ISP up to the internet.

Regarding the gateway IP provided by DHCP on the second WAN, there is again no point in monitoring that address itself but you need to use it as a gateway to the monitored destination in the recursive next-hop search scheme, and you cannot easily assign an alias to it (well, you can in some cases, but in exactly those cases it is pointless to do that). So if the DHCP server runs on the modem+router combo you’ve got from the ISP, there is a 0.001% chance that the gateway address will ever change; if that box acts as a bridge and the DHCP server is physically located at the other end of the WAN link, the chance that the gateway IP will change is much higher. So in the latter case, you would have to permit the dhcp client to install a default gateway, but you would tell it to set a high *distance* = low priority value to it and copy the address of that gateway to the individual route to the monitored destination using the *script* parameter. So each time a new DHCP assignment arrives, you’d check whether the gateway IP has changed as compared to the previous one and if yes, you’d modify the individual route(s) to the monitored IPs.

             
            
           
          
            
            
              Hello guys,

Thank you all for the precious help. Tonight I had some time to try the things up and everything seemed to work good with one exception. The remote address of the PPPoE  is changing. It seems to be either 5 or 12 but it changes.

So What I’ve done till now:

/ip route

add check-gateway=ping distance=1 gateway=8.8.8.8

add check-gateway=ping distance=2 gateway=8.8.4.4

add distance=1 dst-address=8.8.4.4/32 gateway=192.168.x.x scope=10

add distance=1 dst-address=8.8.8.8/32 gateway=x.x.x.x scope=10


So when the Line 1 fail and then reconnect it may take a different Remote address so the recurse fails as the gateway in line 3 is different.  You have mentioned some kind of a script but isn’t there an easier way to always take the current remote address and to put it as the GW without scripting? It’s really sad that this isn’t working with the pppoe interface. I was able to get the address of the GW from the status bar of the pppoe interface. I wasn’t completely able to understand the method proposed by @Sob’s.

Also I found in an article that the recursive method has the following limitation:

Whatever IP you use as your target is only reachable via the primary route. If the primary route is down, that IP address will be unreachable. If you use 8.8.8.8 to resolve DNS, the DNS service will be down when the primary route is down. Therefore if you use Google for DNS and use 8.8.8.8 as the routing target, you should use a different Google DNS server such as 8.8.4.4 for DNS instead.


The pppoe is using 1.1.1.1 and 8.8.8.8 as DNS and for the ADSL I have to check it because currently I’m using the ADSL address as DNS.

So except the problem with the changing remote address everything seems to work. If I’m able to reslove this too I’d be able to make it with multiple host checks and leave it this way.

             
            
           
          
            
            
              
You’re mixing together the DHCP case with the PPPoE case.

For DHCP (used at your WAN2), there is no other way than a script to get the assigned IP address of default gateway and set it as a gateway in the individual routes to the monitored anchor addresses, but you obviously don’t need it because the gateway IP provided by DHCP on WAN2 does not change.

For PPPoE (used at your WAN1), there is a script-less way which @Sob has described: you create a copy of */ppp profile* named *default*, give it a name like *my-pppoe-profile*, and set the *remote-address* item in that new profile to some private address which isn’t in conflict with any private subnet you use anywhere in your network - say, *10.22.33.44*. In */interface pppoe-client* configuration, you set the *profile* item to *my-pppoe-profile*. And in the individual route(s) to the anchor IP(s) used to monitor PPPoE availability, you use the 10.22.33.44 as a gateway address. This way, the *remote-address* setting from the */ppp profile my-pppoe-profile* overrides the setting which came from the PPPoE server, and so it remains stable even though the PPPoE server sends you a different one each time.




This is normal - for any destination address the routes with the longest, i.e. most exactly matching, *dst-address* prefix are chosen. So if at least one route with *dst-address=8.8.8.8/32* exists and is active, routes whose *dst-address* prefixes also match 8.8.8.8 but are shorter (wider), such as *8.8.8.0/24* or *0.0.0.0/0*, are never chosen for delivery of packets to 8.8.8.8. This has two consequences:

- you must not set *check-gateway=ping* for the individual routes to monitored anchor addresses, because if you do and the gateway becomes unreachable, the route becomes inactive and the check-gateway pings of the routes one level higher in the recursion start taking another route, ruining the idea of using inaccessibility of the anchor address as indication of network path failure
- you cannot use the anchor IP for any other purpose than network path monitoring because the anchor IP has to be inaccessible if the path whose availability it monitors is broken, so you cannot set up an alternative route to the anchor IP.

 
            
           
          
            
            
              Yes I don’t have problem with the WAN2 a its gateway is constant. I’m using the ADSL modem as GW and it won’t change. The route to WAN2 is static. The only thing that is changing is the remote address of the PPPoE which I’m using as WAN1 (main link).

The current set is:

WAN 1 - Optic → media convertor → Mikrotik at eth1

WAN 2  - phone line->ADSL modem ->Mikrotik at eth2

I have full access to the ADSL modem. I’m only not sure which DNS it was using but now on the Mikrotik  I’m using the modem as DNS.That’s why I believe the only problem is the PPPoE with it’s changing GW. I’ll try the proposed workaround for it and I’ll write if there is any success as it’s not still completely clear for me. If it was possible to use the pppoe interface instead of exact GW it would be way easier…

I’m really interested how in fact the current TP-link failover is in fact realized behind the wizard.

             
            
           
          
            
            
              
PPPoE creates a Point-to-Point interface. For all interfaces of this type, there is no actual need to use any address of the remote device because “the remote end of the tunnel” is the only address you need - whatever you send out that interface will end up on the single remote device. This is a difference to Point-to-Multipoint interface where you need an address of a particular device in addition to the name of the interface. For practical reasons, the gateway addresses are configured as IP addresses, which allows to quickly choose the interface by its associated “network” address, and there the IP address of the gateway device is translated into its MAC address.

So a common habit is to use IP address as a gateway even for PPP interfaces although in these cases it actually acts only as an alias to the interface name. The recursive next-hop search needs IP addresses of gateways, that’s a fact you have to merely accept 

But as the “remote” address of a PPP interface plays no other role in the process than the alias of the interface name, it is only meaningful in the local context of the sending device. Thus you may label the PPP interface with any “remote” address you like. And whilst */interface pppoe-client* doesn’t have a direct parameter *remote-address*, it does accept that parameter if provided by means of the *profile* and uses it to override the value provided by the server.




I’ll stay polite so I won’t translate any of the Czech sayings related to this kind of statements, but it would at least take long to happen (if at all possible because I’m not deep into the recursive next hop search algorithm, so maybe there is some reason which excludes using the interface name as gateway). So if you want it now, use the workaround suggested or use a script. In fact, the next-hop search mechanism was also not originally intended for the failover use. Which BTW also means that the failover may happen up to about 10 seconds after the active WAN path breaks because this is how often the check-gateway pings are sent.

             
            
           
          
            
            
              
For PPPoE (used at your WAN1), there is a script-less way which @Sob has described: you create a copy of /ppp profile named default, give it a name like my-pppoe-profile, and set the remote-address item in that new profile to some private address which isn’t in conflict with any private subnet you use anywhere in your network - say, 10.22.33.44. In /interface pppoe-client configuration, you set the profile item to my-pppoe-profile. And in the individual route(s) to the anchor IP(s) used to monitor PPPoE availability, you use the 10.22.33.44 as a gateway address. This way, the remote-address setting from the /ppp profile my-pppoe-profile overrides the setting which came from the PPPoE server, and so it remains stable even though the PPPoE server sends you a different one each time.


So I’ve tried that  but sadly while trying to connect it says the connection is terminated and it isn’t able to make a connection when I’m using a random remote address.