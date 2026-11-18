Setting up HAProxy and Let’s Encrypt on OPNsense
If you’re reading this, wondering why my blog came up before the official documentation - they’ve removed the original documentation on account of thier enterprise-only Reverse Proxy and Webserver.
You can find a copy of the original documentation at archive.org
HAProxy uses ACME Let’s Encrypt for SSL authentication
OPNSense’s HAProxy package can use ACME for certificates.
First, we must install those two packages.
Installing required software
- In your OPNsense, go to: System --> Firmware --> Updates and install all updates.
- Then, head to: System --> Firmware --> Plugins and install the following plugins:os-acme-client ,os-haproxy
Reverse Proxy OPNsense Preperation
There are several changes we have to make to the defaults of OPNsense before we can intake traffic to our router.
System preparation
In OPNsense go to: System --> Settings --> Administration
You will need to checkbox the Disable web GUI redirect rule and change the Web GUI TCP port to a number you can remember, example: 4443.
This change is to allow your router to reply to requests on the default ports for HAProxy’s traffic (80/443). By moving the port number OPNsense uses, you’re able to free up those port numbers for HAProxy.
 Again, be sure you have change the  Web GUI TCP port to ** a number you can remember. **
Then, reconnect to OPNsense’s web interface using the port you entered, example: https://192.168.1.1:4443
Setting a Virtual IP
I like to use a virtual IP instead of just pointing all traffic back to the localhost.
It helps me see traffic a little bit better. If you want to create a virtual IP open: Interfaces --> Virtual IPs --> Settings
Creating a Virtual IP
When thinking about this new IP address for your “SSL Offloading Server” you would want to chose an IP that belongs to another network than any you’re using.
The localhost subnet should, generally, be available to us to help avoid IP conflicts in your local network.
The chosen IP/Subnet will be the IP on which the HTTP_frontend and HTTPS_frontend will be listening on.
We can use anything from 127.0.0.0-127.255.255.255. For this, we will choose to use: 127.1.2.3/32
- Mode should beIP Alias
- Interface can remainLAN
- Network Address is the address we picked above127.1.2.3/32
- Save.
- Apply.
Port Alias for HAProxy’s Ports
To make things easier on us, we should create an alias for all the ports we need HAProxy to use.
Go to: Firewall --> Aliases
You will likely see some other aliases here.
Now, we are going to create an alias for the ports that HAProxy will be listening on. Click the add button to begin.
- Name anything you like, example:HAProxy_Ports .
- Type should bePort(s)
- Content needs to be80 and443 . You can type the number and hit enter to save the field.
- Now, both ports are set and we have a recognizable name for the alias, click save.
Let the traffic start following
Modify the firewall: Firewall --> Rules --> WAN
Make a firewall rule to allow any inbound traffic on the WAN interface connecting to the HAProxy_Ports alias.
- Add a new firewall rule.
- Set the action toPass
- Interface isWAN
- Direction isIN
- TCP/IP version4
- Protocol needsTCP
- Source can beAny
- Destination needs to beThis Firewall
- Destination port range should be set to ourHAProxy_Ports alias.
- Save.
Appeasing the OCSP Must Staple
Head to settings: System --> Settings --> Cron to create a new cron job.
Because our certificate has the OCSP Must Staple extension we need to update HAProxy’s OCSP data regularly.
If this isnt done, clients connecting to HAProxy will get a security warning and won’t be able to connect.
The fix is to create a cron job
I have had issues with OCSP Staples so I set my cron job to run everyday, every hour. This is more than likly not nessasary and may be a part of this tutorial that I update… but until then…
- Add a new cron job, set any number forMinutes and the other fields should be an* .
- For command chooseUpdate HAProxy OCSP data .
- Save.
It doesn’t matter if this job runs before or after a certificate renewal as the OCSP data gets updated anyway after installing new certificates in HAProxy. This is because the ACME plugin restarts HAProxy after installing the new certificates.
Let’s Encrypt (ACME Client)
Enable and Update Schedule
Moving on, go to: Services --> ACME Client --> Settings
- Uncheck Show introduction pages
- Check Enable Plugin
- We don’t need the HAProxy integration . Leave this one alone. It is for ‘HTTP-01’ and this tutorial is using the ‘DNS-01’ challenge.
Update Schedule
Click on the Update Schedule tab next to the Settings tab in the ACME Client services section.
This is located in: Services --> ACME Client --> Settings --> Update Schedule
- The Update Schedule tab will allows us to configure at which time of the day our certificates are renewed.
Why change time of day?
If everyone renews their certs at the same time, let’s say, on the :00 of the hour – there will be a heavy load of renewals on the CA.
You want your renewal request to happen at a time of the day where there is not much load on your services as well. The ACME plugin restarts HAProxy so it can use your new certificates which results in a very short downtime of HAProxy.
- Pick from the numbers below for the minutes of the Renew ACME certificate command, the hour is up to your use case.
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59
Restart services when there are changes
Once you complete the certificate renewal, you have to make sure the service restarts so it uses the new file.
To automate our service restart, visit: Services --> ACME Client --> Automations
Inorder to restart HAProxy after the cert updates, create a new automation.
- Name can be anything to identify the automation, example:RestartHAproxy
- Run command should select the commandRestart HAProxy (OPNsense plugin)
Account and CA
Next, go to: Services --> ACME Client --> Accounts
- On this page, enter a Name – for reference, I use the domain name (ssl.test.house.lan).
- E-Mail Address is the most important part. This is what is tied to the certificate for renewals. Too many renewals you’ll hit your limit and have to wait a week to try again.
Note: You can use the staging environment, Let's Encrypt Test CA, instead of the default ACME CA Let's Encrypt
ACME DNS-01 challenge
To request a certificate, we need to issue a challenge.
Head to: Services --> ACME Client --> Challenge Types
To get a wildcard certificate we need to use a DNS challenge. This tells Let’s Encrypt we own the entire domain and can therefore issue certificates to the subdomains beneath it.
- For Name I would put the assocaited domain and the challenge type (acmedns.test.house.lan).
- Challenge Type should be set toDNS-01
- DNS Service is up to your provider.
  - I use CloudFlare, as anyone can use them by changing Nameservers. Create a new API Token (with correct credentials).
- Save and double check you got the Account ID, API Token, and all credential’s permissions set correctly.
Issuing a new certificate
Issue a certificate: Services --> ACME Client --> Certificates
- Common Name is the URL of the certificate you’re requesting. We want a wildcard for a subdomain (*.test.house.lan).
- Select the ACME Account we created earlier (example: ssl.test.house.lan).
- Choose the Challenge Type that you named above (example - acmedns.test.house.lan).
- Key Length is a preference. I useec-384 . Generally, the higher number the better.
- Be sure you check the OSCP Must Staple checkbox. This is a modern requirement.
- Automations should have our restart policy we made earlier, select that now.
- Save
Depending on what Certificate Authority you chose earlier (Test CA or Not) will issue your certificate.
To issue your certificate, you need to hit the circular arrow button, it’s alongside all the other buttons under commands
Log file view of the action
Check what happened at: Services --> ACME Client --> Log Files --> ACME Log
There are two tabs, System and ACME Log.
- System will display what OPNsense decided to do.
- ACME Log is the script that runs. You can find most errors here.
- If everything went OK, you should have Installed a key to: /var/etc/acme-client/keys/99gbijk0z22.6749280/private.key
What if I used the Testing CA?
Go back to: Services --> ACME Client --> Accounts
Since you, presumably, successfully issued a staging certificate you can now change from the test environment to the default ACME CA Let's Encrypt and issue the production certificate.
Issue a production certificate
Back to: Services --> ACME Client --> Certificates
Now, again, you dont need to do this if you’ve already used the production server to generate a certificate.
Otherwise, forcefully issue your certificate with the circular arrow button
This time it you should receive a valid and trusted SSL certificate. Make sure to check the ACME log for any errors though!
HAProxy configuration
This is where the convoluted part begins. The instructions sound similar and become more difficult to understand. There are several sections, including using a map file, rules, conditions, backend server pools, services, dns overrides and more.
Please re-read if needed and be patient.
Please note, a lot of these pages are located as dropdowns in the menu headers on the settings page.
HAProxy inital configuration
HAProxy Service
First, we need to set the defaults we’re going to use across the HAProxy service in:
Services --> HAProxy --> Settings --> Service
On this page,
- Uncheck Show introduction pages
- Do checkbox Store OCSP responses
Enable HAProxy
Let’s turn it on: Services --> HAProxy --> Settings --> Service
On this page just checkbox Enable HAProxy and hit `Apply.
- Then, Apply.
HAProxy Global Paramaters
The next drop down we need is: Services --> HAProxy --> Settings --> Global Parameters
- In the upper left hand corner, check the advanced mode button.
- The number of HAProxy threads should not exceed the number of CPU threads of your OPNsense (example, 4).
- Maximum connections keeps from overloading the server, say800
- Increase the Maximum SSL DH Size to4096
- Apply.
HAProxy Default Paramaters
This area will helps us temper our reverse proxy to ensure there isnt an overloaded server.
Enter: Services --> HAProxy --> Settings --> Default Parameters
- Maximum Connections (Public Services) set to1200
- Maximum Connections (Servers) set to8000
- You can leave everything else the same.
- Apply.
HAProxy background services configuration
Setting conditions for non-HTTPS connections
Here we only need to create a NoSSL_condition, which is necessary in order to identify non-HTTPS traffic. This will be our only condition.
Next, go to: Services --> HAProxy --> Settings --> Rules & Checks --> Conditions
- Name should be set toNoSSL_condition .
- Condition type needs to be configured toTraffic is SSL (locally deciphered) .
- Negate condition has to be checked for this rule to make sense.
- Apply.
Map file for ease and convenience
Map files are found in: Services --> HAProxy --> Settings --> Advanced --> Map Files
Here we will create a new map file PUBLIC_SUBDOMAINS_map file for our public subdomains that we want to access from outside of our network.
This map file is telling HAProxy that any FQDN that starts with nextcloud then belongs to our NEXTCLOUD_backend (which finds our NEXTCLOUD_server”).
- Name isPUBLIC_SUBDOMAINS_mapfile
- Content for this example, should look like:1 2 # comment to tell you comments are allowed nextcloud NEXTCLOUD_backend
- Apply.
Rules to connect the moving parts
Finally, go to : Services --> HAProxy --> Settings --> Rules & Checks --> Rules
Here we add the rules that decide what to do with the traffic based on our map files (or conditions if necessary).
Forwarding HTTP to HTTPS
First, we must create a HTTPtoHTTPS_rule that will forward all HTTP traffic to HTTPS port, so it can go to our HTTPS_frontend.
- Name isHTTPtoHTTPS_rule .
- Select conditions this is set to the condition made earlier, theNoSSL_condition .
- Execute function are built into HAProxy, the one we want ishttp-request redirect .
- HTTP Redirect this is part of the paramateres for the function we chose earlier. Copy & Paste:1 scheme https code 301
- Save.
Mapping the map file to PUBLIC_SUBDOMAINS_rule
The PUBLIC_SUBDOMAINS_rule maps our subdomains to our backends using the map file we created in the previous step.
Continuing in Rules. Make a new rule for mapping domains to backends using a map file.
- Name isPUBLIC_SUBDOMAINS_rule
- Execute function must be set toMap domains to backend pools using a map file .
- Map file must also be set, the map file made above was namedPUBLIC_SUBDOMAINS_mapfile .
- Save.
- Apply.
Creating HAProxy Reverse Proxy
This is the meat and potatos of the tutorial. You will either get satiated or sick.
Adding the application service backend
First go to: Services --> HAProxy --> Settings --> Real Servers
This is where all of the servers on your network live. These are the “real servers” that exist on your network to which HAProxy can communicate with inorder to faciliate a reverse proxy. Note: unless you’re encrypting from the server to HAProxy with an internal CA, you dont need to verify SSL certificates.
The Virtual IP SSL_server
- Name addSSL_server for our virtual IP address we set earlier.
- IP will be the address of the virtual IP127.1.2.3
- Uncheck Verify SSL Certificate
- Save.
- Apply.
Adding application service servers you’re hosting
- Name add the name of your service for reference here, example:NEXTCLOUD_server
- IP enter the IP address of the machine you want to reverse proxy.
- Port enter the port number for the service(s) you want to proxy.
- Uncheck Verify SSL Certificate
- Save.
- Apply.
Loadbalance SSL_backend pool
Next go to: Services --> HAProxy --> Settings --> Virtual Services --> Backend Pools
These are servers that lie in the backend pool. The backend pool cares for health monitoring and load distribution. If you had multiple servers sending out the same content, they’d reside in the same pool group.
A Backend Pool must be configured, even if you only have one server.
Next, we will create the SSL_backend. This is the backend to which the SNI_frontend sends most of its traffic to.
Adding backend pools
SSL_backend
- In the upper left hand corner of the edit page, find advanced mode and turn it on (green).
- Name useSSL_backend for the first pool.
- Mode is set toTCP (Layer 4) for the SSL_backend.
- Proxy Protocol needs to be configured forVersion 2 .
- Servers should be set toSSL_server made earlier, the one you made with the virtual IP127.1.2.3 .
- Save.
Make sure that “SSL_backend” above is set to TCP mode, since the “SNI_frontend” is also running in TCP mode and you can’t mix HTTP mode and TCP mode with a frontend to backend.
Adding application service backends
Now we create the backend that belongs to an actual service.
- You will need one backend for each service.
- Make SURE the backend is named the same as the one in your mapfile!
- Name should reference the service you’re adding, example:NEXTCLOUD_backend
- Mode needs to beHTTP (Layer 7) [default] , this is different from the SSL_backend.
- Servers set to the correspondingReal Server you made earlier.
- Save.
- Apply.
Note: If you have multiple servers serving the exact same content than you will want to add all servers into a single backend so HAProxy can actually balance the load between the servers.
Life without a map file
Setting an application’s condition to a backend pool… without using a map file… if you so desire
This step is not required if you used the map file, this is an optional step!
If you didnt use a map file, or had a more particular configuration setup than “starts with”, feel free to set rules to point a condition to a backend pool that you made.
The Condition is generally made for the condition type. Again, as mentioned above, Host starts with.
The Rule states if the condition is met to execute a function. The function being, Use specified Backend Pool.
HAProxy Public Services
Next, go to: Services --> HAProxy --> Settings --> Virtual Services --> Public Services
Here we will create the frontends that are listening on our interface IPs and the virtual IP we created earlier.
You will have to place the rules for all of your services that you don’t want to get SSL offloaded in here.
At first we will create our SNI_frontend which will decide whether the traffic is going to be SSL offloaded or not.
Our default backend in this frontend will be the SSL_backend, that redirects all traffic to the virtual SSL_server which is actually the HTTPS_frontend.
Server Name Indicator frontend service
- Name is0_SNI_frontend
- Description should beListening on 0.0.0.0:443, 0.0.0.0:80 because you dont get to see the Listen Addresses for Public Services. It’s a convience thing.
- Listen Addresses need to be typed in, begin with0.0.0.0:443 and then enter0.0.0.0:80
- Type needs to beTCP
- Default Backend Pool is for theSSL_backend
- Save.
- Apply.
HTTP redirect to HTTPS frontend service
Now we will create our HTTP_frontend.
Make sure to place the HTTPtoHTTPS_rule in this frontend!
This frontend is necessary in order to redirect HTTP traffic to HTTPS. But you could also use it to serve non SSL encrypted services on port 80.
- Create a new Public Service and in the upper left hand corner of the create page, findadvanced mode and turn it on (green).
- Name is1_HTTP_frontend
- Listen Addresses is127.1.2.3:80 this combines our localhost address,127.1.2.3 plus the HTTP port:80 .
- Bind option pass-through must be:1 accept-proxy
- Checkbox Enable HTTP/2
- Also, checkbox X-Forwarded-For header
- Select Rules needs to haveHTTPtoHTTPS_rule set by clicking inside the square and selecting it.
- Save.
- Apply.
HTTPS frontend service
Now, the big show, create an “HTTPS_frontend”.
- This will be the primary frontend for traffic.
- It will be doing SSL offloading using the Let’s Encrypt certificate from the begining of this tutorial.
- You will place the “PUBLIC_SUBDOMAINS_rule” and any other rules for services that you want to get SSL offloaded in here.
- Create a new Public Service
- In the upper left hand corner of the edit page, find advanced mode and turn it on (green).
- Name is1_HTTPS_frontend
- Listen Addresses is127.1.2.3:443 this combines our localhost address,127.1.2.3 plus the HTTPS port:443 .
- Bind option pass-through must be:1 accept-proxy
- Checkbox Enable SSL offloading
- A new settings area, ‘SSL Offloading,’ will appear, and you will need to select your Let’s Encrypt certificate under the Certificates section.
- SSL option pass-through should be set to:1 curves secp384r1
- Enable Advanced settings needs to be checked.
  - This opens a NEW section for Cipers
Current Ciphers and Cipher Suites for a 100% A+ SSLLabs rating
Last updated/verified on 20230223 using Mozilla SSL Configuration Generator.
All ciphers with a strength of 128 bit or below have been removed in order to get a 100% A+ rating at SSL Labs.
1
2
3
4
5
Cipher List
ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES256-GCM-SHA384
Cipher Suites
TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
- Cipher List should read from the code block aboveECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES256-GCM-SHA384
- Cipher Suites should also read from the code block aboveTLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
- HSTS includeSubDomains needs checked.
- HSTS preload also needs checked.
- HSTS max-age can be increased to63072000
- Bind options need to haveprefer-client-ciphers removed, and addno-sslv3 ,no-tlsv10 ,no-tlsv11 ,no-tls-tickets
- Checkbox Enable HTTP/2
- Also, checkbox X-Forwarded-For header
- Select Rules needs to havePUBLIC_SUBDOMAINS_rule set by clicking inside the square and selecting it.
- Save.
- Apply.
Update Map file? Restart Service!
Anytime you want to add a new service and subdomain here are the steps:
- Create a Real Server that points to the IP address and PORT of the service that is running (example: I’m running Proxmox and want to verified SSL that service. I’d find the IP address of the machine, and the port number of the service you’re trying to connect to. In Proxmox’s case this is:  [IP - 192.168.1.12 with a PORT 8006] and, as Proxmox is encrypted by default, we must check theSSL checkbox to tell to HAProxy to expect an SSL connection. MAKE SURE to UNCHECKVerify SSL Certificate .)
  - That’s it! There are a lot of options, but all the Real Server needs is IP and PORT and if that port is SSL or not.
- That’s it! There are a lot of options, but all the 
- Creating a new Backend Server . Enter YOURSERVICENAMEHERE_backend for the name of this backend server. Then, select the name of the Real Server created above in theServers section.
  - This is where your pool would live if you had multiple servers doing the same thing.
- Modifing the Rules file. This is why it’s important to be consistant with names, we now must enter the subdomain to match with a backend. Naming our backends consistantly helps ensure when we need to add/edit the map file, it’s seemless.
  - The map file matches the entered subdomain with the backend service.
- Restart HAProxy.
  - That’s right nothing will work unless HAProxy loads the new map file.
Life without a map file: part 2
Setting a certificate for applications’ backend pool… without using a map file… if you so desire
If you didnt use a map file and are not going to point your traffic to a Virtual IP, then these instructions should get you up and running.
- Name set it to anyting you like, example:Public_Faceing_Pool .
- Listen Address should be your Public IP Address with port 443 on the end, example:123.45.67.89:443 .
- Type isHTTP/HTTPS (SSL offloading) [default] .
- Default Backend Pool can be set to any backend pool you made earlier – for testing of course, as ALL subdomains will connect to this one backend that then finds the server.
- Enable SSL offloading check this box. It allows us to specify a certificate.
- Certificates if you click in the box, you should see the certificate name you used above.
- Default certificate this is a drop down, choose.
- Select Rules use the rule that connects the condition to the backend pool.
- Save.
Test your new certificate
Access from external networks should now already be working.
Just try to access your URL “nextcloud.test.house.lan” from any device that is not connected to your local network, a good test device is your smartphone on cellular data.
You can now test your SSL settings at: https://www.ssllabs.com/ssltest/index.html
Access to your services, directly from internal networks
Choosing a DNS solution
If you try to access your URL “nextcloud.test.house.lan” from a device in your internal network, it should fail. Not just because that’s not a real TLD, but because your search domain is in your local network.
There are two ways of fixing this.
- With (Option A) being the better of the two. * Option A - Split DNS
- With (Option B) you lose the ability to track originating source IP in HAProxy when going through NAT.
- Option B - NAT Reflection
Option A - Split DNS (DNS Overrides)
Option A presumes you can create DNS entries on your local network’s DNS.
Unbound DNS will easily set up DNS overrides.
To set this up, go to: Services --> Unbound DNS --> Overrides
- Here you will need to create “Host Overrides” for each of your services.
- Yes, every subdomain that you want to seperate from the upstream public DNS.
The IP address can be any LAN (or VLAN) interface IP of your OPNsense.
I am using the LAN IP for OPNsense, 192.168.1.254 on which the 0_SNI_frontend is also listening on.
- Host set the name to*
- Domain should be the subdomain you registered the Let’s Encrypt certificate for, example: “test.house.lan”.
- Type is anA (IPv4 address)
- IP Address is the IP address of the reverse proxy server for local services, example: 192.168.1.254.
- Save.
Option B - NAT Reflection
This option uses firewall rules to check NAT first.
Please note that “NAT Reflection” is only applicable when port forwarding.
Open: Firewall --> NAT --> Port Forward
Create a new rule with the following:
- Interface is set toWAN
- Destination needs to beWAN address
- Destination port range should be set to the alias,HAProxy_Ports
- Redirect target IP should be one of the Virtual IPs we set earlier that the0_SNI_frontend is listening on.
- NAT reflection must changed to beEnable for this to work.
- Save.
Advanced Configuration: local-access-only subdomains
Imagine you have a service that you would like to access / protect using your brand new reverse proxy without making it available on the internet?
Well, HAProxy has got you covered!
Map file for local only subdomains
Going back to the map files section: Services --> HAProxy --> Settings --> Advanced --> Map Files
- Clone the current, PUBLIC_SUBDOMAINS_mapfile , rename it toLOCAL_SUBDOMAINS_mapfile .
- Add all of the subdomains you want to be local-access-only along with their corresponding backends.
Keep in mind that the contents of your “PUBLIC_SUBDOMAINS_mapfile” must also be in the “LOCAL_SUBDOMAINS_mapfile”.
HAProxy is processing the rules in the frontends based on the order they appear.
So if you place your PUBLIC_SUBDOMAINS_rule before your LOCAL_SUBDOMAINS_rule in the frontend configuration, you won’t get access to your local-access-only subdomains.
Vice versa this will also happen and you will no longer have access to your public subdomains.
To avoid this you have to also put the content of your PUBLIC_SUBDOMAINS_mapfile in the LOCAL_SUBDOMAINS_mapfile and place their rules in the correct order.
LOCAL_SUBDOMAINS_mapfile, example:
1
2
3
4
5
# local access only subdomains
localstuff MYSTUFF_backend
# public access subdomains
nextcloud NEXTCLOUD_backend
Create a local address condition
Conditions are in HAProxy: Services --> HAProxy --> Settings --> Rules & Checks --> Conditions
Now you need to create a new condition that detects if the source of the request is a local IP.
- Name is set set toLOCAL_SUBDOMAINS_SUBNETS_condition
- Condition type should beSource IP matches specified IP
- The Source IP should be as small and specific to your network as possible, or just use the entire RFC1918 IP range:1 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
You can use the predefined “Source IP is local” condition instead if needed.
Use DNS addresses for the condition
If you really need to, you can use DNS entries to match IPs that should be able to access local services.
But the resolving is only done once during the start / restart of HAProxy.
Adding the LOCAL_SUBDOMAINS_rule
Move over to rules: Services --> HAProxy --> Settings --> Rules & Checks --> Rules
- Clone the current, PUBLIC_SUBDOMAINS_rule , rename it toLOCAL_SUBDOMAINS_rule
- Name isLOCAL_SUBDOMAINS_rule
- Select Conditions and check/select theLOCAL_SUBDOMAINS_SUBNETS_condition you made earlier.
  - If you are using more than one set of SUBDOMAIN_conditions, you will need to change the Logical operator for conditions toOR .
- If you are using more than one set of SUBDOMAIN_conditions, you will need to change the 
- Execute function needs to be set toMap domains to backend pools using a map file
- Map file should be set to the name of our cloned and recreated mapfile,LOCAL_SUBDOMAINS_mapfile
- Save.
Ordering your HTTPS frontend rules
Final step: Services --> HAProxy --> Settings --> Virtual Services --> Public Services
- Edit your 1_HTTPS_frontend
- Scroll to the Select Rules section.
- Make sure you have your LOCAL_SUBDOMAINS_rule first.
  - This needs to be in the upper left corner, as in, first.
 Make sure your LOCAL_SUBDOMAINS_rule comes before your PUBLIC_SUBDOMAINS_rule entry in the rules for your HTTPS frontend public service.
Hide your certificate association with your WAN address
You might have noticed that if you can access your OPNsense using your public WAN IP (https://YOUR_PUBLIC_IP/) the connection will be secured, but upon further inspection you will see that your Let’s Encrypt certificate tied to your domain is beeing used.
While this is not a major security problem it still presents at least some privacy issues.
To fix this we can present a dummy certificate to everyone accessing your reverse proxy directly using your public WAN IP (https://YOUR_PUBLIC_IP/).
Create a placeholder certificate
Create a certificate authority
Create a new CA: System --> Trust --> Authorities
Here we have to create a placeholder certificate authority in order to create the placeholder certificate in the next step.
- Name will beInvalid_SNI
- Method is set toCreate an internal Certificate Authority
- Key Type should beElliptic Curve
- Curve needs to besecp521r1
- Digest Algorithm isSHA512
- Lifetime can be10950
- Fill in the rest of the fields with: Invalid_SNI
- Save.
Create a certificate
Now, the certificate:  System --> Trust --> Certificates
Create the placeholder certificate.
- Method isCreate an internal Certificate
- Descriptive Name isInvalid_SNI
- Certificate authority is alsoInvalid_SNI
- Type is aServer Certificate
- Key Type isElliptic Curve
- Curve issecp521r1
- Digest Algorithm isSHA512
- Lifetime can be10950
- Private key location should beSave on this firewall
- Fill in the rest of the fields with: Invalid_SNI .
- Save.
Connecting your frontend public service to a default placeholder certificate
Finally, get to: Services --> HAProxy --> Settings --> Virtual Services --> Public Services
The last thing left to do is to configure the placeholder certificate as “Default certificate” in your 1_HTTPS_frontend.
- Open your 1_HTTPS_frontend in yourPublic Services
- Scroll to the Default certificate section.
- Choose our newly minted placeholder certificate, Invalid_SNI
You should now no longer get presented with your trusted Let’s Encrypt certificate when accessing “https://YOUR_PUBLIC_IP”, but instead with the “Invalid_SNI” certificate. Thus masking your IP address connected to your domains.
Sources:
- TheMaw Tech for the basic pfsense style HAProxy Setup
- https://www.youtube.com/watch?v=uACQrhtsgFk
- TheHellSite forum post on OPNsense’s forums
- https://forum.opnsense.org/index.php?topic=23339.0
- OPNsense’s documentation
- https://docs.huihoo.com/m0n0wall/opnsense/manual/how-tos/haproxy.html