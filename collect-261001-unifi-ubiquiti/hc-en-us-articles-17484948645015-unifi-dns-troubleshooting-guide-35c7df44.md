UniFi - DNS Troubleshooting Guide
The DNS server is responsible for translating a url (ex., google.com) into the IP address where the host is located. There's a number of UniFi services (updating, remote management, etc) that require you to sufficiently resolve ui.com domains. Incorrect configuration could impact these services.
Check Firewalls & ISP Restrictions
DNS works using UDP Port 53. Ensure that this is not being blocked by any upstream firewalls, gateways or ISP modems. If it is, DNS resolution will fail.
Selecting a Reliable DNS Server
By default, UniFi will use the DNS Server provided by your Internet service provider. This is usually okay, but if you are unsure, we recommend using a public DNS Server such as 1.1.1.1 or 8.8.8.8.
We caution against the use of custom, internal DNS servers because they may not be updated to resolve the required ui.com domains, or they may block them entirely.
Re-Configuring Your DNS Server
UniFi Cloud Gateways
Navigate to UniFi Network > Settings > Internet > DNS Server and enter the new DNS Server.
CloudKeys, Network Video Recorders & Other Non-Gateway Consoles
Navigate to UniFi OS > Console Settings and check if the IP Configuration is set to DHCP or Static.
- If it is Static, enter the new DNS Server and select Apply Changes.
- If it is DHCP, you will need to modify the DNS Server directly from your DHCP server.
 
  - If you have a UniFi gateway, this is found in UniFi Network > Settings > Networks > [Network Name] > DHCP Service Management > DNS Server.
  - Note: This change will take effect after your device’s DHCP lease expires, usually after 24 hours. To expedite this, turn your device off and back on again.