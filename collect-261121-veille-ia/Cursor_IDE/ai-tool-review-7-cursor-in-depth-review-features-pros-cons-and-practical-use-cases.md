---
id: collect-261121-veille-ia/cursor-ide/ai-tool-review-7-cursor-in-depth-review-features-pros-cons-and-practical-use-cases
title: "[AI Tool Review #7] Cursor In-Depth Review: Features, Pros & Cons, and Practical Use Cases"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["review", "cursor", "agent", "tool", "plan", "features", "agents", "composer"]
source: https://medium.com/@piona_log/ai-tool-review-7-cursor-in-depth-review-features-pros-cons-and-practical-use-cases-df1b57ab93d3
source_anchor: ""
source_lines: [1, 98]
section: "Cursor_IDE"
---
# [AI Tool Review #7] Cursor In-Depth Review: Features, Pros & Cons, and Practical Use Cases

## Introduction: The Editor Became an Agent Workspace

Cursor began with a simple question: what if the editor understood the whole repository, not just the file in front of you? That question is now out of date. In November 2026, Cursor is less an editor with AI inside it than a workspace where you direct several coding agents and occasionally open a file yourself.

The release history shows the shift. Cursor 3 in April 2026 made the Agents Window a first-class interface; May brought Composer 2.5, the company’s own agentic model; September introduced Projects, a fleet of agents under one coordinator, which went multiplayer in November. The first week of November alone added app deployment from a cloud agent and real-time voice chat.

That pace is both the appeal and the problem. This review covers what is worth using today, what it costs after the October 12 repackaging into The X Subscription, and where the product still gets in the way.

## Key Takeaways

- **Tab remains the quiet reason to switch.** Cursor’s own Tab model now makes 21% fewer suggestions with a 28% higher accept rate — less noise, better aim.
- **Plan Mode is the highest-leverage habit.** Press Shift+Tab and the agent asks questions, researches the codebase, and writes an editable plan before touching a file.
- **Composer 2.5 is competitive, not frontier.** It scores 69.3% on Terminal-Bench 2.0 and 79.8% on SWE-Bench Multilingual — level with Opus 4.7 on those measures, behind GPT-5.5 on Terminal-Bench, at $0.50/$2.50 per million input/output tokens.
- **Pricing changed shape on October 12, 2026.** Cursor now sells through xAI’s The X Subscription alongside Grok and X: Free, Plus ($8), Premium ($30), Super ($100), Ultra ($200), with legacy Pro ($20) still working for existing subscribers.
- **Cloud agents are where the leverage is — if the environment is ready.** Parallel VMs, multi-repo work, artifacts, and pull requests in one flow; without dependency setup and a test command, results fall apart.
- **Third-party models cost extra.** Claude, GPT, and Gemini are not in the included pool on any tier; from Premium up they are billed at API list price from prepaid credit.

## Core Features

## Tab: predicting the next action, not the next token

Tab is where most people meet Cursor, and it is still the feature used most. It proposes multi-line edits, jumps across files, and follows a refactor through the places it ripples into. Because it reads your current task, recent changes, and open files, the same code can draw different suggestions an hour later.

The interesting part is the trade: a new Tab model suggests 21% less often while acceptance rose 28%. A quieter autocomplete is a better one, because a suggestion at the wrong moment costs attention even when rejected.

## Agent and Plan Mode

The Agent reads the repository, edits files across directories, runs terminal commands, and checks the result with a browser tool, console logs included. The part that changes outcomes is Plan Mode: Shift+Tab rotates into it, and the agent asks clarifying questions, researches the relevant files, and produces a structured plan you can edit directly.

Plans are saved as Markdown and can be moved into the workspace. The habit worth building: when the agent builds the wrong thing, do not patch it with more prompts. Revert, tighten the plan, run again — cheaper than arguing with an agent mid-flight. For work that touches many files or admits several approaches, Plan Mode is close to mandatory.

## Composer 2.5 and the Agents Window

Composer 2.5, released in May 2026, is Cursor’s own model, tuned for long-horizon agentic coding: tool use, file edits, terminal operations, and hundreds of tool calls without losing the thread. On published benchmarks it lands next to frontier models on some axes and behind on others: Terminal-Bench 2.0 has Composer 2.5 at 69.3%, Opus 4.7 at 69.4%, GPT-5.5 at 82.7%; SWE-Bench Multilingual, 79.8%, 80.5%, and 77.8%. The price is the argument — $0.50 per million input tokens and $2.50 per million output, against $3/$15 for the fast variant.

The Agents Window makes several agents practical: it spans local, cloud, and remote SSH, isolates each task in its own Git worktree, and moves a task from cloud to local and back. Long-running work goes to a cloud subagent with **/in-cloud**, and a pull request can sit on **/autopilot** while you continue locally. The classic editor is still there.

## The 2026 layer: cloud agents, automations, Bugbot, Projects

**Cloud agents** run in isolated VMs with a full development environment — cloned repositories, installed dependencies, secrets, startup commands, network access. You can run as many in parallel as your plan allows, from desktop, web, iOS, the CLI, or a GitHub or Bitbucket pull request comment. They open merge-ready PRs with screenshots, video, and logs, and you can take over the agent’s remote desktop to test the change.

**Automations** wake the same machinery on a cron schedule or an event: a pull request, a push, a Slack message, a webhook, a Linear issue. The Automations page also hosts Cursor-managed agents including Bugbot, Security Agents, and PR Routing and Approval.

**Bugbot** reviews pull request diffs, comments with explanations and suggested fixes, and can start a cloud agent to autofix what it found. Billing moved to usage in May 2026, with Cursor quoting an average of $1.00 to $1.50 per review. One trap: project rules in `.cursor/rules` are ignored in Bugbot runs. Review guidance belongs in `.cursor/BUGBOT.md`.

**Projects**, from September, coordinate many agents under one coordinator; since November 10 they are multiplayer, with shared specs, a task board, and agent-to-agent coordination. **Origin**, Cursor’s git forge, is in early beta with an app marketplace of twenty-plus integrations. **Build Live Apps**, from November 5, lets a cloud agent scaffold a web app, provision Postgres, wire auth behind Cursor SSO, and hand you a live link once security checks pass. **Voice chat**, from November 6, runs on Grok Voice.

## Practical Workflow Tips

- **Start with one AGENTS.md.** Add a line only when you see the same mistake twice. Rules written before you understand your own patterns go stale and bloat context.
- **Use Plan Mode for width, not depth.** Multi-file changes and architectural choices benefit; a one-line fix does not.
- **Budget by task, not by model.** Draft and iterate with Composer 2.5, then hand the final judgment to a frontier model. The cost difference over a week is large.
- **Build the cloud environment before the agent.** A Dockerfile or saved snapshot with dependencies installed and tests runnable is the single biggest lever on cloud agent quality.
- **Make Bugbot block what matters.** Findings report neutral by default, so requiring the check only proves it ran. Turn on fail-on-unresolved where available.
- **Keep review guidance separate.** Coding conventions in`.cursor/rules` , review conventions in`.cursor/BUGBOT.md` .

## Free vs Paid

- **Free — $0.** Limited Agent and Chat usage, Tab completions with Cursor’s own models (Grok and Composer), standard X benefits. No third-party models, no own API keys.
- **Plus — $8/month, $80/year.** A base weekly pool for Cursor models at fixed settings (fast off, medium effort). No Auto, cloud agents, automations, Bugbot, Origin, or third-party models.
- **Premium — $30/month, $300/year.** Four times the Plus pool, cloud agents, automations, Bugbot, Origin, Grok Bot, and third-party models at API list price from prepaid credit. This is where legacy Pro subscribers map.
- **Super — $100/month, $1,000/year.** Fifteen times the Plus pool and priority model access at peak times.
- **Ultra — $200/month, $2,000/year.** Fifty times the Plus pool and earliest access to new products and models.
- **Teams — $30, $100, or $200 per seat/month.** Mixable tiers, team rules, a marketplace for internal rules and skills, usage analytics, SSO. Third-party models add a $0.25 per million token Cursor Token Rate.
- **Enterprise — custom.** Pooled usage across Grok and Cursor, SCIM, audit logs, invoice billing.

Two details matter more than headline prices. Included usage is a weekly pool shared with Grok, so one heavy day no longer eats the month. But the pool covers only Cursor’s own models: from the first third-party request you pay API list price from prepaid credit, capped only by a monthly additional-usage limit.

In practice: solo coding with occasional agent work fits Plus. Wanting Claude, GPT, or Gemini inside Cursor means Premium — the $30 tier is the practical entry point in 2026. Super and Ultra make sense only when you run several agents or automations daily.

## Pros and Cons

**Pros**

- Tab is best in class, and Plan Mode genuinely improves output rather than adding ceremony.
- Local, cloud, web, mobile, CLI, and pull-request entry points share one context and one usage pool, so work moves between surfaces without restarting.
- Rules, skills, hooks, and AGENTS.md give teams a versioned way to hold behavior steady as models change.
- Parallel cloud agents with artifacts and remote desktop control are a real capability, not a demo.
- Composer 2.5 keeps high-volume agent work affordable enough to leave running.

**Cons**

- Pricing became harder to reason about: a shared weekly pool, prepaid credit, and separate third-party billing sit on top of a tier ladder.
- Legacy and new plans coexist with different names for similar positions — Pro at $20 versus Premium at $30 — which makes advice and comparison confusing.
- Composer is not frontier. For the hardest reasoning-heavy tasks, the included models are the wrong tool and you will be paying for someone else’s.
- Cloud agents are only as good as their environment. Without setup, they fail in ways that look like model failure but are configuration failure.
- Bugbot ignoring project rules, and findings not blocking merges by default, are sharp edges that surprise new teams.
- The interface moves fast. Weekly releases mean documentation, tutorials, and muscle memory lag behind the product.

## Conclusion

Cursor’s advantage is no longer a model. It is a way of working: write the plan first, encode the rules in files, isolate each task, run agents in parallel, and review the artifacts they produce. That method survives model changes, which is why it is worth learning even if you do not stay on Cursor forever.

Its weakness comes from outside the editor: the October bundle made the product cheaper to try and harder to price, and legacy and new plans will confuse buyers for a while. Start free with Tab and Plan Mode, move to Plus when your hands already reach for the agent, and to Premium the moment you need Claude, GPT, or Gemini.

**Rating: 4.4 / 5** — the tool that set the standard for AI-assisted coding. It now asks you to understand your own usage before you can understand its price.
