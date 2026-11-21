---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-copilot-at-ignite-2026-office-in-copilot-ga-copilot-code-in-december-autopilot
title: "Microsoft Copilot at Ignite 2026: Office in Copilot GA, Copilot Code in December, Autopilot"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "microsoft", "code", "office", "december", "ignite", "available", "inside"]
source: https://www.datastudios.org/post/microsoft-copilot-ignite-2026-office-in-copilot-copilot-code-december-autopilot
source_anchor: ""
source_lines: [1, 117]
section: "Copilot_Microsoft"
---
Microsoft updates Copilot at Ignite 2026: Office apps ship now, Copilot Code in December

3 days ago

7 min read

Updated: 3 days ago

Microsoft used its Ignite 2026 conference in San Francisco on November 17 to move several pieces of the redesigned Microsoft 365 Copilot from preview into general availability. Office in Copilot, which lets users open Word, Excel and PowerPoint documents directly inside the Copilot app, is now generally available, and the new Home screen that merges Copilot Chat and Copilot Cowork into a single composer is rolling out worldwide. Copilot Code, the natural-language app builder first shown in September, is in the Frontier early-access program today and is scheduled for general availability in December.

The company also widened the reach of its agent features. Autopilot, a long-running personal agent with its own identity and memory, is rolling out to customers in the Frontier program, and Copilot Workspaces, a shared space for teams, people and agents that replaces Copilot Notebooks, is available there as well. Microsoft added plugins for Dynamics 365 Sales and Dynamics 365 ERP apps, both generally available, and a Salesforce plugin in preview with more than 38 prebuilt sales skills.

On the administration side, Microsoft announced Agent 365 Cost Management, a central view of usage-based AI spending with limits and alerts, and Agent 365 Business Analytics, which measures completed work against cost. According to Microsoft, Agent 365 already manages more than 50 million agents, and nearly 80 percent of Fortune 500 companies use Copilot Cowork.

··········


MICROSOFT 365 COPILOT HOME, AUTO ROUTING AND OFFICE IN COPILOT.

Home is the new starting point of the Copilot app. It combines the conversational Chat experience and the task-oriented Cowork experience behind one composer, and it relies on a router Microsoft calls Auto to decide how each request is handled. When a prompt looks like a multi-step job, Auto recommends moving it from Chat to Cowork; that recommendation is generally available. Inside Cowork, Auto can also use Microsoft-Decision-1.1 to split a task into parts and assign an effort level to each, a capability currently limited to Frontier customers. Employees can still pick OpenAI or Anthropic models manually, subject to the policies their organization sets.

Office in Copilot is the most immediately visible change for everyday users. A conversation can now continue inside a Word document, an Excel workbook or a PowerPoint deck without leaving the Copilot app. Microsoft says the files are saved to OneDrive and keep the Microsoft 365 permissions, sensitivity labels and compliance controls that apply to any other document in the tenant, and that the feature is included in the Copilot subscription license.

Microsoft also previewed Today, a personalized view inside Home. Today Brief prepares a morning summary of priorities and suggested next steps and is included with a Microsoft 365 Copilot license. Today Feed refreshes during the day as new emails, messages and meetings arrive, and its recurring updates consume Copilot Credits. Both are in private preview through the Frontier program, in the Copilot app and in Outlook.

··········

COPILOT CODE, COPILOT MANAGED RUNTIME AND THE DECEMBER RELEASE.

Copilot Code lets users describe a dashboard, a tracker or a small business app in plain language and have Copilot build it. Cloud projects run in a sandbox inside the customer's own tenant, and a local sandbox is available on Windows, with macOS support described as coming soon. During the Frontier phase, Copilot Code works on Windows, on the web, on iOS and on Android. Microsoft's Book of News states that general availability is planned for December.

Apps built this way are published through Copilot Managed Runtime, which became generally available at Ignite. The runtime provides Microsoft-managed hosting and execution separately from the place where an app is created, and it includes an optional Fabric data backend for databases, storage and data APIs. Developers can also build Copilot Studio agents and workflows locally, publish them from the command line and keep them under version control in GitHub.

The coding models behind Microsoft's developer tools were part of the same keynote. MAI-Code-2 and MAI-Code-2-Flash are generally available in GitHub Copilot, a launch covered in our article on Microsoft MAI-Code-2 in GitHub Copilot.

··········

AUTOPILOT AND COPILOT WORKSPACES IN THE FRONTIER PROGRAM.

Microsoft describes Autopilot as its first enterprise-grade, long-running agent for personal productivity. It works continuously on behalf of one employee, keeps its own memory and acts under its own Microsoft Entra Agent ID, so administrators can see which actions the agent took and manage it like any other identity in the tenant. Colleagues can @mention it in Microsoft Teams. Autopilot was announced in September together with Home and Copilot Code, as reported in our coverage of the new Microsoft Copilot with Home, Code and Autopilot; the Ignite news is the start of its rollout to Frontier customers.

Copilot Workspaces evolves the earlier Copilot Notebooks into a shared space in the Copilot app where a team, its files and Copilot work together. Microsoft says it surfaces what changed and which items are still unresolved, and that integration with each member's personal Autopilot is coming soon. Like Autopilot, Workspaces is available only through the Frontier program for now, and Microsoft has not given a general availability date for either.

··········

MICROSOFT COPILOT IGNITE 2026 ANNOUNCEMENTS BY STATUS AND LICENSE.

The table below collects the status of each Copilot announcement as Microsoft listed it on November 17. Where Microsoft did not state a license requirement, the table says so.

For most organizations the practical split is simple. Office in Copilot, Home and the Auto recommendation reach licensed users without any opt-in, while Copilot Code, Autopilot, Workspaces and the Today features require the tenant to be enrolled in Frontier. Companies that are not in the program should plan for Copilot Code in December and treat the other agent features as previews without a date. Readers comparing what each plan includes can start from our guide to Microsoft Copilot pricing tiers and Microsoft 365 plans.

··········

[[ADPLUS_300x250_1]]

MICROSOFT 365 COPILOT PLUGINS FOR SALES, FINANCE, LEGAL AND SERVICE.

Microsoft is packaging role-specific capabilities as plugins made of prebuilt skills. According to the Copilot blog, the sales plugin contains 44 skills, the customer service plugin 13, the finance plugin 30 and the legal plugin more than 20. Dynamics 365 contributes more than 100 skills across Sales, Service, and Finance and Operations.

The Dynamics 365 Sales plugin, now generally available, covers account plans, meeting preparation, deal insights and pipeline prioritization, and can generate PowerPoint presentations. The ERP apps plugin, also generally available, brings ERP context and actions into Copilot Cowork and Excel, with source-to-pay processes given as an example. Salesforce in Copilot, in preview, adds more than 38 sales skills, while Sales Cloud in Teams is still in a pilot. Microsoft also announced a plugin catalog and a unified plugin registry so that administrators can discover plugins and manage them centrally, without giving dates for either.

··········

AGENT 365 COST MANAGEMENT AND COPILOT CREDITS.

Microsoft's commercial model for Copilot combines a per-user subscription with usage-based Copilot Credits for the most demanding tasks, such as agentic workflows. The Ignite materials do not state the price of either component. Agent 365 Cost Management is meant to keep that consumption under control from the Microsoft admin center: spend visibility, policies, limits and alerts are in preview, forecasting and optimization recommendations reach the Frontier program in November, and value insights plus delegation of budgets to business leaders are planned for December.

Agent 365 Business Analytics approaches the same question from the business side by tracking cost drivers and completed work, with scenario forecasts. Insights about Microsoft services require at least one Microsoft 365 Copilot license in the tenant, while insights about non-Microsoft services require an Agent 365 license. Microsoft also said that usage metrics for its own agents no longer require an Agent 365 license, whereas full governance needs an Agent 365 or ME7 license.

The day before Ignite, Microsoft published a cost comparison it commissioned from GigaOm. Microsoft ran the tests in October 2026 on 37 prompts across Copilot Chat, Cowork and the Office apps, modeling 100 tasks a month for an everyday user and 140 for an advanced user, including 20 agentic workflows; GigaOm validated the methodology and the cost analysis. The resulting monthly figures, based on list or assumed prices without negotiated discounts, are vendor-funded estimates rather than independent measurements.

........

Modeled user

Microsoft 365 Copilot

ChatGPT Enterprise

Claude Enterprise

Everyday user, per month

$30

$81

$205

Advanced user, per month

$96

$149

$299

Copilot saving claimed by Microsoft

Baseline

63% (everyday), 35% (advanced)

84% (everyday), 68% (advanced)

........

The comparison depends heavily on how usage charges accumulate for competing products, since Microsoft concedes that some competitor subscriptions may list $10 lower than Copilot before usage is added. According to the same post, usage charges erased that gap within the first week of modeled use. The figures describe Microsoft's chosen prompts and quality checklist, so buyers should test their own workloads before relying on them.

··········


MICROSOFT COPILOT AFTER IGNITE 2026: CONFIRMED RELEASES AND OPEN DATES.

The confirmed changes for licensed Microsoft 365 Copilot users are Office in Copilot, the worldwide rollout of Home, the Auto recommendation to Cowork, Copilot Managed Runtime and the two Dynamics 365 plugins, all described by Microsoft as generally available or rolling out. Copilot Code has a stated general availability month, December, and runs today in the Frontier program on Windows, the web, iOS and Android.

Autopilot, Copilot Workspaces, the Today features and Salesforce in Copilot remain in Frontier, private preview or preview, with no general availability dates. Microsoft has not published the price of Copilot Credits or the per-user cost of the redesigned subscription in its Ignite materials, and its cost advantage over ChatGPT Enterprise and Claude Enterprise rests on a study Microsoft funded and ran. Organizations planning budgets for 2027 have a clear December milestone for Copilot Code and a new set of Agent 365 controls for usage-based spending, while the timing of Autopilot's broad release is still undisclosed.
