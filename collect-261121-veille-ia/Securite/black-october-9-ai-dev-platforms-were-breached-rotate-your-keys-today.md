---
id: collect-261121-veille-ia/securite/black-october-9-ai-dev-platforms-were-breached-rotate-your-keys-today
title: "Black October: 9 AI Dev Platforms Were Breached. Rotate Your Keys Today"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["keys", "october", "access", "stolen", "about", "platforms", "incidents", "attackers"]
source: https://dev.to/lycore/black-october-9-ai-dev-platforms-were-breached-rotate-your-keys-today-35d3
source_anchor: ""
source_lines: [1, 101]
section: "Securite"
---
**TL;DR:** In October 2026, at least nine AI developer platforms disclosed security incidents, according to a Wiz research post published on October 30. The common thread is that attackers went after stored environment variables, especially the ones that unlock AI models. Stolen keys were tested within about 30 minutes and later resold. If you have an OpenAI, Anthropic, Bedrock or similar key sitting in a hosting or dev platform, rotate it today. Then put a spend cap on it and narrow its scope. There's a checklist below.

Any other month, one AI platform breach would have been the security story of the week. This month there were nine, and Wiz's researchers had to point out that hardly anyone was connecting them. They called it "Black October".

I'm writing this for developers and small teams, not security specialists. Most of us have pasted an LLM API key into a platform dashboard at some point because it was the quickest way to get a demo running. This post covers what's confirmed so far, what attackers did with the keys, and what you can do in the next hour.

This is Wiz's list with the disclosure dates they give:

| Date (Oct 2026) | Platform | 
|---|---|
| Oct 6 | Fireworks AI | 
| Oct 9 | Andromeda | 
| Oct 14 | Zeabur | 
| Oct 15 | Lightning AI | 
| Oct 16–17 | OpenCode | 
| Oct 20 | Railway | 
| Oct 23 | Warp | 
| Oct 28 | CommandCode | 
| Oct 30 | Northflank | 

What's known about each one varies a lot. Most of these incidents are still under investigation, and Wiz says detailed technical information mostly isn't public yet. Here's what the post does say, attributed to the companies:

- 
**Fireworks, OpenCode and Warp** reported signs of an "agent swarm" attack.**Andromeda** called its incident part of an "industry-wide attack."
- 
**OpenCode and CommandCode** explicitly attributed their incidents to an attempt at "distillation" (using stolen model access to train or copy models).
- 
**Fireworks, Zeabur and Lightning AI** attribute their root causes to credential access. Wiz notes that Lightning AI had previously been hit by a supply chain attack in the Mini-Shai Hulud campaign.
- 
**Railway** has released a preliminary report and says it believes initial access came through "privilege escalation in our build infrastructure."
- 
**Warp** has suggested the root cause was a third-party vendor dependency, and**OpenCode** points to a "legacy system."
- 
**Zeabur** has published a full write-up and, according to Wiz, will shut down because of the incident.

**There's no single shared vulnerability.** Wiz says no single vulnerability or dependency links these attacks. The shared pattern is what the attackers wanted: every public report describes targeted hunting for stored environment variables, with a focus on those that grant AI access.

**Unconfirmed:** whether one group is behind all nine incidents. Wiz doesn't claim that, and neither do I.

Wiz ties the campaign to two trends. Agentic attacks let attackers compromise targets faster. LLMJacking is the other: stolen model access gets resold through "LLM broker" services. The resale market is what turns a stolen key into money quickly, and that's why so many people are now hunting for these keys.

Wiz also traced downstream abuse of two enterprise keys, which it could attribute to one of the vendor breaches. Both cases are worth reading in full. These are the numbers that matter for your own setup:

- 
**Validation came fast.** In both cases, the first unauthorised call came within 30 minutes of the reported credential access at the vendor. It was a single model call from a residential IP, made during a one-minute sweep that tested stolen keys across multiple organisations.
- 
**Case A (unrestricted key):** Over about 30 hours, until the key was remediated, the actor made approximately 800,000 Bedrock calls, about 90% of them successful. The key's policy allowed`global.*` cross-region inference profiles, which spread the load across regions, so only about 6% of calls were throttled.
- 
**Case B (restricted key):** This key only allowed Bedrock through`us.*` inference profiles. The reseller pool asked for`global.*` , so thousands of calls failed at first. Then a checker found the working`us.*` model IDs, and over about 13 hours a single address made approximately 1.7 million calls, over a million of them successful. Most of the rest were throttled by the account's regional quotas.
- 
**The cost:** At list price, Wiz estimates these two cases alone likely ran up over $100k in usage.

Case B doesn't show that restrictions save you. They didn't stop the abuse. They slowed it, and the regional quotas limited it. That's the right way to think about scoping: it limits how much damage a stolen key can do, and you still have to rotate fast.

Wiz also mentions anecdotal impact reports from social media: alleged theft of about $371,000 in cryptocurrency using crypto keys stored in Railway, and stolen AI access (for example from OpenCode) producing consumer bills in the hundreds to thousands of dollars. Treat these as reports, not confirmed totals.

Northflank, the most recent platform on the list, put out the clearest customer guidance I've seen this month. Its status page told every customer to **assume all environment variables have been decrypted** and to rotate everything: secrets on workloads, secret groups and global secrets. It also recommended disabling public access to database addons, and said multiple users had confirmed their AI keys had been used. A later update told customers to revoke credentials with the issuing provider (not just change them in Northflank) and to check AI provider usage and billing logs for unexpected activity.

You don't have to be a Northflank customer for this to apply. If a platform that stores your env vars discloses an incident, the safe assumption is that every value in those env vars is gone. You rotate the keys first and investigate later.

Start with every platform that holds an AI key: hosting, CI, notebook services, agent platforms and coding tools.

1. 
**Make the inventory.** List every AI key (OpenAI, Anthropic, Bedrock/IAM, Google, Mistral, OpenRouter and so on), and note which platform and which environment holds it. If you can't say where a key lives, assume it's on one of these platforms.
2. 
**Create the replacement first.** Generate a new key at the provider, update the platform secret and redeploy. Confirm the app works.
3. 
**Revoke the old key at the provider.** Deleting it from the platform isn't enough, because the attacker has a copy. Revocation has to happen where the key was issued.
4. 
**Do the non-AI secrets too.** Database URLs, payment keys, cloud credentials, webhook signing secrets and crypto wallet keys. Northflank's "rotate everything" applies here, and the Railway reports show attackers take whatever they find.
5. 
**Read your usage and billing logs.** Look for unfamiliar models, regions or IPs, traffic at odd hours, or a sudden jump in output tokens. In Wiz's cases there was only a trickle of validation calls for days before the flood, so a small amount of strange usage still matters.
6. 
**Report abuse.** Tell the provider and the platform. Some providers can help with fraudulent usage if you report it quickly.
7. 
**Write down the date.** Add each rotation to your secrets inventory, so the next incident takes ten minutes, not an afternoon.

A stolen AI key works like a payment card with no limit. So put a limit on it.

- 
**Set the hardest spend limit your provider offers.** Most AI provider consoles have per-project or per-workspace budgets, limits or alerts. A hard limit is better than an alert, and an alert that wakes someone up beats a monthly invoice. Set it close to your real usage, not 10x above it.
- 
**One key per app, per environment.** Production, staging and that weekend prototype shouldn't share a key. When one leaks, you rotate one key and you can see exactly whose usage it was.
- 
**Restrict models and regions.** On Bedrock, use IAM policies that allow only the specific models and inference profiles your app calls. Case B shows the effect: abuse was delayed and capped by regional quotas instead of spreading worldwide.
- 
**Use short-lived credentials where you can.** Workload identity or OIDC role assumption for cloud model access means there's no long-lived key in an env var to steal.
- 
**Keep keys out of the platform where possible.** If a platform supports pulling secrets from a dedicated secrets manager at runtime, use that, so the platform holds a reference instead of the value.

On our own client projects we keep production AI keys in the cloud provider's secrets manager rather than pasting them into every dashboard. Each environment gets its own key, and each provider account gets a budget alert. When a platform we use discloses an incident, we rotate first and investigate second. This month, the habit that made the biggest difference was keeping a written list of where every key lives. Rotating takes minutes. Working out where the keys are is the slow part.

- Whether the incidents share an operator. Wiz links resale infrastructure to keys from at least one breach, but says it can't definitively tie the resellers to the initial compromises.
- Root causes for most of the nine platforms. Only Zeabur has a full write-up and Railway a preliminary report, according to Wiz.
- How many customer keys were taken in total. No platform has published that number yet.

I'll update this post if the platforms publish more detailed reports.

Be honest: how many dashboards hold a copy of your OpenAI or Anthropic key at the moment? Have you set a spend limit on any of them? Tell me in the comments. I'm curious whether this month has changed how anyone stores these keys.

## Top comments (0)
