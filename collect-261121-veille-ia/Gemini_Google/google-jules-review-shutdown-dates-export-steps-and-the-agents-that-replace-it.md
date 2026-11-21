---
id: collect-261121-veille-ia/gemini-google/google-jules-review-shutdown-dates-export-steps-and-the-agents-that-replace-it
title: "Google Jules Review: Shutdown Dates, Export Steps and the Agents That Replace It"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["jules", "google", "export", "down", "steps", "agent", "tasks", "prompt"]
source: https://www.danilchenko.dev/posts/google-jules-review/
source_anchor: ""
source_lines: [1, 214]
section: "Gemini_Google"
---
**Update, November 12, 2026:** Google announced on October 29 that Jules will wind down on December 31, 2026. Sessions could be finished until November 11; since then Jules is read-only. This review was first published in May, while Jules was a live product bundled with the ~$20/month Google AI Pro plan. It’s now kept as a record of what Jules did, plus the export steps and the alternatives that cover the same work.


## TL;DR

Google Jules was the most prominent coding agent built around queuing instead of live chat: describe a task, walk away, get a pull request. It is shutting down. Google’s turndown notice says Jules “will wind down on December 31, 2026” and went read-only after November 11. Google says the lessons are shaping its other products, including Google Antigravity. If you used it, export your settings, scheduled tasks, memories and session history before the end of the year with the Jules CLI (steps below). The async lane it pioneered didn’t die with it: Claude Code cloud sessions, GitHub Copilot’s cloud agent and OpenAI Codex all run tasks in the background now, and all three can run on a schedule, which was Jules’s headline feature in May.

## What Happened to Google Jules

The turndown entry in the Jules changelog, dated October 29, 2026, gives the reason in one clause: “as part of our work to unify our agentic coding efforts, Jules will wind down on December 31, 2026.” It calls Jules “an experiment to learn how asynchronous coding agents could help you tackle your backlog” and says what users built with it “taught us where agentic development is headed”, with those learnings “shaping products across Google, including our agent-first platform, Google Antigravity.” TechRadar’s coverage adds that Google confirmed the move in a post on X.

The notice doesn’t spell out what read-only covers, but presumably it means past sessions stay viewable until December 31 while new work can’t start. Your code is safe in any case. Jules always worked through GitHub branches and pull requests, so everything it shipped already lives in your repositories. What disappears with the product is the configuration and history stored inside Jules itself.

That makes this a short-lived product by any measure. Here’s the full arc, taken from the dated entries in the same changelog:

| Date | Event | 
|---|---|
| May 19, 2025 | Jules launches in beta | 
| Aug 6, 2025 | Out of beta, paid tiers appear | 
| Oct 3, 2025 | Jules API ( `v1alpha` ) | 
| Dec 10, 2025 | Scheduled Tasks, Suggested Tasks, Render integration | 
| Jan 30, 2026 | Gemini 3 Flash becomes the base model on all tiers | 
| Feb 2, 2026 | MCP support with six curated servers | 
| Feb 19, 2026 | CI Fixer: automatic fixes for failing GitHub Actions checks | 
| Mar 9, 2026 | Gemini 3.1 Pro becomes the default for Pro users | 
| Oct 29, 2026 | Turndown announced | 
| Nov 11, 2026 | Last day to finish sessions; read-only afterwards | 
| Dec 31, 2026 | Jules winds down | 

That’s about nineteen months from launch to shutdown, and the March model upgrade was the last changelog entry before the turndown, seven months of silence that look obvious in hindsight.

## How to Export Your Jules Data

Google’s notice lists what the export covers: “account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, and every session, including its prompt, plan, full activity log, and code patches.” The route is the Jules Tools CLI plus any CLI coding agent:

```
npm install -g @google/jules
jules login                 # interactive browser sign-in
jules remote list --repo    # sanity check: lists your connected repos
```
Then, from your home directory, open a CLI agent (Google suggests Antigravity CLI, but the prompt is plain text) and paste the export prompt from the changelog entry. The prompt has the agent check your login, save the Python export script Google supplies, run it into a local `jules-export` folder, and report counts.

Google’s own prompt flags two things to know before you run it. First, the export reads settings, environment variables, repos and memories from an internal `$rpc` endpoint, and sessions from REST endpoints under `aida.googleapis.com/v1/swebot`. The prompt describes both as undocumented and tells the agent to report a failing call rather than guess at a fix. Second, the CLI’s access token expires about an hour after login, so a long export may need a fresh `jules login` halfway through. If `ListSources` returns 401 while other calls work, your GitHub account isn’t connected to Jules, and you can connect it at the GitHub app link the prompt gives.

The CLI itself has been frozen for a while. When I queried the npm registry for this update, `@google/jules` reported `latest: 0.1.42`, published December 16, 2025. Export while the read-only window is open. Don’t plan on it working after December 31.

## How Jules Worked

Every task ran in an isolated Google Cloud VM. Jules cloned your repo, read the codebase, built an execution plan and showed it to you before touching files. You could edit, approve or scrap the plan. Once approved, Jules worked through the changes, ran the tests it found, and opened a PR on GitHub. Since January 2026 a second “Planning Critic” agent reviewed every auto-approved plan before execution, which Google said cut task failures by 9.5%.

The loop was: submit, approve a plan, wait for the PR notification. No terminal session, no streaming output.

### Tiers and models

Jules never had its own subscription. It bundled into Google’s AI plans. Task limits below come from the usage limits page; the model and feature rows come from the changelog, because the usage page still lists the 2025 models:

|  | Free | Pro (Google AI Pro, ~$20/mo) | Ultra (Google AI Ultra) | 
|---|---|---|---|
| Daily tasks | 15 | 100 | 300 | 
| Concurrent tasks | 3 | 15 | 60 | 
| Model (final state) | Gemini 3 Flash | Gemini 3.1 Pro | Gemini 3 Flash base, priority access to newer models | 
| Suggested Tasks | No | Yes | Yes | 
| Scheduled Tasks | Yes | Yes | Yes | 

Google AI Ultra, the plan that carried the 300-task tier, dropped from $250 to a $100 entry tier at I/O 2026, with a $200 option above it, per Engadget. The usage page says paid tiers were “currently available only for individual Google Accounts (ending in @gmail.com)” and that Google was “working to quickly support other upgrade paths” for Workspace and enterprise accounts. No changelog entry ever announced that support.

Google reported 80.6% on SWE-bench Verified for the Gemini 3.1 Pro model, which became Jules’s Pro default in March. That was competitive in the spring; Google’s own coding tools have since moved to the Gemini 3.8 Flash and Gemini 4 Argon generation (covered in the Antigravity CLI comparison), and Jules never got them.

## What Jules Got Right

### Batch parallelism

The async model changed how work got batched. Queue five dependency bumps in the morning, write the design doc you’ve been putting off, come back to five PRs. In a real-time agent, the same five tasks meant approving edits and answering clarifying prompts one at a time. On Pro, 15 concurrent slots let a small backlog run in parallel without queueing.

### CI Fixer

From February 2026, when a GitHub Actions check failed on a PR Jules had created, Jules received the error, worked out a fix, committed it and resubmitted, “all without manual intervention” in the changelog’s words. The same release added commit-authorship modes (Jules alone, co-authored, or you as sole author), so your GitHub contributions could reflect the work you started. Before CI Fixer, the changelog admits, a failed check on a Jules PR “would stall your workflow” until someone pasted the error logs back in. In my view this was the feature that made the async loop hold together.

### Scheduled and Suggested Tasks

Scheduled Tasks, announced in December 2025 for every tier, ran recurring jobs: nightly lint passes, weekly dependency audits, monthly dead-code sweeps. A weekly `pip-audit` job turns a check most teams run once a quarter into a Monday-morning PR. Suggested Tasks, on Pro and Ultra, scanned up to five repos for stale TODOs and, from January, performance optimizations, then proposed fixes. In May this review called scheduling Jules’s strongest argument. By November, all three alternatives below have a scheduler of their own.

### A curated MCP list

In February 2026 Jules added Model Context Protocol support with six hand-picked servers: Linear, Stitch, Neon, Tinybird, Context7 and Supabase, all connected by API key. Google’s explanation was security. Jules ran in a cloud environment wired to your GitHub repos, so each server’s data flow was reviewed before it went on the list. Six servers was limiting next to Claude Code, which connects to any server you configure, but a malicious MCP server in a cloud VM with repo access can exfiltrate code, so the restriction had a security payoff. The same allowlist approach works for your own agent setups; the AI agent guardrails guide covers it in more depth.

## Where Jules Fell Short

### Speed

Everything about the design added latency: VM spin-up, a cloned repo, a full planning phase, the Planning Critic on top. Google itself said the critic “adds a small amount of time to the planning phase”. A one-line fix that a terminal agent turns around while you watch went through the same pipeline as a large refactor. For anything urgent, Jules was the wrong tool, and I found no official task-time figures to plan around.

### GitHub only

Jules connected to GitHub and nothing else; the changelog never added GitLab, Bitbucket or self-hosted Git. Add the Gmail-only restriction on paid tiers, and Workspace-based companies had no way to buy it. My read is that a product individuals could pay for but organizations mostly couldn’t was always going to struggle to justify itself inside Google.

### Trusting “completed”

Any async agent asks you to trust its status report, because you weren’t watching. The REST API’s session states show how many outcomes sat behind one status line: `QUEUED`, `PLANNING`, `AWAITING_PLAN_APPROVAL`, `AWAITING_USER_FEEDBACK`, `IN_PROGRESS`, `PAUSED`, `FAILED`, `COMPLETED`. A session that stops at `AWAITING_USER_FEEDBACK` overnight hasn’t failed, but it hasn’t produced a PR either. With any queue-and-forget agent, read the diff and the test log before you trust the badge.

## The Jules API, Checked Against the Live Schema

The Jules API is still `v1alpha`. The May version of this review printed a session-creation call that, it turns out, didn’t match the schema: it had no `prompt` and nested the repo under a `gitHub` object that doesn’t exist. Since the API publishes a discovery document that needs no key, I checked request bodies against it for this update instead of trusting the docs page. Here’s the script:

```
import json, urllib.request
doc = json.load(urllib.request.urlopen(
    "https://jules.googleapis.com/$discovery/rest?version=v1alpha"))
schemas = doc["schemas"]
def check(body, name, path=""):
    props, problems = schemas[name]["properties"], []
    for key, value in body.items():
        if key not in props:
            problems.append(f"unknown field {path}{key}")
        elif "$ref" in props[key] and isinstance(value, dict):
            problems += check(value, props[key]["$ref"], f"{path}{key}.")
    for key, prop in props.items():
        if prop.get("description", "").startswith("Required.") and key not in body:
            problems.append(f"missing required {path}{key}")
    return problems
old = {"sourceContext": {"gitHub": {"repository": "owner/repo", "branch": "main"}},
       "title": "Add input validation to /users POST endpoint"}
new = {"prompt": "Add input validation to the /users POST endpoint, with tests",
       "sourceContext": {"source": "sources/github/owner/repo",
                         "githubRepoContext": {"startingBranch": "main"}},
       "requirePlanApproval": True,
       "automationMode": "AUTO_CREATE_PR"}
print("revision", doc["revision"])
print("May snippet:", check(old, "Session") or "OK")
print("corrected:  ", check(new, "Session") or "OK")
print("automationMode:", schemas["Session"]["properties"]["automationMode"]["enum"])
```
Running it on November 12 printed:

```
revision 20261111
May snippet: ['unknown field sourceContext.gitHub', 'missing required sourceContext.source', 'missing required prompt']
corrected:   OK
automationMode: ['AUTOMATION_MODE_UNSPECIFIED', 'AUTO_CREATE_PR']
```
The May text also got the two control fields backwards. Plan approval is a separate boolean, `requirePlanApproval`, and the schema says that “if not set, plans will be auto-approved”. That’s the opposite of the safe default the old review described. `automationMode` only decides whether Jules opens a PR on its own: `AUTO_CREATE_PR` is the one real value, and without it no PR is created. If you’re writing a final script to pull sessions out through the API, `GET v1alpha/sessions` and `GET v1alpha/sessions/{id}/activities` are the calls you want. The turndown notice gives no separate date for the API, so expect it to stop with the product.

## Project Jitro, and Where Google Is Taking This

Before I/O 2026, byteiota reported that Google was building Project Jitro, a goal-driven successor to Jules: instead of “fix this function”, you’d say “get test coverage to 85%” and the agent would pick the files and tests. We couldn’t find a Google announcement confirming Jitro, a waitlist, or a launch, and the turndown notice doesn’t mention it. What the notice does name is Antigravity, Google’s agent-first platform, whose CLI replaced Gemini CLI for consumer plans in June (see the Gemini CLI tutorial’s update note). If the goal-driven idea ships anywhere, that’s the likely home.

## Alternatives to Google Jules

In May, the comparison table here said Claude Code ran one serial task with no proactive features, and that only Jules offered CI auto-fixing and scheduled tasks. Neither is true anymore. Every one of the three main alternatives now runs work in the cloud while you’re away, and each has some form of scheduler. Checked against each vendor’s docs on November 12, 2026:

|  | Claude Code | GitHub Copilot cloud agent | OpenAI Codex | 
|---|---|---|---|
| Async execution | Cloud sessions from web, mobile, Desktop or `claude --cloud` ; several can run at once | Cloud agent takes work delegated from issues, chat or PRs and opens a PR | Codex Cloud tasks from the web, app or IDE | 
| Scheduled work | Scheduled tasks (formerly routines) on a cadence, API call or GitHub event; research preview | Automations hourly, daily, weekly or on issue/PR events | Scheduled tasks in the app; Team Tasks run in the cloud for workspaces | 
| CI-failure fixes | PR auto-fix watches CI and review comments | “Watch and fix” on failed Actions runs and merge conflicts | Manual “Fix” attaches failing checks to a chat | 
| Git hosts (cloud) | GitHub; GitHub Enterprise Server on Team/Enterprise | GitHub only | GitHub; GitLab code review and `@codex` tasks in beta | 
| Price to get cloud tasks | Claude Pro $20/mo ($17 annual), Max $100, Ultra $200 | Copilot Pro $10/mo, Pro+ $39, Max $100 (Free has no cloud agent) | ChatGPT Plus $20/mo, Pro from $100 | 

**Moving Jules’s scheduled tasks:** the export writes them to `scheduled-tasks.json`, and each one roughly maps onto a scheduled task in any of the three, with caveats: Codex’s app-based tasks need the computer on and the app running, and Copilot automations have their own repository eligibility rules. Claude Code’s `/schedule` command is the closest match in feel, since you describe the job in plain language and give it a cadence.

**Moving CI Fixer:** Copilot’s “Watch and fix” is the nearest equivalent if your workflow lives in GitHub Actions, because it runs where the failure happens. Claude Code’s auto-fix needs the Claude GitHub App installed and handles review comments too, but per the docs it can’t resolve merge conflicts on its own.

**Moving batch work:** I found no published concurrency cap for any of the three to set against Jules’s 60 slots on Ultra; Claude Code’s docs say parallel cloud sessions share your plan’s rate limits. Claude Code gets closest with parallel cloud sessions, and for large fan-out jobs on your own machine, its dynamic workflows script hundreds of subagents from one file. Codex’s tradeoffs against Claude Code are covered in the Claude Code vs Codex CLI comparison.

If you’d rather stay with Google, Antigravity is where the turndown notice points. It’s a terminal and desktop agent, so it suits the real-time half of what Jules users did. The turndown notice doesn’t promise Jules-style scheduled cloud runs in Antigravity.

## Who Should Do What Now

**If you used Jules daily:** export this month, while the read-only window is open. Then move your scheduled tasks first. One-off sessions you can recreate, but a weekly audit job that stops on December 31 won’t announce itself.

**If you’re on GitLab:** Codex’s GitLab beta (code review plus `@codex` tasks on merge requests) is the closest thing to cloud support among the three, and it lacks GitHub-style controls such as “Create pull request” for now. Claude Code’s local CLI works with any Git host.

**If you were about to try Jules:** run the same backlog task through one of the three alternatives instead. The workflow Jules made popular (queue it, review a PR later) is now standard in every one of them.

## FAQ

### Is Google Jules shutting down?

Yes. Google’s changelog entry of October 29, 2026 says Jules “will wind down on December 31, 2026.” Users could finish sessions until November 11, 2026, and Jules has been read-only since then. Your code, branches and pull requests stay in GitHub.

### How do I export my data from Google Jules?

Install the Jules Tools CLI (`npm install -g @google/jules`), run `jules login`, then paste the export prompt from the turndown entry into a CLI coding agent started in your home directory. It saves settings, environment variables, scheduled tasks, repos, memories, setup scripts and full session history into a local folder.

### What replaces Google Jules?

Google points to Antigravity, its agent-first platform. For the async, PR-producing workflow Jules was known for, Claude Code cloud sessions, the GitHub Copilot cloud agent and OpenAI Codex cloud tasks all run in the background and support scheduled work.

### Was Google Jules free?

While it was live, yes: the free tier gave 15 tasks per day with 3 concurrent tasks, running on Gemini 3 Flash from January 2026. Google AI Pro (about $20/month) raised that to 100 tasks and 15 concurrent on Gemini 3.1 Pro, and Ultra to 300 and 60.

### How did Google Jules compare to Claude Code?

Jules was built around async work: queue a task, get a PR. Claude Code started as a real-time terminal agent and has since added cloud sessions, scheduled tasks and PR auto-fix, so it now covers both modes. Jules was GitHub only; Claude Code’s local CLI works with any Git host, though its cloud sessions also need GitHub.

## Sources

- Jules changelog: turndown notice (Oct 29, 2026), export prompt, and every dated feature and model change cited here
- Jules usage limits: task quotas, concurrency and the Gmail-only restriction
- Jules API documentation and the v1alpha discovery document: request schema used in the executed check
- Jules proactive features announcement: Scheduled Tasks, Suggested Tasks, Render integration
- TechRadar on the Jules shutdown: independent coverage
- Engadget on Google AI Ultra pricing: the $100 and $200 Ultra tiers
- Project Jitro report (byteiota): pre-I/O reporting on the goal-driven successor
- Claude Code on the web and scheduled tasks: cloud sessions, auto-fix, Git host limits
- About the Copilot cloud agent and Copilot automations changelog
- Codex scheduled tasks, Codex GitLab beta and Codex pricing

## Bottom Line

Jules got the shape of the async coding agent right early: work that runs while you’re away, fixes its own failing CI and turns up as a PR on Monday morning. Then its changelog went quiet for seven months, still Gmail-only and GitHub-only, while Anthropic, GitHub and OpenAI added schedulers, cloud sessions and CI fixes to products that companies can buy.

If you have anything in Jules, export it before December 31 and move your scheduled tasks first. If you’re shopping for an async agent now, any of the three alternatives above does what Jules did in May, and two of them auto-fix CI as well.
