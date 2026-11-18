Why do I need AdGuard Home as my internal DNS resolver?
AdGuard Home is a free and open-source software that can block ads and tracking on all devices connected to your home Wi-Fi network. Unlike traditional ad blockers that only work on specific devices or browsers, AdGuard Home covers all devices without the need to install any client-side software. Additionally, AdGuard Home offers a range of features beyond ad-blocking and tracking prevention, including traffic encryption, making it a comprehensive solution for controlling and securing your home network.
The majority of websites we browse nowadays come equipped with supplementary components for advertising, analytics, and engagement tracking. While these tools can be useful for website owners to generate revenue and gain insight into their audience’s preferences,
Wondering how AdGuard Home works? In essence, it serves as a DNS server that reroutes ad and tracking domains to a “black hole,” effectively preventing your devices from establishing connections to those servers. This is the same software that powers our reliable and tested public AdGuard DNS servers, and AdGuard Home shares many similarities in code. As a result, AdGuard Home can manage traffic for virtually any internet-connected device, including smart TVs, refrigerators, and even light bulbs.
In simple words, the Domain Name System (DNS) is a set of databases that convert hostnames into IP addresses. DNS is like a phone book for the internet, where easy-to-recall hostnames such as www.google.com are transformed into IP addresses like 216.58.217.46. It occurs seamlessly behind the scenes once you enter a URL into your web browser’s address bar.
To accomplish this, your computer contacts its designated DNS server and provides the website name, such as google.com. The DNS server searches for the IP address associated with that domain and supplies your computer with the result, such as 203.0.113.52. Following this, your computer can connect to the designated address and load the website content.
We can derive several benefits from using DNS-level blockers, including enhanced privacy and reduced advertising clutter when browsing the web. Additionally, by blocking unwanted content at an early stage, we can minimize bandwidth usage and data costs. This may also lead to slight performance improvements, as there is less content to load for each site. As an example it reduces the bandwidth use if you’re connected via mobile network.
Another significant advantage is security, as several DNS blocklists continuously update with the latest suspicious or malicious domains. Blocking and preventing clients from connecting to these domains quickly can enhance our security.
However, there are a few downsides to using DNS-level blockers. For instance, many of these blockers draw from various website blocklists, which may not always be entirely accurate. This means that some advertisements or tracking may still appear. Additionally, legitimate parts of some sites may also get blocked, and several websites nowadays require the loading of third-party components to work correctly. Although most things work correctly, users should be aware that some troubleshooting and manual unblocking of website components may be necessary at times.
STEP1: Decide on the AdGuard DNS implementation
There are plenty of ways to implement the AdGuard in your network. The Adblocker on all major platforms Windows, Linux, Docker and could be implemented into the network devices such as firewalls and routers. Please check the official GitHub repository.
If you already running OPNsense, instead of linking the firewall to the AdGuard running on small server you can embed it into the firewall system. This way you’ll get rid of additional failure point for your Internet, if you’d like to resolve all the traffic by internal DNS.
However, if you prefer to set up AdGuard (or Pi-hole, or others) on other separate machine, it is also fine. Following this approach you’ll need to update your client network’s DHCP options to use the desired, internal DNS servers.
Please keep in mind that the AdGuard DNS may not be suitable in Enterprise scenarios with a need of complex traffic filtering for thousands of machines. For that kind of purpose is better to stick to commercial ZenArmor plug-in for OPNSense or hardware Proxy like Zscaler or Symantec EdgeSWG.
NOTE: Adguard will only filter DNS requests with the domain name, it won’t prevent machine to connect directly to Public IP address!
STEP2: AdGuard + OPNsense topology
In the original scenario your computer uses external DNS provided by the DHCP and sends a request via your firewall to the public DNS and receives response on the default route to reach the specific website.
This is how the classic DNS request is handled:
Once you implement the AdGuard internal DNS on your firewall, it will act as a mediator between public DNS and will filter traffic from unwanted requests and make the requests sent to the public DNS significantly more secure.
Here is the request flow via internal DNS to public DNS resolver:
STEP3: Add the Community Repository to OPNsense
By default, AdGuard Home is not included in the available plugins to download/install in OPNsense. In order to extend the list of plugins, we need to add the community plugin repository that includes a list of additional packages.
Before we can install the AdGuard Home plugin, we will need to setup & install that community repository.
To do this, we’ll need direct SSH or console access to our OPNsense appliance from our LAN.
SSH is disabled by default, but we can enable it quickly by navigating to System > Settings > Administration and then scrolling down to the Secure Shell section.
We’ll need to check the box for Enable Secure Shell and Permit Password Login. If you’re logging into OPNsense with the root account, you’ll also need to select Permit root user login.
Then scroll down to the bottom of the page & click Save.
By default OPNsense will set SSH Listen Interface set to All. It is recommend to enable access only LAN interface.
If you don’t need SSH to be accessible all the time, please remember to disable this service once you’re finished setting this up.
Okay, now that’s enabled – we can connect to our OPNsense appliance using your preferred SSH client (like PuTTY).
If you’re using the root account, you’ll likely be dropped into the OPNsense shell – but you can select option 8 here to access the underlying FreeBSD shell.
In order to install the community repository, we’ll pull down the repository config file using the following command:
fetch -o /usr/local/etc/pkg/repos/mimugmail.conf https://www.routerperformance.net/mimugmail.conf
Then, we’ll need to ask OPNsense to update it’s local cache with the new repo – so it knows what packages are hosted there:
pkg update
If everything is successful, you’ll see output similar to below – which lists the mimugmailrepository now:
root@0xOPNsense:/home/matt # pkg update
Updating OPNsense repository catalogue...
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01
Fetching packagesite.pkg: 100%  229 KiB 234.3kB/s    00:01
Processing entries: 100%
OPNsense repository update completed. 822 packages processed.
Updating mimugmail repository catalogue...
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01
Fetching packagesite.pkg: 100%   54 KiB  54.8kB/s    00:01
Processing entries: 100%
mimugmail repository update completed. 177 packages processed.
All repositories are up to date.
After these steps, you no longer need opened SSH connection, so please close the port 22 by going to System > Settings > Administration and then scrolling down to the Secure Shell section.
Step4: Installing the AdGuard Home Package
Now that the additional package repository is set up, we can download & install the AdGuard Home plugin via the OPNsense web interface.
So back in our browser, we can nagivate to: System > Firmware > Plugins. On this page we can search for adguard or scroll through the list to find it.
Then we just click the plus icon on the right side to install (not shown in the screenshotbelow).
This should install pretty quickly:
***GOT REQUEST TO INSTALL***
Currently running OPNsense 22.7.9 (amd64/OpenSSL) at Sun Dec  4 12:48:38 EST 2022
Updating OPNsense repository catalogue...
OPNsense repository is up to date.
Updating mimugmail repository catalogue...
mimugmail repository is up to date.
All repositories are up to date.
The following 1 package(s) will be affected (of 0 checked):
New packages to be INSTALLED:
	os-adguardhome-maxit: 1.8 [mimugmail]
Number of packages to be installed: 1
The process will require 35 MiB more space.
7 MiB to be downloaded.
[1/1] Fetching os-adguardhome-maxit-1.8.pkg: .......... done
Checking integrity... done (0 conflicting)
[1/1] Installing os-adguardhome-maxit-1.8...
[1/1] Extracting os-adguardhome-maxit-1.8: .......... done
Stopping configd...done
Starting configd.
Migrated OPNsense\Adguardhome\General from 0.0.0 to 0.0.1
Reloading plugin configuration
Configuring system logging...done.
Reloading template OPNsense/Adguardhome: OK
Checking integrity... done (0 conflicting)
Nothing to do.
***DONE***
Now all we have to do is enable the plugin. Please refresh the browser page with F5.
Navigate down to Services > Adguardhome > General. Our only option here will be an Enable checkbox, so we’ll select that & Save.
From Adguard ver 1.9 onwards, on OPNSense 23.1.6<…, Adguard may need to have one more field ticked for ensuring that is the main DNS
The rest of the setup & initial configuration will be done directly from the AdGuard-specific web interface.
Step5: Reconfigure Unbound
In our setup, we will actually use two internal DNS server services. The Adguard DNS requests will be forwarded to Unbound which would act as a validating, recursive, and caching DNS resolver and will encrypt our traffic with DNSSEC.
Since DNS as default is listening on port 53 we also want AdGuard Home to listen on this port to make or life easier. Out of the box OPNsense is already running Unbound on this port. We need to change this so they don’t conflict with each other.
- Navigate to Services > Unbound DNS > General
- Change Listen Port to 5353
- Enable DNSSEC Support
- Enable Register DHCP leases
- Enable Register DHCP static mappings (this will resolve hostnames for us in AdGuard Home)
- Register IPv6 link-local addresses
- Save the settings.
Now we are ready to configure AdGuard Home itself. I will not go in to all configuration here but some things are needed to make this work optimal with OPNsense.
Step6: Initial Setup for AdGuard
By default, the AdGuard Home web interface will run on port 3000 & is not HTTPS-enabled. So if your OPNsense firewall is at https://192.168.1.1, you’ll need to connect to http://192.168.1.1:3000.
As long as that works – we’ll see the initial setup prompt below:
We’ll click on Get Started.
Now we’ll be asked to configure the Admin Web interface (the interface we’re connected to now) and the DNS server interface (which clients will use to resolve domain names).
By default, AdGuard home will try to set both of these to listen on All interfaces – and set the web on port 80 & DNS on port 53.
I would recommend setting the Listen Interface on both of these to only your LAN-side networks. There is no reason to enable them on your WAN, and it can be a security risk to do so.
You may also get warnings that port 80 & 53 may already be in use. For the web interface, we could change 80 to 3000 & just keep what we’re using now.
However, if we change the default DNS port, that will cause some additional problems since client machines will query port 53.
So here’s what my set up looks like so far, with 192.168.60.1 being my LAN side interface:
- AdGuard administration board will use port 3000 and could be only reached from the LAN network
- AdGuard will be the default DNS for LAN and VLANS
On the next page, we’ll be prompted to set up an administrative user & password for logging into AdGuard.
Remember to put a strong password initially.
Next we’ll be given instructions on how to set up client devices. In my lab network, the OPNsense firewall is providing DNS server configuration via DHCP – so we’ll get to that configuration shortly.
For now, we’ll just click Next.
On the last screen, we’ll just get a message saying that setup is complete & a link to open the dashboard:
And now we can log in:
Browse to http://192.168.1.1:3000 or in my example http://192.168.60.1:3000
NOTE: If the AdGuard didn’t come up, please disable and then enable the service from OPNsense Services > Adguard Home. As last resort please reinstall the Adguard (first disable plugin, change the port 5353 in unbound to 53)
Step6: AdGuard additional configuration
After logging in, the first thing we’ll see is a pretty empty dashboard. We don’t have any clients configured to use this yet, so there isn’t anything to report on.
If there’s an update available, please download it first.
Configure AdGuard to use Unbound
When the wizard is complete we can login to AdGuard Home with the credentials you entered.
- In AdGuard Home navigate to Settings -> DNS settings and scroll down toUpstream DNS servers -> Private reverse DNS servers .
- Here we enter the Unbound server we changed earlier in OPNsense settings, 192.168.1.1:5353 , or with other port pointing to you OPNsense instance if you have another one.
If you use a domain name to resolve local hosts by name instead of IP you might need to tweak that in AdGuard Home as well. Let’s say you’ve entered a domain under System: Settings: General that is home.mydomain.xyz, and you want that to take precedence over the public DNS, if that also exists, when you are at home.
- In AdGuard Home navigate to Settings -> DNS settings and go to top section underUpstream DNS servers .
- Add [/home.mydomain.xyz/]192.168.1.1:5353 at the top of that list.
- Now you will resolve local machines when connected to your LAN, and if connecting over the internet the public DNS record will be used instead.
In my example my firewall is on lan address 192.168.60.1 with port 5353 for Unbound
- In AdGuard Home navigate to Settings -> DNS settings and go to top section underPrivate reverse DNS servers
- Here we enter the Unbound server we changed earlier in OPNsense settings, 192.168.1.1:5353 , or with other port pointing to you OPNsense instance if you have another one.
- Leave the Bootstrap DNS servers as default
- In Private reverse DNS servers type your Unbound server once more 192.168.1.1:5353
- Use private reverse DNS resolvers should be enabled
Now Click on Save and then Test upstreams. If the test is successful you will got a prompt.
Step7: Configure the Unbound upstream DNS
Let’s set the upstream Unbound DNS server to use encryption when sending a request to public DNS server.
In OPNsense please go to Services > Unbound DNS > DNS over TLS
We are using the Cloudflare and Quad9 Public DNS as an example. Click on Add and fill the gaps:
- Select enabled
- Domain: (leave this field empty)
- Server IP: 1.1.1.1
- Server port: 853
- Veify CN: cloudflare-dns.com
and Save.
Do the same for 3 more public
- Select enabled
- Domain: (leave this field empty)
- Server IP: 1.1.1.3
- Server port: 853
- Veify CN: cloudflare-dns.com
- Select enabled
- Domain: (leave this field empty)
- Server IP: 149.112.112.112
- Server port: 853
- Veify CN: dns.quad9.net
- Select enabled
- Domain: (leave this field empty)
- Server IP: 9.9.9.9
- Server port: 853
- Veify CN: dns.quad9.net
When all of the DNS servers are selected click on Save:
Please note that if you’ve experience an issue with Unbound TLS forwardings, crashing Unbound with high CPU, please check Firewall > Diagnostics > Live View > port is 853 if none of the requests are being blocked
In my case there was an issue happening from time to time with Cloudflare cert exchange, so there was need to create rules for that on WAN port
Action: PASS
Direction: IN
Protocol: TCP/UDP
Source: 1.1.1.1/32
Port: 853
Destination: WAN address
Step8: Make the AdGuard OPNsense a systemwide DNS resolver
Under OPNsense you can navigate to System: Settings: General and add backup DNS servers under Networking -> DNS servers, but if you leave them empty only AdGuard Home will be used.
I usually leave them empty.
Also make sure that:
- Disable Allow DNS server list to be overridden by DHCP/PPP on WAN
Step10: Configure OPNsense DHCP to use AdGuard
By default, if a specific DNS server is not configured for your client DHCP settings, then OPNsense will provide the clients with the same DNS server it uses. This could have been a DNS server that was configured when you set up OPNsense, or it also can use DNS servers that are provided by your internet service provider.
So to update our LAN DHCP configuration, we’ll head back to our OPNsense web interface. From there, we’ll navigate to Services > DHCPv4 > [LAN].
Make sure that the DNS server field is empty.
After these changes all your DNS requests from PC should go through our configured internal DNSes.
Step11: Client Testing
Now we should be all set up! However, it’s important to note that because of the way DHCP works, clients may not pick up the new configuration immediately. When DHCP assigns an IP address, it also tells the client how long it can use that address for. So if a client stays powered-on & connected, it won’t ask for new configuration until that timer expires.
We can speed that up by resetting the network interface on our clients. This can be done in a number of ways including rebooting the client or simply disconnecting from wifi/ethernet & reconnecting.
In Windows please type in Search > View Network Connections. It will show you a list of active adapters for Internet. Right Click on the connected adapter and click on Properties. Choose Internet Protocol Version 4 > Properties and make sure that the option Obtain DNS server address automatically is enabled.
If you’ve changed anything, please reset your Internet connection on PC.
I’m using a Windows computer as my test system, so first I’ll check via the nslookup command from command prompt – which will query our configured DNS server & return the resolved IP addresses.
As we can see, we did get the correct IP address.
Now we can test one of the sites that are blocked by default via Adguard and see the result.
- adblock-one-protection.com
- uvsvlisbartwq.com
- wooden-comfort.com
Now that’s the result we want! By returning the 0.0.0.0 result, our client can no longer resolve that domain. So if this was an advertisement or tracking domain, it’s now blocked from loading.
You can check the query from the Adguard Administration Panel > Query
You can search the request by URL, check the filter result (Blocked) and the IP/hostname of device which was trying to reach the domain.
The filtering service is running excellent!
Step13: DNS leak test
When you’re doing plain text requests to your Public DNS, your hostname/IP and URL can be easily intercepted by all intermediary devices, ISPs, marketing companies that are on the way to reach the Public DNS. They gather such information for your Public DNS to sell you ads, profile you and spy on your traffic.
Therefore we have set up DNS over TLS encryption in the Unbound in previous steps.
We can test if our queries are going in encrypted format by visiting sites:
If none of the results has your Public IP or hostname that means that the encryption works fine.
Step12: Configure blocklists in AdGuard Home
Blocking Domains
First thing we’ll look at is our DNS blocklists. We’ll navigate to Filters > DNS blocklists.
Here is where we can ask AdGuard to query lists of what domains to block. By default, AdGuard does include two – but we can add more if we want:
Photo
If we want to add to the configured blocklists, we can do so by clicking the Add Blocklist button. This will prompt us whether we want to choose from a pre-populated list, or supply our own custom list:
Photo
The easy option will be selecting from the provided lists:
Photo
There are a ton of different curated block lists available depending on what you’re trying to block. If we wanted to use a custom list, a lot can be found on GitHub just by searching for PiHole or Adguard blocklists.
How to pick a blocklist will be up to you. There are blocklists that focus on advertisements, tracking & analytics, parental controls, etc. So it just depends on what areas you want to focus on.
Allowing Domains & Custom Filtering
If we have a list of known services that we want to ensure are never blocked, we can pull those lists via Filters > DNS allowlists. However, it’s more likely you’ll find a handful of domains you want to unblock, rather than a whole list.
For that – we can go to Filters > Custom filtering rules. At the bottom of this page there is a tool to check filtering, where we can enter a domain name & instantly see what the result is.
For example, with the default ruleset I’ll check to see if 0x2142.com is filtered:
Photo
So by default that domain isn’t found anywhere, so it will be permitted. The tool also gives us a convenient button to quickly block a domain.
We can click that button, or add the syntax ||0x2142.com^ to the custom filtering rules at the top of the page (and saving via the Apply button). Now if we check the results again – the filter check will show the domain is blocked:
Photo
And of course, we don’t want to block 0x2142.com!! So let’s add this to our allowlist instead, so that it can never be blocked . We can do that by adding @@||0x2142.com^$important to the custom filtering.
And now we’ll see a green box that shows that the domain is permitted via an allowlist:
Blocking Known Services
The other option worth mentioning is the ability to block certain known services, like WhatsApp, Twitter, Reddit, etc. This can be great if there are certain services you want to block, or for use as parental controls.
This can be found on the Filters > Blocked Services page.
This way we can select a service to block, rather than having to know all of the individual domains that service uses. For example, I’ll go ahead and select YouTube to block – and we’ll check that later on after we configure our clients.
Troubleshooting Blocked Domains
Okay, so now we know our blocking works…. But now someone in our home is trying to access YouTube & it’s not working. How can we tell if that’s our AdGuard service?
Our first stop might be the AdGuard query log. Opening this log, we can filter by domain name or client – or show only blocked queries if we like.
Pretty quickly we can see the issue – we blocked YouTube’s services:
Now we know how to fix the issue, which would be to unblock that service. However, if it was just a specific domain that was blocked, we would likely want to add it to our custom filtering as we showed earlier.
Reporting
Last but not least, we can also check our AdGuard Home dashboard again, which should be much more interesting than before:
Here we can quickly see how many queries have been made & how many were blocked for various reasons. We’ll also see what clients are using our DNS server, and which are making the most queries.
Most interesting (at least to me), is being able to see the top domains that were queried or blocked. Here’s where you might find some interesting information. For example, on my test machine – it’s a fresh installation of Ubuntu & we used FireFox to test. But we can see that even during the brief time it’s been set up, almost all of the highest queried domains belong to Mozilla’s analytics services. So it may be tempting to add those to our custom blocklists.
Additional Info
What if I have AdGuard running on a different server? Or want to keep using Unbound DNS?
How do I change the interface / port for the Web UI or DNS?
So perhaps we mis-typed something when configuring AdGuard. Or just wanted to change the interface IP address AdGuard listens on. No problem!
Unfortunately, since this is a community plugin – there is no configuration for the plugin within the OPNsense interface.
We’ll need to reconnect to the OPNsense command line to make some additional configuration changes. This can be done via SSH or the device console.
Once there, we can use the command edit /usr/local/AdGuardHome/AdGuardHome.yaml.
At the top, bind_host & bind_port pertains to the admin web interface. A little below there, under the dns section – you’ll see another bind_hosts and port config. Those ones are specific to the DNS server side of things.
Once done, save the config file by pressing Esc then selecting to quit the editor & save the file.
Lastly – Go back into the OPNsense web UI & restart the AdGuard Home service for the changes to take effect.
Src:
https://samuelsson.dev/install-adguard-home-on-an-opnsense-router/
https://0x2142.com/how-to-set-up-adguard-on-opnsense/
Maciej Zytowiecki
Network security expert with a deep passion for wireless networks, networking and data security. When I'm not working, you'll find me diving into hobby projects, contributing to open-source initiatives, or enjoying hands-on experiments with cutting-edge tech. My goal is to bridge the gap between complex concepts and accessible knowledge, making the world of network security both intriguing and approachable for all.