---
id: collect-261121-veille-ia/securite/microsoft-agent-framework-compromised-credential-stealer-targets-azure-and-ci-pipelines-wiz-blog
title: "Microsoft agent-framework Compromised: Credential Stealer Targets Azure and CI Pipelines | Wiz Blog"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["azure", "microsoft", "stealer", "agent-framework", "credential", "package", "agent", "compromised"]
source: https://www.wiz.io/blog/malicious-version-of-microsofts-agent-framework-published-to-pypi
source_anchor: ""
source_lines: [1, 121]
section: "Securite"
---
Microsoft agent-framework Compromised: Credential Stealer Targets Azure and CI Pipelines

On November 17, 2026 at 20:35 UTC, versions 1.26.1 & 1.26.2 of the Microsoft PyPI package agent-framework were published with an added credential stealer. The package gets almost 200k downloads a month. The malicious versions have since been quarantined.

At 20:35 UTC on November 17, 2026, a malicious version (1.26.1) of the agent-framework package (Microsoft Agent Framework) was uploaded to PyPI. A second version, 1.26.2, followed at 21:08 UTC. As of 21:30 UTC, both versions were no longer available on PyPI. The package gets about 41,000 downloads a week.

The most recent safe version is 1.26.0.

When a malicious version is installed, it drops a .pth file into the site-packages root (agent_framework_meta.pth in 1.26.1, af_runtime_diag.pth in 1.26.2). Python runs .pth files on every interpreter start, so the credential stealer runs whenever Python starts in that environment, even if agent-framework is never imported.

Updated Nov 18 14:00 UTC - Updated root cause and analyzed exfiltrated data. Wiz promptly reported these findings to Microsoft on the 17th.

Root Cause

The attacker's tooling reported its own progress to the same publicly accessible webhook it used for exfiltration, which let Wiz Research observe part of the operation.

The attacker appears to have compromised a client certificate and private key for Microsoft ESRP (Engineering System Release Pipeline), the internal service Microsoft uses to sign and publish this package.

Their first publish attempt, from AWS, was rejected by ESRP's IP filtering. A second attempt from Microsoft-owned IP space succeeded. How the credentials were obtained is not yet known.

The tooling identifies itself as Cairn, an open-source AI penetration-testing framework: it staged the package at /tmp/cairn_pkg and sent a CAIRN_ESRP_PROBE2 connectivity probe. Before publishing, the attacker ran the stealer on their own machine. That run captured an AI coding agent configuration that appears to be the open-source Pi harness backed by a GLM-family model (pi_glm53), routed through a local proxy.

Earlier this year, another Microsoft Python package, durabletask, was compromised after its PyPI token was stolen. We have not linked the two incidents.

Timeline (UTC)

Time

Event

2026-11-12 18:25

Legitimate 1.26.0 published (wheel + sdist, alongside agent-framework-core 1.26.0)

2026-11-17 20:35

Malicious 1.26.1 published

2026-11-17 ~21:08

Malicious 1.26.2 published

2026-11-17 ~21:30

1.26.1 & 1.26.2 no longer available on PyPI

Payload

agent_framework_meta.pth contains one line: import agent_framework_meta

In 1.26.1, agent_framework_meta/__init__.py then calls _telemetry._start(). The stealer forks into a detached session (os.setsid()), or falls back to a daemon thread where fork isn't available. It sets _AF_TEL=1 to avoid running again in child processes.

The file and function names (_telemetry, "Lightweight runtime diagnostics hook", user-agent af-runtime-diagnostics/1) are made to look like legitimate SDK telemetry.

Credential Stealer

The stealer is focused on Azure and Azure DevOps. It collects:

Environment variables with names containing strings such as TOKEN, SECRET, PASSWORD, KEY, AZURE, AWS, GITHUB, OPENAI, HF_, FOUNDRY, NPM, PIP, NUGET, SYSTEM, BUILD, AGENT

Other processes' environments: secret-looking values read from up to 300 /proc/*/environ files

Azure DevOps OIDC tokens: if SYSTEM_ACCESSTOKEN and SYSTEM_OIDCREQUESTURI are set, it requests an OIDC token for api://AzureADTokenExchange

Azure managed-identity tokens from IMDS or the App Service identity endpoint for 12 resources: ARM, Graph, Key Vault, Storage, Cognitive Services, Azure AI/ML, Search, Container Registry, Azure DevOps, and AzureADTokenExchange

Azure instance metadata

Everything is put into one JSON document, zlib-compressed, base64-encoded, and sent in one HTTPS POST to webhook[.]site. The URL is hardcoded and the payload has no fallback channel. Failures are silently ignored and not retried.

Version 1.26.2

1.26.2 was uploaded at 21:08 UTC, 33 minutes after 1.26.1. It is a repacked 1.26.1 that sends to the same webhook.site URL, with these changes:

New trigger.agent_framework_meta/__init__.py is restored to the clean 1.26.0 file. Instead, a new top-level af_runtime_diag.pth imports a new top-level module, af_runtime_diag.py, which starts the stealer as soon as it is imported. Searching only for the 1.26.1 file names will miss this version.

GitHub Actions OIDC theft. In workflows with id-token: write, it requests OIDC tokens for api://AzureADTokenExchange and https://management.azure.com/. Where an Entra ID federated credential trusts the workflow, these tokens can be exchanged for Azure access, with no stored secret needed.

Wider collection. Environment-variable matching now includes ACTIONS, OIDC, RUNNER and ARM_. GitHub-hosted runner paths (/home/runner/work/_temp/*, /home/runner/.azure/) were added, and the file cap rose from 60 to 80.

New User-Agent:runtime-diagnostics/1

How webhook.site limits the impact

webhook.site is a public request-inspection service. On its free tier, a URL that isn't attached to an account:

accepts at most 100 requests. After that, new requests get HTTP 410 Gone or 429 Too Many Requests and are not logged.

expires after 7 days.

We have confirmed the attackers token was on the free tier, and a maximum of 50 requests were captured. However, those requests can contain working cloud, CI and registry credentials.

Data Analysis of Exfiltrated Data

Added Nov 18 14:00 UTC

Wiz Research was able to analyze the 50 requests that were stored by webhook[.]site's free tier.

Four of the requests originated from the attacker's machines and the attack process.

Twenty-nine of the requests appear linked to sandbox environments, vendor scanning, and tests.

The remaining seventeen requests represent only three unique, successful cases of exfiltration of victim data.

Identify exposure: search lockfiles, CI logs, virtualenvs and container images for agent-framework 1.26.1 or 1.26.2. Pin to 1.26.0.

Check for execution: look in site-packages for agent_framework_meta.pth and agent_framework_meta/_telemetry.py (1.26.1), or af_runtime_diag.pth and af_runtime_diag.py (1.26.2). Any Python process started in that environment ran the payload.

Check egress: search proxy and DNS logs for webhook[.]site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb. A 200 means that host's data was accepted. A 410/429 suggests it was rejected.

Rotate credentials on affected hosts and runners:

every secret that was in the environment

Azure CLI sessions, Azure DevOps PATs and service connections, npm/PyPI/NuGet/Docker/Hugging Face tokens, git credentials

Audit Azure identities: review sign-in and activity logs for managed identities and federated credentials (workload identity federation) that were reachable from affected VMs, App Services, AKS nodes or ADO pipelines. Stolen managed-identity and OIDC-exchanged tokens are valid for up to 24 hours.

Audit GitHub Actions (1.26.2): for workflows that installed 1.26.2 with id-token: write, treat GitHub OIDC tokens for api://AzureADTokenExchange and https://management.azure.com/ as exposed. Review the Entra ID federated credentials that trust those repositories and workflows, and sign-ins for the matching app registrations. Rotate repository and organization secrets that were exposed to the job.

See how the Green Agent balanced security urgency with business continuity to turn a toxic combination into an executable remediation plan and ready-to-merge pull requests.
