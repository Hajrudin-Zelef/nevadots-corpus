---
id: collect-261121-veille-ia/microsoft-actu/everything-microsoft-announced-at-ignite-2026-so-far
title: "Everything Microsoft Announced at Ignite 2026 So Far"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "copilot", "preview", "windows", "security", "generally", "available", "still"]
source: https://www.techrepublic.com/article/news-microsoft-ignite-2026-announcements/
source_anchor: ""
source_lines: [1, 52]
section: "Microsoft_Actu"
---
Some of Microsoftâs biggest Ignite announcements are ready for IT teams to try. Others are still waiting for a preview slot.

At Ignite 2026, which runs Nov. 17â20 in San Francisco and online, Microsoft announced updates across Copilot, Windows, security, and Azure. As of Nov. 19, Office editing inside Copilot and Azureâs new Cobalt 200 virtual machines are generally available, while Windows Server 2028 and Power BI Proâs new app-building tools are still in preview.

Hereâs what IT teams can use, what belongs in a test environment, and where to check before committing budget. This roundup covers the major announcements for IT teams. Microsoftâs Book of News has the full list.

## Whatâs live and whatâs still in preview

| Announcement | Status as of Nov. 19 | 
|---|---|
| Office in Copilot | Generally available | 
| Copilot Autopilot | Rolling out through Frontier | 
| Agent 365 Business Analytics | Rolling out through Frontier | 
| Windows Server 2028 | Insider preview; hybrid AI features have separate, limited early access | 
| Windows Endpoint Security Platform | Generally available | 
| Azure Cobalt 200 VMs | Generally available in select regions | 
| Azure HorizonDB | Generally available; Fabric HorizonDB is a separate preview | 
| Power BI Pro data apps | Preview | 

## Microsoft 365 and Copilot: What changes for users

Word, Excel, and PowerPoint inside Copilot let users edit files alongside a chat. Microsoft also highlighted Copilot Workspaces, the successor to Copilot Notebooks. Microsoft lists Workspaces in Frontier preview, and its support guidance says existing notebooks and sharing carry over. Bringing a personal Autopilot agent into a workspace is still forthcoming.

Autopilot is a separate rollout. The persistent Copilot agent can carry work forward between prompts and is rolling out through Microsoftâs Frontier early-access program. Teams that already know the Copilot Home, Code, and Autopilot redesign should look at what users can access in their own tenant before updating training. Microsoft and Salesforce also unveiled Copilotforce.

Its Salesforce plugin for Microsoft Copilot is in public preview; that status doesnât apply to every integration announced under the partnership. Copilot Managed Runtime, Microsoftâs hosting and governance layer for compatible apps, is generally available.

Agent 365 Business Analytics provides leaders with visibility into AI spending and usage. Microsoft describes its âassisted hoursâ metric as an estimate of work AI helped complete. It doesnât measure time saved or financial return. For a pilot, compare it with actual task completion time and how much work people had to redo.

## Windows and security: What to test first

The Windows Server 2028 preview gives admins a chance to see how a hardened Server Core configuration and a new security baseline affect existing workloads.

Microsoft is offering those security changes through the Windows Server Insider Program, with a separate, limited early-access program for hybrid AI features. Use a lab to find out whether older applications and management tools still work before making upgrade plans.

On the PC side, Microsoftâs Windows and Intune updates include the generally available Windows Endpoint Security Platform, which gives approved security products a new way to protect devices. Windows baseline protection is still in preview. Those are separate changes, so check your security vendorâs support before counting on the new platform.

Microsoft also made Perception generally available. It uses specialized agents and playbooks for security work such as alert triage and investigation. Some newer capabilities remain in preview, so check individual features rather than assuming the whole package is ready to deploy.

## Azure and data: Whatâs ready to benchmark

Microsoft announced new AI models, including the generally available Microsoft-Decision-1.1. Microsoft says the model helps Copilot decide how much reasoning effort a task needs.

Azure Cobalt 200 VMs are generally available in select regions. Microsoft says the Arm-based instances deliver up to 50% better performance than Cobalt 100. Benchmark an actual workload in your target region and check that its dependencies support Arm before planning a move.

Microsoft also made its managed PostgreSQL-compatible Azure HorizonDB generally available. Fabric HorizonDB, a separate serverless PostgreSQL offering inside Fabric, is in preview. And in Power BI Desktop, Power BI Pro users can preview data apps that collect inputs and write changes back to a data source. An internal approvals workflow could be a useful test, provided the team verifies who can change what.

Microsoft also announced a long-term Azure agreement with Intel covering multiyear Xeon supply and advanced chip packaging. Itâs a roadmap announcement, separate from the Arm-based Cobalt instances customers can deploy now.

For IT teams, the best first step is to pick one change that solves a current problem and specify a test it must pass. A benchmark, a compatibility check, or a timed workflow will tell you more than the keynote did.

**Related reading: See how severe weather disrupted Igniteâs show floor and where attendees could find session updates.**
