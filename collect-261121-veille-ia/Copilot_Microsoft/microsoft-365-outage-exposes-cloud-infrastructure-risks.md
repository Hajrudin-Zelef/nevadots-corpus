---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-365-outage-exposes-cloud-infrastructure-risks
title: "Microsoft 365 outage exposes cloud infrastructure risks"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["microsoft", "outage", "services", "infrastructure", "network", "issue", "affected", "capacity"]
source: https://dig.watch/updates/microsoft-outage-network-upgrade-cloud-failures
source_anchor: ""
source_lines: [1, 32]
section: "Copilot_Microsoft"
---
# Microsoft 365 Outage: Network capacity upgrade sparks cascading Cloud failures


Microsoft said a technical issue introduced during a network‑capacity upgrade caused a 24‑hour global outage of Microsoft 365 services.

The global outage affecting Microsoft 365 services on 12–13 November 2026 disrupted access to email, collaboration tools and cloud-based applications used by businesses, public administrations and essential services worldwide. Outlook and Exchange Online were particularly affected, alongside functions of Teams, Copilot, SharePoint, OneDrive and other services relying on Microsoft’s email infrastructure.

The disruption spread from North America to 74 countries and territories within an hour, according to reports on the outage‑tracking site Downdetector, while governments, courts, healthcare providers, police departments and financial institutions reported operational disruption.

According to Microsoft, the incident began at approximately 16:20 UTC on Thursday, 12 November, when users started experiencing connectivity problems with Exchange Online. The disruption affected access through Outlook and other email clients, as well as the delivery of messages, which accumulated in processing queues. Other Microsoft 365 services were affected where their functions depended on Exchange Online, although some features remained operational. The consequences extended beyond ordinary workplace communications.

Microsoft gradually restored services on 13 November, with the company confirming that the remaining affected customers and regions had been recovered by early Saturday morning. The same day, Microsoft stated that “this was a technical issue not related to cyber or AI / agents. The incident was introduced by an issue during an earlier network capacity upgrade that surfaced following a recent network change. We have addressed the underlying issue and continue to restore service to remaining customers and regions.”

The company subsequently published a preliminary post-incident report explaining how several separate conditions combined to produce a cascading failure across its global networking infrastructure.  The preliminary report, dated 15 November, indicated that further corrective measures would be detailed in a final review. Microsoft’s preliminary analysis identified **three factors behind the failure**. 

First, a latent software defect introduced through a change deployed from July 2026 caused contention between processes within an internal Windows component responsible for establishing network connections. Under particular load conditions, this exhausted server CPU and memory resources, leaving machines unresponsive. The defect had remained undetected for more than three months because Microsoft’s testing environments did not reproduce the traffic patterns and hardware configurations that exposed it; the problem was more pronounced on newer machines with more processor cores.

Second, approximately fifteen hours before the outage, Teams traffic was automatically redirected from an alternative gateway to the primary network gateway also supporting Exchange Online. This routine failover increased the load and changed traffic patterns.

Third, nearly 14% of the gateway’s server capacity was already unavailable because of an unrelated issue. Although neither the increased load nor the capacity reduction was sufficient to cause an outage independently, their combination exposed the underlying defect. As servers became unresponsive, automated traffic-management systems redirected requests towards remaining operational endpoints, further increasing their workload. The resulting cascade rendered more than 80% of the affected gateway servers unresponsive at the incident’s lowest point.

## **Why does it matter?** 

The outage illustrates how a technical defect in a relatively narrow infrastructure component can have consequences across a broad ecosystem of digital services. It also challenges assumptions about redundancy and high availability: automated failover can amplify disruption when alternative systems share dependencies or lack sufficient isolation.

The incident also exposed limitations in Microsoft’s recovery mechanisms. Automated traffic redirection and server-restoration processes, normally designed to maintain availability, were unable to contain a failure of this scale. Servers brought back online were overwhelmed by waiting requests and frequently became unresponsive again.

Moreover, the usual rollback mechanisms could not rapidly reverse a defect introduced months earlier. Engineers eventually developed an update restoring the connection-handling code to its previously functioning state. They applied it progressively across groups of servers, restarting infrastructure and carefully redistributing traffic. Microsoft also increased capacity, extended the email delivery window to prevent queued messages from being rejected, strengthened monitoring and introduced safeguards intended to limit future cascading traffic failures.

For governments and organisations relying on cloud platforms to deliver public services, the incident reinforces the importance of independent contingency arrangements, tested recovery procedures, business continuity planning and clear accountability between providers and customers. More broadly, it raises questions about the systemic risks created by concentration in the cloud market.

As public administrations, healthcare systems, courts and emergency services increasingly depend on a small number of global providers, cloud resilience becomes not merely a matter of corporate service quality, but an issue of digital governance and critical-infrastructure oversight.
