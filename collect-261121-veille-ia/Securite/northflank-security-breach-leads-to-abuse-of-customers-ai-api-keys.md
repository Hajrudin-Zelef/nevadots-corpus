---
id: collect-261121-veille-ia/securite/northflank-security-breach-leads-to-abuse-of-customers-ai-api-keys
title: "Northflank security breach leads to abuse of customers' AI API keys"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["northflank", "keys", "customers", "breach", "security", "credentials", "company", "stored"]
source: https://www.bleepingcomputer.com/news/security/northflank-security-breach-leads-to-abuse-of-customers-ai-api-keys/
source_anchor: ""
source_lines: [1, 66]
section: "Securite"
---
Cloud application hosting platform Northflank is warning customers to rotate all credentials and secrets stored on its platform after a security breach led to unauthorized use of customers' AI API keys.

Northflank is a cloud platform that allows developers to deploy and manage applications, containers, databases, and AI applications without managing the underlying infrastructure.

The company disclosed a security incident on October 30, warning customers to immediately rotate their environment variables and API keys after receiving a threat alert from cloud security company Wiz.

Soon after, Northflank told all customers to assume their environment variables had been decrypted and that the secrets stored within them were stolen.

"Assume all environment variables have been decrypted. Disable/rotate all keys," Northflank warned in an October 30 update.

Applications commonly use environment variables to store sensitive information, including database passwords, cloud service credentials, authentication tokens, and API keys.

If exposed, these credentials can be used to breach other customer services, potentially leading to data theft or unauthorized charges through API keys.

Northflank later confirmed that multiple customers had reported unauthorized use of AI API keys stored on the platform.

"Users have reported unauthorised use of AI API keys supplied to the Northflank platform," the company said.

"If you have not already rotated your user-provided secrets stored in Northflank, please do so immediately."

Customers were also advised to review their AI provider usage and billing logs for unexpected activity and report suspicious usage to Northflank and their AI providers.

According to Northflank's published incident timeline, Wiz warned them about the breach at 11:43 UTC on October 30, which the company's engineers reviewed one minute later.

Northflank quickly responded with a network policy patch, followed by additional patches at 12:50 UTC and 13:05 UTC. The company also requested assistance from Wiz's incident response team.

The company has not disclosed how attackers gained access to its systems, how many customers were affected, or whether attackers stole or abused credentials other than the reported AI API keys.

As part of its response, Northflank enabled credential rotation for PostgreSQL, MongoDB, MySQL, and Redis.

The company also enabled audit logs for all customers, including those using free accounts, to help them investigate any suspicious activity.

Northflank continues to advise customers to rotate secrets stored in workloads, secret groups, and global secrets, and to disable public access to add-ons where enabled.

BleepingComputer contacted Northflank with questions about the breach, its impact on customers, and whether stored credentials were accessed or stolen, but has not received a response.

## Black October

The breach comes amid a series of attacks targeting AI development platforms, which security researchers believe are valuable targets because they store API keys that can be used to access expensive AI services.

In a report titled "Black October for AI Developer Platforms," Wiz identified at least nine security incidents affecting AI development platforms during the month, including Northflank, Railway, OpenCode, Warp, and Lightning AI.

The Railway incident also involved potential exposure of decrypted customer environment variables, while OpenCode disclosed exposed AI API keys. Warp reported a breach that exposed AI agent logs and user telemetry data.

Wiz says attackers involved in some of these incidents have specifically targeted environment variables containing AI service credentials, which can be exploited through a practice known as LLMJacking.

LLMJacking involves using stolen credentials to access AI models using another organization's API keys, generating massive API charges or allowing the stolen access to be resold.

In two cases investigated by Wiz, compromised enterprise credentials were used to make approximately 2.5 million Amazon Bedrock API calls, generating approximately $100,000 in AI usage.

The researchers also discovered infrastructure linked to services reselling access to AI models, although they could not link those operations to the initial breaches or large-scale abuse.

Wiz has not confirmed that all nine incidents were conducted by the same attackers. However, many of the incidents involved attempts to steal AI API keys.

Northflank also referenced Wiz's research in its incident updates, noting that customer reports of unauthorized AI key usage may be tied to the attacks seen by the researchers.

Adaptive Security's AI agents analyze email intent, context, and behavior to uncover phishing, business email compromise, and invoice fraud.

See what's getting through with a 14-day read-only scan of Microsoft 365 or Google Workspace, with no MX changes.

Start your free email security scan.
## Post a Comment Community Rules

## You need to login in order to post a comment

Not a member yet? Register Now
