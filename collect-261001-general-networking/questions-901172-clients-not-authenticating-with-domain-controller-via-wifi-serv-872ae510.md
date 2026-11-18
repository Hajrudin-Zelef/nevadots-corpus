I run a bit of a complicated setup, let me give you a quick rundown:
Local pfSense Firewall -> IPSec Tunnel to AWS -> Server 2016 DC.
pfSense runs DNS in the local network, forwards domain queries to the DC via Domain Override.
If I am connected via ethernet, everything works fine. Soon as I use WiFi (UniFi), I can't run gpupdate or print. When running gpupdate I get the error that the computer name can not be resolved.
When I reboot the machine, I get a Netlogon ID 5719, the computer can't establish a safe connection with the DC.
Why does this happen, and why does it work via ethernet and not WiFi?