---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-0-15-0-60-pricing-agent-benchmarks-and-the-hallucination-caveat-builders-should-t
title: "MiniMax M3.1 Flash: $0.15/$0.60 Pricing, Agent Benchmarks and the Hallucination Caveat Builders Should Test"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "flash", "venturebeat", "agent", "says", "cost", "about", "benchmarks"]
source: https://www.harelasaf.com/articles/minimax-m3-1-flash-agent-model-price-claims-hallucination-caveat
source_anchor: ""
source_lines: [1, 97]
section: "MiniMax"
---
# MiniMax M3.1 Flash: $0.15/$0.60 Pricing, Agent Benchmarks and the Hallucination Caveat Builders Should Test

MiniMax says M3.1 Flash is nearly 3x faster and 70% cheaper for agent tasks than its predecessor. What the launch post claims, what VentureBeat reports from Artificial Analysis testing, and what to check before you switch.

MiniMax released M3.1 Flash on October 19 and says it makes agent inference nearly three times faster and cuts usage costs by 70% against its predecessor. The vendor numbers look strong. The independent numbers reported by VentureBeat also look good on cost, but they come with a warning about wrong answers. I have not run the model.

I read MiniMax's launch post, MiniMax M3.1 Flash: Redefining Agent Performance and Cost, and VentureBeat's report, MiniMax's new M3.1 Flash beats DeepSeek and Opus 5 on a key business benchmark at lower cost, but with a high hallucination rate. I found the model through the TLDR AI newsletter of October 20. I did not open the Artificial Analysis page itself, and I only read the first part of the VentureBeat article.

## What MiniMax claims

From the launch post:

- **Speed.** Output speeds can reach 200+ tokens per second, with median speeds consistently 170+ TPS in everyday use. MiniMax says inference is nearly three times faster than before.
- **Cost.** Overall usage cost for agentic tasks is 70% lower than the predecessor.
- **Price.** $0.15 per million input tokens, $0.60 per million output, and $0.015 per million cached input.
- **Agent and coding benchmarks (vendor-reported).** 76.2 on BankerToolBench, 73.7% on DeepSWE v1.1, 61.7 on OSWorld V2, 29.6 on Agents' Last Exam, and 82% on CyberGym.
- **Availability.** Through the MiniMax API, M Plan and MiniMax Code. Weights are to be released later, and a Pro model is to arrive later.

MiniMax also says an anonymous early version, "Space Bunny," processed more than 200 trillion tokens. That is a usage claim I cannot check.

## What independent testing reportedly shows

VentureBeat reports that Artificial Analysis gave M3.1 Flash an Intelligence Index score of 42, up from 29 for MiniMax M3. It says the average cost of completing an Index task fell from about $0.51 to about $0.22. It lists comparison points: GLM-5.3 Flash at 42 (about $0.28 per task), DeepSeek V4.1 Flash-1017 at 45 (about $0.26) and Gemini 3.8 Flash at 41 (about $1.24). VentureBeat also says M3.1 Flash used about 189 million output tokens in the evaluation versus roughly 324 million and 334 million for GLM-5.3 Flash and Gemini 3.8 Flash.

I am relaying these from VentureBeat's article, not from Artificial Analysis directly.

## The caveat

VentureBeat says Artificial Analysis found M3.1 Flash answered more than half of its factual knowledge questions correctly, but had an "exceptionally high tendency" to give wrong answers instead of admitting uncertainty. The headline calls it a high hallucination rate. For agents that act on what the model says, that matters more than a cheaper token price. I do not have the exact figure, because I did not open the Artificial Analysis data.

## How the claims differ

MiniMax's headline comparisons use its own chosen benchmarks and comparison models. VentureBeat's numbers come from a third party. They are not the same evaluation, so do not mix them.

## What I would check

These are my own untested suggestions.

**1.** **Run your own task set.** Compare accuracy, retries and total cost per finished task against your current model, not price per token alone.

**2.** **Test for confident wrong answers.** Add questions where the right behavior is "I don't know," and see how often it guesses.

**3.** **Check tool-call reliability** on your real tools, especially chained calls.

**4.** **Measure speed from your region** at your prompt sizes. The 170+ TPS median is MiniMax's number.

**5.** **Check data handling and terms** for the MiniMax API before sending sensitive data. I did not read them.

**6.** **Plan for weights later.** The post says weights are coming, with no date, so self-hosting is not an option yet.

## What I would not conclude

- That it matches frontier models on your work. MiniMax says "on par with top models" on selected benchmarks. The independent index puts it near other low-cost models, behind the most capable systems, per VentureBeat.
- That cheaper per task means safer to deploy. The reported hallucination tendency cuts the other way.
- That the vendor and third-party figures can be compared directly.

## Frequently asked questions

### What is MiniMax M3.1 Flash?

A model from MiniMax released October 19, 2026, aimed at agentic tasks, coding, computer use and cybersecurity, per the launch post.

### How much does it cost?

$0.15 input, $0.60 output and $0.015 cached input per million tokens, according to MiniMax.

### How fast is it?

MiniMax reports 200+ TPS output, with a 170+ TPS median.

### What did independent testing find?

VentureBeat, citing Artificial Analysis, reports an Intelligence Index score of 42 and a cost per task of about $0.22, and a high tendency to answer wrongly instead of abstaining.

### Are the weights open?

Not yet. MiniMax says weights will be released later.

### Where can I use it?

The MiniMax API, M Plan and MiniMax Code, per the post.

### Should I switch my agents to it?

Test it on your own tasks first, especially for wrong-answer behavior.

## Sources

- MiniMax, MiniMax M3.1 Flash: Redefining Agent Performance and Cost (October 19, 2026): vendor claims, pricing and availability.
- VentureBeat, MiniMax's new M3.1 Flash beats DeepSeek and Opus 5 on a key business benchmark at lower cost, but with a high hallucination rate (October 19, 2026): reported Artificial Analysis results. First part of the article read.
- TLDR AI newsletter, October 20, 2026: discovery pointer only.

## Related reading

- Agent Skills Moves to the Agentic AI Foundation: What the Donation Changes for BuildersAnthropic donated the Agent Skills open standard to the Linux Foundation's Agentic AI Foundation on October 22, alongside Open Responses, OpenEnv and OpenSandbox. What the announcements say, what is still a roadmap, and what to check before you depend on it.
- OpenAI's Decisions API Is Generally Available: Typed Answers, $0.10 per Million Input Tokens and What to TestOpenAI says the Decisions API is generally available, built on GPT-6 Luna, up to 10x faster than the Responses API for bounded questions, with no output-token charges. What the launch post and docs say, and how to test it for routing and classification.
- Claude Duet: Anthropic's Proactive Agent Preview and the Design Choices Builders Can Learn FromAnthropic opened a waitlist research preview of Claude Duet, an agent that checks your apps every few hours and asks approval before it sends, pays, signs or deletes. What the launch post says, and the agent design patterns worth studying.
