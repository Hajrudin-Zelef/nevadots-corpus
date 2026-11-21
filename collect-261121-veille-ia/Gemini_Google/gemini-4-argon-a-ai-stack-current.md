---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-a-ai-stack-current
title: "Gemini 4 Argon â AI Stack Current"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["output", "supported", "gemini", "tokens", "argon", "model", "input", "thinking"]
source: https://www.aistackcurrent.com/models/gemini-4-argon/
source_anchor: ""
source_lines: [1, 175]
section: "Gemini_Google"
---
**Gemini 4 Argon** is Googleâs generally available hosted reasoning model for complex software engineering, autonomous multi-step agents, mathematics, science, multimodal analysis, and long-running knowledge work. Google made the stable model `gemini-4-argon` generally available on **October 13, 2026**. The newer Gemini 4 Carbon starts with a restricted cybersecurity-defender rollout; that is a different availability contract, not evidence that Argonâs general API has retired.

Its defining serving change is a **unified 1,048,576-token context**: input, internal thinking, and generated output all share one window. Ordinary synchronous generation remains capped at 65,536 tokens, while **long decode** can continue beyond 64K through background execution until the model finishes or the remaining context is exhausted.

## Current specification

| Property | Gemini 4 Argon | 
|---|---|
| Developer | Google DeepMind | 
| Model family | Gemini 4 | 
| Availability | Generally available | 
| Stable API ID | `gemini-4-argon` | 
| Release date | October 13, 2026 | 
| Inputs | Text, image, video, audio, PDF | 
| Output | Text | 
| Unified context | 1,048,576 tokens shared across input, thinking, and output | 
| Synchronous output ceiling | 65,536 tokens | 
| Long decode | Up to the remaining unified 1M-token window | 
| Thinking levels | `low` ,`medium` ,`high` | 
| Default thinking | `medium` | 
| Function calling | Supported | 
| Structured outputs | Supported | 
| Code execution | Supported | 
| Computer use | Preview | 
| Search grounding | Supported | 
| Google Maps grounding | Supported | 
| File search | Supported | 
| URL context | Supported | 
| Agentic video understanding | Supported | 
| Context caching | Implicit only | 
| Explicit cache objects | Unsupported | 
| Batch / Flex / Priority | Supported | 
| Live API | Unsupported | 
| Image generation | Unsupported | 
| Audio generation | Unsupported | 
| Public weights | Not published | 
| Parameter count | Not published | 

Google calls Argon its most capable model. That is provider positioning. The table above describes the current documented hosted contract.

## Unified context means input and output compete for the same budget

The 1,048,576-token limit is not a 1M input window plus a second output allowance.

The effective budget is:

**input + thinking + generated output â¤ 1,048,576 tokens**

If a request uses a large input, the space available for reasoning and output falls accordingly. Applications that previously treated input and output ceilings as independent should change their capacity calculations before migrating.

`max_output_tokens` also includes thought tokens. If the model consumes the configured ceiling while reasoning, the request can end incomplete with little or no visible output while still incurring token cost.

## Long decode extends one interaction beyond 64K

A synchronous Gemini 4 Argon request has a maximum `max_output_tokens` value of **65,536**.

For larger results, use background execution. Google implements long decode as a sequence of continuation hops, each up to roughly 64K tokens, under the same interaction. Each hop creates another `model_output` step.

The client must concatenate the text across those steps after completion.

Long decode therefore introduces a stateful execution contract:

- preserve the interaction ID;
- poll or stream until terminal state;
- retain stream event IDs for recovery;
- reconnect after network interruption instead of blindly creating a new interaction;
- join all model-output steps;
- keep timeouts long enough for hard reasoning tasks.

Google says background interactions can run for up to 24 hours. Long-running agent or research systems should separate transport failure from interaction failure so reconnects do not duplicate work.

## Thinking levels control the quality/latency trade-off

| Thinking level | Operational use | 
|---|---|
| `low` | Lower token use and latency for simpler or budget-sensitive work | 
| `medium` | Default; balanced setting for most coding and agentic work | 
| `high` | Highest reasoning use for difficult code, math, science, and long-horizon agent tasks | 

Google recommends testing all three and selecting the lowest level that meets the workloadâs quality threshold.

Do not use a very small output ceiling as a substitute for a lower reasoning level. `max_output_tokens` is a hard cutoff; it can truncate reasoning rather than make reasoning more efficient.

## Interactions is the preferred runtime for new work

Googleâs current Argon examples use the **Interactions API**, and its long-decode implementation is simplest there because background execution performs continuation server-side.

The legacy `generateContent` surface remains relevant for existing applications and can use client-side continuation with current SDKs. Treat the two paths as different runtime contracts rather than syntax variants.

For background Interactions long decode, the client should:

1. create the interaction with `background: true` ;
2. set `max_output_tokens` above 65,536 when a longer result is needed;
3. store the interaction ID;
4. poll or reconnect until the interaction reaches a terminal state;
5. combine every `model_output` step.

A client that assumes the first output step is the complete result can silently lose later continuation hops.

## Migration from Gemini 3.x needs configuration changes

Googleâs migration guidance identifies several compatibility boundaries:

- replace the model ID with `gemini-4-argon` ;
- review all output-token caps and raise legacy limits below 32K to at least 32,768 where appropriate;
- use `thinking_level` instead of`thinking_budget` ; old budgets map to a level but produce a warning;
- remove explicit context-cache objects, which Argon does not support;
- increase timeouts for longer reasoning;
- implement long-decode continuation or background execution for workloads that can exceed the single-call limit.

A successful request after only changing the model string is not proof that the migration is complete. Low output caps, old cache code, retry logic, and client timeouts can still create regressions.

## Caching is implicit-only

Gemini 4 Argon supports **implicit context caching**.

It does **not** support explicit cache objects created through APIs such as `cachedContents` / `caches.create`. Google says explicit caching attempts return an error.

This is a direct migration concern for applications that pre-create or explicitly reference caches. Remove that dependency or keep such workloads on a model/runtime that supports the required cache contract.

Caching should also remain separate from interaction storage. A cache hit, stored conversation state, and application-side memory are different mechanisms with different lifecycle and privacy properties.

## Tool and agent capabilities

Argon supports function calling, structured outputs, code execution, file search, URL context, Google Search grounding, Google Maps grounding, and computer use in preview.

The model also supports agentic video understanding, where the model can inspect video evidence dynamically instead of relying only on fixed static frame sampling.

These are model and platform capabilities, not grants of authority. A production agent still needs independent controls for:

- tool admission;
- identity and credential scope;
- filesystem and network reach;
- approval for irreversible mutations;
- retry and idempotency behavior;
- external side effects;
- logging and audit;
- rollback and recovery.

See tool-calling and execution engineering and agent security boundaries and prompt-injection defense.

## Pricing changes on December 1, 2026

Google publishes introductory Gemini API rates through **November 30, 2026**:

| Effective period | Input | Output including thinking | 
|---|---|---|
| Through Nov. 30, 2026 | $2 / 1M tokens | $10 / 1M tokens | 
| From Dec. 1, 2026 | $4 / 1M tokens | $20 / 1M tokens | 

These prices should be combined with actual token behavior. Hard prompts at `high` thinking or with long decode can consume substantially more tokens than ordinary interactive requests.

Model-cost attribution should therefore preserve the **effective date**, not only the model ID and token counts.

## Availability and lifecycle

The stable hosted identifier is `gemini-4-argon`. Googleâs October 13 release notes mark the model generally available.

Google also lists Argon in Google AI Studio and Gemini API developer surfaces, while other Google products and third-party platforms may expose the model under separate rollout, policy, billing, or client contracts.

No public downloadable checkpoint or open-weight license is documented. No parameter count is published in the reviewed first-party material.

For lower-cost or higher-throughput workloads, Argon should not automatically replace Gemini 3.8 Flash. Measure task completion, reasoning-token consumption, latency, tool behavior, and total cost under the actual workload.

## Practical takeaway

Gemini 4 Argon is best treated as a new **model-plus-runtime contract**.

The minimum production record should include:

**model ID + thinking level + context/output caps + API surface + background/long-decode mode + cache contract + tools + policy + effective pricing date**

That record is more durable than a label such as âuse Googleâs best model,â because every one of those dimensions can affect correctness, cost, latency, and recovery behavior.

Evidence
