# OPNSense logs every second: postfix/smtpd OTP unavailable because can&#39;t read/write key database /etc/opiekeys: Permission denied

*Score : 0 | Source : https://serverfault.com/questions/1061757/opnsense-logs-every-second-postfix-smtpd-otp-unavailable-because-cant-read-wri*

I am using Postfix on OPNSense as a Smart Host for my local servers to relay mail for notifications. When I first set it up, it would spam System: Log Files: General with the following message:
postfix/smtpd[67716]    OTP unavailable because can't read/write key database /etc/opiekeys: Permission denied
I was unable to find the setting causing this issue for the longest time. Since the postfix relay otherwise worked, it seemed to be mostly an annoyance. It did render that particular log almost completely useless, however.

---

### Reponse — score 4

The answer was pretty obscure, but by looking up on Google I found an old freebsd mailing list from 2008 that mentioned
Reinstall cyrus-sasl2 without OTP support worked, and now no more OTP related messages.
Under Services: Postfix: General almost all of the way to the bottom, there is an option that was turned on called Permit SASL Authenticated. Disabling this option fixed that logging issue.
It seems to be a feature that I wasn't using, and it almost certainly wasn't working anyways, so it's most likely completely safe to turn off.