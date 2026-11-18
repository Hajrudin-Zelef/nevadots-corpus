# Mikrotik & Bluetooth coexistence

*Source : https://superuser.com/questions/429994/mikrotik-bluetooth-coexistence | Site : superuser.com | Score : 1*

I'm having some issues with my bluetooth devices when wifi is running, especially when it's busy doing a backup.

I solved this on my old router with a mode called "bluetooth coexistence" in tomato on a Linksys WRT54GL.

Now I've switched over to a Mikrotik 751G-2HnD and it's absolutely fantastic except that I can't find a bluetooth coexistence mode. Does anyone know if such a mode is supported or can be enabled for Mikrotik hardware?

---

## Reponse — score 0

As I know in 802.11 itself no such thing "bluetooth coexistence". It can be implemented in bluetooth, or in devices with both Wi-Fi and Bluetooth (to interleave using interfaces).
http://wireless.kernel.org/en/users/Documentation/Bluetooth-coexistence