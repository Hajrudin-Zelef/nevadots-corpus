On a L3 switch there are two equal cost ISIS routes.
Destination/Mask Proto Pre Cost Flags NextHop Interface
10.150.4.12/32  ISIS-L2 15   10          D   10.200.15.42    Vlanif27
                ISIS-L2 15   10          D   10.200.15.38    Vlanif26
the switch is huawei s5700 I want to understand which route will be taken and why?
also if MPLS is enabled on it. I see there are two LSP's as shown in the image.
and MPLS RFC says: "Packets belonging to an FEC will always follow the same path".
so I want to understand which LSP will be chosen and why? is there any kind of load balancing happening?