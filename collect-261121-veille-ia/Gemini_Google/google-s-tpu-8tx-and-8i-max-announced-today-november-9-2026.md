---
id: collect-261121-veille-ia/gemini-google/google-s-tpu-8tx-and-8i-max-announced-today-november-9-2026
title: "Google's TPU 8tx and 8i Max, Announced Today, November 9, 2026"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-09"]
keywords: ["google", "data", "november", "system", "built", "announced", "today", "single"]
source: https://dev.to/paullovelace/googles-tpu-8tx-and-8i-max-announced-today-november-9-2026-5d2e
source_anchor: ""
source_lines: [1, 68]
section: "Gemini_Google"
---
Google announced two new TPU systems today, November 9, 2026: TPU 8tx for confidential inference, and TPU 8i Max for massive context windows. It is the fourth TPU system Google has introduced in a single year — 8t and 8i arrived at Cloud Next in April — and it says something about where AI infrastructure is heading: the bottleneck is no longer just raw compute. It is trust, and memory.

I have not touched this hardware — nobody outside Google has — and this article is built from Google's announcement itself, checked 9 November 2026. Where a number is Google's, I'll say so, because a launch-day blog post is a promise, not a benchmark.

Google is unusually explicit about this, and it is the most useful sentence in the whole announcement. Designing a new chip takes years. A system is how you package the compute, memory, networking, and security around a chip — and that can be reconfigured in months.

TPU 8tx is built on the same silicon as TPU 8t. TPU 8i Max is two TPU 8i chips combined onto a single package. Nothing here is a new transistor layout. What is new is what Google built around the silicon, and the two problems those packages are aimed at are real ones developers actually hit.

The problem 8tx solves is one every regulated team knows: data is normally encrypted at rest and in transit, but it has to be unlocked in memory while a computer processes it. For a bank, a government agency, or a hospital running a frontier model next to its most sensitive data, that unlocked moment is the risk.

TPU 8tx builds the security into the hardware layer, keeping sensitive data and model weights protected and isolated even while in active use. It pairs the 8t silicon with 5th Generation AMD EPYC CPU hosts, and it adopts the same low-latency Boardfly network Google built for real-time reasoning. Google claims up to 2x lower latency compared to TPU 8t, with a near-zero performance penalty for the encryption.

That last phrase is doing a lot of work, and it is Google's claim, not a measured result. Confidential computing has historically cost you performance — that is the whole trade-off the category exists to negotiate. If the penalty really is near zero, 8tx matters. If it is not, it is a niche product for teams whose compliance department outranks their latency budget. We will not know until someone independent measures it.

The more interesting part for developers is what 8tx lets you do on one system: because it pairs training-grade compute with a fast inference network, teams can fine-tune models on private data, distill large models into smaller ones, and train agents through reinforcement learning — all without migrating sensitive data between separate clusters. That is the workflow that usually forces a regulated team to choose between keeping data in one place and doing anything useful with it.

8i Max attacks the other bottleneck. When dozens of AI agents work together across an entire codebase, or hours of video at once, memory bandwidth becomes the wall — the processors sit idle waiting for data.

Google's answer is blunt: take two low-latency TPU 8i chips and put them on a single package. The result is double the compute, double the SRAM, and double the HBM bandwidth of a single TPU 8i, with twice the silicon inside the existing rack footprint — so a data center gets significantly more computing power per square foot and per watt.

That is the system aimed at the agentic future Google keeps describing: dozens of agents reasoning across million-token contexts at once. The context window race — Claude at 1M tokens, Gemini pushing further — is quietly becoming a memory-bandwidth race, and 8i Max is Google's bet that the way to win it is packaging, not a new chip.

Together, the eighth-generation lineup now covers a stage of the agentic lifecycle each:

| System | Built for | 
|---|---|
| TPU 8t | Large-scale training, maximum goodput | 
| TPU 8i | Low-latency real-time reasoning | 
| TPU 8tx | Confidential inference and small-scale training | 
| TPU 8i Max | Massive context windows, highest compute density per rack | 

For reference, Google says 8t offers up to 2.7x better training performance and 8i offers 80% better inference price performance versus the previous generation. Those are Google's numbers, from the April announcement, and they still have no independent verification attached.

The hardware is the headline, but the software section of the announcement is arguably more important for anyone who might actually use a TPU. Google has spent the year removing the barriers that kept TPUs a Google-and-friends technology:

- 
**TorchTPU, open-sourced last month** , so PyTorch code and Hugging Face models run natively on TPUs, with out-of-the-box TPU backends for vLLM and SGLang. No code rewrites, per Google.
- 
**A new open-source TPU agent suite, now in preview** , that plugs into Claude Code, Cursor, and Google Antigravity to automate TPU onboarding, diagnose bottlenecks, and autotune custom kernels in plain language.
- 
**XLA Serving Engine, also open source** , to train in Python and serve in a low-latency C++ runtime across TPUs, GPUs, and CPUs.
- 
**TPU Sync, open-sourced** , a model-state layer that turns chip HBM, host DRAM, shared storage, and the data center network into one memory hierarchy — Google claims it cuts KV transfer latency and time to first token by more than 90%.
- 
**Agent Compute Engine** , which suspends idle agent sessions and shifts working memory off the chip in milliseconds, cutting costs for stateful agents that pause for seconds or hours.
- 
**Cloud Pathways, now generally available on GKE** , bringing the DeepMind runtime behind Gemini to every Google Cloud customer, and**GKE dynamic slicing, also GA** , which Google says cuts job startup times by up to 5x.

If you are a developer, that list is the real story. The hardware tells you where Google thinks the workloads are going. The software tells you Google finally wants you to be able to get there without hand-tuned code.

- 
**No price, no availability date.** Google's announcement gives neither. Bloomberg reports the systems are expected in 2027 — that is a press report, not Google's word, and there is no launch window to plan around yet.
- 
**Every performance number is Google's own.** The 2x lower latency on 8tx, the 2x compute and bandwidth on 8i Max, the 2.7x training and 80% inference figures — all from Google, with no independent benchmark published. Google points to InferenceX and AgentX benchmark results for its software stack, but those are its own production measurements too.
- 
**These are repackaged systems, not new chips.** That is a strength (fast to market) and a limit (no new silicon capability). The ceiling of what 8tx and 8i Max can do is the ceiling of the 8t and 8i silicon underneath.
- 
**TPU's customer base is still small.** Google says nearly 100 customers — frontier labs, trading firms, retailers, universities. That is a real business, and a growing one, but it is not the thousands-strong ecosystem NVIDIA sells into.

Four TPU systems in one year is a pace Google has never hit before, and the pattern is clear: stop waiting for the next chip, and ship the system the workload needs now. 8tx is for the teams whose data cannot leave the room. 8i Max is for the teams whose agents cannot fit in memory. The software opening — TorchTPU, the agent suite, TPU Sync — is what makes either of them reachable for developers who are not Google.

The honest test is still ahead: independent benchmarks, a price, and a date. Until then, this is a credible direction, announced well — and a promise, not a product you can buy.

If you run agents against a whole codebase today, what breaks first for you — the context window, the memory bandwidth, or the security review? I'd genuinely like to hear which of those three is the wall you actually hit.

*Sources: Google Blog — "Introducing TPU 8tx and TPU 8i Max" by Amin Vahdat, 9 November 2026 (blog.google); coverage by The Verge, Gizmodo, Unite.AI and WebProNews, 9 November 2026; Bloomberg reporting that the systems are expected in 2027. All performance and capability claims are Google's, stated as such. No hands-on testing was performed for this article, and no independent benchmark exists yet. This breaking article is not the daily buyer-intent post, and it contains no affiliate links.*

## Top comments (0)
