---
id: collect-261121-veille-ia/microsoft-actu/microsoft-agent-framework-pypi-package-hijacked-to-steal-credentials-cybersecurity
title: "Microsoft Agent Framework PyPI Package Hijacked to Steal Credentials - Cybersecurity"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["azure", "package", "pypi", "version", "tokens", "malicious", "versions", "agent-framework"]
source: https://dailysecurityreview.com/?p=45937
source_anchor: ""
source_lines: [1, 37]
section: "Microsoft_Actu"
---
Attackers published two malicious versions of Microsoft’s “agent-framework” meta-package on PyPI on November 17, planting credential stealers that target Azure and CI/CD secrets. The package draws about 190,000 downloads per month, and PyPI has quarantined both versions.

## Two Malicious agent-framework Releases Published 33 Minutes Apart

Versions 1.26.1 and 1.26.2 of the package went live 33 minutes apart. The first appeared at about 8:35 p.m. UTC, and the exposure window runs to roughly 9:20 p.m. UTC. How the attacker obtained publishing access is unclear. OX Security says the attackers appear to have hijacked the wheel creation flow to add the malicious logic.

Security vendors Aikido, Chainguard, Endor Labs, OX Security, SafeDep, Socket and Wiz all published analyses of the incident.

Multiple security vendors published their analyses on November 18, and the quarantine of both versions followed the same day. Because the package is a meta-package that draws about 190,000 downloads per month, the short exposure window still matters for any pipeline that resolves the latest release automatically.

### How the .pth File Runs the Stealer at Every Python Launch

Each malicious wheel drops a .pth file into site-packages: agent_framework_meta.pth in version 1.26.1 and af_runtime_diag.pth in version 1.26.2. Python executes import lines in .pth files every time the interpreter starts. The stealer therefore runs on any Python launch in the environment, including pip itself, even if the package is never imported.

### What the agent-framework Stealer Collects From Azure and CI Hosts

The malware gathers environment variables matched by substring and credential files for the Azure CLI, .npmrc, pip.conf, .netrc, .git-credentials, .docker/config.json, Hugging Face and Azure DevOps agents, along with .env and JSON files. It also takes Azure DevOps OIDC tokens, Azure managed-identity tokens and Azure instance metadata, keeping the first 8,000 characters of the metadata.

The code also asks the platform to issue fresh tokens, for example using SYSTEM_ACCESSTOKEN and SYSTEM_OIDCREQUESTURI in Azure DevOps. That means rotating stored secrets alone does not cover everything the malware could obtain. It compresses the data with zlib, encodes it in Base64 and POSTs it to a webhook.site endpoint.

## Version 1.26.2 Widens the Net to GitHub Actions OIDC Tokens

The second release is more aggressive than the first. Version 1.26.2 adds a module that also collects GitHub Actions OIDC tokens and widens the environment-variable search patterns used to pick up secrets. Pipelines that deploy to Azure from GitHub Actions are therefore in scope alongside Azure DevOps runners.

Any environment or CI runner that installed either version during the window could have had its cloud and pipeline credentials exfiltrated.

### Second Microsoft-Related PyPI Compromise This Year After TeamPCP’s durabletask Attack

This is the second Microsoft-related PyPI package compromised in 2026. In May, the TeamPCP group pushed three malicious versions of the durabletask package that spread the Mini Shai-Hulud worm. No actor has been tied to the agent-framework incident in the reporting so far.

## Response Steps for Teams That Installed agent-framework 1.26.1 or 1.26.2

Guidance from the security vendors centers on containment and clean rebuilds. Affected hosts and runners should be isolated, and the package pinned to the last safe version, 1.26.0, with environments rebuilt from a clean state. Teams should search proxy and DNS logs for the webhook.site URL used for exfiltration.

Exposed credentials should be rotated from a clean system. Because the malware requested fresh tokens from the platform, defenders also need to audit Azure identities, sign-in logs and any GitHub Actions workflows that deploy to Azure, not just replace the secrets stored on disk.

The attack takes advantage of a feature that has no connection to the package’s code: the way Python processes .pth files at startup means that simply having the poisoned wheel installed was enough to trigger the theft, regardless of whether any application used the framework.
