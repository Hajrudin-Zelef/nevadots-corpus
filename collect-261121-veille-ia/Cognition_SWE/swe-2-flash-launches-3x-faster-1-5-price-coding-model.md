---
id: collect-261121-veille-ia/cognition-swe/swe-2-flash-launches-3x-faster-1-5-price-coding-model
title: "SWE-2 Flash Launches: 3x Faster, 1/5 Price Coding Model"
domain: cognition-swe
role: reference
task: article
actors: ["Cognition"]
dates: []
keywords: ["swe-2", "flash", "model", "cognition", "coding", "faster", "price", "launches"]
source: https://emergent.sh/news/swe-2-flash-launches-3x-faster-1-5-price
source_anchor: ""
source_lines: [1, 35]
section: "Cognition_SWE"
---
# SWE-2 Flash Launches: 3x Faster, 1/5 Price Coding Model

Cognition released SWE-2 Flash on November 4, 2026, positioning the new coding model as a faster, cheaper alternative to its flagship SWE-2. The 276-billion-parameter mixture-of-experts architecture, which activates 12 billion parameters per inference pass, runs at three times the speed and one-fifth the cost of SWE-2 while targeting routine software-engineering tasks such as code review, feature development, and documentation updates. The model is available free to paid Devin users through November 30, 2026, via Devin Desktop and CLI.

## Architecture and Base Model

SWE-2 Flash builds on Thinking Machines Lab's Inkling-Small, a mixture-of-experts base model released with full weights on July 30, 2026. Cognition post-trained Inkling-Small using a multi-stage recipe that includes large-scale mid-training followed by reinforcement learning. The team curated a held-out coding dataset to represent real software-engineering scenarios and tested candidate data mixtures by measuring how many tokens each required to reach the baseline validation loss. The final mixture drew from a few trillion tokens across scientific articles, blogs, code repositories, and other sources, with the scarcest and highest-quality data reserved for later training stages.

## Benchmark Performance

SWE-2 Flash shows mixed results against its larger sibling. On FrontierCode 2.0, Flash scores 24.0 compared to SWE-2's 23.8, marking a narrow edge. However, SWE-2 leads on Terminal-Bench 4.0 (27.3 vs 20.0) and DeepSWE 1.1 (73.0 vs 60.1). Cognition frames these results as evidence that Flash is a cost-and-speed option rather than a uniform upgrade. The company also claims Flash outperforms all American open-weight models on the reported evaluations, including models three times larger.

- FrontierCode 2.0: Flash 24.0, SWE-2 23.8
- Terminal-Bench 4.0: Flash 20.0, SWE-2 27.3
- DeepSWE 1.1: Flash 60.1, SWE-2 73.0

## Training Recipe and Reinforcement Learning

Cognition reports that this is the first time its recipe placed a large-scale mid-training stage before reinforcement learning. The team attributes most of the improvement over the SWE-2 recipe to higher-quality and more diverse tasks rather than architectural changes. Task instructions, environment setup, and verifiers underwent audits, and new task types were added to build broader reasoning and problem-solving capabilities for real Devin workflows. Held-out validation loss improved at a stable log-linear rate across all training stages, according to Cognition's internal metrics.

## Speed and Cost Comparison

Cognition illustrates the speed advantage by replaying each model's Terminal-Bench 4.0 gsea-proteomics run at simulated rates of 200 tokens per second for Flash and 60 tokens per second for SWE-2, with transcripts shortened to equal length. At one-fifth the price, Flash is designed for use cases where speed and cost matter more than maximum benchmark scores. The model is available inside Devin Desktop and CLI for routine automations, code review, and incremental feature work.

## Weights Access and Security Restrictions

Flash's weights are not publicly available. Cognition is building a secure sharing program for trusted industry partners and research labs, with plans to enable fine-tuning and deployment in customer-controlled environments, including on-premise infrastructure. Researchers will be able to study behavior and potential misuse under the program. Cognition cites cybersecurity concerns as the reason for withholding public weights, noting that targeted training raised ExploitBench from Inkling-Small's 11.3 percent to 34.8 percent. The company warns that the same capabilities could assist malicious attackers. Cognition's October 26, 2026, security post discussed AI's role in speeding up both attackers and defenders, referencing PHP and Node.js remote-code-execution vulnerabilities disclosed on October 21, 2026.

## Release Date and Availability

Officially launched on November 4, 2026, SWE-2 Flash is available free to paid Devin users through November 30, 2026. After that date, pricing details have not been disclosed. The model is accessible via Devin Desktop and CLI, with further partner and researcher access dependent on approval through Cognition's gated program.

## What This Means

SWE-2 Flash addresses a clear gap in Cognition's product line by offering a cost-and-speed trade-off for routine software-agent work. While it does not uniformly outperform SWE-2 across benchmarks, the three-times speed increase and one-fifth cost reduction make it a practical choice for automations and incremental development tasks. The decision to withhold public weights reflects growing industry caution around dual-use capabilities, particularly in cybersecurity. Whether the gated program can deliver fine-tuning and customer-controlled deployment at scale will determine Flash's reach beyond Devin users.
