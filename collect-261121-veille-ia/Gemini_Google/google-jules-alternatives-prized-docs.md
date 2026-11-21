---
id: collect-261121-veille-ia/gemini-google/google-jules-alternatives-prized-docs
title: "Google Jules alternatives - Prized docs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["google", "jules", "github", "cloud", "claude", "code", "prized", "pull"]
source: https://prized.dev/docs/jules-alternatives
source_anchor: ""
source_lines: [1, 57]
section: "Gemini_Google"
---
# Google Jules alternatives

Updated

Google's Jules runs sessions until November 11, 2026, then goes read-only, and shuts down on December 31. Where Jules users are taking their async coding tasks (Google Antigravity's remote agents, GitHub Copilot's cloud agent, Claude Code on the web, Codex Cloud, Cursor Cloud Agents, Devin, OpenHands or a Prized box), with when each fits, from each vendor's own pages, checked November 5, 2026.

Jules runs sessions until **November 11, 2026**. After that it is read-only, and on **December 31, 2026** it shuts down (Google's notice). Your code, branches and pull requests stay in GitHub; your settings, environment variables, setup scripts, memories, scheduled tasks and sessions live in Jules, and Google's export saves them to a folder on your computer (Moving from Jules, step 1). Google says what it learned from Jules is shaping products across Google, including Antigravity; Antigravity's remote agents, which run in Google's cloud, started rolling out on November 3.

- **Your own Claude Code or Codex on a cloud machine that keeps its disk: a Prized box.** A box is a Linux VM, so what your Jules setup script installed stays for the next task, and it clones from any git host. Hand a task over from a browser tab or with`prized prompt` , close the tab, and the agent opens the pull request when the task asks for one; the`prized` label on a GitHub issue works like the`jules` label, through a small script you run on a box kept awake, and cron on the box runs scheduled tasks (Moving from Jules). What it lacks: an agent of its own (you bring Claude Code or Codex and the plan they sign in with; for Gemini models, OpenCode or pi with a Gemini API key), a page to review plans and diffs as in Jules (you review the pull request on GitHub), Slack or Linear triggers, and a phone app (the box's terminal opens in a phone browser, and Claude Code's Remote Control shows a session in the Claude app). On the free credit you get one box at a time, and it pauses after 2 idle hours; set**Auto-pause** to its longest setting to keep it up (12 hours on the free credit;**Off** on a plan or after a top-up), and it bills while idle.
- **Stay with Google: Antigravity's remote agents.** Google says what it learned from Jules is shaping products across Google, including Antigravity. A remote agent runs your session in an isolated environment Google manages, on GitHub repos you attach to a project, and keeps working with your laptop asleep; start it from the Antigravity app, the CLI (`agy --remote-agent` ) or a browser. Only Google AI Pro ($19.99 a month) and Ultra subscribers on a personal Google Account get it, and it is still rolling out: not on the free plan, Google AI Plus, or business accounts (Workspace or Google Cloud).
- **Assign an issue, get a pull request, all on GitHub: GitHub Copilot's cloud agent.** The GitHub-native way to hand over an issue: assign it to Copilot (public preview), and it opens a pull request and asks for your review. On any paid Copilot plan, from Pro at $10 a month, not Copilot Free; GitHub only.
- **Anthropic's VM per task, on your Claude plan: Claude Code on the web.** Start from claude.ai/code, the Claude mobile and desktop apps or`claude --cloud` ; a scheduled task (research preview) can run on a schedule, or start when an issue is labeled once the Claude GitHub App is on the repo, though its filters can't pick out a single issue label. Cloning and pull requests need GitHub.
- **OpenAI's VM per task, on your ChatGPT plan: Codex Cloud.** Codex sets up and tests the environment with you, then each task gets its own VM; you review the changes and open the pull request. GitHub only for now.
- **Hosted, on GitHub, GitLab, Bitbucket Cloud or Azure DevOps: Cursor Cloud Agents.** Start them from the web, the iOS app, Slack, Linear, an`@cursor` comment on a GitHub issue, or the API, or with Automations, on a schedule or when an issue's label changes. Cursor Premium, $30 a month, includes Grok models; Claude and GPT bill at their API list price.
- **Tickets handed over from Slack, Linear or Jira: Devin.** Each session runs in a VM started from a saved snapshot of your environment, or on your own machines with Outposts, and opens the pull request, on GitHub, GitLab or Bitbucket. Automations start it on a schedule, or when an issue is labeled, with a condition for the label.
- **Open source, on your own machines: OpenHands.** MIT-licensed, it runs on your laptop or a VM you run, or in OpenHands Cloud's sandboxes, with your own LLM key or credits for OpenHands' own. In OpenHands Cloud, label an issue`openhands` or comment`@openhands` , and it opens a pull request if it judges the issue resolved.

|  | Where the agent runs | Start a task from | Git hosts | Scheduled tasks | Opens the pull request | Cheapest plan with it | 
|---|---|---|---|---|---|---|
| Prized | Your box, a Linux VM that keeps its disk between tasks | A browser tab, `prized prompt` , the API, the`prized` label on a GitHub issue | Any git remote; pull requests through `gh` on GitHub | Cron on a box kept awake, or a scheduler that wakes it through the API | Yes, with `gh` , when the task asks for one | Plans from $10 a month of credit, billed by the hour, plus your own Claude or ChatGPT plan; $30 of credit once, no card | 
| Antigravity remote agents | An isolated environment Google manages | The Antigravity app, `agy --remote-agent` , the web | GitHub; other code as uploaded files | Not stated in its remote agent docs | Not stated in its remote agent docs | Google AI Pro, $19.99 a month, personal accounts, rolling out | 
| GitHub Copilot cloud agent | A GitHub cloud sandbox, GitHub Actions, or a computer or VM you register | An assigned issue, the Agents tab, Copilot Chat, IDEs, GitHub Mobile, the CLI, the API, Slack, Teams, Jira, Linear | GitHub | Yes, automations, in private and internal repos | Yes, from an assigned issue | Copilot Pro, $10 a month; cloud sandbox use past your plan's included compute billed from November 2026 | 
| Claude Code on the web | A VM Anthropic manages, per session | claude.ai/code, the Claude mobile and desktop apps, `claude --cloud` , Slack | GitHub; other hosts upload only | Yes, on a schedule, an API call or a GitHub event such as a labeled issue, with no filter for one issue label (research preview) | Pushes a branch; you click **Create PR** in the session, and a scheduled task can open one itself | Claude Pro, $20 a month | 
| Codex Cloud | A VM OpenAI runs, per task | ChatGPT web, the desktop app, iOS; Slack and Teams on Enterprise | GitHub | Not stated for cloud environments | You open it after review | ChatGPT Plus, $20 a month | 
| Cursor Cloud Agents | An isolated VM Cursor runs; tool execution can run on machines you manage | Web, desktop, iOS, Slack, Linear, `@cursor` on a GitHub issue, the API | GitHub, GitLab, Bitbucket Cloud, Azure DevOps | Yes, Automations, on a schedule or an issue label change | Yes | Cursor Premium, $30 a month; Claude and GPT models at API list price | 
| Devin | A VM Devin runs, from a saved snapshot; or your own machines (Outposts) | Web, Slack, Teams, Linear, Jira, the API, `/devin` on a pull request | GitHub, GitLab, Bitbucket | Yes, Automations, on a schedule or an issue label | Yes | Devin Pro, $20 a month | 
| OpenHands | Your laptop or a VM you run, or OpenHands Cloud's sandboxes | Its app (Agent Canvas); in OpenHands Cloud, the web, Slack, Jira, the API, or the `openhands` label or`@openhands` on an issue | GitHub, GitLab, Bitbucket; Azure DevOps on Enterprise | Yes, Automations | Yes, if it judges the issue resolved | Free and open source (MIT) to run yourself; OpenHands Cloud sells credits for its LLM provider | 

Jules in Google AI Pro allowed 100 tasks a day, 15 at a time. Claude Code on the web, Codex Cloud and Cursor Cloud Agents count cloud tasks against the same plan usage as the rest of your work (on Cursor Premium that covers Grok models, and Claude and GPT bill at API list price), and Antigravity's plans list rate limits and an AI credit pool rather than a task count. Prized bills the box: a Small (4 vCPU, 16 GB) is $0.137 an hour awake, $1.37 for a 10 hour run overnight, and only its disk while it sleeps, $0.030 an hour, about $22 a month (Billing), and several agents can work on one box at once (Parallel agents).

1. **Export from Jules, ideally before November 11:** Google's export, run by a coding agent after you sign in to Jules Tools, saves your setup scripts, environment variables, memories, scheduled tasks and sessions to`~/jules-export` (Moving from Jules).
2. **Get a box:** sign up; the tour starts one on $30 of free credit, with no card, and opens Claude Code in your browser (Quickstart).
3. **Bring the setup:** sign Claude Code and the GitHub CLI in on the box, clone the repo, run its Jules setup script once (Node.js, Go, Rust, Java and Bun aren't preinstalled, so put their install at the top), and put the memories in a`CLAUDE.md` or`AGENTS.md` (Moving from Jules, step 3).
4. **Hand over tasks the Jules way:** a prompt that ends with "open a pull request" (step 4),`prized prompt` from your terminal (Prompt an agent remotely), the`prized` label on an issue (Label a GitHub issue), or a schedule (Run Claude Code on a schedule).

You can run sessions until November 11, 2026. After that Jules is read-only until it shuts down on December 31, 2026. Google's notice gives no export deadline, so run the export after your last session, and before November 11 to be safe.

To keep assigning GitHub issues and getting pull requests back: GitHub Copilot's cloud agent. To stay with Google and Gemini: Antigravity's remote agents, on Google AI Pro or Ultra. For a hosted VM per task on a plan you already pay for: Claude Code on the web or Codex Cloud. For your own Claude Code or Codex on a machine that keeps what it installed, with any git host: a Prized box.

Jules gave free accounts 15 tasks a day. Antigravity has a free plan, but its remote agents need Google AI Pro or Ultra. OpenHands is free to self-host. Prized gives $30 of credit once, with no card. Claude Code on it needs a paid Claude plan or an API account; Codex works with a free ChatGPT account, with less use than a paid plan.

Yes, through Google's export: it writes them to a local folder with your memories, scheduled tasks and every session, and `env-variables.json` holds your values, so keep it out of your repos. On a box, run the setup script once, put the variables on the box's **Settings** tab, and the memories in a `CLAUDE.md` or `AGENTS.md` (Moving from Jules).

Cursor Cloud Agents, Devin and OpenHands, and a Prized box for git itself (its `prized` label and pull request steps are GitHub only). Claude Code on the web can take a GitLab or Bitbucket repository as an upload but can't push back to it. Codex Cloud and Copilot work with GitHub only; Antigravity's remote agents attach GitHub repos, and other code only as uploaded files.

Checked November 5, 2026:

- **Jules:** the turndown notice (dates, export), limits and plans (daily tasks), GitHub issues (the`jules` label).
- **Google Antigravity:** remote agents (where they run, plans, GitHub), changelog (rollout on November 3, Automations), pricing, Google AI Pro (price).
- **GitHub Copilot:** Copilot on GitHub (environments, GitHub only), use the cloud agent on GitHub (assigning issues, pull requests), start sessions (starting points), automations, plans, sandbox billing, changelog, October 28 (paid plans, billing from November).
- **Claude Code:** cloud sessions (plans, starting points, GitHub, uploads, usage), web quickstart (pull requests), scheduled tasks (triggers, research preview), pricing.
- **Codex Cloud:** cloud environments (VMs, setup, starting points, GitHub only), scheduled tasks, pricing (Plus, shared usage).
- **Cursor:** Cloud Agents (VMs, starting points, git hosts, plans, usage), Automations, self-hosted machines, pricing (Premium, third-party models at API list price).
- **Devin:** pricing (Pro, git hosts, starting points), environment (VM, snapshot), GitHub (`/devin` on pull requests), Automations, Outposts.
- **OpenHands:** pricing (license, self-hosted plans), GitHub (MIT, runs locally), OpenHands Cloud (git hosts), Cloud UI (billing, LLM, Slack, API), Jira, GitHub installation (the`openhands` label,`@openhands` , pull requests), Automations, Azure DevOps (Enterprise), Cloud backend (sandboxes), API keys (credits).

Something here out of date? Write to .
