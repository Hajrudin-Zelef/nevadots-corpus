---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-vs-opus-5-benchmarks-and-cost
title: "Claude Opus 5.5 vs Opus 5: Benchmarks and Cost"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "anthropic", "claude", "models", "output", "haiku", "sonnet", "tokens"]
source: https://www.cosmicjs.com/blog/claude-opus-5-5-vs-opus-5
source_anchor: ""
source_lines: [1, 186]
section: "Claude_Anthropic"
---
Anthropic released Claude Opus 5.5 on September 22, 2026. The short version: it costs less than Opus 5 and scores higher than Opus 5 on every benchmark Anthropic published for both models.

That combination is unusual. Most frontier releases ask you to trade cost for capability. This one moves both in the same direction, which makes the decision simpler than the last few launches did.

Benchmark figures below were read from Anthropic's launch announcement on September 22, 2026, the day of release. Prices were re-read from Anthropic's pricing page on September 25, 2026.

## The short answer

| If you are... | Use | Why | 
|---|---|---|
| Already running Opus 5 in production | **Opus 5.5** | Lower list price, higher score on every published benchmark, faster output. | 
| Running Sonnet 5 by default | **Keep Sonnet 5** | At $2 and $10 per million tokens it is still half the price of Opus 5.5. Escalate selectively. | 
| Running long agentic or terminal work | **Opus 5.5** | The largest published gaps are on agentic and terminal benchmarks, not on chat. | 
| Cost-bound on very high volume | **Haiku 5.5** | Now listed in Anthropic's models overview at from $0.10 and $0.50 per million tokens, read October 7, 2026. It supersedes the Haiku 4.5 recommendation this table originally carried. | 

**You will make this same decision again in a few weeks.** In the three weeks since this post went up, Anthropic has shipped both Claude Sonnet 5.5 and Claude Haiku 5.5. If `claude-opus-5-5` is a string literal in your application code, every one of those launches becomes a pull request, a review, and a deploy before anyone can test the new model.

Cosmic lets you store the model ID as content and read it at request time, so changing models is an edit in a dashboard instead of a release. The working pattern is further down, under Keep model config out of your source code.

The Cosmic Free plan is $0 per month with no credit card, and includes 300k input and 300k output AI tokens per month, so you can point generation at Opus 5.5 and compare it against Opus 5 on your own content. Start free.


## What Anthropic shipped

The model string is `claude-opus-5-5`. It is available through the Anthropic API and on AWS, Google Cloud, and Microsoft Azure.

Two behavioral notes worth knowing before you swap the string:

- Extended thinking is always on for this model. It cannot be disabled, so plan for thinking tokens in your output budget.
- Anthropic says Opus 5.5 produces output more than 30% faster than Opus 5, and that it costs around 40% less to run for the same work once speed and pricing are combined.

Both models Anthropic trailed alongside this release have now shipped. Claude Sonnet 5.5 landed on September 28, 2026, and Claude Haiku 5.5 appears in the Claude models overview read on October 7, 2026, at from $0.10 per million input tokens and from $0.50 per million output tokens. That is four frontier models from one vendor inside about three weeks, which is the real planning constraint rather than any single benchmark gap.

## Pricing, side by side

List prices per million tokens, re-read from Anthropic's pricing page on September 25, 2026.

|  | Input | Output | Cache read | Cache write | 
|---|---|---|---|---|
| **Opus 5.5** | $4 | $20 | $0.20 | $5 | 
| **Opus 5** (now legacy) | $5 | $25 | $0.50 | $6.25 | 
| Sonnet 5.5 | $2 | $10 | – | – | 
| Sonnet 5 (now legacy) | $2 | $10 | $0.20 | $2.50 | 
| Haiku 5.5 | from $0.10 | from $0.50 | – | – | 
| Haiku 4.5 (now legacy) | $1 | $5 | $0.10 | $1.25 | 
| Fable 5.1 | $10 | $50 | $0.25 | $12.50 | 

Anthropic also lists a fast mode for Opus 5.5 at 2x standard pricing, returning output up to 2.5x faster.

Sonnet 5.5 and Haiku 5.5 input and output rates were read from the Claude models overview on October 14, 2026, which no longer lists Opus 5 or Haiku 4.5 among its current models. Cache rates for the two 5.5 models are not published on that page, so they are left blank here rather than carried over from an older model.

### The cost math

Take a workload of 10 million input tokens and 2 million output tokens in a month, which is a realistic monthly footprint for a content pipeline doing summarization, tagging, and draft review.

- **Opus 5** : $50 input plus $50 output =**$100**
- **Opus 5.5** : $40 input plus $40 output =**$80**

A flat 20% saving before caching. The cache read rate is where it widens: $0.20 against $0.50 per million is 60% cheaper, so anything that replays a large system prompt or a big content payload on every call saves considerably more than 20%.

One caveat that cuts the other way. Thinking cannot be disabled on Opus 5.5, and thinking tokens are billed as output. Measure billed tokens on your own workload before you assume the full 20%.

## Benchmarks

Anthropic's published scores for the three models most teams are choosing between. Read from the launch announcement on September 22, 2026.

| Benchmark | Opus 5.5 | Fable 5.1 | Opus 5 | 
|---|---|---|---|
| Terminal-Bench 4.0 | **66.4%** | 55.8% | 52.3% | 
| CursorBench 4.0 | **57.8%** | 51.8% | 46.6% | 
| FrontierCode v1.1 (Main) | **54.4%** | 50.3% | 48.0% | 
| GDPval-AA v2.1 (Elo) | **1846** | 1735 | 1708 | 
| AutomationBench | **40.0%** | 31.4% | 26.9% | 
| Terminal-Bench-Science 0.1 | **58.7%** | 52.6% | 29.0% | 
| OSWorld 2.0 | **81.8%** | 80.7% | 74.0% | 
| Humanity's Last Exam | **67.7%** | 65.6% | 63.6% | 

The pattern is consistent: the widest margins are on agentic and terminal work, and the narrowest are on knowledge benchmarks. Terminal-Bench-Science moves from 29.0% to 58.7%, which is the single largest jump in the table. Chat-shaped evaluations move by a few points.

### Read those numbers with care

Anthropic itself notes in the announcement that benchmark margins have become a less reliable guide to real-world quality, and that the gap between Opus 5.5 and Fable 5.1 is narrower in practice than the scores suggest. That is an unusual thing for a vendor to publish about its own launch, and it is worth taking at face value.

The practical read: a 10 point gap on an agentic benchmark is a reason to test, not a guarantee your workload improves. Run your own evaluation on your own tasks.

## What changes in production

For most teams the migration is a one-line change from `claude-opus-5` to `claude-opus-5-5`. The things that actually need attention:

1. **Output token budgets.** Always-on thinking means your output token count per request will likely rise. If you have hard caps or per-request cost alarms, recalibrate them before you switch.
2. **Latency assumptions.** Faster output changes timeout tuning and any streaming UI that was built around Opus 5's pacing.
3. **Prompts tuned to Opus 5.** Prompts that were heavily shaped around one model's quirks rarely transfer cleanly. Re-run your evaluation set rather than assuming parity.
4. **Opus 5 is now legacy.** It still runs at its original rate, so nothing breaks today, but legacy status is a signal about where the roadmap is going.

## The control question that comes with agentic work

The widest published margins for Opus 5.5 are on agentic and terminal benchmarks, which is also where most teams will actually point it: long tool-calling runs that read and write real systems. Once your content is one of those systems, the scope of the credential you hand the agent matters as much as the model you picked. A model that is better at long tool-calling runs will also execute the wrong write faster if you give it a key that allows one.

Cosmic issues separate read and write keys per bucket. An agent connected with a read-only key can research, summarize, and audit your content, and is blocked at the API boundary on every create, update, and delete, so the refusal does not depend on the model behaving well. The four controls worth checking before you give any model write access, plus the two Cosmic does not ship, are in AI agent write access: the 4 real controls you get with your CMS. To point Claude at your content directly, the Cosmic MCP server exposes 18 bucket-scoped tools over a hosted endpoint, and dropping the write key removes every write tool from that surface.

Start free and connect a client to your own bucket. The Free plan is $0 per month with 1 Bucket, 2 team members, and 1,000 Objects, and no credit card.

## Using Opus 5.5 in Cosmic

Opus 5.5 is available in Cosmic, so you can point content generation at it without standing up a separate integration. Pass `claude-opus-5-5` as the model to `cosmic.ai.generateText()`, and generation runs through the same REST API and TypeScript SDK as the rest of your content:

```
import { createBucketClient } from '@cosmicjs/sdk'
const cosmic = createBucketClient({
  bucketSlug: 'BUCKET_SLUG',
  readKey: 'BUCKET_READ_KEY',
  writeKey: 'BUCKET_WRITE_KEY'
})
const response = await cosmic.ai.generateText({
  prompt: 'Write a launch announcement for our September release',
  model: 'claude-opus-5-5',
  max_tokens: 1000
})
console.log(response.text)
```

Cosmic's model list describes Opus 5.5 as the recommended Opus for long-running agentic coding and knowledge work, with adaptive thinking always on. That matches Anthropic's own note that thinking cannot be disabled on this model, so plan for reasoning tokens on every call rather than treating them as opt-in.

One thing to plan for on any frontier model: Cosmic bills AI text generation in tokens drawn from your plan allocation, and the deduction is multiplied by the model's tier, so a frontier model draws down faster than a budget one for the same number of tokens. The current model IDs and tier multipliers are listed in the AI API reference. We covered the rollout itself the day the model landed, including how to select it inside the dashboard: Claude Opus 5.5 is now available in Cosmic.

## Keep model config out of your source code

Anthropic shipped four frontier models in about three weeks: Fable 5.1, Opus 5.5, Sonnet 5.5 on September 28, and Haiku 5.5, which is listed in the models overview read on October 7, 2026. If the model name is a string literal in your application code, every one of those releases becomes a pull request, a review, and a deploy.

A better shape: store the model identifier as content, read it at runtime, and change it from a dashboard. If that routing config is read by a React or Next.js front end, the shape of the data fetch and the caching decision that follows it (static generation, timed revalidation, or per-request rendering) are covered in how to choose the best headless CMS for Next.js and React.

```
import { createBucketClient } from '@cosmicjs/sdk';
const cosmic = createBucketClient({
  bucketSlug: process.env.COSMIC_BUCKET_SLUG!,
  readKey: process.env.COSMIC_READ_KEY!,
});
// Model routing lives in Cosmic, not in this file.
const { object: config } = await cosmic.objects
  .findOne({ type: 'ai-settings', slug: 'model-routing' })
  .props(['metadata']);
const model =
  task.needsJudgment
    ? config.metadata.escalation_model // 'claude-opus-5-5'
    : config.metadata.default_model;   // 'claude-sonnet-5'
```

When Sonnet 5.5 landed on September 28, a team running this pattern changed one field and was live on the next request. The teams that hardcoded the string are still queuing a deploy. No engineering ticket, no waiting for a release window.

That same read works anywhere your application renders. If you are wiring it into Next.js, using Cosmic as a React and Next.js CMS covers fetching config like this with the TypeScript SDK from a server component. If you have not settled on a content layer yet, the best headless CMS for React and Next.js compares the options on API surface, pricing, and editor experience.

This is how Cosmic's AI features are built, and it is the same pattern behind the Cosmic MCP server, which lets Claude read and write your content through scoped keys rather than through your application code.

The question most teams hit immediately after that one: once Claude can write to your content, what stops it writing the wrong thing? That is a permissions and review question, and it is worth settling before you hand any agent a write key. We wrote up the real controls you get over agent write access, and how to scope them so an agent's work arrives as something you approve rather than something you discover.

You can set this up on the Free plan and point it at whichever Opus you landed on above. Create a free Bucket, then connect Claude to it through the MCP server and let it read and write real content behind scoped keys.

## FAQ

**Is Opus 5.5 cheaper than Opus 5?**

Yes. $4 per million input tokens and $20 per million output tokens, against $5 and $25 for Opus 5. Cache reads are $0.20 against $0.50. Re-read from Anthropic's pricing page on September 25, 2026.

**Is Opus 5 being retired?**

Anthropic now lists Opus 5 among its legacy models, still available at its original rate. No shutdown date was announced in the release.

**Should I move from Sonnet 5 to Opus 5.5?**

Not by default. Sonnet 5 is $2 and $10 per million tokens, half the Opus 5.5 rate. The case for escalating is judgment-heavy and long-horizon agentic work, which is where the published margins are widest.

**Can I turn off extended thinking on Opus 5.5?**

No. Anthropic states that thinking cannot be disabled on this model.

**What about Sonnet 5.5 and Haiku 5.5?**

Claude Sonnet 5.5 shipped on September 28, 2026 at $2 per million input tokens and $10 per million output tokens, with a 1M context window, read from the Claude models overview on October 6, 2026. It also breaks Sonnet 5 code in five documented ways, covered in Claude Sonnet 5.5 vs Sonnet 5. Claude Haiku 5.5 is now listed in the Claude models overview, read October 7, 2026, at from $0.10 per million input tokens and from $0.50 per million output tokens, with a 1M context window. Both models trailed alongside Opus 5.5 have now shipped. One further difference worth noting if you are routing between tiers: the overview lists thinking as adaptive and always on for Opus 5.5 and Fable 5.1, but simply adaptive for Sonnet 5.5 and Haiku 5.5, so the two newer tiers accept thinking being turned off and Opus 5.5 does not.

## Build on infrastructure that outlives the model

The model tier you pick today has a shelf life measured in weeks. The content layer underneath it does not.

Cosmic is an AI-powered headless CMS with a REST API and a TypeScript SDK, so your content, your prompts, and your model configuration can all be edited by the people who own them rather than shipped through a deploy pipeline. See how the AI features work, or read the model comparison this post extends.

Start free on the Free plan: 1 Bucket, 2 team members, 1,000 Objects, no credit card required. If you want to talk through a specific setup, book time with our CEO.
