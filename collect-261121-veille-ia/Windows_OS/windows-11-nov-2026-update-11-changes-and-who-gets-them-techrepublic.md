---
id: collect-261121-veille-ia/windows-os/windows-11-nov-2026-update-11-changes-and-who-gets-them-techrepublic
title: "Windows 11 Nov. 2026 Update: 11 Changes and Who Gets Them - TechRepublic"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-13"]
keywords: ["windows", "update", "security", "changes", "supported", "devices", "november", "fixes"]
source: https://www.techrepublic.com/article/news-windows-11-november-2026-update-features/
source_anchor: ""
source_lines: [1, 96]
section: "Windows_OS"
---
Microsoftâs November 2026 Windows 11 update brings security improvements, fixes for Remote Desktop and printing problems, and a new round of features rolling out to supported PCs. The mandatory Patch Tuesday release arrived Nov. 10, but not every device will receive the same changes immediately.

The main update, KB5132061, moves Windows 11 26H2 to build 26300.9974, 25H2 to build 26200.9974, and supported 24H2 Enterprise and Education devices to build 26100.9974.

The release also marks the end of security updates for Windows 11 23H2 Enterprise and Education. Organizations still using that version need to upgrade, while IT teams managing supported devices should prioritize security patches and test the newer features before wider deployment.

## Who gets the Windows 11 November 2026 update?

Microsoftâs November security update covers three supported Windows 11 versions.

| Windows 11 version | November update | OS build | 
|---|---|---|
| 26H2, all editions | KB5132061 | 26300.9974 | 
| 25H2, all editions | KB5132061 | 26200.9974 | 
| 24H2 Enterprise and Education | KB5132061 | 26100.9974 | 
| 23H2 Enterprise and Education | KB5131087, final security update | Separate release | 

Windows 11 24H2 Home and Pro reached their end-of-updates date on Oct. 13, 2026. Microsoft recommends upgrading those devices to a supported version.

The company also ended updates for 23H2 Enterprise and Education on Nov. 10, making the November patch their final monthly security release.

Organizations planning an upgrade can compare Windows 11 26H2 and 25H2 before deciding which supported version fits their deployment schedule.

### Whatâs new in the November update?

KB5132061 incorporates security fixes and improvements from Microsoftâs October updates, including the Oct. 28 optional preview, KB5127936.

The changes include:

1. **Redesigned context menus.** Microsoft is introducing a cleaner right-click interface with options to customize available commands and extensions.
2. **Cloud Rebuild recovery.** Supported devices can download Windows and drivers for a fresh installation without USB recovery media.
3. **Memory and performance improvements.** Microsoft is working to reduce memory consumption and improve responsiveness on lower-memory PCs.
4. **Widgets improvements.** Changes to background loading aim to reduce unnecessary resource use.
5. **Memory Integrity protections.** Eligible devices may receive updated default security settings, subject to existing user and administrator configurations.
6. **Secure Boot certificate updates.** Microsoft is expanding the number of devices eligible to receive replacement certificates.
7. **Stronger NTLM security.** Clients now offer NTLM message signing by default for non-anonymous connections, helping reduce exposure to relay attacks.
8. **PowerShell signing improvements.** Stricter encoding validation strengthens script signature verification.
9. **Remote Desktop fixes.** Microsoft has corrected a certificate mismatch that could prevent some RDP connections.
10. **Printing fixes.** The update addresses an error affecting printers added through Active Directory.
11. **Audio reliability fixes.** Microsoft has resolved problems affecting USB Audio 1.0 devices and, on supported 24H2 editions, certain AMD HDMI and DisplayPort audio configurations.

Feature availability varies. Some changes began appearing in earlier preview updates, while others are included in the November security release. Gradual rollout means an updated PC may not immediately display every interface improvement.

Microsoftâs release notes provide the authoritative list of fixes and version-specific changes.

### Cloud Rebuild and security changes deserve extra attention

Cloud Rebuild is particularly useful for devices that cannot start Windows normally. It downloads a fresh operating system and compatible drivers through the recovery environment, reducing reliance on USB installation media.

However, the recovery process removes local files, applications, accounts, and settings. IT teams should confirm backups and recovery requirements before using it.

The NTLM and PowerShell changes also warrant testing in managed environments.

NTLM signing improvements can affect authentication behavior in legacy environments, while stricter PowerShell signature validation may reject scripts using invalid UTF-8 encoding.

Organizations should identify affected scripts and authentication workflows before expanding deployment.

## What IT teams should check before rolling out KB5132061

The November update is a mandatory security release, so organizations should install it through their normal patch management process.

However, Microsoft continues to distribute some feature changes gradually, and compatibility testing remains important for business-critical systems.

### Verify update prerequisites

For Windows 11 26H2, Microsoft requires an update released on or after Sept. 22, 2026, such as KB5124010, along with Servicing Stack Update 26100.9539 or later.

Windows 11 24H2 and 25H2 have different prerequisites, including a qualifying update released on or after June 10, 2025, and SSU 26100.4193 or later.

Administrators deploying KB5132061 through Microsoft Update Catalog should follow Microsoftâs documented installation sequence for checkpoint cumulative updates.

### Test security, connectivity, and recovery

Before expanding deployment, IT teams should:

- **Check Memory Integrity settings.** Confirm existing configurations and test older drivers for compatibility with virtualization-based security.
- **Validate remote access.** Test Remote Desktop, VPN connectivity, and authentication workflows used by employees.
- **Review PowerShell scripts.** Identify signed scripts that may require UTF-8 encoding corrections and new signatures.
- **Test recovery procedures.** Confirm that Cloud Rebuild is available, backups are complete, and support teams understand its data-removal consequences.
- **Monitor installation results.** Track failed updates, application errors, and user-reported issues during the pilot phase.

Microsoft currently lists no known issues for KB5132061 in its official release notes. Individual installation failures may still occur, particularly on devices missing required servicing updates.

Administrators should consult the release-health dashboard and Microsoftâs troubleshooting guidance before attempting repeated installations.

### Prioritize unsupported Windows versions

Devices running Windows 11 23H2 Enterprise and Education have reached the end of monthly security updates. Those systems should move to a supported release as soon as practical.

Windows 11 24H2 Home and Pro devices also require an upgrade because their support ended in October.

For organizations already running 25H2 or 26H2, the immediate priority is installing Novemberâs security fixes and validating any newly enabled features.

The safest rollout is a staged one: patch a representative group of devices, verify business applications and security controls, then expand deployment while monitoring support tickets and update failures.

**Also read: Windows 11 24H2 Home and Pro support ended Oct. 13, leaving affected PCs without monthly security updates until they move to a supported version.**
