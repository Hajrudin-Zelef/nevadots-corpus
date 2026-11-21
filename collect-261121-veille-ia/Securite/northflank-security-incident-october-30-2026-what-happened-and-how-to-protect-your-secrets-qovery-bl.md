---
id: collect-261121-veille-ia/securite/northflank-security-incident-october-30-2026-what-happened-and-how-to-protect-your-secrets-qovery-bl
title: "Northflank Security Incident (October 30, 2026): What Happened and How to Protect Your Secrets - Qovery Blog"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["northflank", "october", "incident", "secrets", "qovery", "security", "happened", "rotate"]
source: https://www.qovery.com/blog/northflank-security-incident-secrets-own-cloud
source_anchor: ""
source_lines: [1, 197]
section: "Securite"
---
- **What happened.** On October 30, 2026, Northflank reported a security incident. Its status page tells every user to "assume all environment variables have been decrypted" and to rotate all secrets. Users report that their AI keys were already used.
- **What Northflank said on November 2.** The incident is now in "Monitoring". Northflank believes the attack was contained within an hour, says running workloads were unaffected and the attack focused on secrets held in its control plane, and says customers on forward-deployed or dedicated control planes are unaffected. Independent forensic auditors are reviewing the attack.
- **What Northflank said on November 6.** The first known decryption of secrets happened between 02:39 and 03:08 UTC on October 30, 8 to 9 hours before the first alert at 11:43 UTC. Northflank says the exploit was patched within 60 minutes of that alert and customers were notified within two hours.
- **What is not known yet.** As of the November 6, 10:09 UTC update, Northflank has not published the root cause. It says it will share a root cause analysis once findings are confirmed.
- **What to do today.** If you use Northflank, rotate AI and payment keys first, then database and cloud credentials, then the rest. Check provider usage logs for the days before the incident.
- **Why this keeps happening.** Wiz's Black October report lists nine AI developer platforms that disclosed incidents in October 2026, including Railway on October 21 and Northflank. Attackers went after stored environment variables, especially AI keys. A shared platform stores the secrets of all its customers in one place, so one break-in pays off many times.
- **What we learn at Qovery.** This can happen to any platform, including ours. Qovery is not a PaaS and does not host customer apps. Your apps run in your own cloud account, Qovery reaches it through an IAM role you can delete, and secret values can stay in your own secret manager. Qovery still stores the secrets you add in the Console, encrypted, and this article says so plainly.

# Northflank Security Incident (October 30, 2026): What Happened and How to Protect Your Secrets

On October 30, 2026, Northflank told all users to assume their environment variables were decrypted and to rotate every secret. Here is the verified timeline, what to rotate first, why shared platforms keep getting hit after Railway, and how Qovery limits the blast radius, including where we still hold secrets.

First, to the Northflank team: we are sorry this happened to you, and we wish you a fast recovery. Northflank competes with Qovery, and we would never wish this on anyone. A security incident can happen to any company, including ours. The engineers working on it today have a hard few days ahead, and we hope their customers are safe again soon.

I am writing this because many teams who use Northflank are asking what to do this week, and because the same question applies to every deployment platform, ours included: if the platform is hacked, what can the attacker read?

This article sticks to what Northflank and its users have said in public. When something is not known, I say so.

Northflank's status page lists a "Security incident" marked as a major outage. Here is the timeline it gives, in UTC:

| Time (UTC) | Event | 
|---|---|
| 11:43 | Wiz, a cloud security company, sends a threat alert | 
| 11:44 | Northflank engineers review the alert | 
| 12:02 | First network policy patch | 
| 12:50 | More patches deployed | 
| 13:00 | Northflank asks the Wiz incident response team (CIRT) for help | 
| 13:05 | More patches deployed | 
| 13:40 | Status "Investigating": "Rotate environment variables immediately." | 
| 13:58 | Status changes to "Identified": "Assume all environment variables have been decrypted. Disable/rotate all keys." | 
| 15:12 | Secret rotation for Postgres and MongoDB enabled for all users. "Multiple users are confirming that their AI keys have been used." | 
| 19:28 | "Our investigation is ongoing. We have no further confirmed findings to share at this time." | 
| 23:38 | Users report unauthorized use of AI API keys stored in Northflank. Northflank links the reports to Wiz's Black October research on attacks against AI credentials. | 
| Oct 31, 01:04 | Audit logs enabled for all customers, including free users | 
| Oct 31, 13:34 | Secret rotation for MySQL and Redis now available to all customers. "Please rotate all secrets if you have not already." | 
| Oct 31, 22:32 | Secret rotation for MinIO addons now available to all customers | 
| Oct 31, 23:31 | Secret rotation for RabbitMQ addons now available to all customers | 
| Nov 2, 11:09 | Status moves to "Monitoring". Attack believed contained "within an hour of the first alert"; running workloads unaffected; customers on forward-deployed or dedicated control planes unaffected; CVE-2026-98432 patched across the fleet; Northflank tokens, sessions, VCS integrations and cross-account IAM roles revoked where possible | 
| Nov 6, 10:09 | First known decryption of secrets "between 02:39 UTC and 03:08 UTC on 30 October"; exploit "patched within 60 minutes of notification"; customers notified within two hours; no evidence that workloads or workload data were accessed; advice to enable IP restrictions and budget limits | 

Northflank's advice covers secrets in workloads, secret groups and global secrets. It also asks users to turn off public access on addons (Northflank's managed databases and services) where it is enabled.

On the morning of November 7, the status page marks Networking, Certificates, Services, Addons, Jobs, Builds, and Logs and Metrics as "Degraded performance", down from a major outage. Northflank says workloads stayed online throughout, and that the degradation came from many customers redeploying at the same time and from its own key rotation and hardening work.

Northflank's November 2 update answers several open questions. In its own words:

- **Containment.** "We believe the attack vector was contained within an hour of the first alert. Our investigation shows no sign that the attacker was in our environment after the attack."
- **Scope.** "Running workloads were unaffected. At this time we have seen no sign that workloads or workload data were accessed during the attack. Evidence suggests it was focused on secrets held in our control plane."
- **Dedicated control planes.** "Customers on forward-deployed or dedicated control planes are unaffected."
- **Patching.** Northflank patched CVE-2026-98432 across its fleet, including BYOC clusters with an active integration. It has not said whether this CVE was how the attacker got in.
- **Access revoked.** "Where we were able to act on your behalf, we have revoked Northflank access tokens and sessions, VCS integrations, and cross-account IAM roles and service accounts." Customers need to sign in again and reconnect their integrations.
- **Review.** Independent forensic auditors are reviewing the attack with Northflank, and Northflank intends to share a root cause analysis once findings are confirmed.

The incident reached the front page of Hacker News the same afternoon. One user wrote that a small app on Northflank now needed new RevenueCat, Apple and AI keys. Another listed six attacks on hosted platforms since September, including Railway.

The November 6 update adds a timeline of the attack itself:

- **Exposure window.** "The first known decryption of secrets occurred between 02:39 UTC and 03:08 UTC on 30 October." The first alert came at 11:43 UTC the same day.
- **Response.** "The exploit was patched within 60 minutes of notification, and customers were notified within two hours."
- **Workloads.** "At this time, we have found no evidence that customer workloads or workload data were accessed."
- **Upstream providers.** Northflank worked with providers to revoke credentials where possible, and asks AI and cloud providers to offer API endpoints for credential revocation, IP restrictions and enforceable budget limits.
- **Advice to users.** Rotate all integration and project secrets, review provider logs, and enable IP restrictions and budget limits wherever available.

- **The root cause.** Northflank has not said how the attacker got in. It patched network policies on October 30 and CVE-2026-98432 on November 2, but has not said whether either was the entry point. Its November 6 update says the investigation and an independent forensic review are still ongoing, and a root cause analysis is promised.
- **Who is affected.** Northflank says the attack focused on secrets held in its shared control plane, and the rotation guidance still applies to all customers. Customers on forward-deployed or dedicated control planes are unaffected. There is no count of affected customers.
- **BYOC customers on the shared control plane.** Northflank's BYOC option runs workloads in the customer's cloud account. The November 2 update says running workloads were unaffected, and that BYOC clusters with an active integration received the CVE patch. It does not say separately whether secrets of BYOC customers managed by the shared control plane were exposed, so rotate them too.

We will update this article when Northflank publishes more.

Rotate the secrets an attacker can turn into money or access fastest. The order below is the one we used in our Railway rotation guide, which has the full step-by-step checklist.

1. **AI provider keys** (OpenAI, Anthropic, and others). Northflank users already report misuse. Revoke the old key, create a new one, and set a spending limit on it.
2. **Payment and billing keys** (Stripe, RevenueCat, App Store Connect). Stripe can keep the old and new keys working together for a short time, so you can deploy the new key before you revoke the old one.
3. **Cloud credentials** (AWS access keys, GCP service account keys). Replace long-lived keys with IAM roles where you can.
4. **Database passwords.** Northflank enabled rotation for its Postgres and MongoDB addons on October 30, then for MySQL, Redis, MinIO and RabbitMQ on October 31. For external databases, create a second user, deploy it, then delete the first.
5. **Session and JWT signing keys.** Changing them logs every user out, so plan the timing.
6. **OAuth client secrets, webhook secrets, Git tokens and registry credentials.**

Rotate today even if you see no misuse yet. In the cases Wiz studied, the stolen keys were tested within 30 minutes, but the heavy use started four and seven days later. Each day you wait gives the attacker more time.

For each key, follow the same four steps: create the new one, deploy it, check that the app works, then revoke the old one. Revoking first is the fastest way to cause your own outage.

Northflank revoked its access tokens and sessions, VCS integrations, and cross-account IAM roles and service accounts where it could. Sign in again, reconnect your Git integrations, and recreate the cloud roles Northflank needs, with new credentials.

When you are done, check the provider side. Look at usage and billing for each rotated key over the last few days, and report anything unusual to Northflank support and to the provider, as Northflank asks. Northflank turned on audit logs for all customers on October 31, so check them for changes you did not make. Look for new API keys, users, SSH keys, OAuth apps or webhooks that you did not create. An attacker who had your keys may have used them to create new ones.

Two more checks, based on the Wiz report:

- **Search your logs for the IP addresses** in the table in the next section, for example in AWS CloudTrail or your AI provider's usage logs.
- **Restrict your new AI keys.** Limit each key to the models and regions you use, and set a quota or spending limit. Northflank also recommends IP restrictions wherever the provider supports them. In Wiz's second case, a key limited to one group of AWS regions blocked thousands of calls.
- **Check usage from October 30 onward, from 02:39 UTC.** Northflank says the first known decryption happened between 02:39 and 03:08 UTC that day, hours before it was detected.

A deployment platform has to give your app its secrets. So at some point it stores them, decrypts them, or both. When the platform is shared by many customers, the store holds the secrets of all of them.

That makes a deployment platform a good target. One way in gives the attacker database passwords, cloud keys and AI keys for many companies at once. AI keys are the easiest to use right away, which matches what Northflank users report.

Wiz's Black October report, published on October 30, lists at least nine AI developer platforms that disclosed incidents in October 2026: Fireworks AI (Oct 6), Andromeda (Oct 9), Zeabur (Oct 14), Lightning AI (Oct 15), OpenCode (Oct 16-17), Railway (Oct 20), Warp (Oct 23), CommandCode (Oct 28) and Northflank (Oct 30). Zeabur is shutting down after its incident. Wiz writes that "all public reports highlight targeted exploitation, explicitly hunting stored environment variables, with a focus on those granting AI access."

The ways in were different. Fireworks, Zeabur and Lightning AI point to stolen credentials. Railway believes the attacker got in through "privilege escalation in our build infrastructure." Warp points to a third-party vendor, and OpenCode to a "legacy system." Fireworks, OpenCode and Warp report signs of an "agent swarm," meaning attackers used AI agents to move faster. Wiz says "there is no single vulnerability or dependency linking these attacks."

Wiz followed two stolen AWS Bedrock keys from start to finish:

1. **Within 30 minutes** of the theft, one test call checked that each key worked.
2. **For several days** , a small script checked the keys a few times a day to make sure they were still valid.
3. **Four and seven days later** , the keys were used at scale. One made about 800,000 calls in 30 hours, the other about 1.7 million calls in 13 hours, mostly to Claude models. Wiz estimates the two cases "likely ran up over $100k in usage."
4. **The keys showed up in resale pools** , websites that sell cheap access to AI models. Wiz names two of them.

The second key did less damage because it was restricted to one group of AWS regions. Thousands of calls failed, and the account's regional quotas slowed down the rest.

The list keeps growing. On November 1, Trigger.dev, a platform that runs background jobs and AI agents, disclosed that an attacker "gained unauthorized access to environment variables" for a subset of its customers. Trigger.dev says it has blocked the exploit and emailed every affected account owner. It has not published how the attacker got in.

Wiz shares these IP addresses, which tested or used stolen keys. Wiz says it has no evidence that links them directly to the platform breaches, so treat a match as a reason to investigate.

| IP address | What Wiz saw | Dates (2026) | 
|---|---|---|
| 23.106.46.239 | Named by Zeabur, later used to check stolen credentials | Oct 26-29 | 
| 38.246.114.118 | Checked permissions on several stolen keys | Oct 25-29 | 
| 104.243.32.50 | Checked permissions on several stolen keys | Oct 19-30 | 
| 31.57.201.201 | Used multiple stolen keys | Oct 18-25 | 

A Hacker News commenter described a second problem. Platforms usually let users read a variable back in the dashboard or the CLI. If a platform can show you a secret, then anyone who takes over the platform's API, or a user's session, can read it too.

Encryption at rest does not solve this. Railway's preliminary report says its variables were encrypted, and some were decrypted anyway. Something has to decrypt a secret to deploy your app, and that something can be attacked.

Qovery deploys code too, and nothing in this section means it could not happen to us. The honest answer has two parts: what we do not hold, and what we still hold.

Northflank and Railway are managed PaaS products: they run their customers' apps on infrastructure they operate. Qovery does not, and we never will. We built Qovery from day one around one rule: the control plane (Qovery) is separate from the data plane (your cloud account).

Qovery installs and manages a Kubernetes cluster in your own AWS, GCP, Azure or Scaleway account, or connects to a cluster you already run. Your apps, databases and TLS keys run there, and your logs are stored there. There is no Qovery-run fleet of servers where customer apps sit next to each other, so there is no shared runtime for an attacker to move through, and another customer's code never runs in your cluster.

Qovery's control plane sends deployment instructions to the Qovery engine over encrypted gRPC or TLS. It holds your configuration and status, and the secrets you choose to store in Qovery. The security overview describes this split in detail.

Railway's entry point was its build infrastructure. By default, Qovery builds your images on a pool of build workers that Qovery runs, in a separate Kubernetes cluster used only for builds and kept apart from the rest of Qovery's infrastructure. Each build gets a disposable, short-lived build node. Only the secrets you mark for the build are passed to it, and they are not stored in the image. If you prefer builds to stay in your own account as well, you can choose to run them in your own cluster instead. When you connect a cluster you already run (BYOK, in Preview), the Qovery Operator does this by default. You install it once with Helm, it opens an outbound connection to Qovery, so Qovery never stores your kubeconfig, and it runs deployments as Jobs inside your cluster. Images are built there and pushed to a registry in your own cloud account.

With the Secret Manager integration, you give Qovery a reference such as `prod/payments/stripe` instead of the value. The External Secrets Operator, running in your cluster, reads the value from AWS Secrets Manager, AWS Parameter Store or GCP Secret Manager and passes it to your app. The value never goes through Qovery. If Qovery's control plane were breached, the attacker would find the path to your secret but not the secret itself.

Two limits to know. This integration is available today on Qovery-managed clusters on AWS and GCP. And the operator reads the values at deploy time, so after you rotate a secret you redeploy the services that use it. For the most sensitive secrets, your app can also read them at runtime with an IAM role, which needs no redeploy. Our article on how Qovery stores secrets shows both setups.

On AWS, Qovery recommends the STS assume role setup. Qovery gets short-lived credentials from an IAM role in your account, instead of storing a permanent access key. Since October 23, the role also requires your organization ID as the `sts:ExternalId`, so it only accepts requests made for your organization (details). On GCP, Workload Identity Federation works the same way.

The role needs wide permissions, because Qovery creates and upgrades clusters, networks and databases for you. You keep control of it: every call Qovery makes with the role appears in your AWS CloudTrail, and you can delete the role at any time. Qovery then loses access to your account in one step, and your apps keep running in your cluster.

Secrets you add in the Qovery Console, without a secret manager, are stored in Qovery's database. They are encrypted with AES-256 and a key unique to your organization, hidden in the Console, and cannot be read back through the API. They are decrypted when a deployment needs them.

Write-only secrets close the read-back path described above, because no screen or API call shows the value. They do not close every path, and no platform is immune to the ways in that Wiz lists, including ours. An attacker with deep enough access to our control plane could reach what the deployment process decrypts. That is why, after the Railway and Northflank incidents, I recommend keeping production secrets in your own secret manager and giving Qovery only the references.

I have always told my team that this can happen to anyone, and a breach like this is one of my biggest fears at Qovery. That is why we have invested in security since the first days of the company. It is a large part of our budget, and we will keep investing in it.

Qovery is SOC 2 Type II certified, and independent experts run a penetration test every year with one main goal: stealing customer secrets. Reports are on our trust center. We do our best to keep our customers safe, and none of that makes a breach impossible. It is why we would rather you hold the values yourself.

Use these with Qovery, Northflank, Railway or any other vendor.

| Question | Why it matters | 
|---|---|
| Where are secret values stored, and who can decrypt them? | Tells you what an attacker gets if the platform is breached | 
| Can I give the platform references instead of values? | If yes, a breach exposes paths, not secrets | 
| Can anyone read a secret back in the UI, CLI or API? | Write-only secrets cut the session-theft path | 
| Do my apps run on servers shared with other customers? | Shared runtime means another customer's code is next to yours | 
| Does the platform store a permanent cloud key for my account? | Short-lived role credentials expire and can be revoked | 
| Can I cut the platform's access to my account in one step? | Your exit plan during an incident | 
| Do my apps keep running if the platform is down? | In your own account, a platform outage stops new deploys, not your running apps | 

To the Northflank team and its users: we hope you are back to normal soon.

## What happened in the Northflank security incident?

On October 30, 2026, a Wiz threat alert at 11:43 UTC led Northflank to investigate and patch its network policies. At 13:58 UTC, Northflank told all users to assume their environment variables were decrypted and to rotate every key. Users reported that their AI keys had been used, and on October 31 Northflank enabled audit logs for all customers. On November 2, Northflank moved the incident to "Monitoring", said the attack was contained within an hour and focused on secrets held in its control plane, and said running workloads and customers on dedicated control planes were unaffected. On November 6, it said the first known decryption of secrets happened between 02:39 and 03:08 UTC on October 30, and that the exploit was patched within 60 minutes of the 11:43 UTC alert. The root cause is not public yet; Northflank says a root cause analysis will follow.

## Do I need to rotate my secrets if I use Northflank?

Yes. Northflank's own guidance is to rotate all secrets in workloads, secret groups and global secrets, and to turn off public access on addons. Start with AI and payment keys, then cloud and database credentials.

## Are Northflank BYOC customers affected?

Partly. On November 2, Northflank said that running workloads were unaffected, that customers on forward-deployed or dedicated control planes are unaffected, and that BYOC clusters with an active integration were patched for CVE-2026-98432. Secrets held in the shared control plane were the target, so if your BYOC setup uses Northflank's shared control plane, rotate your secrets.

## Is this the same attack as the Railway incident?

Nobody has shown that. Wiz's Black October report names both Railway and Northflank among nine platforms hit in October, but says "there is no single vulnerability or dependency linking these attacks." Railway has said its initial access was through privilege escalation in its build infrastructure. Northflank has not published its root cause.

## Is Qovery a PaaS like Northflank?

No. Qovery does not host customer apps. It deploys and manages them in your own AWS, GCP, Azure or Scaleway account, or on a Kubernetes cluster you already run. The control plane and the data plane have been separate since the first version of the product.

## Could the same thing happen to Qovery?

Qovery's control plane stores the secrets you add in the Console, encrypted, so a deep enough breach of Qovery could expose them. What we do not hold: your apps, your data, or permanent cloud keys when you use STS or Workload Identity Federation. Secrets you keep in AWS Secrets Manager, Parameter Store or GCP Secret Manager never pass through Qovery.

## How do I keep secret values out of my deployment platform?

Store them in a secret manager in your own cloud account and give the platform a reference. With Qovery, the External Secrets Operator in your cluster reads the value at deploy time. For the most sensitive values, have your app read them at runtime with an IAM role.

Romaric founded Qovery to make Kubernetes accessible to every engineering team. He writes about platform strategy, developer experience, and the future of cloud infrastructure.

### Keep your secrets in your own cloud account.

Qovery deploys your apps into your own AWS, GCP, Azure or Scaleway account, and can pull secret values straight from your secret manager inside your cluster.
