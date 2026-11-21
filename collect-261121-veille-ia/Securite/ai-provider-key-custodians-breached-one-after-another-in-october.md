---
id: collect-261121-veille-ia/securite/ai-provider-key-custodians-breached-one-after-another-in-october
title: "AI Provider Key Custodians Breached One After Another in October"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["provider", "fireworks", "keys", "variables", "says", "opencode", "console", "litellm"]
source: https://www.optimuslabs.io/research/briefings/ai-provider-key-custodians-breached-oct-2026
source_anchor: ""
source_lines: [1, 184]
section: "Securite"
---
An intruder reached a legacy OpenCode Console system and may have exposed provider API keys that teams saved there, for 1.4% of Console users. OpenCode has not said how.

Read
## Summary

Live incidents, as of Nov 10, 2026. On Oct 6, Fireworks found that someone had used internal credentials to read environment variables, including API keys, in a limited number of customer accounts. On Oct 17, OpenCode said a breach of its hosted Console may have exposed provider keys for 1.4% of Console users. On the morning of Oct 21, a poisoned LiteLLM release went out through PyPI and Docker with a credential stealer built to take the provider keys a proxy stores. The same day, Railway said someone may have read decrypted customer environment variables and paused all deployments. In its full incident report on Nov 10, Railway says the attacker escaped its build isolation, used a stolen copy of its source code to find a misconfigured cloud login, and decrypted variables for up to 1.85% of workspaces while hunting for AI provider keys to sell for distillation. LiteLLM's attack chain starts with a master key committed to a public repository on May 27 and never revoked. Fireworks' update adds billing data, datasets and unfinished model weights to its own list, and ties the attack to a group selling stolen model access. No one has published a root cause that links all four. Wiz counts at least nine AI developer platforms that disclosed incidents in October.

## Am I affected?

Any single yes means treat this as an incident.

- Did a host, CI runner or container of yours run litellm 1.107.0, or pull the latest, main-latest or main-stable LiteLLM image, between 11:39 and 14:20 UTC on Oct 21?
- Do you keep API keys or registry credentials in Railway variables, or link a GitHub account or BYOK agent key to Railway, run Fireworks evaluation jobs with secrets, upload datasets or fine-tune models on Fireworks, or save provider keys in OpenCode Console (briefing #13)?
- Did you store provider keys or environment variables on Andromeda, Zeabur, Lightning AI, CommandCode or Northflank, or use NVIDIA Brev, in October?
- Does provider billing since Oct 6 show models, volumes or regions you do not recognise, or calls through global cross-region inference profiles you never use?

Not affected if

- LiteLLM says its private enterprise release and every version before 1.107.0 are safe. Fireworks says no notice means it has not identified your account. Both are the vendors' own claims.

Rotate stored keys at the provider that issued them. Fireworks says removing a stored secret does not invalidate the underlying credential, and the same holds for all four custodians.

## Three hosted platforms and one package

**Fireworks**, **OpenCode** and **Railway** are hosted platforms that store customer secrets, and an intruder reached each one. Fireworks says internal credentials were used to capture evaluation-job variables and that it found no access to customer models, datasets or inference traffic. OpenCode's Console was reached through a legacy system being retired. Railway says the attacker escaped its build isolation and later decrypted stored variables for up to 1.85% of workspaces.

**LiteLLM** is different. It is the open source gateway many teams run themselves, and the poisoned release ran inside their own environments. It reads the proxy's provider-key table directly, takes cloud credentials, SSH keys and environment variables, and installs a backdoor that checks in hourly.

## What Fireworks found later

Fireworks' update widens its Oct 9 bulletin. Beyond stored secrets, the attacker reached an internal application that exposed resource metadata and billing data: addresses, invoices, payment history, balances and credit usage. Fireworks says it does not store card numbers. For a limited set of accounts, secrets, datasets and unfinished model weights were also accessed.

On Oct 23 a third-party cloud provider that hosted Fireworks research machines had its own incident. On Oct 24 the attacker used it to reach Fireworks development machines and run a workload that touched more customer data. Fireworks says it has seen no further successful access to its customer platform since Oct 25, found no impact to Inference, Training SDK or Nexus, and has switched off self-serve dataset uploads and downloads while the investigation, with Palo Alto Networks Unit 42, continues.

**Our read:** the second way in was a vendor's vendor. The inventory you keep may list Fireworks; it does not list the cloud provider under Fireworks' research machines, which is where this part of the breach started.

## How the LiteLLM release was taken

LiteLLM's attack chain starts on May 27, when a config file committed to a public repository held the master key to its internal LiteLLM Agent Platform. The commit was force-pushed over, but the key was never revoked. On Oct 10 the attacker used it to read the Agent Platform, which returned a GitHub CLI token belonging to an organization owner. LiteLLM says that token was not scoped down and skips GitHub's re-authentication for sensitive actions.

With owner access, the attacker pushed throwaway workflows that posted two maintainers' tokens, stored as CI secrets, to the attacker's endpoint, and took tokens for other internal systems, with a focus on upstream inference API keys. Release approval had been relaxed that week from a device-locked account to any two engineers, so one token triggered the release and another approved it; a third deleted the run records. The build ran from a look-alike repository, Berri-AI instead of BerriAI, and the official source tag stayed clean. JFrog notes that reading the source would have missed it.

Hijacked publishing access is a repeat pattern in our own briefings: in arrayref, a compromised maintainer account pushed malicious crates to crates.io.

## How Railway's variables were taken

Railway's full report describes two ways in. On Oct 20 at 07:28 UTC, the attacker used a previously unknown flaw to partly escape a shared build machine and read a credential meant only for pushing build images. Railway's internal Temporal cluster, which runs build tasks, accepted that credential as a trusted client. Through it the attacker read the stored histories of 353 other builds, sealed variables included, and pulled a copy of Railway's internal monorepo from the build cache.

Railway revoked the credential that evening. The attacker searched the copied code for other routes and probed endpoints unused for months. At 03:26 UTC on Oct 21 it found one: a misconfigured OIDC binding that issued a short-lived cloud credential. That led to an SSH key and an orchestration machine holding a long-lived credential that could decrypt customer variables. The attacker queried the variables database for names matching AI provider keys, favouring environments with many variables, and decrypted a subset. A second query matched cloud, database and payment credentials.

Railway puts the upper bound at 1.85% of workspaces, about 100,000, because its logs identify environments, not single variables. Linked GitHub OAuth and BYOK agent tokens for 1,852 users were also decrypted and have been reset. Railway estimates about $740,000 in unauthorized spend and says it will cover what providers do not refund. It alerted providers before customers, and says it will now set a fixed window after which everyone is told, confirmed impact or not.

**Our read:** Railway already used envelope encryption, with per-environment keys wrapped by a root key that never leaves its cloud provider's key service. That did not stop this, because one machine held a standing credential allowed to call decrypt. A build credential trusted beyond its job, a misconfigured login and old endpoints left in the code did the rest. Railway's fixes, a dedicated virtual machine per build, decryption limited to one secrets service with every call logged, and full retirement of deprecated systems, match practices 5 and 6 below.

## Why the middleman holds the prize

OpenCode and Railway tie their breaches to distillation, the pulling of a strong model's answers at scale to train a cheaper copy. Anthropic says distillers often use stolen API credentials from legitimate customers. Railway's full report says the attacker used abliterated models, and that misuse customers reported, from coding-agent requests to grading of AI training tasks, fits keys passed to services that relay or resell AI access. Railway has not established whether the attacker ran those services. LiteLLM says the attacker's activity focused on upstream inference API tokens. Fireworks links its breach to a financially motivated group selling stolen model access to illicit routing gateways, known as LLMJacking; Google's Nov 2 LLMJacking report describes the practice but does not attribute these incidents. No source links the four incidents to one actor.

**Our read, not the vendors':** a gateway or platform holds keys for many customers in one place, so one intrusion yields a pile of paid, aged accounts with normal usage history. That is plausible, not proven. Stored tokens as the way in is not new: in Salesloft Drift, attackers stole the OAuth tokens a vendor held for customers and used them against customer CRMs.

## Part of a wider October

Wiz counts at least nine AI developer platforms that disclosed incidents in October: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank. Fireworks, OpenCode and Warp describe an "agent swarm" attack, and OpenCode and CommandCode attribute theirs to distillation. Root causes differ: credential access at Fireworks, Zeabur and Lightning, build infrastructure at Railway, a third-party dependency at Warp, a legacy system at OpenCode. Every public report describes attackers hunting stored environment variables, especially AI access. Zeabur is shutting down.

Warp's CEO said on Oct 26 that an agent swarm, likely starting with a hole in a vendor dependency, reached a limited amount of agent trace and telemetry data, with an upper bound of roughly 1% of users. Warp has not reported provider keys taken, and is working with CrowdStrike and law enforcement.

NVIDIA Brev may be a tenth. International Cyber Digest reports that NVIDIA told users of unauthorised activity from Oct 4 to 20 that may have exposed the SSH keys Brev set up for their VMs. We have not seen NVIDIA's notice, so treat this as reported, not confirmed. Andromeda, Zeabur, Lightning AI, CommandCode and Northflank have published less, and no single vulnerability links them.

## The other October incidents

Five more platforms disclosed incidents in October, per Wiz. Details are thin, and the dates are Wiz's.

**Andromeda** (Oct 9): its incident report calls the breach part of an "industry-wide attack".

**Zeabur** (Oct 14): its write-up puts the root cause at credential access and names attacker address 23[.]106[.]46[.]239, active Oct 12 to 14. Zeabur is shutting down because of the incident.

**Lightning AI** (Oct 15): its notice also points to credential access. Wiz notes Lightning was hit earlier in the Mini Shai-Hulud campaign.

**CommandCode** (Oct 28): its post on X ties the incident to an attempt at distillation, as OpenCode did.

**Northflank** (Oct 30): disclosed on its status page. No root cause is public yet.

**NVIDIA Brev** (reported, not confirmed): see above. NVIDIA has not published a notice we can check.

If you stored keys or environment variables on any of these, treat them as exposed and rotate them at the provider that issued them.

## What happens to a stolen key

Wiz followed two enterprise AWS Bedrock keys it ties to one of these vendor breaches. Each was tested within 30 minutes of the theft by a single model call from a residential address, in a one-minute sweep across several organizations. A small checker then pinged both keys every day to confirm they still worked.

Four days later, the first key was flooded: about 800,000 Bedrock calls in 30 hours, mostly Opus models. Its policy allowed global cross-region inference profiles, so only about 6% of calls were throttled. The second key was restricted to US profiles. A reseller pool failed for hours until a checker found the working model IDs, then one address made about 1.7 million calls in 13 hours, with most of the rest throttled by regional quotas. Wiz estimates over $100,000 in usage at list price across the two.

Wiz traced checks on both keys to two open resale sites selling discounted frontier-model access, one at an address Zeabur named in its own breach. Wiz cannot tie either site to the original theft. **Our read:** key scope is a blast-radius control. The restricted key still leaked, but it cost the attacker a day and ran into quotas.

## LiteLLM was already hit in March

On Mar 24, versions 1.82.7 and 1.82.8 reached PyPI for about 40 minutes after attackers took LiteLLM's publishing token through a poisoned Trivy scanner (Cloud Security Alliance). CloudSEK later counted more than 2,500 organizations in the captured data, which means potential exposure, not confirmed compromise. JFrog says the October release uses different payloads and infrastructure and that no evidence links it to the March actor. Both went after the same asset: the keys a gateway holds.

## How it maps to OWASP

In the Agentic Applications Top 10, ASI04 Agentic Supply Chain Vulnerabilities covers LiteLLM and the hosted custodians, and ASI03 Identity & Privilege Abuse covers the stolen keys. In the LLM Applications Top 10, LLM02 Sensitive Information Disclosure names API keys as protected information, and LLM06 Unbounded Consumption covers what stolen keys are used for, including distillation theft.

*The OWASP GenAI Security Project does not endorse any product, service or tool, including Optimus Labs. We cite its lists to classify these incidents.*

## If you hold other people's keys

Provider keys are crown jewels.

Any vendor that stores a customer's API key for them (a model gateway, an agent platform, an observability or CI tool) is one intrusion away from these same incidents. Only Railway has described how its stored keys were protected (see above). For the other three, none of this is a finding about them. It is the path we would expect a vendor to follow, drawn from OWASP and NIST key-management guidance.

LiteLLM adds a question for any vendor: who can change what you ship? **Our read:** keep an organization owner's token out of developer tooling, require approval from an account not reachable the same way, and copy workflow logs where an account cannot delete them. LiteLLM's chain also shows that a leaked key which was "cleaned up" but never revoked is still live: practices 6 and 8 apply to your own keys too. Environment variables are a favorite place to look: in the Coder registry hijack, poisoned modules scraped credentials from environment variables, config files and shell history, and no CVE was assigned. The eight practices below cover the stored keys themselves.

1. Hold less ### 1. Store lessUse short-lived, scoped credentials or delegated access wherever a provider offers them. Many model providers still issue long-lived static keys, so a gateway often has to store them. Where the design allows, pass a key through for one request without saving it.
2. At rest ### 2. Use envelope encryptionA unique data key per credential, AES-256-GCM, with the data key wrapped by a master key in a KMS or HSM. OWASP says not to keep encryption keys next to the secrets they protect unless those keys are themselves encrypted.
3. ### 3. Separate customersOne master key per customer, and bind each ciphertext to its customer and record so a copied blob will not decrypt in another row. Deleting a customer's key then makes their stored credentials unrecoverable.
4. ### 4. Make stored keys write-onlyNo reveal button and no export. Show the last four characters and the date it was set. Never log a key: strip authorization headers from logs and error reports.
5. In use ### 5. Decrypt per use, and watch itDecrypt only for the outbound call, wipe the plaintext from memory, and keep any cache short. Log every decrypt and alert on spikes, so a bulk pull stands out.
6. Beyond the vault ### 6. Retire systems completelyBefore a system is decommissioned, move or delete the credentials it holds and confirm they are gone. Any system still holding live keys is still in scope.
7. ### 7. Give customers the brakesPer-key spend caps that stop traffic, model and IP allowlists, last-used time and source for every key, and plain revoke steps for each provider.
8. ### 8. Plan the breach before it happensKeep a per-customer list of stored keys by last four characters, agree a revocation contact with each provider ahead of time, and tell customers in hours, not days. Anthropic and Google acted on Oct 17, within about a day of the first alert, and a pre-agreed route makes that routine.

## What we still don't know

As of Nov 10, 2026, unpublished: Railway's indicators of compromise, which it shares privately until law enforcement clears release, the third-party software flaws it is disclosing upstream, how the Fireworks intruder got the internal credentials, which cloud provider under Fireworks was breached, whether the four (or the nine Wiz counts) share an actor, who runs the resale services Railway and Wiz describe, and how many of the stolen keys are in use today.

## Remediation

- Rotate keys at the issuing provider (Anthropic, OpenAI, Google and others), not only in the custodian's dashboard, and check billing since Oct 6.
- LiteLLM: isolate any host or container that ran 1.107.0. Do not start with pip, because any Python start re-runs the startup hooks. Delete cook.pth, cook.py and sitecustomize.py, rebuild from verified artifacts, and rotate everything the host could reach: proxy provider keys, master and virtual keys, DATABASE_URL, the IAM role and every environment secret.
- Railway and Fireworks: follow their notices. Railway recommends that every customer who signed up before Oct 21 rotate keys stored on Railway, including environment variables and registry credentials, and asks customers to report charges they did not make; it says it will cover stolen-key spend that providers do not refund. Fireworks says to rotate the third-party credentials named in your notice; its update says customers with affected datasets or model weights are being notified, with questions to security-notice@fireworks.ai.

- Block and search for www[.]litellm[.]dev and the GitHub dead drop in the indicators below. The marker /tmp/.pc_once_72129118c59f5636c0bb proves exfiltration if present, but its absence proves nothing.
- Check cloud audit logs for the instance role being used from outside your infrastructure after install time. LiteLLM suggests starting with CloudTrail GetCallerIdentity.
- Railway customers who linked GitHub: review your GitHub audit log for Railway app activity from Oct 20 to Oct 22 that your team did not trigger. Railway has reset the 1,852 linked tokens it says were decrypted.
- AWS Bedrock: search CloudTrail for InvokeModel from unfamiliar addresses, Bedrock or EC2 quota listing, and PutAccountDataRetention calls. Wiz saw all three on stolen keys.

- Pin gateways and base images by digest. Do not pull a mutable tag such as latest, and verify the exact wheel, not only the source tag.
- Keep provider keys in a secrets manager you control, one key per tool, with a hard spend cap that stops traffic. On Bedrock, deny global cross-region inference profiles unless you need them.
- Ask every vendor that holds your keys how they are encrypted, how releases are approved, and how fast you will be told about a breach.

## Indicators

- `Malicious package: litellm 1.107.0 (PyPI, all seven wheels). Last safe version 1.106.2.`
- `Docker tags on Oct 21: v1.107.0, latest, main-latest, main-stable (digest sha256:1bbcb1443bc962e45f5aad53fe3d2c30600e0379715fb6c664cdd3ac126c339e)`
- `Files in site-packages: cook.pth, cook.py, sitecustomize.py`
- `Exfiltration: hxxp://www[.]litellm[.]dev/healthcheck. Dead drop: raw[.]githubusercontent[.]com/letters9813/pythonclient-test/main/domain.txt`
- `Key-checking addresses reported by Wiz (not tied directly to the original breaches): 23[.]106[.]46[.]239, 38[.]246[.]114[.]118, 104[.]243[.]32[.]50, 31[.]57[.]201[.]201`
- `Lock file /tmp/remote_code_loader.lock. Look-alike source in build provenance: Berri-AI/litellm (legitimate: BerriAI/litellm)`

## Sources

## Sourcing & confidence

Confirmed by the vendor

- The vendors' own statements: Railway, its preliminary report and its full incident report, LiteLLM and its attack chain, Fireworks and its update, whose forensics are assisted by Palo Alto Networks Unit 42, and OpenCode.
- Technical analysis of the LiteLLM release by JFrog, Socket, Wiz and Endor Labs, and reporting by The Hacker News and The Register.

Assessed by Civilizations

- That the four incidents form one pattern aimed at provider keys is our read; Wiz also groups them, with five more, as one month of attacks. OpenCode and Railway name distillation, and Railway calls its incident one of a series of attacks on platforms that hold AI keys. No source links the four to one actor.
- Counts of exposed accounts, keys and organizations are the vendors' or researchers' figures and are not independently verified. Railway's figures are upper bounds from its own logs, and its forensics firm's investigation is ongoing.

## Related briefings

The target was the registry's build service, not the people who use it. Publishing a gem triggered code execution on RubyDoc.info without anyone running gem install.

Read
Coder's own delivery infrastructure was hijacked to serve credential-stealing modules from the real registry.coder.com. No CVE, no poisoned package, nothing for a scanner to match.

Read
Full advisory

## Download the briefing PDF

The complete advisory deck for this incident, with the attack chain diagram, indicators, and remediation bands in one file.

Optimus Labs · Civilizations

## Get the next briefing first

Threat research, disclosures, and practical tips on enterprise Agentic AI attack surface management, directly in you or your agent's inbox.

Subscribe
