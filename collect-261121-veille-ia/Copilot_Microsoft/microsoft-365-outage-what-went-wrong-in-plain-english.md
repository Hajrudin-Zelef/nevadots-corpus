---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-365-outage-what-went-wrong-in-plain-english
title: "Microsoft 365 outage: what went wrong, in plain English"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "servers", "microsofts", "email", "outlook", "outage", "back", "three"]
source: https://office-watch.com/2026/microsoft-365-outage-explained/
source_anchor: ""
source_lines: [1, 197]
section: "Copilot_Microsoft"
---
The Microsoft 365 outage on November 12 and 13 cut off email and Microsoft now says three separate problems lined up to cause it. If you couldn’t open your mailbox in Outlook, Outlook on the web or the Outlook phone app, you weren’t alone. The good news: your email wasn’t lost and there’s nothing to fix on your own computer. The one thing worth doing is checking that anything important you sent on Thursday or Friday actually arrived. Here’s what went wrong, in plain English plus Microsoft interim incident report.

## What happened

**Microsoft 365 email was down for over a day** last week, and Microsoft’s preliminary report says three separate problems lined up to cause it. Customers couldn’t open their mailbox in Outlook, Outlook on the web or the mail app on their phone. The little email that did get through was often late. Teams, SharePoint, OneDrive, Copilot and other services had problems too, mostly in the parts that rely on **Exchange Online** (Microsoft’s email service). Teams chats and meetings kept working for most people.

As with many modern system failures, there wasn’t a single cause. Instead, it was a “sleeper” bug that had been sitting on Microsoft’s systems for over three months before a sequence of events triggered the bug which then cascaded through the servers.

## The hidden bug in front door servers

The root cause was a **hidden bug** on Microsoft’s “front door” servers (Microsoft calls them the **networking gateway**). Connections to Exchange Online and many other Microsoft 365 services pass through these servers. In July, Microsoft rolled out a Windows fix just for these servers, and it ran without trouble for more than three months. But under heavy load on newer servers with lots of processor cores, the fix made the machines seize up. Microsoft’s lab testing used less powerful hardware with fewer cores, and different traffic, so it never found the problem.

## The domino effect

On the day, two more things piled on. About 15 hours before the outage, Teams traffic was automatically moved onto the same gateway Exchange uses, which is a routine move. And nearly 14% of server capacity was already out of action because of an unrelated fault the day before.

When US business hours hit their peak, servers started locking up. Microsoft’s automatic safety net did what it always does and moved the traffic to the servers that were still healthy. Those servers then locked up too.

It was a **domino effect**. In under two hours, more than 80% of the servers had stopped responding. The barriers that should stop a failure in one region from spreading to others didn’t work, for a reason Microsoft hasn’t explained yet.

Recovery was slow because the waiting traffic swamped every server the moment it was restarted. Microsoft’s usual “undo” tools didn’t help either, because the faulty change was months old. At 7:20 AM UTC Friday, engineers started rolling out a fix that put the code back to its earlier working state. They brought servers back in batches, starting with Asia Pacific and then Europe. Webmail was back to normal worldwide by 2:17 PM UTC. Microsoft didn’t declare the incident over until 4:10 PM UTC Friday.

## What does this mean for me?

**Email wasn’t lost.** Microsoft gave incoming messages extra time to be delivered, so they weren’t bounced back as undeliverable.

Outgoing messages from desktop Outlook should have stayed in the Outbox until a connection was reestablished. Outlook mobile apps may have failed to send. If you sent something important on Thursday or Friday and haven’t heard back, it’s worth checking with the recipient.

**There’s nothing to fix on your own PC, Mac or device.** The bug was in a Windows fix made only for Microsoft’s gateway servers.

**The uncomfortable lesson:** Exchange Online can’t switch away from this gateway “for scale reasons.” So when that one front door jams, email stops for nearly everyone.

A **final report** with Microsoft’s longer term fixes is due by November 20, 2026.

## Full report: Preliminary Post Incident Report for Microsoft 365

*Here’s the full interim report for everyone to read. Microsoft 365 admins can see it via EX1550543.*

*Microsoft’s wording is kept exactly except for the boilerplate legal disclaimers.*

**Report Date:** November 15, 2026

### Incident Information

| Field | Details | 
| **Important Note** | This is a preliminary Post Incident Report (PIR) that is being delivered to provide early insight into details of the issue. The information in this PIR is preliminary and subject to change. A final PIR will be provided within five (5) business days from full event resolution and will supersede this document upon publication. | 
| **Incident ID** | EX1550543/MO1550821 | 
| **Incident Title** | Users may be unable to access or perform several actions in Microsoft 365 services. Users may have been unable to connect to their mailboxes in Exchange Online through multiple connection methods. | 
| **Service(s) Impacted** | Microsoft 365 services, including Exchange, Copilot, Teams, SharePoint, OneDrive, Defender, Purview, Fabric, Intune, Planner, Power Apps, To Do, Power BI, and Universal Print. | 

### User Impact

**Primary impact scenario:** The most significant impact affected Exchange Online, with most users being unable to access their mailbox through Outlook or other mail clients. Additionally, email delivery was delayed for many users.

**Secondary impact scenarios:** Other services that utilize capabilities of Exchange Online were impacted, though most of these services largely remained online and accessible. This included, but was not limited to, elements of Copilot, Teams, SharePoint, OneDrive, Defender, and Purview that interact with Exchange Online.

### Scope of Impact

This event affected most users when they accessed or used features that rely on Exchange Online. The severity of impact varied by region, connection method, and time over the course of the incident. Partial or intermittent access may have occurred while the service was degraded.

### Incident Start Date and Time

Thursday, November 12, 2026, at 4:20 PM UTC

### Incident End Date and Time

Friday, November 13, 2026, at 4:10 PM UTC

### Root Cause

The incident on Thursday, November 12th, 2026, was caused by three main factors that together caused a global outage for the networking gateway that fronts many Microsoft 365 services.

First, a latent defect caused Windows servers to become overwhelmed and unresponsive under certain load conditions. This was due to lock contention inside an internal Windows process which caused the machine’s resources (CPU & memory) to be exhausted. This defect was the result of a fix that was prepared specifically for Microsoft 365 networking gateway servers; neither the underlying defect nor the fix applies to broader Windows deployments.

The initial change in behavior first started deployment in July of this year. The update went through safe deployment and saturated globally over several weeks, during which time no issues were observed. For more than three months, the change was deployed worldwide without impact.

Our normal load testing did not detect the latent issue because the traffic mix and SKUs used in our labs. The contention issue is worsened on machines with more cores and newer hardware; whereas our load testing tended to focus on weaker hardware with fewer cores.

Second, a surge in load increased the likelihood for the networking gateways to encounter the contention. Within Microsoft 365, we maintain two independent and heterogenous network gateway systems designed for isolation and high availability. This allows many Microsoft 365 services to fail to/from them during outages or disruptions. Approximately fifteen hours before the outage, Teams failed over from the alternative networking gateway to the ‘primary’ gateway that also supports Exchange Online. This increased the load on the networking gateway and changed its traffic patterns. This type of failover is part of normal operations and is integral to standard Microsoft resiliency efforts.

Third, at the same time, nearly 14% of server capacity for the networking gateway was out of service due to an unrelated issue from the previous day. This server capacity was on track to be returned to active service through routine maintenance and health processes. The service had sufficient capacity headroom, so the reduction in server capacity had no impact on its own.

Taken together, the increased load and the reduced server capacity, caused the network gateway to encounter the latent bug from July. This caused requests to build up until servers ultimately stopped responding. This occurred during the peak window of traffic in the United States.

When servers lose connectivity, our traffic management systems automatically route users to other healthy endpoints. This happens often and is critical to avoiding any interruption due to hardware or software defects and is normally what makes the service resilient. However, the redirected traffic increased the likelihood for the same problem to occur on the remaining endpoints. This caused a cascading failure from server to server as the same amount of load was spread across fewer servers. In less than two hours, the previously unknown defect was encountered across our fleet and rendered them unresponsive. At its lowest, more than 80% of servers were unresponsive.

Normally, the network gateway is composed of thousands of ‘capacity units’ which represent shards of capacity, hardware, and networking infrastructure. These ‘capacity units’ are then organized into regions. In this case, due to a separate issue, the normal bulkheads between these different partitions did not stop the cascading failure. This is what allowed the impact to spread across our network gateway infrastructure globally.

Those services that were able – like Teams, OneDrive, SharePoint, and Office apps – failed out of the faulty network gateway infrastructure. That is why those services were partially restored earlier, with Teams chats and meetings functioning without issue for most of the outage. But not all services, like Exchange Online, can fail out of this network gateway for scale reasons.

Recovery took longer than usual because as servers returned to service, they were immediately overwhelmed by waiting traffic and became unresponsive again. Our normally automated recovery processes were not designed to restore service from this broad unresponsive state. Additionally, since the main code defect was a latent bug from several months previous, our normal rollback, flighting, and feature flags were unable to quickly revert the change.

After engineers confirmed the defect as the cause, we developed an update that returned the code path to the previously known good state in Windows. To effectively apply this fix, and to slowly bring the service online again, we took groups of servers out of service, applied the change, and restarted them before returning them to service. This process was carefully applied to large quantities of infrastructure at a time (e.g. 50%+ of a region).

The final Post-Incident Review (PIR) will provide more details.

### Actions Taken (All times UTC)

**Thursday, November 12**

**1:07 AM:** The automated failover system moved Teams traffic onto the network gateway front end servers shared with Exchange Online.

**1:07 AM to 4:00 PM:** Load on the front-end servers increased gradually. Isolated server failures began in some regions during this period, automated mitigations applied.

**4:03 PM:** Our connectivity anomaly detection monitor identified an increase in end-user connectivity issues.

**4:17 PM:** Due to the increase in connectivity issues observed, automation began to redirect traffic to alternate healthy endpoints.

**4:20 PM:** We received a high-priority alert indicating impact to Exchange Online services, which triggered immediate communication to the Service Health Dashboard under EX1550543.

**4:25 PM:** We confirmed that customers were experiencing issues across multiple Exchange connection methods, including Outlook, Outlook on the web, Exchange ActiveSync, and Messaging and REST APIs. We began reviewing internal telemetry.

**4:33 PM:** We received the first support case associated with the broader incident, confirming that customers were reporting issues alongside those already detected through our monitoring. Users reports began to accumulate on 3rd party outage reporting sites such as DownDetector.

**5:03 PM:** Our connectivity anomaly detection monitors began to see a further increase in connectivity issues.

**5:32 PM:** Automated recovery actions continued to redirect traffic to healthy endpoints; however, further service degradation was detected across multiple regions.

**5:45 PM:** We identified connectivity errors occurring on a portion of infrastructure which receives and processes users’ access requests. We collected network logs to further investigate the source of the errors.

**6:00 PM:** We progressively restarted and attempted to repair impacted portions of our infrastructure and worked to redirect traffic to alternate infrastructure to provide relief.

**6:27 PM:** We published additional communication under MO1550821 to the Service Health Dashboard, describing impact to additional services affected by this event, including Microsoft Teams, Copilot, and Defender.

**6:33 PM:** We triggered failovers to ensure traffic was distributed evenly across our infrastructure and components that manage user connections.

**7:00 PM:** We expanded our response to accelerate mitigation efforts. As a precaution, we paused all ongoing and planned deployments and configuration changes.

**7:20 PM:** We determined that the elevated load we were observing was not driven by an overall increase in traffic, but by many servers being unexpectedly unavailable, which was shifting load onto and overwhelming the remaining infrastructure.

**8:00 PM:** We deployed a targeted change to bypass components we found to be triggering failed health checks on affected servers while we also investigated why those servers were showing elevated processor usage while handling requests.

**8:30 PM:** While attempting to validate the effectiveness of our targeted change, we found that many of the affected servers had become unresponsive. We began work to restart those servers.

**9:00 PM:** We prepared an alternative health check method on a subset of affected infrastructure, which allowed the servers in that subset to successfully return to service, and we began applying that change more broadly. In parallel, we prepared an automated recovery process to continually detect and restart unresponsive servers, to help keep them in service while the change progressed.

**10:00 PM:** Our telemetry and signals confirmed that the actions we’d taken to restore service had not been effective, and we broadened the scope of our investigation and response accordingly. We began analyzing traffic patterns across our entire service infrastructure and evaluating whether isolating traffic within each region would allow affected regions to recover independently.

**11:00 PM:** Work was underway to return a portion of the affected servers to service and shift traffic and connections to alternative pathways wherever possible. We observed limited recovery in some regions. To further support recovery, we implemented protective traffic management measures in a specific location to reduce load on the affected infrastructure, with plans to expand these measures to additional locations if we observed positive results.

**11:55 PM:** We determined that the servers we’d returned to service initially remained healthy but then become unresponsive again upon receiving traffic.

**Friday, November 13**

**12:30 AM:** We identified the connection negotiation as the primary area responsible for the widespread access failures and determined that competing processes were preventing connections from being handled successfully.

**1:00 AM:** Our investigation found that a process on the affected servers responsible for establishing connections was becoming overwhelmed, causing the servers to stop processing requests.

**1:15 AM:** We introduced measures to reduce pressure on the affected systems, including adjustments to monitoring activity and service load. These measures were partially successful and helped stabilize the service while we continued working toward broader recovery.

**1:53 AM:** We disabled some automated traffic redirection mechanisms performed by our connectivity anomaly detection monitors.

**2:10 AM:** We applied infrastructure capacity improvements where possible in an effort to help support traffic in regions coming online.

**3:00 AM:** While monitoring the progression of the changes applied to improve service health, we determined those changes were not applying as expected and established new workstreams in response.

**4:00 AM:** We worked to improve service stability through traffic protocol adjustments, traffic redistribution, and strategic traffic blocking measures. We observed partial recovery in regions we applied adjustments to, but these actions were not resulting in sustained recovery.

**5:00 AM:** We encountered persistent challenges with machines being stuck in bad states, preventing our changes from syncing as expected. We scaled up virtual machines to increase capacity and reduce service load.

**7:20 AM:** We identified a mitigation and applied the mitigation, an update that reverted the defective change to a portion of the capacity.

**8:00 AM:** We confirmed server stability across the portions of infrastructure to which we’d applied the update and initiated its expansion systematically to focus on infrastructure supporting regions that were currently within or entering peak business hours.

**10:00 AM:** The service began to stabilize, though capacity had not returned to normal levels.

**11:00 AM:** Recovery actions were completed across portions of the affected infrastructure in the Asia Pacific region. Traffic continued to be gradually restored, and we observed improvements in key customer scenarios including mailbox access and email processing.

**11:35 AM:** Availability in Europe returned to healthy levels, indicating that services in the region were stabilizing.

**1:00 PM:** Recovery progress was sustained across all regions and queues of delayed emails messages began to reduce.

**2:17 PM:** Global webmail availability returned to expected levels, and by 2:44 PM the improvement had remained stable under continued monitoring.

**3:05 PM:** We completed the move of eligible customer traffic to alternative connection paths. This further improved sign-in and service access across affected regions.

**3:54 PM:** Most major access methods had recovered to their normal traffic levels, however monitoring for full recovery was still in progress.

**4:10 PM:** We confirmed that customers were no longer experiencing active impact from the incident and moved into an extended monitoring and backlog recovery phase.

**After 4:10 PM:** We continued to monitor service for sustained recovery, ensuring that all queued mail had been processed, and as load continued to increase, we confirmed the service remained healthy.

**Saturday, November 14**

**4:15 AM:** After extended monitoring, which confirmed service stability, we issued our final customer updates to EX1550543 and MO1550821.

### Mitigation / Repair Items

*The repair items below focus on the actions that restored service and have been applied as protections since the incident. Many longer-term changes from our full root cause analysis will be in the final Post Incident Report later this week.*

| Action | Status | 
| A comprehensive list of repair items will be delivered as part of the final PIR | 11/20/2026 | 
| Developed an update that returned the connection handling code path to the previously known good state | Completed | 
| Extended the email delivery window to ensure delayed messages weren’t returned as undeliverable and all messages that were queued for delivery during the event were processed. | Completed | 
| Added additional capacity as a proactive measure to help with recovery. | Completed | 
| Additional monitoring for traffic-redirection patterns and connection-handling health. | Completed | 
| Implemented guardrails for traffic distribution to avoid cascading impact. | Completed |
