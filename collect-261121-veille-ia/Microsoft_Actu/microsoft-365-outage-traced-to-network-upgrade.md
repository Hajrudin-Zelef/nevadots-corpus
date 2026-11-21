---
id: collect-261121-veille-ia/microsoft-actu/microsoft-365-outage-traced-to-network-upgrade
title: "Microsoft 365 Outage Traced to Network Upgrade"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["microsoft", "network", "outage", "teams", "services", "online", "email", "traced"]
source: https://www.technology.org/2026/11/16/microsoft-365-outage-network-capacity-upgrade-outlook-teams/
source_anchor: ""
source_lines: [1, 35]
section: "Microsoft_Actu"
---
## Microsoft 365 Outage Explained

The fault that took Outlook and Teams offline for about a day had been waiting inside Microsoft’s network before anyone noticed it. Microsoft has apologized and traced the global Microsoft 365 outage of November 12 and 13 to a problem introduced during an earlier network capacity upgrade, which surfaced only after a more recent network change.

### **Key Takeaways**

- The disruption began on Thursday, November 12, and services recovered through Friday. Outlook, Teams, SharePoint and Copilot were among those affected.
- Microsoft says the cause was technical and unrelated to a cyberattack or to AI agents.
- A preliminary post-incident report is expected within two business days and a final version within five.

## How the Failure Spread

Microsoft’s records put the start at 16:17 UTC on Thursday, when users began losing access to Exchange Online mailboxes. A wider Microsoft 365 incident followed at 18:26 UTC. Problems appeared first in the Americas and then in other regions.

According to the company’s status updates, Exchange Online servers failed automated health checks and dropped out of service. Their load moved to the remaining servers until those were overwhelmed as well. Microsoft said a rise in traffic was not the trigger.

Services that depend on Exchange Online went down with it, including Teams, SharePoint Online, Microsoft 365 Copilot, Intune and Defender.

Recovery was slow by design. Microsoft paused all deployments, moved traffic off the affected infrastructure and restarted servers. Late on Thursday it said those steps had not brought the expected relief and changed its approach. It then restored service in stages and checked stability at each one. By Friday morning it reported recovery in North America, sustained improvement in Europe and early signs of recovery in Asia Pacific.

The company also extended email timeouts to 48 hours so that queued messages would be delivered instead of returned.

## Who Was Affected

Downdetector logged more than 100,000 reports within an hour of Microsoft’s first notice. Businesses, schools, governments and healthcare providers reported disruption. Staff at the UK’s National Health Service reportedly lost email and Teams for the day, which delayed some services. New York City Emergency Management warned of delays to city email and online services.

“This was a technical issue not related to cyber or AI / agents,” Microsoft said. Charles Lamanna, an executive vice president, said teams are reviewing what happened and how to prevent a repeat.

## Dependence on a Single Cloud

The outage ranks among the longest Microsoft 365 disruptions of recent years, and it was not a one-off. In July, a bug in an automated network maintenance system removed routes from more devices than intended and disrupted Azure and Microsoft 365. In January, another outage lasted more than nine hours during maintenance. Three incidents tied to network changes in one year show how hard it is to modify infrastructure of this size without side effects. The pressure to keep modifying it is rising, because the same providers are borrowing heavily to add AI data center capacity.

For customers, the lesson concerns concentration. Email, meetings, files and security tools bought from one vendor fail together when a shared layer breaks, and businesses, schools and governments around the world lost email at the same time. Public services have renewed questions about relying on a single supplier for daily communication. Recent failures elsewhere make the same argument for independent fallbacks, including Yandex Cloud’s loss of customer data.

*Written by Vytautas Valinskas*
