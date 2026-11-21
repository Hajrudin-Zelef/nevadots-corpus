---
id: collect-261121-veille-ia/langages-dev/troubleshooting-tip-avoid-errors-when-adding-an-interface-to-an-sd-wan-community
title: "Troubleshooting Tip: Avoid errors when adding an interface to an SD-WAN | Community"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["interface", "sd-wan", "avoid", "errors", "adding", "fortigate", "interfaces", "policies"]
source: https://community.fortinet.com:443/t5/FortiGate/Troubleshooting-Tip-Avoid-errors-when-adding-an-interface-to-an/ta-p/196589
source_anchor: ""
source_lines: [1, 29]
section: "Langages_Dev"
---
**Description**


This article describes how to avoid common errors when adding an interface to an SD-WAN.


**Scope**


FortiGate.

**Solution**


Before configuring FortiGate interfaces as SD-WAN members, it is necessary to remove or redirect existing configuration references to those interfaces in routes and security policies. 

This includes the default Internet access policy that’s included with many FortiGate models. 

Note that after removing the routes and security policies, traffic cannot reach the WAN ports through the FortiGate. 

Redirecting the routes and policies to reference other interfaces prevents the need to create them again later.


For example:


1) WAN2 is the physical interface to add the SD-WAN member into, but WAN2 has a reference in the static route and policies.

**Network options -> Interfaces**, then scroll left to the specific interface. Select the appropriate reference number in the reference column.
