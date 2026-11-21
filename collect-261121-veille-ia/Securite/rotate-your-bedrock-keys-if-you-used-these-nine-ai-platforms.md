---
id: collect-261121-veille-ia/securite/rotate-your-bedrock-keys-if-you-used-these-nine-ai-platforms
title: "Rotate Your Bedrock Keys If You Used These Nine AI Platforms"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "bedrock", "platforms", "about", "keys", "calls", "used", "nine"]
source: https://www.beri.net/article/wiz-black-october-ai-developer-platform-breaches-stolen-bedrock-keys-llmjacking-rotate-env-vars-cloudtrail
source_anchor: ""
source_lines: [1, 81]
section: "Securite"
---
**If any of your developers deployed on Railway, Fireworks AI, Zeabur, Lightning AI, OpenCode, Warp, CommandCode, Northflank or Andromeda, assume every environment variable those platforms could read is in someone else's hands, and rotate it.** Nine AI developer platforms disclosed breaches in October 2026, and Wiz's "Black October" report, published October 30, followed two stolen enterprise Amazon Bedrock keys from those breaches into use. Both were tested within 30 minutes of the vendor compromise. One then made about 800,000 Bedrock calls over roughly 30 hours, and the other about 1.7 million in 13 hours, together likely more than $100,000 of usage at list price.

What the attackers wanted from the platforms was the model credentials customers had parked there.

## Which Platforms Were Breached and What Attackers Took

Every public report Wiz reviewed describes the same target: stored environment variables, especially the ones that grant access to AI models. Wiz lists the disclosures in order: Fireworks AI on October 6, Andromeda on October 9, Zeabur on October 14, Lightning AI on October 15, OpenCode on October 16-17, Railway on October 20, Warp on October 23, CommandCode on October 28 and Northflank on October 30.

The root causes differ. A summary of the Wiz findings by Beam notes that no single vulnerability or dependency links the nine. Three of the vendor write-ups show how much reached the attacker:

- Railway's preliminary report describes "potential privilege escalation in our build infrastructure", followed by "an unauthorized query of encrypted environment variables", a subset of which the attacker decrypted. Railway puts the exposure at "at most 1.85% of all workspaces", about 100,000 workspaces, and on October 21 asked vendors including Anthropic, OpenAI, OpenRouter, GitHub and Stripe to revoke keys.
- Zeabur's attacker read 3.2 million environment-variable records covering 321,810 accounts, BleepingComputer reported. The attacker used leaked internal credentials stored in plaintext; Zeabur said its admin tokens had no expiry and had not been rotated since January 2024, and it will shut down entirely by December 31, 2026.
- Fireworks' bulletin says a third party used internal credentials to read environment variables in evaluation jobs for a limited number of customer accounts, "including API keys and tokens stored as secrets."

Fireworks' bulletin also has the line every platform team should forward to its developers: "Removing a stored secret alone does not invalidate the underlying credential." Deleting the variable from the dashboard does nothing. The key has to be revoked by whoever issued it.

## How a Stolen Bedrock Key Gets Used

A stolen Bedrock key gets validated within the hour, can turn up in what Wiz calls an apparent reseller pool, and is then flooded until someone turns it off. Wiz saw the same opening in both cases: one `InvokeModel` call from a residential IP, part of a one-minute sweep across several organisations, asking for Claude Fable 5.1 and falling back to Opus 5.5 when that failed. A small Go client then kept checking the key 14 to 120 times a day.

What happened next depended on the key's policy.

Case A was unrestricted and could call `global.*` inference profiles. On day four a rotating proxy pool probed one model per vendor, a boto3 script enumerated Bedrock and EC2 quotas, and bulk traffic began 22 seconds later. Over about 30 hours the key made around 800,000 calls, roughly 90% of them successful and only about 6% throttled. Opus models made up about three quarters of the successful calls.

Case B was restricted to `us.*` profiles. When it appeared in a reseller pool on day seven, thousands of calls failed for four hours because the pool asked for `global.*` profiles. The next morning a checker found the working `us.*` model IDs. After that a single Level 3 IP made about 1.7 million calls in 13 hours, more than 1 million of them successful, and most of the failures were throttling from regional quotas.

The restricted key was still abused heavily. The restriction bought hours, and the regional quota put a ceiling on the flood, but scoping alone did not stop it. Treat a narrow policy as a way to limit the damage and buy detection time.

Wiz also names two resale operators that tested these keys. One, behind 23.106.46.239 (the IP Zeabur published), is tied to an LLM resale site that takes USDT and tried a `PutAccountDataRetention` call with the Case B key. Wiz is explicit that it could not tie either operator to the original breaches or to the floods, and most vendor investigations were still open on publication day.

## What to Search for in CloudTrail Today

You can hunt this in CloudTrail without turning anything on, because Bedrock's main inference calls are management events. AWS's CloudTrail page for Bedrock says `InvokeModel`, `InvokeModelWithResponseStream`, `Converse` and `ConverseStream` are logged as management events, which CloudTrail records by default. Event history covers the recent window even without a trail.

Search for:

- Source IPs 23.106.46.239, 38.246.114.118, 104.243.32.50 and 31.57.201.201, and traffic from the hosting ASNs Wiz lists: AS54600, AS398823 (PEG TECH), AS45102 (Alibaba US), AS142002 (Scloud), AS46844 (Sharktech) and AS10753 (Level 3). Wiz has no evidence tying them directly to the breaches.
- `InvokeModel` failures followed by a fallback to another model a few seconds later, and calls requesting`global.*` profiles from a key your application only uses with regional ones.
- Quota enumeration from a boto3 user agent, and any `PutAccountDataRetention` call.
- The `additionalEventData.inferenceRegion` field, which AWS documents as the place to see where a cross-Region request actually ran.

When you count calls for streaming APIs, filter on `eventType` = `AwsApiCall`. AWS notes that each stream also writes a completion event with the token counts, and counting both doubles your volume.

One gap to plan around: the Bedrock API keys documentation says the keys "are passed as authorization headers and are not logged." CloudTrail tells you which IAM identity made a call. If several platforms share one IAM user, it cannot tell you which leaked copy was used. One key per platform fixes that.

## How to Narrow a Bedrock Key Before the Next Breach

The Case B pattern is the one to copy: regional profiles only, low quotas, and short-lived credentials where you can manage it. AWS's own guidance points the same way.

Global profiles are the setting most teams will argue about. AWS's cross-Region inference comparison lists roughly 10% savings and routing across every supported commercial Region for global profiles, against geographic profiles that stay inside the US, EU or APAC. That 10% is a real saving, and for some workloads the extra capacity matters. The same table says global profiles need SCPs to allow `aws:RequestedRegion` of `"unspecified"`. Leave that value out of your region allowlist and global routing is off for every key in the organisation. The LLM data residency argument already pointed most regulated teams to geographic profiles. Wiz's Case A adds an abuse argument.

Short-lived credentials are the second control. The API keys page says short-term keys last up to 12 hours and warns that "long-term keys are for exploration only." The AWS Security Blog's guidance on Bedrock API keys goes further: use temporary STS credentials wherever possible, and use SCPs to deny API keys if you do not need them. The levers are `bedrock:CallWithBearerToken` and `bedrock-mantle:CallWithBearerToken` (deny both to block key use on either endpoint), the bearer-token-type condition key to allow `SHORT_TERM` but not `LONG_TERM`, and `iam:ServiceSpecificCredentialAgeDays` to cap how long a new long-term key can live.

A static key pasted into a PaaS environment variable is the case all of this is meant to retire. If a third-party platform cannot use role-based credentials, give it its own key, its own IAM user and a quota sized to its real traffic. Then put a spend alarm on that key. Wiz's Case A ran for about 30 hours before remediation. A per-key alarm tied to normal daily volume should have fired within the first hour of bulk traffic. Our AI FinOps tooling guide covers the tagging you need first.

The standing-privilege problem in agent authorization and the non-human identity sprawl behind it are the same issue at a larger scale.

## What to Do Before Monday

**This Week:**

1. Ask each engineering lead one question in writing: which of the nine platforms hold a deployment, a preview environment or an eval job of ours? Include personal accounts that developers used for company work.
2. For every yes, revoke and reissue every secret that platform could read at the issuer (AWS, Anthropic, OpenAI, OpenRouter, GitHub, Stripe, database passwords), whether or not you received a notice. Railway tells anyone who signed up before October 21 to rotate keys stored there.
3. Run the CloudTrail searches above over October 1 to today, for every account with Bedrock enabled.
4. Zeabur customers: plan the migration now. Shared clusters pause on December 1.

**This Month:**

1. Remove `"unspecified"` from the SCP region allowlist unless a named workload needs global profiles, and document that exception.
2. Deny `LONG_TERM` bearer tokens across the organisation, and cap any long-term key that survives at 30 days with`iam:ServiceSpecificCredentialAgeDays` .
3. Give every third-party platform its own IAM identity, its own Bedrock quota and a CloudWatch alarm on invocation count.

**Before Your Next Platform Renewal:**

1. Ask every PaaS and AI platform vendor how customer secrets are encrypted at rest, who can decrypt them, and whether they support workload identity federation instead of static keys. Zeabur's plaintext internal credentials are the failure this question is meant to surface.

## The Bottom Line

October's breaches follow the pattern of the LiteLLM 1.107.0 backdoor and the eval-sandbox key theft in Anthropic's threat report: attackers go wherever enterprise model keys sit, and right now that is a long list of third-party developer platforms. AWS's answer for cloud access keys was temporary STS credentials, and its Bedrock key guidance gives the same answer for model access. Model keys are now worth enough to resell through the operators Wiz found, and Bedrock already has the controls to stop handing them out as static secrets.

Rotate first, then search CloudTrail for the four Wiz IPs.
