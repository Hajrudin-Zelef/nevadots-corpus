---
id: collect-261121-veille-ia/qwen/qwen3-8-max-ships-2-4t-moe-95b-active-1m-context
title: "Qwen3.8-Max Ships: 2.4T MoE, 95B Active, 1M Context"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "release", "qwen", "model", "context", "post", "alibaba", "text"]
source: https://www.digitalapplied.com/blog/qwen3-8-max-full-release-benchmarks-open-weights
source_anchor: ""
source_lines: [1, 178]
section: "Qwen"
---
Qwen3.8-Max is out — for real this time. On August 3, 2026, Alibaba moved its biggest model ever from teaser to general availability: a 2.4-trillion-parameter sparse Mixture-of-Experts that activates just 95 billion parameters per token, takes text, image, and video input, and carries a headline 1M-token context window.

The release answers most of what the July preview left open. There is now a full vendor benchmark table — roughly 40 rows with 20 numbered methodology footnotes — an active-parameter disclosure, a live hosted API with drop-in snippets for popular coding agents, and a dated commitment to something Qwen has never done before: open-sourcing the weights of a Max-class flagship, promised for the following week alongside a new 27B model.

This guide separates what verifiably shipped on day one from what is still a promise, reads the vendor benchmark table with its own footnotes in hand, and covers the pricing that is circulating but not yet on Alibaba’s own price page. Everything below is sourced from the Qwen Team’s release post and same-day coverage, and every score is vendor-stated unless noted.

1. 01This is full GA, not another preview.2.4T total / 95B active sparse MoE with hybrid attention, built on the Qwen 3.5 architectural foundation. Multimodal input (text, image, video), text output, 1M-token headline context, hosted API live on day one.
2. 02The benchmark table is real — and vendor-stated.Roughly 40 rows plus 20 methodology footnotes on the release post. Alibaba's table shows wins on OSWorld-Verified (86.1), PaperBench (93.0), and IFBench (82.8), and clear deficits on SWE-bench Pro and FrontierSWE.
3. 03Open weights are a dated promise, not a fact.Weights for both Qwen3.8-Max and a new Qwen3.8-27B were promised for the week of August 10 — the first Max-class open weights in Qwen history. No license was disclosed anywhere in the release post.
4. 04The $2 / $6 pricing is reported, not vendor-listed.Multiple independent write-ups converge on $2.00 input / $6.00 output per million tokens (standard hosted-API rate), but the figure was absent from Alibaba's own Model Studio pricing page at the time of writing.
5. 05The autonomy showcases are striking and vendor-run.A 16-day autonomous coding run, a research-paper reproduction that beat the paper's own method, and a live-contest result against 526 human teams — all Alibaba-reported, with partially checkable public artifacts.

`Qwen/Qwen3.8-2.4T-A95B` and an FP8 sibling are live on Hugging Face, public and ungated, at 224 files and roughly 4.89 TB. The licence that was disclosed nowhere in the release post turned out to be a bespoke `qwen3.8-max` licence rather than the Apache 2.0 used by earlier models in the line — so the caution below about treating precedent as a commitment was the right call. The promised Qwen3.8-27B checkpoint has still not appeared.
## 01 — What ShippedFrom preview to *general availability*.

The Qwen Team’s release post opens without hedging: this is the official release of the most capable model in the Qwen family, not a staged preview. The model is a sparse Mixture-of-Experts design with hybrid attention, built on the architectural foundation of Qwen 3.5, and the post states the activation figure plainly — despite 2.4 trillion total parameters, only 95 billion are active per token. Input is multimodal (text, images, and video), output is text, and the headline context window is one million tokens.

That inverts the July situation. When Alibaba teased this model at WAIC Shanghai, there were no benchmarks, no pricing, and no open-weight date — our coverage of the July 19 preview treated the 2.4T claim as exactly that, a claim. The full release fills in two of those three gaps with published detail and converts the third into a dated commitment. The August 3 headlines treated it as a market event too: Bloomberg’s headline framed the release around benchmark claims rivaling Anthropic, and CNBC’s led with an Alibaba share rally.

"Today, we are officially releasing Qwen 3.8-Max, the most capable model in the Qwen family to date."— Qwen Team, release blog, August 3, 2026

*August 3, 2026*: the hosted API with official drop-in snippets for coding assistants, a full benchmark table of roughly 40 rows with 20 methodology footnotes, the 95B active-parameter disclosure, and a new Qwen-MM-Plugins library for making existing agent harnesses multimodal-native. Not shipped: the weights themselves (promised for the week of August 10) and any license text at all.

## 02 — Shipped vs. PromisedDrawing the line most coverage *skipped*.

The one same-day write-up we could read in full got the “what has been published” question outright wrong — as we detail in Section 04. The table below is our own audit of the release post: every claim sorted into shipped, promised, or undisclosed, based on what is actually present on (or absent from) the page.

| Audit of the Qwen3.8-Max release sorting each element into shipped on day one, promised with a date, or undisclosed, with the basis for each classification. |  |  | 
|---|---|---|
| Item | Status as of Aug 3 | Basis | 
|---|---|---|
| Shipped on day one |  |  | 
| Hosted API access | Live | API Usage section with official code snippets on the release post | 
| Full benchmark table | Published | ~40 rows across two tables, 20 numbered methodology footnotes | 
| Active-parameter disclosure | Published | 95B active, stated twice on the release post | 
| 1M context · multimodal input | Published | Release post + Alibaba Cloud’s official republication | 
| Promised, with a date |  |  | 
| Open weights · Qwen3.8-Max | Promised — week of Aug 10 | “Next week” language, repeated twice on the release post | 
| Open weights · Qwen3.8-27B | Promised — same window | Announced alongside Max; nothing else disclosed about it | 
| Undisclosed or unverified on vendor pages |  |  | 
| License | Undisclosed | No license text anywhere on the release post | 
| API pricing | Reported, not vendor-listed | $2 / $6 per Mtok corroborated across write-ups; absent from Alibaba’s Model Studio pricing page at the time of writing | 

The pattern this table exposes is the story: Alibaba shipped the proof points that make headlines — benchmarks, specs, API — on day one, and deferred the two items that determine whether enterprises can actually build on this model long-term: the weights and the license. Both of those now have a clock running on them, which is exactly why they deserve tracking rather than assumption.

## 03 — BenchmarksWhat the vendor table *actually* shows.

Every figure in this section comes from Alibaba’s own benchmark table on the release post — self-reported scores, scored against Claude Opus 4.8, Claude Fable 5, GPT-5.6 Sol, Gemini 3.1-Pro, and its own predecessor, Qwen3.7-Max. Treat them as the vendor’s case, not a neutral evaluation. Read that way, the table is more interesting than a clean sweep would be: Alibaba published rows where its model loses, and the wins cluster in a specific place.

#### Qwen3.8-Max vs. the field · selected vendor-table rows

Source: Qwen Team release blog, full benchmark table (vendor-stated) · Aug 3, 2026
Alibaba published individual rows with no roll-up, so we computed the margins ourselves. In the table below, every margin is our own arithmetic on the vendor’s published rows: the Qwen3.8-Max score minus the strongest rival score on the same row.

| Derived comparison of Qwen3.8-Max against the strongest rival model on each vendor-published benchmark row, with the margin computed as the Qwen score minus the strongest rival score. |  |  |  |  | 
|---|---|---|---|---|
| Benchmark | Qwen3.8-Max | Strongest rival | Rival score | Margin | 
|---|---|---|---|---|
| Rows where the vendor table shows Qwen3.8-Max ahead |  |  |  |  | 
| OSWorld-Verified | 86.1 | Claude Fable 5 | 85.0 | +1.1 | 
| PaperBench | 93.0 | GPT-5.6 Sol | 90.5 | +2.5 | 
| IFBench | 82.8 | Qwen3.7-Max | 79.1 | +3.7 | 
| Rows where a rival leads |  |  |  |  | 
| Terminal-Bench 2.1 | 86.6 | GPT-5.6 Sol (max) | 88.8 | −2.2 | 
| RecreationBench | 51.7 | Claude Fable 5 | 56.1 | −4.4 | 
| SWE-bench Pro | 67.7 | Claude Fable 5 | 80.0 | −12.3 | 
| FrontierSWE | 73.5 | Claude Fable 5 | 88.8 | −15.3 | 
| DeepSWE 1.1 | 56.6 | GPT-5.6 Sol | 73.0 | −16.4 | 

The shape is consistent. Qwen3.8-Max’s wins sit in **computer use, research-workflow reproduction, and instruction following** — the agentic-operations cluster. Its deficits sit in **repo-scale software engineering**, where the vendor table itself shows double-digit gaps to Claude Fable 5 on SWE-bench Pro and FrontierSWE. Elsewhere in the table Alibaba reports a GPQA Diamond of 92.6, and the release cites rankings on Alibaba’s own internal arenas (5th in its Text Arena, 2nd in Vision, 4th in Frontend Code) — vendor-run leaderboards, not an independent community ranking.

The generational jump is real even on the rows it loses. On DeepSWE 1.1, Qwen3.8-Max scores 56.6 against its predecessor Qwen3.7-Max’s 21.6 — a 35-point improvement in one generation on the vendor’s own numbers, even though it still trails GPT-5.6 Sol’s 73.0 on the same row. A model family that closes gaps at that rate is worth re-evaluating every cycle regardless of where it stands today.

## 04 — Methodology Fine PrintRead the *footnotes* before the scores.

Day-one coverage of this release is a case study in why primary sources matter. MarkTechPost’s same-day write-up claimed: “No benchmark table, license, or activated-parameter count has been published.” Two of those three claims do not survive contact with the release post itself, which carries a full benchmark table under an explicit heading and states the 95B active-parameter figure twice. Only the license claim holds — no license text appears anywhere on the page.

*weaker*prior model rather than the prior flagship, which flatters the generational delta on those rows. Second, Alibaba’s own footnotes disclose generous evaluation budgets: Terminal-Bench 2.1 ran with a 5-hour timeout and a 131,072-token output cap, and PaperBench allowed up to 12 hours per run, averaged over three runs. Long-budget harnesses can produce scores a production configuration will not reproduce.

Neither caveat is a scandal — disclosing your harness in 20 numbered footnotes is better practice than most launches manage. But together they define how to use this table: as Alibaba’s best case under favorable budgets, against a baseline of its own choosing on the multimodal rows. The gap between a 5-hour-timeout benchmark score and what your agent does under a production time budget is precisely the gap your own evaluation has to measure.

## 05 — Pricing & APIThe price everyone cites and *nobody* can point to.

The circulating price for Qwen3.8-Max is **$2.00 per million input tokens and $6.00 per million output tokens** — a standard hosted-API rate, with reported cache tiers of $0.25 per million for implicit cached input, $2.50 for explicit cache creation, and $0.17 for explicit cache reads. Multiple independent write-ups converge on those numbers to the cent, which suggests a real vendor source behind them. But we could not find any qwen3.8-listed rate on Alibaba’s own public Model Studio pricing page at the time of writing — its Qwen section listed model IDs only through the qwen3.7 family. Treat the figures as reported, not as a confirmed vendor list price, and note the cache tiers are the least-corroborated of the set.

##### per 1M tokens, in / out

Standard hosted-API rate, corroborated across several independent write-ups but absent from Alibaba's own public pricing page at the time of writing. At the reported $0.25 implicit-cache tier, cached input runs an eighth of the fresh-input rate.

##### max input tokens

The 1M headline resolves to a 991K effective input cap, dropping to 983K with extended thinking enabled. Output tops out at 131,072 tokens and the reasoning budget at 262,144, per MarkTechPost's reading of the model page. Rate limits: 2M tokens and 15K requests per minute.

##### reasoning_effort default

Three settings — xhigh, medium, low — with the most expensive one enabled by default and preserve_thinking on for all workloads. Reasoning tokens bill as output under the reported pricing, so a team that never touches this parameter pays the ceiling rate per task.

The operational trap is the default. With `reasoning_effort` shipping at `xhigh` and thinking preserved across turns, the out-of-box configuration is tuned for benchmark-grade output, not for cost. Before comparing this model’s economics against your incumbent, decide the effort level per workload class — and if you are already inside Alibaba’s ecosystem, reconcile these per-token rates against Alibaba’s token-plan pricing structure, which changes the effective math for prepaid quota.

## 06 — Autonomous ShowcasesVendor-run demos with *checkable* artifacts.

The most distinctive part of the release post is not the benchmark table — it is three long-horizon autonomy case studies. All three are Alibaba’s own showcases and should be labeled that way, but two of them left public artifacts that anyone can inspect, which is more receipts than launch-day demos usually offer.

##### commits · oh-my-cli

Roughly 16 days of fully autonomous operation (as of July 30, 2026) building a self-evolving coding harness: 265 commits, 127 pull requests, 151 issues, with the whole repository open-sourced on GitHub as a public trace.

##### AIME24 over the paper's method

Reproduced an arXiv paper on data selection for LLM reasoning end-to-end in about 5 days (~125 hours): ~7,600 lines of code, 1,100+ actions, 33 rounds of GPU training — then beat the paper's own method across 4 improvement rounds and 18 self-generated ideas.

##### of 526 human teams beaten

Entered the WWW2025 Multimodal Dialogue Intent Recognition Challenge on Alibaba's own Tianchi platform under a 24-hour limit and finished ahead of 458 of 526 human teams, climbing from 0.60 to 0.853 accuracy across 45 submissions.

The research-reproduction study comes with a round-by-round table on the release post, which we reproduce here with attribution — these are Alibaba’s numbers, not ours:

| Round-by-round results of Qwen3.8-Max improving on a reproduced research paper’s method, as published by the Qwen Team, showing the AIME24 score and gain versus baseline per round. |  |  |  | 
|---|---|---|---|
| Round | Best idea that round | AIME24 | Gain vs. baseline | 
|---|---|---|---|
| Baseline | Paper’s method, reproduced | 49.58% | — | 
| 1 | Split the data by difficulty before selecting | 50.42% | +0.84 | 
| 2 | Weight examples by an entropy–score gap | 51.67% | +2.09 | 
| 3 | Tune the selection width | 51.25% | +1.67 | 
| 4 | Count the hard decision points (“nhighgate”) | 52.29% | +2.71 | 

A fourth showcase is fully in-house: on Alibaba’s E-Commerce Bench — a 365-day simulated store operation starting from ¥100,000 in capital, spanning 12 store types, roughly 600 suppliers, 7,000 products, and 152 embedded fraudulent suppliers — Qwen3.8-Max finished at ¥416,252, a 4.16x return that Alibaba reports as 38% ahead of second-place GLM 5.2 and a 152% improvement over Qwen3.7-Max. A vendor benchmarking rivals on its own simulation deserves the heaviest discount of anything in the release. The more durable signal is the format itself: publishing inspectable, long-horizon artifacts — a public repo, a contest leaderboard — is becoming how labs argue for agentic capability, and it is a better argument than any static table row.

## 07 — Open WeightsA *dated commitment*, not a download link.

The release’s biggest strategic move ships nothing at all on day one. Qwen has never open-sourced a Max-class flagship — its 2026 pattern has been closed flagships, open everything else — and the release post commits to breaking that pattern within a week, for two models at once: Qwen3.8-Max itself and a new, otherwise-undescribed Qwen3.8-27B.

"This also marks the first time we will open-source the weights of a Qwen-Max-class model — the open weights will be released next week."— Qwen Team, release blog, August 3, 2026

##### Qwen3.8-*Max*

The first Max-class Qwen ever promised as open weights. At this scale, self-hosting is a serious infrastructure commitment even with only 95B active — the promise matters more as leverage and auditability than as something most teams will run themselves.

##### Qwen3.8-*27B*

Announced alongside Max with weights promised in the same window, and almost nothing else disclosed at the time of writing. If it lands, this is the variant most teams could realistically fine-tune and self-host.

The missing piece is the license. The release post contains no license text at all, and the license determines everything about what “open weights” is worth: commercial-use rights, redistribution, fine-tune ownership. Qwen’s 3.5 and 3.6 open releases shipped under Apache 2.0, which sets a reasonable expectation — but that is precedent, not a fact about 3.8, and a promise with no license attached is not yet something to build a procurement decision on. The contrast with Moonshot’s K2.7-Code release is instructive: that model shipped weights and license on launch day, while Qwen is running a closed-then-open sequence with a one-week gap. We cover exactly what to verify when the weights actually land — checksums, license text, config parity with the hosted model — in our pre-download checklist for the Qwen3.8 weights.

## 08 — ImplicationsWhat this means for *working teams*.

Alibaba made trialing this model unusually cheap. The release post includes official drop-in configurations for Claude Code, Codex, Qoder CLI, Qwen Code, and OpenClaw — pointing Claude Code at Qwen3.8-Max is two environment variables (`ANTHROPIC_BASE_URL` set to Alibaba’s Anthropic-compatible endpoint and `ANTHROPIC_MODEL=qwen3.8-max`), and the new Qwen-MM-Plugins library extends existing agent harnesses with image, video, and visual-tool capabilities. The evaluation cost is an afternoon, not a migration.

##### Desktop & *browser* automation

OSWorld-Verified 86.1 tops the vendor's own six-model comparison, and the wins cluster consistently in agentic operations. If computer use is your workload, this is the row that justifies a real evaluation on your own tasks.

##### Serious software engineering

The vendor's own table concedes double-digit gaps to Claude Fable 5 on SWE-bench Pro (67.7 vs 80.0) and FrontierSWE (73.5 vs 88.8). When the seller's best case shows the deficit, believe it.

##### Cheap structured trial

Official drop-in snippets for Claude Code, Codex, and other harnesses make an A/B trial nearly free to set up. Route a bounded slice of tasks through it, set reasoning_effort deliberately, and measure cost per completed task — not benchmark deltas.

##### Building on the weights

The weights are a dated promise and the license is undisclosed. Apache 2.0 precedent from Qwen 3.5/3.6 is encouraging but not binding. Nothing about a self-hosted deployment should be decided until both artifacts exist.

The trend this release confirms is that the open-weight frontier is now a staged-release game. Chinese labs in particular have spent 2026 converging on the same play: launch closed with a benchmark case, monetize the API window, then open the weights once the news cycle has done its work. Qwen running that sequence on its Max-class flagship — the tier it always kept closed — is the strongest version of the pattern yet, and it puts pressure on every lab whose open-weight story stops below flagship scale.

Looking forward, the week of August 10 is the real test. If both checkpoints land under a permissive license, a Max-class frontier model becomes self-hostable for the first time in Qwen’s history, and the 27B gives mid-size teams a fine-tunable sibling from the same generation. If the weights slip or the license arrives with meaningful restrictions, this release will be remembered as a benchmark story instead. For teams deciding how — or whether — to fold a model like this into production pipelines, that is a routing-and-governance question before it is a benchmark question, and it is exactly the kind of comparative evaluation our AI transformation engagements are built around.

## 09 — ConclusionJudge the release *twice*.

### Half of this release shipped. The other half is a promise with a date on it.

What shipped on August 3 is substantial: a 2.4T / 95B-active flagship with genuine, vendor-documented strength in computer-use and agentic workloads, a benchmark table honest enough to include its own losses, methodology footnotes worth reading, and an API cheap to trial through *drop-in* coding-assistant configs.

What did not ship is the part that changes procurement: the weights are a one-week promise, the license is undisclosed, and the widely-cited $2 / $6 pricing was still absent from Alibaba’s own public price page at the time of writing. None of that is disqualifying — but each item deserves to be tracked to resolution rather than assumed, because each one has a specific way it could disappoint.

The practical stance: evaluate the hosted model now on the workloads where the vendor’s own table is strongest, keep your coding stack where the same table concedes the gap, and hold every self-hosting decision until the weights and license actually exist. A release this staged should be judged in stages.
