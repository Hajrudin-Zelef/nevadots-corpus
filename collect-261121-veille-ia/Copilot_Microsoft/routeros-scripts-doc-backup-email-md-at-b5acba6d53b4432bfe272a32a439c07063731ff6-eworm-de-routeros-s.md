---
id: collect-261121-veille-ia/copilot-microsoft/routeros-scripts-doc-backup-email-md-at-b5acba6d53b4432bfe272a32a439c07063731ff6-eworm-de-routeros-s
title: "routeros-scripts/doc/backup-email.md at b5acba6d53b4432bfe272a32a439c07063731ff6 · eworm-de/routeros-scripts"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["backup-email", "script", "backup", "routeros-scripts", "configuration", "system", "whether", "send"]
source: https://github.com/eworm-de/routeros-scripts/blob/HEAD/doc/backup-email.md
source_anchor: ""
source_lines: [1, 42]
section: "Copilot_Microsoft"
---
ℹ️ **Info**: This script can not be used on its own but requires the base
installation. See main README for details.


This script sends binary backup (`/system/backup/save`) and complete
configuration export (`/export terse show-sensitive`) via e-mail.

Just install the script and the required module:

```
$ScriptInstallUpdate mod/notification-email,backup-email;
```
Also make sure you configure sending notifications via e-mail.

The configuration goes to `global-config-overlay`, these are the parameters:

- `BackupFileNameDate` : whether to add date & time in filenames
- `BackupSendBinary` : whether to send binary backup
- `BackupSendExport` : whether to send configuration export
- `BackupSendGlobalConfig` : whether to send`global-config-overlay`
- `BackupPassword` : password to encrypt the backup with
- `BackupRandomDelay` : delay up to amount of seconds when run from scheduler

ℹ️ **Info**: Copy relevant configuration from
`global-config` (the one without `-overlay`) to
your local `global-config-overlay` and modify it to your specific needs.


Just run the script:

```
/system/script/run backup-email;
```
Creating a scheduler may be an option:

```
/system/scheduler/add interval=1w name=backup-email on-event="/system/script/run backup-email;" start-time=09:15:00;
```
- Upload backup to Mikrotik cloud
- Save configuration to fallback partition
- Send notifications via e-mail
- Upload backup to server
