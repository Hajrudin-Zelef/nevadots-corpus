---
id: collect-261121-veille-ia/copilot-microsoft/sheynshield-fortinet-fortigate-cheatsheet-system-fabric-management-checklist-md-at-172fb9490610bedf2
title: "sheynshield/Fortinet/FortiGate/CheatSheet/System/Fabric-Management-Checklist.md at 172fb9490610bedf2470836990d0ff5347fa74a0 · Shayan-heydarikhah/sheynshield"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["verified", "firmware", "reviewed", "identified", "documented", "upgrade", "fabric", "devices"]
source: https://github.com/shayan-heydarikhah/sheynshield/blob/HEAD/Fortinet/FortiGate/CheatSheet/System/Fabric-Management-Checklist.md
source_anchor: ""
source_lines: [1, 1270]
section: "Copilot_Microsoft"
---
**FortiOS 7.2.0 | Security Fabric | Fabric Management | Federated Upgrade | Firmware Upgrade | TFTP | USB | Controlled Upgrade | Firmware Verification | Recovery**

**SheynShield | Engineering Secure Networks**


FortiGate **Fabric Management** provides centralized firmware and device management for devices participating in the Security Fabric.

- FortiGate devices identified.
- FortiAP devices identified.
- FortiSwitch devices identified.
- Fabric topology verified.
- Fabric root FortiGate identified.
- Fabric members authorized.
- Device registration status verified.
- Device operational status verified.
- Current firmware versions documented.
- Target firmware version documented.
- Upgrade status reviewed.
- Firmware maturity reviewed.

```
System
└── Fabric Management
```
- Device authorization.
- Device registration.
- Device monitoring.
- Firmware version visibility.
- Firmware maturity visibility.
- Upgrade management.
- Upgrade scheduling.
- Multi-device upgrade.
- Fabric upgrade status.

Caution

**Never start a production firmware upgrade without a rollback/recovery plan.**

- Current FortiOS version documented.
- Current build number documented.
- Hardware model verified.
- HA topology documented.
- Security Fabric topology documented.
- Managed FortiAP devices documented.
- Managed FortiSwitch devices documented.
- Critical services documented.
- Critical VPNs documented.
- Routing dependencies documented.
- External dependencies documented.

- FortiGuard connectivity verified.
- DNS resolution verified.
- Internet connectivity verified where required.
- Routing verified.
- Management connectivity verified.
- Out-of-band management available where possible.
- Console access available.

- NTP configured.
- NTP synchronization verified.
- System time verified.
- Time zone verified.

- FortiCare status verified.
- FortiGuard services verified.
- Required licenses verified.
- Security service subscriptions verified.

- Configuration backup created.
- Backup stored outside the device.
- Backup tested/reviewed.
- Recovery configuration available.
- Previous firmware image available.

- Target firmware downloaded.
- Correct hardware image selected.
- Firmware source verified.
- Firmware authenticity verified.
- Firmware integrity verified.
- Upgrade path verified.
- Firmware maturity reviewed.
- Feature/Mature classification reviewed.

- Firmware tested in lab where possible.
- Pilot device identified.
- Low-risk devices identified.
- Production devices grouped by risk.
- Critical devices scheduled last where possible.

- Maintenance window approved.
- Expected reboot time estimated.
- Stakeholders notified.
- Rollback procedure documented.
- Recovery equipment available.
- TFTP server prepared if required.
- USB recovery media prepared if required.

Starting with **FortiOS 7.2.0**, FortiOS firmware images use maturity tags. ([Fortinet Documentation][2])

- Feature tag identified.
- New functionality requirements reviewed.
- Production risk evaluated.
- Feature release warning reviewed.

- Mature tag identified.
- Release contains no new major features.
- Bug fixes reviewed.
- Vulnerability patches reviewed.
- Production suitability evaluated.

```
Feature → Gray
Mature  → Green
```
`get system status`
Look for:

```
GA.F → Feature
GA.M → Mature
```
- Current firmware maturity identified.
- Target firmware maturity identified.
- Mature → Feature transition evaluated.
- Feature firmware warning reviewed.

Important

**Feature vs Mature is a release maturity classification, not simply "new vs old".**

- Current FortiOS version identified.
- Target FortiOS version identified.
- Official upgrade path checked.
- Intermediate versions identified.
- Direct upgrade availability verified.
- Hardware compatibility verified.
- Configuration migration impact reviewed.
- Feature changes reviewed.
- HA dependencies reviewed.
- Fabric dependencies reviewed.

```
Current Version
      │
      ▼
Target Version
      │
      ▼
Supported Upgrade Path?
      │
   ┌──┴──┐
  YES    NO
   │      │
   ▼      ▼
Proceed  STOP
         Review
         Intermediate
         Versions
```
Warning

**Do not assume that every FortiOS release can be upgraded directly to every newer release.**

When the upgrade path contains multiple builds, FortiGate can automatically follow the path during a federated update. ([Fortinet Documentation][3])

- Follow upgrade path option reviewed.
- Intermediate versions reviewed.
- Multiple reboot requirement understood.
- Direct upgrade risk evaluated.

- Local configuration backup created.
- External backup created.
- FortiManager backup verified if applicable.
- Backup timestamp recorded.
- Backup location documented.
- Recovery configuration available.
- Configuration changes frozen before upgrade.

For custom/scheduled Fabric upgrades:

- Configuration backup timing understood.
- Pending configuration changes reviewed.
- Final configuration backup performed if necessary.
- No uncommitted production changes remain.

Caution

In a custom scheduled upgrade, the configuration backup is created when the upgrade is scheduled. Configuration changes made afterward may not be included in that scheduled backup. ([Fortinet Documentation][4])

- FortiGuard connectivity verified.
- DNS resolution verified.
- Routing verified.
- Licensing verified.
- Auto-update functionality verified.

After firmware installation:

`execute update-now`
- AV definitions updated.
- Attack definitions updated.
- FortiGuard connectivity verified.
- Security services operational.
- System processes healthy.

```
Firmware Upgrade
      │
      ▼
Reboot
      │
      ▼
get system status
      │
      ▼
execute update-now
      │
      ▼
Verify Definitions
      │
      ▼
Verify Services
      │
      ▼
Validate Traffic
```
Important

**A successful firmware installation does not automatically mean the complete system is healthy.**

```
System
└── Fabric Management
    └── Fabric Upgrade
```
- Logged into correct FortiGate.
- Root FortiGate identified.
- Fabric topology verified.
- Target firmware selected.
- Latest firmware reviewed.
- All available upgrades reviewed.
- Maturity level reviewed.
- Upgrade path reviewed.
- Immediate vs Custom schedule selected.
- Configuration backup confirmed.
- Upgrade schedule reviewed.
- Device list reviewed.
- Upgrade started during approved window.

Fortinet documents Fabric Upgrade as the centralized mechanism for upgrading the root/Fabric devices and managed devices. ([Fortinet Documentation][4])

Before Fabric firmware-management workflows:

- FortiGuard Auto-Update Tunnel requirements reviewed.
- FortiGuard connectivity verified.
- DNS verified.
- Routing verified.
- NTP verified.
- Licensing verified.
- Fabric connectivity verified.

```
Fabric Device
      │
      ▼
FortiGate / Fabric Root
      │
      ▼
FortiGuard
      │
      ▼
Firmware / Security Services
```
A federated upgrade can coordinate firmware upgrades across Fabric devices and can follow a multi-build upgrade path where supported. ([Fortinet Documentation][3])

`execute federated-upgrade initialize``execute federated-upgrade status``execute federated-upgrade cancel``execute federated-upgrade restart`
Note

`restart` is available in later 7.2.x documentation. Always verify the exact CLI options against the FortiOS build being used.

- Fabric topology verified.
- Root FortiGate identified.
- Target version selected.
- Upgrade path reviewed.
- Intermediate versions reviewed.
- Upgrade schedule selected.
- Configuration backup confirmed.
- Fabric devices reviewed.
- FortiGate upgrade sequence reviewed.
- FortiAP behavior reviewed.
- FortiSwitch behavior reviewed.
- Multiple reboot requirement understood.

Important

In FortiOS 7.2.0, FortiAP and FortiSwitch devices cannot follow the same multi-build upgrade path as FortiGate devices; they upgrade directly to the target version. ([Fortinet Documentation][3])

- TFTP server installed.
- Firmware copied to TFTP root.
- Correct firmware filename confirmed.
- Correct hardware image confirmed.
- TFTP server IP confirmed.
- Network connectivity confirmed.
- Firewall rules verified.

Example:

```
TFTP Root
└── a.pkg
```
`execute ping 192.168.20.200`
- Ping successful.
- TFTP server reachable.
- Firmware file available.

`execute restore image tftp a.pkg 192.168.20.200````
This operation will replace the current firmware version!
Do you want to continue? (y/n)
```
- Upgrade warning reviewed.
- Firmware source verified.
- Correct image confirmed.
- Upgrade approved.
- Confirmation entered.

FortiGate performs operations including:

- Firmware download.
- Signature verification.
- Firmware integrity verification.
- Firmware installation.
- Reboot.

```
Ping
 ↓
TFTP
 ↓
Download
 ↓
Verify Signature
 ↓
Verify Integrity
 ↓
Install
 ↓
Reboot
 ↓
New FortiOS
```
Controlled upgrades allow a firmware image to be staged in a separate partition before activation. ([Fortinet Documentation][1])

`execute restore secondary-image tftp a.pkg`
Possible sources:

```
FTP
TFTP
USB
```
`execute set-next-reboot secondary`
Important

`secondary` must be used when the newly staged firmware is located in the secondary partition. The `primary`/`secondary` value refers to the partition containing the firmware that should be loaded on the next reboot. ([Fortinet Documentation][1])

- Firmware image prepared.
- Firmware image verified.
- Secondary partition available.
- Firmware staged.
- Staging completed successfully.
- Correct partition identified.
-  `set-next-reboot` configured.
- Maintenance window approved.
- Reboot scheduled.
- Post-reboot validation planned.

```
Current Firmware
      │
      ▼
Secondary Partition
      │
      ├── Load New Firmware
      │
      ▼
Verify
      │
      ▼
set-next-reboot secondary
      │
      ▼
Maintenance Window
      │
      ▼
Reboot
      │
      ▼
New Firmware
```
- Staged upgrades.
- Maintenance-window optimization.
- Multiple FortiGate deployment.
- Script-based deployment.
- FortiManager-controlled workflow.

- Firmware downloaded from trusted Fortinet source.
- Correct hardware model verified.
- Firmware build verified.
- Signature verification performed.
- Integrity verification performed.
- Unexpected signature warning investigated.

```
Firmware Image
      │
      ▼
Digital Signature
      │
      ▼
FortiGate Verification
      │
   ┌──┴──┐
  PASS   FAIL
   │      │
   ▼      ▼
Continue STOP
          Investigate
```
Caution

**Never ignore firmware authenticity warnings in production.**

- Stop upgrade if unexpected.
- Verify firmware source.
- Verify file.
- Verify model compatibility.
- Verify build.
- Re-download firmware if necessary.
- Investigate before proceeding.

Use console-based boot testing when you need to verify a firmware image without immediately making it the permanent default firmware.

- Console cable.
- TFTP server.
- Firmware image.
- Local management information.
- TFTP server IP.
- Firmware filename.

`execute reboot`
Confirm:

```
y
```
Watch for:

```
Press any key to display configuration menu ..
```
- Press key during boot window.
- Enter boot menu.
- Avoid missing the short interruption window.

```
Boot
 │
 ▼
Press Any Key
 │
 ├── YES → Boot Menu
 │
 └── NO  → Normal Boot
```
Typical options:

| Key | Function | 
|---|---|
| `g` | Get firmware image from TFTP | 
| `f` | Format boot device | 
| `b` | Boot backup firmware and set as default | 
| `c` | Configuration/information | 
| `q` | Continue normal boot | 
| `h` | Help | 

- Enter boot menu.
-  Select `g` .
- Enter TFTP server address.
- Enter local FortiGate address.
- Enter firmware filename.
- Download image.
- Verify image.
- Select run-without-saving option where supported.
- Test firmware.
- Validate basic functionality.

[!DANGER]
`f` can format the boot device. **Never select it casually in production.**


Select:

```
c
```
Potential configuration options:

| Key | Function | 
|---|---|
| `p` | Firmware download port | 
| `d` | DHCP mode | 
| `i` | Local IP address | 
| `s` | Local subnet mask | 
| `g` | Local gateway | 
| `v` | VLAN ID | 
| `t` | TFTP server | 
| `f` | Firmware filename | 
| `e` | Reset TFTP parameters | 
| `r` | Review parameters | 
| `n` | Network diagnostic/ping | 
| `q` | Quit | 
| `h` | Help | 

- Download port verified.
- Local IP configured.
- Subnet mask configured.
- Gateway configured if required.
- VLAN configured if required.
- TFTP server configured.
- Firmware filename configured.
- Parameters reviewed.
- Network test completed.

- USB drive available.
- Firmware image copied to USB root.
- Correct filename verified.
- Correct hardware image verified.
- USB connected to FortiGate.

`execute restore image usb <filename>`
Example:

`execute restore image usb a.pkg````
This operation will replace the current firmware version!
Do you want to continue? (y/n)
```
- Warning reviewed.
- Correct firmware confirmed.
- Upgrade approved.
- Reboot expected.

`get system status``execute update-now`
- FortiOS version.
- Build number.
- Device model.
- System status.
- FortiGuard connectivity.
- AV definitions.
- Attack definitions.
- Critical services.

GUI location:

```
System
└── Settings
    └── Startup Settings
```
- Detect Firmware requirement identified.
- Firmware filename configured.
- Firmware copied to USB root.
- USB connected.
- Startup behavior understood.
- Upgrade performed if required.
- Auto-detection disabled after temporary use if no longer required.

Warning

Avoid leaving automatic firmware detection enabled unnecessarily after maintenance or testing.

Caution

**Firmware downgrade is a high-risk operation.**

- Operational reason documented.
- Downgrade path verified.
- Target firmware verified.
- Hardware compatibility verified.
- Configuration backup created.
- Recovery plan prepared.
- Previous firmware available.
- Console access available.
- Maintenance window approved.

`execute ping 192.168.20.200`
Then:

`execute restore image tftp a.pkg 192.168.20.200`
FortiGate may display:

```
This operation will downgrade the current firmware version!
```
- Downgrade warning reviewed.
- Configuration impact understood.
- Confirmation approved.

`get system status``execute update-now`
- Firmware version.
- Build.
- Configuration.
- Interfaces.
- Routing.
- Firewall policies.
- VPN.
- HA.
- Security Fabric.
- FortiGuard.
- Critical services.

`get system status`
- Correct FortiOS version.
- Correct build.
- Correct hardware model.
- System healthy.

- Interfaces operational.
- VLANs operational.
- Routing verified.
- Default route verified.
- DNS verified.
- Internet connectivity verified.

- Firewall policies verified.
- Security profiles verified.
- IPS operational.
- Antivirus operational.
- Web filtering operational.
- Application control operational.

- IPsec tunnels verified.
- SSL VPN verified if used.
- Authentication verified.
- VPN traffic tested.

- HA status verified.
- Primary/secondary roles verified.
- Synchronization verified.
- Session synchronization verified where applicable.
- HA monitoring verified.

- Fabric root healthy.
- Fabric members connected.
- FortiAP status verified.
- FortiSwitch status verified.
- Fabric topology healthy.

- FortiGuard connectivity verified.
- AV definitions updated.
- Attack definitions updated.
- License status verified.

- Critical services verified.
- Logs reviewed.
- Authentication tested.
- Monitoring restored.
- Alerts reviewed.

- Internal traffic tested.
- Internet traffic tested.
- Critical applications tested.
- NAT verified.
- VIPs verified.
- Routing verified.
- VPN traffic verified.

- Backup configuration.
- Verify upgrade path.
- Verify target firmware.
- Verify hardware compatibility.
- Verify firmware authenticity.
- Verify firmware integrity.
- Verify FortiGuard.
- Verify licenses.
- Verify NTP.
- Verify HA.
- Verify Fabric.
- Verify VPN.
- Verify routing.
- Verify critical services.
- Prepare rollback plan.
- Prepare console access.
- Prepare TFTP/USB recovery.
- Approve maintenance window.

- Monitor firmware transfer.
- Monitor signature verification.
- Monitor integrity verification.
- Monitor reboot.
- Do not interrupt power.
- Do not interrupt firmware transfer.
- Monitor HA state.
- Monitor Fabric state.
- Monitor management connectivity.

-  `get system status`
-  `execute update-now`
- Verify AV definitions.
- Verify attack definitions.
- Verify interfaces.
- Verify routing.
- Verify firewall policies.
- Verify NAT.
- Verify VPNs.
- Verify HA.
- Verify Fabric.
- Verify FortiGuard.
- Verify logs.
- Verify monitoring.
- Test critical applications.
- Confirm production traffic.
- Document final state.

```
Firmware Upgrade Failed
        │
        ▼
Can FortiGate Boot?
   ┌────┴────┐
  YES       NO
   │         │
   ▼         ▼
Check OS   Console
   │         │
   │         ▼
   │      Boot Menu
   │         │
   │         ▼
   │      TFTP / USB
   │         │
   └────┬────┘
        ▼
Verify Firmware
        │
        ▼
Verify Signature
        │
        ▼
Verify Integrity
        │
        ▼
Verify Upgrade Path
        │
        ▼
Recover / Retry
```
-  Run `get system status` .
- Verify firmware version.
- Verify build.
- Verify interfaces.
- Verify routing.
- Verify services.
- Verify FortiGuard.
- Verify security definitions.
- Review logs.

- Connect console.
- Interrupt boot.
- Enter boot menu.
- Verify TFTP connectivity.
- Verify firmware image.
- Verify image filename.
- Verify boot partition.
- Use recovery workflow.
- Avoid destructive formatting unless explicitly required.

| Purpose | Command | 
|---|---|
| System status | `get system status` | 
| Test connectivity | `execute ping <IP>` | 
| Update definitions | `execute update-now` | 
| Reboot | `execute reboot` | 
| TFTP upgrade | `execute restore image tftp <file> <server>` | 
| USB upgrade | `execute restore image usb <file>` | 
| Stage firmware | `execute restore secondary-image tftp <file>` | 
| Select secondary boot | `execute set-next-reboot secondary` | 
| Select primary boot | `execute set-next-reboot primary` | 
| Initialize federated upgrade | `execute federated-upgrade initialize` | 
| Federated status | `execute federated-upgrade status` | 
| Cancel federated upgrade | `execute federated-upgrade cancel` | 
| Restart federated upgrade | `execute federated-upgrade restart` | 

Important

Always verify command availability and exact behavior against the specific FortiOS build and hardware platform before production execution.

```
# Test TFTP server reachability
execute ping 192.168.20.200
# Start firmware upgrade
execute restore image tftp a.pkg 192.168.20.200
# Confirm the firmware replacement
y
# Confirm additional warning if displayed
y
# After reboot
get system status
# Update security definitions
execute update-now
```
- Ping successful.
- Firmware file exists.
- Correct model verified.
- Upgrade path verified.
- Firmware signature verified.
- Upgrade confirmed.
- FortiGate rebooted.
- New firmware verified.
- Security definitions updated.
- Production traffic tested.

```
# Stage new firmware into secondary partition
execute restore secondary-image tftp a.pkg
# Configure the partition containing the staged firmware
# to be loaded at the next reboot
execute set-next-reboot secondary
# Reboot during the maintenance window
execute reboot
```
- Firmware staged successfully.
- Secondary partition contains target firmware.
-  `set-next-reboot secondary` configured.
- Maintenance window reached.
- Reboot performed.
- New firmware loaded.
- System status verified.
- Production traffic validated.

Warning

Do not blindly use `primary` after staging an image to `secondary`. `set-next-reboot` selects the partition that will be booted next. ([Fortinet Documentation][1])

| Method | Source | Activation | Primary Use | 
|---|---|---|---|
| GUI Upgrade | FortiGuard / uploaded image | Immediate or scheduled | Normal upgrade | 
| Fabric Upgrade | FortiGuard | Immediate or scheduled | Fabric-wide upgrade | 
| Federated Upgrade | FortiGuard | Coordinated | Multi-build/Fabric upgrade | 
| TFTP Restore | TFTP | Immediate | Manual/recovery | 
| USB Restore | USB | Immediate | Offline/recovery | 
| Controlled Upgrade | Secondary partition | Next reboot | Staged activation | 
| Boot Menu TFTP | TFTP | Boot-time | Recovery/testing | 

- Console cable.
- Laptop.
- TFTP server.
- Known-good firmware.
- Previous firmware.
- USB recovery media.
- Configuration backup.
- Management IP information.
- TFTP server IP.
- Firmware filename.

```
Failed Upgrade
      │
      ▼
Console Access
      │
      ▼
Boot Menu
      │
      ├── TFTP
      │
      ├── Backup Firmware
      │
      └── USB / Alternative Recovery
      │
      ▼
Boot Known-Good Image
      │
      ▼
Verify Configuration
      │
      ▼
Verify Network
      │
      ▼
Verify Services
      │
      ▼
Restore Production State
```
- Firmware downloaded from trusted source.
- Firmware authenticity verified.
- Signature warnings investigated.
- Firmware model compatibility verified.
- Firmware file protected.
- TFTP access restricted.
- USB media protected.
- Configuration backups protected.
- Recovery images protected.

- Management access restricted.
- Console access controlled.
- Firmware upgrade permissions restricted.
- Maintenance window approved.
- Upgrade activity logged.
- Recovery procedure documented.

- OOB management available.
- Console access available.
- HA state verified.
- Fabric topology verified.
- NTP synchronized.
- FortiGuard operational.
- Licenses valid.
- Backup available.
- Previous firmware available.
- TFTP server ready.
- USB recovery ready.
- Maintenance window available.
- Rollback plan documented.

Important

**Fabric Management** is the central GUI area used for firmware management in FortiOS 7.2.x.

Important

**FortiOS 7.2.0 introduced Feature and Mature firmware maturity tags.** ([Fortinet Documentation][2])

Tip

Feature firmware is associated with newer functionality; Mature firmware does not introduce new major features and focuses on maintenance, fixes, and applicable vulnerability patches. ([Fortinet Documentation][5])

Important

`get system status` can be used to identify the FortiOS version, build, and maturity notation.

Important

A **federated update** can automatically follow multiple firmware builds in an upgrade path for supported FortiGate devices. ([Fortinet Documentation][3])

Warning

FortiAP and FortiSwitch do not follow the same multi-build upgrade path behavior as FortiGate in the FortiOS 7.2.0 federated-update workflow. ([Fortinet Documentation][3])

Important

`execute restore image tftp` replaces the current firmware and causes a reboot after installation.

Important

Firmware signature verification is a security control. Do not ignore unexpected authenticity warnings.

Important

`execute restore secondary-image` stages firmware in another partition for later activation. ([Fortinet Documentation][1])

Important

`execute set-next-reboot secondary` selects the secondary partition for the next boot. ([Fortinet Documentation][1])

Warning

`execute set-next-reboot primary` does **not** mean "boot the newly uploaded firmware." It means boot the firmware stored in the primary partition.

Important

`execute update-now` should be considered during post-upgrade security-definition validation.

Warning

A firmware downgrade can introduce configuration migration/reversion problems.

Caution

The boot-menu `f` option can format the boot device. Treat it as destructive.

```
NO BACKUP
   ↓
NO PRODUCTION UPGRADE
```
```
Current
   ↓
Supported Path
   ↓
Target
```
Never assume:

```
Current → Latest
```
is supported.

```
Source
 ↓
Model
 ↓
Build
 ↓
Signature
 ↓
Integrity
```
Controlled upgrades allow:

```
Stage
 ↓
Validate
 ↓
Schedule
 ↓
Reboot
 ↓
Activate
```
This can reduce maintenance-window pressure.

```
Reboot
 ↓
System Status
 ↓
Definitions
 ↓
Services
 ↓
HA
 ↓
Fabric
 ↓
Traffic
```
```
Firmware
+
Config Backup
+
Console
+
TFTP
+
USB
=
Recovery Capability
```
```
LAB
 ↓
PILOT
 ↓
LOW-RISK
 ↓
PRODUCTION
 ↓
CRITICAL
```
Firmware images and configuration backups are operationally sensitive assets.

- Restrict access.
- Use trusted sources.
- Protect backups.
- Protect recovery media.
- Document version provenance.

```
Fabric Management
↓
Central firmware/device management
```
```
Feature
↓
New features
↓
Gray
```
```
Mature
↓
Maintenance-oriented release
↓
Green
```
```
Upgrade Path
↓
Current
↓
Intermediate
↓
Target
```
```
TFTP Upgrade
↓
execute ping
↓
execute restore image tftp
↓
Signature Verification
↓
Integrity Verification
↓
Install
↓
Reboot
```
```
Controlled Upgrade
↓
restore secondary-image
↓
Stage Firmware
↓
set-next-reboot secondary
↓
Maintenance Window
↓
Reboot
```
```
Federated Upgrade
↓
Fabric Management
↓
Follow Upgrade Path
↓
Intermediate Builds
↓
Multiple Reboots
↓
Target Firmware
```
```
Recovery
↓
Console
↓
Boot Menu
↓
TFTP / Backup / USB
↓
Known-Good Firmware
```
```
Post Upgrade
↓
get system status
↓
execute update-now
↓
Check Services
↓
Check HA
↓
Check Fabric
↓
Check Traffic
```
```
# System
get system status
# Connectivity
execute ping <IP>
# Security definitions
execute update-now
# Reboot
execute reboot
# TFTP firmware
execute restore image tftp <file> <server>
# USB firmware
execute restore image usb <file>
# Controlled firmware staging
execute restore secondary-image tftp <file>
# Boot secondary partition next
execute set-next-reboot secondary
# Boot primary partition next
execute set-next-reboot primary
# Federated upgrade
execute federated-upgrade initialize
# Federated status
execute federated-upgrade status
# Cancel federated upgrade
execute federated-upgrade cancel
```
Before clicking **Upgrade**:

- I know the current firmware.
- I know the target firmware.
- I verified the upgrade path.
- I verified hardware compatibility.
- I verified firmware authenticity.
- I created a configuration backup.
- I have console/OOB access.
- I have a recovery firmware image.
- I have a recovery method.
- I verified HA/Fabric state.
- I verified FortiGuard.
- I verified NTP.
- I verified licensing.
- I have an approved maintenance window.
- I have a rollback plan.
- I know what I will validate after reboot.

-  `get system status`
-  `execute update-now`
- Verify security definitions.
- Verify interfaces.
- Verify routing.
- Verify policies.
- Verify NAT.
- Verify VPN.
- Verify HA.
- Verify Security Fabric.
- Verify FortiGuard.
- Verify critical applications.
- Review logs.
- Confirm production traffic.
- Document final firmware/build.

`FortiGate Firmware Upgrade` ·
`FortiOS Upgrade` ·
`FortiOS 7.2.0 Firmware Upgrade` ·
`FortiGate Upgrade Path` ·
`FortiGate Fabric Management` ·
`Fortinet Security Fabric Upgrade` ·
`FortiGate Federated Upgrade` ·
`FortiGate TFTP Upgrade` ·
`FortiGate USB Firmware Upgrade` ·
`FortiGate Controlled Upgrade` ·
`FortiOS Firmware Verification` ·
`FortiGate Firmware Signature` ·
`FortiGate Firmware Recovery` ·
`FortiGate Boot Menu` ·
`FortiGate Downgrade` ·
`FortiGuard Update` ·
`execute update-now` ·
`execute restore image tftp` ·
`execute restore secondary-image` ·
`execute set-next-reboot` ·
`FortiAP Firmware Upgrade` ·
`FortiSwitch Firmware Upgrade` ·
`FortiOS Upgrade Path` ·
`FortiGate Maintenance` ·
`FortiGate Firmware Troubleshooting`

- 
YouTube — SheynShield 
  - Fortinet NSE content
  - FortiGate troubleshooting
  - Network Security Engineering

**Never treat a firmware upgrade as a single reboot operation.**

**A professional FortiGate upgrade is a controlled lifecycle:**

`PLAN → BACKUP → VERIFY → STAGE → UPGRADE → REBOOT → UPDATE → VALIDATE → DOCUMENT`

**Engineering Secure Networks**

**SheynShield | Security & Design Knowledge Base**
