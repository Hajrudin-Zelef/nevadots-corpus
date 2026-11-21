---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-specs-and-search-api
title: "DeepSeek V4.1 Flash 1017: Specs and Search API"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-25"]
keywords: ["flash", "flash-1017", "deepseek", "search", "october", "release", "model", "deepseek-flash"]
source: https://www.superbash.ai/articles/deepseek-v4-1-flash-1017-search-api/
source_anchor: ""
source_lines: [1, 155]
section: "DeepSeek"
---
**Bottom line**

Flash-1017 upgrades deepseek-flash with unchanged list prices and MIT weights. DeepSeek claims shorter, faster agent runs; Superbash has not measured accepted-task cost. Meter Search and Fetch, including the server-side tool promotion ending October 31 at 16:00 UTC.

**Sources checked October 25, 2026.** This is a documentation-backed release analysis with no Superbash measured run. Specifications and prices come from DeepSeekâs documentation; benchmarks and shorter-output claims are vendor-cited. The suggested comparison below is editorial advice, not an API test report.

Flash-1017 changes what the existing `deepseek-flash` alias serves. It does not require a new model ID or introduce a list-price cut. The practical work is to record the served version and meter token usage alongside Search and Fetch, especially as the server-side tool promotion approaches its deadline.

## What the October 17 DeepSeek reveal includes

DeepSeek describes **DeepSeek-V4.1-Flash-1017** as a minor update to the September V4.1-Flash release. The October 17 announcement claims stronger benchmark performance, faster task completion, and shorter outputs, and makes Search and Fetch APIs available to all users.[1][2]

Keep the release history straight: the earlier DeepSeek Vision Exp release was the August multimodal experiment; September brought V4.1-Flash; October 17 brings Flash-1017. V4 Pro is a separate product, not this updateâs new name.[1][4]

The launch says the new model is live on the website, app, and API and links published weights. Those are availability statements, not evidence that our own agent has completed a job with it.[2]

## Drop-in API reality

Continue using **`deepseek-flash`**. The pricing page currently maps it to DeepSeek-V4.1-Flash-1017, while `deepseek-v4-pro` maps to V4-Pro-0813. The Flash alias serves the latest version, so pinning the alias alone does not freeze the underlying model. Record the served version where exposed, the documentation date, and the run date.[4]

Legacy `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` names are still accepted, but their original models are retired and requests are served by Flash-1017 at Flash prices. An old configuration name therefore does not prove that you are evaluating the historical model.[4]

| Hosted Flash property | Documented behavior | 
|---|---|
| API ID | `deepseek-flash` | 
| Current version | DeepSeek-V4.1-Flash-1017 | 
| Context | 1M tokens | 
| Maximum output | 384K tokens | 
| Thinking | Thinking by default; non-thinking supported | 
| Inputs and integrations | Vision, tool calls, JSON, Responses API, Anthropic-compatible API | 

These are documented capabilities; Superbash has not verified them through a successful request here. DeepSeek recommends **Harness 0.2.1-rc.1 or later** for this release and names Harness, Tencent CodeBuddy, and WorkBuddy among its optimized integrations.[2][4]

The local model card recommends temperature 1.0, top_p 1.0, a 1M context, and max_tokens of at least 256K. Treat that as a local evaluation recipe. It does not replace the hosted APIâs 384K maximum or mean every task needs a huge output budget.[3][4]

## Before/after benchmark table

The table below reproduces the vendorâs September Flash versus Flash-1017 comparison. Changes are absolute score points, or percentage points where the benchmark reports percentages; they are not relative percentage gains.[1][3]

| Benchmark | September Flash | Flash-1017 | Change, points | 
|---|---|---|---|
| HLE without tools | 36.8 | 42.6 | +5.8 | 
| Terminal-Bench 4.0 | 31.2 | 42.3 | +11.1 | 
| DeepSWE v1.1 | 74.2 | 77.0 | +2.8 | 
| ProgramBench | 20.3 | 29.1 | +8.8 | 
| NL2Repo-Bench | 65.4 | 68.2 | +2.8 | 
| Automation-Bench v1.0.6 | 54.8 | 60.5 | +5.7 | 
| Agentsâ Last Exam | 31.8 | 35.4 | +3.6 | 
| Apex Bench v1.1 | 64.6 | 72.1 | +7.5 | 
| Chartography with tools | 78.9 | 83.0 | +4.1 | 
| BabyVision with tools | 89.6 | 92.5 | +2.9 | 
| ZeroBench-main with tools | 49.0 | 51.0 | +2.0 | 

The main HLE without-tools result is **36.8 to 42.6**. The card separately gives a text-only subset of 39.1 to 44.0; substituting that subset would change the comparison. Terminal-Bench uses Claude Code, while DeepSWE uses Mini-SWE-V2. The chart shows five selected pairs; the adjacent table preserves the wider set.[1][3]

These gains justify considering a bounded trial. They do not establish how many corrections your agent needs or whether its total bill falls. A leaderboard score and a vendor cost-per-task plot cannot supply an unrun Superbash result.

## Cost structure

Flash-1017 keeps the existing Flash list prices. All rates below are **USD per million tokens**; cache hits, cache misses, and output have different prices.[1][4]

| Flash token category | Off-peak | Peak | 
|---|---|---|
| Input, cache hit | $0.003 | $0.006 | 
| Input, cache miss | $0.15 | $0.30 | 
| Output | $0.60 | $1.20 | 

Peak hours are **MondayâFriday 01:00â04:00 and 06:00â10:00 UTC**, excluding Chinese public holidays. All other times are off-peak, including weekends and those holidays. The table gives exact cache-hit values because their tiny bars should not be read as free usage.[4]

For context, the separate Pro SKU has these rates; this is a price comparison, not an assertion of equal capability:[4]

| Pro token category | Off-peak | Peak | 
|---|---|---|
| Input, cache hit | $0.022 | $0.044 | 
| Input, cache miss | $0.66 | $1.32 | 
| Output | $1.98 | $3.96 | 

Shorter output can reduce token expense at an unchanged rate. DeepSeekâs claim that this update completes tasks faster with less output remains a vendor claim until measured on your workload. Count rejected attempts, retries, tool fees, and human correction time when comparing accepted-task cost.

## Search and Fetch open to all

Search and Fetch are separate standalone APIs, each priced at **$0.75 per 1,000 successful calls**. These charges are separate from model token charges.[2][4]

| Endpoint | Billing unit | List price | 
|---|---|---|
| Search | 1,000 successful Search calls | $0.75 | 
| Fetch | 1,000 successful Fetch calls | $0.75 | 

The promotion applies specifically to server-side tool calls made by DeepSeek API users through DeepSeek Harness, Claude Code, or Codex. Those calls are currently free, with billing starting **October 31, 2026 at 16:00 UTC**, which is **November 1 at 00:00 HKT/Beijing**. That is not a blanket free-access period for all standalone endpoint use.[2][4]

Instrument successful Search and Fetch calls independently. As an illustrative calculation, not a measured agent bill, 1,000 successful Search calls plus 1,000 successful Fetch calls total $1.50 at list rates, before model tokens. A single task may use both endpoints repeatedly; counting only top-level user requests hides that cost.

## Open weights (MIT) and local caveats

The exact published repository is deepseek-ai/DeepSeek-V4.1-Flash-1017. Its model card identifies the release under MIT; review the repository license for the actual terms.[3]

Hugging Faceâs approximately **763B** model-size field is not an activated-parameter count and does not establish a hardware requirement. Downloadable weights do not prove that a particular machine, quantization, or serving stack will meet your context, throughput, and reliability needs. Superbash makes no hardware recommendation for this release.

If you plan to self-host, validate the serving implementation, request format, output limits, tools, and acceptance rubric separately. A local result and a moving hosted alias are distinct evaluation records even when their release names match.

## Where it sits versus Kimi, Claude, and GPT

Flash offers low token list prices, optional thinking, separately metered Search/Fetch, and published MIT weights. Kimi K3.1 Proâs API constraints and weight-release schedule differ: thinking is always on, Highspeed is a separate speed SKU, and full weights are scheduled by November 4.[3][4]

For Claude and GPT, the relevant question is whether their result needs less review or fewer retries on the same job. DeepSeekâs model-card peer table includes older K3, Opus 5, and GPT-5.6 Sol entries. It should not be presented as a current K3.1 Pro, Opus 5.5, or GPT-6 Astra head-to-head.[3]

Kimiâs and DeepSeekâs published Terminal-Bench scores also come from different vendor publications and conditions. Even when both mention Claude Code, that does not make them a controlled Superbash comparison. Use the tables to choose candidates, then compare accepted results under one rubric.

## What to do now

1. Inspect your configured Flash alias and record the current served version and date.
2. Check the Harness version and set a task-appropriate output cap.
3. Log cached and uncached input, output, successful Search calls, and successful Fetch calls separately.
4. Define one bounded task and an acceptance rubric; include retries, elapsed time, and correction time in the comparison.
5. Revisit the server-side tool promotion before October 31 at 16:00 UTC so the next bill is expected.

The lesson on choosing a model by the job helps separate low list prices from a useful finished result.

### Do I need to change the model ID?

No. The documented ID remains `deepseek-flash`; record what version the alias serves.[4]

### Did token prices fall?

No. List prices are unchanged. Reduced output might lower a particular taskâs bill, but Superbash has not measured that outcome.[1][2]

### Is this a new Pro flagship?

No. Flash-1017 is the October update to the Flash family. `deepseek-v4-pro` remains a separate SKU.[1][4]

## Sources

Sources checked October 25, 2026. The changelog, announcement, model card, and pricing page were re-fetched; no authenticated model request or Superbash benchmark run was made.

[1] https://api-docs.deepseek.com/updates/ â October 17 release changelog and vendor comparison

[2] https://api-docs.deepseek.com/news/news261017/ â Launch, Harness recommendation, Search/Fetch, and promotion deadline

[3] https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017 â Published model card, benchmark footnotes, and MIT release

[4] https://api-docs.deepseek.com/quick_start/pricing â Hosted versions, capabilities, token rates, and tool billing

### Try

Audit the Flash alias, harness version, output budget, and Search/Fetch call counters before a bounded comparison.

### Prove it worked

Record served version, accepted output, token usage, successful calls per endpoint, elapsed time, and retries.

### Keep in view

- Keep the deepseek-flash API ID and record the served version; Flash-1017 is the October 17 update.
- Token list prices are unchanged, with separate peak and off-peak rates.
- Search and Fetch list $0.75 per 1,000 successful calls per endpoint; the server-side tool promotion ends October 31 at 16:00 UTC.
