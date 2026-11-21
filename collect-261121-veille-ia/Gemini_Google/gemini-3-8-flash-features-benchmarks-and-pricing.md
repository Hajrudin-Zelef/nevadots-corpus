---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-features-benchmarks-and-pricing
title: "Gemini 3.8 Flash: Features, Benchmarks, and Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["flash", "gemini", "model", "google", "models", "coding", "effort", "features"]
source: https://www.datacamp.com/blog/gemini-3-8-flash-cyber
source_anchor: ""
source_lines: [1, 337]
section: "Gemini_Google"
---
Google is releasing Flash models at a pace that is hard to keep up with. Gemini 3.8 Flash is the company's third Flash release in six weeks, and it lands as its best reasoning and coding model in the Flash tier, arriving at the same speed and low cost as 3.7 Flash from three weeks earlier.

Alongside the standard model, Google introduced Gemini 3.8 Flash Cyber, a variant tuned for cybersecurity work that is available only to trusted defenders through the Fairwind Program, a limited-access initiative launched at the same time as the new models. Both models share the same foundational intelligence.

The headline numbers are strong, with Gemini 3.8 Flash scoring 90.8% on Terminal-Bench 2.1 (up from 81.6% for 3.7 Flash), 54.9% on HLE-Verified, and a real-world vulnerability discovery rate above 70% for the Cyber variant.

In this article, I'll cover everything new with Gemini 3.8 Flash and 3.8 Flash Cyber, looking at the new features, exploring the benchmarks, and putting them through their paces with hands-on examples. See the model in action in our Gemini 3.8 Flash API tutorial, and read about Google's newest voice assistant model in our Gemini 3.8 Live guide.

To get up to speed on the rival models these compete with, see our guides to DeepSeek v4.1 vs Gemini 3.8 Flash and Claude Opus 5 vs Sonnet 5.

## In a Nutshell

- Gemini 3.8 Flash is Google's most intelligent Flash-tier model, released September 2, 2026, and its third Flash model in six weeks.
- It scores 90.8% on Terminal-Bench 2.1 (up from 81.6% for 3.7 Flash) and outperforms most larger frontier models on DeepSWE v1.1 for long-horizon coding.
- Gains are uneven: coding and tool use jumped, but Humanity's Last Exam stayed flat at 45.4% vs 45.7%.
- Pricing holds at $0.75 input and $3.75 output per 1M tokens through December 31, 2026, then $1.50/$7.50.
- Gemini 3.8 Flash Cyber, restricted to Fairwind Program defenders, exceeds a 70% real-world vulnerability discovery rate and sits on the CWE-Bench Pareto frontier for patching.

## What Is Gemini 3.8 Flash?

Gemini 3.8 Flash is Google's most intelligent Flash-tier model, engineered for long-horizon software engineering, autonomous agents, and complex enterprise workflows.

It sits below the Pro tier (like Gemini 3.1 Pro) but is priced at a fraction of the cost, keeping the introductory rate of $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026.

The main change from 3.7 Flash is that 3.8 Flash simply works harder.

On complex tasks, it runs extra reasoning steps and calls tools iteratively, which is why it can approach the performance of higher-cost frontier models. The trade-off is token overhead: at higher effort levels, the model may burn more tokens to squeeze out the last few points of accuracy.

The most striking benchmark headline is on the Terminal-Bench 2.1, where 3.8 Flash scored 90.8% against 3.7 Flash's 81.6%. Terminal-Bench measures how reliably a model can complete real command-line and coding tasks end to end, so a 9-point jump points directly at better agentic coding behavior. 

## What's New With Gemini 3.8 Flash?

The features in 3.8 Flash are aimed at developers running agents and long coding sessions rather than casual chat users.

Here are the capabilities worth knowing about.

### Effort levels you can tune per task

You can dial the model's effort up or down depending on whether you care more about accuracy or cost.

- At higher effort levels, 3.8 Flash executes more reasoning steps and calls tools repeatedly, which lifts its scores on hard engineering tasks.
- At lower effort levels, it minimizes token overhead for latency-sensitive or compute-constrained work.

This matters because 3.8 Flash's gains come at the price of extra tokens.

If your workload is efficiency-first, Google explicitly says you can stay on 3.7 Flash, which remains fully supported.

That is an unusually honest note for Google to include in a launch post, and it tells you the effort dial is a genuine trade-off rather than free performance.

### Long-horizon autonomous agents

Gemini 3.8 Flash is built to run agents that work through multi-step engineering problems without hand-holding.

On DeepSWE v1.1, a long-horizon software engineering benchmark, it outperforms most larger frontier models at a fraction of their cost while solving complex problems end to end.

For practitioners building coding agents, this is the number that counts.

Reddit and X users leaking pre-release impressions described it as "Fable 5 level performance at Flash pricing," and the Wall Street Journal reporting noted that in Google's internal Jetski head-to-head testing, engineers preferred 3.8 Flash to Anthropic's Opus model for coding.

That is an aggressive competitive claim, and worth verifying once the API is widely available.

### Reliability in specialized professional domains

3.8 Flash targets regulated, high-stakes fields where a wrong answer is expensive.

It outperforms 3.7 Flash and other frontier models on Vals Finance Agent V2 and Harvey's Legal Agent Benchmark, both of which test analysis and reporting in finance and law.

Think about a workflow where an agent reads a set of quarterly filings and drafts a compliance summary, or reviews contract clauses against a policy. The dependability required there is different from generic chat, and Google is pitching 3.8 Flash directly at enterprise autonomy in these knowledge domains.

Gemini 3.8 Flash runs inside Google Antigravity and Google AI Studio, where a single looping prompt can drive it through an entire build.

Google's own example is a fully playable DOS-style version of Google Maps generated from one prompt in Antigravity, complete with locations, directions, and Street View.

For your own work, imagine feeding it a single instruction like "build an interactive dashboard that pulls this CSV, plots three chart types, and lets me filter by region," and letting the agentic loop iterate until it runs.

The looping instruction pattern is where the extra effort levels pay off, since the model keeps refining rather than stopping at a first draft.

## Gemini 3.8 Flash Benchmarks

The benchmark picture is uneven, which is more useful than a clean sweep because it tells you where the gains actually landed.

Coding and terminal tasks improved sharply, while exam-style reasoning barely moved.

Here are the numbers from Google's developer docs and announcement.

### Terminal-Bench 2.1

Gemini 3.8 Flash scored 90.8% on Terminal-Bench 2.1, up from 3.7 Flash's 81.6%.

This benchmark measures how reliably a model completes real command-line and coding tasks from start to finish, including running tools and recovering from errors.

For context on rival mid-tier models, in our GPT-5.6 Terra vs Claude Sonnet 5 comparison, we found Terra scored 87.4% on Terminal-Bench 2.1 and Sonnet 5 scored 80.4%. On that scoreline, 3.8 Flash's 90.8% edges ahead of both, which is a notable claim for a Flash-tier model at $0.75 input.

### DeepSWE v1.1

On DeepSWE v1.1, a long-horizon software engineering benchmark, 3.8 Flash outperforms most larger frontier models at a fraction of their cost.

Google did not publish the exact percentage in the extracted notes, but the framing is that it solves complex engineering problems autonomously, end-to-end.

This is the benchmark that backs the "agent that finishes the job" pitch.

If your workload involves an agent iterating over a codebase for many steps, DeepSWE is closer to your reality than a single-shot coding score.

### SWE-Bench Pro and SWE-Atlas

On SWE-Bench Pro, 3.8 Flash scored 61.6% against 3.7 Flash's 60.4%, and on SWE-Atlas it reached 51.9% versus 48.0%.

These are repository-level software engineering benchmarks that measure fixing real issues in real codebases.

The SWE-Bench Pro gain of just over 1 point is modest, which is worth flagging. The bigger jumps live in Terminal-Bench, so if you care about terminal and tool reliability specifically, that is where 3.8 Flash separates from its predecessor.

### HLE-Verified and Humanity's Last Exam

3.8 Flash achieves 54.9% on HLE-Verified, a test of multi-step reasoning across STEM, humanities, and professional fields.

On the broader Humanity's Last Exam comparison in Google's developer docs, though, the score was essentially flat: 45.4% for 3.8 Flash versus 45.7% for 3.7 Flash.

General exam-style reasoning did not advance much, so if your use case is hard open-ended reasoning rather than coding and tool use, the upgrade from 3.7 to 3.8 Flash may not be worth the extra tokens.

### Cyber benchmarks: CyberGym and CWE-Bench

Gemini 3.8 Flash Cyber posts frontier-level results on vulnerability discovery and patching.

On CyberGym, the standard industry benchmark for autonomous vulnerability discovery, it surpasses 3.5 Flash Cyber and significantly larger frontier models, and on Google's internal benchmark spanning 20 programming languages, it exceeds a 70% success rate.

On CWE-Bench, an external patching benchmark run by Collinear, Cyber sits on the Pareto frontier with a pass@1 of 47.2% against a leading frontier model's 47.8%, at a much lower cost.

Google deliberately prioritized vulnerability fixing over offensive exploitation, which mirrors how Anthropic ships Sonnet 5 with cyber safeguards on by default (0.0% on a Firefox exploit-development eval, as we noted in our Opus 5 vs Sonnet 5 review).

### Gemini 3.8 Flash vs Gemini 3.7 Flash

In the table below, you can see how the new model compares against its weeks-old predecessor: 

| Benchmark | Gemini 3.8 Flash | Gemini 3.7 Flash | 
|---|---|---|
| Terminal-Bench 2.1 | 90.8% | 81.6% | 
| SWE-Bench Pro | 61.6% | 60.4% | 
| SWE-Atlas | 51.9% | 48.0% | 
| τ³-bench Banking | 38.1% | 30.9% | 
| CharXiv (multimodal) | 86.2% | 84.5% | 
| Humanity's Last Exam | 45.4% | 45.7% | 

## Gemini 3.8 Flash Pricing and Availability

Gemini 3.8 Flash is generally available through Google AI Studio and the Gemini Enterprise Agent Platform, released September 2, 2026, with the API model ID `gemini-3.8-flash`.

The 3.8 Flash Cyber variant is restricted to trusted defenders via the Fairwind Program and is not on general release.

The pricing keeps the same introductory rates as 3.7 Flash:

- **Input:** $0.75 per 1M tokens through December 31, 2026
- **Output (including thinking tokens):** $3.75 per 1M tokens through December 31, 2026
- **After January 1, 2027:** $1.50 per 1M input and $7.50 per 1M output

To put that in perspective against rivals as of September 2026: GPT-5.6 Sol runs roughly $5 input and $30 output per 1M tokens, and the new Claude Fable 5.1 costs $10 input and $50 output.

Gemini 3.8 Flash undercuts both by a wide margin, which is the whole point of the Flash tier.

## Final Thoughts

Gemini 3.8 Flash is Google pushing hard on the value end of the coding-model market, and the strategy is clear: match near-frontier coding performance at $0.75 input while shipping a new Flash model every couple of weeks.

The 90.8% on Terminal-Bench 2.1 and the sub-$1 pricing make it a serious default for agentic coding workloads.

My take is that the release is uneven, and Google is upfront about it.

Terminal and tool-use scores jumped, DeepSWE and the finance and legal benchmarks improved, but HLE stayed flat, and SWE-Bench Pro barely moved.

If you build coding agents, this is a meaningful upgrade from 3.7 Flash.

If you need hard open-ended reasoning, the case is thinner, and staying on 3.7 Flash for efficiency-first work is a reasonable call.

The Cyber variant is the more interesting long-term play. A 70%+ real-world vulnerability discovery rate and Pareto-frontier patching, gated behind the Fairwind Program and biased toward defense over exploitation, is a defensible position for a company already using it to secure its own code.

I'd want to see independent reproductions before taking the CyberGym and CWE-Bench numbers at face value, since almost all the data so far comes from Google itself.

If you want to build agents and coding workflows around models like this, our AI Agent Fundamentals skill track is a solid place to start.

Google is releasing Flash models at a pace that is hard to keep up with. Gemini 3.8 Flash is the company's third Flash release in six weeks, and it lands as its best reasoning and coding model in the Flash tier, arriving at the same speed and low cost as 3.7 Flash from three weeks earlier.

Alongside the standard model, Google introduced Gemini 3.8 Flash Cyber, a variant tuned for cybersecurity work that is available only to trusted defenders through the Fairwind Program, a limited-access initiative launched at the same time as the new models. Both models share the same foundational intelligence.

The headline numbers are strong, with Gemini 3.8 Flash scoring 90.8% on Terminal-Bench 2.1 (up from 81.6% for 3.7 Flash), 54.9% on HLE-Verified, and a real-world vulnerability discovery rate above 70% for the Cyber variant.

In this article, I'll cover everything new with Gemini 3.8 Flash and 3.8 Flash Cyber, looking at the new features, exploring the benchmarks, and putting them through their paces with hands-on examples. See the model in action in our Gemini 3.8 Flash API tutorial, and read about Google's newest voice assistant model in our Gemini 3.8 Live guide.

To get up to speed on the rival models these compete with, see our guides to DeepSeek v4.1 vs Gemini 3.8 Flash and Claude Opus 5 vs Sonnet 5.

## In a Nutshell

## What Is Gemini 3.8 Flash?

Gemini 3.8 Flash is Google's most intelligent Flash-tier model, engineered for long-horizon software engineering, autonomous agents, and complex enterprise workflows.

It sits below the Pro tier (like Gemini 3.1 Pro) but is priced at a fraction of the cost, keeping the introductory rate of $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026.

The main change from 3.7 Flash is that 3.8 Flash simply works harder.

On complex tasks, it runs extra reasoning steps and calls tools iteratively, which is why it can approach the performance of higher-cost frontier models. The trade-off is token overhead: at higher effort levels, the model may burn more tokens to squeeze out the last few points of accuracy.

The most striking benchmark headline is on the Terminal-Bench 2.1, where 3.8 Flash scored 90.8% against 3.7 Flash's 81.6%. Terminal-Bench measures how reliably a model can complete real command-line and coding tasks end to end, so a 9-point jump points directly at better agentic coding behavior.

## What's New With Gemini 3.8 Flash?

The features in 3.8 Flash are aimed at developers running agents and long coding sessions rather than casual chat users.

Here are the capabilities worth knowing about.

## Effort levels you can tune per task

You can dial the model's effort up or down depending on whether you care more about accuracy or cost.

This matters because 3.8 Flash's gains come at the price of extra tokens.

If your workload is efficiency-first, Google explicitly says you can stay on 3.7 Flash, which remains fully supported.

That is an unusually honest note for Google to include in a launch post, and it tells you the effort dial is a genuine trade-off rather than free performance.

## Long-horizon autonomous agents

Gemini 3.8 Flash is built to run agents that work through multi-step engineering problems without hand-holding.

On DeepSWE v1.1, a long-horizon software engineering benchmark, it outperforms most larger frontier models at a fraction of their cost while solving complex problems end to end.

For practitioners building coding agents, this is the number that counts.

Reddit and X users leaking pre-release impressions described it as "Fable 5 level performance at Flash pricing," and the Wall Street Journal reporting noted that in Google's internal Jetski head-to-head testing, engineers preferred 3.8 Flash to Anthropic's Opus model for coding.

That is an aggressive competitive claim, and worth verifying once the API is widely available.

## Reliability in specialized professional domains

3.8 Flash targets regulated, high-stakes fields where a wrong answer is expensive.

It outperforms 3.7 Flash and other frontier models on Vals Finance Agent V2 and Harvey's Legal Agent Benchmark, both of which test analysis and reporting in finance and law.

Think about a workflow where an agent reads a set of quarterly filings and drafts a compliance summary, or reviews contract clauses against a policy. The dependability required there is different from generic chat, and Google is pitching 3.8 Flash directly at enterprise autonomy in these knowledge domains.

## Built for agentic tooling in Antigravity and AI Studio

Gemini 3.8 Flash runs inside Google Antigravity and Google AI Studio, where a single looping prompt can drive it through an entire build.

Google's own example is a fully playable DOS-style version of Google Maps generated from one prompt in Antigravity, complete with locations, directions, and Street View.

For your own work, imagine feeding it a single instruction like "build an interactive dashboard that pulls this CSV, plots three chart types, and lets me filter by region," and letting the agentic loop iterate until it runs.

The looping instruction pattern is where the extra effort levels pay off, since the model keeps refining rather than stopping at a first draft.

## Gemini 3.8 Flash Benchmarks

The benchmark picture is uneven, which is more useful than a clean sweep because it tells you where the gains actually landed.

Coding and terminal tasks improved sharply, while exam-style reasoning barely moved.

Here are the numbers from Google's developer docs and announcement.

## Terminal-Bench 2.1

Gemini 3.8 Flash scored 90.8% on Terminal-Bench 2.1, up from 3.7 Flash's 81.6%.

This benchmark measures how reliably a model completes real command-line and coding tasks from start to finish, including running tools and recovering from errors.

For context on rival mid-tier models, in our GPT-5.6 Terra vs Claude Sonnet 5 comparison, we found Terra scored 87.4% on Terminal-Bench 2.1 and Sonnet 5 scored 80.4%. On that scoreline, 3.8 Flash's 90.8% edges ahead of both, which is a notable claim for a Flash-tier model at $0.75 input.

## DeepSWE v1.1

On DeepSWE v1.1, a long-horizon software engineering benchmark, 3.8 Flash outperforms most larger frontier models at a fraction of their cost.

Google did not publish the exact percentage in the extracted notes, but the framing is that it solves complex engineering problems autonomously, end-to-end.

This is the benchmark that backs the "agent that finishes the job" pitch.

If your workload involves an agent iterating over a codebase for many steps, DeepSWE is closer to your reality than a single-shot coding score.

## SWE-Bench Pro and SWE-Atlas

On SWE-Bench Pro, 3.8 Flash scored 61.6% against 3.7 Flash's 60.4%, and on SWE-Atlas it reached 51.9% versus 48.0%.

These are repository-level software engineering benchmarks that measure fixing real issues in real codebases.

The SWE-Bench Pro gain of just over 1 point is modest, which is worth flagging. The bigger jumps live in Terminal-Bench, so if you care about terminal and tool reliability specifically, that is where 3.8 Flash separates from its predecessor.

## HLE-Verified and Humanity's Last Exam

3.8 Flash achieves 54.9% on HLE-Verified, a test of multi-step reasoning across STEM, humanities, and professional fields.

On the broader Humanity's Last Exam comparison in Google's developer docs, though, the score was essentially flat: 45.4% for 3.8 Flash versus 45.7% for 3.7 Flash.

General exam-style reasoning did not advance much, so if your use case is hard open-ended reasoning rather than coding and tool use, the upgrade from 3.7 to 3.8 Flash may not be worth the extra tokens.

## Cyber benchmarks: CyberGym and CWE-Bench

Gemini 3.8 Flash Cyber posts frontier-level results on vulnerability discovery and patching.

On CyberGym, the standard industry benchmark for autonomous vulnerability discovery, it surpasses 3.5 Flash Cyber and significantly larger frontier models, and on Google's internal benchmark spanning 20 programming languages, it exceeds a 70% success rate.

On CWE-Bench, an external patching benchmark run by Collinear, Cyber sits on the Pareto frontier with a pass@1 of 47.2% against a leading frontier model's 47.8%, at a much lower cost.

Google deliberately prioritized vulnerability fixing over offensive exploitation, which mirrors how Anthropic ships Sonnet 5 with cyber safeguards on by default (0.0% on a Firefox exploit-development eval, as we noted in our Opus 5 vs Sonnet 5 review).

## Gemini 3.8 Flash vs Gemini 3.7 Flash

In the table below, you can see how the new model compares against its weeks-old predecessor:

## Gemini 3.8 Flash Pricing and Availability

Gemini 3.8 Flash is generally available through Google AI Studio and the Gemini Enterprise Agent Platform, released September 2, 2026, with the API model ID

`gemini-3.8-flash`.
The 3.8 Flash Cyber variant is restricted to trusted defenders via the Fairwind Program and is not on general release.

The pricing keeps the same introductory rates as 3.7 Flash:

Input:$0.75 per 1M tokens through December 31, 2026Output (including thinking tokens):$3.75 per 1M tokens through December 31, 2026After January 1, 2027:$1.50 per 1M input and $7.50 per 1M output
To put that in perspective against rivals as of September 2026: GPT-5.6 Sol runs roughly $5 input and $30 output per 1M tokens, and the new Claude Fable 5.1 costs $10 input and $50 output.

Gemini 3.8 Flash undercuts both by a wide margin, which is the whole point of the Flash tier.

## Final Thoughts

Gemini 3.8 Flash is Google pushing hard on the value end of the coding-model market, and the strategy is clear: match near-frontier coding performance at $0.75 input while shipping a new Flash model every couple of weeks.

The 90.8% on Terminal-Bench 2.1 and the sub-$1 pricing make it a serious default for agentic coding workloads.

My take is that the release is uneven, and Google is upfront about it.

Terminal and tool-use scores jumped, DeepSWE and the finance and legal benchmarks improved, but HLE stayed flat, and SWE-Bench Pro barely moved.

If you build coding agents, this is a meaningful upgrade from 3.7 Flash.

If you need hard open-ended reasoning, the case is thinner, and staying on 3.7 Flash for efficiency-first work is a reasonable call.

The Cyber variant is the more interesting long-term play. A 70%+ real-world vulnerability discovery rate and Pareto-frontier patching, gated behind the Fairwind Program and biased toward defense over exploitation, is a defensible position for a company already using it to secure its own code.

I'd want to see independent reproductions before taking the CyberGym and CWE-Bench numbers at face value, since almost all the data so far comes from Google itself.

If you want to build agents and coding workflows around models like this, our AI Agent Fundamentals skill track is a solid place to start.
