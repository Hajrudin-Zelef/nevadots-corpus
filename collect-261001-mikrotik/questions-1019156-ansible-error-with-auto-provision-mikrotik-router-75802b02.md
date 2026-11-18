I have a problem that I cannot understand how to solve ...
From my ansible central machine I have written a routine for Mikrotik, to execute a series of configuration commands and thus auto provision my RBs, but it returns the following error, and I just don't know how to pass the OS detection configuration to me asks, do I have to pass it to it as a variable in the Ansible configuration file? About Ansible's own script? I just couldn't find much info about it.
Error :
fatal: [10.0.21.10]: FAILED! => {"msg": "Unable to automatically determine host network os. Please manually configure ansible_network_os value for this host"}
If someone has already worked on Ansible + Mikrotik automation I would really appreciate the clarification!