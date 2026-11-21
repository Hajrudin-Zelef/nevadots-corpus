---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-code-2-review-benchmarks-pricing-maia-200
title: "Microsoft MAI Code 2 Review: Benchmarks, Pricing & Maia 200"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "maia", "code", "models", "ignite", "cost", "seven", "microsofts"]
source: https://www.alekseialeinikov.com/en/blog/topics/ai/microsoft-mai-code-2-review-benchmarks-pricing
source_anchor: ""
source_lines: [1, 104]
section: "Copilot_Microsoft"
---
At **Microsoft Ignite** (November 17), Microsoft made the move the entire cloud industry saw coming for two years — and still blinked when it landed. Standing on stage in San Francisco, Microsoft AI CEO Mustafa Suleyman unveiled **seven in-house commercial AI models**, backed by Microsoft’s own 3nm **Maia 200** inference silicon, and pointed them straight at the pricing power of both **Anthropic** and **OpenAI**.

The headline number lit up Hacker News (500+ points, 330+ comments) within hours: **MAI Code 2 reaches 96% of Claude Opus 5.5’s coding pass rate for roughly 5% of the cost, and 98% of GPT-6.1 Sol for 40% of the cost.**

That claim is half genuine engineering breakthrough and half clever framing. This review traces **how Microsoft got from OpenAI’s exclusive cloud banker to a vertically integrated AI lab**, breaks down all seven models, puts Microsoft’s exact MSBench scores under a microscope (including the **14.4-point SWE-Atlas cliff**), and separates promotional pricing from the standard list bill.

**The short verdict.** Microsoft is no longer just reselling OpenAI APIs. With **Maia 200 + MAI Code 2 + Decision 1.1**, it now owns a complete vertical stack like Google. `MAI Code 2` is a legitimate cost weapon for high-volume enterprise coding inside GitHub Copilot (`$0.50/$2.50` promo → `$1/$5` list), **but it is not a drop-in Opus 5.5 replacement** on complex repository-scale tasks (`SWE-Atlas`: `43.1%` vs `57.5%`) or long-horizon autonomous agents.


## How Microsoft Got Here: From OpenAI’s Banker to Vertical AI Lab

People keep asking whether this Ignite wave is Microsoft’s “first” neural network release. Strictly speaking, no — Microsoft Research shipped the small open-weight **Phi** family for years, and Microsoft AI released early first-generation MAI models before and during Microsoft Build. What changed at Ignite is **commercial intent and vertical integration**.

For five years, Microsoft’s AI margin had an uncomfortable structural flaw: every time GitHub Copilot or Microsoft 365 Copilot usage surged, Microsoft paid a massive external inference tax — either in NVIDIA GPU capex or in revenue share / API payouts to OpenAI and Anthropic. As OpenAI restructured toward full commercial independence, Microsoft executed a quiet four-stage pivot:

1. **The Exclusive Cloud Banker (2019–2023):** Microsoft invested billions into OpenAI, built Azure’s supercomputing clusters and wired GPT models into GitHub Copilot and Office. Distribution was Microsoft’s moat, not model weights.
2. **Building Microsoft AI (March 2024):** Microsoft hired DeepMind co-founder**Mustafa Suleyman** to build an internal frontier model organization alongside the research-oriented Phi team.
3. **Custom Silicon (`Maia 200`):** Microsoft rolled out**Maia 200** , a 3nm second-generation AI accelerator built specifically for high-efficiency inference, delivering roughly**1.7× more tokens per watt** across Azure. Just as Google’s TPUs let Gemini 4 Neon and Gemini 4 Hydrogen undercut GPU-bound rivals, Maia 200 gave Microsoft its own cost floor.
4. **Ignite Commercial Wave:** Seven production-grade MAI models shipped in one wave, co-optimized for Maia 200, plus**MAI Licensed Weights** — letting large enterprise customers license and run customized MAI weights inside dedicated Azure environments.

## The Seven Models Announced at Ignite

Rather than trying to beat every lab on a single giant generalist model, Microsoft aimed each MAI model at a high-volume enterprise cost center:

| The Seven Models Announced at Ignite |  |  |  | 
|---|---|---|---|
| Model | Role & Architecture | Headline Claim / Metric | Availability | 
|---|---|---|---|
| **MAI Code 2** | Flagship coding sparse MoE ( `1.05M` ctx:`922K` in /`128K` out) | `63.2%` MSBench avg (`96%` of Opus 5.5,`98%` of GPT-6.1 Sol) | **GA** in GitHub Copilot | 
| **MAI Code 2 Flash** | Fast coding MoE ( `~12B` active params,`1M` ctx) | Matches GPT-6 Luna / Haiku 5.5 at `20%–65%` of their cost | **GA** in Copilot; private preview in Foundry | 
| **MAI Cyber 2 Flash** | AppSec & SOC vulnerability triage / patching | Beats GPT-6 Astra on **CyberGym-E2E** ; powers Defender ISOC | Security partner preview; Defender rollout mid-Dec | 
| **Decision 1.1** | Fast routing, classification & verification (Gemma-based MoE) | Up to **50× faster** and**79× cheaper** than GPT-6.1 Sol | **GA** in Microsoft Foundry | 
| **MAI Image 2.7 (+ Flash)** | Enterprise image generation & editing | **#1 on Artificial Analysis** for image editing (`1/5` cost of GPT Image 2.6 Sunburst max) | Preview in Microsoft Foundry | 
| **MAI Realtime 1** | Native speech-to-speech conversational audio | Outperforms GPT-Realtime 2.1, starting at **$0.01 / audio min** | Rolling out in Foundry | 
| **MAI Transcribe 2** | Multilingual speech transcription (Dragon Copilot) | Halves multilingual error rates across 10M monthly patient encounters | Production (Dragon Copilot) | 

Notice the one model already validated on an independent leaderboard: **MAI Image 2.7 (#1 on Artificial Analysis for image editing)**. For `MAI Code 2`, however, all numbers currently come from Microsoft’s own MSBench evaluation — which is why we need to inspect both the rest of the MAI lineup and the raw coding table, not just the press-release average.

### Deep Dive: What Microsoft Ships Beyond MAI Code 2

Most headlines stop at `MAI Code 2`, but Microsoft’s official release makes clear that the other five models are designed to attack specific enterprise margins where general frontier models are wildly overpriced:

- **MAI Cyber 2 Flash (Security & Patching — `$0.45` vs `$34.38` per task):** Built with a`1M` -token context window for defensive vulnerability discovery, threat triage and automated patching. On Microsoft’s**CyberGym-E2E** remediation benchmark, it scores**92.3%** task success against**GPT-6 Astra’s 91.5%** at**under 2% of the cost per task** (`$0.45` vs`$34.38` offline). To curb offensive abuse, Microsoft gates access through verified defender harnesses (including*Codename MDASH* and Defender ISOC) rather than an open pay-as-you-go token endpoint.
- **Decision 1.1 (Enterprise Routing — `$0.042 / 1M` tokens):** Built on Google DeepMind’s open-weight**Gemma** architecture (Gemma-based MoE), Decision 1.1 is GA in Microsoft Foundry as a dedicated classifier, verifier and traffic router. Microsoft positions it as up to**50× faster** and**79× cheaper** than GPT-6.1 Sol — letting platform teams filter and route prompts before paying frontier token rates.
- **MAI Image 2.7 & Flash (Visual Synthesis — `~$35` / `~$10` per 1,000 images):** Available in Foundry public preview,**MAI Image 2.7** focuses on identity-preserving, localized image editing (ranked**#1 on Artificial Analysis for image editing** at roughly`1/5` the cost of GPT Image 2.6 Sunburst max), while**MAI Image 2.7 Flash** (`~$10 / 1,000` images) drives low-latency interactive editing in Microsoft’s*Canvas* workspace.
- **MAI Realtime 1 (Native Voice Agents — from `$0.01 / audio min`):** Microsoft’s first native speech-to-speech model handles live interruptions, background noise and steerable prompting while outperforming**GPT-Realtime 2.1** . Note the billing nuance Microsoft discloses:`$0.01 / audio min` covers the audio layer, while calls to an underlying reasoning engine are billed separately by token.
- **MAI Transcribe 2 (60-Language ASR in Production):** Supports**60 languages** with speaker diarization, word-level timestamps and heavy noise robustness. Microsoft already runs it in production inside**Dragon Copilot Transcribe** , reporting a**50% cut in multilingual error rates** across**10 million monthly clinical encounters** .

## Exact MSBench Benchmarks: The 14.4-Point SWE-Atlas Cliff

Microsoft evaluated **MAI Code 2**, **GPT-6.1 Sol** and **Claude Opus 5.5** on its **MSBench** platform via **GitHub Copilot CLI** with sub-agents disabled and all models set to **max reasoning effort**:

| Exact MSBench Benchmarks: The 14.4-Point SWE-Atlas Cliff |  |  |  |  | 
|---|---|---|---|---|
| Benchmark | **MAI Code 2** | **GPT-6.1 Sol** | **Claude Opus 5.5** | **MAI vs Opus** | 
|---|---|---|---|---|
| **DeepSWE 1.1** | **73.2%** | 74.6% | 69.0% | **+4.2 pts** | 
| **Terminal-Bench 2.1** | **84.6%** | 85.0% | 78.7% | **+5.9 pts** | 
| **SWE-Atlas** | **43.1%** | 47.4% | **57.5%** | **−14.4 pts** | 
| **FrontierCode v1.1** | **52.0%** | 49.4% | **56.0%** | **−4.0 pts** | 
| **Average Pass Rate** | **63.2%** | **64.1%** | **65.3%** | **−2.1 pts** | 

Looking at the four individual bars tells a much sharper engineering story than the “96% of Opus” headline:

1. **Where MAI Code 2 genuinely shines (`DeepSWE 1.1` & `Terminal-Bench 2.1`):** On focused debugging, shell execution and bounded engineering fixes,`MAI Code 2` beats Claude Opus 5.5 by +4.2 to +5.9 points and sits within 1.4 points of GPT-6.1 Sol. Early developers testing it in GitHub Copilot on Hacker News confirmed that for everyday PRs and fixes, it feels surprisingly close to Sol and Opus.
2. **Where the model cracks (`SWE-Atlas`):** On**SWE-Atlas** , MAI Code 2 falls to**43.1%** against Opus 5.5’s**57.5%** — a**14.4-percentage-point drop** . That aligns directly with Microsoft’s own model-card disclaimer:**MAI Code 2 is not yet tuned for long-horizon autonomous agents or fundamental scientific reasoning.**
3. **Why a “4% average gap” feels bigger in production:** As multiple engineers on Reddit and HN pointed out, averaging four benchmarks smooths out tail failures. In real repositories, missing 14% more hard cross-file invariants turns into subtle regressions that eat the time you saved on token cost.

## Pricing Reality Check: Promo Rate vs Standard List Rate

Microsoft’s pricing is genuinely aggressive — even after you strip away the promotional discount. Here are the exact published API rates (with the 50% introductory promo active through **May 17, 2027**) alongside current frontier peers:

| Pricing Reality Check: Promo Rate vs Standard List Rate |  |  |  |  | 
|---|---|---|---|---|
| Model | Input / 1M | Output / 1M | Cache Read / 1M | MSBench Avg Cost / Task | 
|---|---|---|---|---|
| **MAI Code 2 Flash (Promo → List)** | **$0.05** → $0.10 | **$0.25** → $0.50 | $0.005 → $0.01 | Ultra-low | 
| **MAI Code 2 (Promo until May 2027)** | **$0.50** | **$2.50** | **$0.025** | **$0.309** | 
| **MAI Code 2 (List after May 2027)** | **$1.00** | **$5.00** | **$0.050** | **~$0.618** | 
| **Gemini 4 Hydrogen** | $1.50 | $7.50 | $0.075 | Low workhorse | 
| **GPT-6.1 Sol** | ~$2.00 | ~$10.00 | Cached tier | **$0.761** | 
| **Gemini 4 Neon** | $3.00 ($1.50 Flex) | $15.00 ($7.50 Flex) | $0.15 | Mid frontier ( `61` AA idx) | 
| **Claude Opus 5.5** | $4.00 | $20.00 | $0.20 | **$6.306** | 

Two pricing facts jump off the chart:

- **The “5% of Opus” math is real during the promo — and ~10% after May 2027:** On MSBench, Opus 5.5 averaged**$6.306 per task** (burning`$10.72` per DeepSWE 1.1 task!) because max-effort reasoning burns huge output volumes at`$20/1M` . MAI Code 2 averaged**$0.309** at promo rates (`4.9%` of Opus) — and even when prices double to`$1.00 / $5.00` in May 2027,**`~$0.618` per task is still 10× cheaper than Opus 5.5** and slightly cheaper than GPT-6.1 Sol (`$0.761` ).
- **The ecosystem catch:** Unlike Gemini 4 Neon (live on OpenRouter and Google AI Studio on day one) or open-weight models you can self-host, MAI models live tightly inside**GitHub Copilot** and**Microsoft Foundry / Azure** . On Hacker News, developers outside the Microsoft stack asked the obvious question:*if you are not already on Azure or Copilot, why adopt a closed proprietary model that has no independent Artificial Analysis score yet?*

## What This Means for Engineering Teams

Microsoft’s Ignite launch changes the economics of enterprise AI in three concrete ways:

1. **If your team already uses GitHub Copilot:** Switch routine coding, test generation and terminal debugging to**MAI Code 2** (and**MAI Code 2 Flash** for boilerplate) immediately — the cost-per-PR drop is massive, and you keep Opus 5.5 or Gemini 4 Neon for hard architectural refactors.
2. **If you run an AI Gateway on Azure / Foundry:****Decision 1.1** (`GA` , up to 50× faster and 79× cheaper than GPT-6.1 Sol) is arguably the quietest sleeper hit of Ignite — using a dedicated Gemma-MoE router to classify and triage prompts before they ever hit a`$15–$20/1M` model pays for itself on day one.
3. **Wait for third-party verification before replacing your hardest agent tier:** Until`MAI Code 2` appears on independent leaderboards (Artificial Analysis, Vals.ai, SWE-bench Verified outside Copilot CLI) and closes its`SWE-Atlas` gap, treat it as a**high-efficiency daily workhorse** , not an autonomous long-horizon architect.

From the community

## Discussion on Mastodon & Bluesky

Replies to this article from the open web, pulled in live. No ads, no tracking scripts.

Reply there — it shows up here automatically.
