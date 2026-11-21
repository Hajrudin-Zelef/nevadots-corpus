---
id: collect-261121-veille-ia/securite/northflank-s-nov-2-update-points-at-control-plane-secrets-rotate-your-ai-keys-first
title: "Northflank's Nov 2 Update Points at Control-Plane Secrets. Rotate Your AI Keys First"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["northflank", "keys", "update", "secrets", "page", "rotate", "first", "november"]
source: https://dev.to/gabrielanhaia/northflanks-nov-2-update-points-at-control-plane-secrets-rotate-your-ai-keys-first-klg
source_anchor: ""
source_lines: [1, 94]
section: "Securite"
---
- 
**Book:** Ship It — the pragmatic startup tech stack
- 
**Also by me:***Thinking in Go* (2-book series) — Complete Guide to Go Programming + Hexagonal Architecture in Go
- 
**My project:** Hermes IDE | GitHub — an IDE for developers who ship with Claude Code and other AI coding tools
- 
**Me:** xgabriel.com | GitHub

At 11:09 UTC on November 2, Northflank moved its security incident to "Monitoring" and posted its fullest account yet. According to its status page, "Running workloads were unaffected." The evidence points to secrets held in the control plane, not to the containers. Those include your environment variables: Northflank's changelog says they are encrypted at rest and decrypted before being injected into your runtime.

That narrows where the attacker was, but your to-do list stays the same: the update still tells every customer to rotate all integration and project secrets, and it puts AI provider keys at the top of that list.

The incident started on October 30. The status page timeline has a threat alert from Wiz at 11:43 UTC, a first network policy patch at 12:02, and more patches at 12:50 and 13:05. At 13:58 UTC the advice became blunt: "Assume all environment variables have been decrypted. Disable/rotate all keys."

By 15:12 UTC the page said "Multiple users are confirming that their AI keys have been used." At 23:38 UTC Northflank wrote that users had reported unauthorised use of AI API keys supplied to the platform, and told them to review provider usage and billing logs (status page).

The November 2 update adds five things, all from the same page:

- Northflank believes the attack vector was contained within an hour of the first alert, and sees no sign the attacker stayed in its environment.
- Customers on forward-deployed or dedicated control planes are unaffected.
- CVE-2026-98432 has been patched across the fleet, including connected BYOC clusters. The update does not say this bug was the way in.
- Northflank revoked access tokens, sessions, VCS integrations and cross-account IAM roles where it could, so you have to sign in again and reconnect integrations.
- Independent forensic auditors are reviewing the incident, and a root cause analysis will follow once findings are confirmed.

For context, CVE-2026-98432 is a use-after-free in the Linux kernel's IPv4 TCP handling, with a demonstrated remote code execution against servers that have Multipath TCP listeners, though the advisory says the bug itself isn't specific to MPTCP. I covered the Go side of it in Go 1.24+ Servers Ask Linux for MPTCP.

Northflank still hasn't said how the attacker got in or how many customers were affected, as both teiss and Cyber Security News noted in their November 2 reports.


*Northflank's read on November 2: the containers kept running, the stored secrets are what to worry about.*

Northflank's 23:38 update pointed customers at Wiz's report Black October for AI Developer Platforms, published the same day. Wiz counts at least nine AI developer platforms that disclosed incidents during October: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank, the last of them on October 30.

Wiz found no single shared vulnerability or dependency across them (Wiz). The vendors' early explanations, as Wiz summarizes them, differ: Railway's preliminary report describes potential privilege escalation in its build infrastructure, with forensics still ongoing; Warp has suggested a third-party vendor dependency; and OpenCode has pointed to a legacy system. What the incidents share, per Wiz, is the target: stored environment variables, especially the ones that grant access to AI models.

The reason is money. Wiz describes a market for stolen AI access, including two resale sites offering discounted frontier-model access that touched the stolen keys (Wiz). A leaked database password needs work to monetize. A leaked model key is billable the minute someone finds it.

Wiz tracked two stolen enterprise credentials on Amazon Bedrock. By Wiz's account, both cases began the same way. Within 30 minutes of the reported credential access came one test call from a residential IP that asked for Claude Fable 5.1, failed, and fell back to Opus 5.5, which worked (Wiz).

Then came volume. In Wiz's Case A, roughly 800,000 Bedrock calls ran over thirty hours, about 90% of them successful, with Opus models making up three quarters of the successful calls. In Case B, roughly 1.7 million calls ran over thirteen hours, over a million succeeded, and nearly all the failures were throttling. Wiz estimates the two cases "likely ran up over $100k in usage" at list price (Wiz).

One detail matters for how you scope keys. Case A's policy allowed `global.*` inference profiles. Case B's key was restricted to `us.*` profiles and still served over a million calls (Wiz), so the region restriction limited where the stolen key worked and put no limit on how much it spent.


*In Wiz's cases, the first test call came less than half an hour after the reported credential access.*

If you run anything on Northflank's shared control plane, the order comes from Northflank itself (status page): AI provider keys first, then cloud credentials, payment keys, private keys and database credentials, across workload secrets, secret groups and global secrets. Revoke each one at the provider, issue a new one, and update the secret and the workloads that read it. Deleting the variable from the platform doesn't revoke anything.

Then check what the old key did. On Bedrock, `InvokeModel`, `InvokeModelWithResponseStream`, `Converse` and `ConverseStream` are logged to AWS CloudTrail as management events, which CloudTrail records by default (AWS docs). The AWS CLI can pull them from Event history:


```
# One region and one event name per call.
# Repeat for Converse, ConverseStream and
# InvokeModelWithResponseStream.
aws cloudtrail lookup-events \
  --region us-east-1 \
  --lookup-attributes \
    AttributeKey=EventName,AttributeValue=InvokeModel \
  --start-time 2026-10-01T00:00:00Z \
  --query 'Events[].[EventTime,Username,AccessKeyId]' \
  --output table
```
Look for access key IDs you don't recognize, bursts at odd hours, and calls from regions you never use. One counting trap: the two streaming operations, `InvokeModelWithResponseStream` and `ConverseStream`, also write a stream-completion event with the same event name, so count only events whose `eventType` is `AwsApiCall` (AWS docs). That field sits inside each event's `CloudTrailEvent` JSON, which the table above drops. For the streaming names, parse it with jq:


```
aws cloudtrail lookup-events \
  --region us-east-1 \
  --lookup-attributes \
    AttributeKey=EventName,AttributeValue=ConverseStream \
  --start-time 2026-10-01T00:00:00Z \
  --query 'Events[].CloudTrailEvent' --output json \
  | jq -r '.[] | fromjson
      | select(.eventType == "AwsApiCall")
      | [.eventTime, .userIdentity.accessKeyId,
         .sourceIPAddress] | @tsv'
```
The last column is the caller's IP address. Wiz's first test call came from a residential IP (Wiz), so an address outside your own infrastructure is worth a second look.

For direct provider keys, the provider's usage page is the log. Northflank asks you to report anything suspicious both to its support team and to the provider (status page).

Rotation closes out this incident. For the next one, if your hosting platform holds an Anthropic key, give that key its own workspace. In the Claude Console, a workspace can carry a monthly spend limit with alerts at thresholds, plus per-model rate limits on requests and tokens per minute (Anthropic docs). The Default Workspace can't take a spend limit, so a production key shouldn't live there.

The pattern carries to any provider: one key per deployment, scoped to the models it calls, with a cap or an alert you'd notice the same day. Wiz's Case B shows why: nearly every call that failed was stopped by the provider's throttling (Wiz), and that was the closest thing the stolen key had to a cap.

I haven't used Northflank or any of the platforms in Wiz's list myself. If you're on one of them, spend an hour this week listing every AI key it holds, then rotate them.

Picking a host is also picking who holds your secrets, and that choice rarely gets the same attention as pricing or deploy speed. Ship It covers everything between localhost and production for a small team: hosting, payments, auth, CI/CD and monitoring. If this week made you rethink where your keys live, it's the wider map.

I also build Hermes IDE, so weigh this with that in mind: it's free, source-available, runs on macOS, Windows and Linux, and each agent signs in with its own CLI, so Hermes never asks for your tokens. It's on GitHub.

## Top comments (0)
