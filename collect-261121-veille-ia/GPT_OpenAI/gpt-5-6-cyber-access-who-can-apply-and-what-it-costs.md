---
id: collect-261121-veille-ia/gpt-openai/gpt-5-6-cyber-access-who-can-apply-and-what-it-costs
title: "GPT-5.6-Cyber Access: Who Can Apply and What It Costs"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-20"]
keywords: ["cyber", "model", "gpt-5", "openai", "input", "access", "daybreak", "official"]
source: https://api.r.qcode.cc/en/gpt-5-6-cyber-guide
source_anchor: ""
source_lines: [1, 137]
section: "GPT_OpenAI"
---
# GPT-5.6-Cyber

Who gets in, who doesn't

                As of 2026-10-20, OpenAI split Daybreak into Blue and Red tiers, and the offense-oriented GPT-5.6-Cyber is offered only under Red — application-vetted. OpenAI's model page lists official rates of $12.5 input / $1.25 cached input / $75 output per 1M tokens. The model aliases gpt-daybreak-blue-latest and gpt-daybreak-red-latest are marked deprecated on their official model pages, which point to the latest cyber model available.

Updated 2026-11-18

One key across Claude, GPT and Chinese models, billed per token.

- Billed per token — live rates on /models
- Pay by card (Visa / Mastercard), Apple Pay, Google Pay or crypto
- Self-service: top up and activate instantly

## Four key numbers

### Advanced security task completion

OpenAI's internal advanced cybersecurity benchmark. The previous GPT-5.5-Cyber scored 57.3%; unguardrailed Sol manages only ~2%.

### Per million tokens, input / output

On the August 2026 basis this was 2.5x GPT-5.6 Sol's price at the time ($5 / $30). As of 2026-10-20 OpenAI's model page lists Cyber at $12.5 input, $1.25 cached input and $75 output per 1M tokens, while Sol's official price is $4/$20, promotional until 2026-11-30.

### V8 0days found pre-release

Two previously unknown Chrome V8 bugs that chain into a heap-sandbox escape; one is CVE-2026-15903, already patched by Google.

### TAC individual compliance deadline

Individual members must complete Advanced Account Security with a hardware passkey before 2026-09-01 or lose access to high-capability models.

## What the Daybreak split is

Checked 2026-10-20: OpenAI's model page still lists GPT-5.6-Cyber as a separately approved cybersecurity model, at official rates of $12.5 / $1.25 / $75 per 1M tokens (input / cached input / output), with a 400,000 context window, 272,000 maximum input, 128,000 maximum output and a 2026-02-16 knowledge cutoff.

On 2026-08-10 OpenAI published "Expanding Daybreak as the Cyber Defense Window Narrows", splitting its cyber-defense program Daybreak into two tiers: Blue offers GPT-5.6 Sol without system-level cybersecurity guardrails for defensive work such as vulnerability discovery, secure code review and malware analysis; Red opens the dedicated offense-capable model GPT-5.6-Cyber for authorized vulnerability research and exploit validation.

OpenAI's developer changelog dates the two-tier launch to 2026-08-07. Cyber is built on Sol and is the first model rated High for cybersecurity capability under OpenAI's Preparedness Framework.

## What's happened since launch

Re-checked 2026-10-20: the model aliases gpt-daybreak-red-latest and gpt-daybreak-blue-latest are marked deprecated on the official model pages, which point users to the latest available cyber model; GPT-5.6-Cyber's official rates are now listed there too ($12.5 / $1.25 / $75 per 1M tokens). The launch came with a 16-company security-vendor partner program (IBM, CrowdStrike, Accenture, Cisco, Cloudflare and others); from 08-13 both Daybreak tiers became available to qualified customers via Amazon Bedrock.

Meanwhile the TAC (Trusted Access for Cyber) hardware-passkey requirement entered enforcement: introduced on 06-01 alongside the Yubico partnership announcement, individual members must comply by 09-01 or fall back to standard model access. On the regulatory side, the White House convened OpenAI, Anthropic, Google and others on 08-04 to discuss a frontier-model cybersecurity testing framework.

## Timeline

The TAC hardware-passkey requirement begins with the Yubico partnership announcement, covering access to high-capability cyber models.

Daybreak splits into Blue / Red and GPT-5.6-Cyber launches, alongside disclosure of the two V8 bugs found pre-release.

Compliance deadline for TAC individual members' hardware passkeys; anyone who misses it reverts to default model access.

## Confirmed vs caution

### Confirmed

The release date, the two-tier structure, the 95.0% / 57.3% / ~2% completion rates, the 16 partners, the Bedrock opening and CVE-2026-15903 all come from OpenAI's official blog and help centre. Pricing of $12.5 / $75 is now listed on OpenAI's official model page as of 2026-10-20, together with cached input at $1.25 per 1M tokens; the same page lists a 400,000 context window, 272,000 maximum input, 128,000 maximum output, a 2026-02-16 knowledge cutoff and the deprecation of the gpt-daybreak-red-latest and gpt-daybreak-blue-latest aliases.

### Caution

Claims like "a third-party platform already lists GPT-5.6-Cyber" or "we can activate Daybreak Red for you, no vetting" contradict the official line: Red is application-vetted, requiring identity verification, usage monitoring and a lawful-use commitment. Treat any such offer as high-risk.

## Blue or Red

### Daybreak Blue

Defensive. GPT-5.6 Sol without system-level cyber guardrails, covering vulnerability discovery, code review and incident response; ~2% completion on advanced security queries.

### Daybreak Red

Authorized offense. The dedicated GPT-5.6-Cyber model for exploit validation and security testing, at 95.0% completion; vetted access, monitored throughout.

## How to apply

The only entry point is openai.com/daybreak/partners: submit organization and use-case details, pass identity verification, and sign the monitoring and lawful-use terms to enter the Red tier. TAC members must additionally enable a hardware passkey before 09-01. There is no self-serve API path.

## On QCode

QCode does not offer GPT-5.6-Cyber. What can be verified on OpenAI's side: the model is supplied only in the Daybreak Red tier, which requires an application and review, and TAC members must additionally enable a hardware passkey. For defensive security work (code review, vulnerability analysis), start with `gpt-5.6-sol` on QCode, billed per token; per-model rates are on /models.

Not ready for a monthly plan? Pay-as-you-go Packs start at $4 for a one-time quota you use within 7 days; if you already have a Personal Plan, add a booster pack when you need more quota.

## FAQ

## Can ordinary API access GPT-5.6-Cyber?

No. Per OpenAI's own documentation, it is supplied only in the application-vetted Daybreak Red tier, requiring an application, identity verification and monitoring terms. QCode does not carry this model; treat any "we can get you access" offer with caution.

## What's the difference between Blue and Red?

Blue gives defensive work the unguardrailed GPT-5.6 Sol; Red gives authorized offensive research the dedicated GPT-5.6-Cyber. Both require an application; Red's vetting and monitoring are stricter.

## What does GPT-5.6-Cyber cost?

OpenAI's official model page, checked 2026-10-20, lists $12.5 input, $1.25 cached input and $75 output per 1M tokens; requests above 272K input tokens are billed at 2x input and 1.5x output for the full request.

## What vulnerabilities did it find?

During pre-release evaluation it found two previously unknown Chrome V8 bugs that chain into a heap-sandbox escape; one is CVE-2026-15903, already patched by Google (Chrome versions before 150.0 are affected).

## What's the TAC hardware passkey about?

TAC members must complete Advanced Account Security with a hardware passkey. The requirement started 2026-06-01; the individual-member deadline is 2026-09-01, after which access to high-capability cyber models is lost.

## Which model for defensive security work?

Daybreak Blue, or the general-purpose GPT-5.6 Sol. Sol is callable on QCode, billed per token, with per-model rates on /models; OpenAI's current official price is $4/$20, promotional until 2026-11-30 — good for getting started with code review and vulnerability analysis and for day-to-day volume.

## Sources

OpenAI official model pages (gpt-5.6-cyber, Daybreak Red, Daybreak Blue; fetched 2026-10-20), the OpenAI API changelog (fetched 2026-10-20), the OpenAI blog post of 2026-08-10, the TAC entry in OpenAI's help centre and Yubico's official blog (2026-06-01); pricing follows OpenAI's official model page.

## Defensive security work starts with a flagship in stock

GPT-5.6 Sol and models from multiple families are callable with one key on QCode, billed per token; see /models for each model's price.

## Related reading

### GPT-5.6 family guide

How to pick between Sol / Terra / Luna.

### Sol vs Terra vs Luna

Capability and pricing across the three same-generation tiers.

### MCP security sandboxing

Isolation and boundary practices for tool calls.

### Crypto payment guide

Nine coins, 56 networks, zero platform fee for AI coding API credits.

Not affiliated with OpenAI. This page offers no access to any restricted tier. Checked 2026-10-20: GPT-5.6-Cyber availability follows OpenAI's Daybreak program; QCode does not offer it, and QCode model availability follows /models.
