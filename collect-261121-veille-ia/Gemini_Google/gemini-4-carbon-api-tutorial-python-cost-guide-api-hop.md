---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-api-tutorial-python-cost-guide-api-hop
title: "Gemini 4 Carbon API Tutorial: Python & Cost Guide | API Hop"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["output", "gemini", "thinking", "tokens", "carbon", "python", "api", "tutorial"]
source: https://apihop.ai/en/blog/how-to-use-gemini-4-carbon
source_anchor: ""
source_lines: [1, 253]
section: "Gemini_Google"
---
This tutorial walks you through the Gemini 4 Carbon API in Python: install the `google-genai` SDK, make your first call in under ten lines, then build up to multi-turn chat, background long-decode, and multimodal document analysis. The model ID is `gemini-4-carbon`; you need a Gemini API paid tier and an API key. The limited-time price is $2/$10 per million tokens through November 30, 2026, reverting to $4/$20 after.

## Key takeaways

- Gemini 4 Carbon uses the **Interactions API** :`client.interactions.create()` — not the old generate_content pattern.
- Default synchronous output caps at 65,536 tokens; longer output requires both `background: true` and a raised`max_output_tokens` .
- Output pricing includes thinking tokens; the 1M context is shared by input, thinking, and output.
- `temperature` ,`top_p` , and`top_k` are ignored; explicit caching isn’t supported — clean both out of old code.
- Run complex tasks in the background with HTTP timeouts of at least 15 minutes, and stitch step outputs when done.
- The static cost floor for this tutorial’s flow is about $0.35 (promo-period Batch); your actual bill follows your token usage.

## Prerequisites

Three things before you start:

1. **API key** : create one in Google AI Studio; the account needs the Gemini API paid tier (Carbon has no free tier). After signing in, create the key on the API keys page and confirm billing is enabled in payout settings. If calls return 401, check this first, then the network.
2. **Python environment** : Python 3.10+ recommended; install the official SDK:

`pip install -U google-genai`
1. **Environment variable** : put the key in an environment variable, never in the repo:

`export GEMINI_API_KEY="your API key"`
## Quickstart: your first call

Start with a minimal request to confirm the key, network, and model access all work. One cURL line verifies it:

```
curl -X POST "https://generativelanguage.googleapis.com/v1beta/interactions" \
  -H "Content-Type: application/json" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -d '{"model": "gemini-4-carbon", "input": "Explain long decoding in one sentence"}'
```
Expect a JSON response containing the model’s brief explanation. The Python version is just as short:

```
from google import genai
client = genai.Client()  # reads GEMINI_API_KEY automatically
interaction = client.interactions.create(
    model="gemini-4-carbon",
    input="Explain long decoding in one sentence",
)
print(interaction.output_text)
```
Expected output: a sentence or two explaining long decoding. Once you see text, the pipeline works and you can move on.

## Step-by-step guide

### Step 1: choose a thinking level to control your first bill

Carbon supports `low`, `medium` (default), and `high` thinking. Higher levels usually mean more thinking tokens and latency. Note: **output is billed as “thinking + visible output” combined**, so the level directly shapes the bill.

```
interaction = client.interactions.create(
    model="gemini-4-carbon",
    input="Analyze the time complexity of this code: for i in range(n): for j in range(i): print(i, j)",
    generation_config={
        "thinking_level": "medium",
        "max_output_tokens": 32768,
    },
)
print(interaction.output_text)
```
Expected output: an analysis of the nested loop’s complexity (O(n²) territory). Use medium for everyday tasks; enable high per task only when regression tests show a clear success-rate gain.

Key parameters used in this tutorial:

| Parameter | Values/default | Role and caveat | 
|---|---|---|
| `thinking_level` | low / medium (default) / high | Thinking depth; higher levels usually cost more tokens and latency | 
| `max_output_tokens` | synchronous default cap 65,536 | Caps “thinking + visible output” combined; raise it for long decodes | 
| `background` | true / false | Long-task switch; enabling it without raising the cap still stops at 64K | 
| `previous_interaction_id` | id returned by the previous turn | Keeps server-side multi-turn state; `store=false` blocks it | 
| `generation_config` | passed per turn | Not inherited from history; re-specify each turn | 

### Step 2: multi-turn chat and function calling with server-side state

Multi-turn interactions reference the previous turn with `previous_interaction_id`; the server keeps the context, so you don’t stitch history yourself:

```
turn1 = client.interactions.create(
    model="gemini-4-carbon",
    input="I plan to analyze a 50-page contract PDF.",
)
turn2 = client.interactions.create(
    model="gemini-4-carbon",
    input="Which clauses should I focus on?",
    previous_interaction_id=turn1.id,
)
print(turn2.output_text)
```
Expected output: a checklist of key clauses for the contract scenario. Two details:

- `tools` ,`system_instruction` , and`generation_config` are**not inherited from history** — re-specify them each turn when needed.
- `store=false` is incompatible with background execution and blocks later`previous_interaction_id` use.

### Step 3: background execution and long decodes

Complex debugging and long-form generation can take minutes. Official guidance: run in the background, set HTTP timeouts to at least 15 minutes, and raise the output cap above 65K.

```
long_task = client.interactions.create(
    model="gemini-4-carbon",
    input="Read this codebase description and produce a complete refactoring plan with phased steps.",
    background=True,
    generation_config={
        "thinking_level": "high",
        "max_output_tokens": 131072,
    },
)
print(long_task.id)  # save the task ID for polling
```
After creation, poll status with `interactions.get`; once done, stitch the text from all `model_output` steps for the full result. Taking only the first segment is how plans end up half-written.

### Step 4: multimodal document analysis

Carbon accepts text, image, video, audio, and PDF input (text output). Here’s an analysis request with contract text (multimodal file formats follow the official Interactions docs — this article invents no fields):

```
contract_text = "...(contract text pasted here)..."
interaction = client.interactions.create(
    model="gemini-4-carbon",
    input=(
        "Extract the payment, breach, and termination clauses from this contract into a table.\n\n"
        f"Contract text:\n{contract_text}"
    ),
    generation_config={"thinking_level": "medium"},
)
print(interaction.output_text)
```
Expected output: a table organized by clause category. For long documents, check that citations trace back to page numbers, not just that the table looks tidy.

## Complete working example: long-document analysis with error handling

Chain the steps into a reusable function — a skeleton with timeout, retry, and output handling:

```
import time
from google import genai
client = genai.Client()
def analyze_document(prompt, max_retries=3):
    for attempt in range(max_retries):
        try:
            task = client.interactions.create(
                model="gemini-4-carbon",
                input=prompt,
                background=True,
                generation_config={
                    "thinking_level": "medium",
                    "max_output_tokens": 131072,
                },
            )
            # Poll the task until it finishes (add backoff and a total timeout in production)
            result = client.interactions.get(task.id)
            return result.output_text
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)  # exponential backoff
print(analyze_document("Summarize the three main risks in this annual report"))
```
Production still needs polling intervals, a total-timeout cap, error-code-aware retry decisions, and segment stitching for long output. The skeleton’s job is making failures visible instead of handing you an incomplete result.

Three practical rules for polling and backoff:

- **Lengthen the polling interval** : short first, then longer (5s, 10s, 20s) — avoid busy-spinning on background tasks and burning rate quota.
- **Make the total timeout configurable** : reserve at least 15 minutes for complex tasks per official guidance; past the deadline, log the task ID and check later — don’t discard and resend, or the same output gets billed twice.
- **Retry by error type** : 401/400 are config and permission problems — fix them, don’t retry; 429 and network timeouts suit exponential backoff (2s, 4s, 8s) with a retry cap.

## Error handling

| Error | Common cause | Fix | 
|---|---|---|
| 401 | Invalid/expired key or missing paid tier | Check the key and account status in AI Studio; don’t use free-tier accounts | 
| 400 | Incompatible parameters: explicit caching, ignored sampling params, malformed output cap | Delete `cachedContents` /`caches.create` , strip`temperature` /`top_p` /`top_k` , check`max_output_tokens` | 
| 429 | Rate limit hit | Exponential backoff, lower concurrency, upgrade tier if needed | 
| Timeout | Complex task waited on synchronously too long | Switch to `background: true` , HTTP timeout ≥15 minutes | 
| Output stops at 65K | Background enabled without raising the cap | Raise `max_output_tokens` above 65,536 | 

## Common issues and six gotchas

1. **Enabled `background: true` but output still stops at 64K.** Both switches matter: background mode + a raised`max_output_tokens` . Official reference levels for complex tasks are 131,072 or 262,144 tokens.
2. **Treating the 1M context as “1M more output”.** Input, thinking, and output share the total; bigger input leaves less room for reasoning and answers.
3. **Keeping explicit-cache code when migrating from older models.** Carbon supports implicit caching only (enabled by default at ≥4096 tokens); delete`cachedContents` .
4. **Wondering why temperature “does nothing”.**`temperature` ,`top_p` , and`top_k` are ignored — remove them rather than tuning style through them.
5. **Reading only the first segment of a long decode.** The server continues in segments; you must stitch all`model_output` steps.
6. **The hidden cost of search grounding.** Thinking tokens, internal tool instructions, and function declarations all bill; Google Search grounding also charges per query.

## Cost estimation

A static estimate at this tutorial’s typical usage: 100K input + 30K thinking + 20K visible output tokens (50K billed output, cache-free):

| Billing mode | Input cost | Output cost | Total | 
|---|---|---|---|
| Standard (through Nov 30) | 100K×$2/M=$0.20 | 50K×$10/M=$0.50 | **$0.70** | 
| Standard (from Dec 1) | 100K×$4/M=$0.40 | 50K×$20/M=$1.00 | **$1.40** | 
| Batch (through Nov 30) | 100K×$1/M=$0.10 | 50K×$5/M=$0.25 | **$0.35** | 

Running the same task on Batch during the promo costs roughly half of Standard. Budget-sensitive long tasks should evaluate Batch/Flex first; after the December 1 switch, re-base long-term budgets on $4/$20.

## Calling through API Hop: current status and migration

As of November 13, 2026, API Hop has not listed `gemini-4-carbon` but does support **Gemini 4 Argon** ($1/$5 per million tokens, cache-hit input $0.05). You can build the pipeline and regression tests on Argon first, then change only the model parameter once Carbon is listed.

API Hop uses an OpenAI-SDK-compatible format, unlike the native Google Interactions code above:

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["APIHOP_API_KEY"],
    base_url="https://api.apihop.ai/openai/v1",
)
response = client.chat.completions.create(
    model="gemini-4-argon",  # switch to gemini-4-carbon once listed
    messages=[{"role": "user", "content": "Explain long decoding in one sentence"}],
)
print(response.choices[0].message.content)
```
Choosing between the two: use the official Google SDK when you need Carbon-native capabilities like background long-decode and Interactions state management; use API Hop’s OpenAI-compatible endpoint when you want one key and a shared balance across many models. Support scope follows the API Hop console and pricing page.

## What to watch next

- **The December 1 price switch** : Standard input/output doubles; Batch/Flex/Priority adjust too — recompute budgets early.
- **SDK updates** : the Interactions API keeps evolving; background and long-decode fields follow the latest official docs, and this tutorial’s code will be revised on breaking changes.
- **API Hop listing** : once Carbon is listed, add same-task OpenAI-compatible endpoint measurements and a price comparison.

## FAQ

### Is the Gemini 4 Carbon API free?

No. Carbon requires the Gemini API paid tier; there’s no free tier. The limited-time price is $2 input / $10 output per million tokens (through November 30, 2026), then $4/$20. The AI Studio interface experience and API billing are separate things.

### Which Python SDK does Gemini 4 Carbon use?

The official `google-genai` package: `pip install -U google-genai`, entry point `client.interactions.create()`, text via `interaction.output_text`. Carbon runs on the Interactions API, so old generate_content tutorials don’t carry over; multi-turn uses `previous_interaction_id`, long tasks use `background: true`, and cURL hits the `v1beta/interactions` endpoint.

### Why does my Carbon output stop at 65K?

The default synchronous output cap is 65,536 tokens. For longer output you must set `background: true` and raise `max_output_tokens` above 65K — either one alone doesn’t work.

### Does Gemini 4 Carbon support image and PDF input?

Yes. Input modalities are text, image, video, audio, and PDF; output is text. Audio generation, image generation, and the Live API are not supported.

### Are thinking tokens billed?

Yes. Output pricing covers thinking plus visible output combined, and `max_output_tokens` caps that same total. Higher thinking levels raise both latency and cost.

### Can I just swap another Gemini model’s code to Carbon?

Not by model name alone. Clean out explicit caching and ignored sampling parameters, move complex tasks to background execution, raise output caps and stitch results, and note that `tools` and `generation_config` don’t inherit across turns.

Information verified 2026-11-13: Gemini API quickstart (Interactions API) · Gemini 4 Carbon model details · What’s new (2026-11-11) · Gemini API pricing (2026-11-11) · Google announcement (Gemini 4 Carbon Now Available, 2026-11-10). SDK fields, prices, and channels follow Google’s latest docs; API Hop’s Carbon support status follows the pricing page.
