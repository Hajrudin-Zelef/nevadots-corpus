---
id: collect-261121-veille-ia/gemini-google/what-is-gemini-4-argon-specs-benchmarks-pricing
title: "What Is Gemini 4 Argon? Specs, Benchmarks, Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-13"]
keywords: ["argon", "gemini", "google", "model", "text", "tokens", "output", "specs"]
source: https://emergent.sh/learn/what-is-gemini-4-argon
source_anchor: ""
source_lines: [1, 242]
section: "Gemini_Google"
---
#### TL;DR

- Gemini 4 Argon is Google's flagship reasoning model and the first in the Gemini 4 family. It reached the public on October 13, 2026.
- It has a 1M-token context window. It reads text, images, video, audio, and PDFs, and it answers in text.
- It leads the Vals Index at 68.9% and DeepSWE v1.1 at 77.9%. It trails rivals on Terminal-Bench 4.0 and FrontierSWE v2.
- API pricing is $2 per million input tokens and $10 per million output tokens through November 30, 2026. It then doubles.


Gemini 4 Argon is Google DeepMind’s flagship reasoning model. It works with different types of input and is designed for demanding tasks such as software engineering, long-running AI agents, and in-depth work in science, law, and finance. But what does that mean for app builders? Google first made Argon available to trusted cyber defenders on September 30, 2026, then expanded access to developers, businesses, and Google AI Pro and Ultra subscribers on October 13.

This guide looks at Gemini 4 Argon’s specs, benchmark results, pricing, and limitations, with a closer look at what you can actually build with it without writing code.

A high benchmark score doesn't tell you everything. Argon performs well on some tests but falls behind on others. If you're choosing a model for a real project, those gaps matter just as much as the wins.

## What Is Gemini 4 Argon?

Gemini 4 Argon is Google DeepMind's newest frontier model and the first in the Gemini 4 family. Google's announcement says it targets deep work in coding, finance, legal, cybersecurity, and science. In plain words, Google built it to stay on a hard task across many steps.

**How Google built it**

- **Sparse mixture-of-experts design:** Each request uses only part of the network, rather than activating the whole model.
- **A new model:** Google says Argon isn't a fine-tuned version or a small update to an older model.
- **Reinforcement learning:** Training focused on multi-step reasoning, problem-solving, and theorem-proving.
- **Native multimodal input:** You can give Argon text, images, audio, video, or PDFs to work with, according to the model card.
- **March 2026 knowledge cutoff:** If you ask about an event that happened after March 2026, Argon may not know about it.

**Gemini 4 Argon release date and rollout**

Google released Argon in two phases. It held back broad access while it tested safeguards and worked through the U.S. government's voluntary pre-release process.

| **Date** | **What happened** | 
|---|---|
| Sep 30, 2026 | Google announces Argon and opens it to trusted cyber defenders through the Fairwind Program | 
| Oct 13, 2026 | Argon reaches the Gemini API, Google AI Studio, Google Cloud, Antigravity, and the Gemini app | 
| Nov 30, 2026 | The introductory price ends | 

## Gemini 4 Argon Specs at a Glance

The Gemini API docs list the full spec sheet. Here are the parts that change how you build.

| **Spec** | **Detail** | 
|---|---|
| Model ID | gemini-4-argon | 
| Context window | 1,048,576 tokens, input and output combined | 
| Max output | 65,536 tokens by default, up to 1,048,576 with long decode | 
| Input types | Text, image, video, audio, PDF | 
| Output type | Text only | 
| Thinking levels | Low, medium, high | 
| Tools | Function calling, code execution, structured outputs, search grounding, URL context, computer use (preview) | 
| Not supported | Image generation, audio generation, Live API, explicit caching | 
| Knowledge cutoff | March 2026 | 

**What these specs mean**

- **The 1M window is shared:** A long prompt leaves less room for the answer.
- **Long decode is opt-in:** The default output limit is 65,536 tokens. Google has raised the maximum to 1M tokens for tasks that need a longer, single-pass response.
- **Output is text only:** Argon will not draw images or speak. Pair it with other tools for that.

## What Gemini 4 Argon Does Well

Google leans on three areas: code, knowledge work, and long inputs. Scores below come from the model card unless noted.

- **DeepSWE v1.1: 77.9%.** This test uses long, real software tasks. Claude Opus 5.5 scores 74.2%.
- **Vibe Code Bench: 91.9%.** That edges Claude Fable 5.1 at 90.3%.
- **Video decoder rewrite.** Google's agents made a memory-safe Rust decoder run 2.7x faster than the earlier Rust port.
- **Chromium refactor.** The Chrome team finished work across hundreds of files in weeks, not months, per the Google Cloud blog.
- **Agent Development Kit.** Overnight runs cut session prep time by 40% in long sessions.

These are Google's own results. Treat them as strong leads, not proof for your codebase.

**Finance, legal, and ops work**

- **Vals Finance Agent v2: 65.4%.** GPT-6 Astra scores 53.5%.
- **AutomationBench: 51.3%.** Claude Opus 5.5 scores 42.5%.
- **Harvey's Legal Agent Benchmark: 19.6%.** Argon leads, yet the low score shows how hard legal agent work is.

Harvey's team also reported clear gains over Gemini 3.1 Pro, mainly in M&A, privacy, and venture work.

**Long context and vision**

- **GraphWalks, 256k to 1M tokens: 84.2%.** GPT-6 Astra scores 71.8%.
- **LVBench: 91.7%.** This long-video test puts Argon ahead of GPT-6 Astra at 87.5%.
- **Chartography: 71.6%.** Chart reading is nearly even with GPT-6 Astra at 71.0%.

Think 300-page contracts, hour-long videos, and messy PDFs. That is where Argon earns its price.

**Cyber defense**

- Argon ties GPT-6 Astra at 68% on CWE-bench v1, which tests vulnerability fixes.
- Wiz used it to find a critical flaw in healthcare software that hospitals use worldwide.
- If someone tries to use the public version for penetration testing, the model blocks the request. Vetted defenders in the Fairwind Program, however, get access to a version without those guardrails.

## Gemini 4 Argon vs Claude and GPT-6 Astra

Argon comes out ahead in seven of the twelve tests, ties in one, and falls behind in the remaining four. Google released the scores in October 2026. The best score in each row is bold.

| **Benchmark** | **Gemini 4 Argon** | **GPT-6 Astra** | **Claude Fable 5.1** | **Claude Opus 5.5** | 
|---|---|---|---|---|
| Vals Index | **68.9%** | 63.1% | 65.8% | 67.0% | 
| AutomationBench | **51.3%** | 41.4% | 31.4% | 42.5% | 
| Vals Finance Agent v2 | **65.4%** | 53.5% | 58.9% | 58.6% | 
| DeepSWE v1.1 | **77.9%** | 74.1% | 67.4% | 74.2% | 
| FrontierSWE v2 | 55.0% | **65.5%** | 56.3% | 62.3% | 
| Terminal-Bench 4.0 | 57.4% | 58.2% | 57.9% | **66.4%** | 
| Terminal-Bench Science 0.1 | 57.6% | **68.1%** | 52.6% | 63.3% | 
| LABBench 2 | **88.8%** | 85.4% | 68.6% | 73.1% | 
| GraphWalks, 256k to 1M | **84.2%** | 71.8% | 65.0% | 66.8% | 
| LVBench | **91.7%** | 87.5% | 79.7% | 83.7% | 
| CWE-bench v1 | **68.0%** (tie) | **68.0%** (tie) | 58.0% | 67.0% | 
| OSWorld-2.0 (offline subset) | 69.2% | **72.6%** | not listed | not listed | 

Source: Google DeepMind model card. Google ran or compiled these results, so check them before you rely on them.

**Where Argon wins**

- When a task involves several stages of analysis or financial decisions, Argon has a strong benchmark record. It tops the Vals Index, AutomationBench, and Finance Agent v2.
- On DeepSWE v1.1, Argon scores 3.7 points higher than the next model.
- On Science it scores 88.8% on LABBench 2.
- Argon leads by 12 points on the longest GraphWalks tier, which tests how models handle longer sequences of information.

**Where Argon trails**

- **Terminal-Bench 4.0.** It scores 57.4% against Claude Opus 5.5 at 66.4%, a 9-point gap.
- **FrontierSWE v2.** It scores 55.0% against GPT-6 Astra at 65.5%.
- **Terminal-Bench Science 0.1.** It scores 57.6% against GPT-6 Astra at 68.1%.
- **OSWorld-2.0.** It scores 69.2% against GPT-6 Astra at 72.6%.

Argon handles long-context tasks well, but struggles more with agent work that relies heavily on terminal commands.

**What independent testers say**

Vals AI, an outside benchmark firm, also ranks Argon first on its index at 68.90%. It lists Claude Sonnet 5.5 at 67.04% and Claude Opus 5.5 at 66.97%.

Cost per test favors Argon too. Vals puts it at $15.68, against $21.34 for Sonnet 5.5.

## Gemini 4 Argon Pricing and Access

Google priced Argon in two steps. The low rate runs through November 30, 2026, per the Google Cloud announcement.

| **Tier** | **Input, per 1M tokens** | **Output, per 1M tokens** | **Cached input, per 1M tokens** | 
|---|---|---|---|
| Introductory (through Nov 30, 2026) | $2 | $10 | $0.10 | 
| Standard (after Nov 30, 2026) | $4 | $20 | $0.20 | 

Google prices cached input at 95% off the input rate. The cached figures above come from that discount.

**What a real job costs**

- A task with 200,000 input tokens and 20,000 output tokens costs about $0.60 at the introductory rate.
- The same task costs about $1.20 at the standard rate.
- Long decode and high thinking levels add output tokens, so test with your own prompts before you set a budget.

**Where you can use Gemini 4 Argon**

- **Gemini API and Google AI Studio:** Paid tier, model ID gemini-4-argon.
- **Google Cloud:** Gemini for business and the Gemini platform.
- **Google Antigravity:** Google's agent-first coding platform. Google AI Ultra subscribers can try Argon there and in AI Studio.
- **Gemini app:** Google AI Pro and Ultra subscribers can pick Argon, per Google's app announcement.
- **Android Studio.** Developers can build with it there on the paid API tier.

Moving from Gemini 3.1 Pro takes one line: swap the model name to gemini-4-argon. The MINIMAL thinking level no longer works on Argon.

## Gemini 4 Argon Safety and Limits

If you're using Argon to review a contract, for example, Google's disclosures about its risks help you see why its output still needs checking before you rely on it.

**What Google says it fixed**

- Argon is designed to resist attempts to override its instructions. On the Gray Swan benchmark, attack success fell to 0.7% across 15 adaptive attempts.
- Argon refuses fewer legitimate requests, with unjustified refusals down 9.6 points against Gemini 3.1 Pro.
- Monitors track the model's reasoning and actions and can stop a run.

**What Google could not rule out**

- **Biological risk:** Google cannot rule out that Argon reaches its biological capability threshold. It applied safeguards it rates below that level.
- **Cyber risk:** The same holds for the cybersecurity threshold. That is why the public version blocks offensive tasks.
- **Stealth and awareness:** Argon passes 14 of 15 related challenges, so Google cannot rule out that threshold either.

These details come from the model card.

**Day-to-day limits**

- Argon may give you a confident answer that turns out to be wrong.
- Some requests also run slowly or time out.
- Since its knowledge only goes up to March 2026, it may miss newer developments.
- It also lacks image and audio generation, Live API support, and explicit caching.

If the workflow involves money, contracts, or customer data, have someone check Argon’s output before acting on it.

## Who Should Use Gemini 4 Argon

Argon suits some jobs far better than others. Use this table as a starting point.

| **If you are...** | **Fit** | **Why** | 
|---|---|---|
| A developer running large migrations or refactors | Strong | Leads DeepSWE v1.1 at 77.9% and holds a 1M-token window | 
| A finance or legal analyst | Strong, with review | Leads Vals Finance Agent v2 at 65.4% | 
| Building agents that live in the terminal | Test first | Trails Claude Opus 5.5 by 9 points on Terminal-Bench 4.0 | 
| On a security team | Good for defense | Ties for first on CWE-bench v1, but offensive tasks stay blocked | 
| Watching costs | Check after Nov 30 | Prices double from $2 and $10 to $4 and $20 | 
| A non-technical founder | Indirect | You need a builder, not a raw model | 

**How to test it fairly**

Before choosing a model, test both on five tasks you already handle at work.

For example, use a real research task.

- Write down what a correct answer should include before you start, then compare the results.
- Keep track of the cost and how long each model takes.
- Google notes that Argon can occasionally run slowly.
- Repeat the test after November 30, when standard pricing begins, to see whether the cost changes your decision.

## What Gemini 4 Argon Means for Non-Technical Builders

Gemini 4 Argon is strong at working through hard problems, long documents, and large amounts of context. It is weaker at agent tasks that rely heavily on terminal commands, where other models do better. Its price also doubles after November 30, 2026, so it’s worth testing before then.

But a good model is only one piece of a working product. Argon can write code, but it won’t host your app, take payments, or protect your customers’ data. Someone still has to build all of that:

- A frontend your customers can use
- A backend that stores and protects data
- Login, payments, and the tools your business already uses
- Deployment that you control

That’s where Emergent fits. Emergent is an AI app-building platform. You describe your app in plain words, and it builds the frontend, backend, database, login, payments, and deployment in one go. It connects to Stripe, MongoDB, Shopify, SendGrid, and Twilio. You get a real product with a real backend, and the code is yours.

Here’s what that looks like for people who can’t code:

- A medical practice that got $50K quotes from developers built its own clinical portal for a fraction of the price.
- A roofing contractor replaced $1,800/month in software subscriptions with one command center for leads, pricing, proposals, and payments.
- An Ottawa marketing agency built a healthcare EMR for clinics and is now earning $20K/month from it.
- A meal prep founder built a multi-tenant SaaS at 1/20th the cost.

Emergent won’t replace a Google-sized engineering team yet. But for most people who would have paid a dev shop $80K and waited four months, it already helps a lot.

Here’s a simple way to start:

- Pick one workflow you want to replace, like bookings, orders, or client intake.
- Describe it to Emergent the way you’d explain it to a new hire.
- Launch it, then adjust based on what happens.

You have the idea. Emergent builds the software.
