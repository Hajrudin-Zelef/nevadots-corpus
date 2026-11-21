---
id: collect-261121-veille-ia/windows-os/mikrotik-ipsec-tunnel-with-ddns-and-nat-occursus-arca
title: "MikroTik IPSec Tunnel with DDNS and NAT - Occursus Arca"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2016-05-29"]
keywords: ["ipsec", "mikrotik", "routeros", "tunnel", "router", "examples", "update", "peer"]
source: https://blog.pessoft.com/2016/05/29/mikrotik-ipsec-tunnel-with-ddns-and-nat/
source_anchor: ""
source_lines: [1, 693]
section: "Windows_OS"
---
This guide describes the following situation:

- VPN site-to-site tunnel using IPSec setup is created in MikroTik routers between two private networks: 10.10.10.0/24 and 10.10.20.0/24
- Both private networks use MikroTik router as a gateway
- Each MikroTik router is behind a NAT and have private network range on WAN ports as well: 192.168.10.0/24 and 192.168.20.0/24
- Each MikroTik router has IPSec NAT-Traversal (4500/UDP) forwarded from its gateway (ISP Router)
- Both public network connections change public IP occasionally

Some more remarks:

- I didn’t find any guide which would describe this setup, so I created one.
- Before the start, make sure that you have a separate access to each router, in case you will break your connection.
- Examples contain some additional security settings which can provide better security. Before you use or change these settings, make sure you know what you are doing.
- IPSec tunnel setup in examples uses pre-shared-key authentication method, which has been chosen only for demonstrative purpose and more secure method should be considered. In any case, make sure that if you are going to use PSK method then you need to use a different secret than the one in the examples – also don’t forget that the secret needs to be the same on both sides.
- Names of interfaces on MikroTik routers in this example are:
  - WAN: ether1-gateway
  - LAN: bridge-local
- I successfully tested the setup on 2x Mikrotik hAP lite classic devices, each running behind different routers ( in one case Draytek Vigor 2700 and Ubee EVW3226, in another case TP-Link TD-W8951NB and Compal CH7465LG )
- Also there is a lot of useful documentation about IPSec VPN on MikroTik Wiki – check it out.

Update 1:

- I have experienced tunnel instability when upload link ( provided by ISP ) has been overloaded and when IPSec tunnel was configured with AES GCM. This issue occurred for me at least on Router OS versions 6.38 – 6.39.1. Therefore I have updated the example to use AES CBC, which proved to be stable. But GCM is more secure than CBC, so I recommend to upgrade the RouterOS to the latest version and try with GCM at first.

Update 2:

- RouterOS 6.38 (2016-Dec-30) added IKEv2 support as key exchange mode for IPSec. This mode can be used to improve the security of the tunnel establishment, so I’ve updated the examples in this article accordingly. IKEv2 is also more recent and updated version of the key exchange mode than previously available modes. Except the security improvements, it has embedded the “dead peer detection” and “NAT traversal”, which makes the configuration easier. Additionally, IKEv2 NAT traversal ensures that if connection cannot be created directly between two peers, port 4500/UDP is used. Therefore in RouterOS firewall you need to allow only 4500/UDP.
- RouterOS 6.41 (2017-Dec-22) introduced possibility to use DNS name as IPSec peer address instead IP address. If you will use DNS names in /ip ipsec peer configurations, you can skip the respective part in ipsec-peer-update script mentioned later in this guide, but it is still required to update the IP address of sa-dst-address in IPSec policy, in case the remote router’s public IP has changed. Examples here do not use DNS names in IPSec peer addresses.

Update 3:

- RouterOS 6.43.12 (2019-Feb-11) moved the IPSec profile outside peer configuration and RouterOS 6.44 (2019-Feb-26) added the IPSec identity menu for peers. Therefore I’ve updated the IPSec configurations with versions before RouterOS 6.43.12 and after RouterOS 6.44.

Update 4:

- RouterOS since some version around 6.46 (2019-Dec-02) requires script policy permission “test” for DNS requests using :resolve command. Script and scheduler creation commands have been updated accordingly.

Update 5:

- Refresh on compatibility and features of long-term support version 6.47.9 (2021-02-09). Change to elliptic-curve crypto in some examples, if your non-Mikrotik peer does not connect, try to switch to RSA/DHmodp/smaller curves.

Update 6:

- To simplify the setup with new features available, this guide now:
  - describes only requirements for IKEv2
  - uses no dedicated script for update of peer’s IP address as peers already support hostnames
  - uses no dedicated script for update of IPSec policy sa-dst-address as that is handled now differently by RouterOS

## IPSec

### VPN Tunnel

IPSec tunnel will provide secure site-to-site VPN.

##### MikroTik router 1

/ip ipsec
profile add name="secure-profile" hash-algorithm=sha512 enc-algorithm=aes-256,aes-128 dh-group=ecp521
peer add name="vpn01" comment="vpn01" address="router2.sn.mynetname.net" exchange-mode=ike2 profile=secure-profile
identity add comment="vpn01" auth-method=pre-shared-key secret=REPLACE_THIS_WITH_RANDOM_SECRET peer=vpn01
proposal add name="secure-proposal" auth-algorithms=sha512 enc-algorithms=aes-256-gcm pfs-group=ecp521
policy add comment="vpn01" dst-address=10.10.20.0/24 src-address=10.10.10.0/24 tunnel=yes proposal=secure-proposal peer=vpn01

##### MikroTik router 2

/ip ipsec
profile add name="secure-profile" hash-algorithm=sha512 enc-algorithm=aes-256,aes-128 dh-group=ecp521
peer add name="vpn01" comment="vpn01" address="router1.sn.mynetname.net" exchange-mode=ike2 profile=secure-profile
identity add comment="vpn01" auth-method=pre-shared-key secret=REPLACE_THIS_WITH_RANDOM_SECRET peer=vpn01
proposal add name="secure-proposal" auth-algorithms=sha512 enc-algorithms=aes-256-gcm pfs-group=ecp521 
policy add comment="vpn01" dst-address=10.10.10.0/24 src-address=10.10.20.0/24 tunnel=yes proposal=secure-proposal peer=vpn01

### Firewall

If some rules are used in NAT tables, they need to exclude IPSec traffic, so they will not translate IP addresses in them. This is managed now in default configuration for masquerade rule by ipsec-policy=out,none. In case of manual configuration add this parameter to your masquerade rule. Alternatively you can exclude IPSec traffic by using IPSec accept rule before the NAT rules ( see NAT bypass on MikroTik Wiki for more info ). The same thing applies for a destination NAT ( known sometimes as port forwarding or DMZ host ) – in such case dstnat rules need ipsec-policy=in,none parameter or NAT bypass in opposite direction before them. Here are examples of NAT bypass rules in both directions.

##### MikroTik router 1

/ip firewall
nat add comment="vpn01" action=accept chain=srcnat dst-address=10.10.20.0/24 src-address=10.10.10.0/24 place-before=0
nat add comment="vpn01" action=accept chain=dstnat dst-address=10.10.10.0/24 src-address=10.10.20.0/24 place-before=0

##### MikroTik router 2

/ip firewall
nat add comment="vpn01" action=accept chain=srcnat dst-address=10.10.10.0/24 src-address=10.10.20.0/24 place-before=0
nat add comment="vpn01" action=accept chain=dstnat dst-address=10.10.20.0/24 src-address=10.10.10.0/24 place-before=0

In addition, IPSec IKE traffic needs to be allowed by firewall. For IKEv2 this traffic is 4500/UDP.

##### MikroTik router 1

/ip firewall
filter add comment="ipsec-ike-natt" chain=input dst-port=4500 in-interface=ether1-gateway protocol=udp
filter add comment="vpn01" chain=forward dst-address=10.10.10.0/24 in-interface=ether1-gateway ipsec-policy=in,ipsec src-address=10.10.20.0/24

##### MikroTik router 2

/ip firewall
filter add comment="ipsec-ike-natt" chain=input dst-port=4500 in-interface=ether1-gateway protocol=udp
filter add comment="vpn01" chain=forward dst-address=10.10.20.0/24 in-interface=ether1-gateway ipsec-policy=in,ipsec src-address=10.10.10.0/24

## Public IP Change Adaptation

### IP Cloud

IP Cloud is used as a dynamic DNS system for lookup of remote site’s public IP. This step can be skipped if different DDNS system is used. Time update via IP Cloud is disabled for a case when NTP is used, however you can enable it if necessary.

##### MikroTik router 1 and MikroTik router 2

/ip cloud set ddns-enabled=yes update-time=no

Afterwards, you can use following command to get dns-name value of a local router, which will be used in configuration of script on remote router:

/ip cloud print

### IPSec Remote Address Update Script

This script is no longer needed, but it is kept here for a reference. In the past RouterOS could use only IPs in peer address configuration, so dynamically updated addresses needed to be updated – now hostnames are allowed. Additionally in the past IPSec policies required to have the sa-dst-address attribute updated with IP of remote peer as well – this is now updated automatically by RouterOS.

Following script updates IPSec peer address and policy SA destination address, if remote peer’s address has changed. Make sure to fill out correct peerid and peerhost variables:

- peerid: Identifies IPSec peer and policy records, which will get updated, by comment field value. In this example it is “vpn01”.
- peerhost: Remote router’s value of dns-name from IP Cloud setup. If different DDNS solution than MikroTik IP Cloud is used for remote site, enter remote DDNS hostname here. In this example it is “0123456789.sn.mynetname.net”. Be aware that peerhost is different for each router.

##### MikroTik router 1 and MikroTik router 2

```
/system script add name="ipsec-peer-update-vpn01" policy=read,write,test source=":local peerid    \"vpn01\"\
    \n:local peerhost  \"0123456789.sn.mynetname.net\"\
    \n:local peerip    [:resolve \$peerhost]\
    \n:local peeruid\
    \n:set peeruid     [/ip ipsec peer   find comment=\"\$peerid\" and address!=\"\$peerip/32\"]\
    \n:local policyuid\
    \n:set policyuid   [/ip ipsec policy find comment=\"\$peerid\" and sa-dst-address!=\"\$peerip\"]\
    \n:if (\$peeruid != \"\") do={\
    \n  /ip ipsec peer set \$peeruid address=\"\$peerip/32\"\
    \n  :log info \"Script ipsec-peer-update updated peer '\$peerid' with address '\$peerip'\"\
    \n}\
    \n:if (\$policyuid != \"\") do={\
    \n  /ip ipsec policy set \$policyuid sa-dst-address=\"\$peerip\"\
    \n  :log info \"Script ipsec-peer-update updated policy '\$peerid' with address '\$peerip'\"\
    \n}"
```
### Schedulers

This scheduler periodically executes enforced update of IP Cloud DDNS IP. Enforced IP Cloud update is used, because MikroTik router behind NAT does not always check public IP in required intervals. Therefore by scheduler and enforced update, this interval is controlled.

Configured intervals should reflect how promptly routers will detect and process public IP change, but also they should avoid any excessive usage. In case of often VPN connection break downs, because of public IP changes, it should be considered to use static public IPs instead and thus avoid IP changes altogether.

##### MikroTik router 1 and MikroTik router 2

/system scheduler
add disabled=yes interval=10m name=ip-cloud-forceupdate on-event="/ip cloud force-update" policy=read,write

As mentioned before, in the past, script was required to update IPSec addresses with remote DDNS IP. This is no longer the case, so creation of such scheduler is now skipped. Using this will rewrite the hostname set in peer’s configuration with IP address, so avoid setting this up if it is not necessary. The script execution scheduler looked like this:

/system scheduler
add disabled=yes interval=1m name=ipsec-peer-update-vpn01 on-event="/system script run ipsec-peer-update-vpn01" policy=read,write,test

### Netwatch and Route

Netwatch checks availability of remote MikroTik router’s LAN IP address. In case remote router is unavailable, Netwatch enables schedulers for update of IP Cloud DDNS IP and IPSec remote address (not needed anymore, so no longer present in this guide). When remote router is available again, schedulers are disabled back. Netwatch is not able to identify correct interface for remote router’s LAN IP, therefore route needs to be added as well. Make sure that gateway is set to your local router’s LAN interface or bridge.

##### MikroTik router 1

/ip route add comment="vpn01" distance=1 dst-address=10.10.20.0/24 gateway=bridge-local
/tool netwatch add comment=ipsec-peer-update-vpn01 down-script="/system scheduler enable ip-cloud-forceupdate" host=10.10.20.1 up-script="/system scheduler disable ip-cloud-forceupdate"

##### MikroTik router 2

/ip route add comment="vpn01" distance=1 dst-address=10.10.10.0/24 gateway=bridge-local
/tool netwatch add comment=ipsec-peer-update-vpn01 down-script="/system scheduler enable ip-cloud-forceupdate" host=10.10.10.1 up-script="/system scheduler disable ip-cloud-forceupdate"

## Result

Netwatch on both Mikrotik routers should detect unavailability of remote router and trigger down-script, enforcing IP Cloud update. If public IP did not change since IP Cloud has been set up. When the tunnel is up, Netwatch will detect presence of remote router via VPN tunnel and disable schedulers, until next public IP change will break the tunnel and trigger the Netwatch again. While tunnel is up, hosts in both private networks can communicate with each other.

## 77 Comments

## dabar · September 18, 2016 at 20:31

thank you for sharing!!

## george · October 14, 2016 at 07:14

I would appreciate a variation of this setup where only one of the routers uses dynamic IP while the other has known static IP (a “central office”).

## Pessoft · November 4, 2016 at 00:16

Setup mentioned in the article should work also in the case when one of the routers is connected using the static IP. In such scenario, for the router connected via static IP you don’t need IP Cloud (dynamic DNS) and ip-cloud-forceupdate scheduler and for the router connected via dynamic IP you don’t need ipsec-peer-update scheduler and temporary placeholder IP set in ipsec section ( 127.99.99.99/32 ) can be configured directly ( with the known static IP ).

## Joao · November 19, 2016 at 22:56

Hi,

i am stuck on phase1 with IPSEC error: phase1 negotiation failed due to time up……

both of routers are behind nat with ports (udp500, udp4500) opened and static ip addresses.

Pessoft, please contact me by email, i need to get this working.

Thanks

## chris · January 21, 2017 at 04:49

The best mikrotik site-site ipsec guide I have seen out there.

I have followed all the step and the connection is up but I can not ping the remote site. Can you help please?

## Pessoft · January 24, 2017 at 00:24

Thanks.

At first try to check IPSec. On both sides you should see in remote-peers

establishedconnection ( /ip ipsec remote-peers print ) and pair ofmatureinstalled SAs ( /ip ipsec installed-sa print ). Then try to ping remote Mikrotik’s internal IP and also IP of some device in remote network. If both, peers and SAs, are correct and ping still does not work via IPSec tunnel, but does locally, then it can be a routing issue. Also, I had issues with the IPSec NAT-T tunnel running on Mikrotik RouterOS 6.38 and had to upgrade to 6.38.1.
## kk · January 23, 2017 at 07:58

thanks, the script works well.

## Kevin · February 28, 2017 at 10:32

Hello，

Firstly, thanks for your doc, I have followed all the step and can get remote site public ip via IP Cloud each other but the ipsec phase I is still fail. can you help please ?

Secondary, I have one question for this solution. if I setup another Mikrotik box in one of the site and paste same config besides the ether1-gateway IP, the ipsec tunnel will be establish with which one?

## Pessoft · February 28, 2017 at 23:50

Hello,

Mikrotiks on both sites need to have IPSec traffic forwarded from their gateway routers ( in example above are these gateways called ISP routers ), so at first I suggest to check that there is 500/UDP and 4500/UDP forwarding configured on these gateways. Also check in which state are remote peers on Mikrotiks ( terminal command /ip ipsec remote-peers print ).

Regarding your second question: IPSec tunnel in config is configured to connect to remote site in case there is traffic going from local private network to remote private network ( in example above: if traffic will go from host in 10.10.10.0/24 network to network host in 10.10.20.0/24 network, then tunnel will be established from Mikrotik router 1 to Mikrotik router 2 and traffic sent through this tunnel – this works also vice versa ). So if you will have additional Mikrotik box in one of the sites with the same config as the other Mikrotik on the same site, it should work let’s say as its backup ( i.e. if there will be traffic going through additional box from local to remote private network, tunnel will be established to remote site ).

## DQ · February 28, 2017 at 10:34

Thanks Pressoft! I was not luck, your post was not working well on my side. I have almost same environment as shown in your topology. After I configured bother MT routers the IPSec tunnel was not up. in the logging file I could find that the phase one packet was sent out in both sides the routers could not receive response packets. What could be the potential reason?

I also have some questions which confused to me. Hope to get your answers.

1. What is the purpose of IP 127.99.99.99/32? is it a real IP or just an sample IP?

2. Is the IP an IP of a public stun server?

3. By using script and DDNS the MT router can know the public IP of peer, but how does the up lever NAT router know the received IPSec negotiation packets need to be forwarded to its direct connected MT router if there is no NAT translation table established before? vice verse.

Thanks a lot!

## Pessoft · March 1, 2017 at 00:16

Hi DQ,

At first make sure that 500/UDP and 4500/UDP traffic is being forwarded from gateways of your MTs to MT routers.

1. 127.99.99.99/32 is just a temporary placeholder IP, it will get replaced by IP of remote peer by the script

2. There is no STUN server used in the configuration, each MT knows about the IP of remote peer from the DDNS name of remote host.

3. There actually needs to be forwarding configured before ( in the article it is in the top – situation description point 4: Each MikroTik router has IPSec protocol, NAT-Traversal (4500/UDP) and IPSec IKE (500/UDP) traffic forwarded from its gateway (ISP Router) ). This is usually done on routers using configuration called port forwarding or DMZ host.

## Anton · May 27, 2017 at 22:49

This was the most useful guilde I have ever encountered! Works like a charm and it updates FAST!

## Anton · May 27, 2017 at 22:49

guide*

## Eduardo · July 25, 2017 at 15:36

Hello!.

I just connect two nets using this tutorial without problems. This is a great guide.

Thanks a lot!.

## Stephane · October 17, 2017 at 12:37

Thanks a lot.

Great job, great sharing .. please do share for us beginner Mikrotik admin, some of your useful tips.

## Dablah · November 13, 2017 at 23:57

Solo puedo decir, muchas gracias!

## hardoverflow · December 1, 2017 at 06:31

Thanks for your great guide! One question about routing. On every site i have a few vlans configured. Is it possible to route them over the ipsec tunnel ?

## Pessoft · December 8, 2017 at 22:36

Hi and thank you. Regarding VLAN routing, I didn’t test such configuration, but generally VLANs are working on OSI layer 2 and are terminated on routers when IP routing occurs. So VLANs on 2 sites are usually different logical networks. It might be worth a try to create a tunnel interface (GRE) on top of the IPSec and bridge the VLANs with tunnel. But as mentioned, I didn’t test such setup on Mikrotik platform, so it’s just a guess 😉 Another possibility (as an example) is to make sure that network in the VLAN 100 on the site A is configured correctly in terms of routing and firewall rules, that it can communicate with network in the VLAN 100 on the site B – and vise versa.

## Lars · April 7, 2019 at 18:07

In terms of VLAN routing over IPsec with GRE and with dynamic protocol

ls like RIPv2 you find a very good howto here:

https://administrator.de/wissen/cisco-mikrotik-vpn-standort-vernetzung-dynamischem-routing-398932.html

Unfortunately in German but the WinBox screenshots are self explaining. With “Pessofts” above guide to dynamic IPsec peer addresses it works like a charme !!

Also the added IPsec related links at the end are worth to read !

## zack12821 · December 4, 2017 at 11:45

Thanks Pressoft! I but not luck, your post was not working well on my side. I have same environment as shown in your topology is about one week that i trying to make it work but way , os 6.40.5

not packet send on ipfirewall, ipsec nagociat fail due to time up

## Pessoft · December 8, 2017 at 23:13

Hi! I tested the setup also on the 6.40.5 and it works well. Your description points out that IPSec communication is not flowing between the two routers. Here are the hints to check:

– verify that routers between Mikrotiks and Internet forward port 4500/UDP to Mikrotik device

– verify that firewall on Mikrotik accepts 4500/UDP

– if router on one site is more susceptible to block communication or difficult to configure, set on the opposing site Mikrotik’s IPSec peer configuration to be “passive” and also disable there “Send Initial Contact”

– try to change the IPSec peer exchange mode on both sites to IKEv2

## felixput · December 12, 2017 at 14:41

Hi Pessoft,

Thanks a lot for your guide, it’s really helpful. I got the IPsec connection established and I can reach both routers from both sides.

Unfortunately, I couldn’t ping any devices that other than the router from another side. When I check the connection, there is a ping request however it never got replied.

Can you help me on this one? Thanks

## Pessoft · December 15, 2017 at 00:22

Hi Felix,

Try to test between hosts on both sides. Using the Torch tool you should see incoming icmp packets on gateway interface. This should help you identify on which side is the issue or whether it is general. Also make sure that you have NAT firewall rules set and in the order prior to your masquerading or other src/dst nat rules, which could influence routing of the packets.

## Mathew · June 19, 2018 at 23:42

Hi Pessoft,

Thanks for the guide. I didnt find anything with this type of topology in mind. I tried to do everything here but cant establish IPsec IKEv2 connection between routers (ping). I dont have dynamic public ips so i used static public IP for SA and peers on both sides. Unfortunately at Remote Peers i have local address ether1-gateway address and remote address correct static public address and also no installed SAs. Can you please help me? This is same on both Mikrotik routers. Thanks in advance

## Pessoft · June 20, 2018 at 00:05

Hi Mathew,

Based on your input, it seems that addresses in Remote Peers are looking good. Check in which state your remote peer is using

`/ip ipsec remote-peers print value-list`. If it is established, then peer connection is fine. Otherwise have a look at logs what they say and also verify network between Mikrotiks: Are both local and remote ports in remote peer view using 4500/UDP? Are ISP routers forwarding 4500/UDP traffic to Mikrotik routers? Can you see packet counts increasing in Mikrotik firewall rules allowing 4500/UDP? Are there visible attempts to establish SA? This could give some information about what can be blocking the connection.
## Mathew · June 20, 2018 at 17:58

i have 4500 and it is increasing count and also peer connection is established. Log shows after 2 SA messages no policy found/generated

## Pessoft · June 20, 2018 at 22:01

It seems that peer connection works well, so the next step should be Policy review using

`/ip ipsec policy print`. Is private network behind Mikrotik set as source address and remote network behind Mikrotik as destination address? Is SA destination address set to remote public IP? This applies also vice-verse on the other Mikrotik. Can you see in the logs what policy is missing and verify the policy configuration accordingly? It might be also beneficial to check whether Mikrotik Router OS version is up-to-date on both sides.
## Mathew · June 20, 2018 at 23:46

Everything working. Thanks

## Charles · August 7, 2018 at 01:35

Great guide. Thanks.

Everything is working once the tunnel is created except that, whilst I can ping addresses apart from each router, I can’t ping one router from the other and vice versa.

Any ideas why that might be?

## Pessoft · August 9, 2018 at 21:16

Hi,

At first I suggest to check firewall configuration, whether it allows ICMP ping from ( and to ) the other router. Next thing is routing: in the section “Netwatch and Route” of this article is a route created, which makes the routers reachable between each other. Also Torch tool could help identify whether ICMP pings leave router via a correct interface.

## Lucky · October 26, 2018 at 18:11

Hi,

Can you explain exactly what part of the update script can be ommited if DNS names are used in peer configurations ?

## Pessoft · October 30, 2018 at 08:48

Hi,

ipsec-peer-update script updates 2 values: IP address of remote peer and SA destination address. If DNS names are used in remote peer configuration, parts of script related to update of remote peer can be omitted. So the command to create the script might look like this:

`/system script add name="ipsec-peer-update-vpn01" policy=read,write source=":local peerid \"vpn01\"\ \n:local peerhost \"0123456789.sn.mynetname.net\"\ \n:local peerip [:resolve \$peerhost]\ \n:local policyuid\ \n:set policyuid [/ip ipsec policy find comment=\"\$peerid\" and sa-dst-address!=\"\$peerip\"]\ \n:if (\$policyuid != \"\") do={\ \n /ip ipsec policy set \$policyuid sa-dst-address=\"\$peerip\"\ \n :log info \"Script ipsec-peer-update updated policy '\$peerid' with address '\$peerip'\"\ \n}"`
## Miguel · November 9, 2018 at 05:50

Thank you!!!! it’s a great guide. Everything working!

## Hannan · February 25, 2019 at 15:15

Nice guide. Really insightful and easy to understand. Thumbs up!!! Will try it with PureVPN…

## Mouhamed · February 26, 2019 at 08:51

Looks like RouterOS v6.43.12 makes the peer add command invalid.

It can no longer understand:

dh-group=modp4096 enc-algorithm=aes-256,aes-128 exchange-mode=ike2 hash-algorithm=sha512

Is there a way to get this to work considering the update?

Thanks!

## Pessoft · March 2, 2019 at 19:32

Hi Mouhamed,

Thanks for the info on peer command. I’ve reviewed the change log, tested the updated configuration on the latest stable RouterOS version 6.44 and updated the article. The arguments that are no longer understood by peer command in RouterOS 6.43.12 have been moved to the new profile object and are understood by the

`/ip ipsec profile`command. RouterOS 6.44 moved also arguments`auth-method`and`secret`to the new identity object and arguments are understood by the`/ip ipsec identity`command.
## Mike · March 13, 2019 at 10:56

Hi, will this Script also working if i use only one LAN port (ethernet2) to “inject” VPN between 2 Locations?

## Pessoft · March 13, 2019 at 21:57

Hi Mike,

Purpose of the script is to resolve the dynamic DNS hostname to an IP address and then update IPSec VPN configuration in a case new IP address is resolved. Script will work, if router is able to resolve the dynamic DNS hostname – probably in your case, as you mentioned, via the one LAN port.

## Kyaw Swa Hein · March 23, 2019 at 07:55

I got this message when ip cloud print “Router is behind a NAT. Remote connection might not work.”

Will this script work for router behind a NAT?

Thank in advance.

## Pessoft · March 26, 2019 at 20:40

Hi,

Yes, the point of the configuration is that both routers are behind NAT. IP Cloud sets the public IP of ISP’s router to a dynamic DNS entry – instead of the public IP of Mikrotik router, because Mikrotik is not connected directly to the public network. On remote side, script takes the IP of dynamic DNS entry and updates the IPSec configuration accordingly. Important point is that IPSec NAT-T traffic is forwarded by ISP router to the Mikrotik router.

## Kyaw Swa Hein · March 28, 2019 at 18:15

Thank you sir, I tried it and i still can’t ping to other client, but route show it reachable…..Can you please guide me where to fix by mail?

## Pessoft · April 1, 2019 at 20:19

Hi,

It might be caused by the firewall configuration. I sent you an email.

## Silvio · June 15, 2019 at 17:50

First thanks to the tutorial, works very well even through dynamic ip addresses.

The question is it possible to connect 3 (or more) mikrotik with VPN (all dynamic IP addresses). I was trying, but it will not work.

## Pessoft · June 16, 2019 at 23:21

Hi,

You can connect more Mikrotik’s together via VPNs. If you have 3 Mikrotiks (for example A, B, C) and you want each site to communicate with any other one, just add VPN between each site (i.e. A-B, B-C, A-C). You need to create every step of the guide again for a new VPN (also scripts). Make sure that you use separate configurations for each VPN, so they do not mix up. Alternative is also to have one Mikrotik configured so it routes between VPNs and then connect all other Mikrotiks there. This routing Mikrotik will then forward traffic between tunnels and will work as a central node. In this case you will just need each site to be connected to this central node (i.e. A-B, A-C).

## Silvio · June 26, 2019 at 00:31

Greeting,

the settings should be B-A-C, router A should work the tunnel according to B and C. Router A already has a tunnel set 10.10.20.0/24 – 10.10.10.0/24 – call it vpn1.

Question: Which ip addresses should be for another tunnel – vpn2? 10.10.30.0/24 – 10.10.40.0/24?

I got caught in Phase 2, I think that the Mikrotik communion is doing, assimilating SA Dst. address for each tunnel. Tunnel A connects almost immediately, but another tunnel B stops at Phase 2

## Silvio · June 27, 2019 at 16:04

Update,

all OK, firewall on ISP ADSL blocked everything possible. 🙁

If someone needs it: since I have more tunnels, on central MT, I used similar addresses 10.10.x.x. Therefore i used firewalls rules and nat 10.10.0.0/16. Each policies must have a special proposal, and peer a special profile.

Thanks.

## Pessoft · June 29, 2019 at 00:23

Hi Silvio,

Thank you for sharing your experience and I’m glad it works for you.

## Manuel · July 13, 2019 at 17:11

Hi Pessoft, i used your example and evrything works great, instead of use the 127.99.99.99 dummy address y use the actual public ip. Problem was when the ip changes. it won’t connect anymore.

How can i check if there is a part of the script not working or something

Thanks for your work!

## Pessoft · July 13, 2019 at 21:06

Hi Manuel,

127.99.99.99 is a dummy address, which is replaced automatically by the script with IP of remote Mikrotik’s dynamic DNS hostname ( IP cloud ). Watchdog enables schedule for the script whenever remote Mikrotik router is not reachable via VPN. You can check if script works, by setting again the dummy IP and then running the script manually. If IP is not replaced with remote Mikrotik’s IP by the script, then something with IP cloud configuration is not working ( you can check it further by trying to ping the remotehost name to see if it resolves to correct IP ) or incorrect hostname is set in peerhost variable of the script. You can also check if scheduler is working in the logs, where there should be the log message “changed scheduled script settings” whenever script schedule has been enabled and disabled ( when remote Mikrotik was not reachable and reachable again ).

## Manuel · July 17, 2019 at 17:18

Hi, and thanks for the fast reply.

I did all you said and also double check de configuration it still doesn’t replace the dummy ip but it resolve the host with the ping i notice in the log that there is nothing related to “changed scheduled script setting”, but when i run the script manually it should change the ip. i have a couple of cuetsions that i’m not sure

should i see the ip changed in the SA Dst. Address field of ipsec/policy tab and the address in ipsec/peers tab when the script runs? or the dummy ip stay there no matter what even when the ip really changes

the schedulers needs to be disabled when are created?

in ip/cloud the update-time must be unchecked?

thanks for everything, i really need this to work and is killing me

## Pessoft · July 18, 2019 at 00:01

Hi Manuel,

IP Cloud update-time decides whether time of your Mikrotik will be updated/synced from Mikrotik Cloud time servers. It is not related to configuration of IPSec tunnel and I have it unchecked as I sync the time using NTP from other sources. Schedulers when created are disabled, because netwatch controls later on when they are enabled and disabled again based on whether the remote Mikrotik is reachable. Dummy IP is there only at the beginning, as soon as the netwatch detects that remote network is not reachable, it enables the scheduler, scheduler executes the script at regular interval, script updates the IP address of IPsec configuration ( peer and SA ) with real IP addresses of remote Mikrotik. If manual start of the script updates the IP addresses in IPSec configuration for you correctly, then there is probably just an issue with your netwatch configuration. Can you check that host parameter of netwatch configuration on each Mikrotik is configured to the IP address of remote Mikrotik ( similar like mentioned in the example within article )?

## Agustin · August 16, 2019 at 17:20

Your post was really helpfull. It helped me to connect 5 different Nat network with each other. Thank you so much!

## Emiel · September 26, 2019 at 13:35

Thanks for this really helpfull post! Just one question, any idea why I’m not able to connect to the remote Mikrotik router using Winbox/via browser?

## Pessoft · September 29, 2019 at 23:55

Hi Emiel, run

`/ip service export verbose`and check for settings ofwwwandwinboxservices. They should have`disabled="no"`( means service is enabled ),`address=""`( means that connection is not restricted by IP range ) and`port="SOME_NUMBER"`( should be 80 for www and 8291 for winbox by default ). If that is ok, check if you have ports 80 for www and 8291 for winbox allowed in firewall input chain from selected IPs/ranges. Be careful that you don’t grant access to these management ports from Internet or other public network.
## Emiel · October 4, 2019 at 12:53

Thanks Pessoft, all settings are correct (by default). Strangely enough I can ping from local router to remote router. But I’m not able to ping from client (connected to locla router) to the remote router. In this case I’m able to connect to the (i.e.) NAS on the remote site and vice versa.

## Joak2602 · November 10, 2019 at 19:51

Hi Pessoft,

First of all, thank you very much for sharing a great tutorial. I have followed your configuration guide and it is working good and I am able to ping the remote route and network from both sides. I want to access internet through the IPSEC tunnel. For example, a device with IP 10.10.20.150 must connect to internet as it will be in 10.10.10.0/24 network. How can I get it working as expected? Thanks again

## Pessoft · December 8, 2019 at 23:29

Hi Joak, thank you for your feedback. Unfortunately, I didn’t yet configure Internet connection via the IPSec tunnel, so I cannot give you exact solution, but you can find a lot of hints on this, including configuration examples, on Mikrotik Forums.

## Kostas · November 24, 2019 at 21:42

Great tutorial, Thank you for sharing!!

Nice to add:

open VPN

## Pessoft · January 8, 2020 at 01:39

Thank you for your feedback. OpenVPN is great VPN technology, but its implementation in RouterOS is missing some security features and therefore I will not consider writing tutorial for it just now. Hopefully it will get improved in near future.

## Harshan · January 5, 2020 at 06:50

Hi,

Can I use a dynamic routing protocol (like OSPF) between the sites? I have more LAN segments in each sites. Usually we add the tunnel IP in OSPF. What is the tunnel IP here?

## Pessoft · January 7, 2020 at 20:41

Hi, this configuration does not use any IPs for tunnel configuration, so I don’t think that it is possible to have OSPF used – although I never tried it. Maybe creating GRE tunnel over IPSec could provide OSPF capability.

## Harshan · January 8, 2020 at 17:25

I have done it with GRE tunnel. But issue is with update script. Could you please help me with that. I have little or no knowledge on scripting side. Could you please email your mail ID?

## Daniel · January 29, 2020 at 17:36

Sir,

What’s your opinion on IPSec IKEv1 with PSK and XAUTH?

I need to connect three remote office branches to a central office, but all the Mikrotik routers will probably have to be NATted behind the fiber router installed by the ISP (I’d like to remove those crappy routers, but their policy is very strict)

IKEv1 with PSK and XAUTH seems to be the easiest and most viable option. Does it have any disadvantages compared to your method?

## Pessoft · January 29, 2020 at 22:33

Hi, method described in my article primarily addresses topics of dynamic IP assignment and ISP router on both sides. Using IKEv1 with PSK and XAUTH should work, assuming IPSec traffic ( or eventually NAT-T ) will not get blocked between your branch and central office. I’d suggest to use IKEv2 as it is more secure and has NAT-T embedded in it. Although IKEv2 does not support XAUTH, so you need to choose a different authentication.

## Rafael · August 4, 2020 at 20:36

Hello, thanks for your settings, they work!

But I have a problem, router1 doesn’t have access to router2’s computers, but router2 accesses router1’s computers, example:

Does not work

10.0.0.84 -> 192.168.1.173:80

It works

192.168.1.122 -> 10.0.0.22:80

Route1 ip 10.0.0.0/24

Route2 ip 192.168.1.0/24

Can you help me?

## Pessoft · August 16, 2020 at 21:35

Hello Rafael, check if VPN traffic reaches the NAT firewall rule or if some other rule is catching the traffic first in NAT table ( in such case reorder NAT rules accordingly ). Also check if you have firewall rule that accepts forwarded traffic of VPN. Both rules are mentioned in my guide, but your results might vary depending on your configuration. Also, just to be sure, check if other service on some other host is also unreachable ( maybe there is local firewall on the machine, … ).

## ismail · August 23, 2020 at 22:32

hi there thanks for all the info.

I would like to ask if you could maybe send me an updated script.

the script to update the peer address works well

but I would like it if the script could also maybe update the local IP/cloud address as well as the peer IP address so that it would be possible to access the VPN from my phone and table.

I hope that you understand what I mean and that you could maybe help me.

## Pessoft · August 24, 2020 at 21:51

Hi ismail, I’m not exactly sure what you need, but VPN tunnel should be accessible by devices connected to the Mikrotik as long as firewall is not blocking them. Also it is quite simple to add to the script setting of additional variables – Mikrotik Wiki can help greatly in this. Otherwise I suppose your request is a bit out of the topic of the article, so feel free to contact me with more details if you need more help.

## Emiel · December 24, 2020 at 18:14

Hi There,

Have been using your solution for quit some time running very stable. As of the newest stable version 6.48 I get an error “IPsec-SA expired before finishing rekey” every 30 minutes. This is the first version I encounter this error.

Do you have any clue why this is happening?

## Emiel · December 27, 2020 at 09:10

Found out it has to do with an update on IKEv2:

https://forum.mikrotik.com/viewtopic.php?f=2&t=147769#p740153

## gliappis · March 15, 2021 at 18:17

Hello, i want t set up the same site to site vpn.

At the isp routers, do you have make any port forwording? if i put the mikrotik as dmz host?

Thank you.

## Pessoft · March 15, 2021 at 23:27

Hello, yes, you need to forward the ports necessary for VPN to the Mikrotik. For IPsec IKEv2 with NAT traversal it will be port 4500/udp. Easy way is to do it using DMZ host as you mentioned, but that will forward all ports, so make sure you do not expose some service on Internet that you do not want to. Some routers also like to terminate IPsec connections if not specifically disabled in configuration, so check also your ISP router configuration or query your ISP for it, if your VPN packets do not come through.

## gliappis · March 16, 2021 at 13:49

Thanks! i put the mikrotiks at dmz! Now i have the ipsec established!

But i can’t ping mikrotik’s subnet hosts! i can ping local interface both of mikrotiks, but when i ping host i receive timeout! any ideas?

## gliappis · March 16, 2021 at 13:50

mikrotik 1 subnet 192.168.1.0/24, local ip 192.168.1.1

mikrotik 2 subnet 192.168.2.0/24, local ip 192.168.2.1

i can ping each other, but not the host from dhcp each side

## Pessoft · March 17, 2021 at 00:00

Most likely it is a firewall topic. Double check if there is a correct filter rule in forward chain which accepts forward between networks, as mentioned in guide, on both Mikrotiks. Also check if there is not some rule in the same chain which would affect the packets before accept rule. Next thing is to check nat rule in srcnat and dstnat chains: either you create accept rule in the same chain before any other nat rule is present ( i.e. before masquerade rule in srcnat chain ) or make sure that rules in nat table ignore IPsec. If all is ok, then check using Torch tool, where the packets get and if there are returning packets – this way you can identify if it is outgoing or incoming traffic that you need to fix. Also try ping from some DHCP host to IP of Mikrotik on remote site, which might help to identify where your packets get lost. Hope that helps.

## gliappis · March 17, 2021 at 12:55

i put at firewall raw, 2 routes for bypassing. Also i have to disable windows firewall, because win 10, does not allow ping from another subnet, only local.

And it works. The other think i have notice is that i have slow speed. The isp at both ends are 120 down/120 up, at i share a big file(3gb) and i saw speeds 3mb/sec. Any idea?

## Pessoft · March 19, 2021 at 01:43

It should be possible to configure Windows 10 firewall, so it accepts pings from more/all subnets and firewall could be enabled again then.

Regarding speed: Check real Internet connection speed at first, for example by speedtest.net to the server close to the location of remote Mikrotik. This should show if it is a general issue or issue with tunnel. Check also connection speed without using wireless network as that my impact the speed as well. Mikrotik also has a Bandwidth test, maybe you can give it a go as well to test connection between those with and without tunnel.

## Comments are closed.
