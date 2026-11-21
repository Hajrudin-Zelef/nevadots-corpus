---
id: collect-261121-veille-ia/microsoft-actu/microsoft-agent-framework-pypi-package-hijacked-ai-europesays-com
title: "Microsoft agent-framework PyPI Package Hijacked - AI | EUROPESAYS.COM"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["package", "agent-framework", "python", "azure", "agent", "microsoft", "framework", "hijacked"]
source: https://www.europesays.com/ai/246219/
source_anchor: ""
source_lines: [1, 79]
section: "Microsoft_Actu"
---
Two releases of Microsoft’s official agent-framework Python package were hijacked to harvest Azure and CI credentials. Traced by OX Radar, the supply chain attack executes automatically on Python startup—no import required.

Overview

A popular official Microsoft Python package, with 192,534 monthly downloads was Hijacked.

OX Security researchers identified a malicious wheel published as agent-framework versions 1.26.1, 1.26.2 Microsoft’s Agent Framework for Python. The package is a credential stealer: it harvests environment secrets, cloud and package-manager credentials, and Azure managed identity tokens, then exfiltrates them to an attacker-controlled webhook.

The payload runs automatically on every Python interpreter start – once the package is installed, without the package ever being imported. Because it pulls in the genuine agent-framework-core 1.26.0 as a dependency, the framework keeps working normally and the compromise is easy to miss.

Key Findings

Microsoft’s agent-framework 1.26.1, and 1.26.2 was compromised

Executes via a .pth file on every Python start, no import required.

Targets Azure and Azure DevOps CI build agents: pipeline OIDC tokens, managed identity tokens and agent credentials.

Exfiltrates to webhook.site as zlib-compressed, base64-encoded JSON.

Recommended Actions

Check all environments, CI images and lockfiles for agent-framework==1.26.1 or agent-framework==1.26.2 and remove it immediately.

Look for site-packages/agent_framework_meta.pth and agent_framework_meta/_telemetry.py on hosts and build agents.

Pin agent-framework to a verified Microsoft release and install with hash checking.

If the package ran anywhere, treat every secret reachable from that host as compromised: rotate Azure service principal secrets, PATs, GitHub, npm, PyPI, NuGet, Docker, Hugging Face and OpenAI tokens.

Review Azure sign-in and activity logs for use of managed identities and federated credentials from affected agents.

Block outbound traffic to webhook.site from build infrastructure.

Affected Packages

The package is an official Microsoft Python package, with 192,534 monthly downloads.

PackageVersionFix VersionEcosystemWheel SHA-256agent-framework1.26.1, 1.26.21.26.0PyPId27fa34414e6201d5e03e559ef634f020057554d7b3f4898dd5d7cd3e989e110

How we traced it: OX Radar

This compromise was identified and traced through OX Radar, OX Security’s early-warning system for emerging threats across the open-source and AI supply chain. Radar continuously monitors npm, PyPI, MCP servers, and Agent Skills from the moment new releases appear.

Infection Analysis

The package ships agent_framework_meta.pth containing a single line: import agent_framework_meta. Python executes import lines in .pth files every time an interpreter starts, so the payload runs in every Python process on the machine once the package is installed.

The package’s __init__.py is a copy of the genuine Microsoft file with one block appended at the end: from . import _telemetry; _telemetry._start(), wrapped so that any error is silently ignored.

_start() sets the environment variable _AF_TEL=1 so it fires only once per process tree, then forks a detached child with setsid (or starts a background thread where fork is unavailable). The user’s program continues as normal.

Technical Analysis

Exfiltration. The webhook URL is stored base64-encoded in the _ENC variable in agent_framework_meta/_telemetry.py. Data is sent as POST https://webhook.site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb?t=telemetry with the header User-Agent: af-runtime-diagnostics/1. The body is the collected data as JSON, zlib-compressed and base64-encoded.

What it collects.

Environment variables whose names contain TOKEN, SECRET, PASSWORD, KEY, AZURE, AWS, GITHUB, HF_, OPENAI, SYSTEM, BUILD, CI_, PIP, NPM, NUGET, COSMOS, TENANT, CLIENT, MSI and similar.

Up to 60 files: ~/.azure/*.json, pip.conf, .npmrc, .netrc, .git-credentials, .docker/config.json, Hugging Face tokens, Foundry config, Azure DevOps agent .credentials (/agent/_work, /vss-agent, /mnt/azure_nvme_temp/_work), /app/.env and /workspace/*.json.

Secrets from /proc/*/environ for up to 300 other processes.

Azure DevOps pipeline identity: if SYSTEM_ACCESSTOKEN and SYSTEM_OIDCREQUESTURI are set, it requests an OIDC token for api://AzureADTokenExchange, which can be exchanged for Azure access.

Managed identity tokens via IMDS (169.254.169.254) or the App Service IDENTITY_ENDPOINT, for ARM, Graph, Key Vault, Storage, Cognitive Services, ML / AI Foundry, ACR, Search and Azure DevOps (499b84ac-…), plus VM instance metadata.

Host details: hostname, user, working directory and argv.

Conclusions

The malicious agent-framework shows how attackers are targeting the AI developer ecosystem and the CI pipelines behind it. While directly targeting an official Microsoft package, it seems that attackers found a way to hijack the wheel creation flow to add malicious logic inside.

While researching this malware, the malicious version 1.26.1 was reverted to 1.26.0, then the attacker uploaded another malicious version with changes – 1.26.2, containing a different path for the same payload. This attack isn’t just a one off exfiltration attempt, but a planned, live and coordinated effort to reach as many victims as they can, while still being able to bypass blocks and restrictions.

IOCs

TypeValueWheel SHA-256d27fa34414e6201d5e03e559ef634f020057554d7b3f4898dd5d7cd3e989e110_telemetry.py (RECORD sha256, base64url)OfoynyqT9zSwu49a2yA-ozMdGuR5gypQFaVMsXqr1mM.pth (RECORD sha256, base64url)IfKOhRehD_5DH41o9YdcheykABrm0RgyKwpBhzmY614URLwebhook.site/d381fd80-ec62-42ad-8f5c-d1b15103c9bbFilessite-packages/agent_framework_meta.pth, agent_framework_meta/_telemetry.py
