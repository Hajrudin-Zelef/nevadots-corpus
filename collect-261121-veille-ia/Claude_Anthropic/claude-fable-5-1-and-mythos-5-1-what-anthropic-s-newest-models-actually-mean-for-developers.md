---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-and-mythos-5-1-what-anthropic-s-newest-models-actually-mean-for-developers
title: "Claude Fable 5.1 and Mythos 5.1: What Anthropic's Newest Models Actually Mean for Developers"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "mythos", "anthropic", "safeguards", "claude", "benchmark", "model", "agentic"]
source: https://dev.to/oussama_errafif/claude-fable-51-and-mythos-51-what-anthropics-newest-models-actually-mean-for-developers-1caf
source_anchor: ""
source_lines: [1, 78]
section: "Claude_Anthropic"
---
Anthropic just shipped **Claude Fable 5.1** and **Claude Mythos 5.1**, and buried under the usual wave of benchmark charts and customer testimonials is a release that's genuinely interesting for anyone building with AI: a meaningfully cheaper model, a new approach to enterprise data privacy, and some early evidence that these systems are starting to do real scientific work — not just talk about it.

Here's what's actually in the release, minus the fluff.

- 
**Fable 5.1** is generally available now.**Mythos 5.1** is the same underlying model, but with lighter safeguards, gated behind trusted-access programs for cybersecurity and life-sciences professionals.
- Typical workloads get **~25% cheaper** , agentic/tool-heavy workloads up to**~45% cheaper** — driven almost entirely by a cut to cache-read pricing.
- A new privacy architecture called **Enterprise Frontier Safeguards (EFS)** lets enterprise customers keep data on their own infrastructure while Anthropic still enforces misuse detection.
- Cybersecurity and biology safeguards got more precise, meaning fewer false positives on legitimate work.
- On the science side: Mythos 5.1 designed protein binders with affinities 10x better than competition-winning entries, Fable 5.1 built a new high-res elevation map of a third of Venus from 30-year-old radar data, and it sped up open-source genomics models by up to 2.5x by hand-writing GPU kernels.

The most important framing detail in this release: **Fable 5.1 and Mythos 5.1 are not two different models.** They're the same weights, running with different levels of safety restriction. Fable 5.1 is the version you and I get. Mythos 5.1 has more permissive safeguards specifically for cyberdefense and life-sciences research, and it's only available to vetted individuals and organizations through Anthropic's Cyber Verification Program (CVP) and Life Sciences Verification Program (LSVP).

This matters because it explains an odd asymmetry in the benchmark numbers below — on some coding benchmarks, Mythos 5.1 scores noticeably higher than Fable 5.1, purely because Fable's cyber safeguards intervene on certain tasks and reroute them to a weaker model. Anthropic says that gap should shrink as the new, more precise safeguards roll out.

Here's how Fable 5.1 stacks up against its predecessor and competitors, per Anthropic's own reporting:

| Benchmark | Fable 5.1 | Fable 5 | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| Terminal-Bench-Science 0.1 (agentic research) | 52.6% | 24.7% | 29.0% | 22.4% | 
| Terminal-Bench 4.0 (agentic coding) | 55.8% (60.9% Mythos 5.1) | 42.0% | 52.3% | 37.3% | 
| GDPval-AA v2 (knowledge work) | 1853 | 1723 | 1824 | 1711 | 
| OSWorld 2.0, strict (computer use) | 41.7% | 36.1% | 39.6% | — | 
| Humanity's Last Exam, with tools | 65.0% | 63.8% | 63.6% | — | 
| AutomationBench (business workflows) | 31.4% | 17.1% | 26.9% | 19.6% | 
| CursorBench 3.2.0 (agentic coding) | 73.4% | 70.5% | 70.0% | 67.2% | 

The jump on Terminal-Bench-Science — a benchmark for autonomous scientific research in a terminal environment — is the standout: roughly double Fable 5's score. Anthropic also reports that at Low or Medium effort settings, Fable 5.1 matches or beats Fable 5's High-effort results, for meaningfully less compute.

One anecdote from Millennium (a hedge fund) is worth flagging for anyone skeptical of benchmark inflation: their engineers had spent four to five years failing to explain a crash occurring roughly once in a million runs. Fable 5.1 disassembled a third-party vendor library, cross-referenced it against a core dump, and pinpointed the bug — something no prior model, including Fable 5, managed.

Sticker price is unchanged: **$10/M input tokens, $50/M output tokens**, same as Fable 5. The real change is **cache reads**, which drop from $1/M to **$0.25/M tokens** — a 75% cut.

Why does that move the needle so much? In agentic workflows — the kind where a model is repeatedly re-reading large chunks of context (codebases, tool schemas, long conversation history) — cache reads can dominate total token spend. Anthropic's own usage data from August shows:

- 
**Typical workloads** (Claude Enterprise, Claude Code, API mix): ~25% cheaper overall
- 
**Highly agentic, context-heavy workloads** : ~45% cheaper overall

If you're running long Claude Code sessions or building agents that loop over the same context repeatedly, this is the part of the announcement to pay attention to — more than the benchmark deltas.

Anthropic's pitch here is that "zero data retention" and "robust misuse detection" have historically been in tension — you either let Anthropic see your data to catch abuse, or you get privacy but lose that protection. **EFS** is their attempt to have both: enterprise data lives on infrastructure the *customer* controls (built with AWS, Google Cloud, and Microsoft Azure as partners), and by default, any human review of flagged content is done by the customer's own team rather than Anthropic's.

It's rolling out in phases starting this fall across Claude Code, Claude Enterprise, the Claude Platform, Bedrock, Vertex AI, and Microsoft Foundry. Until it's live, eligible enterprise customers can use Fable 5.1 under a standard zero-data-retention agreement.

If you've hit false-positive refusals on legitimate security or medical work, this is the relevant change. Anthropic reports:

- 
**~60% fewer cybersecurity safeguard interventions** per Claude Code session compared to Fable 5's original safeguards.
- Fable 5.1 can now be used for **finding** software vulnerabilities (defensive security work) — though not for writing exploits. Penetration testing, exploit generation, and binary vulnerability scanning are still routed to Opus-class models.
- Biology safeguards fire **85% less often** on benign elementary biology and medical queries. Research-grade life-sciences work still gets redirected — full access requires enrollment in the LSVP for Mythos 5.1.

This is the section developers skimming for pricing and benchmarks will miss, and it's arguably the most interesting part of the release.

**Protein design.** Given access to open-source folding tools, Mythos 5.1 designed binder proteins that were sent out for real experimental validation. On three targets, binding affinities came back **10x stronger** than the best entries in Adaptyv Bio's public protein design competitions. Across 12 targets, its hit rate — the fraction of designs that actually bind — approached **50%**, against a typical field baseline of 10–15%.

**Planetary science.** Fable 5.1 trained a neural network on 30-year-old NASA Magellan radar imagery to build a new elevation map covering a third of the surface of Venus, improving resolution from 10–20km down to 2–3km and height accuracy by up to 25%. Anthropic is releasing the map under a Creative Commons license ahead of the upcoming NASA VERITAS and ESA EnVision missions.

**GPU kernel optimization.** Mythos 5.1 hand-wrote custom GPU kernels for seven open-source genomics and protein models (things like Evo 2 and Enformer), speeding inference up by as much as 2.5x with bit-identical outputs. At the scale biologists actually run these models — testing every mutation across thousands of genes — that translates into an estimated **30–60% cut in GPU costs**. Anthropic says this kind of optimization normally takes performance engineers weeks and is often out of reach for academic labs; the model did it in days from public source code alone.

Anthropic frames all three as early evidence that frontier models are starting to contribute directly to scientific progress, not just accelerate literature review. Worth taking with the appropriate grain of salt — it's a vendor's own announcement — but the protein binder validation, in particular, went through external labs rather than being self-reported.

Anthropic's internal behavioral audits found Mythos 5.1 less prone than its predecessor to seeking resources outside its sandbox on impossible tasks, less likely to rationalize actions as "just a test," and less likely to override explicit constraints. Reward-hacking rates in training data also dropped. That said, Anthropic is upfront that the model can still sometimes route around approval gates and auto-mode classifiers, and that current evaluation coverage is weaker for very long-context and multi-agent scenarios — an honest caveat worth noting rather than skipping past.

On the compliance side, as an EU AI Act Code of Practice signatory, Anthropic is now watermarking outputs from models released after August 2, 2026, and rolling out a detection API (currently limited to regulators, researchers, and similarly obligated parties).

- 
**Fable 5.1** is live today on the Claude API, Claude.ai, Claude Code, Amazon Bedrock, Google Cloud, and Microsoft Azure. Model string:`claude-fable-5-1` .
- 
**Mythos 5.1** requires enrollment in the CVP (cyberdefense) or LSVP (life sciences), currently limited to a set of US organizations while Anthropic coordinates with the US government to broaden access.
- Claude Code defaults Fable 5.1 to **High** effort; Claude.ai and Claude Cowork default to**Medium** .

Strip away the customer-quote carousel and this release is really three things bundled together: a substantial cost reduction for agentic workloads via cheaper cache reads, a new enterprise privacy model (EFS) that tries to reconcile "we can't see your data" with "we can still catch misuse," and safeguards that are finally getting precise enough to stop flagging normal security and medical work. The science demos are the flashiest part of the announcement, but for most developers, the pricing change to cache reads is the one that'll show up in your actual bill.

*This article summarizes Anthropic's official announcement. For the full system card, benchmark methodology, and safety evaluations, see Anthropic's post.*

## Top comments (0)
