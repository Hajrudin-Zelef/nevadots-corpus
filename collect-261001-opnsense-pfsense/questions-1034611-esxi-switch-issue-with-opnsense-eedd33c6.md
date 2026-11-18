# ESXi switch issue with OPNSense

*Score : 1 | Source : https://serverfault.com/questions/1034611/esxi-switch-issue-with-opnsense*

I've got an issue with configuration of 6 port ESXi. I read ESXI single vSwitch with 2 physical NICS and that does not really resolve my issues. I just can't get this configuration below to work.
I've got a 6 NIC (WAN+LAN+4xOPT) ESXi The NICs are connected as following:
The ESXi config
Configuring WAN was straightforward but the LAN is giving me headache. I just want to have DeviceA..DeviceZ be able to communicate on the LAN-network of OPNSense, but all I see in ESXi are load-balancing options..
I configured OPNSense with a LAN and a WAN interface