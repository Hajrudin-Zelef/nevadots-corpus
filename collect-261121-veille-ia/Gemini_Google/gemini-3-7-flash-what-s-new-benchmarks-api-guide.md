---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-what-s-new-benchmarks-api-guide
title: "Gemini 3.7 Flash: What's New, Benchmarks & API Guide"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "index", "model", "task", "googles", "reasoning", "artificial"]
source: https://deepinfra.com/blog/gemini-3-7-flash-model-overview-integration-guide
source_anchor: ""
source_lines: [1, 132]
section: "Gemini_Google"
---
Gemini 3.7 Flash is Google’s Flash-tier reasoning model. It takes text, images, video, audio and PDF files as input, returns text, and has a 1,000,000-token context window on DeepInfra. Google DeepMind released it on August 13, 2026, about three weeks after Gemini 3.6 Flash, and positioned it as its workhorse model for coding and agents.

It is a closed model. DeepInfra serves it as a partner model through Google’s API, so you call it with the same OpenAI-compatible endpoint, API key and bill you already use for open models such as DeepSeek-V4.1-Flash, GLM-5.3 and MiMo-V2.6-Flash. DeepInfra charges $0.75 per 1M input tokens and $3.75 per 1M output tokens, the same as Google’s current discounted standard price.

Google has since shipped newer Flash releases. Artificial Analysis now marks Gemini 3.7 Flash as deprecated and suggests Gemini 3.8 Flash instead. If you already run 3.7 Flash, or want it next to open models under one API, this guide covers what changed, how it benchmarks, what it costs and how to call it.

Model string: `google/gemini-3.7-flash`

Gemini 3.7 Flash keeps 3.6 Flash’s 1M-token context and input types. The changes are in agentic work, speed per task and price.

**Agentic coding (Google-reported).** Google’s launch figures, as summarised by MindStudio, show the largest gains on long-horizon software engineering: DeepSWE rises from 49% to 65.3%, and FrontierCode from 34.4% to 43.6%. Web Dev Arena moves from about 1538 to about 1588 Elo.

**Agents and knowledge work (independent).** Artificial Analysis attributes most of the improvement to agentic evaluations: Terminal-Bench v2.1 up 8 points, Tau3 Banking up 3 points, GDPval-AA v2 up 103 Elo to 1525, and its AA-Briefcase knowledge-work evaluation up 169 Elo to 1132.

**Speed per task.** Artificial Analysis measured about 340 output tokens per second at launch and an average of 1.7 minutes per Intelligence Index task at high reasoning. That put 3.7 Flash on its Intelligence vs. Time per Task Pareto frontier.

**Cost per task.** At Google’s discounted price, Artificial Analysis put Gemini 3.7 Flash (high) at $0.40 per Intelligence Index task, 30% less than Gemini 3.6 Flash. At medium reasoning, the figure fell to $0.26.

**Reasoning levels.** Google exposes three reasoning levels (high, medium, low), and Artificial Analysis benchmarked all three. The DeepInfra parameter for this is covered under Parameters.

These are vendor-reported figures from Google’s launch, via MindStudio’s write-up.

| Benchmark | Gemini 3.6 Flash | Gemini 3.7 Flash | 
|---|---|---|
| DeepSWE (long-horizon software engineering) | 49% | **65.3%** | 
| FrontierCode | 34.4% | **43.6%** | 
| Web Dev Arena (Elo) | ~1538 | **~1588** | 
| AutomationBench | ~17% | **~30%** | 

AutomationBench shows the biggest relative jump, though the model still fails most tasks on Google’s version of the test. DeepSWE is the strongest absolute result.

Artificial Analysis benchmarked the model ahead of release. These scores come from the version of the Intelligence Index in use in August 2026. They aren’t comparable with scores on the current index (v5.0.1), where Artificial Analysis no longer reports a score for this model.

| Metric (AA, August 2026) | Gemini 3.7 Flash | 
|---|---|
| Intelligence Index, high reasoning | 56 (+4 vs 3.6 Flash) | 
| Intelligence Index, medium / low reasoning | 53 / 51 | 
| Output speed | ~340 tokens/s | 
| Average time per Index task (high) | 1.7 min | 
| Cost per Index task (high / medium, discounted price) | $0.40 / $0.26 | 
| AutomationBench-AA | 62.7% (highest at launch) | 
| AA-AnalystAgent, pass^5 | 60% (highest at launch) | 
| AA-Briefcase | 1132 Elo | 
| GDPval-AA v2 | 1525 Elo | 

**Where it led and where it didn’t.** At launch, Gemini 3.7 Flash led AA’s AutomationBench-AA, ahead of Kimi K3 (53%) and GPT-5.6 Sol (51.2%), and AA-AnalystAgent, ahead of Claude Opus 5 (54%) and Fable 5 (49%). On the overall Index it sat just behind GPT-5.6 Terra and Muse Spark 1.2 (both 57). On GDPval-AA v2 its 1525 was level with GLM-5.2 (1506) and below DeepSeek V4 Flash 0731 (1558).

AutomationBench-AA (62.7%) is Artificial Analysis’s own version of the benchmark, so it doesn’t line up with Google’s AutomationBench figure above.

Artificial Analysis now lists Gemini 3.7 Flash as deprecated: it still benchmarks the model, but every 18 hours instead of every 6, and points to Gemini 3.8 Flash. Its current leaderboard is led by closed frontier models such as Claude Fable 5.2, Gemini 4 Carbon and Claude Opus 5.5. Gemini 3.7 Flash’s strengths are throughput and price per task on agentic and document work, not the top of that table.

| Spec | Gemini 3.7 Flash on DeepInfra | 
|---|---|
| Input | Text, images, video, audio, PDF | 
| Output | Text | 
| Context window | 1,000,000 tokens | 
| Reasoning | Yes (reasoning model) | 
| Features | OpenAI-compatible API, JSON mode, structured output, function calling, OCR | 
| Weights | Proprietary (no open weights) | 

PDF and OCR support make it a practical pick for document-heavy agent steps: reading contracts, invoices or slide decks and returning structured JSON. That matches its lead on AA-AnalystAgent, which tests questions about spreadsheets and documents.

Pricing as of 2026-11-17. The model page always shows current rates.

| Tier | Input / 1M | Output / 1M | Cached input / 1M | 
|---|---|---|---|
| Standard | $0.75 | $3.75 | Not listed | 

What DeepInfra offers for this model is consolidation, not a lower price. One API key, one invoice and one OpenAI-compatible client for Gemini and for open models. You don’t need a Google Cloud project, a second SDK or a second billing relationship.

A document-extraction step that sends 20,000 input tokens and returns 2,000 output tokens costs:

Reasoning tokens are billed as output, so higher reasoning effort raises the output side of that sum.

Gemini 3.7 Flash uses DeepInfra’s OpenAI-compatible API:

`https://api.deepinfra.com/v1/openai``POST /chat/completions``Authorization: Bearer $DEEPINFRA_API_KEY` (create a key in the dashboard)
The model requires a signed-in account with an API key; anonymous free calls aren’t available.

The full schema is on the model’s API page.

| Parameter | Type | Default | Notes | 
|---|---|---|---|
| `model` | string | — | `google/gemini-3.7-flash` | 
| `messages` | array | — | Chat history; supports multimodal content | 
| `reasoning_effort` | string | — | DeepInfra accepts `none` ,`minimal` ,`low` ,`medium` ,`high` ,`xhigh` ,`max` . Lower values cut reasoning tokens and latency | 
| `max_tokens` | integer | — | DeepInfra doesn’t publish a maximum output for this model, so set this explicitly for long outputs | 
| `temperature` | float | 0.7 | Sampling temperature | 
| `top_p` | float | 0.9 | Nucleus sampling | 
| `stream` | boolean | false | Server-sent events | 
| `stop` | array | — | Up to 16 stop strings | 
| `n` | integer | 1 | Up to 4 completions | 
| `presence_penalty` /`frequency_penalty` | float | 0 | −2 to 2 | 
| `response_format` | object | `{"type": "text"}` | `json_object` or`json_schema` for structured output | 
| `tools` /`tool_choice` | array / string | — | Function calling: `none` ,`auto` or`required` | 
| `seed` | integer | random | Determinism not guaranteed | 

`reasoning_effort` is part of the OpenAI SDK’s standard arguments, so you can pass it directly. Test the levels on your own prompts and compare quality, latency and output-token counts before choosing one for production.

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["DEEPINFRA_API_KEY"],
    base_url="https://api.deepinfra.com/v1/openai",
)
response = client.chat.completions.create(
    model="google/gemini-3.7-flash",
    messages=[
        {"role": "system", "content": "Extract fields as JSON."},
        {
            "role": "user",
            "content": "Invoice 4417 from Acme GmbH, dated 2026-10-02, total EUR 1,240.00. "
                       "Return invoice_number, vendor, date and total.",
        },
    ],
    reasoning_effort="low",
    response_format={"type": "json_object"},
    max_tokens=2048,
)
print(response.choices[0].message.content)
print(response.usage)
```
Switching between Gemini 3.7 Flash and an open model is a one-line change to `model`. This is useful for A/B tests on cost and quality, and for two-tier agents that send mechanical steps to a cheaper model and keep a stronger one for the hard steps.

The model page sets 1,000,000 tokens as the maximum context, counting input and output together. DeepInfra doesn’t state a separate maximum output for this model, so pass `max_tokens` whenever you expect long answers, and check `finish_reason` for `"length"` to catch truncated responses.

Gemini 3.7 Flash brought real gains over 3.6 Flash on agentic coding and document work, and was fast per task at launch. Artificial Analysis now points users to newer Gemini releases, so it’s the right choice mainly when you’ve already validated it, or need its PDF, audio and video input next to open models under one API.

On DeepInfra it costs the same per token as Google’s discounted rate, and it shares a key, an invoice and a client with the rest of your stack. If cached-input discounts or batch pricing drive your bill, compare Google’s own API before committing.

**Next steps**
