---
id: collect-261121-veille-ia/kimi/moonshot-releases-kimi-k3-1-pro
title: "Moonshot releases Kimi K3.1 Pro"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-23"]
keywords: ["kimi", "moonshot", "days", "cyber", "after", "opus", "weights", "cutoff"]
source: https://postcutoff.com/e/2026-10-23-kimi-k3-1-pro/
source_anchor: ""
source_lines: [1, 95]
section: "Kimi"
---
The takeaway

On Oct 23, 2026 Moonshot AI launched Kimi K3.1 Pro (2.8T total / 104B active MoE, 1M context, multimodal) and a 2â3x faster Highspeed variant on Kimi.ai, Kimi Code and its API at $2/$10 per 1M tokens, with full weights promised by Nov 4.

## Status

- Claim
- Confirmed
- Our reporting
- High confidence
- Importance
- Major (4 of 5)
- Last verified
- 5 November 2026

## Your AI and this story

- GPT-6.2 Astra145 days after its cutoff
- Claude Opus 5.5115 days after its cutoff
- Gemini 4 Hydrogen206 days after its cutoff
- Grok 4.7145 days after its cutoff

None of these four assistants can know about it. The closest, Claude Opus 5.5, stops 115 days before it.

## Key facts

- Weights released: huggingface.co/moonshotai/Kimi-K3.1-Pro (repo created Nov 2, public by Nov 4; ~2.78T parameters in compressed-tensors format; Hacker News 552 points on Nov 4); licence: Kimi K3.1-Pro License, MIT-style but Model-as-a-Service businesses with over $20M revenue in 12 months need a separate agreement; Huawei Ascend and Alibaba Cloud announced day-zero support (Pandaily, Nov 5)
- Architecture: 2.8T total / 104B active parameters (MoE), 1M-token context, multimodal input (Moonshot on X, 2.58M views)
- API ids: kimi-k3.1-pro and kimi-k3.1-pro-highspeed; OpenRouter moonshotai/kimi-k3.1-pro at $2 input / $10 output / $0.20 cached per 1M tokens (Willison: half Opus 5.5âs price; Highspeed about twice the price)
- Artificial Analysis: Intelligence Index 55 (K3: 44), 4th behind Opus 5.5 (58), Sonnet 5.5 (56) and Muse Wonder Full (56); GDPval-AA 1,846 Elo (3rd, +312 vs K3); Terminal-Bench 4.0 67%, the highest AA had measured (K3: 13%); AutomationBench-AA 58% â 73%; HLE 47% â 52%
- Moonshotâs own numbers: Terminal-Bench 4.0 65.7% (Opus 5.5 66.4%, GPT-6 Astra 58.2%); BenchCAD with tools 92.7% Voxel IoU; OfficeQA Pro v2 64.4; claim-level hallucination rate on its hard benchmark 23.4% (K3) â 8.3%
- Cyber data removal (blog): cyber content was found embedded in the general mid-training corpus and in the RL mix; âwe identified and removed cyber-related data from both training stagesâ. Goal: not to release open weights âthat would extend the absolute frontier of cyber capabilityâ. On xdev-bench-hard the released model scores 81.0 vs 90.5 for the unfiltered checkpoint (GPT-5.6 Sol 100)
- Moonshot says K3.1 Proâs cyber capability trails frontier proprietary models that have been public for at least three months, and plans to âprogressively raiseâ the cyber capability available in open weights
- Marketed for full-project game coding in Unreal, Unity and Godot, parametric 3D/CAD in Blender, and finance modelling

## Show 8 more

- Enterprise admins must switch it on (off by default); Zero Data Retention offered on the API
- Hacker News: 927 points, the dayâs top AI story
- Rate-limit apology (Kimi on X, Oct 25, 20:14 UTC; ~599k views): âWeâre sorry for the rate limits and unnecessary quota consumption on the K3.1 Pro subscription plan. These were our issues, and we should have fixed them sooner.â Every subscriber on every plan got a one-time reset restoring 25% of the monthly quota (usage below 25% reset to zero), âequal to one week of usageâ; Moonshot also said it is âreducing unnecessary subagent calls and output lengthâ
- Pandaily (Oct 26, via IT Home and National Business Daily) links the quota drain to agent workflows in which one request spawns several sub-agents and long outputs; Kimi had paused new consumer subscriptions days after the K3 launch in July for lack of compute
- The420.in (Oct 24) relays Moonshotâs statement that the released K3.1 Pro still does better on cyber tasks than its predecessor, so the filtering limits rather than removes the capability; the 81.0 vs 90.5 benchmark is Moonshotâs own
- Influencer allegations (VentureBeat, Oct 24): X user @elshayib_ posted screenshots of what he called a Kimi promotion brief asking creators to submit drafts for approval, with the line âPlease DO NOT put on the paid partnership labelâ and talking points casting Kimi as an open Claude Opus 5.5 at a fraction of the price; the document is not authenticated, its issuer and reach are unknown, and no Moonshot response was found as of Oct 25. VentureBeat notes this does not affect the independent Artificial Analysis results (Intelligence Index 44 â 55 from K3)
- Distillation report (The Information, Oct 28; Reuters Oct 29): people with direct knowledge say Moonshot relied heavily on GPT-6 Astra outputs to train K3.1 Pro; Moonshot did not respond (see The Information: DeepSeek and Moonshot relied heavily on GPT-6 Astra outputs to train V4.1-Flash 1017 and Kimi K3.1 Pro). On Artificial Analysis Index v5 (Oct 27) K3.1 Pro is the top open-weights model at 46 (weights not yet released)
- 3D Make (Oct 30, @Kimi_Moonshot, ~105k views): âKimi connects to tools like Blender and CAD tools to create models, assemble scenes, and develop gamesâ, available on kimi.ai and Kimi Work. Same week: Gmail, Google Drive and Calendar plugins. Open weights still not on Hugging Face as of Oct 30 (promised by Nov 4)

## What happened

Moonshot released K3.1 Pro three months after Kimi K3. It is served first through its own apps and API, with the weights to follow by Nov 4 (K3âs weights followed 11 days after launch). Artificial Analysis ranked it just behind Anthropicâs two top models and Metaâs Muse Wonder Full. It leads every OpenAI and Google model on that index and posts the best Terminal-Bench 4.0 score AA has measured.

The launch post also explains a safety choice. Moonshot found that vulnerability-research skill rose steadily as post-training scaled, because cyber material was mixed into general data and RL tasks. It filtered this data out of both stages, and the released model gives up about 10 points on its internal exploit benchmark.

## Why it matters

Once the weights ship, a near-frontier model will be free to download, three points behind the best closed model on a broad index. A Chinese lab is also publicly holding back offensive cyber capability in an open release. Western labs have asked for this, and it comes the same day OpenAI took all its frontier training offline over cyber-capable models. Whether the filtering survives fine-tuning by others is untested.

## Sources

16 sources from 11 sites. Numbers match the chips in the text.

16 sources: 6 primary, 6 press, 4 reactions

### Primary

1. Hugging Face: moonshotai/Kimi-K3.1-Pro weightshuggingface.co, code
2. Kimi blog: Kimi K3.1 Prokimi.ai, official
3. Kimi.ai on X: Introducing Kimi K3.1 Pro (2.58M views)x.com, official
4. Kimi.ai on X: apology for K3.1 Pro rate limits, 25% quota reset (Oct 25)x.com, official
5. OpenRouter: moonshotai/kimi-k3.1-proopenrouter.ai, docs
6. @Kimi_Moonshot on X: Introducing 3D Make in Kimi (Oct 30)x.com, official

### Press

1. TechNode: Moonshot AI releases Kimi K3.1 Pro model weightstechnode.com, press
2. Pandaily: Huawei Ascend day-zero support for Kimi K3.1 Propandaily.com, press
3. VentureBeat: Kimi K3.1 Pro flirts with open-weights leadership amid paid influencer scandal allegations (Oct 24)venturebeat.com, press
4. The Decoder: Chinese AI model Kimi K3.1 Pro is closing in on Anthropicâs Claude Opus 5.5the-decoder.com, press
5. Pandaily: Kimi apologizes for K3.1 Pro rate limits, resets 25% of membersâ monthly quota (Oct 26)pandaily.com, press
6. The420.in: Moonshot deliberately curbs Kimi AI hacking ability before open release (Oct 24)the420.in, press

### Reactions

1. Artificial Analysis on X: Kimi K3.1 Pro launches 4th on the Intelligence Indexx.com, discussion
2. Simon Willison: Introducing Kimi K3.1 Prosimonwillison.net, discussion
3. Andrew Curran on X: the cyber-data passage of the launch postx.com, discussion
4. Hacker News discussion (927 points)news.ycombinator.com, discussion

## Changes

- Weights released on Hugging Face (repo, licence, day-zero hardware support); upcoming item closed
- Added 3D Make (Blender/CAD connections) and the open-weights check (news agent)
- Added The Informationâs distillation report and its AA v5 score (news agent)
- Added the Oct 25 rate-limit apology and 25% quota reset (Kimi on X, Pandaily) and The420.in on the cyber filtering; weights not yet on Hugging Face as of Oct 26
- Added the undisclosed-influencer-promotion allegations (VentureBeat)
- Filed (lab X posts, Hacker News; launch blog read directly). Resolves the K3.1 calendar item
