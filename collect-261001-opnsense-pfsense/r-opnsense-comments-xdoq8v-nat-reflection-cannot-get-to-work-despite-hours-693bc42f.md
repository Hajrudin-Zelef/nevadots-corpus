# NAT reflection - cannot get to work despite hours of troubleshooting!

Hi,

I've recently migrated from pfsense for my home router. While pfsense has always worked well, I found it a bit clunky to use, and hoped my experience with opnsense would be better. So far I'm still finding *idiosyncrasies* that make me really admire the patience network admins must have.

My set up is OPNSENSE -> Unifi switch -> LAN.

Vlans have been created but no traffic is directed there currently.

I run an unraid server with NGINX which I have port forwarded, copying the rules that worked as intended in pfsense. The server is accessable by domain name (i.e. plex.example.com) from outside the network, but times out when resolving from inside the LAN.

I want to be able to point apps on my phone to the domain name and have it resolve regardless of if it's inside the network or outside. The only thing that's changed in the network since I had a working config is the router software.

Above are screenshots of the NAT port forwards and advanced settings I believe are relevant. What am I missing here?