---
id: collect-261121-veille-ia/claude-anthropic/site-to-site-wireguard-with-both-mikrotiks-behind-nat-can-you-do-it
title: "Site to site Wireguard with both Mikrotiks behind NAT. Can you do it?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["bridge", "interface", "site", "comment", "defconf", "address", "router", "wireguard"]
source: https://forum.mikrotik.com/t/site-to-site-wireguard-with-both-mikrotiks-behind-nat-can-you-do-it/167889
source_anchor: ""
source_lines: [1, 446]
section: "Claude_Anthropic"
---
Hi everyone,

Does anyone know if it’s possible to make a site to site tunnel with these requisites??

Mikrotik on site A is behind an ISP router. That ISP router has a public dynamic IP address. Thankfully, it doesn’t suffer any CGNAT. I have the admin password of the ISP router, so I can open any ports I want.

Mikrotik on site B is behind an ISP-owned router. That ISP router has a public dynamic IP address, but unfortunately it’s suffering a terrible CGNAT applied by the ISP. So I cannot open any ports at all.

None of the Mikrotiks can use any NAT rules, nor Firewall rules. (Nat rules and firewall rules must be completely empty in both sites)

Basically what I’m asking is: can you replicate the current VPN that is now using SSTP, but with Wireguard instead?

Current Mikrotik config on site A (port 42345 is open on ISP router)

```
/ip address add address=192.168.100.2/24 interface=bridge network=192.168.100.0
/ip route add disabled=no dst-address=0.0.0.0/0 gateway=192.168.100.1
/interface sstp-server server set enabled=yes port=42345
/ppp secret add local-address=172.26.1.1 name=vpnuser password=blablabla profile=default-encryption remote-address=172.26.1.2 routes="192.168.200.0/24 172.26.1.2 1" service=sstp
/ip cloud set ddns-enabled=yes ddns-update-interval=2m
```

Current Mikrotik config on site B (cannot open any ports)

```
/ip address add address=192.168.200.2/24 interface=bridge network=192.168.200.0
/ip route add disabled=no dst-address=0.0.0.0/0 gateway=192.168.200.1
/ip route add disabled=no dst-address=192.168.100.0/24 gateway=sstp-out1
/interface sstp-client add connect-to=blablabla.sn.mynetname.net disabled=no name=sstp-out1 port=42345 profile=default-encryption user=vpnuser password=blablabla verify-server-address-from-certificate=no
```

Thanks a lot!

             
            
           
          
            
            
              Sure,

but you need to  post both configs to see what you are doing.

/export file=anynameyouwish  ( minus serial number and any public WANIP information, keys etc. )

             
            
           
          
            
            
              Basic wireguard setup.

Just make sure the site behind dynamic IP (normal NAT) has some sort of ddns running (IP Cloud service from MT or whatever).

Then use that as target when setting up wireguard from the other side.

You also may want to consider running some script in case dynamic IP changes (it should resolve it on it’s own using keepalive but I like to have a safeguard).

All this and more can be found in the nice wireguard compilation made by our own fire spitting llama 

https://forum.mikrotik.com/viewtopic.php?t=182340

             
            
           
          
            
            
              Yes, it’s a very basic setup.

Here’s the full config of Mikrotik on site A:

```
/interface bridge add admin-mac=XX:XX:XX:XX:XX:XX auto-mac=no comment=defconf name=bridge
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface bridge port
add bridge=bridge comment=defconf interface=ether1
add bridge=bridge comment=defconf interface=ether2
add bridge=bridge comment=defconf interface=ether3
add bridge=bridge comment=defconf interface=ether4
add bridge=bridge comment=defconf interface=ether5
/ip neighbor discovery-settings set discover-interface-list=LAN
/ipv6 settings set disable-ipv6=yes
/interface list member add comment=defconf interface=bridge list=LAN
/interface sstp-server server set enabled=yes port=42345
/ip address add address=192.168.100.2/24 comment=defconf interface=bridge network=192.168.100.0
/ip cloud set ddns-enabled=yes ddns-update-interval=2m
/ip dns set allow-remote-requests=yes servers=8.8.8.8,8.8.4.4
/ip dns static add address=192.168.100.2 comment=defconf name=router.lan
/ip route add disabled=no dst-address=0.0.0.0/0 gateway=192.168.100.1
/ppp secret add local-address=172.26.1.1 name=vpnuser password=blablabla profile=default-encryption remote-address=172.26.1.2 routes="192.168.200.0/24 172.26.1.2 1" service=sstp
/system clock set time-zone-name=Europe/Madrid
/system note set show-at-login=no
/tool mac-server set allowed-interface-list=LAN
/tool mac-server mac-winbox set allowed-interface-list=LAN
```

And here’s full config of Mikrotik on site B:

```
/interface bridge add admin-mac=XX:XX:XX:XX:XX:XX auto-mac=no comment=defconf name=bridge
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface sstp-client add connect-to=blablabla.sn.mynetname.net disabled=no name=sstp-out1 port=42345 profile=default-encryption user=vpnuser password=blablabla verify-server-address-from-certificate=no
/interface bridge port
add bridge=bridge comment=defconf interface=ether1
add bridge=bridge comment=defconf interface=ether2
add bridge=bridge comment=defconf interface=ether3
add bridge=bridge comment=defconf interface=ether4
add bridge=bridge comment=defconf interface=ether5
/ip neighbor discovery-settings set discover-interface-list=LAN
/ipv6 settings set disable-ipv6=yes
/interface list member add comment=defconf interface=bridge list=LAN
/ip address add address=192.168.200.2/24 comment=defconf interface=bridge network=192.168.200.0
/ip cloud set ddns-enabled=yes ddns-update-interval=2m
/ip dns set allow-remote-requests=yes servers=8.8.8.8,8.8.4.4
/ip dns static add address=192.168.200.2 comment=defconf name=router.lan
/ip route add disabled=no dst-address=0.0.0.0/0 gateway=192.168.200.1
/ip route add disabled=no dst-address=192.168.100.0/24 gateway=sstp-out1
/system clock set time-zone-name=Europe/Madrid
/system note set show-at-login=no
/tool mac-server set allowed-interface-list=LAN
/tool mac-server mac-winbox set allowed-interface-list=LAN
```

All the computers use Windows 10 in both sites. And I have manually added the required routes using this command in CMD window:

Computers in site A:

```
route -P ADD 192.168.200.0 MASK 255.255.255.0 192.168.100.2
```

Computers in site B:

```
route -P ADD 192.168.100.0 MASK 255.255.255.0 192.168.200.2
```

             
            
           
          
            
            
              Really?

There are no wireguard settings on Router A,

You are missing the interface member for WAN…  ( if ether1 is your wan, then it should not be on the bridge )

You have no input chain rule to indicate whether the unknown listening port is being triggered by MT B.

You can  get rid of the static DNS setting not required ( besides being wrong, if anything should be set to 192.168.100.1 vice  100.2 )

Missing the address for wireguard.

Missing the ip routes for wireguard if needing to access the LANS on MT B.

Missing any sourcenat rule…

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Same comments for Router B. ( except no need for input chain rule for listening )

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Read the article and make an effort!

Repost when you have made some progress.



Also to understand how the routers firewall works recommend you at least  put in the basic firewall…

MT A

/ip firewall filter

{Input Chain}

add action=accept chain=input comment=“defconf: accept established,related,untracked” connection-state=established,related,untracked

add action=drop chain=input comment=“defconf: drop invalid” connection-state=invalid

add action=accept chain=input comment=“defconf: accept ICMP” protocol=icmp

add action=accept chain=input comment=“defconf: accept to local loopback (for CAPsMAN)” dst-address=127.0.0.1

add action=accept chain=input in-interface-list=LAN

add action=accept chain=input  dst-port=WireguardPort  protocol=udp

add action=drop chain=input comment=“drop all else”

{forward chain}

add action=fasttrack-connection chain=forward comment=“defconf: fasttrack” connection-state=established,related

add action=accept chain=forward comment=“defconf: accept established,related, untracked” connection-state=established,related,untracked

add action=drop chain=forward comment=“defconf: drop invalid” connection-state=invalid

add action=accept chain=forward comment=“allow internet traffic” in-interface-list=LAN out-interface-list=WAN

add acction=accept chain=forward comment=“allow remote wg inbound”  in-interface=wireguardNAME dst-address=192.168.100.0/24

add action=accept chain-forward comment=“allow local wg outboud”  out-interface=wireguardNAME src-address=192.168.100.0/24

add action=accept chain=forward comment=“allow port forwarding” connection-nat-state=dstnat

add action=drop chain=forward comment=“drop all else”

/ip firewall nat

add action=masquerade chain=srcnat comment=“defconf: masquerade” out-interface-list=WAN

             
            
           
          
            
            
              
The config I’ve posted is full and complete, these routers are currently working perfectly (using SSTP), but I would like to change to Wireguard. That’s why I’m asking for help. The examples in Mikrotik’s website do not work for me. I tried to use them, but I reverted back to my normal SSTP configuration, because I couldn’t get it to work.

I don’t use the firewall in any of the Mikrotik (firewall is already in place by the ISP router). The firewall is completely blank on both Mikrotiks. It’s not needed in my situation.

I also don’t use any NAT or sourcenat rule. Again, the NAT is handled by the ISP routers. Mikrotiks don’t need to do NAT at all. The config I posted works perfectly with SSTP!!

ether1 is a member of the bridge, like all the other ethernet ports. So, you’re right, I can delete the WAN group in the interface list. It’s useless because there’s no interface in the WAN group, it’s empty. I forgot to delete it, sorry.

So, again, my question is: can Wireguard do what SSTP is doing for me right now? Is Wireguard equally capable in my situation?

             
            
           
          
            
            
              You seem to know everything about how to setup the MT router. Clearly you dont need help.

Also Holvoe provided a link with information, just read that, you should be up and running in no  time.

As I stated I would use the input chain rule to ensure the wireguard handshake is successful.  Why you dont want confirming information is illogical.

No, the router needs to do Sourcnat.

Follow the logic tree,  the subnet on the mikrotik does not exist on the ISP router, it knows nothing about the subnet.

So when subnetA goes out to the internet by way of the WANIP of the mikortik it leaves as its subnet address and heads out the ISP router to the net.

When the return traffic arrives at the ISP router with dst-address of the subnet on the Mikrotik, the ISP router will drop it as it doesnt reccognize this as a valid local address.

The purpose of sourcenat is to change the srce address to the WANIp of the MT which is also its LANIP on the ISP router.  Thus return traffic is recognized and then sent to the MT router by the ISP router.  The MT router un sourcenats back the traffic to the originator.

             
            
           
          
            
            
              I wish I could knew everything about Mikrotik! I wouldn’t be asking for help in that case!

The ISP router in site A has a LAN interface, and a WAN interface.

The LAN interface of the ISP router has IP address 192.168.100.1 (it serves as the gateway for Mikrotik in site A). The WAN inteface of the ISP router is connected to a fiber ONT box, and receives a public dynamic address using PPPoE.

This configuration works perfectly well with SSTP. No firewall rules, no NAT rules, no sourcenat. Everything works perfectly.

My question is simple: Why is Wireguard unable to work in this situation? Or isn’t it? I’m not familiar with the Wireguard protocol and I’m struggling to understand what is needed to make it work.

It puzzles me that SSTP in my situation works so easily and effortlessly, why does Wireguard need so much complexity???

             
            
           
          
            
            
              Well using SSTP probably means your LAN users never go out the ISP WAN, but instead go out the SSTP tunnel?

OTherwise you wouldnt have any working internet.

I am trying to ensure your router is setup properly in general and also to ensure wireguard success.

You can drive your car to the story everyday but never get into the accident and ask WHY SHOULD I wear my seatbelt, I havent needed it thus far>…

Or perhaps you want to drive to a different state, and it has snow, so maybe one should consider better tires…

             
            
           
          
            
            
              All the computers in the LAN are running Windows 10. They have DHCP client active, and they receive a dynamic IP address from the ISP router. For example, in site A the computers receive private addresses in the range from 192.168.100.33 to 192.168.100.254.

The DHCP server in the ISP router in site A sets the gateway as 192.168.100.1

That’s why I’m forced to type this command in all the computers in site A:

```
route -P ADD 192.168.200.0 MASK 255.255.255.0 192.168.100.2
```

That command creates a static route inside Windows (the -P parameter makes it permanent, so it survives a reboot), and tells the computer to route the packets to site B using Mikrotik.

Basically, the Mikrotiks are only dealing with the SSTP tunnel and nothing more. The rest of the surfing of the internet is done using the ISP router.

             
            
           
          
            
            
              hello filament,

The config I’ve posted is full and complete, these routers are currently working perfectly (using SSTP), but I would like to change to Wireguard. That’s why I’m asking for help. The examples in Mikrotik’s website do not work for me. I tried to use them, but I reverted back to my normal SSTP configuration, because I couldn’t get it to work.


did you try to re-use your isp sstp port dnat for the wg port?

router a is the server.

router b is the client. cgnat-ed

so what is the error log says?

             
            
           
          
            
            
              In my view, Wireguard is conceptually not more complex then SSTP.

The only thing where you need to be careful is the correct entry of public keys (easy to goof up) and the definition of allowed address on the peer side.

Other then that, it’s very simple (a LOT easier then e.g. IPSEC/IKE2).

What’s so difficult about MT’s own Wireguard instructions ?

They are really dead simple.

The point anav wants to make (and me too) is spoon-feeding is not going to help you.

You will not learn from it.

So try those instructions again, post the config you have made and indicate what doesn’t work at that point.

             
            
           
          
            
            
              Arghhg, jesuschrist! I’m now seeing the mistake. I had created several peers in Mikrotik A during the testing and I was copying and pasting another peer’s public key into Mikrotik A (instead of the public key from the Wireguard interface of Mikrotik B). F*ck my life… 

The example from Mikrotik’s official web DOES in fact work. Here are the lines needed in Mikrotik site A:

```
/interface wireguard add listen-port=42346 mtu=1420 name=wireguard1
/ip address add address=10.255.255.1/30 interface=wireguard1 network=10.255.255.0
/ip route add dst-address=192.168.200.0/24 gateway=wireguard1
/interface wireguard peers add allowed-address=192.168.200.0/24 interface=wireguard1 public-key="BLABLABLAMAKESUREYOUCOPYTHERIGHTKEYDAMNIT="
```

And these are the lines needed in Mikrotik site B:

```
/interface wireguard add listen-port=13231 mtu=1420 name=wireguard1
/ip address add address=10.255.255.2/30 interface=wireguard1 network=10.255.255.0
/ip route add dst-address=192.168.100.0/24 gateway=wireguard1
/interface wireguard peers add allowed-address=192.168.100.0/24 endpoint-address=blablabla.sn.mynetname.net endpoint-port=42346 interface=wireguard1 public-key="BLABLABLA="
```

Now it’s working as intended. I have removed the SSTP configuration and will keep Wireguard in its place!

Thanks everyone, and sorry for the inconveniences!

             
            
           
          
            
            
              Very common mistake is to mix up the keys, its confusing the first time and then becomes clearer.

Note when using a third party provider often they will give you the private key to use in your router vice having the router generate a private key like we do normally

WHY →  because the private key generates  a constant known public key, so the ISP provider an give you the private key to use which means they already know the public key to setup on their side and you dont have to send them anything.

             
            
           
          
            
            
              Mmm, that’s interesting. I’m wondering if Mikrotik could implement a simple “wizard-style” process like they do for DHCP servers?

Maybe you could run the “Wizard”, it’ll ask you a few questions, it’ll save the configuration on your current Mikrotik, and then give you a Summary of the commands that you would need to copy & paste into the other Mikrotik?

That would make it much easier for beginners, and would eliminate human errors like mine. Also, if the Wizard could cover the two most common scenarios: site-to-site and roadwarriors, it would help a LOT of people!

             
            
           
          
            
            
              Hi Filament,

i am planning to do such a config for my configs and would like to post complete setups here for both router A and router B. In my case, i will have something like 9 VLANs and will need to setup several Wireguard connections, one for each VLAN.

The thing is i would like to use your basic setup and try to setup wireguard interfaces 1-9 for each VLAN.

Could you pls post it here or send it to me?

tx

korg

             
            
           
          
            
            
              Start your own thread, your scenario bares little to no resemblance to the original threads situation.

State the traffic flow requirements and the design will fall out naturally, for example, there is probably no need to have a different wireguard per vlan approach.

Post a a diagram of your intentions and also post both configs to see what you have done so far.

             
            
           
          
            
            
              Really,  you think keeping track of extra keys is FUN?    You need a vacation LOL.
