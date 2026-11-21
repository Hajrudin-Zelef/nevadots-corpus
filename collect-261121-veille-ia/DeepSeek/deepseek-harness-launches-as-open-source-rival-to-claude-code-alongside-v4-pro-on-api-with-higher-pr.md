---
id: collect-261121-veille-ia/deepseek/deepseek-harness-launches-as-open-source-rival-to-claude-code-alongside-v4-pro-on-api-with-higher-pr
title: "DeepSeek Harness launches as open source rival to Claude Code, alongside V4-Pro on API with higher prices"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "harness", "code", "claude", "agent", "developers", "prices", "developer"]
source: https://venturebeat.com/technology/deepseek-harness-launches-as-open-source-rival-to-claude-code-alongside-v4-pro-on-api-with-higher-prices
source_anchor: ""
source_lines: [1, 167]
section: "DeepSeek"
---
DeepSeek is expanding beyond the model layer and deeper into the software developers use to put AI agents to work.

The Chinese AI lab on Thursday launched the official version of DeepSeek-V4-Pro, an updated flagship model focused heavily on agentic workloads, alongside DeepSeek Harness v0.1, a new open-source agent harness that gives developers an alternative to integrated coding-agent environments such as Anthropic’s Claude Code.

Together, the releases amount to a broader developer push from DeepSeek. V4-Pro is now available across DeepSeek’s web interface, mobile app and API, with native support for the OpenAI Responses API and integration with Codex.

DeepSeek Harness, meanwhile, is entering developer preview under the MIT license and the code is available now for download and use on GitHub. It's built around an unusually modular premise: practically every part of the agent runtime can be swapped out as a plugin.

But developers accessing V4 through DeepSeek’s API will soon pay considerably more for it. DeepSeek is simultaneously abandoning its existing flat API pricing in favor of peak and off-peak rates beginning at 16:00 UTC on Sunday, Aug. 16 (2 am ET).

Even the discounted off-peak cache-miss and output prices will be substantially higher than the prices available today.

The combination is significant because DeepSeek is no longer competing solely over model intelligence and token prices. With Harness, it is moving into the layer that determines how models use tools, manipulate files, maintain sessions and execute long-running agent workflows — territory where Anthropic’s Claude Code and other coding agents have become increasingly important developer products.

## **DeepSeek builds its own agent harness**

DeepSeek describes Harness, or `dsh`, as an open-source agent harness built on Cordis, a framework designed around composable plugins.

Its guiding principle is simple: **“Everything is a plugin.”**

That extends to models, tools, skills, sessions, sandboxes, filesystems, loops, orchestration and user interfaces, according to DeepSeek. Rather than making those components fixed pieces of a single coding agent, Harness is designed to let developers mix, replace and extend them.

The project is available under the MIT license and can currently be launched from npm with `npx @deepseek-ai/dsh web`. DeepSeek also provides instructions for building it directly from source. The repository describes the software explicitly as a developer preview and warns that “THERE WILL BE COMPATIBILITY-BREAKING CHANGES.”

That caveat matters for enterprise developers. Harness is not yet being presented as a stable drop-in production platform. But its architecture points toward a potentially important strategy: DeepSeek can now offer developers not only models but an open framework for assembling the systems that surround them.

That makes Anthropic's Claude Code and OpenAI's Codex useful competitive references, although the products should not be treated as functionally identical.

DeepSeek Harness is an open-source, model-agnostic alternative to the agent infrastructure underlying Claude Code and Codex—not yet a full replacement for either product’s broader developer experience.

It can already inspect repositories, edit files, execute shell commands, search files and the web, maintain plans, invoke skills, delegate work to subagents and enforce approval policies. Those are the essential capabilities that make Claude Code and Codex agentic coding tools rather than autocomplete systems.

DeepSeek explicitly describes Standard mode as a full coding agent with file editing, shell access, search, planning, subagents and workflows. Its local web interface lets users select a workspace and approve sensitive operations.

But Claude Code and Codex now extend well beyond that agent loop. Here's a quick comparison:

| **Dimension** | **DeepSeek Harness** | **Claude Code** | **OpenAI Codex** | 
| Read, edit and test a repository | Yes | Yes | Yes | 
| Shell and development tools | Yes | Yes | Yes | 
| Planning and subagents | Yes | Yes | Yes | 
| Permission controls and sandboxing | Yes, configurable through plugins | Yes, mature built-in permission and sandbox system | Yes, granular sandbox and approval controls | 
| Primary interfaces | Local web UI; headless command; Python SDK | Terminal, VS Code, JetBrains, desktop, browser, mobile and Slack | CLI, IDE extension, desktop app, web/cloud and integrations | 
| Hosted background agents | Not documented as a DeepSeek-managed service | Yes | Yes | 
| GitHub-native PR workflow | Not documented as a finished integration | GitHub Actions, automatic reviews, issue-to-PR workflows | Cloud tasks, automatic reviews, PR fixes and GitHub Action | 
| Model choice | DeepSeek, Anthropic, OpenAI and custom compatible endpoints | Primarily Claude, including Bedrock, Google Cloud and Microsoft hosting | Primarily OpenAI models, with configurable providers in the open-source CLI | 
| Extensibility | Exceptional: virtually every component is replaceable | Strong: skills, hooks, MCP, plugins and agent teams | Strong: skills, MCP, custom agents, SDK and app server | 
| Product maturity | Developer preview; breaking changes expected | Established commercial product | Established commercial product plus open-source CLI | 
| License | MIT | Commercial product with extensibility interfaces | Codex CLI is open source; cloud and app services are managed products | 

DeepSeek Harness instead emphasizes modularity and replacement: the model itself is another plugin rather than necessarily the center of a vertically integrated stack.

DeepSeek’s repository was already attracting significant developer attention on launch day, showing roughly 27,500 GitHub stars and 2,000 forks as of Aug. 13, although those rapidly changing figures are best viewed as a snapshot rather than an adoption metric.

## **V4-Pro gets an agent-focused upgrade**

Harness arrives alongside the general-availability release of DeepSeek-V4-Pro-0813.

DeepSeek originally introduced the V4 family in preview in April. The lineup consists of the 1.6-trillion-parameter V4-Pro, with 49 billion parameters activated per token, and the smaller 284-billion-parameter V4-Flash, with 13 billion activated. Both support context windows of up to one million tokens.

The company’s Aug. 13 release therefore is not the first appearance of V4-Pro. It is the transition from the earlier preview into an updated official version, with DeepSeek emphasizing agent performance.

“The official version of DeepSeek-V4-Pro has been released, featuring significantly enhanced agent capabilities and support for the Responses API and Codex integration,” DeepSeek says on its API website. “It is now fully available across the web, mobile app, and API; we welcome your testing and feedback.”

DeepSeek’s changelog similarly says the general-availability model has “significantly enhanced Agent capabilities,” particularly in production environments. Developers using the API do not have to change model identifiers: `deepseek-v4-pro` now resolves to the latest V4-Pro version.

The company has also added native OpenAI Responses API support, lowering the amount of integration work required for applications already built around that interface.

DeepSeek says V4-Pro is optimized for OpenAI's own open source harness, Codex, with one-click setup. Its current API documentation lists Responses API, tool calling, JSON output and an Anthropic-format API among the supported interfaces for both V4-Pro and V4-Flash.

For developers using DeepSeek directly rather than through an API, V4-Pro is now accessible through “Expert Mode” on the company’s app and website.

## **Reasoning effort becomes another deployment knob**

DeepSeek is also making reasoning effort an explicit control across V4-Pro and V4-Flash.

The V4 model documentation describes three levels: Non-think, designed for fast routine tasks; Think High, intended for more complex problem-solving and planning; and Think Max, which allocates substantially more reasoning to difficult problems.

That distinction can be operationally important for agent systems because maximum reasoning on every step can consume unnecessary time and tokens. A coding agent might use relatively little reasoning to inspect a file or execute a routine tool call, then increase effort when diagnosing a difficult bug or planning a multi-stage code change.

DeepSeek’s latest benchmark table suggests the 0813 model improves substantially on agent-oriented tests, although the figures are company-reported and some results depend on the harness configuration.

DeepSeek reports V4-Pro-0813 scores of **87.9 on Terminal Bench 2.1, 74.1 on Toolathlon-Verified, 71.1 on DSBench-FullStack and 67.2 on DSBench-Hard**. It does not lead every comparison in DeepSeek’s own table: Fable 5, for example, scores 77.9 on Toolathlon-Verified and 77.2 on DSBench-FullStack.

There is an especially important qualification buried beneath the benchmark table. For public Code Agent tasks, DeepSeek says V4-Pro-0813 was tested using its upcoming DeepSeek Harness in “minimal mode.”

In other words, some of the agent results arriving alongside Harness are not purely model benchmarks. They measure the model operating inside an agent execution environment — precisely the software layer DeepSeek is now releasing to developers.

## **A sharp reversal in DeepSeek’s API price trajectory**

The bigger immediate change for teams already running DeepSeek in production may be pricing.

DeepSeek’s current API documentation lists V4-Flash at $0.14 per million cache-miss input tokens and $0.28 per million output tokens, while V4-Pro costs $0.435 for cache-miss input and $0.87 for output. Cache hits are dramatically cheaper at $0.0028 for Flash and $0.003625 for Pro.

Those prices themselves represented a major reduction from V4’s original April launch economics. When V4 arrived in April, V4-Pro was priced at $1.74 per million cache-miss input tokens and $3.48 per million output tokens. By late May, DeepSeek had made a 75% reduction permanent, intensifying its position as an unusually inexpensive option for high-volume agent workloads. Now the pendulum is moving in the other direction.

Beginning Aug. 16 at 16:00 UTC, DeepSeek will charge different rates depending on when API calls occur. Peak hours are 01:00–04:00 UTC and 06:00–10:00 UTC (9:00 PM – 12:00 AM ET and 2:00 AM – 6:00 AM ET, respectively) with all other hours classified as off-peak. Off-peak rates are half the corresponding peak prices.

For V4-Flash, off-peak cache-miss input rises from $0.14 to $0.22 per million tokens, while output rises from $0.28 to $0.66. During peak hours those rates reach $0.44 input and $1.32 output.

V4-Pro moves from $0.435 per million cache-miss input tokens and $0.87 output today to $0.66 and $1.98 off-peak, respectively. Peak rates rise to $1.32 input and $3.96 output.

The increases are even more pronounced for cached input. V4-Pro cache hits rise from $0.003625 per million tokens today to $0.022 off-peak and $0.044 at peak. Flash moves from $0.0028 to $0.007 off-peak and $0.014 peak.

| **Model** | **Old input (per 1M token)** | **Old output (per 1M tok)** | **Old total (1M in/1M out)** | 
| deepseek-v4-flash | $0.14 | $0.28 | $0.42 | 
| deepseek-v4-pro | $0.435 | $0.87 | $1.305 | 

The new prices still position DeepSeek as an affordable alternative via API to Western proprietary labs, but Reuters reported Thursday that, depending on model, token category and time of use, the changes represent increases ranging from 50% to more than 1,100% over existing rates.

| **Model** | **Input ($/1M)** | **Output ($/1M)** | **Total ($/1M)** | **Source** | 
| Muse Spark 1.2 Contributor | $0.10 | $0.20 | $0.30 | Meta | 
| MiMo-V2.5 Flash | $0.10 | $0.30 | $0.40 | Xiaomi | 
| **DeepSeek-V4-Flash — off-peak** | **$0.22** | **$0.66** | **$0.88** | **DeepSeek** | 
| GPT-5.6 Luna | $0.20 | $1.20 | $1.40 | OpenAI | 
| MiniMax-M3 | $0.30 | $1.20 | $1.50 | MiniMax | 
| LongCat-2.0 — limited-time promo | $0.30 | $1.20 | $1.50 | LongCat | 
| **DeepSeek-V4-Flash — peak hours** | **$0.44** | **$1.32** | **$1.76** | **DeepSeek** | 
| MiMo-V2.5 | $0.40 | $2.00 | $2.40 | Xiaomi | 
| **DeepSeek-V4-Pro — off-peak** | **$0.66** | **$1.98** | **$2.64** | **DeepSeek** | 
| LongCat-2.0 — standard | $0.75 | $2.95 | $3.70 | LongCat | 
| MiMo-V2.5 Pro (≤256K) | $1.00 | $3.00 | $4.00 | Xiaomi | 
| **DeepSeek-V4-Pro — peak hours** | **$1.32** | **$3.96** | **$5.28** | **DeepSeek** | 
| Muse Spark 1.1 / 1.2 | $1.25 | $4.25 | $5.50 | Meta | 
| GLM-5.2 | $1.40 | $4.40 | $5.80 | Z.ai | 
| Grok 4.6 — <200K prompt tokens | $2.00 | $6.00 | $8.00 | xAI | 
| MiMo-V2.5 Pro (>256K) | $2.00 | $6.00 | $8.00 | Xiaomi | 
| Qwen3.8-Max | $2.00 | $6.00 | $8.00 | QwenCloud | 
| Gemini 3.6 Flash | $1.50 | $7.50 | $9.00 |  | 
| GPT-5.6 Terra | $2.00 | $12.00 | $14.00 | OpenAI | 
| Grok 4.6 — ≥200K prompt tokens | $4.00 | $12.00 | $16.00 | xAI | 
| GPT-5.4 | $2.50 | $15.00 | $17.50 | OpenAI | 
| Kimi K3 | $3.00 | $15.00 | $18.00 | Moonshot AI | 
| Claude Opus 5 | $5.00 | $25.00 | $30.00 | Anthropic | 
| Sakana Fugu Ultra (≤272K) | $5.00 | $30.00 | $35.00 | Sakana AI | 
| GPT-5.6 Sol — Standard mode | $5.00 | $30.00 | $35.00 | OpenAI | 
| Claude Fable 5 / Claude Mythos 5 | $10.00 | $50.00 | $60.00 | Anthropic | 
| GPT-5.6 Sol — Fast mode | $10.00 | $60.00 | $70.00 | OpenAI | 

That makes the “50% lower” off-peak framing potentially misleading without context. Off-peak is 50% cheaper than DeepSeek’s *new* peak rate; it is not a 50% discount from the API prices developers are paying today.

For a simple workload consisting of one million cache-miss input tokens plus one million output tokens, V4-Pro currently costs $1.305. The same token mix will cost $2.64 off-peak, roughly twice as much, or $5.28 during peak hours, more than four times the current price.

V4-Flash moves from $0.42 under the same simple calculation to $0.88 off-peak and $1.76 peak.

Actual application costs will vary considerably depending on the ratio of cached input, uncached input and generated output, making those combined figures illustrative rather than universal total-cost estimates.

## **DeepSeek is moving up the agent stack**

The timing makes the strategic direction difficult to miss.

When DeepSeek released the V4 preview on April 24, the major story was how much frontier-class capability the company could deliver with an unusually efficient architecture.

V4-Pro uses a hybrid attention design combining Compressed Sparse Attention and Heavily Compressed Attention; at a one-million-token context, DeepSeek says it requires only 27% of the single-token inference FLOPs and 10% of the KV cache required by V3.2.

By late May, the discussion had shifted toward what those efficiencies meant economically for high-volume agents, whose repeated context reads can make caching a major component of inference costs. DeepSeek’s steep V4 price cuts amplified that advantage.

The Aug. 13 releases move the competition another layer upward.

DeepSeek now has an updated V4-Pro tuned around agent workloads, standardized interfaces designed to make it easier to connect with existing developer tooling, configurable reasoning effort, and an MIT-licensed harness for controlling the models, tools, sandboxes, filesystems and orchestration surrounding an agent.

At the same time, DeepSeek is demonstrating that developers cannot assume its aggressively low API rates are permanent. For organizations considering the platform, workload scheduling, caching behavior and the option to run open weights on their own infrastructure now become more important parts of the total-cost calculation.

That leaves DeepSeek pursuing two potentially conflicting advantages at once: making its agent stack more accessible and open while making its own hosted API considerably more expensive.

For enterprise developers, Harness may ultimately be the more consequential part of Thursday’s announcement. Models can increasingly be swapped behind standardized interfaces. The harness that controls how an agent reasons, invokes tools, edits software and persists across a workflow can be much harder to replace.

DeepSeek is now competing for that layer, too.
