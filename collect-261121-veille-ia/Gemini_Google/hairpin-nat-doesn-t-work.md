---
id: collect-261121-veille-ia/gemini-google/hairpin-nat-doesn-t-work
title: "Hairpin NAT doesn't work"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hairpin", "rule", "address", "doesn", "server", "srcnat", "masquerading", "source"]
source: https://forum.mikrotik.com/t/hairpin-nat-doesnt-work/184105/14
source_anchor: ""
source_lines: [1, 18]
section: "Gemini_Google"
---
Thanks anav!

Do we need 2 rules in order to achieve hairpin nat:  A DSTNAT and a SRCNAT rule.

Or, just 1 rule:

`add chain=srcnat action=masquerade dst-address=192.168.88.0/24 src-address=192.168.88.0/24`

This from mkx is indeed very clear:

Standard SRC-NAT is masquerading source address and standard DST-NAT is masquerading destination address. And hairpin NAT is masquerading both addresses, one to each end (client doesn't use the correct dst-address and server doesn't see correct src-address).


I think this means that the "1 rule" above masquerades both the destination address (i.e., changes it from the public IP to the server's internal/private IP) as well as the source address (from the internal IP to the public IP) so that (1) routing of frames is done correctly and (2) both the client and server see the expected source and destination IP address on the frames.

If my understanding is correct, then am I misunderstand lurker888's post to state that 2 rules are necessary:  A port forwarding (dstnat to-port) rule and a hairpin (srcnat masquerate) rule?  Or was he addressing the specific needs of the original poster (and not a generic hairpin need)?

In addition, I cannot understand the paragraph that starts "Usual hair-pin NAT (in typical port forwarding) makes sure that the LAN client can talk to LAN server via some foreign...."
