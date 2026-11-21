---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-just-moved-into-xcode-jetbrains-air-slack-and-teams-in-one-week
title: "GitHub Copilot just moved into Xcode, JetBrains Air, Slack and Teams in one week"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-03"]
keywords: ["copilot", "teams", "github", "xcode", "jetbrains", "slack", "agent", "code"]
source: https://medium.com/@mvpkenlin/github-copilot-just-moved-into-xcode-jetbrains-air-slack-and-teams-in-one-week-f192c2c4b346
source_anchor: ""
source_lines: [1, 65]
section: "Copilot_Microsoft"
---
# GitHub Copilot just moved into Xcode, JetBrains Air, Slack and Teams in one week

## Copilot is no longer a VS Code feature. It is becoming the agent that follows your developers into every tool, and that changes how you govern it.

For most of its life, GitHub Copilot has been something you used inside Visual Studio Code. It worked elsewhere, but VS Code was home.

In the first week of November, that changed quietly. In four days, GitHub’s changelog announced Copilot inside Apple’s Xcode, inside JetBrains’ new agent tool Air, and generally available as **@GitHub** in Slack and Microsoft Teams. Microsoft and GitHub also pushed further on a package manager for agent instructions.

None of these is a headline on its own. Together, they say something about where Copilot is going, and it matters for anyone who has to buy, budget or secure it.

## What shipped, day by day

- **3 November: JetBrains Air.** Copilot is now an agent option in JetBrains Air, using your existing Copilot subscription. It connects through the Agent Client Protocol (ACP), an open protocol for swapping agents in and out. Tasks can run locally in the IDE or in the cloud through Air Teams, where each task runs in a Docker container on a fresh virtual machine, with traffic through a proxy and no state kept between tasks.
- **4 November: Xcode.** Copilot is now available inside Xcode’s own coding assistant, in public preview from the Xcode 27.3 beta. According to Paul Lovelace’s write-up, you turn it on in Xcode’s Intelligence settings with no extension needed, and all Copilot plans are supported.
- **6 November: Slack and Teams.** @GitHub in Slack and Teams is now generally available. From a chat thread, you can investigate a bug, create an issue, implement a change and open a pull request. In Teams, a Canvas view lets the group review the plan and the code changes beside the conversation. Issues and pull requests created this way say which conversation and person they came from.
- **3 November: agent packages.** Dependabot now updates Agent Package Manager (APM) dependencies. APM treats the skills, prompts and instructions that guide AI agents as versioned dependencies, declared in an`apm.yml` file.

## Why this is a distribution change, not a feature change

Lovelace calls this “a distribution change,” and I agree. There is no new model and no new price. What changed is *where* Copilot appears.

That matters because adoption follows convenience. In my experience training IT teams, the tool people actually use is the one already open on their screen. An iOS developer who lives in Xcode was never going to switch to VS Code to use an AI assistant. A product manager who lives in Teams was never going to open an IDE. Now neither has to.

Read the four announcements together and a pattern appears. Copilot is becoming less of an editor feature and more of an **agent that sits behind every surface**: IDE, chat, cloud runner. APM points the same way: if agents are going to run everywhere, their instructions need to be versioned and updated like code.

## The governance gap this opens

Here is the part I would flag to anyone responsible for licences or security. The new surfaces do not all come with the same controls.

GitHub’s own changelog for JetBrains Air lists the limits plainly:

- You **can’t choose per task** whether it bills your personal or your organisation’s Copilot plan.
- **Air’s analytics don’t include Copilot usage,** so you need GitHub’s own usage views to see it.
- Admins can disable Copilot in Air with a policy, but **can’t set Copilot spending limits inside Air.** Cost controls sit on the GitHub side.

The Xcode integration is a public preview inside a beta IDE. And the Slack and Teams integration means a pull request can now begin in a chat thread, which is a new place for work to start and a new place for something to go wrong.

The counter-argument is fair: this is what customers asked for. Developers want Copilot in the tools they already use, and it would be odd for GitHub to refuse. More surfaces also mean more competition between agents, through open protocols like ACP. That is good for buyers.

But each new surface is one more place where usage, cost and access have to be tracked. When Copilot lived in one editor, governance was simple. It isn’t any more.

## What I would do

1. **List every surface where Copilot can now run in your organisation:** VS Code, Visual Studio, JetBrains and Air, Xcode, CLI, GitHub.com, Slack, Teams.
2. **Decide which ones you allow,** and use the policies GitHub and each tool provide to switch off the rest.
3. **Watch spend from GitHub, not from the tool.** Air does not show Copilot usage; GitHub does.
4. **Treat chat-started pull requests like any other.** Same review rules, same branch protection.
5. **Pilot Xcode on non-release machines** until the stable Xcode release arrives.

## What would change my mind

If every Copilot surface reported usage and cost into one place, with one set of spending limits, I would stop worrying about the spread. That is likely where GitHub is heading. Today, the controls trail the distribution by a few months.

**How many places can Copilot run in your organisation today, and does anyone have the full list?**

*Ken Lin is a Microsoft Certified Trainer of 25 years and an MCT Regional Lead, based in Sydney.*

## Sources

- GitHub Changelog, “GitHub Copilot is now available in JetBrains Air” (3 Nov 2026): https://github.blog/changelog/2026-11-03-github-copilot-is-now-available-in-jetbrains-air/
- GitHub Changelog, “GitHub Copilot is now available in Xcode’s coding assistant” (4 Nov 2026): https://github.blog/changelog/2026-11-04-github-copilot-is-now-available-in-xcodes-coding-assistant
- GitHub Changelog, “@GitHub in Slack and Teams is generally available and more” (6 Nov 2026): https://github.blog/changelog/2026-11-06-github-copilot-in-slack-and-teams-is-generally-available
- GitHub Changelog, “Dependabot version updates support Agent Package Manager” (3 Nov 2026): https://github.blog/changelog/2026-11-03-dependabot-version-updates-support-agent-package-manager
- Agent Package Manager documentation: https://microsoft.github.io/apm/
- Paul Lovelace, “GitHub Copilot Just Stopped Being a VS Code Thing” (DEV Community, 5 Nov 2026): https://dev.to/paullovelace/github-copilot-just-stopped-being-a-vs-code-thing-bn2
