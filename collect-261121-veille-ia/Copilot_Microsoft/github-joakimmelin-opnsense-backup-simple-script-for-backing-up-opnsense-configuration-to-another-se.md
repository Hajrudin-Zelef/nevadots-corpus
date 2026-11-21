---
id: collect-261121-veille-ia/opnsense-pfsense/github-joakimmelin-opnsense-backup-simple-script-for-backing-up-opnsense-configuration-to-another-se
title: "GitHub - joakimmelin/opnsense-backup: simple script for backing up OPNSense configuration to another server."
domain: opnsense-pfsense
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["script", "simple", "backing", "opnsense", "configuration", "another", "server", "github"]
source: https://github.com/joakimmelin/opnsense-backup
source_anchor: ""
source_lines: [1, 16]
section: "Copilot_Microsoft"
---
A simple script for backing up OPNSense configuration to another server. Feel free to copy and/or modify as needed.

The script is supposed to run every 24 hours and creates a unique file name every time (`<hostname>-config-<date>.xml`).

Requires the following:

1. Install Bash (`pkg install bash` )
2. Make sure login via SSH works without password (i.e with SSH key)

The script is pretty self-explanatory. Save it where ever you want to run it but /usr/local/etc is probably best.

Create a new entry in root's crontab:

`*       *       2       *       *       (/usr/local/etc/backup.sh) >/dev/null 2>&1`

Make sure to modify the script and include hostname, user and target directory for where the backups will be sent.
