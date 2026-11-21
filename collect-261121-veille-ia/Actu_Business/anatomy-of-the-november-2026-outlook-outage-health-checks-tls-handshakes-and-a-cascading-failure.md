---
id: collect-261121-veille-ia/actu-business/anatomy-of-the-november-2026-outlook-outage-health-checks-tls-handshakes-and-a-cascading-failure
title: "Anatomy of the November 2026 Outlook Outage: Health Checks, TLS Handshakes and a Cascading Failure"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-14"]
keywords: ["microsoft", "outlook", "health", "network", "november", "checks", "outage", "online"]
source: https://dev.to/furqank729/anatomy-of-the-november-2026-outlook-outage-health-checks-tls-handshakes-and-a-cascading-failure-39n8
source_anchor: ""
source_lines: [1, 222]
section: "Actu_Business"
---
On Thursday, 12 November 2026, shortly before 1 p.m. Eastern, Exchange Online started failing for a large share of its users. Outlook inboxes wouldn't load, calendars went blank, and within hours Teams, SharePoint Online, Microsoft 365 Copilot and Defender XDR were degraded too. Microsoft said on Saturday 14 November that it had fixed the underlying issue and restored the last regions.

There's plenty of "Outlook is down" coverage already. This post is for engineers. It covers what Microsoft has actually said, how those statements fit a classic **cascading failure**, and what SRE and IT teams can take from it: health-check design, TLS handshake economics, retry behaviour, SMTP queueing, and how to watch Microsoft 365 service health through the API.

**Status of facts:** Microsoft has **not** published its Post-Incident Report (PIR) yet. Anything quoted or attributed to Microsoft comes from its status updates as reported by the press. Sections marked **Analysis** are my engineering reading, not Microsoft's. *Last updated: 14 November 2026.*


| **Window** | ~1 p.m. ET Thu 12 Nov → Sat 14 Nov 2026. Most tenants had close to 24 h of disruption | 
| **Incident IDs** | `EX1550543` (Exchange Online),`MO1550821` (Microsoft 365 dependent services) | 
| **Blast radius** | Al Jazeera counted 40,000+ problem reports from 70+ countries at one point on Friday | 
| **Stated trigger** | A fault from *an earlier network capacity upgrade* , exposed by a more recent network change | 
| **Stated mechanism** | Servers became unavailable / failed automated health checks → remaining servers overloaded → "a process that establishes secure connections" overwhelmed, high CPU | 
| **Not** | A cyberattack (per Microsoft), a traffic surge (per Microsoft), linked to Patch Tuesday (no advisory says so), or an AWS/Google outage | 
| **Mitigations** | Alternative health-check method, rollback of a network config change, staged server restarts, traffic rerouting, 48 h email retry window | 

**Thu 12 Nov, ~13:00.** Users in the Americas can't reach Exchange Online mailboxes. Outage trackers spike.

**Before 15:00.** `EX1550543` and `MO1550821` appear in the Microsoft 365 admin center as service degradations. Microsoft warns that Teams, SharePoint Online, Copilot and Defender XDR may be affected (SecurityWeek).

**Afternoon.** Microsoft attributes the problem to *elevated load across multiple regions*, and says explicitly that the load did not come from more traffic. A number of servers had become unexpectedly unavailable, leaving the rest to carry the work. Affected servers are failing automated health checks. Microsoft tests a workaround that **bypasses the component that processes health checks**, and investigates high CPU.

**~16:00.** Microsoft has "validated an alternative service health check method." Some servers come back, and the method is rolled out more widely.

**Evening.** Microsoft starts rolling back a recent **network configuration change** that coincided with the start of the outage. By ~21:00, restarts are "showing signs of improvement," but recovery isn't confirmed.

**Fri 13 Nov, overnight.** The disruption runs through the APAC and EMEA business day, when reports peak outside the US. Systems are taken offline and brought back in stages.

**Fri morning.** Microsoft: "no evidence of malicious activity," and "We are deeply sorry for the disruption." Cause refined to *an issue during an earlier network capacity upgrade*, surfaced by a newer network change.

**Fri ~10:30.** "Encouraging recovery progress." Mail queues approaching normal. APAC is among the first regions back.

**Sat 14 Nov.** Underlying issue addressed, remaining regions restored. PIR pending.

To read Microsoft's updates properly you need a rough model of the request path. Simplified:


```
Outlook / OWA / mobile / SMTP senders
        │  DNS: outlook.office365.com, <tenant>.mail.protection.outlook.com
        ▼
Microsoft edge network (anycast entry, TCP + TLS termination)
        ▼
Exchange Online front-end / protocol layer
  (auth, routing to the right mailbox server: MAPI over HTTP, EWS,
   REST/Graph, ActiveSync, IMAP/POP, SMTP)
        ▼
Mailbox servers (database copies replicated across servers)
        ▲
Load balancers decide who gets traffic, based on health probes
```
Three properties of this design matter for what happened:

1. 
**Every client connection starts with TLS.** Outlook (MAPI over HTTP), OWA, Graph, ActiveSync and modern SMTP are all TLS. A server that can't complete handshakes is, from the client's point of view, simply down.
2. 
**Health probes decide membership.** A server that fails its probes is removed from the pool and its traffic is spread across the survivors. In on-premises Exchange this is the documented*Managed Availability* system of probes, monitors and responders. Microsoft hasn't said what Exchange Online uses internally, so treat the comparison as an analogy.
3. 
**Exchange Online is a shared dependency.** Teams calendars and meeting scheduling, SharePoint notifications, Copilot's access to mail, and parts of Defender XDR read from or write to Exchange. That's why there were two incident IDs.

**Analysis.** Microsoft supplied the individual steps. How they connect is my reading, and the PIR may change it.


```
 (1) latent fault from earlier network capacity upgrade
                  │
 (2) newer network change exposes it
                  ▼
 (3) some servers unreachable / failing health probes ──┐
                  ▼                                     │
 (4) LB evicts them → same traffic, fewer servers       │
                  ▼                                     │
 (5) survivors: more TLS handshakes, CPU saturates      │
                  ▼                                     │
 (6) busy survivors miss probe deadlines ───────────────┘
```
Step 6 feeding back into step 3 is what turns a partial failure into a regional one. Each part is a reasonable safety mechanism on its own. Together they make a positive feedback loop.

Microsoft's wording, "a process that establishes secure connections was becoming overwhelmed," together with high CPU, points at the handshake path. Some background on why that path can fall over when traffic shifts suddenly:

- 
**A full handshake costs far more CPU than sending data over an established connection.** The key exchange (ECDHE) and the server's signature are asymmetric operations. Bulk data afterwards uses AES-GCM or ChaCha20, which is cheap, often hardware-accelerated.
- 
**Session resumption keeps the cost down in normal operation.** Clients that reconnect to a server they've seen recently can resume (TLS 1.3 PSK / session tickets) and skip most of the expensive work.
- 
**Evictions break resumption.** When a server leaves the pool, its clients are rehashed onto other servers. If resumption state isn't shared across the fleet (ticket keys aren't synchronised everywhere, or caches are local), every moved client does a full handshake on a server that's already busier than usual.
- 
**Clients retry, and retries mean new handshakes.** Outlook and mobile clients reconnect automatically. Millions of reconnects become millions of full handshakes, all landing on the shrinking healthy pool.

So "load rose because servers went missing" and "secure-connection setup was overwhelmed" are two descriptions of the same loop, not two separate problems. I'm inferring that. Microsoft hasn't published the detail.

Here's a deliberately simple simulation of the loop. Assume a fleet of `N` servers, constant offered load, and a per-server capacity above which probe responses time out:


```
def cascade(n=100, load=70.0, capacity=1.0, initially_failed=15, fail_open_at=None):
    healthy = n - initially_failed
    step = 0
    while healthy > 0:
        per_server = load / healthy
        print(f"step {step}: healthy={healthy:3d} load/server={per_server:.2f}")
        if per_server <= capacity:
            return "stable"
        # overloaded servers miss probe deadlines; assume a share proportional
        # to the overload gets evicted this round
        evicted = max(1, int(healthy * min(1.0, per_server - capacity)))
        if fail_open_at is not None and (healthy - evicted) / n < fail_open_at:
            return "fail-open: stop evicting, serve degraded but alive"
        healthy -= evicted
        step += 1
    return "total outage"
print(cascade())                     # 85 servers left for 70 units: stable
print(cascade(initially_failed=35))  # 65 left for 70 units: collapses
print(cascade(initially_failed=35, fail_open_at=0.5))
```
With 15 servers lost, the remaining 85 absorb the load. Lose 35 and each survivor runs at ~108%, starts missing probes, gets evicted, and pushes the rest over. The system goes from "slightly degraded" to "total outage" in a few rounds. Nothing about the offered load changed, which matches Microsoft's "not more traffic" statement.

The third call shows the usual defence, covered next.

Microsoft's first effective mitigation was an **alternative health-check method**, and a workaround that bypassed the health-check processing component. In the model above, that's the same as stopping evictions of servers that are *busy* but not *broken*. Common ways to build that in:

- 
**Panic / fail-open thresholds.** Envoy, for example, has a*healthy panic threshold* (default 50%). If fewer than that share of hosts are healthy, it ignores health status and balances across all hosts, on the reasoning that sending traffic to a slow host is better than dumping everything on a few healthy ones.
- 
**Split liveness from readiness.** "Process is alive and can do work" and "process wants more traffic right now" are different signals. Evicting on readiness alone causes flapping. Kubernetes keeps the two probes separate for this reason.
- 
**Keep probes cheap and on a protected path.** If the probe shares a thread pool, CPU or the TLS stack with user traffic, the probe fails precisely when the server is busy, which is exactly when you don't want it to.
- 
**Cap the eviction rate.** Never let automation remove more than X% of a pool per interval, whatever the probes say.

Retries turn overload into collapse. Each client retrying is rational, but their combined effect isn't. This is the same class of problem Microsoft described in **November 2024**, when a change caused "an influx of retry requests" and Outlook and Teams were disrupted for more than a day (ThousandEyes analysis).

Standard mitigations you should insist on in your own clients and services:

- 
**Exponential backoff with full jitter** :`sleep = random(0, min(cap, base * 2**attempt))` , so clients don't retry in lockstep.
- 
**Retry budgets** : e.g. retries may not exceed 10% of requests over a window. Past that, fail fast.
- 
**Server-side load shedding** : reject early and cheaply (before the TLS handshake if possible, e.g. connection limits at the edge) rather than accept work you can't finish.
- 
**Honour `Retry-After`** on 429/503 responses.

Outlook clients failing is one problem. Inbound SMTP from the rest of the internet is another, and it behaves differently.

- When Exchange Online can't accept a message it should respond with a **4xx temporary failure** (or the connection fails). Under RFC 5321 the sending server keeps the message queued and retries. It shouldn't bounce on a 4xx.
- 
**How long the sender keeps trying depends on the sender's software, not Microsoft.** Postfix's default`maximal_queue_lifetime` is 5 days. On-premises Exchange Server's default`MessageExpirationTimeout` is 2 days. Many SaaS senders give up much sooner. Mail from systems with short queue lifetimes is the most likely to have bounced.
- 
**Microsoft extended its retry window to 48 hours** for delayed mail, so messages already inside its pipeline kept trying instead of expiring.
- 
**Watch for duplicates and ordering.** When an SMTP session drops after the message is received but before the final`250 OK` reaches the sender, the sender retries and the recipient can get two copies. Message ordering across a big backlog isn't guaranteed either.

If you run applications that send transactional mail to M365 tenants, check your MTA's queue lifetime and alerting. A 24-hour outage is a good test of whether your system bounces silently.

Outage trackers tell you *something* is wrong. The admin center tells you *what*. For automation, use the Microsoft Graph service communications API.

**List active issues for your tenant** (requires the `ServiceHealth.Read.All` application permission):


```
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://graph.microsoft.com/v1.0/admin/serviceAnnouncement/issues?\$filter=isResolved eq false" \
  | jq '.value[] | {id, service, classification, status, title, startDateTime}'
```
**Fetch a specific incident and its update history:**


```
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://graph.microsoft.com/v1.0/admin/serviceAnnouncement/issues/EX1550543" \
  | jq '{id, status, impactDescription, posts: [.posts[] | {createdDateTime, text: .description.content}]}'
```
The PIR is normally attached to the issue once published (`/issues/{id}/incidentReport`). Wire this into Slack or PagerDuty so your helpdesk hears about it from Microsoft before users start calling.

**Measure from your own network.** A cheap synthetic check catches the "handshakes are slow" pattern before the status page admits it:


```
curl -o /dev/null -s -w \
  "dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ttfb=%{time_starttransfer} code=%{http_code}\n" \
  https://outlook.office365.com/owa/healthcheck.htm
```
Alert on the `tls - tcp` delta, not only on HTTP status. During a handshake-bound incident the TCP connect stays fast while TLS setup balloons or times out.

| Date | Duration | Cause (per Microsoft or cited analysis) | 
|---|---|---|
| **Jan 2023** | Hours | WAN router IP change during capacity work disrupted Azure and M365 (Microsoft PIR) | 
| **Jun 2023** | Intermittent, days | **Actual attack** : Layer 7 HTTP DDoS by Storm-1359 / Anonymous Sudan | 
| **Nov 2024** | >1 day | A change caused an influx of retry requests | 
| **Mar 2025** | Hours | "Problematic code change," reverted | 
| **Jul 2025** | 11+ h | Global Outlook disruption | 
| **Jan 2026** | ~10 h | NA infrastructure "not processing traffic as expected"; external mail largely stopped (The Register) | 
| **Nov 2026** | ~24 h | Latent network-upgrade fault + later network change → health-check eviction + TLS overload | 

Only one of these was an attacker. The rest started with a routine change that interacted badly with the machinery meant to keep the service up: load balancers, health probes, retries. If you run any large distributed system, your outage history probably looks similar.

1. 
**Latent faults outlive the change that caused them.** A capacity upgrade that "went fine" introduced a fault that only showed up after a later change. Track change dependencies and keep rollback possible for more than the change window.
2. 
**Health checks are a control system.** Give them fail-open thresholds, eviction rate limits, and isolation from the resources they measure.
3. 
**Know your handshake budget.** Measure full vs resumed handshakes per core, and check that resumption survives node churn (shared ticket keys, consistent hashing that minimises rehashing).
4. 
**Retries need a budget.** Backoff and jitter on the client side, load shedding and`Retry-After` on the server side.
5. 
**Shared dependencies set your blast radius.** List which of your business processes depend on Exchange Online, directly or through Teams and Copilot, and decide in advance what happens when it's gone.
6. 
**Have an out-of-band channel.** If your incident comms run over Teams and Outlook, you lose them exactly when you need them.

- What exactly was the latent fault from the earlier capacity upgrade, and why didn't validation catch it?
- Did health-check eviction lack a fail-open floor, and will that change?
- Was TLS session resumption lost during evictions, and how much did it add to handshake load?
- Why did recovery take close to 24 hours once the network change was rolled back on day one?
- How many messages bounced permanently rather than arriving late?

I'll update this post when Microsoft publishes the report.

**Sources:** SecurityWeek, CNBC, Al Jazeera, NPR, BetaNews, The Register (Jan 2026), Computerworld on Storm-1359, ThousandEyes on Nov 2024, Microsoft Graph serviceAnnouncement API, Envoy panic threshold, RFC 5321, and Microsoft 365 Status updates for EX1550543 and MO1550821.

*What would you add? If you run a large fleet, I'd like to hear how you bound health-check evictions. Drop it in the comments.*

## Top comments (0)
