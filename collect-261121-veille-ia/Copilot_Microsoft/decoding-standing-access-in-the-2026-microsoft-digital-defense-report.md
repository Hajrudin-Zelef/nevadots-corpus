---
id: collect-261121-veille-ia/copilot-microsoft/decoding-standing-access-in-the-2026-microsoft-digital-defense-report
title: "Decoding standing access in the 2026 Microsoft Digital Defense Report"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["access", "password", "microsoft", "devolutions", "remote", "account", "checkout", "phishing"]
source: https://devolutions.net/blog/decoding-standing-access-in-the-2026-microsoft-digital-defense-report/
source_anchor: ""
source_lines: [1, 60]
section: "Copilot_Microsoft"
---
Microsoftâs red team describes a weakness it keeps exploiting: âtoo much standing access, too loosely enforced.â Most of what the team exploits in identity, it adds, âhas nothing to do with AI.â

The 2026 Microsoft Digital Defense Report covers July 2025 through June 2026. Across its findings, attackers use valid accounts and familiar administration tools to reach more systems. A compromised identity can open a remote session, expose another credential, and provide a route to higher privileges.

Those are concrete problems for the teams that manage privileged access. A password can remain useful after a maintenance window closes. An account can retain administrative rights between tasks. A remote connection can leave too little evidence to explain what happened.

The five findings below connect those problems to specific controls. Each section explains the finding and where Devolutions fits today. The final section looks at the planned 2027.1 work.

## 1. 52.2% of valid-account intrusions led to more credential theft

In Microsoft Defender Experts observations, 52.2% of intrusions that began with a valid account involved follow-on credential theft. Once inside, attackers collected more credentials and used them to expand access. An account that can retrieve another administratorâs password gives an intruder another way to move through the environment.

Devolutions PAM checkout policies can require approval, a reason or ticket, and a defined access window. In Devolutions Cloud, check-in resets the privileged account password, so the previous password no longer works. Approval limits who receives access; rotation limits how long a copied password remains useful.

## 2. AiTM-URL phishing made up about 45% of identified phishing techniques

Adversary-in-the-middle (AiTM) URL phishing accounted for about 45% of identified phishing techniques in Microsoftâs chart. Microsoft also reports that password attacks fell 26% year over year, while AiTM phishing and token theft gained share in the first half of 2026. Session theft matters because an attacker may obtain access after the legitimate user has completed the sign-in checks.

In Cloud and Devolutions Server, Devolutions PAM can require multi-factor authentication (MFA) when a privileged account is checked out. This is a separate verification from MFA at sign-in. An external identity providerâs sign-in challenge does not automatically satisfy the checkout requirement. Administrators must configure checkout MFA as its own control.

## 3. About 82% of advertised access offers involved credentials or remote-access portals

Intel 471 tracked 2,591 advertised access offers. Compromised login credentials accounted for 45%, and compromised corporate remote-access portals, including VPN devices, accounted for another 37%. Together, those rounded shares add up to about 82% of the offers. A working account or remote connection is valuable enough to sell before the next attacker begins work.

For Remote Desktop Protocol (RDP) sessions, Devolutions Gateway can broker the connection and inject credentials through a proxy, keeping the password off the userâs machine. External users can receive `Execute` permission without `View password` permission. A contractor can then use the configured session without receiving the target accountâs password.

## 4. An unsanctioned RMM agent is a reliable mid-stage intrusion signal

In its endpoint investigations, Microsoft found that an unsanctioned remote monitoring and management (RMM) agent was one of the most reliable mid-stage signs of a human-operated intrusion. These tools give attackers remote command and file-transfer functions that also appear in legitimate administration work. Investigators need a way to establish who authorized access and what happened during the session.

With Cloud, Devolutions PAM and Gateway can record supported RDP, ARD, VNC, and SSH sessions. Checkout policies can also terminate sessions at check-in. Alongside the checkout history, a recording gives reviewers evidence of the work performed. These controls support oversight of approved access; detecting an unauthorized RMM agent still requires endpoint and security monitoring.

## 5. Ransomware detections in the IT sector rose from 248 to 649

Microsoftâs enterprise ransomware table lists 248 detections for the information technology industry in 2025 and 649 in 2026, about 2.6 times as many. These figures describe the IT industry, rather than attacks on IT systems across all sectors. The increase does not establish which access controls failed, but it reinforces the need to limit the authority available to a compromised account.

Just-in-time (JIT) elevation in Devolutions PAM adds group membership at checkout and removes it at check-in. Standard accounts retain their base permissions, while zero-standing-privilege accounts receive the required privileges only for the checkout window. Administrators can complete elevated work without leaving those rights assigned between tasks.

## Whatâs next: privileged access in 2027.1

The planned 2027.1 work builds on checkout and session controls with more detailed access decisions. A workflow-based rule engine will define sign-in and checkout steps using the requester, provider, elevation group, access duration, reason, and ticket. The branch that decides the request will be logged.

WebAuthn and Duo are planned as checkout MFA methods, with the exact checkout factor model still being defined. Sign-in workflows are planned to require MFA and restrict the allowed methods, including passkeys. Devolutions Remote Desktop Manager (RDM) is also planned to use passkeys stored in the workspace when a website opens in its embedded browser.

The release direction also addresses what happens on a machine after access is granted. Luc Fauvel, Head of Product at Devolutions, describes the planned expansion into machine management:

âAs we strengthen our already solid privileged credential management and secure remote connections in 2027.1, weâre also helping administrators control what happens on the machines themselves once those connections are established. The next iteration of PEDM introduces centralized machine management, and with it, control over application installation through UniGetUI. From a single place, administrators can manage their machines and define which applications users are allowed to install on each of them.â


Luc Fauvel, Head of Product at Devolutions

PEDM stands for Privilege Elevation and Delegation Management. The planned machine-management controls extend oversight to application installation on managed machines. The 2027.1 items described here remain planned capabilities.

## The bottom line

The useful test is the full privileged-access workflow. Approval should establish the need for access. Session controls should limit credential exposure and preserve evidence of the work. Check-in should retire the credential or elevated privileges according to policy.

Microsoftâs findings give IT teams specific reasons to examine each step. The next evaluation can start with one test account and follow it from the access request through to check-in.

Download Devolutions RDM to evaluate checkout, JIT elevation, and brokered sessions with a test privileged account.
