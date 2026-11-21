---
id: collect-261121-veille-ia/grok-xai/mercury-diffusion-llm-api-ends-dec-28-explainx-ai-blog
title: "Mercury Diffusion LLM API Ends Dec 28 | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["mercury", "diffusion", "inception", "after", "december", "november", "vercel", "before"]
source: https://explainx.ai/blog/inception-mercury-api-shutdown-december-2026-migration
source_anchor: ""
source_lines: [1, 179]
section: "Grok_xAI"
---
**Inception told Mercury API customers the hosted diffusion LLM service winds down after December 28, 2026 — while third-party routers already removed Mercury ids and a Vercel AI SDK provider pull request closed the same day citing the shutdown.** The news spread through Hacker News on November 12, 2026 before Inception published a standalone blog explainer.

## TL;DR

| Item | Detail | 
|---|---|
| Deadline | **December 28, 2026** (customer email quoted on HN) | 
| Scope | **Mercury API** hosted service; data on platform not retained after | 
| OpenRouter | Routes reportedly removed **early November 2026** | 
| Ecosystem signal | Vercel AI SDK PR #24975 closed Nov 12: "Mercury API is winding down" | 
| Prior context | Mercury 3 launch claims and Mercury 2 speed story | 

## What customers reported

HN user `_matthew_` posted the full email text attributed to **hello@inceptionlabs.ai**:

- Mercury API remains available **through December 28, 2026**
- After that date, customers lose **service access and data submitted to the platform**
- Inception encourages exporting necessary data before the cutoff

A shorter **Discord** message from **Yannis Miraoui** quoted on HN adds that **OpenRouter access ended November 8, 2026** for Mercury models — aligning with third-party catalog trackers that saw routes disappear after listed expiry dates.

No matching press release was indexed on Inception's models page at publication time — the marketing site still describes Mercury 3, Decide, and Voice, which creates **claimed vs verified** tension for new signups.

## What developers are saying on Hacker News

These are **opinions** from the November 12 thread, not verified facts:

- **johnshahbazian** wrote that they rely on**Decide** and wondered whether Inception is being acquired.
- **stilesja** reported heavy**Mercury 3** use and speculated about an acquisition after the abrupt email.
- **wing_rets** said Mercury was**cheap and fast** and asked whether the tech might be open sourced.
- **carterschonwald** praised Mercury's coding style (admitting gaps, looking things up) and hoped for an**IP release** .

explainx.ai treats acquisition and open-weight rumors as **unconfirmed** until Inception posts primary documentation.

## Ecosystem impact the same afternoon

Vercel contributor **aayush-kapoor** opened a draft **`@ai-sdk/inception`** provider on November 12, then **closed** PR #24975 with comment **"Mercury API is winding down - closing this PR"** — a fast signal that framework maintainers will not ship first-party Inception support while the API sunsets.

Teams that pinned Mercury for **diffusion decoding speed** in agent loops should assume **no new SDK surface area** from Vercel this quarter.

## Migration checklist

1. **Inventory calls** — grep for`inceptionlabs.ai` ,`mercury-3` , and OpenRouter slugs.
2. **Export datasets** — prompts, eval transcripts, and billing CSV before December 28.
3. **Rerun latency baselines** — diffusion LLMs marketed**1,000+ tokens/s** (Mercury 2 coverage); replacements may be autoregressive.
4. **Voice stacks** — if you used**Mercury Voice** , map time-to-first-token requirements to alternate duplex vendors such as Fish Audio Live 3.
5. **Decision APIs** — Mercury Decide workflows need explicit confirmation whether Decide endpoints sunset on the same date.
6. **Legal** — enterprise agreements may differ from self-serve email terms quoted on HN.

## Replacement model strategies

| Workload | Possible direction | Caveat | 
|---|---|---|
| Fast codegen loops | Google **Hydrogen** , Flash-class APIs | New launch — autoregressive, not diffusion | 
| Cheap bulk generation | Hosted small models via Bedrock gateways | Verify tool-calling parity | 
| Calibrated decisions | Microsoft **Decision-1** or Nace**Drex** | Different API shapes | 
| Local diffusion research | NVIDIA Nemotron diffusion LLM notes | Not a drop-in Mercury API | 

None are mechanical drop-ins; budget **two weeks of eval** before December.

## Business context

Inception marketed Mercury 3 in October 2026 with **50% launch discounts** and **1M context** claims we analyzed in claimed vs measured coverage. A shutdown email six weeks later is a reminder that **speed-first API startups** can retract hosting even while docs pages still upsell.

Deprecation sunset graphic for hosted diffusion LLM API wind-down

## What people are asking (November 12)

| Question | Our read | 
|---|---|
| Is this an acquisition? | **Unconfirmed.** HN speculation only until Inception or a buyer files a statement. | 
| Will weights open source? | **No promise** in the customer email quoted on HN. | 
| Can I keep using Mercury 3 until Christmas? | Email says **full availability through Dec 28** ; export data before then. | 
| Does Vercel AI SDK support Mercury? | **No** — provider PR closed Nov 12 citing wind-down. | 
| Is Decide included? | **Unknown** — ask Inception support with your account id. | 
| What about self-hosted diffusion? | Requires **future release** or different research stack; not available today. | 

## What remains unverified

- Whether **Mercury Decide** ,**Voice** , or enterprise contracts follow the same sunset
- Any **acquisition** buyer or**open-weight** release
- Official Inception blog or status page mirroring the HN-quoted email
- Whether self-hosted or partner-hosted Mercury inference will appear elsewhere

We will update this post when Inception docs publish a shutdown notice matching the email.

## Timeline leading to November 12

| Date | Event | Source type | 
|---|---|---|
| June 2026 | Mercury 2 speed marketing | explainx.ai Mercury 2 post | 
| Oct 19, 2026 | Mercury 2/2.5 closed to new users; legacy until Nov 15 | Inception models page footnote | 
| Oct 19, 2026 | Mercury 3 launch promo pricing | Mercury 3 claimed vs measured | 
| Early Nov 2026 | OpenRouter Mercury routes disappear | Marginal Token tracker | 
| Nov 8, 2026 | Discord message: API access ends (per HN) | HN user quote | 
| Nov 12, 2026 | Customer email: full shutdown Dec 28 | HN thread | 
| Nov 12, 2026 | Vercel AI SDK Inception PR closed | GitHub PR #24975 | 

The sequence looks like **distribution choke first** (OpenRouter), **hosted API second** (December), with **marketing pages lagging** reality.

## What diffusion LLMs changed in agent loops

Mercury pitched **parallel token decoding** unlike classic autoregressive chat models. Teams adopted Mercury for:

- **Tight agent loops** where milliseconds per step compound
- **Voice** stacks needing low time-to-first-token (Live 3 contrast)
- **Cheap bulk classification** adjacent to Mercury Decide

If you must migrate to autoregressive APIs, expect **higher tail latency** unless you adopt speculative decoding elsewhere — see NVIDIA Nemotron diffusion research for how labs still experiment, without guaranteeing hosted APIs.

## Mercury Decide and Voice — separate products?

Inception docs still list **Mercury Decide** and **Mercury Voice** with pricing tables on November 12. The HN email refers to **Mercury API** generically. Enterprise buyers should open tickets asking:

1. Do Decide endpoints sunset on the same calendar date?
2. Are Voice enterprise contracts carved out?
3. Will hosted inference move to a partner domain?

Until answers land, treat **all Inception hosted inference** as at-risk.

## Data export and compliance

The quoted email says customers lose **data submitted to the platform** after December 28. That may include:

- Prompt/response logs used for evals
- Fine-tuning datasets uploaded for experiments
- API usage records needed for finance audits

Export early; do not rely on a last-week scrape. If you operate under GDPR or HIPAA, align with counsel on whether shutdown notices satisfy processor change requirements.

## Rewriting routers and SDK config

Search your monorepo for:

- `api.inceptionlabs.ai`
- `INCEPTION_API_KEY`
- `mercury-3` ,`mercury-2` ,`mercury-voice`
- LiteLLM provider blocks naming Inception

Update **fallback chains** so production does not call a dead host at year-end traffic spikes. Stage drills **before Thanksgiving week** if your team freezes deploys in late December.

## Financial and vendor-risk lessons

- **Promotional pricing** on young APIs is not a multi-year contract.
- **Speed benchmarks** in marketing do not imply**business continuity** .
- **Third-party routers** can remove models before the vendor email arrives — design routers to**fail loudly** , not fall back to mystery defaults.

## Monitoring and alerting before the cutoff

Add synthetic checks that call a **read-only Mercury endpoint** weekly until December 20, then daily. Alert if:

- HTTP **402/403** rates spike
- **Latency** doubles (early deprecation throttling)
- **Error bodies** mention scheduled maintenance without a public status page

Pair with Bedrock agent observability patterns if you move inference to AWS-hosted models — different metrics, same need for **per-step model id logging**.

## Contract language to re-read this week

Enterprise agreements sometimes define **wind-down notice periods**, **data retention**, and **refund clauses** separately from self-serve email text. Legal teams should compare:

- Master services agreements signed before Mercury 3 launch
- POC addenda referencing Mercury 2 sunset dates
- Partner contracts naming Inception as a subprocessors

If your contract lacks a **successor vendor clause**, negotiate transition assistance **in writing** before December — verbal Discord answers are not procurement evidence.

## If Inception open-sources weights later

HN commenters hope for an **IP drop**. If that happens, self-hosting still requires **GPU ops**, **safety review**, and **license terms** unknown today. Budget engineering time only when a primary Inception or buyer announcement names a repo — not when rumors circulate in shutdown threads.

Usage quota hourglass approaching zero for API credits

## Related reading

Product managers should schedule a **thirty-minute migration review** with engineering and finance before Black Friday traffic: confirm which customer-facing features still call Mercury, what substitute model fits each SLA, and whether marketing pages must be edited so new signups do not advertise a dead API. Silence from Inception's blog does not relieve teams of customer communication duties once the shutdown email lands in inboxes.

*Shutdown dates quoted from customer reports on November 12, 2026 until Inception posts an official mirror.*
