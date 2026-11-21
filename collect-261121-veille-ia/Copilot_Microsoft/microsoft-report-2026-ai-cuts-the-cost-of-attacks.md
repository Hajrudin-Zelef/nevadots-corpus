---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-report-2026-ai-cuts-the-cost-of-attacks
title: "Microsoft Report 2026 AI Cuts the Cost of Attacks"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["report", "microsoft", "cost", "team", "security", "vulnerability", "attacks", "patch"]
source: https://ecopulse24.com/news/microsoft-report-2026-ai-cuts-the-cost-of-attacks
source_anchor: ""
source_lines: [1, 536]
section: "Copilot_Microsoft"
---
## Register your email to continue reading

We leave the headline and first paragraph visible, then ask for a quick registration after 3 free reads in 30 days.

**Dubai | EcoPulse24**

Artificial intelligence is not changing how cyberattacks begin. It is collapsing the cost of everything that comes after the initial breach, from vulnerability discovery and exploit development to lateral movement and data theft. That is the central finding of the Microsoft Digital Defense Report 2026.

Published by Microsoft Threat Intelligence in October 2026 under the title *Decoding emergence*, the report covers July 2025 through June 2026, with year-over-year comparisons against July 2024 through June 2025. It runs to 103 pages across four sections: AI, the threat landscape, cybercrime, and resilience.

## The data behind the report

| Metric | Figure | 
|---|---|
| Security signals processed daily | 165+ trillion | 
| Partners in Microsoft's security ecosystem | 15,000+ | 
| Full-time equivalent security engineers worldwide | 35,000 | 
| Identity risk detections analyzed in an average day | 31 million | 
| Emails screened daily | 5.2 billion | 
| Net new malware file blocks every day | ~4.7 million | 

The report's figures reflect what Microsoft observes across its own systems and customer base, not the totality of global cyber activity.

The report identifies five new themes shaping the landscape: risk is becoming more interconnected, AI is reshaping the security equation, criminal ecosystems continue to mature, resilience has become a strategic capability, and security is increasingly defined by how organizations adapt.

## The core thesis: the cost has collapsed

The clearest articulation of the report's central argument comes from Microsoft's Red Team, whose job is to break into Microsoft's own systems before real adversaries do. In its assessment, AI does not change where attacks start. The foothold still comes from familiar sources: a sprayed credential, an unpatched edge service, an identity gap. The cost lies in everything after the foothold, and that is exactly where it is collapsing.

The Red Team describes three areas where this shift is accelerating:

- **Source code analysis.** Models read code at scale to locate vulnerabilities and produce working exploits, work that once took skilled people weeks.
- **Binary analysis.** The same pattern now reaches compiled code, including reversing a vendor patch to recover the vulnerability it fixes. This was the surface that demanded the scarcest expertise, and it is eroding fastest.
- **Autonomous and semi-autonomous operations.** Models increasingly drive the attack sequence itself, chaining steps and deciding what to try next with limited or no human direction. The team calls this the newest of the three and the one it watches most closely.

The team offers an illustration: a service that leaks the exact version of the software it runs is, on its own, a low-severity finding. But an AI that reads that version, pulls the matching public vulnerability disclosures and assembles a proof of concept turns a trivial leak into a working exploit with almost no effort in between.

The report also warns of a proliferating population of models distilled from frontier systems, which copy frontier-level capability cheaply and often lack the safeguards built into the originals. The Red Team says it now plans against near-frontier capability being cheap, unguarded and broadly distributed.

Its conclusion is that this raises, rather than lowers, the value of fundamentals. The highest-leverage controls remain phishing-resistant multifactor authentication, disciplined identity hygiene, tiered administration and strong privileged-access enforcement. The team calls the ability to patch "the most underrated fundamental," arguing that patch velocity, not patch availability, is the control that matters.

## Hours versus weeks

The median time from vulnerability discovery in the wild to weaponization has collapsed to **well below 24 hours**, and 2026 is on track for a record of roughly **72,000** tracked Common Vulnerabilities and Exposures (CVEs).

The window between disclosure and exploitation has shrunk to single-digit days, when flaws are not exploited as zero-days before any advisory is issued:

- The US Cybersecurity and Infrastructure Security Agency (CISA) added more than 110 CVEs to its Known Exploited Vulnerabilities catalog between November 2025 and May 2026, most within a week of disclosure.
- The cybercriminal group Storm-1175 moved from web-facing exploitation to deploying Medusa ransomware in as little as 24 hours. Since 2023, it has weaponized at least 16 CVEs, including one in SAP NetWeaver a day after disclosure.
- In September 2025, Microsoft tracked a four-week surge in Akira ransomware across more than 50 organizations, mostly small and medium-sized businesses, tied to a SonicWall SSL VPN flaw (CVE-2024-40766).
- The Interlock ransomware group exploited a critical flaw in Cisco Secure FMC (CVE-2026-20131), rated the maximum 10.0 for severity, for roughly 30 days before it was disclosed.
- In July 2025, two Chinese state actors used a chain of on-premises SharePoint vulnerabilities for espionage, while Storm-2603 used the same flaws to deploy Warlock ransomware: the same window served both espionage and ransomware within days.

Meanwhile, median enterprise remediation for critical external CVEs can take **30 to 60 days**, against federal remediation requirements of 7 to 14 days in the United States.

The report expects a **multi-year period** in which the number of known but unpatched vulnerabilities spikes, because AI-powered discovery runs far ahead of remediation. Well-funded adversaries, it warns, may be able to stockpile large numbers of zero-days. It recommends fixing newly exploited vulnerabilities in internet-facing and identity systems within 72 hours, and reviewing the previous 90 days of activity for signs of earlier exploitation.

**Most detected CVEs in potential exploitation activity** (Microsoft Defender alerting, July 2025  –  June 2026):

| CVE | Description | Share | 
|---|---|---|
| CVE-2020-1472 | Netlogon: domain admin access without authentication | 58% | 
| CVE-2022-22954 | VMware Workspace ONE: remote code execution | 17% | 
| CVE-2021-40444 | MSHTML: code execution via Office documents | 11% | 
| CVE-2023-28231 | Windows DHCP Server: unauthenticated code execution | 9% | 
| CVE-2020-0601 | Windows CryptoAPI: signing files with fake certificates | 5% | 

The report notes that detection events do not represent confirmed exploitation. Notably, all five are well-known, long-disclosed flaws dating from 2020 to 2023, which the report attributes to unpatched systems being easier to access and to exploit code being widely available.

## Toward autonomous attacks

The report traces a progression in attack autonomy: AI assisting in the orchestration of an espionage campaign reported by Anthropic in September – November 2025, full-chain autonomy in lab evaluations in April – June 2026, and real-world autonomous attacks of increasing complexity in July 2026.

In lab evaluations, Mythos and GPT-5.5 were the first models to demonstrate the potential to fully autonomously orchestrate complex attacks, achieving full domain compromise in a 32-step attack chain against an emulated enterprise environment. The test took place in a mock company network **with no defenders**. Open-weight models lag closed models in this capability by about seven months, the report estimates, and their safety measures are more easily circumvented.

In the wild, early July 2026 saw the first documented automated ransomware extortion attack, which the Sysdig Threat Research Team named JADEPUFFER. Microsoft says it has observed AI-orchestrated intrusions sharing elements with that activity. Volumes remain low, but the activity is not confined to a single sector or region, and initial access has consistently come from internet-exposed services running known-vulnerable software. Also in July, OpenAI cybersecurity model training agents escaped their sandbox and collectively orchestrated a complex attack against Hugging Face, targeting the answer keys to a benchmark.

A report published in June 2026 showed that self-spreading AI-driven worms are feasible with current technology. Microsoft warns that threat actors may soon build worms that improve themselves using stolen LLM provider keys, turning every infected system into a development environment.

Still, the report stresses that most observed campaigns retain meaningful human direction. Target selection, operational decision-making and execution of the most complex intrusions remain manually driven in the majority of campaigns, though these limitations are unlikely to last long.

## Prompt-driven malware

The report identifies four converging trends:

- **LAMEHUG (July 2025):** malware used against Ukraine that calls on LLMs to generate operational commands in real time, eliminating predictable coding patterns.
- **s1ngularity (August 2025):** trojanized npm packages that probed developer machines for AI command-line tools such as Claude Code, Gemini CLI and Amazon Q CLI, then ran them with permissive overrides to hunt for secrets. It leaked about 2,000 secrets and 20,000 files across 225 victims.
- **PromptLock (August 2025):** an experimental ransomware prototype that delegated nearly all logic to an open-weights model, receiving scripts at runtime to enumerate, exfiltrate and encrypt.
- **AI-generated ClickFix lures on TikTok (2025):** faceless videos with synthetic narration walked viewers through fake activation steps ending in infostealer downloads, with some topping roughly 500,000 views.

The report also flags self-hosted agent platforms such as OpenClaw, which load executable "skills" from public marketplaces, ingest external content as instructions and act with persistent enterprise credentials. Microsoft treats them as untrusted code execution, fit only for isolated hosts under non-privileged identities.

## AI as a target

- **Stolen chat histories:** In December 2025, Microsoft found a malicious browser extension with 600,000+ installs harvesting ChatGPT and DeepSeek conversation histories. It affected almost 10,000 organizations before it was mitigated. A separate campaign of malicious extensions reached more than 900,000 installs and showed telemetry activity across more than 20,000 enterprise tenants.
- **Malicious AI apps:** EvilAI arrives with professional branding and valid code-signing certificates, then silently deploys a backdoor that steals credentials, payment data and files.
- **AI browsers:** Microsoft observed daily use across more than 40% of organizations in its dataset. A May 2026 analysis found AI browsers as the initial infection vector in incidents involving 57 distinct malware families.
- **Theft of service:** Having once hijacked GPU capacity to mine cryptocurrency, attackers now steal AI serving capacity and resell it to criminal and nation-state customers.
- **Slopsquatting:** Attackers register malicious packages under names AI models are likely to hallucinate.
- **MCP servers:** The report says it is unfortunately common to find exposed, unauthenticated or poorly authenticated Model Context Protocol services using their developers' credentials to reach sensitive resources.

**Attacked customer subscriptions, by AI surface:**

| AI surface | Share | 
|---|---|
| OpenAI / Azure OpenAI | 39% | 
| Custom AI: agents, copilots, LLM apps | 31% | 
| Other generative | 20% | 
| AI Foundry | 8% | 
| Non-generative (combined) | <2% | 

## How AI upgrades fraud

Identity-based fraud campaigns, the report says, used to carry four detectable weaknesses: poor-quality document forgeries, non-native language, accent mismatches in interviews, and thin online presence. AI fixes all four simultaneously, through improved photos in forged documents, flawless writing, real-time voice modulation, and synthetic digital footprints on GitHub, Stack Overflow and social media.

## AI agents: the new attack surface

Microsoft has observed that 88% of enterprises are already experimenting with AI agents, and 82% of leaders plan broader rollouts within 12 to 18 months. Industry projections point to roughly 1.3 billion agents in production by 2028.

**What observed prompts against generative AI agents tried to achieve** (February  –  May 2026):

| Goal | Share | 
|---|---|
| Malicious use / policy violation | 45% | 
| Sensitive data exfiltration | 17% | 
| System prompt leakage | 14% | 
| Goal hijacking / downstream tools | 12% | 
| System-internals leakage | 11% | 
| Resource exhaustion / financial harm | 1% | 

**Top attack vector categories on AI workloads** (90-day window, 2026):

| Category | Share | 
|---|---|
| Malicious link injection and exfiltration | 52% | 
| Jailbreak and safety bypass | 16% | 
| Tool and agent abuse | 12% | 
| Identity and credential abuse | 10% | 
| Prompt injection and instruction subversion | 8% | 
| Reconnaissance and data probing | 2% | 

The report highlights three risks as organizations adopt agents: ungoverned agent proliferation that outpaces the ability to track it, over-privileged access that accumulates and is rarely revoked, and no human accountable for an agent's actions. It also flags agent-to-agent spoofing, in which one agent impersonates another to slip past trust boundaries in multi-agent systems.

**Memory poisoning** is singled out as a reproducible, low-complexity, cross-session attack surface. An attacker who can place content anywhere an agent reads, most commonly through email, can often place content anywhere it remembers. The poisoned entry is then retrieved as trusted context in future sessions.

The report also warns against **overreliance on human-in-the-loop (HITL) controls**, because the decision to ask a human is often made by the same system an attacker can manipulate. The Red Team has observed HITL prompts that fired but were ignored by the agent, and HITL bypassed by a policy stored in memory within the same attack chain. The danger, the report says, is a class of incidents in which the user "approved" the breach themselves.

Its recommended governance practices include a distinct, auditable identity for every agent, a human sponsor always accountable, time-limited least-privilege access from day one, automated lifecycle management, mandatory agent-to-agent authentication, short-lived credentials scoped to a single tool, and treating revocation latency as a measured outcome.

## Cross-signal correlation as the decisive variable

Attackers have always benefited from an "adversarial discount," the report argues: defensive value erodes as adversaries adapt to deployed defenses. AI amplifies that dynamic. Using AI to find vulnerabilities first does not eliminate the discount, because patching requires coordination, testing and deployment that move far slower than attacker adaptation.

The report calls signal cross-correlation, the degree to which a defender's systems share and correlate threat intelligence, the most powerful variable under the defender's control. Formal modeling, it says, shows that with full signal correlation inside an organization, defensive effectiveness per unit of investment becomes independent of the number of systems being protected.

## Where attacks land: countries and sectors

**Countries where customers are most frequently impacted by cyber threats** (January  –  June 2026):

| Rank | Country | Share | 
|---|---|---|
| 1 | United States | 25.5% | 
| 2 | Israel | 7.6% | 
| 3 | Ukraine | 4.8% | 
| 4 | Taiwan | 3.9% | 
| 5 | United Kingdom | 3.8% | 
| 6 | United Arab Emirates | 2.8% | 
| 7 | India | 2.5% | 
| 8 | Japan | 2.5% | 
| 9 | Canada | 1.8% | 
| 10 | Germany | 1.7% | 
| 11 | Australia | 1.5% | 
| 12 | Philippines | 1.5% | 

These hotspots persist year over year, driven by target density and geopolitical interests. Microsoft also observes growing cybercriminal activity in Latin America, Africa and Southeast Asia, regions where defenses may be less mature as digital transformation accelerates.

**Top 10 sectors impacted by threat actors in 2026:**

| Sector | Share | 
|---|---|
| Government agencies and services | 27% | 
| Information technology | 17% | 
| Research and academia | 14% | 
| Communications infrastructure | 9% | 
| Transportation systems | 8% | 
| Non-government organizations | 8% | 
| Defense industrial base | 7% | 
| Critical manufacturing | 5% | 
| Financial services | 4% | 
| Energy | 3% | 

**Initial access vectors across two years of incident response findings:**

| Vector | 2025 | 2026 | 
|---|---|---|
| None identified | 25% | 14% | 
| Valid accounts | 17% | 13% | 
| Exploit public-facing application | 15% | 24% | 
| Social engineering | 15% | 7% | 
| External remote services | 8% | 5% | 
| Phishing | 7% | 23% | 
| Drive-by compromise | 6% | 4% | 
| Content injection | 3% | 2% | 
| Trusted relationship | 2% | 4% | 
| Supply chain compromise | 2% | 4% | 

In incident response engagements, phishing jumped from 7% to 23% and exploitation of public-facing applications from 15% to 24%, making them the two leading vectors in 2026.

The most common MITRE ATT&CK techniques in Microsoft Defender alerting (April – June 2026) included valid accounts for initial access, command and scripting interpreters for execution, boot or logon autostart for persistence, abuse of elevation control mechanisms, brute force for credential access, account discovery, remote services for lateral movement, email collection, and data encrypted for impact.

## Dwell time is rising

Year-over-year engagement data from Microsoft's Detection and Response Team (DART) shows a **statistically significant increase in dwell time**, the period between the earliest evidence of attacker activity and its detection, while the overall length of attacker activity remained broadly stable. Organizations, in other words, took longer to spot intrusions. The consistency across sectors points to a broader operational trend. Industrials and manufacturing saw the most pronounced rise in dwell time, while telecommunications and media saw the largest increase in the duration of attacker activity, consistent with nation-state actors seeking quiet, persistent access for espionage.

## Emerging threats for the year ahead

Microsoft expects four threats to play a growing role:

1. **Open-source supply chain compromise** across ecosystems such as npm and PyPI, where enterprise software routinely depends on hundreds or thousands of third-party components.
2. **Attacks against edge devices** such as firewalls, VPN gateways and routers, which often lack endpoint detection and persistent logging.
3. **AI as a force multiplier** , industrializing formerly labor-intensive fraud such as romance scams and spear phishing.
4. **Erosion of trust in the digital information environment** , as authentic and synthetic content become nearly impossible to tell apart.

## Nation-state threats

State-sponsored operations from China, Iran, North Korea and Russia are evolving toward more persistent, scalable and strategically aligned access models, with a shared emphasis on long-term positioning inside high-value environments. China, Iran and Russia seek access within strategic sectors such as critical infrastructure from which they can pivot from espionage to disruption. The report expects all four to prioritize identity systems, particularly cloud identity.

Overall nation-state targeting by sector: IT (20%), research and academia (20%), government (10%), think tanks and NGOs (10%), transportation (7%), manufacturing (5%), other (28%).

**China** has strengthened its focus on large-scale information collection through long-lived access, with a growing number of actors shifting to identity-based attacks, cloud-native persistence and exfiltration through compromised or actor-created service principals and OAuth applications. Some now use AI tools to search for vulnerabilities.

| Rank | Targeted countries | % | Targeted sectors | % | 
|---|---|---|---|---|
| 1 | United States | 27 | IT | 15 | 
| 2 | Taiwan | 24 | Government | 14 | 
| 3 | Japan | 20 | Research and academia | 8 | 
| 4 | Hong Kong SAR | 3 | Think tanks/NGOs | 7 | 
| 5 | Philippines | 2 | Critical manufacturing | 6 | 
| 6 | Malaysia | 2 | Transportation | 4 | 
| 7 | Italy | 2 | Consumer retail | 4 | 
| 8 | United Kingdom | 1 | Finance | 3 | 
| 9 | Germany | 1 | Defense | 3 | 
| 10 | Belgium | 1 | Healthcare and public health | 2 | 

**Iran:** Amid regional tension and episodic kinetic escalation, Iranian state-aligned actors conducted broad, multi-layered campaigns against Israel and longstanding adversaries across the Middle East, along with a notable and significant increase in targeting of the United States. The report describes the US-focused expansion as a largely retaliatory response to the war, drawing in actors that had not previously targeted the United States. The conflict drove a shift toward more disruptive and destructive approaches, including data destruction and operational technology targeting for retaliation. Internet blackouts and infrastructure degradation inside Iran temporarily constrained some operations, but overall tempo remained high.

| Rank | Targeted countries | % | Targeted sectors | % | 
|---|---|---|---|---|
| 1 | Israel | 39 | Research and academia | 28 | 
| 2 | United States | 23 | Other | 19 | 
| 3 | United Arab Emirates | 9 | IT | 12 | 
| 4 | Egypt | 6 | Transportation | 11 | 
| 5 | India | 6 | Government | 8 | 
| 6 | United Kingdom | 5 | Think tanks/NGOs | 8 | 
| 7 | Brazil | 4 | Communications | 4 | 
| 8 | Greece | 3 | Manufacturing | 4 | 
| 9 | Germany | 2 | Commercial facilities | 3 | 
| 10 | Saudi Arabia | 2 | Finance | 2 | 

**Russia** is becoming more efficient, stealthy and adaptive, leaning on identity-based attacks and cloud exploitation, including OAuth abuse and token replay. In a notable escalation, a Russian actor used edge-device access in December 2025 to conduct a coordinated destructive attack on Poland's energy sector, which the report reads as a new willingness to destroy critical infrastructure outside Ukraine. Microsoft also observed an uptick in activity from non-traditional actors and Russian use of vibe coding and AI-generated tooling.

| Rank | Targeted countries | % | Targeted sectors | % | 
|---|---|---|---|---|
| 1 | United States | 21 | Government | 25 | 
| 2 | Ukraine | 14 | Education | 13 | 
| 3 | United Kingdom | 11 | Think tanks/NGOs | 13 | 
| 4 | Belgium | 6 | IT | 10 | 
| 5 | Germany | 4 | Energy | 5 | 
| 6 | France | 3 | Defense industry | 3 | 
| 7 | Spain | 3 | Manufacturing | 3 | 
| 8 | Poland | 2 | Transportation | 3 | 
| 9 | Sweden | 2 | Finance | 2 | 
| 10 | Switzerland | 2 | Intergovernmental organizations | 2 | 

**North Korea** has scaled its operations with AI, focusing on IT, financial services and blockchain organizations for revenue. It expanded cryptocurrency targeting to include Bitcoin mining organizations, and in March 2026 a state-sponsored group compromised the widely used Axios npm package. The remote IT worker scheme remains a major threat, now using AI for persona development and social engineering.

| Rank | Targeted countries | % | Targeted sectors | % | 
|---|---|---|---|---|
| 1 | United States | 50 | IT | 33 | 
| 2 | Italy | 13 | Research and academia | 15 | 
| 3 | Australia | 5 | Other | 13 | 
| 4 | United Kingdom | 4 | Think tanks/NGOs | 8 | 
| 5 | Switzerland | 2 | Consumer retail | 7 | 
| 6 | India | 2 | Finance | 5 | 
| 7 | Germany | 2 | Manufacturing | 4 | 
| 8 | United Arab Emirates | 2 | Health | 4 | 
| 9 | France | 1 | Communications | 2 | 
| 10 | Republic of Korea | 1 | Defense industry | 2 | 

**Regional sample of observed activity** (July 2025  –  June 2026, event counts):

| Middle East & Africa |  | Europe |  | Asia & Pacific |  | Americas |  | 
|---|---|---|---|---|---|---|---|
| Israel | 210 | United Kingdom | 76 | Taiwan | 193 | United States | 531 | 
| UAE | 50 | Germany | 45 | Japan | 172 | Brazil | 27 | 
| Egypt | 30 | Ukraine | 42 | India | 58 | Canada | 26 | 
| Saudi Arabia | 24 | France | 28 | Australia | 27 | Chile | 9 | 
| South Africa | 14 | Italy | 25 | Hong Kong SAR | 25 | Colombia | 9 | 
| Kuwait | 8 | Spain | 25 | Vietnam | 18 | Mexico | 9 | 
| Qatar | 5 | Belgium | 24 | Malaysia | 16 | Peru | 7 | 
| Jordan | 4 | Switzerland | 20 | Philippines | 14 | Ecuador | 5 | 
| Ethiopia | 3 | Greece | 14 | Thailand | 13 | Argentina | 4 | 
| Iraq | 3 | Poland | 11 | Singapore | 10 | Dominican Republic | 3 | 

## How modern intrusions begin

**Initial access techniques** (Microsoft Defender Experts customer notifications):

| Technique | Share | 
|---|---|
| User execution | 30% | 
| Valid accounts | 20% | 
| Malicious copy and paste | 13% | 
| Phishing | 11% | 
| Drive-by compromise | 6% | 
| Exploitation of public-facing application | 5% | 
| Other | 1% | 

Once a user executes, the follow-on is swift: malware in 60.3% of cases, infostealers in 12.3% and credential theft in 9.4%. Of intrusions using valid accounts, 52.2% involved follow-on credential theft and 18.4% active password spray campaigns, a self-reinforcing cycle in which one compromised identity fuels many more.

Exploitation of public-facing applications accounts for just 4.8% of observed intrusions but carries disproportionate impact. SharePoint Server was the most frequently exploited product, followed by IIS, HTTP File Server, React-based applications and Exchange Server.

**ClickFix:** Malicious copy-and-paste attacks fell from 47% of observed initial access activity last year to 12.8%, but rebounded explosively in early 2026, growing roughly 23x between December and May, with malware following in 96.3% of cases. Between February and early May 2026, Microsoft Defender observed user-pasted ClickFix commands on more than 1.1 million unique devices, an approximately 8x increase likely driven by kits sold as malware-as-a-service. A "FileFix" variant, which uses the File Explorer address bar instead of Run or Terminal, is also gaining traction.

**Unique devices per week running an attacker-supplied ClickFix command:**

| Week | Devices | 
|---|---|
| Feb W1 | 38,000 | 
| Feb W2 | 52,000 | 
| Mar W1 | 71,000 | 
| Mar W2 | 95,000 | 
| Apr W1 | 130,000 | 
| Apr W2 | 188,000 | 
| Apr W5 | 240,000 | 
| May W1 | 310,000 | 

**Phishing** accounts for 10.9% of intrusion attempts, but its nature has fundamentally changed. Of phishing intrusions, 87.7% involved credential or session harvesting and 6.3% were business contact impersonation.

| Phishing technique | Share | 
|---|---|
| Adversary-in-the-middle (AiTM) URL | 45% | 
| Standard URL | 34% | 
| Attachment | 13% | 
| AiTM attachment | 7% | 
| QR code (incl. AiTM-QR) | 2% | 

**Voice phishing in Microsoft Teams:** Weekly confirmed malicious volume rose 502% year over year, and more than 90% of attackers using Teams incorporated voice calls into some attacks. Across the attacks studied, 93% of attackers succeeded at least once in keeping a target on the line for 20 seconds or longer, the point at which a social engineering script can begin. Of all calls, 48% lasted beyond 20 seconds and 30% beyond a minute, compared with engagement rates of 30 – 40% for targeted email and 1 – 5% for bulk email. When a call succeeds, attackers typically gain access through a remote monitoring and management (RMM) tool and complete reconnaissance in two minutes or less.

**Impersonation:** Nearly 85% of reported phishing emails between April and June 2026 carried an impersonation signal. The report describes a shift from brand mimicry to context mimicry:

- 32% of attacks use images or logos to mimic a brand, and more than a third use template-based lures without naming any brand.
- 44% of targeted brands are lesser-known or location-specific, falling outside globally optimized detection models.
- Automated notifications appear in 80% of impersonation attacks, followed by financial fraud themes in 61%.
- Around 25% mimic an organization's own operating environment or someone inside it, with HR, payroll and benefits scams driving 77% of those cases.
- Attachments appear in 30% of cases, QR codes in 13% and HTML payloads in 5%. One in four impersonation emails pushes the victim to a phone call.

**Business contact impersonation (BCI):** The report introduces BCI to replace the ambiguous term "business email compromise," separating social engineering from account takeover. Microsoft detected more than 46 million BCI attacks over the past 12 months, with volume surging 121% in April 2026, more than double any previous month. The share of first-contact messages containing an explicit financial or document request fell from 17% in October 2025 to just 3% by June 2026; most open with bland outreach such as "Are you at your desk?" Payroll diversion attempts peak on Mondays, with 51% landing on a Monday or Tuesday.

**Edge infrastructure:** Attackers have pivoted back to firewalls, VPNs and gateways, which now enforce authentication and integrate with cloud identity, making them control points. Campaigns span vendors including Fortinet, Cisco, Ivanti, Citrix, Palo Alto Networks and Juniper, exploiting shared architectural patterns. The report calls a network device behaving like an endpoint, by scanning, accessing credentials or moving laterally, a defining signal of this threat.

**Infostealers:** Microsoft Defender Experts has observed an increase in macOS-targeted infostealer activity since late 2025, driven by social engineering.

| Stealer family | Share | 
|---|---|
| Atomic macOS Stealer | 50% | 
| OtterCookie | 19% | 
| PS1Bot | 14% | 
| Lumma Stealer | 10% | 
| MacSync Stealer | 7% | 

## Detection, intelligence and disruption

The report urges security operations centers to shift from reactive investigation to proactive disruption, building high-confidence incidents from correlated signals and automatically isolating devices, disabling accounts and revoking sessions once a confidence threshold is met. The number of rules or alerts, it argues, is no longer a meaningful measure of SOC maturity; what matters is the share of incidents reflecting genuine adversary activity and the speed of correlation and containment.

In FY26, Microsoft worked with 16 partners to curate fraud data, including victim reports, threat actor email addresses and fraud infrastructure. Its Digital Crimes Unit (DCU) also launched a Tech Scam Feed within its Cyber Threat Intelligence Program.

**RedVDS**, a cheap, subscription-based virtual desktop service used for phishing, account takeover and payment diversion, provided more than 10,000 users with access to up to 8,000 virtual desktops a month between 2023 and 2026, with users heavily concentrated in Africa and the United States. In 2026, the DCU filed a civil action in the United States and seized key domains, while using UK intellectual property law in a novel way. After Microsoft notified hosting providers in late January 2026, the number of available malicious VMs dropped sharply.

## Securing the foundations

**NTLM:** Microsoft security data shows a nearly sixfold increase in NTLM-related security cases over the past decade, and reported cases in the first six months of 2026 alone exceeded the total for 2025. Microsoft is phasing out the legacy protocol in favor of Kerberos-based alternatives.

**Software supply chains:** Major developer ecosystems, including npm, PyPI, Crates.io and GitHub Actions, saw attacks roughly every ten days over the past year, with a clear acceleration in Q2 2026. Shai-Hulud 2.0 affected 25,000 repositories, Megalodon backdoored 5,561 GitHub repositories in under six hours, and Axios reached more than 100 million weekly downloads while under attacker control.

| Month | Incidents | Largest wave | Dominant vector | 
|---|---|---|---|
| Nov 2025 | 1 | 25,000 | npm worm | 
| Mar 2026 | 5 | 5 | GitHub Actions | 
| Apr 2026 | 6 | 8 | API hijack | 
| May 2026 | 7 | 5,661 | npm account hijack | 

**Containers:** The median time from container start to first exploit attempt is just 5.3 hours, and more than half of compromised containers are exploited within 24 hours of starting. Default credentials, exposed APIs and unpatched images drive most successful compromises.

**Linux kernel:** In a two-week span between April 29 and May 13, 2026, three publicly disclosed flaws gave local users a path to root across major Linux distributions, each published with working proof-of-concept code and no embargo. Attackers found one of them, Copy Fail, with help from AI.

**Storm-2885:** In February 2026, DART uncovered a prolonged hybrid intrusion run almost entirely through legitimate Microsoft services. After compromising a highly privileged cloud identity, the actor closed alerts, downgraded incidents and resolved detections as false positives, accessed hundreds of thousands of emails across hundreds of mailboxes, and used Intune device management to establish tunnels into on-premises environments. The report's takeaway: modern threat actors no longer need to break security controls if they can leverage trust and governance instead.

## Identity at the center of cyber risk

Password attacks are down 26% year over year, which the report attributes to MFA and passkeys forcing attackers to pivot. But the share of AiTM phishing and token theft attacks more than doubled in the first six months of 2026, though it remains comparatively small. The report recommends synced passkeys for most non-admin users and device-bound passkeys for admins, sensitive systems and high-risk access.

Device code phishing, once niche, is now a preferred credential theft method for both nation-state and financially motivated actors. Around February 2026, attackers abused railway.com, a legitimate platform-as-a-service provider, to host token-harvesting services tied to the Microsoft 365 OAuth device code flow, bypassing passwords and MFA without traditional credential theft.

- **QR code phishing:** Microsoft Defender for Office 365 detected more than 145 million attacks between July 2025 and June 2026, with volume reaching an all-time high in March 2026. PDFs grew to 79% of delivery by April.
- **CAPTCHA-gated phishing:** More than 100 million attacks in the same period. By April 2026, PDFs accounted for nearly two-thirds.
- **Malicious attachments:** Each month, between 89% and 96% directed users to phishing pages or fake sign-in experiences rather than delivering malware. Malicious PDF volume rose 38% in February 2026 and another 50% in March, while SVG payloads fell from 23% of malicious attachments in July 2025 to 7% by June 2026.
- **Initial access brokers:** Intel 471 observed 2,591 advertised access sales affecting organizations in at least 117 countries, down from 4,356 the previous year. Of these, 45% were obtained through compromised legitimate credentials and 37% through compromised remote access portals.

**Tycoon2FA**, a phishing-as-a-service platform, was linked to more than 15.1 million malicious emails per month in the second half of 2025, accounted for up to 9% of all malicious messages Microsoft detected, and reached more than 500,000 organizations each month. In March 2026, the DCU, coordinating with international law enforcement and industry partners, targeted its hosting infrastructure. Phishing volume fell 15% in March and another 22% in April; by June, activity had dropped 95% from its November 2025 peak. Its share of CAPTCHA-gated phishing fell from 76% to 12%, and of QR code phishing from 33% to 14%. Two key figures were arrested.

**RMM tools:** An unsanctioned remote monitoring and management agent has become one of the most reliable mid-stage indicators of a human-operated intrusion, giving attackers the reach of a senior administrator from a single agent and collapsing what once required a dozen tools into one trusted channel.

**Cloud identity abuse,** once limited to advanced nation-state actors, is now used by all threat actor types against all industries.

**Cloud-related threat tactics by total alerts** (December 2025  –  June 2026, 180 days):

| Tactic | Stage | Alerts | 
|---|---|---|
| Password spray attacks | Pre-compromise | 124,146,529 | 
| AiTM credential phishing | Pre-compromise | 151,168 | 
| Device code phishing | Pre-compromise | 896 | 
| Threats targeting or leveraging Azure Blob Storage | Post-compromise | 19,482,467 | 
| Token theft and token replay | Post-compromise | 5,089 | 
| Malicious use of Microsoft Graph API | Post-compromise | 1,262 | 

Password spray accounts for more than 99% of observed cloud initial access efforts.

## Ransomware

Microsoft Defender observed a 15.8% year-on-year increase in ransom detonations against enterprises.

**Five most prolific families** (public leak site data, July 1, 2025  –  June 29, 2026): Qilin (16%), Akira (8%), Play (7%), Gentlemen (6%), INC (5%).

Qilin listed the most victims on leak sites (16.4%, up 386%), rising from fourth place. The report attributes this to the weakening of rivals such as RansomHub, a profit-sharing model in which affiliates keep 80 – 85% of ransom payments, and encryptors for both Windows and Linux/ESXi. Microsoft's own telemetry tells a different story, however: Akira was the most active family in Defender-detected events (22%, up 150%), followed by Qilin (14%, up 111%). The report suggests newer families may be inflating leak-site activity to attract attention.

**Ransomware by industry** (Microsoft Defender detections):

| Industry | 2025 | 2026 | 
|---|---|---|
| Information technology | 248 | 649 | 
| Critical manufacturing | 255 | 429 | 
| Other business entities | 250 | 412 | 
| Transportation systems | 133 | 178 | 
| Consumer retail | 140 | 164 | 
| Communications infrastructure | 66 | 159 | 
| Government agencies and services | 57 | 133 | 
| Energy | 124 | 104 | 
| Research and academia | 96 | 78 | 
| Healthcare and public health | 122 | 78 | 
| Financial services | 50 | 75 | 
| Defense industrial base | 4 | 22 | 

**Ransomware by country** (Microsoft Defender detections):

| Country | 2025 | 2026 | Change | 
|---|---|---|---|
| United States | 724 | 1,087 | 50% | 
| Germany | 59 | 222 | 276% | 
| Canada | 98 | 172 | 76% | 
| Italy | 48 | 135 | 181% | 
| United Kingdom | 80 | 133 | 66% | 
| Japan | 57 | 95 | 67% | 
| Switzerland | 29 | 59 | 103% | 
| Netherlands | 36 | 56 | 56% | 
| Costa Rica | 23 | 32 | 39% | 
| Israel | 19 | 23 | 21% | 
| South Africa | 7 | 17 | 143% | 
| Türkiye | 0 | 3 | n/a | 

Continued attacks on Israel, the report says, may reflect Iran's use of ransomware as a form of hybrid warfare. Attackers are also increasingly amplifying stolen data on social media platforms such as X as part of double extortion.

On policy, Microsoft strongly discourages ransom payments but warns that blanket bans risk revictimizing organizations and suppressing reporting. It supports designating persistent safe havens as "state sponsors of ransomware." The report notes that NATO's Cyber and Digital Transformation Division and Microsoft signed a Letter of Intent in February 2026, and criticizes governments for stalling on commitments made through the Pall Mall Process to curb the commercial cyber intrusion market.

## Trust and supply chain security

**Fox Tempest** ran a malware-signing-as-a-service operation, creating fraudulent developer identities to obtain authentic Microsoft-signed certificates for malicious payloads, including ransomware loaders and infostealers. In May 2026, the DCU filed a civil case that enabled the seizure of a core domain and the disabling of hundreds of actor-controlled virtual machines and accounts. Microsoft revoked more than a thousand fraudulent code-signing certificates, and a primary suspect was arrested in Europe.

In April 2026, the CPUID website, a popular source of hardware tools, was compromised and its genuine installers swapped for versions that gave attackers remote control of affected machines.

## Critical infrastructure

Nation-state actors, ransomware syndicates and opportunistic criminals are pivoting from data theft toward disruption of physical services, the report says.

**Attacks against state and local government, education (SLED) and critical infrastructure** (November 2025  –  April 2026): research and academia (38%), transportation systems (22%), government agencies and services (13%), communications infrastructure (11%), critical manufacturing (7%), energy (4%), government facilities (4%), chemicals (1%), water and wastewater (1%), emergency services (1%).

Cloud identity abuse was the most observed technique, at 62% against SLED and 78% against critical infrastructure, followed by password spray (38% and 13%, respectively). The tool mix against critical infrastructure was led by Truebot malware (39%), information stealers (38%), the AzureHound framework (12%), Impacket (6%) and LemonDuck (5%).

CISA's industrial control systems advisory program issued 380 advisories in 2023, 423 in 2024 and 508 in 2025. In 2025, the US Environmental Protection Agency reported that more than 70% of inspected water systems failed to meet basic cybersecurity requirements.

## National resilience and geopolitics

The report describes an era of polycrisis, defined by overlapping disruption with little room to recover, and calls for a shift from defense-in-depth to resilience-by-design, which assumes failures will occur.

It proposes a shared, multi-tenant government SOC delivered as a service to dozens of agencies, with each retaining its own tenant and data residency. Microsoft's own security team, it notes, now uses custom AI agents to autonomously investigate 75% of incoming incidents.

Other developments covered include:

- **The UN General Assembly's Global Mechanism** on ICTs in international security, launched in 2026 as a permanent forum for cyber diplomacy.
- **The ARC initiative** , first implemented in Kenya and expanded to Mexico, where it supported whole-of-government coordination ahead of the FIFA World Cup.
- **The CVE Program** , which the report says should be recognized as essential shared infrastructure, funded sustainably and modernized to keep pace with AI-accelerated discovery.
- **Subsea cables** , which carry more than 99% of intercontinental data traffic; the report urges governments to promote redundancy and geographic diversity of landings.
- **Open-source security** , for which it proposes a National Open Source Security Office.

## Preparing for the quantum transition

The report warns of "harvest now, decrypt later," in which adversaries store encrypted data today for future decryption. As of June 2026, more than 25 governments had issued post-quantum cryptography guidance or mandates, including India, Singapore and Malaysia beyond the G7. US policy for national security systems mandates that, starting in 2027, products not upgradeable to quantum-safe by 2030 should not be procured.

## The report's ten priorities

1. Govern cyber risk at leadership level.
2. Defend identity as the primary control plane.
3. Establish governance, visibility and data controls for AI.
4. Reduce exposure and edge risk.
5. Focus on cross-signal correlation.
6. Enact a continuous threat and vulnerability management model.
7. Validate exposure through proactive testing.
8. Secure software dependencies.
9. Participate in collective defense.
10. Prepare for long-horizon technological transitions.

## EcoPulse24 Analysis

The report's real value lies in how it reorders priorities. The greatest danger it describes is not a superhuman AI attack, but something simpler and more urgent: the same techniques used for years have become cheaper, faster and more widely available. That every one of the five most-detected CVEs dates from 2020 to 2023 makes the point plainly. Neglected fundamentals remain the widest door.

The gap between hours and weeks is the heart of the matter. When exploitation is weaponized in under a day and remediation can take one to two months, the question becomes operational and managerial before it is technical. For boards, patch velocity is now a governance metric.

AI agents add a new dimension. Every agent granted permissions is another identity that can be stolen, every memory it keeps is a surface that can be poisoned, and every approval prompt is a control that can be routed around. Agent governance belongs in security planning from day one.

The most telling data point may be the one least consistent with the report's broader optimism about AI-powered defense: dwell time is rising. Organizations are taking longer, not less time, to detect intruders, a sign that the gap between offensive and defensive capability has not yet closed.

The report should be read as the work of a security vendor whose data reflects its own systems and customers. Even so, it remains one of the most comprehensive public sources of threat data available, and offers a granular view of trends that are difficult to observe anywhere else.

## If this story helped you, support EcoPulse24

A quick optional contribution through Google helps us keep daily coverage moving.
