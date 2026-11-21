---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-does-it-deliver
title: "MiMo-V2.6-Pro: Does It Deliver?"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["model", "mimo-v2", "tokens", "deepseek", "kimi", "against", "scores", "artificial"]
source: https://dimsnsums.com/models/mimo-v2-6-pro/verdict
source_anchor: ""
source_lines: [1, 54]
section: "Xiaomi_MiMo"
---
MiMo-V2.6-Pro scores 46.32 on the Artificial Analysis Intelligence Index, the highest of any open-weights model as of September 2026, and it matches or beats frontier closed models on several agent benchmarks. It also generates at 46 tokens per second, which Artificial Analysis calls notably slow.

If you are picking a model for production, the headline score is only half the decision. This chapter keeps the scoreboards separate: the open field, the closed frontier, the gaps, then the speed and the invoice.

### Where it wins

### The open field

every model here is downloadable

DeepSWE v1.1 long-horizon coding, average at 3. Scores from the benchmark appendix published with the launch.

- DeepSeek V4.1 Flash: the KV-cache specialist, covered here in model 005
- Kimi K3: the 2.8T model, covered here in model 003
- MiMo-V2.6-Flash: the 310B-A15B sibling

### Where Pro leads the open field

Four benchmarks from the same published appendix, all on MiMo-V2.6-Pro.

- GDPval-AA 2.1 (Elo): vs 1600 DeepSeek V4.1 Flash, 1524 Kimi K3
- MiMo Code Bench: in-house: vs 60.2 DeepSeek, 60.1 Kimi K3
- JobBench: vs 54.3 Kimi K3, 45.8 DeepSeek V4.1 Flash
- MiMo Visual Coding: in-house: vs 70.6 DeepSeek, 70.3 Kimi K3

Against the closed frontier the picture is specific rather than dominant. Pro reaches 71.9 on DeepSWE v1.1 where Claude Opus 5 reports 74.0 and GPT-5.6 Sol 73.0, and it posts 89.9 on Terminal Bench 2.1 against 89.1 and 88.8. On AutomationBench v1.0.6 it leads the report's comparison set at 53.1, ahead of Opus 5 at 50.3. The report evaluates every configurable baseline at its maximum reasoning effort, which is the setting these numbers belong to.

### Where it loses

### Rebuilding a program from its binary

ProgramBench: reconstruct a program from its compiled binary and documentation. Report Table 3.

The security row is the widest gap. On ExploitBench, which scores progress through real exploitation stages, Pro reaches 47.9 while the closed models reach 70.0 to 78.5. On ExploitGym, developing an exploit past the initial crash, Pro scores 17.8 against 22.1 for Opus 5 and 30.3 for GPT-5.6 Sol. Terminal Bench 4.0 shows a similar pattern, 34.9 against 49.0. Vulnerability reproduction is strong, as CyberGym at 94.0 shows; weaponizing and extending the finding is where the gap lives.

### The speed problem

Intelligence is not delivery. Artificial Analysis measured this model at 46.1 output tokens per second on Xiaomi's own API, against a median of 75.1 for comparable open-weights models, with 3.53 seconds to first token. The same measurements call the model somewhat verbose, at 140M output tokens across the index suite. If your workload is conversation or anything time-sensitive, that is the operating constraint, not the score.

Xiaomi's answer is Pro-UltraSpeed: the same model quality at up to 20 times the output speed, for ten times the price, $4.35 in and $8.70 out per million tokens. The model also carries its own acceleration. A 5-layer DFlash drafter writes 7 candidate tokens in one forward pass from a clean anchor, and the backbone verifies all 7 in parallel. During RL rollouts the block-6 configuration raised average accepted length 31.3% over the previous multi-token-prediction setup, block 6 beat block 8 on throughput by about 6% in mixed-task evaluation, and running the drafter in FP8 added about 10.3% per-node throughput on the long-context workload.

### What the price buys

### Price one agent session

Drag the token counts and the cache share, then compare the three tiers.

This session bills $0.137 on Pro: 630K cached tokens at $0.0036, 270K fresh at $0.435, and 20K generated at $0.87. Artificial Analysis measured a whole Intelligence Index task on this model at **$0.13** on average, with cache effects included. The 99% cache discount is where an agent session gets cheap, because an agent resends the same history on every turn.

### What is still open

The license is MIT, weights included, with no commercial scale threshold attached, and the RL environments and framework are published so the training story can be checked rather than believed. What remains unverifiable is narrower: the three in-house benchmarks, the aesthetic and music evaluations that have no public harness, and the effect of design choices the report describes without ablating, such as the 128-token window from chapter 2.

So where does that leave a decision? The scoreboard says this is currently the strongest open-weights model on Artificial Analysis's suite at $0.13 per task. It does not say it beats the closed frontier everywhere, and the ProgramBench and exploit rows are the receipts for that. If your workload is latency-sensitive, price the UltraSpeed tier first. If it is long-horizon coding or vulnerability work, start from the boards above and then measure the weights yourself. They are on HuggingFace under MIT.
