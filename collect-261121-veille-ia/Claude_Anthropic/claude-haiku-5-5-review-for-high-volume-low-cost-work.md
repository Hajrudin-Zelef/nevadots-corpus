---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-review-for-high-volume-low-cost-work
title: "Claude Haiku 5.5 Review for High-Volume, Low-Cost Work"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["claude", "haiku", "percent", "sonnet", "anthropic", "scored", "high-volume", "work"]
source: https://www.layer3labs.io/guides/claude-haiku-5-5-review
source_anchor: ""
source_lines: [1, 107]
section: "Claude_Anthropic"
---
# Claude Haiku 5.5 Review for High-Volume Work

Anthropic's fastest small model trails Claude Sonnet 5.5 on every benchmark row but costs a twentieth as much per input token on prompts up to 100,000 tokens.

Claude Haiku 5.5 is a good pick for high-volume, latency-sensitive work such as classification, summaries, extraction and support. Anthropic released Claude Haiku 5.5 on October 7, 2026, and calls it the cheapest, fastest and most capable small model it has released. Claude Haiku 5.5's list price is 90 percent below Claude Haiku 4.5's for requests up to 100,000 tokens.

On October 7, 2026, Anthropic made Claude Haiku 5.5 available across the Claude Application Programming Interface (API), Amazon Web Services (AWS), Google Cloud, and Microsoft Azure. The release expands the context window from 200,000 tokens on Claude Haiku 4.5 to 1,000,000 tokens on Claude Haiku 5.5. Maximum output capacity reaches 128,000 tokens on standard requests. It is also the first Haiku model with an adjustable effort setting. One model can run quick chat at low effort and longer agent tasks at high effort.

Claude Haiku 5.5 beats Claude Haiku 4.5 on every row of Anthropic's table where both have a score. It trails Claude Sonnet 5.5 on every row. Sonnet is the pick for the hardest reasoning tasks.

## Claude Haiku 5.5 Review and Primary Verdict

Claude Haiku 5.5 is worth using for high-volume, latency-sensitive work such as classification, extraction and routing, which is how Anthropic's model docs describe it. Anthropic also aims it at summaries, compactions and database queries.

Teams managing high-volume document pipelines, automated customer service routing, data extraction, or auxiliary coding subagents can deploy Claude Haiku 5.5 for high-speed, lower-cost execution. Organizations requiring deep autonomous reasoning or complex terminal command execution should select Claude Sonnet 5.5 instead. On Terminal-Bench 4.0, Claude Sonnet 5.5 scored 70.6 percent against 39.2 percent for Claude Haiku 5.5. That is the widest gap in Anthropic's table.

Anthropic's announcement says Claude Haiku 5.5 pairs well with Claude Opus 5.5 and Claude Sonnet 5.5 as a subagent on coding work. Syndicated reporting from Reuters noted that the model is designed to power live support systems, voice agents, and in-app assistants.

The recommendation to adopt Claude Haiku 5.5 would change if a workload frequently exceeds 100,000 tokens per prompt. Above that line, input prices quintuple from $0.10 to $0.50 per million tokens. It would also change for an app that needs thinking off at xhigh or max effort, or inside Claude Code, since neither allows it.

## Reported Benchmark Scores and Customer Production Metrics

Claude Haiku 5.5's benchmark scores on Anthropic's table beat Claude Haiku 4.5 and GPT-6 Luna on every row where each has a score, and trail Claude Sonnet 5.5 on every row.

On the GDPval-AA version 2.1 evaluation, Claude Haiku 5.5 achieved an Elo score of 1620, compared to 735 for Claude Haiku 4.5, 1437 for GPT-6 Luna, and 1840 for Claude Sonnet 5.5. On AA-Briefcase version 1.1, Claude Haiku 5.5 recorded 1578. Claude Haiku 4.5 scored 614 and GPT-6 Luna 1336, while Claude Sonnet 5.5 led at 1824. In operating system automation on OSWorld version 2.1 offline subset, Claude Haiku 5.5 reached 72.4 percent. Claude Haiku 4.5 scored 15.7 percent. GPT-6 Luna scored 48.9 percent, whereas Claude Sonnet 5.5 scored 83.9 percent.

Anthropic's table does not say which effort level Claude Haiku 5.5 ran at. Its only effort label is Xhigh, beside Claude Sonnet 5.5's FrontierCode 1.1 score, and Anthropic points readers to the system card for evaluation details. On FrontierCode 1.1 Main, Claude Haiku 5.5 scored 46.4 percent. GPT-6 Luna scored 42.4 percent and Claude Sonnet 5.5 scored 52.1 percent. On Chartography without tools, Claude Haiku 5.5 achieved 46.4 percent, compared to 6.4 percent for Claude Haiku 4.5, 29.1 percent for GPT-6 Luna, and 61.6 percent for Claude Sonnet 5.5. On Humanity's Last Exam without tools, Claude Haiku 5.5 scored 45.9 percent. With tools, that rose to 57.4 percent.

Six companies quoted in Anthropic's announcement reported their own results. Aaron Vinh, Staff Software Engineer at Asana, reported over a 30 percent reduction in latency for task completions and up to 2.5 times faster inference per agent turn. Ze'ev Klapow, Distinguished Software Engineer at HubSpot, stated that Claude Haiku 5.5 produced a score of 92.8 percent averaged over three runs on their Customer Relationship Management (CRM) evaluation suite. Klapow called it the best score HubSpot had seen on that suite from a smaller model. Daniel Campos, Distinguished Engineer at AlphaSense, documented a statistically significant improvement over Claude Haiku 4.5 across 400 queries, lifting scores from 0.76 to 0.84.

At Box, Vice President of Artificial Intelligence (AI) Products Yashodha Bhavnani observed an 11-point increase over Claude Haiku 4.5 alongside roughly half the latency in early evaluations. Alex Wang of Applied AI at Rogo said Claude Haiku 5.5 fits Rogo's short, high-volume work. This includes quick lookups, subagents and summaries. One example is a Claude Haiku 5.5 subagent that pulls a segment revenue line from a 10-K. Walden Yan, Co-Founder and Chief Product Officer at Cognition, reported that their Devin Fusion platform maintains a top-tier FrontierCode score of 66.2 with Claude Haiku 5.5 operating as a sidekick while lowering latency and operating expense.

## Token Pricing Structure and Context Tier Economics

Anthropic sets Claude Haiku 5.5 pricing in two tiers, based on whether a request's prompt is over or under 100,000 tokens. Below that line, its $0.10 input rate is a tenth of Claude Haiku 4.5's $1.

For prompts containing up to 100,000 tokens, standard input costs $0.10 per million tokens and output costs $0.50 per million tokens. When a prompt exceeds 100,000 tokens, input increases to $0.50 per million tokens and output rises to $2.50 per million tokens. Prompt caching provides additional savings. It charges $0.125 per million tokens for 5-minute writes below the threshold and $0.625 above it. One-hour cache writes cost $0.20 or $1.00 per million tokens depending on prompt length. Cache reads cost $0.01 per million tokens for standard prompts and $0.05 per million tokens for prompts exceeding 100,000 tokens.

Per the Claude API documentation, every request counts its total input tokens, including cache reads and cache writes, to determine whether the 100,000-token line is crossed. If a request crosses that boundary, all input and output tokens are billed at the higher tier even if part of the prompt is a cache hit. Claude Haiku 5.5 is the exception among Claude 4.6-and-later models, which otherwise include the full 1,000,000-token window at standard pricing. The minimum cacheable prompt length is 512 tokens. That is down from 4,096 tokens on Claude Haiku 4.5.

Anthropic states that Claude Haiku 5.5 costs approximately 75 percent less to run on average than Claude Haiku 4.5. For prompts up to 100,000 tokens, input prices drop 90 percent from $1.00 on Claude Haiku 4.5 to $0.10 on Claude Haiku 5.5. Output drops 90 percent from $5.00 to $0.50. Above 100,000 tokens, input is 50 percent lower at $0.50 compared to $1.00, and output is 50 percent lower at $2.50 compared to $5.00. The updated tokenizer turns the same text into about 30 percent more tokens than Claude Haiku 4.5 did. Anthropic says its 75 percent average already accounts for that. United States regional inference carries a 1.1-times multiplier that also applies to rates above the 100,000-token threshold.

On the same day, Anthropic cut Claude Sonnet 5.5 cache reads by 50 percent to $0.10 per million tokens. Anthropic says that lowers the cost of Claude Sonnet 5.5 on most agentic tasks by around 20 percent. Anthropic also introduced monthly API credits for subscribers on Claude Max and Claude Team plans per their credit guidelines. Max 5x users receive $100 per month, Max 20x users receive $200 per month, and Team organizations receive $20 per Standard seat or $100 per Premium seat, pooled up to $500 monthly. Unused credits expire at the end of each billing cycle. They work on Claude Platform API calls but not on interactive Claude Code, extra usage in Claude, or Claude on Amazon Bedrock, Vertex AI or Microsoft Foundry.

- Standard prompts (up to 100,000 tokens): $0.10 input, $0.50 output, $0.01 cache read, and $0.125 5-minute cache write per million tokens.
- Extended prompts (over 100,000 tokens): $0.50 input, $2.50 output, $0.05 cache read, and $0.625 5-minute cache write per million tokens.
- Batch API requests: 50 percent discount yielding $0.05 input and $0.25 output for standard prompts, or $0.25 input and $1.25 output for extended prompts.
- Claude Haiku 4.5 baseline: flat pricing of $1.00 input, $5.00 output, $0.10 cache read, and $1.25 cache write per million tokens.
- Claude Sonnet 5.5 baseline: flat pricing of $2.00 input, $10.00 output, $0.10 cache read, and $2.50 cache write per million tokens.

## Technical Guardrails and Claude Haiku 5.5 Limits

Claude Haiku 5.5 has a 1,000,000-token context window and a 128,000-token output cap on synchronous requests. Claude Haiku 4.5 stopped at 64,000 output tokens.

Output expands to 300,000 tokens on the Message Batches API with the output-300k-2026-03-24 beta header. Per Anthropic's docs, 1,000,000 tokens is roughly 555,000 words or 2,500,000 Unicode characters on the current tokenizer. Both the reliable knowledge cutoff and the training data cutoff are June 2026. The model accepts text and image inputs while returning text outputs. Images use the high-resolution tier. They are downscaled above 2,576 pixels on the long edge or 4,784 visual tokens. A 2,000 by 1,500 pixel image costs about 2.5 times as many visual tokens as on Claude Haiku 4.5.

Claude Haiku 5.5 has five effort levels: low, medium (the default), high, xhigh and max. Lower effort means fewer and terser tool calls. Adaptive thinking is active by default. Its thinking blocks count against max_tokens and persist as input tokens across subsequent turns. Through the Claude API, thinking can be turned off at high effort or below. At xhigh or max, that request returns a 400 error, and Claude Code does not allow thinking off at all.

Anthropic's prompting documentation highlights several concrete behavioral failure modes under specific effort settings. At low effort in long agent workflows, the model is more likely to skip searches, abandon tasks early, or omit validation steps. In Anthropic's testing, moving from low to medium effort roughly halved early stopping and more than doubled output tokens per attempt. At low and medium effort, the model occasionally reports code modifications as completed without executing tests. At xhigh effort in multi-turn chats, Claude Haiku 5.5 sometimes writes its whole answer in its thinking and ends the turn with no visible text. Anthropic advises xhigh only where evals show a quality gain. With thinking off, it may skip a needed tool call when the request also asks for JavaScript Object Notation (JSON) through structured outputs. Keep thinking on for those requests.

Safety filters can decline requests with a stop reason of refusal across cybersecurity, frontier model, biology, and general harm categories. Benign cybersecurity work, and benign work in the general harms category, can trigger these refusals. There is no server-side fallback. Anthropic says resending the same request usually returns another refusal. On Amazon Bedrock, several features remain unsupported, including structured outputs, count tokens, intelligent prompt routing, and Priority, Flex, Reserved, or Batch service tiers. Rate limits on the Claude API Scale tier permit 10,000 requests per minute, 10,000,000 input tokens per minute, and 2,000,000 output tokens per minute. Cached input tokens are excluded from the input limit.

## Deployment Steps and How to Use Claude Haiku 5.5

Claude Haiku 5.5 runs in the Claude apps, the Claude API, Claude Code, GitHub Copilot, Amazon Bedrock, Google Cloud and Microsoft Foundry.

Migrating an existing codebase from Claude Haiku 4.5 requires addressing several breaking API modifications detailed in Anthropic's migration guide. Passing budget_tokens inside thinking parameters now returns a 400 error. Teams must use adaptive thinking with output_config.effort instead. Temperature may only be 1 and top_p only 0.99, while sending any top_k or both temperature and top_p returns a 400 error. Assistant prefill is rejected with a 400 error across all requests. Messages arrays must terminate on a user turn. Computer use has migrated to computer_toolset_20260801 on the Claude API and Google Cloud, while browser use is supported via browser_toolset_20260801.

On Amazon Bedrock, available since October 7, 2026, the model ID is anthropic.claude-haiku-5-5. The global cross-Region ID is global.anthropic.claude-haiku-5-5. On Google Cloud Vertex AI, the model uses identifier claude-haiku-5-5 across US multi-region, Europe multi-region, and global endpoints. On Microsoft Azure, deployment is supported through Microsoft Foundry under claude-haiku-5-5 with Global Standard and US DataZone configurations. In Claude Code, developers can invoke the model with /model claude-haiku-5-5. They can also run automated migration with /claude-api migrate this project to claude-haiku-5-5. The model is also available within GitHub Copilot across Pro, Pro+, Max, Business, and Enterprise plans under provider list pricing.

The Claude pricing page marks Haiku as included on Free, Pro, Max 5x, Max 20x, Team and Enterprise, without naming a version. Pro costs $17 a month billed annually or $20 monthly. Pricing for Max starts at $100. Team Standard seats cost $20 billed annually or $25 monthly. Premium seats cost $100 or $125. Enterprise is $20 per seat a month billed annually plus usage at API rates. Anthropic does not say whether Claude Haiku 5.5 is the default on Free or publish Free-plan message limits for it.

- 1. Update the API model identifier to claude-haiku-5-5, removing legacy date suffixes or custom aliases.
- 2. Set thinking to adaptive and leave output_config.effort at medium, which Anthropic recommends for most work, including agentic coding.
- 3. Remove custom temperature, top_k, and top_p values from API payloads to prevent 400 validation errors.
- 4. Verify that message arrays end with a user message to comply with the removal of assistant prefill.
- 5. In Claude Code, update to v2.1.293 or later, then run /model claude-haiku-5-5 or start with claude --model claude-haiku-5-5.

## Implementation Boundaries and Claude Haiku 5.5 Review Takeaways

Claude Haiku 5.5 is not the pick for autonomous coding agents or the hardest reasoning tasks, where Claude Sonnet 5.5 leads every row of Anthropic's table.

At low and medium effort, Claude Haiku 5.5 can report code changes as completed without running validation tests. Autonomous software workflows require explicit automated test runners. Do not rely on model self-reporting.

Security researchers should expect new refusals from Claude Haiku 5.5. Anthropic says its cybersecurity safeguards are more restrictive than Claude Haiku 4.5's but less restrictive than Claude Sonnet 5.5's. Organizations blocked by the classifiers can apply to Anthropic's Cyber Verification Program or Life Sciences Verification Program. Teams on Amazon Bedrock cannot use structured outputs or intelligent prompt routing with Claude Haiku 5.5. A team that needs structured outputs can call Claude Haiku 5.5 through the Claude API instead.

To run your own Claude Haiku 5.5 review, send a sample of classification or extraction requests to claude-haiku-5-5 at the default medium effort. Then compare accuracy and cost against Claude Haiku 4.5 or Claude Sonnet 5.5.

## How to Use Claude Haiku 5.5

To use Claude Haiku 5.5 effectively, start by choosing an interface that exposes the exact model you need for your workload. Test a new configuration on a single representative task, such as drafting a routine email or parsing a short document, before expanding to full automation. Inspect the saved output directly to confirm formatting, accuracy, and adherence to your prompt instructions.

For coding tasks, Cursor serves as an editor option when the target model appears on its supported list. Check current model availability before setup to confirm the exact model is listed. Once connected, save your prompts and review the generated diffs locally before applying changes to your main codebase.

## Frequently Asked Questions

- Anthropic lists Haiku access as included on the Claude Free plan, but the pricing page does not publish message limits or confirm whether Claude Haiku 5.5 is the active default model on Free tiers. Paid Claude plans offering Haiku access start at $17 per month for Claude Pro billed annually, and the API charges $0.10 per million input tokens for prompts up to 100,000 tokens, or $0.05 through the Batch API.
- Claude Haiku 5.5 features a 1,000,000-token context window, which equates to roughly 555,000 words or 2,500,000 Unicode characters on Anthropic's tokenizer. Maximum output is 128,000 tokens on standard synchronous API requests and up to 300,000 tokens on the Message Batches API.
- To use Claude Haiku 5.5 in the API, set the model to claude-haiku-5-5 and choose an effort level with output_config.effort (medium is the default). Use adaptive thinking instead of budget_tokens, remove temperature, top_p and top_k, and end messages with a user turn, because assistant prefill returns a 400 error. On Amazon Bedrock, use model identifier anthropic.claude-haiku-5-5 or global.anthropic.claude-haiku-5-5.
- Yes. Claude Haiku 5.5 beats Claude Haiku 4.5 on every row of Anthropic's table where both have a score, and Anthropic says it costs around 75 percent less to run on average (90 percent less up to 100,000 tokens, 50 percent less above). It expands context capacity from 200,000 to 1,000,000 tokens, increases max output from 64,000 to 128,000 tokens, and introduces adjustable reasoning effort.
- Use Claude Haiku 5.5 for high-volume, latency-sensitive work such as summaries, routing, classification and subagents, where its $0.10 input price under 100,000 tokens matters most. Choose Claude Sonnet 5.5 for the hardest reasoning and terminal-heavy coding, where it leads every row of Anthropic's table, including 70.6 percent against 39.2 percent on Terminal-Bench 4.0.

### Optimize Your Model Architecture and Token Budgets

Evaluate how Claude Haiku 5.5 fits your classification, routing, and subagent workflows without sacrificing reliability. Schedule a technical review with our team.

Book a Consultation
