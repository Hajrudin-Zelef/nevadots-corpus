---
id: collect-261121-veille-ia/claude-anthropic/cursor-supply-chain-breach-trojanized-claude-extension
title: "Cursor Supply Chain Breach: Trojanized Claude Extension"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "fireworks", "extension", "breach", "inference", "access", "developer", "cursors"]
source: https://datawater.com/cursor-fireworks-ai-supply-chain-breach-backdoor/
source_anchor: ""
source_lines: [1, 182]
section: "Claude_Anthropic"
---
A breach at AI inference provider Fireworks AI handed attackers a Cursor access key — which they used to steal developer credentials and push a backdoored Claude Code extension to as many as 5,000 developer machines. Uber and Notion were also hit in the same incident. The attack chain redefines AI inference providers as a critical supply-chain risk class.

**What:** A supply-chain attack rooted in a Fireworks AI credential theft. Attackers compromised Fireworks’ development environment, extracted a Cursor-issued access key, copied developer session traces from Cursor’s shared storage, harvested unredacted API credentials from those traces, then used one of them to inject trojanized Claude Code extension versions 2.1.312 and 2.1.313 into Cursor’s VS Code marketplace cache. The tampered extension installed a persistent SSH backdoor and collected host information.

**Status:** Incident confirmed and contained. Cursor disclosed full forensic details on November 13, 2026. No active attacker access observed since November 1. Fireworks investigation ongoing. Uber and Notion confirmed separate data theft via the same Fireworks breach. No CVE assigned — this is a supply-chain incident, not a traditional software vulnerability.

**Deadline:** No CISA KEV entry. No regulatory deadline, though affected organizations with exposed personal data may face notification obligations under applicable data-protection laws.

**Fix:** If you installed Cursor extension versions 2.1.312 or 2.1.313 on October 29, 2026: uninstall those versions and check for an unauthorized SSH key and backdoor process, then rotate every credential that was stored in or passed through Cursor. All Cursor users outside Privacy Mode should rotate API keys as a precaution. Cursor has since rebuilt its marketplace and rotated its own credentials.

### Key takeaways

- **Your AI inference provider is a supply-chain risk, not just a vendor:** The Cursor breach began at Fireworks AI, a third-party inference provider. Attackers with Fireworks credentials could reach Cursor’s storage and ultimately its developer machines. Any organization sharing a data bucket or access key with an AI inference provider should audit those permissions now.
- **Developer tool marketplace caches are an emerging attack surface:** The trojanized extension was placed not in the official Anthropic or Open VSX repositories, but in Cursor’s internal distribution cache — a path that bypassed the original publisher’s controls and reached users who expected a legitimate update.
- **Up to 5,000 Cursor users may have installed a backdoored extension:** Versions 2.1.312 and 2.1.313 were available for approximately 11 hours on October 29, 2026. Users who installed either build during that window should assume possible compromise even if the SSH step appeared to fail — the extension also collected host information and may have left persistence artifacts beyond SSH.
- **The blast radius extended to Uber and Notion:** The same Fireworks breach that exposed Cursor’s key also gave attackers access to data tied to Uber, resulting in confirmed theft of driver license records, and to Notion data. This is a multi-victim supply-chain incident, not an isolated developer-tool story.
- **Non-Privacy-Mode AI coding sessions may contain credentials:** Cursor’s coding-agent traces from non-Privacy-Mode sessions held unredacted API keys and other secrets for fewer than 0.05% of users. This is a reminder that AI tool sessions — including agent runs, code completions and chat — can record and store sensitive material if privacy controls are not enforced.

## Executive Summary

**The developer tool supply chain just had its own SolarWinds moment.**

On October 23 to 24, 2026, attackers compromised Fireworks AI’s development infrastructure and obtained a Cursor-issued access key that Cursor had shared with Fireworks for joint inference work. With that key they copied a read-only storage bucket containing coding-agent session traces from Cursor users who had not enabled Privacy Mode. Some of those traces had not been fully redacted and contained API credentials. The attackers used at least one of those credentials to access Cursor’s internal systems, reaching archived non-production source repositories, support data, product-usage metrics and summaries of agent conversations.

What turned a data-access incident into an active attack was the next step: on October 29, the attacker used a Cursor internal credential to overwrite two extension versions — 2.1.312 and 2.1.313 — in the cache that serves Cursor’s extension marketplace. For about eleven hours, any user who triggered an update of the Claude Code extension received a trojanized build. That build collected host information and attempted to establish a persistent SSH backdoor. Cursor took the marketplace offline, rebuilt it, and by November 1 had observed no further attacker activity.

Cursor’s November 13 forensic disclosure confirmed the Fireworks connection. It also revealed that Uber had separately confirmed thousands of driver license records stolen through the same Fireworks breach, and that Notion had confirmed data theft as well. The same compromised Fireworks environment served multiple companies’ data, turning a single vendor breach into a multi-victim supply-chain incident.

For security leaders, this incident rewrites risk models in two ways. First, AI inference providers now belong in the same supply-chain risk tier as software build systems and code-signing infrastructure. Second, AI coding tool sessions — completions, agent runs, chat — can retain sensitive data if the tool’s privacy mode is not enforced, which creates a new category of credential exposure that most DLP programs do not monitor.

## Why This Matters to CISOs and Security Leaders

**Most organizations do not know which AI vendors their developers are sharing data with.** Cursor’s arrangement with Fireworks — a shared storage bucket for inference data — is a reasonable technical choice for improving model performance. But from a security standpoint, it means Cursor’s developer session data, including any credentials left in those sessions, was accessible to Fireworks, and therefore to anyone who could compromise Fireworks. The standard vendor-risk questionnaire does not capture this relationship.

**The distribution-cache attack surface is new and underfended.** Security teams monitor official package registries and code repositories for malicious packages. The Cursor incident shows that an attacker with internal credentials can bypass those controls by targeting the distribution cache directly — the layer between the official package and the end user. The tampered builds were never in Anthropic’s repository or Open VSX; they appeared only in Cursor’s internal cache for a single afternoon.

**Developer machines are high-value targets.** A developer’s workstation typically has access to production systems, cloud credentials, code repositories and internal APIs. An SSH backdoor on a developer machine is not a nuisance — it is a persistent foothold in the environment. The threat model that treats developer tools as lower risk than server software is out of date.

Ask your head of platform engineering two questions this week: Which AI tool vendors have read access to any storage bucket or data feed that our developers touch? And do we have privacy controls enforced — not optional — on every AI coding tool in use? If you don’t know the answers, that’s the gap to close before the next Fireworks-style breach.

## The Timeline

| Date (2026) | What happened | 
|---|---|
| Oct 23–24 | Fireworks AI confirms unauthorized access to its cloud provider’s R&D machines and to Fireworks development machines. | 
| Oct 25 | Last unauthorized access to Fireworks’ customer platform that Fireworks has detected, per its November 9 bulletin. | 
| Oct 29, ~2:27 a.m. PT | Trojanized Claude Code extension versions 2.1.312 and 2.1.313 placed in Cursor’s marketplace cache. Available for approximately 11 hours. | 
| Oct 29, ~1:35 p.m. PT | Cursor takes marketplace offline and removes tampered builds. | 
| Nov 1 | Last attacker activity Cursor has observed; Cursor says the activity was blocked. | 
| Nov 2 | SecurityWeek covers the extension incident before the Fireworks connection is public. Anthropic confirms it did not publish the tampered extensions. | 
| Nov 9 | Fireworks AI updates its security bulletin. Uber confirms theft of driver license records tied to the Fireworks breach. | 
| Nov 13 | Cursor publishes full forensic account tracing the October 29 extension incident to the exposed Fireworks key. Notion also confirms data stolen. | 
| Nov 14–16 | The Hacker News, BleepingComputer and multiple security outlets cover the full supply-chain picture. | 

The attacker didn’t need to compromise Cursor’s build system. They compromised the AI vendor Cursor trusted with its data, then used the credentials that fell out of that trust relationship to poison the update cache directly.


## Attack Chain Analysis

| Stage | Technique | Enterprise Control | 
|---|---|---|
| Initial access | Breach of Fireworks AI cloud provider R&D machines (method not publicly disclosed) | Vendor risk management; require MFA and secrets scanning on all shared infrastructure | 
| Lateral movement | Use Cursor-issued access key obtained from Fireworks to copy read-only storage bucket | Least-privilege key scoping; separate storage buckets per vendor with no cross-vendor access | 
| Credential harvesting | Extract unredacted API keys from coding-agent session traces in bucket | Secret scanning and redaction in AI tool logging pipelines; enforce Privacy Mode by policy | 
| Privilege escalation | Use harvested credential to access internal Cursor SaaS tools and source archives | Privileged access monitoring; alert on credential use from new IPs or outside business hours | 
| Payload delivery | Overwrite extension versions 2.1.312/2.1.313 in Cursor marketplace cache | Extension integrity verification (hash pinning); code-signing on internal distribution caches | 
| Persistence | Trojanized extension installs SSH backdoor and collects host information | EDR alerting on new SSH authorized_keys additions; block outbound SSH from developer workstations unless explicitly required | 
| Exfiltration (secondary) | Uber driver license records and Notion data accessed via same Fireworks breach | Multi-vendor data minimization; audit which inference providers can reach each company’s datasets | 

**What nobody is talking about**

The focus is on the 5,000 Cursor users who might have installed the backdoored extension. But the deeper exposure is the coding-agent trace data: session recordings from AI coding tools that contain credentials users typed, pasted or allowed the agent to read. Most organizations have no DLP coverage for AI tool session logs, because those logs live in the vendor’s infrastructure rather than in the enterprise’s own systems. The Cursor incident showed those logs can be breached and mined for credentials. Every organization using an AI coding tool should audit whether its session data is being retained, by whom, and whether a breach of that vendor would expose those sessions.

## Threat Intelligence Breakdown

**Attribution:** No threat actor has been publicly identified. Cursor has not named an attacker or attributed the incident to a known group. The FBI and CISA have not issued a joint advisory as of November 16, 2026. Attribution is unknown.

**Victim scope:** Cursor (up to 5,000 extension users directly; a broader set of users whose session traces were accessed); Uber (driver license records for an unspecified number of drivers, confirmed by Uber); Notion (data confirmed stolen, volume undisclosed). All victims trace back to the same Fireworks AI breach.

**What is still unknown:** The initial access method into Fireworks’ cloud provider. The full list of organizations that shared data with Fireworks and may have had data accessed. The exact number of developer machines where the SSH backdoor successfully installed. Whether any attacker access beyond November 1 occurred that Cursor has not yet detected.

**Indicators of compromise** (from Cursor’s security notice, as reported; organizations should obtain the full list from Cursor’s linked advisory):

| Type | Indicator | Context | 
|---|---|---|
| Extension versions | Claude Code for VS Code v2.1.312, v2.1.313 | Tampered builds distributed via Cursor marketplace cache on October 29, 2026 | 
| File/key artifact | Unauthorized SSH authorized_keys entry added by extension | Persistence mechanism; check `~/.ssh/authorized_keys` on all machines that ran these versions | 
| Process artifact | Background process or service started by extension outside normal VS Code execution | Inspect running processes and startup items on affected machines | 
| Network | Outbound SSH connection from developer workstation to unknown external host | Review firewall/EDR logs for outbound SSH on October 29–November 1 from affected machines | 

*Note: Cursor’s full indicators of compromise and remediation scripts are available in its linked security notice. Organizations should obtain and apply the full set rather than relying solely on the summary above.*

## Enterprise Impact

**Developer machines as an entry point to production:** Any developer machine where the SSH backdoor installed represents a potential persistent access point to code repositories, cloud environments, internal APIs and production deployment systems. Even if the backdoor appeared to fail, partial artifacts may remain. Treat any machine that ran versions 2.1.312 or 2.1.313 on October 29 as potentially compromised until verified clean.

**AI session data as a credential store:** The coding-agent traces that yielded credentials were not logs of risky behavior — they were normal developer sessions where the AI saw code that happened to contain API keys, database connection strings or cloud credentials. Any organization using AI coding tools in non-privacy mode has session data being retained at the vendor that may hold sensitive material. If that vendor is breached, that material is exposed.

**The Uber and Notion exposure broadens the enterprise footprint:** Enterprises that use Uber for Business or Notion workspaces may have employee or partner data among the records stolen in the Fireworks breach. These organizations did not use Cursor; their exposure came purely through Fireworks’ role as a shared infrastructure provider. Procurement teams that reviewed Fireworks as a vendor did not evaluate its role in Cursor’s data pipeline — that relationship was invisible to them.

**Regulatory and notification risk:** Uber’s confirmed theft of driver license records is likely subject to state breach-notification requirements and potentially CCPA. Organizations that share regulated personal data with AI inference providers should review whether a breach at those providers would trigger their own notification obligations.

## What Security Leaders Should Do Next

### Today

1. **Identify all machines that ran Cursor extension versions 2.1.312 or 2.1.313 on October 29, 2026.** Check endpoint management logs, software inventory, or Cursor’s notification list if your organization was contacted. These machines need manual investigation before you can clear them.
2. **On each identified machine, check for:** unauthorized entries in`~/.ssh/authorized_keys` ; unexpected background processes or startup items added around October 29; outbound SSH connections in firewall or EDR logs between October 29 and November 1.
3. **Revoke and rotate any API keys, cloud credentials or secrets that may have been visible in Cursor coding-agent sessions since those sessions began,** not only credentials Cursor confirmed as exposed. The scope of what was recorded in traces is broader than what was confirmed as stolen.
4. **Audit which AI inference providers have access to any of your organization’s data buckets, storage feeds or agent session logs.** Map the trust relationships now, before the next vendor breach.

### Within 48 hours

1. **If you find SSH backdoor artifacts on any machine, treat it as a full incident:** isolate the machine, preserve forensic images, hunt laterally for signs the attacker pivoted to connected systems, and rotate all credentials that machine had access to.
2. **Review your AI tool policies for Privacy Mode enforcement.** If your organization uses Cursor, GitHub Copilot, Codeium, or similar tools, determine whether Privacy Mode is enforced by policy or only optional for users. Optional privacy controls are not controls — they are a data retention choice left to individual developers.
3. **Contact Cursor’s security team** at the address in its November 13 disclosure to obtain the complete indicators-of-compromise list and remediation scripts. Apply them to every machine in scope, not only machines where you found obvious artifacts.
4. **Brief your CISO and legal team** on whether your organization has employees whose data may have been among the Uber or Notion records affected, and whether any of your own data was shared with Fireworks through products that used its inference infrastructure.

### Long-term

1. **Add AI inference providers to your third-party risk management program** with the same rigor applied to build-system vendors. Require them to disclose data-sharing arrangements, enforce key scoping, and notify you of breaches within 72 hours.
2. **Implement secret scanning in AI tool pipelines.** Before agent session logs are stored or shared with any vendor, strip or redact credentials using the same tooling you would apply to a code repository push. Many AI tool vendors do not do this by default.
3. **Require code-signing or hash verification for internal extension distribution caches.** The tampered builds bypassed publisher controls because they were injected at the cache layer. A cryptographic chain from the original publisher to the installed build would have blocked or detected this attack.
4. **Treat developer workstations as high-value targets in your threat model.** Extend EDR alerting, privileged-access monitoring and lateral-movement detection to developer machines with the same rigor as servers.

#### Stay ahead of AI supply-chain threats

DataWater’s executive threat briefing surfaces the incidents and techniques security leaders need to act on — before they reach the board.

## Winners and Losers

| Better positioned | More exposed | 
|---|---|
| Organizations that enforced Cursor Privacy Mode by policy, ensuring session data was never retained or shared | Organizations where Privacy Mode was optional and developers routinely left it off | 
| Teams that scoped vendor access keys to minimum permissions with no cross-dataset reach | Companies that issued broad API keys to AI vendors for convenience without reviewing data access scope | 
| Security teams with EDR coverage and SSH-key monitoring on developer workstations | Organizations that treat developer laptops as lower-risk endpoints and do not monitor them for persistence mechanisms | 
| Enterprises that audit their AI tool vendor graph annually and require breach notification SLAs | Procurement teams that evaluated Cursor or Fireworks independently without mapping their data-sharing relationships | 
| Developers who installed extension versions outside the October 29 window, or who used only the official Open VSX or VS Code Marketplace builds | Developers who installed versions 2.1.312 or 2.1.313 on October 29 and have not yet checked their machines for backdoor artifacts | 

## Final Executive Takeaway

**The weakest link in your AI toolchain is not the model — it’s the infrastructure around it.**

The Cursor breach did not exploit a code vulnerability in Anthropic’s model or in Cursor’s core product. It exploited a trust relationship between Cursor and a third-party inference provider, extracted credentials from AI session logs that most organizations don’t think about, and used them to poison the update path for a developer tool.

The lesson for every organization using AI coding tools is blunt: audit the data-sharing arrangements your AI vendors have with their own subcontractors, enforce Privacy Mode by policy rather than preference, and treat developer machines as first-class targets in your threat model.

**The SSH backdoor is the visible threat. The invisible threat is that your developers’ AI sessions may be storing secrets you don’t know about, in infrastructure you don’t control, with subcontractors you’ve never reviewed.**

## Frequently Asked Questions

## What was the Cursor supply chain attack and how did it happen?

The Cursor supply chain attack began with a breach at Fireworks AI, a third-party inference provider that Cursor had issued an access key to for shared work. Attackers compromised Fireworks’ development environment around October 23–24, 2026, obtained Cursor’s key, copied developer session traces from Cursor’s shared storage bucket, and extracted API credentials from those traces. Using a harvested credential, they placed trojanized Claude Code extension versions 2.1.312 and 2.1.313 into Cursor’s marketplace distribution cache on October 29, 2026. The tampered extension attempted to install an SSH backdoor on developer machines and collect host information. Cursor removed the builds after about 11 hours and confirmed in a November 13 forensic disclosure that the Fireworks breach was the root cause.

## Am I affected if I use Cursor? What should I check?

Cursor users are potentially affected if they installed Claude Code extension versions 2.1.312 or 2.1.313 during the exposure window on October 29, 2026 (approximately 2:27 a.m. to 1:35 p.m. Pacific time). Users who installed those versions should check `~/.ssh/authorized_keys` for unauthorized entries, review running processes and startup items added around that date, and inspect firewall or endpoint logs for unexpected outbound SSH connections. All Cursor users outside Privacy Mode should rotate any API keys and credentials that may have appeared in coding-agent sessions, as a broader precaution beyond the extension versions. Cursor has sent notifications to users who installed the affected builds and has published remediation scripts.

## Did Anthropic or the official Claude Code extension get hacked?

No. Anthropic did not publish the tampered builds, and the official Claude Code extension was not compromised. The trojanized versions 2.1.312 and 2.1.313 were injected specifically into Cursor’s internal marketplace distribution cache, which is separate from Anthropic’s distribution channels, the VS Code Marketplace, and Open VSX. Anthropic’s Felix Rieseberg confirmed in SecurityWeek’s November 2 coverage that Anthropic neither built nor published the tampered copies. The compromise was in Cursor’s distribution infrastructure, not in the upstream extension source.

## Why were Uber and Notion affected if they don’t use Cursor?

Uber and Notion were affected because the same Fireworks AI breach that exposed Cursor’s access key also gave attackers access to data associated with other Fireworks customers. Fireworks AI is an inference provider that serves multiple companies, and its breach created a multi-victim supply-chain incident. Uber confirmed that thousands of driver license records were stolen through the Fireworks breach. Notion confirmed data theft as well. Neither Uber nor Notion was a Cursor customer; their exposure came entirely through Fireworks’ role as a shared infrastructure provider for AI inference.

## What risk do AI coding tool sessions create for credential security?

AI coding tools such as Cursor’s coding agent can record and store session traces that include code the developer was working on, questions asked, and agent context. If a developer’s code or session context contained API keys, database connection strings, cloud credentials, or other secrets — even pasted temporarily — those may appear in the session trace stored by the vendor. In the Cursor supply chain attack, unredacted credentials in session traces were the mechanism by which the attacker elevated access from a read-only storage bucket to internal Cursor systems. Organizations should audit whether Privacy Mode is enforced by policy on all AI coding tools in use, and implement automated secret scanning and redaction before any session data is shared with or retained by a vendor.

## What is the difference between this attack and earlier AI supply-chain incidents?

Earlier AI supply-chain incidents such as the LiteLLM supply chain breach and the SCKit ScreenConnect campaign involved compromised open-source packages or remote-access tooling. The Cursor incident introduced a new vector: the AI inference provider as a trusted intermediary with read access to developer session data. The attacker did not need to compromise a code repository or build pipeline; they compromised the company that Cursor trusted with its usage data. This creates a new risk category — inference-provider trust — that sits outside the attack surfaces most security teams currently model.

## What should enterprises do to prevent a similar AI supply-chain attack?

Enterprises should take three structural steps. First, map every AI tool vendor’s data-sharing arrangements with their own subcontractors — inference providers, fine-tuning services and usage analytics partners — and apply the same third-party risk standards to those subcontractors as to the primary vendor. Second, enforce Privacy Mode (or equivalent data-minimization controls) by policy rather than user preference on all AI coding tools, and implement automated secret scanning before session data is retained or shared. Third, require cryptographic integrity verification on internal extension distribution caches, so that a compromised credential cannot silently replace a legitimate build with a backdoored one. These controls address the specific gaps the Cursor attack exploited.

## Sources

**Primary sources:** Cursor security disclosure, November 13, 2026 (via The Hacker News and CASRAI relay); Fireworks AI security bulletin, November 9, 2026; SecurityWeek, November 2 and November 13–14, 2026; The Hacker News, November 13–14, 2026; Anthropic statement via SecurityWeek (Felix Rieseberg, November 2, 2026); BleepingComputer, November 14–15, 2026. Additional context: Senthorus Cybersecurity Week in Review, November 9–15, 2026 (multi-vendor scope confirmation). Single-source claims are labeled in the body text. Related DataWater briefings: LiteLLM Supply Chain Breach; ARTEX AI Agent WorkOS Breach; SCKit ScreenConnect MSP Attack. Reflects public reporting as of November 16, 2026.
