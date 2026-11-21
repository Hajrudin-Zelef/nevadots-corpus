---
id: collect-261121-veille-ia/gemini-google/google-expands-its-8th-gen-tpu-portfolio-for-agentic-ai-electronics-weekly
title: "Google expands its 8th-Gen TPU portfolio for agentic AI ⋆ Electronics Weekly"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "data", "portfolio", "compute", "vahdat", "memory", "double", "expands"]
source: https://www.electronicsweekly.com/news/google-expands-its-8th-gen-tpu-portfolio-for-agentic-ai-2026-11/
source_anchor: ""
source_lines: [1, 27]
section: "Gemini_Google"
---
Google has expanded its eighth generation TPU portfolio with the TPU 8tx and TPU 8i Max.

Following on from the TPU 8t and TPU 8i releases in April, it marks four new TPU systems in a year from the AI hyperscaler.

The TPU 8t was for large-scale training and and TPU 8i for real-time reasoning, right. Whereas the new TPU 8i Max is for massive context windows (the collection of tokens in a single processing) and the highest compute density per rack. And the TPU 8tx is for secure and confidential inference.

## Beyond DeepMind

“While we’ve always co-designed our TPUs in partnership with Google DeepMind, we’re now increasingly getting feedback from external organisations on their specific needs,” writes Amin Vahdat, SVP and Chief Technologist for Google AI and Infrastructure.

“With computing moving from human to agent scale, businesses are asking for accelerators that can reason across AI models’ massive context windows. In addition, some highly regulated customers are asking for specialised encryption capabilities for unique workloads. The new TPU systems we are announcing today address this new reality.”

## TPU 8tx

For TPU 8tx, Vahdat explains the system is for organisations running proprietary models and autonomous agents on their most sensitive data.

“Normally, data is encrypted when stored or sent over a network, but has to be unlocked in memory while a computer processes it. Built on the same silicon as TPU 8t and paired with 5th Generation AMD EPYC CPU hosts, TPU 8tx builds security directly into the hardware layer, keeping sensitive data and model weights protected and isolated even while in active use. And by adopting the same low-latency Boardfly network we created for real-time reasoning, TPU 8tx keeps multi-step agents moving, delivering up to 2x lower latency compared to TPU 8t with a near-zero performance penalty.”

## TPU 8i Max

For TPU 8i Max, Vahdat says Google was addressing the bottleneck problem of the “memory wall”

The new system combines two low-latency TPU 8i chips onto a single package.

“TPU 8i Max scales compute and memory in lockstep, delivering double the compute, double the SRAM and double the HBM bandwidth of TPU 8i, so processors remain equipped with essential data and so that large workloads do not stall. Further, because TPU 8i Max fits twice the silicon within the existing rack footprint, data centers can deploy significantly more computing power per square foot and per watt.”

You can read more on the latest TPU releases on the Google blog post.
