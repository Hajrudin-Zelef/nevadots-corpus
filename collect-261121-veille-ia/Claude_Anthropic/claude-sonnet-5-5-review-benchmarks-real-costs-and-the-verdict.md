---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-review-benchmarks-real-costs-and-the-verdict
title: "Claude Sonnet 5.5 review: benchmarks, real costs, and the verdict"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "opus", "claude", "model", "anthropic", "launch", "verdict", "costs"]
source: https://www.eesel.ai/blog/claude-sonnet-5-5-review
source_anchor: ""
source_lines: [1, 171]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5 is the best mid-tier model Anthropic has shipped, as long as you keep it at Low or Medium effort. At those settings it lands within a few points of Opus 5.5 on most launch benchmarks, beats it on Terminal-Bench 4.0 (70.6% vs 66.4%), and costs half as much per token at $2 in and $10 out.

The part the launch post underplays: at Max effort, Sonnet 5.5 costs more per task than Opus 5.5, $7.60 against $5.98 on Artificial Analysis, because it burns about 193k tokens per task against Opus's 119k. Crank the dial and you pay Opus money for a slightly weaker model.

My verdict: switch now for well-scoped work (bug fixes, sub-agents, drafting support replies), keep Opus 5.5 for open-ended judgment, and fix forced `tool_choice` calls before you swap the model ID. For support teams, eesel runs models like this inside a ready-made AI helpdesk teammate, so the speed-up lands on your queue without you owning the migration.

## My verdict at a glance

The day after launch I spent reading Anthropic's announcement and the model docs, then the migration guide, and also the roughly 550-comment Hacker News launch thread. My job is building AI agents for eesel's helpdesk teammate, so I went through all of it with one question in mind, which is whether I would put this model under a real support queue tomorrow.

Here is the short version, scored on the things that matter once you actually ship it:

| What I checked | Score | Why | 
|---|---|---|
| Coding quality | 9/10 | 70.6% on Terminal-Bench 4.0, above Opus 5.5 | 
| Knowledge work | 8/10 | GDPval-AA 1844, two points behind Opus 5.5's 1846 | 
| Token efficiency | 9/10 at Low/Medium, 4/10 at Max | ~121k vs 497k tokens per answer at Balyasny, but 193k per task at Max | 
| Speed | 8/10 | 30%+ faster output than Sonnet 5 | 
| Migration pain | 5/10 | Five breaking changes, one of them nasty for support bots | 
| Support fit | 8/10 | Zendesk saw tickets processed 20% faster | 
| **Overall** | **8/10** | Best value in the lineup, with one expensive setting to avoid | 

A quick note on method, so it's clear what these scores rest on. I did not run a months-long production trial, since the model is only one day old. The scores are built from Anthropic's published numbers and the named customer results in the launch post, plus independent benchmark runs people posted in public, and then my own read of the API docs against how eesel wires models into ticket workflows. Where a number comes from Anthropic, I say so.

## What Claude Sonnet 5.5 is

Claude Sonnet 5.5 is the mid-tier model in Anthropic's Claude 5.5 family, released on September 28, 2026. It sits below Claude Opus 5.5 and above Haiku 4.5, while Claude Fable 5.1 holds the very top of the range. Anthropic pitches it as "a faster, lower-cost complement" to Opus, one that is strongest at "well-scoped everyday tasks, fixing bugs, and creating polished documents, slides, and spreadsheets."

In terms of specs, the model overview lists a 1M-token context window and 128K max output (300K on Batch), with a June 2026 knowledge cutoff and the same tokenizer as Claude Sonnet 5. For the full launch walkthrough, my colleague's Sonnet 5.5 overview covers it well. This post is more the verdict.

## Where Claude Sonnet 5.5 is strong

Three things stood out in the launch data, and all three show up in named customer results, not just Anthropic's own charts.

### It codes like an Opus model

The headline number is Terminal-Bench 4.0, which is an agentic coding test that runs in a command line. Sonnet 5 scored 10.3% there, and Sonnet 5.5 scores 70.6%, above Opus 5.5's 66.4%. That is not a typo. It's the biggest single-generation jump I have seen on any Sonnet model so far.

On the other coding tests it trails Opus a little bit: CursorBench 4.0 is 55.5% against 57.8%, and FrontierCode is 52.1% at Xhigh against 54.4%. For a model at half the per-token price, a two-point gap is honestly a bargain. That is also the reason people in the Opus vs Sonnet debate mostly stopped arguing about quality and moved on to arguing about cost.

The customer numbers are backing this up too. Base44 said that across 118 real app builds, Sonnet 5.5 "produced apps that scored level with Opus 5" in 3.6 iterations per build, where Opus 5 needed 7.7. Unity's claim is that it completed 90% of tasks in their multi-step editor benchmark. And if your coding happens in Claude Code, this is now the model you will see by default at Medium effort.

### It gets there in far fewer tokens

The price per token did not change; what changed is the number of tokens. Balyasny Asset Management ran 2,441 finance tasks and found Sonnet 5.5 used about 121k tokens per answer, where Sonnet 5 used 497k. On its Slackbot evals Slack saw about 14% fewer output tokens with no prompt changes, and Lovable for its part reported a third fewer tool calls.

Anthropic's own demo points at the same thing. On a "wind shaping sand dunes" prompt, Sonnet 5 was still busy writing code by the time Sonnet 5.5 already had a running animation:

That efficiency is really the product here. According to Anthropic the result is "up to 30% less per task" than Sonnet 5. With the teams I talk to, fewer tokens also mean fewer seconds that a customer spends waiting on a reply, and that part matters more to them than the invoice.

### It handles support tickets well

This is the part I care about the most. Zendesk's Director of AI said in the launch post that they "fed Claude Sonnet 5.5 hundreds of real support use cases across replies and escalation requests. It made fewer wrong decisions," and tickets were processed 20% faster. On the Atlassian side, the company says Rovo agents will run up to 30% faster than on Sonnet 5.

For Zendesk AI-style work, those two are the numbers that decide if a model is good: fewer wrong escalation decisions, and less waiting. Both of them moved the right way.

## Where Claude Sonnet 5.5 falls short

The weak spots are narrower than the skeptics suggest, but one of them changes how you should run the model.

### Max effort costs more than Opus

Here is the finding that changed my verdict. Anthropic's own launch post admits that Sonnet 5.5 "complements Opus 5.5 best when running at lower effort settings," and that "at higher settings, it can perform comparably at a similar cost." The independent runs go a step further than that.

On Artificial Analysis, as one Redditor pointed out, Sonnet 5.5 at Max costs $7.60 per task against $5.98 for Opus 5.5 at Max, because it used 193k tokens per task where Opus used 119k. Simon Willison on launch day ran the same SVG prompt at every effort level. Low cost 1.6 cents and took 10 seconds. Max cost $1.28 and ran for 15 minutes 40 seconds, burning its full 128,000 thinking tokens before it failed to produce the image.

Even the FrontierCode footnote from Anthropic has the same shape. Sonnet 5.5 scores 52.1% at Xhigh but only 46.2% at Max, because at Max it "more often ran Claude Code's code-review skill," which led to timeouts or edits beyond the task's scope. More thinking, in this case, made the result worse.

So the rule is a simple one: treat Max as off-limits for Sonnet 5.5. A task that needs that much reasoning is a task for Opus 5.5, which gets there with fewer tokens.

### Opus is still better at open-ended judgment

Anthropic is quite candid on this point: "Opus 5.5 remains clearly stronger at complex, open-ended work requiring sustained judgment." The early hands-on reports agree with it.

"Tried Sonnet 5.5 but worse than OPUS for thinking for sure, less error/inconsistency check. I used Opus 5.5 med vs. Sonnet 5.5 High on hermes with the same agent.md, and soul.md"


Translated to support, that is the angry enterprise customer who has a billing dispute and three prior tickets. That ticket I would not hand to Sonnet 5.5 unsupervised. The password reset or the order status check, or the "how do I export my data" question, those I would hand it all day.

### It stops to ask questions

There is one developer who runs an adversarial benchmark built on an esoteric programming language. On it Sonnet 5.5 scored 7.4% against Sonnet 5's 17.8%, and their explanation was that it "is more reluctant to keep going to get an answer, instead it returns to ask the user questions whether to keep going." Base44, by contrast, said it "rarely stopped mid-build to ask the user a question."

My read on this is that in interactive work, pausing to check is often what you want anyway. In an unattended pipeline it's different: a model that stops to ask can stall a job overnight. So test it on your own agent loop before you trust it headless.

### Cyber safeguards and five breaking changes

Sonnet 5.5 is the first Sonnet to launch with cyber safeguards like Opus 5.5's, so "higher-risk cybersecurity tasks will visibly fall back to Sonnet 5." Routine bug fixing is not affected. Still, the biggest sub-thread on Hacker News was from people doing authorized security work who got flagged anyway.

The migration guide lists five breaking changes, each of which now returns a 400 error:

| Change | What breaks | Fix | 
|---|---|---|
| `thinking: disabled` | Requests with thinking fully off | Use the new `between_tools` setting | 
| Forced `tool_choice` (any or tool) | Bots that force a classification tool | Let the model choose, then validate | 
| Edited history before a thinking block | Apps that rewrite past turns | Keep history append-only | 
| `computer_20251124` | Old computer-use tool on the API and Google Cloud | Use `computer_toolset_20260801` | 
| Some advisor tool pairings | A Sonnet 5 or Opus 4.8 advisor on a Sonnet 5.5 executor | Pair it with Opus 5.5 or Sonnet 5.5 | 

For support teams the trap is the forced `tool_choice` one. Many AI helpdesk integrations force a "classify this ticket" tool on every request for ticket triage, so if you swap the model ID without fixing that, every ticket errors out. There is a quieter one as well: text between tool calls now arrives inside thinking blocks, which means a chat widget that streams "Checking your order..." can go silent. Why that silence matters to customers is covered in my guide to AI agent handoffs.

## What developers are saying

The reception on launch day was mixed, but in a useful way. Nobody disputes the benchmarks; the argument is more about where Sonnet 5.5 fits next to Opus.

"It appears, at least from a quick look, to be noticeably faster than Opus. If true, and you don't need xhigh/max reasoning for your use case (like a well-defined set of code changes), Sonnet might get the job done much more quickly."


The skeptics are focused on price, especially the fact that cache reads cost the same $0.20 as Opus 5.5:

"I feel like sonnet is priced too close to opus right now. If Sonnet 5.5 were half its current price it would make sense to use."


The use case people keep coming back to is Sonnet as the fast implementer, sitting under an Opus planner:

"Firmly places itself as a solid subagent for opus, great work from anthropic, for once I'm interested in what haiku turns out as."


Another commenter described their own split as "80% Sonnet 5.5, Opus 5.5 to finish the last 20%." The pattern maps quite neatly onto Claude Code subagents, and it's the setup I would copy for myself.

## Claude Sonnet 5.5 pricing in one table

The rates, per million tokens, come from Anthropic's pricing docs:

| Price per 1M tokens | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | Haiku 4.5 | 
|---|---|---|---|---|
| Input | **$2** | $2 | $4 | $1 | 
| Output | **$10** | $10 | $20 | $5 | 
| 5-minute cache write | $2.50 | $2.50 | $5 | $1.25 | 
| Cache read | $0.20 | $0.20 | $0.20 | $0.10 | 
| Batch input / output | $1 / $5 | $1 / $5 | $2 / $10 | $0.50 / $2.50 | 
| Context window | 1M | 1M | 1M | 200K | 

Thinking tokens are billed as output, and that is why the effort setting matters this much. A support reply at Medium effort might think for a few hundred tokens, while the same reply at Max can think for tens of thousands, all of it at $10 per million. The model line is rarely the biggest part of an AI support agent's cost, but the effort dial is the fastest way of making it one.

Consumer access is the simpler part. Anthropic says anyone can chat with Sonnet 5.5 on Claude.ai, and the paid plans are in my Claude Pro pricing guide. From day one it is also on Amazon Bedrock and Google Cloud, plus Microsoft Foundry. If you are weighing up providers, start with the three-way API comparison. For the head-to-head with OpenAI, read the GPT-6 Sol vs Opus 5.5 breakdown.

## Who should switch, and who should wait

Here is how I would make the call, depending on what you run today:

| You run... | My call | Effort setting | 
|---|---|---|
| Sonnet 5 in production | Switch this week, after the migration fixes | Medium | 
| Opus 5.5 for everyday coding | Move routine tasks to Sonnet 5.5 sub-agents | Low or Medium | 
| Opus 5.5 for open-ended planning | Stay on Opus | n/a | 
| A support bot with forced tool calls | Wait until you have fixed `tool_choice` | Medium | 
| Security or pentest workflows | Wait, and apply to the Cyber Verification Program | n/a | 
| High-volume tagging and routing | Stay on Haiku 4.5 until Haiku 5.5 ships | n/a | 

Anthropic says Claude Haiku 5.5 will join the family "in the coming weeks," so if cost per ticket is your main worry, that one is worth waiting for. For a wider look at options, my roundup of Claude customer service alternatives covers support-specific options. For general model swaps, see the Sonnet alternatives list.

## A smarter model does not fix a wrong knowledge base

There is one thing a model review can't show you, and it's that most support bot failures I have seen have nothing to do with the model. A B2B vehicle telematics team running about 200 Zendesk tickets a month found out their bot was telling customers it supported car brands that were not in their database. The model was not hallucinating, though. Their knowledge base said they supported "all models," and the bot simply believed it. Jumping from Sonnet 5 to Sonnet 5.5 would have given the same wrong answer, only 30% faster.

This is the reason I never judge a model for support on benchmarks alone. At eesel every rollout is simulated against historical tickets before the teammate ever talks to a customer, since that is the place where you catch the knowledge problems a better model can't fix.

## Try eesel with Claude-class models on your queue

If you want Sonnet 5.5's speed on your tickets without owning the migration yourself, eesel is the shortcut. Its AI helpdesk teammate joins the helpdesk you already have, such as Zendesk, and learns from your past tickets and help center, then drafts or sends replies with a frontier model underneath. All the effort tuning and breaking changes from this review become eesel's job instead of yours.

If you like working from a terminal, the eesel CLI operates the same teammate and workspace that the dashboard does. You can script it, or you can let a coding agent like Claude Code drive it, which is basically the same Opus-plans, Sonnet-builds pattern from above, just applied to your support setup. My post on the AI agent CLI goes deeper, and the one on MCP servers covers the connector side.

The free plan comes with 100 credits and no card, and paid plans start at $299 for 500 credits. Try eesel on a slice of your real tickets and see if the faster model actually shows up as faster answers.

## Is Claude Sonnet 5.5 worth it?

Yes, with one rule attached to it. At Low or Medium effort, Claude Sonnet 5.5 gives you near-Opus coding and knowledge work at half the per-token price, and it's 30%+ faster in far fewer tokens. That makes it the new default for well-scoped work and sub-agents, and for everyday support replies as well.

At Max it costs more than Opus and sometimes fails anyway, so don't use it there. Keep Opus 5.5 for the hard, open-ended calls and fix your forced tool calls before migrating. After that, let the effort dial decide your bill, not the model name.
