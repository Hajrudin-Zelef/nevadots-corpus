---
id: collect-261121-veille-ia/microsoft-actu/malicious-microsoft-agent-framework-1-26-1-and-1-26-2-on-pypi-steal-cloud-and-ci-credentialsremove-i
title: "Malicious Microsoft agent-framework 1.26.1 and 1.26.2 on PyPI steal cloud and CI credentialsRemove it"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "pypi", "azure", "says", "agent-framework", "after", "tokens", "devops"]
source: https://infrastack.ai/live/MAL-2026-20101
source_anchor: ""
source_lines: [1, 47]
section: "Microsoft_Actu"
---
Details

1.26.1 (uploaded 20:35:20 UTC) and 1.26.2 (21:08:39 UTC, after PyPI had quarantined 1.26.1) were wheel-only repacks of the clean 1.26.0 wheel with no matching tag in the repository. PyPI quarantined them at 21:06:16 and 21:16:44 UTC (StepSecurity's timestamps).

PyPI Security says both are removed, 1.26.0 is not affected, and it worked with Microsoft to review the project's accounts and finish remediation (PYSEC-2026-4388).

Chainguard says its Libraries for Python repository blocked 1.26.1 at 20:36:08 UTC and 1.26.2 at 21:09:21 UTC, 48 and 42 seconds after upload, and served neither to customers. A .pth file in site-packages starts the stealer at every Python start in that environment, even if the package is never imported.

The stealer forks a detached collector that takes: secret-looking environment variables, also from up to 300 other processes via /proc/*/environ; Azure CLI token caches, pip.conf, .npmrc, .netrc, .git-credentials, Docker config, Hugging Face and Foundry tokens, Azure DevOps agent .credentials, and .env/JSON files under /app and /workspace.

It also requests Azure DevOps OIDC tokens, managed-identity tokens for 12 Azure resources plus instance metadata from IMDS or the App Service identity endpoint, and in 1.26.2 GitHub Actions OIDC tokens for api://AzureADTokenExchange and https://management.azure.com/.

These are attempts: whether a token is issued depends on the job's permissions and identity setup. The Azure DevOps request omits the required serviceConnectionId parameter, and JFrog says real Azure DevOps endpoints reject it with HTTP 400.

Where a job exposes SYSTEM_ACCESSTOKEN, the stealer still takes that token itself from the environment. Everything goes in one POST to a public `webhook[.]site` URL.

Wiz read the 50 requests the free-tier webhook stored: 4 from the attacker, 29 from sandboxes and scanners, and 17 from three real victims.

How it was published: Wiz (update of 18 Nov, 14:00 UTC) says the attacker appears to have used a stolen client certificate and private key for Microsoft ESRP, the internal service that signs and publishes the package.

A first attempt from AWS was blocked by ESRP's IP filtering; a second from Microsoft-owned IP space succeeded. How the credential was obtained is not known.

The tooling calls itself Cairn, an open-source AI penetration-testing framework. Cairn is public: Unit 42 has reported a customized fork used by the China-nexus cluster CL-STA-1238 against at least 19 organizations in Aug-Sep 2026, but no source links that cluster to this attack.

Microsoft has confirmed no root cause. A Microsoft member of the project wrote in issue #10411 at 22:08 UTC on 17 Nov that both 'compromised versions' had been pulled and that Microsoft was 'actively investigating' (quoted by Cybersixt; the issue is no longer public).

Microsoft later told The Register it has 'made some repositories private while we investigate malicious content', without naming them. microsoft/agent-framework and the ONNX Runtime repositories went dark the same night; no source ties ONNX Runtime to this attack.

Correction to earlier reports: no source ties the PyPI account vibiret to the uploads.

StepSecurity says its evidence does not establish which account was used, and SecurityWeek calls the link unconfirmed. vibiret joined on 17 Nov and is still listed on agent-framework, five agent-framework-* packages, semantic-kernel and the AutoGen packages, even after the PyPI and Microsoft review.

Only the meta-package was touched. Endor Labs checked 22 sibling agent-framework-* packages, including agent-framework-core, and none has a release after 12 Nov; none of the projects listed for vibiret has one either, and none has a malware report.

PyPI logged 427 downloads of the two bad versions on 17 Nov, a count that includes scanners and mirrors (ClickPy, via SXZ.io). Renovate opened upgrade PRs within six minutes of 1.26.1 (JFrog).

On 20 Nov two national CERTs, ThaiCERT and CSIRT Italia (red risk), issued alerts on the malicious releases. Status on 21 Nov, 10:47 UTC: Microsoft has published no advisory and has not said how the ESRP certificate was taken or whether it is revoked.

PYSEC-2026-4388 is unchanged since 18 Nov, github.com/microsoft/agent-framework still returns 404, and PyPI still lists microsoft and vibiret as the package's maintainers. There is no new agent-framework release; 1.26.0 is still the latest.

Risk

Who is affected

You are exposed if any environment installed the PyPI package agent-framework (the Microsoft Agent Framework meta-package that pip install agent-framework pulls) at 1.26.1 or 1.26.2: a developer machine, a CI job (Azure DevOps, GitHub Actions or other), a container build, or an Azure VM, App Service or AKS workload. PyPI served them only on 17 Nov 2026, 20:35-21:06 UTC (1.26.1) and 21:08-21:16 UTC (1.26.2). In practice that means an unpinned or loose requirement (agent-framework, >=1.x) that resolved in that window, the CI run of a Renovate or Dependabot PR from that evening, or a later install from a pip/uv cache, internal mirror or image that kept the files. Once installed, the stealer runs at every Python start in that environment, and it does the most damage on Azure DevOps agents, GitHub Actions jobs with id-token: write and Azure hosts with a managed identity. You can skip it if agent-framework was pinned or locked to 1.26.0 or earlier, if you install only agent-framework-core or other agent-framework-* packages without the meta-package (none of them had a malicious release), or if you install Python packages only through Chainguard Libraries (Chainguard says it blocked both versions within a minute and served neither). You can also skip it if you use only the .NET or Go SDK, for which no tampered release has been reported.

**Not affected if** agent-framework-core-only, chainguard-libraries, dotnet-only, go-only, hash-pinned-lockfile, pinned-1.26.0
