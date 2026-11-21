---
id: collect-261121-veille-ia/grok-xai/grok-4-7-in-depth-review-2026-pricing-coding-vs-astra-fable-carthage-electronics
title: "Grok 4.7 In-Depth Review (2026): Pricing, Coding, vs Astra & Fable | Carthage Electronics"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-23"]
keywords: ["grok", "coding", "model", "tokens", "review", "artificial", "analysis", "launch"]
source: https://carthageelectronics.com/grok-4-7-in-depth-review-2026/
source_anchor: ""
source_lines: [1, 127]
section: "Grok_xAI"
---
*In-depth review — September 23, 2026. Primary sources: xAI / SpaceXAI “Introducing Grok 4.7”, Artificial Analysis “Benchmarking Grok 4.7”, and secondary coverage from Ground Truth and OpenTools.*

Grok 4.7 landed on September 21, 2026 as xAI’s (branded SpaceXAI on the launch post) latest flagship for coding and knowledge work. The pitch is unusually clear for a frontier launch: same list price and speed class as Grok 4.6, a larger base model, longer reinforcement learning on multi-hour tasks, stronger self-verification, and a new safeguard stack. If you live in Cursor, Grok Build, or the Grok API, this is the drop that matters most this week — but the independent numbers also show where the sticker price stops being the whole story.

This review expands the thin first cut into a practical deep dive: what xAI claims about architecture and training, how pricing actually works once prompts cross 200k tokens, what Artificial Analysis measured on the Intelligence Index and Coding Agent Index, how Terminal-Bench and CursorBench should be read (including harness caveats), coding vs chat use cases, hard limits, comparison context versus Claude Fable 5.1 and GPT-6 Astra, and a FAQ for builders deciding whether to switch daily drivers.

## What Grok 4.7 actually is

Per the official launch post, Grok 4.7 is positioned as the company’s most capable model for coding and knowledge work. It “works longer on difficult tasks, checks its own work more carefully,” and ships with what xAI calls its best-calibrated safeguards to date. Critically, it is “served at the same price and speed as Grok 4.6,” which is the commercial hook — capability uplift without a rate-card hike.

On architecture and training, xAI is specific about direction even when it is silent on parameter counts: Grok 4.7 uses a new, larger base model than Grok 4.6; it was trained with a longer reinforcement learning run on a harder mix of tasks weighted toward problems that take many hours to complete; it is better at verifying its own work and managing longer context; and it was trained to natively understand the Grok Bot harness for conversational and general knowledge work. That last point matters if you are comparing “bare model in a third-party agent” versus “model plus first-party harness” scores later in this review.

- **Context window:** 500,000 tokens (unchanged class vs Grok 4.6; confirmed by Artificial Analysis).
- **API model id:**`grok-4.7` .
- **Availability day one:** Grok API, Cursor, Grok Build, plus third-party coding harnesses, model routers, and cloud platforms (xAI lists OpenRouter, Vercel, and Cloudflare in secondary coverage).
- **Reasoning effort:** configurable from low through xhigh (Artificial Analysis evaluates at xhigh).
- **Fast variant:** launch post mentions 2× output speed at 2× price; check live docs for the exact product id.
- **Knowledge / training notes:** secondary launch cards report a June 2026 pretraining cutoff with supplemental training through August 2026 — treat as documentation-dependent and verify on the live model card.

## Pricing: cheap on paper, watch tokens and task cost

List rates match Grok 4.6. Under 200k prompt tokens: **$2 per million input**, **$6 per million output**, with cached input at **$0.50**. At or above 200k prompt tokens, the *whole request* bills at the higher tier — **$4 / $12** (cached $1) — so a 500k context window does not carry the headline price all the way through. That undercuts Claude Fable 5.1’s $10 / $50 sticker by a wide margin and undercuts GPT-class flagship rates that sit in the mid-single to double-digit dollars per million tokens.

Independent reporting is the caveat. Artificial Analysis scored Grok 4.7 (xhigh) at **46** on the Intelligence Index — about +2 over Grok 4.6 — while noting roughly **81k output tokens per Intelligence Index task**, more than double Grok 4.6’s ~38k at xhigh and roughly triple GPT-6 Astra’s ~27k at max in their comparison. Equal list rates do not guarantee equal cost per finished task. Ground Truth and OpenTools both emphasize the same translation: agentic gains arrived with heavier reasoning-token use, and Coding Agent Index tasks measured around **$8.82** average pay-per-token API cost and ~39 minutes wall time for Grok Build + 4.7 xhigh versus ~$3.57 and ~19.5 minutes for 4.6 xhigh in AA’s published snapshot.

Practical takeaway: for short interactive coding turns in Cursor, the $2/$6 card is genuinely competitive. For long unattended loops at xhigh, estimate from measured token burn — not from marketing “same price as 4.6” shorthand.

## Published benchmarks: vendor card vs Artificial Analysis

Always separate *vendor-published* tables from *independent* harness-standardized scores. xAI’s launch comparison (Grok 4.7 xHigh vs Grok 4.6 High, GPT-5.6 Sol Max, and Fable 5.1 Max) highlights the following vendor numbers:

| Benchmark | Grok 4.7 | Grok 4.6 | Notes | 
|---|---|---|---|
| CursorBench 4.0 | 46.3% (xHigh) | 40.4% High | Fable 5.1 Max 51.8% on xAI card | 
| DeepSWE v1.1 | 71.0% (high)* | 65.2% | *high effort on card | 
| EEBench | 64.0% | 53.0% | Fable 5.1 56.4% | 
| AA Briefcase v1.1 | 1,657 | 1,546 | Fable 5.1 1,678 | 
| Terminal-Bench 4.0 | 37.6% | 20.3% | Fable 5.1 57.9% on xAI card | 
| Harvey Legal Agent | 19.6% | 15.8% | Large relative gap vs Sol on card | 
| HealthBench Professional | 56.7% | 48.5% | Fable 5.1 62.1% | 

Artificial Analysis independently places Grok 4.7 in the top tier of labs on agentic knowledge work while still trailing the absolute frontier on the composite Intelligence Index:

- **Intelligence Index:** 46 at xhigh (+2 vs Grok 4.6).
- **AA-Briefcase:** 1657 Elo (+111 vs 4.6 high), just behind Claude Opus 5 and Claude Fable 5.1.
- **GDPval-AA:** 1695 Elo (+90 vs 4.6 high).
- **Coding Agent Index (Grok Build + 4.7 xhigh):** 56 (+9 vs 4.6), ranking 4th among native harnesses behind Fable 5.1, GPT-6 Astra, and Opus 5.
- **Coding Agent components:** DeepSWE 73% (from 65%), Terminal-Bench 4.0**33%** (from 18%), SWE-Atlas-QnA 63% (from 58%).
- **Speed:** ~188 tokens/sec answer output on long prompts; ~7.1 minutes average per Intelligence Index task in AA’s run.
- **Hallucinations:** AA-Omniscience hallucination rate 29% vs 34% for 4.6 high; accuracy roughly flat (~47–48%).

Notice the Terminal-Bench discrepancy: xAI’s card shows ~37.6–38% (with Grok Build / xhigh framing), while AA’s standardized Coding Agent component lists 33% for Build + 4.7 xhigh. Both are large gains over 4.6’s teens, and both still trail Fable 5.1’s high-50s on the vendor peer table. Quote the source you mean, and never mix harnesses.

## Coding vs chat: where Grok 4.7 feels different

### Coding and agentic engineering

Day-one Cursor and Grok Build availability is the product story as much as the model story. CursorBench 4.0 is explicitly framed by xAI as stressing longer-running coding tasks, and Grok 4.7 sits at a price-performance frontier on that axis even when absolute peak scores still belong to Fable 5.1. DeepSWE gains (vendor high-effort ~71%; AA Build combo ~73%) suggest stronger software-engineering agent behavior on implementation-heavy suites.

EEBench is a quieter differentiator on the launch card: 64% vs 53% for 4.6 and ahead of Fable 5.1’s 56.4% in xAI’s table. If your work mixes firmware-adjacent reasoning, electrical constraints, or hardware-aware debugging, that row is worth more attention than another generic coding leaderboard screenshot.

### Chat, documents, and knowledge work

Native Grok Bot harness training is meant to lift conversational and general knowledge-work utility, not only repo agents. AA-Briefcase and GDPval-style professional deliverables (documents, spreadsheets, slides) improved meaningfully in independent scoring. For everyday chat, expect the usual Grok personality bias — useful when you want direct answers, less ideal if you need heavily hedged enterprise tone by default.

## Comparison context: Grok 4.7 vs Claude Fable 5.1 vs GPT-6 Astra

As of late September 2026 board snapshots:

- **Intelligence Index (AA):** Fable 5.1 and GPT-6 Astra around the low-to-mid 50s on rebased index versions; Grok 4.7 ~46. Peak composite intelligence still favors Fable/Astra.
- **List token price:** Grok 4.7 $2/$6 under 200k; Fable 5.1 $10/$50 with $0.25 cache reads; Astra sits in a higher flagship band depending on product SKU — Grok wins sticker price.
- **Native coding agents:** AA Coding Agent Index ranks Fable 5.1, Astra, and Opus 5 above Grok Build + 4.7, but Grok’s +9 jump is the largest recent step in that harness for SpaceXAI.
- **Terminal / long-horizon agents:** Fable still owns the wide Terminal-Bench gap on published peer tables; Grok closed much of the distance vs its own prior generation.
- **Editor convenience:** Grok’s day-one Cursor path is frictionless for many indie and SMB developers already in that IDE.

Pick Grok 4.7 when iteration speed and dollar-per-session matter more than peak unattended Terminal-Bench reliability. Pick Fable 5.1 or Astra when the job is multi-hour autonomous agent work and you can absorb flagship rates (or Fable’s cheaper cache-heavy agent economics).

## Limits, safety, and operational caveats

xAI says Grok 4.7 ships with an entirely new safeguard stack and leads their internal testing on refusals and jailbreak resistance. In dual-use domains it claims a better balance of utility on benign tasks and refusal on dangerous ones, including a LatchBio biosafety score of 62.4% and strong HackerBench v0.3 safety (allowing only 3.3% of risky dual-use prompts through in their write-up). Select cybersecurity partners get invite-only red-team access. Treat these as vendor safety claims — useful context, not a substitute for your own threat model.

- Higher effort ⇒ more tokens ⇒ higher task cost even at unchanged list rates.
- Prompts ≥200k tokens flip the entire request to the expensive tier.
- Harness-dependent scores (Grok Build vs generic agent) are not interchangeable.
- Composite Intelligence Index still trails Fable 5.1 / Astra.
- Fast variant pricing/availability should be confirmed in live API docs before you design around it.

## Practical tips for trying Grok 4.7 this week

- Start in Cursor or Grok Build on a real multi-file feature, not a toy prompt — that is the launch’s intended surface.
- A/B the same task at medium vs xhigh effort and log output tokens; decide from $/task, not vibes.
- Keep prompts under 200k when you can; if you need the full 500k window, budget the doubled rates.
- For terminal-heavy agents, run a small Terminal-Bench-like smoke suite in your harness before migrating production jobs from Fable/Astra.
- Use EEBench-style or hardware-aware tasks if that is your niche — that is where the vendor card looks unusually strong.
- Cache aggressively where the API supports it ($0.50 cached input under 200k).

## Cursor, API, and Grok Build: availability in practice

xAI’s launch post and follow-on coverage put Grok 4.7 in **Cursor** and **Grok Build** on day one, alongside the Grok API and third-party coding harnesses, routers, and clouds. For many developers that is the entire adoption path: flip the model picker in Cursor, keep your existing rules and MCP tools, and run the same agent loop you already trust. Secondary write-ups name OpenRouter, Vercel, and Cloudflare among router/cloud surfaces — always confirm the live model id `grok-4.7` is mapped in your org’s allowlist before you assume faregates are open.

API users should treat configurable reasoning effort (low through xhigh) as a first-class cost control. Artificial Analysis evaluates at xhigh; that is the right setting for apples-to-apples frontier comparisons and the wrong default for every autocomplete-shaped turn. Pair low/medium effort with interactive editing, and reserve xhigh for overnight agents, hard refactors, and eval suites. If you enable a fast variant (2× output speed at 2× price per the launch note), isolate it to latency-sensitive interactive sessions rather than mixing it into cost reports for xhigh agents.

## Reading Terminal-Bench and CursorBench without fooling yourself

Terminal-Bench 4.0 and CursorBench 4.0 are easy to misuse in slides. Vendor cards may report Grok 4.7 near the high-30s on Terminal-Bench (xAI’s table shows about 37.6% at xhigh with Grok Build framing), while Artificial Analysis’s Coding Agent Index component lists about 33% for Grok Build + 4.7 xhigh — both large gains over Grok 4.6’s teens, both still well behind Fable 5.1’s high-50s on peer tables. The gap is not a rounding error; it is the difference between “stronger overnight terminal agent than last month” and “best published autonomous terminal agent.”

CursorBench is closer to how many humans actually work: longer-running editor tasks with a realistic IDE harness. Grok 4.7’s 46.3% xhigh against Fable 5.1’s 51.8% max on xAI’s card is a narrower gap than Terminal-Bench, which is why price-performance narratives focus there. If your production agent is closer to “human in Cursor steering a model” than “model alone in a raw shell for hours,” CursorBench is the more decision-relevant chart. If you ship unattended DevOps agents, trust Terminal-Bench and AA’s Coding Agent Index more — and keep Fable or Astra in the bakeoff.

## FAQ

### Is Grok 4.7 “better” than Grok 4.6?

On coding-agent and long-horizon knowledge-work metrics, yes — especially with Grok Build (+9 Coding Agent Index points; large Terminal-Bench and DeepSWE lifts). On raw list price and context size, it is a lateral move. On cost-per-hard-task at xhigh, maybe not, because token use roughly doubled in AA’s Intelligence Index measurement.

### Should I switch from Claude Fable 5.1?

Only if price-performance and Cursor/Grok workflow fit beat Fable’s higher agentic ceiling for your workload. Fable still leads published Terminal-Bench peer numbers and sits higher on the Intelligence Index. Many teams will keep Fable for the hardest agents and use Grok 4.7 as a cheaper daily coding driver.

### Where is it available?

Grok API (`grok-4.7`), Cursor, Grok Build, and listed routers/cloud partners. Confirm your org’s router SKU maps to the new id.

### What about chat quality vs coding?

Coding and multi-hour knowledge work are the headline. Chat benefits from Grok Bot harness training, but if your primary need is long-form literary writing or maximum cautious enterprise tone, evaluate Fable/Astra side by side on your own prompts.

Bottom line: Grok 4.7 is a same-price capability bump aimed at longer-horizon coding and professional agents, with independent confirmation of real gains — and a clear warning that those gains can cost more tokens. Use it where Cursor velocity and $2/$6 economics win; keep Fable or Astra on the desk for peak autonomous reliability.
