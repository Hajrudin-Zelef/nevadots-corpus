Anyone had issues with accessing webserver from local using RouterOS with 6.27 Firmware? I had everything working fine until upgrade to 6.27.
In addition, Hairpin NAT does not work.
The key to the issue is that dstnat is processed earlier than srcnat. If a data has passed through dstnat, then, and only then `srcnat rules will work.
Here is the config that makes Hairpin NAT to work:
/ip firewall nat> print
Flags: X - disabled, I - invalid, D - dynamic
 0    ;;; Hairpin NAT
      chain=srcnat action=masquerade protocol=tcp src-address=192.168.0.0/24
      dst-address=192.168.0.0/24 out-interface=bridge-local log=no log-prefix=""
 1    ;;; NAT masquerade for outgoing connections
      chain=srcnat action=masquerade out-interface=ether1-gateway log=no log-prefix=""
 2    ;;; HTTP
      chain=dstnat action=netmap to-addresses=192.168.0.150 to-ports=80 protocol=tcp
  dst-address=76.35.222.205 dst-address-type=local dst-port=81 log=no log-prefix=""
76.35.222.205 is a fake address representing my external IP address. I tried to work without it, but experience a several seconds delay when using Hairpin NAT.
When requesting a local HTTP server from inside, the rule 2 works as first, then the rule 0.
It may also be that something is wrong in your Filter rules.