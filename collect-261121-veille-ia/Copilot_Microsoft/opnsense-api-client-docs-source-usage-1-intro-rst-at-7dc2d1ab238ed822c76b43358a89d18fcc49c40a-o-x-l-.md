---
id: collect-261121-veille-ia/opnsense-pfsense/opnsense-api-client-docs-source-usage-1-intro-rst-at-7dc2d1ab238ed822c76b43358a89d18fcc49c40a-o-x-l-
title: "opnsense-api-client/docs/source/usage/1_intro.rst at 7dc2d1ab238ed822c76b43358a89d18fcc49c40a · O-X-L/opnsense-api-client"
domain: opnsense-pfsense
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["opnsense-api-client", "opnsense", "client", "docs", "source", "usage", "intro", "rst"]
source: https://github.com/o-x-l/opnsense-api-client/blob/HEAD/docs/source/usage/1_intro.rst
source_anchor: ""
source_lines: [1, 19]
section: "Copilot_Microsoft"
---
This is a Python3 client for interacting with the official OPNsense API.

It enables easy management and automation of OPNsense firewalls.

The base-code is a Fork of this OPNsense Ansible-Collection that was refactored for use within raw Python.

This can be useful if you want to automate your Infrastructure and do not use Ansible.

`pip install oxl-opnsense-client````
from oxl_opnsense_client import Client
with Client(
    firewall='192.168.10.20',
    port=443,  # default
    credential_file='/tmp/.opnsense.txt',
    # token='0pWN/C3tnXem6OoOp0zc9K5GUBoqBKCZ8jj8nc4LEjbFixjM0ELgEyXnb4BIqVgGNunuX0uLThblgp9Z',
    # secret='Vod5ug1kdSu3KlrYSzIZV9Ae9YFMgugCIZdIIYpefPQVhvp6KKuT7ugUIxCeKGvN6tj9uqduOzOzUlv',
) as c:
    c.test()
```
