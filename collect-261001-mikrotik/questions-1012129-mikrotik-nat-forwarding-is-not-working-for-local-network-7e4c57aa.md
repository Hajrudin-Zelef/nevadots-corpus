I try to access my external IP address from the local network, but instead of reaching my webserver behind NAT - the webfig page shows up.
NAT forwarding is working when accessing from the internet.
EDIT: This is my current nat config
[admin@MikroTik] > ip firewall nat print
Flags: X - disabled, I - invalid, D - dynamic
 0    ;;; defconf: masquerade
      chain=srcnat action=masquerade out-interface-list=WAN ipsec-policy=out,none
 1    chain=dstnat action=dst-nat to-addresses=<server's ip> to-ports=80 protocol=tcp in-interface=ether1 dst-port=80