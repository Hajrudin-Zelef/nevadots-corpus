---
id: collect-261121-veille-ia/gemini-google/google-jules-alternatives-after-the-shutdown-november-2026
title: "Google Jules Alternatives After the Shutdown (November 2026)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-13"]
keywords: ["google", "jules", "november", "after", "github", "agent", "does", "team"]
source: https://www.agentqueue.dev/alternatives/google-jules/
source_anchor: ""
source_lines: [1, 112]
section: "Gemini_Google"
---
# Google Jules alternatives after the shutdown

Updated November 13, 2026 · first published November 12, 2026 · by the AQ team

Jules stopped taking new work on November 11, 2026 and shuts down on December 31, 2026. If you want to stay with Google, the replacement Google names is Google Antigravity, whose remote agents reached paid Google AI Pro and Ultra subscribers on November 9. If your team already pays for ChatGPT, Claude, GitHub Copilot or Cursor, each of those plans now includes a cloud coding agent that works from a GitHub repository; OpenHands is the open source option you host yourself, and AQ suits teams that want the real agent CLIs running in shared, persistent workspaces on machines they control or on an AQ-managed machine. Every product fact below was checked on the vendor's own pages on November 12, 2026.

## What happened to Jules

Google announced the turndown in the Jules changelog on October 29, 2026. Sessions could be finished until November 11, 2026; after that Jules is read-only, and it winds down entirely on December 31, 2026. Google says the work continues in its agent-first platform, Google Antigravity.

Your code, branches and pull requests are already on GitHub and stay there. What lives only inside Jules is configuration and history: account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts and every session (prompt, plan, activity log and code patches). The same changelog entry explains how to export that to a local folder with the `@google/jules` CLI. Run the export as soon as possible: Google gives no end date for it, and the export reads undocumented endpoints that can fail. The setup scripts and environment variables in it are what you will need to rebuild in the next tool. Our Jules export and migration guide walks through the steps.

## A checklist for choosing a replacement

Use these eight questions on any candidate, including the ones on this page. They come from what Jules did for you and from what tends to break during a move.

1. **Active after November 11.** Can you start new tasks today on the plan you would actually buy? Some cloud agents are still rolling out by plan, region or account type.
2. **GitHub handoff.** Does the tool clone your repository, work on a branch and open a pull request you review like any other? Check whether it can start from an issue or a PR comment if your team worked that way in Jules.
3. **Live steering.** Can you send a message to a task while it runs, and does it act on that message before finishing? Jules's editable plan was a checkpoint before code was written; find out where the equivalent checkpoint sits in the new tool.
4. **Model and CLI choice.** Is the tool tied to one vendor's models, or can it run several? Can you use the agent CLIs your team already pays for?
5. **Infrastructure custody.** Does the code run in the vendor's cloud, on GitHub's runners, or on machines your team controls? Who holds the repository credentials and secrets?
6. **Session persistence.** Does a task keep running when your laptop sleeps, and can you return to the same session later with its history and files intact?
7. **Review.** Where do you read the diff, run the app and leave comments before merging? Does anyone other than the person who started the task see the work?
8. **Pricing.** Is the cloud agent included in a plan you already pay for, how is usage limited, and what does a second seat cost?

## The options at a glance (checked November 12, 2026)

| Option | Where tasks run | GitHub handoff | Steering a running task | Models and CLIs | Entry price for the cloud agent | 
|---|---|---|---|---|---|
| Google Antigravity remote agents | Google-managed remote environment | Connect repositories by OAuth; PR creation not documented on the pages checked | Through the Antigravity app, CLI or web | Gemini by default; Claude models in the app picker (remote agent model choice not documented) | Google AI Pro (paid) or Ultra; not on Free, AI Plus or business accounts | 
| Codex cloud (OpenAI) | A per-task workspace in OpenAI's cloud | GitHub or GitLab; commit or open a PR at review | Follow-up changes in the same chat | OpenAI models | Included in ChatGPT plans; Plus is $20 a month | 
| Claude Code on the web (Anthropic) | An Anthropic-managed VM, or your organization's self-hosted environment | Claude GitHub App or your `gh` login; create a PR when done | Yes; messages queue until Claude reads them | Claude models | Claude Pro, $20 a month ($17 billed annually) | 
| GitHub Copilot cloud agent | A GitHub Actions runner or a cloud sandbox | GitHub only; one branch and one PR per task | Review and refine before the PR; sessions can be shared | Model choice depends on how the task starts | Paid Copilot plans; Pro is $10 a month | 
| Cursor cloud agents | Isolated VMs in Cursor's cloud, or your machines for tool execution | GitHub, GitLab, Azure DevOps or Bitbucket Cloud; `@cursor` on a PR or issue | Follow-ups from the owner, and from teammates if an admin allows it | A curated set of models | Premium, $30 a month, or Teams from $30 per user | 
| OpenHands | Infrastructure you run | Integrations vary by plan | In the Agent Canvas client | Bring your own model key; can run other agents | Agent Canvas is free to self-host; Teams is $500 a month for up to 10 users | 
| AQ | A persistent workspace on your own cloud machine, an AQ-managed machine, or your Mac | Connect GitHub; review, commit and open the PR from the Changes panel | Live terminals and threads teammates can join | The real CLIs (Claude Code, Codex, Cursor, Antigravity and others) plus AQ's built-in agent | Free on your own Mac (one person); Pro $50 per user per month, promotional (standard $200) | 

## Google Antigravity remote agents

Antigravity Remote Agents run agent sessions and tools in an isolated, Google-managed remote environment. The Antigravity changelog records the rollout starting on November 3, 2026 and finishing for Google AI Ultra and paid Google AI Pro subscribers on November 9. The remote agents documentation says they are not available on the free plan, on Google AI Plus, or for Google Workspace and Google Cloud business accounts, and that they require a personal Google Account. You attach GitHub repositories to a project with OAuth, and conversations clone them into the remote workspace; the pages checked do not describe opening a pull request. Remote agents can be started from the Antigravity app, the `agy` CLI (1.4.12 or later, `agy --remote-agent`) or the web. Google One lists the Google AI Pro bundle at $19.99 a month.

Fit: individuals who liked Jules's Google account setup and want the platform Google itself points to. Teams on Workspace accounts cannot use remote agents yet.

## Codex cloud

Codex cloud gives each task its own workspace that keeps working while your computer is asleep. It checks out repositories from your connected GitHub or GitLab account, and when you review the result you can request follow-up changes, commit, or open a pull request. The Codex pricing page lists Codex in ChatGPT Plus at $20 a month with Codex on the web, and notes that cloud tasks and local messages share one usage allowance and that cloud tasks may use more of it.

Fit: teams already on ChatGPT plans that want Jules's fire-and-review pattern with OpenAI models.

## Claude Code on the web

Claude Code on the web runs each session in an isolated, Anthropic-managed VM, or in your organization's self-hosted environment when it is routed there. You connect GitHub through the Claude GitHub App or by sending your local `gh` login, steer the session from the browser or the mobile app (a message sent mid-task waits until Claude reads it), create a PR when it finishes, or move the session to your terminal with `claude --teleport`. Cloud sessions are included in the Pro, Max, Ultra and Team plans; Claude pricing lists Pro at $20 a month, or $17 a month billed annually. Organizations using Zero Data Retention, HIPAA or customer-managed keys cannot use cloud sessions.

Fit: teams that work in Claude Code already and want the same agent in the cloud, with a path back to a local terminal.

## GitHub Copilot cloud agent

Copilot's cloud agent works inside GitHub's issue and pull request workflow. Sessions run on a GitHub Actions runner (one repository per task, at most 59 minutes per session, with support for self-hosted runners) or in a cloud sandbox that can span repositories in one organization. Copilot works on one branch and opens one pull request per task, and only for repositories hosted on GitHub. GitHub's documentation says the newer Copilot in the cloud experience has been available to paid Copilot plans since October 28, 2026; the Copilot plans page lists Pro at $10 a month and Business at $19 per seat.

Fit: teams that want the work to start and end in GitHub with no new tool, and whose tasks fit inside an Actions session.

## Cursor cloud agents

Cursor cloud agents run in isolated VMs with full development environments. You can start one from the Cursor apps, the web, Slack, Linear, the API, or by commenting `@cursor` on a GitHub pull request or issue, and the agent pushes a branch for handoff. The owner can send follow-ups, and teammates can too when an admin turns on team follow-ups. Self-Hosted Machines move file edits and terminal commands onto your own workers while Cursor keeps the agent loop and inference; Team Pools need Enterprise. Cursor pricing lists cloud agents from the Premium plan at $30 a month and Teams from $30 per user, with third-party models billed by usage on top.

Fit: teams that already work in Cursor and want cloud agents next to the editor.

## OpenHands

OpenHands is an open source agent platform. OpenHands pricing lists Agent Canvas as free to self-host for a single user with your own model key, and Teams as self-hosted at $500 a month for up to 10 users with multi-user access control and SSO. A managed OpenHands Cloud also exists; its price is not on that page. Running it yourself means you also own upgrades, sandboxes and model spend.

Fit: teams for whom self-hosting and an open license are requirements after watching a hosted agent shut down.

## Where AQ fits

AQ is the multiplayer coding harness for running coding agents in isolated workspaces with shared live access and review on team-controlled or AQ-managed machines. Each task is a work item with its own git branch and worktree, and you drive it with the real agent CLIs your team already signs into (Claude Code, Codex, Cursor, Antigravity, Kimi Code, Grok and others) or with AQ's built-in agent. Sessions persist on the machine, so a task keeps running after you close the laptop, and a teammate can open the same work item to watch the terminal, read the conversation, try the live app preview and send the agent a message. Review happens in the Changes panel: diff scopes, line comments that reach the agent, and the pull request with its checks.

AQ does not import Jules sessions and does not migrate anything from Jules automatically. Your repositories come over by connecting GitHub; the setup script and environment variables from your Jules export become the repository's setup command and secrets in AQ, which you enter yourself. AQ pricing: Free runs agents on your own Mac with the AQ Mac app, for one person; AQ Pro is $50 per user per month (promotional, standard $200) and adds your team, sharing, your own cloud and an always-on AQ-managed machine.

Fit: teams that want several agent CLIs in one place, sessions that stay put, and teammates in the same live workspace. If you want a single vendor's agent that runs and reports back, the options above are simpler.

## A practical first week after Jules

1. Run the Jules export (see the export and migration guide) and keep the folder. List each repository's setup script, environment variables and scheduled tasks, since none of them move on their own.
2. Pick two finalists from the checklist above, usually one you already pay for and one that changes what bothered you about Jules.
3. Give both the same small, known-answer ticket on one repository, with the same setup script and secrets. Record how long setup took and how often you had to step in.
4. Review both pull requests through your normal CI and human review, and keep branch protection unchanged.
5. Recreate scheduled tasks last, once the agent has handled one-off tickets well.

For the wider field of cloud coding agents, see the cloud coding agents guide.

## Frequently asked questions

### Is Google Jules shutting down?

Yes. According to the Jules changelog entry of October 29, 2026, sessions could be finished until November 11, 2026, Jules is read-only after that, and it winds down on December 31, 2026.

### What does Google recommend instead of Jules?

The turndown notice names Google Antigravity, Google's agent-first platform. Its remote agents, which run in a Google-managed environment, finished rolling out to Google AI Ultra and paid Google AI Pro subscribers on November 9, 2026. They are not available on the free plan, Google AI Plus or business accounts.

### Can I export my Jules data?

The Jules changelog describes exporting account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts and every session to a local folder with the @google/jules CLI. Run it as soon as possible: Google gives no end date for the export, and it reads undocumented endpoints that can fail. Code, branches and pull requests are already on GitHub.

### Which Jules alternative works with the subscription I already have?

Codex cloud is included in ChatGPT plans, Claude Code on the web in Claude Pro, Max, Ultra and Team, Copilot's cloud agent in paid Copilot plans, and Cursor cloud agents from Cursor Premium and Teams. AQ runs Claude Code, Codex and Cursor CLIs signed in with each person's own account.

### Does AQ import Jules sessions?

No. AQ has no Jules importer. You connect the same GitHub repositories and enter the setup command and secrets from your Jules export yourself.
