---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-guide-pricing-benchmarks-open-weights
title: "Kimi K3.1 Pro Guide: Pricing, Benchmarks, Open Weights"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "moonshot", "october", "input", "model", "kimi-k3", "tokens", "weights"]
source: https://codersera.com/blog/kimi-k3-1-pro-complete-guide-2026/amp/
source_anchor: ""
source_lines: [1, 252]
section: "Kimi"
---
**Quick answer.**Kimi K3.1 Pro (

`kimi-k3.1-pro`) is Moonshot AI's new flagship, released October 23, 2026. As of October 2026 the Kimi API charges $2.00 per million input tokens, $10.00 per million output tokens and $0.20 for cached input. It has a 1M-token context window and always reasons; thinking cannot be turned off. It scores 55 on the Artificial Analysis Intelligence Index at $2.11 per task, against 58 and $5.98 for Claude Opus 5.5. Moonshot has promised the full weights by November 4, 2026, but they had not been released as of October 26.
Moonshot AI shipped Kimi K3.1 Pro on October 23, 2026 (the release date Artificial Analysis lists), about three months after Kimi K3 (July 2026). It is a post-trained version of K3, not a new base model. Moonshot says it spent substantially more post-training compute on it, running joint reinforcement learning across hundreds of downstream domains. The result is a model that sits near the top of independent leaderboards, costs a third less than K3 per token, and lists for roughly half of Claude Opus 5.5's per-token price.

Two things make this release harder to evaluate than a normal model launch. First, the "open-weight" label is still a promise. Moonshot's launch post says the weights will be released by November 4, but on the date of writing nothing is downloadable. Second, a set of unverified paid-influencer allegations surfaced around the launch. Neither changes what the API does today, but both are reasons to lean on independent measurements rather than launch-week social posts.

This guide covers the specs and price table (with worked cost examples), the API quirks that break code written for K2, the benchmarks (vendor-reported and independent, labelled separately), a short comparison with Claude Opus 5.5, the open-weights question including China's draft export controls, and what K2 users need to do before the year-end retirement. If you are new to the K3 family, start with our Kimi K3 complete guide.

## Kimi K3.1 Pro at a glance

| Spec | Kimi K3.1 Pro | 
|---|---|
| Developer | Moonshot AI | 
| Released | October 23, 2026 (per Artificial Analysis) | 
| API model IDs | `kimi-k3.1-pro` ,`kimi-k3.1-pro-highspeed` | 
| Lineage | Post-trained version of Kimi K3 | 
| Parameters | 2.8T total (Artificial Analysis; Moonshot calls it "3T-class"). The Kimi K3 model card lists 2.8T total and 104B activated parameters in a Mixture-of-Experts design. Moonshot has not published a separate architecture table for K3.1 Pro. | 
| Context window | 1,048,576 tokens (1M) | 
| Max output | `max_completion_tokens` defaults to 131,072 and can be set up to 1,048,576 (minus prompt tokens) | 
| Modalities | Text, image and video input; text output | 
| Reasoning | Always on. `reasoning_effort` :`low` ,`high` ,`max` (default`max` ) | 
| Highspeed variant | Equivalent intelligence at about 2-3x the speed, according to Moonshot | 
| Open weights | Promised by November 4, 2026; not released as of October 26 | 
| API access | Unlocked after a top-up of at least $1 on platform.kimi.ai | 

K3.1 Pro is also available in the Kimi web and mobile apps, the Kimi Work desktop app (version 3.1.0 or later) and Kimi Code, where you pick it with the `/model` command. For Kimi Enterprise customers it is off by default, and an administrator has to enable it.

## Pricing: K3.1 Pro vs Highspeed vs K3

These are the list prices on Moonshot's Kimi API pricing page as of October 2026, in USD per million tokens. Prices exclude taxes and may change.

| Model | Input | Output | Cached input | 5-minute cache write | 1-hour cache write | 
|---|---|---|---|---|---|
| `kimi-k3.1-pro` | $2.00 | $10.00 | $0.20 | $2.00 | $4.00 | 
| `kimi-k3.1-pro-highspeed` | $4.00 | $20.00 | $0.20 | $4.00 | $8.00 | 
| `kimi-k3` | $3.00 | $15.00 | $0.30 | $3.00 | $6.00 | 

Points to note:

- **K3.1 Pro is cheaper than K3.** Input, output and cached input are all about a third lower. If you are on K3 today, moving to K3.1 Pro lowers your per-token bill.
- **Highspeed costs double.** Input, output and cache writes are all 2x the standard model's rates. Cached input is $0.20 on both, so the gap is smaller for workloads that are mostly cache hits.
- **Cache writes are a separate line item.** Caching is automatic, with a default 5-minute TTL. Writing a prefix into the cache is billed at the write rate for the TTL you choose. A 5-minute write costs the same as uncached input, and a 1-hour write costs double. Each cache hit refreshes the entry's TTL. You set the TTL with`prompt_cache_options.ttl` (`5m` or`1h` ).
- **No context-length tiers.** The quickstart says pricing is flat across the full 1M window.
- **Partial blocks miss the cache.** Caching works in blocks. A trailing partial block is billed as a cache miss.
- **Changing reasoning effort mid-session invalidates prefix-cache hits** , according to Moonshot's parameter reference. Pick a level before the conversation starts and keep it.

For a deeper look at K3-family cost planning, see our Kimi K3 pricing and API costs breakdown and our explainer on how prompt caching reduces AI cost.

### Worked example: an agent workload

Assume 1,000 requests per day. Each request sends 50,000 input tokens, of which 40,000 are a warm cached prefix (system prompt, tool definitions, repository context) and 10,000 are new. Each request returns 5,000 output tokens, counting reasoning. Cache-write costs are left out here for simplicity and covered below.

| Cost line | K3.1 Pro | K3.1 Pro Highspeed | K3 | 
|---|---|---|---|
| Cached input: 40,000 tokens | 40,000 x $0.20/1M = $0.008 | 40,000 x $0.20/1M = $0.008 | 40,000 x $0.30/1M = $0.012 | 
| Uncached input: 10,000 tokens | 10,000 x $2/1M = $0.020 | 10,000 x $4/1M = $0.040 | 10,000 x $3/1M = $0.030 | 
| Output: 5,000 tokens | 5,000 x $10/1M = $0.050 | 5,000 x $20/1M = $0.100 | 5,000 x $15/1M = $0.075 | 
| **Per request** | **$0.078** | **$0.148** | **$0.117** | 
| **Per day (x1,000)** | **$78** | **$148** | **$117** | 

Output dominates in every column. With the cached prefix already warm, output is 64% of the K3.1 Pro bill ($0.050 of $0.078). That matters because K3.1 Pro is a verbose reasoner. Artificial Analysis recorded 150M output tokens on its Intelligence Index run, against a median of 87M. In practice, `reasoning_effort` is the strongest cost lever you have. Test `low` and `high` before defaulting to `max`.

Cache writes add a cost each time the prefix is written. Writing the 40,000-token prefix once costs 40,000 x $2/1M = $0.08 with the 5-minute TTL, or 40,000 x $4/1M = $0.16 with the 1-hour TTL. With steady traffic, every hit refreshes the 5-minute TTL and the write is rare. Pay for the 1-hour tier only if requests that share the prefix are regularly more than five minutes apart.

## API quirks that break K2-era code

The Kimi API is OpenAI-compatible. The quickstart uses the OpenAI Python SDK with base URL `https://api.moonshot.ai/v1` and an API key from platform.kimi.ai. An Anthropic Messages-compatible endpoint is also documented. Swapping only the model string is not enough, though. K3.1 Pro has the same parameter constraints as K3, and they differ from K2.6.

### Thinking is always on

K3.1 Pro always reasons, with Preserved Thinking always on. There is no non-thinking mode. Moonshot's FAQ answers "How do I turn off Kimi K3.1 Pro's chain-of-thought?" with "You can't." The K2.x `thinking` object (including `{"type": "disabled"}`) is not a K3-family parameter, and the migration guide says to remove it. To reduce latency, set `reasoning_effort` to `low`.

### Fixed sampling parameters

These values are fixed: `temperature=1.0`, `top_p=0.95`, `n=1`, `presence_penalty=0` and `frequency_penalty=0`. Moonshot's parameter reference says "passing any other value returns an error." Many shared LLM wrappers send `temperature=0` by default, and those requests will fail. Strip these fields from your requests rather than setting them.

### Return the full assistant message

In multi-turn conversations and tool loops, append the complete assistant message the API returned, including `reasoning_content`. Do not keep only `content`. Streaming returns separate `reasoning_content` and `content` deltas. For structured output (`response_format` with `json_schema` and `strict: true`), parse only `message.content`.

### Other constraints

- `tool_choice` accepts`auto` ,`none` and`required` .`required` was not supported on K2.6 and K2.7 Code.
- Vision input does not accept public image URLs. Use base64 data URLs or `ms://<file-id>` , and send`content` as an array of objects.
- Moonshot says its built-in web search is being updated and is not recommended for production for now.
- Rate limits (concurrency, RPM, TPM, TPD) depend on your cumulative top-up tier.
- In Claude Code, the troubleshooting guide says to use the compatible alias `kimi-k3[1m]` . Direct API calls and Codex use`kimi-k3.1-pro` .

### Minimal call

This example follows Moonshot's quickstart. Note that it sends no `temperature`, `top_p` or `thinking` field.

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["MOONSHOT_API_KEY"],
    base_url="https://api.moonshot.ai/v1",
)
stream = client.chat.completions.create(
    model="kimi-k3.1-pro",
    reasoning_effort="low",
    messages=[{"role": "user", "content": "Explain why the sky is blue."}],
    stream=True,
)
for chunk in stream:
    delta = chunk.choices[0].delta
    reasoning = getattr(delta, "reasoning_content", None)
    if reasoning:
        print(reasoning, end="", flush=True)
    if delta.content:
        print(delta.content, end="", flush=True)
```
### Platform and plan notes

The global developer platform is **platform.kimi.ai**, which hosts the docs, console and API keys. Moonshot's troubleshooting guide says users outside mainland China should use platform.kimi.ai, and that accounts and keys from different regional Kimi platforms are separate and cannot be mixed. If an older key returns authentication errors, check that its region matches the endpoint.

The API is pay-as-you-go and separate from Kimi membership and **Kimi Code** subscriptions. A Kimi Code plan does not give you API credit, and API credit does not unlock Kimi Code plan features. Per the Kimi Code docs, K3.1 Pro is available on Moderato / Plus plans and above, while the 1M context window and K3.1 Pro Highspeed require Allegretto / Pro and above. Kimi Code's `/login` command signs you in to the Kimi Code platform under your membership plan, and Moonshot's troubleshooting guide says Kimi Code keys and Open Platform keys are not interchangeable. Check the current plan table before you buy seats for a team.

## Benchmarks: what Moonshot reports and what independent tests show

Benchmark numbers for this release come from two places, and they should not be mixed. The tables below label each source.

### Moonshot-reported results

All figures below come from the benchmark data published with Moonshot's launch post, with every model at max effort. Moonshot ran K3.1 Pro itself and took some competitor scores from official leaderboards or system cards, and the harnesses differ by benchmark, so these are not controlled head-to-head trials.

| Benchmark (Moonshot-reported) | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | 
|---|---|---|---|
| Terminal-Bench 4.0 | 65.7% | 66.4% | 58.2% | 
| SWE-Marathon v1.1 | 51.7 | 46.3 | 42.5 | 
| Agents' Last Exam (ALL) | 43.2 | 42.6 | 41.8 | 
| OfficeQA Pro v2 | 64.4 | 62.2 | 54.4 | 
| BenchCAD, Voxel IoU, with tools | 92.7 | 96.2 | 95.9 | 
| Terminal-Bench-Science 0.1 | 38.1 | 63.3 | 68.1 | 

Moonshot also reports internal results: 61.2% on Kimi Code Bench 3.0 Long-horizon (up from 55.95% for K3), and gains on internal finance evaluations. On factuality, Moonshot says the claim-level hallucination rate on its internal Kimi Hallu Bench Hard (run with tools) fell from 23.4% for K3 to 8.3% for K3.1 Pro, and the share of responses with at least one factual error was 42.4% of K3's rate. VentureBeat reports the same 23.4% and 8.3% figures. Moonshot describes the benchmark as a stress test built from challenging, de-identified prompts drawn from practical work tasks and deliberately crafted to probe situations where models are prone to errors, and says "the reported rates do not represent overall error rates in daily use."

The vendor numbers tell a consistent story. K3.1 Pro roughly matches Opus 5.5 on Terminal-Bench 4.0, leads on long-horizon software and office tasks in Moonshot's own runs, and trails clearly on scientific terminal work (Terminal-Bench Science 0.1) and CAD with tools.

### Independent results (Artificial Analysis)

| Metric (Artificial Analysis) | Kimi K3.1 Pro | Claude Opus 5.5 | 
|---|---|---|
| Intelligence Index | 55 (#4 when we checked on October 26) | 58 (#1) | 
| Cost per Intelligence Index task | $2.11 | $5.98 | 
| Output speed | 52.2 tokens/s | 96.8 tokens/s | 
| List price (input / output per 1M) | $2.00 / $10.00 | $4.00 / $20.00 | 
| Open weights | Listed as not public | No | 

VentureBeat also reported several of Artificial Analysis's per-benchmark results:

- **Terminal-Bench 4.0:** 67%, up from 13% for K3. This is close to Moonshot's own 65.7%.
- **GDPval-AA:** 1,846 Elo, third behind Opus 5.5 (1,866) and Wonder Full (1,857).
- **K3.1 Pro ahead of Opus 5.5:** AutomationBench-AA (73% vs 70%).
- **K3.1 Pro behind Opus 5.5:** Humanity's Last Exam (52% vs 61%) and CritPt (25% vs 32%).

Leaderboard positions change as models are added, so treat the rank as a snapshot. For how the K3 family compares more broadly, see our Kimi K3 benchmarks comparison.

**A note on launch-week sentiment.** VentureBeat reported that an X user posted screenshots of what he described as a brief for a paid Kimi promotional campaign. The brief allegedly told creators not to use the paid-partnership label and to frame Kimi as an open-source equivalent of Opus 5.5. The document has not been authenticated. It is unclear who wrote it or whether any paid posts were published, and VentureBeat had found no Moonshot response as of October 25. Whatever the truth, it is a good reason to base decisions on third-party measurements and your own evals, not on social media threads.

## Kimi K3.1 Pro vs Claude Opus 5.5

On Artificial Analysis's measurements, Opus 5.5 is the stronger model by three index points, and it produces output nearly twice as fast. K3.1 Pro costs $2.11 per index task against $5.98 for Opus 5.5. That is about 35% of Opus's cost ($2.11 / $5.98 = 0.353), so Opus costs about 2.8x more per task ($5.98 / $2.11 = 2.83). Per-task cost is a more honest comparison than list price, because it reflects how many tokens each model actually uses.

| If you need... | Lean toward | Why | 
|---|---|---|
| The highest measured capability, scientific reasoning, CAD | Claude Opus 5.5 | Higher Intelligence Index; leads on HLE, CritPt, Terminal-Bench Science and BenchCAD | 
| Interactive latency | Claude Opus 5.5, or K3.1 Pro Highspeed | Standard K3.1 Pro measures 52 tokens/s; Highspeed doubles the price for a claimed 2-3x speedup | 
| High-volume agent runs and office/automation workflows on a budget | Kimi K3.1 Pro | About a third of the cost per task; competitive on Terminal-Bench 4.0 and AutomationBench-AA | 
| A path to self-hosting or data-residency control | Kimi K3.1 Pro (once weights ship) | Opus is API-only; K3.1 Pro's weights are promised but not yet released | 

For full Opus details, see our Claude Opus 5.5 complete guide. For the previous generation of this matchup, see Kimi K3 vs Claude Opus 5.

## The open-weights question

As of October 26, 2026, the position is this:

- **Moonshot** says in its launch post and quickstart that the full weights will be released by November 4, 2026. VentureBeat reports that Moonshot said they will ship under an open license. Moonshot has not named the license.
- **Hugging Face** has no K3.1 Pro repository. The newest model under the`moonshotai` organization is Kimi-K3, released under the custom Kimi K3 License.
- **Artificial Analysis** currently lists K3.1 Pro's weights as not publicly available.

The weights are promised, not released. Until they are, treat K3.1 Pro as an API-only model for planning purposes. We will update this section once the November 4 date passes.

### What Moonshot changed for the open-weight release

Moonshot says it removed cyber-related data from both mid-training and RL for the open-weight release, while keeping general-purpose coding tasks. Its in-house exploit benchmark, xdev-bench-hard (pass@3), shows the effect. A checkpoint trained without those removals scored 90.5%. The released K3.1 Pro scored 81.0%. K3 scored 33.3%. Moonshot's stated principle is not to release open weights that extend the absolute frontier of cyber capability. Security teams evaluating the model for defensive work should expect deliberately reduced offensive capability.

### China's draft export controls

On October 16, 2026, The Decoder reported a draft from China's commerce and science ministries that would control 23 AI technologies. For large models, the controls apply once a model meets any one of three thresholds: 10^26 FLOPs of training compute, at least 2 trillion parameters, or a global top-10 ranking on public leaderboards. Controlled items include weights, checkpoints, source code and architecture.

K3.1 Pro has 2.8T parameters and ranks in Artificial Analysis's top five, so it appears to meet at least two of those thresholds. The draft is not a ban on open releases. It exempts technology that is already public, and it offers fast-track "automatic licensing" for publishing restricted technology as open source. It does add a registration step and gives Beijing discretion. Dean W. Ball, quoted by The Decoder, said the framework establishes "the optionality to shut down future open-weight models at will." We do not know how or whether it will affect Moonshot's November 4 date, but it is a real risk to the timeline. If your roadmap depends on self-hosting K3.1 Pro, plan around K3 (already open) until the weights are actually published.

### If and when you self-host

Kimi K3's model card is the best guide to what K3.1 Pro's weights will likely look like. It lists 2.8T total and 104B activated parameters, MXFP4 weights with MXFP8 activations from quantization-aware training, and deployment recipes for vLLM and SGLang. A model this size needs multi-GPU inference infrastructure, plus engineers who can manage quantization, serving throughput and evaluation (Codersera can help you hire vetted AI developers with LLM serving experience). Read the license terms when they are published before you build a product on them. For the wider open-model landscape, see our best open-source LLMs of 2026 roundup.

## K2 deprecation: what K2 users need to do

Moonshot's pricing and model-list pages carry this notice. Starting **October 31, 2026**, K2 series models will no longer be available to newly registered API users. Existing users are not affected for now. Moonshot plans to **retire all K2 series models by the end of December 2026**. The K2 models still listed are `kimi-k2.7-code`, `kimi-k2.7-code-highspeed` and `kimi-k2.6`, all with a 262,144-token context window.

| Model (as of October 2026) | Input | Output | Cached input | 
|---|---|---|---|
| `kimi-k2.7-code` | $0.95 | $4.00 | $0.19 | 
| `kimi-k2.7-code-highspeed` | $1.90 | $8.00 | $0.38 | 
| `kimi-k2.6` | $0.95 | $4.00 | $0.16 | 

Moving from K2.7 Code to K3.1 Pro raises input pricing about 2.1x ($0.95 to $2.00, since 2.00 / 0.95 = 2.11) and output pricing 2.5x ($4.00 to $10.00). Because output dominates agent bills, expect your total to rise by roughly 2-2.5x. Re-run your cost model with your real token counts before the cutover, not after.

Migration checklist:

1. Change `model` to`kimi-k3.1-pro` (or`-highspeed` ).
2. Remove the `thinking` object, and stop relying on K2.6's non-thinking mode.
3. Remove `temperature` ,`top_p` ,`n` and the penalty fields from every request path, including shared wrappers.
4. Pass the complete assistant message, including `reasoning_content` , back into history in multi-turn and tool flows.
5. Choose a `reasoning_effort` level per workload and keep it fixed within a session so you keep cache hits.
6. Revisit `max_completion_tokens` and timeouts. The default is 131,072 output tokens, and the model always reasons.
7. Check your account tier's rate limits before moving production traffic.

Background on the model you are leaving is in our Kimi K2.7 complete guide.

## FAQ

### What is the Kimi K3.1 Pro API model ID?

`kimi-k3.1-pro` for the standard model and `kimi-k3.1-pro-highspeed` for the faster variant. The quickstart's base URL is `https://api.moonshot.ai/v1`, used with an OpenAI-compatible client.

### How much does Kimi K3.1 Pro cost?

As of October 2026: $2.00 per million input tokens, $10.00 per million output tokens, $0.20 per million cached input tokens, and $2.00 or $4.00 per million for 5-minute or 1-hour cache writes. Highspeed is $4.00 / $20.00 / $0.20.

### Are the Kimi K3.1 Pro weights available?

Not as of October 26, 2026. Moonshot has promised full weights by November 4, 2026. There is no Hugging Face repo yet, and Artificial Analysis lists the weights as not public.

### Can I disable thinking on Kimi K3.1 Pro?

No. K3.1 Pro always reasons. Use `reasoning_effort="low"` to cut latency and output tokens. Do not send the K2-style `thinking` parameter, and do not send non-default sampling values. Moonshot says fixed parameters return an error if changed.

### Is Kimi K3.1 Pro better than Claude Opus 5.5?

Not on Artificial Analysis's index, where Opus 5.5 scores 58 to K3.1 Pro's 55. K3.1 Pro is much cheaper per task ($2.11 vs $5.98) and close on Terminal-Bench 4.0. Moonshot reports 65.7% vs 66.4%, and AA measured 67% for K3.1 Pro.

### When do K2 models stop working?

New API users lose access to K2 from October 31, 2026. Moonshot plans to retire all K2 models by the end of December 2026.

## Sources

If your team wants to evaluate K3.1 Pro against closed models, or self-host open-weight models like the K3 family on your own infrastructure, Codersera can help you hire vetted remote AI developers with hands-on LLM serving and evaluation experience.
