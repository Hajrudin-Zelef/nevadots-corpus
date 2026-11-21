---
id: collect-261121-veille-ia/copilot-microsoft/github-gdnuwanlakmal-fortiget-sd-wan-configurations-cli-how-to-configuration-about-fortigate-sd-wan-
title: "GitHub - gdnuwanlakmal/Fortiget-SD-WAN-configurations-CLI: how to configuration about FortiGate sd-wan and add ISP interface"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["interface", "edit", "next", "sd-wan", "fortigate", "config", "github", "gdnuwanlakmal"]
source: https://github.com/gdnuwanlakmal/fortiget-sd-wan-configurations-cli
source_anchor: ""
source_lines: [1, 36]
section: "Copilot_Microsoft"
---
This guide provides CLI commands to configure SD-WAN on a FortiGate device, including adding ISP interfaces to WAN links.

```
config system interface 
edit "wan1"
    set alias to_ISP1
    set mode dhcp
    set distance 10
next
edit "wan2"
    set alias to_ISP2
    set ip 10.100.20.1 255.255.255.0
next
end
```
```
config system sdwan
    set status enable   
    config members
    
    edit 1
        set interface "wan1"
    next
    edit 2
        set interface "wan2"
        set gateway 10.100.20.2
    next
end
```
```
config router static
    edit 1
        set sdwan-zone "virtual-wan-link"
    next
end
```
