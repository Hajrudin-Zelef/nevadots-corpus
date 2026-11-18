Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
2
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I have two networks Network 1 and Network 2 shown in the picture, and one client machine that must access resources on both networks. Due to security reasons the two networks must be isolated from each other. I have only basic understanding of networking and am complete newbie to Mikrotiks. How do I configure the router to support this configuration? I understand that this scenario is like having 2 ISPs, but I cannot figure out how to configure the routes so that each gateway is used only to access it's own resources.
so traffic between net1 and net2 is blocked but traffic between net1/net2 and operator is allowed. Of course you will have to add the correct routes on all the devices, for example for operator, if it's a windows system
You should be able to put in ACLs on the interfaces for Network 1 and Network 2 to prevent those two networks from talking to each other. The ACLs should permit everything else, so your Operator can access both networks with no problem.