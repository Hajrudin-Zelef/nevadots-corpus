---
id: collect-261121-veille-ia/copilot-microsoft/the-november-2026-outage-wave-explained-what-broke-at-microsoft-365-the-faa-aws-chase-and-united-and
title: "The November 2026 Outage Wave, Explained: What Broke at Microsoft 365, the FAA, AWS, Chase, and United, and What Didn’t Happen"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["microsoft", "chase", "november", "united", "five", "traffic", "outage", "cyberattack"]
source: https://dev.to/noahcontrolplane/the-november-2026-outage-wave-explained-what-broke-at-microsoft-365-the-faa-aws-chase-and-dop
source_anchor: ""
source_lines: [1, 108]
section: "Copilot_Microsoft"
---
In mid-November 2026, Microsoft 365, the FAA’s air traffic control network, AWS us-east-1, JPMorgan Chase, and United Airlines each had a major outage within five days, each with a different stated cause or none published yet, and none has been linked to a cyberattack or to another outage in the wave. Where a mechanism has been published, it shows how a fault in one widely used component can spread into a wide outage, a risk every team can check in its own architecture.

Disclosure: Control Plane publishes this guide.

Five widely reported outages hit five organizations between November 12 and November 16, 2026, and no organization or official reported evidence of a cyberattack.

- Five outages in five days: Microsoft 365, the FAA, AWS us-east-1, Chase, and United Airlines.
- Each has stated a different cause or has not published one yet.
- None was linked to a cyberattack, and none has been linked to the others.
- Durations ranged from about 90 minutes (United) to about 24 hours (Microsoft 365).
- The “90-day blackout starting November 17” claim was a hoax built on the routine Emergency Alert System test.
- The shared lesson is general: a fault in one widely used component can spread to many services.
- Before the next one, remove single shared dependencies, run in more than one region with traffic already flowing, test failover, and keep a break-glass sign-in path.
- Control Plane handles Day 2 operations and runs each workload in every location of its Global Virtual Cloud (GVC), Control Plane’s named set of locations, with all of them serving traffic, so when a workload runs in two or more locations one failed region does not stop it.

Five major services failed between November 12 and November 16, 2026, each for a different stated reason or one not yet published.

| Date | What failed | How long | Stated cause | Cyberattack? | Source | 
|---|---|---|---|---|---|
| Nov 12 to 13 | Microsoft 365: Exchange Online and Outlook, plus Teams, Microsoft 365 Copilot, SharePoint Online, Defender for Office 365, Intune, and other services that depend on Exchange Online, in North America, Europe, Asia Pacific, and Australia | About 24 hours, 11:20 AM ET on Nov 12 to 11:10 AM ET on Nov 13 (16:20 to 16:10 UTC) | A latent defect in a July 2026 update to Microsoft 365 network gateway servers, exposed by extra load after Teams failed over to the gateway used by Exchange Online and by almost 14% of gateway capacity already offline | “A technical issue not related to cyber or AI / agents” (Microsoft) | Microsoft preliminary post-incident report (MO1550821); BleepingComputer; The Register; Ars Technica | 
| Nov 13 | FAA nationwide ground stop, with network instability at 17 of the FAA’s 22 Air Route Traffic Control Centers | Stop announced 3:36 PM ET; traffic resumed about 8:15 PM ET | Excess connection traffic, about 1,000 connection attempts every four to five seconds, traced to two sites: a radio communications site in El Dorado, Arkansas, where a new router had been installed during a copper-to-fiber replacement, and a radar site in Amarillo, Texas. Root cause under investigation. | Homeland Security said there were no indications of a cyberattack | Transportation Secretary Sean Duffy and FAA Administrator Bryan Bedford; FAA; CBS; Homeland Security Secretary Markwayne Mullin | 
| Nov 15 | AWS us-east-1: connectivity and sign-in errors across 11 services in N. Virginia, including IAM Identity Center sign-in and a subset of Network Load Balancers in use1-az1 | 3 hours 16 minutes, 3:05 AM to 6:21 AM PST | AWS has not published a root cause | AWS’s updates do not mention a cyberattack | AWS | 
| Nov 16 | Chase (JPMorgan Chase) banking; customers at other banks saw delays on transactions with Chase | Not publicly specified | A software bug in an on-premises hardware system, per Chase | “Not cyber-related” (Chase) | Chase via AP | 
| Nov 16 | A United technology system that departures depended on | Departures delayed for about 90 minutes; no impact on flights in progress | Not stated; the pilots’ union said the weight-and-balance system was affected | “No indication” of a cyber issue (United) | United Airlines via CBS and AP | 

Microsoft’s preliminary post-incident report, published November 15, says three problems combined. Teams had failed over to the gateway used by Exchange Online about 15 hours before the outage, adding load, and almost 14% of gateway capacity was already offline. A latent defect from a July 2026 update then caused lock contention that exhausted CPU and memory, and more than 80% of gateway servers became unresponsive. Rerouted traffic hit other gateways, which failed the same way. Exchange Online could not fail over to other infrastructure for scale reasons, restarts did not hold, and rollback was not possible because the change was months old, so engineers deployed an update reverting the defective code path. Its final report is due November 20. Azure status showed no active events, and there was no confirmed impact on customer Azure workloads.

The FAA network was unstable for about 45 minutes before the stop while engineers searched for the cause, and roughly 3,500 flights were already airborne when departures stopped. Some frequencies and radar feeds stayed operational, so the outage was partial. L3Harris blocked the two source locations and the network stabilized. The FAA and L3Harris are still investigating the root cause, and HPE, which owns Juniper, says the device worked as configured. The FAA disabled the same functionality on more than 60 other Juniper routers and paused installing them. It was one of only two nationwide ground stops since September 11, 2001.

AWS reported that authentication was fully mitigated by 5:20 AM PST, and the subset of Network Load Balancers in one cell of use1-az1 was the last part to recover. AWS advised customers with affected Network Load Balancers to create a new load balancer with targets in multiple Availability Zones.

A Chase spokesman said the cause was a software bug in an on-premises hardware system and that it “is not cyber-related.” Customers at other banks saw delays on transactions with Chase, including Zelle payments; Zelle’s operator said its network was fully operational. United said a technology issue delayed departing flights for about 90 minutes, with no impact on flights in progress, and it has not stated a cause. FlightAware showed 715 delayed United flights that afternoon, though CBS reported it is unclear how many the outage caused.

No organization or official reported evidence of a cyberattack, and the popular theories about the wave do not match the published evidence.

| Claim | What the evidence shows | Verdict | 
|---|---|---|
| “It was a coordinated attack” | Microsoft, Homeland Security, Chase, and United each ruled out a cyber cause or said there was no indication of one, and AWS’s updates do not mention a cyberattack. The stated causes differ: a latent software defect in network gateway servers, excess connection traffic from two network sites, and a software bug in on-premises hardware. AWS and United have not published causes. | Not supported | 
| “A 90-day blackout starting November 17” | The November 17 event was the routine nationwide Emergency Alert System test at 2:20 PM ET, which ran about one minute on TV and radio. | Hoax | 
| “A solar storm caused it” | NOAA space weather data showed quiet conditions on the outage days. | Not supported | 
| “The outages are connected” | Each incident involved a different organization, different technology, and a different stated cause or none published yet. No published evidence links any two of them. | Not supported | 
| “Cyber activity is up” | Ransomware against hospitals and universities was elevated in the same period, and CISA added more known-exploited vulnerabilities than usual in September and October. None of it was linked to these outages. | True, but separate | 

On November 18, DHS Secretary Markwayne Mullin, Director of National Intelligence Jay Clayton, and FBI Deputy Director Christopher Raia told the House Homeland Security Committee the outages were not malicious, according to Nextgov. Elevated ransomware activity is a real and separate risk. Treating it as the cause of these outages would point teams at the wrong problem.

Where a mechanism has been published, a fault in one component spread to many services that depended on it.

| Incident | What failed | Question to ask of your own system | 
|---|---|---|
| Microsoft 365 | Network gateway servers with a latent defect, already short of capacity, took on rerouted Teams traffic (Microsoft preliminary report) | If several servers fail at once and traffic reroutes, can the remaining capacity absorb it without spreading the fault? | 
| FAA ground stop | Excess connection traffic from two sites caused instability at 17 of 22 air traffic control centers (root cause under investigation) | Can one site send unlimited connection traffic to every other site? | 
| AWS us-east-1 | Connectivity and sign-in errors in one AWS Region (root cause not published) | Do your load balancer targets and sign-in path span more than one Availability Zone and Region? | 
| Chase | One on-premises hardware system, per Chase (Chase has not described what depends on it) | How many customer channels depend on one platform? | 
| United Airlines | A technology system departures depended on (cause not stated) | Can critical workflows continue at reduced throughput if a key system is unavailable? | 

The mechanism repeats across industries. A shared component turns a local fault into a wide one. Resilient designs split load and control across independent failure domains and keep traffic flowing to more than one location, so losing one domain reduces capacity without stopping service.

None of this assigns fault. Each of these systems is large, and each organization disclosed what it knew. The pattern is useful because it is general, and the same review applies to any team’s own architecture.

The November wave stands out for how many incidents landed in one week, and its durations sit within the range of past major incidents.

The cloud outage tracker catalogs 63 major AWS, Azure, Google Cloud, and Cloudflare outages since January 2020 (median 5 hours 57 minutes; 32% caused by a configuration or deployment change). By that measure, the AWS us-east-1 event at 3 hours 16 minutes ran shorter than the median. The Microsoft 365 outage, a SaaS incident the tracker does not count, lasted about 24 hours.

In the tracker, 32% of major outages trace to a configuration or deployment change. Of the November incidents, only Microsoft has published a mechanism that fits that pattern, and AWS has not published a root cause.

Outages like these will recur, so the practical goal is to limit how far any one failure can spread.

- Remove single shared dependencies. List every component that many services rely on, such as a load balancer, an identity provider, a database, or a network segment. Give each path users depend on at least two independent ways to succeed.
- Run in more than one region with traffic already flowing. A standby region that has never served production traffic has an untested failover path. Active-active deployments across regions or providers keep every location exercised. How companies stay online during AWS us-east-1 outages walks through that pattern.
- Spread across Availability Zones inside each region. AWS’s own advice after November 15 was to create load balancers with targets in multiple Availability Zones.
- Test failover regularly. Run drills that remove a region, an identity provider, or a key dependency, and measure recovery time against your targets.
- Keep a break-glass sign-in path. Maintain an administrative login that does not depend on the same region or sign-in service as your primary workloads.
- Separate status communications from the systems they report on. Host status pages and alerting on independent infrastructure so the team can still communicate during an outage.
- Size resilience to the cost of downtime. Regulated industries such as financial services face operational and compliance consequences from long outages.

Staged rollouts and isolated testing for new hardware and configuration also limit blast radius by keeping a misbehaving change inside one segment. Designing systems that survive the inevitable covers active-active architecture and failure domains in more depth.

Control Plane handles Day 2 operations for you, including autoscaling, built-in observability, and security. A Global Virtual Cloud (GVC) is Control Plane’s named set of locations across AWS, GCP, Azure, and Oracle Cloud Infrastructure regions, plus Kubernetes clusters you already run and your own servers. Every location in a GVC serves traffic, so when a workload runs in two or more locations and one region fails, users are routed to the healthy locations.

On October 20, 2025, 20% of Control Plane customers had AWS us-east-1 in their GVC. Their workloads were “active and running in healthy locations within 10 seconds,” with no customer downtime, according to Control Plane’s internal incident telemetry.

Control Plane is PCI DSS Level 1, SOC 2 Type II, HIPAA, and GDPR compliant. Control Plane runs the workloads you deploy on it; it does not change how third-party SaaS, identity providers, or other organizations’ networks behave.

Short answers on the November 2026 outages, their causes, and the hoax.

The longest 2026 entries in the cloud outage tracker are AWS’s me-central-1 (UAE) and me-south-1 (Bahrain) Regions, out since early March after physical damage and still not fully restored. Next come Azure West US 2 on May 29 to 30 (22 hours 6 minutes), a single Availability Zone in AWS us-east-1 on May 7 to 8 (21 hours 30 minutes to majority recovery), and Azure West US on February 7 to 8 (20 hours 26 minutes). In November, AWS us-east-1 had connectivity and sign-in errors for 3 hours 16 minutes on November 15, and the Microsoft 365 SaaS outage, which the tracker does not count, lasted about 24 hours. The tracker lists 63 major outages since January 2020, with a median duration of 5 hours 57 minutes.

No evidence of a cyberattack has been reported for any of the five outages. Microsoft called its outage “a technical issue not related to cyber or AI / agents.” Homeland Security said there were no indications of a cyberattack at the FAA. Chase said its outage “is not cyber-related,” United said there was “no indication” of a cyber issue, and AWS’s updates do not mention a cyberattack. On November 18, the DHS Secretary, the Director of National Intelligence, and the FBI Deputy Director told the House Homeland Security Committee the outages were not malicious.

Microsoft’s preliminary post-incident report says three things combined: a latent defect in a July 2026 update to its Microsoft 365 network gateway servers, extra load after Teams traffic shifted onto the gateway used by Exchange Online, and almost 14% of gateway capacity already offline. The defect exhausted server resources, rerouted traffic hit other gateways that failed the same way, and more than 80% of gateway servers became unresponsive. Exchange Online could not fail over to other infrastructure, so email was down longest. Microsoft’s final report is due November 20.

The FAA stopped departures because its air traffic control network became unstable at 17 of its 22 Air Route Traffic Control Centers. The FAA traced excess connection traffic, about 1,000 connection attempts every four to five seconds, to two sites: a radio communications site in El Dorado, Arkansas, where a new router had been installed during a copper-to-fiber replacement, and a radar site in Amarillo, Texas. The network was unstable for about 45 minutes before the FAA announced the stop at 3:36 PM ET, and traffic resumed about 8:15 PM ET. The root cause is still under investigation.

AWS reported connectivity and sign-in errors affecting 11 services in N. Virginia from 3:05 AM to 6:21 AM PST. Affected services included IAM Identity Center sign-in and a subset of Network Load Balancers in use1-az1, which were the last to recover. Authentication was fully mitigated by 5:20 AM PST. AWS has not published a root cause.

No. The “90-day blackout starting November 17” claim was built on the routine nationwide Emergency Alert System test, which ran at 2:20 PM ET on November 17 for about one minute on TV and radio.

Companies that stay online usually run workloads in more than one region, or on more than one cloud provider, with live traffic flowing to every location. When one region fails, traffic shifts to locations that are already serving users. They also keep break-glass sign-in paths, test failover on a schedule, and host status pages and alerting apart from the systems they monitor.

There was no confirmed impact on customer Azure workloads. Azure status showed no active events during the Microsoft 365 outage, which centered on the network gateway servers behind Exchange Online and the services that depend on it.

No published evidence links any two of the five outages. Each involved a different organization, different technology, and a different stated cause or none published yet. The general lesson they share is that a fault in one widely used component can spread to many services.

Talk to the Control Plane team about Day 2 operations and running workloads across regions, or sign up free to start.

*Originally published on the Control Plane blog.*

## Top comments (0)
