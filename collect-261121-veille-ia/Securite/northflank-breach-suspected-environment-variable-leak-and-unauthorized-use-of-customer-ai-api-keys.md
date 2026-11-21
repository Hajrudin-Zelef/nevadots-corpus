---
id: collect-261121-veille-ia/securite/northflank-breach-suspected-environment-variable-leak-and-unauthorized-use-of-customer-ai-api-keys
title: "Northflank Breach: Suspected Environment Variable Leak and Unauthorized Use of Customer AI API Keys"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["northflank", "keys", "environment", "unauthorized", "customers", "secrets", "cloud", "related"]
source: https://dev.to/anoymask/northflank-breach-suspected-environment-variable-leak-and-unauthorized-use-of-customer-ai-api-keys-367l
source_anchor: ""
source_lines: [1, 114]
section: "Securite"
---
- Incident Name: Security incident
- Publisher: Northflank
- Publication Date: 2026-10-30
- Original Source: Northflank
- Related Source: Northflank security breach leads to abuse of customers' AI API keys
- Related Malware: None specified
- Related Threat Group: None specified
- Related CVE: None specified
- Related Products: Northflank Platform, AI provider API keys, PostgreSQL, MongoDB, MySQL, Redis
- Severity: critical

Northflank notified all customers to assume that their stored environment variables were decrypted and compromised, and received reports from multiple customers regarding the unauthorized use of AI API keys. The initial access vector, the number of affected customers, and the abuse of non-AI credentials remain publicly undisclosed.

All secrets stored in Northflank must be treated as compromised. Unauthorized use of customer AI API keys has been reported, while exposure of other secrets could enable further compromise of cloud, database, and SaaS environments and financial losses.

1. Wiz notified Northflank of a threat alert, prompting Northflank to apply a network policy patch and additional patches. The initial intrusion and acquisition methods are undisclosed.
2. Northflank treated all environment variables as compromised and requested that customers rotate all secrets.
3. Multiple customers reported unauthorized use of AI API keys stored in Northflank. The abuse of non-AI credentials and unauthorized data access have not been confirmed.
4. The company enabled audit logs and database secret rotation for all customers and continued its investigation.

- The attacker's initial positioning and privileges within Northflank are undisclosed. At the stage of using leaked credentials, an attacker could authenticate to external AI, cloud, database, and SaaS providers using valid API keys or other exposed credentials.

- End-user screen changes are not expected, but increases in AI provider usage and billing, as well as service disruptions, may occur.

- Indications include Northflank incident notifications, requests to rotate environment variables and secrets, unfamiliar IP addresses, models, or requests at AI providers, billing spikes, and abnormal authentication events across databases, cloud platforms, and SaaS.

- Valid environment variables and secrets are stored in Northflank, and the attacker is able to acquire or decrypt them.
- The leaked keys have not been revoked by the issuer, and their scope, quota, and network policies permit abuse.

- Revoke all secrets stored in Northflank at their issuing source and update workloads after reissuing them. Do not leave old keys active simply by overwriting values.
- Restrict AI, cloud, database, and SaaS keys to minimal scopes, short lifetimes, and IP/network limitations, equipped with quota and billing alerts.
- Disable public access to Northflank addons and investigate usage during the leakage period via provider-side audits.

- Quota consumption, billing charges, and provider account abuse due to the unauthorized use of AI API keys.
- Potential chained compromises if environment variables for databases, cloud platforms, SaaS, or CI/CD pipelines were acquired. The success of the latter has not been confirmed.
- Operational impacts associated with rotating all secrets and redeploying services.

Inference: The following are candidate logs to investigate within your organization. This depends on logging and retention configurations as well as product visibility, and does not mean all items were recorded in the primary source.

- 
**Email** : Check for incident, usage, billing, and credential notifications from Northflank and AI/cloud providers.
- 
**Proxy / SWG / DNS** : Check for normal traffic from Northflank workloads and abnormal destinations before and after rotation.
- 
**Endpoint / EDR** : No relevant information.
- 
**Identity / IdP** : Check Northflank administrator logins, role and secret modifications, API token usage, and unfamiliar IP addresses or devices.
- 
**SaaS / Cloud** : Check AI provider usage and billing, cloud audit logs, database connections, SaaS APIs, and CI/CD secret usage on a per-key basis.
- 
**Network** : Check for public access to databases and addons, connections from unknown sources, and changes in egress traffic.

- 
**Data or session compromise confirmed** : Northflank received reports from multiple customers of unauthorized AI API key usage and instructed users to treat all environment variables as compromised. The extent of actual environment-variable theft has not been confirmed. Scope: AI API keys of the reporting Northflank customers.

- 
**Follow-on compromise** : Look for evidence of unauthorized AI provider requests or charges, or use of pre-rotation secrets at other providers. These are investigation criteria; public information is limited to reports of unauthorized AI API key usage. Scope: External providers used by customers.

Inference: These are internal investigation procedures.

- 
**Trigger** : Northflank security incident notification, unfamiliar AI usage, billing spikes, rotation requests.
- 
**Initial Checks** : Inventory all secrets in Northflank workloads, secret groups, and global secrets, and check each credential’s issuer, scope, last rotation date, and owner.
- 
**Endpoint** : Verify whether the same secrets remain on CI/CD hosts, local shells, IaC states, or deployment logs that submitted secrets to Northflank.
- 
**Identity and Cloud** : Investigate Northflank administrative logs alongside audit logs and billing data from AI, cloud, database, SaaS, CI/CD, webhook, and signing providers.
- 
**Follow-On Activity** : Track tokens, resources, users, and deployments created using leaked keys, along with database sessions and data access enabled by those keys.
- 
**Containment** : Revoke old secrets at their issuers, issue new secrets with minimal scopes, and update and redeploy Northflank workloads. Stop public addon access and isolate unauthorized resources.
- 
**Classification** : Document findings by separating the precautionary assumption of compromise, confirmed key usage, resource creation, data access, financial losses, and follow-on compromise.

- 
**Single Event** : Detect AI API key usage by unfamiliar IP addresses, user-agents, or models, sudden quota consumption, and Northflank administrator operations.
- 
**Time Series** : Correlate across providers the timeline of the Northflank incident, secret usage, resource creation/data access, and billing changes.
- 
**Threat Hunting** : Search provider logs around October 30 using secret hashes or key IDs that were stored in Northflank.
- 
**Logging Gaps** : Relying solely on Northflank logs may cause key usage at issuers to be overlooked, and historical usage of old keys may become obscured after rotation.
- 
**Priority Controls** : Prioritize revoking and reissuing all secrets, enforcing minimal scopes, short lifetimes, network restrictions, quota/billing alerts, and secret inventories.

- Northflank announced that it received a threat alert from Wiz on October 30, 2026, at 11:43 UTC, applied a network policy patch at 12:02 UTC, and applied additional patches at 12:50 and 13:05 UTC.
- The company requested that all customers assume all environment variables were decrypted and compromised, and urged them to revoke and update all credentials stored in workloads, secret groups, and global secrets.
- Northflank announced that it received reports from multiple users regarding unauthorized use of AI API keys stored on the platform. These are customer observation reports; the company has not disclosed individual providers, affected counts, or billing amounts.
- The company enabled audit logs for all customers and provided secret rotation for PostgreSQL, MongoDB, MySQL, and Redis, followed by Minio and RabbitMQ on October 31.
- As of October 31, 2026, at 23:31 UTC, the incident status remained identified/major outage, and the initial intrusion vector, number of affected customers, and credential theft or abuse beyond AI remain undisclosed.
- Northflank indicated that reports of unauthorized AI API key usage may be consistent with the pattern of AI credential attacks reported in Wiz's Black October, but did not conclude they are the same actor or campaign.

- Inference: Verify provider-side audits and billing outside of Northflank for each type of secret stored in environment variables; do not judge leakage solely based on platform-internal logs.

No additional hypotheses. Unconfirmed items are listed under "Unknowns and Limitations".

| ID | Technique | Confidence | Basis | 
|---|---|---|---|
| T1078.004 | Valid Accounts: Cloud Accounts | medium | Stage of abusing API keys or cloud credentials that may have been stolen as legitimate accounts. Customer reports exist for AI API keys. | 
| T1496 | Resource Hijacking | medium | Customer quotas and billing resources may have been consumed due to unauthorized use of AI API keys. The scale is undisclosed. | 

- The initial intrusion vector, threat actor, number of affected customers, scope of acquired secrets, abuse of non-AI credentials, and presence of data exfiltration remain unknown.

Organizations utilizing Northflank must inventory environment variables by secret type, revoke and reissue all credentials—including not only AI providers but also databases, cloud platforms, SaaS, CI/CD pipelines, signing keys, and webhooks—at their issuing sources, and retrospectively review provider-side audit logs and billing data around October 30.

- 
**SOC** : Correlate Northflank audit logs with logins, APIs, and billing across credential issuing sources to determine unauthorized use on a per-secret basis.
- 
**Administrators** : Rotate credentials in all workloads, secret groups, and global secrets, as well as database credentials, and disable public addon access.
- 
**Users** : This is not an issue resolvable by individual operations. Follow instructions from administrators regarding token reissuance or password changes.

## Top comments (0)
