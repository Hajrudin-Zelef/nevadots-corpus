---
id: collect-261121-veille-ia/gpt-openai/openai-astra-benchmarks-ten-proofs-and-three-launch-scores
title: "OpenAI Astra Benchmarks: Ten Proofs and Three Launch Scores"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "openai", "proofs", "about", "published", "problems", "proof", "three"]
source: https://www.layer3labs.io/guides/openai-astra-benchmarks
source_anchor: ""
source_lines: [1, 110]
section: "GPT_OpenAI"
---
# OpenAI Astra Benchmarks

What ten machine-checked proofs measure about Astra.

OpenAI published three benchmark scores for Astra at launch: FrontierMath Tier 4 at 98%, ARC-AGI-3 at 99.9%, and ExploitBench at 100%. At Layer3Labs, we build launch-page families for major model releases across the sites we run, so we pay close attention to the difference between a result and a score.

The stronger evidence came a month earlier. Ten open problems in mathematics and theoretical computer science, backed by machine-checkable proofs, is evidence no one can dispute. It is still evidence about ten problems.

What is still missing is the ordinary comparison set. There is no MMLU or GPQA figure and no SWE-bench result, so Astra sits on no public leaderboard next to the models you would weigh it against.

## What OpenAI Published About Astra

OpenAI published ten solved problems in mathematics and theoretical computer science on August 1, 2026, produced by an internal version of Astra. It is the only Astra result anyone outside OpenAI can verify, because the proofs ship as files a machine can check.

The problems were not toy examples. One is an explicit construction of a non-sofic group. That question had been open since Mikhail Gromov set out soficity in 1999.

The set spans several areas rather than one speciality, which carries the real information about general capability. A model that only cleared Ramsey-number problems would say much less.

- An explicit construction of a non-sofic group, open since 1999, which is the headline result of the ten.
- A disproof of Connes's rigidity conjecture on von Neumann algebras, a long-standing question in operator algebras.
- A proof of Ehrhart's volume conjecture, from lattice-point geometry.
- Three problems from the Erdos catalogue, including problem 183 on multicolour Ramsey numbers.
- A 249-page manuscript published alongside the proof files, so the reasoning can be read rather than inferred from a summary.

## What the $2,000 Compute Figure Means

OpenAI put the compute cost for all ten solutions at roughly $2,000 at GPT-5.6 Sol API rates. That figure did more for the story than the proofs did.

Read it as a price on research output rather than a price on Astra. Sol currently runs $5 per million input tokens and $30 per million output, so $2,000 buys a large but ordinary quantity of tokens. What it bought here was ten unsolved problems.

The figure says nothing about what Astra will cost to run, because OpenAI has published no Astra rate. It is a measure of token efficiency on one task, quoted in a neighbouring model's currency.

- Roughly $2,000 total, for all ten solutions combined, at GPT-5.6 Sol API rates.
- Quoted in Sol rates, because Astra has no published price of its own.
- It measures token efficiency on formal mathematics, which is not the workload most buyers care about.
- It does not predict a bill, so no team can use it to size an Astra budget.

## Why These Proofs Can Be Checked by Machine

The Astra proofs are machine-checkable, which puts them in a different category from a benchmark score you have to take on trust. OpenAI released Lean 4 proof certificates on GitHub under an Apache 2.0 licence.

Lean is a proof assistant. It refuses any step that does not follow. A proof that compiles in Lean has therefore been checked end to end by software, rather than reviewed by a person who might miss something.

In Lean, the keyword sorry marks a step the author has left unproved. The Astra repository reports a sorry count of zero, so no gap was papered over in the ten formalised proofs.

- Lean 4 certificates were published, so anyone can rerun the check rather than trusting a claim.
- Apache 2.0 licensing means the files can be used and inspected without permission.
- A sorry count of zero means no proof step was skipped, which is the difference between a formal proof and a convincing draft.
- Machine verification removes the usual doubt about model output, since a wrong step would fail to compile.

## The Astra Benchmarks OpenAI Still Has Not Published

The three scores OpenAI published at launch measure frontier reasoning rather than routine work. FrontierMath Tier 4, ARC-AGI-3, and ExploitBench are hard problems by design, and a near-perfect score on them says nothing about how a model handles a messy contract or an ambiguous support ticket.

The ordinary comparison set is still missing. There is no MMLU, GPQA, SWE-bench, or arena result, so Astra cannot be ranked against the models a buyer is choosing between. A benchmark is only useful when the same test has been run on both sides.

Across the model launches we cover on the sites we operate, the gap that matters is between a vendor's own evaluation and a third party's. Astra has the first and none of the second, so every published number so far comes from the company selling the model.

- No MMLU, GPQA, or SWE-bench figures, so Astra sits on no public leaderboard beside its competitors.
- No third-party evaluation at all, so every score in circulation is OpenAI's own.
- No latency or throughput numbers, which production teams need more than an eval score.
- No published error rates on ordinary business documents, which is the work most buyers would actually route to it.
- The scores that do exist are near their ceilings, at 98%, 99.9%, and 100%, so they cannot separate Astra from whatever comes next.

## The One Astra Comparison OpenAI Did Publish

OpenAI's own evaluation compared Astra against GPT-5.6 Sol on cybersecurity work, and Astra came out ahead on both vulnerability identification and exploit development while using fewer tokens. That comparison is the reason Astra was classified at the Critical cybersecurity capability threshold on August 7, 2026.

Treat it as the most informative number OpenAI has released, because a vendor rarely publishes a capability gain that costs it a launch. OpenAI then stopped two weeks of deployment-focused training and held its largest planned frontier run.

For a buyer the useful read is directional. Astra is meaningfully stronger than Sol at security reasoning, by its maker's own measurement, and OpenAI thought the gap was large enough to change its release plan.

- Astra beat GPT-5.6 Sol on vulnerability identification in OpenAI's own evaluation.
- Astra beat Sol on exploit development in the same evaluation, which is the finding that triggered the Critical classification.
- Astra used fewer tokens to do both, so the gain is efficiency as well as capability.
- The classification is the highest severity level in OpenAI's Preparedness Framework, and Astra is the first model placed there.

## How Much Weight to Put on the Astra Evidence

Treat the mathematics result as proof of reasoning depth on formal problems, and treat everything else about Astra as unmeasured. The published evidence supports nothing more.

Who should not read this as a buying signal: any team whose workload is support tickets, document extraction, drafting, or routine automation. Nothing in ten formal proofs predicts performance on a messy customer email, and a cheaper GPT-5.6 tier will keep handling that work better per dollar.

What would change this answer: a model card with standard evaluations, a published context window, and an Astra rate to compare against Sol. Two of those three would be enough to run a real head-to-head, which is on the OpenAI Astra vs GPT-5.6 Sol page as soon as the numbers exist. Until then, run your own task on GPT-5.6 Sol and record the result, so you have a baseline to measure Astra against on day one.

- Supported — Astra can carry long chains of formal reasoning to a verified conclusion.
- Supported — Astra outperforms GPT-5.6 Sol on security reasoning, on OpenAI's own evaluation.
- Unsupported — any claim about Astra on coding, writing, extraction, or customer-facing work.
- Unsupported — any claim about Astra cost, speed, or context length, since none has been published.

## How to Use GPT-6 Astra

To use GPT-6 Astra effectively, start by choosing an interface that exposes the exact model you need for your workload. Test a new configuration on a single representative task, such as drafting a routine email or parsing a short document, before expanding to full automation. Inspect the saved output directly to confirm formatting, accuracy, and adherence to your prompt instructions.

For coding tasks, Cursor serves as an editor option when the target model appears on its supported list. Check current model availability before setup to confirm the exact model is listed. Once connected, save your prompts and review the generated diffs locally before applying changes to your main codebase.

## Frequently Asked Questions

- OpenAI reports FrontierMath Tier 4 at 98%, ARC-AGI-3 at 99.9%, and ExploitBench at 100%. There is still no MMLU, GPQA, or SWE-bench figure. The other published result is ten solved problems in mathematics and theoretical computer science, released on August 1, 2026 with machine-checkable proofs.
- On one measured dimension, yes. OpenAI's own evaluation found Astra stronger than GPT-5.6 Sol at vulnerability identification and exploit development, using fewer tokens. On every other dimension there is no comparison to make, because Astra has no published evaluations and is not available to test.
- OpenAI put the total compute for all ten solutions at roughly $2,000 at GPT-5.6 Sol API rates. That figure prices the research output rather than Astra itself, since OpenAI has published no Astra rate.
- Yes. OpenAI published Lean 4 proof certificates on GitHub under an Apache 2.0 licence, alongside a 249-page manuscript. Lean rejects any step that does not follow, and the repository reports a sorry count of zero, meaning no step in the ten formalised proofs was left unproved.
- In the Lean proof assistant, sorry is a keyword that marks a step the author has not proved, letting the rest of the file compile around the gap. A sorry count of zero means there are no such gaps, so every step of the proof has been checked by the software.
- OpenAI has not said. It released GPT-6 Astra on September 3, 2026 with a system card and three frontier scores, but no MMLU, GPQA, or SWE-bench result. Third-party evaluations usually follow a release by weeks, and those are the ones that allow a like-for-like ranking.

### Need to know whether a frontier model would change your numbers?

We can benchmark your actual workflow against the models you can buy today, so an Astra decision later is a swap rather than a rebuild.

Book a Consultation
