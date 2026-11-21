---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-pricing-benchmarks-and-claude-code-changes-mcsa-guru
title: "Claude Fable 5.2: Pricing, Benchmarks and Claude Code Changes | MCSA Guru"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "anthropic", "opus", "anthropicas", "code", "mythos", "pricing"]
source: https://mcsaguru.com/claude-fable-5-2-mythos-5-2-pricing-claude-code
source_anchor: ""
source_lines: [1, 109]
section: "Claude_Anthropic"
---
Anthropic released Claude Fable 5.2 and Claude Mythos 5.2 on November 9, 2026. The price dropped to $8 per million input tokens and $40 per million output tokens, down from $10 and $50 for Fable 5.1 on Anthropicâs pricing page. Anthropic also says the new models generate output up to 30% faster than Fable 5.1 and Mythos 5.1.

Claude Fable 5.2 and Mythos 5.2 are the same underlying model. Fable ships with cybersecurity and biology safeguards, which is why Anthropic can offer it to all users. Mythos drops those safeguards and goes only to vetted organizations through Anthropicâs Cyber and Life Sciences Verification Programs.

For Claude Code users, the change is already live. Claude Code v2.1.321 added `claude-fable-5-2` as the default Fable model, with a 1M-token context window and $0.20 per million tokens for cache reads.

## Claude Fable 5.2 pricing next to the rest of the lineup

Anthropicâs pricing page lists these standard rates per million tokens (MTok):

| Model | Input | Output | Cache hit | Notes | 
|---|---|---|---|---|
| Claude Fable 5.2 | $8 | $40 | $0.20 | Fast mode $16 / $80 | 
| Claude Mythos 5.2 | $8 | $40 | $0.20 | Verification Programs only | 
| Claude Fable 5.1 | $10 | $50 | $0.25 | Previous generation | 
| Claude Opus 5.5 | $4 | $20 | $0.20 | Anthropicâs suggested daily driver | 
| Claude Sonnet 5.5 | $2 | $10 | $0.10 |  | 
| Claude Haiku 5.5 | from $0.10 | from $0.50 | from $0.01 | Higher rates over 100K-token prompts | 

A few other numbers from the same page matter if you run Fable through the API:

- **Cache writes:** $10/MTok for a 5-minute write and $16/MTok for a 1-hour write. A cache hit on Fable 5.2 costs 2.5% of the base input price.
- **Batch API:** Fable 5.2 drops to $4 input and $20 output, a 50% discount. Fast mode canât be combined with the Batch API.
- **Fast mode:** Anthropic says it speeds up token generation by up to 2.5 times and charges $16 input and $80 output for it.

Fable still costs twice as much per token as Opus 5.5. Anthropicâs case for it rests on cost per finished task, which the benchmark section below covers.

## What Anthropic says about the benchmarks

Anthropic published a table comparing Fable 5.2 against its own models and two competitors. These are Anthropicâs numbers, run with safeguards on and at each modelâs highest available effort unless marked otherwise.

| Benchmark (Anthropic) | Fable 5.2 | Fable 5.1 | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | 49.1% (Xhigh) | 32.4% (Xhigh) | 45.5% | 42.8% | 41.1% (Medium) | 
| FrontierSWE v2 (agentic coding) | 69.9% | 56.3% | 62.3% | 65.5% | 61.2% | 
| OSWorld 2.1, offline set, partial (computer use) | 89.3% | 83.5% | 87.2% | 79.5% | 81.5% | 

FrontierCode measures whether a modelâs code changes would be merged by the maintainers of open-source repositories. The more useful number for budgeting sits below the top score. Anthropic says Fable 5.2 at High effort scores 45.3% on FrontierCode 2.0, about the same as Opus 5.5âs best score of 45.5%, for roughly half the cost per task.

Opus 5.5 costs half as much per token. Fable 5.2 at High effort spends fewer tokens to reach the same FrontierCode score, so Anthropicâs chart puts its cost per finished task at roughly half of Opus 5.5 at its best.

## Which model Anthropic tells you to use

Anthropic doesnât tell everyone to switch. For most coding and knowledge tasks, it recommends starting with Claude Opus 5.5, which it calls its strongest daily driver. From there you move up to Fable or Mythos for the most ambitious, open-ended technical work, or down to Sonnet 5.5 or Haiku 5.5.

Anthropic also describes two mixed setups:

- **Advisor:** Haiku, Sonnet or Opus consult Fable 5.2 when they hit a harder problem.
- **Orchestrator:** Fable 5.2 plans the work and delegates simpler tasks to smaller models.

The orchestrator setup is close to how Claude Code subagents already work: a main session hands smaller jobs to agents that can run on cheaper models. Our Claude Code subagents guide covers how to assign a model per agent.

## What changes in Claude Code v2.1.321

The Claude Code changelog for v2.1.321 lists the model switch first: Claude Fable 5.2 (`claude-fable-5-2`) is now the default Fable model, with 1M context, $8/$40 per MTok and $0.20/MTok cache reads.

The same release moves saved model picks. If you had Fable 5.1 selected, the changelog says that pick moves to `fable`. On Pro and Max plans this happens at once. On Team and Enterprise plans it happens once your organization allows it. To go back to the old model:

`/model claude-fable-5-1`
The release also carries security and reliability fixes:

- MCP OAuth requests could send secrets such as refresh tokens to another host when a server redirected them. Thatâs fixed.
- A read-only Bash command could run outside the sandbox without a permission prompt in some cases. Thatâs fixed too.
- When fast serving is rate-limited, overloaded or failing, fast mode now tries standard speed on the same model first instead of losing replies or switching models.

The MCP OAuth fix applies whether or not you use Fable. If you connect Claude Code to remote MCP servers, our post on Claude Code MCP security risks explains why tokens leaking to another host is a real concern.

Claude Code v2.1.322 followed and added fast mode on Vertex in the global, us and eu regions.

## Safeguards: why some security requests get a different model

Fable 5.2 carries safety classifiers that block high-risk cyber activity, including penetration testing, exploit generation and binary-based vulnerability work, plus most tasks in research biology. A blocked request doesnât end in a plain refusal. Anthropic says the user gets a response from Claude Opus 4.8 for cybersecurity and from Claude Opus 5 for biology.

Anthropic reports fewer false positives than in the last release. Across claude.ai, Claude Code and Cowork in early testing, it saw about 55% fewer blocks with Fable 5.2 than with Fable 5.1: about 60% fewer in cybersecurity and 40% fewer in biology.

If your work is security engineering, Mythos 5.2 is the model without those blocks. Anthropic calls it its most capable model for cybersecurity engineering, and access goes through the Cyber Verification Program. Claude Enterprise customers can also use Claude Security, which Anthropic says now includes Mythos 5.2.

Anthropic also notes that for customers outside its Enterprise Frontier Safeguards beta, it keeps 30 days of data for safety monitoring on these models.

## Where you can run Claude Fable 5.2

#### Claude Fable 5.2 availability

| Claude API ID | claude-fable-5-2 | 
|---|---|
| Amazon Bedrock ID | anthropic.claude-fable-5-2 | 
| Context window | 1M tokens | 
| Max output | 128K tokens | 
| Platforms | Claude Platform, AWS, Google Cloud, Microsoft Azure | 
| Fast mode | Claude Code, Claude Platform, Claude Models on Google Cloud, Microsoft Foundry (Anthropic hosted) | 
| Retirement | Not sooner than November 9, 2027 | 

Mythos fast mode is available only to organizations in Anthropicâs Verification Programs.

For subscribers, Anthropic is giving extra Fable usage during launch week. Plans and seats that include Fable get one to three Fable resets based on how long theyâve subscribed, and Fable counts for up to 50% of weekly usage limits on Pro plans and standard Team seats.

#### Before you switch to Fable 5.2

- Update Claude Code to v2.1.321 or later for the MCP OAuth and sandbox fixes
- Check which model your saved picks now point to with /model
- Confirm the effort level your tool sends before comparing costs
- Test integrations that edit prior turns against preserved thinking
- Expect security tasks to be answered by Opus 4.8 unless you have Mythos access

## Should you move to Claude Fable 5.2?

If you already pay for Fable 5.1, Fable 5.2 costs less per token ($8/$40 against $10/$50), and Anthropic says itâs faster and scores higher. Claude Code makes the switch for you, and `/model claude-fable-5-1` takes you back if something breaks.

If youâre on Opus 5.5, Anthropic itself says to stay there for most coding work. Fable 5.2 makes sense for long, hard agentic tasks, or as an advisor or orchestrator above cheaper models. Run a few of your own tasks at High effort and compare the cost per finished task, not the per-token price. For more ways to keep the bill down, see how to cut Claude Code costs.
