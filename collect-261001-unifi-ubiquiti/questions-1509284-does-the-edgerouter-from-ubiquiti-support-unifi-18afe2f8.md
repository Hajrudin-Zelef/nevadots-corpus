I need to replace my wifi router and have looked at different Ubiquiti solutions.
I'm going to install this in my home, with two meshed wireless access points and several wired devices.
My initial setup was:
1x Ubiquiti Networks EdgeRouter X SFP (will be my router and plugged to the modem) 2x Ubiquiti Networks UniFi AP AC Lite Xx (several) unmanaged switches that is already place in my home
But after some googling around, I'm thinking I need to replace the EdgeRouter?
New proposition:
1x Ubiquiti Networks UniFi Security Gateway 2x Ubiquiti UniFi US-8 Switch 2x Ubiquiti Networks UniFi AP AC Lite The second setup uses 2x US-2 switches that has a single PoE port for the wireless access point units. But this setup does cost more.
Can I do the first setup? Or am I required to run "UniFi" on all components in the network. I guess I'll still be able to add my unmanaged switches, and I can use PoE injector for the wireless access points - but is there some software that makes everything easier? I see "UniFi" has these web based setup and management, the EdgeRouter does not have this, so adding and managing the wireless access points might become a problem?