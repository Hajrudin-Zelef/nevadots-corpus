Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
2
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I'm working at ISP and we provide internet over PPPoE. Our servers are running on FreeBSD and we are considering some improvement to enhance the administration of the server. We've thought of moving to MikroTik router OS. Each of our servers handle about 500 users at time.
Is it a good idea to replace our FreeBSD PPPoE servers with MikroTik or you can suggest something better?
The MikroTik RouteOS is very capable, but definitely designed for smaller installations. They're really aiming at hot-spots and small multi-tenant buildings. FreeBSD is going to be a significantly more capable, configurable, and able to provide greater capacity.
What exact pain points are you experiencing with your FreeBSD servers? You say you want to "enhance the administration of the server", but that really doesn't mean anything.
Depends. If you are using Routerboard than it worth the effort. No HDD, passive cooling, you can make it faul-tolerant with two boards. So it's more green, easy to configure and no need to mess with a BSD.