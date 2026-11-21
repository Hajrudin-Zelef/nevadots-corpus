---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-vs-sonnet-5-5-18x-cheaper-and-just-as-accurate-in-a-three-task-api-test
title: "Claude Haiku vs Sonnet 5.5: 18x Cheaper and Just as Accurate in a Three-Task API Test"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "sonnet", "claude", "tasks", "million", "code", "test", "tokens"]
source: https://dev.to/ranthis/claude-haiku-vs-sonnet-55-18x-cheaper-and-just-as-accurate-in-a-three-task-api-test-4d1c
source_anchor: ""
source_lines: [1, 124]
section: "Claude_Anthropic"
---
*Originally published at ranthis.com. The scripts and raw answers are in the public repo.*

We gave Claude Haiku 5.5 and Sonnet 5.5 the same three office tasks through the API and graded them with code. Both scored 15/15, but Haiku was faster and cost 18 times less.

By the RanThis team, written with AI assistance · Updated October 17, 2026 · 6 min read

Why trust this: every result, number and screenshot comes from a real run on our own accounts, not from the AI. How we test

In our Claude Haiku vs Sonnet test, Claude Haiku 5.5 matched Claude Sonnet 5.5 on three short office tasks: both scored 15/15 on code-graded checks. Haiku also finished faster (11.7 s vs 15.7 s in total) and cost 18 times less ($0.00129 vs $0.02357). Sonnet's edge never showed up on these tasks; long, multi-step work and coding were not tested.

Tested on 13 October 2026 by calling the Claude API directly with `claude-haiku-5-5` and `claude-sonnet-5-5`, each at its default effort (Haiku: medium, Sonnet: high). Each task ran once, so this is a real-world observation, not a statistic. The tasks use made-up data and were written in Vietnamese, so this is also a test with non-English content. Prices come from Anthropic's official pricing page on the same day.

**Watch the real run (Vietnamese narration):** https://youtu.be/AqLwBZqVld8

- 
**Accuracy:** Haiku 5.5 and Sonnet 5.5 both scored 15/15 items graded by code (5 invoice fields, 6 commission figures, 4 email conditions).
- 
**Speed:** 11.7 seconds for all three tasks on Haiku vs 15.7 seconds on Sonnet.
- 
**Cost:** $0.00129 on Haiku vs $0.02357 on Sonnet, an 18× gap. Scaled to 1,000 runs of these tasks, that is about $1.30 vs about $23.60.
- 
**Instruction following:** each model slipped once on formatting. Sonnet ignored "No explanation" on the commission task; Haiku added a "[Company name]" signature after the hotline that was supposed to end the email.

On the price list, Claude Haiku 5.5 costs a twentieth of Claude Sonnet 5.5.

|  | Claude Haiku 5.5 | Claude Sonnet 5.5 | 
|---|---|---|
| Input price (per million tokens) | $0.10 (prompts up to 100,000 tokens) | $2 | 
| Output price (per million tokens) | $0.50 (prompts up to 100,000 tokens) | $10 | 
| Anthropic's description | Released 7 October 2026. For high-volume, latency-sensitive tasks such as classification, extraction and routing | The best combination of speed and intelligence | 

One Haiku 5.5 caveat: prompts **over 100,000 tokens** cost $0.50 input and $2.50 output per million tokens. Sonnet 5.5 keeps one price across its full 1M-token window. Sources: Anthropic's pricing page and the Claude Haiku 5.5 overview.

1. 
**Three office tasks** , with answers worked out by hand before running:  Read a messy email and extract invoice details as JSON. Traps: a mistyped amount that was later corrected, an old company name, and a due date written in words. Work out tiered sales commission for 5 staff: 2% / 4% / 6% by tier, plus a 1 million VND bonus for sales of 150 million VND or more with no returns. Traps: one person at exactly 200 million, one at 149.5 million (500,000 short of the bonus), and one with enough sales but a returned order. Write a reply to a customer whose order arrived late: at most 120 words, a 10% discount code, no promised delivery date, and end with the hotline.
2. 
**The same prompt** sent to each model through the API.
3. 
**Code grades each item** : 5 invoice fields, 6 commission figures, 4 email conditions. That is 15 items per model.
4. 
**Real measurements** : time from request to response, token counts reported by the API, and cost calculated from the price list.

Both models returned the same JSON with all 5 fields right: tax code, pre-tax amount of 12,450,000 VND (not the mistyped figure), tax of 996,000, total of 13,446,000, and a due date of 24 October 2026. Haiku took 3.9 seconds, Sonnet 5.2.


*Terminal output: the invoice JSON Claude Haiku 5.5 returned in 3.9 seconds, with all 5 fields graded correct by code*

Both got all 6 figures right, and neither paid the bonus to the person who was 500,000 short. The difference was in following the instruction "No explanation": Haiku returned only the table, while Sonnet wrote out its full working before the table.


*Commission answers side by side: Haiku 5.5 gives only the table, Sonnet 5.5 writes out its working before the table*

The two drafts are very similar: 110 and 113 words. Both apologize, give the warehouse move as the reason, offer the 10% code and promise no date. Sonnet added a subject line and put the hotline on the last line as asked. Haiku put a "[Company name]" signature after the hotline.


*The two customer emails side by side: Haiku 5.5 at 110 words, Sonnet 5.5 at 113 words, both with the 10% code and the hotline*

Across all three tasks, the Claude Haiku vs Sonnet totals look like this:

|  | Haiku 5.5 | Sonnet 5.5 | 
|---|---|---|
| Correct (graded by code) | 15/15 | 15/15 | 
| Total time, 3 tasks | 11.7 s | 15.7 s | 
| Total cost, 3 tasks | $0.00129 | $0.02357 | 
| These 3 tasks × 1,000 (scaled from the measurement) | about $1.30 | about $23.60 | 

Why 18× and not the 20× on the price list? Haiku used slightly more output tokens (thinking included): 674, 800 and 909 tokens, versus Sonnet's 640, 626 and 899.


*Terminal summary: Haiku 5.5 and Sonnet 5.5 both score 15/15; Haiku took 11.7 s and cost $0.00129, Sonnet took 15.7 s and cost $0.02357*

Below is the API call and cost calculation from the script we used (Python, `httpx`). Each task sends the same `prompt` to both models. A separate function compares the reply with the expected answer: it parses the JSON and checks each field (task 1), takes the last number on each table row (task 2), and counts words and looks for "10%", the hotline and any dates (task 3).


```
import time, httpx
PRICES = {"claude-haiku-5-5": (0.10, 0.50), "claude-sonnet-5-5": (2.00, 10.00)}  # USD per million tokens
def ask(model: str, prompt: str, api_key: str):
    started = time.monotonic()
    reply = httpx.post(
        "https://api.anthropic.com/v1/messages",
        headers={"x-api-key": api_key, "anthropic-version": "2023-06-01"},
        json={"model": model, "max_tokens": 8000, "messages": [{"role": "user", "content": prompt}]},
        timeout=300,
    ).json()
    text = "".join(b.get("text", "") for b in reply["content"] if b["type"] == "text")
    used = reply["usage"]
    price_in, price_out = PRICES[model]
    cost = used["input_tokens"] * price_in / 1e6 + used["output_tokens"] * price_out / 1e6
    return text, time.monotonic() - started, cost
```
We ran the same three-task method on Google's models too: see Gemini 4 Argon vs Gemini 3.8 Flash.

- 
**Each task ran once.** A rerun may word things differently.
- 
**All tasks were short with clear answers.** Long multi-step work, coding and large-document analysis were not tested. That is where a larger model usually has an edge, and it needs its own test.
- 
**Very long prompts** (over 100,000 tokens) make Haiku five times more expensive, narrowing the gap with Sonnet.

1. 
**Short, well-defined tasks** (extraction, rule-based calculation, templated writing): try Haiku first. In this test it matched Sonnet, ran faster and cost 18 times less, which makes it the cheapest Claude model worth starting with for this kind of work.
2. 
**Write a few lines of code to check the output** , as this test does, so you know the cheaper model is good enough for your work.
3. 
**For harder work, compare Claude Haiku vs Sonnet on your own tasks.** With the API, switching models is a one-line change to`model` .

If you have a Claude Max plan, you can run tests like this with its included API credit: see how to claim Claude Max API credits.

$0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens; $0.50 and $2.50 above that. The Batch API is 50% off. Prices are from Anthropic's official pricing page on 13 October 2026.

Not on the three short office tasks in this test: both scored 15/15 items graded by code, and each slipped once on formatting. Long, multi-step work and coding were not tested.

20 times by the price list, for prompts up to 100,000 tokens. In this test the actual total differed by 18 times ($0.02357 versus $0.00129), because Haiku used slightly more output tokens.

Haiku 5.5. It finished all three tasks in 11.7 seconds versus 15.7 seconds for Sonnet 5.5; on the invoice task alone it took 3.9 seconds versus 5.2.

Prepare a few tasks with known answers, send the same prompt to each model through the API, and let code compare the replies with the answers. Record the time and the `usage` token counts from the API response to work out cost from the price list.

1. Claude API — PricingVendor page
2. Claude Haiku 5.5 — model overviewVendor document
3. Claude models overviewVendor document

## Top comments (0)
