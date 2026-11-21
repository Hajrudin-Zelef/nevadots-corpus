---
id: collect-261121-veille-ia/copilot-microsoft/routing-failover-with-2-isp-gateway-mikrotik-script-routeros
title: "Routing FailOver with 2 ISP (gateway) - MikroTik Script RouterOS"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["gateway", "routing", "failover", "mikrotik", "script", "routeros", "check-gateway", "distance"]
source: https://buananetpbun.github.io/mikrotik/Routing-FailOver-with-2-ISP-gateway.html
source_anchor: ""
source_lines: [1, 11]
section: "Copilot_Microsoft"
---
# Routing FailOver with 2 ISP (gateway) - MikroTik Script RouterOS

Using more than one internet gateway (ISP) allows us to fail over where one link can be used as the main gateway and the other becomes the backup link. For these needs, the usual configuration is to define a check-gateway and differentiate the distance value for each routing rule.
WAN-1 = IP 192.168.2.2/24
WAN-2 = IP 192.168.3.2/24
```
/ip route
add check-gateway=ping distance=1 gateway=192.168.2.1 target-scope=10
add check-gateway=ping distance=2 gateway=192.168.3.1 target-scope=10
```
Credit: www.o-om.com
