Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
3
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I have a MikroTik router set up at the entrance of our office network. I'm looking to configure it for specific domain-based port forwarding. The task seems to be rather common, but I am stuck. Here's what I need to achieve:
Requests to office1.example.com:443 should be redirected to Server1
on port 443.
Similarly, requests to office2.example.com:443 should be
redirected to Server2 on port 443. How can I set up these
domain-specific port forwarding rules on the MikroTik router?
Any guidance or steps to follow would be greatly appreciated.
Requests to office1.example.com:443 should be redirected to Server1 on port 443.
Similarly, requests to office2.example.com:443 should be redirected to Server2 on port 443. How can I set up these domain-specific port forwarding rules on the MikroTik router?
You can't. Port forwarding doesn't work that way. IP headers does not contain any information about domain names, only IP addresses.
TLS contains a Server Name Indication. That can't be done at L3 level, which is where your firewall operates. Furthermore, with encrypted SNI, the TLS connection has to be terminated to know the SNI hostname sent. You need a reverse proxy server that can terminate (and proxy) TLS connections to the destination based on the SNI.