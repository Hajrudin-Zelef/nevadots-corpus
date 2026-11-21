---
id: collect-261121-veille-ia/qwen/qwen3-8-27b-complete-guide-benchmarks-vram-vs-claude
title: "Qwen3.8-27B Complete Guide: Benchmarks, VRAM, vs Claude"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "default", "qwen", "xhigh", "benchmarks", "claude", "model", "august"]
source: https://codersera.com/blog/qwen-3-8-27b-complete-guide-2026/
source_anchor: ""
source_lines: [1, 132]
section: "Qwen"
---
**Quick answer.** Qwen3.8-27B is a dense 27B open-weights vision-language model released August 14, 2026 under Apache-2.0. At its default `xhigh` reasoning effort it scores 33.70 on Artificial Analysis' Intelligence Index v4.3.2 — up 12.3 points over Qwen3.6-27B on an identical architecture — fits in an 18 GB Ollama download, and is AA's top-ranked open-weights model under 40B.

Alibaba's Qwen team shipped three releases in twelve days this August: Qwen3.8-Max went GA on the 3rd, the 2.4-trillion-parameter open flagship landed on the 12th, and on **August 14 came the one most people can actually run — Qwen3.8-27B**, a dense 27B vision-language model under Apache-2.0. Within four days the Unsloth GGUF quants alone passed 2.7 million downloads, Ollama logged 236K pulls, and the release thread hit 1,423 points on Hacker News. This guide covers what shipped, which benchmark numbers deserve your trust (fewer than the headlines suggest), the overthinking problem and its fix, and where the 27B fits against its giant sibling.

## What shipped on August 14?

The specs, in one pass:

- **Dense 27B parameters** — not a mixture-of-experts. One ~17 GB file at Q4 quantization.
- **Apache-2.0 licence** — genuinely free for commercial use, unlike the revenue-gated custom licence on the open 2.4T flagship.
- **Native vision-language** — image and video input, including hour-scale video. The far larger open 2.4T model, oddly, is text-only.
- **262,144-token native context** , extensible to 1,000,000 with YaRN rope scaling — and that extension is real rather than aspirational: Alibaba, Novita and Phala all serve the 27B at a 1M window on OpenRouter today.
- **Thinking on by default** , with a`reasoning_effort` control (xhigh default / medium / low). The default is a problem — more below.

The most interesting spec is the one that didn't change. Diff the `config.json` against Qwen3.6-27B and **the architecture is identical**: same 64 layers, same hidden size, same hybrid Gated DeltaNet + attention layout, same 262K max positions. Every point of improvement came from post-training — RL environments plus on-policy distillation. That's why llama.cpp supported it on day one. It's also what makes the headline result striking: on Artificial Analysis' independent Intelligence Index v4.3.2 (October 2026), Qwen3.6-27B (Reasoning) scores 21.43 and Qwen3.8-27B at its default xhigh effort scores **33.70 — a 12.3-point jump from post-training alone**, at identical parameter count. We break down exactly what changed (and whether 3.6 owners should bother upgrading) in our Qwen 3.8 vs Qwen 3.6 comparison.

## The benchmarks — and who ran them

Model-card numbers first, against Qwen's own comparator columns. Bold marks the winner per row.

| Benchmark | Qwen3.8-27B | Qwen3.6-27B | Claude Opus 4.6 Max | 
|---|---|---|---|
| Terminal-Bench 2.1 (Terminus) | 73.0 | 63.4 | **78.2** | 
| SWE-bench Pro* | **61.7** | 53.5 | 53.4 | 
| NL2Repo-Bench | 42.3 | 36.2 | **47.6** | 
| DeepSWE 1.1 | **42.2** | 13.3 | — | 
| LiveCodeBench v6 | **90.3** | — | 88.8 | 
| GPQA-Diamond | 89.2 | — | **91.3** | 
| Humanity's Last Exam | 30.8 | — | **40.0** | 
| OSWorld-Verified (vision) | **84.3** | — | 72.7 | 
| AndroidWorld (vision) | **81.9** | — | 62.0 | 
| AA Intelligence Index v4.3.2 (independent) | **33.70** (Xhigh) | 21.43 (Reasoning) | 31.95 (Max) | 

** Qwen's "refined" variant of SWE-bench Pro — see the trust audit below. The card also reports IFBench 79.5, WebArena-Verified 64.8, and SWE-MM 38.6 (vs Opus 27.1).*

### How much should you trust each number?

Our standard practice with launch benchmarks: grade every number by who ran it before you repeat it.

- **Independent — trust it.** The Artificial Analysis Intelligence Index score of**33.70** — v4.3.2, ten evals, run by AA on the`xhigh` default rather than by Qwen — is the anchor. It puts the 27B level with GLM-5.2 (Max) at 33.71, within a point of DeepSeek V4 Flash 0731 (Max) at 34.33 — a 284B MoE — and just past Claude Opus 4.6 (Max) at 31.95, the comparator Qwen's own card picked. AA ranks it**#1 of 142** open-weights models in the 4B–40B size class.
- **Self-run on public benchmarks — plausible, verify.** Terminal-Bench, NL2Repo-Bench, LiveCodeBench, GPQA-Diamond, and HLE are Qwen-run but on standard public evals, and the card is honest enough to print its own losses: Opus 4.6 Max wins Terminal-Bench, NL2Repo, GPQA, and HLE outright.
- **Self-run on a modified benchmark — heavy discount.** The headline SWE-bench Pro 61.7 came from a version where Qwen "corrected problematic tasks" and re-ran baselines on the refined set — while the Opus 53.4 in the same row is the officially reported score on the original benchmark. Apples to oranges; this is the weakest provenance link in the table.
- **In-house — discount entirely.** QwenSWEBench 79.0 and CoWorkBench 70.7 are benchmarks Qwen built itself. No comparator lab has run them.
- **Debunked — don't repeat it.** The viral claim that the 27B "beats Opus 4.7 on DeepSWE, 42.2 vs 40" has no basis in the model card: the Opus cell for DeepSWE 1.1 is literally a dash. Someone invented the 40.

**One thing to know before you compare that number to anything.** AA rebased the Intelligence Index from v4.1.1 to v4.3.2 in October 2026, moving from nine evals to ten. On a rebase every model's score shifts because the benchmark basket changed, not because the models did, and AA publishes no conversion factor. The **52** quoted everywhere at launch — including in earlier versions of this guide — is a v4.1.1 figure and does not translate onto the v4.3.2 scale. Compare v4.3.2 scores only against other v4.3.2 scores.

Two independent caveats ride along with the 33.70. First, **cost**: AA puts the 27B at **$1.01 per Intelligence Index task**, which is expensive for the score it buys — Claude Opus 4.6 (Max) reaches 31.95 at $0.06 per task, and DeepSeek V4 Flash 0731 (Max) reaches 34.33 at $0.22. Back those per-task costs out against each model's token prices and the 27B is spending roughly **2× the tokens per task** of Qwen3.6-27B and about 2.2× those of GPT-5.6 Luna (Max): the intelligence is bought with much longer reasoning traces, still the under-reported number in this launch. Run it locally and that is a wall-clock cost; rent it and it is a billing one. Second, **knowledge**: the post-training traded world knowledge for agentic skill. AA now scores knowledge reliability inside the Index itself as AA-Omniscience, one of the ten v4.3.2 evals, and the 27B's strength there is non-hallucination rather than recall. It knows what it doesn't know better than it knows.

Cost per Index task is the comparison that holds up. AA runs a different number of tasks per model, so whole-suite totals are not comparable between rows — per-task figures are. All v4.3.2:

| Model (variant) | Index v4.3.2 | USD per Index task | 
|---|---|---|
| Claude Opus 4.6 (Max) | 31.95 | **$0.06** | 
| GPT-5.6 Luna (Max) | 37.32 | $0.18 | 
| DeepSeek V4 Flash 0731 (Max) | 34.33 | $0.22 | 
| **Qwen3.8 27B (Xhigh)** | **33.70** | $1.01 | 
| Claude Opus 4.7 (Max) | 40.69 | $1.34 | 
| GLM-5.2 (Max) | 33.71 | $1.47 | 

As a single absolute figure, AA's whole-suite cost for the 27B at xhigh is **$1,335.92**. And the reasoning dial moves the score a long way — 33.70 at xhigh, 27.55 at medium, 26.20 at low, 20.15 with thinking off — so any 27B benchmark number you read is only meaningful next to the effort setting that produced it.

Honest card summary: the 27B beats Opus 4.6 Max on 5 of 12 text rows and 4 of 6 vision rows, and loses Terminal-Bench, NL2Repo, GPQA-Diamond, and HLE. That is a genuinely remarkable result for a 17 GB download. It is not "Opus is obsolete".

## Is it really "Opus at home"?

Reception split into two camps within hours of release.

**The euphoria camp** has real datapoints. Simon Willison called it the best local model he's tested — "a 17GB file… is a miracle" — and successfully drove his Pi coding agent with it. One production team reported deploying it in NVFP4 on vLLM and seeing **99% agent-task completion in production**. A Reddit user with an RTX 5090 summarized the mood: "I don't need Claude for 80%+ of my work anymore."

**The skeptics** point at Qwen's benchmark-first reputation. One much-upvoted HN reply: "They do not beat opus on real-world usage… Number goes up on a benchmark isn't it." Others noted that Qwen releases have historically scored better on launch tables than in month-two daily use.

The capability *shape* reconciles the two camps. Qwen3.8-27B is an **agentic executor**: post-trained hard for tool use, terminal work, and multi-step coding tasks inside a harness, at the cost of world knowledge and creative writing, where it's noticeably weaker than frontier hosted models. If your day is well-specified coding tasks in Claude Code, OpenCode, or Cursor, a large fraction of it really can move local. If you need broad knowledge, judgment on ambiguous problems, or writing voice — it isn't Claude. We put that question through a full hands-on in Qwen3.8-27B as a local Claude Code replacement.

## Why does it think so long — and how do you fix it?

The single biggest day-one complaint. Willison's verdict: "Qwen 3.8 27B is excellent, but it defaults to wildly overthinking things." His numbers: the pelican SVG test at the default xhigh effort took **21 minutes and 22,276 reasoning tokens**. Asked to "draw an svg of a circle", it produced an unrequested animated "geometric study". A corroborating M4 Pro run clocked an "svg owl" prompt at 17m12s — the same prompt on Qwen3.6-35B-A3B finished in 1.59 seconds.

The mechanics matter here: the `reasoning_effort` levels are not architectural switches. They're one-sentence system-prompt additions the model was post-trained to respond to. `xhigh` — the default — injects "Reasoning effort is set to xhigh. Please think carefully…". `medium` injects nothing at all; it's the model's native behaviour.

**The fix:** set `reasoning_effort` to `medium` or `low`, optionally with a reasoning budget around 5,000 tokens. Willison's own advice: "Run Qwen 3.8 27B on low or even no reasoning levels at first." Keep thinking enabled for genuinely complex tool-building — his bounding-box vision tool worked with thinking on and broke with it off — but xhigh as an everyday default mostly burns your GPU time. One trap to avoid: LM Studio's default 8,192-token context gets consumed entirely by the thinking phase. Raise it.

## How do you run Qwen3.8-27B locally?

The short version — the full walkthrough with speculative decoding, 16GB configs, and known bugs lives in our dedicated guide: how to run Qwen 3.8 locally.

`ollama run qwen3.8`
That pulls the 27B at q4_K_M — an 18 GB download with 256K context and image input. Ollama also ships harness launchers now: `ollama launch claude --model qwen3.8` drops it straight into Claude Code.

- **Quant sizes (Unsloth GGUF):** Q4_K_M 17.1 GB is the sweet spot for 24 GB cards; UD-Q3_K_XL at 13.4 GB fits 16 GB cards; Q8_0 is 29.1 GB. Add ~0.9 GB for the vision projector, plus KV cache.
- **Speeds:** RTX 4090 at Q4 runs ~48 t/s, rising to 60–80 t/s with MTP speculative decoding. Apple M5 Max / DGX Spark land at 15–30 t/s in LM Studio.
- **The dense-model caveat:** a dense 27B is memory-bandwidth-bound. On a ~100 GB/s-bandwidth laptop you'll see ~4 t/s, and CPU-only is ~1 t/s — the MoE Qwen3.6-35B-A3B is roughly 5× faster on CPU. "The 17 GB file fits" is not the same as "it's usable" on low-bandwidth hardware.

## Where does the 27B fit next to Qwen3.8-Max and the 2.4T?

The Qwen3.8 family has grown to nine callable SKUs since August — we map every one of them in the Qwen 3.8 model lineup — but three matter for this comparison: **Max** (hosted, with vision, 1M context, and built-in tools), the **open 2.4T-A95B** (the weights behind Max, released August 12), and the **27B**. Two contrasts matter for choosing:

**Licence.** The 2.4T ships under the custom "Qwen3.8-Max License" — a modified MIT that's free for most commercial use but adds revenue-gated carve-outs: products over 100M MAU or $20M monthly revenue must display the model name in their UI, and model-as-a-service or "AI work assistant" businesses past $50M in a year need a separate licence. The 27B is plain **Apache-2.0**. For anyone building a product on open weights, that's the difference between "read the licence with a lawyer" and "ship it".

**Modality — inverted.** The 27B has native image and video input. The 2.4T, ninety times its size, is text-only in its open form. If your workload involves screenshots, UI automation, or video, the small model is the only open Qwen3.8 that can see.

If you'd rather not host anything, Apache-2.0 means plenty of people will serve it for you: **17 providers** list the 27B on OpenRouter, from $0.024/M input at the cheapest to $0.99/M at Cerebras, with Alibaba's own endpoint at $0.425/M input and $2.55/M output; AA's reference price is $0.50/$3.00. Most cap context at 262K — Alibaba, Novita and Phala serve the full 1M. Check it against the per-task table above before assuming cheap tokens mean a cheap job. For the flagship's full story — pricing, the near-parity between open 2.4T and hosted Max, and the harness caveats on its coding numbers — see the Qwen3.8-Max complete guide.

## What's next for the Qwen3.8 family?

Two things worth watching. A **Qwen3.8-35B-A3B MoE** was spotted in a modelscope/ms-swift commit (ab726e9) on August 15, and it is still the community's loudest ask — an A3B MoE would run fast on the CPUs and Macs where the dense 27B crawls. Seven weeks on, it has not shipped: there is no `Qwen/Qwen3.8-35B-A3B` under the Qwen organisation on Hugging Face, and the 3.8 ladder runs 27B dense → ~180B → 2.4T with nothing in between. What *did* arrive is **Qwen3.8-Flash-Next**, which AA scores at **39.82 on Index v4.3.2** — essentially level with the open 2.4T-A95B's 39.89 — at $0.15/M input.

## FAQ

### Is Qwen3.8-27B really better than Claude?

On Artificial Analysis' independent Intelligence Index v4.3.2 it scores 33.70 at xhigh, just past Claude Opus 4.6 (Max) at 31.95 — though Claude's current flagship, Opus 4.7 (Max), sits at 40.69, about seven points clear. On some agentic coding and computer-use benchmarks it beats Opus 4.6 Max, the model card's comparator — 5 of 12 text rows and 4 of 6 vision rows, mostly self-run. It loses Terminal-Bench, GPQA, and HLE, and independent testing shows a world-knowledge regression. Treat it as a remarkable local coding agent, not a frontier replacement.

### Can I use Qwen3.8-27B commercially?

Yes. It's Apache-2.0 — free for commercial use, modification, fine-tuning, and redistribution with no revenue gates. That's unlike the open 2.4T flagship, whose custom licence adds display requirements and licensing thresholds for very large businesses.

### How much VRAM does Qwen3.8-27B need?

About 17 GB at Q4_K_M, so a 24 GB card runs it comfortably with context to spare. 16 GB cards work with the 13.4 GB UD-Q3_K_XL quant, and Q8_0 needs 29.1 GB. Add ~0.9 GB for the vision projector and headroom for KV cache.

### Does Qwen3.8-27B have vision?

Yes — native image and video input, including hour-scale video. It posts strong (self-run) scores on OSWorld-Verified (84.3) and AndroidWorld (81.9). Unusually, the much larger open 2.4T model is text-only.

### What does Qwen3.8-27B score on Artificial Analysis?

33.70 on the Intelligence Index v4.3.2 (October 2026) at its default `xhigh` reasoning effort — which is #1 of 142 open-weights models in AA's 4B–40B size class. The lower effort tiers score 27.55 (medium), 26.20 (low) and 20.15 with thinking off. Scores around 52 are v4.1.1-era figures from before AA's October rebase and do not convert to v4.3.2.

### Why does Qwen3.8-27B think for so long?

Thinking is on by default at `reasoning_effort: xhigh`, which injects an instruction to think carefully — Simon Willison measured 21 minutes and 22,276 reasoning tokens for one SVG drawing. Set the effort to medium or low for everyday use; medium is the model's native default with nothing injected.
