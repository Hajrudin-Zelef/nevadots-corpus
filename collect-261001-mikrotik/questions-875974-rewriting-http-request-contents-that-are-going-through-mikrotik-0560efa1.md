Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
0
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I need to pipeline through all the requests from my VPN clients connected to Mikrotik OpenVPN gateway, but when request being made is and HTTP request and is targeting particular destination, I want to rewrite some parts of the requests.
Does Mikrotik scripting allow such request manipulations?
You can match the requests based on the protocol and destination address and send them (dst-nat) to another server that runs some proxy that will modify whatever you want.