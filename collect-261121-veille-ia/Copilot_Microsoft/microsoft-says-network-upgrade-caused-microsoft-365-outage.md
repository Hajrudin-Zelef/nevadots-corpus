---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-says-network-upgrade-caused-microsoft-365-outage
title: "Microsoft says network upgrade caused Microsoft 365 outage"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "network", "upgrade", "traffic", "servers", "outage", "capacity", "issue"]
source: https://betanews.com/article/microsoft-365-outage-network-upgrade/
source_anchor: ""
source_lines: [1, 39]
section: "Copilot_Microsoft"
---
Microsoft has traced Thursday’s global Microsoft 365 outage, which cut users off from Outlook, Copilot and Teams, to a fault left behind by a past upgrade that added capacity to its network. By Friday morning ET, the company reported that mail backlogs had shrunk to near-normal levels, though intermittent access trouble and late deliveries could persist.

Writing on X on Friday, the company explained that the flaw stayed hidden until a recent network change exposed it. Its statement described a “technical issue not related to cyber or AI / agents,” apologized to customers and partners, and pledged a complete post-incident review once the investigation wraps up.

We are deeply sorry for the disruption this incident has caused our customers and partners. We know the trust they place in our services and take that responsibility extremely seriously. We are seeing broad recovery and stability across our services. This was a technical issue not related to cyber or AI / agents. The incident was introduced by an issue during an earlier network capacity upgrade that surfaced following a recent network change. We have addressed the underlying issue and continue to restore service to remaining customers and regions. Given the scale of demand, we restored services gradually, validating stability at each stage. While this took longer, it helped ensure a stable restoration. We will provide a full post-incident report once our review is complete.

— Microsoft (@Microsoft) November 13, 2026

Trouble first surfaced in the Americas. Downdetector, an outage-tracking service, said Outlook complaints started rising at about 12:25 p.m. ET and that reports covered 74 nations and territories within roughly an hour. An initial public alert from Microsoft at 12:54 p.m. ET concerned Exchange Online mailbox access in the Americas, and a follow-up 13 minutes later warned the connectivity problem could reach customers anywhere in the world.

Admins tracking the wider incident in the Microsoft 365 admin center found it under MO1550821, logged as starting at 1:26 p.m. ET. Listed services facing possible connection failures included Microsoft Teams, Exchange Online, Microsoft Defender XDR, SharePoint Online and Microsoft 365 Copilot.

Early updates pinned the errors on elevated load in several regions. Less than an hour later, Microsoft revised that diagnosis, posting that the load “isn’t driven by an increase in traffic” and tracing it to servers that had unexpectedly dropped out, which left the remaining machines overstretched.

We determined the elevated load isn't driven by an increase in traffic, but by a number of servers being unexpectedly unavailable; straining the remaining infrastructure. We're working to safely return them to service. More info under EX1550543 and MO1550821 in the Microsoft 365 admin center, as well as https://t.co/pYdahqxDv3 if the admin center is not available to you.

— Microsoft 365 Status (@MSFT365Status) November 12, 2026

To contain the damage, engineers froze all deployments, spread connection traffic across Microsoft’s worldwide infrastructure, steered more of it away from the struggling systems and rebooted affected servers. That afternoon, Microsoft floated rolling back a handful of recent configuration updates as a possible way to speed up recovery.

Progress came slowly. Initial fixes failed to deliver the expected relief, Microsoft acknowledged shortly after 5 p.m. ET, once engineers found one core process buckling on the affected machines and draining capacity. Servers brought back online could also turn unresponsive again as soon as traffic reached them.

We’re opening all available pathways to restore service, as our actions taken thus far have not provided the anticipated relief. We found a core process on affected servers is becoming overwhelmed, causing capacity loss. To validate a potential recovery, we're isolating a portion of infrastructure, restarting affected components, and reintroducing traffic to confirm the services remain stable before applying the change more broadly. More info under EX1550543 and MO1550821 in the Microsoft 365 admin center, as well as https://t.co/pYdahqxDv3 if the admin center is not available to you.

— Microsoft 365 Status (@MSFT365Status) November 12, 2026

Around 6 p.m. ET, Microsoft reported limited recovery after putting part of the failed server fleet back into service and sending traffic along alternate routes. Overnight, it lengthened the email timeout window to 48 hours so delayed messages would not expire before delivery.

Updates through Friday morning described a phased recovery in which mailbox access improved and held mail began moving again. Late that morning, Microsoft said it had fixed the underlying problem and was seeing broad stability. Service for the last affected customers was still being restored at that point.

Hours before the main outage, a separate incident in Teams, logged as TM1550488, delayed or blocked message sends for people across North and South America between 9:45 and 10:53 a.m. ET. Resource contention inside Microsoft’s infrastructure caused that earlier disruption, according to the company, which has yet to link the two events or rule out a connection.

Disruption spread well beyond office inboxes. Government agencies, health services, courts, rail operators and AI companies relying on Microsoft integrations all reported problems. In the UK, National Rail advised passengers who could not open emailed tickets to retrieve them from the retailer’s website or app, or to ask staff at stations or on board for assistance.

Anderson Economic Group, a Michigan consultancy, estimated that lost productivity from the outage could reach $7 billion. No cost figure has come from Microsoft itself.

Marcus Pruitt covers Windows, desktop software and the open-source tools that help people get more from their computers. His work focuses on solving practical problems, from failed updates and confusing system settings to clean installations, bootable media and everyday performance issues. Marcus approaches technical advice from the reader’s point of view: explain what is happening, identify the safest fix and make every step easy to follow. He is especially interested in useful software that gives users greater control over their devices. When he is not troubleshooting Windows, he keeps an eye on the tools and changes shaping the wider PC ecosystem.

## Discussion
