---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-what-it-is-benchmarks-and-pricing
title: "Gemini 3.7 Flash: What It Is, Benchmarks, and Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "model", "google", "input", "supported", "coding", "weeks"]
source: https://emergent.sh/learn/gemini-3-7-flash
source_anchor: ""
source_lines: [1, 193]
section: "Gemini_Google"
---
#### TL;DR

- Gemini 3.7 Flash is Google's newest workhorse model for coding and agents, released on August 13, 2026, just three weeks after Gemini 3.6 Flash.
- It is an algorithmic refinement of 3.6 Flash, not a new base model, with its biggest gains in coding, agent workflows, and document comprehension.
- On the independent Artificial Analysis Intelligence Index it scores 56 at the high thinking level, ahead of 3.6 Flash at 52.
- Introductory pricing is $0.75 per 1M input tokens and $3.75 output through December 31, 2026, then doubles to $1.50 and $7.50 on January 1, 2027.
- It handles a 1M-token context window, accepts text, image, audio, video, and PDF input, and returns text.


Google is now shipping Flash models faster than most teams can evaluate them, and Gemini 3.7 Flash is the clearest sign yet. It arrived three weeks after its predecessor with measurably better coding and agent performance at the same long-term price. This guide is the hub for everything we have published on the model: what it is, what it can do, how it scores, what it costs, and where it fits against the alternatives. Each section answers the core question and links to a deeper guide when you want the full detail.

## What is Gemini 3.7 Flash?

Gemini 3.7 Flash is Google's fast, low-cost model built for coding and agentic workflows, and it is the most capable Flash-tier release Google has shipped. It sits in the middle of the Gemini lineup: cheaper and faster than the Pro tier, and tuned for high-volume work like software engineering, web development, and multi-step automation rather than frontier reasoning.

Google describes it as an algorithmic refinement of Gemini 3.6 Flash rather than a new base model. The improvements come from changes to the model's reasoning core, informed by developer feedback, which is why it could ship only three weeks after 3.6 Flash while still posting real gains. It keeps the same 1M-token context window and multimodal input as its predecessor, and adds meaningful accuracy on coding, agents, and document comprehension.

## Where it sits in the Gemini 3 line

Gemini 3.7 Flash is the latest step in a fast-moving Gemini 3 release cadence. Google shipped Gemini 3.1 Pro Preview in February 2026, followed by the Flash line through the spring and summer, with Gemini 3.6 Flash landing on July 21, 2026 and 3.7 Flash following on August 13, 2026. Three weeks between Flash releases is unusually short, and it signals a shift in how Google iterates: frequent, feedback-driven refinements to the reasoning core instead of occasional large rebuilds.

That cadence has a practical implication worth naming. If you are building on the Flash line, expect the model under you to improve on a scale of weeks, not quarters, so it is worth designing your evaluations to re-run easily rather than tuning heavily to a single point release.

## Key capabilities and specifications

Gemini 3.7 Flash combines a large context window, broad multimodal input, and a full set of agentic tool capabilities. The table below collects the specifications that matter most when you are deciding whether the model fits your build.

| **Specification** | **Detail** | 
|---|---|
| Model ID | gemini-3.7-flash | 
| Release date | August 13, 2026 | 
| Input types | Text, image, video, audio, and PDF | 
| Output type | Text | 
| Input token limit | 1,048,576 (1M) | 
| Output token limit | 65,536 (64K) | 
| Thinking levels | Low, medium, high | 
| Caching | Supported | 
| Function calling | Supported | 
| Code execution | Supported | 
| Computer use | Supported (preview) | 
| Search and Maps grounding | Supported | 
| Structured outputs | Supported | 
| Image or audio generation | Not supported | 

*Table 1: Gemini 3.7 Flash core specifications, sourced from Google's model documentation (August 2026)*

Two entries deserve a note. The model accepts audio and video as input but returns only text, so it can analyze a video or a recording but cannot generate media. And its tool support, including function calling, code execution, and search grounding, is what makes it usable as an agent rather than a plain chat model.

## What changed from Gemini 3.6 Flash

Gemini 3.7 Flash improves on 3.6 Flash across coding, agents, and knowledge work while holding the same price. The gains are not spread evenly; they concentrate on coding, agent execution, and document work, as the change view below shows.

| **Area of change** | **3.6 Flash** | **3.7 Flash** | **What it unlocks** | 
|---|---|---|---|
| Long-horizon coding (DeepSWE v1.1) | 48.6% | 65.3% | Carries multi-file tasks across more steps before losing the thread | 
| Production code quality (FrontierCode 1.1) | 34.4% | 43.6% | Cleaner first-pass code with fewer fixes | 
| Web and UI generation (Code Arena Elo) | 1538 | 1588 | Functional, feature-complete interfaces in fewer prompts | 
| Agent execution (AutomationBench) | 17.0% | 30.4% | Longer tool loops with less oversight and fewer retries | 
| Document comprehension (GDP.pdf) | 22.0% | 34.0% | Reliable answers pulled from deep inside large files | 

*Table 2: What changed from Gemini 3.6 Flash to 3.7 Flash, with the gain expressed as a capability. Google-reported figures, August 2026.*

The takeaway is that 3.7 Flash is a coding and agent upgrade first, and a knowledge-work upgrade second. The standout is the near 17-point jump on DeepSWE, which is what moves the model from useful-for-snippets to dependable-across-a-feature. The agent and document gains compound that: AutomationBench nearly doubling means fewer human handholds in a long automation, and the price staying flat means none of it costs you more. The one place to stay alert is that these are Google-reported figures on Google's harness, so treat the independent Intelligence Index as the neutral cross-check.

3.6 Flash still holds a narrow edge in one place: CharXiv chart reasoning, where it scores slightly higher than 3.7 Flash. If your workflow leans on reasoning over complex charts, keep it in your regression set. For the full upgrade decision, see our Gemini 3.7 vs 3.6 comparison.

## What Gemini 3.7 Flash is good at, and what it isn't

Gemini 3.7 Flash is strongest on high-volume, cost-sensitive work and weakest on the hardest fully autonomous tasks. Matching the model to the job matters more than any single benchmark number, so it helps to be concrete about both sides.

### 1. Where it excels

It excels at scoped coding and implementation, agent loops that run many quick tool calls, web and UI generation, document and long-context processing, and latency-sensitive or high-frequency workloads where its speed compounds. Its low price makes it a natural default for anything you run at scale, such as a support assistant, a code helper, or a document parser.

### 2. Where it hits a ceiling

It reaches a ceiling on long, tool-heavy agentic tasks that must plan, execute, and recover across many steps without human input. On those, frontier models still lead, and Flash-tier speed and price come with a real limit on the most demanding autonomous work. The honest read is that Gemini 3.7 Flash covers the majority of production workloads well and cedes only the hardest edge cases to pricier models.

## Benchmarks at a glance

Gemini 3.7 Flash posts leading scores for its price tier, competitive with models that cost two to three times as much. This section keeps two kinds of evidence apart: the benchmarks Google reported on its own harness, and the independent read from Artificial Analysis. The generational change from 3.6 Flash is covered above; the focus here is where 3.7 Flash stands against its actual price-tier rivals.

### 1. Vendor-reported benchmarks

The table below is from Google's model card, which reports 3.7 Flash against 3.6 Flash and two similarly positioned rivals, Claude Sonnet 5 and GPT-5.6 Terra. These are Google-run figures, so read them as the vendor's own account.

| **Benchmark** | **Measures** | **3.7 Flash** | **3.6 Flash** | **Claude Sonnet 5** | **GPT-5.6 Terra** | 
|---|---|---|---|---|---|
| FrontierCode 1.1 Main | Production code quality | 43.6% | 34.4% | 42.7% | 41.3% | 
| DeepSWE v1.1 | Long-horizon software engineering | 65.3% | 48.6% | 53.8% | 69.6% | 
| Code Arena (Elo) | Web development | 1588 | 1538 | 1541 | 1523 | 
| Terminal-bench 2.1 | Agentic terminal coding | 85.8% | 78.0% | 80.4% | 87.4% | 
| AutomationBench | Enterprise workflow automation | 30.4% | 17.0% | 10.7% | 23.6% | 
| GDP.pdf | Expert PDF comprehension | 34.0% | 22.0% | 28.0% | 24.7% | 
| GDM-MRCR v2 | Long-context (128k, 8-needle) | 97.0% | 91.8% | 81.5% | 93.5% | 
| LVBench | Long video understanding | 85.4% | 84.2% | 68.5% | 78.9% | 
| HLE-Verified | Multidisciplinary expert reasoning | 53.6% | 51.2% | 31.0% | 51.1% | 
| CharXiv Reasoning (no tools) | Chart information synthesis | 84.5% | 85.2% | 77.0% | 85.9% | 

*Table 3: Vendor-reported benchmarks from Google's Gemini 3.7 Flash model card, August 2026. All figures are Google-run and not independently audited.*

Two patterns stand out. Flash leads its price tier on web development, enterprise automation, document comprehension, long-context recall, and video, often by wide margins against Claude Sonnet 5. It trails GPT-5.6 Terra, which costs several times more per token, on the hardest agentic coding, and it sits a hair behind 3.6 Flash on CharXiv chart reasoning, the one place its predecessor still edges it. The takeaway is that 3.7 Flash is built to win the everyday coding, agent, and document work most applications actually run, and cedes only the most demanding coding to pricier models.

### 2. Independent benchmarks

For a neutral cross-check, Artificial Analysis evaluates every model on the same harness, which removes the vendor-advantage problem in the table above. On its composite index, Gemini 3.7 Flash scores well and stands out most on speed and price. The table compares it with the two rivals from the vendor table, plus 3.6 Flash, on the independent index.

| **Metric (Artificial Analysis)** | **3.7 Flash** | **3.6 Flash** | **Claude Sonnet 5** | **GPT-5.6 Terra** | 
|---|---|---|---|---|
| Intelligence Index | 56 | 52 | 55 | 57 | 

*Table 4: Independent Intelligence Index from Artificial Analysis, same-harness across models, August 2026.*

The independent read matches the vendor story on direction, Flash is strong for its class, while adding what a vendor table cannot. Its Intelligence Index of 56 sits well above the median of 35 for comparable models and just behind GPT-5.6 Terra at 57, despite costing a fraction as much. It also ranks first of 181 models Artificial Analysis tracks for output speed, at 389.5 tokens per second, and its 90% cache discount sharply cuts the cost of workloads that replay a long context. For the full benchmark set, including every sub-score and thinking level, see our Gemini 3.7 Flash benchmarks guide, and Google's complete table is on the DeepMind model card.

## How the thinking configurations work

Gemini 3.7 Flash lets you set a thinking level that trades quality against cost and latency, and that setting explains a lot of the score variation you will see. The model supports three levels: low, medium, and high. A higher level spends more tokens reasoning through a problem before answering, which lifts accuracy on hard tasks but raises both cost and response time.

This is why nearly every benchmark score for the model carries a "(high)" label, including the Intelligence Index figure of 56. Those numbers reflect the top thinking configuration, not the default you might run in production. If you route real traffic at a lower thinking level to save cost or latency, expect the model to trade some of that measured accuracy for speed. The practical takeaway is to test at the thinking level you actually plan to ship, not the one that produces the best headline number.

## Pricing at a glance

Gemini 3.7 Flash costs $0.75 per 1M input tokens and $3.75 per 1M output tokens, an introductory rate that doubles at the start of 2027. The output rate includes thinking tokens, so reasoning-heavy work bills at the output price for the tokens the model generates while working.

| **Token type** | **Introductory (through Dec 31, 2026)** | **Standard (from Jan 1, 2027)** | 
|---|---|---|
| Input | $0.75 / 1M | $1.50 / 1M | 
| Output | $3.75 / 1M | $7.50 / 1M | 
| Cached input | $0.075 / 1M | $0.15 / 1M | 

*Table 5: Gemini 3.7 Flash API pricing, paid tier. Pricing as of August 2026, sourced from Google.*

Budget on the standard rate, not the introductory one, if your usage will run into 2027. The introductory window gives you several months to test the model cheaply, but a workload costing $2,000 a month now would cost $4,000 at the standard rate on the same volume. Context caching, which drops reused input to a fraction of the base rate, is the largest lever for agent workloads that replay a long context on every step. The full cost breakdown is in our pricing guide.

## Safety and responsible use

Gemini 3.7 Flash ships with updated safeguards in higher-risk domains under Google's Frontier Safety framework. Google states the model includes strengthened protections against misuse in chemical, biological, radiological, and nuclear (CBRN) and cyber-offense contexts, while preserving legitimate use cases. Google reports that 3.7 Flash performs similarly to 3.6 Flash on safety and tone, with low rates of unjustified refusals. For teams building in regulated or sensitive areas, the model card is the primary reference for the specific evaluations and mitigations.

## When Gemini 3.7 Flash isn't the right choice

Gemini 3.7 Flash is the low-cost default, but three situations make a rival the better pick, and each points to a different kind of model. The trigger is rarely price, since Flash already wins there; it is a capability or constraint Flash does not cover.

### 1. Your workload hinges on one hard reasoning problem

If your workload lives or dies on a single difficult, novel-logic task rather than high-volume throughput, a frontier model like Claude Opus 5 or GPT-5.6 Terra earns its higher token cost by finishing work Flash cannot. Flash is tuned for speed and breadth across many everyday tasks, not for grinding out the single hardest proof, novel algorithm, or deep multi-step deduction where a frontier model's extra reasoning headroom is the difference between a right answer and a plausible wrong one.

### 2. You need open weights or self-hosting

When data control, air-gapping, or the right to keep running a model after its vendor moves on matters, an open-weight model such as Kimi K3 or DeepSeek V4 is the only fit, since Flash is closed and API-only. This is common in regulated industries, on-premise deployments, and any setup where sending data to a third-party API is off the table, or where you need to guarantee a model stays available on your own terms regardless of vendor roadmap.

### 3. You are locked into another ecosystem

If your stack is already built around OpenAI or Anthropic tooling, staying in that ecosystem can outweigh a switch, even when Flash scores well on paper. The cost of re-plumbing prompts, evals, and integrations, plus retraining a team on new tooling, can erase Flash's price advantage, so the switch only pays off when the volume is large enough to justify the migration.

Weigh total cost per finished task, not the token rate, and pilot any switch on your own workload before committing. Our Gemini 3.7 Flash alternatives guide compares the full shortlist on price, capability, and fit.

## How Gemini 3.7 Flash compares to other models

Gemini 3.7 Flash wins on cost and speed and trades reasoning depth to frontier models, so the right comparison depends on what you are weighing it against. The table below places it next to the three models builders most often consider alongside it, using the independent Artificial Analysis Intelligence Index and published list pricing so the columns are comparable.

| **Model** | **AA Intelligence Index** | **Input $/1M** | **Output $/1M** | **Best for** | 
|---|---|---|---|---|
| Gemini 3.7 Flash | 56 | $0.75 | $3.75 | Low-cost coding and agents | 
| Gemini 3.1 Pro Preview | 48 | $2.00 | $12.00 | More reasoning in the Google stack | 
| Claude Opus 5 | 63 | $5.00 | $25.00 | Frontier reasoning and long-horizon work | 
| GPT-5.6 Terra | 57 | $2.00 | $12.00 | Hardest agentic coding | 

*Table 6: Gemini 3.7 Flash against common alternatives. Intelligence Index from Artificial Analysis (high or max-effort configuration); pricing as of August 2026, Gemini 3.7 Flash at its introductory rate.*

### 1. Against Google's Pro tier, Flash wins on independent intelligence

Against Google's own Pro tier, Gemini 3.7 Flash is the surprising winner. On the independent Intelligence Index it scores 56 against 48 for the older Gemini 3.1 Pro Preview, and it is cheaper and faster, which upends the usual assumption that a Pro model beats a Flash one. The reason is timing: Flash is six months newer, so a fresh, efficiency-focused release overtook an aging flagship on the composite. Pro Preview still leads on some vendor-reported reasoning benchmarks, so it is not obsolete for hard, novel-logic tasks. Our 3.7 Flash vs 3.1 Pro comparison works through where each one wins.

### 2. Against a frontier model, the two split the work

Against a frontier reasoning model, the picture flips to a split decision. Our Gemini 3.7 Flash vs Opus 5 analysis lands on a 4-4 tie: Flash takes cost per completed task, output speed, multimodal input, and agentic document work, while Claude Opus 5 takes frontier reasoning, complex coding, aggregate intelligence at 63 on the index, and long-horizon recovery. Opus 5 costs roughly six to seven times more per token, so the split is less about which model is better and more about which kind of work you are routing to it.

### 3. Against a peer workhorse, price is the deciding factor

Against a peer workhorse, the gap narrows to a few points. GPT-5.6 Terra edges Flash on the Intelligence Index, 57 to 56, and leads on the hardest agentic coding, but it costs several times more per token and carries no scheduled price increase, which makes the choice a trade between Flash's price and Terra's ceiling on the toughest tasks.

Beyond these three, self-hosting and open-weight options widen the field further, as the section above lays out.

## How to access and start building

Gemini 3.7 Flash is available through Google's developer surfaces and through platforms that bundle model access. You can call it directly via the Gemini API in Google AI Studio using the model ID gemini-3.7-flash, build agent-first workflows in Google Antigravity, or access it through Gemini Enterprise for larger deployments. Individuals reach it through Gemini Spark, Google's personal agent, on Google AI Pro and Ultra plans.

## From choosing a model to shipping the product

Gemini 3.7 Flash is Google's most capable workhorse model to date, delivering strong coding, agent, and document performance at a low, if temporary, price. It refines 3.6 Flash rather than replacing the architecture, wins its tier on independent intelligence, and cedes only the hardest autonomous work to frontier models. For the majority of production workloads, it is the sensible default, provided you budget for the 2027 price increase and test at the thinking level you plan to ship.

Choosing the model is the first decision; turning it into a running application is the larger one. Emergent lets you build production-grade, full-stack apps on leading Gemini, Claude, and GPT models through a single Universal LLM Key, with unified billing and no API plumbing to manage. You describe what you want, pick the model that fits the project, and deploy real software you own.
