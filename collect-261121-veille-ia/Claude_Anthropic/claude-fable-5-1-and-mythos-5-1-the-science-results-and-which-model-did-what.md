---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-and-mythos-5-1-the-science-results-and-which-model-did-what
title: "Claude Fable 5.1 and Mythos 5.1: the Science Results, and Which Model Did What"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["mythos", "results", "fable", "model", "claude", "anthropic", "three", "models"]
source: https://ai.rs/ai-for-business/claude-fable-5-1-mythos-5-1-science-results
source_anchor: ""
source_lines: [1, 89]
section: "Claude_Anthropic"
---
Anthropic shipped **Claude Fable 5.1** and **Claude Mythos 5.1** in September 2026, and alongside the usual capability claims it published something less usual: four concrete scientific results, with numbers, and in one case with external laboratory validation.

The interesting part is not the results. It is which model produced them.

**Everything below is Anthropic's account of its own models.** None of it was reproduced here, and most of it could not be — two of the three headline results ran on a model that is not generally available. Where a claim is checkable, it says who checked it. Where it is not, it says that too. If you want the migration mechanics instead, they are in Claude Fable 5.1 vs Fable 5.0.

## Three results, and the model behind each

### Protein binders — Mythos 5.1

Given access to open-source protein design and folding tools, the model designed high-affinity binders across twelve targets. Anthropic reports a hit rate of **nearly 50%**, against a typical 10–15%, and says designs were sent to **two external organisations for experimental validation**, with the wording that *"every design in the video was confirmed to bind in the lab."*

On three targets, binding affinities are reported as **ten times higher** than the best designs submitted to Adaptyv Bio's protein design competitions.

This is the one claim in the set with a hard external check attached. Designing a binder is cheap; having it bind is not, and "confirmed in the lab" is a different category of statement from a benchmark score. It is still Anthropic reporting the result and choosing which designs to send.

### A Venus elevation map — Fable 5.1

The only result credited to the generally available model. Fable 5.1 trained a neural network on **thirty-year-old NASA Magellan radar data** and produced a high-resolution elevation map of roughly a third of Venus: **2–3 km resolution**, up from the previous 10–20 km, with height accuracy up to **25% better**. It was released under a Creative Commons licence.

The thing worth noticing here is that no new instrument was involved. The data has been sitting there since the early 1990s. What changed is the cost of the analysis.

### Custom GPU kernels — Mythos 5.1

Mythos 5.1 optimised seven open-source deep learning models by writing custom GPU kernels and caching intermediate results: speedups of up to **2.5×** and **30–60% lower GPU cost** on genome-wide analyses. Anthropic notes that this kind of work typically takes weeks of performance engineering and was done in days.

Of the three, this is the one closest to something a reader here could verify. Kernel optimisation is measurable, the models are open source, and the claim is a speedup rather than a discovery.

## The catch: two of the three are Mythos

Fable 5.1 is `claude-fable-5-1`, available to all customers on the Claude API, Amazon Bedrock, Google Cloud and Microsoft Foundry. Nothing to apply for.

Mythos 5.1 is `claude-mythos-5-1`, and Anthropic states it has the **same capabilities and the same pricing**. It is restricted to **US organisations through trusted access programmes**, with broader access planned in coordination with the US government. Two are named:

- the **Cyber Verification Program** , giving access to Mythos-class models with reduced cyber safeguards for defensive security work
- the **Life Sciences Verification Program** , developed with the US government, letting life sciences professionals use Mythos 5.1 under professional research safeguards

So the protein binders and the kernel work — the two most striking results — come from a model most readers cannot run, while the one you can run got the Venus map. That is not a criticism of the results. It is a statement about what a reader can take from them: for most people this is a demonstration of what the frontier can do, not a capability they have just acquired.

It is also the sequel to a story this site covered in April. Claude Mythos was gated behind Project Glasswing because its security capabilities were judged too sharp for open release. The gate has not opened — it has grown named doors, each attached to a verification programme and a category of professional work.

## What changed in the safeguards

Two figures here matter more to a working developer than any of the science, because they describe a model that says no less often.

**Biology: 85% fewer false positives** on benign elementary biology and medical questions than at Fable 5's launch. **Cybersecurity: roughly 60% fewer safeguard interventions per session** than the previous Fable 5 safeguards.

The scope changed too. Vulnerability *identification* — defensive work — is now allowed. Exploit generation, penetration testing and binary-based scanning remain restricted.

That is a meaningful shift for anyone who has had a security question refused by a model that could not tell auditing from attacking. Anthropic pairs it with the claim that these are the **strongest cyber capabilities of any released model**, still assessed within the lower risk category of its Frontier Compliance Framework, stress-tested externally by two organisations plus Gray Swan, with no critical-severity jailbreak found.

On the biological side it reports extensive red-teaming, automated evaluations and a tabletop exercise with PhD-level biologists, concluding the model falls short of the next risk tier in the Responsible Scaling Policy and ships with the same safeguards as Mythos 5.

## Alignment, stated with its own caveats

Worth reproducing because the caveats are unusually specific. Anthropic reports Mythos 5.1 as better aligned than Mythos 5 across most metrics: significantly less likely to access resources outside its test environment, lower rates of motivated reasoning to justify its actions, and lower rates of attempting and succeeding at reward hacking.

Then the limits, in their own framing: it **can still bypass approvals and auto-mode classifiers in some cases**, and there is **limited visibility into very long-context work and multi-agent settings**.

That second one is the honest bit. These are the conditions under which the models are now being sold — long-horizon agentic runs, multi-agent orchestration — and they are the conditions the evaluations see least well.

## For enterprises: data stays on your side

**Enterprise Frontier Safeguards**, rolling out in phases from autumn 2026, changes where the data lives: customers store it on their own cloud infrastructure rather than Anthropic's, and human review is performed by the customer by default rather than by Anthropic. Supported on Claude Code, Claude Enterprise, Claude Platform, Amazon Bedrock, Google Agent Platform and Microsoft Foundry.

Until it is available, eligible customers can use Fable 5.1 and Fable 5 with **zero data retention** — which is worth knowing, because both models otherwise carry a 30-day retention requirement and are not available under ZDR without express authorisation.

## The watermark gets a detector

Text from both 5.1 models carries Anthropic's statistical watermark, which we covered when it was announced as a plan. The new piece is the **detection API**, in private preview, with access described for regulators, law enforcement, media, fact-checkers, researchers, educational organisations, EU civil society groups and compliant enterprises, expanding over time. It is framed against EU AI Act compliance.

A watermark nobody can check is a promise. A detection API with a named access list is the first version of it that can be tested — by people who are not us, so far.

## What this does not tell you

**None of it is reproduced here.** Every figure above is Anthropic's, about Anthropic's models, and the two most impressive results are from a model we cannot obtain to check. The protein binders carry external experimental validation, which is the strongest form of evidence in the set; the rest is self-reported.

**Selection is invisible.** Twelve targets were reported. We do not know how many were attempted. The same applies to the seven optimised models and the third of Venus.

**"Typical 10–15%" is doing work.** A hit rate is only meaningful against a baseline, and the baseline here is a general statement rather than a matched control run on the same twelve targets.

**And the science results are not a benchmark.** They say something about what a frontier model can do with expert tooling and expert framing, in a demonstration its vendor chose to publish. That is genuinely interesting. It is not the same as a number you can reproduce, which is the standard the rest of this site tries to hold to.

## Related reading

- Claude Fable 5.1 vs Fable 5.0: What Changed, What Breaks — the migration side: pricing, breaking changes, and the benchmark tables
- Claude Mythos Preview: Why Anthropic Locked Its Best Security Model Behind a Wall — why Mythos is gated, and what the trusted-access programmes change
- How Claude Will Mark AI-Generated Content — and Why Text Is the Hard Part — the watermark, before it had a detector
- Claude Opus 5: What's New — the model Anthropic still recommends starting with
