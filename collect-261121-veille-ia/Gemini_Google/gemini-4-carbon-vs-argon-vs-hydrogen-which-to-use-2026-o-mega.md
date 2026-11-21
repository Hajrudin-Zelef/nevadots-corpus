---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-vs-argon-vs-hydrogen-which-to-use-2026-o-mega
title: "Gemini 4 Carbon vs Argon vs Hydrogen: Which to Use (2026) | o-mega"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "model", "gemini", "hydrogen", "google", "carbon", "three", "price"]
source: https://o-mega.ai/articles/gemini-4-carbon-vs-argon-vs-hydrogen-which-to-use-2026
source_anchor: ""
source_lines: [1, 648]
section: "Gemini_Google"
---
*Since this was written, OpenRouter's model feed lists GPT-6.2 Sol in place of GPT-5.6 Sol and GPT-6.1 Sol; GPT-6.2 Astra in place of GPT-6 Astra.*

*Google now sells three Gemini 4 models. Two of them cost exactly the same per token, and the cheapest one ties the middle one on the leading independent index while finishing tasks several times faster. On December 1 the two frontier models double in price. This guide explains what each model is for, what each really costs per task, and how to choose between them.*

The strangest fact about Google's new lineup is on its own price sheet. **Gemini 4 Carbon** and **Gemini 4 Argon** cost the same: $2 per million input tokens and $10 per million output tokens through November 30, 2026, and $4 and $20 from December 1 - Google AI Studio. Yet on the Artificial Analysis Intelligence Index, Carbon scores **60** and Argon **50** - Artificial Analysis. Same price, ten points apart, and Carbon's cost per task on that index is only about 10% higher.

Then there is **Gemini 4 Hydrogen**, released on November 12 at $1.50 input and $7.50 output per million tokens. At its default setting it scores **51**, one point above Argon, and it finishes the index's tasks in **8.4 minutes** on average against Argon's **30.9** - Artificial Analysis. Google's own safety assessment, published in Hydrogen's model card, reached the same conclusion from a different direction: Hydrogen "did not show a material increase in general capability" compared with Argon, so it did not need a new round of critical capability testing - Google model card.

Read by measured results rather than by marketing names, Google's lineup is **two capability levels sold at three prices**. Carbon sits alone at the top. Below it, roughly the same level of capability is available slowly from Argon or quickly and more cheaply from Hydrogen. That raises the question most comparison pages skip: what is Argon still for? This guide answers it, along with the broader one that brought you here. It covers what each model is and where it came from, every price and the December 1 change, Google's benchmarks lined up across its two separate tables, what independent testers measured, cost per task worked out from first principles, speed, thinking levels, access, routing all three models together, the Claude and OpenAI alternatives, failure modes, and a practical checklist for deciding.

*This guide reflects the Gemini 4 lineup, its prices and the competing models as of November 15, 2026. Google's price change for Carbon and Argon takes effect on December 1, 2026, and independent benchmarks for Hydrogen are still arriving, so verify current figures on the linked official pages before committing a budget.*

## Contents

1. The Short Answer: Which Gemini 4 Model to Use
2. What Each Model Is: Lineage, Roles and the Naming Problem
3. The Price Sheet: Identical Prices, Very Different Bills
4. Google's Benchmarks, Lined Up Across Two Tables
5. What Independent Testers Measured
6. Cost per Task: The Number That Actually Decides
7. Speed: Minutes per Task and the Wait for the First Token
8. Thinking Levels: Three Dials That Overlap
9. Carbon vs Argon: When the Frontier Model Is Worth It
10. Hydrogen vs Argon: The Case For and Against the Middle Model
11. Access, Limits and Where Each Model Runs
12. Using All Three Together: Routing and Orchestration
13. Against the Field: Claude and OpenAI Alternatives
14. Failure Modes and Limitations
15. December 1 and What Comes Next
16. How to Decide: A Practical Checklist

## 1. The Short Answer: Which Gemini 4 Model to Use

For most agent and coding work, use **Gemini 4 Hydrogen** at its default high setting. It matches Argon's capability on the leading independent index, costs less per task on three of the five independent boards that price both, runs three to four times faster, and is the only one of the three with a free tier. For the hardest work (long-horizon agents, security, large refactors, anything where a failed run is expensive), use **Gemini 4 Carbon**, starting at its default medium setting. **Gemini 4 Argon** is the hardest of the three to recommend for new work after November 12: it costs the same per token as Carbon and scores like Hydrogen. It keeps a real but narrow role, covered in sections 9 and 10, and anyone running it today should test Carbon at low and Hydrogen at high before its price doubles on December 1. The strongest setups use two models at once, with Carbon planning and Hydrogen executing.

| If your work is mostly... | Start with | The evidence in one line | 
|---|---|---|
| Agent loops, coding, tool calls, anything a person waits for | **Hydrogen, high** | Index score 51 at $1.87 and 8.4 minutes per task | 
| Long-horizon, high-stakes, security or very long context | **Carbon, medium** (high where tests show it pays) | Index score 60; largest leads on long, open-ended tasks | 
| A mix of planning and execution (most real systems) | **Carbon plans, Hydrogen executes** | Near-Carbon success with 24% less quota and 19% faster in Google's tests | 
| Output-heavy writing, cautious factual answers, Pro-plan or regional needs | **Argon, medium** | Lower hallucination rate (4% against Carbon's 11%) and fewer output tokens | 
| Yes-or-no and pick-a-label decisions, routing | **Gemini Decision 1** | $0.10 per million input tokens and no output charge | 

Each line is developed in the sections that follow, with sources.

### The scorecard: the three models against their nearest competitors

The table below scores the three Gemini 4 models and the Claude and OpenAI models nearest to them on the three criteria that decide most production deployments, using one consistent independent source, the Artificial Analysis Intelligence Index v5.0.1, for every row. **Capability (40%)** is the index score divided by 6.5, so that 65 scores 10. **Cost (35%)** is the cost per index task at the prices that apply from December 1 (because a decision made now will mostly pay those prices), scored as 10 minus 2 points for every doubling above $1. **Speed (25%)** is minutes per index task, scored as 10 minus 2 points for every doubling above 5 minutes. Capability carries the most weight because a model that cannot do the task is worthless at any price; cost comes second because it compounds across every task an agent runs; speed comes third because it matters enormously for some products and not at all for overnight batch work.

| # | Model and setting | Capability (40%) | Cost per task, Dec 1 (35%) | Speed (25%) | Final | 
|---|---|---|---|---|---|
| 1 | GPT-6.1 Terra (max) | 7.8 - index 51 | 10.0 - $0.96 | 7.0 - 14.1 min | **8.4** | 
| 2 | **Gemini 4 Hydrogen (high)** | 7.8 - index 51 | 8.2 - $1.87 | 8.5 - 8.4 min, fastest here | **8.1** | 
| 3 | GPT-6.1 Sol (max) | 8.5 - index 55 | 9.3 - $1.27 | 5.6 - 22.9 min, 191 s to first token | **8.1** | 
| 4 | **Gemini 4 Hydrogen (xhigh)** | 8.3 - index 54 | 6.7 - $3.16 | 6.9 - 14.8 min | **7.4** | 
| 5 | Claude Opus 5.5 (max) | 9.1 - index 59 | 3.3 - $10.04 | 5.3 - 25.4 min | **6.1** | 
| 6 | **Gemini 4 Carbon (high)** | 9.2 - index 60, second only to Fable 5.2 | 3.1 - $10.96 ($5.48 until Nov 30) | 4.5 - 33.8 min | **5.9** | 
| 7 | Claude Sonnet 5.5 (max) | 8.5 - index 55 | 3.4 - $10.02 | 4.5 - 33.3 min | **5.7** | 
| 8 | **Gemini 4 Argon (high)** | 7.7 - index 50 | 3.4 - $9.94 ($4.97 until Nov 30) | 4.7 - 30.9 min | **5.4** | 
| 9 | Claude Fable 5.2 (max) | 10.0 - index 65, the highest measured | 1.0 - $26.54 | 3.7 - 44.5 min | **5.3** | 

Data from each model's Artificial Analysis page, with Hydrogen's high-setting cost via VentureBeat (sources in sections 5 and 13); December costs for Carbon and Argon calculated from Google's price change. At today's promotional prices, Carbon would score **6.6** and Argon **6.1**.

Read the table for what it is: a value ranking for typical production agent work. It rewards cheap, fast competence, which is why OpenAI's mid-tier models and Hydrogen lead it. A team whose problem is hard enough that only the best result will do should read the Capability column alone, where Claude Fable 5.2 and Gemini 4 Carbon lead. And the table measures one benchmark suite: the other evaluators in section 5 order costs differently, which is why section 6 explains how to work out cost per task for your own workload.

## 2. What Each Model Is: Lineage, Roles and the Naming Problem

The three models arrived over six weeks, and the order in which they arrived explains most of their differences. Each one was built from the one before it, which is why two of them share a price and why the cheapest one is so capable.

### Argon came first

**Gemini 4 Argon** was the first Gemini 4 model. Google announced it on **September 30, 2026**, but only for a set of trusted cyber defenders in its Fairwind Program, Google's own engineers and a small group of testers, and it released that first version "without cyber guardrails" so defenders could use its full security capabilities - Google. Thirteen days later, on **October 13**, Google made it globally available through the paid Gemini API and AI Studio, in Antigravity for Google AI Ultra subscribers, and in the Gemini app and AI Mode in Search for AI Pro and Ultra subscribers - Google. Its launch post positioned it for complex, multi-step work in coding, finance, law, cybersecurity and science, and cited an "industry-leading 1M unified input-output token limit."

Google's pricing page now describes Argon as "our efficient Gemini 4 frontier reasoning model," delivering "faster responses and lower token spend per turn" than Carbon across complex coding, agentic, math and science tasks - Google AI Studio. That one sentence is the most useful description of Argon in Google's documentation, because it defines the model by what it does not do: it does not think as long as Carbon. We covered Argon in depth when it launched, in our Gemini 4 Argon guide.

### Carbon was built on Argon

**Gemini 4 Carbon** followed on **October 28**. Its launch post calls it Google's "strongest model with state-of-the-art cybersecurity and long horizon capabilities" and says it "builds on Gemini 4 Argon," sharing Argon's safety and security stack - Google. Like Argon, it went first to a subset of cyber defenders while Google took part in the U.S. government's voluntary pre-release testing process. On **November 10**, Google opened it to all paying developers and enterprises through Google AI Studio, the paid Gemini API tier, Gemini for business, Antigravity and CodeMender, and began rolling it out to Google AI Ultra subscribers in the Gemini app and AI Mode - Google.

The pricing page calls Carbon "our most capable reasoning model, built for complex software engineering, deep mathematical and scientific problem solving, high factual accuracy, autonomous multi-step agents, and long-decode generation up to 1M tokens" - Google AI Studio. The November 10 post adds the clearest statement Google has made about how the two frontier models divide the work: Argon is the best fit for most software engineering and knowledge work, while Carbon is built for harder tasks and spends more tokens exploring and verifying - Google.

Carbon's model card is more direct still: "Gemini 4 Carbon is based on Gemini 4 Argon" - Google model card. Argon, by contrast, is described in its own card as "a new frontier model" that "is not a modification or a fine-tune of a prior model" - Google model card. The phrase "based on" matters for the price question. If Carbon is Argon with further training and a habit of thinking longer, then the cost to serve one Carbon token is close to the cost to serve one Argon token, and identical per-token pricing is simply honest accounting. The difference between the two models then lives entirely in how many tokens each one spends on a task, which is exactly what the independent measurements in sections 5 and 6 show.

### Hydrogen was adapted from Carbon

**Gemini 4 Hydrogen** arrived on **November 12**. Its model card states three times that it "is adapted from Gemini 4 Carbon" and refers readers to Carbon's model card for its architecture, training data and hardware - Google model card. The card describes Carbon and Argon as "built for deep, long-horizon reasoning on your hardest problems," while Hydrogen "is built for rapid execution across diverse agentic workflows." It accepts text, images, audio and video, shares a combined input and output window of up to 1M tokens, returns up to 64K output tokens per synchronous request (up to 1M through a background "long decode" mode) and has a knowledge cutoff of **March 2026**.

Hydrogen is the only one of the three on the free tier of the Gemini API, and it is the model Google chose for Search, where it is "now available globally in Google Search for all users" - Google. In the consumer Gemini app, the model picker now labels it "Everyday help," with Argon as "Code & analysis" and Carbon as "Most complex work," and free users reach Hydrogen through automatic model selection - 9to5Google. Our Gemini 4 Hydrogen guide covers that model on its own in more detail; this guide focuses on the choice between the three.

### Why distillation produces a cheap model this good

A model "adapted from" a stronger parent is usually a distilled one: a smaller network trained to reproduce the parent's behavior, including its reasoning traces and tool-use habits. Distillation transfers capability efficiently because the student learns from the teacher's full output distribution rather than from raw data alone. Google has not published Hydrogen's size or method, so the details are unknown, but the outcome is consistent with the technique: Hydrogen generates about **169 output tokens per second** at its default setting - Artificial Analysis. Argon manages 63 at the same setting - Artificial Analysis, so Hydrogen writes nearly three times as fast, which is what a smaller network on the same hardware looks like. Its capability, meanwhile, was inherited from a parent that scores ten points above Argon. A student of Carbon landing level with Argon is not a coincidence; it is roughly what you would expect.

That gives the most useful mental model of the lineup. Carbon is the frontier. Argon is an earlier, more frugal version of the same frontier family. Hydrogen is Carbon compressed for speed and cost, landing at Argon's level of capability.

### The naming problem

Developers have not warmed to the chemistry. On the Hacker News thread for Hydrogen's launch, one commenter pointed out that Hydrogen (atomic number 1) being weaker than Carbon (6) makes sense, but Carbon being stronger than Argon (18) does not, while the other labs use legible ladders: Luna to Terra to Sol to Astra at OpenAI, and Haiku to Sonnet to Opus to Fable and Mythos at Anthropic - Hacker News. The order of release explains the names better than the periodic table does: Argon was named first, Carbon was a later and stronger successor, and Hydrogen took the lightest element for the lightest model.

The simplest fix is to ignore the elements and think in roles. Hydrogen is the workhorse that runs most traffic. Argon is the economical frontier model. Carbon is the model you reach for when the task is hard enough that being wrong is expensive. Google's own coding environment shows all three side by side, along with two "Orchestrator" options that combine them (section 12):

The picker above lists each model with its thinking level ("Medium") and puts Claude Opus 5.5 and Claude Sonnet 5.5 in the same menu, a reminder that even Google's own agent environment treats model choice as a per-task decision rather than a brand loyalty.

## 3. The Price Sheet: Identical Prices, Very Different Bills

Prices are where the three models look most alike and behave most differently. The table below shows the standard paid rates per million tokens, with thinking tokens billed as output, taken from Google's pricing page - Google AI Studio.

| Model | Input, to Nov 30 | Output, to Nov 30 | Input, from Dec 1 | Output, from Dec 1 | Cached input | Free tier | 
|---|---|---|---|---|---|---|
| **Gemini 4 Carbon** | $2.00 | $10.00 | **$4.00** | **$20.00** | $0.10, then $0.20 | No | 
| **Gemini 4 Argon** | $2.00 | $10.00 | **$4.00** | **$20.00** | $0.10, then $0.20 | No | 
| **Gemini 4 Hydrogen** | $1.50 | $7.50 | $1.50 | $7.50 | $0.075 | **Yes** | 

Every price component of Carbon and Argon doubles on December 1, including the cache rate, so any cost you measure on them today simply doubles next month. Hydrogen has no scheduled change. Both frontier models carry the same note on Google's page: "not available on the Free tier," and every token the model processes is billed, "including thinking tokens and internal tool system instructions and function declarations when using grounding."

### The other ways to buy the same models

Google sells each model through several service tiers, and the gaps between them are as large as the gaps between the models themselves.

| Tier | Carbon and Argon, to Nov 30 | Carbon and Argon, from Dec 1 | Hydrogen | What it trades | 
|---|---|---|---|---|
| **Standard** | $2 / $10 | $4 / $20 | $1.50 / $7.50 | The default | 
| **Batch** | $1 / $5 | $2 / $10 | $0.75 / $3.75 | Asynchronous, half price | 
| **Flex** | $1 / $5 | $2 / $10 | $0.75 / $3.75 | Sheddable capacity, half price | 
| **Priority** | $3.60 / $18 | $7.20 / $36 | $2.70 / $13.50 | Reliability at 1.8x | 
| **Contributor** | Argon only: $1 / $5 | Argon only: $1 / $5 | $0.75 / $3.75 | Invite-only, data used to improve products | 

All figures are input and output per million tokens, from Google's pricing page - Google AI Studio. Three details in that table change real decisions. First, **Batch and Flex halve every price**, which means Carbon on Batch after December 1 costs exactly what Carbon on Standard costs today. Any overnight or non-interactive workload that moves to Batch absorbs the whole price increase. Second, the **Contributor tier** exists for Argon and Hydrogen but is not listed for Carbon; it is an invite-only preview, and it is the one paid tier where Google says your content is used to improve its products, so it suits public data and not client documents. Third, the Contributor price for Argon carries no December date on Google's page, which, if it holds, would make it a quarter of Argon's new standard rate for those who have access.

### Search grounding and caching: two line items outside the token price

Grounding with Google Search costs the same on all three models: 5,000 free requests per month shared across all Gemini 3 and newer models, then **$14 per 1,000 requests**, with a daily cap of 1,500 requests, and it is not offered on Hydrogen's free tier - Google AI Studio. A research agent that grounds twenty searches per task pays $0.28 per task in search fees once past the free allowance, which on Hydrogen can exceed the entire token bill. Search fees do not shrink when you choose a cheaper model, so the cheaper the model, the larger the share of the bill they become.

Caching is the other hidden line. All three models list context caching as **implicit only**, at a 95% discount on cache hits. Implicit caching is automatic and free, but it only works when the start of a request is byte-for-byte identical to a recent one. That makes prompt layout a cost decision on every Gemini 4 model: stable system prompt and tool definitions first, new turns appended at the end, nothing volatile near the top. Section 6 shows why the cache hit rate can move an agent's bill more than the choice between models does, and our prompt caching guide covers prefix design in detail.

### What identical pricing tells you

Google could have priced Argon below Carbon, the way OpenAI and Anthropic price their tiers. It did not, and the structural reason is worth spelling out. A provider's cost to serve a request is driven by the size of the model and the number of tokens it processes. If Carbon is a further-trained Argon of similar size (section 2), each token costs Google about the same to produce, so a lower per-token price for Argon would mean selling the same compute for less. What Argon offers instead is fewer tokens per task. Google is letting buyers choose between a model that thinks longer and one that thinks less, at a single rate for the compute either one uses.

The consequence is that the **per-token price** tells you nothing about which of the two is cheaper for you. Only the number of tokens your tasks consume does, and that depends on the workload. Section 6 works through it.

## 4. Google's Benchmarks, Lined Up Across Two Tables

Google never published one table with all three models in it. Carbon's launch post compares Carbon and Argon against GPT-6 Astra, Claude Opus 5.5 and Claude Fable 5.1 on seventeen benchmarks. Hydrogen's model card, two weeks later, compares Hydrogen against Claude Sonnet 5.5 and GPT-6.1 Terra on ten. Six benchmarks appear in both, under the same names and versions, and lining them up gives the closest thing to an official three-way comparison that exists.

Here is Carbon's table as Google published it:

Carbon's column is highlighted in blue on fifteen of the seventeen rows, meaning it led. The two rows it did not lead are FrontierSWE 2 (GPT-6 Astra, 65.5%) and OSWorld-2.1 (Claude Opus 5.5, 87.2%). Argon, in the second column, is never the leader, which is what you would expect of a model Google now positions as the efficient option.

### The six benchmarks in both tables

| Benchmark (what it tests) | Gemini 4 Carbon | Gemini 4 Argon | Gemini 4 Hydrogen | 
|---|---|---|---|
| **Agents' Last Exam v1.1** (multi-step agent tasks) | **77.4%** | 69.0% | 71.6% | 
| **OSWorld-2.1** , offline subset (operating a desktop) | **81.5%** | 74.7% | 77.6% | 
| **Vending-Bench 2** (running a simulated business, bank balance) | **$16,484** | $13,718 | $14,549 | 
| **Chartography** (reading charts) | **72.5%** | 71.6% | 66.6% | 
| **Terminal-Bench-Science 0.1** (science tasks in a terminal) | **64.8%** | 52.9% | 51.4% | 
| **SWE-sweep** (agentic coding) | 9.6% | 4.3% | **13.5%** | 

Carbon and Argon figures come from Carbon's launch post - Google, and Hydrogen's from its model card - Google model card. Both are Google's own measurements, published two weeks apart, and Google ran each model at its highest thinking setting unless noted: high for Carbon and Argon, xhigh for Hydrogen - Google DeepMind. Treat small differences (a point or two) as noise.

Three patterns stand out. **Carbon leads five of the six**, usually by a wide margin. **Hydrogen beats Argon** on every agentic row (Agents' Last Exam, OSWorld, Vending-Bench and SWE-sweep), the benchmarks that reward completing many steps in sequence. **Argon beats Hydrogen** on the reasoning-heavy rows (Chartography by five points, Terminal-Bench-Science by 1.5).

One result is an anomaly worth understanding. On SWE-sweep, a hard agentic coding benchmark with low scores across the board, Hydrogen posts the best result of the three, 13.5% against its parent's 9.6%. Google's methodology notes explain why: Carbon's SWE-sweep score was computed with mini-swe-agent, a deliberately minimal open-source harness - Google DeepMind, while Hydrogen's SWE-sweep and Agents' Last Exam scores were computed inside Google's own Antigravity CLI, a full agent environment. Those two rows therefore compare harnesses as much as models, and the clean three-way comparisons are OSWorld, Vending-Bench, Chartography and Terminal-Bench-Science. The anomaly carries its own lesson for builders: the scaffolding around a model can move a score by more than the gap between two model tiers, which section 12 returns to.

### A seventh comparison: Terminal-Bench 4.0 as Artificial Analysis runs it

Terminal-Bench gives a model a command-line environment and a task (configure a server, fix a build, process data) and checks the end state. Google's two tables report different versions of it (4.0 for Carbon, 4.1 for Hydrogen), so they cannot be lined up directly. But Artificial Analysis runs Terminal-Bench 4.0 itself, and Carbon's table cites that run: Carbon **65.2%**, Argon **57.1%** - Google. On the same Artificial Analysis run, Hydrogen solved **54%** at both its high and xhigh settings - VentureBeat. The order is Carbon, then Argon three points ahead of Hydrogen, then a long way down to the small models. For an agent whose main job is working in a shell, that ranking matters more than the averages.

### Where Carbon's lead over Argon is large, and where it is not

The rows that only Carbon's table reports are the best evidence for the Carbon-or-Argon decision, because both models were measured together, at the same time, by the same team. Sorted by the size of the gap, they tell a clear story.

| Benchmark | Carbon | Argon | Gap (points) | 
|---|---|---|---|
| ExploitGym (exploiting vulnerabilities) | 54.8% | 42.0% | **+12.8** | 
| GraphWalks, 256K to 1M tokens (long-context reasoning) | 96.2% | 84.2% | **+12.0** | 
| Terminal-Bench-Science 0.1 | 64.8% | 52.9% | **+11.9** | 
| Agents' Last Exam v1.1 | 77.4% | 69.0% | +8.4 | 
| Terminal-Bench 4.0 (Artificial Analysis) | 65.2% | 57.1% | +8.1 | 
| OSWorld-2.1 (offline subset) | 81.5% | 74.7% | +6.8 | 
| FrontierSWE 2 (software engineering) | 61.2% | 55.0% | +6.2 | 
| SWE-sweep | 9.6% | 4.3% | +5.3 | 
| SEC-bench Pro (finding vulnerabilities) | 88.2% | 83.4% | +4.8 | 
| Blueprint-Bench 2 (similarity score) | 58.6% | 54.4% | +4.2 | 
| RiemannBench (mathematics) | 80.0% | 76.0% | +4.0 | 
| CWE-bench (patching vulnerabilities) | 71.0% | 68.0% | +3.0 | 
| AutomationBench (Zapier) | 53.9% | 51.3% | +2.6 | 
| Vals Index v2.2 (finance, legal, tax, coding) | 70.4% | 69.2% | **+1.2** | 
| Chartography | 72.5% | 71.6% | **+0.9** | 

All figures from Carbon's launch post - Google. The biggest gaps are on tasks that are **long, open-ended** or need verification: exploiting a vulnerability end to end, reasoning across hundreds of thousands of tokens, running science workflows in a terminal, completing multi-step agent tasks. The smallest gaps are on tasks that are **bounded**, mostly about reading and judgment: interpreting a chart, answering domain questions on finance, law and tax (the Vals Index), or completing a predefined business automation.

That split follows directly from what Google says distinguishes the two models. Carbon spends more tokens exploring and verifying. On a bounded task, extra exploration finds little that a careful first pass did not, so the two models converge. On a long task, every step is a chance to go wrong, and a model that checks its work and backs out of dead ends compounds that advantage across the whole trajectory. Google says Carbon has run internally "for days," persisting through dead ends on multi-step engineering tasks - Google. The practical rule is simple: the longer and less structured the task, the **more Carbon is worth** over Argon, and on short, well-specified work the difference nearly disappears.

### Reading vendor benchmarks without being misled

Every lab chooses benchmarks that flatter its model, and Google's tables have the usual blind spots: no Carbon-versus-Hydrogen table at all, no cost per task on most rows, and comparisons against competitors' previous releases (Claude Fable 5.1 rather than 5.2, which shipped after Carbon's launch). Three habits keep vendor tables useful. Compare only within the same benchmark version and harness. Weight independent measurements above vendor ones wherever both exist. And map each benchmark to the job your agent actually does rather than averaging across all of them. Our article on why AI coding benchmarks mislead goes deeper into how scores get inflated, and the next section turns to the independent measurements.

## 5. What Independent Testers Measured

Independent measurements matter more than usual for these three models, because Google's own tables never put all three side by side and because the most decision-relevant numbers (cost and time per task) are mostly absent from them. By mid-November, at least seven outside evaluators had published results for two or more of the Gemini 4 models. They agree on the order of capability and disagree, sometimes sharply, on cost.

### Artificial Analysis: the index most buyers look at

Artificial Analysis runs the most widely cited independent index of model capability. Version 5.0.1 combines nine evaluations across knowledge work, coding, science and general knowledge, several of them long agentic tasks with many turns - Artificial Analysis methodology. It measures each model at every thinking level it offers, which turns out to be essential for this comparison.

| Model and thinking level | Index score | Cost per task | Minutes per task | Output tokens to run the index | 
|---|---|---|---|---|
| **Carbon, high** | **60** | $5.48 | 33.8 | 205M | 
| Carbon, medium (default) | 58 | not published | 28.8 | 124M | 
| Carbon, low | 54 | not published | 28.1 | 83.9M | 
| **Hydrogen, xhigh** | 54 | $3.16 | 14.8 | 304M | 
| **Hydrogen, high (default)** | 51 | $1.87 | **8.4** | 110M | 
| **Argon, high** | 50 | $4.97 | 30.9 | 113M | 
| Argon, medium (default) | 48 | not published | 28.5 | 81.7M | 
| Hydrogen, medium | 41 | $0.80 | 3.8 | 54.4M | 
| Hydrogen, low | 27 | $0.29 | 1.4 | 20.3M | 

Carbon's figures come from its Artificial Analysis page - Artificial Analysis, Argon's from its page - Artificial Analysis, and Hydrogen's from its own - Artificial Analysis, except Hydrogen's per-task costs below xhigh, which VentureBeat reported from Artificial Analysis - VentureBeat. Costs use the current $2/$10 price for Carbon and Argon, so both double on December 1.

On the overall leaderboard, Carbon at high is the second-strongest model Artificial Analysis has measured. The only model above it is Claude Fable 5.2, which scores 65 at its maximum setting and 62 at its xhigh and high settings - Artificial Analysis. That is a different position from Argon, which ranks 22nd of 80 - Artificial Analysis, and from Hydrogen, which ranks 16th of 80 at xhigh.

Two warnings come with these figures. First, the index was rebuilt for version 5 in late October, and version 4 scores are not comparable; Argon's launch-day score of 53, published on September 30, belongs to the older version - Artificial Analysis. Second, the figures have moved since launch even within version 5. Artificial Analysis's launch article for Carbon reported **59** on version 5.0, with **$7.76** per task for Carbon and **$7.68** for Argon at the $4/$20 standard price - Artificial Analysis. Halve those to today's promotional price and they sit near the current $5.48 and $4.97, but not exactly, because the evaluation suite itself was revised in between. Always read a cost-per-task figure with its version and its price basis beside it.

The same launch article carries the most important caveat about Carbon in any independent source. On AA-Omniscience, a test of factual knowledge that penalizes confident wrong answers, Carbon's accuracy rose to 58% but its **hallucination rate rose from 4% to 11%** compared with Argon. Carbon knows more, and it also guesses more often when it does not know. That sits awkwardly beside Google's own claim that Carbon cut unsupported claims roughly in half versus Argon - Google, and the likely reconciliation is that the two tests measure different situations: Google's counts claims made during agentic work, while AA-Omniscience asks direct factual questions where the right answer is sometimes "I don't know." For a research or question-answering product, that difference is not academic.

### Vals AI: three models within two points

Vals AI scores models on professional tasks in finance, law, tax and coding. Its Vals Index, updated November 11, puts all three Gemini models in its top six, separated by less than two points.

| Rank | Model | Vals Index | Cost per test | Time per test | 
|---|---|---|---|---|
| 1 | Claude Fable 5.2 | 71.31% | $17.94 | 43m 42s | 
| 2 | Claude Sonnet 5.5 | 70.85% | $16.18 | 1h 26m | 
| 3 | **Gemini 4 Carbon** | **70.45%** | $10.51 | 47m 45s | 
| 4 | Claude Opus 5.5 | 70.14% | $24.77 | 1h 25m | 
| 5 | **Gemini 4 Argon** | **69.39%** | $13.37 | 44m 58s | 
| 6 | **Gemini 4 Hydrogen** | **68.49%** | **$9.84** | **32m 05s** | 

All figures from the Vals Index leaderboard - Vals AI. Two things stand out. The tight spread confirms the pattern from section 4: on bounded professional tasks, the three models converge. And on Vals's cost figures, **Argon costs more per test than Carbon**, $13.37 against $10.51, while Hydrogen is cheapest and fastest. The leaderboard does not say which price basis it used for each model, so treat the cost column as directional. Hydrogen's own Vals page adds that it ranks first of 85 models on Vals's Tax Agent Bench at 52.30%, just ahead of Carbon's 51.78% - Vals AI.

### Agent Arena: real sessions, a different cost story

Agent Arena ranks models on real agent sessions, using signals such as task success, user satisfaction and steerability, and it reports median cost, time and output tokens per task. Its November 11 ranking tells a different cost story from Artificial Analysis.

| Rank | Model | Net improvement | Median cost per task | Median time per task | Median output tokens | 
|---|---|---|---|---|---|
| 1 | Claude Fable 5.2 (max) | 30.48% | $8.56 | 18.6 min | 120.2K | 
| 2 | Claude Opus 5.5 (max) | 19.51% | $5.54 | 15.3 min | 108.1K | 
| 3 | **Gemini 4 Carbon (high)** | **16.90%** | $1.36 | 9.7 min | 45.3K | 
| 5 | **Gemini 4 Hydrogen (high)** | **14.80%** | $1.53 | **5.4 min** | 65.3K | 
| 11 | **Gemini 4 Argon (high)** | **10.72%** | **$0.69** | 7.1 min | 23.2K | 

All figures from Agent Arena - Arena, where Carbon's and Hydrogen's rows are marked preliminary. On real sessions, **Hydrogen cost more** per task than Carbon ($1.53 against $1.36), because it wrote 44% more output tokens, and **Argon was the cheapest** of the three by half while ranking well below both on quality. Argon's frugality is real; it simply did not translate into results users rated as highly.

### The other boards

Four more evaluators fill in the picture, each from a different angle:

- **Zapier's AutomationBench** (business automations across real apps): Carbon at high passes 53.88% of tasks, Argon at high 51.29% and Hydrogen at high 51.14%, with Hydrogen costing**$0.82** per task - Zapier. Zapier prices Argon at its standard list rate ($1.70 per task) and footnotes a promotional figure of $0.85, so Hydrogen matches Argon's pass rate at about the promotional cost and half the standard one.
- **CursorBench 4.0** (Cursor's coding evaluation): Carbon scores 60.4% at high and 60.1% at medium, Hydrogen 57.8% at xhigh and 54.3% at high, and Argon 52.9% at high, against 60.0% for Claude Fable 5.2 at max - Cursor. Cursor estimates costs from published token prices, and on its figures Argon at high ($4.57) costs slightly less per task than Hydrogen at high ($5.59).
- **ARC Prize** (abstract reasoning puzzles): on ARC-AGI-2, Carbon scores 94.2% at medium and 92.1% at high - ARC Prize, while Argon scores 91.3% at high and 87.1% at medium - ARC Prize. On the harder ARC-AGI-3 games, Carbon reached 65.1% against Argon's 49.8% at low. Carbon's high-setting run on ARC-AGI-3 was left out because it "completed only 30 of 55 games," with long gaps between actions causing game sessions to expire.
- **Arena's human-preference boards** : in text chat, Carbon ranks first at 1526 and Argon second at 1522, within each other's margin of error, with Hydrogen 15th at 1490 - Arena. On WebDev Arena, Carbon ranks fourth at 1768, ahead of Hydrogen (10th) and Argon (13th) - Arena.

### What practitioners report

Benchmarks miss the texture of daily use, and the developer threads add it. The most common complaint about the frontier pair is speed. One Google AI Ultra subscriber wrote that "Argon is itself already so slow that I'm not sure Carbon will make sense for most activities" - Hacker News. Another said that in the time Argon or Carbon takes to complete a single task, Opus "already managed to complete multiple things," and that they had switched to 3.8 Flash because of it - Hacker News. A third found that Argon at low and medium "had higher quality results than it did when running in high mode, almost every time" on a visual reasoning project - Hacker News, which echoes Carbon's ARC-AGI-2 result, where medium beat high.

The sharpest critique applies to the whole family. One developer, a self-described Gemini fan, called early results with Hydrogen and Carbon disappointing and argued that labs are trading pass-at-one benchmark gains for "worse disasters at p90/99": overreach, overwork, doing the opposite of what was asked - Hacker News. Section 14 takes that failure profile seriously, because the rare bad run is what costs money in production.

### What the independent evidence adds up to

Line the boards up and a consistent picture emerges. **On capability**, the order is the same everywhere: Carbon first, then Hydrogen at its top settings, then Argon, with the gaps largest on long agentic work and smallest on bounded professional tasks and chat. **On cost per task**, there is no consistent order: Artificial Analysis and Zapier find Hydrogen far cheaper than Argon, Vals finds Argon dearer than Carbon, and Agent Arena and CursorBench find Argon the cheapest of the three. Those disagreements are not noise. They are the workload dependence of agent costs showing through, which is exactly what the next section explains.

## 6. Cost per Task: The Number That Actually Decides

Start from what a task costs, because the per-token price is only one of three factors in it. For any model and any task, the bill is the input tokens the agent sends across all its turns, times the input price after cache discounts; plus the output tokens it generates, thinking included, times the output price; plus tool fees such as search grounding. The price sheet shows the prices. It never shows the two quantities that vary most between models: how many **tokens** a model needs to finish the task, and how many **turns** it takes, since every turn resends the growing context.

That is why two models at the same price can differ in cost, and why a cheaper model can cost more. For Carbon and Argon, which share every price, the per-task cost difference is entirely a difference in tokens. For Hydrogen against either, it is tokens and price together.

### The cost table, before and after December 1

Artificial Analysis publishes cost per task at each model's top setting, and Hydrogen's lower settings are available through VentureBeat's report of the same data. Because every component of Carbon's and Argon's price doubles on December 1, their per-task cost on the same workload doubles too. The December figures below are our calculation from that rule.

| Model and setting | Index score | Cost per task, to Nov 30 | Cost per task, from Dec 1 | Cost per index point, from Dec 1 | 
|---|---|---|---|---|
| **Carbon, high** | 60 | $5.48 | **$10.96** | $0.18 | 
| **Hydrogen, xhigh** | 54 | $3.16 | $3.16 | $0.06 | 
| **Hydrogen, high** | 51 | $1.87 | **$1.87** | **$0.04** | 
| **Argon, high** | 50 | $4.97 | **$9.94** | $0.20 | 

Sources as in section 5: Artificial Analysis for the current figures, December figures calculated by us from Google's published price change.

Three conclusions follow from that table, and they are the core of this guide.

First, Carbon costs only **about 10% more** per task than Argon for ten more index points: $0.51 more today, about $1.02 more from December 1. On this evidence, anything you run on Argon at high would cost almost the same on Carbon at high and score markedly better.

Second, Hydrogen at its default costs **38% of Argon** today and **19%** from December 1, for a one-point higher score. Since Hydrogen's per-token prices are 75% of Argon's today, a per-task cost of 38% implies that Hydrogen processed roughly half as many tokens per task, which is consistent with it finishing tasks in a quarter of the time. From December 1, Argon at high costs **5.3 times** as much per Artificial Analysis task as Hydrogen at high.

Third, Hydrogen at xhigh comes **within six points** of Carbon for **less than a third** of Carbon's December cost. For many teams, the real decision after December 1 is not Carbon against Argon but Carbon against Hydrogen at xhigh.

### Why Carbon costs so little more than Argon

Carbon generated **205 million** output tokens to complete the index at high, 81% more than Argon's **113 million**, yet costs only 10% more. The explanation is the ratio between what an agent reads and what it writes. To complete the index, Argon read **7.55 billion** input tokens, an output-to-input ratio of 1.5% - Artificial Analysis, and Carbon read **7.62 billion**, almost exactly the same - Artificial Analysis. An agent rereads its whole context on every turn, so on long agentic tasks input tokens outnumber output tokens heavily (about 37 to 1 for Carbon and 67 to 1 for Argon on this index), and input is most of the bill even after cache discounts. Carbon's extra thinking inflates the smaller term.

This is the single most useful fact for the Carbon-or-Argon decision, and it cuts both ways. **On input-heavy work** (agent loops, document analysis, long-context reasoning), Carbon's extra thinking barely moves the bill, and its quality advantage comes almost free. **On output-heavy work** (writing long documents, generating code from short specifications, producing reports), the bill is dominated by output, and Carbon's 81% extra output tokens show up nearly in full.

### Four workload shapes, worked through

To make the dependence concrete, here are four common shapes at the December 1 prices. The agent-loop row uses Artificial Analysis's measured costs. The other three are our illustrative calculations from Google's list prices, assuming Argon and Hydrogen produce the same tokens on a bounded task and Carbon produces 1.8 times the output, the ratio Artificial Analysis measured at high.

| Workload shape (illustrative) | Carbon | Argon | Hydrogen | Carbon vs Argon | 
|---|---|---|---|---|
| **Long agent loop** (Artificial Analysis index task) | $10.96 | $9.94 | **$1.87** | +10% | 
| **One-pass document analysis** (300K tokens in, 4K out) | $1.34 | $1.28 | **$0.48** | +5% | 
| **Long-form generation** (3K in, 25K out) | $0.91 | $0.51 | **$0.19** | +78% | 
| **Short classification** , per million calls (2K in, 300 out) | $18,800 | $14,000 | **$5,250** | +34% | 

Our calculations from Google's December 1 prices - Google AI Studio. Halve the Carbon and Argon columns for work done before December 1, or for Batch and Flex jobs after it.

The table confirms the structural point: Carbon's premium over Argon ranges from 5% to 78% depending on whether the work reads or writes. It also shows that Hydrogen is cheapest in every shape, by a factor of two to five, provided its token use matches Argon's. That proviso matters. At their defaults, Hydrogen thinks at high while Argon and Carbon think at medium, and on short single-turn tasks a deliberate model can write several times more tokens than the job needs. For the last two rows, run Hydrogen at medium or low; Cursor's documentation warns that quality "drops noticeably at medium and low, so use those only for simple, latency-sensitive tasks" - Cursor, which describes the short classification row exactly. For truly closed-set decisions (yes or no, pick one of five labels), Google's new Gemini Decision 1 costs $0.10 per million input tokens and nothing for output - Google, which would bring that million-call row down to about **$200**. For open-ended short calls that a decision model cannot handle, the small models from other labs are cheaper still, as our comparison of Muse Ember, Claude Haiku 5.5 and GPT-6 Luna shows.

### Why the independent boards disagree on cost

Section 5 ended with a puzzle: Artificial Analysis finds Argon expensive per task, while Agent Arena and CursorBench find it the cheapest of the three. The workload-shape analysis explains part of it, and the rest is about what a "task" is.

Agent Arena's medians show Argon writing **23.2K** output tokens per task, about half of Carbon's 45.3K and a third of Hydrogen's 65.3K. Working back from its costs, Argon also read far less input per task than either. A model that reads less and writes less per task is doing less per task: it stops earlier, takes fewer turns and asks for confirmation more often. One developer described exactly that behavior, saying Argon "stops and waits for confirmations far more often" than other models - Hacker News. Agent Arena users rated the outcome accordingly, ranking Argon 11th, below both its siblings and below OpenAI's GPT-6.1 Sol and Terra.

So the honest metric is not cost per task but **cost per successfully completed task**. Zapier's board allows that calculation: dividing each model's cost per task by its pass rate gives about **$3.93** per passed task for Carbon at high, **$1.60** for Hydrogen at high, and **$3.31** for Argon at high at standard pricing ($1.66 at the promotional price), all from Zapier's published figures - Zapier. On that measure Hydrogen and promotional-price Argon tie, and at the standard prices that apply from December 1, Hydrogen costs about half as much per success as Argon.

### Caching moves the bill more than the model choice

All three models cache implicitly at a 95% discount, on prompts of at least 4,096 tokens, and Google's caching guide adds that in agent loops it caches the full previous request only if the new request adds at least 2,050 tokens - Google AI Studio. At input-to-output ratios like these, the difference between a well-cached agent and a poorly cached one is a factor of several on the total bill, larger than the gap between Argon and Hydrogen. Before you switch models to save money, measure your cache hit rate from the usage metadata on every call, and fix your prompt layout first. Our prompt caching guide covers how.

## 7. Speed: Minutes per Task and the Wait for the First Token

Speed is where the three models differ most, and it is the dimension most comparison pages ignore. Google labels them plainly on its model overview: Carbon's speed is "Deliberate," Argon's "Moderate" and Hydrogen's "Fast" - Google AI Studio. Artificial Analysis measures what those labels mean.

| Model and setting | Minutes per index task | Output tokens per second | Seconds to first answer token | 
|---|---|---|---|
| Carbon, high | 33.8 | 61 | 72.1 | 
| Carbon, medium (default) | 28.8 | 60 | 22.2 | 
| Carbon, low | 28.1 | 51 | 11.0 | 
| Argon, high | 30.9 | 63 | 43.7 | 
| Argon, medium (default) | 28.5 | 59 | 10.2 | 
| Hydrogen, xhigh | 14.8 | 174 | 46.6 | 
| Hydrogen, high (default) | **8.4** | 169 | 16.8 | 
| Hydrogen, medium | 3.8 | 173 | 11.4 | 
| Hydrogen, low | **1.4** | 167 | **5.7** | 

Figures from each model's Artificial Analysis page: Carbon, Argon and Hydrogen.

The striking pattern is that turning Carbon or Argon down **barely makes them faster**. Carbon at low still takes 28.1 minutes per task, only 17% less than at high, because it still writes at about 50 to 60 tokens per second. The thinking level changes how much a model thinks, not how fast it writes. Hydrogen writes at roughly 170 tokens per second at every setting, nearly three times the frontier pair, so its speed advantage holds whatever level you choose. If speed matters to your product, the setting will not rescue Carbon or Argon. Only the model choice will.

### Why speed changes the economics of agents

For a chatbot, speed is comfort. For an agent, it changes cost and design in four ways.

**A person waiting.** In an editor, a support chat or any interactive tool, 43 seconds before the first token (Argon at high) or 72 seconds (Carbon at high) is not a slower experience but a broken one. Hydrogen at medium answers in about 11 seconds, and at low in under 6.

**Paid wall-clock time.** Agents that run inside sandboxes, virtual machines or browser sessions are often billed by the minute. A task that takes 8 minutes instead of 31 costs about a quarter as much in infrastructure, regardless of tokens.

**Throughput on a fixed fleet.** With a fixed number of concurrent agent slots, faster tasks mean more tasks per slot per day. At Artificial Analysis's averages, one slot running Hydrogen at high completes about 3.7 tasks in the time Argon at high completes one.

**Timeouts and expiring sessions.** Slowness is also a reliability problem. Carbon's high-setting ARC-AGI-3 run was excluded because long gaps between actions made game sessions expire, and Google's model card warns of "occasional slowness or timeout issues" - Google model card. Google's migration guide for Carbon tells developers to use background execution or "set HTTP client timeouts to at least 15 minutes" - Google AI Studio, and the Argon guide says the same. Any external system with a session limit (a payment page, a login, a remote shell) needs that budget designed in.

The flip side is that speed is worth nothing when nobody is waiting. Overnight document processing, evaluation runs and backfills should run on Batch at half price, where a 30-minute task and an 8-minute task finish by morning all the same.

## 8. Thinking Levels: Three Dials That Overlap

Each Gemini 4 model has a thinking dial, set per request with `thinking_level`, and the dials overlap in a way that changes the comparison. Google's documentation lists the options and defaults - Google AI Studio:

| Model | Levels | Default | 
|---|---|---|
| **Carbon** | low, medium, high | medium | 
| **Argon** | low, medium, high | medium | 
| **Hydrogen** | low, medium, high, xhigh | **high** | 

None of the Gemini 4 models supports `minimal`, which returns an error. Google describes high as the "maximum reasoning depth" on Carbon and Argon, and as the recommended default on Hydrogen "for most tasks, including complex coding, agentic execution, tool calling, and multimodal knowledge work." It reserves xhigh for "hard, multi-step coding and agentic problems where high falls short," with the warning that it "can cause overthinking or exhaust `max_output_tokens` on simpler prompts." Its general advice is to run evaluations at every level with a generous output limit and pick the lowest level that meets your quality bar.

### The overlaps that matter

Put the three dials on one scale, using Artificial Analysis scores, and they interleave:

Three overlaps stand out. Carbon at its **lowest setting (54)** outscores Argon at its **highest (50)**. Carbon at low **ties** Hydrogen at xhigh at 54. And Hydrogen at its **default (51)** edges out Argon at its **maximum (50)**. Every Argon setting Artificial Analysis measured is bracketed: above it on the scale sits a Carbon setting at the same price per token, and a Hydrogen setting at a lower one.

The comparison between Carbon at low and Argon at its default medium is the sharpest. Artificial Analysis does not publish a per-task cost for Carbon at low, but its output tokens (**83.9M**) and time per task (**28.1 minutes**) are almost identical to Argon at medium (**81.7M** and **28.5 minutes**), and the two models charge the same per token. Unless Carbon at low reads far more input than Argon at medium, which nothing in the data suggests, the two cost about the same per task, and Carbon scores **six points higher**. On this evidence, Carbon at low is a better version of Argon's default for the same money.

### When higher thinking does not help

More thinking is not always better, and the evidence for that is unusually consistent across these models. On ARC-AGI-2, Carbon at medium scored 94.2% against 92.1% at high - ARC Prize. On CursorBench, Carbon at medium (60.1%) is within a third of a point of high (60.4%) - Cursor. On Artificial Analysis's run of Terminal-Bench 4.0, Hydrogen scored 54% at both high and xhigh, so the higher setting "bought no improvement" while raising the estimated cost from $1.87 to $3.16 - VentureBeat. And one developer reported that Argon at low and medium beat high "almost every time" on a visual task.

The mechanism is not mysterious. Extra thinking helps when a problem has hidden structure that more search can uncover, and hurts when the model talks itself out of a correct first answer, runs out of output budget, or spends time a timed task cannot afford. The practical rule: start Carbon and Argon at their **medium default** and move to high only where an evaluation shows a gain; start Hydrogen at **high** and treat **xhigh** as an exception. Thinking level is also the cheapest dial you have. On Hydrogen, moving from high to medium cuts the Artificial Analysis cost per task from $1.87 to $0.80 and the time from 8.4 minutes to 3.8, at a ten-point cost in score. Which side of that trade you want depends on the task, which is why section 12 suggests choosing the level per request rather than per application.

## 9. Carbon vs Argon: When the Frontier Model Is Worth It

This is the comparison most people searching for these models want settled, so it is worth stating the evidence plainly before the nuance. On every capability measure that includes both models, Carbon is ahead: by ten points on the Artificial Analysis index, by 15 points on ARC-AGI-3, by 7.5 points on CursorBench, by six points of net improvement on Agent Arena, and on all seventeen rows of Google's own head-to-head table. The two models cost the same per token. The only questions are how many more tokens Carbon spends on your work, whether you can tolerate its speed, and whether any of Argon's specific advantages apply to you.

### The case for Carbon

**Long, open-ended work.** Section 4 showed that Carbon's lead grows with task length and uncertainty: twelve points on long-context reasoning across 256K to 1M tokens, nearly thirteen on end-to-end vulnerability exploitation, twelve on science in a terminal, eight on multi-step agent tasks. If your agents run for many steps without a person checking each one, those are the gaps that decide whether the run succeeds.

**Agent reliability.** Google reports that Carbon cut unsupported claims roughly in half versus Argon, reduced "overreach" (doing more than asked) by 26% and "under-service" (doing less than asked) by 29%, and dropped to 0.0% attack success on Gray Swan's indirect prompt injection benchmark against Argon's 0.7% - Google. Its November 10 post adds that on an agentic honesty benchmark of 375 tasks Carbon passed 92% against Argon's 70%, declining to fill in missing experimental data 98% of the time, and that its per-attempt breach rate against an adaptive red-teaming agent fell to 0.8% from Argon's 3.1% - Google. These are vendor numbers, but they measure the behaviors that cause expensive failures in unattended agents.

**Security work.** Carbon leads Google's table on finding (SEC-bench Pro 88.2%), exploiting (ExploitGym 54.8%) and patching (CWE-bench 71.0%) vulnerabilities, and Google reports more than double Argon's success rate at generating secure code. For defensive security teams, Carbon is the obvious choice, and Google offers a less restricted "Carbon for cyber" variant to vetted defenders through its Fairwind Program.

**Almost free on input-heavy work.** On agent loops and document analysis, Carbon's extra thinking adds 5% to 10% to the bill (section 6). When the quality gap is large and the cost gap is small, the decision makes itself.

The video below, from an independent AI engineer, walks through how Carbon differs from Argon in practice and how both compare with Claude Fable 5.2, recorded on the day Carbon opened to all paying developers.

### The case for Argon

Argon's advantages are narrower, but they are real, and each one corresponds to a specific kind of team.

**Conservative factual answers.** On AA-Omniscience, Argon's hallucination rate was 4% against Carbon's 11% - Artificial Analysis. For a question-answering product where a confident wrong answer is worse than "I don't know" (internal knowledge search, compliance lookups, customer answers about policy), Argon's caution is a feature. Measure it on your own data before relying on either number, but do not assume the stronger model is the safer one.

**Output-heavy work.** When the job is mostly writing (long reports, generated code from short specifications, translation of large documents), Carbon's 81% extra output tokens appear almost fully in the bill. In the long-form generation shape from section 6, Carbon costs 78% more than Argon for the same deliverable. If Argon's quality is sufficient for the writing, it is the cheaper frontier option.

**Conversational quality.** In blind human-preference voting on Arena's text leaderboard, Argon (1522) sits within the margin of error of Carbon (1526), on about five times as many votes - Arena. For chat-style answers judged by people, the two are effectively tied, and Argon answers faster: 10.2 seconds to the first answer token at its default against Carbon's 22.2 - Artificial Analysis.

**Access and price channels.** Argon is included in the $19.99-a-month Google AI Pro plan, while Carbon's app access starts with Google AI Ultra at $99.99 - Google. Argon has an invite-only Contributor tier at $1/$5 that Carbon lacks. On Google Cloud's Gemini Enterprise Agent Platform, Argon is offered in more regions (including europe-west2 and asia-southeast1), while Carbon "is available to verified enterprise customers in select regions" - Google Cloud. For a team with a data residency requirement, that alone can decide it.

**A month of production history.** Argon has been generally available since October 13, its Arena ratings rest on over 20,000 votes, and its behavior is known. Carbon's independent ratings are still marked preliminary. A team with a validated Argon pipeline does not need to rush, though it should test Carbon before December 1 (section 15).

### The decision rule

Put the evidence together and the choice reduces to a short rule. **Default to Carbon** for long-horizon agents, security, large refactors and any task where a failed run is expensive, and run it at medium unless an evaluation shows that high pays. **Choose Argon** when the work is output-heavy and Argon's quality suffices, when conservative factual answers matter more than breadth of knowledge, when you need its regions or its plan pricing, or when chat-style responsiveness matters. And before keeping Argon at its default medium for general work, test **Carbon at low**: on Artificial Analysis's data it spends about the same tokens and time as Argon at medium and scores six points higher (section 8).

## 10. Hydrogen vs Argon: The Case For and Against the Middle Model

Google's own Frontier Safety assessment is the cleanest summary of this comparison. Before releasing Hydrogen, Google checked whether it had "meaningful new capabilities or material increases in performance relative to" Argon, the most recent model to go through a full critical capability assessment, and concluded that "Gemini 4 Hydrogen did not show a material increase in general capability" compared with Argon - Google model card. That is a safety finding rather than a product claim, but it means Google itself regards the two as being at roughly the same level. The comparison is therefore about how each one delivers that level.

### Where Hydrogen wins

| Dimension | Gemini 4 Hydrogen | Gemini 4 Argon | Source | 
|---|---|---|---|
| Artificial Analysis index (default setting) | **51** (high) | 48 (medium) | Artificial Analysis | 
| Cost per index task, from Dec 1 | **$1.87** | $9.94 (high) | Artificial Analysis, our December calculation | 
| Minutes per index task | **8.4** | 30.9 (high) | Artificial Analysis | 
| Output speed | **169 tokens/s** | 63 tokens/s | Artificial Analysis | 
| OSWorld-2.1 (Google) | **77.6%** | 74.7% |  | 
| Agent Arena net improvement | **14.80%** | 10.72% | Arena | 
| CursorBench 4.0 (high) | **54.3%** | 52.9% | Cursor | 
| WebDev Arena | **1704** | 1681 | Arena | 
| Free tier and Google Search | **Yes** | No |  | 

On almost everything that measures getting agentic work done quickly and cheaply, Hydrogen is ahead, and it has a higher ceiling through its xhigh setting (54 on the index, 57.8% on CursorBench). It is also the only one of the two you can prototype on for free. Google's own coding environment made it the default model for its hosted Antigravity agent - Google AI Studio.

### Where Argon wins

| Dimension | Gemini 4 Argon | Gemini 4 Hydrogen | Source | 
|---|---|---|---|
| Chartography (reading charts, Google) | **71.6%** | 66.6% |  | 
| Terminal-Bench 4.0 (Artificial Analysis run) | **57.1%** | 54% | Google, VentureBeat | 
| Terminal-Bench-Science 0.1 (Google) | **52.9%** | 51.4% |  | 
| Text Arena (human preference) | **1522** | 1490 | Arena | 
| Vals Index | **69.39%** | 68.49% | Vals AI | 
| Agent Arena median cost per task | **$0.69** | $1.53 | Arena | 
| Long context (GraphWalks, 256K to 1M) | **84.2%** | not reported |  | 
| Cloud regions | **More** | Fewer | Google Cloud | 

Argon's wins cluster in two places: careful reasoning over visual and scientific material, and frugality per interaction. On Agent Arena's real sessions, Argon wrote about a third of Hydrogen's output tokens per task and cost less than half as much, although users rated its outcomes lower. And because Google never reported Hydrogen on GraphWalks, Argon is the one of the two with published evidence of strong reasoning across very long contexts.

Two of Hydrogen's documented weaknesses matter here too. Its model card reports that unjustified refusals rose by 5.57 percentage points compared with Gemini 3.8 Flash, "particularly for multimodal prompts," and that it reaches the alert thresholds (though not the critical levels) for biological and cyber capabilities, which in practice means stronger safety filtering in those domains - Google model card. A workload of security research or life-science analysis may hit refusals on Hydrogen that it would not hit on the frontier models.

### The verdict

For most new work, **Hydrogen replaces Argon**: the same level of capability at a fraction of the cost per agent task, three to four times the speed, a free tier for development and a higher ceiling. Argon keeps a defensible role in four situations: visual and scientific reasoning where its Chartography and science scores matter, very long context work where Hydrogen is unmeasured, interaction-light workloads where its low token use per session keeps costs down, and teams bound by its regions or its consumer plan. Everywhere else, the honest description of Argon after November 12 is a model squeezed from both sides: Carbon above it at the same price, Hydrogen beside it at a lower one.

## 11. Access, Limits and Where Each Model Runs

Which model you can use, and how hard you can push it, depends on where you buy it. The technical specifications are nearly identical across the three, so the differences are almost all about access.

### What is the same

All three share a **1,048,576-token** combined input and output window, a default synchronous output limit of **65,536 tokens** with up to 1M through background "long decode," and the same tool support: function calling, code execution, grounding with Google Search and Google Maps, URL context, file search, structured outputs and computer use. None supports the Live API for real-time voice, and none supports explicit context caching - Google AI Studio. All three also share a **March 2026** knowledge cutoff - Google model card. The sampling parameters `temperature`, `top_p` and `top_k` are ignored on all three, so prompt design rather than sampling settings controls output variety. Google's migration guide for Hydrogen notes that switching between the three requires "no request schema changes" - Google AI Studio, which is what makes the routing patterns in section 12 practical.

### What differs

| Channel | Gemini 4 Carbon | Gemini 4 Argon | Gemini 4 Hydrogen | 
|---|---|---|---|
| **Gemini API free tier** | No | No | **Yes** | 
| **Gemini API paid tier** | Yes, since Nov 10 | Yes, since Oct 13 | Yes, since Nov 12 | 
| **Gemini app** | AI Ultra ($99.99+) | AI Pro ($19.99) and Ultra | AI Plus ($4.99) and up; free users via auto selection | 
| **Google Search** | AI Mode for Ultra | AI Mode for Pro and Ultra | **Everyone** | 
| **Antigravity** | Ultra; Orchestrator (Carbon) for Ultra | Ultra; Orchestrator (Argon) for Pro and Ultra | Pro and Ultra | 
| **Google Cloud (Agent Platform)** | GA, verified enterprises, select regions | GA, more regions | GA | 
| **Cursor** | Yes, at API list price | Yes | Yes | 

Plan details are from Google's subscription page - Google, Antigravity availability from its Hydrogen announcement - Google Antigravity, and Cursor's list from its models page, which bills third-party models "at the model's API list price" - Cursor. On Google Cloud, prices outside the global endpoint run 10% higher, so Hydrogen costs $1.65/$8.25 in a regional endpoint - Google Cloud.

### Rate limits and spend caps

Google does not publish per-model request limits for these models; they appear inside AI Studio per project. What it does publish is a spend-based limit: **$10 per 10 minutes** on usage Tier 1 and **$200 per 10 minutes** on Tiers 2 and 3, returning a `429 RESOURCE_EXHAUSTED` error when exceeded - Google AI Studio. Tier 2 requires $100 of payments plus three days since the first one. That cap matters more for Carbon and Argon than for Hydrogen: at Artificial Analysis's December figures, $10 covers fewer than one Carbon index task but five Hydrogen tasks, so a team testing parallel agents on a new account will hit the cap within minutes on the frontier models. Batch limits also differ: on Tier 1, Carbon and Argon can each have 5 million tokens enqueued against Hydrogen's 3 million.

### A billing rule that arrived on November 13

One more access detail deserves attention because it is easy to miss and expensive to discover. On Google Cloud, "when a completion exceeds 32K tokens in length, users will be charged for retrievable token usage even if the request ultimately fails due to client or server timeouts," effective November 13, 2026 - Google Cloud. Combined with Carbon's and Argon's slow generation, a long request with a short client timeout can now cost money and return nothing. Use background execution for long outputs, or set timeouts generously.

## 12. Using All Three Together: Routing and Orchestration

The best answer to "which Gemini 4 model?" is often "more than one." The three share a request format, so a system can send each step of a task to the model that suits it. Google's own coding environment does exactly that, and its published results show what the pattern is worth.

### Google's Orchestrator: Carbon plans, Hydrogen executes

Alongside Hydrogen, Google launched "Orchestrator" in Antigravity, a model option that pairs Hydrogen with a frontier model inside a single task. Carbon starts with read-only exploration and planning; Hydrogen then makes the edits, updates packages and runs the tests; and if Hydrogen runs into friction, the task returns to Carbon to inspect the state, repair the plan and resume. On Terminal-Bench 4.1, Orchestrator reached "near-Carbon task success" while using **24% less quota** and finishing **19% faster** than Carbon alone - Google Antigravity. An Argon-based Orchestrator is available too, to AI Pro subscribers, and Google suggests Carbon on its own for "massive, multi-worktree engineering tasks" through Antigravity's `/swarm` command.

The structural reason this works is the same asymmetry section 4 found in the benchmarks. Planning, diagnosis and recovery are where long-horizon judgment pays, and they are a small share of an agent's tokens. Editing files, running commands and reading output are where most tokens go, and they need competence and speed more than deep deliberation. Put the expensive, slow model where judgment is concentrated and the fast, cheap one where volume is concentrated, and you get most of the quality for a fraction of the time.

The `/swarm` mode shows the other end of the spectrum. Google's demo below shows Carbon coordinating subagents to model 3D assets, build a browser game and add sound and music, the kind of long, multi-part project where Carbon's persistence is the point.

### A routing architecture you can build

Outside Antigravity, the same idea can be built with three components: a cheap classifier that decides what each step needs, a workhorse that handles most steps, and a frontier model reserved for planning and escalation.

The router is the new piece. Google's Decisions API, launched on November 11, runs Gemini Decision 1, a model that scores a closed set of options (yes or no, one of several categories, a rubric score) rather than generating text. It handles up to 1,000 questions per request in 150 to 250 milliseconds and costs $0.10 per million input tokens with no output charge, which Google says is nearly five times cheaper than Gemini 3.5 Flash-Lite - Google. OpenAI's equivalent went generally available on October 22, running on GPT-6 Luna - OpenAI. Spending a fraction of a cent to decide whether a step needs Carbon, Hydrogen at high or Hydrogen at low is the cheapest optimization in this whole guide, because it lets the thinking level and the model follow the difficulty of each step instead of being fixed for the whole application. Our comparison of decision APIs covers that category in more depth.

### Three rules for routing well

**Escalate on evidence, not on guesses.** The most reliable escalation trigger is a failed check (a test that does not pass, a schema that does not validate, a tool call that errors twice), not a model's own claim that a task is hard. Hydrogen is fast enough that letting it try first and escalating on failure is usually cheaper than predicting difficulty up front.

**Keep the prefix stable across models.** Implicit caching only hits on byte-identical prefixes, and each model has its own cache. A router that flips between models on every turn can destroy the cache hit rate that section 6 showed is worth more than the model choice. Route by phase (planning, execution, review) rather than by individual turn wherever you can.

**The harness matters as much as the model.** Section 4's SWE-sweep anomaly (Hydrogen beating Carbon inside Google's own agent harness, Carbon scoring lower in a minimal one) is a reminder that scaffolding, tool design and context management move results by more than a model tier. Before upgrading a model to fix a failing agent, check whether the agent's tools and context give the model what it needs.

For teams that would rather not maintain routing tables, thinking-level policies and escalation rules at all, there is a different option: platforms that make these decisions internally. **O-mega**, for instance, builds and operates a company's website, app, billing and content through one AI, so which model handles which step is the platform's concern rather than something its users configure. That trades fine-grained control over each routing choice for not having to build the machinery, and which side of that trade suits you depends on whether model routing is part of your product or just a cost of building it.

## 13. Against the Field: Claude and OpenAI Alternatives

Choosing among Google's three models only makes sense if one of them beats what you would otherwise use. Here is where they sit against the Claude and OpenAI models at comparable levels, on the same Artificial Analysis index and cost basis.

| Model and setting | Index | Price per 1M tokens (in / out) | Cost per task today | Cost per task from Dec 1 | Minutes per task | 
|---|---|---|---|---|---|
| Claude Fable 5.2 (max) | **65** | $8 / $40 | $26.54 | $26.54 | 44.5 | 
| **Gemini 4 Carbon (high)** | 60 | $2 / $10, then $4 / $20 | $5.48 | $10.96 | 33.8 | 
| Claude Opus 5.5 (max) | 59 | $4 / $20 | $10.04 | $10.04 | 25.4 | 
| GPT-6.1 Sol (max) | 55 | $2 / $10 | $1.27 | $1.27 | 22.9 | 
| Claude Sonnet 5.5 (max) | 55 | $2 / $10 | $10.02 | $10.02 | 33.3 | 
| **Gemini 4 Hydrogen (xhigh)** | 54 | $1.50 / $7.50 | $3.16 | $3.16 | 14.8 | 
| **Gemini 4 Hydrogen (high)** | 51 | same | $1.87 | $1.87 | **8.4** | 
| GPT-6.1 Terra (max) | 51 | $1 / $5 | **$0.96** | **$0.96** | 14.1 | 
| **Gemini 4 Argon (high)** | 50 | $2 / $10, then $4 / $20 | $4.97 | $9.94 | 30.9 | 

Index scores, costs and times come from each model's Artificial Analysis page: Fable 5.2, Opus 5.5, GPT-6.1 Sol, Sonnet 5.5 and GPT-6.1 Terra; Claude prices from Anthropic - Anthropic, OpenAI prices from OpenAI - OpenAI. The December column applies Google's price change and assumes the others hold their prices.

### What the table says about each Gemini model

**Carbon meets Opus 5.5.** After December 1 the two collide head on. Today Carbon costs about half what Opus 5.5 costs per index task, for one more point. From December 1, Carbon at $10.96 costs slightly *more* than Opus 5.5 at $10.04, and takes 33.8 minutes per task against Opus's 25.4. On Artificial Analysis's workload, the price change turns Carbon from a clear bargain into a near tie with a faster competitor. Other workloads tell a different story: on Vals AI's professional tasks Carbon costs $10.51 per test against Opus's $24.77 - Vals AI, and on Agent Arena's real sessions it costs $1.36 against Opus's $5.54 at today's prices. As always, the per-task comparison depends on the work, and our Gemini 4 Argon vs Opus 5.5 analysis works through the same Google-against-Anthropic cost question for Argon, task shape by task shape.

**Claude Fable 5.2** is the one model clearly above Carbon. Anthropic released it on November 9 at $8/$40, and states that it and the verified-access Claude Mythos 5.2 "are the same underlying model," with Mythos available to vetted cybersecurity and life-sciences organizations through verification programs - Anthropic. Anthropic's own table shows Fable 5.2 ahead of Carbon on FrontierSWE v2 (69.9% against 61.2%), Terminal-Bench-Science (74.6% against 64.8%) and OSWorld 2.1 (89.3% against 81.5%), with Carbon ahead on AutomationBench (53.9% against 51.4%). On Agent Arena, Fable 5.2 leads by a wide margin. It also costs 4.8 times as much as Carbon per Artificial Analysis task today, and 2.4 times as much from December 1. One developer reported that Carbon "matches Fable 5.2" on their own benchmark while remaining "slower and more costly," and another replied that on Artificial Analysis Fable 5.2 costs more than Carbon at every effort level where it scores higher - Hacker News. Both can be true on different workloads, which is the recurring theme of this guide. Our Claude Fable 5.2 vs Opus 5.5 comparison covers the Anthropic side in depth.

**GPT-6.1 Sol and Terra** are the price competitors to Hydrogen. Sol scores 55, above Hydrogen at xhigh, for $1.27 per task, less than half Hydrogen's $3.16. Terra matches Hydrogen's default score of 51 for $0.96, about half Hydrogen's $1.87. What Hydrogen sells against them is time: Sol takes 22.9 minutes per task and waits 190.8 seconds before its first answer token at its maximum setting - Artificial Analysis, while Hydrogen at high takes 8.4 minutes. For overnight batch work, OpenAI's mid-tier models buy intelligence more cheaply. For anything interactive or billed by the minute, Hydrogen's speed is worth its premium. Our GPT-6 Sol vs Astra vs Luna guide covers OpenAI's own tier decision.

**Argon has no row it wins.** Every model near Argon on the index is either cheaper per task (Terra, Sol, Hydrogen), faster, or both. That does not mean Argon is a bad model; it means its specific advantages (conservative factual answers, low output volume, chat quality, regions) are not captured by an index, which is why section 9 lists them separately.

### Switching costs are falling

The practical cost of moving between these models is shrinking, which makes the per-task comparison more important than loyalty. Cursor's model list now carries all three Gemini 4 models alongside Claude Fable 5.2, Opus 5.5, Sonnet 5.5 and Haiku 5.5, billed at each model's API list price - Cursor. Its OpenAI models are still GPT-5.6 Sol, Terra and Luna, even though OpenAI had proposed ending its agreement with Cursor on November 12 and said future models would not be made available there - Tom's Guide. For teams building their own agents, the three Gemini models share a request format, and moving between providers is mostly a matter of re-running evaluations. Our November ranking of LLMs for agents places all of these models in a wider field.

## 14. Failure Modes and Limitations

Every model in this comparison fails in characteristic ways, and knowing them in advance is cheaper than discovering them in production.

### Slowness, timeouts and expiring sessions

Carbon and Argon generate about 60 tokens per second and spend 28 to 34 minutes per Artificial Analysis task at any setting (section 7). That breaks interactive products, but it also breaks automated ones: Carbon's high-setting ARC-AGI-3 run was excluded because sessions expired between its actions, and any agent that drives an external system with a session timeout (a browser login, a payment page, a remote shell) needs that budget designed in. Google's own guidance is to raise client timeouts to at least 15 minutes or use background execution, and to raise `max_output_tokens` to at least 32,768 because "Gemini 4 Carbon can spend more of the limit on thinking than earlier models" - Google AI Studio.

### Verbosity and runaway thinking

All three models think a lot. Hydrogen at xhigh ranks **78th of 79** on Artificial Analysis's verbosity measure, generating 304 million output tokens to complete the index against a median of 77.3 million - Artificial Analysis; Carbon at high ranks 75th of 79. Thinking tokens are billed as output, and Google warns that xhigh "can cause overthinking or exhaust `max_output_tokens` on simpler prompts." The failure looks like an answer that never arrives, or a bill that is several times what the task deserved. The cure is the lowest thinking level that meets your quality bar, chosen per request.

### Confident wrong answers

Carbon's hallucination rate on AA-Omniscience was **11%** against Argon's 4% (section 5). More capable models often know more and guess more. For agents that answer factual questions, build in grounding (search or retrieval) and an explicit path to "I don't know," and measure abstention on your own data rather than assuming the frontier model is the more truthful one.

### Tail failures in unattended agents

The most important critique of the whole family is about rare, expensive runs. One developer argued that labs are trading benchmark gains for "worse disasters at p90/99" in agentic workflows: overreach, overwork, doing the opposite of what was asked, unrelated work - Hacker News. Google's own numbers say Carbon reduced overreach by 26% and under-service by 29% compared with Argon, which is progress, not elimination. Argon has the opposite tendency: developers report that it "stops and waits for confirmations far more often," which is safer and more annoying. Whatever model you choose, the controls are the same: narrow tool permissions, checkpoints before irreversible actions, tests as the definition of done, and logs you actually read.

### Refusals and restricted capabilities

Hydrogen's model card reports more unjustified refusals than Gemini 3.8 Flash, particularly on multimodal prompts, and all three models carry cyber and biology safeguards. Google Cloud's price list names separate "Gemini 4 Argon Cyber Permissive" and "Gemini 4 Carbon Cyber Permissive" models - Google Cloud, and Google limits Carbon for cyber work to vetted defenders in its Fairwind Program - Google. Security and life-science teams should test their actual prompts before committing, and apply for the programs if the public models refuse too much.

### Billing traps

Four billing details catch teams out. Grounding with Google Search costs $14 per 1,000 requests beyond the free monthly allowance, on every model. Caching is implicit only, so a prompt layout that changes its first few thousand tokens pays full price on every turn. Regional endpoints on Google Cloud cost 10% more than the global one. And on Google Cloud, completions longer than 32K tokens are now billed even when the request fails on a timeout. None of these appears in the headline price.

### What none of them does

None of the three supports the Live API, so none is a drop-in for real-time voice agents. None supports explicit context caching. And all three ignore `temperature`, `top_p` and `top_k`, so teams that relied on sampling settings for creative variety or deterministic output need other techniques.

## 15. December 1 and What Comes Next

The December 1 price change is the most immediate fact in this comparison, and it rewards a little preparation.

### Before December 1

**Inventory your Carbon and Argon traffic.** Every component of their price doubles, cache included, so any measured cost simply doubles. Know which workloads will be affected and by how much.

**Re-run your evaluations.** For Argon workloads, test Carbon at low and medium and Hydrogen at high and xhigh. For Carbon workloads, test Hydrogen at xhigh and an Orchestrator-style split (section 12). The evidence in this guide says most Argon traffic should move, but your evaluation is the one that counts.

**Batch everything non-interactive.** Batch and Flex both halve every price, so a Batch job on Carbon after December 1 costs exactly what a Standard call costs today. Overnight analysis, evaluation runs and backfills should never pay the Standard rate.

**Check pinned model names.** Google's pricing page states that from December 1, requests to `gemini-2.5-pro` "will automatically route to Gemini 3.8 Flash" and be billed at its rates - Google AI Studio. A forgotten integration keeps working on a different model. And on January 1, 2027, Gemini 3.8 Flash itself doubles to $1.50/$7.50, exactly Hydrogen's price, which removes the last reason to stay on it; our Gemini 3.8 Flash guide covers that model's history and its introductory pricing.

### Will the doubling hold?

Google has framed $2/$10 as introductory since Argon's first announcement, and Google Cloud describes the discount as "50% credits back on net spend on select models within a given period" - Google Cloud, which reads like a promotion with an end date rather than a list price Google might keep. Nothing in Google's documentation suggests an extension. Budget for $4/$20, and treat any extension as upside.

### The market around the choice

Four developments of the past two weeks frame this decision. **Decision models** have become a category: Google's Gemini Decision 1 arrived on November 11, three weeks after OpenAI's Decisions API went generally available, and both price classification at $0.10 per million input tokens with no output charge, which makes per-step routing nearly free. **Anthropic** released Claude Fable 5.2 and Mythos 5.2 on November 9 and launched its IPO roadshow on November 10, offering 825 million shares at an expected $70 to $78 each for listing on Nasdaq under the ticker ANT - Anthropic; CNBC, citing people familiar with the matter, reports that it plans to price on November 18 and begin trading on November 19 - CNBC. **Enterprise governance** is consolidating around the control layer rather than the model: IBM and Microsoft announced their Imagine Enterprise agent platform on November 10 - PR Newswire, CrowdStrike agreed on November 3 to acquire the agent observability company Fiddler - CrowdStrike, and Microsoft's Ignite conference opens on November 17 with a keynote featuring Agent 365 as "the observability layer for agents, models and assistants" - Microsoft Tech Community. Microsoft sells Agent 365 at $15 per user per month on an annual commitment - Microsoft.

The thread connecting them is structural. As frontier models converge in capability (Carbon, Opus 5.5 and Fable 5.2 within six index points of each other; Hydrogen, Argon and Terra within one), the differences that decide a deployment move to cost per task, speed, and the layer that routes, governs and observes many models at once. Choosing between Carbon, Argon and Hydrogen is a smaller decision than it looks, because the right architecture will use more than one of them and will keep the option to swap any of them out.

### What to watch

No Gemini 4 model has an announced shutdown date in the Gemini API, and Google Cloud lists Argon's earliest retirement as "October 13, 2027 or later" - Google Cloud. Over the next weeks, watch three things: whether independent results for Hydrogen (still preliminary on Arena and missing from ARC Prize) confirm its position level with Argon; whether Carbon's preliminary ratings hold as votes accumulate; and how Google positions Argon once its price doubles, since a model priced like Carbon and performing like Hydrogen has an obvious problem.

## 16. How to Decide: A Practical Checklist

Pulling the evidence together, here is a sequence that works for most teams choosing among the three.

1. **Start with Hydrogen at high** for all agent and coding traffic. It is the cheapest per task on most measurements, three to four times faster than the frontier pair, and level with Argon in capability. Move short, simple calls to medium or low, and closed-set decisions to Gemini Decision 1.
2. **Find the high-stakes tasks.** Long-horizon agents, security analysis, large refactors, high-stakes research. Route those to Carbon at medium, and test whether high pays before using it.
3. **Try the planner-executor split** before paying for Carbon everywhere. Carbon plans and reviews, Hydrogen executes. Google's own version reached near-Carbon success with 24% less quota and 19% more speed.
4. **Keep Argon only where it fits** : output-heavy writing where its quality suffices, conservative factual answers, chat-style responsiveness, its regions or its consumer plan. Otherwise test Carbon at low as its replacement.
5. **Batch non-interactive work** before December 1, and set realistic timeouts for anything running on Carbon or Argon.
6. **Measure cost per successful task** , not cost per token or per attempt. Log tokens, cached tokens, turns and outcomes per task, and re-run your comparison whenever prices or models change.
7. **Fix your prompt layout** for caching before switching models to save money. A stable prefix is worth more than a model downgrade.
8. **Keep a non-Google option evaluated.** GPT-6.1 Sol and Terra are cheaper per task at Hydrogen's level when speed does not matter, and Claude Fable 5.2 is the stronger choice when only the best result will do.

The models will change again within months; the method will not. Measure what a completed task costs and how long it takes, on your own work, at each model's settings, and let those two numbers make the decision that the names and the price sheet cannot.

*This guide reflects Google's Gemini 4 lineup, its pricing and the competing models as of November 15, 2026. Carbon and Argon change price on December 1, 2026, several independent ratings for Carbon and Hydrogen are still preliminary, and AI model prices and benchmarks change frequently, so verify current details on the linked official pages before you commit.*

## What this article rests on

Every model figure above is read from its record, re-read from OpenRouter's model feed every 6 hours, so the figures on this page are current even where the prose around them is older. Prices are OpenRouter's listed price per million tokens: for a lab's closed models that is the lab's own list price, and for an open-weight model it is the price of the host OpenRouter routes to by default, which can differ from the lab's own API and from other hosts.

- Gemini 4 Hydrogen (Google): $1.50 in and $7.50 out per million tokens, cached input $0.075, a 1M token window, listed 12 November 2026.
- Claude Fable 5.2 (Anthropic): $8 in and $40 out per million tokens, cached input $0.20, a 1M token window, listed 9 November 2026.
- Claude Opus 5.5 (Anthropic): $4 in and $20 out per million tokens, cached input $0.20, a 1M token window, listed 22 September 2026.
- **openai/gpt-6.1-sol** : the feeds no longer list it; they list GPT-6.2 Sol in its place. The figures this article gives for it are as written.
- Claude Sonnet 5.5 (Anthropic): $2 in and $10 out per million tokens, cached input $0.10, a 1M token window, listed 28 September 2026.
- GPT-6.1 Terra (OpenAI): $1 in and $5 out per million tokens, cached input $0.05, a 1.05M token window, listed 26 October 2026.
