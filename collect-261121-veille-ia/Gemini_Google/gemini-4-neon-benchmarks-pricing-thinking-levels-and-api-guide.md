---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-benchmarks-pricing-thinking-levels-and-api-guide
title: "Gemini 4 Neon: Benchmarks, Pricing, Thinking Levels and API Guide"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["gemini", "neon", "model", "tokens", "google", "thinking", "pick", "googles"]
source: https://mer.vin/news/gemini-4-neon-benchmarks-pricing-api-guide/
source_anchor: ""
source_lines: [1, 453]
section: "Gemini_Google"
---
**Gemini 4 Neon** (gemini-4-neon) is Google’s new mid-tier Gemini 4 model. It costs $3 per million input tokens and $15 per million output tokens, has a 1M-token context window and four thinking levels, and on Google’s own benchmark table it beats Claude Opus 5.5 and GPT-6.2 Sol on most rows. Google released it on 18 November 2026 and it is available in the Gemini API the same day.

```
%%{init: {"theme": "base", "themeVariables": {"background": "transparent", "lineColor": "#000000"}}}%%
graph TD
    REQ[Your request] --> API[Gemini API paid tier]
    API --> PICK{Which Gemini 4 model}
    PICK -->|Everyday coding and agents| HYD[Hydrogen 1.50 and 7.50 USD]
    PICK -->|Complex, high-frequency work| NEON[Gemini 4 Neon 3 and 15 USD]
    PICK -->|Hardest long-horizon projects| CARB[Carbon 4 and 20 USD]
    NEON --> LVL{Thinking level}
    LVL -->|low or medium| FAST[Fast, cheap answers]
    LVL -->|high default| BAL[Balanced quality and cost]
    LVL -->|xhigh| DEEP[Deepest reasoning, AA Index 61]
    NEON --> CTX[Unified 1M token window]
    CTX --> LD[Long decode up to 1M output via background]
    NEON --> TOOLS[Function calling, code execution, computer use, search grounding]
    FAST --> OUT[Answer or agent action]
    BAL --> OUT
    DEEP --> OUT
    classDef agent fill:#8B0000,color:#fff
    classDef hook fill:#189AB4,color:#fff
    classDef decision fill:#444,color:#fff
    class REQ,NEON,OUT agent
    class API,HYD,CARB,CTX,LD,TOOLS,FAST,BAL,DEEP hook
    class PICK,LVL decision
```
*Google’s own family chart. Neon sits between Carbon and Hydrogen. It scores 61 on the Artificial Analysis Index against Carbon’s 60 and Hydrogen’s 54, at $3/$15 per million tokens.*

## What is Gemini 4 Neon?

Gemini 4 Neon is a large language model from Google that you can call over an API, much like you would call a cloud database. It is the “middle” model of the Gemini 4 family: smarter than the cheap, fast **Hydrogen**, and faster and cheaper per task than the heavyweight **Carbon**. Google built it for long software-engineering sessions, document-heavy office work and science problems, which is where most paying API customers spend their tokens.

| Question | Answer | 
|---|---|
| What is it? | An AI model from Google that reads text, images, audio, video and PDFs, and writes text and code | 
| Who makes it? | Google DeepMind, released 18 November 2026 | 
| How do I use it? | Through the Gemini API (paid tier), Google AI Studio, Gemini for business, Google Antigravity, or the Gemini app on Pro/Ultra plans | 
| What does it cost? | $3 per million input tokens and $15 per million output tokens (roughly 750,000 words in for $3) | 
| Is there a free tier? | No. Neon is paid-tier only on the API | 
| Why should I care? | On Google’s numbers it matches or beats the top rival models on most benchmarks, and finishes tasks at a lower cost than Carbon | 

| Old way | New way | Everyday analogy | 
|---|---|---|
| Pick the biggest, slowest model for anything hard and wait | Pick Neon and set a “thinking level” per request: low, medium, high or xhigh | A car with four driving modes rather than two separate cars | 
| Separate input and output budgets (1M in, 64K out on Gemini 3.8 Flash) | One shared 1,048,576-token window for input, thinking and output | One shared bank account instead of two fixed wallets | 
| Explicit context caches you create and manage | Automatic (implicit) caching with a 95% discount on repeated prompt prefixes | A shop that remembers your usual order without a loyalty card | 
| Long answers cut off at 64K tokens | Background “long decode” continues in 64K hops up to 1M tokens | A printer that feeds more paper automatically | 

*In one line: Neon is Google’s new mid-priced model for work you would previously have sent to the most expensive model.*


## Gemini 4 Neon specs at a glance

These figures come from Google AI Studio’s model page and the “What’s new in Gemini 4 Neon” developer guide.

| Property | Value | 
|---|---|
| Model code | `gemini-4-neon` (stable; no preview suffix) | 
| Release date | 18 November 2026 (blog timestamp 21:00 UTC) | 
| Inputs | Text, image, video, audio, PDF | 
| Output | Text only (no image or audio generation) | 
| Context window | 1,048,576 tokens, unified across input, thinking and output | 
| Max output (synchronous) | 65,536 tokens per request, streaming or not | 
| Max output (long decode) | Up to 1,048,576 minus input tokens, in 64K background hops | 
| Thinking levels | `low` ,`medium` ,`high` (default),`xhigh` ;`minimal` is rejected | 
| APIs | `generateContent` and the Interactions API | 
| Consumption options | Standard, Batch API, Flex inference, Priority inference | 
| Caching | Implicit only (prompts of at least 4,096 tokens); explicit `cachedContents` not supported | 
| Free tier | Not available | 
| Managed Agents | Supported by the Antigravity agent, standalone or paired with Hydrogen | 

### Capability matrix

| Capability | Status | 
|---|---|
| Agentic video understanding | Supported | 
| Code execution | Supported | 
| Computer use | Supported | 
| Context caching | Supported (implicit only) | 
| File search | Supported | 
| Function calling | Supported | 
| Grounding with Google Maps | Supported | 
| Grounding with Google Search | Supported | 
| Structured outputs | Supported | 
| Thinking | Supported | 
| URL context | Supported | 
| Audio generation | Not supported | 
| Image generation | Not supported | 
| Live API | Not supported | 

## Where Neon sits in the Gemini 4 family

Google now has four Gemini 4 endpoints. Neon scores highest on Artificial Analysis, but Google still positions Carbon as the model for the hardest long-horizon work.

|  | Carbon | Neon | Hydrogen | Argon | 
|---|---|---|---|---|
| Model code | `gemini-4-carbon` | `gemini-4-neon` | `gemini-4-hydrogen` | `gemini-4-argon` | 
| Best for | Hardest long-horizon projects and deep research | Deep reasoning across software engineering, knowledge work and science | Speed and efficiency in everyday coding and agentic workflows | Efficient Gemini 4 frontier reasoning, lower spend per turn than Carbon | 
| AA Intelligence Index | 60 | 61 | 54 | Not in Google’s chart | 
| Price in / out per 1M | $4 / $20 ($2 / $10 promo through 30 Nov) | $3 / $15 | $1.50 / $7.50 | $4 / $20 ($2 / $10 promo through 30 Nov) | 
| Thinking levels | low, medium, high | low, medium, high, xhigh | low, medium, high, xhigh | low, medium, high | 
| Default thinking level | medium | high | high | medium | 
| Speed (Google’s 5-rocket scale) | 2 of 5 | 3.5 of 5 | 5 of 5 | Not rated | 
| Consumer plan | Google AI Ultra | Google AI Pro and Ultra | Everyone | Not listed | 
| API free tier | No | No | Yes | No | 

- **Carbon:** Gemini API in AI Studio (paid), Gemini for business, Gemini app, Google Antigravity and AI Mode in Search for Ultra subscribers.
- **Neon:** Gemini API in AI Studio (paid), Gemini for business, Gemini app and Google Antigravity (paid) for Pro and Ultra subscribers.
- **Hydrogen:** Gemini API in AI Studio (all tiers), Gemini for business, Gemini app, Google Search, and Antigravity (paid) for Pro and Ultra.

Inside Google Antigravity, Neon can run on its own or as a fast “Orchestrator” that directs parallel Hydrogen subagents. That is how Google intends the two models to be paired in multi-model agent setups.

## Pricing, and why Neon costs more than Carbon this month

Neon has flat pricing across the full 1M window, with no surcharge for long context. Thinking tokens are billed as output.

| Item (per 1M tokens, USD) | Gemini 4 Neon | Gemini 4 Carbon / Argon until 30 Nov | Carbon / Argon from 1 Dec | Gemini 4 Hydrogen | 
|---|---|---|---|---|
| Input | $3.00 | $2.00 | $4.00 | $1.50 | 
| Output (incl. thinking) | $15.00 | $10.00 | $20.00 | $7.50 | 
| Cached input (implicit) | $0.15 (95% off) | $0.10 | $0.20 | $0.075 | 
| Free tier | Not available | Not available | Not available | Free of charge | 
| Search grounding | 5,000 free requests/month shared across Gemini 3 and newer, then $14 per 1,000 (cap 1,500/day) |  |  |  | 
| Maps grounding | 5,000 free requests/month shared, then $14 per 1,000 queries |  |  |  | 
| Data used to improve products (paid) | No |  |  |  | 

Until 30 November, Carbon’s promotional price of $2/$10 makes it **cheaper per token** than Neon. Neon only works out cheaper per token from 1 December, when Carbon goes to $4/$20. Google’s “32% lower cost per task than Carbon” claim relies on Neon using fewer tokens per task, not on a lower rate card. If you plan to switch, test on your own traffic before December.

### Third-party listing: OpenRouter

| OpenRouter slug | Input / 1M | Output / 1M | Context | Max completion | 
|---|---|---|---|---|
| `google/gemini-4-neon` | $3.00 | $15.00 | 1,048,576 | 943,718 | 
| `google/gemini-4-neon:batch` | $1.50 | $7.50 | 1,048,576 | 943,718 | 
| Flex endpoint (Google AI Studio) | $1.50 | $7.50 | 1,048,576 | 943,718 | 

The batch and flex listings halve the price. If your workload can wait, such as nightly evals, document backfills or offline agents, those listings bring Neon down to Hydrogen’s standard rate card.

## Benchmarks: Neon vs Claude Opus 5.5 vs GPT-6.2 Sol

Every number below is **vendor-reported** by Google, which publishes its methodology at deepmind.google/models/evals-methodology/gemini-4-neon. The comparator models and harnesses are Google’s choice, and none of these rows has yet been independently replicated with the same harness.

| Area | Benchmark | Metric | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|---|
| Capability index | AA Index v5.0.1 | Score | **61** | 59 | 57 | 
| Capability index | Vals Index v2.2 | Score | **70.9%** | 70.1% | 66.1% | 
| Agentic coding | CursorBench 4.1 | Score | **65.4%** | 58.1% | — | 
| Agentic coding | Terminal-Bench 4.1 | Cheating-adjusted accuracy | 65.2% | **65.8%** | 61.5% | 
| Agentic coding | FrontierCode 2.0 | Main score | 46.2% | **47.3%** | 43.8% | 
| Agentic coding | SWE-sweep | Provider harness | **39.0%** | 10.1% | 8.3% | 
| Agentic coding | SWE-sweep | Mini-SWE harness | **12.8%** | 3.6% | 11.6% | 
| Knowledge work | AutomationBench | Score | **56.3%** | 42.5% | 41.3% | 
| Knowledge work | Agents’ Last Exam v1.1 | Score | **75.8%** | 71.6% | 73.0% | 
| Science and math | RiemannBench | Score | **84.0%** | 80.0% | 80.0% | 
| Science and math | Terminal-Bench-Science 0.1 | Score | **70.5%** | 63.3% | 68.0% | 
| Reasoning | HLE-Diamond | Score | **71.3%** | 67.9% | 64.0% | 
| Multimodal | MKA | Climb WSR | **56.5%** | 53.0% | 52.7% | 
| Multimodal | Chartography | Score | 73.5% | 66.3% | **75.1%** | 
| Multimodal | LVBench | Score | **89.0%** | 83.7% | 87.6% | 
| Long context | GraphWalks | 256K to 1M, BFS (F1) | **95.2%** | 66.8% | 80.1% | 
| Cybersecurity | CWE-Bench v2 | Score | **70.7%** | 67.1% | 66.0% | 
| Cybersecurity | SEC-bench Pro | Score | **90.5%** | — | 88.3% | 
| Cybersecurity | WebExploitBench v1.2 | Score | **51.3%** | 37.1% | — | 

- **Neon leads 16 of 19 rows** in its own vendor table.
- **Opus 5.5 leads both pure coding-agent rows** : Terminal-Bench 4.1 (65.8% vs 65.2%) and FrontierCode 2.0 (47.3% vs 46.2%).
- **GPT-6.2 Sol leads chart reading** : Chartography 75.1% vs 73.5%.
- **The biggest gap is long context** : GraphWalks 95.2% vs 66.8% (Opus) and 80.1% (Sol) at 256K–1M tokens.
- **Dashes are missing scores** , not zeros: no Sol score on CursorBench or WebExploitBench, and no Opus score on SEC-bench Pro.

### Leaderboard placements Google cites

| Leaderboard | Neon result | Rank claimed | 
|---|---|---|
| Artificial Analysis Intelligence Index | 61 (xhigh) | #3 overall, behind larger, slower models | 
| Artificial Analysis Coding Agent Index | 58.6 | #1 overall | 
| Vals Index | 70.9% | #2, on the Pareto frontier | 
| Vals Legal Research Bench | Not stated | #1 | 
| Vals Tax Agent Bench | Not stated | #1 | 
| Agent Arena Index (Arena) | 1336 across 4,200+ real agent sessions | #2 globally; #2 on steerability | 
| Surge AI benchmarks | Not stated | #1 on most finance, accounting, spreadsheet-modelling and constraint-following benchmarks | 
| Zapier AutomationBench | 56.3% | #1 overall | 
| Andon Labs Vending-Bench 2 | $16,843 | #1 overall | 

The table and the blog post do not quite agree. The post says Neon is “#3 overall” on Artificial Analysis, yet the table shows Neon (61) above both comparators (59 and 57). The two models ranked above Neon are left out of Google’s table, so it is not a full leaderboard.

## Cost per task across the four thinking levels

Google published an AA Index v5.0.1 chart that plots intelligence score against dollars per task. The values below are read off that chart by eye, so treat them as approximate. The named Neon labels and the 61 xhigh score are exact.

| Model / setting | AA Index (approx.) | Cost per task (approx.) | 
|---|---|---|
| Neon low | ~44 | ~$0.70 | 
| Neon medium | ~55 | ~$1.90 | 
| Neon high (default) | ~60 | ~$3.70 | 
| Neon xhigh | 61 | ~$6.40 | 
| Carbon (top setting) | ~60 | ~$7.70 | 
| Claude Opus 5.5 (top setting) | ~59 | ~$10.00 | 
| GPT-6.2 Sol (top setting) | ~57 | ~$1.00 | 

| Efficiency claim (vendor-reported) | Number | 
|---|---|
| Neon high vs Carbon, cost per task | 32% lower | 
| Neon high vs top competing frontier models (Vals Index, Agent Arena) | ~50% to 80% lower cost per task | 
| Neon xhigh vs competitors at their highest setting | 36%+ lower cost per task | 

*Google’s own chart shows GPT-6.2 Sol reaching ~57 at roughly $1 per task. Neon needs ~$1.90 to reach ~55. Neon is cheaper per point than Carbon and Opus, but not cheaper than Sol.*


| Thinking level | Token use | Google’s guidance | 
|---|---|---|
| `low` | Fewest tokens, lowest latency | Cost- and latency-sensitive work: fast chat, high-throughput processing; still capable on agentic tasks | 
| `medium` | Lower token use, faster responses | Interactive coding, live UI prototyping, responsive chat, latency-sensitive tool calls | 
| `high` (default) | Balanced | Most tasks: complex coding, agentic execution, tool calling, multimodal knowledge work | 
| `xhigh` | Most tokens, deepest reasoning | Hard multi-step engineering, analytical and scientific problems where high falls short | 

Google’s recommended process is to run your evals at all four levels with a generous `max_output_tokens`, then pick the lowest level that meets your quality bar.

## Coding and long-running agent workloads

Google pitches Neon at long, tool-heavy agent sessions. The specific behaviours it claims are fewer redundant file reads, progress updates during the run, reliable context compaction, and targeted edits across many files.

| Coding result | Neon | Note | 
|---|---|---|
| AA Coding Agent Index | 58.6 | #1 overall | 
| Terminal-Bench 4.1 | 65.2% | Opus 5.5 is higher at 65.8% | 
| FrontierCode 2.0 | 46.2% | At default high setting; Opus 5.5 is higher at 47.3% | 
| CursorBench 4.1 | 65.4% | vs Opus 5.5 58.1% | 
| SWE-sweep, mini-SWE harness | 12.8% | Leads on a neutral harness | 
| SWE-sweep, Google Antigravity harness | 39.0% | More than triples; stated as state of the art | 

The SWE-sweep pair shows how much the harness matters. On a neutral harness Neon scores 12.8%, and Sol is close behind at 11.6%. On Google’s own Antigravity harness the score jumps to 39.0%. Unless you run Antigravity, the 12.8% figure is the one that predicts what you will see.

1. Set `thinking_level` to`high` as a baseline. Try`medium` for interactive edits.
2. Remove hardcoded low `max_output_tokens` caps such as 8,192. Raise them to 65,536.
3. Declare tools natively with function calling. Prompting for raw JSON tool calls with no declared tools can return `MALFORMED_FUNCTION_CALL` .
4. Use `previous_interaction_id` to keep multi-turn tool sessions stateful.
5. Raise HTTP client timeouts to at least 15 minutes, or use background execution.

## Knowledge work, science and long context

Google built Neon to read spreadsheets, PDFs, charts, audio and video in one 1M-token context and turn them into structured deliverables. These are the non-coding results it reports.

| Domain | Benchmark | Neon | Claim | 
|---|---|---|---|
| Enterprise | Vals Index | 70.9% | #2 | 
| Legal | Vals Legal Research Bench | — | #1 | 
| Tax | Vals Tax Agent Bench | — | #1 | 
| Charts | Chartography | 73.5% | Dense professional charts | 
| Agents | Agents’ Last Exam v1.1 | 75.8% | On par with leading models | 
| Automation | Zapier AutomationBench | 56.3% | #1 | 
| Long-horizon business sim | Vending-Bench 2 | $16,843 | #1 | 
| Agent sessions | Agent Arena Index | 1336 | #2 globally, 4,200+ sessions | 
| Science | Terminal-Bench Science | 70.5% | Best in class | 
| Mathematics | RiemannBench | 84.0% | Best in class | 
| Reasoning | HLE-Diamond | 71.3% | Best in class | 
| Video | LVBench | 89.0% | Ahead of Opus 5.5 (83.7%) and Sol (87.6%) | 
| Multimodal | MKA (Climb WSR) | 56.5% | Ahead of Opus 5.5 (53.0%) and Sol (52.7%) | 
| Long context | GraphWalks 256K–1M BFS F1 | 95.2% | Widest lead in the table | 

GraphWalks is the result most worth testing yourself. If 95.2% F1 on graph traversal at 256K–1M tokens holds up on your data, you may be able to drop some retrieval steps for large-repository or large-contract work. Retrieval still costs less per call, though: at $3 per million input tokens, filling the whole 1M window costs about $3 per request before any caching.

## Cybersecurity results and the Fairwind Program

Google reports cyber results at each stage of hardening code: writing secure code, finding vulnerabilities, proving they can be exploited, and patching them. The offensive-capable variant is not on the general API.

| Stage | Benchmark | Neon | Context | 
|---|---|---|---|
| Write secure code | AutoBaxBench (hard tier) | 76% | State of the art; beats Carbon on easy, medium and hard | 
| Penetration testing | WebExploitBench v1.2 | 51.26% (table: 51.3%) | State of the art at half the cost of large competing models; Opus 5.5 37.1% | 
| Find vulnerabilities | SEC-bench Pro | 90.5% | State of the art at roughly half the cost per challenge; Sol 88.3% | 
| Validate exploitability | ExploitGym | Frontier-level (no score given) | Uses half the tokens of Carbon | 
| Patch root cause | Collinear CWE-bench v2 | 70.7% | 3.4% uplift over Carbon; Opus 5.5 67.1%, Sol 66.0% | 

- **Gemini 4 Neon for cyber** is rolling out “in the coming days” to trusted defenders through the**Fairwind Program** .
- To get access you request it through your Google Cloud account team. It is not a model ID you can call today.
- The general `gemini-4-neon` endpoint has real-time safeguards against cyber and biosecurity misuse that Google calls comparable to Carbon’s.

## Safety and agent-behaviour numbers

For anyone running Neon unattended in production, these figures matter more than leaderboard rank: they measure how often the model refuses, cheats, oversteps or falls for injected instructions. All are from Google’s internal evaluations; the full detail is in the Gemini 4 Neon model card.

| Measure | Result | 
|---|---|
| Refusal rate, Terminal-Bench 4.0 (legitimate cyber/bio-adjacent work) | 1.98% of tasks | 
| Refusal rate, Terminal-Bench-Science | 0% | 
| Cheating on Terminal-Bench 4.0 | 0.0% (matches Carbon) | 
| Shortcuts on coding cheating evaluation | 2.45% (lowest of any Gemini 4 model) | 
| Misaligned action rate, agentic safety suite | 0.09% | 
| Agent honesty evaluation pass rate | 99.2% | 
| Under-service vs Carbon | Reduced 43%–56% across effort levels | 
| Overreach vs Carbon | Reduced 15%–35% across effort levels | 
| Gray Swan indirect prompt injection, highest thinking level with safeguards | 0.0% attack success rate | 
| Chain-of-thought and action misalignment monitor alerts | Below Carbon’s baseline in internal coding use | 
| Fabrication rate | Lowest yet in internal Google Antigravity testing (no figure given) | 

*Note the condition on the injection result: 0.0% applies at the highest thinking level with real-time safeguards on. Google gives no number for the default high level.*


## Calling gemini-4-neon from the API

Install the Google GenAI SDK (`pip install google-genai`) and set `GEMINI_API_KEY` from a paid-tier project. Google’s docs use the Interactions API in their examples.

### Minimal request (defaults to high thinking, 64K output)

```
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-neon",
    input="Build an interactive single-file HTML token bucket rate-limiter simulator "
          "with live sliders for burst size and request traffic.",
)
print(interaction.output_text)
```
### Choosing a thinking level per request

```
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-neon",
    input="Audit this payment retry queue for race conditions and rewrite the lock acquisition logic.",
    generation_config={
        "thinking_level": "xhigh",   # low | medium | high (default) | xhigh
    },
)
print(interaction.output_text)
```
### Long decode past 64K tokens with background execution

```
import time
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-neon",
    input="Generate a complete multi-module migration specification and test suite for this service.",
    background=True,
    generation_config={
        "max_output_tokens": 262144,   # above 65,536 triggers 64K continuation hops
        "thinking_level": "high",
    },
)
while interaction.status in ("in_progress", "queued"):
    time.sleep(5)
    interaction = client.interactions.get(id=interaction.id)
text = "".join(
    content.text
    for step in interaction.steps
    if step.type == "model_output"
    for content in step.content
    if content.type == "text"
)
print(text)
print("Status:", interaction.status)
print("Usage:", interaction.usage)
```
| Workload | Recommended `max_output_tokens` | 
|---|---|
| Standard tasks | Keep the 65,536 default or omit the field | 
| Existing apps with a low cap | Raise to 65,536; lower `thinking_level` to save tokens instead | 
| Long outputs and multi-step tasks | 131,072 or 262,144 with `background=True` , up to 1,048,576 minus input | 
| `background=True` with no cap set | Server stops at 65,536 | 

## Migration checklist from Gemini 3.x and other Gemini 4 models

If you are moving from another Gemini 4 model, the request schema does not change. If you are coming from Gemini 3.8 Flash or older, several defaults break silently.

| Change | From Gemini 4 Carbon / Argon / Hydrogen | From Gemini 3.x | 
|---|---|---|
| Model ID | Set to `gemini-4-neon` | Set to `gemini-4-neon` | 
| Default thinking level | `high` (Carbon and Argon default to`medium` and cap at`high` ) | `high` rather than`medium` ;`xhigh` added;`minimal` returns an error | 
| Context budget | Unchanged | One shared 1,048,576 window replaces separate 1M input / 64K output | 
| Explicit caching | Unchanged (already implicit-only) | Remove `cached_content` ,`cachedContents` ,`caches.create` | 
| Sampling parameters | Unchanged | Remove `temperature` ,`top_p` ,`top_k` ; in-range values ignored, out-of-range values return`400` | 
| Tool calls | Unchanged | Declare tools natively or use structured outputs to avoid `MALFORMED_FUNCTION_CALL` | 
| Thought signatures | Unchanged | Return prior turns and signatures unchanged; never copy one across parallel `functionCall` parts | 
| Long outputs | Unchanged | Use `background=True` above 65,536 tokens | 
| Timeouts | Unchanged | 15 minutes or more, or background execution | 
| Billing | Paid-tier project required | Every token billed, including thinking and grounding system instructions | 

```
# Before (Gemini 3.8 Flash era)
config = {"temperature": 0.2, "top_p": 0.9, "max_output_tokens": 8192,
          "thinking_level": "minimal", "cached_content": cache.name}
# After (gemini-4-neon)
config = {"max_output_tokens": 65536, "thinking_level": "medium"}
# implicit caching applies automatically to prompts of 4,096+ tokens
```
Antigravity users can automate the upgrade with the `gemini-api-dev` skill by running `/gemini-api-dev migrate my app to Gemini 4 Neon`.

## What the launch does not tell you

Every number in the announcement comes from Google. These are the gaps and inconsistencies you should account for before changing your stack.

| Issue | Why it matters | 
|---|---|
| Comparison table is vendor-curated | It shows Opus 5.5 and GPT-6.2 Sol only. Neon is “#3 overall” on AA, so two stronger models are missing from the table | 
| Headline AA score of 61 is at xhigh | The default high level sits at about 60 on Google’s own chart. You pay xhigh token counts to get the extra point | 
| Carbon is cheaper per token until 30 Nov | Carbon’s $2/$10 promo undercuts Neon’s $3/$15. The per-task saving depends on Neon’s lower token use | 
| Sol is far cheaper per task | Google’s own AA chart puts Sol at ~57 for ~$1 per task | 
| SWE-sweep 39.0% needs Antigravity | With a neutral harness the score is 12.8% | 
| Opus 5.5 still leads two coding rows | Terminal-Bench 4.1 and FrontierCode 2.0 | 
| No free tier, no explicit caching | Prototyping costs money from the first call, and cache control is automatic only | 
| Sampling knobs removed | Pipelines tuned with `temperature` lose that control | 
| Cyber variant gated | The exploit-validation capability needs Fairwind Program access | 
| Some ranks have no published score | Legal Research Bench, Tax Agent Bench and Surge AI wins are stated as ranks only | 
| Carbon kept for long horizons | Google still recommends Carbon for the hardest projects, despite Neon’s higher index | 

### What developers reported on launch day

The launch thread on Hacker News reached 993 points and 751 comments within a day. The reports below are anecdotal and unverified, but they show what early users ran into.

- **Speed:** one user measured about 127 tokens per second for Neon on OpenRouter, against about 46 for Carbon. That fits Google’s speed ratings (3.5 vs 2 rockets), but it is a single measurement.
- **Naming:** the most-replied comment complained about mapping Carbon, Neon, Hydrogen and Argon to use cases. Another user noted that Argon now looks superseded.
- **Benchmarks vs feel:** several commenters said Opus 5.5 still feels stronger in daily use despite Neon’s numbers. Others said Neon edges Opus 5.5 on Vals comparisons.
- **Jagged capability:** one commenter pointed out that Neon’s AA Index is higher than Carbon’s while Carbon still scores better on research-scale reasoning evals. That matches Google’s own positioning.
- **Access gaps:** student accounts reported that they could not use the new models in Antigravity.

## Who should switch to Neon this week

| If you run… | Recommendation | Reason | 
|---|---|---|
| Gemini 3.8 Flash for agents | Evaluate Neon at `medium` and`high` | Large reasoning jump; work through the migration checklist first | 
| Gemini 4 Carbon for everyday complex work | A/B test Neon now; decide before 1 December | Carbon doubles to $4/$20 on 1 Dec, and Neon claims 32% lower cost per task | 
| Gemini 4 Hydrogen | Keep Hydrogen as the default; route hard turns to Neon | Hydrogen costs half as much and is the fastest; Google pairs them as subagent and orchestrator | 
| Claude Opus 5.5 for coding agents | Benchmark on your own repo before moving | Opus leads Terminal-Bench 4.1 and FrontierCode 2.0 | 
| Long-document or 1M-context RAG | Strong candidate | GraphWalks 95.2% at 256K–1M, flat pricing across the window | 
| Cost-capped batch jobs | Use Batch or Flex | Batch and Flex listings show $1.50/$7.50 | 
| Free-tier prototypes | Stay on Hydrogen | Neon has no free tier | 

## Gemini 4 Neon summary in numbers

| Metric | Value | 
|---|---|
| Model ID | `gemini-4-neon` | 
| Price (input / cached / output per 1M) | $3.00 / $0.15 / $15.00 | 
| Context window | 1,048,576 tokens (unified) | 
| Sync / long-decode output | 65,536 / up to 1,048,576 tokens | 
| Thinking levels | 4 (low, medium, high default, xhigh) | 
| AA Intelligence Index | 61 at xhigh (#3 overall) | 
| AA Coding Agent Index | 58.6 (#1) | 
| Vals Index | 70.9% (#2) | 
| Agent Arena Index | 1336 (#2) | 
| GraphWalks 256K–1M | 95.2% | 
| SEC-bench Pro | 90.5% | 
| Cost per task vs Carbon (high) | 32% lower (vendor-reported) | 
| Gray Swan IPI attack success | 0.0% (highest thinking, safeguards on) | 
| Free tier | None | 

*Bottom line: Neon is a real option for Carbon-class work at a lower cost per task, especially for long-context and document-heavy agents. Test it on your own traffic before 1 December, and treat every benchmark win as vendor-reported until independent results arrive.*
