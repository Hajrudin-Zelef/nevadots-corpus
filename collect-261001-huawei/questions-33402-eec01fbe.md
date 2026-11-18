On a switch running Huawei VRP: What is the safest way to deconfigure a dynamic priority value on a root bridge and then configure a static priority value?
What's the problem?
I can do this:
[Switch]stp instance 0 priority 20480
[Switch]stp instance 0 root primary
But I can't do this:
[Switch]stp instance 0 root primary
[Switch]stp instance 0 priority 20480
So I have to do this:
[Switch]undo stp instance 0 root
[Switch]stp instance 0 priority 20480
In the meantime, the bridge has a priority value of 32768.
I can't assign a priority value because the command is not accepted: Error: Failed to modify priority because the switch is configured as a primary root or secondary root.
Currently, I have three options. None of them are satisfying. 1) Disconnect the bridge from the network and configure it from a terminal. 2) Decrease the priority value on every other bridge below the default value of 32768. 3) Deconfigure and reconfigure as fast as possible and hope for the best.
primaryorsecondarykeywords to set the priority, why do you then want to change the priority right away? In effect, you are setting the priority, then setting it again. The default value is32786, and setting the priority torootsets the value below the default value, and setting the priority value tosecondarysets it somewhere between therootand default values. You should not need to change the value after setting it toroot.switchportcommand withno switchportbefore you can enter theip address <address> <mask>command.