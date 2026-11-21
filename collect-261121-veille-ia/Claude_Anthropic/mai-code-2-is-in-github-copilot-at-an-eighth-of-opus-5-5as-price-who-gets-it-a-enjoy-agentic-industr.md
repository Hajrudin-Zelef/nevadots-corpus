---
id: collect-261121-veille-ia/claude-anthropic/mai-code-2-is-in-github-copilot-at-an-eighth-of-opus-5-5as-price-who-gets-it-a-enjoy-agentic-industr
title: "MAI Code 2 is in GitHub Copilot at an eighth of Opus 5.5âs price. Who gets it â Enjoy Agentic Industries"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "copilot", "github", "models", "opus", "price", "flash", "model"]
source: https://enjoy.dev/blog/mai-code-2/
source_anchor: ""
source_lines: [1, 114]
section: "Claude_Anthropic"
---
- To
- Everyone on GitHub Copilot
- From
- Mo, Chief Executive
- Date
- November 17, 2026, 3:13 PM CST
- Re
- MAI Code 2 and MAI Code 2 Flash

# MAI Code 2 is in GitHub Copilot at an eighth of Opus 5.5âs price. Who gets it

On November 17, 2026, Microsoft put MAI Code 2 in GitHub Copilot on Pro and up, and MAI Code 2 Flash on every plan. Until May 17, 2027, Code 2 costs $0.50 per million input tokens and $2.50 output, an eighth of what Copilot charges for Claude Opus 5.5, which Pro doesnât include. Microsoft says it gets 96% of Opus 5.5âs scores at 5% of the cost per task, in its own tests, at the promotional price.

## What Microsoft released

On Tuesday, November 17, 2026, at its Ignite conference in San Francisco, Microsoft released two coding models among seven new models, and GitHub put both in Copilot the same day. Its changelog describes them this way:

- **MAI Code 2** , âA powerful, general purpose coding model,â âIdeal for complex tasks like deep debugging, multi-file implementation, refactoring, and repository-wide reasoning.â
- **MAI Code 2 Flash** , âA lightweight, cost-efficient coding model optimized for everyday development workflows,â for âcode explanations, targeted edits, routine fixes, and rapid iteration.â

Both are in the model picker in VS Code, Visual Studio, Copilot CLI, the cloud agent, the Copilot app, github.com, GitHub Mobile, JetBrains IDEs, Xcode and Eclipse. GitHubâs model docs list both with a 1 million token context window, which Copilot offers in VS Code and Copilot CLI only, and with configurable reasoning. They also note that âMAI models are continuously improving models. Performance and behavior may evolve over time as new checkpoints are released.â

Microsoft is plain about what they arenât for. Its announcement says theyâre built for âthe coding work our customers do every day,â and are âexplicitly not yet tuned for harder problems, like fundamental science research and very long horizon agentic work.â

## Who gets it

GitHub lists MAI Code 2 on Copilot Pro, Pro+, Max, Business and Enterprise, and Flash on those plans plus Free and Student. Here is how that compares with Claude Opus 5.5, by GitHubâs plan table and its allowances on November 17:

| Plan | Price | AI credits a month | MAI Code 2 | MAI Code 2 Flash | Claude Opus 5.5 | 
|---|---|---|---|---|---|
| Free and Student | $0 | An allowance | No | Through Auto only | Not selectable | 
| Pro | $10 a month | 1,500 | Yes | Yes | No | 
| Pro+ | $39 a month | 7,000 | Yes | Yes | Yes | 
| Max | $100 a month | 20,000 | Yes | Yes | Yes | 
| Business and Enterprise | Per user | Per user, pooled | Yes | Yes | Yes | 

Free and Student âhave access to models through auto model selection only,â and Flash is one of the models Auto can choose. On Business and Enterprise, ânew models are enabled automatically unless an administrator has turned off the global default or explicitly disables the model.â

For Pro, this is the news. Proâs other models include Claude Sonnet 5.5, GPT-6.1 Terra and Gemini 4 Hydrogen, but not Opus 5.5, GPT-6.1 Sol or GPT-6.2 Sol. Of the models GitHubâs price list calls âPowerful,â Pro had only GPT-5.3-Codex and Kimi K3, which leaves Copilot on December 14. MAI Code 2 joins them at a fraction of their price.

Annual subscribers get both too: GitHub says theyâre âavailable to eligible GitHub Copilot subscribers on annual plans.â On the legacy request-based annual Pro and Pro+ plans, GitHubâs multiplier table charges MAI Code 2 1 premium request and Flash 0.33, against 9 for Claude Sonnet 4.6 and 27 for Claude Opus 4.8, though the same page says users on those plans âwill not receive access to new models and features.â

## What it costs

Copilot bills chat and agents by the token, in AI credits, âwhere 1 AI credit = $0.01 USD.â Both models start at a promotional price that ends on May 17, 2027. GitHubâs price list, per million tokens, on November 17:

| Model | Input | Cached input | Output | 
|---|---|---|---|
| MAI Code 2, until May 17, 2027 | $0.50 | $0.025 | $2.50 | 
| MAI Code 2, from May 17, 2027 | $1.00 | $0.05 | $5.00 | 
| MAI Code 2 Flash, until May 17, 2027 | $0.05 | $0.005 | $0.25 | 
| MAI Code 2 Flash, from May 17, 2027 | $0.10 | $0.01 | $0.50 | 
| Claude Opus 5.5 | $4.00 | $0.20 | $20.00 | 
| GPT-6.1 Sol, up to 272K input | $2.00 | $0.10 | $10.00 | 
| Claude Sonnet 5.5 | $2.00 | $0.10 | $10.00 | 
| GPT-6.1 Terra, up to 272K input | $1.00 | $0.05 | $5.00 | 
| Claude Haiku 5.5, up to 100K input | $0.10 | $0.01 | $0.50 | 
| GPT-6 Luna, up to 272K input | $0.10 | $0.01 | $0.50 | 

So until May, a million output tokens costs 250 credits on MAI Code 2, 1,000 on Sonnet 5.5 and 2,000 on Opus 5.5. The Claude and GPT models above also charge for cache writes, and the MAI models donât. After May 17, Code 2 costs what GPT-6.1 Terra does, and Flash what Haiku 5.5 and GPT-6 Luna do.

## How good it is, by Microsoftâs numbers

Microsoftâs headline is that MAI Code 2 âgives you **98%** of the performance of GPT 6.1 Sol at **40%** of the cost, and **96%** of Claude Opus 5.5âs performance at just **5%** of the cost.â The chart behind it, from the announcement:

| Benchmark | MAI Code 2 | GPT-6.1 Sol | Claude Opus 5.5 | 
|---|---|---|---|
| DeepSWE 1.1 | 73.2% | 74.6% | 69.0% | 
| Terminal-Bench 2.1 | 84.6% | 85.0% | 78.7% | 
| SWE Atlas | 43.1% | 47.4% | 57.5% | 
| FrontierCode v1.1 | 52.0% | 49.4% | 56.0% | 
| Average | 63.2% | 64.1% | 65.3% | 
| Average cost per task | $0.309 | $0.761 | $6.306 | 

Three things to know before you read too much into it:

1. **Theyâre Microsoftâs tests.** All models ran âat max reasoning effort,â three of the benchmarks on Microsoftâs own platform âwith the GitHub Copilot CLI without subagents,â and FrontierCodeâs figures came from Artificial Analysis. We found no independent test of MAI Code 2 yet.
2. **The costs use the promotional price.** âMAI-Code-2 cost is shown at its 50% promotional price.â At the full price from May 17, 2027, the same tasks would cost about $0.62 each by Microsoftâs figures, against $0.761 for GPT-6.1 Sol.
3. **The averages hide a gap.** On SWE Atlas, MAI Code 2 is 14 points behind Opus 5.5.

For Flash, Microsoftâs chart has it at 61.0% across DeepSWE 1.1, Terminal-Bench 2.1 and SWE Atlas, against 55.1% for GPT-6 Luna and 53.9% for Claude Haiku 5.5, at $0.052 a task to their $0.08 and $0.319, again at its promotional price. Microsoft also says that in blind frontend tests, judged by 258 annotators, âreviewers preferred its results over GPT-6 Sol 67% of the time.â

## Using it outside Copilot

For now, Copilot is the place. Flash is also âin limited-access preview in Microsoft Foundry,â and Microsoft adds: âSoon, weâll be releasing it on Open Router, Vercel, Hermes, Open Code, and Kilo.â It hasnât said when, or at what price.

OpenCode can already use your Copilot plan, which GitHub has officially supported since January on Pro, Pro+, Business and Enterprise. Its provider docs say it âfetches the accountâs current model list after login,â so it offers what GitHub offers your account; we havenât yet confirmed MAI Code 2 appears there. Enjoy runs OpenCode beside Claude Code, Codex and the other labsâ agents, each on its own plan, and lets you switch agents mid-conversation, so everyday work can go to a cheap Copilot model and the hard part to Claude Code on your Claude plan.

## What to do

1. **Try it on the work itâs built for.** Pick MAI Code 2 in Copilotâs model picker for edits, fixes and front-end work, and compare it with what you use now on a task you know. Visual Studio needs version 17.14.6 or later; for the other apps, GitHubâs table of minimum versions doesnât give one yet, so update them.
2. **Keep a stronger model for the long jobs.** Microsoft says the models arenât tuned for âvery long horizon agentic work,â and Opus 5.5 leads on SWE Atlas. In Copilot, Opus 5.5 takes Pro+ or above.
3. **Cap what you can spend.** GitHubâs advice: âcreate or edit a**Bundled AI credits budget** , set it to $0 USD, and enable**Stop usage when budget limit is reached** .â
4. **Mark May 17, 2027.** Thatâs when both prices double. And on December 14, 2026, Copilot drops Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3, so if you use one of them, try Flash or GitHubâs suggested replacement before then.
5. **Admins:** check your model policies, since new models are on by default.

## What Microsoft hasnât said

Microsoft and GitHub havenât said how big the models are, whether MAI Code 2 itself is coming to Foundry, when either model reaches the outside services or what it will cost there, or what changes when a new checkpoint arrives. And we found no scores from anyone outside Microsoft. Weâll update this memo when there are some.

## 8.Questions.

- ### What is MAI Code 2?
- Itâs Microsoftâs new coding model, released in GitHub Copilot on November 17, 2026, with a smaller MAI Code 2 Flash. GitHub calls Code 2 âIdeal for complex tasks like deep debugging, multi-file implementation, refactoring, and repository-wide reasoning,â and Flash fit for âcode explanations, targeted edits, routine fixes, and rapid iteration.â
- ### Is MAI Code 2 free in GitHub Copilot?
- No. MAI Code 2 is on Copilot Pro, Pro+, Max, Business and Enterprise, and bills AI credits like other models. MAI Code 2 Flash is on Copilot Free and Student too, but those plans get models only through auto model selection, which can pick Flash.
- ### How much does MAI Code 2 cost in Copilot?
- Until May 17, 2027, $0.50 per million input tokens, $0.025 cached and $2.50 output, where 1 AI credit is $0.01. From May 17, 2027, itâs $1.00, $0.05 and $5.00. Flash costs a tenth of that, $0.05, $0.005 and $0.25, until the same date, then double.
- ### Is MAI Code 2 better than Claude Opus 5.5?
- Not by Microsoftâs own numbers. Across four benchmarks it averaged 63.2% to Opus 5.5âs 65.3% and GPT-6.1 Solâs 64.1%, for far less per task, and it trailed Opus by 14 points on SWE Atlas. Microsoft says itâs âexplicitly not yet tunedâ for science research and âvery long horizon agentic work.â
- ### Can I use MAI Code 2 outside GitHub Copilot?
- Not yet, apart from Flash in a limited preview on Microsoft Foundry. Microsoft says âSoon, weâll be releasing it on Open Router, Vercel, Hermes, Open Code, and Kilo,â without a date. OpenCode can already sign in with your Copilot plan and loads the models GitHub offers your account, though we havenât confirmed MAI Code 2 shows up there.
- ### Does MAI Code 2 work on an annual Copilot plan?
- Yes. GitHub says both models are available to eligible subscribers on annual plans. On legacy request-based annual Pro and Pro+ plans, MAI Code 2 costs 1 premium request and Flash 0.33, against 27 for Claude Opus 4.8.
