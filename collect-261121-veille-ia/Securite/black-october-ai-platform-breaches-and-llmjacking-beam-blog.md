---
id: collect-261121-veille-ia/securite/black-october-ai-platform-breaches-and-llmjacking-beam-blog
title: "Black October: AI Platform Breaches and LLMjacking | Beam Blog"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["opencode", "access", "keys", "october", "vendor", "railway", "says", "stored"]
source: https://agentbeam.com/blog/black-october-wiz-ai-developer-platforms-llmjacking-stolen-keys
source_anchor: ""
source_lines: [1, 151]
section: "Securite"
---
Wiz says at least nine AI developer platforms disclosed security incidents in October 2026, and that almost all of them involved targeted hunting for stored environment variables, especially the ones that grant access to AI models. If you deploy agents, run coding tools or keep model keys on any of these platforms, treat the keys they held as exposed and rotate them. The Wiz write-up, published October 30, also shows what happens next: two enterprise keys from vendor breaches were validated within 30 minutes and then used for hundreds of thousands of model calls.

This post summarises Wiz's findings, separates what the vendors have confirmed from what Wiz infers, and lists the checks that matter for anyone whose coding agents depend on provider API keys.

## TL;DR

| Question | Answer | 
|---|---|
| Am I affected? | If you stored keys or environment variables on Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode or Northflank, assume they may be exposed until the vendor says otherwise. | 
| Is there a fix? | No patch. The fix is rotation, narrow key policies and quota limits. | 
| Is one vulnerability responsible? | Wiz says no single vulnerability or dependency ties the incidents together. | 
| How quickly did abuse start? | In Wiz's two cases, within 30 minutes of the reported credential access at the vendor. | 
| Does data leave my machine? | Reading this does not. The incidents are vendor-side; your keys left through the platform that stored them. | 

## The nine incidents Wiz lists

Wiz's list, with the dates it gives:

| Date | Platform | 
|---|---|
| Oct 6 | Fireworks AI | 
| Oct 9 | Andromeda | 
| Oct 14 | Zeabur | 
| Oct 15 | Lightning AI | 
| Oct 16-17 | OpenCode | 
| Oct 20 | Railway | 
| Oct 23 | Warp | 
| Oct 28 | CommandCode | 
| Oct 30 | Northflank | 

Wiz notes that Fireworks, OpenCode and Warp have reported indications of an "agent swarm" attack, that Andromeda's report calls its incident part of an "industry-wide attack", and that OpenCode and CommandCode explicitly attribute theirs to an attempt at distillation. Zeabur has published a full write-up and says it will shut down because of the incident. Railway has released a preliminary report and timeline. Most of the others are still being analysed.

Beam has covered several of these individually: Railway's breach and the environment variables to rotate, Zeabur's second breach and shutdown, Warp's vendor breach claim and the OpenCode console breach. The week-by-week view is in our Black October week-of-Oct-19 roundup.

## The pattern: credentials, not a shared bug

Wiz is careful here. Fireworks, Zeabur and Lightning attribute their root causes to credential access. Lightning had earlier been affected by a supply chain attack in the Mini-Shai Hulud campaign. Railway believes initial access was "privilege escalation in our build infrastructure". Warp suggested a third-party vendor dependency, and OpenCode named a "legacy system". These are vendor statements as Wiz relays them, and Wiz says detailed technical information is mostly unavailable.

What the public reports share, according to Wiz, is targeted exploitation that explicitly hunts stored environment variables with a focus on those granting AI access. Wiz ties this to two trends it has written about: agentic attacks, where attackers move faster with AI agents (see our summary of Wiz's seven fingerprints), and the LLMjacking economy, where "LLM brokers" make stolen credentials easy to monetise. OpenCode and CommandCode's distillation attribution points to another source of demand for access.

## What the impact looks like so far

Public impact reports are mostly anecdotal social media posts, according to Wiz:

- Alleged theft of roughly $371,000 in cryptocurrency using keys stored in Railway.
- Consumer bills in the hundreds to thousands of dollars from stolen AI access, such as OpenCode's.

Those are unverified claims. The part Wiz can observe itself is downstream abuse of enterprise credentials that it attributes to one of these vendor breaches.

## Two cases from Wiz's telemetry

Wiz describes two enterprise keys that had previously been used heavily from a now-compromised vendor's infrastructure. Both saw identical first unauthorized activity within 30 minutes of the reported credential access at the vendor: a single `InvokeModel` call from a residential IP address, part of a one-minute sweep testing stolen keys across multiple organisations. The call asked for Claude Fable 5.1, failed, and fell back to Opus 5.5, which worked.

For the next days, both keys saw a trickle of 14 to 120 calls a day from the same small Go client, running in lockstep about a minute apart. Wiz reads this as one ongoing job checking the keys stayed valid.

### Case A: the permissive key

Four days after validation, a rotating proxy pool ran a catalog probe across dozens of addresses with one call per model across vendors, including Amazon Nova, DeepSeek, gpt-oss, GLM, Kimi, Grok and older Claude models. Everything worked except Fable. Hours later a `boto3` script enumerated Bedrock and EC2 quotas, and 22 seconds after that, bulk traffic began.

Over thirty hours, before the key was remediated, the actor made about 800,000 Bedrock calls, about 90% successful. The key's policy allowed `global.*` cross-region inference profiles, so the actor routed everything through them, spreading load across regions so only about 6% of calls were throttled. Opus models made up three quarters of successful calls.

### Case B: the restricted key

Seven days after validation, the second key surfaced in an apparent reseller pool. For four hours, thousands of calls failed because the key's policy allowed Bedrock only through `us.*` inference profiles while the pool requested `global.*`. Later a checker on a shared VPN exit identified the working `us.*` model IDs, and two hours after that a `boto3` script, matching the one from Case A down to the version string, retested both profile types and enumerated quotas.

Over thirteen hours, a single Level 3 address made about 1.7 million Bedrock calls, over a million successful. Nearly all the rest were throttled by the account's regional quotas. Without global routing, quotas were the limiting factor.

### What the traffic looked like

In both cases the token profile resembles resold coding-agent traffic: typical calls sent a handful of uncached input tokens (the signature of prompt caching) and returned a few hundred output tokens, with a long tail carrying tens of thousands of uncached input tokens, consistent with many end users behind a reseller. Wiz says the two cases alone likely ran up over $100,000 at list price.

The contrast between the cases is the useful lesson. A restrictive policy did not stop the abuse, but it forced the attacker to spend hours finding the permitted profiles and left regional quotas as a real ceiling.

## Who was reselling

Wiz links the key usage to overt reselling organisations but says it cannot definitively establish a relationship between them and the original compromise or the flood of abuse.

- **"momo".** An actor centred on`23.106.46.239` , tied to an LLM resale site that advertises discounted frontier models and accepts USDT. The IP was publicly named by Zeabur as part of its compromise, hosted an ARTEX landing page on October 8, and in late October ran API key checks on the keys from both cases and attempted a`PutAccountDataRetention` call with the Case B key.
- **"Xingjiabi".** A second resale hub whose infrastructure (`38.246.114.118` ) enumerated the Case A key's inference profiles and quotas and manually tested the Case B key, after high-volume abuse from unrelated IPs had already begun. Wiz sees that as evidence of an active key market. A snapshot of the site showed a "distillation behaviour detection" page scoring users as high risk, watch or normal.

Wiz also published indicators of compromise: four IPs that tested stolen keys (`23.106.46.239`, `38.246.114.118`, `104.243.32.50`, `31.57.201.201`) and several hosting ASNs used in rotating pools. It stresses it has no evidence linking them directly to the Black October intrusions, so use them to search your logs, not to attribute.

## What this means if you run AI coding agents

Your coding agent's provider key is one of the credentials these attackers were hunting. Some concrete steps:

1. **Inventory where keys live.** List every platform that stores your model-provider, cloud and Git credentials as environment variables. If any are on the nine platforms above, rotate all of them, not just the AI keys. Our first 24 hours after a vendor breach runbook gives an order.
2. **Review provider logs.** For AWS, look in CloudTrail for`InvokeModel` and`InvokeModelWithResponseStream` from unfamiliar IPs, calls to unusual models, and`ListInferenceProfiles` or quota enumeration (Service Quotas, EC2) from addresses that are not yours. Compare against Wiz's IOCs.
3. **Narrow the key policy.** Case B shows the value of restricting Bedrock inference profiles to the regions and models you use, and of keeping regional quotas low. CloudTrail is where you will see the calls. Case A shows the cost of allowing`global.*` .
4. **Set budgets and alarms.** Alert on invocation counts and spend per key, since the flood arrived within hours of validation.
5. **Keep keys out of agent-readable places.** A coding agent can read`.env` files and process environments. See how API keys leak through logs and env vars and the earlier Wiz LLMjacking research on GCP billing hijack.
6. **Prefer short-lived credentials.** Role-based, expiring credentials make a stolen key a smaller prize than a static access key.

## What Beam can and cannot do here

These breaches happened on vendors' servers. Beam runs on your machine and cannot see or stop a platform breach, nor can it revoke a key that has already been stolen.

What Beam does cover is the local side of the same problem. It flags credential exposure in what your coding agents read, print or send, and redacts known credential formats before anything is stored. For Claude Code and Codex, its policy can deny a flagged tool call through a PreToolUse hook. That is hook-based, not a kernel sandbox, and detection is heuristic. Skill and MCP config scanning checks for risky patterns before you run them. Everything stays on-device. See what Beam watches and why it stays local.

## What we do not know yet

Wiz's own caveats matter: the investigations are in progress, no single entry point is shown, the crypto-theft and consumer-bill figures are anecdotal, and the link between the resale actors and the initial compromises is not established. The "agent swarm" language comes from the vendors' reports and we have not seen the underlying evidence. We will update this post as vendors publish detail.

## Update (October 31, 2026): Northflank confirms unauthorized use of customers' AI keys

Northflank's status page now says multiple users confirmed that AI API keys they supplied to the platform were used without authorization, and tells all users to assume their environment variables were decrypted. BleepingComputer reported the incident on October 31.

The status page timeline (all October 30, UTC): Wiz threat alert at 11:43, Northflank engineers reviewing at 11:44, an initial network policy patch at 12:02, further patching at 12:50 and 13:05, and a request for Wiz's incident response team at 13:00. At 13:58 Northflank said to assume all environment variables were decrypted and to disable or rotate all keys.

What changed for customers:

- Secret rotation for Postgres and MongoDB was enabled for all users on October 30, and for MySQL and Redis on October 31.
- Audit logs were enabled for all customers, including free users.
- Rotate secrets on workloads, secret groups and global secrets, and disable public access to add-ons if you enabled it.
- Revoke the old credentials with their issuing providers, issue replacements, then update the secrets in Northflank. Review AI provider usage and billing logs and report suspicious usage to Northflank and the provider.

Northflank says the reports may be consistent with the broader pattern of attacks on AI credentials, and that similar IP addresses appear in Wiz's Black October report. It has not published how the attackers got in, how many customers were affected, or whether anything beyond AI keys was misused. Treat those as open.

## Update (November 9, 2026): Fireworks expands October disclosure

Fireworks AI published a follow-up security bulletin on November 9, 2026, with Palo Alto Networks Unit 42 assisting forensics. Beyond evaluation-job secrets known in early October, Fireworks now says **billing information**, additional **secrets**, **datasets**, and **incomplete model weights** were accessed for a limited set of accounts. The company attributes the activity to a financially motivated group aligned with **LLMjacking** resale and documents a **October 23–24** path through a **third-party cloud provider** hosting R&D machines that led to further customer data impact before remediation on October 25. Fireworks reports **no detected impact** to Inference, Training SDK, or Nexus (zero data retention by default) and has **disabled self-serve dataset uploads and downloads** for Batch Inference and dataset-based training during the investigation. Beam's full breakdown of the bulletin covers response steps for teams that stored keys or custom weights on the platform. Same-day Bloomberg Law and Forbes reporting names **Cursor** and other large customers in the vendor's coordination — details in that post's customer-response update.

## Update (November 14, 2026): Cursor ties Fireworks to agent traces and marketplace malware

Cursor's November 13 forensic update confirms Fireworks was the entry point for stolen **coding-agent traces** and a **tampered Claude Code marketplace build** — see Cursor Fireworks incident.

## Related reading

## Frequently asked questions

### Which platforms did Wiz list in Black October?

Fireworks AI (Oct 6), Andromeda (Oct 9), Zeabur (Oct 14), Lightning AI (Oct 15), OpenCode (Oct 16-17), Railway (Oct 20), Warp (Oct 23), CommandCode (Oct 28) and Northflank (Oct 30). Wiz says most investigations are still ongoing and technical detail is limited.

### Is there one vulnerability behind all nine incidents?

Wiz says no single vulnerability or dependency links them. Root causes reported so far differ: credential access (Fireworks, Zeabur, Lightning), build-infrastructure privilege escalation (Railway), a third-party vendor dependency (Warp) and a legacy system (OpenCode).

### How fast were stolen keys abused?

In the two cases Wiz documents, the first unauthorized call was a single InvokeModel from a residential IP within 30 minutes of the reported credential access at the vendor.

### What is LLMjacking?

Using stolen cloud or model-provider credentials to run inference at the victim's expense, often resold through discount API sites. Wiz estimates its two cases alone likely ran up over $100k at list price.

### What should I do if a platform I use was on the list?

Rotate every environment variable and key the platform could read, prioritising cloud and AI provider keys, then review provider logs for model invocations you did not make and restrict keys with narrow policies and quotas.

*Details are accurate as of November 9, 2026, based on Wiz's October 30 report and vendor updates through the Fireworks November 9 bulletin. Investigations are ongoing and vendor statements may change.*
