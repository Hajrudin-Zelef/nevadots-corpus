# Trouble enabling OTP for Opnsense logins

*Score : 1 | Source : https://serverfault.com/questions/946612/trouble-enabling-otp-for-opnsense-logins*

I'm trying to secure my firewall machine by enabling OPT for Opnsense. At a glance, it seems straight forward, see https://wiki.opnsense.org/manual/how-tos/two_factor.html. However, it doesn't seem to be working as intended, the authentication fails when the OTP token is included although it works fine using the builtin Tester.
OTP Configuration for root user:
Auth Servers OTP Configuration OTP Configuration for root user
Is there an extra step that I'm missing here?

---

### Reponse — score 1

After a few hours looking at the source code and clever google'ing, I stumbled upon hard to find solution:
Source: https://forum.opnsense.org/index.php?topic=9690.msg44232#msg44232
Step 6 is a bit unclear. You need to change the appropriate service (IPsec, Captive Portal, Web Proxy, OpenVPN or web GUI) to use the TOTP server.
For the web GUI the setting is under System: Settings: Administration: Authentication Server.