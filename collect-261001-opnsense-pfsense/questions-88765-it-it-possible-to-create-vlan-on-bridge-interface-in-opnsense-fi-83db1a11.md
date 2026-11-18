Is it possible to create VLAN on bridge interface in OPNsense firewall? I have tried, but while creating VLAN, on interface call drop down it does not show bridge interface as an option.

On an OPNsense, you cannot use tagged VLANs on a bridged interface. However, you can create tagged VLAN interfaces on the participating physical ports and bridge them together. Bridging to an untagged (access) port is also possible.

Repeat the above process for each VLAN that you want to work across the OPNsense, practically creating a VLAN trunk for the physical ports.

With the OPNsense being a software device, a bridge may perform somewhat poorly (latency and bandwidth wise). I'd seriously recommend connecting the OPNsense to a bridged VLAN on a hardware switch.