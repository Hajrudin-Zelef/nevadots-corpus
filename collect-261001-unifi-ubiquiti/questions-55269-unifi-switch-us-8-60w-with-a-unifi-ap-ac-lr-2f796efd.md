I've bought myself a UniFi AP-AC-LR and a UniFi Switch US 8-60w. Are those two devices compatible to use power over ethernet cable to link them?
With kind regards
The access point UniFi AP-AC-LR uses 803.3/af mode A, and the switch US 8-60w supports it (according to their datasheets).
Ubiquiti publishes its compatibility matrix confirming this, here: https://help.ubnt.com/hc/en-us/articles/115000263008--UniFi-Understanding-PoE-and-How-UniFi-Devices-are-Powered
I believe the switch also supports so-clled "Passive POE" -- which is really just power-over-CAT5 -- but this isn't a good idea for long cable runs, easy management, or standards compliance.