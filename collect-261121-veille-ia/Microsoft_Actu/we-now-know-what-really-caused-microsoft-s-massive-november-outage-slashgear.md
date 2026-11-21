---
id: collect-261121-veille-ia/microsoft-actu/we-now-know-what-really-caused-microsoft-s-massive-november-outage-slashgear
title: "We Now Know What Really Caused Microsoft's Massive November Outage - SlashGear"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "outage", "november", "servers", "caused", "more", "problems", "know"]
source: https://www.slashgear.com/2320849/microsoft-november-2026-outage-cause-explained/
source_anchor: ""
source_lines: [1, 11]
section: "Microsoft_Actu"
---
# We Now Know What Really Caused Microsoft's Massive November Outage

On Thursday, November 12, several Microsoft 365 services came to a standstill. The cause: a widespread failure in Microsoft's networking gateway infrastructure. At the time, everybody theorized about the cause. A widespread cyberterror attack? Data centers drew a little too much power? Turns out it was little more than an undiscovered bug that snowballed out of control. Recently, Charles Lamanna, the Executive Vice President of Copilot, Agents, and Platform at Microsoft, explained the source of the Microsoft 365 outage in a post on X. According to their statement, the issue started with a "latent defect" that snuck its way into a prior Microsoft 365 networking gateway server fix. Under normal server loads, the bug was completely dormant, especially when tested with computers that had weaker components. But once the load increased and newer servers with more cores were used, the glitch flared to life.

Because the bug flew under the radar for several weeks, it was able to sneak onto platforms around the globe. However, according to Lamanna, Microsoft normally has safeguards to prevent such problems. Microsoft 365's gateway system is built on redundancy, so if the main network is disrupted, services can switch to an alternative one. These automatic switchovers are normal for Microsoft 365, but you'd never notice because of how the system is supposed to work. However, November 12 was different, as a routine switch of Teams traffic increased the load on servers already operating at reduced capacity because of an unrelated issue. These problems combined to create loads the servers couldn't handle, all thanks to a bug that went undetected. Thus, Microsoft 365 essentially DDoS-attacked itself, causing the service disruption.

## And the problems keep snowballing

Once again, we must stress that Microsoft 365 is built with redundancy upon redundancy. Even with networking issues, service shouldn't have been disrupted to the level we experienced. However, the underlying bug caused what Lamanna called a "cascading failure." The more things went wrong, the more opportunities the defect had to muck up services even more. According to Lamanna, Microsoft 365's infrastructure is split into shards that, while connected, are still technically separate. If one goes down, the others remain intact. However, during the outage, the normal barriers didn't hold, allowing the cascade failure to continue unabated. The bug and load problems created the outage, but the malfunctioning barriers transformed it into a widespread outage.

Even after technicians got services back up and running, the problems didn't stop, as all the traffic from people waiting for Microsoft 365 overwhelmed the servers, so they stopped working again. To make matters worse, the underlying cause had stuck around for months, so fixing the problem wasn't as easy as rolling back the code. In order to effectively patch the issue, Microsoft's teams needed to disconnect servers in batches, apply a hotfix, and restart them before bringing them back online. It took time, but it worked. While we applaud the company's technicians for their herculean efforts, we must also consider this event when comparing the pluses and minuses of Office 2024 vs. Microsoft 365. After all, you can't take a service down if there are no servers.
