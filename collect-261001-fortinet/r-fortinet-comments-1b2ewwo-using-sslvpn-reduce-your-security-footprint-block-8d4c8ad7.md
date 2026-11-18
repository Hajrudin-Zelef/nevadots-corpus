Using SSLVPN? Reduce your security footprint - Block access from unnecessary threats. 
        
    During the surge in SSLVPN attempts on our UTMs stemming from the Ivanti CVE's, I collected a lot of data on where access attempts were coming from and I wanted to share what I found.
We have our services on a loopback interface and only allow for US IP's to connect. From the US, we received plenty of failed login attempts, probes to port 20443 (incoming connection, close-notify, etc.) From all those failed attempts and prying scans, I blocked many unwanted subnets and assembled a list of offending ASNs.
We had results from many ASN categories, but hosting was the primary one. I assume either compromised servers or VPN services being hosted by them. I only play whack-a-mole for so long and any time I had 10 or more hits from a specific hosting ASN, I blocked the entire network from our listening ports. In our use case, our staff only connect from ISPs and I saw no reason for hosting networks to ever need to connect to our VPN ports. The biggest offenders were:
      AS212238 - Datacamp Limited
AS14061  - Digital Ocean
AS398101, AS398108, AS26496, AS400754 - Godaddy
AS14576 - HostingSolution LTD
AS46562 - Performative
AS8075 - Microsoft
AS62240 - Clouvider
    
Many here recommend emailing the abuse lines for these networks. That can be done but as soon as they do something about one IP, another one gets used and I dont want to send hundreds or thousands of emails.
After blocking those ASNs, I get maybe 1 unwanted vpn connection attempt per day now. According to the hit count on our deny policy, we've quietly rejected 187,000 attempts in the last 20 days.
Our firewalls are updated and no longer vulnerable, but who knows if another bug will be found tomorrow. By reducing your footprint proactively, you reduce the risk of being reachable by networks that could pose a threat in the future.
Section des commentaires
oh, nice i will implement these as well. i did not think about blocking the whole ASN for various providers, i did it more manaully by looking up the IP address space for things like cloudflare and blocking all of those in a threat feed. this is a lot more elegant and dynamic.
to help with the whack a mole as you put it, i have automation scripts that will auto block the 20x most common brute forced usernames i have seen.
this works well, but because the trigger configuration field options only have an AND option not an OR option, a separate trigger and task is needed per desired username. However the action only needs to be configured once and all of the stitches will use the same action.
here is my example for the username "guest" notice that it uses *uest*
so if the word guest is lower or upper case then it will be caught.
note: i use a loopback interface with the address group below blocked. I do analyze the entries in the address group when i get to between 100-150 entries. i will then add them to external thread feed files which my loop back interface also blocks. i will use whois look ups to determine the larger IP address ranges that the individual /32 addresses are part of and block that entire ranges in my threats feed. once addresses are in my threats feed, then i remove the IP address objects from the fortigate config as the units do have hard limits (few thousand) on the number of address objects.
Trigger
config system automation-trigger
edit "SSL_VPN_USER_SSL_LOGIN_FAIL_guest"
set description "SSL_VPN_USER_SSL_LOGIN_FAIL"
set event-type event-log
set logid 39426
config fields
edit 1
set name "user"
set value "*uest*"
next
end
next
end
ACTION
config system automation-action
edit "Block_SSL_Failed"
set description "Block_SSL_Failed"
set action-type cli-script
set script "config firewall address
edit SSL_VPN_Block_%%log.remip%%
set subnet %%log.remip%%/32
end
config firewall addrgrp
edit Block_SSL_Failed
append member SSL_VPN_Block_%%log.remip%%
end"
set accprofile "super_admin"
next
edit "SSL_VPN_Block"
set description "SSL_VPN_Block"
set action-type email
set email-to "
email@email.com"
set email-from "
email@email.com"
set email-subject "SSL VPN IP Auto Blocked"
set message "%%log.remip%% address has been added to the address group Block_SSL_Failed"
next
end
the actual stitch
config system automation-stitch
edit "SSL_VPN_Block_guest"
set description "SSL_VPN_Block"
set trigger "SSL_VPN_USER_SSL_LOGIN_FAIL_guest"
config actions
edit 1
set action "Block_SSL_Failed"
set required enable
next
edit 2
set action "SSL_VPN_Block"
set required enable
next
end
next
end
I think you posted something similar to another comment I made some weeks ago :)
I always block the whole listed subnet for the offending IP and then track the ASN it originated from. Basically keep a tally. Eventually I get tired of it and just block the whole ASN as I mentioned.
In your approach, common offending usernames earn themselves a block, but dangerous ASNs may still be able to try other usernames later. Depending on the CVE you or I might be trying to mitigate, this could leave you open to malformed packets and other approaches used by attackers. While reviewing access logs, I also see a lot of SSL 'CloseNotify' messages on our listening port. Sometimes 300-400 in sequence from the same IP over just a couple minutes. Since its not a legitimate login attempt there is no 'failed to login'. These probes can still carry dangerous payloads sometimes.
Finally, I would imagine your address objects in your firewalls can get a little out of hand after a while if you dont clean them up. Feeds are a great way to keep things organized.
it doesnt work for 7.2.8 ;/
I work for IPinfo and here are "all" the ranges that belong to these ASNs. I am using our free IP to ASN database for this. Ping me if you need any other IP metadata like this.
https://gist.github.com/abdullahdevrel/54320e40394c84da50bd567457dcffa3
You all patrol reddit subs pretty aggressively. :)
Anytime I mention ipinfo im almost certain to get a reply from someone who represents them.
Great service by the way!
Here I was trying to get chatgpt to help me code a way to consolidate all of these lists into a free github account I was trying to create to host this list. I did not want to create 30 different threat feeds in my fortigate and have to manage adding it to all of my policies.
Do you mind sharing how you merged the multiple sources into a single .txt file if this a freely available tool or is this something you have internally working there that makes it easier to do? I'd like to store something on a github I control just for the sake of me being able to potentially add more IP's to it easily and not having it disappear if someone decides to move/delete it.
To add to your list, this is the ASNs we have blocked as well. I think some of these were before I setup a loopback and didn't understand how the firewall worked on SSL-VPN (not the same at all), so feel free to block whichever makes sense:
AS6939 Hurricane Electric LLC
AS8100 QuadraNet Enterprises
AS9009 M247 Europe SRL
AS11878 tzulo
AS13213 UK-2 Limited
AS13335 Cloudflare
AS20093 Performive LLC
AS46562 Performive LLC
AS53356 Free Range Cloud Hosting
AS54203 Netprotect SP
AS61317 Hivelocity Inc
AS63949 Linode
AS203020 HostRoyale
AS212238 Datacamp Limited
AS263735 Buena Hosting
AS263740 Corporacion Laceibanetsociety
AS14061 DigitalOcean
AS398108 GoDaddy.com
AS265506 ISD NETWORKS
AS35196 Ihor Hosting
AS4766 Korea Telecom
AS34665 Petersburg Internet Network
AS49505 SELECTEL
AS263735 SOCIEDAD BUENA HOSTING
AS46337 Website Hosting
Thanks!
I have many of these on my naughty-list and handfuls of their IP blocks are already in my feeds, but they havent spammed me enough to be completely blocked yet.
Things like Korea telecom probably woudnt get noticed by me as following my deny policy, the next policy only allows US based IPs to connect (geo filter)
Is there an easy way to group threatfeeds together in a single line item in the firewall policy? I scripted the CLI add of a bunch of the threat feeds but I really do not want like 40 lines in a single policy. It would be nice to group these feeds together in a group called "Threatfeed-Consolidated" or something so it is a single line. Is that possible? When I go to create a group in the fortigate it does not allow me to add the threat feeds to it.
Thanks
Not entirely sure.
I manage my own threat feeds internally. Example, I have all godaddy ASNs fetched and appended together into a single TXT file on an internal webserver. They get updated monthly. If for any reason one of the sites I source my lists from during a reboot or internet outage isnt available, I dont want to have a gap in availability. Threat feeds have to reload/refresh after a restart and I want the data in-house for that.
yes, i have exactly that. it takes 70x different ASNs i am blocking
lists here: https://github.com/wallacebrf/dns/blob/main/ASN_LIST.txt
and turns them into a single file i can use in a single external thread feed
script is here: https://github.com/wallacebrf/dns/blob/main/ASN_block_lists_all.php
Maybe I'm stupid, but how are you blocking the ASNs? Is there some translation into a threat feed or something? Or are you just taking a full internet BGP table and dropping those routes?
I use ASN info to look them up. For example: https://asn.ipinfo.app/AS8100
You can then go to Resources -> Blacklists -> List -> Text and you get this https://asn.ipinfo.app/api/text/list/AS8100
You can then create an external feed, set it to update maybe once an hour or more, then add that external feed into your firewall policy. It's great and easy to do.
I saw a comment in a different thread where someone was creating lists of IPs from https://bgp.he.net/ and then creating pastebins with the ips.
Did you happen to crosscheck these with existing ISDB for malicious IPs or with any other public threatfeeds?
Block TOR networks if you haven’t already.
Ive noticed a few IPs listed as TOR exit nodes. Havent seen many of them yet though. Trick is that some of the nodes are listed as ISPs not Hosting providers. I try to be careful with blocking ASNs of ISPs as that might cause problems for traveling employees.
Fortinet ISDB and External IP block list like abuseIP or another other doesnt help much in this case?
Its easier to manage my own lists. When botnets and attackers started spamming attempts on the ivanti exploit, it opened my eyes to how many silent hosts were just waiting to become active.
Since we have no reason for hosting ASNs to login to our VPN, there is no reason to keep their IPs unblocked. If tomorrow there is a 10/10 CVE from fortinet, im already sitting better than many others as networks known to be used for these attacks are already blacked out for me.
I used to volunteer as a moderator for a phpbb forum, and for a couple of months we had a persistent spammer who kept posting successfully no matter how many times we blocked their address.
I spent an evening examining the attacks, and determined initially that they used a different IP address each time, but they were all withing a common address block. I then verified that nobody else was posting from that address block, so we were safe to block off the entire thing.
It didn't help. The spammer kept getting through anyway, for several other distinct address blocks.
I examined WHOIS records for those address blocks, and they all had a common owner. I then searched WHOIS for that owner, and discovered more than 50 additional large blocks of addresses owned by the same spammer.
I wrote up the results of my research and sent it to the owner of the phpbb forum, and recommended he block all of them. He did so, and the spammer never posted again afterward.
MS-ISAC and cisa already have the list complied and you can hook your gate into it by using the external threat feeds in security fabric
u/CallEither683 : Can you refer to exact link of MS-ISAC and cisa?
Thanks for posting this. I've now implemented this in my environment and am seeing way less spam in my logs.
What a great idea! I will definitely be using this.
Excellent idea this and some brilliant variations on theme. I’m going to start doing this for all my ssl von endpoints as well.