---
id: collect-261121-veille-ia/modeles-open/reflection-beam-specs-benchmarks-access-and-news
title: "Reflection Beam: specs, benchmarks, access and news"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "reflection", "model", "parameters", "reasoning", "billion", "benchmarks", "open-weight"]
source: https://reflectionbeam.com/
source_anchor: ""
source_lines: [1, 117]
section: "Modeles_Open"
---
# Beam: an open-weight model built to do more with less compute

Beam is Reflection AI's first open-weight model: a 501-billion-parameter Mixture-of-Experts that activates only 23 billion parameters per token. It targets coding, reasoning and agent work, and Reflection says it matches GLM 5.2 on hard reasoning while using three to four times less inference compute.

## Release status today

Beam was announced with a waitlist. Here is what you can and cannot do with it right now.

### Beta API (waitlist)

OpenAI-compatible Chat Completions endpoint for selected early-access users.

### Open weights on Hugging Face

Promised under Apache 2.0 later in October 2026. No public repository yet.

### OpenRouter

Not in the OpenRouter catalog yet.

### Ollama / LM Studio

Not available. Even 4-bit weights would need around 250 GB of memory, so local use will be limited to large workstations.

In one minute

## What Beam is, and why people call it the “DeepSeek of the West”

Reflection AI is a New York lab founded in 2024 by two former Google DeepMind researchers, Misha Laskin and Ioannis Antonoglou. Beam is its first frontier model and the first in a planned series.

The pitch is efficiency. Beam is a sparse Mixture-of-Experts: of its 501 billion parameters, only about 23 billion do work for each token. That keeps serving costs closer to a mid-sized model while drawing on the knowledge of a very large one.

It is “open-weight”: Reflection says it will publish the weights under the Apache 2.0 license later in October 2026, so anyone can download, run, fine-tune and deploy it on their own hardware. Until then, access is through a waitlisted beta API.

Investors and press have framed Beam as an American answer to strong open models from Chinese labs such as DeepSeek, Alibaba (Qwen), Z.ai (GLM) and Moonshot AI (Kimi). Reflection itself is careful: its own charts show Kimi K3 still ahead on raw capability.

Benchmarks

## How Beam scores against the strongest open models

Selected results from Reflection's announcement. Bars show Beam against GLM 5.2 and Kimi K3 where those scores were reported.

### SWE-Bench Pro v1

### Terminal-Bench v2.1

### τ³ banking

### MCP Atlas

### GPQA Diamond

### Humanity's Last Exam (no tools)

Vendor-reported scores. Not yet independently verified.

All 21 benchmarks
The efficiency claim

## 3–4× less inference compute than GLM 5.2 on hard reasoning

Reflection estimates compute as 2 × active parameters × generated tokens. With 23B active parameters and a length penalty trained into reinforcement learning, Beam spends fewer FLOPs per answer than larger open models. This is the company's own estimate based on Artificial Analysis and DataCurve data; it excludes prompt prefill and serving overhead and has not been reproduced independently.

- 23B active parameters per token
- Reasoning effort you can turn up or down
- Trained to avoid unnecessary reasoning tokens

Head to head

## Beam compared

Side-by-side pages with every benchmark both models reported, plus size, license and availability.

Latest

## Beam news

### Reflection updates Beam's benchmark table

An October 8 note on the launch post says Beam's results were revised.

### Beam is not on OpenRouter, Ollama or Hugging Face yet

Distribution is thin until the weights ship.

### Why Reflection is called the “DeepSeek of the West”

The nickname predates Beam, and launch coverage picked it up again.

Quick answers

## Common questions

## ### What is Reflection Beam?


Beam is a large language model from Reflection AI, announced on October 5, 2026. It is a sparse Mixture-of-Experts model with 501 billion total parameters and 23 billion active per token, built for coding, reasoning and agentic work. It is Reflection's first open-weight model.

## ### Who makes Beam?


Reflection AI, a New York lab founded in 2024 by Misha Laskin (CEO) and Ioannis Antonoglou, both former Google DeepMind researchers. Its investors include Nvidia, Sequoia and Citigroup.

## ### Is Beam open source?


Beam is open-weight. Reflection says it will release the weights under the Apache 2.0 license, which allows commercial use, modification and redistribution. The training data is not open: part of it comes from proprietary licensed datasets.

## ### When will Beam's weights be released?


Reflection says “later this month”, meaning October 2026, together with a technical report, model card and tools for running, evaluating and fine-tuning. No exact date has been given. Our release tracker shows the current status.

## ### How can I use Beam today?


Join the waitlist for Reflection's beta API. Approved users get an OpenAI-compatible endpoint at https://api.reflection.ai/openai/v1 with the model ID Beam-501B-A23B. Once the weights are out you will also be able to run it yourself or through hosting providers.
