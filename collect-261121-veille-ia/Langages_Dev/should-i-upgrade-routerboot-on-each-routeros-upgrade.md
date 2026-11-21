---
id: collect-261121-veille-ia/langages-dev/should-i-upgrade-routerboot-on-each-routeros-upgrade
title: "Should I upgrade RouterBOOT on each RouterOS upgrade?"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["upgrade", "routerboot", "each", "should", "routerboard", "routeros", "system", "auto-upgrade"]
source: https://forum.mikrotik.com/t/should-i-upgrade-routerboot-on-each-routeros-upgrade/169470/5
source_anchor: ""
source_lines: [1, 18]
section: "Langages_Dev"
---
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
