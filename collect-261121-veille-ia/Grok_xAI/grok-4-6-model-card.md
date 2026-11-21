---
id: collect-261121-veille-ia/grok-xai/grok-4-6-model-card
title: "Grok 4.6 Model Card"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-02-01"]
keywords: ["grok", "model", "spacexai", "work", "reasoning", "high", "knowledge", "context"]
source: https://www.thequery.in/glossary/grok-4-6
source_anchor: ""
source_lines: [1, 79]
section: "Grok_xAI"
---
# Grok 4.6 Model Card

Models & Architectures
SpaceXAI's August 2026 frontier model for coding, long-running agents, and knowledge work, with a 500K-token context window and configurable reasoning.

Grok 4.6 is SpaceXAI's August 2026 frontier large language model for software engineering, long-running AI agents, visual and interactive projects, and professional knowledge work. It builds on Grok 4.5 rather than introducing a publicly documented new architecture. The practical change is a longer supplemental training run followed by new supervised fine-tuning and reinforcement learning aimed at sustaining work across more steps, checking results, and completing larger projects.

SpaceXAI positions Grok 4.6 as its default model for both code and general text work. It is available through the SpaceXAI API under the model ID `grok-4.6`, in Grok Build, in Cursor, and through gateways including OpenRouter, Vercel, and Cloudflare. The model remains proprietary: SpaceXAI has not released its weights, parameter count, detailed architecture, or complete training dataset.

## What changed from Grok 4.5

Grok 4.6 underwent a longer supplemental training run using curated model-generated reasoning data, advanced technical material, and high-quality engineering data. SpaceXAI says it also changed the optimizer and training recipe. Grok 4.5 then regenerated supervised fine-tuning trajectories across different reasoning settings, agent harnesses, STEM tasks, software engineering, and knowledge work. Model-based checks filtered problematic traces before the later reinforcement-learning stage.

The reinforcement-learning environments covered general coding and knowledge work as well as narrower domains including GPU-kernel optimization, web development, and computer-aided design. SpaceXAI's launch demos emphasize a model that can research an unfamiliar domain, plan a product, implement a substantial first version, and continue testing and refining it. Those demonstrations illustrate the intended workflow; they do not establish that every long-running task will finish correctly without supervision.

## Context window, modalities, and tools

Grok 4.6 has a **500,000-token context window** and a stated knowledge cutoff of **February 1, 2026**. It accepts text and images and produces text. SpaceXAI's API documentation lists no fixed text-output limit, although real requests remain constrained by context, account limits, latency, and cost. Images can be mixed with text in any order, with supported JPEG and PNG files up to 20 MiB each.

The API supports low, medium, high, and xhigh reasoning effort, with high as the default. More reasoning can improve difficult results while increasing output tokens, latency, and cost. Grok 4.6 also supports function calling, web search, X search, code execution, structured outputs, and the Responses and Chat Completions APIs. Current information is not automatic: web or X search must be enabled for events after the training cutoff.

Long agent loops can use SpaceXAI's context-compaction feature. The documentation also recommends a stable prompt-cache key so related requests reach the same server and reuse cached context. Without consistent routing, an apparently cacheable conversation can land on a cold server and incur full input-token charges.

## Official benchmark comparison

SpaceXAI's official launch table compares Grok 4.6 at high reasoning with Grok 4.5 at high, GPT-5.6 Sol at max, and Claude Fable 5 at max.

| Evaluation | Grok 4.6 High | Grok 4.5 High | GPT-5.6 Sol Max | Fable 5 Max | 
|---|---|---|---|---|
| AA Intelligence Index | 61 | 56 | 61 | **62** | 
| GDPval-AA v2, Elo | **1,753** | 1,526 | 1,728 | 1,741 | 
| CursorBench 3.2 | 69.9% | 66.7% | 67.2% | **70.5%** | 
| DeepSWE 1.1 | 65.9% | 54.0% | **73.0%** | 70.0% | 
| FrontierCode 1.1 Extended | 61.3% | 56.6% | 60.6% | **63.6%** | 
| APEX-Agents | 57.5% | 47.1% | 56.7% | **59.2%** | 
| Terminal-Bench 3.0 | 26.0% | 15.7% | **34.6%** | 34.1% | 
| APEX-SWE | 56.4% | 53.6% | Not reported | **58.8%** | 
| AA-Briefcase, Elo | **1,577** | 1,313 | 1,502 | 1,574 | 
| Harvey LAB by Vals | **15.8%** | 12.9% | 2.5% | 11.3% | 

Bold marks the best number in each official comparison row. The table does not show an overall sweep. Grok 4.6 leads GDPval-AA v2, AA-Briefcase, and Harvey LAB; Fable 5 leads the broad Intelligence Index and several coding or agent evaluations; GPT-5.6 Sol leads DeepSWE and Terminal-Bench 3.0. Grok's most consistent result is that it closes much of the gap while retaining a substantially lower API price.

## How to read the official table

SpaceXAI states that third-party model figures are the best self-reported or publicly available results from developer system cards or benchmark leaderboards. That means the rows are not one neutral laboratory run with identical prompts, harnesses, effort budgets, and dates. Grok's own results are launch claims, while competitor results can come from different public sources.

Harness differences matter especially for coding benchmarks. Grok may run through Grok Build or another supported scaffold, while Claude and GPT models use their own agent systems. Reasoning effort also differs: Grok 4.6 is shown at high while GPT-5.6 Sol and Fable 5 are shown at max. The table is useful evidence of frontier competitiveness, but it should not be read as a controlled ranking of the underlying models alone.

The **AA Intelligence Index score of 61** comes from the independently administered Artificial Analysis composite, even though xAI reproduces it in the launch table. SpaceXAI says that score matches GPT-5.6 Sol. Its own table lists Fable 5 one point higher at 62. The Grok 4.5 score shown as 56 is also a later benchmark snapshot than the score of 54 reported at that model's original launch, illustrating how index versions and reruns can change a model's displayed position.

## Pricing and API access

Standard Grok 4.6 API pricing is **USD 2 per million input tokens** and **USD 6 per million output tokens**. SpaceXAI also offers a fast variant at twice the price, equivalent to USD 4 input and USD 12 output per million tokens. The standard rate is unchanged from Grok 4.5 and is substantially below the published prices of several frontier competitors in the launch table.

The rate card is not the complete task cost. Higher reasoning effort generates more tokens, agent loops can make many calls, tools can have separate charges, and a 500K-token context can become expensive when repeatedly sent without effective caching or compaction. Teams should measure cost per completed and accepted task rather than multiplying a single prompt by the headline rate.

The API is OpenAI-compatible through the Responses and Chat Completions interfaces, and SpaceXAI also publishes its own SDK. Aliases such as `grok-4.6` can move to the latest stable version; dated model IDs are preferable when reproducibility matters.

## Availability

At launch, Grok 4.6 was available in the SpaceXAI API, Grok Build, Cursor on all plans, OpenRouter, Vercel, and Cloudflare. SpaceXAI made it the default model in Grok Build and offered temporary doubled included usage in Grok Build and Cursor during the first week. Availability through a gateway does not guarantee identical latency, caching, rate limits, or data handling, so enterprise users should check the provider-specific terms.

## When to use it

Grok 4.6 is a strong candidate for repository-scale coding, long-running terminal agents, web research, interactive application prototypes, visual front-end work, professional document analysis, and tool-heavy workflows where frontier capability is needed at a lower token price. Its 500K context window is useful for large codebases and document sets, provided the harness selects and compacts context rather than treating capacity as perfect recall.

## When not to use it

It is not suitable when downloadable weights, local inference, architecture transparency, or offline operation are requirements. Simple classification, extraction, and short chat workloads may not need a frontier agent model. High-stakes legal, medical, financial, or security decisions still require external verification and human approval; a launch claim of fewer hallucinations does not make outputs authoritative. Long autonomous runs also need constrained permissions, checkpoints, and review because stronger persistence increases both useful work and the consequences of a mistaken plan.

## Grok 4.6 vs. Grok 4.5

Grok 4.6 keeps Grok 4.5's 500K context and $2/$6 standard pricing while improving every score in SpaceXAI's launch table. The largest displayed gains include DeepSWE 1.1 rising from 54.0% to 65.9%, GDPval-AA v2 from 1,526 to 1,753 Elo, APEX-Agents from 47.1% to 57.5%, and Terminal-Bench 3.0 from 15.7% to 26.0%. The reason to upgrade is therefore better long-horizon execution and coding at the same token rate, not a new public architecture or larger context window.

## Bottom line

Grok 4.6 is a meaningful agentic upgrade rather than an undisputed new number one. It matches GPT-5.6 Sol on the broad Intelligence Index, leads selected knowledge-work evaluations, and approaches or exceeds more expensive models on several coding tasks. It also loses important rows to Fable 5 and GPT-5.6 Sol. Its clearest product argument is the combination of frontier-level results, a 500K context window, broad tool support, and unchanged $2/$6 pricing—not universal benchmark dominance.

Last updated: September 22, 2026
