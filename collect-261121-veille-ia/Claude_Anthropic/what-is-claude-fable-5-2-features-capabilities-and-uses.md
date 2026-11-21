---
id: collect-261121-veille-ia/claude-anthropic/what-is-claude-fable-5-2-features-capabilities-and-uses
title: "What Is Claude Fable 5.2? Features, Capabilities, and Uses"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "claude", "model", "anthropic", "agent", "features", "reasoning", "task"]
source: https://emergent.sh/learn/what-is-claude-fable-5-2
source_anchor: ""
source_lines: [1, 268]
section: "Claude_Anthropic"
---
#### TL;DR

- Claude Fable 5.2 is Anthropic’s top AI model, built for hard reasoning, coding, and agent tasks that run for hours.
- Its features include adaptive thinking, text and image input, and a 1M-token context window.
- It costs $8 per million input tokens and $40 per million output tokens.
- It scores 65 on the Artificial Analysis Intelligence Index, up from 50 for Fable 5.1.
- You can use it in Claude apps, the Claude API, AWS, Google Cloud, and Microsoft Foundry.


Claude Fable 5.2 comes down to a trade-off: it scores 65 on the Artificial Analysis Intelligence Index, but costs about 2.4 times as much per task as Fable 5.1.

Most AI models handle a short question well. They lose the thread when a task runs for hours, spans thousands of files, or needs many steps in a row.

So what is Claude Fable 5.2? It is Anthropic’s top model, built for deep reasoning and long agent work. All Artificial Analysis figures in this guide use Intelligence Index v5.0.1 and were checked on November 10, 2026.

Teams that build with an AI agent builder can use these facts to match the right model to each task.

## What Is Claude Fable 5.2?

Claude Fable 5.2 is a large language model from Anthropic. It reads text and images and writes text. Anthropic released it on November 9, 2026.

Anthropic designed it for its most daunting technical challenges. The Claude Platform docs name three typical jobs. They are hours-long agent sessions, multistep research, and analysis that ends in a finished document, spreadsheet, or deck.

Fable 5.2 and Mythos 5.2 are the same underlying model. Fable carries strong cybersecurity and biology safeguards, so Anthropic offers it to every user. Mythos drops those limits and is available only to verified organizations.

Fable 5.2 is Anthropic’s most advanced Claude model, ahead of Opus 5.5, Sonnet 5.5, and Haiku 5.5. Anthropic recommends starting with Opus 5.5 for most tasks. Use Fable when you need more advanced capabilities, or choose Sonnet or Haiku for simpler tasks.

## Key Features of Claude Fable 5.2

The main Claude Fable 5.2 features are reasoning, coding, long-running agent work, vision, and adaptive thinking. Each one gets its own section below.

### Advanced Reasoning and Problem-Solving

Fable 5.2 handles problems that need many connected steps. Anthropic says it produces strong results even from vague requests. It also shows sound engineering judgment.

Cost matters here too. On Humanity’s Last Exam, a test of expert-level knowledge, low effort matches Opus 5.5's top score for less than one-tenth of the cost per task. For what each test measures and how the scores compare, see our Fable 5.2 benchmarks guide.

Testers point to honest reasoning. An engineer at Cognition said the model admitted a past estimate was wrong. It refused to give a new number until it measured the change.

Box tested the model on loan application files. It noticed that the salary deposits did not match the bank statement balance. The underwriter then knew what to check.

### Coding and Software Development

Anthropic says Claude now writes over 90% of the changes merged into its core codebase, with human guidance. Fable 5.2 builds on that work. Anthropic used it to fix stubborn bugs in minutes and to speed up its services.

Vercel said projects that dragged on for weeks with other models finished in a few days. Human reviewers had less to say about the code.

A Shopify engineer said the model feels like another staff-level engineer. Within a few turns, a loose idea becomes a task to hand off.

To build that kind of handoff yourself, a coding agent builder lets you set the steps and where a person reviews.

### Long-Running Agentic Tasks

Fable 5.2 keeps working on one job for hours or days without losing the plan. Three examples show the range.

- **An operating system from scratch.** An Anthropic engineer asked it to design a prototype from an empty folder. It worked around the clock for eight days. The result boots on a laptop and includes drivers, file storage, windows, a login screen, and an installer.
- **A security audit.** An Uber engineer ran a 10- to 12-hour audit with about 170 reviewer agents. The run returned a write-up, fix pull requests, and a policy proposal.
- **A language migration.** Agents running an earlier Fable 5.2 build moved a production service from Python to Rust. The service now starts about 26 times faster.

The Uber engineer said the model works until the job is done. It does not stop at a plausible checkpoint.

### Vision and Multimodal Capabilities

Fable 5.2 reads images and text. It writes text only, so it cannot create images.

Vision supports real work. Unity said the model builds higher-quality 3D scenes from scratch and rebuilds an experience from a visual reference. It keeps improving the scene instead of getting stuck.

Computer use adds another layer. Fable 5.2 works through a screen with the computer use toolset. It replaces the older computer_20251124 tool on every platform except Amazon Bedrock.

### Adaptive Thinking

Adaptive thinking lets Fable 5.2 decide how long to think before it answers. It stays on, and you cannot switch it off. You control depth with the effort setting instead.

The API offers five effort levels:

- **Low:** Best for easy turns.
- **Medium:** The default in Claude apps and Claude Code.
- **High:** The API default. Anthropic says most of the capability gain already shows up here.
- **Xhigh:** Built for the hardest long coding and agent work. Use it when you have measured a gain.
- **Max:** Meant for testing the model’s ceiling, or rare tasks where time and tokens do not matter.

Artificial Analysis ran five of its Intelligence Index tests at all five effort settings. Scores are in percent, and higher is better.

| **Benchmark** | **What it tests** | **Max** | **Xhigh** | **High** | **Medium** | **Low** | 
|---|---|---|---|---|---|---|
| AutomationBench-AA | Agentic SaaS workflows | 76% | 74% | 71% | 69% | 68% | 
| FrontierCode 2.0 | Agentic software engineering in open-source repositories | 49% | 48% | 49% | 41% | 38% | 
| Terminal-Bench 4.0 | Agentic coding and terminal use | 60% | 62% | 60% | 55% | 55% | 
| HLE-Diamond | Reasoning and knowledge | 80% | 76% | 74% | 66% | 61% | 
| CritPt v2 (Hard Subset) | Physics reasoning | 77% | 74% | 73% | 62% | 60% | 

Data checked on November 10, 2026.

The scores on knowledge and science tests rise more with higher effort settings. HLE-Diamond scores 19 points higher at max effort than at low effort, while CritPt improves by 17 points. AutomationBench-AA has a smaller gain of 8 points.

On FrontierCode 2.0, high and max effort both score 49%. Terminal-Bench 4.0 does best at xhigh effort, reaching 62%, which is 2 points higher than max.

These results back Anthropic’s advice to start at high effort. Step up only on tasks where your own tests show a gain.

**Also Read:** Claude Fable 5.1 high vs low effort for how effort changed cost and results on the earlier model.

## What Makes Claude Fable 5.2 Different?

Fable 5.2 stands out for how much work it finishes on its own. Three traits set it apart from earlier Fable models.

### Handling Complex, Multi-Step Tasks

Fable 5.2 can carry a task from the first step to a finished file. Anthropic reports that testers found it handles vaguer, more data-heavy tasks than earlier models. It also makes better judgment calls.

Fundamental Research Labs gave it a hard spreadsheet job. The model had to pull a company’s financials from a 10-K into a forecasting model. The template did not say which metrics to report and which to calculate.

The model read the cell formatting to infer the author’s intent. It also caught that the regional numbers did not sum to the reported total. It got every cell right and scored 91%, compared with 86% for the next-best model.

Harvey tested an I-9 compliance review. Fable 5.2 named the employees who needed attention first. It explained how to fix each issue and who should act.

### Working With Large Codebases

The Claude Fable 5.2 context window holds 1M tokens. Artificial Analysis puts that at about 1,500 A4 pages of 12-point Arial text. A large codebase or a long document set fits in one request.

Datadog said the model works inside new, unfamiliar systems. It acts like an incident commander, sending faster models out to gather quick signals.

iGent asked it to build a hypervisor, the software that walls off virtual computers on one machine. In a single session, it built one and backed its core with more than 3,500 machine-checked proofs.

Snowflake called it the strongest model it has measured on data-eng-bench, an open-source set of 103 data-engineering tasks. It solved tasks consistently across repeated runs.

### Coordinating Multiple AI Agents

Fable 5.2 can lead a team of agents. In Anthropic’s math test, a team of 1,000 Fable 5.2 agents formalized the proof of Fermat’s Last Theorem in 30 hours. Anthropic’s first attempt took 11 days.

Opus 5.5 finished only 26% of that job in 18 hours with the same setup. The Fable team started with minimal prompts and built its own coordination approach. The operating system project used more than 1,000 subagents.

Fable 5.2 also works as an advisor or an orchestrator. As an advisor, it helps Haiku, Sonnet, or Opus models solve harder problems. As an orchestrator, it splits the work and hands simpler tasks to smaller models. For how a lead agent and its helpers fit together, see our guide on how to build AI agents from scratch.

Jump Trading saw the model play every role in its research setup. It acted as code-writer, coordinator, planner, and job-submitter. Earlier models needed hours of manual role balancing.

## What Can You Use Claude Fable 5.2 For?

Fable 5.2 is useful for software development, research, science, and workflow automation when tasks require deeper reasoning. The following sections explain where you can use it.

### Building and Debugging Software

Give it jobs that other models leave half done. Large migrations, performance tuning, and bug hunts all fit. Hand it the repo, the goal, and the tests that must pass. If the goal is a product customers will log into, an AI SaaS builder can turn the same description into a working app with accounts and a database

Oracle tested how well the models could write SQL queries using data on country debt, repayment history, and regional benchmarks. Fable 5.2 produced the correct results. Two other models ran the queries but calculated the country totals incorrectly, returning figures that were five to eight times higher than the actual values.

### Research and Data Analysis

Fable 5.2 suits research that needs judgment, not only speed. IMC said it reached new best scores on three of four quant research problems. It found a second, new angle on top of the insight it shared with Fable 5.1.

Jump Trading measured 10% lower error than the next best model on an internal research benchmark. Balyasny Asset Management saw it trace an answer to the right sources and avoid overstating the evidence. It also flagged that the question’s framing could mislead.

XTX Markets reported strong gains over Fable 5.1, especially with several coordinating agents.

### Scientific and Technical Work

Anthropic says Fable 5.2 solved two open problems in black hole physics. One was a 2003 calculation too laborious to finish by hand. The model completed it and found that black holes also form abruptly in very small universes.

Discovered Materials used it to speed up a physics code called Quantum ESPRESSO. The model cut 13 benchmark cases from 3.3 hours to under four minutes. That is about 36 times faster on average.

It also designed hardware. From a one-paragraph prompt, it produced three working devices: a home voice-assistant hub, a USB speakerphone, and a handheld virtual pet. Each circuit board was ready for manufacturing in under a day.

Fable blocks high-risk biology and cybersecurity work. Verified teams in those fields apply for Mythos 5.2 instead.

### Automating Complex Workflows

Fable 5.2 fits workflows with many steps and many tools. It can end a task with a finished document, spreadsheet, or deck. AutomationBench-AA, which tests agentic SaaS workflows, measures this kind of work.

Two beta features help long sessions. Compaction on demand lets you choose when to replace older turns with a summary. Mid-conversation tool changes let you add or remove tools without editing the tool list.

## Claude Fable 5.2 Specifications

Fable 5.2 reads up to 1M tokens and writes up to 128K tokens in one request. The table lists the core specs from the Claude Platform docs.

| **Spec** | **Detail** | 
|---|---|
| Model ID (Claude API) | claude-fable-5-2 | 
| Model ID (Amazon Bedrock) | anthropic.claude-fable-5-2 | 
| Release date | November 9, 2026 | 
| Context window | 1M tokens | 
| Max output | 128K tokens | 
| Input and output | Text and images in, text out | 
| Thinking | Adaptive, always on | 
| Default effort (API) | high | 
| Comparative latency | Moderate | 
| Knowledge cutoff | June 2026 | 
| Retirement | Not before November 9, 2027 | 

### Claude Fable 5.2 Pricing

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens. That is twice the per-token price of Opus 5.5, which charges $4 and $20.

Source: Claude Platform docs. Prices checked on November 10, 2026.

Fast mode speeds up token generation by up to 2.5 times, at double the price. Batch jobs cost half.

Here is a simple example. Say one job sends 100,000 input tokens and gets 10,000 output tokens back. Input costs $0.80 and output costs $0.40, so the job costs $1.20.

The same job costs $0.60 in a batch and $2.40 in fast mode. If the input comes from the cache, input drops from $0.80 to $0.02.

### Safeguards and Data Rules

Safety classifiers block penetration testing, exploit generation, and most research biology tasks. A blocked request gets an answer from Opus 4.8 for cyber topics or Opus 5 for biology.

Anthropic reports about 55% fewer blocks than Fable 5.1. That includes 60% fewer in cybersecurity and 40% fewer in biology.

Fable 5.2 needs 30-day data retention. Zero data retention requires express approval from Anthropic.

## Claude Fable 5.2 vs Fable 5.1

Fable 5.2 beats Fable 5.1 on intelligence and costs less per token, but it takes longer and costs more per task. The table uses Artificial Analysis Index v5.0.1 data for Fable 5.2 and Fable 5.1.

| **Metric** | **Fable 5.1** | **Fable 5.2** | 
|---|---|---|
| Intelligence Index | 50 | 65 | 
| Intelligence rank | 22 of 74 | 1 of 74 | 
| Time to first answer token | 58.37 sec | 215.95 sec | 
| Output speed | 83 tokens/sec | 98 tokens/sec | 
| Time per task | 19.4 min | 46.4 min | 
| Cost per task | $10.85 | $26.54 | 
| Input price per million tokens | $10 | $8 | 
| Output price per million tokens | $50 | $40 | 
| Input tokens in the index run | 7.73B | 28.3B | 
| Output tokens in the index run | 115M | 275M | 

The per-token price fell 20%, yet cost per task rose about 2.4 times. The reason shows in token use. Fable 5.2 produced about 2.4 times as many output tokens in the index run.

Output speed rose from 83 to 98 tokens per second. Anthropic says Fable 5.2 generates output up to 30% faster than Fable 5.1. The wait for the first answer token grew, though, because the model takes longer to think.

Switching from Fable 5.1 involves more than changing the model ID. Anthropic's What’s new page lists four changes that could cause code to stop working:

- The prefix check returns a 400 error on every account if your code edits earlier turns.
- Only Fable 5.2 and Mythos 5.2 can read Fable 5.2’s thinking blocks.
- The older computer_20251124 tool fails on every platform except Amazon Bedrock.
- A Fable 5.2 executor in the advisor tool needs a Fable 5.2 or Mythos 5.2 advisor.

No code changes are needed if Claude Code, claude.ai, or the Claude Agent SDK builds your requests.

**Also Read:** Fable 5.2 vs Fable 5.1 for the price, speed, and benchmark gaps side by side.

## How to Access and Use Claude Fable 5.2

You can use Fable 5.2 in the Claude apps, the Claude API, AWS, Google Cloud, and Microsoft Foundry. Anthropic says it is available today on all of them. Developers can start with claude-fable-5-2 on the Claude Platform.

- **Claude apps and Claude Code:** Pick it from the model list. This week, eligible paid plans get extra Fable usage. Fable counts for up to 50% of weekly limits on Pro plans and standard Team seats.
- **Claude API:** Use claude-fable-5-2. The docs also offer a playground to test the model first.
- **AWS:** Use anthropic.claude-fable-5-2 on Amazon Bedrock, or claude-fable-5-2 on Claude Platform on AWS.
- **Google Cloud:** Use claude-fable-5-2 through Claude Models on Google Cloud, in the Anthropic-managed or Google-managed option.
- **Microsoft Foundry:** Use claude-fable-5-2 on Anthropic-hosted deployments.

Fast mode runs on the Claude Platform, Google Cloud, and Microsoft Foundry. On Foundry, it is a separate model called claude-fable-5-2-fast.

### Tips for Better Results

Fable 5.2 follows instructions closely, so writing clear prompts can help you get better results. Anthropic's Claude documentation explains a few things to keep in mind.

- **Start at high effort** : Move to xhigh only when your tests show a gain.
- **Tell it to finish the whole task:** On long jobs, it sometimes ends a turn with a list of what remains. This happens more at low and medium effort.
- **Say who owns the code.** Without that line, it tends to add to the existing structure and report nearby problems instead of fixing them.
- **Skip prompts that ask for less thinking:** At medium effort and above, they do not stop the model from thinking. Lower the effort setting instead.
- **Ask for progress updates:** Long runs of tool calls can pass with little text.
- **Set limits for subagents:** They can use up a large part of your session budget.
- **Avoid strict sentence-length limits:** They can make the writing sound broken and unnatural.

## Conclusion

Claude Fable 5.2 is a good fit for complex coding, research, and tasks that need extended reasoning. Its 1M-token context window and image input also help with larger projects. Compared with Fable 5.1, it scores 15 points higher on the Artificial Analysis Intelligence Index and costs 20% less per token, though complex tasks can still take more time and money.

For developers who want to turn an idea into a working product, the AI model is just one part of the process. Fable 5.2 is now available on Emergent. Emergent is an **AI agent builder** that helps turn your ideas into working software. Describe what you want to build in simple words, and Emergent helps create the application.
