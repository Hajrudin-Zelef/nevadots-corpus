---
id: collect-261121-veille-ia/claude-anthropic/codex-vs-claude-code-2026-which-coding-agent-to-run
title: "Codex vs Claude Code 2026: which coding agent to run"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "code", "codex", "july", "import", "default", "chatgpt", "model"]
source: https://hashnode.com/blog/codex-vs-claude-code-2026
source_anchor: ""
source_lines: [1, 131]
section: "Claude_Anthropic"
---
**tldr:** Run Claude Code if you want the more settled agent, since its subagents, sandboxing, and auto mode are already defaults rather than opt-in previews. Run Codex if your team lives in ChatGPT or you need voice. The reason this choice got easier in July 2026 is that Codex can now import your entire Claude Code setup in one command, so picking wrong costs far less than it used to.

## Codex vs Claude Code at a glance

|  | Codex | Claude Code | 
| Best for | Teams already in ChatGPT, voice-driven work | Long agentic runs, tight sandboxing | 
| Default model | GPT-5.6 family, Sol on Bedrock | Sonnet 5, Opus 5 as the default Opus | 
| Context | 1.05M tokens | 1M tokens | 
| Sub-agents | Multi-agent V2, opt-in, configurable models and concurrency | Background by default, nested to depth 3, 200 per session | 
| Extensibility | MCP servers, plugins, commands, skills, AGENTS.md | MCP servers, plugins, skills, subagents, CLAUDE.md | 
| Sandboxing | Sandboxed commands | Filesystem isolation and network allowlist, both configurable | 
| Migration | `/import` pulls in a Claude Code or Cursor setup | No equivalent import | 
| Key limitation | Several flagship features still labelled experimental | No path in from a Codex setup | 

## What changed in July 2026?

Three releases inside four days, and together they explain why this comparison is worth redoing.

**Codex CLI 0.145.0, 21 July.** The changelog is unusually direct about the headline: `/import` now migrates "Cursor and Claude Code settings, MCP servers, plugins, sessions, commands, and project-scoped memories." It also added experimental Amazon Bedrock login with custom endpoint support and GPT-5.6 Sol as the default Bedrock model, stabilised an opt-in multi-agent V2 with configurable sub-agent models, reasoning levels and concurrency, and added experimental paginated thread history.

**ChatGPT desktop 26.715, 23 July.** ChatGPT Voice, powered by GPT-Live, lets you talk through work across Chat, Work, and Codex in one desktop app. Local projects gained multi-folder support, where a designated primary folder drives new chats, Git operations, and AGENTS.md, skills, and config.toml discovery.

**Claude Code 2.1.219, 24 July.** Opus 5 arrived as the default Opus model with a one-million-token context and Fast mode at $10 and $50 per million tokens. Subagents gained nested spawning to depth 3, up from 1. A `sandbox.network.strictAllowlist` setting now denies non-allowlisted hosts for sandboxed commands without prompting.

One correction while we are on dates, because a lot of coverage got it wrong: GPT-5.6 launched on 9 July, more than two weeks before any of this. The 24 July news was Codex and ChatGPT converging, not a new frontier model.

### The buried lede is `/import`, and it runs one way

Everything else in that list is incremental. `/import` is not, because it changes the shape of the decision. Migrating settings, MCP servers, plugins, sessions, commands, and project-scoped memories in one command means the cost of having chosen the other tool is now close to zero.

It is also one-directional. Codex imports a Claude Code setup; Claude Code has no equivalent import. So the question stopped being "which one do I commit to" and became "which one do I default to," which is a far lower-stakes question, and OpenAI has made itself the easier destination.

## How we are comparing them, and what we did not do

This is a comparison of documented capabilities and an argued recommendation. It is not a benchmark. We have not run the same task through both agents under controlled conditions, and we are not going to imply otherwise.

That matters because a casual side-by-side is close to worthless here. A real head-to-head would need to hold the repository state, the task, the model tier, the permission mode, and the context budget constant, and then run enough trials to separate signal from the variance you get on any single agentic run. Most published comparisons control for none of that.

What you can compare honestly is what each vendor ships, what it calls stable, and what it charges. That is the ground this piece stands on.

## Which one is more settled?

Claude Code, and the evidence is in how each vendor labels its own work.

Codex's flagship July additions carry OpenAI's own hedges. Bedrock login is "experimental." Paginated thread history is "experimental." Multi-agent V2 is "opt-in," described as stabilised rather than default. These are capable features arriving with the vendor's caution attached.

Claude Code shipped the equivalents earlier and has already moved them into the default path. Subagents have run in the background by default since 1 July. Auto mode became available on Bedrock, Vertex AI, and Foundry without an opt-in environment variable on 11 July. Nested subagents went from depth 1 to depth 3 as a default on 24 July, with a per-session spawn cap of 200 to stop runaway delegation.

Read the two changelogs side by side and the pattern is consistent: Anthropic is tuning limits on features that already run by default, while OpenAI is still promoting its equivalents out of preview. If you are choosing an agent to run unattended on real work, which is the premise of an AI software factory, that difference matters more than any feature checklist.

## How does each handle a large repo?

Both now carry roughly a million tokens of context, so the interesting question is not window size but what happens at the edges of a monorepo.

Codex's multi-folder projects are the more explicit answer. You designate a primary folder, and that folder drives new chats, Git operations, and where Codex looks for AGENTS.md, skills, and config.toml. Secondary folders stay searchable and editable. If your work spans several repositories that need to be treated as one project, that maps cleanly onto how you already think.

Claude Code approaches it through working directories rather than project definitions. `/add-dir` registers additional roots mid-session, a `DirectoryAdded` hook fires when that happens, and the session's extra directories are exposed to MCP servers through `roots/list` with change notifications. It is the more programmable model and the less declarative one.

Neither removes the real constraint, which is that a million-token window you actually fill is expensive. That is where effort settings and caching decide your bill, and we worked through what those do to real numbers separately.

## How do you extend them?

The two extension stories have converged to the point where the file names are almost the only difference. Both support MCP servers, plugins, reusable skills, and a repo-level instructions file, AGENTS.md for Codex and CLAUDE.md for Claude Code. `/import` moving MCP servers and plugins across is only possible because the models are so similar.

Where they still differ is in the operational detail around delegation. Claude Code's skills can declare `context: fork` and run in the background by default, MCP tool calls that exceed two minutes move to the background automatically so the session stays usable, and subagents that were spawned with an explicit model override keep that override when resumed. Codex's multi-agent V2 exposes sub-agent models, reasoning levels, and concurrency as configuration, which is the more legible surface if you want to tune a fleet by hand.

We have shipped a real skill against the Hashnode GraphQL API, and the thing that decided the platform was not the feature list. It was whichever ecosystem already had the servers we needed. For most teams that will be the tiebreaker too.

## What does each cost to run?

Both are sold as subscriptions with metered API access underneath, so the honest answer is that your bill depends on the model you run and how many tokens it spends, not on the agent wrapper.

Two concrete numbers worth knowing. Claude Code's Fast mode on Opus 5 and Opus 4.8 bills at $10 and $50 per million tokens, exactly double standard rates. And Codex now defaults to GPT-5.6 Sol on Bedrock, which is the more expensive of the two vendors' near-frontier models on output, $30 against Opus 5's $25.

Everything else is a routing decision rather than a product decision. Our LLM API pricing comparison has the current verified rates for every model either agent can drive, including the long-context surcharges that make repository-scale requests cost very differently across vendors.

## Do you have to choose at all?

For a lot of teams, no, and `/import` is what changed that. Running both and defaulting to one is now cheap enough to be a reasonable strategy rather than an indulgence.

The stronger version of that idea is to stop choosing per team and choose per task. Buildful, which we build, does a version of this: it picks an open-weight model for each task by default, and on paid plans you can run a task on Claude Code or Codex yourself, with prepaid credit or your own API key. Either way it returns a tested pull request on your GitHub repo. We compare it with Claude Code and Codex separately. If your reason for picking one agent is that you did not want to maintain two, that constraint is worth re-examining.

## Which one should you use?

Four cases, four answers, each with the trade-off it accepts.

**Solo developer on a large monorepo: Claude Code.** Background subagents, nested delegation, and configurable filesystem isolation are defaults rather than flags, which is what you want when a long run is unattended. You accept a less declarative multi-repo story than Codex's primary-folder model.

**Team standardised on ChatGPT: Codex.** Voice across Chat, Work, and Codex in one desktop app is a real workflow, not a demo, and fighting your organisation's existing tooling to gain marginal agent maturity is a bad trade. You accept that several features you will rely on are still labelled experimental.

**Cost-sensitive CI and background automation: Claude Code.** The cheaper near-frontier output rate and a batch discount that stacks with caching decide this, and the effort parameter gives you a spend dial that does not require changing models. You accept managing the sandbox configuration yourself.

**Already invested in the other one: stay, then try `/import` if you are on Claude Code.** The migration path exists in exactly one direction, so Claude Code users can evaluate Codex over an afternoon while Codex users face a manual rebuild. Evaluate accordingly, and note the asymmetry is a fact about tooling rather than a verdict about quality.

## Key takeaways

- Codex can import your Claude Code settings, MCP servers, plugins, sessions, commands, and memories in one command. Claude Code has no equivalent, so switching cost collapsed in one direction only.
- Claude Code is the more settled agent by each vendor's own labels: its subagents and auto mode are defaults, while Codex's Bedrock login, thread history, and multi-agent mode are still experimental or opt-in.
- Both carry roughly a million tokens of context, so the differentiator is multi-repo handling, not window size.
- Extension surfaces have converged on MCP, plugins, and skills. Pick on which ecosystem already has what you use.
- The agent does not set your bill. The model and the token count do.

## FAQ

### Is Codex better than Claude Code?

Neither is better across the board. Claude Code is the more settled of the two, since its delegation and sandboxing features are defaults rather than previews. Codex wins when your team already works in ChatGPT or wants voice-driven workflows. The question also matters less than it did, because Codex can import a Claude Code setup in one command.

### Can you use Codex and Claude Code together?

Yes, and it is more practical since the 21 July 2026 `/import` change. Codex will pull across settings, MCP servers, plugins, sessions, commands, and project-scoped memories from a Claude Code install, so running both and defaulting to one no longer means maintaining two configurations by hand. The reverse migration has no equivalent command.

### Does Codex support MCP and skills?

Yes to both. Codex works with MCP servers and plugins, and its desktop app discovers AGENTS.md, skills, and config.toml from a project's designated primary folder. Its `/import` command explicitly carries MCP servers and plugins across from a Claude Code or Cursor setup, which is the clearest signal of how closely the two extension models now match.

### Is Codex free?

Codex access comes with paid ChatGPT plans rather than as a standalone free product, and voice features are listed for Plus, Pro, Business, Edu, and Enterprise on desktop. Beyond the subscription, what you actually spend depends on the model doing the work; our pricing comparison has the current per-token rates.

### Which is cheaper for daily use?

It depends on the model rather than the agent. On near-frontier work Claude Opus 5 costs $25 per million output tokens against GPT-5.6 Sol's $30, and Anthropic does not surcharge long-context requests while OpenAI charges 2x input above 272,000 tokens. For repository-scale work that gap widens considerably.

### Which has better sub-agents?

Claude Code's are further along. They run in the background by default, nest to depth 3, and carry a per-session spawn cap to prevent runaway delegation. Codex's multi-agent V2 is opt-in but exposes sub-agent models, reasoning levels, and concurrency as explicit configuration, which some teams will prefer for tuning.

## About the author

1. “Claude Code is more settled; Codex wins if you’re already in ChatGPT” matches what I’m seeing. The /import escape hatch is underrated — it makes the choice reversible, which is rare in agent tooling. Still: nested subagents + sandbox defaults matter more than feature checklists once runs go unattended. ByteForward’s TypeSafe/coding deep dive is useful context for agent-tooling taste — https://www.youtube.com/watch?v=GpAm9RypwyA
