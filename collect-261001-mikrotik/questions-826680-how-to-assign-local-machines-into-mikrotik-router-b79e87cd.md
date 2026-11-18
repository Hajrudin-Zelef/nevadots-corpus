I have a Dedicated Server and installed a VMWare-Esxi as main OS on it.
Then, I added 4 VMs on VMWare-Esxi :
VM1 - Windows Server 2008 - Local IP  192.168.100.10
VM2 - Windows Server 2008 - Local IP  192.168.100.11
VM3 - Windows Server 2008 - Local IP  192.168.100.12
VM4 - Mikrotik Router 6.6 - Public IP 149.252.96.29   // definitely it's not my real IP :)
What I want to do?
I want to implement port forwarding on Mikrotik Router to Local VMs like so :
149.252.96.29:1573 == forward to => 192.168.100.10:3389
149.252.96.29:1574 == forward to => 192.168.100.11:3389
149.252.96.29:1575 == forward to => 192.168.100.12:3389
The main question :
The main question is that I don't know how to add the VMs with local IP to the Mikrotik Router?
Any helps would be great appreciated.