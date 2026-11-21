---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-365-outage-what-microsoft-says-broke-and-what-it-still-owes-customers
title: "Microsoft 365 Outage: What Microsoft Says Broke and What It Still Owes Customers"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-12"]
keywords: ["microsoft", "servers", "outage", "says", "incident", "affected", "still", "customers"]
source: https://datafloq.com/microsoft-365-outage-what-microsoft-says-broke-and-what-it-still-owes-customers/
source_anchor: ""
source_lines: [1, 91]
section: "Copilot_Microsoft"
---
Microsoft 365 customers lost dependable access to Outlook mailboxes for more than a day last week, and Microsoft has so far offered only a one-sentence preliminary cause. The company says an issue during an earlier network capacity upgrade surfaced after a recent network change, and that the outage was a technical failure with no link to cyberattacks or AI agents. The post-incident report that should show how routine infrastructure work disabled email across dozens of countries had not appeared in public reporting by Monday, November 16.

## **What happened on November 12 and 13**

Microsoft opened an Exchange Online incident, tracked as EX1550543, at 11:17 a.m. Eastern Time on Thursday, November 12, 2026, after users could not connect to their mailboxes. It confirmed the problem publicly at about 12:54 p.m., describing it as an access failure in the Americas, and opened a second incident, MO1550821, at 1:26 p.m. to cover Microsoft 365 services that depend on Exchange Online. Microsoft warned that users in any region could be affected, and they were: Downdetector registered reports from as many as 74 countries and territories within the first hour, according to Inc.

Reports on the outage tracker passed 100,000 within an hour of Microsoft’s first public post, ABC News reported, and peaked at about 130,000 for Outlook, according to CNN. Downdetector counts user submissions rather than affected accounts, and CNN noted that the peak understates the real scale because most people never report an outage.

The sequence below follows Microsoft’s service health updates as published by the outage trackers Epifive and StatusStack, cross-checked against news coverage. All times are Eastern.

| **Time** | **What Microsoft reported** | 
| Thu, Nov 12, 11:17 a.m. | Exchange Online incident EX1550543 opens: users may be unable to connect to their mailboxes | 
| 12:54 p.m. | Public confirmation of mailbox access problems in the Americas | 
| 1:26 p.m. | Broader incident MO1550821 opens for Microsoft 365 services that depend on Exchange Online | 
| 2:06 p.m. | Deployments and configuration changes paused | 
| 2:57 p.m. | Microsoft says it has found no recent change that correlates with the start of the impact, and that failed server health checks may trace to one component on the servers | 
| 3:56 p.m. | An alternative health-check method returns some servers to service; a few configuration updates come under investigation | 
| 5:00 p.m. | A core process on affected servers is becoming overwhelmed, and the servers stop responding | 
| 6:59 p.m. | Microsoft warns that restored servers may become unresponsive again once they take traffic | 
| 8:06 p.m. | A secure-connection process on affected servers is overwhelmed; Microsoft begins reverting a recent network configuration change | 
| Fri, Nov 13, early morning | A patch to part of the infrastructure shows stability gains; Microsoft reports early stabilization and extends an email timeout window to 48 hours | 
| Midday | Microsoft apologizes publicly and states a preliminary cause | 
| 11:17 p.m. | Final update: both incidents marked service restored | 

## **How Microsoft’s explanation changed**

Microsoft’s account shifted as engineers learned more. Early in the incident it attributed the errors to elevated load across multiple regions. About an hour later it said the load did not come from a rise in overall traffic. Servers had become unexpectedly unavailable, and that strained the infrastructure still running. Microsoft then tied the unavailable servers to failed automated health checks, the stage that Datafloq’s first-day report covered while the outage was still running.

At 2:57 p.m. Microsoft reported no recent change that matched the start of the impact and pointed to a component on the servers as the possible source of the failed checks. At 3:56 p.m. an alternative health-check method returned some servers to service. By 5 p.m. it said a core process on the affected servers was becoming overwhelmed. By 8:06 p.m. it named a secure-connection process as the one overwhelmed and said it was reversing a recent network configuration change that did correlate with the start.

Recovery then stalled. At 6:59 p.m. Microsoft warned that restored servers could become unresponsive again once traffic returned to them. Late Thursday it reported partial recovery in some regions but said the actions it had taken were not producing sustained recovery, and it described one serviceability change as not working as expected. It scaled up virtual machines to add capacity and linked a surge in outbound calls from routing services to the rebalancing of traffic.

On Friday, Microsoft named a preliminary cause: an issue during an earlier network capacity upgrade that surfaced following a recent network change. It said it had addressed that issue, would publish a full post-incident report, and called the failure a “technical issue not related to cyber or AI / agents.” Charles Lamanna, Microsoft’s executive vice president for Copilot, agents and platform, had said late Thursday that nothing indicated malicious activity.

One reading of those updates is that a network change exposed a weakness left behind by earlier capacity work, and that the weakness appeared first in the process that sets up secure connections to Exchange Online servers. Microsoft has not confirmed that sequence. The post-incident report is where it should appear.

That Microsoft named AI agents in its denial likely reflects the speculation that filled the first hours. Inc reported that many people suspected a hack, given the recent rise in AI-powered attacks, and some kept suspecting one after Microsoft’s update.

## **Which services failed, and which did not**

Exchange Online took the direct hit. Outlook on the web, Outlook desktop, Exchange ActiveSync and REST access all struggled to connect for affected mailboxes, and Microsoft warned that email delivery could be delayed. The broader incident listed Teams calendar and scheduling, Copilot, SharePoint features that depend on Exchange, Defender, Purview, Planner, To Do, Intune, Power BI, Windows 365 and the admin centers. Microsoft’s incident text listed Teams calendar and scheduling as affected and said Teams chat, channel messages and meeting joins were not impacted.

Early reports also listed Azure as down. Microsoft’s incident did not include Azure, and the Azure status page showed no active events on Friday morning, according to UptimeRobot’s review of the incident. ITPro reported that business and enterprise customers were hit much harder than consumer users, based on Microsoft’s health status page.

The failure also traveled through integrations. Anthropic reported errors in its Microsoft 365 connector at 12:52 p.m. Thursday. Zendesk reported mail delivery problems with its Exchange connector at 1:15 p.m., and Twilio reported email delays to Microsoft-hosted inboxes at 2:42 p.m. OpenAI said new connections from ChatGPT to Microsoft 365 might be broken. Any workflow that read from or wrote to Exchange Online through connectors like these inherited the outage.

## **The cost, and who felt it**

Anderson Economic Group, a Michigan research firm, estimated that lost productivity could approach $7 billion. Its president, Patrick Anderson, told CNN that as many as 26 million users on Thursday and 51 million on Friday may have had their work disrupted enough to cause an economic loss, and that the estimate would probably rise. The figure comes from a preliminary model. It does not total losses reported by companies, and no verified global figure exists.

The disruption reached public services. New York City Emergency Management warned residents of possible delays reaching city agencies by email, and other US local governments asked residents to call instead of write. In Australia, court live feeds went offline and schools texted parents to phone in absences, CNN reported. The Australian Broadcasting Corporation reported missed job interviews and supply problems at distribution centers that stock supermarkets.

In England, NHS England said local services were enacting long-standing measures for IT outages, including paper records where possible, and told patients who needed care to keep using emergency numbers or their local pharmacy. Guy’s and St Thomas’ NHS Foundation Trust said it could not send or receive email.

Companies in the Gulf improvised with what they had. Danube Group, a UAE company, moved some transactions to WhatsApp because the loss of email delayed financial approvals, Arab News reported, and a Saudi school used WhatsApp to distribute exam materials. Tamer Group, a Saudi company, said it had alternative access through Google accounts and reported no breach linked to the incident.

## **Why recovery outlasted the fix**

Microsoft said on Friday morning that email queues were approaching normal levels but warned that intermittent access and delayed delivery remained possible. It later said it restored service gradually because of the scale of demand, validating stability at each stage, and acknowledged that the approach took longer. Overnight, Microsoft extended an email timeout window to 48 hours, which UptimeRobot read as giving delayed messages more time to arrive.

Microsoft’s final incident entry lists the Exchange Online impact window as 5:05 a.m. Eastern on Thursday to 11:10 a.m. on Friday, just over 30 hours, according to Office Watch and StatusStack. That start time falls about six hours before the incident first opened at 11:17 a.m., which accounts for the gap between 30 hours and the roughly 24 hours between the first entry and the stated end. Microsoft kept monitoring the email backlog after the window closed and posted its last update at 11:17 p.m. on Friday.

Restoring server capacity did not clear the mail waiting behind it, so customers faced two recoveries: access to the mailbox, then delivery of the backlog. Each one ran on its own clock.

## **What the service credits cover**

Under the Exchange Online service level agreement as Epifive summarizes it, a monthly uptime below 99.9% earns a 25% credit, below 99% earns 50%, and below 95% earns 100%. Thirty hours of downtime in a 30-day month works out to about 95.8% uptime. That puts the outage in the 50% tier. Office Watch notes that the 100% tier would need more than 36 hours of downtime in the month, which this incident alone does not reach.

The credit applies to the downtime the agreement defines, which Epifive describes as users being unable to send or receive email through Outlook Web Access. Consumer plans do not qualify, Office Watch reported. Direct customers have until December 31, 2026 to claim, and resellers set earlier deadlines. The SLA treats credits as the sole remedy, so the agreement itself does not compensate for lost business. Companies should check their own contract terms, since Teams and other services may not meet the threshold at all.

## **A pattern across 2026**

This was not Microsoft’s first multi-service disruption this year. On January 22, Microsoft acknowledged problems at 19:37 UTC and said it had restored access by 05:33 UTC the next day, nearly ten hours later, blaming a portion of its infrastructure in North America that was not processing traffic as expected, The Register reported. On August 31, an Exchange Online outage that Microsoft tied to core authentication configuration issues also hit Teams, SharePoint, Purview, Defender and Copilot, and took roughly three days to clear fully, according to BleepingComputer.

Microsoft pointed to its own infrastructure or configuration in all three cases, and to an outside attacker in none. The data suggests that for tenants of a hyperscale cloud, the vendor’s own change process is a more likely source of downtime than an adversary, at least on this record of three incidents. Three incidents are a small sample, and the post-incident reports will say more about whether they share a cause.

## **What operators should take from it**

The implication for operators is practical, and most of it can be done before the next incident.

- **Keep incident communications off Microsoft identity.** BeyondTrust chief security adviser Morey Haber, quoted in Arab News, said a recovery plan that depends on the provider that failed has a fundamental weakness, and that extra copies of documents help little when staff cannot log in to open them. A call tree, a status channel and an emergency contact list should still work when Exchange Online and Microsoft sign-in are both unavailable.
- **Name one trusted source for status updates.** Kaspersky researcher Leonid Bezvershenko warned that outages invite fake password resets, cloned login pages and help-desk impersonation, and advised sticking to a single verified source. Sophos’ Aiden Sinnott advised caution with messages that appear to come from Microsoft or from affected businesses. Staff should know in advance where official updates will appear.
- **List the automations that depend on Exchange Online.** The Anthropic and OpenAI connectors, Zendesk’s Exchange connector and Twilio’s email delivery all reported problems. Any support routing, scheduled report or AI agent that reads or sends mail through Exchange Online fails along with it, so a company needs to know which of its own workflows sit on that dependency before the next outage.

## **What the post-incident report must answer**

Microsoft has committed to a preliminary post-incident report within two business days and a final report within five. Counted from Friday, November 13, StatusStack puts those dates at about November 17 and November 20, though Microsoft has not published dates. Microsoft has said the report will cover the root cause and repair items.

The report should show how a flaw from earlier capacity work passed testing and stayed hidden until a later change exposed it. It should also settle the questions left open on Thursday night: why the servers failed their automated health checks, and why restored servers froze again under traffic. A third is why Microsoft’s first analysis found no recent change matching the start of the impact when later updates tied the failure to one.

Customers should read the corrective actions most closely, because a fix for this one fault does little for the next network change that interacts badly with older work. If Microsoft publishes on schedule, the answers should arrive within days, and they will show whether the company is changing how it rolls out infrastructure work or only repairing what broke.
