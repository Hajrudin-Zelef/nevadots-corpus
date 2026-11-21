---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-release-benchmarks-pricing-limits
title: "MiniMax M3.1 Flash Release: Benchmarks, Pricing, Limits"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-23"]
keywords: ["minimax", "flash", "tokens", "artificial", "analysis", "model", "independent", "pricing"]
source: https://kie.ai/blog/minimax-m3-1-flash-deep-dive
source_anchor: ""
source_lines: [1, 160]
section: "MiniMax"
---
TLDRSpace Bunny was MiniMax M3.1 Flash. $0.15/$0.60 pricing, 1M context, vendor vs. independent benchmarks, and the open questions before you switch.

## What MiniMax M3.1 Flash at $0.15/$0.60 Means for Teams Running Agentic Coding Workloads

MiniMax M3.1 Flash shipped on October 19, 2026. There was no keynote, no model card, and no countdown. It first appeared as a new model ID in MiniMax's official API list, spotted by @aitrackerbot at 04:36 UTC, hours before the lab said anything. By that afternoon, MiniMax had confirmed that "Space Bunny," the anonymous model that topped two developer leaderboards in late September, was an early build of this model. About 48 hours later, independent evaluators have posted numbers and the community has posted complaints. The open-weight release is still pending.

**TLDR** MiniMax M3.1 Flash is a 1M-context reasoning model that takes text and images. It costs $0.15 per 1M input tokens and $0.60 per 1M output tokens. MiniMax reports 62.2% on Automation-Bench and 73.7% on DeepSWE. Independent testing puts it at 42–43 on the Artificial Analysis Intelligence Index, close to DeepSeek V4.1 Flash 1017 but a few points behind, and flags a high hallucination tendency. Weights are reportedly due October 23.

**Updated 2026-10-23:** MiniMax has released open weights for M3.1 Flash on Hugging Face and ModelScope, confirming its 428B/23B MoE architecture (see the Update below).


## Key Takeaways

- **Space Bunny was M3.1 Flash.** MiniMax says the stealth build processed more than 200T tokens before the reveal.
- **Thinking cannot be switched off.** A five-tier Effort Ladder defaults to`max` , and requests that disable thinking reportedly fail with a 400 error.
- **Cost per task is the real story.** It averages $0.22 per Artificial Analysis index task, down from roughly $0.51 for MiniMax M3.
- **Vendor and independent numbers diverge.** MiniMax claims 200+ tokens/s, and Artificial Analysis measured about 105 tokens/s.
- **Legacy traffic moves automatically.** M2.5 and M2.7 requests on MiniMax plans now route to M3.1 Flash.
- **Community sentiment is split.** Some developers report "benchmaxxing" and context drift, and others prefer it to rival Flash models.

## What Actually Shipped

The confirmed surface area is fairly narrow. MiniMax released M3.1 Flash through the MiniMax API, the M Plan subscription, and MiniMax Code, according to 智东西China AI News' summary of the launch. Techmeme quoted MiniMax calling it "a generational leap" in agentic tasks, coding, computer use, and cybersecurity, with weights due October 23.

| Item | Value | Source | 
|---|---|---|
| Input price | $0.15 / 1M tokens | MiniMax announcement, Artificial Analysis | 
| Output price | $0.60 / 1M tokens | MiniMax announcement, Artificial Analysis | 
| Cached input | $0.015 / 1M tokens (90% discount) | MiniMax announcement, Artificial Analysis | 
| Context window | 1M tokens | MiniMax docs, Artificial Analysis | 
| Max output | 524K tokens | Independent eval-index listing | 
| Input modalities | Text, image (docs also list video) | MiniMax docs | 
| Output modality | Text only | Artificial Analysis | 

One early post listed $0.165/$0.66 with cache at $0.0165. That is exactly 10% above the figures every other source reports. Treat $0.15/$0.60 as the list price and the higher numbers as a likely regional or tax-inclusive variant, which is unverified.

Two operational details matter more than the headline price. The first is **Legacy Auto-Routing**. Starting October 19, MiniMax's documentation moved M2.5 and M2.7 to Legacy, and all requests to those models under the M Plan and Token Plan now route automatically to M3.1 Flash. Legacy Auto-Routing is MiniMax's server-side redirection of requests for older model IDs to M3.1 Flash, with no client-side change required. Teams that pinned M2.7 for reproducibility on those plans are no longer running M2.7.

The second detail is media limits. @mylifcc reports from the docs that the model accepts images up to 10MB and video up to 512MB, and that it does not generate images.

## The Space Bunny Stealth Run, by the Numbers

The release is partly a reveal of a codename. Space Bunny was the anonymous model MiniMax ran in public before attaching its name. This Stealth Leaderboard Run gave MiniMax weeks of real-world traffic before launch.

The reported numbers do not quite add up, which is typical for this kind of reveal:

- **OpenCode:** #1 for 12 consecutive days, 106T tokens served, per OpenCode's own account.
- **A third-party routing marketplace:** #1 for 10 straight days and 56.7T tokens processed before Space Bunny was pulled on October 5, per thehype. Kilo puts the figure at "over 50 trillion".
- **MiniMax's total:** "over 200T tokens," as relayed in the launch summaries.

Together, 106T and 56.7T make about 163T. The 200T claim therefore implies other surfaces, such as the M3.1 Flash Preview that reportedly launched on September 27, or a different counting method. Nobody has reconciled these figures publicly.

*Source: @opencode*

MiniMax's own framing deserves attention too. The company told developers that Space Bunny "was only the early build" and invited them to "see what changed." That caveat cuts both ways. It may explain the negative Space Bunny reports below. It also means none of the 200T tokens of usage directly validates the final checkpoint.

## The Effort Ladder and Always-On Thinking

The most consequential API change is how the model handles reasoning.

The Effort Ladder is MiniMax's five-tier reasoning control (`low`, `medium`, `high`, `xhigh`, `max`) that defaults to `max` when a request leaves it unset.

Always-On Thinking is the rule that M3.1 Flash cannot run without reasoning. According to @mylifcc's read of the docs, passing `thinking.type="disabled"` returns HTTP 400 with error 2013. This reverses MiniMax M3, which reportedly had thinking off by default.

*Source: @mylifcc*

The practical effect is that every request pays for reasoning tokens. If your client never sets effort, every request also pays the maximum. This affects any comparison of list prices. Artificial Analysis rates the model "very verbose", at 180M output tokens across its Intelligence Index against a median of 100M. VentureBeat reports roughly 189M, compared with about 324M for GLM-5.3 Flash and 334M for Gemini 3.8 Flash. Both readings hold: the model is wordy against the whole field but relatively lean against its closest peers.

## What Independent Testing Shows

MiniMax's published benchmarks and the independent numbers measure different things, so they belong in separate columns.

| Benchmark | M3.1 Flash | Comparison | Source | 
|---|---|---|---|
| Automation-Bench | 62.2% | DeepSeek V4.1 Flash 60.5%, Opus 5 50.3% | MiniMax | 
| DeepSWE | 73.7% | Opus 5 74.0% | MiniMax | 
| OSWorld | 61.7% | Opus 5 70.6% | MiniMax | 
| BabyVision | 91.0% | n/a | MiniMax | 
| AA Intelligence Index | 42–43 | M3: 29 | Artificial Analysis | 
| Humanity's Last Exam | 48% | M3: 39% | Artificial Analysis via VentureBeat | 
| CritPt (physics) | 22% | M3: 4% | Artificial Analysis via VentureBeat | 
| Vibe Code Bench v1.1 | 73.94% | M3: 47.57% | Independent eval index | 
| Terminal-Bench 4.0 | 25.25% | n/a | Independent eval index | 

The vendor numbers come from the Chinese-language launch summary and TestingCatalog's chart. One daily digest noticed that MiniMax's table prints 62.2 on Automation-Bench while its cost chart prints 62.8% at max effort. The gap is small, but it shows these are self-reported figures.

*Source: @testingcatalog*

The independent picture is mixed. The model scored 53.78% on the Vals Index, ranked #15 of 23, at max effort. The same evaluation called it "weaker on long-horizon agentic work": 25.25% on Terminal-Bench 4.0 and 7.50% on HLAB-Vals-1.1. Its average latency per test was 43 minutes 18 seconds.

The key caveat comes from Artificial Analysis, via VentureBeat. M3.1 Flash answered more than half of the factual-knowledge questions correctly, but showed "an exceptionally high tendency to generate incorrect answers rather than acknowledge uncertainty." For agents that act without supervision, that matters more than any coding score.

## MiniMax M3.1 Flash vs DeepSeek V4.1 Flash: What the Reports Show

DeepSeek V4.1 Flash 1017 is the competitor named most often in the posts reviewed, and it shipped days earlier.

| Dimension | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 | 
|---|---|---|
| AA Intelligence Index | 42–43 | 45 | 
| Cost per AA index task | $0.22 | $0.26 | 
| Measured output speed (AA) | ~105 tokens/s | ~209 tokens/s | 
| Automation-Bench (MiniMax-reported) | 62.2% | 60.5% | 
| Context window | 1M | unverified — no public number in available sources | 

At $0.22 per Artificial Analysis Intelligence Index task versus $0.26 for DeepSeek V4.1 Flash 1017, MiniMax M3.1 Flash is about 15% cheaper per task but trails by 2–3 index points. Its measured output speed is roughly half of DeepSeek's. That gap matters for the "3× faster" claim, which compares against MiniMax M3, not against rivals.

Community comparisons lean toward DeepSeek. In r/LocalLLaMA, one tester who used Space Bunny heavily said it is "fast, but it's not as fast or as capable as DS4.1 flash" on coding. Another noted that MiniMax's cache pricing is higher than DeepSeek's. On the NVIDIA DGX Spark forum, local-inference users are already framing the question as how it will "perform against DSv4.1F 1017."

There is one non-technical factor. The same news cycle included a U.S. DHS order removing DeepSeek products from federal civilian agencies. MiniMax is pitching a US-hosted, zero-data-retention option via Ollama. For some procurement teams, that could matter more than three index points.

## The Benchmark-to-Vibe Gap

The Benchmark-to-Vibe Gap is the distance between a model's published scores and what developers report in daily use. It is unusually wide here.

The r/opencode benchmark thread lists 62% on Automation-Bench, 30% on ALE, 74% on DeepSWE, and 91% on BabyVision. The replies are mostly hostile. Commenters call it "BenchMAXXED." One user says Space Bunny "broke a whole project deleting the main files." Another says it "doesn't really hold context really well" and ignores plan-only instructions. There is support too: one subscriber to both MiniMax and GLM plans says "minimax flash does better than glm flash" on their workloads.

Most of these reports describe Space Bunny, which MiniMax calls an early build. Early hands-on posts about the final model describe it as "super fast" but include no measurements. The gap may shrink, or it may not. No structured test of the final checkpoint has been published yet.

## What We Know vs. What We Don't

**What we know:**

- M3.1 Flash went GA on October 19, 2026, on the MiniMax API, M Plan, and MiniMax Code, and left preview the same day.
- Pricing is $0.15 input, $0.60 output, and $0.015 cached input per 1M tokens.
- Context is 1M tokens, with text and image input (docs also list video) and text-only output.
- The Effort Ladder defaults to `max` , and disabling thinking reportedly returns error 2013 (@mylifcc).
- MiniMax confirmed that Space Bunny, #1 on OpenCode for 12 days, was an early build.
- M2.5 and M2.7 requests on the M Plan and Token Plan now route to M3.1 Flash.
- Artificial Analysis scores it 42–43 at $0.22 per task, compared with 29 for M3.

**What we don't know:**

- Whether weights land on October 23 as Techmeme and TestingCatalog report, and under what license. One digest says the research post gives no date.
- The final parameter count. Commenters cite a MiniMax guide describing the *preview* as a 428B-total, ~23B-active MoE with sparse attention, which is unverified for the final model.
- Which throughput figure holds: MiniMax's 200+ tokens/s (P50 170+) or Artificial Analysis' ~105 tokens/s.
- How the hallucination tendency shows up in real agent loops.
- When the Pro model TestingCatalog mentions will ship, and what it will cost.
- Whether the "70% lower agentic cost" claim holds for your workload. One digest notes that the list change is a 50% input cut versus M3 on inputs up to 512K.

On the limited evidence so far, MiniMax M3.1 Flash appears to be a strong price-per-task option for structured business automation, but its hallucination behavior and long-horizon terminal scores have not been tested enough to trust it unsupervised.

## What Builders Should Do Today

1. **Audit Legacy Auto-Routing.** If you call M2.5 or M2.7 through the M Plan or Token Plan, your outputs changed on October 19. Re-run regression suites now.
2. **Set effort explicitly.** The`max` default will inflate both latency and output tokens. Benchmark`medium` and`high` against your own tasks before accepting the default.
3. **Measure abstention, not just accuracy.** Given the hallucination finding, add tests where the correct answer is "I don't know." Run the same harness on a comparable Flash-tier model, such as DeepSeek-V4.1-Flash, to see whether the gap appears on your data.

The free windows help with this: 72 hours on the routing marketplace, 3 days on OpenCode, a free period in Cline, and reportedly a week inside MiniMax plans. Use them to run evals, and keep production on a model you have already tested.

## The Week Ahead

Three signals will decide whether the numbers hold. Watch for the October 23 weight drop and its license, because that will confirm or replace the 428B/23B preview figures. Run your own throughput test to see whether you get closer to 105 or 200 tokens per second. Pin your model ID and effort level, then compare against DeepSeek V4.1 Flash 1017 once local quantizations of both are available.

## Update — 2026-10-23

On October 23, 2026, MiniMax officially released the open weights for M3.1 Flash on Hugging Face and ModelScope, resolving the unconfirmed preview architecture. MiniMax confirmed a Mixture-of-Experts design totaling 428B parameters with 23B activated per token, released under a "MiniMax Community License." The release includes sparse attention implementations, quantized training checkpoints, and DSpark speculative decoding recipes optimized for both vLLM and SGLang runtimes.

Ecosystem hosting and local serving support rolled out concurrently. Together AI made the open weights available as a managed serverless endpoint alongside day-zero API integrations from Runware and Inferact. On the infrastructure side, ten chip vendors announced day-one compatibility, while initial community deployment reports verified that the model can be served locally on a 4x NVIDIA DGX Spark setup using tensor parallelism (TP4) and vLLM.

Building similar agentic coding pipelines? On kie.ai you can try Gemini 3.8 Flash, Claude Haiku 5.5, and Kimi K3.1 Pro.
