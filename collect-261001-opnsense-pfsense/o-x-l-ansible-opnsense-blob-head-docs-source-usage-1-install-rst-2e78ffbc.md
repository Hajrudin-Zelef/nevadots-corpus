See the documentation on how to install Ansible.

If you DO NOT want to use Ansible - this fork provides you with a raw Python3 interface.

The httpx python module is used for API communications!

`python3 -m pip install --upgrade httpx````
# stable version:
ansible-galaxy collection install oxlorg.opnsense
# latest version:
ansible-galaxy collection install git+https://github.com/O-X-L/ansible-opnsense.git
# install to specific directory for easier development
cd $PLAYBOOK_DIR
ansible-galaxy collection install git+https://github.com/O-X-L/ansible-opnsense.git -p ./collections
```