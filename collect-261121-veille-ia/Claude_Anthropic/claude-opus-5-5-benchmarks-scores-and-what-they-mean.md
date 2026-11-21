---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-benchmarks-scores-and-what-they-mean
title: "Claude Opus 5.5 Benchmarks: Scores and What They Mean"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "anthropic", "claude", "scores", "agentic", "benchmarks", "benchmark", "model"]
source: https://emergent.sh/learn/claude-opus-5-5-benchmarks
source_anchor: ""
source_lines: [1, 201]
section: "Claude_Anthropic"
---
#### TL;DR

- Claude Opus 5.5 launched on September 22, 2026 as the first model in Anthropic's Claude 5.5 family.
- It takes the top spot on the independent Artificial Analysis Intelligence Index at 58 (max effort), the highest score measured there by several points.
- On Anthropic's own table it leads in agentic coding, knowledge work, and multidisciplinary reasoning, scoring 66.4% on Terminal-Bench 4.0 and 1846 Elo on GDPval-AA v2.1.
- GPT-6 Astra still edges it on business-workflow automation and agentic science.
- Pricing is $4 input and $20 output per 1M tokens, 20% below Opus 5, with cache reads cut 60% to $0.20. Anthropic says it costs about 40% less to run than Opus 5.


The Opus 5.5 benchmarks are strong, but the scores alone will not tell you whether the model is right for what you are building. Anthropic reports category leads in coding and knowledge work. Independent testing at Artificial Analysis largely agrees, then trims a few of the margins. The gap between those two views is the part worth understanding.

This guide lays out every published benchmark, separates vendor-reported numbers from independently verified ones, breaks down the real cost, and translates all of it into plain guidance for people who build with the model rather than train it. You can run Opus 5.5 today on Emergent, so the practical question is simple: what do these numbers actually buy you?

## Where Opus 5.5 fits in Anthropic's lineup

Opus 5.5 is the first model in Anthropic's Claude 5.5 family, released on September 22, 2026. It replaces Opus 5 as the default general-purpose model and sits below the larger Fable 5.1 and the access-gated Mythos 5.1, which list at $10/$50. Anthropic has said Sonnet 5.5 and Haiku 5.5 will follow in the weeks ahead, so the value picture across Claude's lineup may shift again soon.

## Claude Opus 5.5 benchmarks at a glance

Anthropic published nine benchmarks at launch, comparing Opus 5.5 against its own Fable 5.1 and Opus 5, plus OpenAI's GPT-6 Astra and GPT-5.6 Sol. Opus 5.5 leads six of them. It trails GPT-6 Astra on AutomationBench and Terminal-Bench-Science.

*Table 1: Claude Opus 5.5 benchmark scores, vendor-reported by Anthropic. Source: Anthropic, "Introducing Claude Opus 5.5."*

| **Benchmark** | **Opus 5.5** | **Fable 5.1** | **Opus 5** | **GPT-6 Astra** | **GPT-5.6 Sol** | 
|---|---|---|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 66.4% | 55.8% | 52.3% | 57.9% | 37.3% | 
| FrontierCode v1.1 Main (agentic coding) | 54.4% | 50.3% | 48.0% | 53.3% | 47.5% | 
| CursorBench 4.0 (agentic coding) | 57.8% | 51.8% | 46.6% | N/A | 41.7% | 
| GDPval-AA v2.1 (knowledge work, Elo) | 1846 | 1735 | 1708 | 1542 | 1588 | 
| AutomationBench (business workflows) | 40.0% | 31.4% | 26.9% | 41.4% | 28.8% | 
| Humanity's Last Exam (with tools) | 67.7% | 65.6% | 63.6% | 57.2% | N/A | 
| Terminal-Bench-Science 0.1 (agentic science) | 58.7% | 52.6% | 29.0% | 64.6% | 22.4% | 
| OSWorld 2.0 (computer use, partial credit) | 81.8% | 80.7% | 74.0% | N/A | N/A | 
| Chartography (visual reasoning, with tools) | 89.0% | 88.4% | 83.4% | N/A | N/A | 

Two things to keep in mind when reading Table 1. Opus 5.5 results use adaptive thinking at max effort, except Terminal-Bench 4.0, which runs at xhigh effort. The GPT-6 Astra and GPT-5.6 Sol figures are as reported by OpenAI, not reproduced by Anthropic. N/A means Anthropic did not publish a comparable score. Anthropic also adds a caveat most vendors skip: at this level of capability, it says benchmark margins have become a less reliable guide to real-world differences, and the gap between Opus 5.5 and Fable 5.1 is narrower than the scores suggest.

## What each Opus 5.5 benchmark measures

Scores mean more once you know what the test is checking. Here is what each benchmark evaluates and how to read the Opus 5.5 result.

### 1. Terminal-Bench 4.0

This measures how well an agent completes multi-step engineering tasks inside a real command line, from start to finish. It is Opus 5.5's widest lead on Anthropic's table, 8.5 points over GPT-6 Astra. Anthropic reports a standard error of 2.6 points, so the margin over Astra sits outside the noise on its own harness.

### 2. FrontierCode v1.1

FrontierCode tests whether a model's code changes would be accepted and merged into real production projects. Opus 5.5 leads at 54.4%, though only 1.1 points ahead of GPT-6 Astra, which is inside the range these tests tend to wobble by.

### 3. CursorBench 4.0

Built from real coding sessions in the Cursor editor, this checks ambiguous, multi-file edits. Opus 5.5 scores 57.8%, six points clear of Fable 5.1. OpenAI did not publish a GPT-6 Astra score here.

### 4. GDPval-AA v2.1

This Artificial Analysis benchmark grades professional deliverables such as memos, spreadsheets, and slide decks across 44 occupations, scored on an Elo scale. Opus 5.5 posts 1846, over 300 Elo ahead of GPT-6 Astra. It is one of the clearest signals for non-coding work.

### 5. AutomationBench

Designed by Zapier, this measures whether an agent can run multi-step workflows across connected SaaS apps without breaking the logic. GPT-6 Astra wins by 1.4 points. Zapier ran Opus 5.5 without fallback models, so every safeguard interruption counted as a failure, which Anthropic says lowered the score below what the model reaches in practice.

### 6. Humanity's Last Exam

HLE poses expert-level questions across many academic fields, designed to resist lookup shortcuts. With tools enabled, Opus 5.5 leads at 67.7%, about 10 points ahead of GPT-6 Astra on Anthropic's harness.

### 7. Terminal-Bench-Science 0.1

This tests agentic scientific research: forming a hypothesis, writing analysis code, running it, and reaching a sound conclusion. GPT-6 Astra leads at 64.6%. The result still marks the biggest generation jump on the table, since Opus 5.5 roughly doubles Opus 5's 29.0%.

### 8. OSWorld 2.0 and Chartography

OSWorld 2.0 checks computer use: operating desktop apps through screenshots, mouse, and keyboard. Opus 5.5 scores 81.8% with partial credit, so the strict completion rate sits lower. Chartography checks how well the model reads charts and figures, where Opus 5.5 posts 89.0%. Both point to a real gain in visual and interface work.

## Vendor-reported vs independently verified scores

Here is the part most launch coverage glosses over. Anthropic runs its own harness, and Artificial Analysis runs a different one. The two do not always land on the same number, and the differences are worth seeing side by side.

On the Artificial Analysis Intelligence Index, Opus 5.5 takes first place at 58 at max effort, the highest the group has recorded by several points. It leads six of the ten evaluations in that index, including Humanity's Last Exam at 61.4% and SciCode at 66.9%, and reaches 1822 Elo on the private AA-Briefcase knowledge-work test, 143 ahead of Fable 5.1.

*Table 2: Claude Opus 5.5 on the Artificial Analysis Intelligence Index, independently verified at max effort. Source: Artificial Analysis.*

| **Metric (Artificial Analysis)** | **Opus 5.5** | **GPT-6 Astra** | **Fable 5.1** | **Opus 5** | **GPT-5.6 Sol** | 
|---|---|---|---|---|---|
| Intelligence Index | 58 | 53 | 53 | 51 | 47 | 
| Humanity's Last Exam | 61.4% | 54.7% | 59.1% | N/A | N/A | 
| SciCode | 66.9% | 56.5% | 63.1% | N/A | N/A | 

Now the divergence. Two benchmarks appear on both Anthropic's table and Artificial Analysis's, and the independent numbers come in lower.

*Table 3: Where the vendor and independent numbers differ. Sources: Anthropic and Artificial Analysis.*

| **Benchmark** | **Anthropic-reported** | **Artificial Analysis** | **Gap** | 
|---|---|---|---|
| Terminal-Bench 4.0 | 66.4% (xhigh) | 59.6% (max) | 6.8 points | 
| Humanity's Last Exam | 67.7% (with tools) | 61.4% | 6.3 points | 

Neither gap means a number is wrong. Harnesses, tool access, effort settings, trial counts, and how safeguard fallbacks are handled all move scores by several points. On the independent Terminal-Bench run, that 6.8-point trim is enough to pull Opus 5.5 level with GPT-6 Astra rather than clearly ahead. The takeaway is not that Anthropic inflated anything. It is that vendor scores are best read as an upper bound, and the independent picture still puts Opus 5.5 at the top of the frontier, just by a smaller margin than the launch table shows.

## Opus 5.5 pricing and the real cost story

Opus 5.5 is cheaper than Opus 5 on every line, and the cache-read cut is the one that matters most for agents that re-read a large context on every turn.

*Table 4: Claude Opus 5.5 pricing per 1M tokens, as of September 2026. Prices can change, so confirm against Anthropic's live pricing before publishing decisions. Source: Anthropic.*

| **Per 1M tokens** | **Opus 5.5** | **Opus 5** | **Change** | 
|---|---|---|---|
| Input | $4.00 | $5.00 | −20% | 
| Output | $20.00 | $25.00 | −20% | 
| Cache reads | $0.20 | $0.50 | −60% | 
| Cache writes (5-minute) | $5.00 | $6.25 | −20% | 
| Fast mode input / output | $8.00 / $40.00 | N/A | up to 2.5x speed | 

Anthropic's headline claim is that Opus 5.5 costs about 40% less to run than Opus 5 on typical workloads. That figure combines the 20% list-price cut with the model using fewer tokens and fewer steps to finish a task. Your actual savings depend on your workload, so treat 40% as a strong case rather than a guarantee.

There is one catch that shapes the whole cost story: effort level. Opus 5.5 has five settings, from low to max, and it defaults to medium. At medium effort it is very efficient, and Anthropic reports it beating GPT-6 Astra's best FrontierCode score at roughly a fifth of the cost per task. At max effort the math flips.

Artificial Analysis measured Opus 5.5 using about 119,000 output tokens per task, against about 27,000 for GPT-6 Astra. So while Opus 5.5 is cheaper per token, at max effort a rival that writes far fewer tokens can finish the same job for less. The practical rule: run medium by default, and raise the effort only for tasks that clearly need it.

To make the cost concrete, here is a single agentic coding session priced on both Opus models at identical token counts. Assume 200,000 uncached input tokens, 1,800,000 cache-read tokens, and 150,000 output tokens, a cache-heavy pattern typical of long agent runs. Cache-write fees are left out to keep the comparison clean.

*Table 5: Worked cost for one heavy agent session, priced at Opus 5.5 and Opus 5 rates. Calculated from Anthropic list pricing as of September 2026.*

| **Cost component** | **Opus 5.5** | **Opus 5** | 
|---|---|---|
| Uncached input (200K) | $0.80 | $1.00 | 
| Cache reads (1.8M) | $0.36 | $0.90 | 
| Output (150K) | $3.00 | $3.75 | 
| Session total | $4.16 | $5.65 | 

On identical token counts, Opus 5.5 comes in about 26% cheaper. In real use it also tends to finish tasks in fewer tokens and steps, which is how Anthropic reaches its roughly 40% figure. Your result depends on how cache-heavy your workload is and which effort level you run.

## How Opus 5.5 compares to Opus 5, Fable 5.1, and GPT-6 Astra

The table below summarizes the trade-offs at a glance. The paragraphs that follow explain each matchup.

*Table 6: How Claude Opus 5.5 compares with Opus 5, Fable 5.1, and GPT-6 Astra. Claude pricing is per Anthropic; the GPT-6 Astra price is OpenAI's list price. Sources: Anthropic and Artificial Analysis.*

| **Model** | **Price per 1M (input / output)** | **Benchmark strength** | **Position vs Opus 5.5** | **Best for** | 
|---|---|---|---|---|
| Claude Opus 5.5 | $4 / $20 | Agentic coding and knowledge work | Reference model | Coding agents and knowledge-work apps at a sensible cost | 
| Claude Opus 5 | $5 / $25 | Prior general-purpose flagship | Beaten on every published benchmark, and over 30% slower | Legacy deployments not yet migrated | 
| Claude Fable 5.1 | $10 / $50 | Long-horizon reasoning | Loses on the launch table, though Anthropic says the real-world gap is narrower | Demanding, open-ended reasoning work | 
| GPT-6 Astra | $10 / $50 | Agentic science and workflow automation | Leads on Terminal-Bench-Science and AutomationBench, and level on the independent Terminal-Bench run | Scientific agents and short-output workloads | 

Against **Opus 5**, this is a clean upgrade. Opus 5.5 scores higher on every benchmark Anthropic published, costs 20% less on tokens, and generates output more than 30% faster. Early testers cited large efficiency gains, including a 200,000-line codebase audit that finished in under three hours where Opus 5 took over 20.

Against **Fable 5.1**, Opus 5.5 wins on the launch table at 40% of the price, since Fable lists at $10/$50. Anthropic's own caveat matters here: it says the real-world gap is narrower than the benchmarks imply, which suggests Fable still holds an edge on some hard, open-ended work. For most everyday builds, Opus 5.5 makes Fable hard to justify as the default.

Against **GPT-6 Astra**, the answer splits by task. Opus 5.5 leads on agentic coding, knowledge work, and Humanity's Last Exam, though the independent Terminal-Bench run has the two level. Astra leads on agentic science and business-workflow automation. Opus 5.5 is 60% cheaper per token, but Astra writes far fewer tokens at high effort. Choose Opus 5.5 for coding agents and knowledge-work deliverables, and reach for Astra when short outputs or scientific agents are the priority.

## Specs and what changed from Opus 5

The spec sheet backs up the benchmark story, with a wide context window and always-on reasoning.

*Table 7: Claude Opus 5.5 specifications. Source: Claude Platform documentation.*

| **Spec** | **Claude Opus 5.5** | 
|---|---|
| API model ID | claude-opus-5-5 | 
| Context window | 1,000,000 tokens | 
| Max output | 128,000 tokens | 
| Reliable knowledge cutoff | June 2026 | 
| Thinking | Adaptive, always on | 
| Effort levels | low, medium (default), high, xhigh, max | 
| Input / output | Text and image in, text out, vision, tool use | 
| Availability | Claude apps, Claude Code, Cowork, Claude Platform API, AWS, Google Cloud, Microsoft Azure | 

The most-cited qualitative change is communication. Anthropic says Opus 5.5 puts the key point first, uses less jargon, and follows the writing rules it is given, which was a common complaint about Opus 5. Thinking can no longer be switched off, and the default effort dropped from high on Opus 5 to medium here, so teams migrating existing code should set effort explicitly and re-test.

## API changes for teams migrating from Opus 5

If you or your developers already run Opus 5 in production, a few changes affect existing integrations. Anthropic's migration guide covers them in full, and these are the ones to know:

- Thinking is always on: the option to disable thinking or set a thinking budget is rejected, so use the effort setting from low to max to control depth and cost.
- Forced tool use is retired: a tool_choice of "any" or a named tool now returns a 400 error, so switch to "auto" with prompt instructions or structured outputs.
- Preserved thinking needs append-only conversations: editing earlier turns, the system prompt, or the tool list mid-session can cause later reasoning to be dropped or rejected.
- A new refusal signal exists: responses can return a refusal stop reason, so add fallback or retry handling for it.

None of this changes what the model can do, but skipping the re-test is the most common way a migration goes wrong. Set effort explicitly and re-baseline cost before you switch.

## Safety and alignment highlights

Anthropic reports that Opus 5.5 scores better than any recent Claude model on its automated behavioral audit, a suite of nearly 2,000 simulated scenarios, and calls it the strongest-performing model it has tested there. In a new test for crossing containment boundaries, it attempted to circumvent boundaries about 85% less often than Opus 5. On prompt injection, it matches or beats Opus 5 across coding, tool use, and browsing, and on a benchmark run by security firm Gray Swan it ties Fable 5.1 for the lowest injection success rate recorded.

The model ships with safeguards in the same class as Fable 5.1 for cybersecurity and biology. When those safeguards trigger, cybersecurity tasks fall back to Opus 4.8 and biology tasks to Opus 5. If you deploy agents that read untrusted content, sandbox them and limit their credentials regardless of the score.

## What the Opus 5.5 benchmarks mean if you're building an app

Most benchmark coverage is written for engineers. If you are a founder or operator describing a product rather than writing the code, here is how the numbers translate.

The coding scores (Terminal-Bench, FrontierCode, CursorBench) predict how well an agent can build and change real features across multiple files without breaking things. Higher scores mean fewer stalls and less rework when you ask for a new screen, a fix, or a refactor. The knowledge-work score (GDPval-AA) predicts quality on the document-shaped work many apps actually produce: reports, summaries, and analysis. The computer-use and chart scores (OSWorld, Chartography) matter if your product reads dashboards or drives an interface on the user's behalf.

The cost story is the practical lever. Because Opus 5.5 defaults to medium effort and is efficient there, you get frontier-level output without paying for max-effort token counts you rarely need. For a builder, that means the strong numbers in this guide are reachable at a sensible cost, not just in a lab setting.

## Build with Opus 5.5 on Emergent

Claude Opus 5.5 is the current front-runner: first on the independent Artificial Analysis Intelligence Index, a clear leader in agentic coding and knowledge work, and cheaper to run than the model it replaces. The honest caveats hold too. Independent scores sit a few points under Anthropic's, GPT-6 Astra still wins on science and automation, and max effort can erase the per-token savings. Read the numbers as an upper bound, run medium effort by default, and measure cost per task on your own workload.

The better news for builders is that you do not have to wire up an API to use any of this. Opus 5.5 is available on Emergent, where you can build production-grade full-stack apps by describing what you want, with Claude, GPT, and Gemini all reachable through a single Universal LLM Key.

Start Building on Emergent and put Opus 5.5 to work on something real.
