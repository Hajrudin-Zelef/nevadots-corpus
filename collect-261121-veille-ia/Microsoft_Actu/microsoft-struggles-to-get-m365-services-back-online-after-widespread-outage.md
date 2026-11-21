---
id: collect-261121-veille-ia/microsoft-actu/microsoft-struggles-to-get-m365-services-back-online-after-widespread-outage
title: "Microsoft struggles to get M365 services back online after widespread outage"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "said", "online", "outlook", "servers", "services", "exchange", "traffic"]
source: https://www.computerworld.com/article/4248253/microsoft-struggles-to-get-m365-services-back-online-after-widespread-outage-2.html
source_anchor: ""
source_lines: [1, 53]
section: "Microsoft_Actu"
---
*Editor’s note: Microsoft has announced that it has fixed the underlying issue for the outage and is restoring services to customers.*

If you’re struggling to send or receive emails, you’re not alone. Microsoft has been unable to fully restore Microsoft 365 more than 13 hours after an Exchange Online failure took down Outlook and spread to Teams, SharePoint, Copilot and parts of the company’s security tooling, and it said overnight that its latest fixes were “not taking as expected.”

The outage began at 16:17 UTC on Nov. 12, according to Microsoft’s service health dashboard.

“Users may be unable to access their mailbox using Outlook on the web, Outlook desktop (MAPI), Exchange ActiveSync (EAS), or REST-based connections, and email delivery may be delayed,” Microsoft wrote in the dashboard.

Users in the Americas could not reach their mailboxes through Outlook on the web, desktop Outlook, mobile clients, or the REST API. Within two hours, Microsoft said users in any region could be affected. It opened a wider incident covering Teams, SharePoint Online, Copilot, Defender XDR, Intune, Power BI, and several Purview services.

Downdetector logged more than 130,000 Outlook problem reports at the peak.

In its latest service-health updates for incident MO1550821, Microsoft said it had observed a surge in calls from routing services to backend servers as it applied mitigation measures. The company said traffic rebalancing had contributed to the increased volume and that it was scaling virtual machines to add capacity and reduce the load.

“As we’ve applied our current ongoing mitigation actions, we’ve observed a surge in outbound call volume from our routing services to our backend servers,” Microsoft said in the update. It added: “We’re continuing to observe heightened volume due to traffic rebalancing actions that we have taken throughout this event.”

## Server failures behind the disruption

Microsoft initially investigated elevated infrastructure load and the possibility that a recent change had contributed to the incident. It later said the problem was not being driven by an overall increase in traffic.

“We’ve determined that the elevated load isn’t driven by an overall increase in traffic, but by a large number of servers being unexpectedly unavailable, which is shifting load onto the remaining infrastructure,” the company said in its Exchange Online incident update.

In a subsequent update, Microsoft identified a process responsible for establishing secure connections as a source of the problem.

“Our investigation has found that a process on the affected servers responsible for establishing secure connections becomes overwhelmed, causing the servers to stop processing requests,” the company said.

Microsoft said it was preparing a configuration change to address the behaviour and testing it on a subset of servers before wider deployment. It was also reverting a recent network configuration change that correlated with the start of the disruption, while restarting affected infrastructure and monitoring server stability.

The recovery process has involved restricting some traffic and gradually reintroducing connections as Microsoft checks whether servers can remain stable under load. The company also reported that some servers became unresponsive again after traffic was restored, requiring further analysis and mitigation.

## Impact extends beyond email

Microsoft’s incident notification said the impact varied by service and feature, depending in part on whether they relied on Exchange Online.

The company said users might be unable to access Exchange Online mailboxes through Outlook on the web, Outlook desktop, Exchange ActiveSync, or REST-based connections, and that email delivery might be delayed. It said Teams’ core messaging and online meetings remained largely functional, but Exchange-dependent features such as calendar access and meeting scheduling could fail.

Microsoft also said that direct access to files stored in SharePoint Online remained functional, while features dependent on Exchange Online could be affected. For Microsoft 365 Copilot, users might be unable to load Copilot Chat or receive responses, and Copilot experiences might be unable to access work data such as email, calendars, and files.

The notification also identified potential impacts to Defender for Office 365 Safe Links, which might be unable to verify URLs clicked in Outlook and Teams, and to some Defender XDR portal pages. Other reported impacts included selected Intune device enrolment and compliance scenarios and Purview functions such as data loss prevention, automatic labelling and communication compliance.

## Enterprise concentration risk

The incident has renewed attention on the operational dependencies created when organisations consolidate email, collaboration and security functions within a single cloud ecosystem.

“Enterprises have priced Microsoft 365 as a licence and never as a single point of failure,” said Sanchit Vir Gogia, chief analyst at Greyhound Research.

Gogia said that when mail, meetings and security functions such as Safe Links depend on the same service estate, an infrastructure disruption can affect several business functions at once.

“The answer is not a second suite. It is a rehearsed fallback for the few things that cannot stop: an out-of-band channel for incident command, mail continuity for critical roles, and a written decision on whether each security control fails open or closed,” he said.

For security teams, the distinction between a service being unavailable and a dependent security function being impaired is important. Microsoft’s notification about Safe Links, for example, points to a specific URL-verification impact rather than the loss of all Defender for Office 365 capabilities.

Gogia also said enterprise customers would want a detailed account of the incident, including a timeline showing when Microsoft identified the different causes and how it validated the recovery measures. Microsoft has continued to publish updates through its Microsoft 365 service status portal. The company has not yet published a final post-incident root-cause report.
