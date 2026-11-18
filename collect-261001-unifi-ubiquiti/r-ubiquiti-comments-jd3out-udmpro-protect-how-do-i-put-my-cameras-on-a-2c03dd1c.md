I'm new to Unifi.  I tried simply creating a new separate vlan for video and then assigning the ports the video cameras on connected to to the VLAN but this did not work - Unifi showed the cameras as "offline" until I put them back on the default network (presumably this is untagged).

I'm not a network admin expert but I have configured Juniper, Cisco and other equipment in the past and have a passing familiarity with L2 protocols.  Can someone point me to some documentation (written or video) that shows me how to isolate my video cameras on a separate vlan?
  

Obviously an attacker should not be able to unplug a cable from a camera, plug that cable into a laptop and use that connection as a way into the network ... Feel free to mention any other best practices (e.g. MAC addr white listing or other things)

As mentioned in the subject line, I have a UDM-Pro (with HDD) and some cameras. I also have a Ubiquiti PoE lite (8 port PoE, 8 regular port) switch and a few unmanaged netgear switches. My plan is for all of my Protect cameras to be connected through Ubiquiti switches only (i.e. no camera on a netgear unmanaged).