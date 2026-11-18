I’ve avoided RouterBOOT firmware problems using this:

```
/system routerboard settings
set auto-upgrade=yes
```

Problem solved with **double reboots** after each ROS upgrade:

```
/system/routerboard> print
routerboard: yes
factory-firmware: 6.45.9
current-firmware: 7.11.2
upgrade-firmware: 7.11.2
```

MikroTik should make this single reboot process if auto-upgrade is enabled.