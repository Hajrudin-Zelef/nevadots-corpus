Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
3
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I'm trying to limit internet users by traffic quota in fortigate firewall.Is there a way to limit users traffic usage and set quota for them in fortigate?
i think there is no built-in feature for this,but i guess by means of an external AAA server it may be possible.
i've used cisco ACS before ,but i don't know how to configure it for this scenario.
and i don't know if fortigate support CoA or Packet of Disconnect either.
What version of fortios are you using? and what model de you have?
FortiOs 5.0 and 5.2 (i think) have what are you looking for which includes an UTM options of client reputation (in other words, users rating) not just by traffic, by malware, network applications or IPS.
I've used FortiGate in the past and you can do that with UTM like M4niac said. One thing to be careful with, maybe you've already experienced this, is Forti units are slow to process changes. In my experience, you do something in the configuration and it takes a couple of minutes before actually applying. Especially when speaking of UTM features. It looks like sessions needs to be dropped then initiated again on the new feature you just applied.
If you are under licensing, you can post a question to their support. They are quite helpful. I've done it a couple times for config issues. :)
Yes ! there is a feature name "traffic shapping" in fortigate firewall . Traffic utilisation can be restricted as per our requirements we can restrict bandwidth utilisation as per users basic,
We can define bandwidth utilisation per user with help of traffic shapping
We can even configuration traffic shapping on specific incoming and outgoing policy too . So traffic on that policy for each session bandwidth is limited .