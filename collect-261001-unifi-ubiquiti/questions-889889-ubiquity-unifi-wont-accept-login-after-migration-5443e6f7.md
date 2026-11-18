Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
4
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I've migrated a existing Unifi Controller to another server. When the configuration Wizard appeard, I choose "Restore from Backup" after the Import the login screen appears, but the Login isn't working.
I tried these versions
5.5.24 -> 5.5.24 and
5.5.24 -> 5.6.26
The issue was I made the wrong backup. I made the Backup at Settings-> Site -> Export Site which seems to be insufficient. I read that on a Migration how-to.
Settings -> Maintenance -> Backup is the right backup to take.