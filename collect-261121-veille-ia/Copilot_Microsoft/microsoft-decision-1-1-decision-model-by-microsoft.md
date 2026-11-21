---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-decision-1-1-decision-model-by-microsoft
title: "Microsoft-Decision-1.1: decision model by Microsoft"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["model", "decision", "microsoft", "microsoft-decision-1", "score", "vendor", "openrouter", "index"]
source: https://modelsystem.one/models/microsoft-decision-1-1/
source_anchor: ""
source_lines: [1, 102]
section: "Copilot_Microsoft"
---
Scored with the public quadrant rubric (maturity Ã capability; bubble = adoption). Revise as evidence lands.

- Maturity6.7/10Foundry catalog lists lifecycle GA. Learn documents noul, choice and score, curl and a Python quickstart; no model SDK. OpenRouter and Vercel AI Gateway both list it. Closed weights. No model SLA found.
- Capability7.6/10Noul, choice and score. Probabilities are in the API; Learn says scores are not absolute calibrated ratings; no ECE. AA v2.1 on 17 Nov: 67.779, 8th of 18 defaults, 123 ms, above Jev 1.14. No Decision Index Full and no JevBench 1.1 row.
- Adoption15/100@msdev on 17 Nov: 242 likes and 9,942 views. OpenRouter lists the id; Vercel is counted under integrations. The 4,000 customers and the Copilot example are Microsoft's account.

## Vendor claims

- AA Decision Index v2.1 score 68, up six points from the first model, and ahead of Jev 1.14, Gemini Decision 1 and GPT-6 Luna (Decisions). More than twice as fast as Decision-1, at the same cost, with better calibration.[Vendor claim â not independently verified]
- On 102 internal benchmarks and more than 385,000 questions kept blind from training, the highest average accuracy of eight decision models, leading seven of nine categories.[Vendor claim â not independently verified]
- More than 4,000 customers since the public preview, including Epic, Equinor, Atos Group and Raiffeisen Bank. In Copilot, routing is as accurate as an LLM, 2.2Ã faster and 4.4Ã cheaper.[Vendor claim â not independently verified]
- Foundry input price: Global Standard $0.042/M, US Data Zone $0.046/M, EU and APAC Data Zone $0.05/M. Output tokens are free. Base is Gemma 4 26B A4B, trained with SFT and RL on a custom inference engine.[Vendor claim â not independently verified]

## News covering this model

**Read this first.**


**This page is Microsoft-Decision-1.1.** The Microsoft-Decision-1 page stays the record of the first model (6.4 / 7.9 / 24, scored on JevBench).
**One board in the score.** Artificial Analysis Decision Index v2.1 is the independent number below. The Hugging Face Decision Index and JevBench do not list 1.1. Those boards are not mixed, and the blogâs 68 is not the figure in the sum.
**The catalog price is the OpenRouter price**, $0.042 per 1M input tokens and $0 output, which matches the blogâs Global Standard line. The US and EU/APAC zone prices are only on the blog.

## What shipped

Microsoftâs blog is dated **2026.11.17** (Achint Srivastava, VP of Software Engineering, Office of the CTO). @msdev posted the same announcement at **18:24:31 UTC** (15:24 BRT). At the time of this reading the post had 242 likes, 36 reposts and 9,942 views.

The Foundry catalog (opened via `https://aka.ms/decision-1.1`) labels the lifecycle **Generally available (GA)**. Input types on that page are text and image. The page says the model returns a probability for each fixed option, is built on Gemma 4 26B-A4B, and accepts up to 8 images. That base-model sentence is the catalogâs. The weights are closed.

Microsoft Learn is now titled for 1.1. The HTML returned on 17 Nov includes the question-type table and also a sign-in note. Question types in that table:

| Question | API type | Result | 
|---|---|---|
| Is this true? | `noul` | A probability from 0 through 1 | 
| Which option is it? | `choice` | One selected option and a probability per option | 
| How much? | `score` | A value on an ordered scale and a probability per level | 

The guideâs endpoint is `POST <resource>/providers/microsoft/v1/systemone`. State can be text or JSON and can reference uploaded images. The at-a-glance table lists model name Microsoft-Decision-1.1, model version 1, and deployment types GlobalStandard and DataZoneStandard. âModel version 1â is that tableâs version field, not the older Microsoft-Decision-1. On scores, the guide says to prefer relative ordering and thresholds rather than absolute calibrated ratings.

## Specs

| Attribute | Value | 
|---|---|
| Id on OpenRouter | `microsoft/microsoft-decision-1.1` (permaslug`microsoft/microsoft-decision-1.1-20261117` ) | 
| Id on Vercel AI Gateway | `microsoft/decision-1.1` | 
| Where it runs | Foundry (GA), OpenRouter, Vercel AI Gateway | 
| Weights | Closed. OpenRouterâs page has no weights link | 
| Types | `noul` ,`choice` ,`score` . Catalog`decisionTypes` are choice, score and boolean | 
| Input | Text and images. OpenRouter context **32,768** tokens. Foundry catalog text says up to 8 images | 
| Price | **$0.042 per 1M input tokens, output $0** on OpenRouter, read 17 Nov. Release time on that page: 2026-11-17T17:57:35Z | 
| Docs | Learn guide above, plus the Vercel changelog example ( `experimental_decide` ,`ai` 7.0.128 or later) | 

The blogâs Foundry table, not repeated as a second measured price: Global Standard $0.042/M, US Data Zone $0.046/M, EU and APAC Data Zone $0.05/M, output free. The $0.042 line matches OpenRouter. The zone prices were not in the Foundry HTML fetched for this page.

## Vendor claims (UnverifiedClaim)

From the blog. These are not the Artificial Analysis table.

- Index **68** , up six points from the first model, ahead of Jev 1.14, Gemini Decision 1 and GPT-6 Luna (Decisions). More than twice as fast as Decision-1, same cost, better calibration. The post says the index scores quality, calibration and latency together.
- Internal set: 102 benchmarks, more than 385,000 questions kept blind from training, highest average accuracy of eight decision models, leading seven of nine categories. microsoftconnect/Decision-Benchmarks is a separate runner over 18 public benchmarks (41,627 items) aimed at an endpoint you choose. It is not that internal set, and this round did not run it.
- More than 4,000 customers in about five weeks, naming Epic, Equinor, Atos Group and Raiffeisen Bank. Inside Microsoft: Copilot routing versus Cowork (as accurate as an LLM, 2.2Ã faster, 4.4Ã cheaper), Cowork Auto, a judge in Foundry Evaluations, and on-call retrieval.
- Base Gemma 4 26B A4B (the Foundry catalog says the same name), SFT plus RL, a custom inference engine, hardware A100, H100/H200, GB200/GB300.

## Artificial Analysis Decision Index v2.1 (17 Nov 2026)

Read from the live page titled **Decision Index v2.1**. Default `decisionModel` rows only. This board is not the Hugging Face Decision Index and not JevBench.

Microsoft-Decision-1.1 is **8th of 18**, index **67.779** (payload 67.77851391780439). Time per task **123.25 ms**. Cost per task **$0.000140** (payload $0.000139637, about $140 per million tasks). That cost is AAâs measured task cost, not the $0.042 token price. Host on the row: Microsoft Azure. The row is a default.

| Rank of 18 | Model | Index | 
|---|---|---|
| 1 | d1.2-ultra | 74.846 | 
| 2 | Drex Max v1.1 | 72.624 | 
| 6 | Decider v1.3 | 70.408 | 
| 8 | **Microsoft-Decision-1.1** | **67.779** | 
| 10 | Jev 1.14 | 66.234 | 
| 11 | Gemini Decision 1 | 65.977 | 
| 17 | GPT-6 Luna (Decisions) | 60.246 | 

The blogâs âahead of Jev 1.14, Gemini Decision 1 and GPT-6 Lunaâ matches this list. The blogâs â68â is that 67.779 rounded. A non-default row named Microsoft-Decision-1 (OpenRouter, not 1.1) is **62.072**. The gap is 5.71 points, which the blog rounds to six. d1.2-ultra leads this reading. The 8 Nov v2 note is an older edition.

## Decision Index and JevBench

`data/index.json` on the Decision Index space, generated **2026-11-17T18:35:28Z**, label Decision Index 0.4.1, 307 models. Jev 1.13 `balanced_skill` is still **59.22**. There is no Microsoft-Decision row. The proportional line 8 Ã Full / 59.22 does not apply.

The JevBench API board has no string âDecision-1.1â. It still lists Microsoft-Decision-1 (Azure Foundry) and an October weights update of that model. Those rows are the older note. They are not a score for 1.1.

## Score (17 Nov 2026)

Weights in the quadrant doc are unchanged. Reference for the proportional rule remains Jev 1.13 Full 59.22. Half-up rounding.

- **Maturity 6.7** = availability 10 Ã 0.30 + docs 7 Ã 0.25 + integrations 6 Ã 0.25 + support 2 Ã 0.20 = 3.00 + 1.75 + 1.50 + 0.40 =**6.65 â 6.7** . Availability 10: the Foundry catalog says GA. Docs 7: Learn has the three types, curl and a Python quickstart, and no model SDK. Integrations 6: OpenRouter and Vercel AI Gateway are both live. Not 8: no framework SDK was checked. Support 2: no model SLA or status page found.
- **Capability 7.6** = types 10 Ã 0.30 + calibration 5 Ã 0.25 + latency 7 Ã 0.25 + accuracy 8 Ã 0.20 = 3.00 + 1.25 + 1.75 + 1.60 =**7.60** . Types 10: noul, choice and score. Images are an input, not a fourth type. Abstaining in the blog is a threshold the caller sets, not a catalog type. Calibration 5: the API returns probabilities and the blog calls them calibrated; Learn says not to treat scores as absolute calibrated ratings; no method and no ECE. Latency 7: 123.25 ms is inside the â¤ 0.25 s band and is one time-per-task, not a p50/p99 pair. The âmore than twice as fastâ line stays a vendor claim. Accuracy 8: no Full and no JevBench row, so half-up(8 Ã Full / 59.22) does not run. Accuracy 9 does not run either: AA above Jev is only one of the two boards that exception needs, and 1.1 is not an untied first on the Decision Index. The 8 is this one independent board (67.779 > Jev 1.14 at 66.234 on the same payload), the same floor used when a model first appears on a public board. The blogâs 68 and the internal 102-benchmark set do not add a point.
- **Adoption 15** = engagement 20 Ã 0.35 + ecosystem 20 Ã 0.35 + production 2 Ã 0.30 = 7.00 + 7.00 + 0.60 =**14.60 â 15** . Engagement 20: the @msdev post and the counts above, well below the Satya launch that scored 30 on the Decision-1 page. Ecosystem 20: the OpenRouter listing. Vercel is already in the integrations subscore. Production 2: Copilot, Cowork and the named customers are Microsoftâs account.

## Links

- Blog, 17 Nov 2026 Â· @msdev
- Foundry catalog Â· Learn
- OpenRouter Â· Vercel changelog
- Artificial Analysis Â· Decision Index space Â· JevBench API board
- Compare: Microsoft-Decision-1 Â· Gemini Decision 1 Â· Jev
- News: GA Â· JevBench on Decision-1 Â· Gemini Decision 1
