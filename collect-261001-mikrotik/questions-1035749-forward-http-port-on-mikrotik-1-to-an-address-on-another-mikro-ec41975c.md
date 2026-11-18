Trying to apply the Hairpin-nat concept on mikrotik1, my config is something like:
/ip firewall nat
add chain=dstnat dst-address=1.1.1.1 protocol=tcp dst-port=80 \
  action=dst-nat to-address=192.168.2.2
add chain=srcnat out-interface=WAN action=masquerade
add chain=srcnat src-address=192.168.1.0/24 \
  dst-address=192.168.2.2 protocol=tcp dst-port=80 \
  out-interface=LAN action=masquerade
But it doesn't work for me because (i think) 192.168.2.2 is defined on another mikrotik (lets call it mikrotik2), not on this one (mikrotik1) wich the configuration is being applied. Mikrotik1 only routes the 192.168.1.0/24 network, and mikrotik2 only routes the 192.168.2.0/24. But, the two mikrotik devices are bridged and the addresses are reachable.
Someone can help-me? I don't have any single clue of what to do in this case. How do I forward my internet traffic to port 80 on mikrotik1, to the mikrotik2 lan address?
PS: Both mikrotiks are gateways, each from a provider.
Thanks.