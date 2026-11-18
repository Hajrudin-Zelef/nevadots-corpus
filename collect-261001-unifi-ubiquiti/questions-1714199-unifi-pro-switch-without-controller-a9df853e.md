# Unifi pro switch without controller

*Score : 3 | Source : https://superuser.com/questions/1714199/unifi-pro-switch-without-controller*

We have a UniFi Switch Pro 48 PoE and we want to use it without a unifi controller but when we connect to the switch and change the configuration it is not saved, after a reboot the configuration is restored to default
Enter command for save configuration in cli
write memory:
copy flash:running-config nvram:startup-config
in shell mode:
save