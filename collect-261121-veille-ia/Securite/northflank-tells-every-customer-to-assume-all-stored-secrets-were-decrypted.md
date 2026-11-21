---
id: collect-261121-veille-ia/securite/northflank-tells-every-customer-to-assume-all-stored-secrets-were-decrypted
title: "Northflank Tells Every Customer to Assume All Stored Secrets Were Decrypted"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "northflank", "variables", "reported", "been", "secrets", "environment", "keys"]
source: https://www.tech-quire.com/articles/northflank-environment-variables-decrypted-ai-api-keys/
source_anchor: ""
source_lines: [1, 45]
section: "Securite"
---
Northflank is a cloud application hosting platform that lets developers deploy and manage applications, containers, databases and AI applications without handling the underlying infrastructure. On 30 October 2026, the company told customers to rotate environment variables and API keys immediately after receiving a threat alert from cloud security firm Wiz. BleepingComputer reported on 31 October 2026 that Northflank then went further, telling every customer to assume that the environment variables stored on its platform had been decrypted and that the secrets inside them had been stolen.

Environment variables commonly hold database passwords, cloud service credentials, authentication tokens and API keys. BleepingComputer reported on 31 October 2026 that exposure of those values can allow an attacker to reach other customer services, leading to data theft or unauthorised charges. A single leaked set of credentials can become a route into connected cloud accounts, model providers and billing systems.

The warning arrived during a month of similar disclosures. Wiz reported on 30 October 2026 that at least nine AI developer platforms disclosed incidents during October: Fireworks AI on 6 October, Andromeda on 9 October, Zeabur on 14 October, Lightning AI on 15 October, OpenCode on 16 and 17 October, Railway on 20 October, Warp on 23 October, CommandCode on 28 October and Northflank on 30 October. Wiz described the period as a Black October for AI developer platforms.

## Key Facts

Northflank's incident page gives a precise timeline. The status page recorded on 30 October 2026 that the incident was opened at 13:40 UTC under the instruction to rotate environment variables immediately. The timeline begins at 11:43 UTC with a threat alert from Wiz. Northflank engineers reviewed it at 11:44 UTC, sent an initial network policy patch at 12:02 UTC, issued further patches at 12:50 and 13:05 UTC, and requested Wiz's incident response team at 13:00 UTC. At 13:58 UTC, the company published the defining line: assume all environment variables have been decrypted, and disable or rotate all keys.

At 15:12 UTC, secret rotation for Postgres and MongoDB add-ons had been enabled for all users, and Northflank warned that multiple users were confirming that their AI keys had been used. It listed the affected scopes as secrets on workloads, secret groups and global secrets. At 23:38 UTC, the status page recorded that users had reported unauthorised use of AI API keys supplied to the Northflank platform. The company told customers to revoke credentials with the issuing providers, generate replacements and review AI provider usage and billing logs. It said the reports may fit the broader pattern of attacks on AI credentials and linked Wiz's Black October report, without claiming one group carried out every incident.

Response work continued into the next day. Northflank's status page reported on 31 October 2026 that audit logs had been enabled for all customers, including free users, and that secret rotation had been extended to MySQL, Redis, Minio and RabbitMQ add-ons. AI Brief reported on 1 November 2026 that customers were told to revoke and replace credentials stored in workloads, secret groups and global secrets, review AI provider usage and billing logs, report suspicious activity and disable public access to add-ons where enabled. Northflank's page also listed the platform as a major outage across seven components.

Wiz's account adds the wider campaign detail. Wiz reported on 30 October 2026 that all public reports described targeted exploitation that explicitly hunted for stored environment variables, with a focus on variables granting AI access. It said there was no single vulnerability or dependency linking the attacks. The pattern instead aligned with agentic attacks and LLMJacking, with stolen keys sold through LLM broker services and demand created by distillation. Zeabur is shutting down because of its incident, while Railway's preliminary report points to privilege escalation in its build infrastructure.

Wiz observed downstream abuse in two enterprise cases. In Case A, an actor made approximately 800,000 Amazon Bedrock calls over 30 hours before remediation, about 90 percent of them successful, leaning on Opus 5 at 36 percent, Opus 5.5 at 22 percent and Opus 4.8 at 10 percent. Routing through global cross-region inference profiles kept throttling to about 6 percent. In Case B, a single Level 3 address made approximately 1.7 million Bedrock calls over 13 hours, more than a million of them successful. Wiz estimates that the usage likely ran up over $100,000 at list price. In both cases, the first unauthorised activity appeared within 30 minutes of the reported credential access, beginning with a single InvokeModel call from a residential IP that tested stolen keys across multiple organisations, requested Claude Fable 5.1, failed and fell back to Opus 5.5.

## Analysis

What this really means is that the exposed environment variable has become the weakest link in the AI supply chain. The incident is a story about a hosting layer that holds credentials for many other services and model providers. When that layer is compromised, the attacker does not need to break into every customer individually. The stolen keys do the work, and the victim's own cloud and AI accounts absorb the cost.

The timeline also carries a warning about detection. Northflank's response began with an external alert from Wiz at 11:43 UTC, not with an internal discovery. More than two hours passed before the company told customers to assume decryption. Network policy patches went out at 12:02, 12:50 and 13:05 UTC. That is a reasonable emergency sequence, but it shows how quickly a platform incident becomes a customer credential incident. The gap between the alert and the most severe public instruction is the window in which customers could still have been using keys that were already exposed.

The response has since widened, but gaps remain. Audit logs were only enabled for all customers on 31 October, the day after the incident became public. Secret rotation started with Postgres and MongoDB add-ons and was later extended to MySQL, Redis, Minio and RabbitMQ. Customers had to revoke credentials at the issuing providers, not merely inside Northflank, because platform rotation cannot invalidate a key that still works at the provider. Northflank has not said how attackers gained access or how many customers were affected. Wiz has also not established that the same attackers carried out all nine incidents. The common thread is the target class, not necessarily the intrusion method.

## Why It Matters

AI API keys are high-value credentials. The two Wiz cases show how quickly stolen access can turn into hundreds of thousands of model calls and a bill above $100,000 at list price. For an enterprise, that can mean unauthorised charges and a review across every cloud account that trusted the stolen key. The first unauthorised call came from a residential IP and fell back from one model to another, which shows that the attackers were probing for working access rather than stumbling onto it.

The incident also changes the trust equation for developer platforms. Northflank's value proposition is that developers can deploy applications, containers, databases and AI applications without managing infrastructure. That convenience concentrates secrets in one place. When the platform tells every customer to assume decryption, it is asking them to rebuild their trust boundary under time pressure. The later addition of audit logs and broader rotation support is necessary, but these controls were not uniformly available at the moment of crisis.

The wider market signal is concentration risk. Nine platforms in one month is not a series of isolated mishaps. It is evidence that attackers have found a repeatable target in AI developer infrastructure. Zeabur's decision to shut down because of its incident shows that the consequences can reach business continuity, not just security operations. Railway's preliminary finding of privilege escalation in build infrastructure points to another route into the same credential layer. The objective appears consistent: find stored secrets, use the AI ones and monetise the access before defenders rotate.

## Next Up

The next disclosure to watch is Northflank's account of how the intrusion happened and how many customers were affected. The company has not provided either detail. Until those facts are published, customers should treat the incident as a full credential compromise, review provider-side usage and billing records, and confirm that every replacement key is scoped to the minimum necessary permissions.

The test is whether the October cluster produces stronger defaults rather than another emergency rotation. Wiz has linked the pattern to agentic attacks, LLMJacking and broker services that resell stolen model access. Defenders therefore need to watch not only for unusual login locations but also for sudden changes in model mix, failed calls followed by fallbacks, and spend that does not match application traffic. The remaining question is whether audit logging and rotation become standard across the AI developer platforms that hold the same kind of secrets.

## Comments (0)

Log in or sign up to leave a comment.

No comments yet. Be the first to share your thoughts.
