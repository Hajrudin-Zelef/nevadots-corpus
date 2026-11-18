Network Engineering is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
1
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I connected my two NanoStation M5 devices as outlined in the manual. Now I want to login to admin and do the setup but I do not see a related ubiquiti SSID to connect to, to enable me to login to Admin via 192.168.1.20? Am I suppose to see an SSID?
If the NanoStations are set up to be a point to point bridge, they won't also act as wireless access points. You'll need to connect the ethernet port on one of the devices to your network.
Note that if the devices came as a pair, they may not require any additional configuration.