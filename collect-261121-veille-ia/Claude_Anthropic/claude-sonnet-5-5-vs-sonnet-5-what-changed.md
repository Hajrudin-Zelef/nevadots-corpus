---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-vs-sonnet-5-what-changed
title: "Claude Sonnet 5.5 vs Sonnet 5: What Changed"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-01"]
keywords: ["sonnet", "claude", "tool", "anthropic", "read", "thinking", "agent", "model"]
source: https://www.cosmicjs.com/blog/claude-sonnet-5-5-vs-sonnet-5
source_anchor: ""
source_lines: [1, 111]
section: "Claude_Anthropic"
---
Anthropic launched Claude Sonnet 5.5 (`claude-sonnet-5-5`) on September 28, 2026, six days after Claude Opus 5.5.

If you have an agent running on Claude Sonnet 5, this upgrade is a bigger job than swapping a model string. Anthropic documents five specific ways Sonnet 5 code can break on Sonnet 5.5. Those five changes are the real substance of this release for anyone running tool loops in production.

Every figure below was read from Anthropic's own documentation on October 1, 2026, with the source linked inline. Where Anthropic has not published a number, this page says so rather than estimating.

## Short answer

|  | Claude Sonnet 5.5 | 
|---|---|
| Launched | September 28, 2026 | 
| Model ID | `claude-sonnet-5-5` | 
| Input price | $2 per million tokens | 
| Output price | $10 per million tokens | 
| Context window | 1M tokens | 
| Max output | 128K tokens | 
| Thinking | Adaptive. Up-front thinking can be turned off with `between_tools` | 
| Knowledge cutoff | June 2026 | 
| Earliest retirement | No sooner than September 28, 2027 | 

Sources: Claude models overview and Claude Platform release notes, both read October 1, 2026.

**Switch if** you run agents, multi-step tool chains, or coding assistants, and you can budget time to work through the five breaking changes below. **Wait if** you have a tuned agent in production and nobody can re-run evals this week. The deprecation announced alongside this release applies to Claude Sonnet 4.5. We found no published retirement date for Claude Sonnet 5 on the pages we read, so there is no forced deadline on Sonnet 5 yet.

## Five ways Sonnet 5 code can break on Sonnet 5.5

Anthropic's release note for September 28, 2026 lists these explicitly.

1. **Turning off up-front thinking changed.** Send`thinking: {"type": "between_tools"}` instead of`"disabled"` , at high effort or below.
2. **Forced tool use is gone.**`tool_choice` types`any` and`tool` return a 400 error. Use`auto` with strict tool use instead.
3. **Thinking blocks are tied to the model and the conversation.** Replaying a block across models no longer behaves the way it used to.
4. **The older computer use tool is rejected.** On the Claude API and Google Cloud,`computer_20251124` is not accepted.
5. **The advisor tool narrowed its accepted advisors.** It rejects Claude Opus 4.8, Claude Opus 4.7, and Claude Sonnet 5.

One more behavior is worth knowing. Thinking blocks produced by Claude Sonnet 5.5 are account-bound. When another account sends one of those blocks, the API drops it before the model sees it and the request still succeeds. That failure mode is silent, so nothing shows up in your error logs.

Items 2 and 4 fail loudly with a 400 you will catch on the first call. Items 1, 3 and 5 change behavior quietly, which makes them the ones to write tests for.

### Giving an agent scoped access to your content

Every item above comes from the same place: an agent holding tools in a loop. The same question applies to whatever that loop is allowed to touch. If it can write to your content store, the blast radius of a quiet behavior change is whatever that key can reach.

Cosmic's MCP server scopes every tool to a single bucket, and read and write keys are issued separately. Connect with the read key alone and every create, update, delete, and AI generation tool is blocked with an explicit error while read tools keep working normally. That gives you a safe way to let a new model drive a tool loop against real content before it gets write access. The write-access controls themselves are covered in AI agent write access and CMS controls.

## What Anthropic has not published

As of October 1, 2026, the Claude models overview and the Claude Platform release notes do not publish a speed benchmark for Sonnet 5.5, a quality benchmark against Sonnet 5, or a current price for Claude Sonnet 5.

So this page makes no claim about a speed gain, a lower cost per completed task, or an unchanged token price. Any such figure you see elsewhere should come with a link to where Anthropic published it.

If you need those deltas, measure them on your own workload. Run the same job against both model strings for a day and compare cost per completed task rather than cost per thousand tokens. For an agent, the number that matters is whether the run finishes cleanly, because a loop that loses the thread at step twelve and re-reads its context is the expensive case regardless of rate card.

## The deprecation clock

Two days after Sonnet 5.5 shipped, on September 30, 2026, Anthropic announced the deprecation of Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`), with retirement on the Claude API scheduled for **November 30, 2026**.

If any workload still points at Sonnet 4.5, that is the migration with a real deadline attached.

## What the current lineup costs

| Model | Input / output per MTok | Context | 
|---|---|---|
| Claude Haiku 5.5 | From $0.10 / From $0.50 | 1M | 
| Claude Sonnet 5.5 | $2 / $10 | 1M | 
| Claude Opus 5.5 | $4 / $20 | 1M | 
| Claude Fable 5.1 | $10 / $50 | 1M | 

Source: Claude models overview. The Sonnet, Opus and Fable rows were read October 1, 2026. The Haiku row was re-read October 8, 2026, when Claude Haiku 5.5 had replaced Haiku 4.5 as the current Haiku-tier model.

Claude Opus 5.5 shipped at $4 / $20 per MTok where Claude Opus 5 was $5 / $25, per the release note for September 22, 2026. Frontier-tier pricing moved down in that release.

Sonnet 5.5 is available on the Claude API, Claude in Amazon Bedrock, Claude Platform on AWS, Claude on Google Cloud, and Claude in Microsoft Foundry.

## Using a model from a CMS

If you are calling a model to generate or transform content, keep the model choice in configuration instead of welding it into application code.

Cosmic's AI is available through the same TypeScript SDK you use for content:

```
import { createBucketClient } from '@cosmicjs/sdk';
const cosmic = createBucketClient({
  bucketSlug: process.env.COSMIC_BUCKET_SLUG!,
  readKey: process.env.COSMIC_READ_KEY!,
  writeKey: process.env.COSMIC_WRITE_KEY!,
});
// Keep the model in config so a new release only changes an environment variable.
const CONTENT_MODEL = process.env.CONTENT_MODEL ?? 'claude-sonnet-5-5';
const response = await cosmic.ai.generateText({
  prompt: 'Summarize this release note for a developer audience.',
  model: CONTENT_MODEL,
  max_tokens: 1024,
});
console.log(response.text);
```

Because the model is a parameter, moving a workload to a newer model is a config change. The available model list is updated as providers ship, so check the AI documentation for the current set before pinning a string in production.

## Release cadence is the real planning problem

Claude Fable 5.1 on September 1. Claude Opus 5.5 on September 22. Claude Sonnet 5.5 on September 28. A Claude Sonnet 4.5 deprecation on September 30. That is four lineup changes in a single month, all documented in the platform release notes.

At that pace, hard-coding a model string, tuning prompts tightly to one model's quirks, and treating a provider choice as a one-time architectural decision all age badly. Build the swap path in early and the cadence starts working in your favor.

For the frontier tier, see Claude Opus 5.5 vs Opus 5. For the argument one level up, see why your AI stack should be model-agnostic. If you are picking a model for content work specifically, we wrote up how we choose one.

## Run content operations on whichever model wins this month

Cosmic is a headless CMS with AI built in, so you can generate, edit and publish content against a current model without maintaining your own provider integration.

Create a free account, review plans and pricing, or book time with our CEO to talk through your setup.
