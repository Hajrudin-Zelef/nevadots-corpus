By default, the hostname used by the RouterOS DHCP client is read from /system identity.
/system identity set name=MyRouter
The "identity" name is, indeed just like /etc/hostname, also used for various other things besides DHCP – for example, it's what shows up in device discovery lists (LLDP, RoMON).
If you want to change the hostname only for DHCP lease requests while keeping the router's "identity" (e.g. ISP requires a specific name that doesn't suit your internal scheme), you can do that through /ip dhcp-client option by changing DHCP option 12.
There you'll find a built-in item that sends the "identity" hostname. To customize it, it's best to create a completely new 'option' item, then assign it to the 'dhcp-client' item:
- 
> /ip/dhcp-client/option
> print
   #   NAME           CODE  VALUE               RAW-VALUE     
   2 * hostname         12  $(HOSTNAME)         456d6265724757
> add name=CustomHostname code=12 value=foo
- 
> /ip/dhcp-client
> print detail
   0  interface=ether1 dhcp-options=hostname,clientid
> set 0 dhcp-options=CustomHostname,clientid
No, there is no Linux shell access on RouterOS. It's an appliance OS that only happens to be based on a Linux kernel (and it does re-use the existing routing and Netfilter/iptables functionality), but there are none of the usual userspace tools present; every single daemon running on the system is fully custom.