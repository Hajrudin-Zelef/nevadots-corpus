---
id: collect-261121-veille-ia/atria-dawn/atria-dawn-preview-a-ai-stack-current
title: "Atria Dawn Preview â AI Stack Current"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["atria", "hosted", "model", "preview", "current", "tokens", "context", "dawn"]
source: https://www.aistackcurrent.com/models/atria-dawn-preview/
source_anchor: ""
source_lines: [1, 139]
section: "Atria_Dawn"
---
**Atria Dawn Preview** is an open-weight agentic language model from the Shanghai Artificial Intelligence Laboratory, designed for long-running research, software engineering, tool use, and artifact-producing workflows. The Atria team describes it as built on a **744-billion-parameter mixture-of-experts GLM-5.2 foundation model** and trained with a Verifiable Experience Pipeline that connects agent trajectories to executable or externally checked outcomes.

Atriaâs current hosted service accepts **text input only**, exposes a **512K-token context window**, and allows request output limits from **1 to 65,536 tokens**. The downloadable BF16 and FP8 Preview checkpoints use a separately documented **256K** context and the **MIT License**. The hosted limit must not be transferred to those artifacts without checkpoint-specific evidence.

The model is a post-trained derivative of GLM-5.2, but its serving contract must be treated independently. GLM-5.2âs current hosted 1M-token context, pricing, tool services, and other Z.ai API behavior do not automatically transfer to Atria Dawn Preview.

## Current model contract

| Field | Current documented state | 
|---|---|
| Model name / hosted ID | `Atria-Dawn-Preview` | 
| Lifecycle | Preview | 
| Organization | Shanghai Artificial Intelligence Laboratory | 
| Foundation | GLM-5.2 MoE | 
| Provider-described foundation size | 744B parameters | 
| Hugging Face repository size metadata | 753B parameters | 
| Hosted context | 512K tokens; current integration configuration uses `512000` | 
| Hosted request output cap | 65,536 tokens | 
| Hosted input modality | Text only | 
| Hosted output modality | Text | 
| Streaming | Supported | 
| APIs | Chat Completions, Anthropic-compatible Messages, OpenAI-compatible Responses | 
| Open weights | BF16 and FP8 repositories | 
| License | MIT | 
| Public API pricing | Not published in the reviewed Atria documentation | 
| Knowledge / training cutoff | Not published | 
| Retirement date | None published | 

The **744B** and **753B** numbers should not be silently merged. The Atria paper and model card describe the underlying GLM-5.2 foundation as 744B parameters, while Hugging Face currently displays **753B params** for the released Atria checkpoint repository. The inspected Atria sources do not explain that difference or publish a separate active-parameter count, so this reference preserves both observations rather than inferring a reconciliation.

## Hosted API surface

The international API base is:

`https://api.atria-asi.ai/v1`

Atria documents one hosted model ID, with capitalization significant: `Atria-Dawn-Preview`.

| Interface | Endpoint | Input field | Output-limit field | 
|---|---|---|---|
| OpenAI-style Chat Completions | `/v1/chat/completions` | `messages` | `max_completion_tokens` or`max_tokens` | 
| Anthropic-style Messages | `/v1/messages` | `messages` | `max_tokens` | 
| OpenAI-style Responses | `/v1/responses` | `input` | `max_output_tokens` | 

All three surfaces support streaming and non-streaming requests. The context window includes input plus generated output. A caller can request an output cap from 1 through 65,536 tokens, but the response must still fit inside the remaining context and can terminate earlier because of stopping conditions.

Atriaâs documentation no longer imposes a separate application-level request-body size ceiling on these inference endpoints. Requests remain subject to the hosted **512K** context limit, service quotas, and infrastructure constraints.

## Text-only modality is an operational constraint

Atria Dawn Preview is explicitly **text only** on the hosted service. The providerâs Codex integration guide warns that sending an image can fail with `400 Atria-Dawn-Preview is not a multimodal model` and recommends declaring `input_modalities: ["text"]` in compatible client metadata.

This matters because many agent harnesses assume that a frontier model accepts screenshots, images, or PDFs. Those files must be converted to text or otherwise processed outside Atria before the relevant content is included in the model request.

The open checkpoint should not be described as vision-capable merely because the surrounding agent system can operate on files or browser state. Tool and environment adapters can turn external observations into text; that does not add an intrinsic image modality to the model.

## Tool use and agent execution

Atria is optimized for agentic workflows, but tool execution belongs to the surrounding runtime. The hosted documentation describes the loop explicitly: provide the task goal, available tools, and current state; the **application or agent executes tool calls** and returns results to the model for the next step.

Atria provides configuration examples for Claude Code, Codex, WorkBuddy, Qoder, OpenClaw, and Hermes through its compatible APIs. These integrations can expose shell, filesystem, web, MCP, or other tools according to the harness configuration. Those capabilities are not provider-side built-in tools owned by the checkpoint.

The public API reference reviewed here does not publish a universal native tool schema, built-in Web Search tariff, or provider-managed tool catalog. Deployments should therefore distinguish **model tool-use behavior** from **the execution permissions of the chosen agent harness**.

## Reasoning controls

Atriaâs current Codex integration metadata advertises three reasoning profiles:

| Codex catalog profile | Provider description | 
|---|---|
| `low` | Faster responses with lighter reasoning | 
| `medium` | Balance of speed and reasoning depth | 
| `high` | Greater reasoning depth for complex tasks | 

These values are published in Atriaâs **Codex model-catalog example**. The general Atria API reference does not separately document a cross-interface HTTP `reasoning_effort` field or guarantee that the three compatible API surfaces expose identical reasoning-control semantics.

For that reason, the profiles should be treated as documented Codex integration metadata rather than projected onto every client or API request format.

## Open weights and deployment

Atria publishes two current checkpoints:

| Repository | Precision / role | Context published by Atria | License | 
|---|---|---|---|
| `internlm/Atria-Dawn-Preview` | Main instruct checkpoint | 256K | MIT | 
| `internlm/Atria-Dawn-Preview-FP8` | FP8-quantized instruct checkpoint | 256K | MIT | 

The project recommends **SGLang 0.5.13.post1 or later** or **vLLM 0.23.0 or later** for local deployment. Those are deployment-version requirements from the release material, not claims that every hardware configuration can serve the full 256K context efficiently.

The repositoryâs MIT License permits use, modification, distribution, sublicensing, and sale subject to preservation of the copyright and permission notice and the licenseâs warranty disclaimer.

Hosted and self-hosted Atria are related but not identical products. The hosted 65,536-token request output cap, quotas, endpoint compatibility, authentication, latency, and any future pricing are service properties and should not be assumed to describe every local runtime.

## Architecture and training position

Atriaâs durable architecture claim is that it is built on the **GLM-5.2 mixture-of-experts foundation**. The Atria paper does not present Atria as a new base architecture with a separately specified expert topology. Instead, the technical novelty emphasized by the release is post-training for verifiable agent trajectories.

The paper calls this process the **Verifiable Experience Pipeline**. During rollouts, the agent observes an environment, selects tools, inspects outputs, and revises its actions. Outcomes are checked with task-specific evidence such as executable tests, experiment metrics, file or application state, geometric checks, or source support. The authors say trajectory curation removes incomplete, contradictory, duplicate, or behaviorally invalid examples.

This training description supports Atriaâs position as an **agentic post-trained model**, but it does not publish the complete training corpus, token count, training cutoff, compute budget, or active parameter count. Those fields remain undocumented rather than inferred from GLM-5.2.

## Evaluation evidence and conditions

The Atria team reports results on **16 agentic benchmarks**. These are provider/author-reported evaluations, not independent measurements. The paper is unusually useful because Appendix B documents several harness conditions that materially affect comparison.

| Benchmark | Atria reported score | Published condition worth preserving | 
|---|---|---|
| AutomationBench | 53.8 | AutomationBench v1.0.6 with the null-type fix from PR #13 | 
| BFCL v4 | 77.0 | Official harness commit `6ea5797` ; 5,217 cases; 16 workers; temperature 0.001; max two attempts | 
| DeepSearchQA | 96.0 | In-house search/visit/Python harness; up to 500 steps; 256K context; 64K max output per generation | 
| BrowseComp | 92.5 | Same research harness plus discard-all context management | 
| SWE-bench Pro | 59.6 | OpenHands 1.23.0; 4.5-hour task limit; 250 steps; network disabled; Git history removed | 
| Terminal-Bench 2.1 | 78.3 | Normally Claude Code 2.1.224; up to 500 turns; four independent runs; average pass rate | 
| MLE-bench Lite | 86.2 HumanRank | Claude Code; 12-hour limit per evaluation | 
| CyberGym | 86.5 | Claude Code 2.1.119; 2 CPUs, 4 GB RAM; 4h10m timeout; high thinking effort | 

The paper reports Atria as highest among the available comparison entries on five of 16 benchmarks: AutomationBench, BFCL v4, DeepSearchQA, BrowseComp, and CyberGym. Missing competitor results are not scored as zero, and the comparison models run through different harnesses on some benchmarks. These results therefore establish the providerâs measured agentic profile under the stated conditions, not a universal model ranking.

The release also presents selected research, coding, document, and cybersecurity demonstrations. The paper explicitly describes those as **selected cases rather than estimates of average task success**. They are useful behavioral evidence, but they should not be generalized into reliability percentages.

## Pricing, availability, and lifecycle

Atria provides hosted access through an international API service and a separate China access path, while the weights are available through Hugging Face and ModelScope. The hosted documentation currently labels the model **Preview**.

The reviewed public API documentation does **not** publish a token price table. A pricing claim should therefore not be inferred from quota messages, console availability, third-party gateways, or the cost of the underlying GLM-5.2 service.

No retirement or replacement date is published. Because both the model name and release status contain `Preview`, production teams should pin the exact model ID, test application-critical workflows, preserve evaluation fixtures, and treat future endpoint or lifecycle changes as possible until Atria publishes a stable-version policy.

## Technical position

Atria Dawn Preview is important because it combines three properties that are often separated in current agent systems:

1. **Open weights** under MIT, rather than a hosted-only agent model.
2. A documented **512K / 65,536-token hosted serving contract** with three common compatibility surfaces, distinct from the downloadable 256K Preview checkpoints.
3. A release focused on **verifiable long-horizon execution** , with benchmark protocols that expose tool, timeout, retry, context, and judge conditions instead of publishing scores without a harness description.

Its main deployment limitation is equally clear: the current model is **text only** and the public API does not yet document a price table, training cutoff, active parameter count, or universal native reasoning/tool parameter contract. Those unknowns should stay explicit until the provider publishes them.

Evidence
