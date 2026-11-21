---
id: collect-261121-veille-ia/grok-xai/grok-4-7-pricing-same-2-6-per-token-double-the-cost-per-task
title: "Grok 4.7 pricing: same $2/$6 per token, double the cost per task"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "tokens", "price", "task", "same", "model", "about", "token"]
source: https://dev.to/axrisi/grok-47-pricing-same-26-per-token-double-the-cost-per-task-43d8
source_anchor: ""
source_lines: [1, 106]
section: "Grok_xAI"
---
Grok 4.7 costs exactly what Grok 4.6 did per token: $2 per million input tokens and $6 per million output. Per task, it costs about double. Artificial Analysis measured xAI's new model writing roughly 2.5 times as many tokens as its predecessor, so its cost per task went from $1.86 to $3.74. If you pick LLMs by the price table, this is the week that table stopped telling you what you will pay.


- Grok 4.7's list price is unchanged, $2 in and $6 out per million tokens. But running Artificial Analysis' Intelligence Index it generated about 240 million tokens, against 94 million for Grok 4.6 and a median of 88 million. Cost per task: $1.86 before, $3.74 now.
- The launch post's headline says "Twice as fast". A few lines down the same post says "Served at the same price and speed as Grok 4.6". Artificial Analysis clocked it at about 57 tokens per second, slower than Grok 4.6's 66, and called it "notably slow and very verbose".
- It did get better where xAI aimed it: long office tasks and coding in its own Grok Build harness, where it ranks 4th among coding agents.
- Developers on Hacker News call it lazy ("claim 'Done!'") and loopy ("Fix 80, Fix 81") at the same time.
- Personality Bench gave it a perfect 5 out of 5 on honesty and humility, tied with 32 other models.

In the Sep 21 episode, I noted that Grok 4.7 kept Grok 4.6's $2/$6 pricing while the headline promised "half the price of comparable models". This follow-up was requested by a viewer, @therealhaas, who asked under our Sep 25 episode: "Can you do a vid on Grok 4.7 :D". A week of independent measurements later, here is what that unchanged price means.

Per-token price is only half the formula. What you pay for a task is the price per token times the number of tokens the model decides to produce, and reasoning models decide a lot. Artificial Analysis publishes both halves:

|  | Grok 4.6 | Grok 4.7 | Median model | 
|---|---|---|---|
| Price per M tokens (in / out) | $2 / $6 | $2 / $6 |  | 
| Tokens generated on the Intelligence Index | 94M | 240M | 88M | 
| Cost per task | $1.86 | $3.74 |  | 
| Output speed | 66 tokens/s | 57 tokens/s | 79 tokens/s | 
| Intelligence Index | 44 | 46 |  | 

Sources: the Grok 4.7 and Grok 4.6 model pages.

So Grok 4.7 writes about 2.5 times what Grok 4.6 did and almost three times a typical model, for two more points on the index. It is a taxi with the same rate per mile that takes the scenic route through two other cities.

One caveat, because it matters. Those model-page figures compare Grok 4.7 at its "xhigh" reasoning effort with Grok 4.6 at "high". At equal effort, the Artificial Analysis article still gives about 81,000 output tokens per task for Grok 4.7 against 38,000 for Grok 4.6 at xhigh. More than double either way, and about three times GPT-6 Astra's 27,000. Each Intelligence Index task took about 7.1 minutes.

The model got bigger, too. The launch post says "Grok 4.7 uses a new, larger base model" and a "longer reinforcement learning run … weighted toward problems that take many hours". On Hacker News (609 points, 488 comments), the biggest thread, from moojacob, did the margin math: "Apparently Grok 4.7 has 40% more weights than Grok 4.6, but the price ($6 output token, $2 input) is the same", and with the release delayed "almost two weeks past the original date, XAI must not have been happy with the results". That is a commenter's claim; xAI's post gives no such figure.

Not by any clock I could find. The launch post opens with: "SpaceXAI's most powerful model for coding and knowledge work. Twice as fast, at half the price of comparable models." Two paragraphs later: "It works longer on difficult tasks, checks its own work more carefully … Served at the same price and speed as Grok 4.6".

Both can only be true if "twice as fast" is measured against somebody else's model. The pricing table in the same post lists GPT-5.6 Sol Max at $4/$20 and Fable 5.1 Max at $10/$50, so "half the price of comparable models" reads as a comparison with other labs' flagships, since the last Grok cost the same.

xAI's own account, which now goes by SpaceX AI, posted the careful version:

That tweet had about 28,600 likes. For once the tweet is more careful than the blog headline. The independent number is slower still: 57 tokens per second, against 66 for Grok 4.6 and a median of 79. Artificial Analysis' two-word summary was "notably slow".

On the chart at the top of the launch post, kristofferR on HN asked: "What's with the deceptive graph on top? Not including Astra can't have been an oversight, did the model compare poorly to it?"

The verbosity bought something. Artificial Analysis headlined its article "Grok 4.7 Scores 46 on AI Intelligence Index, Puts SpaceXAI in Top 4 Labs". The gains sit where xAI said it trained, on long, multi-step work:

- 
**AA-Briefcase** (long office tasks): 1657 Elo, up 111, "just behind Claude Opus 5 and Claude Fable 5.1".
- 
**GDPval-AA** : 1695, up 90.
- 
**Coding Agent Index** , run in xAI's own Grok Build harness: 56, up 9. It "ranks 4th, behind only Claude Fable 5.1, GPT-6 Astra, and Claude Opus 5".
- 
**Hallucination rate** : 29 %, down from 34 %.

On the overall Intelligence Index it is 21st of 211 models. The model page calls it "amongst the leading models in intelligence and reasonably priced", which is true per token. The same sentence continues with "notably slow and very verbose", which is true per task.

The people using Grok 4.7 describe something else. In the same HN thread, slowin: "Grok will end tasks almost immediately and claim "Done!". It's definitely the laziest and most "dishonest" of all the models." Saline9515 saw the opposite failure: "It will loop in thinking mode ("Let me implement those fixes: Fix 1, Fix 2, Fix 3 .... Fix 80, Fix 81"), ignore the AGENTS.md instructions, corrupt plan files".

Not everyone agrees: sidgtm called it "a no nonsense model and stays on its course".

Lazy and verbose are not a contradiction. A model can spend 81,000 tokens thinking and still stop before the job is done. It is the coworker who writes an 81-step plan, then says "done" and goes home. For you that is the worst combination: you pay for the plan and do the work.

Personality Bench runs new models through a battery of personality tests. Grok 4.7's profile is titled "The humble type".

It "Maxes out the Honesty-Humility scale (mean 5.00, #1 of 49, tied with 32 others)". So a perfect score, shared with two-thirds of the field. The same page's locus-of-control test found the dominant locus is "Powerful Others": the model "attributes outcomes more to powerful entities than to its own actions — unusual for a frontier model and worth investigating as a possible training artifact." I would call it reading the org chart. And per the birth chart the page includes "for fun", it is a Virgo. The whole run cost Personality Bench $2.13 for 160 runs.

- 
**Compare cost per task, not price per token.** A price table tells you the rate. Your bill is rate times tokens, and the token count is set by the model. Artificial Analysis publishes cost per task; so can your own logs.
- 
**Check the reasoning effort.** The published Grok comparison pits xhigh against high. Know which effort level your provider or SDK uses before you compare.
- 
**Cap the budget before you switch.** Set a per-task token or spend limit in your agent harness, then compare runs on your own tasks.
- 
**Read the benchmark next to the launch post.** Here the independent numbers confirmed the gains xAI claimed on long tasks and contradicted the speed claim. Both are useful.

**The Copilot+ PC brand is dead.** Microsoft's Surface boss Brett Ostrum told Windows Central: "these are not called Copilot+ PCs. They do meet all the requirements of our previous bar for what Copilot+ devices are." The 2024 launch feature, Recall, was delayed after security criticism, and PC makers have dropped the name. Zac Bowden's own take: "these PCs have *nothing* to do with Copilot." (HN)

**One month without AI.** A developer who maintains LibreWeddingPlanner quit AI tools for a month. The low point before: "an agent had been stalled for 30 minutes … The AI agent profusely apologized, and gave me in 20 seconds what it had not given me in 30 minutes. I looked at the token expenses, and it had spent $30 in tokens for absolutely nothing." After the month: "I recovered the joy of programming." Grok would call $30 a warm-up. (HN)

I stamped Grok 4.7 NEEDS REVIEW. I would use it for long office work and inside its own harness, where the independent benchmarks show real gains. But I would put a spending cap on it first, because the price didn't change and the bill did. And "twice as fast" should come off the headline until a clock agrees.

**How much does Grok 4.7 cost?**

$2 per million input tokens and $6 per million output, the same as Grok 4.6. Artificial Analysis measured about $3.74 per task, against $1.86 for Grok 4.6, because it generates far more tokens.

**Is Grok 4.7 faster than Grok 4.6?**

Not per token. xAI's own tweet says "same price and speed"; Artificial Analysis measured about 57 tokens per second versus 66 for Grok 4.6.

**Is Grok 4.7 good at coding?**

In xAI's own Grok Build harness it scores 56 on Artificial Analysis' Coding Agent Index, 4th behind Claude Fable 5.1, GPT-6 Astra and Claude Opus 5. Some developers on HN report it ending tasks early or looping.

**Why does a model with the same price cost more?**

Cost per task is price per token times tokens used. Grok 4.7 writes about 2.5 times as many tokens as Grok 4.6 on the same benchmark.

- xAI launch post (archived Sep 23): https://web.archive.org/web/20260923060447/https://x.ai/news/grok-4-7
- @SpaceXAI on X: https://x.com/SpaceXAI/status/2102069815225586149
- Artificial Analysis, "Benchmarking Grok 4.7": https://artificialanalysis.ai/articles/benchmarking-grok-4-7
- Artificial Analysis, Grok 4.7 model page: https://artificialanalysis.ai/models/grok-4-7
- Artificial Analysis, Grok 4.6 model page: https://artificialanalysis.ai/models/grok-4-6
- Hacker News, Grok 4.7 launch: https://news.ycombinator.com/item?id=49788838
- Personality Bench, Grok 4.7: https://persona.earthpilot.ai/models/x-ai/grok-4.7
- Windows Central, "The Copilot+ PC brand is dead": https://www.windowscentral.com/microsoft/windows-11/the-copilot-pc-brand-is-dead-microsoft-and-pc-makers-quietly-pull-back-on-tarnished-windows-11-ai-pc-branding
- "One Month Without AI": https://blog.bustikiller.com/2026/09/25/one-month-without-ai.html

*This article expands on an episode of **The Daily Diff**, a five-minute daily video on what shipped and what broke in tech. Watch the episode · Subscribe on YouTube · the written diff lands in your inbox every morning at thedailydiff.dev.*

## Top comments (0)
