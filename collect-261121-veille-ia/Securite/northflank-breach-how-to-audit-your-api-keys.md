---
id: collect-261121-veille-ia/securite/northflank-breach-how-to-audit-your-api-keys
title: "Northflank breach: how to audit your API keys"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["northflank", "keys", "customers", "first", "breach", "platform", "attacker", "october"]
source: https://www.trickyworld.in/computer-tricks/cyber-security/northflank-breach-audit-api-keys/
source_anchor: ""
source_lines: [1, 212]
section: "Securite"
---
Some time between 02:39 and 03:08 UTC on 30 October 2026, an attacker began decrypting the secrets Northflank stores for its customers. The first alert came about nine hours later. By the time the Northflank breach went public that afternoon, the platform's advice was blunt: assume every environment variable you ever gave it has been decrypted, and rotate every key.

That evening, customers reported strangers spending their AI API keys. Northflank was the ninth AI developer platform to disclose an incident in October, a run that the cloud security firm Wiz calls "Black October". The lesson reaches past one vendor. If a platform can decrypt your keys to start your app, an attacker who gets inside the platform can decrypt them too. Below is what Northflank has confirmed, what it hasn't, and a tested way to find, revoke and verify every key a cloud platform holds for you.

## What happened in the Northflank breach?

An attacker reached the part of Northflank's platform that holds customer secrets and decrypted them on 30 October 2026. Within hours, customers reported their AI API keys being used by someone else.

Northflank is a platform-as-a-service (PaaS). Developers deploy containers, databases and scheduled jobs on it, and hand it their API keys and passwords as environment variables. Its incident page gives this timeline, in UTC:

- 02:39 to 03:08: the first known decryption of customer secrets.
- 11:43: Wiz sends the first alert. Engineers start reviewing it a minute later and deploy a network policy patch at 12:02, with more patches at 12:50 and 13:05.
- 13:40: the incident goes public with one instruction, "Rotate environment variables immediately."
- 13:58: "Assume all environment variables have been decrypted. Disable/rotate all keys."
- 23:38: "Users have reported unauthorised use of AI API keys supplied to the Northflank platform."
- 31 October: audit logs are switched on for every customer, free accounts included. Secret rotation reaches MySQL, Redis, MinIO and RabbitMQ add-ons, after PostgreSQL and MongoDB the day before.

Put the first and third lines side by side. Roughly 11 hours passed between the first decryption and the first public warning of the Northflank breach. Northflank says it patched the hole within 60 minutes of the alert and told customers within two hours, so the slow part was detection.

Later updates said what Northflank believes was and wasn't touched. On 2 November it said it believed the attack vector was contained within an hour of the first alert. The attacker went after "secrets held in our control plane", not running workloads, and customers on forward-deployed or dedicated control planes were unaffected. Northflank also revoked what it could on customers' behalf: Northflank access tokens and sessions, version-control integrations, and cross-account IAM roles and service accounts.

Much is still unknown. Northflank hasn't said how the attacker got in, how many customers were affected, or whether anything besides AI keys was abused, and BleepingComputer's questions to the company went unanswered. It also hasn't said plainly whether customers who run Bring Your Own Cloud (BYOC) clusters on its shared control plane are in the clear.

## Has Northflank published a post-mortem?

Northflank hasn't published one yet. Its last update, on 6 November, says an investigation and an independent forensic review are still under way, and promises "a root cause analysis once available".

Nothing newer had appeared by 17 November. Any write-up that tells you exactly how the attacker got in is going beyond what Northflank has said.

One detail is already being misread. In its 2 November update, Northflank wrote: "Separately, we have patched CVE-2026-98432 across our fleet, including BYOC clusters where we have an active integration." The Cloud Security Alliance's research note on the Northflank breach describes that as patching "the exploited vulnerability". Northflank's own word is "separately", and it has not tied the bug to the breach.

The bug is worth knowing about anyway. CVE-2026-98432 is a use-after-free in the Linux kernel's IPv4 TCP code, rated 9.8 out of 10 in its NVD record. On 27 October Anthropic described an exploit Claude wrote for it, which lets a remote attacker "run arbitrary commands as root" on Linux servers with a Multipath TCP (MPTCP) listener. The proof of concept needed 48 minutes to leak a kernel pointer and five more to run its command. Go's security team pointed out that Go programs have enabled MPTCP on listening sockets by default since Go 1.24.

NVD lists the fixed kernels as 5.10.272, 5.15.223, 6.1.190, 6.6.159, 6.12.113, 6.18.56 and 7.2.10, one per stable branch. Anthropic's write-up came out three days before the breach began, and that timing invites a connection. Northflank hasn't made it. Patch your own Linux hosts either way, and treat the link as unproven until the root cause analysis is out.

## Why were AI API keys abused first?

Because a stolen AI key pays out at once: it works from anywhere on the internet, every call is billed to its owner, and resellers buy access in bulk.

Wiz's "Black October" report, published the evening of the Northflank breach, counted at least nine AI developer platforms that disclosed incidents that month: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank. Its running log kept growing, and by 10 November it had added Brev, Trigger.dev, Chainstack, kernel.sh, Blacksmith, Tensorlake and NEAR AI, which by my count puts 16 platforms on the list. Wiz found no single vulnerability linking them, but a shared goal: "All public reports highlight targeted exploitation, explicitly hunting stored environment variables, with a focus on those granting AI access."

Wiz also traced what happens to a key after it is stolen, using two enterprise Amazon Bedrock credentials taken in one of the vendor breaches. It did not say which vendor, so don't read these cases as Northflank's. Both keys were first tested within 30 minutes of the theft. The test was a single call that asked for Claude Fable 5.1, failed, and fell back to Opus 5.5, which worked.

Then the keys went quiet. For days, each saw only 14 to 120 calls a day from the same small Go client, a job checking that they still worked. The flood came four to eight days after the first test. One key served about 800,000 Bedrock calls in 30 hours. The other was restricted to US inference profiles, and it still served more than a million successful calls in 13 hours. Wiz estimates that the two cases "likely ran up over $100k in usage" at list price. It also found two resale operations, which it calls "momo" and "Xingjiabi", testing the same keys, though it could not tie either to the floods.

Two lessons sit in those numbers. The theft-to-test gap was minutes, but the test-to-flood gap was days, so rotating within a day or two would likely have prevented most of the cost. And the restricted key was still abused at scale; its limits only held the traffic to the account's regional quotas. Scoping a key limits the damage, but it doesn't make a stolen key safe. OpenCode, another October victim, gave its users the same advice, as we covered in the OpenCode console breach.

## Are your API keys exposed on other cloud platforms?

They are if the platform can decrypt them and the platform is breached. Every PaaS that injects environment variables into your containers has to be able to decrypt them, so encryption at rest protects against a stolen disk, not an attacker inside the platform.

Railway and Zeabur, two other platforms on Wiz's list, have published the most detail, and both read like warnings for everyone else. Railway's preliminary report says an attacker escalated privileges in its build infrastructure from 20 October, pulled encrypted environment variables and decrypted a subset with stolen credentials. Railway puts the exposure at no more than 1.85% of workspaces, about 100,000 of them. Before going public, it sent potentially exposed keys to GitHub, GitLab, Anthropic, OpenAI, OpenRouter, Stripe and others to be revoked, and it asks everyone who signed up before 21 October to rotate.

Zeabur's report is harder on itself. It names its root cause plainly: "long-lived admin credentials were stored in plaintext and could be used from the public internet". The attacker read 3,218,965 service environment variable records and data on 321,810 accounts, then searched them for keys it could use straight away, especially for AI services. Zeabur also found that 602 of the 607 n8n instances it checked kept their encryption key in an environment variable, so the credentials stored inside those workflows leaked too. The company says it will shut down on 31 December 2026.

So the useful question isn't only whether your platform is on the list. It's which systems can read your keys: the PaaS, the CI service, preview environments, any AI gateway and any automation tool. Each one that can decrypt a key shares that key's fate. Northflank's note that dedicated and forward-deployed control planes were unaffected points the same way. Keys held somewhere else stayed out of the Northflank breach.

## How do you audit your environment variables for leaks?

List every secret the platform holds, check each issuer's logs for use you don't recognise, revoke and replace in priority order, then prove that no old value survives anywhere, build output included.

The first step is the hard one. On Northflank alone, a key can sit in a service's runtime variables, its build arguments, a secret group, a global secret, an add-on's credentials or a secret file, often in several at once. I wrote a small tool for this, envaudit: one Python file, standard library only, that reads exports or the Northflank API and never prints, stores or sends a secret value. The steps below work with or without it.

### How to list every secret your platform holds

Pull the values from the platform's API into memory rather than into files on your laptop, and sort them by what they unlock. With a Northflank API token whose role can read secrets:

```
export NORTHFLANK_API_TOKEN=...   # read-only role: projects, services, jobs, secrets, add-ons
python3 envaudit.py scan --northflank
```
The tool walks every project's services, jobs, secret groups, add-ons and global secrets through Northflank's documented API. For services and jobs it uses the "details" endpoints, which also return values inherited from secret groups and add-ons, plus the older values they override. Here is part of its output for a test project with fake keys, served by a local stand-in for Northflank's API:

```
1  AI provider keys
   OpenAI API key                     sk-proj-...YTn5              OPENAI_API_KEY
      northflank:shop/secret-group/ai-keys  data.secrets.OPENAI_API_KEY
   ...
   OpenAI API key                     sk-proj-...YTn5              VITE_OPENAI_API_KEY
      northflank:shop/service/web/build-arguments  data.buildArguments.VITE_OPENAI_API_KEY
      ! Bundled into browser JavaScript at build time, so anyone who loads the site can read it. Move it server-side.
   ...
5  Database and other service credentials
   Database URL with password         postgres://app:***@shop-db:5432 DATABASE_URL
   ...
Same value in more than one place (rotate it once, then replace every copy):
   OpenAI API key sk-proj-...YTn5: 3 places
```
Three findings there need action. One OpenAI key sits in three places, so rotating one copy leaves two. One copy is a `VITE_` build argument, so it also shipped to every browser that loaded the site. And the database URL needs its password changed inside the database, not just in the variable.

If you're not on Northflank, point the tool at files instead: `.env` exports, `kubectl get secret NAME -o json`, `docker inspect` output, or any JSON of names and values. Delete any plaintext export once you have scanned it.

### How to check whether a key was used by someone else

Ask the issuer, not the platform, because each provider logs when and from where its keys were used. Check from the breach to today, since Wiz saw stolen keys wait four to eight days before the flood.

For AWS keys, two commands answer most of it. The first shows when, where and for which service a key was last used. The second lists its recent management events in one region. Amazon Bedrock logs model calls such as InvokeModel and Converse as management events by default, so they show up in CloudTrail's 90-day event history:

```
aws iam get-access-key-last-used --access-key-id AKIA...
aws cloudtrail lookup-events --region us-east-1 \
  --lookup-attributes AttributeKey=AccessKeyId,AttributeValue=AKIA... \
  --start-time 2026-10-30T00:00:00Z
```
Run the lookup in every region you use. Look for model calls from addresses you don't recognise, models your app never calls, and quota lookups by an unfamiliar caller. In both of Wiz's cases, the attackers enumerated Bedrock quotas just before the flood.

For Anthropic keys, the Admin API's usage report filters by `api_key_ids[]` and buckets by the hour. Its key list shows each key's `partial_key_hint`, the same prefix-and-last-four shape envaudit prints, so you can match a leaked value to its key ID. OpenAI's project key list gives each key's `last_used_at`. For Gemini keys, Google's leak checklist says to check billing and API usage in the Cloud console. If you find spend you didn't make, contact the provider with the key and the time window, as Northflank advised.

### How to rotate without leaving the old key alive

Revoke the old key with its issuer; replacing the variable alone leaves the stolen copy working. Go in the order Northflank gave on 2 November: "AI provider keys, cloud provider credentials, payment provider keys, private keys, and database and other service credentials".

Take a snapshot first, so you can check your work later. It keeps salted scrypt fingerprints and hints, never values:

`python3 envaudit.py snapshot --northflank -o before.snapshot.json`
Then, for each key, create the new one, deploy it and revoke the old one within the hour. Zeabur's guidance is to revoke first; Google's Gemini checklist says to deploy the new key before disabling the old one, to avoid downtime. If the provider's logs show the key is already being abused, revoke it at once and accept the outage. Three kinds of secret need more than a new value:

- Database passwords. Change the password inside the database, or use Northflank's add-on secret rotation, which now covers PostgreSQL, MongoDB, MySQL, Redis, MinIO and RabbitMQ add-ons.
- Build-time values. Docker build arguments and any `NEXT_PUBLIC_` or`VITE_` variable are baked in when the app is built. Rebuild after rotating, and remember that older images and deployed bundles still hold the old value.
- Keys that guard other secrets. If n8n or a similar tool keeps its encryption key in an environment variable, replace the key and re-create every credential stored with it.

Finally, sign in to Northflank again and reconnect the integrations it revoked for you, and turn off public access to any database add-on that had it.

### How to prove the rotation worked

Run the same inventory again and compare it with the snapshot. Any value that matches an old fingerprint, in its old place or anywhere new, is still the stolen one:

```
$ python3 envaudit.py verify before.snapshot.json --northflank
NOT ROTATED: still the old value
   OpenAI API key                     sk-proj-...YTn5              VITE_OPENAI_API_KEY
      northflank:shop/service/web/build-arguments  data.buildArguments.VITE_OPENAI_API_KEY
Rotated: 6   Gone: 0   New: 0   Old values still present: 1
```
In that test run, everything else had changed: the OpenAI key in the secret group and the API service, the Stripe key, the AWS key pair, the database password and the GitHub token. Only the frontend's build argument was missed, and it held the most exposed copy of all. The check exits with code 1, so it can block a deployment.

## Can a key leak through your frontend build?

Any variable with a `VITE_`, `NEXT_PUBLIC_` or similar prefix is copied into the JavaScript every visitor downloads, and a real Vite build shows the key sitting there in plain text.

I built a three-file Vite app that makes the classic mistake: it calls an AI API from the browser with `VITE_OPENAI_API_KEY`. I used Vite 8.3.8 on Node.js 22.23.2 and a fake key. The production bundle, a 1.11 kB file named `index-BXzo5A67.js`, carried the key as a plain string:

```
var e=`sk-proj-…1MgR`;document.querySelector(`#ask`).addEven…
…headers:{"Content-Type":`application/json`,Authorization:`Bearer ${e}`}…
```
The key is shortened here; the file held all 56 characters. Vite's documentation warns about exactly this.

Running `envaudit.py scan dist --strict` found the key and exited with code 1, which is enough to stop a CI job. Then I changed the variable and ran the verify step without rebuilding. It still reported the old key in the bundle, and it passed only after `npm run build`. That is the rotation trap in one test: the dashboard shows the new key while the site keeps serving the old one. Every copy a browser already downloaded stays out there until you revoke the key.

Bundles aren't the only escape route. Keys also leak through repositories and CI logs, as the 1,122 GitHub files of Claude Code settings and the Gheist branches that dumped secrets into Actions logs showed.

## How do you limit the damage next time?

Give each service its own narrowly scoped key, turn on the IP limits and hard spend caps your AI providers already offer, and replace static keys with short-lived credentials where you can.

Northflank's 6 November update ends with a request to the providers themselves: "We strongly recommend providing API endpoints for credential revocation, IP restrictions, and enforceable budget limits." Most of those controls already exist, but they are off until you turn them on. This is what each provider documents, as of 17 November 2026:

| Provider | Revoke a key | IP allowlist | Hard spend cap | Beyond static keys | 
|---|---|---|---|---|
| OpenAI | Yes, the Admin API deletes project keys. | Yes, per project or organisation. | Yes, monthly, per organisation or project. | Workload identity federation. | 
| Anthropic | Yes, the Admin API sets a key to inactive. | Yes, organisation-wide, in beta. | Yes, monthly, per workspace except the default one. | Key expiry dates and workload identity federation. | 
| Google Gemini API | Disable or delete it in the Cloud console. | Yes, per key. | A project cap, still experimental. | Keys bound to a service account, the default for new AI Studio keys since 28 May 2026. | 
| Amazon Bedrock | Deactivate the IAM access key or Bedrock API key. | Through IAM policy conditions. | No; AWS Budgets actions can apply a deny policy. | Bedrock API keys that last up to 12 hours, and IAM roles. | 

The caps have limits of their own. OpenAI says enforcement "is not instantaneous", so spend can slightly pass a hard limit. Google's cap runs about 10 minutes behind, and it warns that batch jobs and agent sessions can overrun it. An alert-only budget stops nothing; it only tells you how much has already been spent.

Beyond the providers' switches, three habits would have shrunk the damage from the Northflank breach and the rest of Black October:

- One key per service and environment. A breached platform then exposes only the keys it needed, and you can revoke them without breaking anything else.
- No AI keys in the browser. Put a small server route between your frontend and the provider, and keep the key there.
- Fewer static keys. Anthropic's IP allowlist page says an allowlist limits where a key works "but not for how long", and recommends short key expiry or workload identity federation as well.

## What should Northflank customers do this week?

Treat every secret you gave Northflank before 30 October as stolen, even if you have seen nothing odd, and work through this list in order:

1. List every secret, including build arguments, secret groups, global secrets, add-on credentials and secret files.
2. Take a fingerprint snapshot before changing anything.
3. Rotate AI provider keys first, then cloud, payment, private keys and databases, revoking each old key with its issuer.
4. Rebuild every service that bakes values in at build time.
5. Check provider logs from 30 October to today for calls, models and addresses you don't recognise.
6. Sign in again, reconnect the integrations Northflank revoked, and turn off public access to database add-ons.
7. Turn on IP limits, hard spend caps and key expiry wherever your providers offer them.
8. Run the verify step, and keep going until it exits with code 0.

None of this depends on how the Northflank breach started, which is why it can't wait for the root cause analysis. If your app reads its keys with `process.env`, our guide to environment variables in Node.js covers the basics. October's lesson is the step after that: decide where those values live, and count who else can read them.

## Frequently asked questions

## Will my AI provider refund charges from a stolen key?

Possibly, but nothing guarantees it. Northflank told affected users to contact their AI providers directly, such as OpenAI and Anthropic, and report the unexpected usage. Give the provider the key's ID, the time window and the addresses you didn't recognise, and revoke the key first so the charges stop growing.

## Is it still safe to keep secrets in environment variables?

Inside your own runtime, yes: environment variables are a normal way to hand a process its configuration. The risk is who else can decrypt them. Prefer short-lived credentials such as workload identity federation where the provider offers it, keep one key per service, and assume any platform that stores a key can expose it.

## How long should I keep watching my provider logs?

At least until every old key is revoked, and then for a few weeks. In the cases Wiz traced, stolen keys were tested within 30 minutes but abused in bulk only four to eight days later, while a small job kept checking that they still worked. Once a key is revoked, any call that uses it should fail.

## Do I need a new Northflank password or API token?

Northflank says it revoked its own access tokens and sessions where it could, so sign in again and create fresh API tokens. That protects your Northflank account, but it does nothing for the keys you stored on the platform: those must be revoked with the services that issued them.

## Sources

1. Write-upSecurity incident (Northflank status page, 30 Oct 2026, updated 6 Nov) · status.northflank.com
2. Write-up“Black October” for AI Developer Platforms (Wiz, 30 Oct 2026, log updated to 10 Nov) · wiz.io
3. Write-upBlack October Reaches PaaS: Northflank, Secret Stores, and AI Key Resale (Cloud Security Alliance, 2 Nov 2026) · labs.cloudsecurityalliance.org
4. CVE recordCVE-2026-98432 (NVD) · nvd.nist.gov
