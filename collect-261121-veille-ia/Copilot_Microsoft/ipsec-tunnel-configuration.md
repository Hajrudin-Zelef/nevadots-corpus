---
id: collect-261121-veille-ia/copilot-microsoft/ipsec-tunnel-configuration
title: "IPsec tunnel configuration"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["ipsec", "tunnel", "secret", "src-addr", "dst-addr", "configuration", "peer", "addr"]
source: https://forum.mikrotik.com/t/ipsec-tunnel-configuration/59320/2
source_anchor: ""
source_lines: [1, 19]
section: "Copilot_Microsoft"
---
Hello,

you may try this, if you have static ip-addresses on your pppoe-client interfaces

# on side1

```
/ip ipsec peer add addr=93.138.77.119 secret="your_very_strong_secret" nat-traversal=yes
/ip ipsec policy add src-addr=172.16.1.0/24 dst-addr=192.168.2.0/24 sa-src-addr=78.0.208.170 sa-dst-addr=93.138.77.119 tunnel=yes
/ip firewall nat add place-before=0 chain=srcnat action=accept src-addr=172.16.1.0/24 dst-addr=192.168.2.0/24 out-interface=INTERNET comment="NAT bypass for IPsec"
```

# on side2

```
/ip ipsec peer add addr=78.0.208.170 secret="your_very_strong_secret" nat-traversal=yes
/ip ipsec policy add src-addr=192.168.2.0/24 dst-addr=172.16.1.0/24 sa-src-addr=93.138.77.119 sa-dst-addr=78.0.208.170 tunnel=yes
/ip firewall nat add place-before=0 chain=srcnat action=accept src-addr=192.168.2.0/24 dst-addr=172.16.1.0/24 out-interface=INTERN comment="NAT bypass for IPsec"
```
