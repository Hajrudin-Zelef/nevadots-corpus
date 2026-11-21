---
id: collect-261121-veille-ia/gpt-openai/codex-cli-now-defaults-to-gpt-6-1-sol-what-changes-for-your-agent-setup
title: "Codex CLI now defaults to GPT-6.1 Sol: what changes for your agent setup"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-29"]
keywords: ["model", "gpt-6", "agent", "codex", "changes", "openai", "default", "setup"]
source: https://amargupta.tech/blog/codex-cli-now-defaults-to-gpt-6-1-sol-what-changes-for-your-agent-setup
source_anchor: ""
source_lines: [1, 67]
section: "GPT_OpenAI"
---
On 2026-09-29 OpenAI shipped three Codex CLI releases in one day: 0.159.0, 0.159.1 and 0.159.2. The middle one is the one that matters. Its release notes have a single new feature: "Added GPT-6.1 Sol as the default model in the bundled catalog and Amazon Bedrock Mantle and Runtime catalogs."

That is one line in a changelog, and it changes the model under every Codex setup that updates without pinning one. Since then the CLI has kept moving: 0.160.0, 0.160.1, 0.161.0 and 0.162.0, the last on 2026-10-08. This post covers what the switch means, what the new model costs, the release-note changes worth knowing about, and what nobody has shown yet.

## What happened

The 0.159.1 release made GPT-6.1 Sol the bundled default. 0.161.0 repeats it in its own notes: "GPT-6.1 Sol is now the default model in the bundled and Amazon Bedrock catalogs." So this is not a one-release experiment. It is the new baseline.

OpenAI's changelog describes Sol as "near-Astra performance for complex work at a lower cost than Astra", and recommends it "for repeated, long-running work across code, apps, and documents". TechCrunch reported that it was available the same day to Plus, Pro, Business, Enterprise and Edu users in ChatGPT Work and Codex. The same report says OpenAI scrapped a GPT-6.1 Astra release over safety concerns from internal testing, which is part of why the cheaper model is the one that shipped.

## What the model is, in numbers

These come from OpenAI's model page:

- **Input: $2 per million tokens.** Cached input: $0.1 per million.
- **Output: $10 per million tokens.**
- **Context window: 1,050,000 tokens.** Max output: 128,000 tokens.
- **Knowledge cutoff: Apr 30, 2026.**

TechCrunch adds two comparisons, both from its reading of OpenAI's launch post. Sol's standard input and output price is one-fifth of GPT-6 Astra's. And at low reasoning effort, the share of responses with a factual error went from 11.4% on GPT-6 Sol to 7.7% on GPT-6.1 Sol, with Sol's error rate staying within 1.9% of GPT-6 Astra's across reasoning settings.

The cached-input price is the one to look at twice. An agent that re-reads the same repository and the same system prompt every turn pays mostly for cached tokens. At $0.1 against $2, that is where a long-running coding loop gets cheap.

## How the switch reaches you

A coding agent's behaviour comes from three things: the prompt, the tools and the model. Most of us tune the first two and treat the third as fixed. A default-model change breaks that assumption quietly. Nothing errors. The agent just answers a little differently, makes different calls about when to stop, and writes code in a slightly different style.

The 0.162.0 release adds a detail that decides whether this touches you. Its notes say new TUI threads now "respect server model and reasoning-summary defaults ... while retaining explicit launch overrides." In plain terms: if you name a model when you launch, your choice wins. If you do not, the default does, and the default is now Sol.

So the practical step is short. If your setup was working well and you want it to keep working the same way, pin the model in your config. If you want Sol, you already have it.

## The release-note changes that matter for web developers

The model got the headline, but the releases around it changed how you work with the agent:

- **Steer a turn mid-response.** 0.159.0 added an opt-in`instant_interrupt` that "lets new input steer Codex during model responses or long-running code-mode calls". You can correct the agent while it is still going, instead of waiting for the turn to end and then undoing it.
- **Sign in to MCP servers from the terminal.** 0.161.0 added`/mcp login <name>` . If your agent talks to an MCP server that needs auth, that used to mean leaving the session.
- **Permissions that survive.** 0.161.0 lets an approved filesystem escalation grant broader write access "while preserving denied reads and network restrictions", and background tasks keep the permissions of the turn that started them. 0.159.0 already made approved commands keep filesystem denials, and protects`.aws` directories by default.
- **Managed Git worktrees.** 0.162.0 adds tools for creating and listing worktrees from trusted local projects, when the worktrees feature is enabled. Worktrees are how you run two agents on one repository without them overwriting each other.
- **Line endings left alone.** 0.162.0 preserves existing CRLF line endings in`apply_patch` updates. Anyone who has reviewed a diff where every line changed because of line endings knows why that is listed.
- **Fewer false failures under load.** 0.161.0 and 0.162.0 both make retries honour the server's retry guidance, so an overloaded API is less likely to look like a broken agent.

Something was taken away too: 0.159.0 removed automatic follow-up prompt suggestions and the `tui.prompt_suggestions` setting, along with the bundled `plugin-creator` skill.

## Why someone building web apps with AI should care

There are two reasons, and only one is about Codex.

The first is the one above. If you use Codex CLI for frontend work, the model writing your components changed on 2026-09-29. If your output looks different this week, that is the first place to look.

The second is about your own product. At $2 in and $10 out per million tokens, with a 1,050,000-token context window, Sol is a model you could put behind a long-running agent in your own app. OpenAI's own description says that is what it is for: repeated, long-running work across code, apps and documents. The price matters more than the benchmark here. An agent feature in a web app lives or dies on what each user session costs, and the cached-input rate is what decides that for anything that reads the same context again and again.

## What is not known yet

- **Whether "near-Astra" holds on frontend work.** Nothing published tests Sol on React or Next.js specifically. The accuracy numbers are about factual errors, not about whether a component renders.
- **The benchmark tables.** OpenAI's launch post could not be read for the research behind this post. The accuracy figures above are TechCrunch's reading of it, not a primary table.
- **How to turn `instant_interrupt` on.** It is opt-in, and the release notes do not name the config key or say whether it will become the default.
- **A faster Sol for Codex.** Coverage mentions one arriving "in the coming days". No primary page confirms it yet.
- **Why GPT-6.1 Astra was scrapped** , beyond TechCrunch's one line about deception and acting without permission.

## My take

*This part is my opinion, Amar's, not a fact from the sources.*

I think the default-model switch is the most important thing in these releases, and the least discussed. I run my own daily automation on a coding agent, and the lesson I keep relearning is that an agent setup is only reproducible if every input to it is written down. The prompt is in git. The tools are in git. The model is the one input that can change on an update without a single file in your repository changing. A default is a decision someone else makes for you, and it is fine to accept it, as long as you know you did.

The changes I would actually use are the boring ones: worktrees, permissions that survive a reconnect, and retries that stop calling a busy server a failure. None of them will trend. They are what makes an agent something you can leave running, which is the whole point of a model sold for long-running work.
