---
id: collect-261121-veille-ia/fortinet/technical-tip-using-dhcp-server-options-on-a-fortigate-community
title: "Technical Tip: Using DHCP server 'options' on a FortiGate | Community"
domain: fortinet
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["fortigate", "dhcp", "server", "option", "technical", "tip", "using", "options"]
source: https://community.fortinet.com/t5/FortiGate/Technical-Tip-Using-DHCP-server-options-on-a-FortiGate/ta-p/196388
source_anchor: ""
source_lines: [1, 21]
section: "GPT_OpenAI"
---
**Description**

It may be required to configure a FortiGate DHCP server that gives out a separate 'option' as well as IP information.  For example, in an environment that must support PXE boot with Windows images. **Solution**

After converting the required option code to HEX, the setup is done in CLI:

 config system dhcp server

edit <dhcpservername>

set option1 <option_code> [<option_hex>]

end

For example, to configure option 252 with value http://192.168.1.1/wpad.dat, the command line would be:

 ascii = ftpservers=192.168.21.30,country=1,language=1

hex = 667470736572766572733d3139322e3136382e32312e33302c636f756e7472793d312c6c616e67756167653d31

Additional information can be found in the FortiGate CLI Administration Guide.
