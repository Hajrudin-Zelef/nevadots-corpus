---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-codex-setup-and-new-api-pricing-explainx-ai-blog
title: "DeepSeek V4 Pro: Codex Setup and New API Pricing | explainx.ai Blog"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "codex", "agent", "setup", "points", "official", "table", "august"]
source: https://explainx.ai/blog/deepseek-v4-pro-0813-terminal-bench-cline-august-2026
source_anchor: ""
source_lines: [1, 173]
section: "DeepSeek"
---
**DeepSeek V4 Pro 0813 is now an official production release**, not an inferred API checkpoint. On August 13, 2026, DeepSeek rolled it out across the app, web, and API with stronger agent performance, native **OpenAI Responses API** support, a **one-click Codex setup**, and **low / high / max** reasoning control. On the web and app, it is available through **Expert Mode**.

The stable API model name remains `deepseek-v4-pro`. That is useful for existing integrations, but the billing story changed: at **16:00 UTC on August 16**, DeepSeek replaces its current flat rates with peak and off-peak prices. “Off-peak is 50% lower” means half of the **new peak rate**, not half of today’s price.

## TL;DR: what changed in the DeepSeek V4 Pro launch?

| Question | Direct answer | 
|---|---|
| **What launched?** | The general-availability **DeepSeek-V4-Pro-0813** release | 
| **Where is it live?** | DeepSeek app, web **Expert Mode** , and API | 
| **Do model IDs change?** | No. Keep `deepseek-v4-pro` or`deepseek-v4-flash` | 
| **What is new for agents?** | Higher vendor-reported scores across Terminal Bench, NL2Repo, Cybergym, DeepSWE, and other suites | 
| **Does it work with Codex?** | Yes. The API now speaks the Responses format natively, and DeepSeek provides one-click setup | 
| **Reasoning controls?** | `low` for simple work,`high` for daily agent workflows,`max` for hard tasks | 
| **When do prices change?** | **August 16, 2026 at 16:00 UTC** | 
| **Is off-peak cheaper than the old rate?** | No. It is cheaper than the new peak rate, but higher than the pre-change flat rate | 

## DeepSeek V4 Pro 0813 agent benchmarks

Official DeepSeek V4 Pro 0813 agent benchmark table comparing V4 Pro, V4 Flash, GLM-5.2, Kimi-K3, Opus 4.8, and Fable 5 across ten evaluations

*Source: DeepSeek’s August 13, 2026 launch announcement. For public code-agent tasks, DeepSeek says V4-Pro-0813 used the upcoming DeepSeek Harness in minimal mode with max effort, `topp=0.95`, and `temperature=1.0`; results can vary with another harness.*

DeepSeek’s official table reports:

| Benchmark | V4 Pro 0813 | V4 Pro Preview | Change | 
|---|---|---|---|
| HLE without / with tools | 42.7 / 60.0 | 37.7 / 48.2 | +5.0 / +11.8 points | 
| Terminal Bench 2.1 | 87.9 | 72.1 | +15.8 points | 
| NL2Repo | 61.5 | 38.5 | +23.0 points | 
| Cybergym | 83.3 | 52.7 | +30.6 points | 
| DeepSWE | 62.7 | 12.8 | +49.9 points | 
| Toolathlon-Verified | 74.1 | 55.9 | +18.2 points | 

The original preview-era coverage correctly spotted the 87.9 Terminal Bench result, but the official release adds the missing evidence: this is a broad agent update, and DeepSeek has now published the full evaluation table. The previous “silent checkpoint” framing no longer applies.

### How much should you trust the table?

Use it to decide what to test, not to skip your own evaluation. These are vendor-reported scores. The table also mixes results generated with different harnesses and includes two internal DeepSeek test sets, **DSBench-FullStack** and **DSBench-Hard**.

Terminal Bench 2.1 remains useful for shell-driven agent work, but it is not interchangeable with Terminal-Bench v3.0. Our Terminal-Bench 2.0 guide explains what this benchmark family measures, while the Grok 4.6 launch analysis covers a different benchmark version. Do not place the scores on one shared leaderboard.

## How to use DeepSeek V4 Pro with Codex

DeepSeek now exposes the **Responses API** at its existing API base URL and publishes a Codex-specific model catalog. Its official integration guide says one configuration works across Codex CLI, the desktop app, and the VS Code extension.

The recommended macOS/Linux setup is:

```
bash <(curl -fsSL https://cdn.deepseek.com/api-docs/codex-deepseek-setup-en.sh)
```
Inspect remote scripts before executing them in a sensitive environment. DeepSeek says its installer backs up `~/.codex/config.toml`, writes a model catalog, preserves settings such as MCP servers and project trust, validates the new files, and lets you restore the earlier configuration.

For manual configuration, the important provider contract is:

```
model = "deepseek-v4-pro"
model_provider = "deepseek"
preferred_auth_method = "apikey"
forced_login_method = "api"
model_reasoning_effort = "high"
model_catalog_json = "~/.codex/models.json"
[model_providers.deepseek]
name = "deepseek"
base_url = "https://api.deepseek.com/"
wire_api = "responses"
experimental_bearer_token = "<your DeepSeek API Key>"
```
The complete model catalog is longer, so copy it from DeepSeek’s official Codex integration guide rather than inventing a partial file. If you are comparing provider setups, our Codex with open-source models guide explains why `wire_api = "responses"` matters.

### Native Responses API does not mean complete feature parity

DeepSeek documents this as format compatibility, with deliberate gaps. Function tools and server-side web search work, and the `apply_patch` custom tool is supported specifically for Codex. Image and file inputs are not supported, however, while built-in tools such as file search, code interpreter, computer use, and MCP are ignored by the API.

The endpoint is also stateless: `previous_response_id`, conversations, storage, and background mode are not supported. A client must resend the conversation state it needs. Streaming ends with a semantic `response.completed`, `response.incomplete`, or `response.failed` event rather than the older `data: [DONE]` marker.

This distinction matters when you move an existing Responses client. DeepSeek says unsupported parameters are silently ignored, so a request can return successfully without delivering the behavior you assumed. Test every tool type and state-management path your harness uses; a successful HTTP response alone is not a compatibility test.

## What low, high, and max reasoning effort mean

Both V4 Pro and V4 Flash now accept three actual effort tiers:

| Effort | DeepSeek’s recommended use | Trade-off | 
|---|---|---|
| `low` | Simple tasks | Faster, lighter reasoning | 
| `high` | Everyday agent workflows | Default balance of depth and latency | 
| `max` | The hardest multi-step tasks | More reasoning time and token use | 

DeepSeek maps requests for `medium`, `high`, or `xhigh` to its actual `high` tier. In the OpenAI Chat Completions format, use `reasoning_effort`; in the Responses format, use `reasoning.effort`. The official thinking-mode guide also warns that `temperature`, `top_p`, `presence_penalty`, and `frequency_penalty` have no effect while thinking mode is enabled.

For agent workflows, do not automatically choose `max`. Start at `high`, collect failures, and route only the hard tail upward. Reliable agent skills and disciplined loop engineering often matter more than spending extra reasoning on every turn.

## DeepSeek V4 peak and off-peak API pricing

Official DeepSeek V4 API pricing table showing peak and off-peak rates for V4 Flash and V4 Pro from August 16, 2026

*Source: DeepSeek API pricing notice. New rates take effect at 16:00 UTC on August 16, 2026. Peak hours are 01:00-04:00 and 06:00-10:00 UTC; all other hours are off-peak.*

For V4 Pro, the before-and-after comparison per million tokens is:

| Token category | Until Aug 16, 16:00 UTC | New off-peak | New peak | 
|---|---|---|---|
| Cache-hit input | $0.003625 | $0.022 | $0.044 | 
| Cache-miss input | $0.435 | $0.66 | $1.32 | 
| Output | $0.87 | $1.98 | $3.96 | 

The key correction is simple: **off-peak is not a 50% discount from the old price**. V4 Pro output becomes about **2.28×** the old rate off-peak and **4.55×** at peak. Cache-miss input becomes about **1.52×** off-peak and **3.03×** at peak.

DeepSeek’s full new schedule is:

| Model and window | Cache-hit input | Cache-miss input | Output | 
|---|---|---|---|
| V4 Flash off-peak | $0.007 | $0.22 | $0.66 | 
| V4 Flash peak | $0.014 | $0.44 | $1.32 | 
| V4 Pro off-peak | $0.022 | $0.66 | $1.98 | 
| V4 Pro peak | $0.044 | $1.32 | $3.96 | 

Scheduling batch evaluations, index refreshes, and non-urgent code maintenance outside peak hours now cuts the **new** rate in half. Interactive production traffic may not be movable, so update cost alerts before August 16. For repeated system prompts and repository context, the prompt-caching playbook remains the practical lever.

## Should you move an agent workflow to V4 Pro?

Test V4 Pro when a production task needs more capability than V4 Flash 0731 provides, especially for long terminal sessions, multi-repository work, and tool-heavy research. Keep Flash as the volume lane when it already clears your quality bar.

A useful evaluation plan is:

1. Run the same 30-50 real tasks on Flash at `high` and Pro at`high` .
2. Route failures to Pro at `max` instead of starting every request there.
3. Record cache-hit input, cache-miss input, output tokens, latency, and successful task completion.
4. Price the trace twice: once at off-peak and once at peak.
5. Keep the cheaper route only when the success-rate gap is operationally acceptable.

The launch gives Codex users a lower-friction provider path, but it does not prove that one benchmark score predicts your repository. The harness, tool schema, retry policy, and test oracle still determine whether an agent completes the work safely.

### Migration checklist before the price switch

Existing `deepseek-v4-pro` clients do not need a new model ID, but unchanged code can still produce a changed bill or behavior. Before August 16:

1. **Export a baseline trace.** Save task success, reasoning tokens, latency, cache-hit ratio and cost from representative pre-change requests.
2. **Pin effort explicitly.** Do not assume the provider default is the tier your workload previously approximated; compare`high` with`max` on failures.
3. **Audit ignored Responses fields.** Remove or flag unsupported built-in tools, storage and conversation parameters so silent compatibility gaps become visible.
4. **Set two cost alerts.** Budget interactive traffic at peak rates and movable jobs at off-peak rates instead of averaging them into one optimistic number.
5. **Retest state reconstruction.** Because the Responses endpoint is stateless, confirm your client resends every message and tool result needed after retries or restarts.
6. **Keep a fallback route.** Preserve the last known-good model and provider configuration until the new checkpoint clears your production acceptance suite.

This is operationally more important than changing the marketing name in a dashboard. Stable model IDs reduce migration work, but they also make checkpoint and pricing changes easier to miss in automated systems.

## Honest limitations

- The benchmark numbers are DeepSeek’s results, not an explainx.ai re-run.
- DeepSeek used its upcoming Harness minimal mode at max effort for public code-agent tasks; another harness can produce different results.
- DSBench-FullStack and DSBench-Hard are internal sets, so outsiders cannot independently reproduce them from the release materials alone.
- Native Responses API compatibility does not guarantee that every Codex feature behaves identically to OpenAI-hosted models.
- Peak/off-peak prices are scheduled rates. Check the live DeepSeek pricing page before budgeting production traffic.

## Related on explainx.ai

- Bolt Forge Adds DeepSeek V4 Pro With Up to 50x More Usage — the experimental Bolt.new lane where DeepSeek competes with GLM and Kimi under a larger, model-weighted allocation
- DeepSeek V4-Flash-Vision-Exp: multimodal agent benchmarks near Opus-4.8 — DeepSeek's August 21 experimental vision model
- DeepSeek V4's new pricing is live — does it really match GPT-5.6? — verified comparison against GPT-5.6 and Claude pricing now that the hike is in effect
- DeepSeek V4 preview: model IDs, 1M context, and migration
- DeepSeek V4 Pro benchmarks and API economics
- DeepSeek V4 Flash 0731 with Codex and Responses API
- DeepSeek Flash 0731 ARC-AGI cost-per-task analysis
- What is an agent harness?
- J-Space Cognition Suite: a community harness claims to unlock DeepSeek V4 Pro — unverified claim that a runtime-only fix pushes this same model's Terminal-Bench 2.1 score from 87.9 to 90.1
- Nvidia DeepSeek-V4-Pro-0813-NVFP4 on Hugging Face — November 2026 NVFP4 weight artifact for self-host inference
- Terminal-Bench: what the benchmark actually tests
- Prompt caching for LLM cost optimization
- Browse agent skills

**Primary sources:** DeepSeek’s August 13 change log · DeepSeek V4 model and pricing table · DeepSeek Codex integration guide · DeepSeek thinking-mode guide

*Features, benchmarks, configuration fields, and prices are accurate as of August 13, 2026. DeepSeek’s new prices are scheduled for August 16 at 16:00 UTC; verify the live documentation before changing a production route. Follow @explainx_ai for updates.*
