---
id: collect-261121-veille-ia/deepseek/deepseek-v4-what-s-inside-how-it-compares-and-where-it-actually-wins
title: "DeepSeek V4: What's Inside, How It Compares, and Where It Actually Wins"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "gpt-5", "opus", "model", "v4-pro", "actually", "wins", "tencent"]
source: https://alexlavaee.me/blog/deepseek-v4-architecture-benchmarks-engineer-verdict/
source_anchor: ""
source_lines: [1, 103]
section: "DeepSeek"
---
DeepSeek V4 shipped on April 24, 2026 — four days after Moonshot’s Kimi K2.6, one day after OpenAI’s GPT-5.5[1]. Two MIT-licensed models, both 1M-context: **V4-Pro** at 1.6T total / 49B active, and **V4-Flash** at 284B / 13B active.

The headline number is the price: **$3.48 per million output tokens for V4-Pro vs $25 for Claude Opus 4.7 and $30 for GPT-5.5**[5]. (DeepSeek is also running a launch promo at 75% off — $0.87/M output — through May 5, 2026, which widens the gap further during the evaluation window.) That’s a 7-9x gap at the standard rate, against a model that’s within ~5-7 points of the closed frontier on most coding benchmarks. That gap is large enough to make many teams reconsider their model routing decisions.

But price isn’t the complete picture. V4 performs well on some workloads and poorly on others, and integration is more difficult than the marketing suggests. Here’s the assessment, engineer reports, and what’s new under the hood.

## Where V4 actually wins (and where it doesn’t)

Three frontier-class models shipped in nine days, and **no single model dominates**. The ranking flips depending on the workload:

- **Real-world software engineering (PRs, refactors, multi-repo bug fixes):** Opus 4.7 leads on independent evaluations that require reasoning across many files — Vals AI’s Vibe Code Benchmark[1], the Aider Polyglot suite, and contamination-resistant tests like LiveCodeBench. It’s the right pick when changes are multi-file and planning is the difficult part. You can run Opus end-to-end (plan + edits), or split the workflow: Opus writes the plan, GPT-5.5 on low or medium reasoning executes the file edits against it. The split is often the better cost-quality tradeoff.
- **Terminal / agentic shell:** GPT-5.5 leads at 82.7% on Terminal-Bench 2.0, ~15 points ahead of V4-Pro. These workloads involve many small tool calls and shell-output error recovery, and V4 hasn’t been RL-trained on them at the same depth.
- **Long-horizon autonomous execution (12+ hour runs):** Kimi K2.6 is the open-source choice, with its Claw Groups multi-agent coordination and demonstrated runs across 4,000+ tool calls[7].
- **Whole-repo reasoning (hundreds of files, >200K tokens):** V4-Pro’s 1M context is the only frontier option that’s economical to use at full length — its architecture cuts inference cost to roughly a quarter of V3.2’s at 1M context[2]. More on why below. The natural fit is the discovery phase of a task: load the whole repo and use V4-Pro for deep research, search, and understanding how a codebase fits together — the analysis pass that feeds into a plan, which you then hand to Opus or GPT-5.5 to execute.
- **Cost-per-task at scale:** V4-Flash at $0.28/M output is 90-107x cheaper than the closed frontier. Tencent Hy3-preview at ~$0.55/M is in a similar range. For batch and overnight workloads, neither closed model is competitive.

One additional entry worth noting: **Tencent Hy3-preview** is not competing for the largest open coding model. It’s a 21B-active model optimized for cost-per-step in real product traffic, with stable agent runs of up to 495 steps in production powering CodeBuddy and WorkBuddy[11]. If you’re building product-embedded agents on tight budgets rather than optimizing for benchmark scores, it’s worth evaluating. Tencent is direct about the tradeoffs: the release notes describe *“weak error recovery capabilities when calling the tool and sensitivity to inference hyperparameters.”*

The benchmarks:

| Benchmark | DeepSeek V4-Pro | Claude Opus 4.7 | GPT-5.5 | Kimi K2.6 | Qwen3.6-27B | Tencent Hy3-preview | 
|---|---|---|---|---|---|---|
| Total / Active params | 1.6T / 49B | undisclosed | undisclosed | 1T / 32B | 27B dense | 295B / 21B | 
| Context window | **1M** | **1M** | **1M** | 256K | 256K (1M YaRN) | 256K | 
| SWE-Bench Verified | 80.6% | **87.6%** | — | 80.2% | 77.2% | 74.4% | 
| SWE-Bench Pro | 55.4% | **64.3%** | 58.6% | 58.6% | 53.5% | — | 
| Terminal-Bench 2.0 | 67.9% | 69.4% | **82.7%** | 66.7% | 59.3% | 54.4% | 
| LiveCodeBench | **93.5%** | 84.69% | 85.30% | 89.6% | — | — | 
| BrowseComp | 83.4% | 79.3% | **84.4%** | 83.2% | — | 67.1% | 
| Output price ($/M tokens) | $3.48 | $25 | $30 | ~$2.50 | ~$1.56 | **~$0.55** | 
| License | **MIT (open weights)** | Closed API | Closed API | **Modified MIT** | **Apache 2.0** | **Open weights** | 

Sources: DeepSeek[1], VentureBeat[5], BenchLM[6], AkitaOnRails[3], Latent Space[7], Tencent[11], Qwen[14].

## What engineers actually report

The more useful signal comes from independent reviewers running real tasks, and the picture from the first 72 hours is mixed in informative ways.

On the positive side, AkitaOnRails ran his RubyLLM benchmark — the same chat-app-against-a-specific-Ruby-library task he’s been using to track open-source coding models — and observed V4 move from hallucinating API methods in V3.2 to writing code that compiled and ran on the first try, with Pro producing essentially reference-quality output[3]. Vals AI observed the same pattern on their Vibe Code Benchmark, where V4 improved roughly 10x from V3.2 and now leads open-source[1]. DeepSeek’s own team, in the release notes, is measured about positioning: V4 is their internal default now, better than Sonnet 4.5 and close to Opus 4.6 in non-thinking mode — but they explicitly stop short of claiming parity with Opus 4.7[1]. The vendor’s own framing is more accurate than much of the launch coverage.

The negative reports cluster on integration. AkitaOnRails couldn’t run V4-Pro through OpenCode at all — it kept failing on the thinking-mode handshake — and his broader assessment of DeepSeek launches reflects a consistent pattern: marketing ships earlier than working tool support, the community spends a few weeks reverse-engineering the protocol, and gaps in open-source harnesses tend to persist[3]. Cursor’s forum is showing similar issues, with open threads reporting V4’s context capped at 200K with reasoning_content errors after tool calls[12] and an open feature request for proper reasoning_content compatibility[13]. Local-inference users are also waiting — no community GGUF at launch, llama.cpp support days out, MLX on Apple Silicon trailing by a similar margin. vLLM works on the native FP4/FP8 checkpoints out of the box, but the hardware floor is one H200 141GB or two A100 80GBs for Flash, and four A100s or two H200s to use the full 1M context[4].

A useful counterpoint came from Chew Loong Nian, who tested all four V4 tiers across 20 real tasks instead of leaderboard prompts[4]. V4-Pro-Max didn’t dominate. **Flash won 7 outright at $0.14 per million input tokens**, mostly on shorter tasks where the price-quality tradeoff favored it. Pro-Max only pulled clearly ahead when the workload genuinely required it: on three long-context retrieval tasks loading 800K tokens of a real GitHub repo and asking for a function’s call graph, Pro hit 3/3 while Flash hit 1/3. That suggests the right approach — V4 is two models with different optimization points, and Pro earns its premium when context is large.

The practical takeaway: budget for integration work, not just inference. The thinking-mode protocol is non-trivial, OpenCode and Claude Code adapters aren’t all working cleanly at launch, and you’ll likely maintain your own patches for several weeks. Run V4 in shadow before deploying it to customers.

## Why V4 performs well where it does

Two design choices explain most of V4’s profile.

**It only activates 49B of its 1.6T parameters per token.** That’s the mixture-of-experts approach — only the experts relevant to the current token activate. Combined with running natively in 4-bit weights at inference (real FP4, not simulated quantization), this is how a 1.6T model fits within deployable economics[2]. It’s also why V4-Flash exists at 13B active: the same approach scaled further down. The cost gap to closed models comes from MoE plus FP4 plus training-efficiency improvements.

**It doesn’t process the full million-token context.** Instead, V4 summarizes long context into compressed blocks and learns which blocks to attend to for a given query. The result is concrete: at 1M context, V4-Pro uses 27% of V3.2’s compute and 10% of its memory[2]. That’s what makes 1M context economically viable to serve, and why whole-repo reasoning is V4’s primary workload.

The tradeoff: **the same compression is why V4 underperforms on terminal/agentic shell tasks.** Those workloads are short-context, high-frequency tool calls — there’s no million tokens to summarize, and the architectural advantage disappears. V4’s weakness there isn’t due to lack of effort; GPT-5.5 has been RL-trained on shell sessions much more heavily, and at short context that’s what matters most.

The technical report has more — including novel work on residual connections that other labs will likely adopt within two release cycles — but for routing decisions, the three points above are the most important.

## What to watch next

Three things will determine whether V4 becomes a production default, or remains a release that performs well on benchmarks but is difficult to integrate, like several DeepSeek launches before it.

- **Tool ecosystem catch-up (2-3 weeks).** OpenCode, Cursor, Claude Code, Cline, and the long tail of agent harnesses need clean thinking-mode and`reasoning_content` support. The Cursor forum threads[12][13] are the leading indicator; if they resolve within a few weeks, V4-Pro becomes a viable production option. If integration drags into May, the practical adoption ceiling stays low.
- **The Birkhoff-constrained transformer in other labs.** mHC is the architectural idea most likely to spread. Watch Llama 5, Qwen 4, and Mistral’s next foundation model for residual-connection changes that reference it.
- **Closed-frontier pricing response.** With V4-Pro at one-seventh the price of Opus 4.7 and GPT-5.5 at near-comparable coding numbers, sustained pressure on closed-API pricing is the most likely industry move. The question is whether Anthropic and OpenAI hold premium pricing on differentiated workloads (real-world SWE for Anthropic, terminal/agentic for OpenAI) or take broader cuts.

The broader context: six months ago, the best open-weights coding model trailed the closed frontier by 15-20 points on SWE-Bench. Today, **three open models — DeepSeek V4-Pro, Kimi K2.6, GLM-5 — sit within ~7 points of Claude Opus 4.7**[3][6]. Chinese labs alone have shipped a coding-focused checkpoint roughly every week for the past three months. The open vs closed framing is no longer the most useful one. The more useful framing is which model fits which workload, at what cost, with which reliability profile. V4 changes the answer for several of those workloads. The rest is integration work.

V3 to V4 is roughly the same step V2 to V3 was, on a similar release cadence. What’s different this time is the timing: it arrives at the point where the open frontier has caught up enough to make multi-model routing — V4-Flash for cheap calls, V4-Pro for long-context, Opus 4.7 or GPT-5.5 for the critical path — the default architecture. The teams that establish routing and evaluation infrastructure first will gain a larger advantage than any single model choice.

## References

[1] DeepSeek, “DeepSeek V4 Preview Release.” DeepSeek API Docs, April 24, 2026. Link

[2] MarkTechPost, “DeepSeek AI Releases DeepSeek-V4: Compressed Sparse Attention and Heavily Compressed Attention Enable One-Million-Token Contexts.” April 24, 2026. Link

[3] AkitaOnRails, “LLM Coding Benchmark (April 2026): GPT 5.5, DeepSeek v4, Kimi v2.6, MiMo, and the State of the Art.” April 24, 2026. Link

[4] Chew Loong Nian, “I Tested All 4 DeepSeek V4 Modes on 20 Real Tasks — The $0.04 Flash Won 7 of Them.” Towards AI on Medium, April 2026. Link

[5] VentureBeat, “DeepSeek-V4 arrives with near state-of-the-art intelligence at 1/6th the cost of Opus 4.7, GPT-5.5.” April 24, 2026. Link

[6] BenchLM, “Best Chinese LLMs in 2026: DeepSeek V4, Kimi 2.6, GLM-5, Qwen, and Every Model Ranked.” April 2026. Link

[7] Latent Space, “Moonshot Kimi K2.6: the world’s leading Open Model refreshes to catch up to Opus 4.6.” April 20, 2026. Link

[8] vLLM Blog, “DeepSeek V4 in vLLM: Efficient Long-context Attention.” April 2026. Link

[9] DeepSeek-V4-Pro on Hugging Face. Link

[10] Hacker News discussion: “DeepSeek v4.” April 24, 2026. Link

[11] Tencent, “Tencent Unveils Hy3 preview; Model Enhances Agent Capabilities and Real-World Usability.” April 23, 2026. Link. Model weights: tencent/Hy3-preview on Hugging Face.

[12] Cursor Community Forum, “DeepSeek V4: context limited to 200K + reasoning_content error.” April 2026. Link

[13] Cursor Community Forum, “Compatibility with DeepSeek models design to return reasoning_content after tool calls.” April 2026. Link

[14] Qwen Team, “Qwen3.6-27B: Flagship-Level Coding in a 27B Dense Model.” April 22, 2026. Link. Model weights: Qwen/Qwen3.6-27B on Hugging Face. Pricing via OpenRouter.

## Stay in the loop

New posts delivered to your inbox. No spam, unsubscribe anytime.
