# Should I run 2 firewalls or manage everything from one?

*Score : 2 | Source : https://serverfault.com/questions/1048254/should-i-run-2-firewalls-or-manage-everything-from-one*

I currently have a UniFI Firewall in place and I plan to get a OPNsense firewall mainly for a VPN.
Setup: Modem - OPNsense Firewall - UniFI Firewall - VLANS (Rules made by UniFi)
Are there any advantages of running a setup with 2 firewalls or should I move everything to the new (more powerful) OPNsense firewall?
Thanks!

---

### Reponse (acceptee) — score 1

Save yourself the headaches and sparing the additional Single Point Failure and just combine onto the more powerful hardware.
The only time I would multi firewall setup if you needed to segregate for "sub" networks. Say to make X rules for workstations but Y rules for servers.