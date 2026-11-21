---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-pricing-api-rates-plans-and-costs
title: "Claude Opus 5.5 Pricing: API Rates, Plans, and Costs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["claude", "opus", "tokens", "million", "percent", "input", "pricing", "output"]
source: https://www.layer3labs.io/guides/claude-opus-5-5-pricing
source_anchor: ""
source_lines: [1, 84]
section: "Claude_Anthropic"
---
# Claude Opus 5.5 Pricing: API Token Rates, Plan Costs, and Workload Budgets

A complete breakdown of Claude Opus 5.5 rates, prompt caching discounts, and monthly operating budgets.

On September 22, 2026, Anthropic announced Claude Opus 5.5, introducing a new Claude Opus 5.5 pricing schedule for enterprise software development, scientific research, and autonomous agentic workflows. Claude Opus 5.5 costs $4 per million input tokens, $20 per million output tokens, and $0.20 per million cached-input tokens through the Anthropic Application Programming Interface (API).

This pricing represents a 20 percent discount on input tokens and output tokens compared to Claude Opus 5, alongside a 60 percent reduction in prompt cache read costs. Anthropic reports that these rate reductions lower total operating expenses by 40 percent on typical production workloads. The model also generates text more than 30 percent faster than its direct predecessor.

Claude Opus 5.5 arrives as the first entry in the Claude 5.5 model family, matching the output capability of Claude Fable 5.1 while lowering running expenses. Development teams budgeting for production deployments must balance base token rates, prompt caching efficiency, and verification requirements across sensitive workflows.

## The Direct Answer on Claude Opus 5.5 Pricing and Rate Card

Anthropic sets the base Claude Opus 5.5 pricing at $4 per million input tokens, $20 per million output tokens, and $0.20 per million prompt-cache read tokens. These rates apply directly to pay-as-you-go usage on the Anthropic console and supported cloud infrastructure partners including Amazon Web Services and Google Cloud.

The new rate structure lowers token costs compared to Claude Opus 5. Claude Opus 5 launched on July 24, 2026, at $5 per million input tokens and $25 per million output tokens. That change delivers an immediate 20 percent baseline reduction on raw input and generation expenses.

Beyond raw token reductions, Anthropic reports a 40 percent overall cost decline for typical production workloads. This cumulative saving combines lower base rates with faster token generation speeds and cheaper prompt caching.

- Base input tokens: $4.00 per million tokens (20 percent discount versus Claude Opus 5)
- Base output tokens: $20.00 per million tokens (20 percent discount versus Claude Opus 5)
- Cached input tokens: $0.20 per million tokens (60 percent discount versus Claude Opus 5)
- Generation speed: More than 30 percent faster text generation compared to Claude Opus 5

## Claude Opus 5.5 Pricing Compared to Opus 5 and Sibling Models

Claude Opus 5.5 provides lower per-token pricing than both Claude Opus 5 and the security-focused Claude Mythos 5.1 across standard and cached operations. Organizations evaluating model migration can assess where each system sits in Anthropic's overall product lineup.

Claude Mythos 5.1 launched on September 1, 2026, with rates detailed in the Claude Mythos 5.1 pricing guide. Mythos 5.1 costs $10 per million input tokens, $50 per million output tokens, and $0.25 per million cached-input tokens. Mythos 5.1 carries specialized safeguards for cybersecurity and life sciences within trusted access cohorts. Claude Opus 5.5 delivers higher benchmark performance on evaluations like Terminal-Bench 4.0 (66.4 percent versus 55.8 percent on Fable 5.1) at less than half the per-token cost of Mythos 5.1.

Against Claude Opus 5, Opus 5.5 cuts per-token costs by 20 percent on base traffic while outperforming the older model on CursorBench 4.0 (57.8 percent versus 46.6 percent). Teams reviewing the Claude Opus 5 alternatives comparison can treat Opus 5.5 as the direct replacement for high-demand reasoning tasks.

- Claude Opus 5.5: $4.00 input, $20.00 output, $0.20 cached input per million tokens
- Claude Opus 5: $5.00 input, $25.00 output per million tokens
- Claude Mythos 5.1: $10.00 input, $50.00 output, $0.25 cached input per million tokens
- Claude Fable 5.1: $10.00 input, $50.00 output, $0.25 cached input per million tokens

## Impact of Prompt Caching on Real Operating Expenses

Prompt caching on Claude Opus 5.5 cuts context read expenses to $0.20 per million tokens, representing a 60 percent reduction compared to Opus 5. This price point alters the economics of software agents, code workspace analysis, and large document ingestion.

In multi-turn agentic workflows, an autonomous agent reads the same system prompt, tool definitions, and conversation history on every execution cycle. Without caching, an agent executing twenty consecutive tool calls re-submits its entire context at full input price on every turn. With prompt caching, Anthropic bills the initial write once, and subsequent turns draw from the cache at $0.20 per million tokens.

Pre-release safety and threat evaluations by METR (Model Evaluation and Threat Research) and Frontier Design evaluated Claude Opus 5.5 across complex computer tasks like OSWorld 2.0 (81.8 percent task completion). For workflows that maintain heavy system context, prompt cache discounts generate the largest portion of the reported 40 percent net savings.

## Subscription Tier Terms Versus Direct Claude Opus 5.5 Pricing on API Surfaces

Anthropic separates Claude Opus 5.5 access into developer API billing and user subscription plans across Pro, Max, Team, and Enterprise accounts. Developers building custom applications pay through the Anthropic console on the per-token schedule, while interactive chat users access the model through monthly software subscriptions.

Paying subscription tiers receive higher five-hour rolling message caps when using Claude Opus 5.5 compared to earlier models. Anthropic also introduced an on-demand, bankable rate-limit reset that account holders can store and activate during peak work periods to avoid workflow interruptions.

Anthropic has not published a specific numeric context-window token limit, per-minute request limit, or daily file upload cap for Claude Opus 5.5 in the launch materials. By comparison, Claude Opus 5 offered a documented 1 million token context window. Operators should verify active account limits directly on Anthropic's pricing page before scheduling bulk data ingestion.

## Sample Monthly Budgets for Production Workflows

Production teams running Claude Opus 5.5 can estimate monthly expenses by separating fresh inputs, cached context tokens, and output generations. The interaction between cached context and output length dictates total monthly billing across different operational shapes.

Consider an automated code review bot processing 500 pull requests per month. Each review uses an 80,000-token repository context that stays cached across four follow-up questions. Across a month, that generates 50 million cached input tokens, 2 million fresh input tokens, and 3 million output tokens. The resulting bill comes to $10 for cached tokens, $8 for fresh inputs, and $60 for completions, totaling $78 per month.

A heavier document workflow might ingest 20 million fresh input tokens and generate 5 million output tokens without caching. That volume incurs $80 in input costs and $100 in output costs, totaling $180 per month. That same volume on Claude Opus 5 would have cost $225, demonstrating the baseline 20 percent savings before factoring in cache utilization.

## Workloads That Should Not Default to Claude Opus 5.5

High-volume single-turn tasks, simple text routing, and basic classification should not run on Claude Opus 5.5 due to unnecessary token expenditure. While $4 per million input tokens is competitive for frontier reasoning, high-throughput extraction workflows accumulate unnecessary costs when deployed on top-tier models.

Workflows that require simple data formatting or standard customer support inquiries can run on smaller models or forthcoming releases like Claude Sonnet 5.5 and Claude Haiku 5.5 once they ship. Deploying Claude Opus 5.5 for lightweight work spends compute budget on unnecessary reasoning capacity.

Compliance-restricted workflows in biology and cybersecurity also face access boundaries. Claude Opus 5.5 requires enrollment in the Life Sciences Verification Program for biological research and the Cyber Verification Program for network infrastructure tasks. Tasks triggering automated safety boundaries automatically fall back to Claude Opus 4.8 for cybersecurity and Claude Opus 5 for biology.

## Conditions That Would Shift Model Selection and Budget Planning

Changes in per-token billing, the release of lower-cost tier models, or altered verification rules would shift deployment decisions away from Claude Opus 5.5. Organizations should track several operational triggers when planning long-term model investments.

If Anthropic releases Claude Sonnet 5.5 with benchmark performance matching Opus 5.5 on coding evaluations at lower rates, mid-tier workloads should migrate immediately. Similarly, competitive pricing maneuvers on flagship models from OpenAI, such as GPT-6 Astra, could alter cost-to-performance rankings for complex enterprise automation.

Anthropic updates token rates, cloud availability terms, and tier limits over time. Operators must verify current pricing directly on Anthropic's official pricing page before committing operational budgets. Before starting your deployment, audit your expected token volume and test small prompt batches to calculate your exact Claude Opus 5.5 pricing requirements.

## Frequently Asked Questions

- Claude Opus 5.5 costs $4.00 per million input tokens, $20.00 per million output tokens, and $0.20 per million cached-input tokens on the Anthropic API.
- Claude Opus 5.5 is 20 percent cheaper on input tokens ($4 versus $5) and output tokens ($20 versus $25) compared to Claude Opus 5. Prompt cache reads are 60 percent cheaper, driving an estimated 40 percent total workload cost reduction.
- Claude Opus 5.5 costs 60 percent less than Claude Mythos 5.1, which is billed at $10.00 per million input tokens, $50.00 per million output tokens, and $0.25 per million cached-input tokens under restricted access programs.
- Yes, Claude Opus 5.5 supports prompt caching at $0.20 per million tokens read from cache. This rate provides substantial savings for repetitive agentic loops, large code repositories, and multi-turn chat sessions.
- Yes, subscription plans like Pro, Max, Team, and Enterprise charge fixed monthly per-seat fees with rolling message caps, whereas developer API usage is billed strictly per token consumed.
- Claude Opus 5.5 provides high value for small teams running complex software engineering, difficult analytical tasks, and agentic workflows, but teams handling basic content generation should wait for lower-cost models like Claude Sonnet 5.5.
