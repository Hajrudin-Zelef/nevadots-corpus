---
id: collect-261121-veille-ia/claude-anthropic/cut-claude-code-costs-with-claude-haiku-5-5
title: "Cut Claude Code Costs With Claude Haiku 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["model", "haiku", "claude", "anthropic", "opus", "code", "subagent", "agent"]
source: https://buildwire.ai/blog/claude-haiku-5-5-subagents/
source_anchor: ""
source_lines: [1, 189]
section: "Claude_Anthropic"
---
Claude Haiku 5.5 bills at a fortieth of Opus 5.5's list price. A marketing subagent may still run at the parent session's rate when no model override is set. One agent file fixes that for the high-volume jobs, such as scoring signals, compressing briefs, and tagging call notes.

**Key takeaway:** **Model routing** means sending each agent job to the cheapest model that still does it well. In Claude Code, a custom subagent with no model override normally inherits the main conversation's model, so a team running Opus 5.5 may pay Opus rates for narrow jobs. Add `model: claude-haiku-5-5` to the agent file for those jobs, keep each prompt under 100,000 tokens, and check `/tasks` to confirm the run landed on Haiku. Judge the switch by cost per accepted output on 50 to 200 past records, not by the price sheet alone.


## What did Anthropic release with Claude Haiku 5.5?

**Claude Haiku 5.5** is Anthropic's smallest and cheapest model in the 5.5 generation, built for fast, high-volume work. Anthropic launched Claude Haiku 5.5 on October 7, 2026. The launch post calls it "the cheapest, fastest, and most capable small model we've ever released." The model ID is `claude-haiku-5-5`. It is the first Haiku-class model with an adjustable effort setting.

Anthropic's launch post includes one result from a marketing system. Ze'ev Klapow, a Distinguished Software Engineer at HubSpot, said Haiku 5.5 "got the best score we've seen on this suite yet, at 92.8% averaged over three runs" on HubSpot's CRM evaluation tasks (Anthropic, 2026). On an audit task that asks models to find stale but ambiguous records, he said it was the fastest model and had the highest hit rate and the lowest false positive rate.

## How much does Claude Haiku 5.5 cost compared with Sonnet and Opus?

Haiku 5.5 has two price tiers, set by prompt length. Anthropic lists Sonnet 5.5 at $2 input and $10 output per million tokens, and Opus 5.5 at $4 and $20, on its models overview.

| Model, per 1M tokens | Input | Output | Opus 5.5 costs this many times more | 
|---|---|---|---|
| Haiku 5.5, prompts up to 100,000 tokens | $0.10 | $0.50 | 40x | 
| Haiku 5.5, prompts over 100,000 tokens | $0.50 | $2.50 | 8x | 
| Sonnet 5.5 | $2.00 | $10.00 | 2x | 
| Opus 5.5 | $4.00 | $20.00 | 1x | 

*List prices as published by Anthropic on October 14, 2026. The ratios are our arithmetic from those prices.*

*Input price per million tokens, from Anthropic's published list prices.*

Anthropic says Haiku 5.5 costs "around 75% less to run" than Haiku 4.5 on average (Anthropic, 2026). Two details on the same page decide whether a marketing team sees that saving. Both are covered in the cost-traps section below.

## Which model do Claude Code subagents use by default?

**A Claude Code subagent** is a separate agent that the main session hands a task to, with its own instructions, tools and context window. A custom subagent normally inherits the main conversation's model when no override is set. A model passed for an individual run takes precedence over its agent file, followed by `CLAUDE_CODE_SUBAGENT_MODEL`, then the parent model. The Claude Code subagents documentation lists `model` as an optional field that accepts aliases such as `haiku` or full model IDs, and omitting it means the subagent inherits the parent's model.

The built-in Explore agent also normally uses the main conversation's model. The current documentation describes an exception for Fable sessions: Explore uses the Opus alias with a Claude subscription, Anthropic Console account or an LLM gateway reached through `ANTHROPIC_BASE_URL`. On the other listed providers, Explore stays on the main model. A team that set up its agents before that change may have expected search passes to run on Haiku. Those passes may now bill at the parent model's rate.

One developer measured how often this happens. In a DEV Community post on October 12, they counted 2,631 subagent runs in their own logs and found only 42 on Haiku. The count comes from a single developer's logs, and it matches what the documentation predicts for anyone who never wrote a model line.

For a marketing team, the pattern looks like this. Opus 5.5 plans the week's campaign work in the main chat. A subagent scores 300 inbound signals, another compresses four long briefs, and a third tags last week's call notes. With no model line in their agent files, all three bill as Opus.

## How do you route a Claude Code subagent to Haiku 5.5?

You route a subagent to Haiku by writing the model into its agent file. Project agents live in `.claude/agents/` and personal agents in `~/.claude/agents/`. Each is a markdown file with YAML frontmatter.

### Step 1: Pin one high-volume job in an agent file

Start with the narrow job your team runs most. Here is an example for signal scoring. Adapt the name, tools and instructions to your own stack.

```
---
name: signal-scoring-runner
description: Scores inbound and account signals against our written rubric. Returns a fit band, the top signal, and a suppression flag. Read-only.
model: claude-haiku-5-5
effort: low
tools: Read, Glob, Grep
---
Use the Signal Scoring Skill and the thresholds in our pinned playbook.
Return a short table: account, band, top signal, suppress (yes or no).
Do not write to the CRM.
```
Save it as `.claude/agents/signal-scoring-runner.md`. The `effort` field is supported in subagent frontmatter, and Haiku 5.5 is the first Haiku model that accepts it, so low effort is a sensible default for a job with a fixed rubric.

The full model ID `claude-haiku-5-5` makes the intended version explicit. Check the ID supported by your provider and confirm the actual model in `/tasks`, since aliases and account restrictions can affect the model that runs.

### Step 2: Confirm the model in /tasks

Run the job, then type `/tasks`. The documentation says `/tasks` shows the subagent's model and any effort level set for it. The command needs Claude Code v2.1.242 or later. Treat the `/tasks` row as the record of what ran, since malformed frontmatter can cause an agent file to be skipped, while account model restrictions can cause a model substitution.

### Step 3: Use the settings override only for the whole worker pool

Claude Code also reads two environment variables, set in the `env` block of a settings file:

```
{
  "env": {
    "CLAUDE_CODE_SUBAGENT_MODEL": "claude-haiku-5-5",
    "CLAUDE_CODE_SUBAGENT_MODEL_FORCE": "1"
  }
}
```
`CLAUDE_CODE_SUBAGENT_MODEL` on its own does not move the built-in Explore and Plan agents. The docs say that to apply one model "to every subagent, teammate, and workflow agent," you also set `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` to 1, which needs v2.1.257 or later. That setting also moves any agent you deliberately kept on Opus or Sonnet for strategy work. It is worth considering only when the worker pool should run on Haiku. Forks and Skills with `model: inherit` remain on the main conversation's model, as the documentation notes. For one or two jobs, the agent file is safer.

## Which marketing agent jobs should run on Haiku 5.5?

Narrow, repeatable jobs with a clear pass test belong on Haiku. Jobs where a wrong answer costs pipeline or positioning stay on Sonnet 5.5 or Opus 5.5 until your own test says otherwise.

| Job | Model | BuildWire Skill | Why | 
|---|---|---|---|
| Score and rank leads on intent signals | Haiku 5.5 | Signal Scoring | Fixed rubric, high volume | 
| Pull objections and win-loss reasons from call transcripts | Haiku 5.5 | Sales Intelligence | Extraction and tagging | 
| Turn a long brief into a one-page summary | Haiku 5.5 | Brief Compressor | Format job with an easy pass test | 
| Flag metrics that move away from their baseline | Haiku 5.5 | Data Anomaly Detective | Pattern check against history | 
| Build a GTM playbook from your site and offer | Sonnet or Opus | GTM Playbook Builder | Judgment across messy context | 
| Define the ideal customer profile and personas | Sonnet or Opus | ICP Research | A wrong answer misroutes every later job | 
| Find gaps in messaging against a competitor | Sonnet or Opus | Positioning Gap Finder | Strategy, with no single right answer | 

Academic work on model routing supports the split. Stanford researchers behind FrugalGPT sent each query to a chain of cheaper and larger models and matched the best single model's accuracy with cost savings of up to 98% on their tested datasets (Chen, Zaharia and Zou, 2023). A UC Berkeley team's RouteLLM router cut costs by more than half on public benchmarks without lowering response quality (Ong et al., 2024). Both studies routed between general chat models, not Claude Code subagents, so treat them as evidence that routing can reduce cost on tested workloads. Your own test is the forecast for your bill.

The table is a starting point. A job moves to Haiku when it passes the test in the meter section below.

## What are the cost traps with Haiku 5.5?

Two details on Anthropic's launch page cut into the saving when a team misses them.

**The 100,000-token price step.** Anthropic prices "prompts up to 100,000 tokens" at $0.10 input and $0.50 output per million, and longer prompts at $0.50 and $2.50. On the input side, a 95,000-token prompt costs about $0.0095 and a 105,000-token prompt costs about $0.0525, so about a tenth more context costs five and a half times as much. Anthropic says 90% of Haiku 4.5 requests stayed under the line (Anthropic, 2026). Marketing agents cross it when someone pastes a full CRM export or a month of call notes into one run. Cap the rows each runner loads, and point it at a short rubric file instead of the raw export, so the prompt stays comfortably below the threshold.

**The new tokenizer.** Anthropic's footnote says Haiku 5.5 "has an updated tokenizer (similar to Sonnet 5.5's and Opus 5.5's), which means it uses slightly more tokens per task." Token counts from Haiku 4.5 invoices will not carry over one to one. Compare cost per finished result, not raw token totals.

## How do you measure whether Haiku 5.5 saves money?

Measure cost per accepted output on your own records before you switch a job in production. Changing the model is a change to a live system, so test it like one.

1. Pull 50 to 200 past records for one job, such as signal scores, reply-intent tags or call summaries.
2. Run the current model and Haiku 5.5 on the same prompt and Skill.
3. Accept or reject each output with a rule your team already trusts, or a human spot check.
4. Count rewrites. A rejected output that needs a second run still costs tokens.
5. Compute: **cost per accepted output = total list cost of all runs and rewrites ÷ number of accepted outputs.**

Review the disagreements first, because the records where the two models agree tell you little. Keep the live path on the current model and log both answers to a sheet while you test.

| Record set | Model | Runs | Accepted | List cost | Cost per accepted output | 
|---|---|---|---|---|---|
| 100 signal rows | Opus 5.5 | 100 | X | $A | $A ÷ X | 
| 100 signal rows | Haiku 5.5 | 100 plus rewrites | Y | $B | $B ÷ Y | 

*Example layout only. Fill in X, Y, A and B from your own test. These are not BuildWire results.*

A quick sense of scale from list prices alone: 1,000 scoring runs at 8,000 input tokens and 1,000 output tokens each cost about $52 on Opus 5.5 and about $1.30 on Haiku 5.5. Even with a rewrite on every tenth run, Haiku stays far cheaper on list price. A cheaper model that misroutes good leads can still cost more than it saves, which is why the acceptance count decides.

Ship the Haiku path when cost per accepted output drops and the miss rate stays inside what that job can tolerate.

## Where does model routing fit in a marketing agent stack?

Routing is one layer of a stack, and it assumes earlier layers are in place:

- **You own and trust the Skill.** Vet a third-party Skill before you choose its model. The four checks are in how to vet Claude Skills from a marketplace.
- **Live data stays read-only.** Skills that touch Google Ads, Search Console or GA4 still need the connector setup in Claude marketing Skills with live data. A cheaper model does not change what a connector may write.
- **The job belongs in an agent.** A scheduled pipeline with branching and retries may fit a workflow tool better, as covered in n8n vs Claude Code vs Claude Skills for marketing ops.
- **Fit rules come first.** Haiku applies the no-list it is given. Write that list before you route scoring to a small model. The negative ICP guide for AI SDR agents shows how.

If you run Anthropic's marketing plugin, the same routing applies to the agents it installs. Setup is in the Claude marketing plugin guide for B2B teams.

## How do you start this week?

1. List every marketing job that runs as a Claude Code subagent.
2. Mark each one Haiku tier or judgment tier, using the table above.
3. Add `model: claude-haiku-5-5` to the agent file for the highest-volume Haiku-tier job.
4. Run it once and confirm the model in `/tasks` .
5. Keep each prompt under 100,000 tokens by capping rows and using a short rubric file.
6. Test 50 to 200 past records and compare cost per accepted output, rewrites included.
7. Leave the FORCE setting off unless every subagent should move.

Haiku 5.5 lowers the bill only for jobs that call it. A good first pin is Signal Scoring or Brief Compressor. Where smaller models fit best will depend on the job. Curious which repeatable jobs your team is trying first?

Keep GTM Playbook Builder and ICP Research on the larger model until your test shows Haiku can match them.

## How we checked this guide

We read Anthropic's Haiku 5.5 launch page, the FrugalGPT and RouteLLM papers on arXiv, the models overview and the Claude Code subagents and settings documentation on October 14, 2026, and took every price, version number and quote from those pages. The 2,631-run count comes from one developer's DEV Community post and is labeled as such. The cost ratios and the 1,000-run estimate are our arithmetic from list prices. The test-log table is an empty example layout, not a BuildWire result. We checked the pricing and routing instructions against current documentation before publication. The marketing job recommendations are starting points for testing, not measured BuildWire results. More about who writes these guides is on our about page.

BuildWire builds AI agents and automations for marketing and growth teams. You can see what we build on the services page. If you want help deciding which of your agent jobs can move to Haiku, book a call. If a call is too much for now, the contact form takes one line on what you run.

*Want the Claude and AI workflows we run every week, written up like this one? Subscribe to The AI Driven Marketer newsletter, and follow along as we turn slow marketing jobs into short ones.*

## Frequently asked questions

## How much does Claude Haiku 5.5 cost?

Anthropic lists Claude Haiku 5.5 at $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens. Prompts over 100,000 tokens cost $0.50 input and $2.50 output per million. Anthropic says it costs around 75% less to run than Haiku 4.5 on average (Anthropic, 2026).

## Is Claude Haiku 5.5 cheaper than Opus 5.5?

Yes. At list prices, Opus 5.5 costs $4 input and $20 output per million tokens, which is 40 times Haiku 5.5's rate for prompts under 100,000 tokens. Above that length the gap narrows to 8 times, because Haiku's price rises to $0.50 input and $2.50 output.

## What model does a Claude Code subagent run on if no model is set?

A custom Claude Code subagent normally inherits the main conversation's model when no override is set. Per-invocation models, agent-file settings and environment settings can change that. Explore also normally follows the main model, with provider-specific exceptions for Fable sessions described in the current documentation.

## How do I make a Claude Code subagent use Haiku 5.5?

Add model: claude-haiku-5-5 to the YAML frontmatter of the subagent's file in .claude/agents/ or ~/.claude/agents/. Use the full model ID rather than the haiku alias. After a run, type /tasks to see which model the subagent used.

## What happens when a Haiku 5.5 prompt goes over 100,000 tokens?

Anthropic bills prompts over 100,000 tokens at the higher rate of $0.50 input and $2.50 output per million tokens, five times the base rate. On the input side, a 105,000-token prompt costs about five and a half times as much as a 95,000-token prompt. Cap the rows an agent loads to stay under the line.

## Is Claude Haiku 5.5 good enough for marketing tasks?

For narrow jobs with a clear pass test, often yes. HubSpot reported a score of 92.8% averaged over three runs on its CRM evaluation suite, the best it had seen on that suite (Anthropic, 2026). Test it on 50 to 200 of your own past records and compare cost per accepted output before you switch a job.

Last reviewed 14 October 2026. Pricing and model-routing documentation checked on this date. Workload recommendations and test tables are illustrative.
