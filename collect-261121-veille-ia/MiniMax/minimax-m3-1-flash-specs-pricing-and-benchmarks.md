---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-specs-pricing-and-benchmarks
title: "MiniMax M3.1 Flash: Specs, Pricing and Benchmarks"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["minimax", "flash", "tokens", "model", "input", "million", "output", "says"]
source: https://www.aighten.com/en/blog/minimax-m3-1-flash-specs-pricing-benchmarks
source_anchor: ""
source_lines: [1, 120]
section: "MiniMax"
---
MiniMax M3.1 Flash is an agent-focused language model from Chinese AI company MiniMax, released on October 19, 2026. It costs $0.15 per million input tokens and $0.60 per million output tokens, half the per-token price of MiniMax M3, and MiniMax says it runs agentic tasks for 70% less than M3 while generating tokens nearly three times faster. It is a 428-billion-parameter mixture-of-experts model with a 1-million-token context window, and it was the anonymous "Space Bunny" model developers had been testing on OpenRouter since late September.

The launch post passed 400 points on Hacker News within a day. Below are the specs, the price against rival "flash" models, MiniMax's own benchmark table, and the first independent results, which back the cost story but not the speed claim.

- MiniMax M3.1 Flash went live on October 19, 2026 through the MiniMax API, the M Plan subscription and the MiniMax Code app. The weights are promised "later"; the launch post gives no date.
- Pricing is flat at any prompt length: $0.15 input, $0.60 output and $0.015 cached input per million tokens, per MiniMax's pricing page. MiniMax M3's standard rate is $0.30 and $1.20.
- It is not the cheapest per token. Claude Haiku 5.5 and OpenAI's gpt-6-luna both list $0.10 input and $0.50 output for shorter prompts, though both charge more above 100K and 272K tokens respectively.
- In MiniMax's table it leads every listed rival on Automation-Bench (62.8) and ties Claude Opus 5 on BankerToolBench (76.2). It trails Opus 5 and GPT-5.6 Sol on OSWorld 2.0, Terminal-Bench 4.0 and HLE.
- Artificial Analysis scores it 43 on its Intelligence Index at $0.22 per task, but measured 122.8 output tokens per second, below the 170+ median MiniMax quotes.

MiniMax calls M3.1 Flash a "next-generation Flash model", a faster and cheaper follow-up to MiniMax M3, which shipped on June 1. The company says it improved inference efficiency "through more aggressive sparsity". The result, per the launch post, is online inference "nearly three times faster", everyday output speeds that "can reach 200+ TPS" (tokens per second) and a median (P50) speed of 170+ TPS.

"Space Bunny", an unnamed model that appeared on OpenRouter in late September, was an early build of M3.1 Flash. MiniMax says more than 200 trillion tokens passed through it before the reveal.

MiniMax also says a more capable M3.1 Pro is on the way, without a date. On the weights, the launch post says only that they "will be released later". VentureBeat reported a planned October 23 release, and MiniMax's model release notes already describe it as open-weight, but nothing was on MiniMax's Hugging Face page when this was written.

The architecture details come from MiniMax's developer guide for the preview release and its text generation docs.

| Spec | MiniMax M3.1 Flash | 
|---|---|
| Architecture | Mixture-of-experts (MoE) with sparse attention and a native vision encoder | 
| Total parameters | 428B | 
| Active parameters per token | About 23B | 
| Context window | 1,000,000 tokens | 
| Input / output | Text, image and video in; text out | 
| Reasoning | Always on; effort levels low, medium, high, xhigh, max (default max) | 
| API compatibility | Anthropic-compatible API; OpenAI-compatible Chat Completions and Responses | 
| Rate limits | 200 requests and 10 million tokens per minute | 
| Weights | Not yet released | 

A mixture-of-experts model splits its parameters into many "expert" blocks and routes each token through only a few of them, which is why a 428B model can run with the compute of a roughly 23B one. The M3 model card lists the same 428B total and 23B active parameters, so the network did not grow between generations.

Reasoning cannot be switched off. The docs say requests that disable thinking or set effort to "none", which worked on M3, now return an error, so check old integrations before swapping the model name.

MiniMax M3.1 Flash costs $0.15 per million input tokens, $0.015 per million cached input tokens and $0.60 per million output tokens, with no surcharge for long prompts or peak hours. Here is how that compares with M3 and with other low-cost models, using each vendor's published list price:

| Model (official list price per 1M tokens) | Input | Cached input | Output | 
|---|---|---|---|
| MiniMax M3.1 Flash, any length | $0.15 | $0.015 | $0.60 | 
| MiniMax M3, standard, up to 512K | $0.30 | $0.06 | $1.20 | 
| DeepSeek V4.1-Flash-1017, off-peak / peak | $0.15 / $0.30 | $0.003 / $0.006 | $0.60 / $1.20 | 
| Claude Haiku 5.5, prompts up to 100K | $0.10 | $0.01 | $0.50 | 
| OpenAI gpt-6-luna, up to 272K | $0.10 | $0.01 | $0.50 | 
| Gemini 3.8 Flash, through Dec. 31, 2026 | $0.75 | $0.075 | $3.75 | 

Sources: MiniMax, DeepSeek, Anthropic, OpenAI, Google.

Against M3, every per-token rate is cut in half or more. Against DeepSeek, which MiniMax treats as its main rival in its charts, the price matches DeepSeek's off-peak rate; DeepSeek doubles it on weekdays from 01:00 to 04:00 and 06:00 to 10:00 UTC, but its cache hits are five times cheaper. Against Haiku 5.5 and gpt-6-luna, M3.1 Flash costs more for short prompts but stays flat at long ones, where Haiku 5.5 rises to $0.50 input and $2.50 output above 100K tokens. We covered the DeepSeek model in our DeepSeek V4.1-Flash-1017 breakdown.

MiniMax's 70% claim is about total cost on agentic tasks, which also depends on how many tokens the model spends per task.

MiniMax published a 15-benchmark table against six models. All figures below are MiniMax's; a blank cell (n/a below) means it had no comparable published score. Most tests ran inside a named harness, the agent framework (such as Claude Code or Codex) that gives the model its tools, and scores from different harnesses are not directly comparable.

The rows that matter most for agent work, with the best score among the columns shown in bold:

| Benchmark (MiniMax table) | M3.1 Flash | DeepSeek V4.1-Flash-1017 | Kimi-K3 | GPT-5.6 Sol | Claude Opus 5 | 
|---|---|---|---|---|---|
| Automation-Bench v1.0.6 | **62.8** | 60.5 | 46.7 | 45.8 | 50.3 | 
| BankerToolBench | **76.2** | n/a | 69.1 | 74.6 | **76.2** | 
| LOCA-Bench 256K | **60.3** | n/a | 49.7 | 60.0 | 53.7 | 
| Terminal-Bench 4.0 | 38.1 | 42.3 | 12.6 | 39.9 | **51.8** | 
| OSWorld 2.0 | 61.7 | n/a | 58.3 | 65.7 | **70.6** | 
| Agents' Last Exam | 29.6 | n/a | 28.3 | 30.6 | **32.2** | 
| DeepSWE v1.1 | 73.7 | **77.0** | 67.5 | 73.0 | 74.0 | 
| CyberGym | 82.2 | n/a | 80.0 | **83.6** | n/a | 
| HLE (no tools, text-only) | 43.4 | 44.0 | 43.5 | 49.5 | **55.6** | 

Its best results are on business-workflow tests. On Automation-Bench, which scores agents on realistic business tasks, it beats every model in the table, including DeepSeek's latest (62.8 vs 60.5). On LOCA-Bench at 256K context, a test of agents working through very long inputs, it edges GPT-5.6 Sol (60.3 vs 60.0). On BankerToolBench, a set of investment-banking tasks, it ties Opus 5 at 76.2.

Frontier models still lead on harder computer-use and reasoning tests. Opus 5 scores 70.6 on OSWorld 2.0 to M3.1 Flash's 61.7, and 51.8 on Terminal-Bench 4.0 to its 38.1. On HLE without tools, Opus 5 is 12 points ahead. DeepSeek V4.1-Flash-1017 also beats it on DeepSWE (77.0 vs 73.7) and Terminal-Bench (42.3 vs 38.1).

MiniMax says M3.1 Flash ranks "first among Chinese models" on OSWorld 2.0 and Agents' Last Exam. That holds within the table, but DeepSeek's Agents' Last Exam cell is blank because MiniMax counts only official leaderboard results. DeepSeek's own model card reports 35.4 for V4.1-Flash-1017 on that test. The runs may not be comparable, but the gap is worth knowing before you quote the claim.

Two more cautions. OSWorld 2.0 reports both a binary success rate and a partial-credit score, and MiniMax does not say which one 61.7 is. And Epoch AI's review of DeepSWE found issues in at least 23 of its 113 tasks and rates the benchmark "Flawed", so small DeepSWE gaps say little.

Artificial Analysis, which runs one test suite across hundreds of models, gave MiniMax M3.1 Flash an Intelligence Index score of 43 on launch day, two points behind DeepSeek V4.1 Flash 1017, at a slightly lower cost per task.

| Artificial Analysis metric | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 (Max) | 
|---|---|---|
| Intelligence Index | 43 | 45 | 
| Output speed | 122.8 tokens/s | 216.5 tokens/s | 
| Time to first token | 1.12 s | 1.11 s | 
| Cost per Intelligence Index task | $0.22 | $0.26 | 
| Output tokens used to run the index | 180M | 210M | 

The measured speed on MiniMax's API, 122.8 tokens per second, is well below the "170+ TPS" median in the launch post, though above the 115.8 median Artificial Analysis lists for comparable models. Launch-day speeds often change as providers add capacity. The model is also wordy: it used 180M output tokens on the index against a 99M median. It still came out slightly cheaper per task than DeepSeek ($0.22 vs $0.26). Note that Artificial Analysis lists DeepSeek at its $0.30 / $1.20 peak rate.

VentureBeat, citing Artificial Analysis data, reported that the index score rose from 29 for M3 and that cost per task fell from about $0.51. It also reported a 93% hallucination rate on the AA-Omniscience knowledge test: M3.1 Flash answered 56% of questions correctly, but when it didn't know, it nearly always gave a wrong answer instead of declining. That matters more for factual lookups than for tool-driven agent work.

The official Agents' Last Exam leaderboard confirms MiniMax's 29.6% pass rate with the Claude Code harness. With the Codex harness it scored 27%.

Two of MiniMax's charts plot score against what a task costs to run, at each reasoning effort level.

On Automation-Bench, M3.1 Flash goes from 52.8% at low effort to 62.8% at max, and every point on its line sits under $0.05 per task. In the same chart, Claude Opus 5.5, which is not in the table above, reaches about 57% at roughly $0.90 per task. MiniMax priced DeepSeek at peak-hour rates here, so an off-peak DeepSeek user would pay less than the chart shows.

On Agents' Last Exam, MiniMax puts its total bill at about $111 for a 29.6% pass rate, the lowest cost on the chart. Claude Opus 5.5 sits at the top near 38%, at more than $1,000 on the log scale: about 8 points higher for more than ten times the cost.

1. The MiniMax API, with model ID `MiniMax-M3.1-Flash` . Because it accepts both the Anthropic and OpenAI formats, most agent frameworks need only a new base URL, key and model name. Effort is set with`output_config.effort` (Anthropic format),`reasoning_effort` (Chat Completions) or`reasoning.effort` (Responses).
2. MiniMax Code, the company's desktop coding agent, and the M Plan subscription.
3. OpenRouter, which currently lists a free variant, `minimax/minimax-m3.1-flash:free` .

Self-hosting has to wait for the weights, and a 428B model needs server-class hardware. For a comparable case, see our guide to running Meta's Muse Spark Open on a Mac Studio.

M3.1 Flash fits high-volume agent work where cost per task decides the budget: business-process automation, document-heavy workflows, long-context agents and coding sub-agents. If your workload depends on the hardest computer-use or terminal tasks, MiniMax's own table puts frontier models ahead. If it depends on factual recall without tools, the hallucination result is a reason to test first.

Open questions worth tracking:

- License terms. M3's weights ship under the MiniMax Community License, which asks commercial users to display "Built with MiniMax M3" and notify the company, and requires prior written approval once yearly revenue from products built on it exceeds $20 million. Flash's terms are not public yet.
- Export rules. China's draft export controls on AI model weights use a 2-trillion-parameter threshold among other tests and exclude technology already public through open source. How they apply to a given release is not settled.
- M3.1 Pro, whose price will show where MiniMax draws the line between its two tiers.

MiniMax M3.1 Flash is an agent-focused large language model MiniMax released on October 19, 2026. Per MiniMax's docs, it is a 428B-parameter mixture-of-experts model with about 23B active parameters, a 1M-token context window, and text, image and video input. It succeeds MiniMax M3.

MiniMax lists $0.15 per million input tokens, $0.015 per million cached input tokens and $0.60 per million output tokens, flat at any prompt length and time of day. That is half of MiniMax M3's standard rate. MiniMax says agentic tasks cost 70% less overall than on M3.

Not yet. MiniMax says the weights "will be released later". VentureBeat reported a planned October 23 release, and no license has been published. For now it is available through the MiniMax API, the M Plan, MiniMax Code and OpenRouter.

Space Bunny was an anonymous model on OpenRouter from late September 2026. MiniMax confirmed on October 19 that it was an early version of M3.1 Flash and said more than 200 trillion tokens were processed through it before launch.

It depends on the task. In MiniMax's table, M3.1 Flash leads on Automation-Bench (62.8 vs 60.5), while DeepSeek leads on DeepSWE v1.1 (77.0 vs 73.7) and Terminal-Bench 4.0 (42.3 vs 38.1). Artificial Analysis scores DeepSeek slightly higher overall (45 vs 43) and measured it faster. Off-peak input and output prices are identical.
