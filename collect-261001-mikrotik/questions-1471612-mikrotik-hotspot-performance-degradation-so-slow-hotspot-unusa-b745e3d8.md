# Mikrotik Hotspot Performance Degradation: So Slow Hotspot Unusable

*Source : https://superuser.com/questions/1471612/mikrotik-hotspot-performance-degradation-so-slow-hotspot-unusable | Site : superuser.com | Score : 1*

FAULT:

Hotspot created with Mikrotik's "ip > Hotspot" wizard is so painfully slow that it is unusable

Googling "mikrotik hotspot slow" returned a gazillion results, none of which resolved this fault.

I checked queues, DNS, proxy and firewall rules created by the Mikrotik "wizard"- which appeared to provide the correct connectivity.

---

## Reponse (ACCEPTEE) — score 2

Mikrotik's automated Hotspot "wizard" created the fault: it inserts its rules ABOVE the one allowing related,established connections in the FORWARD chain.

RESOLUTION:

Moving the related,established rule to the top of the FORWARD chain successfully resolves this fault, restoring normal web browsing performance.

Hopefully long-suffering Mikrotik users will find this answer and save themselves reading numerous posts which lead them in the wrong direction...