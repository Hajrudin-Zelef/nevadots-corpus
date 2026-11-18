@UI-Team
Currently, on a VLAN, walled off from my trusted LAN, I have a UniFi-Video NVR, about 20 UniFi cameras, and four Samsung tablets running the UniFi-Video app to serve as monitors for the security video. The NVR stores the video files on a NAS that I’ll discuss below.
For a litany of reasons, I believe the NVR is the weakest link in my cyber-security (e.g. obsolete OS no longer patched, potential access from the internet, etc.). Thus, I really don’t want it on my Trusted LAN.
Here is the rub: I have a single NAS that I share between my Trusted LAN and the NVR (currently) on the VLAN via an NFS share. I do this by having one NIC from the NAS on the Trusted LAN and another NIC on the VLAN. This seems like a bad idea for security. The best answer would be to buy an additional NAS, but that takes space, maintenance, and lots of $, so I’m looking for a compromise solution.
Since I cannot block access to the NAS from its own subnet & VLAN, I think I must remove that from the VLAN, leaving it only on the Trusted LAN. Then I have to create firewall rules to allow NFS traffic between the NVR (on the VLAN) and the NAS (on the Trusted LAN).
I’m looking for a sanity check (confirmation that my current method is foolish and my proposed method is in fact the right way to do it), as well as some clues on building the firewall rule. The NAS is a Synology Rackstation 1219+, which uses port 111 (TCP & UDP) for portmap, port 2049 (TCP & UDP) for the nfsd, and port 892 (TCP & UDP) for mountd.
…and the rest, which I’m not sure I need: The statd daemon uses a dynamic port, but there is no mention of it in the Synology NFS setup docs, so I’m guessing it is standard practice to just do without the reboot/lost-lock notification service – I can live with that. I also did not find any lockd or rquotad daemons running at all on the Synology, so I’m guessing again here, that it is standard practice to do without locking on NFS shares, so I don’t need to figure out how to make those ports static and make firewall rules for them. I’m probably overthinking the NFS service, but the idea of *no locking* just seems like it needs confirmation from somebody before I can get my mind to accept it.
As an aside, I wonder about not having the rquotad daemon. The UniFi-Video controller checks free space and deletes the oldest files to keep a certain amount (which you can set in the GUI) of free disk space. Without being able to query the rquotad daemon on an NFS-hosting server, UniFi-Video cannot know the quota, and thus, if it is less than the NAS capacity minus the “free space” setting in the UniFi-Video controller GUI, it will likely hang when it reaches the quota. Maybe it was naïve to assume the UniFi-Video controller would attempt to do any of that, even when it did have unfettered access to the NFS server. Should I assume that it just does a “df” command to get free space and nothing further? That’s OK – I just have to do some math and remember to update the setting whenever I change the total amount of storage in the NAS.
Does anybody have an EdgeRouter firewall snippet they’d be willing to share for such a configuration (UniFi-Video NVR in a VLAN, talking to an NFS share on the Trusted LAN)? I’m imagining complexities involving rules to prevent spoofed IP addresses from the WAN interface and all kinds of scenarios, but I’m sure I’m overthinking this aspect too.
I think I figured out the firewall config to have the NAS on the Trusted LAN while UniFi-Video is on the surveillance VLAN. I created some groups:
admin@EdgeRouter-Lite-3-Port:~$ show firewall group
Name       : NFS
Type       : port
Description: NFS and mount
References : SURVEILLANCE_IN-20-destination
Members    :
             111
             892
             2049
Name       : NAS
Type       : address
Family     : IPv4
Description: NAS address
References : SURVEILLANCE_IN-20-destination
Members    :
             192.168.0.201
Name       : PRIVATE_NETS
Type       : network
Family     : IPv4
References : GUEST_IN-1-destination, IOT_IN-30-destination, SURVEILLANCE_IN-30-destination, balance-10-destination
Members    :
             10.0.0.0/8
             172.16.0.0/12
             192.168.0.0/16
admin@EdgeRouter-Lite-3-Port:~$
And made the following rule:
admin@EdgeRouter-Lite-3-Port:~$ show firewall
--------------------------------------------------------------------------------
<snip>
--------------------------------------------------------------------------------
IPv4 Firewall "SURVEILLANCE_IN":
 Active on (eth0.30,IN)
rule  action   proto     packets  bytes
----  ------   -----     -------  -----
10    accept   all       481      140087
  condition - state RELATED,ESTABLISHED
20    accept   tcp_udp   236      43204
  condition - match-DST-ADDR-GROUP NAS match-set NFS dst
30    drop     all       0        0
  condition - match-set PRIVATE_NETS dst
40    drop     all       10       400
  condition - state INVALID
10000 accept   all       359      66952
--------------------------------------------------------------------------------
<snip>
--------------------------------------------------------------------------------
admin@EdgeRouter-Lite-3-Port:~$
(long version:)
 name SURVEILLANCE_IN {
     default-action accept
     description "surveillance to lan/wan"
     rule 10 {
         action accept
         description "allow established/related"
         log disable
         protocol all
         state {
             established enable
             invalid disable
             new disable
             related enable
         }
     }
     rule 20 {
         action accept
         description "allow nfs to nas"
         destination {
             group {
                 address-group NAS
                 port-group NFS
             }
         }
         log disable
         protocol tcp_udp
     }
     rule 30 {
         action drop
         description "drop surveillance to lan"
         destination {
             group {
                 network-group PRIVATE_NETS
             }
         }
         log disable
         protocol all
     }
     rule 40 {
         action drop
         description "drop invalid state"
         log disable
         protocol all
         state {
             established disable
             invalid enable
             new disable
             related disable
         }
     }
 }
------------------------------------------------------------------------
<... and the interfaces defined...>
------------------------------------------------------------------------
admin@EdgeRouter-Lite-3-Port# show interfaces
 ethernet eth0 {
     address 192.168.0.1/24
     description Local
     duplex auto
     speed auto
     vif 20 {
         address 192.168.20.1/24
         description Guest
         firewall {
             in {
                 name GUEST_IN
             }
             local {
                 name GUEST_LOCAL
             }
         }
         mtu 1500
     }
     vif 30 {
         address 192.168.30.1/24
         description Surveillance
         firewall {
             in {
                 name SURVEILLANCE_IN
             }
             local {
                 name SURVEILLANCE_LOCAL
             }
         }
         mtu 1500
     }
     vif 40 {
         address 192.168.40.1/24
         description IoT
         firewall {
             in {
                 name IOT_IN
             }
             local {
                 name IOT_LOCAL
             }
         }
         mtu 1500
     }
 }
 ethernet eth1 {
     address dhcp
     description WAN
     duplex auto
     firewall {
         in {
             name WAN_IN
         }
         local {
             name WAN_LOCAL
         }
     }
     speed auto
 }
 ethernet eth2 {
     address dhcp
     description "WAN 2"
     duplex auto
     firewall {
         in {
             name WAN_IN
         }
         local {
             name WAN_LOCAL
         }
     }
     speed auto
 }
 loopback lo {
 }
[edit]
admin@EdgeRouter-Lite-3-Port#
exit
Are there any holes that jump out at ya?
--Thanks,
--Jim
P.S. After further research on the side issue, I'm 100% convinced that the "Space To Keep Free" setting on UNiFi-Video (and surely on UniFi-Protect as well) can only screw you (i.e. risk write fails due to insufficient space) when using a NAS with a shared folder quota. The best answer was to remove the quota and use time-based purging as a very rough proxy for keeping the security videos from using too much of the NAS storage space. There is probably a custom solution to be had, but that is for another thread and another day.
These cookies enable the website to provide enhanced functionality and personalisation. They may be set by us or by third party providers whose services we have added to our pages. If you do not allow these cookies then some or all of these services may not function properly.
These cookies allow us to count visits and traffic sources so we can measure and improve the performance of our site. They help us to know which pages are the most and least popular and see how visitors move around the site. All information these cookies collect is aggregated and therefore anonymous. If you do not allow these cookies we will not know when you have visited our site, and will not be able to monitor its performance.
These cookies may be set through our site by our advertising partners. They may be used by those companies to build a profile of your interests and show you relevant adverts on other sites. They do not store directly personal information, but are based on uniquely identifying your browser and internet device. If you do not allow these cookies, you will experience less targeted advertising.
These cookies are necessary for the website to function and cannot be switched off in our systems. They are usually only set in response to actions made by you which amount to a request for services, such as setting your privacy preferences, logging in or filling in forms. You can set your browser to block or alert you about these cookies, but some parts of the site will not then work. These cookies do not store any personally identifiable information.