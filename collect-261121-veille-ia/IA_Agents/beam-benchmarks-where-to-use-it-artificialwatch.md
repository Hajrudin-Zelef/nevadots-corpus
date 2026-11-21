---
id: collect-261121-veille-ia/ia-agents/beam-benchmarks-where-to-use-it-artificialwatch
title: "Beam: benchmarks & where to use it · ArtificialWatch"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "reflection", "beam-1", "b-a23b", "model", "artificialwatch", "opencode", "october"]
source: https://artificialwatch.com/model/reflection-beam
source_anchor: ""
source_lines: [1, 73]
section: "IA_Agents"
---
The watchers keep tracking it: lineup changes, pricing moves and deprecations all alert.

Launch detailsLive — Oct 16; open weights on Hugging Face under Apache 2.0 (BF16, FP8, NVFP4, plus a DFlash2 drafter); 501B total / 23B active MoE, 1M context, 128K max output, text only, knowledge cutoff Jun 30 2026; Reflection's API is a free public beta and Vercel's gateway lists it at $0 / $0; OpenRouter lists one host, Nebius, at $0.25 / $1 per 1M (FP8, 131K max output); Artificial Analysis Intelligence Index 30 on launch day; ran in stealth as cat-jam (OpenCode) and MissingNo (Nous Portal), per Reflection.

UPDATE, OCTOBER 16, 16:47 UTC: Beam now has a paid price. OpenRouter lists it as reflection/beam-1.0-501b-a23b with one host, Nebius, at $0.25 per million input tokens and $1 per million output, serving FP8 with the full 1M context and up to 131K output tokens. Reflection's own API is still a free beta.

Nvidia is in early talks to buy Reflection AI outright or to invest more in it, the Financial Times reported on Saturday, October 10, citing people familiar with the matter. No deal has been announced, and the terms had not been settled.

Reflection AI announced Beam today, its first open-weight model: a sparse Mixture-of-Experts with 501 billion total parameters and 23 billion active, built for coding, reasoning and agentic work. It is text-only. The weights are not out yet.

19 providers list Beam in the models.dev catalog (as of 2026-11-20), under the model ids shown below.

OpenRouterreflection/beam-1.0-501b-a23b

Vercel AI Gatewayreflection/beam · reflection/beam-free

Cloudflare AI Gatewayreflection-ai/beam-1.0-501b-a23b

CommandCodereflection/beam:free

CoreWeavereflection/Beam-1.0-501B-A23B

DevPass (LLM Gateway)beam-1.0-501b-a23b

DigitalOceanreflection-beam

Eden AItogether_ai/reflection/Beam-1.0-501B-A23B

Infersiareflection/beam-1.0-501b-a23b

Kenaribeam-1-0:free

Kilo Gatewayreflection/beam-1.0-501b-a23b · reflection/beam-free

LLM Gatewaytogether-ai/beam-1.0-501b-a23b

Also listed by NanoGPT, Nebius Token Factory, Ollama Cloud, OpenCode, OpenCode Go, Reflection, Together AI.

Listings cover the model family, so a provider may list a sibling tier under the same name. Check the id before you build on it.

Beam: quick answers

Is Beam out?

Yes. Beam is live and has been since October 16, 2026.

When was Beam released?

October 16, 2026.

Is Beam good for coding?

On the ArtificialWatch coding index it scores 24.2, #43 of 51 ranked models. The index combines LMArena · Coding, LiveBench · Coding and Vals Vibe Code Bench, taking the median of each model's percentile across them.

Is Beam open source?

It has open weights: the trained model is public on Hugging Face as reflection/Beam-1.0-501B-A23B, 501B parameters as Hugging Face counts them, license Apache 2.0. Among the 17 open-weights models the ArtificialWatch Bench Index ranks, it is #14.

Who makes Beam?

Reflection AI. ArtificialWatch also tracks Next Beam (1.1 / 2) from Reflection AI.

Where can I use Beam?

19 providers list it in the models.dev catalog, including OpenRouter, Vercel AI Gateway, Cloudflare AI Gateway, CommandCode and CoreWeave. The exact model ids each one lists are on this page.

Was Beam tested anonymously before launch?

Yes. OpenCode listed it as the anonymous "Cat Jam" from October 13, 2026, and it was named on October 16, 2026, 3 days later: OpenCode merged Cat Jam's usage into Reflection AI's Beam. Every anonymous OpenCode model and the lab behind it is on ArtificialWatch's stealth models page.

Is Beam open weights?

Yes. Its weights are public on Hugging Face as reflection/Beam-1.0-501B-A23B.
