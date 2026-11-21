---
id: collect-261121-veille-ia/securite/black-october-ai-developer-platforms-breached-matt
title: "Black October: AI developer platforms breached | MATT"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "platforms", "railway", "breached", "nine", "keys", "black", "developer"]
source: https://www.matt.security/blog/black-october-ai-developer-platforms
source_anchor: ""
source_lines: [1, 159]
section: "Securite"
---
**In October 2026 attackers breached at least nine platforms that developers use to build and run AI apps, and in each case they went for the same thing: the stored settings that hold customers' AI keys.** Wiz, which traced the keys into use, calls the month Black October. It found no single vulnerability or dependency connecting the nine.

The same month a poisoned release of LiteLLM, stolen GitHub tokens and infected hosting panels hunted the same secrets. A stolen AI key turns straight into compute on someone else's bill, and in the two cases Wiz traced, that bill likely passed $100,000 within days.

## The 30‑second version

- →**What happened:** between 6 and 30 October, Wiz counts at least nine AI developer platforms breached: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank.
- →**What was taken:** stored environment variables, the settings where platforms keep their customers' secrets, with a focus on those that grant access to AI models.
- →**How fast:** in one case Wiz traced, the stolen keys were first used within 30 minutes of the reported theft of credentials at a platform.
- →**What it cost:** two cases Wiz traced likely ran up more than $100,000 at list price; OpenCode users reported overnight charges of up to $1,700.
- →**Who:** unknown. Railway believes its attacker wanted AI keys for distillation, training another model on a stronger model's answers.
- →**Around it:** a malicious LiteLLM release, a campaign of stolen GitHub tokens that Wiz calls Gheist, and compromised server panels at hosting providers hunted the same kind of secret. No investigator has linked them to the nine.

## Nine platforms in 25 days

Wiz, the cloud security company, published its account on 30 October as Black October ↗. It lists at least nine AI developer platforms breached that month, from Fireworks AI on 6 October to Northflank on 30 October, and one common thread: attackers "hunting stored environment variables, with a focus on those granting AI access". These platforms host apps, run builds or serve models, and their customers store there the keys their software uses to call AI models.

- →**Fireworks AI, 6 October:** detected the breach that day and published a bulletin on 9 October, according to the Cloud Security Alliance ↗.
- →**Andromeda, 9 October:** listed by Wiz.
- →**Zeabur, 14 October:** publicly named an attacker IP address that Wiz ties to an AI API resale service; Wiz says Zeabur is shutting down.
- →**Lightning AI, 15 October:** listed by Wiz.
- →**OpenCode, 16–17 October:** AI provider keys stored in its hosted console by the 1.4% of console users who had linked an outside provider were exposed and used to run up charges (OpenCode ↗).
- →**Railway, 20–22 October:** build infrastructure, then CI; a subset of encrypted variables decrypted, in at most 1.85% of workspaces (Railway ↗).
- →**Warp, 23 October:** an attack on one of its vendors reached AI agent traces and telemetry for up to about 1% of users (Warp ↗).
- →**CommandCode, 28 October:** listed by Wiz.
- →**Northflank, 30 October:** told customers to assume all environment variables had been decrypted and to rotate every key (Northflank ↗).

The dates are Wiz's; Warp disclosed its incident on 26 October.

OpenCode and Warp are on the Record, and our piece on three side doors covers them in detail. Railway and Northflank have published the most about how the attacks unfolded.

## Railway: from the build system to the variables

Railway, a hosting platform with about 5 million users, published a preliminary report ↗ on 22 October and updated it on 26 October. Its on-call page fired at 13:58 UTC on 20 October, it became aware of the campaign at 19:53 UTC, and the attack ran until 03:32 UTC on 22 October. Railway cites "potential privilege escalation in our build infrastructure by the attackers". On 21 October, between 05:27 and 07:25 UTC, the attacker "accessed credentials to access our CI environment", and an unauthorised query of encrypted variables ran, "of which a subset were decrypted".

Railway puts the exposure at "at most 1.85% of all workspaces", about 100,000 workspaces, and asked GitHub, GitLab, Anthropic, OpenAI, OpenRouter, Stripe and others to reset exposed keys. It paused new sign-ups. It says it is "unable to refund customers for spend performed with compromised API keys", and offers to take fraud claims to the providers on customers' behalf.

Railway's first disclosure ↗ called the attack "part of a series of attacks recently targeting platform companies", "with the aim of compromising AI inference providers". Its report says the attacker was "attempting to obtain AI provider API keys to perform distillation attacks": using a strong model's answers to train another model, which the major providers' terms prohibit. Railway adds that initial evidence suggests the attacker "had access to abliterated models and substantial compute resources"; an abliterated model is a copy with its refusals removed. Wiz cites social-media reports, unconfirmed, of about $371,000 in crypto stolen using keys stored on Railway.

## Northflank, hour by hour

Northflank's status page ↗ shows how fast a platform has to move. On 30 October, in UTC:

- →**11:43** a threat alert arrives from Wiz.
- →**12:02** Northflank patches a network policy.
- →**13:00** it asks Wiz's incident response team for help.
- →**13:58** "Assume all environment variables have been decrypted. Disable/rotate all keys."
- →**15:12** "Multiple users are confirming that their AI keys have been used."
- →**19:28** no further confirmed findings.
- →**23:38** it links Wiz's Black October post: "Similar IP addresses are contained in this article".
- →**01:04 on 31 October** audit logs are switched on for all customers, including free users.

Northflank told customers to rotate every key 2 hours 15 minutes after the alert. In the case Wiz traced from the start, the stolen keys were tested within 30 minutes.

## What happens to a stolen key

Wiz ↗ followed keys from one of the breaches into Amazon Bedrock, the AWS service that serves AI models including Anthropic's. The first call came "within 30 minutes of the reported credential access within the vendor breach": a single request from a home internet address, part of a one-minute sweep that tested stolen keys across several organisations. It asked for Claude Fable 5.1, failed, and fell back to Opus 5.5, which worked.

Then came volume. In one case Wiz counts about 800,000 Bedrock calls over 30 hours, about 90% of them successful, most of them to Anthropic's Opus models; in another, about 1.7 million calls over 13 hours, more than a million of them successful. Wiz estimates the two cases "likely ran up over $100k" at list price.

Wiz also found two services that resell access to AI models, which it calls momo and Xingjiabi, holding the stolen keys and testing them. It says it cannot "definitively establish the relationship" between the resellers and the breaches, and notes that Zeabur publicly named momo's IP address as part of its own compromise. Wiz's subtitle calls the month "a series of agentic attacks on AI Developer Platforms", meaning attacks run with AI agents; its text is more careful, calling the picture "strongly aligned with" reports of agentic attacks.

## The same month, the same prize

Three more attacks in October went after the same kind of secret. No investigator has tied any of them to the nine platforms; the Cloud Security Alliance lists LiteLLM as "related; unlinked".

**A key nobody revoked.** LiteLLM is an open-source gateway that puts many AI providers behind one key, which makes it a store of provider keys. On 21 October a malicious version, 1.107.0, was live on PyPI from 11:39 to 13:23 UTC, and in its Docker Hub images until 14:20, according to LiteLLM ↗. The payload, a 16-module stealer, took "every provider key stored in the LiteLLM proxy DB" along with cloud credentials. Wiz ↗ counts about 49,000 downloads of the malicious version and sees "strong indications of LLM-aided development" in its code. About four and a half hours after the data left, stolen AWS credentials were used to list the AI models available on Bedrock.

LiteLLM's attack-chain post ↗ traces the way in to 27 May, when a configuration file committed to a public repository "inadvertently contained an administrative key to an internal deployment". The file was deleted about three hours later, but it stayed in the repository's public history until 22 October, and "nobody followed through on revoking the administrative key". On 10 October the key was used to obtain an organisation owner's GitHub command-line token; eleven days later the malicious release went out. LiteLLM's first theory, that tokens leaked through its logs, has been superseded.

**Stolen GitHub tokens.** Wiz ↗ has tracked a campaign it calls Gheist since 15 September: more than 13,500 branches planted from more than 850 compromised GitHub accounts, using tokens from infostealer logs and from earlier supply-chain compromises such as Trivy's in March. A variant added on 19 October runs on self-hosted build machines and searches for 31 token formats, including keys for AI providers such as Anthropic, Hugging Face, Groq and NVIDIA. Wiz notes the job is buggy.

**Hosting panels.** On 18 October Gcore, a cloud and hosting provider, disabled its control panels ↗ after finding a vulnerability in third-party software; on 20 October it said attackers had exploited a vulnerability in third-party virtualization software supplied by its vendor, which it has not named. It says it has found no evidence that customer data was moved outside its infrastructure. The same day ISPsystem, which makes the VMmanager server panel, published a notice ↗ about a vulnerability in a VMmanager 6 build. ForestsNet, a hosting provider, wrote ↗ that attackers infected panels from 11 to 16 October before the main attack on the night of 16 to 17 October, and that at some providers a collector searched customers' disks for SSH keys, .env files and "AI service API keys"; it found no trace of that on its own servers.

## One campaign, or one target?

Wiz says there is "no single vulnerability or dependency linking these attacks". What links them is the target. The Cloud Security Alliance's research note ↗ of 23 October names the pattern platform-chain attacks, which "reach an AI inference account by going through one or more platforms" that hold its credentials. It differs from a software supply-chain attack, CSA says, because "the platform's legitimate custody of customer secrets is the asset being exploited": nothing the customer installs has to change. CSA adds that the campaign framing so far comes from Railway alone.

Our reading: whether one group did all of this or several did, the target list is the same. Anywhere an AI key sits as a stored setting, on a platform, in a build machine, in a gateway or on a disk, is worth breaking into, because the key turns into compute the moment it is read.

## Claims to stop repeating

- →**"The AI labs were hacked."** The keys were taken from platforms that stored them. No investigator has reported a breach at OpenAI, Anthropic, Google or Amazon in this campaign.
- →**"The resellers sold the stolen keys."** Wiz says both had access to the keys and tested them, and that it cannot establish how they relate to the breaches.
- →**"LiteLLM leaked Claude Code tokens."** That was LiteLLM's first theory, which its attack-chain post superseded; the way in was an admin key left in a public repository's history.

## What to check this week

These questions apply to any team that runs software with an AI provider key.

- →**Which platforms hold your AI keys?** List every hosting platform, CI service, AI gateway and coding tool that stores a provider key. Each one is a way into your AI account.
- →**What is the most a key can spend?** Set hard budget limits and usage alerts at each provider, and on Amazon Bedrock alarm on call volume. Wiz saw about 1.7 million calls in 13 hours in one case.
- →**Is each key scoped to one job?** Use one key per platform and per environment, limited to the models it needs, so one breach exposes one key.
- →**Can the key go away altogether?** Where a provider supports it, use workload identity, short-lived credentials issued to a running service, such as IAM roles for Bedrock or OIDC federation from CI, so there is no stored secret to copy.
- →**Who can decrypt your platform's variables?** Ask each platform how environment variables are encrypted, which of its systems can decrypt them, and whether you get audit logs. Our piece on three side doors has more questions to put to vendors.
- →**How fast can you rotate?** After a platform's notice, rotate within the hour; in the case Wiz traced, stolen keys were tested within 30 minutes.
- →**What did you install on 21 October?** Check lock files, images and caches for LiteLLM 1.107.0, and rotate every key on any machine that ran it. Pin dependencies by hash and wait a few days before adopting a new release; the malicious version was live for under two hours.
- →**What is in your git history?** A deleted file stays in history. Scan it, and revoke any key you find; LiteLLM's sat in public history for five months.
- →**Which tokens can push to your repositories?** Use fine-grained, expiring GitHub tokens, keep self-hosted build machines off public repositories, and watch for branches nobody created.

## Rules that hold or break

Each of those checks can be written as a rule: every AI key has a spend limit; no AI key is shared between two platforms; a key found in git history is revoked the same day. Written down, a rule can be re-proven on every release, holding or broken. That is the practice ExploitOps describes, and the rules page shows how rules are written. For the same pattern in crypto, where a fake job interview and an internal AI tool led to a hot wallet's signing service, see the $6.3 billion job interview.

## What is still unknown

- →Who is behind the nine breaches, and whether it is one group.
- →How attackers got into most of the platforms; Railway and LiteLLM have published the most on how.
- →How many keys were taken, and how much was spent with them.
- →Whether the resale services sold access paid for with stolen keys.
- →Whose software was exploited at Gcore, and how many hosting providers were hit.

We will update this piece as the platforms publish more.

## Sources

- →Wiz, Black October: a series of agentic attacks on AI developer platforms ↗, 30 October 2026
- →Railway, security disclosure ↗, 21 October 2026, and preliminary report ↗, 22 October 2026, updated 26 October
- →Northflank, incident status page ↗, 30–31 October 2026
- →OpenCode, on X ↗, 17 October 2026; TechCrunch, on OpenCode ↗, 19 October 2026
- →Warp's CEO Zach Lloyd, on X ↗; BleepingComputer, on Warp ↗, October 2026
- →LiteLLM, security incident ↗ and attack chain ↗, October 2026; Wiz, LiteLLM compromised ↗, 21 October 2026
- →Wiz, Gheist ↗, 25 October 2026
- →Gcore, incident status page ↗, 18–26 October 2026; ISPsystem, VMmanager 6 notice ↗, 20 October 2026; ForestsNet, VMmanager incident ↗, 18 October 2026
- →Cloud Security Alliance, Inference providers as the prize ↗, 23 October 2026

The logos are each company's own published versions. They identify the companies named in their public statements and in Wiz's research; MATT has no affiliation with any of them.

## Questions

## ### What is Black October?


Wiz's name for a series of breaches in October 2026 at AI developer platforms, at least nine between 6 and 30 October: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank. In each, attackers went after stored environment variables, with a focus on keys that grant access to AI models. Wiz found no single vulnerability or dependency linking them.

## ### Why do attackers want AI API keys?


A stolen key runs AI models on its owner's bill. Railway believes its attacker wanted keys for distillation, training another model on a stronger model's answers. Wiz traced two cases that likely ran up more than $100,000 at list price on Amazon Bedrock, and found AI API resale services testing stolen keys.

## ### What happened at Railway?


Railway says the attack began with a potential privilege escalation in its build infrastructure on 20 October, reached its CI environment on 21 October and decrypted a subset of customers' encrypted variables, in at most 1.85% of workspaces, about 100,000. It believes the attacker wanted AI provider keys for distillation, paused new sign-ups, and says it cannot refund spend on compromised keys.

## ### Was LiteLLM part of Black October?


No investigator has linked it. A malicious LiteLLM release, 1.107.0, was on PyPI for under two hours on 21 October and stole provider keys and cloud credentials from machines that installed it, with about 49,000 downloads according to Wiz. LiteLLM traces it to an admin key committed to a public repository in May and never revoked.

## ### What should I do if my AI key was stored on an affected platform?


Rotate it, check the provider's usage and billing for calls you did not make, and set spend limits and alerts. Ask the provider about refunds: Railway says it cannot refund spend on compromised keys but will take fraud claims to providers, and OpenAI said it would refund credits used in the OpenCode attack.

## ### How do I protect AI keys from platform breaches?


Use one key per platform and environment, limited to the models it needs; cap spend; use workload identity where a provider offers it; keep keys out of git history; pin dependencies; and ask each platform which of its systems can decrypt your environment variables.
