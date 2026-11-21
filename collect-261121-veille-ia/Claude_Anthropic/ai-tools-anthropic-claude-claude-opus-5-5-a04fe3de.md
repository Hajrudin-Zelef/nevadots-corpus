---
id: collect-261121-veille-ia/claude-anthropic/ai-tools-anthropic-claude-claude-opus-5-5-a04fe3de
title: "Ai tools anthropic claude claude opus 5 5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "claude", "anthropic", "model", "says", "most", "through", "less"]
source: ""
source_anchor: ""
source_lines: [1, 89]
section: "Claude_Anthropic"
---
What Is Claude Opus 5.5? Anthropic's New Model, Explained
Claude Opus 5.5 is Anthropic's newest Opus model and the first release in its Claude 5.5 family, launched on September 22, 2026. Anthropic says it performs at the level of its top widely released model, Claude Fable 5.1, on most work, while costing 40% less to run than Claude Opus 5. This overview explains what it is, what changed, what it costs, where you can use it, and what the safety testing found, in plain language.
What Claude Opus 5.5 Is
Claude Opus 5.5 is a large language model from Anthropic, the company behind the Claude assistant. Opus is Anthropic's line for demanding work: long coding projects, research, and analysis that runs across many documents. Anthropic's launch announcement calls it the first model in the new Claude 5.5 family and says Claude Sonnet 5.5 and Claude Haiku 5.5 will follow in the coming weeks.
The simplest way to picture it: most people meet AI as a chat window that answers one question at a time. Opus 5.5 is built to take on a whole task, read a large codebase or a stack of documents, plan the work, use software tools, and keep going for a long stretch without being walked through each step. It thinks through hard problems before answering and moves quickly through easy ones.
It is the upgrade to Claude Opus 5, and Anthropic's guide to choosing a model now says it directly: "Most workloads start with Claude Opus 5.5." The more expensive Claude Fable 5.1 remains Anthropic's most capable widely released model, for the hardest jobs.
Developers and engineering teams that hand large coding jobs to AI agents, and analysts and knowledge workers who want long documents read, checked and summarized. Everyday users meet it inside the Claude apps; businesses reach it through the Claude API and the major cloud platforms.
What Changed from Claude Opus 5
Four changes matter most for someone deciding whether to use it.
It is cheaper to run
Anthropic says its tests show Opus 5.5 costing 40% less than Opus 5 on typical workloads at default settings. Part of that comes from lower prices, and part from the model finishing work with fewer tokens. The price cuts are 20% on input and output and 60% on cache reads, the discounted rate for re-reading text the system has already processed.
It is faster
The announcement says Opus 5.5 generates output more than 30% faster than Opus 5. For long answers and long agent runs, that means less waiting.
It writes more plainly
Anthropic says Opus 5.5 communicates more naturally than earlier models, and that early testers found its writing clearer and easier to follow, which answers common feedback about Opus 5. It puts the most important information up front, is less likely to use jargon, and follows the writing rules you give it. One early customer, Box, reported that in its own evaluations the answers were 40% less verbose without losing accuracy. That figure is Box's finding, not a general Anthropic benchmark.
It thinks by default, at a lower setting
Opus 5.5 always uses what Anthropic calls adaptive thinking: it decides how much to reason before answering, and, as Anthropic's what's-new page for Opus 5.5 notes, developers cannot switch that reasoning off. How hard it works is set by an effort level, from low through medium, high and xhigh to max. The default is medium, down from high on Opus 5. Anthropic's effort documentation describes medium as a balanced approach with moderate token savings, and advises running an effort sweep on your own tests rather than carrying settings over from an earlier model.
For developers, the switch is not a drop-in swap. Four changes can make existing Opus 5 code return errors, including the always-on thinking above. Our Opus 5.5 pricing and API guide walks through each one with the fix.
What Claude Opus 5.5 Costs
On the Claude API, you pay per token, the small chunks of text a model reads and writes. Opus 5.5 costs $4 per million input tokens and $20 per million output tokens, according to Anthropic's pricing page.
| Model | Input | Output | Cache read | 
|---|---|---|---|
| Claude Opus 5.5 | $4 | $20 | $0.20 | 
| Claude Opus 5 | $5 | $25 | $0.50 | 
| Claude Fable 5.1 | $10 | $50 | $0.25 | 
Claude API list prices, USD per million tokens, from Anthropic's pricing page as of September 22, 2026.
Three other prices are worth knowing. The Batch API, for work that can wait for an answer, halves the rates to $2 and $10. Fast mode, a research preview that delivers up to 2.5 times the output speed, costs $8 and $40. And the full 1M-token context window is billed at the standard rate, with no surcharge for long prompts.
If you use Claude through a subscription instead of the API, you do not pay per token. Alongside the launch, Anthropic is raising the five-hour usage limits on the Pro, Max, Team and seat-based Enterprise plans.
Where You Can Use It
Anthropic's announcement says Opus 5.5 is available on all platforms. In practice there are three routes:
- The Claude apps. Subscribers use it in Claude on the web, desktop and mobile, in Claude Code for software work, and in Claude Cowork. Anthropic's newer Claude Duet app, a proactive agent in research preview, also runs on Opus 5.5.
- The Claude API. Developers call it by the model ID claude-opus-5-5.
- Cloud platforms. The same ID works on Google Cloud, Microsoft Foundry and Claude Platform on AWS. Amazon Bedrock uses anthropic.claude-opus-5-5.
| Spec | Claude Opus 5.5 | 
|---|---|
| Context window | 1M tokens | 
| Max output per reply | 128K tokens (300K on the Message Batches API, beta) | 
| Thinking | Adaptive, always on | 
| Default effort | medium | 
| Reliable knowledge cutoff | June 2026 | 
| Earliest retirement | Not sooner than September 22, 2027 | 
From Anthropic's Opus 5.5 overview documentation, September 22, 2026.
What the Benchmarks Say
Benchmarks are standardized tests. They are useful for comparing models, but they are chosen and run by the company that makes the model, so treat them as the vendor's own claims. The figures below come from Table 8.1.A of the Claude Opus 5.5 System Card, with Claude models run at adaptive thinking and max effort.
| Test (what it measures) | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | 
|---|---|---|---|---|
| SWE-bench Pro (fixing real software issues) | 89.9% | 81.2% | 79.2% | not listed | 
| Terminal-Bench 4.0 (working in a command line) | 66.4% (xhigh) | 55.8% | 52.3% | 57.9% (high, as reported by OpenAI) | 
| Terminal-Bench-Science 0.1 (scientific command-line tasks) | 58.7% | 52.6% | 29.0% | 64.6% | 
| Humanity's Last Exam, with tools (expert questions) | 67.7% | 65.6% | 63.6% | 57.2% | 
| GDPval-AA (professional work, Elo rating) | 1846 | 1735 | 1708 | 1542 | 
Claude Opus 5.5 System Card, Table 8.1.A, September 22, 2026. GDPval-AA ratings are from Artificial Analysis. Competitor figures are drawn from the respective developers' published results, as the system card notes.
The pattern is that Opus 5.5 leads Fable 5.1 and Opus 5 on each of these tests. Against OpenAI's GPT-6 Astra it leads on three of the four tests where the table lists an Astra score. The exception is scientific command-line work, where GPT-6 Astra scores 64.6% to Opus 5.5's 58.7%. The system card's full table also includes AutomationBench, a test of business workflows across connected apps, where GPT-6 Astra narrowly leads, 41.4% to Opus 5.5's 40.0%.
Anthropic itself cautions against reading too much into those margins. Its launch announcement says that at this level of capability, benchmark margins have become "a less reliable guide to real-world differences," and that in Anthropic's own use the gap between Opus 5.5 and Fable 5.1 is "narrower than these scores suggest."
Two of the scores carry a further caveat. The system card reports the Terminal-Bench 4.0 and Terminal-Bench-Science results with Anthropic's production safeguards switched on. Requests the safeguards flagged were answered by an older fallback model (2.5% of Terminal-Bench 4.0 requests and 3.9% of Terminal-Bench-Science requests), so those two scores partly reflect that model. For the full evaluation picture, see our Opus 5.5 system card breakdown, and for head-to-head comparisons, Opus 5.5 vs GPT-6 Astra and Opus 5.5 vs Fable 5.1.
Updated October 5, 2026: Google now compares its own model against Opus 5.5. The benchmark table on Google DeepMind's Gemini 4 Argon page includes an Opus 5.5 column. For models from other companies, Google's methodology note says it used the developers' self-reported numbers unless it notes otherwise. For a few tests, including PostTrainBench, LABBench 2 and GraphWalks, Google ran every model itself, so read each row's source before treating it as a like-for-like comparison. Our Gemini 4 Argon breakdown covers that comparison and who can use Argon today.
How Safe Is It?
Anthropic tested Opus 5.5 before release against its Responsible Scaling Policy and published the results in the system card. The announcement says external evaluators, including Frontier Design and METR, tested it before release. In plain terms, here is what that testing concluded:
- Misalignment risk is low. Anthropic assesses the overall risk of catastrophic harm from the model acting against its developers' intent as low.
- Biology is restricted. Under that policy, Anthropic treats Opus 5.5 as having CB-1 capabilities, which relate to known, non-novel chemical and biological weapons, but not CB-2 capabilities, which relate to novel ones. It ships with expanded biology safeguards. Everyday health and educational questions are unaffected, and vetted research organizations can apply for exemptions.
- Cybersecurity is restricted. Finding and fixing vulnerabilities in source code is allowed as part of normal software work; analyzing compiled programs for exploits is blocked. Anthropic says verified security professionals will be able to get wider access through its Cyber Verification Program.
What happens when a safeguard blocks a request is worth knowing. A blocked biology request is handed to Claude Opus 5, and a blocked cybersecurity request to Claude Opus 4.8, so the user still gets an answer from an older model. In Anthropic's own apps that fallback is automatic; developers on the API turn it on themselves, as Anthropic's refusals and fallback documentation explains.
Opus 5.5, Fable 5.1 or Opus 5: Which to Use
Anthropic's own advice is short: start with Opus 5.5, and only move up to Fable 5.1 if your tests still fall short at the xhigh or max effort settings on demanding reasoning or long-running agent work. Open each question below for the reasoning.
I use Claude for everyday work in the app
Opus 5.5 is the model Anthropic now points most work to, and it writes more plainly than Opus 5. On the Pro, Max, Team and seat-based Enterprise plans, Anthropic is also raising five-hour usage limits with this launch.
I build on the API and cost matters
Start with Opus 5.5. At $4 and $20 per million tokens it costs less than Opus 5 on every line of the price sheet, and less than half of Fable 5.1's $10 and $50. Plan for the four code changes before you switch.
I need the most capable model available
Test Opus 5.5 at xhigh or max effort first. Anthropic's guidance is to move to Fable 5.1 only when those settings still fall short on demanding reasoning or long-running agent work.
My Opus 5 setup works and I cannot change code yet
Anthropic still lists Opus 5 on its pricing page, so you can plan the move instead of rushing it. Our pricing and API guide lists the four code changes to make first.
Frequently Asked Questions
What is Claude Opus 5.5?
How much does Claude Opus 5.5 cost?
Is Claude Opus 5.5 better than Claude Fable 5.1?
What is the Claude Opus 5.5 model ID?
How big is the Claude Opus 5.5 context window?
Video Resources
Go Deeper
Resources from across Tech Jacks Solutions
FREEAI Governance Charter
Establish your organization's AI principles in one document
AI Career Paths
Explore roles that work with these tools daily
EU AI Act Guide
Check your compliance obligations under the EU AI Act
FREEAI Risk Management Template
Identify, assess, and mitigate AI deployment risks
