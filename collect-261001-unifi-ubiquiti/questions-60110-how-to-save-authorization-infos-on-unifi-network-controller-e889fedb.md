We have two "Ubiquiti UniFi UAP AC LITE Router". They are great!
But ex.: annually, I want to upgrade the firmware on them.
For this, I quickly install an Ubuntu LTS in a virtual machine, install the:
UniFi Network Controller x.x.xx for Debian/Ubuntu Linux and UniFi Cloud Key
software on the Ubuntu VM from https://www.ui.com/download/unifi/unifi-ap-ac-lite/default/unifi-network-controller-5642-debianubuntu-linux-and-unifi-cloud-key
Then, even though I have the "admin" password from before (not the wireless network pw) I cannot log in to the two routers. The UniFi Network Controller sees the two routers, but cannot auth to them.
Then I have to physically reset them to apply the admin pw on them, so the software can log in to them and upgrade the firmware. Have to set up the wireless ssid, wireless pw, etc..
After I upgraded the firmware on them, I destroy the Ubuntu VM. After a year, I will create a new clean VM and it starts over again..
The Question: which files do I need to save from the VM before destroying it, since it looks like more needed than just "admin" password for the devices..
Additional interesting thing is that I cannot log in with "admin" user via SSH to the devices with the password. Maybe it is only pubkey allowed and not pw?