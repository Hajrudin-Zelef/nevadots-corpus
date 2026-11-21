---
id: collect-261121-veille-ia/copilot-microsoft/use-your-github-copilot-plan-in-jetbrains-air-in-your-ide-and-in-the-cloud-the-jetbrains-blog
title: "Use Your GitHub Copilot Plan in JetBrains Air – in Your IDE and in the Cloud - The JetBrains Blog"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["github", "copilot", "jetbrains", "cloud", "task", "agent", "plan", "tasks"]
source: https://blog.jetbrains.com/air/2026/11/github-copilot-in-air/
source_anchor: ""
source_lines: [1, 78]
section: "Copilot_Microsoft"
---
# Use Your GitHub Copilot Plan in JetBrains Air – in Your IDE and in the Cloud

GitHub and JetBrains are bringing GitHub Copilot to JetBrains Air. Air lets you hand coding tasks to agents, run them in parallel, and review their changes in one place. You can now pick GitHub Copilot as the agent in Air – locally in your JetBrains IDE, and in the cloud with Air Teams. Tasks run on your existing Copilot plan, not on JetBrains AI credits.

Air runs the official GitHub Copilot CLI over the Agent Client Protocol (ACP), the open protocol Air uses to connect agents. You get the real GitHub Copilot agent and the models it offers you, in the same task list as Claude, Codex, and Junie. With Air Teams, you can hand GitHub Copilot a cloud task and close your laptop, and it’ll keep working on its own sandboxed machine.

Try it now in your JetBrains IDE or on the web.

## Run GitHub Copilot locally in your IDE

GitHub Copilot runs locally in Air in JetBrains IDEs through ACP. You don’t need Air Teams, but you do need a GitHub Copilot subscription.

1. Install Air in your JetBrains IDE and open it.
2. Choose *GitHub Copilot* in the agent picker.
3. Sign in with GitHub when Air prompts you to do so.
4. Describe your task and click *Send* .

GitHub Copilot works in your local project, and you review its changes in Air as with any other agent.

## How to run your first GitHub Copilot task in the cloud

With Air Teams, GitHub Copilot can also run in the cloud. You can start a cloud task from your IDE or the web, and it keeps working after you close your laptop.

### What you need

- A JetBrains Account with a company that uses Air Teams (available to JetBrains business customers).
- A GitHub Copilot subscription.

We recommend starting with a small, well-defined task – maybe something you’d normally put off: a flaky test, a dependency bump, or a small bug with a clear repro.

### From the web

1. Open Air and sign in with your JetBrains Account.
2. Go to *Settings* |*VCS Providers* and connect GitHub. There’s no separate Copilot sign-in: Air uses this connection to sign in to GitHub Copilot on the cloud machine when the task starts.
3. Click *New Task* and pick a repository and a branch. Cloud agents work from a remote branch, so push your changes first.
4. Choose *GitHub Copilot* in the agent picker. If it isn’t there, ensure that GitHub is connected and that your GitHub account has a Copilot subscription. If the issue persists, get in touch with your Air admin.
5. Describe the task and click *Send* .

### From your IDE

1. In your IDE, open Air, start a new task, and choose *Cloud* as the environment.
2. Pick a GitHub repository and a branch. Cloud agents work from a remote branch, so push your changes first. If GitHub isn’t connected yet, Air shows a banner asking you to authorize it.
3. Click *Authorize* and approve the GitHub app in your browser. The connection page doesn’t mention GitHub Copilot, but Air uses the same connection to sign in to both.
4. Your browser opens *Settings* |*VCS Providers* in the web version of Air, with GitHub connected.
5. Go back to your IDE and choose *GitHub Copilot* in the agent picker.
6. Describe the task and click *Send* .

### After you send the task

The task shows up in the *Tasks* list under your repository, both in your IDE and on the web. GitHub Copilot starts in a fresh cloud machine. When it’s done, open the task and review its changes.

## Where cloud tasks run

Each task gets its own virtual machine. GitHub Copilot runs in a Docker container, signed in to GitHub with your account. All its traffic goes through a proxy, and nothing carries over to the next task. So it can build, test, and install packages without touching your laptop. For more details, check out our blog post *How Air Cloud-Based Agent Sandboxing Works in Air*.

## Run GitHub Copilot on a schedule or on every pull request

Automations start cloud tasks for you – either on a schedule or when something happens in your repository, such as a new pull request. If you pick GitHub Copilot as the agent, each run uses your Copilot plan.

We recommend starting with a weekly dependency update as your first automation: It’s easy to review and keeps credit usage predictable. In contrast, an automation that reviews every pull request can drain your credits much faster, especially if the repository is busy. Keep in mind that Air doesn’t warn you when your credits are all used up – your runs will just start failing.

## FAQ

**How is GitHub Copilot different from the other agents in Air?**

GitHub, not Air, handles your Copilot account. When a task starts, the Copilot CLI checks your plan and credits with GitHub. Because Air doesn’t show your Copilot usage and can’t limit it, you have to keep track of it on GitHub.

**Which GitHub Copilot plans can I use?**

Any plan that GitHub accepts for the Copilot CLI. Air doesn’t check which specific plan you’re using.

**Why did my task fail soon after it started?**

Most likely, your GitHub plan has no credits left. Air checks that you have an active Copilot subscription before it starts a task, but it can’t see how many credits you have left. Check your usage on GitHub.

**How do I know how many credits were spent on a task?**

Check your usage on GitHub. Air doesn’t track Copilot credits for local or cloud tasks, and it can’t set a spending limit.
