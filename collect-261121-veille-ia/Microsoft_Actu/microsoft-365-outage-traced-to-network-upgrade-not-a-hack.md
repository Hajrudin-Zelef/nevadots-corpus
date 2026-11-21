---
id: collect-261121-veille-ia/microsoft-actu/microsoft-365-outage-traced-to-network-upgrade-not-a-hack
title: "Microsoft 365 Outage Traced to Network Upgrade, Not a Hack"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["microsoft", "said", "network", "email", "outage", "upgrade", "reported", "traffic"]
source: https://shiftdelete.net/en/microsoft-365-outage
source_anchor: ""
source_lines: [1, 68]
section: "Microsoft_Actu"
---
# Microsoft 365 Outage Traced to Network Upgrade as Service Returns

Microsoft says an issue from a network capacity upgrade, not a cyberattack, knocked Outlook and Teams offline for about a day. Delayed email gets 48 hours.

**Update, Nov. 13, 2026, 11:15 a.m. PT:** Microsoft says the outage was caused by an issue from an earlier network capacity upgrade, not a cyberattack, and reports broad recovery. This article has been updated with Microsoft's explanation, the impact on hospitals and governments, and a full timeline.

Microsoft says the Microsoft 365 outage that began on Nov. 12 was caused by an issue introduced during an earlier network capacity upgrade that surfaced after a recent network change, not by a cyberattack or AI agents. The company says it now sees broad recovery after roughly a full day of disruption, with service gradually restored while stability was validated at each stage.

## What caused the Microsoft 365 outage?

In updates posted on the official Microsoft 365 Status account on X, @MSFT365Status, Microsoft said it first began investigating Exchange Online mailbox access issues in the Americas at 9:54 a.m. PT on Nov. 12 under incident EX1550543, and then said at 10:07 a.m. that users in any region may be affected. It later added a second incident ID, MO1550821. Exchange Online is Microsoft's cloud email and calendar service, and Engadget reported that Outlook and Teams were among the impacted services.

Microsoft later said the elevated load was not caused by traffic growth. At 11:31 a.m. PT, the company wrote that the load "isn't driven by an increase in traffic, but by a number of servers being unexpectedly unavailable".

At 2:09 p.m. PT, Microsoft said its actions so far had not brought the expected relief and that it had found a core process on affected servers becoming overwhelmed, which was causing capacity loss. By 4:06 p.m. PT, Microsoft warned that servers returned to service could become unresponsive again once they received traffic, so it was limiting which traffic types reached the affected infrastructure and rebuilding a subset of servers using a previous configuration.

At 7:13 p.m. PT, Microsoft said the changes it had applied to improve serviceability were "not taking as expected" and that it was identifying alternate workstreams amid "sustained traffic management imbalances". By 8:36 p.m. PT, it reported partial recovery in regions where traffic protocol adjustments, redistribution and strategic traffic blocking had been applied.

By 1:24 a.m. PT on Nov. 13, Microsoft reported early stabilization and said it had extended the email timeout to 48 hours to preserve delayed mail. At 6:17 a.m. PT, the company said email queues were "significantly reduced and approaching normal levels". In its latest update at 10:05 a.m. PT, Microsoft said it had addressed the underlying issue and was continuing to restore service to remaining customers and regions.

We are deeply sorry for the disruption this incident has caused our customers and partners. We know the trust they place in our services and take that responsibility extremely seriously. We are seeing broad recovery and stability across our services.

— Microsoft 365 Status (@MSFT365Status) November 13, 2026

## Who was hit?

Microsoft said users in any region may have been affected. The BBC reported that municipal governments including Austin, Texas, Jacksonville, Florida, and Baltimore, Maryland relied on Microsoft for email, while essential and emergency services were not affected. The BBC also reported that Unity Health Toronto declared a "Code Grey" because patients could not reach doctors by email, and that Halton Healthcare and the Hospital for Sick Children in Toronto also reported email and phone contact problems.

The Boston Globe reported that UMass Memorial Health lost email access for about 20 hours and restored service just before 9 a.m. ET on Friday. The Boston Globe also said Downdetector reports for Outlook and Microsoft 365 spiked at more than 150,000 around 1:15 p.m. ET on Thursday. Euronews reported that Portugal's Ministry of Health Shared Services, or SPMS, confirmed entities under the ministry were affected, and that Copilot was among the affected services. Gizmodo and Bloomberg reported that some UK NHS hospitals saw email disruptions, and people familiar told Bloomberg some procedures elsewhere had to be rescheduled.

This is not Microsoft's first productivity outage: an earlier Microsoft outage took Outlook, Teams and Office 365 down in the US.

The company has also dealt with Outlook email issues that needed a fix. Microsoft has not linked either to this incident.

## Is the Microsoft 365 outage over?

Microsoft's latest update says the company has addressed the underlying issue and is continuing to restore service to remaining customers and regions. It also says there is no indication the event was caused by malicious or cyber activity, and that the incident was introduced by an issue during an earlier network capacity upgrade that surfaced after a recent network change.

That does not mean every user will see an immediate return to normal. Microsoft said some users may still see intermittent access issues or delayed mail during recovery, even as broad improvements were reported across North America, Europe and Asia Pacific.

## What should admins check now?

- Admins: review incidents MO1550821 and EX1550543 in the Microsoft 365 admin center.
- Check Microsoft's public status page for any remaining regional or service-specific notices.
- Keep the delayed mail retry window in mind: Microsoft extended email timeout to 48 hours.
- Watch for the full post-incident report, the root-cause write-up Microsoft publishes after a major incident, once its review is complete.
- Review fallback communications for email, calendar and Teams if your organization still relies on them for critical operations.

Separately, Windows 11 admins are still waiting on Microsoft's KB5132061 install failure fix, which is unrelated to the Microsoft 365 outage.

## Timeline of key updates

| Time PT | Microsoft update | 
|---|---|
| Nov. 12, 9:54 a.m. | Microsoft began investigating Exchange Online mailbox access issues in the Americas under EX1550543. | 
| Nov. 12, 10:07 a.m. | Microsoft said users in any region may be affected under MO1550821. | 
| Nov. 12, 11:31 a.m. | Microsoft said the elevated load was not driven by traffic increases, but by unexpectedly unavailable servers. | 
| Nov. 12, 2:09 p.m. | Microsoft said earlier actions had not provided the expected relief and a core process was becoming overwhelmed. | 
| Nov. 12, 4:06 p.m. | Microsoft said returned servers could become unresponsive again and it was limiting traffic types. | 
| Nov. 12, 7:13 p.m. | Microsoft said changes were not taking as expected and it was identifying alternate workstreams. | 
| Nov. 12, 8:36 p.m. | Microsoft reported partial recovery in some regions. | 
| Nov. 13, 1:24 a.m. | Microsoft extended email timeout to 48 hours to preserve delayed mail. | 
| Nov. 13, 5:32 a.m. | Microsoft reported broader improvements across North America, Europe and Asia Pacific. | 
| Nov. 13, 6:17 a.m. | Microsoft said email queues were significantly reduced and approaching normal levels. | 
| Nov. 13, 7:39 a.m. | Microsoft said there was no indication the incident was malicious or cyber related. | 
| Nov. 13, 10:05 a.m. | Microsoft said the issue was technical, not related to cyber or AI agents, and that service was still being restored. | 

**Our take:** The most important detail here is that Microsoft now says the root cause was a Microsoft network change, not a traffic surge or an attack. Earlier updates, including servers going unresponsive again after restart and changes that were "not taking as expected" help explain why recovery stretched to about a day. For hospitals, governments and other organizations dependent on one cloud suite for email, Teams and calendars, this was a reminder of how much operational risk sits in a single platform. The right time to judge Microsoft's handling is after the post-incident report is published.
