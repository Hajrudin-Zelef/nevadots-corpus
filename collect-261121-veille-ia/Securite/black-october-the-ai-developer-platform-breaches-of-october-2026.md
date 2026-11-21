---
id: collect-261121-veille-ia/securite/black-october-the-ai-developer-platform-breaches-of-october-2026
title: "Black October: The AI Developer Platform Breaches of October 2026"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-31"]
keywords: ["october", "month", "keys", "platform", "about", "black", "fireworks", "warp"]
source: https://www.helix.ml/blog/black-october-ai-platform-breaches
source_anchor: ""
source_lines: [1, 183]
section: "Securite"
---
# Black October: Nine AI Platforms Breached in One Month

Oct 31, 2026

Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, Command Code and Northflank all disclosed incidents in October 2026. What the attackers were after (your environment variables), what two stolen Bedrock keys did next, and what to rotate before November.

**In short.** In October 2026 at least nine platforms that developers use to
build and run AI software disclosed security incidents. Wiz called the month
"Black October".
No single vulnerability or dependency links them. What links them is the target:
the environment variables customers store on the platform, and above all the AI
provider keys inside them. Those keys were validated within half an hour and
resold through LLM reseller pools. One stolen key took about 1.7 million Bedrock
calls in 13 hours. If you kept a secret on any of these platforms this month,
rotate it at the issuer tonight. Deleting it from the platform does not revoke it.
Every incident below has a curated record on Threat Watch.

It is Halloween, and the scariest thing on a developer's screen this month was not
a jump scare. It was a status page saying *assume all environment variables have
been decrypted*.

That line is from Northflank, posted at 13:58 UTC on 30 October, two hours and a quarter after a Wiz threat alert fired. It was the ninth such page this month. We spent the week checking each one against its primary source for Threat Watch. Here is how the month went, what the attackers were actually doing, and what to do about it.

## The calendar

Nine platforms, nine different stories:

- **Fireworks AI** (detected 6 October). Someone using stolen*internal* Fireworks credentials
read the environment variables attached to evaluation jobs in a limited number
of customer accounts. Fireworks believes API keys and tokens stored there as
secrets were captured.
- **Andromeda** (activity from about 9 October). Some of the company's own credentials and
some customer credentials were exposed. Its notice was the first to call this
"an industry-wide attack".
- **Zeabur** (14 October). This is the most detailed write-up of the month, and the worst
outcome. More on it below.
- **Lightning AI** (15 October). The Studio platform went fully offline after unauthorized
credential access. This is Lightning's second incident of 2026. In April, the`lightning` PyPI package was trojanized for 42 minutes
in the Mini Shai-Hulud campaign. Wiz notes the history; nobody has shown the
two incidents share a cause.
- **OpenCode** (17 October). Attackers reached an older system behind the hosted Console and
may have read the Anthropic, OpenAI and Google keys of about 1.4% of Console
users. Victims reported overnight charges from a few hundred dollars up to
about $1,700.
- **Railway** (20 October). The attacker escalated privileges in Railway's build
infrastructure, then used compromised CI credentials to decrypt a subset of
customer environment variables. Railway puts the exposure at up to 1.85% of
workspaces, about 100,000, and says the attacker was "attempting to obtain AI
provider API keys to perform distillation attacks".
- **Warp** (23 October). An unnamed third-party vendor in Warp's infrastructure was
exploited, exposing agent decision logs and telemetry for roughly 1% of users.
Warp has not said whether those logs contained code, prompts or credentials.
- **Command Code** (28 October). It revoked every API key after what it called a large-scale
distillation attack on Anthropic and OpenAI models. It says it has no evidence
of user impact yet.
- **Northflank** (30 October). Within hours, multiple users confirmed that their AI keys had
been used. Northflank linked its incident to Wiz's report, noting that
"similar IP addresses" appear in it.

Three of them (Fireworks, OpenCode and Warp) reported signs of an "agent swarm", an attack run by many cooperating AI agents rather than a person at a keyboard. None has published the evidence yet, so treat that as a lead rather than a finding.

## The haunted house was the environment variable

Every platform on that list asks you for the same thing: paste your secrets here and we will inject them at runtime. It is convenient, and it means each platform holds the master keys of everyone who builds on it.

Zeabur's incident report shows exactly what that costs. Starting at 01:36 UTC on 14 October, an attacker used long-lived admin tokens and cloud keys that Zeabur had left in plaintext in a configuration repository. The cluster's admin interface was reachable from the internet. Over the next two days the attacker:

- read 3,218,965 environment-variable records covering 321,810 accounts
- ran commands inside 865 customer database containers
- signed in as root on 101 dedicated servers
- joined the internal network and queried the platform database

Zeabur only noticed 36 hours in, because a user reported it. No alert fired. The attacker "mainly looked for API keys they could use directly, especially keys for AI services", including the n8n credential stores and new-api/one-api key pools that customers ran on the platform. The attacker also tried Amazon Bedrock with Zeabur's own AWS keys; every call failed.

The credentials had survived a rotation after an August incident. Three days after the intrusion, Zeabur announced it is shutting down. All services stop by 31 December.

The lesson is not about Zeabur. A platform environment variable is a secret shared with everyone who can reach that platform's control plane. That includes its build system, its CI, its vendors and its admin tokens. Railway's attacker went in through the build system. Warp's went in through a vendor. Fireworks' attacker used the vendor's own internal credentials.

## Why AI keys: the reseller economy

A stolen database password has to be used against a specific database. A stolen
AI provider key can be sold to anyone in the world, by the hour. That is the
business model behind **LLMjacking**: harvest keys, check them, pool them behind
an OpenAI-compatible API, and sell cheap access, often paid in USDT.

**Distillation** is the demand side. Distillation means training a smaller model
on a frontier model's outputs, and it takes a lot of frontier-model calls that
somebody else pays for. OpenCode, Railway and Command Code all named it as the
motive. Whether every buyer of those keys was distilling, or simply wanted cheap
coding-agent tokens, the result for the victim is the same bill.

## Following two stolen keys

Wiz followed two stolen enterprise AWS Bedrock keys from platform breaches through to their abuse. This part of the report deserves the most attention.

The pattern repeated in both cases:

1. **Validation, within 30 minutes** of the reported vendor credential access.
A single`InvokeModel` call came from a residential IP. It tried Claude Fable
5.1 first, which failed, then fell back to Opus 5.5, which worked.
2. **Quiet checks for days.** A small Go client made 14 to 120 calls a day,
which looks like a job confirming the key still works.
3. **Enumeration.** A catalogue probe across model vendors, then a`boto3` script
that read Bedrock and EC2 service quotas. The same script appeared in both
cases.
4. **The flood.** In Case A, bulk traffic began about 22 seconds after the quota
check. It ran for 30 hours, and about three-quarters of the successful calls
went to Opus models.

The difference between the cases is the useful part. The Case A key allowed
`global.*` cross-region inference profiles, and the attacker used them. The Case B
key was restricted to `us.*` profiles. When it appeared in a reseller pool seven
days after validation, the pool kept requesting `global.*` and failed thousands
of times from rotating hosting IPs. Then it found what worked and ran about
1.7 million calls in 13 hours. Over a million succeeded, and regional quotas
throttled most of the rest.
**The restriction slowed the abuse. It did not prevent it.**

Wiz tied the key checks to two reseller operations, which it calls *momo*
(`llm-routing.com`) and *Xingjiabi* (`xingjiabiapi.org`). One of momo's hosts
served a page titled "autonomous penetration testing console" on 8 October. The
Xingjiabi site hosted a "distillation behavior detection" page, apparently to
help its customers avoid account suspension. Wiz is careful to say it cannot prove
either group carried out the platform intrusions. It is clear that a working resale
market existed, and that it found stolen keys fast.

## The exorcism: what to do before November

If you used any of the nine platforms in October:

1. **Rotate every secret at the issuer.** That means AI provider keys, cloud
keys, database passwords, registry tokens and wallet keys. Revoke each one at
Anthropic, OpenAI, Google, AWS or whoever issued it, then reissue it. Fireworks
and the Cloud Security Alliance both make the same point: deleting a secret from
the platform leaves the credential valid.
2. **Change database passwords inside the database.** On Zeabur, changing the
environment variable did not change the password, because the attacker had
already been inside the container.
3. **Read your provider bills now.** Look at everything from mid-September
onward, because some of the reseller pools Wiz tracked were active from
10 September. OpenAI refunded OpenCode victims and Zeabur is taking
compensation requests. Railway says it cannot refund spend on leaked keys,
so contact your provider directly.

If you run AWS Bedrock, whether or not you used these platforms:

1. **Hunt for the validation pattern in CloudTrail.** Look for a single`InvokeModel` from an IP that is not yours, followed by days of low-volume
calls. Also look for`ListFoundationModels` ,`ListInferenceProfiles` ,`GetServiceQuota` and`PutAccountDataRetention` from`boto3` user agents you
do not recognise.
2. **Alert on the credential-check IPs Wiz published:** 23.106.46.239,
38.246.114.118, 104.243.32.50 and 31.57.201.201. Wiz says they are not directly
tied to the platform breaches, so a hit means a leaked key, not attribution.
3. **Scope the key, then assume it will leak anyway.** Allow only the models and
inference profiles you use, add a source-IP or VPC condition, lower service
quotas to what you actually need, and set a budget alarm on Bedrock. Case B
shows that scoping buys time, not safety.
4. **Plant a decoy.** Leave a key with no permissions where your real ones live,
and make any use of it raise an alarm. Every key in this campaign was used
within half an hour of being stolen, so a decoy's alarm can arrive before the
bill does.

The structural fix is to stop handing long-lived master keys to platforms. Prefer short-lived, workload-scoped credentials (OIDC federation, role assumption) over static keys pasted into a dashboard. Where you cannot avoid a static key, give it its own spend limit so that losing it costs a known amount.

## Every incident on Threat Watch

We track these incidents on Threat Watch, our public incident feed. All of this month's records, including the campaign record with Wiz's indicators, have been checked against their primary sources: vendor incident pages, status pages and the Wiz and CSA write-ups. Each one gives the dates, what was exposed and what to do. Where the dates in sources disagree, the record follows the vendor's own page. Fireworks, for example, detected the activity on 6 October but published its bulletin on 9 October. Claims we could not verify are labelled as claims. One example is a single social-media report that about $371,000 in cryptocurrency was drained using wallet keys stored on Railway. Nobody has corroborated it yet.

Every record also carries an agent brief, so your coding or security agent can check whether an incident affects the systems it manages and report what it changed.

## Frequently asked questions

- What was Black October?
- Black October is Wiz's name for a run of at least nine security incidents at AI developer platforms in October 2026: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, Command Code and Northflank. The intrusions used different entry points, but all of them went after the environment variables and AI provider keys that customers store on the platform.
- Was it one attacker or one vulnerability?
- Neither has been shown. Wiz found no single shared vulnerability or dependency across the incidents. The entry points differed: stolen internal credentials at Fireworks, plaintext admin tokens at Zeabur, build-infrastructure privilege escalation at Railway, a third-party vendor at Warp. What they share is the target and the downstream resale of stolen AI keys.
- What is LLMjacking?
- LLMjacking is the theft and resale of access to paid AI models. Attackers steal API keys or cloud credentials, check that they work, and sell metered access through reseller APIs. The victim pays the provider's bill. In the two Bedrock cases Wiz followed, the abuse ran to over $100,000 at list price in less than two days of flooding.
- What does distillation have to do with stolen keys?
- Distillation trains a smaller model on a larger model's outputs, which takes very large numbers of frontier-model calls. OpenCode, Railway and Command Code all said their attackers were after AI keys for distillation. Stolen keys move that cost onto the victims.
- I deleted my secrets from the platform. Am I safe?
- No. Deleting a secret from a platform does not invalidate the credential itself. You have to revoke it at the issuer (the AI provider, cloud, database or registry that created it) and issue a new one. For databases, change the password inside the database too.
- Does restricting a Bedrock key to us.* inference profiles protect it?
- Only partly. In Wiz's Case B, a key restricted to us.* profiles made a reseller pool fail on global.* requests for a while, but the pool adapted and ran about 1.7 million calls in 13 hours. Scope keys tightly, add source-IP or VPC conditions, cap quotas and budgets, and still plan for rotation.
