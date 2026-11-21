---
id: collect-261121-veille-ia/gpt-openai/technical-tip-how-to-monitor-fortigate-using-manageengine-opmanager-community
title: "Technical Tip: How to monitor FortiGate using ManageEngine OpManager | Community"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["fortigate", "manageengine", "opmanager", "below", "select", "monitor", "using", "address"]
source: https://community.fortinet.com/t5/FortiGate/Technical-Tip-How-to-monitor-FortiGate-using-ManageEngine/ta-p/401609
source_anchor: ""
source_lines: [1, 5]
section: "GPT_OpenAI"
---
| Description | This article describes how to monitor FortiGate using ManageEngine OpManager, with an example. | 
| Scope | FortiGate. | 
| Solution | In this example, ManageEngine OpManager is installed on a Windows client which has the following IP assigned manually: IP address: 10.117.4.115 Subnet Mask: 255.255.240.0 Gateway: 10.117.4.147  A Windows client is connected with FortiGate on port3 and the configuration of port3 on the FortiGate is as below:  IP address: 10.117.4.147 Subnet Mask: 255.255.240.0    From GUI of the FortiGate, go to **System -> SNMP -> Create New:**       On the next page, select 'Apply' to apply the configurations:    Execute below commands from the CLI to enable SNMP system info configuration:  **config system snmp sysinfo** set status enable end  On ManageEngine OpManager, go to **Inventory -> Add Device** , as below:    After selecting 'Add Device', the following page will be opened. Add the IP address of the FortiGate, select 'Add Creden,tials' and then choose 'SNMP v1/v2c', as below:    Fill the fields as below and then select 'Save':    Select the Credentials as below and then select: 'Add Device':    After taking the action above, the FortiGate is added successfully to the ManageEngine OpManager, as shown below:     | 

Enter your E-mail address. We'll send you an e-mail with instructions to reset your password.
