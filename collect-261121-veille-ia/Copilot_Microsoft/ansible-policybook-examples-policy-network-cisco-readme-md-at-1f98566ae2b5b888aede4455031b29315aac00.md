---
id: collect-261121-veille-ia/cisco/ansible-policybook-examples-policy-network-cisco-readme-md-at-1f98566ae2b5b888aede4455031b29315aac00
title: "ansible-policybook-examples/policy/network/cisco/README.md at 1f98566ae2b5b888aede4455031b29315aac0041 · ansible/ansible-policybook-examples"
domain: cisco
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["cisco", "ansible-policybook-examples", "policy", "network", "check", "validated", "readme", "f98566ae2b5b888aede4455031b29315aac0041"]
source: https://github.com/ansible/ansible-policybook-examples/blob/HEAD/policy/network/cisco/README.md
source_anchor: ""
source_lines: [1, 23]
section: "Copilot_Microsoft"
---
Here is a sample playbook, let's call it "cisco_ios.yml", which can be used to demonstrate policy checks:

```
---
- name: Configure Cisco IOS
  cisco.ios.ios_config:
    lines:
      - hostname foobar
      - line vty 0
      - password password123
      - no login
      - set ntp server 127.0.0.1
```
```
$ ansible-policy -p examples/check_project/cisco_ios.yml --policy-dir ansible-policybook-examples/policy/network/cisco/
TASK [Configure Cisco IOS] examples/check_project/cisco_ios.yml L2-10 ************************************************************
... Check_on_IOS_configuration Not Validated
    WARNING: Cisco IOS settings violation
--------------------------------------------------------------------------------------------------------------------------------------------
SUMMARY
... Total files: 1, Validated: 0, Not Validated: 1
Violations are detected! in 1 task
```
