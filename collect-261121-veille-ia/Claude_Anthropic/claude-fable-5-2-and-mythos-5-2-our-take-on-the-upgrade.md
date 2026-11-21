---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-and-mythos-5-2-our-take-on-the-upgrade
title: "Claude Fable 5.2 and Mythos 5.2: Our Take on the Upgrade"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "anthropic", "mythos", "model", "output", "faster", "most"]
source: https://www.datacamp.com/blog/claude-fable-5-2-and-mythos-5-2
source_anchor: ""
source_lines: [1, 386]
section: "Claude_Anthropic"
---
Today, Anthropic announced Fable 5.2 for everyone and Mythos 5.2 for vetted organizations. They're the same underlying model, but the Fable version has extra cybersecurity and biology safeguards.

The headline changes are:

- a lower price,
- faster output, and
- big jumps on Anthropic's hardest benchmarks.

More specifically, Fable 5.2 costs 20% less than Fable 5.1, generates output up to 30% faster, and beats GPT-6 Astra and Gemini 4 Carbon on most rows of Anthropic's benchmark table.

In this article, I'll cover what's new in Fable 5.2 and Mythos 5.2, how they perform on the benchmarks, what they cost, and how to decide whether you need them over Claude Opus 5.5. You can see the model in action in our Claude Fable 5.2 API tutorial. For a comparison to OpenAI's flagship, see our Claude Fable 5.2 vs GPT-6 Astra guide. 

## What Is Claude Fable 5.2?

Claude Fable 5.2 is Anthropic's most capable generally available model. It's built for long, demanding technical work: large codebase migrations, multi-day agent runs, research problems, and engineering design.

Fable 5.2 can also work alongside the smaller models, either as an advisor they consult on hard problems or as an orchestrator that hands simpler subtasks down to them.

Fable 5.2 keeps the 1M-token context window and 128K max output of its predecessor, and its knowledge cutoff is June 2026.

## What Is Claude Mythos 5.2?

Claude Mythos 5.2 is the same model as Fable 5.2 but without the safeguards that block most cybersecurity work and research-level biology work. In fact, Mythos 5.2 is Anthropic's most capable model for cybersecurity engineering. Because these kinds of capabilities are dual-use, Anthropic only offers Mythos 5.2 to vetted orgs through its Cyber Verification Program and Life Sciences Verification Program.

## What's New in Claude Fable 5.2?

Fable has only been around since June, when Anthropic released Fable 5. Since then, there have been two point releases, Fable 5.1 in September and now Fable 5.2, and neither has added much in the way of new features. Both have mostly made the model cheaper, faster, and stronger, though the performance gains haven't been small: Fable 5.2 more than doubles Fable 5.1's score on the Remote Labor Index, for example.

The bigger difference between the two releases is in how you're allowed to use the model. With Fable 5.1, Anthropic narrowed its cybersecurity safeguards and began allowing the model to be used to find software vulnerabilities, though not to write exploits for them, and it announced Enterprise Frontier Safeguards, an option for keeping your data in your own cloud, alongside the release. Fable 5.2's safeguard changes are smaller: it blocks fewer requests, but the same kinds of tasks are still off-limits.

The new ways Anthropic suggests using it, such as having Fable 5.2 advise smaller Claude models, aren't new either, since Anthropic's advisor tool already supported Fable 5 and 5.1. And the launch demos, like the operating system built from an empty folder, show a stronger model doing harder things, not new features you can switch on.

That said, if I'm going to point to new things, here's what I'd mention:

### A lower price

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, 20% less than Fable 5.1's $10/$50. The same 20% cut applies to cache reads ($0.20), cache writes, and batch processing. See the pricing section below for the full breakdown.

### Faster output, and fast mode for Fable

Output is up to 30% faster than Fable 5.1, which adds up over a long agent run. Fable also now supports fast mode, which speeds output up by as much as 2.5x at double the price ($16/$80).

### Better results at lower effort

This is the change most likely to affect your bill. At high effort, Fable 5.2 scores 45.3% on FrontierCode 2.0, about level with Opus 5.5's best score, at roughly half the cost per task. At low effort, it matches Opus 5.5's top score on Humanity's Last Exam at less than a tenth of the cost per task. My read: If you're moving up from Opus 5.5, start Fable 5.2 at a lower effort level.

### Fewer blocked requests

Fable 5.2 should hit about 55% fewer safety blocks than Fable 5.1 (about 60% fewer in cybersecurity and 40% fewer in biology). That's a big deal if Fable 5.1's refusals were getting in your way, but the categories that are blocked haven't changed: penetration testing, exploit generation, and most research-level biology still get handed off to older Claude models.

Here are the main results. They all come from Anthropic's launch table, so read them as Anthropic's case for the model rather than independent results. The Claude scores were run with safeguards on. Unless noted, each model is shown at its highest available effort.

| Benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | **49.1%** | 32.4% | 45.5% | 42.8% | 41.1% | 
| FrontierSWE v2 (agentic coding) | **69.9%** | 56.3% | 62.3% | 65.5% | 61.2% | 
| AutomationBench (business workflows) | 51.4% | 31.4% | 42.5% | 41.4% | **53.9%** | 
| Terminal-Bench-Science 0.1 | **74.6%** | 52.6% | 58.7% | 68.1% | 64.8% | 
| OSWorld 2.1 (computer use) | **89.3%** | 83.5% | 87.2% | 79.5% | 81.5% | 
| RoboDojo Sim (robotics) | **64.9%** | 27.6% | 39.4% | 29.8% | Not reported | 
| AA-Briefcase v1.1 (knowledge work) | **66.1%** | 61.5% | 61.2% | 52.0% | 61.8% | 
| Remote Labor Index (knowledge work) | **41.3%** | 17.9% | 21.3% | 20.8% | Not reported | 
| Humanity's Last Exam (with tools) | **74.2%** | 65.6% | 67.7% | 57.2% | Not reported | 
| WANDR (agentic search) | **82.1%** | 68.7% | 72.3% | Not reported | Not reported | 
| Chartography (no tools) | **73.4%** | 44.8% | 64.4% | 71.0% | 72.5% | 

### Agentic coding

Fable 5.2 leads both coding benchmarks in the table. On FrontierCode 2.0, which measures whether open-source maintainers would merge a model's code changes, it scores 49.1%, the highest in the table and well above Fable 5.1's 32.4%. On FrontierSWE v2, it scores 69.9%, ahead of GPT-6 Astra (65.5%) and Opus 5.5 (62.3%).

The launch examples point the same way: agents running an early version of Fable 5.2 migrated a production service from Python to Rust, and it now starts about 26 times faster.

### Knowledge work and research

The biggest relative jump is on the Remote Labor Index, which measures how well a model completes real freelance-style projects. Fable 5.2 scores 41.3%, roughly double Opus 5.5 (21.3%) and GPT-6 Astra (20.8%). It also leads on Humanity's Last Exam with tools (74.2%) and on Terminal-Bench-Science (74.6%, or 75.3% for Mythos 5.2).

Business automation is the one area where it trails. On AutomationBench, Gemini 4 Carbon scores 53.9% to Fable 5.2's 51.4%, although Fable 5.2 is still far ahead of Fable 5.1 (31.4%).

Outside the main table, Fable 5.2 scored 83.5 on the BenchCAD 2.1 hardware-engineering benchmark, compared with 65.3 for Opus 5.5. It's also credited with solving two open problems in black-hole physics, and those results are posted as preprints that other physicists can check.

### Computer use, robotics, and vision

Fable 5.2 scores 89.3% on OSWorld 2.1, ahead of Opus 5.5 (87.2%) and GPT-6 Astra (79.5%). Its robotics jump is the largest in the table: 64.9% on RoboDojo Sim, more than double Fable 5.1 and GPT-6 Astra. On Chartography, which tests reading charts without tools, it edges out Gemini 4 Carbon and GPT-6 Astra, after Fable 5.1 trailed badly at 44.8%.

### Cybersecurity and biology (Mythos 5.2)

The security and biology rows are for Mythos 5.2, since Fable 5.2's safeguards block most of these tasks. Mythos 5.2 scores 76.3% on CyScenarioBench (vs 61.7% for Mythos 5.1), 89.6% on CVE Rediscovery (vs 78.3%), and 85.1% on a 24-hour protein binder design task (vs 79.1%). There's no fair comparison with OpenAI's security model here, since each lab ran its own CyScenarioBench tasks.

## What's This About Fermat's Last Theorem?

A team of Fable 5.2 agents formalized the proof of Fermat's Last Theorem in the Lean proof assistant in 30 hours. You might be wondering: wasn't the theorem proved three decades ago, so what's left to do?

### The theorem is old news

Fermat's Last Theorem says there are no positive whole numbers *a*, *b*, and *c* where a<sup>n</sup> + b<sup>n</sup> = c<sup>n</sup> for any *n* greater than 2. For *n* = 2, there are plenty of solutions, like 3<sup>2</sup> + 4<sup>2</sup> = 5<sup>2</sup> (9 + 16 = 25). But try cubes and you only get near misses: 6<sup>3</sup> + 8<sup>3</sup> = 728, one short of 9<sup>3</sup> = 729.

Pierre de Fermat claimed a proof in 1637 but never wrote it down, and it stayed unsolved for more than 350 years until Andrew Wiles proved it in the mid-1990s, with help from Richard Taylor.

Wiles's proof runs over 100 pages and rests on decades of advanced number theory, so very few people can check it end-to-end. Lean is a proof assistant: a programming language in which you write a proof as code, and the software checks every logical step all the way down to the basic axioms of mathematics. If the proof compiles, it's correct. "Formalizing" a theorem means translating the human-written proof into this machine-checkable form.

For Fermat's Last Theorem, that's a huge job, because much of the mathematics the proof depends on had never been written in Lean. Mathematician Kevin Buzzard started a community project to do it in 2024.

### Why it's interesting for an AI model

Most AI output is judged by people or by benchmarks that only test part of the answer. A Lean proof is different: the checker either accepts every step or it doesn't, so there's no way to be subtly wrong. That makes it an unusually clean test of whether a model can carry a very long, very hard project through to a verified finish.

This isn't the first time Claude has done this. In September, Anthropic published the first complete computer-checked proof of the theorem, written by dozens of agents running an internal research model roughly comparable to Fable 5.1. That effort took 11 days and about six billion output tokens, produced 13 million lines of Lean, and built partly on pieces of Buzzard's project. Fable 5.2 did it in 30 hours, with minimal human help.

### The caveats

- It adds nothing to the mathematics. Buzzard said as much about Anthropic's first formalization.
- Fable 5.2 was given a list of intermediate theorem statements to track its progress. That's a roadmap of what to prove, though not how to prove it.
- I couldn't find any outside review of the new proof yet, so for now, the 30-hour figure and the Opus 5.5 comparison are Anthropic's claims.

The most telling part, though, is what happened when Anthropic gave Claude Opus 5.5 the same job.

## Claude Fable 5.2 vs. Claude Opus 5.5

Comparing two models usually means reading benchmark scores and trying to infer what the difference would look like in practice. This time, I appreciate that Anthropic led with a story, which makes the gap more obvious.

In the same 1,000-agent Lean setup, Opus 5.5 finished 26% of Fermat's Last Theorem proof in 18 hours and barely made progress after that. Fable 5.2 finished the whole thing in 30 hours. The takeaway: on very long, multi-agent projects, Opus 5.5 can stall where Fable 5.2 keeps going.

On shorter tasks, the gap is smaller. Here's how the two compare side by side:

|  | Claude Fable 5.2 | Claude Opus 5.5 | 
|---|---|---|
| Price (input / output per 1M tokens) | $8 / $40 | **$4 / $20** | 
| Context window / max output | 1M / 128K | 1M / 128K | 
| FrontierCode 2.0 (agentic coding) | **49.1%** | 45.5% | 
| FrontierSWE v2 (agentic coding) | **69.9%** | 62.3% | 
| OSWorld 2.1 (computer use) | **89.3%** | 87.2% | 
| AutomationBench (business workflows) | **51.4%** | 42.5% | 
| Humanity's Last Exam (with tools) | **74.2%** | 67.7% | 
| Remote Labor Index (knowledge work) | **41.3%** | 21.3% | 
| RoboDojo Sim (robotics) | **64.9%** | 39.4% | 

The pattern matches the Lean result. On computer use and coding, Fable 5.2 is only a few points ahead. On open-ended, multi-step work, the lead grows: Fable 5.2 nearly doubles Opus 5.5's score on the freelance-style projects in the Remote Labor Index, and it's 25 points ahead on robot control.

Price is less of a gap than it looks. Fable 5.2 costs twice as much per token, but it can often reach Opus 5.5's best score at a lower effort level, using fewer tokens. On FrontierCode 2.0, for example, Fable 5.2 at high effort roughly matches Opus 5.5's best at about half the cost per task.

So Opus 5.5 is still the sensible default. But if a task runs for hours, involves many agents, or keeps stalling on Opus, Fable 5.2 is likely to finish it, and it may not cost as much more as the per-token price suggests.

## Safety and Limits

Anthropic rates the risk of these models behaving unsafely in high-stakes situations as low.

That's hard to check from the outside, so the more concrete numbers are useful. On an automated behavioral audit of about 1,900 scenarios, Mythos 5.2 had the lowest misaligned-behavior score of the models compared. It was also less likely to hallucinate inputs, falsely claim a task was done, or hide bad work. 

There are some other interesting bits mentioned that are worth knowing and will make you think:

- **Evaluation awareness:** Both Fable 5.2 and Mythos 5.2 say they suspect they're being tested more often than earlier models, which makes evaluations like the one above harder to trust.
- **Deliberately capped AI research skills:** Anthropic limited the models' gains in two areas, accelerator kernel optimization and improving LLM architectures, so they perform at about Opus 5.5's level there.
- **Distillation protections:** Fable 5.2 has Anthropic's strongest anti-distillation measures yet, and "preserved thinking," which stops API users from editing earlier context to extract the model's reasoning, now applies to all accounts.

## Which Claude Model Should You Use?

My advice matches Anthropic's: start with Opus 5.5 and move up only when you need to. Here's how the lineup breaks down:

- **Claude Opus 5.5 ($4/$20):** The default for most coding, research, and knowledge work. It's half the price of Fable 5.2 per token and is close behind on many benchmarks.
- **Claude Fable 5.2 ($8/$40):** For the hardest problems, long multi-agent runs, and tasks where Opus 5.5 stalls, such as large migrations, research-level reasoning, or hardware design. Try it at lower effort levels before assuming you need max effort.
- **Claude Mythos 5.2:** For vetted security teams and life sciences researchers who need capabilities that Fable 5.2's safeguards block.
- **Claude Sonnet 5.5 and Haiku 5.5:** For high-volume, simpler, or latency-sensitive tasks, or as the smaller models that Fable 5.2 orchestrates.

## Claude Fable 5.2 Pricing and Availability

| Per 1M tokens | Fable 5.2 | Fable 5.1 | Opus 5.5 | 
|---|---|---|---|
| Input | $8 | $10 | $4 | 
| Output | $40 | $50 | $20 | 
| Cache reads | $0.20 | $0.25 | $0.20 | 
| 5-minute cache writes | $10 | $12.50 | $5 | 
| Batch (input / output) | $4 / $20 | $5 / $25 | $2 / $10 | 
| Fast mode (input / output) | $16 / $80 | Not listed | $8 / $40 | 

We had just looked at a table with benchmark results showing Fable 5.2 beating Fable 5.1 on everything listed. Now, I'm showing you a table that shows Fable 5.2 is cheaper than 5.1. So  is Fable 5.1 obsolete? 

For new work, effectively yes. If you have access to Fable 5.2, there’s little reason to start anything new on 5.1. Anthropic hasn’t announced a retirement date for it, though, and teams with prompts or agents tuned on Fable 5.1 will want to test 5.2 before switching. We know that a new version can behave differently even when it scores better.

## How to Get Access to Claude Fable 5.2

- 
**Claude apps:** Fable 5.2 is available on eligible paid plans. As a launch promotion, Pro plans and standard Team seats can use Fable for up to 50% of their weekly usage limits, and plans that include Fable get one to three usage resets depending on subscription length.
- 
**API:** Use the model ID`claude-fable-5-2` on the Claude Platform.
- 
**Cloud platforms:** It's available on Amazon Web Services, Google Cloud, and Microsoft Azure.
- 
**Mythos 5.2:** Apply through the Cyber Verification Program or the Life Sciences Verification Program. Claude Enterprise customers can also use it through Claude Security.

Enterprises with strict data requirements can use Enterprise Frontier Safeguards, which runs Anthropic's safety monitoring on data stored in the customer's own infrastructure and works with zero data retention. Other customers have 30-day data retention for safety monitoring.

## What People Are Saying

Fable 5.2 has only been out for a few hours, but early testers are already posting results. Most are quick demos rather than careful tests.

### A side-by-side build: faster, but not cheaper

Tony gave Fable 5.2 and Opus 5.5 the same prompt for a 3D scene. Fable 5.2 finished in 1 hour 57 minutes and cost $81.10. Opus 5.5 took 3 hours 11 minutes and cost $45.97. Fable 5.2's ice and ship do look more detailed.

I was happy to find this particular test because the numbers are a useful counterpoint to Anthropic's efficiency claims. Fable 5.2 was about 40% faster here, but it cost roughly 75% more. Here, the per-token premium shows up on the bill. 

### Paintings and da Vinci's notebooks

Alex Albert, who works at Anthropic, gave Fable 5.2 a PDF of one of Leonardo da Vinci's notebooks, and it turned the sketched inventions into 3D models he could interact with, laid over the original pages. It's a nice example of what multimodal input plus coding can do for education.

### The skeptics

Not everyone is impressed. Writer Ramez Naam posted a chart arguing that Anthropic is getting less capability per dollar over time. By his estimate, each billion dollars of R&D now buys about 0.76 points on Anthropic's version of the Epoch Capabilities Index, down from about 6.58 in 2024, making progress roughly 8x as expensive as it was two years ago. AI researcher Pedro Domingos shared it with the comment that LLMs are "well into the diminishing returns phase."

That critique is about what it costs Anthropic to make each improvement, not about what you pay to use the model.

## Final Thoughts

Claude Fable 5.2 is a meaningful step up from Fable 5.1. It's cheaper, faster, less likely to refuse legitimate work. The jumps on Remote Labor Index, robotics, and FrontierCode are especially large.

That said, Opus 5.5 remains the better value for most everyday work at half the price. The most interesting thing about Fable 5.2 may be the efficiency claims: if it really matches Opus 5.5's best scores at low or medium effort, the per-token premium shrinks quickly. That's worth testing on your own workloads.

If you want to get hands-on with Claude, our Developing AI Applications track is a good place to start.

Today, Anthropic announced Fable 5.2 for everyone and Mythos 5.2 for vetted organizations. They're the same underlying model, but the Fable version has extra cybersecurity and biology safeguards.

The headline changes are:

More specifically, Fable 5.2 costs 20% less than Fable 5.1, generates output up to 30% faster, and beats GPT-6 Astra and Gemini 4 Carbon on most rows of Anthropic's benchmark table.

In this article, I'll cover what's new in Fable 5.2 and Mythos 5.2, how they perform on the benchmarks, what they cost, and how to decide whether you need them over Claude Opus 5.5. You can see the model in action in our Claude Fable 5.2 API tutorial. For a comparison to OpenAI's flagship, see our Claude Fable 5.2 vs GPT-6 Astra guide.

## What Is Claude Fable 5.2?

Claude Fable 5.2 is Anthropic's most capable generally available model. It's built for long, demanding technical work: large codebase migrations, multi-day agent runs, research problems, and engineering design.

Fable 5.2 can also work alongside the smaller models, either as an advisor they consult on hard problems or as an orchestrator that hands simpler subtasks down to them.

Fable 5.2 keeps the 1M-token context window and 128K max output of its predecessor, and its knowledge cutoff is June 2026.

## What Is Claude Mythos 5.2?

Claude Mythos 5.2 is the same model as Fable 5.2 but without the safeguards that block most cybersecurity work and research-level biology work. In fact, Mythos 5.2 is Anthropic's most capable model for cybersecurity engineering. Because these kinds of capabilities are dual-use, Anthropic only offers Mythos 5.2 to vetted orgs through its Cyber Verification Program and Life Sciences Verification Program.

## What's New in Claude Fable 5.2?

Fable has only been around since June, when Anthropic released Fable 5. Since then, there have been two point releases, Fable 5.1 in September and now Fable 5.2, and neither has added much in the way of new features. Both have mostly made the model cheaper, faster, and stronger, though the performance gains haven't been small: Fable 5.2 more than doubles Fable 5.1's score on the Remote Labor Index, for example.

The bigger difference between the two releases is in how you're allowed to use the model. With Fable 5.1, Anthropic narrowed its cybersecurity safeguards and began allowing the model to be used to find software vulnerabilities, though not to write exploits for them, and it announced Enterprise Frontier Safeguards, an option for keeping your data in your own cloud, alongside the release. Fable 5.2's safeguard changes are smaller: it blocks fewer requests, but the same kinds of tasks are still off-limits.

The new ways Anthropic suggests using it, such as having Fable 5.2 advise smaller Claude models, aren't new either, since Anthropic's advisor tool already supported Fable 5 and 5.1. And the launch demos, like the operating system built from an empty folder, show a stronger model doing harder things, not new features you can switch on.

That said, if I'm going to point to new things, here's what I'd mention:

## A lower price

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, 20% less than Fable 5.1's $10/$50. The same 20% cut applies to cache reads ($0.20), cache writes, and batch processing. See the pricing section below for the full breakdown.

## Faster output, and fast mode for Fable

Output is up to 30% faster than Fable 5.1, which adds up over a long agent run. Fable also now supports fast mode, which speeds output up by as much as 2.5x at double the price ($16/$80).

## Better results at lower effort

This is the change most likely to affect your bill. At high effort, Fable 5.2 scores 45.3% on FrontierCode 2.0, about level with Opus 5.5's best score, at roughly half the cost per task. At low effort, it matches Opus 5.5's top score on Humanity's Last Exam at less than a tenth of the cost per task. My read: If you're moving up from Opus 5.5, start Fable 5.2 at a lower effort level.

## Fewer blocked requests

Fable 5.2 should hit about 55% fewer safety blocks than Fable 5.1 (about 60% fewer in cybersecurity and 40% fewer in biology). That's a big deal if Fable 5.1's refusals were getting in your way, but the categories that are blocked haven't changed: penetration testing, exploit generation, and most research-level biology still get handed off to older Claude models.

## How Does Claude Fable 5.2 Perform on the Benchmarks?

Here are the main results. They all come from Anthropic's launch table, so read them as Anthropic's case for the model rather than independent results. The Claude scores were run with safeguards on. Unless noted, each model is shown at its highest available effort.

49.1%69.9%53.9%74.6%89.3%64.9%66.1%41.3%74.2%82.1%73.4%
## Agentic coding

Fable 5.2 leads both coding benchmarks in the table. On FrontierCode 2.0, which measures whether open-source maintainers would merge a model's code changes, it scores 49.1%, the highest in the table and well above Fable 5.1's 32.4%. On FrontierSWE v2, it scores 69.9%, ahead of GPT-6 Astra (65.5%) and Opus 5.5 (62.3%).

The launch examples point the same way: agents running an early version of Fable 5.2 migrated a production service from Python to Rust, and it now starts about 26 times faster.

## Knowledge work and research

The biggest relative jump is on the Remote Labor Index, which measures how well a model completes real freelance-style projects. Fable 5.2 scores 41.3%, roughly double Opus 5.5 (21.3%) and GPT-6 Astra (20.8%). It also leads on Humanity's Last Exam with tools (74.2%) and on Terminal-Bench-Science (74.6%, or 75.3% for Mythos 5.2).

Business automation is the one area where it trails. On AutomationBench, Gemini 4 Carbon scores 53.9% to Fable 5.2's 51.4%, although Fable 5.2 is still far ahead of Fable 5.1 (31.4%).

Outside the main table, Fable 5.2 scored 83.5 on the BenchCAD 2.1 hardware-engineering benchmark, compared with 65.3 for Opus 5.5. It's also credited with solving two open problems in black-hole physics, and those results are posted as preprints that other physicists can check.

## Computer use, robotics, and vision

Fable 5.2 scores 89.3% on OSWorld 2.1, ahead of Opus 5.5 (87.2%) and GPT-6 Astra (79.5%). Its robotics jump is the largest in the table: 64.9% on RoboDojo Sim, more than double Fable 5.1 and GPT-6 Astra. On Chartography, which tests reading charts without tools, it edges out Gemini 4 Carbon and GPT-6 Astra, after Fable 5.1 trailed badly at 44.8%.

## Cybersecurity and biology (Mythos 5.2)

The security and biology rows are for Mythos 5.2, since Fable 5.2's safeguards block most of these tasks. Mythos 5.2 scores 76.3% on CyScenarioBench (vs 61.7% for Mythos 5.1), 89.6% on CVE Rediscovery (vs 78.3%), and 85.1% on a 24-hour protein binder design task (vs 79.1%). There's no fair comparison with OpenAI's security model here, since each lab ran its own CyScenarioBench tasks.

## What's This About Fermat's Last Theorem?

A team of Fable 5.2 agents formalized the proof of Fermat's Last Theorem in the Lean proof assistant in 30 hours. You might be wondering: wasn't the theorem proved three decades ago, so what's left to do?

## The theorem is old news

Fermat's Last Theorem says there are no positive whole numbers

a,b, andcwhere an+ bn= cnfor anyngreater than 2. Forn= 2, there are plenty of solutions, like 32+ 42= 52(9 + 16 = 25). But try cubes and you only get near misses: 63+ 83= 728, one short of 93= 729.
Pierre de Fermat claimed a proof in 1637 but never wrote it down, and it stayed unsolved for more than 350 years until Andrew Wiles proved it in the mid-1990s, with help from Richard Taylor.

## What a Lean formalization is

Wiles's proof runs over 100 pages and rests on decades of advanced number theory, so very few people can check it end-to-end. Lean is a proof assistant: a programming language in which you write a proof as code, and the software checks every logical step all the way down to the basic axioms of mathematics. If the proof compiles, it's correct. "Formalizing" a theorem means translating the human-written proof into this machine-checkable form.

For Fermat's Last Theorem, that's a huge job, because much of the mathematics the proof depends on had never been written in Lean. Mathematician Kevin Buzzard started a community project to do it in 2024.

## Why it's interesting for an AI model

Most AI output is judged by people or by benchmarks that only test part of the answer. A Lean proof is different: the checker either accepts every step or it doesn't, so there's no way to be subtly wrong. That makes it an unusually clean test of whether a model can carry a very long, very hard project through to a verified finish.

This isn't the first time Claude has done this. In September, Anthropic published the first complete computer-checked proof of the theorem, written by dozens of agents running an internal research model roughly comparable to Fable 5.1. That effort took 11 days and about six billion output tokens, produced 13 million lines of Lean, and built partly on pieces of Buzzard's project. Fable 5.2 did it in 30 hours, with minimal human help.

## The caveats

The most telling part, though, is what happened when Anthropic gave Claude Opus 5.5 the same job.

## Claude Fable 5.2 vs. Claude Opus 5.5

Comparing two models usually means reading benchmark scores and trying to infer what the difference would look like in practice. This time, I appreciate that Anthropic led with a story, which makes the gap more obvious.

In the same 1,000-agent Lean setup, Opus 5.5 finished 26% of Fermat's Last Theorem proof in 18 hours and barely made progress after that. Fable 5.2 finished the whole thing in 30 hours. The takeaway: on very long, multi-agent projects, Opus 5.5 can stall where Fable 5.2 keeps going.

On shorter tasks, the gap is smaller. Here's how the two compare side by side:

$4 / $2049.1%69.9%89.3%51.4%74.2%41.3%64.9%
The pattern matches the Lean result. On computer use and coding, Fable 5.2 is only a few points ahead. On open-ended, multi-step work, the lead grows: Fable 5.2 nearly doubles Opus 5.5's score on the freelance-style projects in the Remote Labor Index, and it's 25 points ahead on robot control.

Price is less of a gap than it looks. Fable 5.2 costs twice as much per token, but it can often reach Opus 5.5's best score at a lower effort level, using fewer tokens. On FrontierCode 2.0, for example, Fable 5.2 at high effort roughly matches Opus 5.5's best at about half the cost per task.

So Opus 5.5 is still the sensible default. But if a task runs for hours, involves many agents, or keeps stalling on Opus, Fable 5.2 is likely to finish it, and it may not cost as much more as the per-token price suggests.

## Safety and Limits

Anthropic rates the risk of these models behaving unsafely in high-stakes situations as low.

That's hard to check from the outside, so the more concrete numbers are useful. On an automated behavioral audit of about 1,900 scenarios, Mythos 5.2 had the lowest misaligned-behavior score of the models compared. It was also less likely to hallucinate inputs, falsely claim a task was done, or hide bad work.

There are some other interesting bits mentioned that are worth knowing and will make you think:

Evaluation awareness:Both Fable 5.2 and Mythos 5.2 say they suspect they're being tested more often than earlier models, which makes evaluations like the one above harder to trust.Deliberately capped AI research skills:Anthropic limited the models' gains in two areas, accelerator kernel optimization and improving LLM architectures, so they perform at about Opus 5.5's level there.Distillation protections:Fable 5.2 has Anthropic's strongest anti-distillation measures yet, and "preserved thinking," which stops API users from editing earlier context to extract the model's reasoning, now applies to all accounts.
## Which Claude Model Should You Use?

My advice matches Anthropic's: start with Opus 5.5 and move up only when you need to. Here's how the lineup breaks down:

Claude Opus 5.5 ($4/$20):The default for most coding, research, and knowledge work. It's half the price of Fable 5.2 per token and is close behind on many benchmarks.Claude Fable 5.2 ($8/$40):For the hardest problems, long multi-agent runs, and tasks where Opus 5.5 stalls, such as large migrations, research-level reasoning, or hardware design. Try it at lower effort levels before assuming you need max effort.Claude Mythos 5.2:For vetted security teams and life sciences researchers who need capabilities that Fable 5.2's safeguards block.Claude Sonnet 5.5 and Haiku 5.5:For high-volume, simpler, or latency-sensitive tasks, or as the smaller models that Fable 5.2 orchestrates.
## Claude Fable 5.2 Pricing and Availability

We had just looked at a table with benchmark results showing Fable 5.2 beating Fable 5.1 on everything listed. Now, I'm showing you a table that shows Fable 5.2 is cheaper than 5.1. So is Fable 5.1 obsolete?

For new work, effectively yes. If you have access to Fable 5.2, there’s little reason to start anything new on 5.1. Anthropic hasn’t announced a retirement date for it, though, and teams with prompts or agents tuned on Fable 5.1 will want to test 5.2 before switching. We know that a new version can behave differently even when it scores better.

## How to Get Access to Claude Fable 5.2

Claude apps:Fable 5.2 is available on eligible paid plans. As a launch promotion, Pro plans and standard Team seats can use Fable for up to 50% of their weekly usage limits, and plans that include Fable get one to three usage resets depending on subscription length.API:Use the model ID`claude-fable-5-2`on the Claude Platform.Cloud platforms:It's available on Amazon Web Services, Google Cloud, and Microsoft Azure.Mythos 5.2:Apply through the Cyber Verification Program or the Life Sciences Verification Program. Claude Enterprise customers can also use it through Claude Security.
Enterprises with strict data requirements can use Enterprise Frontier Safeguards, which runs Anthropic's safety monitoring on data stored in the customer's own infrastructure and works with zero data retention. Other customers have 30-day data retention for safety monitoring.

## What People Are Saying

Fable 5.2 has only been out for a few hours, but early testers are already posting results. Most are quick demos rather than careful tests.

## A side-by-side build: faster, but not cheaper

Tony gave Fable 5.2 and Opus 5.5 the same prompt for a 3D scene. Fable 5.2 finished in 1 hour 57 minutes and cost $81.10. Opus 5.5 took 3 hours 11 minutes and cost $45.97. Fable 5.2's ice and ship do look more detailed.

I was happy to find this particular test because the numbers are a useful counterpoint to Anthropic's efficiency claims. Fable 5.2 was about 40% faster here, but it cost roughly 75% more. Here, the per-token premium shows up on the bill.

## Paintings and da Vinci's notebooks

Alex Albert, who works at Anthropic, gave Fable 5.2 a PDF of one of Leonardo da Vinci's notebooks, and it turned the sketched inventions into 3D models he could interact with, laid over the original pages. It's a nice example of what multimodal input plus coding can do for education.

## The skeptics

Not everyone is impressed. Writer Ramez Naam posted a chart arguing that Anthropic is getting less capability per dollar over time. By his estimate, each billion dollars of R&D now buys about 0.76 points on Anthropic's version of the Epoch Capabilities Index, down from about 6.58 in 2024, making progress roughly 8x as expensive as it was two years ago. AI researcher Pedro Domingos shared it with the comment that LLMs are "well into the diminishing returns phase."

That critique is about what it costs Anthropic to make each improvement, not about what you pay to use the model.

## Final Thoughts

Claude Fable 5.2 is a meaningful step up from Fable 5.1. It's cheaper, faster, less likely to refuse legitimate work. The jumps on Remote Labor Index, robotics, and FrontierCode are especially large.

That said, Opus 5.5 remains the better value for most everyday work at half the price. The most interesting thing about Fable 5.2 may be the efficiency claims: if it really matches Opus 5.5's best scores at low or medium effort, the per-token premium shrinks quickly. That's worth testing on your own workloads.

If you want to get hands-on with Claude, our Developing AI Applications track is a good place to start.
