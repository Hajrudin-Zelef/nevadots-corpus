---
id: collect-261121-veille-ia/microsoft-actu/microsoft-bug-sparks-global-outage-across-365-services
title: "Microsoft Bug Sparks Global Outage Across 365 Services"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "traffic", "outage", "servers", "services", "teams", "across", "capacity"]
source: https://osintsights.com/microsoft-bug-sparks-global-outage-across-365-services
source_anchor: ""
source_lines: [1, 44]
section: "Microsoft_Actu"
---
"In less than two hours, the previously unknown defect was encountered across our fleet and rendered them unresponsive," explained Microsoft, describing how a months‑old software bug and a cascade of traffic shifted a routine system change into a near‑global outage.

## The July update and a latent bug in Microsoft 365 networking gateways

Microsoft says the root cause was a software bug introduced in July via an update to Microsoft 365 networking gateway servers. The update had been deployed for more than three months without apparent issues, but it contained a defect that could overwhelm servers under certain traffic conditions and cause them to stop responding.

Microsoft told customers that its load testing did not detect the defect because those tests used different hardware and traffic mixes than production. "Our normal load testing did not detect the latent issue because the traffic mix and SKUs used in our labs," the company explained, and noted the contention issue was worse on machines with more cores and newer hardware while lab testing tended to focus on weaker hardware with fewer cores.

## How a Teams failover and reduced capacity triggered the cascade

The bug remained latent until traffic conditions changed. Microsoft says that approximately 15 hours before the outage, Microsoft Teams automatically switched to networking infrastructure also used by Exchange Online, increasing traffic on those gateway servers. Separately, nearly 14% of the gateway's server capacity was already offline due to an unrelated issue.

While Microsoft wrote that the infrastructure still had sufficient capacity to handle the reduction in isolation, the combined effect of increased traffic and reduced capacity allowed the latent bug to be triggered. As individual servers stopped responding, systems redirected users to remaining servers, which then became overwhelmed as well — producing a rapid, worldwide cascading failure.

### This site is the portfolio.

OSINTSights runs on Cloudflare Workers, D1, R2, and Vectorize, with an AI pipeline on Hetzner ARM. Nubivance designed, built, and operates it. We do the same for clients.

See what we build
## Timeline: November 12–13 — nearly 24 hours of disruption

The incident began at 11:20 AM ET on Thursday, November 12, and continued until 11:10 AM ET the following day, when Microsoft confirmed full restoration of services. During the outage Microsoft reported that, "At its lowest, more than 80% of servers were unresponsive."

Microsoft also said Microsoft Teams experienced a separate outage earlier on November 12, but the company stated it was unclear whether that earlier incident was related to the later global outage affecting Microsoft 365 services.

## Service impact: Exchange Online, Outlook, Teams, Copilot and recovery challenges

The outage disrupted Microsoft 365 services worldwide, including Outlook, Exchange Online, Teams, and Copilot, leaving users unable to access email and causing significant delays in message delivery. Microsoft said other services such as Teams, OneDrive, and SharePoint could switch to alternative infrastructure and recover earlier; Exchange Online could not make the same switch and remained inaccessible for many customers.

Recovery was made harder because restarting affected servers caused them to be quickly overwhelmed by backed‑up traffic. After identifying the bug, Microsoft engineers developed a fix and deployed it across affected servers, allowing services to stabilize throughout Friday. Microsoft confirmed that active customer impact had ended by 11:10 AM ET on November 13.

## Immediate mitigations and the public response

In its preliminary post‑incident report shared in the Microsoft 365 Admin Center, Microsoft said it has increased capacity, improved monitoring, and added safeguards to prevent traffic redirection from triggering similar cascading failures. The company said its investigation is ongoing and that a final post‑incident report was expected later in the week.

On X, Charles Lamanna, Microsoft's executive vice president of Copilot, Agents, and Platform, apologized for the disruption: "We are deeply sorry for the impact to all our customers and recognize the impact that this has on their normal business & operations," he wrote.

## What this means for technologists, enterprises, and email security teams

- Technologists and security teams: will need to reassess load testing strategies — Microsoft reported its labs used different SKUs and less powerful hardware than production, a gap that let a latent defect persist for months.
- Enterprises and procurement leaders: should note that not all services could fail over identically — Microsoft stated Teams, OneDrive, and SharePoint could shift to alternate infrastructure while Exchange Online could not, leaving mailboxes inaccessible.
- Email operations and incident response teams: will watch whether Microsoft’s announced capacity increases, monitoring improvements, and new safeguards are sufficient once the company's final post‑incident report is published later in the week.

Microsoft's preliminary report lays out a clear chain: a July update harboring a traffic‑sensitive bug, a traffic shift from Teams, reduced effective capacity, and the breakdown of containment safeguards combined to create a nearly 24‑hour global outage. The company has fixed the immediate defect and taken short‑term steps to harden the service; customers and technical teams now await the final post‑incident report for fuller detail on long‑term fixes and testing changes.
