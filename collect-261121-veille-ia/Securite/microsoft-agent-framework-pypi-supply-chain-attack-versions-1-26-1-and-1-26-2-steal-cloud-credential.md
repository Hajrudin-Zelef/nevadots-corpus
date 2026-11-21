---
id: collect-261121-veille-ia/securite/microsoft-agent-framework-pypi-supply-chain-attack-versions-1-26-1-and-1-26-2-steal-cloud-credential
title: "Microsoft Agent Framework PyPI Supply-Chain Attack: Versions 1.26.1 and 1.26.2 Steal Cloud Credentials"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["versions", "credentials", "agent-framework", "pypi", "cloud", "azure", "tokens", "version"]
source: https://orca.security/resources/research/microsoft-agent-framework-pypi-supply-chain-attack-versions-1-26-1-and-1-26-2-steal-cloud-credentials/
source_anchor: ""
source_lines: [1, 73]
section: "Securite"
---
A critical supply-chain compromise was discovered affecting Microsoft’s agent-framework PyPI meta-package, where versions 1.26.1 and 1.26.2 were injected with credential-stealing malware capable of harvesting cloud identity tokens, API keys, and sensitive credentials from developer workstations, CI/CD pipelines, and cloud build hosts. Due to the package’s approximately 190,000 monthly downloads and the breadth of targeted credentials, immediate remediation is required for any environment that installed the compromised versions.

## Agent-framework compromise at a glance

- **Affected package:** agent-framework on PyPI, versions 1.26.1 and 1.26.2
- **Last clean version:** 1.26.0
- **Not affected:** agent-framework-core and 22 other sibling packages
- **Exposure window:** November 17, 2026, 20:35 to approximately 21:20 UTC
- **Payload:** a Python .pth startup hook that harvests credentials on every interpreter launch
- **Exfiltration:** zlib-compressed, base64-encoded JSON posted to a webhook.site URL
- **Status:** malicious versions quarantined by PyPI; no CVE assigned

## What happened in the agent-framework supply-chain attack

Both malicious versions were published within a 33-minute window on November 17, 2026 (20:35 to 21:08 UTC) and removed by approximately 21:20 UTC.

The attack was entirely in packaging. Neither malicious release changed Agent Framework code or pulled in a rogue dependency. Instead, each version added a Python .pth startup hook file that executes on every Python interpreter launch, even if Agent Framework is never imported.

### How the .pth startup hook runs and evades detection

The collector forks a detached background process, sets a flag to prevent re-execution in child processes, and silently harvests credentials. Version 1.26.2 restored the legitimate __init__.py byte-for-byte to evade diff-based detection while moving the payload to a new top-level module.

## What credentials does the agent-framework malware steal?

The malware actively requested fresh credential tokens rather than only copying stored secrets. Targeted credentials include:

- Environment variables matching sensitive substrings (TOKEN, SECRET, KEY, AZURE, AWS, GITHUB, OPENAI, HF_, and more) from the current process and up to 300 other processes via /proc/*/environ
- Azure managed-identity tokens for 12 Azure resources (ARM, Microsoft Graph, Key Vault, Storage, ML, AI Foundry, Container Registry, Azure DevOps) via IMDS and identity endpoints
- Azure DevOps SYSTEM_ACCESSTOKEN and OIDC tokens for federated identity
- GitHub Actions OIDC tokens (version 1.26.2 only) when workflows have id-token: write permissions
- Credential files including Azure CLI state, .npmrc, pip.conf, .netrc, .git-credentials, Docker config.json, Hugging Face tokens, and .env files

All harvested data was exfiltrated via zlib-compressed, base64-encoded JSON posted to a webhook.site URL.

## Which agent-framework versions and environments are affected?

The following components are affected: agent-framework versions 1.26.1 and 1.26.2 on PyPI. The last clean version is 1.26.0. The dependency agent-framework-core and 22 other sibling packages were checked and are not affected.

Any environment that installed the compromised versions during the exposure window is at risk, including through loose version pins such as agent-framework>=1.26. This spans developer workstations, CI/CD pipelines (Azure DevOps, GitHub Actions), and cloud build hosts running Python.

At the time of writing, the malicious versions have been quarantined by PyPI and the Microsoft publisher account has been secured. No CVE has been assigned.

## Why stolen cloud and CI/CD credentials raise the stakes

Successful exploitation allows attackers to obtain cloud identity tokens that grant access to Azure resources, steal CI/CD pipeline secrets that could enable lateral movement across repositories and infrastructure, and harvest stored credentials that could lead to full infrastructure compromise across multiple cloud providers.

The attack also demonstrates sophisticated evasion techniques, including restoring legitimate code to bypass diff-based reviews while hiding the payload in a separate module.

## How to remediate the agent-framework compromise

Organizations should take the following remediation steps immediately:

1. **Isolate** affected hosts and CI runners.
2. **Detect exposure** by searching lockfiles, container images, CI caches, and environments for versions 1.26.1 or 1.26.2, and look for .pth files (agent_framework_meta.pth, af_runtime_diag.pth) and payload modules in site-packages.
3. **Pin** to agent-framework==1.26.0 with hash verification and rebuild from clean environments.
4. **Rotate all credentials** that may have been exposed, including Azure service principal credentials, Azure DevOps PATs and service connections, GitHub tokens, npm/PyPI/Hugging Face tokens, Docker registry credentials, and any secrets in environment variables or .env files.
5. **Audit** Azure Entra ID sign-in logs, Azure DevOps audit logs, and GitHub Actions workflow runs for unauthorized access since November 17.
6. **Hunt for IOCs** by searching proxy and DNS logs for connections to webhook.site, User-Agent strings “af-runtime-diagnostics/1” and “runtime-diagnostics/1”, and bursts of IMDS token requests.
7. **Block** versions 1.26.1 and 1.26.2 at your package proxy and consider blocking webhook.site at the network level.

### Indicators of compromise (IOCs)

- Package versions: agent-framework 1.26.1, 1.26.2
- Files in site-packages: agent_framework_meta.pth, af_runtime_diag.pth
- Network: outbound connections to webhook.site
- User-Agent strings: “af-runtime-diagnostics/1”, “runtime-diagnostics/1”
- Behavior: bursts of IMDS token requests

## How Orca helps detect compromised agent-framework packages

Orca enables customers to quickly identify assets running vulnerable package versions, understand their exposure in context, including internet accessibility, runtime reachability, and asset criticality, and prioritize the risks that matter most. Orca’s SCA (Software Composition Analysis) scanning capabilities can detect compromised packages in cloud workloads. Customers running Orca’s agentless scanning should check whether their workloads contain the affected versions. Orca’s platform highlights affected assets directly, helping security teams focus on the most critical remediation paths first.

Because this payload targets build hosts and pipelines as much as production, coverage has to start before deployment. Orca helps teams catch risky code before it reaches production. And because the real damage comes from what stolen tokens can reach, Orca also helps teams reduce identity-based cloud risk and detect and respond to cloud threats that follow a credential compromise. As agent frameworks become a recurring supply-chain target, securing AI systems means securing the packages they are built on.
