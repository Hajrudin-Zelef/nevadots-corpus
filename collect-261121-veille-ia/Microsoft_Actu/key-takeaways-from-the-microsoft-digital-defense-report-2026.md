---
id: collect-261121-veille-ia/microsoft-actu/key-takeaways-from-the-microsoft-digital-defense-report-2026
title: "Key takeaways from the Microsoft Digital Defense Report 2026"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "phishing", "report", "identity", "digital", "defense", "year", "attackers"]
source: https://blog.oceanleaf.ch/mddr-2026/
source_anchor: ""
source_lines: [1, 102]
section: "Microsoft_Actu"
---
Every autumn Microsoft publishes the Digital Defense Report. The 2026 edition is called "Decoding emergence", has 103 pages, four chapters (AI, Threat Landscape, Cybercrime, Resilience) and covers July 2025 to June 2026. I read the whole thing, so you don't have to. Like two years ago, this post is your TL;DR ð

The one sentence I took away: **attackers don't break in anymore, they log in.** Identity, supply chain, edge devices, cloud services and now AI agents are trust relationships, and risk travels along them. AI makes all of this faster and cheaper. However, it does not make it fundamentally different.

**TL;DR**

- **AI** compresses the time from vulnerability to exploit to under a day
- **Identity** is the control plane: over 99% of observed cloud intrusion attempts start with password spray, followed by AiTM & Device Code Phishing
- **ClickFix** = social engineering campaign that tricks users into copying and running malicious commands into their computer's terminal by disguising the prompts as fake CAPTCHAs, browser error fixes, or security verifications, grew 8x in four months, vishing (Voice + Phishing) in Teams is up 502%
- **Phishing** no longer delivers malware, it steals sessions: 89 to 96% of malicious attachments lead to credential phishing
- **Agents** need their own identity, a human sponsor and least privilege from day one

Microsoft opens the report with the claim that AI is changing the physics of cybersecurity. The numbers back it up:

- The median time from discovering a vulnerability to weaponizing it is now **under 24 hours**
- Microsoft expects a record of around **72,000 CVEs** for 2026
- In a lab, frontier models (the report names Mythos and GPT-5.5) orchestrated a **32-step attack chain** up to full domain compromise without any human help
- In July 2026, **JADEPUFFER** was the first documented fully automated ransomware extortion in the wild

The part I would pin to the wall is the Red Team chapter: the most damaging intrusions Microsoft's own red team models rarely depend on anything new, but on a fundamental that was missing, misconfigured or bypassable. A leaked version number used to be low severity. With AI, that leak plus a public CVE becomes a working exploit in minutes. Their verdict: "Being able to patch is the most underrated fundamental."

Microsoft's incident response team (DART) still sees identity compromise as the leading threat. The pattern is always the same: compromise a human identity, discover non-human credentials (service principals, API keys), escalate, exfiltrate. What changed is scale, speed and automation.

- **Password spray** accounts for over**99%** of observed cloud initial access attempts, 124 million alerts in 180 days
- Password attacks overall are **down 26%** year over year. MFA and passkeys work
- The share of **AiTM phishing and token theft more than doubled** in the first half of 2026. Attackers go where MFA doesn't reach: the session
- **Device code phishing** went from niche to preferred method, including abuse of legitimate PaaS providers to host the lures

Looking at Microsoft Defender Experts cases, user execution is the number one initial access technique (30%), followed by valid accounts (20%), malicious copy and paste (13%) and phishing (11%).

- **ClickFix** hit more than**1.1 million devices** between February and May 2026, roughly**8x** .. ClickFix builders are sold as malware-as-a-service, and a "FileFix" variant uses the Explorer address bar instead of the Run dialog
- **Vishing in Teams** exploded: confirmed weekly volume**+502%** year over year. Over 90% of Teams attackers use voice, 48% of the calls last longer than 20 seconds. Compare that to 30 to 40% engagement for targeted email phishing. After the call comes an RMM tool and reconnaissance within two minutes
- **Impersonation** is in about**85%** of reported phishing. The shift is from brand mimicry to context mimicry: a quarter of attacks imitate the target's own organization, and 77% of those pretend to be HR, payroll or benefits

Microsoft introduces a new term: **Business Contact Impersonation (BCI)**. It describes social engineering where the attacker pretends to be a trusted contact (CEO, colleague, vendor).

- Over **46 million** BCI attacks in twelve months, with a spike of**+121%** in April 2026
- The first email asks for nothing. "Are you at your desk?" or "Do you have a moment?" Explicit requests in the first contact dropped from 17% in October 2025 to **3%** in June 2026
- **Payroll diversion** attacks peak on Monday and Tuesday (51%), exactly when HR processes last week's requests

The role of the malicious attachment has flipped. Between 89 and 96% of malicious attachments per month lead to credential phishing pages instead of delivering malware. Microsoft puts it bluntly: the attachment is no longer the weapon, it is a vehicle that takes the user to the point of compromise.

- **PDF** is the dominant vehicle now, 79% of QR code phishing by April 2026, HTML fluctuates with campaigns and DOCX is fading
- **QR code phishing** : over**145 million** attacks, all-time high in March 2026
- **CAPTCHA-gated phishing** : over**100 million** attacks. The CAPTCHA keeps scanners out while the user sees a routine check
- **AiTM** sits behind 45% of phishing URLs, and 87.7% of phishing intrusions target credentials or session cookies
- **Tycoon2FA** , the phishing-as-a-service platform behind 15 million malicious emails per month, lost**95%** of its activity after the takedown by Microsoft and Europol in March 2026

Attackers need about five days from disclosure to exploitation. Enterprises need 30 to 60. And yet the most exploited vulnerabilities in Microsoft's telemetry are years old:

- **CVE-2020-1472** (Netlogon) 58%,**CVE-2022-22954** (VMware Workspace ONE) 17%,**CVE-2021-40444** (MSHTML) 11%
- Storm-1175 went from web exploit to Medusa ransomware in **24 hours** and exploited SAP NetWeaver one day after disclosure
- Akira hit more than 50 organizations through the SonicWall SSL VPN vulnerability CVE-2024-40766

Firewalls, VPN gateways and routers are no longer just the way in, they are the target. They authenticate users, manage sessions and integrate with cloud identity, which makes them control points. Attackers exploit pre-authentication vulnerabilities and then persist inside the device with configuration changes, hidden accounts or firmware implants that survive a standard remediation. The report names Fortinet, Cisco, Ivanti, Citrix, Palo Alto Networks and Juniper.

The second front is the software supply chain:

- Attacks on developer ecosystems (npm, PyPI, Crates.io, GitHub Actions) happened roughly **every ten days** , accelerating in Q2 2026
- **Shai-Hulud 2.0** affected 25,000 repositories,**Megalodon** backdoored 5,561 repositories in under six hours,**Axios** reached over 100 million weekly downloads while under attacker control
- **s1ngularity** : trojanized Nx packages hijacked Claude Code, Gemini CLI and Amazon Q to hunt for secrets on developer machines
- New words to learn: **slopsquatting** (malicious packages with names that AI tends to hallucinate) and exposed, vibe-coded MCP servers without authentication. Microsoft's red team calls the developer environment the new perimeter

Attackers rely less on custom malware and more on your own tools, cloud services and admin functions. That is why dwell time is going up.

- An **unsanctioned RMM agent** is the most reliable mid-stage indicator of a human-operated intrusion. It appears minutes after the foothold, auto-starts at logon and replaces a dozen separate tools. Most follow-on activity lands on domain controllers
- In the cloud, attackers abuse **Azure Blob Storage** (SAS tokens, shared keys),**Graph API** ,**mailbox rules** and**Power Automate** to collect and exfiltrate without triggering anything
- Hybrid attacks persist on **four layers** at once: endpoint, infrastructure, identity and cloud. Clean up one layer and they come back through another

This is the chapter that will age fastest, and the one with the most practical advice.

- **88%** of enterprises are experimenting with agents, 82% of leaders plan broader rollouts within 12 to 18 months, and the industry expects**1.3 billion agents** in production by 2028
- Observed attacks on AI workloads: **malicious link injection 52%** , jailbreak 16%, tool and agent abuse 12%, identity and credential abuse 10%

Two concepts from the report are worth knowing. **Memory poisoning**: whoever can place content an agent reads (email is the main path) can write to its memory. That content comes back in later sessions as trusted context, even across agent handoffs. And **over-reliance on human-in-the-loop**: when the same manipulable system decides whether to ask you, "ask the user" is not a safety net. The report describes act-then-ask inversions, chaining of harmless actions and plain consent fatigue.

Also noteworthy: AI browsers are used daily in over 40% of organizations and were the infection vector for 57 malware families in May 2026, and a browser extension harvested ChatGPT and DeepSeek conversations from almost 10,000 organizations.

Globally, ransomware events detected by Microsoft Defender were roughly flat (8,749 to 8,521), while ransom detonations against enterprises grew 15.8%. Regionally the picture is very different:

- **Switzerland** : 29 to 59 cases,**+103%**
- **Germany** : 59 to 222 cases,**+276%** , by far the strongest growth in Europe
- Italy +181%, UK +66%, United States +50%
- By industry, IT jumped from 248 to 649 cases, critical manufacturing from 255 to 429

Microsoft discourages ransom payments but warns against blanket bans, because they punish victims twice and reduce reporting.

- **Nation-state activity** : 20 cases, rank 8 in Europe behind the UK (76), Germany (45), Ukraine (42), France, Italy, Spain and Belgium
- Globally, the United States takes 25.5% of all impacted customers, Israel 7.6%, Germany 1.7%. Switzerland is not in the top 12

- **165 trillion** security signals per day (2024: 78 trillion)
- **31 million** identity risk detections per day
- **5.2 billion** emails scanned per day, around 4.7 million new malware blocks per day
- 15,000+ partners and 35,000 full-time security engineers

1. **Identity** : phishing-resistant MFA for everyone, device-bound passkeys for admins, block the device code flow
2. **Patching** : a 72-hour rule for internet-facing and identity systems, isolate the rest
3. **People** : Awareness training, enforce an RMM allowlist, attack surface reduction rules in block mode
4. **Agents** : an inventory with identity, sponsor, least privilege and tested revocation
5. **Measuring** : exposure reduced, detection coverage and time-to-mitigate instead of counting alerts

The full report is available at microsoft.com/mddr
