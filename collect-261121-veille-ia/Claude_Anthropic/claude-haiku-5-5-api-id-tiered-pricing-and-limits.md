---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-api-id-tiered-pricing-and-limits
title: "Claude Haiku 5.5: API ID, Tiered Pricing and Limits"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "claude", "tokens", "prompts", "anthropic", "output", "text", "model"]
source: https://aicraftbook.com/models/claude-haiku-5-5
source_anchor: ""
source_lines: [1, 274]
section: "Claude_Anthropic"
---
Model by Anthropic

# Claude Haiku 5.5

Claude Haiku 5.5 is Anthropic's fastest current model, launched on 7 October 2026 for high-volume, latency-sensitive work such as classification, extraction and routing. Short prompts start at $0.10 for input and $0.50 for output per million tokens, and requests over 100,000 tokens pay higher rates. Like the bigger Claude models, it reads up to 1M tokens and writes up to 128K.

Updated . Sources checked . 8 sources

- Status
- Active
- Context window
- 1M tokens
- Input price
- $0.1 / M tokens
- Output price
- $0.5 / M tokens
- Prices checked
- 21 Oct 2026

## What can it do?

- Accepts
- Text, Image
- Produces
- Text

Listed capabilities are documented by Anthropic; linked ones go to the page that documents them.

- **First Haiku with adaptive thinking and effort.** All five effort levels are supported, with`medium` as the default.`low` is the cheapest and fastest setting.
- **Thinking you can switch off.**`thinking: disabled` works at`high` effort or below. Unlike Sonnet 5.5, it does not accept`between_tools` .
- **Big window for a small model.** 1M tokens of context and 128K output, against 200K and 64K on Haiku 4.5.
- **Browser use tool.** New support on the Claude API and Google Cloud, alongside tool calling, structured outputs, computer use and web search.
- **Images in, text out.** It can read photos of forms, receipts and screenshots.
- **Our view:** its price makes it the natural choice for classifiers, extractors and subagents, as long as prompts stay under 100,000 tokens.

## What is it best for?

- **Lead classification.** A brokerage sends each web-form submission. Haiku 5.5 returns JSON with lead type (buyer, seller, renter, spam), phone number and property address.
- **Invoice field extraction.** An accounts team uploads photos of invoices. It returns supplier, invoice number, date and total as JSON for a person to check.
- **Ticket triage.** A support desk passes each new ticket. It returns a category, priority and suggested queue.
- **Subagent summaries.** A research agent built on Opus 5.5 hands Haiku 5.5 each fetched web page. It returns a 100-word summary with the key facts, so the orchestrator reads less.
- **Live chat first replies.** A retailer's chat widget uses it to answer order-status questions from order data passed in the prompt.

## When should you use it?

**Our recommendation:**

- **Sorting and routing leads.** Haiku 5.5 suits deciding whether a form submission is a buyer, seller, renter or spam, pulling the phone number and property address out of a message, or tagging a CRM note. These are short prompts in large numbers, which is where its price shines.
- **Short lead replies at volume.** Once a reply prompt is stable, try Haiku 5.5 against Claude Sonnet 5.5 on the same leads. If the replies hold up, the saving is large. Keep Sonnet while you are still tuning tone.
- **Not for heavy reasoning.** For multi-step agent work, careful analysis or anything where a wrong answer is costly, use Sonnet 5.5 or Claude Opus 5.5.
- **Keep prompts under 100,000 tokens** where you can. Above that, every token in the request costs five times as much.

## What are its strengths?

- **Fastest in the lineup.** Anthropic's comparison table rates its latency "fastest" among current models.
- **Very low cost for short prompts.** At $0.10 input and $0.50 output per million tokens for prompts up to 100,000 tokens, it is the cheapest current Claude model. Its input costs a tenth of Claude Haiku 4.5's $1.
- **Built for volume.** Anthropic positions it for classification, routing, extraction, summarising, compacting context and subagent work beside larger models. Its announcement also names live customer support and browser use as speed-sensitive fits.
- **Large window when you need it.** It has the same 1M-token context and 128K output as the larger models, up from 200K and 64K on Haiku 4.5, though long prompts cost more (see Limitations).
- **Text and images in, text out** , with adaptive thinking. The default effort on the Claude API is`medium` .
- **Published results.** Reported by Anthropic in October 2026: 72.4% on OSWorld 2.1 (offline subset), 57.4% on Humanity's Last Exam with tools and 46.4% on FrontierCode 1.1 (Main).

## What are its limitations?

- **Price jumps above 100,000 tokens.** A request whose prompt exceeds 100,000 tokens pays the higher prices for the whole request, cache reads and writes included. Each request is priced on its own.
- **More tokens for the same text.** It uses the newer tokenizer from Claude Opus 4.7 onward. The same text counts as about 30% more tokens than on Claude Haiku 4.5, so compare costs on real prompts.
- **Breaking changes from Haiku 4.5.** Manual extended thinking (`budget_tokens` ), assistant prefill and the`computer_20250124` tool all return errors. Changing earlier turns invalidates thinking blocks.
- **No sampling controls.** Anthropic says to omit`temperature` ,`top_p` and`top_k` . From API version 2026-10-21 they return a 400 error on every model.
- **No server-side fallback.** Safety classifiers can decline requests, and server-side fallback is not available for Haiku 5.5.
- **Thinking blocks are account-bound.** They only work in the account that produced them, or in a linked account.
- **Knowledge cutoff.** Its reliable knowledge runs to June 2026. Give it current facts in the prompt.
- **Output is text only.** It reads images but does not create them.

## Technical specifications

- Family
- Claude 5
- Version
- 5.5
- API model ID
- `claude-haiku-5-5`
- Status
- Active
- Released
- 7 Oct 2026
- Context window
- 1,000,000 tokens
- Maximum output
- 128,000 tokens
- Tool calling
- Documented
- Structured output
- Documented
- Reasoning controls
- Documented
- Open weights
- No
- Licence
- Proprietary (Anthropic API terms)
- Where it runs
- Provider API, Consumer app, Amazon Bedrock, Google Vertex AI, Microsoft Azure

- **Effort:**`low` ,`medium` (default),`high` ,`xhigh` and`max` . Anthropic suggests`medium` for most work,`low` for chat and simple high-volume calls, and`high` for knowledge work and strict instruction following.
- **Per-message effort:** on the Claude API and Google Cloud. It cannot change mid-conversation while thinking is disabled.
- **Output:** 128K tokens, or up to 300K on the Message Batches API with the`output-300k-2026-03-24` beta header.
- **Thinking compatibility:** on the Claude API and Google Cloud, Opus 5.5 and Sonnet 5.5 read Haiku 5.5's thinking blocks. Haiku 5.5 cannot read blocks from Opus 5, Opus 5.5, Sonnet 5.5 or any Fable or Mythos model.
- **Pricing tiers:** the 100,000-token threshold counts all input tokens, including cache reads and writes.
- **Cutoff:** reliable knowledge runs to June 2026.

## How much does it cost?

| Item | Price | 
|---|---|
| Input, prompts up to 100,000 tokens | $0.10 / MTok | 
| Output, prompts up to 100,000 tokens | $0.50 / MTok | 
| 5-minute cache write, prompts up to 100,000 tokens | $0.125 / MTok | 
| 1-hour cache write, prompts up to 100,000 tokens | $0.20 / MTok | 
| Cache hits, prompts up to 100,000 tokens | $0.01 / MTok | 
| Input, prompts over 100,000 tokens | $0.50 / MTok | 
| Output, prompts over 100,000 tokens | $2.50 / MTok | 
| 5-minute cache write, prompts over 100,000 tokens | $0.625 / MTok | 
| 1-hour cache write, prompts over 100,000 tokens | $1 / MTok | 
| Cache hits, prompts over 100,000 tokens | $0.05 / MTok | 
| Batch API, prompts up to 100,000 tokens | $0.05 input / $0.25 output per MTok | 
| Batch API, prompts over 100,000 tokens | $0.25 input / $1.25 output per MTok | 
| US-only inference (inference_geo: "us") | 1.1x all token prices | 

Prices as published on Anthropic's pricing page on 21 Oct 2026, in USD. Haiku 5.5 is priced by prompt length. Check the pricing page before you budget.

API prices as published by the provider, checked 21 Oct 2026. Consumer app subscriptions are priced separately. Prices change; confirm on the provider's pricing page before you budget. Compare prices across all models. How we check prices.

### What a real job costs

A lettings agency classifies 100,000 web-form submissions a month. Each prompt is about 600 tokens and each answer is a 30-token JSON label.

- Input: 60M × $0.10/M = $6.00
- Output: 3M × $0.50/M = $1.50
- **Total: $7.50 a month** , or $3.75 through the Batch API

The over-100,000 tier changes the sums. One 150,000-token contract with a 2,000-token summary costs 150K × $0.50/M + 2K × $2.50/M = $0.075 + $0.005 = $0.08.

## How do you access it?

- **Claude API:** model ID`claude-haiku-5-5` . The ID is a pinned snapshot, and the alias is the same string.
- **Amazon Bedrock:**`anthropic.claude-haiku-5-5` .
- **Google Cloud (Vertex AI):**`claude-haiku-5-5` .
- **Microsoft Foundry** and**Claude Platform on AWS:**`claude-haiku-5-5` .
- **Claude apps:** Anthropic publishes the system prompt Haiku 5.5 uses on claude.ai and the Claude apps, so it is offered there. Which plans include it is not stated on the pages we read.

Bedrock and Google Cloud set their own prices and lifecycle dates. Anthropic commits to keeping it available on its own platforms until at least 7 October 2027.

```
import anthropic
client = anthropic.Anthropic()
message = client.messages.create(
    model="claude-haiku-5-5",
    max_tokens=1000,
    messages=[{"role": "user", "content": "Hello, Claude"}],
)
for block in message.content:
    if block.type == "text":
        print(block.text)
```
Minimal call in the shape of the provider's quickstart (source). Set your API key as the docs describe. Not run by us.

## Example prompts and applications

Illustrative prompts. The expected results describe a typical answer, not a recorded run.

1. `Classify this form submission as buyer, seller, renter or spam. Reply with JSON only: {"type": ..., "phone": ..., "address": ...}. Use null for anything missing.` Expect one JSON object.
2. `Read this ticket and return JSON with category, priority (P1 to P4) and a one-line reason.` Expect one JSON object. Our ticket triage JSON prompt is a fuller version.
3. `Extract the supplier name, invoice number, invoice date and total from this invoice image as JSON.` Expect four fields. Check totals against the image before posting.

### AI Craftbook prompts that suit it

## How does it compare with alternatives?

- Claude Sonnet 5.5 is better for tone-sensitive replies and multi-step agent work. Its input costs $2 per million against $0.10.
- GPT-6 Luna, OpenAI's high-volume GPT-6 model, lists the same $0.10/$0.50 and a 1.05M window. It suits teams already built on OpenAI's API.
- Gemini 3.5 Flash-Lite costs $0.30/$2.50 and accepts audio, video and files. It is the better pick for parsing media that Haiku cannot read.
- Qwen3.8-Flash lists at $0.15/$0.47 on Alibaba's API. Its base model has open weights, so teams that must host a model themselves can start there. See Claude Haiku 5.5 vs Qwen3.8-Flash.

Also compared with Claude Haiku 5.5 on their own pages: Claude Sonnet 4.5, Claude Sonnet 4.6, Gemini 2.5 Flash-Lite, Gemini 3.8 Flash, GPT-5.4 Mini, GPT-5.4 nano, GPT-5.6 Luna, Mistral OCR 4.1, Amazon Nova 2 Lite, Perplexity Sonar, Rerank 5 Pro.

## How do you put it to work?

- **Lead routing.** In the n8n lead follow-up automation, use Haiku 5.5 for the classify step and a larger model for the reply if needed.
- **Invoices.** The invoice processing playbook and the n8n invoice extraction automation fit its extraction strengths.
- **Subagents.** In a research agent, let Haiku 5.5 summarise pages while Opus 5.5 plans.
- **Watch prompt size.** Log input tokens per request. Anything over 100,000 moves the whole request to the higher tier.

## What we could not confirm

- Which Claude subscription plans include Haiku 5.5 is not stated on the pages we read.
- Benchmarks are reported by Anthropic; the OSWorld 2.1 figure is on an offline subset of the benchmark.

## Published benchmark figures

| Benchmark | Result | Published by | 
|---|---|---|
| GDPval-AA v2.1 | 1620 | Anthropic, 2026-10 | 
| AA-Briefcase v1.1 | 1578 | Anthropic, 2026-10 | 
| OSWorld 2.1 (offline subset) | 72.4% | Anthropic, 2026-10 | 
| Humanity's Last Exam (no tools) | 45.9% | Anthropic, 2026-10 | 
| Humanity's Last Exam (with tools) | 57.4% | Anthropic, 2026-10 | 
| Terminal-Bench 4.0 | 39.2% | Anthropic, 2026-10 | 
| FrontierCode 1.1 (Main) | 46.4% | Anthropic, 2026-10 | 
| Chartography (no tools) | 46.4% | Anthropic, 2026-10 | 

Reported by the publisher named in each row, not measured by us. Benchmarks measure narrow tasks; test on your own work before you decide.

## In-depth comparisons

- Claude Haiku 5.5 vs Claude Sonnet 5.5: Anthropic's small and mid Claude models: Haiku 5.5 from $0.10 / $0.50 for classification, extraction and routing, Sonnet 5.5 at $2 / $10 for everyday writing, coding and agents. When the small model can take over.
- Claude Haiku 5.5 vs Gemini 3.8 Flash: Anthropic's cheapest Claude at $0.10 / $0.50 against Google's Flash at $0.75 / $3.75. Haiku wins on price for short text and image jobs; Flash reads video, audio and PDFs and is built for agents.
- Claude Haiku 5.5 vs GPT-6 Luna: Two ten-cent models for tagging, extraction and routing. Same short-prompt price, very different rules once prompts grow past 100K tokens, and different clouds.
- Claude Haiku 5.5 vs Qwen3.8-Flash: Two low-cost models for tagging, extraction and routing: Anthropic's fastest Claude against Alibaba's Qwen API model with open base weights, video input and flat 1M pricing.

Reviewed by our editors, with use cases, worked cost examples and sources. Or compare Claude Haiku 5.5 with any model.

## How it connects

Connections

- Follow up with real estate leads automaticallyAI Playbook
- Process invoices automaticallyAI Playbook
- Claude Sonnet 5.5Model
- Claude Haiku 4.5Model
- Claude Sonnet 4.5Model
- Claude Sonnet 4.6Model
- GPT-6 LunaModel
- Gemini 2.5 Flash-LiteModel
- Gemini 3.8 FlashModel
- GPT-5.4 MiniModel

## Common questions about Claude Haiku 5.5

## What is the API model ID for Claude Haiku 5.5?

`claude-haiku-5-5` on the Claude API, Google Cloud, Microsoft Foundry and Claude Platform on AWS. On Amazon Bedrock it is `anthropic.claude-haiku-5-5`.

## Why does Claude Haiku 5.5 pricing say "from"?

Haiku 5.5 is priced by prompt length. Prompts up to 100,000 tokens cost $0.10 per million input tokens and $0.50 per million output tokens. Longer prompts cost $0.50 and $2.50, applied to the whole request.

## Is Claude Haiku 5.5 good enough to reply to customers?

Anthropic names live customer support as a fit for Claude Haiku 5.5. Our advice is to try it against Sonnet 5.5 on your own messages before switching, because tone and accuracy matter more than speed in a first reply.

## Can I turn off thinking on Claude Haiku 5.5?

Yes, you can turn off thinking on Claude Haiku 5.5 with `thinking: disabled` at `high` effort or below. At `xhigh` or `max` that returns a 400 error.

## When will Claude Haiku 5.5 be retired?

Anthropic commits to keeping Claude Haiku 5.5 available until at least 7 October 2027.

## Sources and how we wrote this

Written by AI Craftbook editors from the sources below, last substantively updated 21 Oct 2026. Technical facts come from these sources. Recommendations and comparisons are our editorial judgement. Examples marked illustrative were written by us, not captured from a recorded run. See our editorial standards.

1. Claude Haiku 5.5 overview, Anthropic. Read 21 Oct 2026. Model IDs per platform, context, max output, tiered pricing, release date, tokenizer note.
2. Models overview, Anthropic. Read 21 Oct 2026. Lineup comparison, latency, default effort, retirement dates.
3. Pricing, Anthropic. Read 21 Oct 2026. Prompt-length pricing, batch prices and data residency multiplier.
4. Claude Haiku 5.5 announcement, Anthropic. Read 21 Oct 2026. Intended use cases and availability.
5. What's new in Claude Haiku 5.5, Anthropic. Read 21 Oct 2026. Changes and breaking changes from Haiku 4.5, browser use tool.
6. Model deprecations, Anthropic. Read 21 Oct 2026. Status Active; retirement not sooner than October 7, 2027.
7. Thinking, Anthropic. Read 21 Oct 2026. Thinking billed as output; thinking-block compatibility between models.
8. Effort, Anthropic. Read 21 Oct 2026. Effort levels and recommended settings per model.

## Update history

- Editorial rewrite from Anthropic's model overview and pricing docs. Added the API ID, limits, dated tiered pricing, access and our usage recommendation.
- Added structured specs, benchmarks, capabilities, use cases, a worked cost example, comparisons, example prompts and uncertainties. Facts re-checked against Anthropic's docs on 21 Oct 2026.
- Corrected the Qwen3.8-Flash alternative: the API model runs under Alibaba's API terms; only its base model has open weights. Linked the new comparison.
