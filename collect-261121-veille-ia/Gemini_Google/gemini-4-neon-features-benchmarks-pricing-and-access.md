---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-features-benchmarks-pricing-and-access
title: "Gemini 4 Neon: Features, Benchmarks, Pricing, and Access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["neon", "google", "gemini", "carbon", "hydrogen", "model", "benchmarks", "tokens"]
source: https://www.datacamp.com/blog/gemini-4-neon
source_anchor: ""
source_lines: [1, 368]
section: "Gemini_Google"
---
Google's Gemini 4 family now has four models. Argon, Carbon, and Hydrogen arrived first, and on November 18, 2026, Google added Gemini 4 Neon, a model that sits between the fast, low-cost Hydrogen and the deep-reasoning Carbon.

Google describes Neon as "frontier-level intelligence with fast end-to-end task completion" for complex software engineering, knowledge work, and science. Its model card reports Neon ahead of Claude Opus 5.5 and GPT-6.2 Sol on 17 of 19 benchmarks, at $3 per million input tokens and $15 per million output tokens.

In this article, I'll cover everything new with Gemini 4 Neon, looking at the new features, exploring the benchmarks, and setting up a hands-on test against Gemini 4 Carbon. To learn more about the rest of the family, check out our guides to Gemini 4 Carbon and Gemini 4 Hydrogen.

To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

- Gemini 4 Neon is Google's new middle tier, built from Carbon and pitched as faster and leaner on tokens.
- On Google's own model card, it leads Opus 5.5 and GPT-6.2 Sol on most benchmarks, with large leads in long context and knowledge work.
- Google publishes no Neon-versus-Carbon scores, so the step down from Carbon is untested in the official numbers.
- Until November 30, Neon costs more than Carbon's introductory price; from December 1, it is the cheaper of the two.
- Use Hydrogen for everyday work, Neon when Hydrogen falls short, and Carbon for the hardest problems.

## What Is Gemini 4 Neon?

Gemini 4 Neon is the mid-tier model in Google's Gemini 4 lineup. Google places it between Gemini 4 Hydrogen, the default for everyday coding and agent work, and Gemini 4 Carbon, its strongest reasoning model. The model card says Neon is "adapted from Gemini 4 Carbon", so it shares Carbon's architecture and training data.

What changes is how much work Neon does per turn. Google's model guide says Neon "finishes faster with fewer output tokens than Carbon while handling harder reasoning than Hydrogen." Neon also defaults to the `high` thinking level and adds an `xhigh` level, while Carbon defaults to `medium` and caps at `high`.

The headline from Google's results table is breadth: Neon leads Claude Opus 5.5 and GPT-6.2 Sol on 17 of the 19 benchmarks reported. The comparison leaves out Google's own models, so it says nothing direct about how much quality Neon gives up against Carbon.

## Gemini 4 Neon Key Features

### Finish complex tasks with fewer tokens

Neon is built for complex work that runs often, where Carbon's extra thinking costs too much time and money. According to Google's release notes, it completes repository-level debugging and multi-file refactoring "in fewer steps with clean edits."

In long agent sessions with many tool calls, Google says Neon reduces redundant file reads, handles context compaction reliably, and minimizes fabricated claims and task shortcuts. These are vendor claims, and they are the ones a team can check fastest on its own agent logs.

### Tune reasoning with four thinking levels

You set how hard Neon thinks with `thinking_level`: `low`, `medium`, `high` (the default), or `xhigh`. Google recommends `medium` for interactive coding and responsive chat, `high` for most complex tasks, and `xhigh` only for hard, multi-step problems where `high` falls short.

The level matters for cost because Google bills thinking tokens at the output rate. Google's advice is to evaluate all four levels and pick the lowest one that meets your quality bar.

### Read and write across a 1M-token window

Neon shares one 1,048,576-token window across input, thinking, and output. A single synchronous response stops at 65,536 tokens, and longer outputs run in the background in 64K-token hops, up to the full window.

It accepts text, images, audio, video, and PDFs. On Google Cloud, it also reads Microsoft Office and OpenDocument files such as `.docx`, `.xlsx`, and `.odt` directly, which suits the dense document and spreadsheet work Google pitches it for.

### Pair Neon with Hydrogen in multi-model workflows

Because Carbon, Neon, and Hydrogen share one request schema, context window, and tool set, you can route each step of a workflow to a different model. Google Cloud's developer guide shows Neon planning and verifying a task, then passing `previous_interaction_id` to Hydrogen subagents for lower-latency execution.

Neon is also supported by the Antigravity agent in Gemini Managed Agents, on its own or paired with Hydrogen.

### Use it for cyber defense

Google says Neon writes code with fewer security vulnerabilities and improves root-cause vulnerability patching. The model card lists secure code generation, penetration testing, and vulnerability finding, validation, and patching among its intended uses, with dual-use offensive capabilities available to defenders through Google's Fairwind Program.

On Google's published table, Neon leads Claude Opus 5.5 and GPT-6.2 Sol on everything except two coding benchmarks and one chart-reading test. Its biggest leads are in long context, knowledge work, and the SWE-sweep provider harness. All scores below are vendor-reported, and the best score in each row is in bold.

### Knowledge work and long context

The standout result is GraphWalks, a long-context test from 256K to 1M tokens, where Neon scores 95.2% against 80.1% for GPT-6.2 Sol and 66.8% for Opus 5.5.

The rest of the knowledge-work rows show the same pattern, with smaller margins.

| Benchmark | What it measures | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|
| AA Index v5.0.1 | Capability index | **61** | 59 | 57 | 
| Vals Index v2.2 | Capability index | **70.9%** | 70.1% | 66.1% | 
| AutomationBench | Knowledge work | **56.3%** | 42.5% | 41.3% | 
| Agents' Last Exam v1.1 | Knowledge work | **75.8%** | 71.6% | 73.0% | 
| GraphWalks (256K to 1M, BFS F1) | Long context | **95.2%** | 66.8% | 80.1% | 

AutomationBench is the other wide gap, at nearly 14 points over Opus 5.5. The two capability indexes are close, so Neon's overall lead is smaller than the long-context row suggests.

### Coding and agentic workflows

Coding is where Neon is least dominant. It leads on CursorBench 4.1 with 65.4% against 58.1% for Opus 5.5, but trails Opus 5.5 on Terminal-bench 4.1 and FrontierCode 2.0.

| Benchmark | What it measures | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|
| CursorBench 4.1 | Agentic coding (score) | **65.4%** | 58.1% | n/a | 
| Terminal-bench 4.1 | Agentic coding (cheating-adjusted accuracy) | 65.2% | **65.8%** | 61.5% | 
| FrontierCode 2.0 | Agentic coding (main score) | 46.2% | **47.3%** | 43.8% | 
| SWE-sweep (provider harness) | Agentic coding | **39.0%** | 10.1% | 8.3% | 
| SWE-sweep (Mini-SWE harness) | Agentic coding | **12.8%** | 3.6% | 11.6% | 

Read the SWE-sweep rows with care. Under the provider harness, Neon scores nearly 4x as much as Opus 5.5, but under the shared Mini-SWE harness, GPT-6.2 Sol is within about 1 point of it, which suggests much of the gap comes from the harness.

### Science, math, and reasoning

Neon posts 84.0% on RiemannBench, 4 points ahead of both rivals.

| Benchmark | What it measures | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|
| RiemannBench | Science and math | **84.0%** | 80.0% | 80.0% | 
| Terminal-Bench-Science 0.1 | Science and math | **70.5%** | 63.3% | 68.0% | 
| HLE-Diamond | Reasoning | **71.3%** | 67.9% | 64.0% | 

The leads here are consistent but moderate, which fits Google's description of Neon as a model for high-frequency science workflows rather than the hardest proofs it reserves for Carbon.

### Multimodal understanding

Neon leads on video, with 89.0% on LVBench, but GPT-6.2 Sol beats it on chart reading.

| Benchmark | What it measures | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|
| MKA (Climb WSR) | Multimodal understanding | **56.5%** | 53.0% | 52.7% | 
| Chartography | Multimodal understanding (charts) | 73.5% | 66.3% | **75.1%** | 
| LVBench | Multimodal understanding (video) | **89.0%** | 83.7% | 87.6% | 

The multimodal spreads are small, so treat these rows as rough parity with GPT-6.2 Sol and a modest lead over Opus 5.5.

### Cybersecurity

Neon scores 90.5% on SEC-bench Pro, against 88.3% for GPT-6.2 Sol.

| Benchmark | What it measures | Gemini 4 Neon | Claude Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|---|
| CWE-Bench v2 | Cybersecurity | **70.7%** | 67.1% | 66.0% | 
| SEC-bench Pro | Cybersecurity | **90.5%** | n/a | 88.3% | 
| WebExploitBench v1.2 | Cybersecurity | **51.3%** | 37.1% | n/a | 

The WebExploitBench gap over Opus 5.5 is the largest in this group, at more than 14 points. Missing cells mean Google did not report that rival on that benchmark.

## Which Tier Should You Use?

Google's own guidance is to start with Hydrogen for most new projects and move up only when it is not enough. Neon is the step for harder reasoning, and Carbon is the step after that.

| Model | Speed | Thinking levels | Price per 1M tokens (input / output) | 
|---|---|---|---|
| Gemini 4 Carbon | Deliberate | `low` ,`medium` (default),`high` | $2.00 / $10.00 through November 30; $4.00 / $20.00 from December 1 | 
| Gemini 4 Neon | Moderate | `low` ,`medium` ,`high` (default),`xhigh` | $3.00 / $15.00 | 
| Gemini 4 Hydrogen | Fast | `low` ,`medium` ,`high` (default),`xhigh` | $1.50 / $7.50 | 
| Gemini 3.5 Flash-Lite | Fastest | `minimal` (default),`low` ,`medium` ,`high` | $0.30 / $2.50 | 

Here is how I would pick:

- **Everyday coding, interactive web apps, and parallel worker agents:** Hydrogen. It is half Neon's price and has a free tier.
- **Complex software engineering, dense documents and spreadsheets, and science workflows:** Neon, when Hydrogen gets the answer wrong or needs too many retries.
- **System architecture, multi-file debugging, formal proofs, and security audits:** Carbon, which Google says spends more thinking tokens per turn to test hypotheses and check fixes.
- **Translation, summarization, and structured JSON extraction at high volume:** Gemini 3.5 Flash-Lite.

Within Neon, start at `high`, drop to `medium` for interactive work, and keep `xhigh` for problems where `high` fails. Every step up costs more thinking tokens, and Google bills those at the output rate.

## Testing Gemini 4 Neon: Hands-On Examples

I gave Gemini 4 Neon and Gemini 4 Carbon the same task: build a single web page where the computer plays a complete game of chess against itself, from the opening move to checkmate or a draw. Each model had to write its own move generator and its own engine that searches ahead, draw the board, and keep a move list in standard chess notation.

The hard part is that every rule has to be right without the model ever seeing the page:

- the rare rules: castling (including when it is not allowed), en passant, and promotion to any piece;
- the endings: checkmate, stalemate, threefold repetition, the fifty-move rule, and insufficient material;
- notation that is correct to the character, such as `Nbd2` ,`e8=Q+` , and`Qxf7#` .

The task maps onto Google's claim that Neon handles complex software engineering in fewer steps with clean edits. I ran both models through the same harness with the same prompt and the same tools, three times each, and checked every move each game played. Both ran at `medium` thinking, which is the second of four levels for Neon and the second of three for Carbon.

Here is the end of Neon's first game at normal speed: White pushes two pawns through, promotes both, and mates with the second queen.

Neon built a working chess page in all three runs. Carbon built one in three, and the two failures came down to output length, not chess knowledge:

- **Does it know all the rules?** Yes, on every page that ran. I replayed every move of every game with the open-source chess.js library and asked each page for the legal moves in ten trap positions, such as en passant, castling through check, a pinned knight, underpromotion, and stalemate. Neon's three pages and Carbon's one page played 16 games without an illegal move or a notation error and answered all ten traps correctly.
- **Does it finish the job?** Neon finished all three runs in six or seven steps each. Carbon finished one: in the other two, it tried to write the whole game in a single response, and that response stopped at Gemini 4's limit of 65,536 output tokens before the file was saved.
- **Does it play real chess?** Mostly. Neon's engines develop their pieces, trade when ahead, and convert endgames, and all 12 of its games ended in checkmate. The weakest moment came in its second run, where White promoted a pawn to a rook that Black captured on the next move.

The difference is in how each model wrote the file. Neon thought for 13,000 to 17,000 tokens across a whole run and then wrote the page in several smaller edits, so no single response came near the limit. Carbon thought for about 40,000 tokens and then tried to write about 26,000 tokens of code in the same response, which pushed it past the limit before the file existed. Even Carbon's successful run came within about 1,500 tokens of the cap, and it was much slower.

Neon's three runs cost $0.95, $0.83, and $1.06. Carbon's working run cost $1.36, and its two failed runs cost $1.35 and $1.33 without producing a page.

On this task, Neon is the more dependable model: the same rules accuracy as Carbon, a working page every time, and a lower bill per run. That matches Google's pitch of fast end-to-end task completion. Carbon may reason more deeply, but here thinking longer was the problem: it spent its output budget before it wrote the answer.

Three runs per model on one task is a small sample, and Carbon's failures come from the output limit on a single response rather than from what it knows about chess. With Google's background mode for long outputs, Carbon would likely finish more often. What the test does show is that at `medium` thinking, Neon reliably finished a rules-heavy build that Carbon often could not.

## Gemini 4 Neon Pricing and Availability

Neon costs $3.00 per million input tokens and $15.00 per million output tokens on the Gemini API, flat across the full 1M-token window. Output includes thinking tokens, and implicit caching cuts cached input by 95%, to $0.15 per million tokens.

| Tier | Input (per 1M tokens) | Output (per 1M tokens) | Cached input (per 1M tokens) | 
|---|---|---|---|
| Standard | $3.00 | $15.00 | $0.15 | 
| Batch | $1.50 | $7.50 | $0.075 | 
| Flex | $1.50 | $7.50 | $0.075 | 
| Priority | $5.40 | $27.00 | $0.27 | 

The timing makes the comparison with Carbon unusual. Carbon's introductory price of $2.00 / $10.00 runs through November 30, 2026, so for the rest of November, Neon costs 50% more per token than Carbon; from December 1, Carbon moves to $4.00 / $20.00 and Neon becomes 25% cheaper per token.

There is no free tier: Neon runs only on paid Gemini API projects, while Hydrogen and Flash-Lite keep free access. Explicit context caching is not supported. In Google's apps, Neon is available to Google AI Pro and Ultra subscribers in the Gemini app, Google AI Studio, and Google Antigravity.

## How to Get Access to Gemini 4 Neon?

The API model ID is `gemini-4-neon`. Neon is available through the Gemini API (both `generateContent` and the Interactions API), Google AI Studio, the Gemini app, Google Antigravity, and Gemini for business on Google Cloud. It is also listed on OpenRouter as `google/gemini-4-neon`.

As of November 18, 2026, Neon is not listed in Cursor's model docs or the Gemini CLI model docs.

A minimal request with Google's Python SDK looks like this:

```
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-neon",
    input="Refactor this module and explain each change.",
    generation_config={"thinking_level": "medium"},
)
print(interaction.output_text)
```

If you already call another Gemini 4 model, switching only means changing the model ID, since the request schema is the same. For a full walkthrough of the shared Gemini 4 API, see our Gemini 4 Hydrogen API tutorial.

## Final Thoughts

Gemini 4 Neon fills the gap between Hydrogen and Carbon, and Google's benchmarks put it ahead of Claude Opus 5.5 and GPT-6.2 Sol on most of the tests it reports. The missing piece is a direct comparison with Carbon, the model it was adapted from, and that is the comparison that decides whether stepping down saves money without losing quality.

My recommendation: keep Hydrogen as your default, try Neon on the tasks where Hydrogen struggles, and wait until December 1 before moving work off Carbon, when Neon becomes the cheaper of the two.

To get hands-on with Google's AI tools, try our Practical AI with Google Gemini and NotebookLM course.

Google's Gemini 4 family now has four models. Argon, Carbon, and Hydrogen arrived first, and on November 18, 2026, Google added Gemini 4 Neon, a model that sits between the fast, low-cost Hydrogen and the deep-reasoning Carbon.

Google describes Neon as "frontier-level intelligence with fast end-to-end task completion" for complex software engineering, knowledge work, and science. Its model card reports Neon ahead of Claude Opus 5.5 and GPT-6.2 Sol on 17 of 19 benchmarks, at $3 per million input tokens and $15 per million output tokens.

In this article, I'll cover everything new with Gemini 4 Neon, looking at the new features, exploring the benchmarks, and setting up a hands-on test against Gemini 4 Carbon. To learn more about the rest of the family, check out our guides to Gemini 4 Carbon and Gemini 4 Hydrogen.

To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

## What Is Gemini 4 Neon?

Gemini 4 Neon is the mid-tier model in Google's Gemini 4 lineup. Google places it between Gemini 4 Hydrogen, the default for everyday coding and agent work, and Gemini 4 Carbon, its strongest reasoning model. The model card says Neon is "adapted from Gemini 4 Carbon", so it shares Carbon's architecture and training data.

What changes is how much work Neon does per turn. Google's model guide says Neon "finishes faster with fewer output tokens than Carbon while handling harder reasoning than Hydrogen." Neon also defaults to the

`high`thinking level and adds an`xhigh`level, while Carbon defaults to`medium`and caps at`high`.
The headline from Google's results table is breadth: Neon leads Claude Opus 5.5 and GPT-6.2 Sol on 17 of the 19 benchmarks reported. The comparison leaves out Google's own models, so it says nothing direct about how much quality Neon gives up against Carbon.

## Gemini 4 Neon Key Features

## Finish complex tasks with fewer tokens

Neon is built for complex work that runs often, where Carbon's extra thinking costs too much time and money. According to Google's release notes, it completes repository-level debugging and multi-file refactoring "in fewer steps with clean edits."

In long agent sessions with many tool calls, Google says Neon reduces redundant file reads, handles context compaction reliably, and minimizes fabricated claims and task shortcuts. These are vendor claims, and they are the ones a team can check fastest on its own agent logs.

## Tune reasoning with four thinking levels

You set how hard Neon thinks with

`thinking_level`:`low`,`medium`,`high`(the default), or`xhigh`. Google recommends`medium`for interactive coding and responsive chat,`high`for most complex tasks, and`xhigh`only for hard, multi-step problems where`high`falls short.
The level matters for cost because Google bills thinking tokens at the output rate. Google's advice is to evaluate all four levels and pick the lowest one that meets your quality bar.

## Read and write across a 1M-token window

Neon shares one 1,048,576-token window across input, thinking, and output. A single synchronous response stops at 65,536 tokens, and longer outputs run in the background in 64K-token hops, up to the full window.

It accepts text, images, audio, video, and PDFs. On Google Cloud, it also reads Microsoft Office and OpenDocument files such as

`.docx`,`.xlsx`, and`.odt`directly, which suits the dense document and spreadsheet work Google pitches it for.
## Pair Neon with Hydrogen in multi-model workflows

Because Carbon, Neon, and Hydrogen share one request schema, context window, and tool set, you can route each step of a workflow to a different model. Google Cloud's developer guide shows Neon planning and verifying a task, then passing

`previous_interaction_id`to Hydrogen subagents for lower-latency execution.
Neon is also supported by the Antigravity agent in Gemini Managed Agents, on its own or paired with Hydrogen.

## Use it for cyber defense

Google says Neon writes code with fewer security vulnerabilities and improves root-cause vulnerability patching. The model card lists secure code generation, penetration testing, and vulnerability finding, validation, and patching among its intended uses, with dual-use offensive capabilities available to defenders through Google's Fairwind Program.

## How Does Gemini 4 Neon Perform on the Benchmarks?

On Google's published table, Neon leads Claude Opus 5.5 and GPT-6.2 Sol on everything except two coding benchmarks and one chart-reading test. Its biggest leads are in long context, knowledge work, and the SWE-sweep provider harness. All scores below are vendor-reported, and the best score in each row is in bold.

## Knowledge work and long context

The standout result is GraphWalks, a long-context test from 256K to 1M tokens, where Neon scores 95.2% against 80.1% for GPT-6.2 Sol and 66.8% for Opus 5.5.

The rest of the knowledge-work rows show the same pattern, with smaller margins.

6170.9%56.3%75.8%95.2%
AutomationBench is the other wide gap, at nearly 14 points over Opus 5.5. The two capability indexes are close, so Neon's overall lead is smaller than the long-context row suggests.

## Coding and agentic workflows

Coding is where Neon is least dominant. It leads on CursorBench 4.1 with 65.4% against 58.1% for Opus 5.5, but trails Opus 5.5 on Terminal-bench 4.1 and FrontierCode 2.0.

65.4%65.8%47.3%39.0%12.8%
Read the SWE-sweep rows with care. Under the provider harness, Neon scores nearly 4x as much as Opus 5.5, but under the shared Mini-SWE harness, GPT-6.2 Sol is within about 1 point of it, which suggests much of the gap comes from the harness.

## Science, math, and reasoning

Neon posts 84.0% on RiemannBench, 4 points ahead of both rivals.

84.0%70.5%71.3%
The leads here are consistent but moderate, which fits Google's description of Neon as a model for high-frequency science workflows rather than the hardest proofs it reserves for Carbon.

## Multimodal understanding

Neon leads on video, with 89.0% on LVBench, but GPT-6.2 Sol beats it on chart reading.

56.5%75.1%89.0%
The multimodal spreads are small, so treat these rows as rough parity with GPT-6.2 Sol and a modest lead over Opus 5.5.

## Cybersecurity

Neon scores 90.5% on SEC-bench Pro, against 88.3% for GPT-6.2 Sol.

70.7%90.5%51.3%
The WebExploitBench gap over Opus 5.5 is the largest in this group, at more than 14 points. Missing cells mean Google did not report that rival on that benchmark.

## Which Tier Should You Use?

Google's own guidance is to start with Hydrogen for most new projects and move up only when it is not enough. Neon is the step for harder reasoning, and Carbon is the step after that.

`low`,`medium`(default),`high``low`,`medium`,`high`(default),`xhigh``low`,`medium`,`high`(default),`xhigh``minimal`(default),`low`,`medium`,`high`
Here is how I would pick:

Everyday coding, interactive web apps, and parallel worker agents:Hydrogen. It is half Neon's price and has a free tier.Complex software engineering, dense documents and spreadsheets, and science workflows:Neon, when Hydrogen gets the answer wrong or needs too many retries.System architecture, multi-file debugging, formal proofs, and security audits:Carbon, which Google says spends more thinking tokens per turn to test hypotheses and check fixes.Translation, summarization, and structured JSON extraction at high volume:Gemini 3.5 Flash-Lite.
Within Neon, start at

`high`, drop to`medium`for interactive work, and keep`xhigh`for problems where`high`fails. Every step up costs more thinking tokens, and Google bills those at the output rate.
## Testing Gemini 4 Neon: Hands-On Examples

I gave Gemini 4 Neon and Gemini 4 Carbon the same task: build a single web page where the computer plays a complete game of chess against itself, from the opening move to checkmate or a draw. Each model had to write its own move generator and its own engine that searches ahead, draw the board, and keep a move list in standard chess notation.

The hard part is that every rule has to be right without the model ever seeing the page:

`Nbd2`,`e8=Q+`, and`Qxf7#`.
The task maps onto Google's claim that Neon handles complex software engineering in fewer steps with clean edits. I ran both models through the same harness with the same prompt and the same tools, three times each, and checked every move each game played. Both ran at

`medium`thinking, which is the second of four levels for Neon and the second of three for Carbon.
Here is the end of Neon's first game at normal speed: White pushes two pawns through, promotes both, and mates with the second queen.

Neon built a working chess page in all three runs. Carbon built one in three, and the two failures came down to output length, not chess knowledge:

Does it know all the rules?Yes, on every page that ran. I replayed every move of every game with the open-source chess.js library and asked each page for the legal moves in ten trap positions, such as en passant, castling through check, a pinned knight, underpromotion, and stalemate. Neon's three pages and Carbon's one page played 16 games without an illegal move or a notation error and answered all ten traps correctly.Does it finish the job?Neon finished all three runs in six or seven steps each. Carbon finished one: in the other two, it tried to write the whole game in a single response, and that response stopped at Gemini 4's limit of 65,536 output tokens before the file was saved.Does it play real chess?Mostly. Neon's engines develop their pieces, trade when ahead, and convert endgames, and all 12 of its games ended in checkmate. The weakest moment came in its second run, where White promoted a pawn to a rook that Black captured on the next move.
The difference is in how each model wrote the file. Neon thought for 13,000 to 17,000 tokens across a whole run and then wrote the page in several smaller edits, so no single response came near the limit. Carbon thought for about 40,000 tokens and then tried to write about 26,000 tokens of code in the same response, which pushed it past the limit before the file existed. Even Carbon's successful run came within about 1,500 tokens of the cap, and it was much slower.

Neon's three runs cost $0.95, $0.83, and $1.06. Carbon's working run cost $1.36, and its two failed runs cost $1.35 and $1.33 without producing a page.

On this task, Neon is the more dependable model: the same rules accuracy as Carbon, a working page every time, and a lower bill per run. That matches Google's pitch of fast end-to-end task completion. Carbon may reason more deeply, but here thinking longer was the problem: it spent its output budget before it wrote the answer.

Three runs per model on one task is a small sample, and Carbon's failures come from the output limit on a single response rather than from what it knows about chess. With Google's background mode for long outputs, Carbon would likely finish more often. What the test does show is that at

`medium`thinking, Neon reliably finished a rules-heavy build that Carbon often could not.
## Gemini 4 Neon Pricing and Availability

Neon costs $3.00 per million input tokens and $15.00 per million output tokens on the Gemini API, flat across the full 1M-token window. Output includes thinking tokens, and implicit caching cuts cached input by 95%, to $0.15 per million tokens.

The timing makes the comparison with Carbon unusual. Carbon's introductory price of $2.00 / $10.00 runs through November 30, 2026, so for the rest of November, Neon costs 50% more per token than Carbon; from December 1, Carbon moves to $4.00 / $20.00 and Neon becomes 25% cheaper per token.

There is no free tier: Neon runs only on paid Gemini API projects, while Hydrogen and Flash-Lite keep free access. Explicit context caching is not supported. In Google's apps, Neon is available to Google AI Pro and Ultra subscribers in the Gemini app, Google AI Studio, and Google Antigravity.

## How to Get Access to Gemini 4 Neon?

The API model ID is

`gemini-4-neon`. Neon is available through the Gemini API (both`generateContent`and the Interactions API), Google AI Studio, the Gemini app, Google Antigravity, and Gemini for business on Google Cloud. It is also listed on OpenRouter as`google/gemini-4-neon`.
As of November 18, 2026, Neon is not listed in Cursor's model docs or the Gemini CLI model docs.

A minimal request with Google's Python SDK looks like this:

If you already call another Gemini 4 model, switching only means changing the model ID, since the request schema is the same. For a full walkthrough of the shared Gemini 4 API, see our Gemini 4 Hydrogen API tutorial.

## Final Thoughts

Gemini 4 Neon fills the gap between Hydrogen and Carbon, and Google's benchmarks put it ahead of Claude Opus 5.5 and GPT-6.2 Sol on most of the tests it reports. The missing piece is a direct comparison with Carbon, the model it was adapted from, and that is the comparison that decides whether stepping down saves money without losing quality.

My recommendation: keep Hydrogen as your default, try Neon on the tasks where Hydrogen struggles, and wait until December 1 before moving work off Carbon, when Neon becomes the cheaper of the two.

To get hands-on with Google's AI tools, try our Practical AI with Google Gemini and NotebookLM course.
