Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
0
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
What a Meraki switch would do with DNS?
switching has nothing to do with Domain name resolution..
the only thing I can come up with, is that their switches probably using some type of domain address instead of IP to connect to their clouds???!!
Merakis are managed via a centralized system hosted in the cloud. DNS is the way by which the Meraki endpoints (...whether switches, firewalls, AP's or cameras) connect back to that cloud system. It's also key to said endpoints being able to reach alternate management points should failures or excessive load occur in Meraki's infrastructure.