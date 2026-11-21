---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-benchmarks-the-real-numbers
title: "Nemotron 3.5 Lightning Benchmarks: The Real Numbers"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nemotron", "lightning", "mmlu", "real", "swe-bench", "diamond", "pinchbench"]
source: https://www.layer3labs.io/guides/nemotron-3-5-lightning-benchmarks
source_anchor: ""
source_lines: [1, 134]
section: "NVIDIA_Hardware"
---
# Nemotron 3.5 Lightning Benchmarks Explained

The published eval numbers, what they measure, and how the model stacks up.

Nemotron 3.5 Lightning posts four headline benchmark scores: SWE-bench Verified 51.56, GPQA Diamond 75.44, MMLU Pro 81.94, and PinchBench 85.37. These are NVIDIA's published numbers for the BF16 checkpoint.

The model is a 30B-total, 3B-active Mixture-of-Experts. That means it runs like a small model but reaches for a large pool of experts. NVIDIA released it on August 11, 2026 as an open-weight model.

This guide breaks down each benchmark in plain terms. It also explains NVIDIA's speed and efficiency claims against gpt-oss-120b and Qwen3.6-35B. Always confirm the live figures on the official model card before you quote them.

## The Four Published Scores

Nemotron 3.5 Lightning reports SWE-bench Verified 51.56, GPQA Diamond 75.44, MMLU Pro 81.94, and PinchBench 85.37. NVIDIA measured these on the BF16 checkpoint.

SWE-bench Verified tests real software fixes. GPQA Diamond tests hard science questions. MMLU Pro tests broad reasoning across many subjects. PinchBench tests agent task execution.

Each number is a percentage of tasks solved correctly. Higher is better. A 51.56 on SWE-bench means the model resolved just over half of a curated set of real GitHub issues.

These scores describe a model tuned for agent work and coding, not a general chat toy. Treat them as a starting point, then test on your own tasks.

## SWE-bench Verified 51.56: Can It Fix Code?

SWE-bench Verified measures whether a model can fix real bugs in real code. Nemotron 3.5 Lightning scores 51.56 on it.

The test gives the model a GitHub issue and the project files. The model must write a patch that passes the project's own tests. There is no partial credit for good intentions.

A score in the low 50s is strong for a model this size. It signals that the model can read a codebase, find the fault, and produce a working change.

This is the benchmark most relevant to coding agents. If you plan to wire the model into a pull-request bot, this is the number to watch.

## GPQA Diamond 75.44: Hard Science Reasoning

GPQA Diamond 75.44 measures the model on graduate-level science questions. These are questions experts get right and web searches often get wrong.

The Diamond split is the hardest subset. It is written so that a smart person with Google still struggles without deep training.

A 75.44 shows the model can reason through chemistry, physics, and biology problems, not just recite facts. That matters for research assistants and technical agents.

This score also hints at how well the model handles multi-step logic. Long agent runs depend on that kind of careful reasoning.

## MMLU Pro 81.94: Broad Knowledge Under Pressure

MMLU Pro 81.94 measures broad reasoning across dozens of subjects. It is a harder, cleaner version of the older MMLU test.

MMLU Pro adds more answer choices and removes easy or broken questions. That makes lucky guessing far less useful.

An 81.94 places the model in strong company for general knowledge and reasoning. It suggests the model stays reliable outside its coding specialty.

For a 3B-active model, this is the headline efficiency story. You get broad competence without paying for a giant dense model on every token.

## PinchBench 85.37: Agent Task Execution

PinchBench 85.37 measures how well the model executes structured agent tasks. It is the score most aligned with the model's stated purpose.

NVIDIA built Nemotron 3.5 Lightning for long-running agents that do specialized work. PinchBench probes exactly that skill.

An 85.37 is the model's highest published score. It reinforces that this model is a task executor first and a chatbot second.

If you run agents that call tools, loop for hours, and stay on task, this benchmark speaks to your use case more than a trivia test would.

## How It Compares to Gpt-oss-120b and Qwen3.6-35B

NVIDIA claims performance comparable to gpt-oss-120b at roughly one quarter of the total parameters. That is the core efficiency pitch.

gpt-oss-120b carries about 120B parameters. Nemotron 3.5 Lightning carries 30B total and activates only 3B per token. Matching a much larger model at that size is the headline.

NVIDIA also says the model completes 10,000 tasks about 30 percent faster than Qwen3.6-35B at similar accuracy. Speed at scale is the second pitch.

The company reports up to 4 times the output speed of similar-sized models. In our work evaluating open-weight models for clients, the recurring failure mode is trusting a launch chart without re-testing on the client's real workload. Vendor benchmarks set expectations; your own eval sets the truth.

## Why a 3B-Active Model Scores This High

The scores are strong because a Mixture-of-Experts activates only the parts it needs. Nemotron 3.5 Lightning holds 30B total parameters but fires just 3B per token.

Think of it like a large team where only the right specialists answer each question. You get the knowledge of the whole team at the cost of a few people.

The architecture is hybrid. It interleaves Mamba-2 layers, MoE layers, and select attention layers, which keeps long-context work fast.

That design is why the model can rival a much larger dense model on quality while staying cheap to run. The benchmarks are the visible payoff of that structure.

It also explains the speed claims. Fewer active parameters per token means faster output, which matters most on long agent runs.

The model trained on more than 20 trillion tokens, which builds the broad knowledge the MMLU Pro and GPQA scores reflect. Scale of training and efficiency of inference work together here.

This pairing is the whole pitch of the Lightning line. Big-model competence, small-model running cost, verified by the published scores.

## What Benchmarks Do Not Tell You

Benchmarks miss the parts of your job that are not in the test set. A strong SWE-bench score does not promise strong results on your private codebase.

Public tests can also leak into training data over time. That can inflate scores in ways that do not hold on fresh problems.

Latency, cost, and tool reliability rarely show up in a single number. Yet those decide whether an agent is usable in production.

Read the scores as one input among several. Pair them with a trial on your own prompts before you commit to the model.

## How to Read These Numbers Honestly

Benchmark scores describe average behavior on fixed test sets, not your specific job. Use them to shortlist, then run your own trial.

NVIDIA measured these on the BF16 checkpoint. The NVFP4 4-bit checkpoint trades a little quality for speed and memory, so results can differ.

Numbers also move as test sets update and as the model card is revised. Check the official card before you cite a figure in a report.

The safest habit is simple. Copy the benchmark, note the checkpoint, link the source, and verify the live value on the model card.

Keep the predecessor in view too. Comparing against the Nemotron 3 family shows how fast this line is improving, which shapes your roadmap.

## What you need to run Nemotron 3.5 Lightning yourself

Nemotron 3.5 Lightning needs real memory, but it is within reach of a high-end workstation or a couple of professional GPUs — and many teams simply rent instead of buying. Match the path below to whether you want to own the hardware or pay by the hour.

| Path | What it is | Best for | Get started | 
|---|---|---|---|
| Call the hosted API | Use Nemotron 3.5 Lightning as a pay-per-token API — zero hardware | Most teams; getting started | OpenRouter | 
| Rent GPUs by the hour | Spin up an H100 / A100 for a few dollars an hour | Flexible self-hosting without buying cards | RunPod | 
| Local on unified memory | One Mac with enough unified memory to hold a 4-bit quant | A single quiet on-prem box | Apple Mac Studio (M4 Max, 128GB) | 
| Local on a workstation GPU | One 48GB pro card, or two 24GB consumer cards | Power users who want hardware they own | NVIDIA RTX 6000 Ada (48GB) | 

To put Nemotron 3.5 Lightning to work once it is live, connect a coding client like Cursor (via OpenRouter) or a local runner such as Ollama.

## Frequently Asked Questions

- NVIDIA reports SWE-bench Verified 51.56, GPQA Diamond 75.44, MMLU Pro 81.94, and PinchBench 85.37 on the BF16 checkpoint. Verify current figures on the official model card.
- NVIDIA published these scores for the BF16 checkpoint. The NVFP4 4-bit checkpoint can score differently because 4-bit weights trade some quality for speed and memory.
- NVIDIA claims comparable performance to gpt-oss-120b at roughly one quarter of the total parameters. It reaches this with a 30B-total, 3B-active Mixture-of-Experts design.
- NVIDIA says Nemotron 3.5 Lightning completes 10,000 tasks about 30 percent faster than Qwen3.6-35B at similar accuracy. Test the claim on your own workload before relying on it.
- PinchBench measures agent task execution, the model's stated specialty. Nemotron 3.5 Lightning scores 85.37 on it, its highest published benchmark result.

### Turn Benchmarks Into a Real Decision

Vendor scores are a starting point, not a verdict. At Layer3Labs, we build eval sets on your own tasks so you pick the right model with evidence.

Book a Consultation
