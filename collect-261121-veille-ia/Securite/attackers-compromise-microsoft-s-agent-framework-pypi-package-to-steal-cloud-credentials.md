---
id: collect-261121-veille-ia/securite/attackers-compromise-microsoft-s-agent-framework-pypi-package-to-steal-cloud-credentials
title: "Attackers Compromise Microsoft's Agent Framework PyPI Package to Steal Cloud Credentials"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["azure", "package", "framework", "agent", "python", "microsoft", "credentials", "environment"]
source: https://www.thehackernews.com/2026/11/attackers-compromise-microsofts-agent.html
source_anchor: ""
source_lines: [1, 56]
section: "Securite"
---
Threat actors have compromised Microsoft's **agent-framework** package to push a credential stealer, according to reports from Aikido, Chainguard, Endor Labs, OX Security, SafeDep, Socket, StepSecurity, and Google-owned Wiz.

The malicious versions, 1.26.1 and 1.26.2, were published 33 minutes apart on November 17, 2026, and are designed to run a credential stealer on every Python start. As of writing, both the release versions have been quarantined on the Python Package Index (PyPI) repository. It's unclear how the attacker managed to obtain publishing access.

Agent-framework is the meta-package for Microsoft Agent Framework, an open-source framework for building and orchestrating artificial intelligence (AI) agents using Python.

"Both versions install Python startup hooks that silently collect credentials, environment data, and cloud identity tokens, then exfiltrate them to a webhook[.]site endpoint," Socket said in a report.

Once either of the versions is installed, the package drops a .pth file in the site-packages root directory, which then gets executed automatically on every interpreter start, causing the credential stealer to run whenever Python is started in an environment, even if the package is not explicitly imported.

"Each wheel adds a .pth file to site-packages," Endor Labs said. "Python executes import lines in .pth files on every interpreter start, so the stealer runs on any Python launch in an affected environment, including pip itself, whether or not anything imports Agent Framework."

The names of the .pth files are below -

- agent_framework_meta.pth in version 1.26.1
- af_runtime_diag.pth in version 1.26.2

The "agent_framework_meta.pth" file contains a single import statement referencing "agent_framework_meta," which then launches the stealer payload. It's capable of capturing the following data -

- Environment variables based on substring-based matching
- Credential files associated with Azure CLI (~/.azure/*.json), .npmrc, pip.conf, .netrc, .git-credentials, .docker/config.json, Hugging Face, Microsoft Azure DevOps agents, and .env and JSON files
- Azure DevOps OIDC tokens
- Azure managed-identity tokens
- Azure instance metadata and retains up to 8,000 characters of the response

"Besides reading credentials already on disk, the code asks the platform to issue new ones," SafeDep said. "Rotating stored secrets does not cover those."

"In an Azure DevOps pipeline, SYSTEM_ACCESSTOKEN and SYSTEM_OIDCREQUESTURI are enough to request a federated token for api://AzureADTokenExchange. On Azure compute, it asks the managed identity endpoint for an access token to each of these resources. It uses IDENTITY_ENDPOINT or MSI_ENDPOINT on App Service and Functions, and 169.254.169[.]254 elsewhere. It also pulls instance metadata."

It's worth noting that the Azure instance metadata service (IMDS) is a RESTful web service that runs on a local IP address ("169.254.169[.]254"), and provides information about the virtual machine, its region, tags, and the identity token.

All the collected information is saved into a JSON document, zlib-compressed, Base64-encoded, and sent via an HTTPS POST request to a hard-coded URL ("webhook[.]site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb").

Version 1.26.2, on the other hand, imports a new top-level module, af_runtime_diag.py, collects GitHub Actions OIDC tokens, and expands the search patterns used for harvesting credential-related environment variables.

Given that the package attracts about 190,000 downloads a month, users are advised to perform the following steps -

- Isolate affected hosts and runners
- Pin the package to the most recent safe version (1.26.0) and restore from a clean environment
- Check for exposure to the malicious versions between 8:35 and roughly 9:20 p.m. UTC on November 17, 2026
- Search proxy and DNS logs for requests to webhook[.]site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb
- Rotate exposed credentials from a clean system
- Audit Azure identities and review sign-in and activity logs
- Audit GitHub Actions workflows that deploy to Azure

"The malicious agent-framework shows how attackers are targeting the AI developer ecosystem and the CI pipelines behind it," OX Security said. "While directly targeting an official Microsoft package, it seems that attackers found a way to hijack the wheel creation flow to add malicious logic inside."

Agent-framework is the second Microsoft-related PyPI package to be compromised this year. In May 2026, the threat actors behind TeamPCP pushed three malicious versions of durabletask, Microsoft's Python client for the Durable Task workflow execution framework, to distribute the Mini Shai-Hulud worm.

### Update

Microsoft has taken the step of making some of its repositories private, including "microsoft/agent-framework" and "microsoft/onnxruntime" as it continues to investigate the incident.

"We have made some repositories private while we investigate malicious content," a Microsoft spokesperson told The Hacker News. "Our priority is to protect customers and the broader ecosystem. We are working to review and will take further action as needed."

*(The story was updated after publication on November 20, 2026, to include a response from Microsoft.)*
