# NTP Traffic, but NTP not installed

*Score : 2 | Source : https://serverfault.com/questions/1096835/ntp-traffic-but-ntp-not-installed*

I have recently started with OPNSense and have limited outgoing traffic to HTTP/s, SSH ports. When analyzing my blocked traffic i found sporadic outgoing NTP-Requests from my local Linux machine.
I am not very familiar with NTP.
I am now wondering a few things.
The source port is always different. Is this normal behavior/ caused by the firewall block?
192.168.1.101:52936
192.168.1.101:54299
192.168.1.101:45992
...
I actually don't have NTP installed. So i don't quite understand why i even have NTP traffic?

---

### Reponse (acceptee) — score 1

Many Linux distros ship with an NTP client enabled by default. Check your system for chrony or systemd-timesyncd. The latter is the default on most systemd-enabled distributions.
chrony
systemd-timesyncd
systemd