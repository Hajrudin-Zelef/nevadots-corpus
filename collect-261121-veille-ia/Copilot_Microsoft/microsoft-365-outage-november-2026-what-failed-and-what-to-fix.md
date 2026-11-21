---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-365-outage-november-2026-what-failed-and-what-to-fix
title: "Microsoft 365 outage, November 2026: what failed and what to fix"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-12"]
keywords: ["microsoft", "november", "admin", "centre", "confirmed", "openai", "anthropic", "status"]
source: https://ecorpit.com/microsoft-365-outage-november-2026-ai-agents/
source_anchor: ""
source_lines: [1, 165]
section: "Copilot_Microsoft"
---
**Summary.** Microsoft 365's Exchange Online-led outage ran from 16:20 UTC on 12 November 2026 to 16:10 UTC on 13 November by Microsoft's own final incident record: 23 hours 50 minutes, with mail backlogs and some tenants still recovering into 14 November. UK time equals UTC in November; in New York it began at 11:20 a.m. Microsoft calls it a technical fault, not a cyberattack and not linked to AI or agents. Its preliminary root cause: traffic shifts during routine network maintenance and upgrades exposed a latent bug in the systems that set up secure connections, so new connection requests queued until servers ran out of processing capacity. Incident MO1550821 covered features that depend on Exchange Online, including Microsoft 365 Copilot, Graph mail and calendar calls, Teams calendars, Purview DLP and labelling, Defender, Intune and Power Apps sign-in. Anthropic and OpenAI both logged failures in their Microsoft 365 connectors. Microsoft's post-incident report was not out on 15 November. What to do now: save the MO1550821 and EX1550543 records before Service health's 30-day history drops them, file any SLA credit claim by 31 December 2026, and give every agent that calls Microsoft Graph backoff that honours the Retry-After header, a circuit breaker, a degraded mode and fail-closed writes.

**About this article.** For IT directors, Microsoft 365 admins and teams running Copilot or their own Graph agents in the US and UK. On 15 November 2026 we checked Microsoft's statements against its admin centre posts (through a University of Pennsylvania page that republishes them), the @MSFT365Status posts on X, Microsoft Learn and Microsoft's October 2026 Online Services SLA, plus the Anthropic, OpenAI, Azure and Google Cloud status pages. Press figures are labelled as reported. This is general information, not legal or contractual advice, and no vendor paid for inclusion. Researched with AI assistance and reviewed by our editorial team.

A later section covers the October 2026 Azure ExpressRoute and Google Cloud Storage post-incident reports. For the Outlook zero-day patched the same week, see our November 2026 Patch Tuesday patch order.

## What Microsoft has confirmed, and what is only reported

Microsoft's admin centre was not available to us, so we read its incident posts through a University of Pennsylvania page that republishes them for its tenant every 15 minutes, plus the @MSFT365Status thread on X. The table shows the position on 15 November 2026.

| Claim | Status on 15 November 2026 | Source | 
|---|---|---|
| Mailbox access failures, first described in the Americas, tracked as EX1550543 | Confirmed by Microsoft | Admin centre and X | 
| Impact could reach users in any region | Confirmed by Microsoft | X | 
| Incident window: 16:20 UTC on 12 November to 16:10 UTC on 13 November | Confirmed by Microsoft | Admin centre, final post | 
| Email timeout window extended to 48 hours to keep delayed mail | Confirmed by Microsoft | Admin centre | 
| No evidence of malicious activity; not a cyber, AI or agent issue | Confirmed by Microsoft | Admin centre | 
| Traffic shifts during network maintenance exposed a latent bug in the secure-connection systems | Confirmed by Microsoft, preliminary | Admin centre, final post | 
| Preliminary post-incident report in two business days, final in five | Promised, not published | Admin centre | 
| Anthropic's Microsoft 365 connector and Claude for Outlook returned errors | Confirmed by Anthropic | Claude status page | 
| New OpenAI Microsoft connector connections could break and tools failed | Confirmed by OpenAI | OpenAI status page | 
| More than 97,000 Outlook reports on Downdetector at 2:07 p.m. ET | Reported | Reuters, via SecurityAffairs | 

The table draws on Microsoft's admin centre posts and X thread, the Anthropic and OpenAI status pages, and Reuters as carried by SecurityAffairs.

Microsoft's explanation shifted as it worked. At 19:43 UTC on 12 November it linked failed health checks to a possible recent change; 14 minutes later it said no recent change correlated with the start of impact. Overnight it began reverting a network configuration change that did correlate, and on 13 November it blamed an earlier network capacity upgrade issue that a later change exposed. Treat the cause as preliminary until the post-incident report (PIR) is out.

## Timeline in UTC and US Eastern time

UK time equals UTC in November, and US Eastern is five hours behind. Times are those of Microsoft's admin centre posts; the EX1550543 and MO1550821 posts differ by a few minutes.

| UTC (UK) | US Eastern | What happened | 
|---|---|---|
| 12 Nov, 14:45 to 15:53 | 9:45 to 10:53 a.m. | Separate Teams chat incident TM1550488 in the Americas, blamed on resource contention; resolved | 
| 12 Nov, 16:20 | 11:20 a.m. | Microsoft's stated start; EX1550543 opened for mailbox access errors | 
| 12 Nov, 18:27 | 1:27 p.m. | First MO1550821 post: elevated load across multiple regions | 
| 12 Nov, 19:07 to 19:18 | 2:07 to 2:18 p.m. | Deployments and configuration changes paused; load traced to servers dropping out, not more traffic | 
| 12 Nov, 19:43 to 20:56 | 2:43 to 3:56 p.m. | Servers failing automated health checks; an alternative health check returns a subset to service | 
| 12 Nov, 22:00 | 5:00 p.m. | A core process on affected servers found to be overwhelmed | 
| 13 Nov, 00:00 | 7:00 p.m., 12 Nov | Returned servers fail again once they take traffic; traffic types limited | 
| 13 Nov, 01:07 | 8:07 p.m., 12 Nov | Secure-connection process identified; rollback of a recent network configuration change begins | 
| 13 Nov, 07:24 | 2:24 a.m. | Patch to an upstream authentication and routing component improves server stability | 
| 13 Nov, 09:03 | 4:03 a.m. | Email timeout window extended to 48 hours | 
| 13 Nov, 11:01 to 12:18 | 6:01 to 7:18 a.m. | Recovery in Asia Pacific, then Europe; North America improving | 
| 13 Nov, 16:10 | 11:10 a.m. | Microsoft's stated end of impact | 
| 13 Nov, 17:57 | 12:57 p.m. | Apology, broad recovery, interim cause, not a cyber or AI issue | 
| 14 Nov, 04:15 | 11:15 p.m., 13 Nov | Final post: service restored, preliminary root cause, PIR timetable | 

The pattern matters more than any timestamp. Servers dropped out, the rest took their load and failed in turn, and returned servers failed again once traffic reached them. That reading is ours, but Microsoft's final post points the same way: connection requests queued faster than servers could complete them. Every client that retries hard against a service in that state adds to the queue that keeps it down.

## What failed, and why agents failed with it

Microsoft's MO1550821 post says features that interact with Exchange Online and email were affected, while parts of the same services that do not depend on Exchange kept working. For anyone building on Microsoft 365, that split is the most useful fact in the incident. The table summarises Microsoft's final list.

| Service | What Microsoft listed | What it meant for agents and automation | 
|---|---|---|
| Exchange Online | No mailbox access through Outlook on the web, desktop Outlook (MAPI), Exchange ActiveSync or REST; delivery delayed | Mail-triage agents and scripts hit timeouts; delayed mail arrived later, possibly out of order | 
| Microsoft Graph | Requests for Exchange data such as mail, calendar and contacts could time out, which also blocked Teams, channel, membership and app management and activity notifications | Scheduling, notification and inbox agents stalled while holding worker threads | 
| Power Apps and Power Automate | Intermittent sign-in failures in the Power Apps and Field Service mobile apps; flows, data refreshes and connectors that rely on Graph data failed intermittently | Low-code automations broke with no change on your side | 
| Microsoft 365 Copilot | Copilot Chat might not load or answer, and could not reach email, calendar or files | No work data to ground answers | 
| Teams | Calendar scheduling, invites, meeting edits and Live or Virtual event creation failed; calls, meetings, group chats and channel messages largely worked | Meeting and scheduling bots failed while chat bots kept running | 
| SharePoint Online | Exchange-dependent features and some external sharing failed; files stored in SharePoint stayed accessible | Retrieval pipelines that read SharePoint files mostly kept working | 
| Purview and Defender for Office 365 | DLP, auto-labelling, sensitivity labels and policy evaluation delayed or failed; earlier posts listed Safe Links and Safe Attachments failures | Controls that agent actions depend on were missing for hours | 
| Intune and Windows 365 | Device enrolment, check-ins and compliance policies intermittently affected; Customer Key Cloud PCs could not connect | Device compliance gates misbehaved | 

A separate Power Platform advisory, PP1550869, ran from 17:36 UTC on 12 November to 15:50 UTC on 13 November; Microsoft blamed an unavailable authentication dependency.

Agents that treated Microsoft 365 as one dependency switched off completely. Agents that tracked each API separately could keep reading SharePoint files and posting to Teams chats while mail and calendar were down. Anthropic says the mail, calendar and search tools in its Microsoft 365 connector, and Claude for Outlook, returned errors from 16:03 UTC on 12 November to 16:40 UTC on 13 November because of the Microsoft incident. OpenAI's incident ran from 18:29 UTC on 12 November, when new Microsoft connector connections could break, to 00:18 UTC on 14 November.

## The fix list for AI agents and Graph integrations

You cannot fix Microsoft from inside your tenant. You control how your code behaves while Microsoft is degraded and how it recovers.

1. **Honour the Retry-After header, then back off with jitter.** Microsoft's throttling guidance says to detect throttling from HTTP 429, wait the number of seconds in`Retry-After` , avoid immediate retries because every request counts against your limits, and use exponential backoff when no header comes back. Graph SDKs do this for single requests, but throttled requests inside a JSON batch are not retried automatically.

1. **Give each dependency a retry budget and a circuit breaker.** Split Exchange-backed calls (mail, calendar, contacts) from SharePoint and OneDrive calls. A breaker that opens after repeated failures keeps your agents out of the retry storm and keeps the healthy half of Microsoft 365 usable.

1. **Time out fast.** Graph calls for Exchange data timed out rather than failing quickly. Keep client timeouts well below the agent's step timeout.

1. **Build a degraded mode.** When mail or calendar data is unavailable, the agent should say so, queue the task and stop, not answer from a stale cache.

1. **Queue writes and make them idempotent.** Sends, event creation and posts go through a queue with a deduplication key and are released at a controlled rate after recovery.

1. **Fail closed when policy services are down.** Purview DLP and labelling were degraded for hours. Block send, share and delete actions when policy checks cannot run; keep reads available.

1. **Resync after recovery.** Handle Graph lifecycle notifications: a missed notification means some change notifications were not delivered, and Microsoft's answer is a full resync, for example with a delta query. Our guide to Microsoft 365 integration failure modes covers the subscription lifecycle.

1. **Read Microsoft's health from code.** The Graph service communications API lists incidents at`/admin/serviceAnnouncement/issues` and needs the`ServiceHealth.Read.All` permission. When an active incident covers Exchange Online, switch mail-dependent agents to degraded mode automatically.

1. **Keep model failover separate.** An AI gateway would not have brought Exchange back; it covers model outages. Our AI gateway comparison covers the options.

1. **Approve fallbacks in advance.** Decide which tools staff and agents may use when Microsoft 365 is down, so nobody pastes customer data into an unapproved chatbot.

A minimal Python sketch of controls 1 to 4 for a Graph mail call (thresholds are examples, not Microsoft guidance):

## The comms and admin fix list for IT teams

| Phase | Action | Detail | 
|---|---|---|
| Before | Turn on Service health email alerts, with one address outside Microsoft 365, and install the Microsoft 365 Admin mobile app | Each admin can name up to two addresses | 
| Before | Give the service desk read access to Service health | Service Support admin and Helpdesk admin roles can view it | 
| Before | Agree an out-of-band channel, a named sender and message templates | If Outlook and Teams are both down, an email or Teams post reaches nobody | 
| During | Check Service health, then status.cloud.microsoft, then @MSFT365Status | Microsoft points admins to its status page when they cannot sign in | 
| During | Follow the incident IDs, freeze your own changes, tell users not to reinstall Office or reset profiles | Microsoft paused its own deployments and configuration changes | 
| During | Warn staff about outage-themed phishing and fake password-reset messages | Attackers use the confusion of any large outage | 
| After | Save the incident text now | Service health shows 30 days of history | 
| After | Download the PIR when the status changes to Post-incident report published | Also available from Graph at /admin/serviceAnnouncement/issues/{id}/incidentReport | 
| After | Run message trace for delayed or bounced mail, rerun failed flows, check Purview jobs | Delayed mail kept retrying inside the 48-hour window | 

The table's details come from Microsoft Learn's Service health and incident report pages and the MO1550821 posts.

## Worked example: the SLA credit for a 300-seat tenant

Microsoft's Online Services SLA (the English edition dated 1 October 2026, the latest listed on 15 November) measures Exchange Online uptime per tenant per calendar month in user minutes. Downtime is any period when users cannot send or receive email with Outlook on the web (the SLA still calls it Outlook Web Access), and each incident counts as its length multiplied by the number of users it hit. Credits are 25% of that service's monthly fee below 99.9% uptime, 50% below 99% and 100% below 95%. November has 43,200 minutes (30 x 1,440), so the average user can lose about 43 minutes before the first tier.

| Average downtime per user in November | Uptime | Credit tier | 
|---|---|---|
| 43 minutes | 99.90% | None (just above the boundary) | 
| 4 hours | 99.44% | 25% | 
| 8 hours | 98.89% | 50% | 
| 24 hours | 96.67% | 50% | 
| 36 hours | 95.00% | 50% (100% needs uptime below 95%) | 

Take a 300-seat firm with offices in New York and Manchester. Its admins record that 200 US users lost mailbox access for 20 hours and 100 UK users for 6 hours. Total user minutes are 300 x 43,200 = 12,960,000. Downtime is (200 x 1,200) + (100 x 360) = 276,000 user minutes. Uptime is 1 - 276,000 / 12,960,000 = 97.87%, below 99%, so the claim is for 50% of the firm's Exchange Online fees for November. On an illustrative Exchange Online fee of $1,500 a month, the credit is $750. The same outage cost the firm 4,600 user-hours of mailbox access, which is why the credit is worth filing but is not compensation.

The SLA attaches conditions. Microsoft's outage communications are not confirmation of a missed service level. Credits cover only the affected service's fees and are your only remedy under the SLA. Claims need a description of the incident, its time and duration, affected resources, the number and locations of affected users and the errors seen; incomplete claims are rejected. For a November incident, Microsoft must receive the claim by 31 December 2026, the end of the following month. Cloud Solution Provider customers go through their partner, who must file within one month of the end of the billing month and supply the tenant ID, the outage ID and an email from the affected tenant's own domain. Microsoft adds that advisories, as opposed to incidents, are not usually eligible.

## What to fix first

| If you run | Fix first | Why | 
|---|---|---|
| Copilot only, no custom agents | Alerts, an out-of-band comms channel and a list of approved fallback tools | Nothing to code; the risk is people improvising | 
| Custom agents or scripts on Graph mail and calendar | Retry-After handling, a circuit breaker, short timeouts and a degraded mode | Timeouts and blind retries tied up workers and added load | 
| ChatGPT or Claude connectors to Microsoft 365 | A list of workflows that depend on them and a manual fallback | Both vendors logged connector failures during the incident | 
| Agents that send, share or delete | Fail-closed writes and an idempotent outbound queue | Purview DLP and labelling were degraded | 
| Power Apps or frontline mobile apps | Tell signed-in users not to sign out; use the web apps | Microsoft's own workaround during the incident | 
| A CSP-billed tenant | Ask your partner to file the credit claim now | Only the partner can file it | 

## US and UK notes

**United States.** Microsoft found no evidence of malicious activity, so treat this as an availability incident, not a security incident. Log it anyway: Purview and Defender policy actions that did not run may need follow-up, and auditors may ask what your controls did while degraded.

**United Kingdom.** The FCA gave firms in scope of its operational resilience rules until 31 March 2025 to show they can stay within impact tolerances for each important business service, with lessons-learned exercises and communication plans in place. Its new operational incident and third-party reporting requirements (PS26/2) come into force on 18 March 2027. If Microsoft 365 underpins an important business service, this incident belongs in your resilience evidence, and it is a good test case for your incident classification before the new reporting rules start. This is not legal advice.

## The October 2026 cloud post-incident reports

Two recent hyperscaler post-incident reports show a similar mechanism: a change overloads a shared control plane or serving layer, and recovery needs a rollback.

**Azure PIR 7Q30-010 (networking gateways and ExpressRoute).** Between 20:30 UTC on 30 September and 02:15 UTC on 1 October 2026, management operations for ExpressRoute Gateway, Application Gateway, Azure Firewall, Bastion, Virtual WAN and VPN Gateway failed intermittently in 33 regions, including UK South, Central US, South Central US and three West US regions. A paused gateway OS update resumed on many gateways at once, while an earlier performance change had slowed a shared regional control plane. Some ExpressRoute gateways were marked healthy too early and lost on-premises connectivity for about 30 minutes, until they applied their last known good configuration. Microsoft's repair items run to June 2027, and it says customers could not have avoided this impact.

**Google Cloud Storage, us-central1.** From 04:00 on 8 October to 09:46 on 9 October 2026 (US Pacific time), Cloud Storage uploads and writes saw elevated latency and errors: 12 hours 30 minutes on the first day, then a 5 hour 41 minute recurrence in us-central1-b, 18 hours 11 minutes in total. Google says a traffic-sorting configuration update to its load-balancing layer bypassed capacity-aware routing and overloaded a few storage backend nodes; the recurrence came from an incomplete rollback. Google will now validate load-balancing changes in smaller regions first.

| Incident | Trigger | What made it worse | Lesson for customers | 
|---|---|---|---|
| Microsoft 365, 12 to 13 Nov | Network maintenance traffic shifts exposed a latent connection bug (preliminary) | Returned servers failed again when traffic reached them | Backoff, circuit breakers and degraded modes for every Graph client | 
| Azure 7Q30-010, 30 Sep to 1 Oct | Paused gateway updates resumed at once | Instances marked healthy too early | Redundant ExpressRoute paths and Service Health alerts | 
| GCS us-central1, 8 to 9 Oct | Load-balancing configuration change | Capacity-aware routing bypassed; incomplete rollback | Multi-region or queued writes for critical uploads | 

For a power failure in Google Cloud's London region in the same month, see our analysis of the Google Cloud London outage.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Our cloud computing services team reviews Microsoft 365 and Azure dependencies and adds backoff, circuit breakers, degraded modes and Service health monitoring to Graph integrations and AI agents. Talk to us at /contact-us/.

Last updated: 15 November 2026.
