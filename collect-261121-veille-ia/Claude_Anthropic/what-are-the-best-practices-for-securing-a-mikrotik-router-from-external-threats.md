---
id: collect-261121-veille-ia/claude-anthropic/what-are-the-best-practices-for-securing-a-mikrotik-router-from-external-threats
title: "What are the best practices for securing a MikroTik router from external threats?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["disabled", "service", "address", "mikrotik", "global", "router", "like", "certificate"]
source: https://forum.mikrotik.com/t/what-are-the-best-practices-for-securing-a-mikrotik-router-from-external-threats/177761
source_anchor: ""
source_lines: [1, 243]
section: "Claude_Anthropic"
---
I’ve recently set up a MikroTik router for my home network to play the ****** game, and I’m concerned about potential security vulnerabilities. I’ve configured the basic firewall rules and updated the firmware, but I’d like to know more about advanced security measures. What are the best practices for securing a MikroTik router from external threats, and are there any specific configurations or tools that can help enhance the security of my network? Any advice on monitoring and responding to security incidents would also be appreciated.

             
                
            
           
          
            
            
              
What are the best practices for securing a MikroTik router from **external** threats?




Leave firewall to default values and do not touch what you do not know, do not follow “youtube & co.” advices.

             
            
           
          
            
            
              Who is that Leonardo de Pussy ??

             
            
           
          
            
            
              It looks like a combination of Vincent van Purr and Claude Meowet

             
            
           
          
            
            
              
If possible disable all /ip/services and only use consolecable to configure your Mikrotik.

Then there are other improvements like VRF etc.

If you choose to enable /ip/services make sure to add a list clients based on IP-address like so (below example have ssh and www enabled and everything else disabled including the mac-based backdoor - also the example assumes your mgmt-client uses IP-address 192.168.1.2 so you need to change that to whatever IP and/or range your mgmt-client will be using):

```
/ip service set telnet address=192.168.1.2/32 disabled=yes
/ip service set ftp address=192.168.1.2/32 disabled=yes
/ip service set www address=192.168.1.2/32 disabled=no
/ip service set ssh address=192.168.1.2/32 disabled=no
/ip service set www-ssl address=192.168.1.2/32 disabled=yes certificate=$myCERT tls-version=only-1.2
/ip service set api address=192.168.1.2/32 disabled=yes
/ip service set winbox address=192.168.1.2/32 disabled=yes
/ip service set api-ssl address=192.168.1.2/32 disabled=yes certificate=$myCERT tls-version=only-1.2
/tool mac-server set allowed-interface-list=none
/tool mac-server mac-winbox set allowed-interface-list=none
/tool mac-server ping set enabled=no
/tool romon set enabled=no
```

In order to enable www-ssl (and disable www) you need a certificate in your box, one way to set such up is using a selfsigned cert like so:

```
:global myCERT "WEBFIG";
:global myCERTCN "TEST.example.com";
:global myCERTSAN "IP:192.168.1.88";
:global myCERTO "EXAMPLE.COM";
:global myCERTOU "TEST";
:global myCERTC "SE";
:global myCERTVALID "730";
:global myCERTDATE "2024-08-01";
:global myCERTTIME "12:00:00";
/system/clock/set date=$myCERTDATE time=$myCERTTIME
/certificate add name=$myCERT digest-algorithm=sha256 country=$myCERTC organization=$myCERTO unit=$myCERTOU common-name=$myCERTCN key-size=2048 subject-alt-name=$myCERTSAN days-valid=$myCERTVALID trusted=yes key-usage=digital-signature,key-cert-sign,crl-sign,tls-server 
/certificate sign $myCERT
/ip service set www address=192.168.1.2/32 disabled=yes
/ip service set www-ssl address=192.168.1.2/32 disabled=no certificate=$myCERT tls-version=only-1.2
[code]
Of course you can and need adjust the above to your choice like if you want key-size=4096 instead of 2048 etc. Note that increased keysize will take more time to generate the cert but also use more cpu once you login to your Mikrotik using https. So unless you administer your Mikrotik over the Internet a keysize of 2048 bits is often good enough even these days.
Edit: Make sure to use latest stable release which is 7.15.3 stable as of writing.
```

             
            
           
          
            
            
              **Why??**

/tool mac-server mac-winbox set allowed-interface-list=**none**

Its an encrypted protocol/service, what should be said if using winbox, make sure this is set to the TRUSTED subnet/interface.

             
            
           
          
            
            
              I find somewhat intriguing how half the new members posts asking how to access the router after having managed to lock themselves out, and the other half posts asking for recipes that ultimately increase the risk of locking oneself out.

Seriously, one thing is doing whatever Is possible to prevent external threats, another one is (IMHO senselessly) making access from the LAN difficult for hypothetical physical intruders while complicating the life of the authorized user(s).

The probability that someone will enter your home and will start fiddling with your (crappy) network is very, very low.

On the other hand, I would hope that the people actually managing security in high risk professional environments already know what to do (and AFAIK most of the effort is about preventing physical access to the local devices and network).

             
            
           
          
          
            
            
              



At least something interesting with artificial intelligence…

             
            
           
          
            
            
              
Mikrotik is not only used in your home - these devices are being used by ISP’s and enterprise coorporations etc.

Having backdoored unfiltered access to your Mikrotik devices leads to situations like these:

https://blog.cloudflare.com/de-de/meris-botnet

https://thehackernews.com/2024/07/ovhcloud-hit-with-record-840-million.html

So in my case I disable ALL such access EXCEPT for console, SSH and HTTPS (www-ssl).

Also the OP asked about how to secure your Mikrotik device - not how to make it wide open…

             
            
           
          
            
            
              In addition to the great suggestions made I suggest that you consider

Who will attack and HACK your Internet connection?

             
            
           
          
            
            
              @Apachez

I thought that *remote* as in:

The Meris botnet is formed of infected routers and networking hardware manufactured by the Latvian company MikroTik. According to MikroTik’s blog, the attackers exploited a vulnerability in the router’s operating system (RouterOS) which enabled attackers to gain unauthenticated > **remote** > access to read and write arbitrary files (CVE-2018-14847).


actually meant *remote* and vulnerability in the OS meant vulnerability in the OS, specifically Meris - according to MIkrotik - did not affect any Mikrotik device using default firewall configuration.

Mikrotik is not only used in your home - these devices are being used by ISP’s and enterprise coorporations etc.


And - as already said - I hope these ISP’s, corporations, etc, have dedicated personnel that know well what to do.

             
            
           
          
            
            
              From the previous link:

```
According to MikroTik’s blog, the attackers exploited a vulnerability in the router’s operating system (RouterOS) which enabled attackers to gain unauthenticated remote access to read and write arbitrary files (CVE-2018-14847).
RouterOS is the router operating system that’s used by MikroTik’s routers and the RouterBOARD hardware product family, which can also be used to turn any PC into a router. Administration of RouterOS can be done either via direct SSH connection or by using a configuration utility called WinBox. The vulnerability itself was possible due to a directory traversal vulnerability in the WinBox interface with RouterOS.
```

https://mikrotik.com/supportsec/meris-botnet

https://blog.n0p.me/2018/05/2018-05-21-winbox-bug-dissection/

https://github.com/BasuCert/WinboxPoC

So there is that…

Again if you want to use the mac-backdoor for whatever reason, good for you.

Personally I prefer (and recommend) to disable ALL “features” and ONLY enable those I actually need (which unfortunately none of the NOS out there (yes Im looking at you Cisco, Arista, Juniper, VyOS, Mikrotik, HPE, Aruba etc) do these days where most junk is enabled by default so the box will happily inform its surroundings (and even the internet) which version it runs etc).

             
            
           
          
            
            
              That was a REMOTE exploit possible only on routers that had “wrong” firewall settings and thus allowed access from the outside (WAN).

By setting:

/tool mac-server mac-winbox set allowed-interface-list=none

you are disabling it completely, also from LAN.

It creates a big inconvenience in normal management.

It makes much more sense in any normal setup to have a dedicated interface for management and allow winbox on that interface.

             
            
           
          
            
            
              Again, disabling not needed features is NOT an “inconvenience” rather the opposite.

             
            
           
          
            
            
              Actually, security always comes with some level of inconvenience.

It’s up to the admin to decide what he/she values most: convenience or security.

Accept inconvenience then or get rid of humans … 

we all should be quite aware the human factor (and it’s accompanying need for convenience) is the biggest thread for anything related to security

Access your router remotely directly

or

first fire up your VPN, then access your router

Most will take the first option. Simply for convenience.
