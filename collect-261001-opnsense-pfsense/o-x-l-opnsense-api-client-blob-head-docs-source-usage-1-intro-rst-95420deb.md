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