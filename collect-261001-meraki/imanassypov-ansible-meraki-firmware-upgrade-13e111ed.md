Imagine you are a network engineer responsible for 50 branch offices, each with a Cisco Meraki security appliance, a stack of switches, and a wireless access point. Every week there are routine tasks: updating firmware, rotating Wi-Fi passwords, adjusting security policies. Done manually — logging into each dashboard, clicking through menus, verifying the change — this work is:
- Slow. An experienced engineer might configure 10 devices per hour.
- Error-prone. One missed checkbox, one typo in a VLAN ID, and you have an incident.
- Undocumented. What was the network configured to before you changed it? Who changed it? Why?
- Unscalable. 50 sites today, 500 sites tomorrow — the work scales linearly with headcount.
Infrastructure as Code (IaC) solves all of these problems.
Infrastructure as Code is the practice of defining and managing your network (or system) configuration in plain text files, and using software tools to apply those files to your infrastructure automatically.
Instead of clicking through a dashboard, you write a file that says:
network: Branch-Office-01
firmware:
  wireless_version_id: "15763"   # MR 32.1.7 stable
  upgrade_datetime: "2026-07-01T02:00:00Z"
Then you run a command — ansible-playbook, python script.py, or terraform apply — and the tool reads that file, talks to the Meraki Dashboard API, and makes the configuration match exactly what you declared. No clicking. No manual steps. No forgetting.
| Traditional "ClickOps" | Infrastructure as Code | 
|---|---|
| Changes are invisible — no record of what changed or when | Every change is a Git commit — full history, author, timestamp, and reason | 
| Documentation is a separate, always-out-of-date Word document | The configuration is the documentation | 
| Reproducing a configuration on a new device requires tribal knowledge | Re-run the playbook or terraform apply on any device | 
| Testing means hoping you did not break anything | You can run --check (dry run) to preview changes before applying them | 
| One engineer can manage ~50 devices | One engineer with automation can manage 5,000 devices | 
| A mistake is hard to reverse | A rollback is git revert followed by re-applying | 
IaC delivers the most value in environments that share some of these characteristics:
- Scale — more than a handful of devices or networks to manage
- Repetition — the same configuration pattern applied across many sites
- Compliance requirements — auditors need proof that configurations match a documented standard
- High rate of change — frequent firmware updates, regular password rotations, dynamic policy changes
- Team environments — multiple people making changes to the same infrastructure
- Disaster recovery — the ability to rebuild a configuration from scratch quickly
Even in small environments (5–10 devices), adopting IaC early builds habits and skills that pay dividends as the network grows.
One of the most powerful concepts in software engineering — and increasingly in network engineering — is abstraction: the idea that you should only have to think about what you want, not how to achieve it.
When you drive a car, you turn the steering wheel — you do not think about the hydraulic fluid pressure in the power steering rack. The steering wheel is an abstraction over a complex mechanical system. IaC works the same way.
The diagram below shows the four layers of abstraction in this repository:
Diagram source: diagrams/iac-abstraction.mmd
Layer 1 — Human Intent. You write a YAML file, a Python script, or an Ansible playbook that expresses what you want in plain, readable language. You do not need to know anything about HTTP headers, JSON payloads, or API rate limits.
Layer 2 — Automation Engine. A tool (Python SDK, Ansible collection, or Terraform provider) reads your intent and translates it into the correct API calls. It handles authentication, pagination, error retries, and idempotency — meaning it only makes a change if a change is actually needed.
Layer 3 — Meraki Dashboard API. The Meraki REST API is the authoritative interface to your Meraki organization. Every operation — reading network state, scheduling firmware upgrades, configuring SSIDs — happens through this API. The automation engines call it on your behalf.
Layer 4 — Physical Infrastructure. Your actual Meraki devices (MX appliances, MS switches, MR access points) receive configuration changes pushed by the Meraki Dashboard cloud. You never connect directly to the devices.
The practical benefit of this layered model is that you — the network engineer — work entirely at Layer 1. You never need to:
- Write an HTTP PUT request by hand
- Remember that firmware upgrades use a different endpoint format than SSID configuration
- Handle API pagination when an organization has more than 1,000 networks
- Worry about whether to use GET beforePUT to check current state
The automation engine handles all of that. Your job is to declare intent clearly.
When multiple engineers use the same tool to apply the same data model, configuration drift becomes impossible. There is no way for one engineer to configure Site A differently from Site B if both sites are defined in the same YAML file and applied with the same playbook. The abstraction enforces consistency as a side effect.
This repository demonstrates three distinct IaC approaches for Meraki network management. Each one sits at a different point on the spectrum from flexibility to declarative intent:
 FLEXIBLE ◄────────────────────────────────────────► DECLARATIVE
   │                        │                               │
Python SDK            Ansible Playbooks           Network as Code
                                                    (Terraform)
 "I write the          "I describe the              "I declare the
  logic"                steps"                       desired state"
Python gives you a full programming language. You write loops, conditionals, data transformations, and error handling exactly the way you want. The Meraki Python SDK wraps the REST API in simple Python function calls.
Best for: Custom logic, one-off data collection, complex multi-step workflows that do not fit neatly into a tool's model.
Ansible is a task automation tool built around the concept of playbooks — ordered lists of tasks expressed in YAML. Each task calls a module (a pre-built piece of logic) that knows how to talk to a specific system. The cisco.meraki Ansible Galaxy collection provides modules for the Meraki Dashboard API.
Best for: Multi-step workflows that must run in order, teams already using Ansible for server or network automation, situations where you want human-readable automation that non-developers can review.
Terraform is a declarative tool: you describe the desired final state of your network in a data model (YAML files), and Terraform figures out what API calls are needed to reach that state. The netascode/nac-meraki Terraform module reads your YAML and translates it into Meraki API operations.
Best for: Long-lived configuration that must always match a declared state, SSID management, policy enforcement, teams familiar with Terraform from cloud infrastructure management.
| # | Use Case | Approach | Files | 
|---|---|---|---|
| 1 | Discover available firmware versions across networks by tag | Python, Ansible | python/1.0_* ,ansible/1.0_* | 
| 2 | Schedule firmware upgrades across networks by tag, only where current ≠ target | Python, Ansible | python/2.0_* ,ansible/2.0_* | 
| 3 | Manage SSID configuration (name, PSK, encryption, visibility) declaratively | Network as Code | network-as-code/ ,data-model/nac/ | 
| 4 | PSK rotation without storing secrets in files using environment variable injection | Network as Code | network-as-code/networks_nac.yaml ,!env tags | 
| 5 | Shared data model that serves both firmware tools (Ansible + Python) from a single YAML file | Python, Ansible | data-model/firmware/firmware_targets.yaml | 
| 6 | CI/CD automation — auto-apply Terraform when the data model changes, using a self-hosted runner | GitHub Actions | .github/workflows/nac-apply.yml | 
| 7 | Network tag–based targeting — operate on any number of networks without listing individual IDs | Python, Ansible | All scripts | 
ansible-meraki-firmware-upgrade/
│
├── README.md                           ← You are here
│
├── data-model/                         ← Shared YAML configuration (the source of truth)
│   ├── firmware/
│   │   └── firmware_targets.yaml       ← Target firmware versions per network (Ansible + Python)
│   └── nac/
│       ├── networks_nac.yaml           ← SSID + PSK declarations (Terraform NaC only)
│       └── defaults_nac.yaml           ← Org-wide SSID defaults merged by NaC module
│
├── python/                             ← Python scripting approach
│   ├── 1.0_check_available_firmware_by_tag.py   ← Read-only: discover firmware versions
│   ├── 2.0_schedule_firmware_upgrade_by_tag.py  ← Schedule firmware upgrades
│   └── requirements.txt               ← pip dependencies (meraki, PyYAML)
│
├── ansible/                            ← Ansible playbook approach
│   ├── 1.0_check_available_firmware_by_tag.yml  ← Read-only: discover firmware versions
│   ├── 2.0_schedule_firmware_upgrade_by_tag.yml ← Schedule firmware upgrades
│   ├── README.md                       ← Detailed Ansible reference documentation
│   ├── collections/
│   │   └── requirements.yml           ← cisco.meraki Galaxy collection dependency
│   ├── inventory/
│   │   └── meraki.yml                 ← Ansible inventory (localhost only)
│   └── group_vars/all/
│       ├── vars.yml                   ← Non-sensitive shared variables
│       └── vault.yml                  ← API key (Ansible Vault encrypted)
│
├── network-as-code/                    ← Terraform Network as Code approach
│   ├── terraform.tf                   ← Provider configuration (CiscoDevNet/meraki ~> 1.12)
│   ├── main.tf                        ← NaC module declaration with methodology comments
│   ├── vault-envconsul.sh             ← CI/CD wrapper: pull PSKs from HashiCorp Vault
│   ├── .envrc.example                 ← Template for local environment variables
│   └── .gitignore                     ← Excludes .terraform/, state files, .envrc
│
├── .github/
│   └── workflows/
│       └── nac-apply.yml              ← GitHub Actions: auto terraform apply on data model push
│
├── diagrams/                           ← Diagrams (Mermaid PNGs + Draw.io sources)
│   ├── iac-abstraction.png            ← IaC abstraction layer diagram
│   ├── iac-abstraction.mmd            ← Diagram source (Mermaid)
│   ├── repo-workflow.png              ← Data flow diagram
│   ├── repo-workflow.mmd              ← Diagram source (Mermaid)
│   ├── 1.0-meraki-admin-logical-flow.drawio
│   ├── 2.0-pb1-module-mapping.drawio
│   └── 3.0-pb2-module-mapping.drawio
│
├── cisco-workflows/                    ← Reference Meraki workflow templates
│   ├── Appliance - Available Firmware.txt
│   └── Schedule Firmware Upgrade for Networks by Tag.json
│
├── ansible.cfg                         ← Ansible configuration (inventory path, settings)
├── .envrc                             ← direnv environment variables (gitignored)
└── .gitignore                         ← Root gitignore
This section walks you through everything you need to set up your environment and run your first command. Take it one step at a time.
Before you begin, make sure the following are installed and available in your terminal.
| Tool | Minimum Version | How to Check | Install Guide | 
|---|---|---|---|
| Git | 2.x | git --version | git-scm.com | 
| Python | 3.9 | python3 --version | python.org | 
| A Meraki API key | — | Meraki Dashboard → Profile → API access | Meraki docs | 
| Tool | How to Install | 
|---|---|
| meraki Python SDK | pip3 install meraki | 
| PyYAML | pip3 install PyYAML | 
| Tool | Minimum Version | How to Install | 
|---|---|---|
| Ansible Core | 2.15 | pip3 install ansible | 
| cisco.meraki collection | 2.18.0 | ansible-galaxy collection install -r ansible/collections/requirements.yml | 
| Tool | Minimum Version | How to Check | Install Guide | 
|---|---|---|---|
| Terraform | 1.8.0 | terraform version | developer.hashicorp.com | 
| direnv (recommended) | any | direnv version | brew install direnv | 
git clone https://github.com/imanassypov/ansible-meraki-firmware-upgrade.git
cd ansible-meraki-firmware-upgrade
Log into the Meraki Dashboard. Click your username in the top-right corner → My profile → scroll down to API access → Generate new API key.
Copy the key somewhere safe. You will only see it once.
Security note: Treat your API key like a password. It grants full read/write access to your Meraki organization. Never paste it into a command directly in a shared terminal, never commit it to Git.
The safest way to provide your API key to all three tools in this repository is via an environment variable. All tools — the Python SDK, the Ansible cisco.meraki collection, and the Terraform CiscoDevNet/meraki provider — read this same variable automatically.
export MERAKI_API_KEY="your-api-key-here"
If you want this to persist across terminal sessions, add the export line to your shell profile (~/.zshrc, ~/.bash_profile), or use direnv (recommended — see Section 12).
Run a quick check with Python to confirm the key works:
python3 -c "
import meraki, os
dashboard = meraki.DashboardAPI(os.environ['MERAKI_API_KEY'], suppress_logging=True)
orgs = dashboard.organizations.getOrganizations()
print('Connected! Organizations visible to this key:')
for o in orgs:
    print(f'  {o[\"name\"]} (ID: {o[\"id\"]})')
"
Expected output:
Connected! Organizations visible to this key:
  Igor M (ID: 443964)
If you see AuthorizationError, double-check that the API key was copied correctly and that API access is enabled in the Meraki Dashboard.
Both the Python scripts and Ansible playbooks use network tags to find which networks to operate on. Tags are set in the Meraki Dashboard:
- Log into dashboard.meraki.com
- Select your organization
- Go to Network-wide → General for each network
- Under Tags, add a tag (e.g., Cisco-Lab ,branch ,production )
Tags are case-sensitive. The scripts will only operate on networks that carry a tag you specify.
Choose the approach that matches your background and the task you want to accomplish:
| If you want to… | Start with… | 
|---|---|
| Check what firmware versions are available | Section 7 (Python) or Section 8 (Ansible) | 
| Schedule firmware upgrades automatically | Section 7.2 or Section 8.2 | 
| Manage SSID configuration as code | Section 9 (Terraform NaC) | 
| Understand how the data model works | Section 10 | 
| Set up CI/CD automation | Section 11 | 
Python is the Swiss Army knife of network automation. The Meraki Python SDK (pip install meraki) wraps the entire Meraki Dashboard REST API in simple Python objects. Instead of constructing HTTP requests by hand, you call methods:
import meraki, os
dashboard = meraki.DashboardAPI(os.environ['MERAKI_API_KEY'])
# List all networks in an organization
networks = dashboard.organizations.getOrganizationNetworks(organizationId="443964")
# Get firmware information for a specific network
firmware = dashboard.networks.getNetworkFirmwareUpgrades(networkId="L_650207196201616147")
The SDK handles:
- Authentication (injects the API key header on every request)
- Pagination (fetches all pages of results automatically)
- Rate limiting (respects Meraki's API rate limits, retries when throttled)
- Error handling (raises Python exceptions with descriptive messages)
pip3 install -r python/requirements.txt
The requirements.txt file specifies:
meraki       # Meraki Dashboard API Python library
PyYAML       # YAML file parsing (for reading data-model/ files)
File: python/1.0_check_available_firmware_by_tag.py
Purpose: Read-only. Discovers all networks matching your tags and prints current and available firmware versions for each device type (MX appliance, MS switch, MR access point).
The script follows this logic:
1. Read tags from data-model/firmware/firmware_targets.yaml
   (or from --tags argument, or MERAKI_NETWORK_TAGS env var)
2. Authenticate with the Meraki SDK
3. List all organizations visible to the API key
4. For each org: list all networks
5. Filter: keep only networks whose tags overlap with your target tags
6. For each matching network: query /networks/{id}/firmwareUpgrades
7. Print a formatted summary showing:
   - Current firmware version and ID for each device type
   - All available versions with their IDs and release types
The version IDs in the output are what you copy into data-model/firmware/firmware_targets.yaml when you want to schedule an upgrade.
# Auto-read tags from data-model/firmware/firmware_targets.yaml (recommended)
python python/1.0_check_available_firmware_by_tag.py
# Specify tags explicitly
python python/1.0_check_available_firmware_by_tag.py --tags Cisco-Lab
# Multiple tags
python python/1.0_check_available_firmware_by_tag.py --tags Cisco-Lab FTD══════════════════════════════════════════════════════════════════
Network : Meraki Core
Org     : Igor M
Tags    : Cisco-Lab
──────────────────────────────────────────────────────────────────
APPLIANCE (MX)
  Current   : MX 18.211.5.2 (ID: 4625)
  Available :
    - MX 19.2.8 [stable]      →  ID: 15806
    - MX 19.2.4 [stable]      →  ID: 15506
──────────────────────────────────────────────────────────────────
SWITCH (MS)
  Current   : MS 15.21.1 (ID: 3101)
  Available :
    - MS 17.2.2 [stable]      →  ID: 6016
──────────────────────────────────────────────────────────────────
WIRELESS (MR)
  Current   : MR 30.7.1 (ID: 11440)
  Available :
    - MR 32.1.7 [stable]      →  ID: 15763
    - MR 31.5.2 [stable]      →  ID: 13875
══════════════════════════════════════════════════════════════════
From this output, you can see that:
- To upgrade MX to the latest stable, use appliance_version_id: "15806"
- To upgrade MS to the latest stable, use switch_version_id: "6016"
- To upgrade MR to the latest stable, use wireless_version_id: "15763"
File: python/2.0_schedule_firmware_upgrade_by_tag.py
Purpose: Reads data-model/firmware/firmware_targets.yaml and enforces the declared firmware state. For each network, it compares the current version against the target and schedules an upgrade only where they differ.
Idempotent means you can run the same operation multiple times and get the same result — no duplicates, no errors, no unintended side effects.
Script 2.0 is idempotent. If a network is already on the target firmware version, the script skips it and prints COMPLIANT. If you run it 10 times, only the first run (when the firmware actually differs) does anything.
# Normal run — evaluate compliance and schedule upgrades
python python/2.0_schedule_firmware_upgrade_by_tag.py
# Dry run — show the plan without making any API changes
python python/2.0_schedule_firmware_upgrade_by_tag.py --check
Always run with --check first on a new environment.
══════════════════════════════════════════════════════════════════
COMPLIANCE EVALUATION (DRY RUN — no changes will be made)
══════════════════════════════════════════════════════════════════
Network : Meraki Core  |  Org: Igor M
  Appliance : UPGRADE NEEDED   Current: MX 18.211.5.2 (4625) → Target: 15806
  Switch    : UPGRADE NEEDED   Current: MS 15.21.1 (3101)    → Target: 6016
  Wireless  : COMPLIANT        Current: MR 32.1.7 (15763)   — no action needed
──────────────────────────────────────────────────────────────────
Scheduled at : next maintenance window (upgrade_datetime not set)
══════════════════════════════════════════════════════════════════
Ansible expresses automation as a playbook — a YAML file listing tasks to run in order, each calling a module. A module is a self-contained piece of logic that knows how to do one specific thing: create a user, copy a file, call an API endpoint.
The key insight is that Ansible is designed for humans to read. A well-written playbook is close to plain English:
- name: Get all Meraki organizations
  cisco.meraki.organizations_info:
    meraki_api_key: "{{ meraki_api_key }}"
  register: org_list
- name: Filter networks matching our tags
  ansible.builtin.set_fact:
    target_networks: "{{ all_networks | selectattr('tags', 'intersect', network_tags) }}"
Anyone on your team can read this, understand what it does, and propose a change — even without deep Python knowledge.
The cisco.meraki collection is distributed through Ansible Galaxy. Install it with:
ansible-galaxy collection install -r ansible/collections/requirements.yml
This downloads the cisco.meraki collection (version ≥ 2.18.0) and makes its modules available to Ansible. The requirements.yml file pins the version so your automation is reproducible.
File: ansible/1.0_check_available_firmware_by_tag.yml
Purpose: Mirrors python/1.0_* exactly — same data, different tool.
# From the repository root
ansible-playbook ansible/1.0_check_available_firmware_by_tag.yml \
  -i ansible/inventory/meraki.yml \
  -e '{"network_tags": ["Cisco-Lab"]}' \
  --vault-password-file .vault_pass
Important: Always use the JSON dict format (-e '{"network_tags": [...]}') for list variables in Ansible. The shorthand key=value format passes the value as a string, not a list, and the tag filter will not work correctly.
Unlike traditional Ansible that SSH into devices, these playbooks connect only to localhost. All Meraki API calls happen over HTTPS from your machine to api.meraki.com. Ansible is acting as an HTTP client, not a remote executor.
Your machine (localhost)
      │
      │  HTTPS port 443
      ▼
api.meraki.com  →  Your Meraki Org → Networks → Devices
No SSH keys. No device credentials. No firewall rules needed on the devices themselves.
File: ansible/2.0_schedule_firmware_upgrade_by_tag.yml
Purpose: Mirrors python/2.0_*. Reads data-model/firmware/firmware_targets.yaml, compares current state to desired state, and schedules upgrades only where needed.
ansible-playbook ansible/2.0_schedule_firmware_upgrade_by_tag.yml \
  -i ansible/inventory/meraki.yml \
  -e '{"network_tags": ["Cisco-Lab"]}' \
  --vault-password-file .vault_passansible-playbook ansible/2.0_schedule_firmware_upgrade_by_tag.yml \
  -i ansible/inventory/meraki.yml \
  -e '{"network_tags": ["Cisco-Lab"]}' \
  --vault-password-file .vault_pass \
  --check
Your Meraki API key grants full access to your organization. Never commit it in plain text.
Ansible provides Ansible Vault — an encryption feature built into Ansible that encrypts files so they can be safely committed to Git. The key is decrypted in memory at runtime — it never touches the disk unencrypted.
# 1. Create a vault password file (strong random password)
echo "$(openssl rand -base64 32)" > .vault_pass
chmod 600 .vault_pass
# 2. Add to gitignore (critical — never commit this file)
echo ".vault_pass" >> .gitignore
# 3. Encrypt the vault file that contains your API key
ansible-vault encrypt ansible/group_vars/all/vault.yml \
  --vault-password-file .vault_pass
After encryption, vault.yml looks like this (safe to commit):
$ANSIBLE_VAULT;1.1;AES256
30333534343937636662323135663063...
For a complete Ansible reference, including all playbook variables, idempotency details, and troubleshooting, see the detailed ansible/README.md.
If Python scripting is imperative ("do step 1, then step 2, then step 3") and Ansible is procedural ("run these tasks in order"), then Terraform is declarative:
You describe the desired final state. Terraform figures out how to get there.
You do not write code that says "create this SSID, then set the PSK, then set the encryption mode." You write a YAML file that says "I want this SSID to exist with these properties" — and Terraform calls the Meraki API to make that true, whatever the current state happens to be.
The netascode/nac-meraki Terraform module is the engine. It reads your YAML data model files and translates them into Meraki API calls using the CiscoDevNet/meraki Terraform provider.
The execution chain:
terraform apply
   │
   ├── reads network-as-code/main.tf
   │
   ├── invokes module "netascode/nac-meraki/meraki"
   │    └── scans yaml_directories: ["../data-model/nac"]
   │
   ├── reads data-model/nac/defaults_nac.yaml   ← org-wide defaults
   ├── reads data-model/nac/networks_nac.yaml   ← your SSID declarations
   │
   ├── merges defaults + network config → unified model
   │
   └── calls CiscoDevNet/meraki provider
        └── Meraki Dashboard API
             ├── GET  /organizations/{id}/networks   (look up existing networks)
             └── PUT  /networks/{id}/wireless/ssids/{n}  (configure SSIDs)
The SSID configuration lives in data-model/nac/networks_nac.yaml. This file is the single source of truth for SSID state. Here is what it looks like:
meraki:
  domains:
    - name: "meraki.local"
      organizations:
        - name: "Igor M"
          managed: false          # Org already exists — do not create
          networks:
            - name: "Meraki Core"
              managed: false      # Network already exists — do not create
              tags: [Cisco-Lab]
              wireless:
                ssids:
                  - name: "Test-WiFi"
                    ssid_number: "0"
                    enabled: true
                    auth_mode: "psk"
                    psk: !env MERAKI_CISCO_LAB_PSK    # ← secret, never stored in file
                    encryption_mode: "wpa"
                    wpa_encryption_mode: "WPA2 only"
                    visible: true
Two things worth noting:
managed: false — This tells the NaC module that the organization and network already exist in the Meraki Dashboard. Terraform will look them up (using data sources) rather than trying to create them. This is the correct setting for most real-world environments where the org and networks were created manually or by another process.
!env MERAKI_CISCO_LAB_PSK — The !env YAML tag is a NaC module feature. At runtime, the module substitutes this tag with the value of the named environment variable. The PSK is never written to any file, never appears in terraform plan output, and never gets committed to Git.
cd network-as-code
terraform init
terraform init downloads the netascode/nac-meraki module and the CiscoDevNet/meraki provider from the Terraform Registry. This only needs to be done once (or when you upgrade provider versions).
Copy the example file and populate it:
cp network-as-code/.envrc.example network-as-code/.envrc
Edit .envrc:
export MERAKI_API_KEY=your-api-key-here
export MERAKI_CISCO_LAB_PSK=your-wifi-password-here
export MERAKI_FTD_PSK=your-other-wifi-password-here
If you use direnv, run direnv allow in the network-as-code/ directory. Variables will be injected automatically whenever you cd into that directory.
Always plan before applying:
terraform plan
Terraform shows exactly what it will do. You should see something like:
  # module.meraki.meraki_wireless_ssid.networks_wireless_ssids["meraki.local/Igor M/Meraki Core/Test-WiFi"] will be updated in-place
  ~ resource "meraki_wireless_ssid" "networks_wireless_ssids" {
      ~ psk      = (sensitive value)
        name     = "Test-WiFi"
        # ...
    }
Plan: 0 to add, 1 to change, 0 to destroy.
If the plan shows unexpected deletions or creations, investigate before applying.
terraform apply
Terraform prompts for confirmation. Review the plan summary, type yes, and Terraform calls the Meraki API to apply the changes.
Rotating a Wi-Fi password is now a one-line operation:
# Direct (local dev)
export MERAKI_CISCO_LAB_PSK=NewSecurePassword123
terraform apply
# Via HashiCorp Vault (CI/CD)
vault kv put secret/meraki/cisco-lab-ssid psk=NewSecurePassword123
./network-as-code/vault-envconsul.sh terraform apply
The PSK change appears in the Meraki Dashboard within seconds of terraform apply completing.
You may notice that data-model/ has two subdirectories:
data-model/
├── firmware/     ← Read by Python and Ansible ONLY
│   └── firmware_targets.yaml
└── nac/          ← Read by Terraform ONLY
    ├── networks_nac.yaml
    └── defaults_nac.yaml
Both firmware_targets.yaml and networks_nac.yaml use the same meraki: root key in their YAML structure. If Terraform were to read both files, it would merge them into a single configuration tree — with unpredictable results. By placing them in separate directories and configuring Terraform to only scan data-model/nac/, the two data models are kept completely isolated. Python and Ansible never see data-model/nac/, and Terraform never sees data-model/firmware/.
The data model is the definition of what you want your network to look like — written in YAML, stored in data-model/, and consumed by your automation tools. It is the bridge between human intent and machine action.
Diagram source: diagrams/repo-workflow.mmd
Think of the data model as a configuration contract: "Networks tagged Cisco-Lab should be running MR firmware version 15763, with SSID Test-WiFi on slot 0, using WPA2 with the PSK in MERAKI_CISCO_LAB_PSK." Any tool that reads the contract and talks to the Meraki API will produce the same outcome.
This file defines the desired firmware end state for each network. Python scripts and Ansible playbooks read it.
meraki:
  domains:
    - organizations:
        - name: "Igor M"
          networks:
            - name: "Meraki Core"
              tags: [Cisco-Lab]
              firmware:
                description: "Cisco-Lab — all device families stable firmware"
                upgrade_datetime: ""           # "" = next maintenance window
                upgrade_strategy: "minimizeClientDowntime"
                target:
                  appliance_version_id: "15806"  # MX 19.2.8 stable
                  switch_version_id: "6016"       # MS 17.2.2 stable
                  wireless_version_id: "15763"    # MR 32.1.7 stable
| Field | Required | Description | 
|---|---|---|
| name | Yes | Exact network name (case-sensitive) as shown in the Meraki Dashboard | 
| tags | Yes | Network tags — used for informational grouping in the data model | 
| firmware.description | No | Human-readable label shown in script output | 
| firmware.upgrade_datetime | No | ISO 8601 UTC timestamp. Empty string = next maintenance window | 
| firmware.upgrade_strategy | No | minimizeClientDowntime (default) orminimizeUpgradeTime | 
| firmware.target.appliance_version_id | No | MX target version ID. Empty string = do not touch | 
| firmware.target.switch_version_id | No | MS target version ID. Empty string = do not touch | 
| firmware.target.wireless_version_id | No | MR target version ID. Empty string = do not touch | 
Version IDs come from running python/1.0_* or ansible/1.0_*. They are stable numeric strings that Meraki uses internally to identify specific firmware builds.
This file defines the desired SSID and wireless configuration. Terraform reads it exclusively.
See Section 9.3 for a full explanation.
For a complete, step-by-step walkthrough of wiring GitHub to the local
runner — including registration, the .env file, a line-by-line workflow
explanation, the Terraform state nuance, and the real issues we hit — see the
dedicated guide: README-GITHUB-ACTIONS.md.
When you run terraform apply locally, the SSID change happens immediately. But what if you want changes to happen automatically whenever a team member edits the data model and pushes to Git? That is what CI/CD (Continuous Integration / Continuous Deployment) provides.
The workflow file .github/workflows/nac-apply.yml defines a GitHub Actions pipeline that:
- Triggers whenever a file in data-model/nac/ is pushed to themain branch
- Runs on a self-hosted runner — your laptop or a dedicated machine — so it has access to local Terraform state and does not need secrets stored in GitHub
- Executes: terraform init →terraform plan →terraform apply
on:
  workflow_dispatch: {}         # Manual trigger from GitHub UI
  push:
    branches: [main]
    paths: ["data-model/nac/**"]    # Only triggers on NaC data model changes
jobs:
  terraform-apply:
    runs-on: self-hosted
    defaults:
      run:
        working-directory: network-as-code
    steps:
      - uses: actions/checkout@v4
      - run: terraform init -input=false
      - run: terraform plan -input=false -out=tfplan
      - run: terraform apply -input=false tfplan
A self-hosted runner is a background process on your machine that listens for jobs from GitHub and executes them locally.
- Go to your repository on GitHub → Settings → Actions → Runners → New self-hosted runner
- Follow the instructions to download and configure the runner
- Set required environment variables in ~/actions-runner/.env :
MERAKI_API_KEY=your-api-key-here
MERAKI_CISCO_LAB_PSK=your-psk-here
MERAKI_FTD_PSK=your-other-psk-here
PATH=/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
Note: The PATH entry is required because the runner's shell does not inherit your interactive shell's PATH. Without it, terraform will not be found even if it is installed.
cd ~/actions-runner
./run.sh
You should see:
√ Connected to GitHub
Listening for Jobs
To survive reboots without manual intervention, install the runner as a launchd service:
cd ~/actions-runner
./svc.sh install
./svc.sh start
The pipeline triggers automatically on any push to main that changes a file under data-model/nac/. It can also be triggered manually from the GitHub UI:
GitHub repository → Actions → NaC — Terraform apply → Run workflow
Never commit credentials to Git. Not in YAML files, not in comments, not in example files. Not even "just for testing."
This repository is designed so that all credentials — the Meraki API key and Wi-Fi PSKs — are provided exclusively through environment variables. No credential ever touches a file that could be committed.
direnv is a shell extension that automatically loads and unloads environment variables when you change directories. It reads from a .envrc file in each directory.
# Install (macOS)
brew install direnv
# Add to your shell profile (~/.zshrc or ~/.bash_profile)
eval "$(direnv hook zsh)"   # for zsh
eval "$(direnv hook bash)"  # for bash
Then create .envrc at the repository root:
# .envrc (this file is gitignored)
export MERAKI_API_KEY=your-api-key-here
export MERAKI_NETWORK_TAGS=Cisco-Lab,FTD
And in network-as-code/.envrc:
# network-as-code/.envrc (also gitignored)
export MERAKI_API_KEY=your-api-key-here
export MERAKI_CISCO_LAB_PSK=your-psk-here
export MERAKI_FTD_PSK=your-other-psk-here
Allow direnv to load each file once:
direnv allow .
direnv allow network-as-code/
After that, variables are injected automatically whenever you cd into the directory.
The self-hosted GitHub Actions runner reads environment variables from ~/actions-runner/.env. This file lives on your machine only — never in the Git repository.
# ~/actions-runner/.env
MERAKI_API_KEY=your-api-key-here
MERAKI_CISCO_LAB_PSK=your-psk-here
MERAKI_FTD_PSK=your-other-psk-here
PATH=/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
For team environments where the API key and PSKs are shared among multiple engineers, HashiCorp Vault is the recommended secret store. The network-as-code/vault-envconsul.sh script wraps any Terraform command to pull PSK values from Vault at runtime using envconsul:
# Store PSK in Vault
vault kv put secret/meraki/cisco-lab-ssid psk=NewSecurePassword
# Apply with PSK injected from Vault
export VAULT_ADDR=https://vault.example.com
export VAULT_TOKEN=<token>
./network-as-code/vault-envconsul.sh terraform apply
| Symptom | Likely Cause | Resolution | 
|---|---|---|
| MERAKI_API_KEY not set | Environment variable missing | Run export MERAKI_API_KEY=your-key-here or use direnv | 
| AuthorizationError (401) | API key invalid or expired | Regenerate the key in Meraki Dashboard → Profile → API access | 
| 403 Forbidden on a specific org | Key does not have access to that org | Check org-level API access in the Meraki Dashboard | 
| No networks found matching tags | Tag misspelled, or networks not tagged | Tags are case-sensitive. Verify tags in Meraki Dashboard → Network-wide → General | 
| Symptom | Likely Cause | Resolution | 
|---|---|---|
| ModuleNotFoundError: No module named 'meraki' | Dependencies not installed | pip3 install -r python/requirements.txt | 
| Script exits immediately with no output | Tags derived from data model but file not found | Run python 1.0_* --tags YourTag to specify tags directly | 
| No networks found | Network not tagged in Meraki Dashboard | Log into dashboard.meraki.com, go to Network-wide → General, and add the tag | 
| Symptom | Likely Cause | Resolution | 
|---|---|---|
| ERROR! A required module is missing: cisco.meraki | Collection not installed | ansible-galaxy collection install -r ansible/collections/requirements.yml | 
| fatal: network_tags is empty | Tag variable not passed | Use -e '{"network_tags": ["YourTag"]}' | 
| ERROR! Attempting to decrypt but no vault secrets found | --vault-password-file missing | Add --vault-password-file .vault_pass to the command | 
| Tag filter returns 0 networks despite correct tag | List passed as string, not array | Always use JSON dict syntax: -e '{"network_tags": ["tag"]}' | 
For the complete Ansible troubleshooting reference, see ansible/README.md.
| Symptom | Likely Cause | Resolution | 
|---|---|---|
| Error: No valid credential sources found | MERAKI_API_KEY not set | export MERAKI_API_KEY=your-key-here | 
| Error: could not find network "Meraki Core" | managed: false lookup fails — network name mismatch | Check exact network name in Meraki Dashboard (case-sensitive) | 
| PSK not updating on Meraki | !env variable not set or set to wrong name | Run echo $MERAKI_CISCO_LAB_PSK to verify the variable is exported | 
| terraform: command not found in GitHub Actions | Runner PATH does not include Homebrew | Add PATH=/opt/homebrew/bin:... to~/actions-runner/.env and restart the runner | 
| Plan shows 4 resources to create (expected 0 to change) | Runner has no Terraform state file — creates fresh state | This is normal; Meraki resources are idempotent and apply succeeds. Consider a remote backend for shared state. | 
If you update the diagram source files, regenerate the PNGs with:
# Abstraction diagram
mmdc -i diagrams/iac-abstraction.mmd -o diagrams/iac-abstraction.png --scale 3 --backgroundColor white
/usr/bin/sips -Z 4000 diagrams/iac-abstraction.png --out diagrams/iac-abstraction.png
# Data flow diagram
mmdc -i diagrams/repo-workflow.mmd -o diagrams/repo-workflow.png --scale 3 --backgroundColor white
/usr/bin/sips -Z 4000 diagrams/repo-workflow.png --out diagrams/repo-workflow.png
Verify size constraints (GitHub requires: longest side ≤ 4000 px, file size ≤ 1.2 MB):
/usr/bin/sips -g pixelWidth -g pixelHeight diagrams/iac-abstraction.png
ls -lh diagrams/iac-abstraction.png
Commit both .mmd source and .png output together.