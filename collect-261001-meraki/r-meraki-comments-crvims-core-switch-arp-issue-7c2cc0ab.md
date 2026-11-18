We have a pair of MS425-32 that handles Layer 3 routing and holds all the VLAN interfaces.

Seems that FW 11.22 has an issue where the ARP table doesn't sync between the two switches, and it causes some very hard to troubleshoot issues.

I started to suspect a FW bug when a device would "wake up" and become pingable if I ping it from a device on the same VLAN.

I would have devices fall offline or maintain a certain packet loss. I guess it depends on if that packet passes through the switch with the ARP entry or not.

Support says we have to wait for a software fix. I got so tired of the packet loss I just powered one of the core switches down until they fix it.

Anyone else experiencing this?