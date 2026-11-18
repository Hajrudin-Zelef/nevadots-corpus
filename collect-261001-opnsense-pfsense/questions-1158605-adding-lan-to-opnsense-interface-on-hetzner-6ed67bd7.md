# Adding LAN to opnsense interface on hetzner

*Score : 0 | Source : https://serverfault.com/questions/1158605/adding-lan-to-opnsense-interface-on-hetzner*

I am a newbie in opnsense. I installed opnsense on hetzner. I want to add LAN to interface but I can't find LAN interface on the assignment menu. What I can add is opt1. How do I add LAN to interfaces?

---

### Reponse (acceptee) — score 0

Go to Interfaces > Assignment > Add wan1 > save and apply changes
Go to interfaces > opt1 > enable interface > IPv4 Configuration Type to DHCP > save > apply changes