---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-guide-pricing-benchmarks-api-2026
title: "Gemini 4 Argon Guide: Pricing, Benchmarks, API (2026)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-30"]
keywords: ["google", "gemini", "argon", "output", "vertex", "tokens", "pricing", "october"]
source: https://codersera.com/blog/gemini-4-argon-complete-guide-2026/
source_anchor: ""
source_lines: [1, 229]
section: "Gemini_Google"
---
**Quick answer.**Gemini 4 Argon (

`gemini-4-argon`) is Google's first Gemini 4 model. It was announced on September 30, 2026 for Fairwind Program cyber defenders and became generally available on October 13, 2026 on the paid Gemini API and Vertex AI. It costs $2 input / $10 output per 1M tokens through November 30, 2026, then $4 / $20 from December 1. It has a 1,048,576-token unified context window, a default 65,536-token output cap per synchronous request, and no free tier. On Google's own table it leads on DeepSWE v1.1 (77.9%), AutomationBench (51.3%) and LVBench (91.7%). It trails Claude Opus 5.5 on Terminal-Bench 4.0 and GPT-6 Astra on FrontierSWE v2.
Google's flagship roadmap took a detour this year. Gemini 3.5 Pro, which Sundar Pichai had earlier said would ship in June, never arrived. Reuters reported on October 1 that Google no longer plans to release Gemini 3.5 Pro, attributing that to a company spokesperson. Argon is the replacement, and Google says it is larger than its previous "Pro" models.

The rollout came in two steps. At first, only trusted cyber defenders in Google's Fairwind Program had access. Two weeks later, on October 13, Argon reached general availability on the paid Gemini API, in Google AI Studio and on Vertex AI. It also arrived in the Gemini app and in Google Antigravity. For engineering teams, the questions are practical. What does it cost before and after the introductory price ends? What limits will you hit? Where does it beat Claude Opus 5.5 and GPT-6 Astra, and where doesn't it? How do you call it?

This guide answers those questions using Google's documentation, Google's launch posts, and independent data from Artificial Analysis and Vals AI. Pricing and availability are as of October 2026.

## Gemini 4 Argon release timeline

| Date (2026) | Event | 
|---|---|
| Sep 30 | Google DeepMind announces Gemini 4 Argon. Access is limited to trusted cyber defenders in the Fairwind Program, and Google says it is taking part in the U.S. government's voluntary pre-release access process. | 
| Oct 1 | Reuters reports, citing a Google spokesperson, that Google no longer plans to release Gemini 3.5 Pro. | 
| Oct 13 | The Gemini API changelog marks `gemini-4-argon` as generally available. Vertex AI lists the same GA date. Argon also arrives in the Gemini app for Google AI Pro and Ultra subscribers, and in Google Antigravity. | 
| Nov 30 | Introductory API pricing ends. | 
| Dec 1 | List pricing starts, at double the introductory rates. | 

If you followed our coverage of the expected Gemini 3.5 Pro launch, treat Argon as the model that took its place in Google's lineup. Below it, Gemini 3.8 Flash remains the cheaper, faster default for high-volume work.

## Specs at a glance

| Property | Gemini 4 Argon | 
|---|---|
| Gemini API / Vertex model ID | `gemini-4-argon` (stable) | 
| OpenRouter ID | `google/gemini-4-argon` | 
| Context window | 1,048,576 tokens, shared by input and output | 
| Output per request | 65,536 tokens by default for a synchronous request. Up to 1,048,576 with long decode, minus your input tokens. | 
| Inputs | Text, image, video, audio, PDF | 
| Output | Text only | 
| Thinking levels | `low` ,`medium` (default),`high` | 
| Caching | Implicit only. Explicit caching is not supported. | 
| Supported tools | Function calling, structured outputs, code execution, Google Search and Google Maps grounding, URL context, file search, computer use (preview) | 
| Not supported | Live API, image generation, audio generation, explicit caching. Vertex also lists tuning and RAG Engine as unsupported. | 
| Service tiers | Standard, Batch, Flex, Priority | 
| Vertex AI retirement date | October 13, 2027 or later | 

### The 1M window vs. the 65,536-token output cap

Google's launch post says it is expanding the output token limit "to an industry-leading 1M tokens, up from the previous 64K tokens." That is true, but the claim has conditions. The context window is a single 1,048,576-token budget, and everything you send and everything the model generates comes out of it. Vertex AI's model page says that output includes thinking tokens, function calls and the visible response. A normal synchronous request still stops at 65,536 output tokens. Vertex documents a lower limit of 32,768 for non-streaming direct HTTP calls. To go past 65,536 you need the long-decode path. On Vertex AI, it is documented for `GenerateContent` on `v1beta1` only. On the Gemini API, long decode uses background execution: set `background` to true and `max_output_tokens` above 65,536. The Gemini API thinking docs say that setting `max_output_tokens` above 65,536 with `background=False` returns a 400. A background request with no `max_output_tokens` set stops at 64K tokens. On either platform, the maximum output is 1,048,576 minus your input tokens.

In practice, a single call can't return a full rewrite of a large module. For whole-repository migrations, plan on the agent writing output in chunks, or design for long decode (background execution on the Gemini API) from the start.

## Gemini 4 Argon pricing: before and after December 1

Google's launch post lists an introductory price and a post-introductory price. The Gemini API pricing page gives exact dates: introductory rates apply through November 30, 2026, and list rates start December 1, 2026. Unlike some older Pro models, Argon has no higher price tier for long prompts. The same per-token rate applies whether you send 10K tokens or 900K.

| Tier (USD per 1M tokens) | Input to Nov 30 | Output* to Nov 30 | Cached to Nov 30 | Input from Dec 1 | Output* from Dec 1 | Cached from Dec 1 | 
|---|---|---|---|---|---|---|
| Standard | $2.00 | $10.00 | $0.10 | $4.00 | $20.00 | $0.20 | 
| Batch | $1.00 | $5.00 | $0.05 | $2.00 | $10.00 | $0.10 | 
| Flex | $1.00 | $5.00 | $0.05 | $2.00 | $10.00 | $0.10 | 
| Priority | $3.60 | $18.00 | $0.18 | $7.20 | $36.00 | $0.36 | 

**Output pricing includes thinking tokens. Source: Gemini API pricing page, as of October 2026.*

Other pricing details:

- **Cached input is 95% off the input price.** $0.10 is 5% of $2.00, and $0.20 is 5% of $4.00.
- **Grounding:** On the Standard and Priority tiers, Google Search grounding and Google Maps grounding each include 5,000 free requests per month, shared across Gemini 3 and newer models. After that, each costs $14 per 1,000. The pricing page lists no grounding rows for Batch or Flex, so keep grounded workloads on Standard or Priority.
- **Data use:** The pricing page says paid Standard, Batch, Flex and Priority traffic is not used to improve Google's products. An invite-only "Contributor" tier costs the same as Batch, but that traffic*is* used to improve Google's products.

### Worked example: one agentic coding task

Assume an agent run on a mid-sized repository uses 400,000 input tokens, of which 300,000 are implicit cache hits, and generates 40,000 output tokens, including thinking.

| Line item | Intro (to Nov 30) | List (from Dec 1) | 
|---|---|---|
| Uncached input: 100,000 tokens | 0.1M × $2.00 = $0.20 | 0.1M × $4.00 = $0.40 | 
| Cached input: 300,000 tokens | 0.3M × $0.10 = $0.03 | 0.3M × $0.20 = $0.06 | 
| Output: 40,000 tokens | 0.04M × $10.00 = $0.40 | 0.04M × $20.00 = $0.80 | 
| **Per task** | **$0.63** | **$1.26** | 
| **Per 1,000 tasks** | **$630** | **$1,260** | 

The same 1,000 tasks on the Batch or Flex tier at list price cost half as much: $630. If you have offline workloads, such as nightly code review, bulk refactoring or document extraction, moving them to Batch from December 1 keeps your cost at the introductory level. Output is the largest line item in both columns. Using the `high` thinking level by default can easily double output tokens, so measure before you change it globally.

## Access rules that catch teams out

### Paid tier only

The pricing page says it directly: "Gemini 4 Argon is not available on the Free tier." Google doesn't document what the failure looks like. In third-party testing by ComputingForGeeks, a key with no billing attached got an HTTP 429 quota error with a limit of 0, not a clear "model not available" message. Waiting and retrying won't fix that. Google's troubleshooting guide warns that a successful `models.list` call doesn't guarantee your project can run inference. It also says upgrading a project to the paid tier does not unlock free-tier quota. If CI or a staging environment uses a free key, expect it to break when you switch the model ID.

### Implicit caching only

You can't create a cache object and reference it, the way you can with explicit caching on older Gemini models. Discounts apply automatically when a request's prefix matches recent traffic. Vertex AI documents a 4,096-token minimum for implicit caching. To get the 95% discount, keep the stable parts of the prompt at the front: system instructions, tool schemas and repository context. Put the per-request content last. Our explainer on how prompt caching reduces AI cost covers the general pattern.

### Thinking levels: low, medium, high

Argon accepts three thinking levels: `low`, `medium` (the default) and `high`. On Vertex AI, Google documents that sending `thinking_budget` or `thinking_level="MINIMAL"` returns HTTP 400. The Gemini API thinking docs list only `thinking_level` for Argon and don't say how a `thinking_budget` value is handled there, so use `thinking_level` on both platforms. Only `low`, `medium` and `high` are documented. Don't pass effort values from other vendors' APIs, such as `max` or `xhigh`. If you route the same request across several vendors, map effort settings per provider.

### Sampling parameters are ignored

Google's troubleshooting page says Gemini 4 and Gemini 3.6+ models ignore custom `temperature`, `top_p` and `top_k` values. It also warns that requests that set them will return 400 errors in a future release. Remove these parameters from your Argon request builders now.

### Vertex AI regions

| Vertex AI consumption option | Regions | 
|---|---|
| Standard and Priority PayGo | `global` ,`us` ,`eu` | 
| Provisioned Throughput | `global` ,`us` ,`eu` ,`europe-west2` ,`asia-southeast1` | 
| ML processing | `us` ,`eu` ,`europe-west2` ,`asia-southeast1` | 

Vertex also lists batch inference. It does not support fixed quota. A separate allowlisted variant, `gemini-4-argon-cyber-permissive`, is available to Fairwind Program members only. It defaults to `HIGH` thinking, has no computer use and runs in `global` and `us` only. OpenRouter notes that EU in-region routing isn't currently available for Argon. If you have EU data-residency requirements, use Vertex `eu`.

## Benchmarks: where Argon leads and where it trails

Every number in the first table comes from Google. Google's launch post directly states DeepSWE v1.1, AutomationBench, LVBench and CWE-bench v1. The rest of the head-to-head rows come from Google DeepMind's evaluation table, as reproduced by CometAPI. Treat all of them as vendor-reported.

| Benchmark (Google-reported) | Gemini 4 Argon | Claude Opus 5.5 | GPT-6 Astra | 
|---|---|---|---|
| DeepSWE v1.1 (long-horizon SWE) | **77.9%** | 74.2% | 74.1% | 
| AutomationBench (Zapier) | **51.3%** | 42.5% | 41.4% | 
| LVBench (long video) | **91.7%** | 83.7% | 87.5% | 
| GraphWalks, 256K–1M (long context) | **84.2%** | 66.8% | 71.8% | 
| Vals Index (Google's table) | **68.9%** | 67.0% | 63.1% | 
| CWE-bench v1 (cyber) | 68.0% (tie) | 67.0% | 68.0% (tie) | 
| Terminal-Bench 4.0 | 57.4% | **66.4%** | 58.2% | 
| FrontierSWE v2 | 55.0% | 62.3% | **65.5%** | 
| Terminal-Bench Science 0.1 | 57.6% | 63.3% | **68.1%** | 

The pattern is consistent. Argon is strongest on long-horizon repository work (DeepSWE), multi-app business automation (AutomationBench), and very long inputs: video, and graph traversal past 256K tokens. It is weaker on terminal-driven agent loops. Google's own table puts Terminal-Bench 4.0 at 57.4% for Argon vs. 66.4% for Opus 5.5, and FrontierSWE v2 at 55.0% vs. 65.5% for GPT-6 Astra. If your agents mostly run shell commands in tight loops, Argon doesn't win in Google's own reporting.

### Independent numbers

| Source (independent) | Metric | Gemini 4 Argon (as of October 26, 2026) | 
|---|---|---|
| Artificial Analysis (Argon, High) | Intelligence Index | 53 (#9 of 221) | 
| Artificial Analysis | Cost per Intelligence Index task | $1.62 | 
| Artificial Analysis | Output tokens used to run the index | 110M (median 81M) | 
| Artificial Analysis | Output speed / time to first token (Google API) | 79.7 tokens/s / 48.23 s (median TTFT across models: 3.90 s) | 
| Vals AI | Terminal-Bench 4.0 | 57.58% (#7 of 55 on the model page; the Sep 30 update note lists #5 of 42) | 
| Vals AI | Vals Index | 69.24% (#3 of 27) on the model page. Vals's Sep 30 update note lists 68.90%, #1 of 41. | 

Vals's Terminal-Bench 4.0 run (57.58%) is close to Google's 57.4%, which suggests Google's figure for its weaker results is accurate. The Artificial Analysis data shows two operational costs. Argon uses more output tokens than the median model to complete the index. And at high thinking, its time to first token on Google's API was about 48 seconds, vs. a ~3.9-second median across models (Artificial Analysis, as of October 26, 2026). That is fine for background agents but too slow for interactive autocomplete or chat UIs. Vals also reports Claude Opus 5.5 at 66.97% on its index, below Argon.

For the full Argon vs. Opus 5.5 vs. GPT-6 Astra comparison, including pricing per task and coding-agent results, see our frontier model comparison. For the models themselves, see the Claude Opus 5.5 guide and the GPT-6 Astra guide.

## Python quickstart

Google's current Gemini API docs use the Interactions API in the `google-genai` SDK. The example below follows the documented pattern, with the model ID changed to `gemini-4-argon`. It needs a paid-tier key in `GEMINI_API_KEY`.

```
# pip install google-genai
from google import genai
client = genai.Client()  # reads GEMINI_API_KEY from the environment
interaction = client.interactions.create(
    model="gemini-4-argon",
    system_instruction="You are a senior engineer reviewing a migration plan.",
    input="List the top five risks of porting a 200K-line C++ service to Rust.",
    generation_config={
        "thinking_level": "high"  # low | medium (default) | high
    }
)
print(interaction.output_text)
print("Thought tokens:", interaction.usage.total_thought_tokens)
print("Output tokens:", interaction.usage.total_output_tokens)
```
Notes for production:

- Leave out `temperature` ,`top_p` and`top_k` . They are ignored now and will return errors later.
- Thought tokens are billed as output. Log `total_thought_tokens` per request so you can see how much`high` costs compared with the default`medium` .
- Google's troubleshooting guide treats `429 RESOURCE_EXHAUSTED` as retryable with exponential backoff. If every call returns 429, check whether the project is on the free tier before you tune your retries.

If you already route traffic through OpenRouter, the model is available at `google/gemini-4-argon` on the OpenAI-compatible endpoint:

```
# pip install openai
import os
from openai import OpenAI
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=os.environ["OPENROUTER_API_KEY"],
)
resp = client.chat.completions.create(
    model="google/gemini-4-argon",
    messages=[{"role": "user", "content": "Summarize the tradeoffs of unsafe Rust in FFI layers."}],
)
print(resp.choices[0].message.content)
```
## Where you can use Argon: Gemini app, Antigravity, and not Gemini CLI

- **Gemini API and AI Studio:** Paid tier only. Google's developer post also mentions Android Studio.
- **Vertex AI:** GA since October 13, in the regions listed above.
- **Gemini app:** Google AI Pro and Ultra subscribers, since October 13.
- **Google Antigravity:** Google's Antigravity post says Argon is available in the Antigravity desktop app and the Antigravity CLI for Google AI Pro and Ultra subscribers, and on "eligible Gemini Enterprise subscriptions."
- **Gemini CLI:** Not supported. The Gemini CLI README says: "Gemini CLI has not been optimized for Gemini 4 Argon. To use Gemini 4 Argon, please use Antigravity." It also notes that Gemini CLI was replaced by Antigravity CLI on June 18, 2026 for unpaid-tier and Google One users. If your team's workflows are built on the tooling described in our Gemini CLI guide, plan the move to Antigravity CLI before you standardize on Argon.

## Who should use Gemini 4 Argon

- **Good fit:** long-running repository migrations and refactors, analysis of large codebases or document sets in a single context, long-video understanding, cross-app business automation, and defensive security review. Google says its internal teams are using Argon for C/C++-to-Rust migrations of re2, libgav1 and the Fuchsia Zircon kernel (800K+ lines). In the libgav1 port, Argon replaced 32K lines of SIMD code and produced a decoder 2.7x faster than the earlier Rust port, with identical output.
- **Weaker fit:** latency-sensitive interactive features, given the reported ~48-second time to first token at high thinking. Also terminal-heavy agent loops, where Opus 5.5 leads on Terminal-Bench 4.0. And high-volume simple tasks, where Gemini 3.8 Flash is much cheaper.
- **Budget note:** If you start a pilot in November, model costs on December 1 list prices, not the introductory rate.

One detail in Google's migration story matters for buyers. Google says such large-scale rewrites "are undergoing rigorous automated and manual auditing, emulation testing, and review before rolling out to production." Even at Google's scale, agents write the code and engineers sign off on it.

## FAQ

### What is the Gemini 4 Argon model ID?

`gemini-4-argon` on the Gemini API and Vertex AI. It is a stable model, not a preview. On OpenRouter it is `google/gemini-4-argon`.

### When was Gemini 4 released?

Google announced Gemini 4 Argon on September 30, 2026, for Fairwind Program cyber defenders. It became generally available on the paid Gemini API and Vertex AI on October 13, 2026.

### How much does Gemini 4 Argon cost?

Through November 30, 2026: $2 input, $10 output and $0.10 cached input per 1M tokens. From December 1, 2026: $4, $20 and $0.20. Batch and Flex cost half the Standard rate, and Priority costs 1.8x the Standard rate. Prices are as of October 2026.

### Is there a Gemini 4 Argon free tier?

No. Google's pricing page says Argon is not available on the Free tier. In third-party testing, a key without billing got a 429 quota error with a limit of 0. You need a project with billing enabled.

### What is the maximum output length?

By default, 65,536 output tokens per synchronous request. With long decode, output can reach up to the 1,048,576-token window minus your input tokens. On the Gemini API, that means a background request with `max_output_tokens` set above 65,536.

### Does Gemini 4 Argon support explicit context caching?

No. Only implicit caching is supported. Cache hits are billed at 95% off the input price, and Vertex documents a 4,096-token minimum.

### What happened to Gemini 3.5 Pro?

Reuters reported on October 1, citing a Google spokesperson, that Google no longer plans to release Gemini 3.5 Pro. Argon fills that flagship slot.

## Sources

Agents can now take on large legacy and C/C++-to-Rust migrations, but someone still has to review, test and own that code. Codersera can help you hire vetted remote AI developers to review, test and own agent-generated code, matched for technical fit, with a risk-free trial.
