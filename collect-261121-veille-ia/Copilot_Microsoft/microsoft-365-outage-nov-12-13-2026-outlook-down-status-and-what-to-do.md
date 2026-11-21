---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-365-outage-nov-12-13-2026-outlook-down-status-and-what-to-do
title: "Microsoft 365 Outage (Nov 12-13, 2026): Outlook Down, Status and What to Do"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["microsoft", "outlook", "service", "status", "health", "exchange", "users", "incident"]
source: http://dev.to/epifive/microsoft-365-outage-nov-12-13-2026-outlook-down-status-and-what-to-do-2d37
source_anchor: ""
source_lines: [1, 180]
section: "Copilot_Microsoft"
---
**Updated 2026-11-13, 11:50 IST:** Microsoft's 06:17 UTC update still lists the incident as "Service degradation". It is now evaluating traffic throttling, protocol adjustments and failover routing, says it is "not aware of any malicious activity", and promises a Post-Incident Report "once mitigated". Status, timeline and the PIR section are updated below.

**Updated 2026-11-13:** Rewritten with Microsoft's full Service health timeline for EX1550543 and MO1550821 (the Exchange incident started at 11:17 a.m. ET, earlier than first reported), the latest status, the cause as Microsoft has described it so far, user workarounds, and the SLA credit terms from Microsoft's November 2026 SLA.

A major Microsoft 365 outage that began on 12 November 2026 is stopping people from opening Outlook mailboxes and is breaking email-dependent features in Teams, Copilot, Defender, Purview, Planner, To Do, Intune and other services, in every region. Microsoft tracks it as **EX1550543** (Exchange Online) and **MO1550821** (multiple Microsoft 365 services). **Status as of 11:47 IST, 13 November 2026 (06:17 UTC; 1:17 a.m. ET):** still "Service degradation"; Microsoft is evaluating traffic controls "to increase capacity and stability", recovery is partial and not sustained, and no root cause or post-incident review has been published yet. If Outlook or Teams is failing for you, it is very likely this incident, not your PC.

This guide is for IT admins and small-business owners who run on Microsoft 365, and for anyone asking "is Outlook down?" today. It covers what Microsoft has said, with its own timestamps, how to confirm the problem is on Microsoft's side, what users can still do, what admins should do now and after recovery, and how the SLA credit process works. EpiFive is not affiliated with Microsoft; every quote below comes from Microsoft's service health posts or from named news reports.

Microsoft's service health record for EX1550543 gives the start time as **16:17 UTC on 12 November 2026**. That is 11:17 a.m. ET and 9:47 p.m. IST. The broader incident, MO1550821, starts at 18:26 UTC. Microsoft's first admin note said: "We're investigating a potential issue and checking for impact to your organization."

The timeline below uses the times of Microsoft's admin-center posts, as published in a public mirror of the Service health feed that the University of Pennsylvania maintains for its own tenant. Posts on the @MSFT365Status account on X can differ by a few minutes. ET is Eastern Standard Time (UTC-5); IST is UTC+5:30.

| ET (12-13 Nov) | IST | What Microsoft said or did | 
|---|---|---|
| 11:17 a.m., 12 Nov | 9:47 p.m., 12 Nov | EX1550543 start time: "Users may be unable to connect to their mailboxes in Exchange Online through multiple connection methods" | 
| 12:45 p.m. | 11:15 p.m. | Connectivity errors on "a portion of Exchange Online infrastructure which receives and processes users' access requests" | 
| shortly before 1 p.m. | about 11:25 p.m. | First public post on @MSFT365Status: "investigating reports of users experiencing issues accessing Exchange Online mailboxes through multiple connection methods in the Americas" | 
| about 1:00-1:30 p.m. | about 11:30 p.m.-midnight | Downdetector user reports for Outlook peak (see the note below the table) | 
| 1:26 p.m. | 11:56 p.m. | MO1550821 opens: "Users may be unable to connect to multiple Microsoft 365 services" | 
| 2:06 p.m. | 12:36 a.m., 13 Nov | "we've paused all deployments and configuration changes while we review recent changes for any correlation to this event" | 
| 2:18 p.m. | 12:48 a.m. | "the elevated load isn't driven by an overall increase in traffic, but by a large number of servers being unexpectedly unavailable, which is shifting load onto the remaining infrastructure" | 
| 2:57 p.m. | 1:27 a.m. | "we haven't identified any recent changes that correlate with the start of impact"; failed health checks may come from "a specific component" on the servers | 
| 3:56 p.m. | 2:26 a.m. | An "alternative health check method" returned some servers to service; "a small number of configuration updates" under investigation | 
| 5:00 p.m. | 3:30 a.m. | "our actions taken thus far have not provided the anticipated relief" | 
| 5:57 p.m. | 4:27 a.m. | Some servers back and stable; "limited recovery in some regions" | 
| 6:59 p.m. | 5:29 a.m. | Restored servers "can become unresponsive again once they begin receiving traffic" | 
| 8:06 p.m. | 6:36 a.m. | "a process on the affected servers responsible for establishing secure connections becomes overwhelmed"; Microsoft "begun reverting a recent network configuration change that correlates with the start of impact" | 
| 9:54 p.m. | 8:24 a.m. | A change applied to improve serviceability "is not taking as expected" | 
| 10:34 p.m. | 9:04 a.m. | "there is no indication this is the result of malicious activity" | 
| 11:10 p.m. | 9:40 a.m. | "partial recovery across regions", but "these actions aren't resulting in sustained recovery" | 
| 11:57 p.m. | 10:27 a.m. | A "surge in outbound call volume" from routing services; scaling up virtual machines; next update due 06:00 UTC (11:30 a.m. IST) | 
| 12:46 a.m., 13 Nov | 11:16 a.m. | "we are not aware of any malicious activity that is causing this incident"; a Post-Incident Report will follow "once mitigated" | 
| 1:17 a.m. | 11:47 a.m. | Evaluating "traffic throttling, protocol adjustments, and failover routing"; next update due 07:00 UTC (12:30 p.m. IST) | 

**Downdetector numbers are user reports, not Microsoft data.** The Tribune (ANI, citing Reuters) reported a peak of 138,058 Outlook reports around 1 p.m. ET; BleepingComputer reported more than 129,500 around 1:28 p.m. ET. The figures differ because they are snapshots taken at different moments.

MO1550821 says features "that interact with Exchange Online and email services are impacted", while parts that do not depend on Exchange "remain functional". Its latest list includes:

- 
**Exchange Online / Outlook:** mailbox access through Outlook on the web, Outlook desktop (MAPI), Exchange ActiveSync and REST; "email delivery may be delayed".
- 
**Microsoft Teams:** joining calls, meetings, group chats and channel messages are "not impacted", but calendar, scheduling, invites, live or virtual event creation and some Copilot features fail.
- 
**SharePoint Online:** features that depend on Exchange; "Accessing files stored directly in SharePoint Online remains functional".
- **Copilot, Defender for Office 365 and Defender XDR, Purview, Planner, To Do, Intune, Power BI and Fabric, Graph, Universal Print, Windows 365 (Customer Key), Microsoft 365 Education and both admin centers.**

A separate, earlier Teams incident, TM1550488, ran from 9:45 to 10:53 a.m. ET (8:15 to 9:23 p.m. IST) on 12 November, according to BleepingComputer. Microsoft has not said whether the two are related.

Microsoft's explanation has changed during the incident, so read it as work in progress. At 2:57 p.m. ET it said it had not found a recent change that correlated with the start. At 8:06 p.m. ET it said it was reverting "a recent network configuration change that correlates with the start of impact" to see whether that helps. The incident record still shows "Root Cause: N/A". On the security question, ABC News Australia reported that Home Affairs Minister Tony Burke said Microsoft told him a cyberattack was not believed to be the cause, and that Australia's cyber agreement with Microsoft "has in fact been activated today to make sure that we get appropriate prioritisation of Australian interests". This guide does not speculate beyond that. Every cause-related statement Microsoft has made so far, with its timestamp, and the lessons for IT teams are collected in Microsoft 365 outage: what we know about the root cause and what IT admins should learn.

Check in this order. The first two are Microsoft's own channels and show the incident IDs.

1. 
**Microsoft 365 admin center > Health > Service health.** Sign in at admin.microsoft.com with an admin account (Microsoft's documentation says Service Support admin and Helpdesk admin roles can view it). Under**Active issues Microsoft is working on** , open**EX1550543** and**MO1550821** ; each has a feed of every update and the next update time.
2. 
**status.cloud.microsoft.** Microsoft's public status page, meant for when you can't sign in to the admin center. During this incident it carried the same "Users may be unable to connect to multiple Microsoft 365 services" post.
3. 
**@MSFT365Status on X.** Microsoft's Learn page points to it "to see information on certain events". It posts shorter versions of the admin-center updates.
4. 
**Microsoft 365 Admin mobile app.** Shows Service health on your phone, with push notifications, which helps if your laptop's Outlook is the thing that's broken.
5. 
**Downdetector.** Useful for a quick "is it just me?", but it only counts user reports. It cannot tell you which tenants are affected, and it often shows spikes for services that merely depend on the one that is down.

If Service health shows no incident and only your organization is affected, the cause is more likely local: sign-in, network or a client problem. Microsoft's page has a **Report an issue** button for that case.

- 
**Try another way in, but don't keep switching.** Microsoft lists Outlook on the web, desktop Outlook and mobile mail apps (Exchange ActiveSync) as affected, so no app is guaranteed to work. Because impact varies by server, try`outlook.office.com` in a browser, then your phone, once each, and then wait.
- 
**Read what's already on your PC.** Classic Outlook for Windows uses Cached Exchange Mode by default for Microsoft 365, which keeps a copy of your mailbox on your computer. Old mail, contacts and calendar items stay readable even when Outlook can't connect. Microsoft notes that Cached Exchange Mode "is not available in new Outlook for Windows".
- 
**Compose and queue.** In classic Outlook you can write replies now; they wait in the Outbox and send when the connection comes back. Check the Outbox after recovery (see Outlook email stuck in Outbox).
- 
**Use Teams for chat and calls.** Microsoft says core Teams chat, channel messages and joining meetings were not affected; calendars, invites and scheduling were. Join meetings from the link in the chat or invite you already have, and try both the Teams desktop app and teams.microsoft.com if one fails.
- 
**Don't reset your password, reinstall Office or delete your Outlook profile.** None of that fixes a server-side outage, and deleting a profile throws away the local cached copy that is letting you read mail.
- 
**Use another channel for urgent mail.** Phone, SMS or a personal account for one-off urgent messages, and only for non-confidential content your company allows.
- 
**Beware of "Microsoft outage" phishing.** Big outages are a common lure: "re-verify your mailbox", "claim outage compensation", "your messages are on hold". Microsoft's updates for this incident list no action for users to take, so treat any email that asks you to sign in through a link "to restore" your mailbox as phishing and report it to IT. During this outage Microsoft also said Safe Links may fail to open wrapped links, so be extra careful with links from unknown senders. See how attackers steal Microsoft 365 sessions.

Send it through a channel that works (Teams chat, SMS, intranet or a phone tree), not only by email.


```
Subject: Microsoft 365 outage - Outlook and some Teams features affected
Microsoft is working on a global Microsoft 365 incident (EX1550543 and
MO1550821). Outlook may not connect, and Teams calendars and invites may fail.
Teams chat and joining meetings from existing links mostly work.
Please:
- Do not reset your password, reinstall Office or delete your Outlook profile.
- Classic Outlook still shows mail already on your PC.
- Ignore any email asking you to sign in "to restore your mailbox".
- For urgent matters, call or message the person directly.
We'll update you at <time> or when Microsoft reports recovery.
IT contact: <phone / chat>
```
Open EX1550543 and MO1550821 in Service health. To get every update by email, go to **Health > Service health > Customize > Email**, select **Send me email notifications about service health**, then choose incidents, advisories and services (up to two addresses per admin). For one incident only, open it and choose **Manage notifications for this issue**.

For monitoring tools, the Service Communications API in Microsoft Graph returns the same data with the `ServiceHealth.Read.All` permission (delegated or application):


```
GET https://graph.microsoft.com/v1.0/admin/serviceAnnouncement/issues/EX1550543
GET https://graph.microsoft.com/v1.0/admin/serviceAnnouncement/issues/EX1550543/incidentReport
```
The second request returns the post-incident review once Microsoft publishes one. In Microsoft Graph PowerShell, the documented cmdlet for the first call is `Get-MgServiceAnnouncementIssue -ServiceHealthIssueId EX1550543`.

Microsoft says "email delivery may be delayed". When a receiving system is unavailable, the sending server normally keeps the message and retries: RFC 5321, the SMTP standard, says retries should usually be at least 30 minutes apart and that "the give-up time generally needs to be at least 4-5 days". After recovery, run a message trace for important senders before asking anyone to resend (see message trace in Exchange Online). Microsoft lists message trace and reporting delays in this incident too, so give it time. Watch for non-delivery reports from senders whose servers gave up early. For mail that Microsoft 365 sends to your own servers or partners through a connector, Microsoft documents a different rule: undeliverable messages are queued and retried for 24 hours, then returned to the sender as an NDR (see Exchange Online connectors).

Write it down while it is happening: which users or sites were affected, from what time to what time (in UTC), which methods failed (OWA, Outlook desktop, mobile, Teams calendar), the exact error messages, and screenshots of Service health. You'll need this for an SLA claim and for your own incident review.

Microsoft's **Service Level Agreement for Microsoft Online Services** (November 2026 edition, downloaded from Microsoft's licensing site on 13 November 2026) sets these terms.

**Exchange Online.** Downtime is "any period of time when users are unable to send or receive email with Outlook Web Access". Uptime is measured per month in user-minutes: (User Minutes - Downtime) / User Minutes x 100.

| Monthly uptime | Service credit | 
|---|---|
| below 99.9% | 25% | 
| below 99% | 50% | 
| below 95% | 100% | 

Microsoft Teams uses the same tiers, but its downtime is when users "are unable to conduct instant messaging conversations or initiate online meetings". Microsoft said those core Teams functions were largely not affected here, so a Teams claim may not qualify.

**What the numbers mean (our arithmetic from the formula, not a Microsoft estimate).** November has 30 days, or 43,200 minutes per user. If every user in a tenant could not use Outlook on the web for 8 hours (480 minutes), uptime is (43,200 - 480) / 43,200 = 98.89%, in the 50% tier. If only a fifth of users were affected for those 8 hours, uptime is 99.78%, in the 25% tier. Microsoft decides the real figure.

**How to claim,** per the SLA's General Terms:

1. Submit the claim to Microsoft customer support with: a detailed description of the incident, the time and duration of downtime, affected resource names, the number and locations of affected users, and a description of the errors. Microsoft says missing information "will result in claims being rejected".
2. Deadline: for services other than Azure, Microsoft must receive the claim "by the end of the Applicable Period following the month in which the Incident occurred". For a monthly subscription and a November incident, that is **31 December 2026** .
3. Microsoft says it typically processes claims within 45 days and applies any credit to fees for that service. Credits are the "sole and exclusive remedy" and never cover lost revenue or other losses.

Two more lines matter. "Microsoft outage communications ... are not a confirmation of missed Service Levels or Service Credits eligibility." And services bought through Open, Open Value and Open Value Subscription agreements get credits as extra service time, not money. If you buy through a Microsoft partner (CSP), ask the partner how they file claims for you.

Microsoft's service description says that for incidents with "broad and noticeable impact across a large number of organizations", a **preliminary post-incident review (PIR)** is delivered through Service health "within 48 hours of incident resolution, followed by a final PIR within five business days". A PIR covers user impact, start and end times, a detailed timeline and the root cause analysis. When it is out, the incident's status changes to **Post-incident report published**. Download it from the incident in Service health (or through the `incidentReport` call above) and attach it to your claim and your internal review. For this incident, Microsoft wrote at 05:46 UTC on 13 November: "Once mitigated, we will provide full details of the root cause, defense-in-depth repair items, and more within a Post-Incident Report." As of 11:47 IST on 13 November, the incident was not resolved, so no PIR exists yet.

The full lessons, with monitoring scripts, message templates and a checklist, are in the follow-up guide: what IT admins should learn from this outage.

- 
**Keep offline access.** Classic Outlook in Cached Exchange Mode and locally synced files kept people working with what they already had. If your fleet is moving to the new Outlook, check what offline access it gives your users before the next outage.
- 
**Have a backup channel that doesn't run on Microsoft 365.** A phone tree, SMS group or second chat tool, with contact numbers stored outside the mailbox.
- 
**Monitor status automatically.** Service health email alerts, the Admin app on two admins' phones, and a script or tool that polls the Graph service health API.
- 
**Keep an outage runbook.** Who declares an incident, the user message template above, how to confirm it's Microsoft, and where to log impact for SLA claims.
- 
**Know your plan's SLA.** Note which services you bought separately; the SLA allows a separate claim per service bought outside a suite.

- Read the full update history of EX1550543 and MO1550821 in the University of Pennsylvania's public mirror of Microsoft's Service health feed, generated 23:50 ET on 12 November (04:50 UTC, 13 November); all times and quotes in the timeline come from it, converted to ET (UTC-5) and IST (UTC+5:30).
- Opened Microsoft's public status page data (status.cloud.microsoft) at 05:01 UTC on 13 November: the post was last updated 04:57 UTC, status "Service degradation", next update by 06:00 UTC. Re-checked after publishing: the 05:46 and 06:17 UTC updates (11:16 and 11:47 IST) kept the status at "Service degradation"; the 06:17 UTC update is the "as of 11:47 IST" status above.
- Downloaded the November 2026 edition of the Service Level Agreement for Microsoft Online Services and read the General Terms (claims, deadline, 45-day processing, credit limits) and the Exchange Online and Teams sections; the 25/50/100% tiers appear in both sections.
- Redid the uptime arithmetic: 30 x 24 x 60 = 43,200 minutes; (43,200 - 480) / 43,200 = 98.89%; (43,200 - 96) / 43,200 = 99.78%.
- Checked Microsoft Learn for the Service health path, email notification labels, the Admin app, the PIR timing (48 hours / five business days) and the Graph `serviceAnnouncement` endpoints; checked RFC 5321 section 4.5.4.1 for the retry wording and Microsoft Support for Cached Exchange Mode.
- Compared the Downdetector figures in The Tribune (138,058) and BleepingComputer (more than 129,500); they are user-report snapshots at different times, so both are given.
- Not verified: the cause, which Microsoft has not published, and whether a specific tenant meets the SLA threshold. Not tested on an affected tenant.

Last tested: 2026-11-13

- Microsoft 365 Service health, EX1550543 and MO1550821, as mirrored by the University of Pennsylvania: https://mailservices.isc.upenn.edu/computing/email/penno365/alerts/ms-incidents.html
- Microsoft 365 status page: https://status.cloud.microsoft
- Microsoft 365 Status on X, first post ("in the Americas"): https://x.com/MSFT365Status/status/2120938282204688468
- Microsoft 365 Status on X, "affect Microsoft 365 service connectivity for users in any region": https://x.com/MSFT365Status/status/2120941584220827979
- Microsoft 365 Status on X, "a number of servers being unexpectedly unavailable": https://x.com/MSFT365Status/status/2120962673911181437
- Microsoft, Service Level Agreement for Microsoft Online Services (November 2026): https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services
- Microsoft Learn, How to check Microsoft 365 service health: https://learn.microsoft.com/en-us/microsoft-365/enterprise/view-service-health
- Microsoft Learn, Service health and continuity (post-incident reviews): https://learn.microsoft.com/en-us/office365/servicedescriptions/office-365-platform-service-description/service-health-and-continuity
- Microsoft Learn, Service communications API in Microsoft Graph: https://learn.microsoft.com/en-us/graph/api/resources/service-communications-api-overview
- Microsoft Learn, Mail flow intelligence (24-hour connector retry): https://learn.microsoft.com/en-us/defender-office-365/connectors-mail-flow-intelligence
- Microsoft Learn, Get incident report: https://learn.microsoft.com/en-us/graph/api/servicehealthissue-incidentreport
- Microsoft Support, Turn on Cached Exchange Mode: https://support.microsoft.com/en-us/office/turn-on-cached-exchange-mode-7885af08-9a60-4ec3-850a-e221c1ed0c1c
- RFC 5321, Simple Mail Transfer Protocol, section 4.5.4.1: https://www.rfc-editor.org/rfc/rfc5321
- ABC News (Australia), Microsoft outage: https://www.abc.net.au/news/2026-11-13/microsoft-outlook-outage/107380958
- BleepingComputer, Massive Microsoft 365 outage disrupts Outlook, Teams, Copilot worldwide: https://www.bleepingcomputer.com/news/microsoft/massive-microsoft-365-outage-disrupts-outlook-teams-copilot-worldwide/
- The Tribune (ANI), Microsoft 365 outage hits Outlook, Teams, AI services: https://www.tribuneindia.com/news/india/microsoft-outage-disrupts-outlook-teams-ai-services-for-over-five-hours/
- Cyber Security News, Massive Microsoft 365 outage: https://cybersecuritynews.com/massive-microsoft-365-outage/
- ABC News (US), Microsoft Outlook outage stretches on for hours: https://abcnews.com/US/outlook-down-microsoft-outage-reported-mail/story?id=138088203

*This guide was researched and drafted with AI assistance and checked against the vendor documentation listed in its References. Read the maintained version and related guides on EpiFive.*

## Top comments (0)
