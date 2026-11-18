# OPNSense Schedule is not working no matter where I place it

*Score : 0 | Source : https://networkengineering.stackexchange.com/questions/87411/opnsense-schedule-is-not-working-no-matter-where-i-place-it*

I try to place a schedule in OPNsense for one of my VLAN interfaces to shut down between 10 PM and 5 AM. However, when I try to enable it on my firewall, it blocks the whole internet, even if it isn't that time of day. Here is a picture of my schedule and OPNSense firewall. I tried placing the schedule at the top of the firewall initially, and that didn't help either. Thanks in advance! .

---

### Reponse (acceptee) — score 1

Never mind, it was simply a misconfigured time zone.