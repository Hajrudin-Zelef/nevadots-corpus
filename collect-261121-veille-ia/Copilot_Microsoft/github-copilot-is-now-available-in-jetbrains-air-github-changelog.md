---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-is-now-available-in-jetbrains-air-github-changelog
title: "GitHub Copilot is now available in JetBrains Air - GitHub Changelog"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-03"]
keywords: ["copilot", "github", "jetbrains", "tasks", "cloud", "subscription", "through", "agent"]
source: https://github.blog/changelog/2026-11-03-github-copilot-is-now-available-in-jetbrains-air/
source_anchor: ""
source_lines: [1, 23]
section: "Copilot_Microsoft"
---
You can now delegate coding tasks, guide execution, and review results with GitHub Copilot in JetBrains Air. The integration uses the Agent Client Protocol (ACP) and supports tasks in your JetBrains IDE or in the cloud.

Use Copilot in Air alongside the GitHub Copilot plugin and choose the workflow that works best for you. For a comparison and FAQ, see GitHub Copilot in JetBrains Air.

- **GitHub Copilot in Air’s agent picker** : Select**GitHub Copilot** and use your existing Copilot subscription for coding tasks.
- **Parallel work and in-context review** : Track multiple agent sessions, use project context and IDE tools, and review changes alongside your code.
- **Cloud tasks on your GitHub Copilot subscription** : Run Copilot in isolated cloud environments through Air Teams, from your IDE or browser. Tasks use your Copilot plan rather than JetBrains AI credits.

For local tasks, install Air in your JetBrains IDE, select **GitHub Copilot**, and connect your GitHub Copilot account.

For cloud tasks, you’ll need a JetBrains account in an Air Teams-enabled business organization, a GitHub Copilot subscription, and Copilot enabled by your Air administrator. There are two ways to run cloud tasks:

- **Browser** : Open Air in your browser, connect GitHub under**Settings > VCS Providers** , and create a task. Choose your repository and remote branch, select**GitHub Copilot** , and send your prompt.
- **IDE** : Use the Air plugin in your JetBrains IDE and select**Cloud** from the environment selector.

When you run a GitHub Copilot cloud task through JetBrains Air, Copilot works in a Docker container on a fresh virtual machine. All traffic passes through a proxy, and no state carries over between tasks. Copilot can build, test, and install packages without modifying your local environment. For more information, see Cloud agent sandboxing in Air.

- **Subscription checks** : Connecting GitHub Copilot to Air does not request GitHub Copilot permissions. Air may only verify subscription or credit availability when the cloud environment starts. If your subscription or credits are unavailable, the task fails and an error appears in the session.
- **Plan selection** : You cannot choose whether an individual task uses your personal GitHub Copilot plan or your organization’s plan.
- **Usage visibility** : Air’s governance and analytics features do not include Copilot usage.
- **Administrative controls** : Air administrators can disable GitHub Copilot through a governance policy, but cannot set Copilot spending limits within Air.

Share your thoughts through in-product feedback, our feedback repository, or the private survey.
