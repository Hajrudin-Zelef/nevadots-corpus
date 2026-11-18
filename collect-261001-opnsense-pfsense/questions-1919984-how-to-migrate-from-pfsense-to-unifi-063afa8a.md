I'd be very grateful if I could get a sanity check of my plan?
My current network is:
- PFSense firewall doing DHCP + DNS
- Unifi Controller in a Docker
- 2 Unifi Switches and 2 APs
I have bought a Unifi Cloud Gateway Fibre. Would my steps be:
- Backup my current Controller & PFSense configs.
- Make a note of all the network settings on PFSense.
- Turn off the Controller and PFSense.
- Connect the new Cloud Gateway to my ISP router.
- Using a laptop connected to the Cloud Gateway, setup the Cloud Gateway, loading the backup config from my Controller.
- Once it's all setup, configure DHCP + DNS, assigning the static IPs I have on my network.
- Plug the Cloud Gateway into my network via the switch. Wait until it adopts all my devices.