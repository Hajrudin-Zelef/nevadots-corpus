---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-vs-opus-5-which-to-use-and-what-it-costs
title: "Claude Fable 5.1 vs Opus 5: Which to Use and What It Costs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "model", "opus", "anthropic", "announcement", "costs", "mythos"]
source: https://blog.beerandcode.com.br/en/news/claude-fable-5-1-vs-opus-5
source_anchor: ""
source_lines: [1, 256]
section: "Claude_Anthropic"
---
Anthropic shipped **Claude Fable 5.1** today, September 1, 2026, alongside Mythos 5.1. It takes the top of the lineup: it's the most capable Anthropic model available to any customer, and it beats Opus 5 on practically every published benchmark.

But that's not the question that matters.

The question is: is it better **enough** for you to pay twice the price per token? Because Fable 5.1 input is $10/MTok versus $5 for Opus 5. Output is $50 versus $25. And on a good chunk of the benchmarks, the gap between the two is two or three percentage points.

In this post I lay out the official numbers from the announcement, run the real math on an agentic session with a warm cache (spoiler: the cost ratio isn't 2x), and build the decision tree between Fable 5.1, Opus 5 and Sonnet 5 — including Anthropic's own recommendation, which is a lot less enthusiastic than the announcement suggests.

## TL;DR

- **What it is:** Claude Fable 5.1, Anthropic's most capable generally available model. Successor to Fable 5, released on September 1, 2026.
- **Model ID:**`claude-fable-5-1` · 1M token window · 128k output · adaptive thinking always on.
- **Cost/Access:** $10 input / $50 output per million tokens. Cache read dropped to $0.25/MTok. Available on the Claude API, Claude Code, claude.ai, AWS, Google Cloud and Microsoft Foundry.
- **Useful links:** official announcement and what changed in the API.

## What changed in Claude Fable 5.1

Fable 5.1 is an extension of Fable 5, not a new model built from scratch. Same tokenizer, same 1M window, same input and output price. What actually changed:

**Cache read dropped 75%.** This is the number that matters most and shows up least in the headlines. On every Claude model, a cache read costs 0.1x the input price. On Fable 5.1 (and Mythos 5.1) it costs **0.025x** — $0.25 per million tokens, versus $1 on Fable 5. It's the first time Anthropic has broken that multiplier. According to the announcement, this cuts cost by ~25% for typical workloads and up to ~45% for heavy agentic work.

**Capability concentrated in long sessions.** The documentation lists six areas of improvement: agentic coding in hours-long sessions, document/spreadsheet/slide work, multistep research, vision on dense PDFs, reasoning across the 1M window, and computer use. Notice the pattern: they're all long-horizon tasks. Nobody promised it answers a classification prompt any better.

**Mythos 5.1 is the same model with a different set of safeguards**, released only to Project Glasswing participants — verified research in cybersecurity and life sciences. If you're not in the program, that model doesn't exist for you.

Here's the boring note nobody reads before migrating: Fable 5.1 and Mythos 5.1 require **30-day data retention**. They don't run under zero data retention without express authorization from Anthropic, and a request from a ZDR org comes back with `400 invalid_request_error`. If your company has a ZDR clause in its contract, the model discussion was over before it started.

Picking a model has become a weekly decision, and a wrong weekly decision becomes a fixed line on the bill every month. That's the kind of math we run live, every week, in the Clã Beer and Code: evals running on the student's own code, not announcement benchmarks.

## How much better than Opus 5, really

On to the numbers. Everything below comes from Anthropic's official announcement:

| Benchmark | Fable 5.1 | Opus 5 | Fable 5 | 
|---|---|---|---|
| Terminal-Bench-Science 0.1 | **52.6%** | 29.0% | 24.7% | 
| Terminal-Bench 4.0 | **55.8%** | 52.3% | 42.0% | 
| AutomationBench | **31.4%** | 26.9% | 17.1% | 
| CursorBench 3.2.0 | **73.4%** | 70.0% | 70.5% | 
| OSWorld 2.0 (strict) | **41.7%** | 39.6% | 36.1% | 
| Humanity's Last Exam (no tools) | **60.9%** | 56.6% | 57.8% | 
| Humanity's Last Exam (with tools) | **65.0%** | 63.6% | 63.8% | 
| GDPval-AA v2 (knowledge work, Elo) | **1853** | 1824 | 1723 | 

Read that table like an engineer, not a fan.

**One row is a massacre.** Terminal-Bench-Science: 52.6% versus 29.0%. That's almost double Opus 5, on a benchmark that measures the agent doing real scientific work in the terminal — installing dependencies, running experiments, interpreting errors, trying again. That's the real gain.

**The rest is a narrow margin.** Terminal-Bench 4.0: 3.5 points. CursorBench: 3.4 points. OSWorld: 2.1 points. HLE with tools: 1.4 points. GDPval: 29 Elo points, which is noise for a good share of use cases. These are real, consistent gains — Fable 5.1 wins every row — but none of them alone justifies doubling the price per token.

The Vals Index, which is an independent evaluation, tells the same story: Fable 5.1 in first at 67.87%, Opus 5 at 67.21%, Fable 5 at 66.04%. A 0.66-point gap between the best model in the world and the sibling that costs half as much.

The honest read is this: **Fable 5.1 isn't a smarter Opus 5. It's an Opus 5 that can stay awake longer.** The longer and more autonomous the task, the bigger the gap. On a single-turn task, the two are tied.

And benchmark gains aren't the only variable. If you followed the discussion about the perceived regression in Opus 5, you know that what changes the day-to-day experience is rarely the number on the chart.

## The math that matters: price per token vs price per task

Price per token is the wrong metric. What leaves your account at the end of the month is **price per completed task**, and that's where the $0.25 cache read changes the game.

Official pricing table, in dollars per million tokens:

| Model | Input | Cache write 5m | Cache read | Output | 
|---|---|---|---|---|
| Claude Fable 5.1 | $10.00 | $12.50 | **$0.25** | $50.00 | 
| Claude Opus 5 | $5.00 | $6.25 | $0.50 | $25.00 | 
| Claude Sonnet 5 | $2.00 | $2.50 | $0.20 | $10.00 | 
| Claude Haiku 4.5 | $1.00 | $1.25 | $0.10 | $5.00 | 

Look at how absurd that is: Fable 5.1's cache read is **half** of Opus 5's, even with input costing twice as much. And it's 25% more expensive than Sonnet 5's, a model five times cheaper on input.

This matters because an agent is a context-rereading machine. Simulate a realistic coding agent: a 120k-token cached prefix (system prompt, tool definitions, project files), 200 turns, 3k of new input and 2k of output per turn.

| Line item | Fable 5.1 | Opus 5 | Sonnet 5 | 
|---|---|---|---|
| Cache write (1x, 120k) | $1.50 | $0.75 | $0.30 | 
| Cache reads (200 x 120k = 24M) | $6.00 | $12.00 | $4.80 | 
| New input (600k) | $6.00 | $3.00 | $1.20 | 
| Output (400k) | $20.00 | $10.00 | $4.00 | 
| **Total** | **$33.50** | **$25.75** | **$10.30** | 

Fable 5.1 costs **1.3x Opus 5** in this session, not 2x. The cheap cache read eats more than half of the price difference, because cache reads dominate the token count in a long session. The longer the loop, the more that ratio shrinks.

Now flip it: on a single-turn call with no cache, you pay the full 2x and get 1.4 benchmark points. There the math doesn't work at all.

And Sonnet 5 is still the best deal on the table for 80% of what most people build: a third of the price of Opus 5 in the same simulation. (If you want to see this math broken down layer by layer on a real agent, we've already detailed where the money goes in a production agent.)

Do not just follow the news — master it. Hands-on AI Engineering, live, every week, in the largest community in Brazil.

Join the Clã
## When to use Fable 5.1, Opus 5 or Sonnet 5

Anthropic's official recommendation is surprisingly restrained. It's in the model selection doc, in plain, dry terms:

"For most workloads, start with Claude Opus 5. Use Claude Fable 5.1 for demanding reasoning and long-horizon agentic work, or when your evals on Claude Opus 5 at higher effort still fall short."


Here's what that means in practice: **Fable 5.1 is not the default.** It's the step you climb after Opus 5 at `effort: xhigh` or `max` has already failed your eval. If you don't have an eval, you have no way of knowing whether you need it — and you probably don't.

If this sounds familiar, it's because it's the same reasoning we walked through a generation ago in Fable 5 or Opus 4.8: which one to use. The numbers change, the method doesn't.

One detail a lot of people ignore: the `effort` parameter is a better lever than switching models. It trades intelligence for latency and cost **within the same model**, without invalidating the cache and without migrating code. Before you move up a model, turn up the effort.

The decision tree I use:

**1. Is the task single-turn or high-volume?** Classification, extraction, summarization, routing, cheap sub-agent. → **Haiku 4.5** or **Sonnet 5**. A top-of-the-line model here is money on fire.

**2. Is it everyday dev work?** Code generation, data analysis, tightly scoped agentic tool use, content. → **Sonnet 5**. It's the sweet spot of the lineup, and $2/$10 with a $0.20 cache read is hard to beat.

**3. Is it complex agentic work, a big refactor, a whole system, computer use, heavy vision?** → **Opus 5**. Start here, optimize your prompt for it, measure.

**4. Is the Opus 5 eval at `xhigh`/`max` still not passing? Does the session run for hours unsupervised? Is it deep multistep research or scientific work in the terminal?** → **Fable 5.1**. This is where the 23-point gap on Terminal-Bench-Science justifies the price.

| If you need... | Start with | Example | 
|---|---|---|
| The highest capability available | Fable 5.1 | Agent sessions that run for hours, multistep deep research, analysis carried all the way to a finished document/spreadsheet/deck | 
| Complex agentic coding and enterprise work | Opus 5 | Multi-hour autonomous agents, large-scale refactors, systems engineering, vision-heavy workflows | 
| Everyday speed and capability | Sonnet 5 | Code generation, data analysis, content creation, agentic tool use | 
| Lowest latency and price, with reasoning | Haiku 4.5 | Real time, high volume, sub-agents | 

One last lever before moving up a model: a multi-model strategy. A cheap executor that escalates hard decisions to an expensive advisor, or an orchestrator that delegates bulk work to a worker. Most of the tokens get billed at the low rate. That usually pays off more than swapping the whole model.

## What breaks if you swap the model id today

Migrating from Fable 5 or Opus 5 to Fable 5.1 isn't just editing the string. There are three real breaking changes.

### 1. Forced tool use returns 400

If your code forces a tool, it breaks. `tool_choice` with `{"type": "any"}` or `{"type": "tool", "name": "..."}` returns `400 invalid_request_error` — including on the token counting endpoint.

```
# QUEBRA no Fable 5.1
response = client.messages.create(
    model="claude-fable-5-1",
    max_tokens=8192,
    tools=[extrair_pedido],
    tool_choice={"type": "tool", "name": "extrair_pedido"},  # 400
    messages=[{"role": "user", "content": texto}],
)
# tool_choice: type "tool" and "any" are not supported for this model.
```
The reason is interesting: thinking is always on in these models, and a forced tool call would skip the reasoning. The model would end up writing its reasoning inside the tool arguments, which degrades argument quality.

The fix is `auto` + an explicit instruction + `strict: true`:

```
extrair_pedido = {
    "name": "extrair_pedido",
    "description": "Extrai os itens do pedido a partir do texto do cliente.",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "itens": {"type": "array", "items": {"type": "string"}},
        },
        "required": ["itens"],
        "additionalProperties": False,
    },
}
response = client.messages.create(
    model="claude-fable-5-1",
    max_tokens=8192,
    tools=[extrair_pedido],
    tool_choice={"type": "auto"},
    messages=[{
        "role": "user",
        "content": f"Use a tool extrair_pedido para responder.\n\n{texto}",
    }],
)
```
If the forced tool only existed to hand you back JSON, the right path is structured outputs (`output_config.format`), not tool use.

### 2. Thinking blocks are now bound to the model

Every thinking block records which model produced it, and preservation is one-way: Fable 5.1 reads thinking from earlier models, but no earlier model reads its thinking. If you have a router that switches models mid-conversation, the blocks get silently dropped before the model sees them. They aren't billed, but the reasoning is gone.

### 3. Editing earlier turns invalidates the thinking

This is the one that will catch the most people off guard. Touching anything **before** a Fable 5.1 thinking block — the `system`, the `tools` array, or an old message — kills the next request with `400 The block is bound to a different conversation`.

Patterns that invalidate everything from that point on:

- editing, reordering or removing an old turn while keeping the later ones;
- injecting a per-request reminder into an old turn and removing it on the next request;
- rebuilding the `system` or the`tools` array between requests in the same conversation;
- an image/document URL that serves different bytes on a later request.

The rule is to treat the conversation as **append-only**. Claude Code, claude.ai, Managed Agents and the Agent SDK already handle this. If your code builds the `messages` array by hand, run the check before migrating. The check is mandatory for accounts created on or after **August 31, 2026**; older accounts are only affected if they set `thinking.block_binding.prefix_mismatch_behavior`.

For a single-turn reminder, the new path is a turn-scoped system message instead of inject-and-delete:

```
{
    "role": "system",
    "clear_at": "next_user_message",
    "content": "Os resultados chegaram na sua inbox. Verifique antes de rodar mais código.",
}
```
And you can change `effort` mid-conversation without blowing the cache — turn it up for the hard step, down for the trivial one:

```
response = client.beta.messages.create(
    model="claude-fable-5-1",
    max_tokens=8192,
    output_config={"effort": "high"},
    messages=[
        {"role": "user", "content": "Refatora o módulo de billing."},
        {"role": "assistant", "content": "..."},
        # daqui pra frente, effort baixo
        {"role": "system", "content": [], "output_config": {"effort": "low"}},
        {"role": "user", "content": "Agora só resume o que mudou."},
    ],
    betas=["mid-conversation-output-config-2026-07-01"],
)
```
## Limitations and things to watch for

The announcement doesn't mention this, but the documentation does. These are behavior differences that show up **without you changing a single line of code**:

- **Parallel tool calls got more variable.** Where Fable 5 fired off several independent reads at once, 5.1 sometimes does one per turn. Quality doesn't drop, but you pay for more turns, more round trips and more wall-clock time. The fix is one line in the prompt asking it to batch independent tool calls.
- **Fewer progress updates during long tool runs.** It writes less text between tool calls, even more so at high effort. And since the default`thinking.display` is`"omitted"` , a long agentic turn looks frozen to the user. If your UI depends on narration, use`display: "updates"` (beta`thinking-display-updates-2026-08-18` ) and explicitly ask for an opening line, updates and a recap.
- **At `effort: low`, it answers from memory.** It calls search tools less. If the turn needs fresh information, turn up the effort or add a verification instruction. In a RAG agent, this is the kind of bug that passes the test and fails in production.
- **Rewrites the whole file for a small change.** When editing text, it tends to rewrite the entire file instead of making a targeted edit. The result is usually the same, the output cost isn't.
- **Denser prose and less formatting in chat.** Longer sentences, fewer paragraph breaks, less bold and fewer lists. An anti-formatting rule written for an older model now suppresses structure the content needs.
- **Unmarked quotes in summaries.** When summarizing a document, it more often reproduces passages from the source without marking them as quotes. If you publish that output, that's a risk.
- **Watermark and provenance.** All text generated by Fable 5.1 carries Anthropic's statistical watermark, and images/video produced via code execution come with C2PA Content Credentials. It doesn't change quality or add tokens, but it's there.

## Quick FAQ

**Why did my request come back with `400 invalid_request_error` just from swapping the model id?**
Three suspects, in this order: forced `tool_choice` (`any`/`tool`), `thinking` with `budget_tokens` or `{"type": "disabled"}`, and prefill of the last assistant message. All three are removed in Fable 5.1. Non-default `temperature`/`top_p`/`top_k` values land here too.

**Is it worth migrating from Opus 5 to Fable 5.1 now?**
Only if you have an eval and it's failing on Opus 5 at `xhigh` or `max`. Anthropic itself recommends starting with Opus 5. Without an eval, you'll trade 3 benchmark points for a 30% to 100% cost increase with no way to measure the return.

**Does the cheaper cache read make up for the 2x more expensive input?**
It depends on the shape of the workload. In a long agentic session that rereads a large prefix, it makes up for a good part of it — in this post's example the cost ratio dropped from 2x to 1.3x. On a single-turn call with no cache, it makes up for nothing.

**Can I use Fable 5.1 with a zero data retention contract?**
No, unless you have express authorization from Anthropic. It requires 30-day retention and is a Covered Model. A request from a ZDR org returns 400. In that scenario, Opus 5 is the practical ceiling.

## Conclusion

Claude Fable 5.1 is the best model available today for anyone paying for it, and the Terminal-Bench-Science number shows the gap to Opus 5 is real where it matters: long, autonomous tasks with nobody watching.

But the model decision is no longer about who wins on the chart. It's about workload shape. A large prefix reread many times changes the entire math, and the $0.25 cache read is the real news in this release — more than any benchmark percentage point.

The next predictable move is that cache multiplier coming down across the rest of the lineup. When it does, the conversation about agent cost changes again, and whoever has an eval in place will find that out in an afternoon. Whoever doesn't will find out on the bill.

Build the eval first. Then pick the model.

{AI Engineer} — apaixonado por Laravel, arquitetura de software e construir produtos com impacto. Compartilho aqui tutoriais, descobertas e reflexões sobre o dia a dia de engenharia.

There is no shortage of content. What is missing is someone to untangle it: what matters now is how to implement it the right way. In the Clã you get that live, every week, with people who have already filtered out the noise.

Join the Clã
