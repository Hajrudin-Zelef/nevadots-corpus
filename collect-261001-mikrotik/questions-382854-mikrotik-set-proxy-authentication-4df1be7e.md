# MIKROTIK - set proxy authentication

*Source : https://serverfault.com/questions/382854/mikrotik-set-proxy-authentication | Site : serverfault.com | Score : 3*

Is it possible to set mikrotik authetication to be used for set the connection of other applications to internet using mikrotik proxy?

For example connect miranda IM using mikrotik proxy server:

I've tried to use hotspot login, bypass hotspot for selected MAC/IP (IP Bindings rule and and login without credentials), but it doesn't work..

---

## Reponse (ACCEPTEE) — score 3

No.

Here is the wiki link, nowhere is authentication mentioned.

MikroTik used to have a Socks proxy, but even that didn't support authentication.

EDIT:

I e-mailed MikroTik support (support at mikrotik.com) to double check my info and got a reply from @Sergejs. The answer is definitely no. The Socks proxy still exists, but this also does not support authentication.