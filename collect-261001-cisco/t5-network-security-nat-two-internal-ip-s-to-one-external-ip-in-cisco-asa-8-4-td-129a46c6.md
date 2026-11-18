- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-07-2013 11:57 AM - edited 03-11-2019 06:40 PM
I found a link to accomplish this on the old code but how can I get this done on 8.4.
https://supportforums.cisco.com/thread/2014532
Solved! Go to Solution.
- Labels:
- 
						
							
		
			NGFW Firewalls
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-08-2013 09:25 PM
Hello Mohammad,
Let me see if I understand, this is for Inbound access to 2 different SMTP servers that are tasked with different dutties, if this is the case then I would go with the dedicated IP address or at least use the same IP address but with dedicated port numbers so we do not perform the sort of load-sharing that will be done here,
Again if the run the same service and duttie this will work as a charm as you will be be doing load-sharing...
Now going further, One is for inbound mail so I would do it like this
Object network SMTP_1
host 192.168.1.3
exit
object service SMTP
service tcp source eq 25 NAT1
exit
object network SMTP_OUT
host 1.1.1.1
exit
nat (inside,outside) 1 source static SMTP_1 SMTP_OUT service SMTP
object network SMTP_2
host 192.168.1.6
exit
nat (inside,outside) 2 source dynamic SMTP_2 SMTP_OUT
With this we will ensure that the port 25 will be always used by the Inbound traffic to server 1....
Let me know how it goes,
Julio Carvajal
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 08:32 AM
Hi,
Your original post and the linked post didnt mention anything about incoming traffic from the Internet/WAN/outside
If this is the actual case then you would need this version of the configuration
NAT Configuration When Using Separate Public IP address
object-group network SOURCE-ADDRESSES
network-object host 192.168.0.3
network-object host 192.168.0.6
object network NAT-IP
 host 
nat (inside,outside) 1 after-auto source dynamic SOURCE-ADDRESSES NAT-IP
object network SMTP-SERVER
host 192.168.0.3
nat (inside,outside) static NAT-IP service tcp 25 25
NAT Configuration When Using ASA "outside" Interface Public IP Address
object-group network SOURCE-ADDRESSES
network-object host 192.168.0.3
network-object host 192.168.0.6
nat (inside,outside) 1 after-auto source dynamic SOURCE-ADDRESSES interface
object network SMTP-SERVER
host 192.168.0.3
nat (inside,outside) static interface service tcp 25 25
Naturally you will have to make sure that you open the TCP/25 port on the ACLs on the ASA
Also possible existing configurations can affect if this configuration works or not. But it can be confirmed either with testing traffic OR using the "packet-tracer" command on the ASA
For example to test the incoming SMTP traffic
packet-tracer input outside tcp 1.2.3.4 12345 
To test the outgoing traffic from the hosts
packet-tracer input inside tcp 192.168.0.3 12345 1.2.3.4 
packet-tracer input inside tcp 192.168.0.6 12345 1.2.3.4 
Hopefully this helps
Remember to mark the question as answered if it was. Or ask more if needed.
- Jouni
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-07-2013 12:04 PM
Hi,
If you are trying to achieve the equivalent of
access-list nat-to-one permit ip host 192.168.1.1 any
access-list nat-to-one permit ip host 192.168.1.2 any
nat (inside) 2 access-list nat-to-one
global (outside) 2 83.111.111.1
Then you can do this
object-group network SOURCE-ADDRESSES
network-object host 192.168.1.1
network-object host 192.168.1.2
object network NAT-IP
host 83.111.111.1
nat (inside,outside) 1 after-auto source dynamic SOURCE-ADDRESSES NAT-IP
Hope this helps
EDIT: Typo
- Jouni
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-07-2013 12:09 PM
If you want to make sure that the above NAT rule is matched before any other NAT rule on the firewall then you could use this command instead of the above one
nat (inside,outside) 1 source dynamic SOURCE-ADDRESSES NAT-IP
Basicly what I did was only remove the "after-auto" parameter. With "after-auto" parameter the rule is moved to Section 3 (of 1 - 3) of the NAT rules. Without it its inserted in Section 1, which makes it one of the first NAT rules matches against connections/traffic.
In addition to have the number "1" there means that the NAT rule is inserted at the very top of the NAT rules of Section 1. It wont remove any existing rules there. It will just be on top of them.
If you have some NAT0 rules for VPN for these source addresses THEN I would suggest to insert it WITHOUT the line number or it might break the NAT0 rule previously configured for VPN. (Since it would be matched for traffic source from these hosts to any host on the "outside" and being line number 1 would override any other rule between "inside" and "outside")
- Jouni
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-07-2013 12:11 PM
Thank you, I will give it a try and post back .
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-08-2013 07:16 PM
Update, so I was able to configure it like that however next issue I ran into was that I was seeing a whole bunch of denies from the google postini servers trying to communicate with the NATed public IP, so seems like I might have to utilize additional public IP address.....?
| 4 | May 08 2013 | 20:38:53 | 106023 | 64.18.3.111 | 45388 | 1.1.1.1 | 25 | Deny tcp src outside:64.18.3.111/45388 dst outside:1.1.1.1/25 by access-group "outside_in" [0x0, 0x0] | 
So I ended up doing something like this:
object network NAT1
host 192.168.0.3
nat (inside,outside) static 1.1.1.1
!
object network NAT2
HOST 192.168.0.6
nat (inside,outside) static 1.1.1.1
So now both email servers inbound and outbound are using the same non RFC1918 IP for the outside world. Is this a best practice though? I think it would be better to have two different public IP's instead and in case of two mail servers arrange the mapping accordingly like:
mail1.yourdomain.com --> 192.168.0.3 --> 1.1.1.1 (inbound mail)
mail2.yourdomain.com --> 192.168.0.6 --> 1.1.1.2 (outbound mail)
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-08-2013 09:25 PM
Hello Mohammad,
Let me see if I understand, this is for Inbound access to 2 different SMTP servers that are tasked with different dutties, if this is the case then I would go with the dedicated IP address or at least use the same IP address but with dedicated port numbers so we do not perform the sort of load-sharing that will be done here,
Again if the run the same service and duttie this will work as a charm as you will be be doing load-sharing...
Now going further, One is for inbound mail so I would do it like this
Object network SMTP_1
host 192.168.1.3
exit
object service SMTP
service tcp source eq 25 NAT1
exit
object network SMTP_OUT
host 1.1.1.1
exit
nat (inside,outside) 1 source static SMTP_1 SMTP_OUT service SMTP
object network SMTP_2
host 192.168.1.6
exit
nat (inside,outside) 2 source dynamic SMTP_2 SMTP_OUT
With this we will ensure that the port 25 will be always used by the Inbound traffic to server 1....
Let me know how it goes,
Julio Carvajal
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 08:23 AM
Thank you for the email, so here is what the servers are doing....
192.168.0.3 --> E Mail comes in to this server
192.168.0.6 --> Sends E Mail out this is Ciscos Iron Port device.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 08:32 AM
Hi,
Your original post and the linked post didnt mention anything about incoming traffic from the Internet/WAN/outside
If this is the actual case then you would need this version of the configuration
NAT Configuration When Using Separate Public IP address
object-group network SOURCE-ADDRESSES
network-object host 192.168.0.3
network-object host 192.168.0.6
object network NAT-IP
 host 
nat (inside,outside) 1 after-auto source dynamic SOURCE-ADDRESSES NAT-IP
object network SMTP-SERVER
host 192.168.0.3
nat (inside,outside) static NAT-IP service tcp 25 25
NAT Configuration When Using ASA "outside" Interface Public IP Address
object-group network SOURCE-ADDRESSES
network-object host 192.168.0.3
network-object host 192.168.0.6
nat (inside,outside) 1 after-auto source dynamic SOURCE-ADDRESSES interface
object network SMTP-SERVER
host 192.168.0.3
nat (inside,outside) static interface service tcp 25 25
Naturally you will have to make sure that you open the TCP/25 port on the ACLs on the ASA
Also possible existing configurations can affect if this configuration works or not. But it can be confirmed either with testing traffic OR using the "packet-tracer" command on the ASA
For example to test the incoming SMTP traffic
packet-tracer input outside tcp 1.2.3.4 12345 
To test the outgoing traffic from the hosts
packet-tracer input inside tcp 192.168.0.3 12345 1.2.3.4 
packet-tracer input inside tcp 192.168.0.6 12345 1.2.3.4 
Hopefully this helps
Remember to mark the question as answered if it was. Or ask more if needed.
- Jouni
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 09:19 AM
Thank you for the reply and my apologies for not elaborating more. Yes that is what I was trying to do. I did what you suggested but there is an issue with that configuration because we have a PAT setup so if I do after auto then the NAT rule comes after that PAT for all the outbound internet traffic and it does not work.
If I remove after auto and just use 1 then it puts it all the way on the top and then the incoming mail does not reach us. So here is what I did:
object-group network Email_InOut
network-object host 192.168.0.3
network-object host 192.168.0.6
exit
object network obj-1.1.1.1
host 1.1.1.1
exit
nat (inside,outside) after-auto 1 source dynamic Email_InOut obj-1.1.1.1 (did not use this)
object network Inbound_Email
host 192.168.0.3
nat (inside,outside) static 1.1.1.1 service tcp 25 25
exit
object network IronPort
host 192.168.0.6
 nat (inside,outside) static 1.1.1.1
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 09:43 AM
Hi,
Yeah, since we dont see the full NAT configuration we wont know how the current existing configuration affects what we are trying to achieve.
The reason why I personally suggest configuring Network Object NAT for the Static PAT / Port Forward AND Twice NAT/Manual NAT type of configuration for the Dynamic PAT is how I personally order the NAT rules in my configurations
- Static PAT and Static NAT always as Network Object NAT (Section 2)
- Default Dynamic PAT/NAT always as Twice NAT / Manual NAT (Usually Section 3 in special cases Section 1)
- Special NAT setups like NAT0 and Policy NAT/PAT type configurations as Twice NAT / Manual NAT (Section 1)
Naturally Static PAT and Static NAT can be done in the Section 1 also but I prefer keeping strict roles for every Section and so far it has worked for me.
I wrote a NAT 8.3+ Document which pretty much states the way I configure and section the different type of configurations. Have a look if you want. Will probably add a lot more information to it later
https://supportforums.cisco.com/docs/DOC-31116
To me it seems the NAT configuration ordering/sectioning is causing the problems why the suggest configurations dont work. The existing configurations is set up so that it overrides the configurations suggested.
Glad to hear you got it working though
- Jouni
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 10:00 AM
Yes you are right the existing NAT configuration was causing the issue so I had to modify it a bit but it seems to be working. Thank you for the document link, good read. I have saved it
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
05-09-2013 09:42 AM
Hello Mohammad,
Exactly, the configuration I sent will do it
Regards
Senior Network Security and Core Specialist
CCIE #42930, 2xCCNP, JNCIP-SEC