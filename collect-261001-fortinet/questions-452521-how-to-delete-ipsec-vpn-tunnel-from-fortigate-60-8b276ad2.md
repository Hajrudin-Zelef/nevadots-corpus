Server Fault is part of Stack Overflow’s open communities: specialist spaces where curiosity is welcome, knowledge is shared freely, and the best answers rise to the top.
This question shows research effort; it is useful and clear
4
This question does not show any research effort; it is unclear or not useful
Save this question.
Show activity on this post.
I have had a IPSEC connection setup between two firewalls. Now I want to remove the tunnel in my firewall, a "Fortigate 60".
There are two phases, "Phase 1" and "Phase 2" for each IPSEC connection. I can delete the "Phase 2" entry by clicking the trashcan icon (in the web interface), but there is not such icon for "Phase 1". Is it possible to delete that?
When I look at the log it alerts about this tunnel not working (after deleting "Phase 2") and it would be nice not ta have loads of such events in the log.
Whenever you can't delete something in the FortiGate, there usually is a reference to that object somewhere. Normally the references are easy to track as they appear on the UI adjacent to the object.
Two notable exceptions:
* A bug did exist in the past (an old version) where the delete button on the phase object wouldn't work
* Chrome browser doesn't always refresh that condition (not sure if a Chrome or Fortinet issue). Refreshing the sessions; close/reopen; empty cache or whatever works for you.