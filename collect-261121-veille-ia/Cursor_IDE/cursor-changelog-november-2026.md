---
id: collect-261121-veille-ia/cursor-ide/cursor-changelog-november-2026
title: "Cursor Changelog (November 2026)"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agent", "chat", "cursor", "voice", "agent-sdk", "november", "tools", "start"]
source: https://www.gradually.ai/en/changelogs/cursor/
source_anchor: ""
source_lines: [1, 334]
section: "Cursor_IDE"
---
## Latest Releases (87)

November 6, 2026

- â¢Voice chat works in the Agents Window, the IDE, on web, and in Cursor for iOS.
- â¢When the agent is idle and the chat input is empty, click the waveform button labeled **Start voice chat** . It sits where Send usually is, next to the microphone button for dictation.
- â¢On iOS, you can also say "Hey Siri, call Cursor" to start a call with a new agent, and mute or hang up from the Lock Screen with a Live Activity.
- â¢The voice agent listens, replies, and hands coding work to the agent in your chat. While that agent runs, you can keep the conversation going. Ask what changed in a diff, check on a run, search earlier messages and calls with this agent, or switch between workers in a Project that has them.
- â¢The coding agent speaks for itself too. It shares short progress notes while it works and a brief answer when it's done, so you usually hear a short update instead of the full written reply.
- â¢Voice chat runs on Grok Voice. Pick from many voices in Voice settings, including Ara, Eve, Leo, Rex, and Sal. With Privacy Mode on, voice chat runs with zero data retention.
- â¢Voice chat is separate from dictation, which earlier releases called Voice Mode. Dictation still turns your speech into text in the chat input, and Cmd+Shift+Space still toggles it in the IDE. Voice chat is a two-way conversation where the agent answers out loud.
- â¢Update Cursor to try voice chat. Learn more in our docs.

November 5, 2026

- â¢Internal ops tools for event registration, account assignments, and desk seating
- â¢Multimedia presentations for customers that combine slides, videos, and interactive demos
- â¢Training, enablement, and onboarding experiences for new hires
- â¢Live dashboards and tools for sales teams to understand their pipeline and opportunities
- â¢Portals to manage engineering services

October 20, 2026

- â¢Run `npx @cursor/agent-sdk init ./my-bot` , then`agent-sdk dev` to start the bot locally with a playground. Pass`--template` to start from a security reviewer, an owners-policy approver, a PR autofixer, or a Linear or Jira triager.
- â¢When you're ready to put an agent into use, you can deploy it to Cursor by running `agent-sdk deploy` from the project directory. The command builds the agent, gives it a URL, and starts it on our infrastructure. You can manage secrets and integrations from the dashboard, while continuing to use the same logs, evals, and playground available locally.
- â¢An agent is a project. `instructions.md` is the prompt. Each file in`tools/` is one typed action the model can call.`skills/` holds procedures the model loads on demand,`mcp-connections/` adds tools from MCP servers, and`channels/` defines where the bot receives and answers messages.`agent-sdk validate` checks the project and`agent-sdk info` prints what was discovered.
- â¢Agents wake on pull request, comment, and CI events from GitHub, GitLab, and Bitbucket; on mentions and DMs in Slack; on webhooks from any provider; and on a cron schedule. GitHub events pull through your Cursor account, so no public webhook URL is required. Slack is well-supported out of the box.
- â¢Each conversation keeps its own session and a complete record of every message and tool call. Follow-ups on a pull request or Slack thread resume the same session.
- â¢An eval sends a fixed message to the bot and asserts on what it did: which tools ran, with what input, and the shape of the reply. Run `agent-sdk eval` locally or in CI.
- â¢`agent-sdk github replay` turns a real pull request into signed webhook payloads and sends them to the bot, so GitHub-driven bots can be tested without waiting for a push. Add`--dry-run --fixtures-dir fixtures/` to save the payloads as eval fixtures.
- â¢The Agent SDK is generally available today. Read the docs and get started.

October 16, 2026

- â¢Learn more about sharing canvases.
- â¢The Environments API is now available in beta. Use it to programmatically create and manage the cloud environments your agents run in.
- â¢Cloud agent environments now support up to 300 repositories. Large environments also load 5x faster.

October 13, 2026

- â¢**Deployments:** Cloudflare, Netlify, Render, Spacelift, and Vercel
- â¢**CI:** Buildkite, CircleCI, CloudBees, Depot, EngFlow, Namespace, and RWX
- â¢**Code review:** CodeRabbit, Greptile, and Tenki
- â¢**Security:** Artemis Security, Semgrep, Socket Security, and Upwind
- â¢**Monitoring:** Sentry
- â¢**Project management:** Linear
- â¢Create an app in your namespace. It stays private to your namespace while you build.
- â¢Build against the Origin API. Apps authenticate with an app JWT and installation access tokens, receive webhook events, and write back check runs, pull request comments, and reviews.

## Show more (2)Show less

- â¢Make the app public, then submit it from the app's page with a slug, suggested tags, and a demo video. The video should show install, uninstall, and your core product flows.
- â¢We review the submission and list the app once it's approved.

October 6, 2026

- â¢Remote Control is on by default for everyone except Enterprise organizations.
- â¢Download the Cursor iOS app and sign in. Computers on your account appear in the app automatically. Tap your computer in the app and approve your pairing request in the Cursor desktop app. Your local agents appear in a list. Tap one to see what it's doing or send it a message.
- â¢Remote Control for local agents in the Cursor iOS app
- â¢Remote control doesn't move your agents anywhere. It keeps running on your computer, and the app connects to it. Your computer needs to stay on and online for remote control to work.
- â¢To stop your computer from sleeping while you're away, turn on "Keep this computer awake" under Remote Control in Cursor's desktop settings. Your computer needs to be plugged in with the lid open.
- â¢Remote control is on by default, except for Enterprise organizations. Enterprise admins can turn it on in Org settings > Security and identity > Remote control.
- â¢Remote control doesn't require cloud agents to work.
- â¢Remote control is available today in the Cursor iOS app.

September 23, 2026

- â¢Both are available today on Teams and Enterprise plans.
- â¢Rollouts attaches a monitor to every pull request and watches the change as it deploys, reporting change health per environment: verified healthy, regression detected, or inconclusive. It's the Cursor version of Firetiger Change Monitors, rebuilt with the Bot Development Kit.
- â¢Enable it from the dashboard and connect source control, your deploy system, and your telemetry provider. Rollouts starts watching on the next pull request.
- â¢When a pull request opens, Rollouts reads the diff and the systems it touches, then writes a monitoring plan as a PR comment. The plan lists the risks it identified, the effect the change is meant to have, the signals it will check, and any gaps in instrumentation that would make the change hard to verify. Edit the plan in the PR and Rollouts uses your version.
- â¢Rollouts wakes on deploy events for the change's commit and runs the plan against your logs, metrics, and traces. It tracks each environment separately, so a change can be verified in staging and still flagged in production. Rollouts checks the change's intended effect alongside error and latency signals, and reports back on the PR when it reaches a verdict.
- â¢When Rollouts detects a regression, it names the change it suspects and notifies the author. Depending on configuration, it can also open a revert PR for review or hand the finding to a cloud agent for a fix. Rollouts does not merge or roll back on its own today.
- â¢Rollouts connects to Origin or GitHub for source control, to your continuous delivery system for deploy events, and to Datadog and other telemetry providers for signals. Feature flag integration is coming soon.
- â¢Security Review is available today. It reads every pull request in the context of the codebase and posts one review comment reporting exploitable bugs. Style and quality stay with Bugbot.

## Show more (7)Show less

- â¢Security Review comment on a pull request reporting an exploitable bug with a severity and proposed fix
- â¢Enable it from the dashboard for the repositories you want reviewed. Draft PRs are skipped.
- â¢Security Review looks for injection across SQL, command, and template surfaces, along with authentication and authorization bypasses, including checks that a refactor stopped running. It also flags secrets and credentials committed to source, SSRF and unvalidated redirects, unsafe deserialization, and dependency changes that introduce known vulnerabilities. It traces where user input enters and what it passes through.
- â¢Each finding carries a severity, the attack path, and a proposed fix. Dismiss one with a reason and Security Review won't raise it again on that PR.
- â¢Add rules for your codebase, such as which client external calls must go through or which tables are never queried from a request handler, and Security Review enforces them on every PR.
- â¢Rollouts and Security Reviewer are available today on Teams and Enterprise plans. Enable either bot from the automations tab.
- â¢For the next 10 days, we're including usage credits so teams can try Rollouts on real changes. Teams and Enterprise customers receive credits for roughly 50 and 500 changes, respectively.

September 10, 2026

- â¢Access Projects from the left-hand nav. The coordinator agent in a project doesn't write code itself; it plans the work, delegates it to agents that implement it, and brings the finished work back to you to check. Coordinators create and manage agents on your behalf, running as many in parallel as the work needs.
- â¢A Project runs on its own computer in the cloud, so closing your laptop doesn't stop it. When something needs testing on your machine, the coordinator spins up a local agent to run it there.
- â¢You shouldn't have to onboard an agent every time you start a task. Each Project maintains a set of files that sync across every cloud and local machine its agents use. Agents add research and artifacts, along with what they learn about the codebase and how you prefer work to be done. If one agent figures out how to test a service, for example, every future agent can use those instructions. The shared context grows with the Project, making the coordinator more effective over time.
- â¢Tell the coordinator agent to watch a Slack channel, run on a schedule, or follow all your PRs. This way it can take action based on signals it detects, without waiting for your prompt.
- â¢Connect Slack and point it at a bug-report channel, and it starts delegating each time a bug comes in.
- â¢Projects are available in beta and rolling out to all users starting today.

September 2, 2026

- â¢Your codebase, build outputs, and secrets all stay on internal machines running in your infrastructure, while the agent handles tool calls locally.
- â¢Run on menu showing Remote Machines with self-hosted options lambda-test, cloud-demo, and jacks-desktop
- â¢**My Machines** connects a single laptop or VM to your account for personal workflows.
- â¢**Team pools** are named queues of workers for a team or enterprise. Capacity can grow as requests arrive and shrink when workers disconnect, so your self-hosted machines can scale with demand. Pools are not tied to one repository: name the pool, and any available worker can claim the request.
- â¢Pools can also hibernate idle machines, then restore within a reconnect window when a follow-up arrives, so you don't keep expensive capacity warm just for the next prompt.
- â¢Cloud Agents dashboard listing Self-hosted Machines pools with active and idle worker counts
- â¢Cloud agents can now execute on infrastructure you already use, including from AWS Lambda, Coder, Cloudflare, Daytona, Modal, Namespace, Vercel, and E2B.
- â¢Self-hosted workers now support computer use on Linux and Mac. With the right desktop packages, an agent can click, type, take screenshots, and drive the browser. You can watch its desktop or take control from Cursor.

August 27, 2026

- â¢Select **Start from scratch** in the repo picker, then prompt the agent. In the background, Cursor creates an Origin repo for you.
- â¢Start from scratch in the repo picker
- â¢Once the agent's build is to your liking, click the **Create repo** button to save your work in an Origin repo. Choose a custom name or pick one of the suggested ones, then set the visibility to private or internal. You'll get a fully scaffolded Origin repo, ready to share or keep building on. Navigate to the Codebase tab to find your repo and access your project.
- â¢Cursor now port-forwards your cloud agent's live environment straight to your browser, so you can preview it and use tools like design mode.
- â¢Connect a Vercel account and hit publish to get a live URL for what you built.
- â¢A Vercel account is required to use the publish feature.
- â¢Get started today.

August 19, 2026

- â¢With this release, cloud agents can automatically pick up work in response to events, hold a goal until it's met, and stay on course through long-running sessions.
- â¢Cursor can now monitor your PRs, watch a Slack thread, or run scheduled tasks. Cursor Agent subscribes to an event source (a thread or conversation) and wakes when something happens. Subscriptions are available for cloud agents only, for now.
- â¢Cloud agents automatically subscribe to PRs they create and drive them to completion, fixing CI and addressing bot comments. In Slack, ask `@cursor check back in an hour and keep going until that feedback is in` .
- â¢Use any skill as a Custom Mode: a skill that stays pinned in the chat. Custom modes keep agents focused on a skill - you can think about it like "always on" skills.
- â¢From `/` , pick a skill and press â¥â (Mac) or Alt+Enter (Windows), or choose**Use as Mode** .
- â¢Subagents can now run on their own virtual machines. Each gets an isolated copy of the project with clean context in its own cloud environment.
- â¢Have subagents test the parent agent's changes in fresh environments or swarm independent fixes without collisions. Try `run a swarm of subagents to test my app for bugs, each in its own environment` .
- â¢Use `/goal` to give the agent a long-lived objective to work towards until it's fully complete.

## Show more (3)Show less

- â¢Try `/goal fix all flaky tests and make CI green` in a new chat. Pair it with a custom mode to follow a playbook, or`/loop` for recurring check-ins.
- â¢You can now send a message to steer the agent while it's working without interruption. Follow-ups wait for the next tool call instead of cutting the agent off mid-action.
- â¢Type a follow-up and hit Send now, or press â twice.

August 17, 2026

- â¢Origin begins rolling out today in early beta on all paid plans. We're starting with the essentials, designed for agent scale: repos, pull requests, code browsing, and GitHub sync. Agent-native features ship soon.
- â¢The new **Codebase** tab is home for Origin repos.
- â¢Click **+New** to create a new repo and name it. Once you do, a page shows you how to install the CLI, with commands for how to clone a repo or push a local project. Push, and your code is hosted on Origin.
- â¢For Origin-hosted repos, Origin is the source of truth. Pushes land on Origin, and GitHub is not in the path.
- â¢Name your codebase when you create your first repo. That name becomes part of every repo's URL: cursor.com/codebase/`acme-corp` .
- â¢Your GitHub repos can sit alongside the ones Cursor hosts. Connect GitHub to Cursor, pick your org, and you'll see the repos you can sync. Select one and Cursor pulls it in. You choose what gets synced and can disconnect a repo at any time. Anyone with read or write access to a synced repo can view it in Cursor too.
- â¢Synced repos update in real time. Browse, search, and pull from the copy in Origin. For synced repos, GitHub stays the source of truth: pushes keep going to GitHub, and Origin mirrors the result. Icons next to each repo name tell you which ones Cursor hosts and which came from GitHub.
- â¢Every repo has pull requests. Open one to see the timeline, commits, checks, and files changed. Review the diff, leave comments, and merge.

## Show more (7)Show less

- â¢Pull requests on synced repos sync both ways: comment in Cursor and it posts to GitHub, react or reply on GitHub and it shows up in Cursor within seconds. Got a review assigned to you on GitHub? Review and merge it from Cursor.
- â¢Your code, PRs, and agents are now in the same place. Ask Cursor questions about code you're browsing. It can answer, make changes, update PRs, or push a branch.
- â¢We're building an app ecosystem so your whole stack works seamlessly with Origin. Integrations with Vercel, Depot, and Buildkite are already available, with more coming soon.
- â¢Connect Vercel from a repo's **Apps** tab and every PR gets a preview deployment where you can test and make comments. Merge, and it ships to production. For CI, connect Depot or Buildkite. Both run your existing GitHub Actions workflows and Buildkite also runs its native pipelines.
- â¢Vercel, Depot, and Buildkite apps connected to an Origin repo
- â¢Every repo has settings. Check sync status for GitHub repos, manage who has access, and see which apps are connected.
- â¢Origin repo settings showing sync status, access, and connected apps

August 13, 2026

- â¢This release introduces **builds** : ready-to-use copies of your development environment that Cursor prepares in the background. Agents boot into a ready environment instead of setting up from scratch each session. Builds are included with Cloud Agents at no additional cost.
- â¢Cursor runs a new build of your environment regularly. When a build succeeds, it becomes the environment future agents start from. Cursor keeps warm copies ready so the next agent does not wait. Internally, our environments now boot 10x faster, with 3x faster time to first token.
- â¢Use your install command for anything that can be prepared ahead of time. The start command still runs when you first prompt an agent and should focus on services that need to be fresh in the session.
- â¢When a bad commit or dependency update breaks your environment, agents keep using the last successful build. The broken build never becomes active, you are notified of the issue, and your agents keep working while you debug in the background.
- â¢Each environment has a **Builds** tab in the Cloud Agents dashboard. You can inspect build status, logs, commit SHAs, and which build each agent run used. Agents can also inspect and manage builds using built-in tools.
- â¢New environments use Builds automatically. For an existing environment, open it in the Cloud Agents dashboard, go to the **Builds** tab, and click**Enable Builds** . Or click**Run setup agent** first to test the migration and review any proposed config changes.
- â¢You can also trigger a Build manually, debug a failing Build with an agent, and control how stale Builds refresh with a configurable threshold.
- â¢Learn more in our announcement post and docs.

August 3, 2026

- â¢**Google Drive:** search files and folders, open and download content, create and organize files
- â¢**Gmail:** search and read mail, draft and send messages, apply labels and manage threads
- â¢**Google Calendar:** read schedules, create and update events, find free time

July 28, 2026

- â¢**Generous access to Cursor models:** Grok and Composer, with enough usage to build with agents every day.
- â¢**Always-on cloud agents** that build, test, and ship code while you keep working.
- â¢**Cursor for iOS** with remote control, so you can launch and steer agents from your phone.
- â¢**Plugins, MCP servers, hooks, and skills** to extend Cursor across your workflows.
- â¢**Local pricing** at â¹649 per month, tax inclusive, billed in INR with UPI or card.

July 22, 2026

- â¢**Intelligence:** Frontier quality, matching the most expensive and powerful models that might be out of reach for daily use.
- â¢**Balance:** Strong quality, matching the frontier models that most people like to daily drive.
- â¢**Cost:** Good quality, reaching the highest available intelligence while optimizing token spend.
- â¢Per-request classification by task type and complexity
- â¢Optimization modes: Cost, Balance, and Intelligence
- â¢Admin controls: per-team and per-group enablement, mode restrictions, default mode, and model allow and block lists
- â¢Routed model can be displayed or hidden (hidden by default)
- â¢Soft and hard enforcement options for standardizing on Auto

July 17, 2026

- â¢Cursor now responds with a plan before it begins, so you can jump in and redirect early. As it works, it updates its status so you can follow each step.
- â¢We also refined how Cursor's responses look in Slack. In-message buttons are gone, replaced by compact footer links. Tables, PRs, and artifacts now render more cleanly.
- â¢From Slack, Cursor can now start in a named multi-repo environment instead of a single default repository. If your frontend, backend, and shared code live in separate repos, Cursor reads your request and targets the environment that gives it access to all of them.
- â¢Mid-task, when Cursor needs a repo outside the current environment, it prompts you with a **Switch repository** button. Click it, choose the repo or environment, and Cursor picks up right where it left off.
- â¢Cursor can now read from and send messages to other Slack channels and threads. During a task, it can pull context from elsewhere in the workspace and post updates back in the original thread or the relevant channel.
- â¢Learn more in our Slack docs.

July 10, 2026

- â¢Improved repo picker grouping with options under No Repo, On This Computer, and Cloud.
- â¢Added the "Run on" picker that shows where your agent can run (Cloud, This Computer, or Remote Machines) and drills into the relevant choices (environments, local options, and so on) from there.
- â¢The branch picker opens on your default branch and recently used branches instead of a long flat list, with search for everything else.
- â¢Removed the Home concept so working without a repo is an explicit No Repo choice.
- â¢Combined all remote optionsâyour machines, team pools, and existing remote workspacesâinto one searchable Remote Machines menu.
- â¢Multi-repo and multi-root selection is a Select Multiple toggle inside the Cloud and This Computer flyouts, replacing the separate Set Up Workspace builder.
- â¢Moved search to the top of the menu.
- â¢Simplified the footer.

## Show more (5)Show less

- â¢Added the ability to find the No Repo option by typing `none` or`no repo` .
- â¢If a repo exists only in the cloud, the picker suggests cloning it locally.
- â¢Added clearer section dividers.
- â¢Added folder icons with a small cloud badge for cloud repos.
- â¢Removed redundant `Current` tag in the branch list.

June 30, 2026

- â¢Admins can now configure Team MCP servers once and distribute them across cloud agents, the agents window, IDE, and CLI.
- â¢When an admin sets up Team MCP servers for cloud agents, they can make those same servers available in a team marketplace from **Dashboard -> Plugins & MCPs** . This allows members of the team to install approved integrations locally without configuring servers themselves.
- â¢Learn more in the docs on migrating existing Team MCPs.
- â¢Team MCP servers
- â¢Team marketplaces now support organization groups, in addition to team-level SCIM directory groups.
- â¢Marketplace organization groups
- â¢Under **Dashboard -> Plugins & MCPs -> Team Marketplaces** , restrict marketplace access to specific organization groups. Marketplaces that already use SCIM directory groups keep that configuration.
- â¢Get started in the Cursor dashboard.

June 29, 2026

- â¢Open the Cursor mobile app, choose a repo, and launch an agent the same way you would on the desktop app. Pick any frontier model, describe ideas out loud with voice input, and use slash commands to guide Cursor in the right direction.
- â¢Cloud agents run in isolated virtual machines with full development environments to test, verify, and demo work. Move sessions from local to cloud to keep them running with your laptop closed.
- â¢Use Remote Control in the Agents Window to take an agent you're running on your computer and keep directing it from your phone.
- â¢Remote Control
- â¢You can also turn on a setting to keep your computer awake, so your machine stays reachable while you're away from your desk.
- â¢On Teams and Enterprise plans, admins must enable Remote Control from the Cursor Dashboard.
- â¢Track the status of your agents with Live Activities on your lock screen. Get push notifications when an agent finishes, needs input, or is ready for review.
- â¢Review demos, screenshots, logs, and diffs from your phone. Leave follow-up instructions, or merge the PR directly from the app.

## Show more (1)Show less

- â¢Download Cursor for iOS to start building from your phone. Read our announcement or the docs to learn more.

June 22, 2026

- â¢You can now add and manage plugins, skills, MCPs, subagents, rules, commands, and hooks at the user, team, or workspace level, and even bring your own custom MCPs.
- â¢Cursor now shows you a leaderboard of the most popular plugins, skills, and MCPs across your team.
- â¢Add any to your setup with one click from the new Customize page and extend Cursor for your workflow.
- â¢Plugins now support prebuilt canvases: shared setup templates your team can open and reuse.
- â¢Use the Hex Canvas to build data visualizations.
- â¢Use the Atlassian Canvas to see a realtime view of all our issues, projects, and documents.
- â¢Team marketplaces now support imports of plugin repositories from GitLab, BitBucket, or Azure DevOps so you easily add plugins and distribute them to your team.
- â¢Learn more in our docs.

June 18, 2026

- â¢**Issue comment:** when a comment is made on a non-PR issue
- â¢**PR review comment:** when an inline comment is left on a pull request diff
- â¢**PR review submitted:** when a PR review is submitted
- â¢**Review thread updated:** when a review thread on a pull request is marked resolved or unresolved
- â¢**Workflow run completed:** when a GitHub Actions workflow run finishes on a pull request or branch
- â¢Automations can now be saved in an incomplete state, so you can navigate away to set up an MCP auth without losing your progress
- â¢Automations can now open PRs by default; so you no longer have to specify that tool in the UI
- â¢You can now delete memory files in the UI, or prompt your automation to delete outdated memories when it runs

June 17, 2026

- â¢Cursor can now help you set up your dev environment in the cloud in less than 10 minutes. You can watch the agent's progress in a shared terminal session as it handles setup tasks like installing dependencies.
- â¢Your environment is captured in a reusable snapshot, so future cloud agents start up faster with the ability to test changes by running your software. It can iterate over long time horizons until outputs are verified. This benefits your entire team when committed to `.cursor/environment.json` .
- â¢Cloud environment setup
- â¢Use `/in-cloud` to spin up a cloud subagent in its own VM to work on the next task you submit. It runs on its own VM and branch, so your local workspace stays clean and responsive.
- â¢This is especially useful for isolating long-running or parallel work like fixing CI, investigating an issue, or exploring a codebase while you keep working locally.
- â¢You can also ask a cloud subagent to babysit a PR by clicking on the quick-action pill or using `/babysit` . The cloud agent will iterate remotely to prepare your PR for merge without tying up the local session.
- â¢The cloud subagent can run in the background without interrupting the parent agent, which can continue to run locally or in the cloud.
- â¢Move agent sessions more reliably between your local computer and the cloud. You can offload long-running work from your machine and run as many cloud agents in parallel as you want. Pull a cloud agent back down to local to test changes yourself.

## Show more (1)Show less

- â¢Handoff between local and cloud

June 10, 2026

- â¢Bugbot is now over 3x faster, 22% cheaper, and finds 10% more bugs per review.
- â¢These performance gains are made possible by progress we've made training Composer 2.5, which now powers Bugbot. Bugbot respects model block lists, and speed and performance can vary depending on your configuration.
- â¢You can now run Bugbot and Security Review with `/review` before pushing code.`/review` prompts you to choose which agents to run, or use`/review-bugbot` and`/review-security` directly.
- â¢`/review` also syncs with Bugbot on GitHub and GitLab. If you run`/review` and then open a PR with the same diff, Bugbot recognizes it, skips the review, and leaves a comment noting it has already reviewed that diff.
- â¢Available in Cursor 3.7+ and on cursor.com/agents, with support in CLI coming soon.
- â¢You can now configure Bugbot to only review what's new since the last review, keeping feedback focused on your latest updates.
- â¢Learn more in our docs.

June 5, 2026

- â¢Click on two or more elements together in the browser. Cursor sees the selected elements, their code, the surrounding layout, and the visual relationships on the page.
- â¢Ask the agent to make one match the other, remove repeated content, or adjust a group of components at once.
- â¢Narrate changes through the Design Mode overlay. The mic stays available while an agent is mid-run, so you can queue the next change by voice without waiting for the previous one to finish.

June 4, 2026

- â¢**Run correlation** : Every`send()` now carries a platform-generated`requestId` , exposed on`Run` and`RunResult` and persisted across the in-memory, SQLite, and JSONL stores. Tie a script or CI run to backend logs, analytics, and support threads without inferring it from`agentId` .
- â¢**Reliable `wait()` on local runs** : Local runs no longer resolve`wait()` before the terminal result is written. Hydration keeps refreshing until the run reaches a final state, so automation reads a complete result.
- â¢**Safe checkpoints on dispose** : Disposing a local agent no longer removes checkpoint data when a root reference is missing but checkpoint blobs still exist. The agent directory is only cleared when there's genuinely nothing left to keep.
- â¢**Cloud streaming over HTTP/1.1** : Cloud agent sessions now stream correctly on HTTP/1.1 transports used by some proxies, older Node fetch stacks, and certain CI images. HTTP/2 behavior is unchanged.
- â¢**Lighter import** : Importing`@cursor/sdk` no longer eagerly loads the full local agent stack. Cloud-only and type-only consumers skip the local runtime cost until the first local call, with no API change. The first local call pays a one-time import, then stays cached.
- â¢**Self-contained TypeScript types** : Published`.d.ts` files no longer reference unpublished workspace packages. This fixes`TS2305` and`TS2307` errors under`skipLibCheck: false` and silent`any` on stream types like`TurnEndedUpdate` .
- â¢**Bundled ripgrep** : Local shell runs use the bundled platform`rg` binary without modifying your global`PATH` . On Windows, prepending ripgrep no longer clobbers the`Path` variable.
- â¢**Composer 2 routes to Composer 2.5** : SDK clients still pinning retired`composer-2` slugs are routed to Composer 2.5 automatically, keeping fast variants intact, so older scripts keep running.

## Show more (3)Show less

- â¢**Workspace-scoped `list_runs`** :`Client` ,`AsyncClient` , and`Agent.list_runs` take an optional`cwd` , and the bridge falls back to its launch workspace. This fixes spurious "agent not found" results when the bridge runs as a subprocess.
- â¢**Clearer not-found errors** : Looking up an agent that isn't in the resolved workspace returns a clear not-found error instead of an opaque internal error.
- â¢**0.1.6 release and analytics** :`cursor-sdk` 0.1.6 documents the Buildkite release path and labels SDK usage as`sdk-python-` for clearer analytics.

June 4, 2026

- â¢Shared canvases can now be opened full-screen in the browser, making them easier to present to others.
- â¢Added the ability for agents to embed buttons in canvases that will run a specific prompt when clicked.
- â¢Improved the agent's ability to fix canvas type errors.
- â¢Improved component styling, and added more chart customization functionality.

June 3, 2026

- â¢Multi-team support so users can be on multiple teams at once
- â¢Organization-level IDP management
- â¢Organization-level usage analytics, with drill downs to each team
- â¢Admins can move users between teams through the dashboard, API, or CSV
- â¢New users joining a team inherit settings and permissions automatically

May 29, 2026

- â¢Auto-review applies to Shell, MCP, and Fetch tool calls. Allowlisted calls run immediately, and calls that can be sandboxed run in the sandbox. All other agent actions go to a classifier subagent that decides whether to allow the call, try a different approach, or ask for your approval.
- â¢Configure your run mode in **Settings > Cursor Settings > Agents > Approvals & Execution** . You can also steer the classifier agent by giving it custom instructions.
- â¢Learn more in our docs.
