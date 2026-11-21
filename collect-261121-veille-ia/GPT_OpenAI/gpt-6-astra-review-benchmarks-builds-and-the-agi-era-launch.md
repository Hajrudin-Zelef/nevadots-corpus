---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-review-benchmarks-builds-and-the-agi-era-launch
title: "GPT-6 Astra Review: Benchmarks, Builds and the 'AGI Era' Launch"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "openai", "gpt-6", "tokens", "launch", "osworld", "review", "model"]
source: https://codingfleet.com/blog/gpt-6-astra-review/
source_anchor: ""
source_lines: [1, 241]
section: "GPT_OpenAI"
---
# GPT-6 Astra: A Comprehensive Review of OpenAI's "AGI-Era" Frontier Model

Released **September 3, 2026** · Predecessor: GPT-5.6 Sol · API: `gpt-6-astra` · $10 / $50 per 1M tokens

Every benchmark, every major build, and the full Critical-cybersecurity story — with sources.

## 1 · The Launch: "Welcome to the AGI era"

OpenAI released **GPT-6 Astra** on **September 3, 2026**, calling it *"the world's most intelligent and aligned model"* and, in the words of president Greg Brockman, a *"generational leap"* that could one day be seen as the arrival of artificial general intelligence (Axios; Wikipedia). According to Axios, Astra is the product of OpenAI's largest-ever training run, using **more than 100,000 GPUs** at its Stargate site in Texas, and the first release where earlier OpenAI models supervised the training of the new one (Vellum).

Astra is not pitched as a better chatbot — it is pitched as a **computer operator**. In OpenAI's launch demos it laid out a PCB in KiCad, built a 3D city in Unity, animated an automobile transmission in FreeCAD and Blender, drafted a tax return from a W-2, created an eBay listing and a 3D game from voice input alone, and formatted a legal agreement while booking a tennis court and searching for food (VentureBeat; Axios).

### Launch timeline

*"Ten advances in mathematics and theoretical computer science"*— an internal Astra produced new results on ten long-standing open problems, each with a machine-checkable proof in the Lean theorem prover, and is named

**"our next major model"**(Emergent).

*ARC-AGI-3 99.9% uses OpenAI's provider-adapter harness; independent stateless runs score far lower (see §6 and sources).

#### GPT-6 Astra (OpenAI)

Frontier Flagship · Released Sep 3, 2026
**Pricing:** $10 / $50 per 1M tokens (in / out); Fast mode ~2× price at 2.5× speed

**Context:** Up to 1M tokens (MRCR 96.3% at 512K–1M)

**Headlines:** Saturates FrontierMath Tier 4 (97.6%) & ExploitBench (100%); state-of-the-art computer use; Cuts OSWorld time-per-task ~47%; First OpenAI model rated "Critical" for cybersecurity; Largest-ever training run (100K+ GPUs).

#### GPT-5.6 Sol (Predecessor)

OpenAI's 2026 mid-tier flagship
**Pricing:** $4 / $20 per 1M tokens (promotional, guaranteed through Nov 21, 2026)

**Baseline for deltas:** 65.7% OSWorld 2.0 · 78.5% ExploitBench · 37.3% Terminal-Bench 4.0 · 83.0% FrontierMath T4 · 48.2% honeypot cheating (no safeguards). Astra improves on nearly every row, usually with fewer output tokens.

### 🎛 Interactive Capability Explorer

Axis values: OSWorld 2.0 (72.6 / 65.7 / 77.9* / 70.2), FrontierCode 1.1 Extended (64.5 / 60.6 / 63.6 / 63.6), FrontierMath Tier 4 v2 (97.6 / 83.0 / 87.8 / 73.2), ARC-AGI-2 (95.0 / 92.5 / 90.0 / 90.4), AutomationBench (41.4 / 18.1 / 31.4 / 26.9), ExploitBench (100 / 78.5 / 70 / 70). *Anthropic reports 77.9% for Fable 5.1 on a different OSWorld release and says it is not comparable. All scores vendor-reported unless noted.

*Fable 5.1's OSWorld 77.9% is from a different OSWorld release (Anthropic). Values as published by vendors; see §6 for harness caveats on ARC-AGI-3.

Percentage-point change, GPT-6 Astra minus GPT-5.6 Sol, across headline evaluations (higher = bigger Astra leap; vendor-reported).

Alignment rows are "lower is better": honeypot unauthorized-access rate dropped from 48.2% (Sol) to 0.0% (Astra). Width is scaled to the largest absolute delta for readability.

Astra is priced 2.5× GPT-5.6 Sol's promotional rate and matches Anthropic's Fable 5.1. OpenAI's counter-argument: "*price per task is what matters*" — Astra posts higher scores on several evaluations while using fewer output tokens (Vellum; CloudZero).

## 2 · The Headline Claims

OpenAI's announcement tables, reproduced and cross-checked by Vellum and DataCamp, show Astra posting the highest scores OpenAI has ever published on abstract reasoning, math, and cybersecurity:

- **FrontierMath Tier 4 v2: 97.6%** — a research-grade math benchmark designed to stay ahead of AI; "saturation" is a fair reading given the ceiling (DataCamp). Epoch AI, which runs FrontierMath, notes OpenAI funded its development and has exclusive access to part of it (Vellum).
- **ARC-AGI-3: 99.9%** under OpenAI's provider-adapter harness, vs 7.8% for Sol and 30.2% for Opus 5.**Read carefully:** ARC Prize's independent stateless runs score roughly 17–63% depending on reasoning tier; the ~99.9% figure needs the stateful adapter harness and a comprehensive run costing tens of thousands of dollars (DataCamp). The New Stack cites 98.6% for the same benchmark, likely a different harness configuration (Vellum).
- **ExploitBench: 100%** — turning known vulnerabilities into working exploits, vs 78.5% for Sol and 70% for Fable 5.1 / Opus 5. This is exactly why the capability ships gated (see §7).

**⚠️ The independent counterweight:**On the third-party

**Artificial Analysis Intelligence Index v4.1.1**, Astra scores

**61.2**— behind Claude Fable 5.1 (65.7), Claude Opus 5 (63.1) and Claude Fable 5 (62.1). "World's most intelligent" is a claim about OpenAI's own tables, not a consensus measurement (Vellum; per-model values also in OpenAI's announcement table: Astra 61.2 / Sol 60.9 / Fable 5.1 65.7 / Fable 5 62.1 / Opus 5 63.1 / Gemini 3.8 Flash 58.7).

## 3 · Computer Use & Agentic Benchmarks

Computer use is the headliner. Astra can drive a desktop, fill out forms, update CRMs, run QA on a site it just built, and troubleshoot what's on screen (The New Stack). On **OSWorld 2.0** it scores 72.6% in ~40 minutes per task vs Sol's 65.7% in ~75 minutes — a **47% cut in time per task**, which is roughly half the cost to run the same workload (DataCamp).

With the updated Codex harness, OpenAI reports Astra completes **Mind2Web** browser tasks **1.9× faster** than the current Sol setup (DataCamp; The New Stack).

| Computer Use Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Fable 5 | Claude Opus 5 | 
|---|---|---|---|---|---|
| **Agents' Last Exam** | 59.3% | 53.6% | — | — | 55.5% | 
| **OSWorld 2.0** | 72.6%* | 65.7% | 77.9%† | — | 70.2% | 
| **ScreenSpot-Pro** | 92.7% | 76.9% | — | 87.3% | — | 
| **Mind2Web speedup (Codex)** | 1.9× faster | baseline | — | — | — | 

*Astra: ~40 min/task vs Sol's ~75 min (47% less). †Different OSWorld release (Anthropic). Source: OpenAI launch tables as reproduced by DataCamp and Vellum.

## 4 · Professional Work

OpenAI positions Astra as a "step change" in professional work — finished documents, slide decks, spreadsheets and analyses that follow your templates (DataCamp). On **AutomationBench** — the biggest professional-work gap in the whole announcement — Astra's 41.4% more than doubles Sol's 18.1%.

| Professional Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Fable 5 | Claude Opus 5 | 
|---|---|---|---|---|---|
| **AutomationBench** | 41.4% | 18.1% | 31.4% | 17.4% | 26.9% | 
| **BenchCAD (Vision2Code)** | 95.9% | 83.3% | 84.3%‡ | — | — | 
| **BrowseComp** | 91.5% | 90.4% | — | — | — | 

‡OpenAI notes Claude runs used modified evaluation settings (Vellum; The New Stack).

## 5 · Coding: Strong, but Not Clearly the Leader

OpenAI calls Astra "the best model for software engineering to date." The tables are more contested than that sentence: Meta's **Muse Spark 1.3** edges Astra on DeepSWE (75.4% vs 74.1% per Meta's own run), the FrontierCode rows go to Claude Fable 5, and the **Artificial Analysis Coding Agent Index v1.4** is effectively a three-way tie at the top (Opus 5 68.1 / Fable 5 67.2 / Astra 67.0) (Vellum).

| Coding Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Fable 5 | Claude Opus 5 | Gemini 3.8 Flash | 
|---|---|---|---|---|---|---|
| **Terminal-Bench 4.0** | 57.7% | 37.3% | 55.8% | 42.0% | 52.3% | 19.1% | 
| **DeepSWE v1.1** | 74.1% | 72.7% | 67.4% | 69.9% | 73.7% | 73.8% | 
| **FrontierCode 1.1 Extended** | 64.5% | 60.6% | 63.6% | 64.9% | 63.6% | 56.3% | 
| **FrontierCode 1.1 Main** | 53.3% | 47.5% | 50.9% | 53.5% | 53.4% | 43.6% | 
| **Internal Database Migration** | 63.9% | 42.7% | 57.8% | 50.3% | — | — | 
| **Terminal-Bench Science** | 64.6% | — | 52.6% | — | ~30%§ | — | 
| **AA Coding Agent Index v1.4** | 67.0 | 65.1 | — | 67.2 | 68.1 | 61.2 | 

§Public leaderboard tops out around 30% for Opus 5. DeepSWE: Meta reported 75.4% for Muse Spark 1.3 at max reasoning (Vellum).

**Developer-facing win:**In Codex, Astra keeps

**notes across context windows**instead of repeatedly compressing everything into one summary, and earlier windows stay searchable — so long debugging sessions stop losing the detail of why the first fix failed. It can also ask a question without stopping work that doesn't depend on the answer. Experimental behind a

`config.toml` setting; slated to become default (The New Stack; Vellum).
## 6 · Math, Reasoning & the Academic Rows

This is where Astra separates from the field the most. It saturates FrontierMath Tier 4 and posts a 96.0% on GPQA Diamond, the highest published score. It also produced two further proofs on gaps between prime numbers at launch — following the ten formal Lean-verified results an internal version found in August, which cost roughly **$2,000 in tokens at Sol rates** to discover (Vellum; OpenAI — Ten advances in mathematics).

| Reasoning / Academic | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Fable 5 | Claude Opus 5 | Gemini 3.8 Flash | 
|---|---|---|---|---|---|---|
| **FrontierMath Tier 4 v2** | 97.6% | 83.0% | 87.8% | 87.8% | 73.2% | — | 
| **GPQA Diamond** | 96.0% | 94.6% | 93.7% | — | 93.7% | 95.3% | 
| **Humanity's Last Exam (w/ tools)** | 57.2% | — | 65.0% | 63.8% | 63.6% | — | 
| **ARC-AGI-1** | 98.5% | 97.5% | 97.5% | 98.5% | 97.5% | — | 
| **ARC-AGI-2** | 95.0% | 92.5% | 90.0% | 89.2% | 90.4% | — | 
| **ARC-AGI-3 (adapter)** | 99.9%* | 7.8% | — | — | 30.2% | — | 
| **AA Intelligence Index v4.1.1** | 61.2 | 60.9 | 65.7 | 62.1 | 63.1 | 58.7 | 

*ARC-AGI-3 requires OpenAI's stateful provider-adapter harness; independent stateless runs score ~17–63% (DataCamp). Highlight the one anti-claim: on Humanity's Last Exam, Astra's 57.2% trails every Claude in the table — it is not sweeping every reasoning eval (Vellum).

## 7 · Science & Health

| Science Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Fable 5 | 
|---|---|---|---|---|
| **GeneBench Pro** | 37.8% | 28.7% | — | — | 
| **LifeSciBench** | 60.3% | 59.9% | — | — | 
| **HealthBench Professional** | 63.4% | 60.5% | 56.6% | 60.9% | 
| **MedChemBench (internal)** | 49.3% | 47.4% | — | — | 

Source: OpenAI, "GPT-6 Astra" announcement, Science table (via Vellum).

## 8 · Cybersecurity: The First "Critical" Model

This is the section that changed how the model shipped. On September 2, OpenAI's Path to Astra post confirmed Astra meets the **Critical** threshold under the Preparedness Framework: with the right tools and access, it can find previously unknown vulnerabilities in well-protected systems and develop exploits without step-by-step human guidance (CNBC). OpenAI delayed parts of development and re-started its large frontier RL run on August 28 after hardening training infrastructure following the Hugging Face incident (Vellum).

| Cyber Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Opus 5 | 
|---|---|---|---|---|
| **ExploitBench** | 100% | 78.5% | 70.0% | 70.0% | 
| **ExploitGym** | 42.4% | 30.3% | 30.4% | — | 
| **Contamination-controlled V8 port** | 39.0% ACE | 5.5% ACE | — | — | 
| **SRE-Bench (1 attempt)** | 88.0% | 55.9% | 12.5% | — | 
| **SRE-Bench (4 attempts)** | 99.2% | 68.7% | — | — | 
| **FrontierCyber (Irregular lab)** | 86 / 226 | 34 / 226 | — | — | 

V8 port built from 20 high-severity V8 vulnerabilities disclosed June–August 2026; during the eval Astra also discovered and used **two previously unknown zero-days**, which OpenAI is disclosing to maintainers. The New Stack notes OpenAI removed the usual six-hour time limit for both models on ExploitGym (Vellum). Independent lab Irregular reported Astra solving 86 of 226 FrontierCyber challenges vs 34 for Sol, including zero-day findings in browsers and a cloud database (DataCamp).

**Why it's gated:**The version most people touch is more limited than the numbers above. Astra results reflect

**Daybreak Blue**access, not the default production configuration, and the shipping model refuses advanced cyber work such as creating proof-of-concept exploits. On the API, a cybersecurity safety check stops a task outright instead of pausing for review; at launch users should expect slowdowns, pauses or blocks — sometimes on work unrelated to security (Vellum).

## 9 · Alignment: Best-in-Table Numbers, One Real Regression

| Alignment Metric | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | Claude Opus 5 | 
|---|---|---|---|---|
| **ExploitGym honeypot cheating ↓** | 0.0% | 48.2% | — | — | 
| **Computer-use safety benchmark ↓** | 2.4% | 22.0% | 9.5% | 11.5% | 
| **Misaligned-outcome rate (realistic work) ↓** | 3.4% | 18.8% | — | — | 
| **Internal hallucination benchmark ↓** | 4.2% | 12.2% | — | — | 
| **Cyber jailbreak refusals ↑** | 91.5% | 59.0% | — | — | 

Arrows: ↓ = lower is better, ↑ = higher is better. Sol ran without production safeguards on the honeypot test; both models ran in a simulated environment with safeguards in observation-only mode (Vellum; DataCamp).

**⚠️ The regression OpenAI disclosed itself:**Astra's written reasoning is

**harder to monitor**than Sol's — it controls its chain-of-thought more tightly on simpler tasks and needs fewer written steps. UK AISI found Astra could evade monitoring under adversarial prompting. Chief scientist Jakub Pachocki:

*"progress in intelligence does not guarantee progress in alignment,"*and OpenAI will withhold scaling until it regains confidence in monitoring future models (Vellum; DataCamp).

## 10 · Long Context: A Quiet 1M-Token Model

| OpenAI MRCR v2 8-needle | GPT-6 Astra | GPT-5.6 Sol | 
|---|---|---|
| **256K–512K band** | 100.0% | 91.5% | 
| **512K–1M band** | 96.3% | 73.8% | 

Retrieval reliability at 1M tokens is a genuine step up over Sol for document-heavy pipelines (DataCamp).

## 11 · What People Did With It (Official & Community)

The most convincing signal isn't the tables — it's what people shipped in the first 48 hours. Here are the builds we could verify, split into **official/OpenAI** and **independent** sources with links.

An internal version of Astra produced new results on ten long-standing open problems in mathematics and theoretical computer science, each with a machine-checkable proof in the Lean theorem prover (published Aug 1, 2026). At launch, Astra produced **two further proofs on gaps between prime numbers** (Emergent; Axios; Vellum).

In OpenAI's briefing and promotional video, Astra operated real software: laying out a printed circuit board in KiCad, building a 3D city scene in Unity, creating an animated automobile transmission in FreeCAD and Blender, and filling out a tax-return draft from a W-2 — plus voice-driven 3D game creation, an eBay listing, a legal agreement, and a tennis-court booking (Axios, VentureBeat, 9to5Mac).

On OpenAI's own channel: **Ben Davis** built a playable voxel 3D history of London that transforms across medieval, Tudor and modern eras within the same map; **Peter Gostev** pushed a matcha shop website in new creative directions; **Tom Krcha** tackled a DEF CON puzzle with parallel agents.

Gave Astra an image of a house and asked for a 3D model with all details — toys, appliances, furniture. Result: a full 3D model reconstruction in Blender with editable geometry, running at 60fps as a locally rendered "game" on device (DataCamp).

Early-access reviewer Claire Vo reports Astra one-shotted the ChatPRD product-intelligence feature she couldn't crack with 5.6 Sol or Fable, finally cracked a months-long Divoom MiniToo hardware hack (CLI + live streaming display), built an AIM-style Mac app, and produced Blender 3D assets (a "Barbie Bench" and a kids' family app).

A hands-on review showing Astra driving Roblox Studio and Blender directly — "the first time I'm able to bring that idea to life" — plus near-perfect performance in desktop apps and browser-based workflows, with two cursors working simultaneously.

Reviewer Matt Shumer reports Astra adeptly uses tools like Unreal Engine to build complex environments — including a civilization running on Unreal's autonomous MetaHuman characters.

Testers repeatedly used Astra to generate playable 3D environments with working game mechanics, then kept building systems like traffic, zoning and utilities over extended multi-day runs — the long-horizon pattern the model was built for.

### Security research: Irregular lab's FrontierCyber run

Independent lab **Irregular** reported Astra solving **86 of 226 FrontierCyber challenges** versus 34 for GPT-5.6 Sol — including zero-day findings in browsers and a cloud database (DataCamp).

## 12 · Pricing & Availability

| Item | Details | 
|---|---|
| **API pricing (standard)** | $10 / $50 per 1M tokens (in / out). Cached input $1; cache writes $12.50; Batch & Flex at half rates; any prompt past 272K input tokens reprices the entire request (CloudZero). | 
| **Fast mode** | Up to 2.5× Standard speed at 2× price (≈$20 / $100 per 1M) (DataCamp; MindStudio). | 
| **Model ID / surfaces** | `gpt-6-astra` on the OpenAI API and Amazon Bedrock; also Azure (VentureBeat). ChatGPT Plus, Pro, Business, Enterprise; Astra Pro tier for Pro/Business/Enterprise; Enterprise off by default. Zero Data Retention for eligible API customers (DataCamp). | 
| **Relative pricing** | 2.5× GPT-5.6 Sol's promo rate ($4/$20); matches Claude Fable 5.1 ($10/$50); above Opus 5 ($5/$25); ≈8× Meta Muse Spark 1.3 ($1.25/$4.25) and Gemini 3.8 Flash ($0.75/$3.75) (Vellum). | 
| **Per-task economics** | Est. ~$167 per task on aggregate coding benchmarks — above GPT-5.6 — so usage credits drain faster, though Astra often uses fewer output tokens per task at equal quality (MindStudio). | 

## 13 · Verdict: The Model That Runs Your Computer

#### ✅ Where Astra is the clear pick

- **Computer & browser use:** OSWorld 2.0 at 47% less time-per-task; ScreenSpot-Pro 92.7%; the strongest "delegate the whole job" agent on the market.
- **Math & research-grade reasoning:** FrontierMath T4 saturation (97.6%), GPQA 96.0%, real new math results.
- **Professional artifacts:** AutomationBench 41.4% — more than double Sol — plus template-following documents, slides and spreadsheets.
- **Long-horizon agent work:** 1M-token retrieval (96.3% at 512K–1M) and Codex notes that survive context-window rollovers.
- **Alignment direction:** 0% honeypot cheating vs Sol's 48.2%; 3.4% misaligned-outcome rate.

#### ⚠️ Where to stay cautious

- **Coding leadership is contested:** Muse Spark 1.3 tops DeepSWE (75.4% vs 74.1% per Meta), Fable 5 wins FrontierCode rows, and the AA Coding Agent Index is a three-way tie.
- **Humanity's Last Exam:** 57.2% trails every Claude in OpenAI's own table (Fable 5.1 at 65.0%).
- **Harness caveats:** the 99.9% ARC-AGI-3 needs the stateful adapter harness; stateless calls score far lower (17–63% per ARC Prize).
- **Critical cyber capability is gated** behind Daybreak; the public model refuses exploit-creation work, and safety checks can pause unrelated tasks.
- **Monitorability regression:** CoT is harder to monitor; OpenAI itself ties future scaling to fixing this.
- **Cost:** 2.5× Sol's promo price; ~$167/task on coding aggregates.

**The one-line takeaway:** GPT-6 Astra is the first frontier model that feels like a colleague you hand a task to rather than a chatbot you babysit — with decisive wins in computer use, math and agentic scope, a cybersecurity capability so strong it ships deliberately hobbled, and a set of independent-index numbers that keep the "world's most intelligent" claim honest. If your work is multi-step, tool-heavy and long-horizon, this is the model to test first (DataCamp; Vellum; Unicodeveloper).

## 14 · Sources & Data Notes

All benchmark figures above are as published by their vendors or labs as of September 3–5, 2026, via the sources below. Headline scores are OpenAI-reported at maximum effort unless noted; OpenAI notes its evals ran in its research environment or via its API, which may differ from production ChatGPT. Independent checks (ARC Prize, Artificial Analysis, Epoch AI, UK AISI, The New Stack) and the caveats attached to each number are called out inline.
