---
id: collect-261121-veille-ia/fortinet/send-feedback-39
title: "SNMP agent"
domain: fortinet
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["snmp", "agent", "fortigate", "system", "manager", "description", "location", "information"]
source: https://docs.fortinet.com/document/fortigate/7.2.0/administration-guide/899762/snmp-agent
source_anchor: ""
source_lines: [1, 27]
section: "Gemini_Google"
---
# SNMP agent

# SNMP agent

The SNMP agent sends SNMP traps originating on the FortiGate to an external monitoring SNMP manager defined in a SNMP community. The SNMP manager can monitor the FortiGate system to determine if it is operating properly, or if any critical events occurring.

The description, location, and contact information for this FortiGate system will be part of the information that the SNMP manager receives. This information is useful if the SNMP manager is monitoring many devices, and enables faster responses when the FortiGate system requires attention.

#### To configure the SNMP agent in the GUI:

1. Go to *System > SNMP* .
2. Enable *SNMP Agent* .
3. Enter a description of the agent.
4. Enter the location of the FortiGate unit.
5. Enter a contact or administrator for the SNMP Agent or FortiGate unit.
6. Click *Apply* .

#### To configure the SNMP agent in the CLI:

```
config system snmp sysinfo
    set status enable
    set description <string>
    set contact-info <string>
    set location <string>
end
```
