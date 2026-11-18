Created on 07-14-2009 01:59 PM Edited on 04-05-2022 02:04 PM

**Description**

It may be required to configure a FortiGate DHCP server that gives out a separate 'option' as well as IP information.  For example, in an environment that must support PXE boot with Windows images. **Solution**

config system dhcp server

edit <dhcpservername>

set option1 <option_code> [<option_hex>]

end

ascii = ftpservers=192.168.21.30,country=1,language=1

hex = 667470736572766572733d3139322e3136382e32312e33302c636f756e7472793d312c6c616e67756167653d31

The Fortinet Security Fabric brings together the concepts of convergence and consolidation to provide comprehensive cybersecurity protection for all users, devices, and applications and across all network edges.