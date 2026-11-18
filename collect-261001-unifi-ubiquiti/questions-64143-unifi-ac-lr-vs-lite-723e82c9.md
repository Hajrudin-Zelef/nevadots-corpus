Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
5
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I want to install an access point for our office. and that is why I have brought an unifi ac lite. but it couldn't cover all of the building! our office has near 70 meters as foundation and is a two storey building.
now I want to know if I buy an unifi ac LR(Long range), can it solve my problem? I mean is unifi ac lr stronger than lite?
You cannot light such a building with a single WAP. You need to do a site survey and plan how to distribute WAPs across the building to provide coverage. Each WAP requires a wired uplink.
The main difference between AC Lite and AC LR is 2x2 MIMO vs 3x3 MIMO for 2.4 GHz. WAP power is generally limited to 250 mW, so there's no difference. The LR should provide higher bandwidth and a little more reach. See https://dl.ubnt.com/datasheets/unifi/UniFi_AC_APs_DS.pdf for details.