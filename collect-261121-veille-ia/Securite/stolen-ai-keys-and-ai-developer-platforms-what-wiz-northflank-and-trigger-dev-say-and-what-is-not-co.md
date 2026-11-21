---
id: collect-261121-veille-ia/securite/stolen-ai-keys-and-ai-developer-platforms-what-wiz-northflank-and-trigger-dev-say-and-what-is-not-co
title: "Stolen AI Keys and AI Developer Platforms: What Wiz, Northflank and Trigger.dev Say, and What Is Not Confirmed"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["trigger", "northflank", "platforms", "stolen", "keys", "zeabur", "account", "platform"]
source: https://www.waveo.io/insights/llm-key-theft-ai-developer-platforms-decoded/
source_anchor: ""
source_lines: [1, 112]
section: "Securite"
---
**The short version**

Short answer: reported in part, not fully confirmed. Trigger.dev says an attacker reached environment variables of a subset of organizations; Northflank tells customers to assume theirs were compromised and relays user reports of unauthorised AI key use. Zeabur names plaintext admin credentials as its cause; Northflank and Trigger.dev have published none, and no single attacker is shown. Why it matters: stolen cloud AI credentials are charged to the account that owns them, and Wiz's two cases saw about 800 thousand and 1.7 million calls.

**In plain words**

An environment variable is a named setting that a platform passes to a running program. Developers often store passwords there. An AI key is one of those secrets: a long password that lets a program use a paid AI model, like a card for a vending machine that bills your account. Whoever holds the key can run the model, and the charges land on the account that owns it.

That is why a stolen key is a billing problem first. The reports describe stolen keys being tested within 30 minutes and then used in floods of calls. The one-line takeaway: know where your AI keys are stored, limit what one key can do, set an alert, and know how to revoke a key at once.

Basis: public sources read on November 4, 2026 - a Wiz research post, the status pages of Northflank and Trigger.dev, Zeabur's preliminary report, and a Google Threat Intelligence Group article. We tested nothing. Each claim is attributed to its source, and our recommendations are marked as ours. Two terms: an environment variable is a named setting that a hosting platform passes to a running program, where developers often keep passwords and keys so they are not written in the code; an AI key is a secret that lets a program call a paid cloud AI model, and in Wiz's two cases these were cloud credentials for Amazon Bedrock, with the charges landing on the account that owns them. If you commission or run AI agents in sales, logistics or retail, the pattern matters: the key your agent uses may be stored on someone else's platform. The exposure starts at the platform holding a secret; the abuse may happen at a different provider that accepts that credential and charges its owner (our summary). Nothing here says that you were affected. If you use a platform named below, check for its notice to your account owner.

## What did Wiz report about AI developer platforms in October 2026?

Wiz's post is dated October 30, 2026, with notes dated November 2 and 3. Its assessment is that a series of attacks on AI developer platforms has been leading to LLMJacking at scale. LLMJacking means stealing someone's access to AI models and using or reselling it, so the victim gets the bill. That is Wiz's assessment, not a platform-confirmed finding.

The headline count is "at least nine" platforms that disclosed incidents during the month: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank, with dates from October 6 to October 30. Wiz does not say whether each date is the disclosure or the incident. Later notes add three more: Brev and Trigger.dev on November 2, and Chainstack on November 3. For those three, Wiz says no attacker methods or traceable fingerprints had been shared. We read primary pages for only Northflank, Trigger.dev and Zeabur; the rest rests on Wiz's relay.

Wiz says all public reports highlight targeted exploitation that explicitly hunted stored environment variables, with a focus on those granting AI access, but no single vulnerability or dependency links the attacks. Most incidents were still being analyzed, and the platforms' own explanations differ: as Wiz relays them, three attribute theirs to credential access, one to privilege escalation in its build infrastructure, one to a third-party vendor dependency and one to a legacy system. Zeabur's own preliminary report, dated October 19, names a root cause - long-lived admin credentials stored in plaintext - and says how the attacker first obtained them cannot yet be confirmed.

## What have Northflank and Trigger.dev said themselves?

Northflank's status page is live; we read its entries from October 30 to November 2, 2026.

- **October 30.** The page lists a Wiz threat alert at 11:43 UTC and patches from 12:02 to 13:05. At 1:40 PM UTC the status read "Investigating", with the instruction to rotate environment variables immediately. At 1:58 PM it told customers to assume all environment variables had been decrypted. At 3:12 PM it said multiple users were confirming that their AI keys had been used, and by 11:38 PM it said users had reported unauthorised use of AI API keys. It says the reports may be consistent with the broader pattern in Wiz's report, which is not proof.
- **November 2.** Northflank said it believes the attack vector was contained within an hour of the first alert, that the evidence suggests a focus on secrets held in its control plane, and that running workloads were unaffected. A root cause analysis is planned.

Northflank's advice: rotate all secrets now, "even if you haven't seen suspicious activity", starting with AI provider keys. Revoke old credentials with their issuing providers, review your AI provider usage and billing logs, and contact your AI provider if spend looks wrong.

Trigger.dev's status entry, at 10:50pm UTC on November 1, 2026 and marked Resolved, says there has been a security incident, that the exploit used has been blocked, and that the investigation found exposure across a subset of organizations. The attacker gained unauthorized access to environment variables for those organizations; all impacted account owners were emailed steps to mitigate, and more details will follow. The page gives no count, does not say what the variables were or whether AI keys were among them, and does not describe the exploit.

## What does a stolen AI key do next, according to Wiz?

Wiz says it observed downstream abuse of enterprise credentials whose compromise it attributes to one of these vendor breaches. It describes two cases and names neither the vendor nor the customers; these were cloud model credentials with an access policy, not a key to a single chat product. From Wiz's account:

- **Fast first test.** The first unauthorized activity came within 30 minutes of the reported credential access at the vendor: one model call, from a residential IP address, in a one-minute sweep testing stolen keys across organizations.
- **Quiet checking.** Then 14 to 120 calls a day, which Wiz infers was a job confirming the keys stayed valid.
- **Then a flood.** In Case A, a key with a permissive policy saw approximately 800 thousand calls over thirty hours, about 90 percent successful, before it was remediated. About three quarters of the successful calls went to one family of top-tier models, according to Wiz.
- **A restricted key still got hit.** In Case B the key was restricted: Wiz says its policy allowed the cloud AI service only through certain regional profiles, and the first calls from an apparent reseller pool failed. Still, it saw approximately 1.7 million calls over thirteen hours, over a million successful; Wiz says nearly all the rest hit the account's regional quotas. The restriction did not prevent the abuse.

Wiz says the same script probed both keys. Its estimate is that the two cases alone likely ran up over $100k in usage, at list price - two cases combined, not an invoiced figure. On who benefits, Wiz is careful. It linked the key usage to overt reselling organizations but says it cannot definitively establish their relationship to either the initial compromise or the flood. Wiz also describes one self-named operator, tied to a resale site, that had access to the keys in both Case A and Case B; Wiz says it cannot definitively tie that operation to the flood. Wiz says one address that Zeabur named in its own report was seen checking the Case A and Case B keys.

## What does Google's threat team add about stolen AI access?

The Google Threat Intelligence Group and Mandiant article, dated November 2, 2026, covers Mandiant's own 2026 incident responses. It does not mention Wiz or the platforms above or say its cases relate to them, so we treat it as separate evidence.

- **Scope and size.** Mandiant saw attackers hijack enterprise access to AI models in financial services, retail, technology and transportation. GTIG says charges can spike within hours: across individual incidents, above $100,000 in a single day, and cumulative charges reaching $1 million at some organizations before access was revoked.
- **How attackers got in.** Across multiple investigations, GTIG says the dominant initial access vector has been exposed or stolen cloud identity tokens, a different route from a platform breach.
- **The market.** GTIG calls resale sites "LLM Mixers": gateways that pool and resell unauthorized access. In one case, an actor's relay passed nearly one million dollars in token costs to a victim organization, while its service advertised an unlimited token plan for approximately $60 a month with alleged access to frontier models. GTIG believes the service relies on stolen keys to operate profitably - its inference.

## Stolen AI Keys: Confirmed Facts vs. Unverified Claims

| Claim | Status on November 4, 2026 | Source | 
|---|---|---|
| Trigger.dev: an attacker reached environment variables of a subset of organizations | Confirmed by the company; no count, AI keys not named | Trigger.dev status page | 
| Northflank: users reported unauthorised use of AI keys to the company | Reported by users and relayed by the company; no count, no root cause yet | Northflank status page | 
| Zeabur: root cause was long-lived admin credentials stored in plaintext | Stated in Zeabur's preliminary report; how the attacker first got them is unconfirmed | Zeabur report | 
| At least nine platforms disclosed incidents in October, three more added on November 2 and 3 | Wiz's list; we read three of the platforms' own pages | Wiz | 
| Stolen keys were tested within 30 minutes, then used in floods of calls | Observed by Wiz in two cases; vendor not named | Wiz | 
| The two cases likely ran up over $100k | Wiz estimate, at list price, two cases combined | Wiz | 
| One group, one campaign or one flaw is behind the platform incidents | Not established; Wiz says no single vulnerability or dependency links them | Wiz | 
| Resale sites stole the keys or ran the floods | Not established; Wiz cannot definitively tie them to either | Wiz | 

## What is the lesson for people who build or buy agents?

These are our recommendations, not the sources'. The question is where your AI keys live, what one key can do, and how fast you would notice.

- **A key stored with a third party is only as safe as that party.** Both status pages tell customers that stored environment variables were or may have been exposed. Know every platform, workflow tool and agent platform that holds a key for you.
- **Assume testing comes first.** Wiz saw a test call within 30 minutes. An incident notice is the moment to rotate, as Northflank advises, not to wait for a bill.
- **Limit what one key can do.** GTIG advises scoping keys by model, environment and usage limit, and setting spending caps and per-model quotas. In Wiz's Case B the restricted key still saw over a million successful calls: a scope is a limit, not a lock. A billing alert is a warning, not a brake: AWS says budget data updates up to three times a day, typically 8-12 hours apart, and Google says an alerts-only budget does not cap spending (spend-cap budgets exist for some services). Quotas and scoped keys act in real time; revoking the key at the provider is the fastest stop.
- **Watch the logs.** GTIG advises alerts for spikes in certain error types, tiny test requests and an AI service switched on unexpectedly.

Our guide to production agent architecture covers where such limits belong; our decode of the OpenAI and Hugging Face agent breach is a related case.

Do this today

Three steps that take an afternoon and need no developer.

1. List every AI and cloud key your agents use, and write down where each one is stored, including hosting, workflow and automation platforms.
2. Set a billing alert and a per-key quota on each cloud AI account, and write down how to revoke a key at once; an alert is only a warning.
3. Rotate any key that sat in a hosted platform's variables after an incident notice, and revoke the old one at the model provider.

## What should you ask a platform vendor or your own team?

- Which AI and cloud keys do our agents use, and which platforms hold them as variables or secrets?
- What quota, scope and enforced limit sit on each account those keys belong to, and do we know the budget alert is only a warning?
- Is there one key per agent and per environment, limited to the models it needs?
- If a platform tells us tonight that variables were exposed, who rotates which key and who revokes the old one at the provider?
- If a credential is stolen tonight, who can revoke it at the provider within minutes, and can we do it without waiting for the next billing report?
- Are usage logs on for model calls, and who gets the alert?

## What does this mean if you run sales, logistics or retail?

You may never use these platforms, but a quote, shipment-status or catalog agent calls a cloud model with a key, and that key lives somewhere. See also our teardown of agent-desktop on what an agent can reach, and our decode of the Cloudflare Workers AI KV cache leak. Ask whoever hosts your agents: where is the key, what can it spend, and would you find out within hours?

## What Is Not Confirmed

No source attributes all the platform incidents to one actor or shows they are one coordinated campaign; Wiz says it cannot definitively tie the resale-linked operator to the compromise or the flood. Some platforms have published their own causes, such as Zeabur, but at Northflank and Trigger.dev root causes are still open: Northflank plans a root cause analysis and Trigger.dev says it will follow up. We do not know how many customers or variables were exposed at either, or which variables were taken, and Trigger.dev has not said whether any customer's AI keys were abused. Wiz does not name the vendor behind its two cases. Total losses are unknown. This article reflects what was public on November 4, 2026; the sources may change again.

Related: another way keys end up in the open, saved Claude Code approval rules that Apiiro found holding secrets in public GitHub repositories, in our decode of the Apiiro report.

Related: another place secrets can end up in the open, AI chat text that Notion says may have been exposed in a Fireworks AI security incident, in our decode of the two notices.

## Sources

- Wiz Blog: "Black October" for AI Developer Platforms, October 30, 2026 (notes dated November 2 and 3), read November 4, 2026
- Trigger.dev status: Security Disclosure, Sunday November 1st, entry of November 1, 2026, read November 4, 2026
- Northflank status: Security incident, entries from October 30 to November 2, 2026, read November 4, 2026
- Zeabur: October 2026 security incident (preliminary report), October 19, 2026, read November 4, 2026
- Google Cloud Blog (GTIG and Mandiant): The Rise of LLMJacking, November 2, 2026, read November 4, 2026
- AWS documentation: Managing your costs with AWS Budgets, read November 4, 2026
- Google Cloud documentation: Create, edit, or delete budgets and budget alerts, read November 4, 2026
