---
id: collect-261121-veille-ia/securite/attackers-hijack-microsofts-agent-framework-package-to-steal-azure-and-ci-credentials
title: "Attackers hijack Microsoft’s agent-framework package to steal Azure and CI credentials"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["azure", "said", "package", "attackers", "credentials", "tokens", "security", "also"]
source: https://www.infoworld.com/article/4250167/attackers-hijack-microsofts-agent-framework-package-to-steal-azure-and-ci-credentials-2.html
source_anchor: ""
source_lines: [1, 47]
section: "Securite"
---
Attackers briefly hijacked Microsoft’s official Agent Framework package on PyPI and used it to distribute credential-stealing malware capable of harvesting cloud tokens, developer secrets and CI/CD credentials from developer workstations and automated build systems, according to multiple security researchers.

Two malicious versions of the package, 1.26.1 and 1.26.2, appeared on PyPI on November 17 and stayed live for less than an hour.

“The package is a credential stealer: it harvests environment secrets, cloud and package-manager credentials, and Azure managed identity tokens, then exfiltrates them to an attacker-controlled webhook,” OX Security, one of several security firms that flagged the releases, said in a blog post.

The payload ran automatically every time Python started, even if no code imported the package, OX said. The malicious versions also installed the genuine framework code, so applications kept working, and the compromise was easy to miss. The package has 192,534 monthly downloads, according to the firm.

Other security firms, including Aikido Security, Endor Labs, and SafeDep, published similar analyses of the attack.

Microsoft has not said publicly how the attacker got access to publish the package. The company did not immediately respond to a request for comment.

## How the stealer worked

The stealer collected environment variables with names containing words such as TOKEN, SECRET, PASSWORD, KEY, AZURE, AWS, GITHUB, and OPENAI, OX said. It also read up to 60 credential files. These included Azure CLI settings, Git and Docker credentials, npm and pip configuration, Hugging Face tokens, Azure DevOps build agent credentials and application .env files. It pulled secrets from up to 300 other running processes as well.

The stealer also asked for new tokens. Inside Azure DevOps pipelines, it requested an identity token that can be exchanged for Azure access, OX said in the blog post. It also requested managed identity tokens for Azure services, including Resource Manager, Microsoft Graph, Key Vault, Storage, Cognitive Services, AI Foundry, and Azure DevOps.

Endor Labs said the attack was aimed mainly at automated build systems. “Developer laptops are in scope, but the payload is written for CI runners and Azure workloads,” the firm wrote in its analysis.

The code was disguised as diagnostics. It sat in a module named _telemetry.py and sent data with a user agent labeled “af-runtime-diagnostics,” OX said. The stolen data went to a webhook.site address as compressed, encoded JSON.

The attacker did not stop after the first release. While OX was researching the malware, version 1.26.1 was reverted to the clean 1.26.0. The attacker then uploaded 1.26.2, which used a different file path for the same payload, the firm said.

The first version went up at 20:35 UTC and the second at 21:08 UTC, according to Aikido Security. Both were gone by about 21:20 UTC.

“This attack isn’t just a one-off exfiltration attempt, but a planned, live and coordinated effort to reach as many victims as they can, while still being able to bypass blocks and restrictions,” OX said.

OX said the attackers appear to have found a way to hijack the process that builds the package’s wheel files. Endor Labs, in its analysis, said both malicious versions were uploaded as wheel files only, without the source code package that came with the clean release. Microsoft has not confirmed how the breach happened.

The short window may not limit exposure. Any unpinned install during that time pulled in a malicious version, Endor Labs said. Container images, CI caches, and lockfiles built in the window could still contain it.

## AI pipelines in focus

OX said the attack shows how attackers are targeting the AI developer ecosystem and the CI pipelines behind it. The stealer’s target list includes OpenAI and Hugging Face tokens and Azure AI Foundry access.

It is also not the first time this year that attackers have compromised an official Microsoft Python package. In May, Endor Labs detected three trojanized versions of durabletask, Microsoft’s Python SDK for Azure Durable Functions. No researcher has linked the two attacks.

## What security teams should do

Removing the package does not undo the damage. “Removing the package stops future runs, but it doesn’t undo anything that was already sent,” Aikido said.

OX advised teams to check all environments, CI images, and lockfiles for either version and remove it. Teams should look for agent_framework_meta.pth and agent_framework_meta/_telemetry.py on hosts and build agents, the firm said. Version 1.26.2 used different files, af_runtime_diag.pth and af_runtime_diag.py, according to Aikido.

If the package ran anywhere, every secret reachable from that host should be treated as compromised, OX said. The firm recommended rotating Azure service principal secrets, personal access tokens, and GitHub, npm, PyPI, NuGet, Docker, Hugging Face, and OpenAI tokens. OX also advised teams to review Azure sign-in and activity logs for use of managed identities and federated credentials from affected build agents. Ox recommended pinning the agent-framework to a verified Microsoft release with hash checking and blocking outbound traffic to webhook.site from build infrastructure.

*The article originally appeared on CSO.*
