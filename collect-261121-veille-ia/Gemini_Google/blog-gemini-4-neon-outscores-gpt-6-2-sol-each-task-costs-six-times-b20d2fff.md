---
id: collect-261121-veille-ia/gemini-google/blog-gemini-4-neon-outscores-gpt-6-2-sol-each-task-costs-six-times-b20d2fff
title: "Blog gemini 4 neon outscores gpt 6 2 sol each task costs six times"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "google", "gemini", "carbon", "task", "artificial", "cost", "analysis"]
source: ""
source_anchor: ""
source_lines: [1, 61]
section: "Gemini_Google"
---
Google's launch post for Gemini 4 Neon, published on Wednesday, November 18, leads with a ranking. Neon, the company wrote, places "#3 overall with a score of 61 on the Artificial Analysis Intelligence Index" at its highest thinking setting.
The same benchmark firm published another number that the launch post does not.
According to Artificial Analysis, evaluating Neon at that setting cost $6.44 per task. OpenAI's GPT-6.2 Sol, released a day earlier, scored 57 on the same index at its top setting. Its cost per task was 1.01 dollars.
That gap is the story of this release. Per token, Neon costs only about 50% more than Sol. Per finished task, it cost more than six times as much, because it produced far more tokens to reach its answers. For any team choosing a default model for agents or coding tools, the second number is the one that shows up on the bill.
Google's Middle Model Lands at a Middle Price
Neon sits between the two Gemini 4 models Google shipped earlier this month. Gemini 4 Carbon is the flagship for the longest, hardest projects. Gemini 4 Hydrogen is the fast, cheaper model for everyday coding and agent work.
Google priced Neon at $3 per million input tokens and 15 dollars per million output tokens, with cached input at 15 cents per million (a 95% discount). It has a 1 million-token context window.
| Model | Input (per 1M tokens) | Output (per 1M tokens) | 
|---|---|---|
| Gemini 4 Hydrogen | $1.50 | $7.50 | 
| Gemini 4 Neon | $3.00 | $15.00 | 
| Gemini 4 Carbon (through Nov 30) | $2.00 | $10.00 | 
| Gemini 4 Carbon (from Dec 1) | $4.00 | $20.00 | 
| GPT-6.2 Sol | $2.00 | $10.00 | 
| Claude Opus 5.5 | $4.00 | $20.00 | 
Pricing for Neon is from Google's launch post. Carbon, Hydrogen and competitor prices are as reported by VentureBeat and Artificial Analysis.
The Carbon rows explain Google's timing. Carbon's introductory price ends on November 30, and on December 1 it doubles. From that date, Neon is 25% cheaper per token than Carbon, and Artificial Analysis scores it a point higher.
Neon is available in the Gemini API through Google AI Studio (paid tier only), in Android Studio, in Gemini for business, in Google Antigravity for paid AI Pro and Ultra subscribers, and in the Gemini app for Pro and Ultra subscribers. Logan Kilpatrick of Google DeepMind acknowledged that Google was having "a little too much fun with naming," VentureBeat reported.
The Benchmarks Google Chose to Lead With
Google's post stacks up a long list of results. Among them:
- 61 on the Artificial Analysis Intelligence Index at the xhigh thinking setting, which Google calls third overall
- 70.9% on the Vals Index, which Google ranks second
- 58.6 on the Artificial Analysis Coding Agent Index, which Google ranks first
- 65.2% on Terminal-Bench 4.1, a test of agents completing tasks in a command-line environment
- Refusals on just 1.98% of Terminal-Bench 4.0 tasks
The cost claims are the boldest part. At its default high setting, Google says Neon delivers "a 32% lower cost per task than Carbon." Against "top competing frontier models," which the post does not name, Google claims roughly 50% to 80% lower cost per task on the Vals and Agent Arena charts.
Artificial Analysis backs the ranking. Its launch write-up puts Neon at xhigh "3rd overall, behind GPT-6.2 Astra (max) and Claude Fable 5.2 (max)," one point ahead of Carbon.
Its cost comparison depends on the setting. At xhigh, the firm measured Neon at $6.44 per task, about 17% less than Carbon's 7.76 dollars at standard pricing. At the default high setting, it said Neon "matches Carbon (high) at less than half the Cost per Task."
What neither Google's post nor that write-up leads with is how Neon compares with OpenAI's newest mid-range model.
Where the Six Times Comes From
Artificial Analysis publishes how many tokens each model burns to complete its Intelligence Index, along with the dollar cost. Put Neon and Sol side by side at their top settings and the pattern is plain:
| Measure (Artificial Analysis) | Gemini 4 Neon (xhigh) | GPT-6.2 Sol (max) | 
|---|---|---|
| Intelligence Index score | 61 | 57 | 
| Cost per index task | $6.44 | $1.01 | 
| Output tokens for the full run | 283 million | 60.9 million | 
| Time per index task | 21.5 minutes | 8.7 minutes | 
| Output speed | 110 tokens/sec | 77 tokens/sec | 
Neon generated about 4.6 times as many output tokens as Sol. Each of those tokens costs 50% more. Multiply the two and you land in the range of the six-fold cost gap.
The token count also explains the timing. Neon streams text faster than Sol, at about 110 tokens a second. It still took 21.5 minutes per index task on average, against 8.7 for Sol, because it had so much more to write. Thinking tokens count as output, and Google bills them at the output rate.
Turning the dial down helps. At its default high setting, Neon scored 60 and used 123 million output tokens on the same run, less than half its xhigh total. Sol at high used 22.7 million.
None of this means Neon is a poor model. It means its sticker price understates what it costs to use. The real unit cost of an agent is not dollars per token. It is dollars per task completed, and that depends on how much a model thinks before it answers.
The Case for Neon
Four index points is not nothing. On Artificial Analysis's scale, Neon at 61 sits above Sol at 57 and a point above Carbon's 60. Artificial Analysis ranks only two models above it. For work where one wrong answer costs more than the compute, such as legal research, financial analysis or long single-request jobs, the extra accuracy can be worth paying for.
Google also argues that the relevant comparison is within its own lineup. A team already running Carbon will see its bill roughly double on December 1. Moving to Neon cuts the per-token price by a quarter, and Artificial Analysis measured a 17% lower cost per task than Carbon, with a slightly higher score.
VentureBeat, in its launch analysis, offered the sharper caveat. Neon's headline price is 50% above both Sonnet 5.5 and GPT-6.2 Sol, so Neon would need roughly a third fewer tokens than those models to erase that premium. Artificial Analysis's numbers show it using several times more, not fewer.
Benchmarks also aren't workloads. Google's cost claims rely on its chosen tasks and settings, and Artificial Analysis measures a fixed test suite. Your agent's prompts, tool calls and context lengths will produce a different ratio. OpenAI's GPT-6.2 launch came with its own benchmark claims, and the same caution applies.
The Bottom Line
Gemini 4 Neon is a strong model at a middle price: 3 dollars in, 15 dollars out, a 61 on Artificial Analysis's index, and a cheaper home for Carbon users facing a price doubling on December 1.
It is also a model that thinks at length. On Artificial Analysis's test suite, that habit turned a 50% per-token premium over GPT-6.2 Sol into a six-fold cost per task and a run time more than twice as long. The launch post leads with the score. The benchmark firm's own pages carry the bill.
Before switching a production agent, run your own tasks at the thinking level you plan to use and divide the invoice by the number of tasks finished. That is the only price that counts.
Sources
- Introducing Gemini 4 Neon, Google (Nov 18, 2026)
- Google Launches Gemini 4 Neon With Opus 5.5-Level Coding Performance at 25% Lower API Token Prices, VentureBeat (Nov 18, 2026)
- Gemini 4 Neon: Intelligence, Performance and Price Analysis, Artificial Analysis (Nov 2026)
- Google Continues to Accelerate Its Release Cadence With Gemini 4 Neon, Artificial Analysis (Nov 18, 2026)
- GPT-6.2 Sol: Intelligence, Performance and Price Analysis, Artificial Analysis (Nov 2026)
- OpenAI Has the #2 Model in the Intelligence Index With GPT-6.2 Astra and GPT-6.2 Sol, Artificial Analysis (Nov 17, 2026)
- Gemini 4 Neon API Pricing: 25% Below Carbon From Dec 1, Data Studios (Nov 18, 2026)
- Gemini 4 Carbon Pricing Doubles After Nov 30, Apidog (Nov 2026)
- Gemini 4 Neon: Pricing and Benchmarks vs Carbon and Hydrogen, ExplainX (Nov 19, 2026)
