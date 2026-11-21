---
id: collect-261121-veille-ia/microsoft-actu/microsoft-agent-framework-pypi-compromise
title: "Microsoft Agent Framework PyPI Compromise"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["malicious", "azure", "credentials", "pypi", "python", "environments", "tokens", "releases"]
source: https://www.hexnode.com/threat-watch/microsoft-agent-framework-pypi-credential-stealer/
source_anchor: ""
source_lines: [1, 133]
section: "Microsoft_Actu"
---
Two malicious releases of Microsoft’s agent-framework Python package on November 17, 2026, introduced a credential stealer targeting Azure environments and CI/CD pipelines.

Versions 1.26.1 and 1.26.2 used Python .pth startup hooks to execute malicious code without requiring the package to be imported.

The payload collected environment variables, credential files, and cloud identity tokens, potentially exposing developer workstations and build environments.

Organizations should audit affected installations, investigate suspicious activity, remove malicious startup hooks, and rotate credentials accessible from compromised environments.

Microsoft Agent Framework Package Compromised on PyPI

Attackers distributed credential-stealing malware through two malicious releases of Microsoft’s official agent-framework package on PyPI on November 17, 2026. The payload targeted cloud credentials and CI/CD authentication tokens, while the method used to obtain publishing access remains under investigation.

Attackers published malicious versions 1.26.1 and 1.26.2 approximately 33 minutes apart, embedding code capable of harvesting credentials whenever Python started in an affected environment.

Security researchers identified the malicious releases shortly after publication, and both were removed from PyPI. However, developers and automated build systems that installed either version may have exposed sensitive credentials before the packages were withdrawn.

How the PyPI Credential Stealer Worked

Unlike traditional malicious dependencies that require an application to import compromised code, this attack exploited Python’s .pth startup mechanism.

The malicious packages installed executable .pth files in the Python site-packages directory. During interpreter initialization, Python processes these files, allowing the embedded import statements to execute automatically.

In affected environments where Python processes the malicious .pth startup hook, the credential stealer could execute during ordinary interpreter initialization, including application execution, testing, and other Python-based operations, even when the application never imports agent-framework.

Malicious releases and execution mechanisms

Component

Version 1.26.1

Version 1.26.2

Publication

Nov. 17, 20:35 UTC

Nov. 17, 21:08 UTC

Startup hook

agent_framework_meta.pth

af_runtime_diag.pth

Payload

_telemetry.py

af_runtime_diag.py

Credential targeting

Azure and Azure DevOps

Azure, Azure DevOps, GitHub Actions

Both versions collected credential-related environment variables, local configuration files, and secrets associated with development tools.

The payload also attempted to obtain fresh Azure managed identity and Azure DevOps OIDC tokens. Version 1.26.2 expanded collection to GitHub Actions OIDC tokens.

Collected information was compressed, Base64-encoded, and transmitted through HTTPS to an attacker-controlled webhook.site endpoint.

Researchers confirmed that the malicious releases were removed, with version 1.26.0 identified as the last known clean release.

Featured Resource

Cybersecurity kit

Access essential cybersecurity resources to strengthen security, reduce risk, and improve cyber resilience.

Enterprise Risk: Azure and CI/CD Credentials Exposed

The compromise highlights how a poisoned software dependency can expose credentials beyond the application that installed it.

Developer workstations, automated build runners, and cloud-hosted environments may store authentication tokens with access to sensitive enterprise resources.

If harvested credentials remain valid, attackers could potentially access cloud services, retrieve protected data, or interfere with software build and deployment workflows.

The risk is particularly significant for CI/CD environments using workload identity federation. Stolen OIDC tokens may provide access to trusted cloud resources when the associated identity configuration permits token exchange.

Because execution occurs during Python startup, an affected environment may generate repeated opportunities for credential collection until the malicious hook is removed.

Importantly, installation alone does not confirm successful exfiltration. Nevertheless, organizations should treat credentials accessible to an affected Python environment as potentially exposed.

Investigating Compromised Developer Endpoints with Hexnode XDR

Organizations responding to this compromise need visibility into affected endpoints, suspicious processes, and potential credential exposure.

On supported, enrolled endpoints, Hexnode XDR can assist investigations through process-level visibility, process trees, and endpoint investigation capabilities. Security teams can use these tools to examine suspicious Python execution and associated activity.

Hexnode’s identity-aware access controls can also help enforce conditional access policies for supported applications, complementing credential rotation and identity-provider investigations.

Recommended response actions

Audit affected environments: Identify developer workstations, CI/CD runners, and container images that installed agent-framework versions 1.26.1 or 1.26.2.

Contain suspicious activity: Isolate affected endpoints where appropriate and investigate Python processes, startup hooks, and outbound connections.

Contain and rebuild affected environments: Identify installations of the compromised package versions and avoid running Python or pip in affected environments until they are secured. Preserve relevant evidence, assess suspicious startup hooks and outbound connections, and rebuild affected environments from trusted dependencies. Remove malicious components as part of remediation rather than treating file deletion alone as proof of recovery.

Rotate exposed credentials: Revoke or rotate accessible Azure, Azure DevOps, GitHub Actions, and other development credentials. Review federated identity sessions and cloud activity logs.

Restore trusted environments: Rebuild affected environments using verified dependencies and review package installation histories for additional exposure.

Hexnode XDR can support endpoint investigation and containment on compatible platforms, but these capabilities do not imply payload-specific detection or recovery of credentials already exfiltrated.

FAQs

Can the Microsoft Agent Framework credential stealer execute without importing the compromised package?

Yes. The malicious releases used Python .pth startup files to execute code automatically during interpreter initialization. This means the credential stealer could run whenever Python started in an affected environment, even if the application never imported agent-framework.

Which Microsoft Agent Framework PyPI versions were compromised?

Microsoft Agent Framework versions 1.26.1 and 1.26.2, published on November 17, 2026, contained malicious credential-stealing code. Both releases were removed from PyPI, and version 1.26.0 was identified as the last known clean release.

What credentials could the malicious Microsoft Agent Framework packages steal?

The malicious packages targeted Azure and Azure DevOps credentials, environment variables, and local configuration files containing development secrets. Version 1.26.2 also targeted GitHub Actions OIDC tokens, potentially exposing cloud and CI/CD resources accessible through those identities.

Strengthening Software Supply Chain Defenses

The Microsoft Agent Framework compromise demonstrates how malicious package releases can threaten enterprise cloud identities without modifying application functionality.

Organizations should prioritize dependency auditing, rapid credential rotation, and endpoint investigation when compromised development packages are discovered.

Combining these practices with endpoint security and identity-aware access controls can help reduce the operational impact of future software supply chain attacks.

Strengthen Software Supply Chain Security

Protect developer endpoints, detect credential theft, and strengthen cloud security with Hexnode.

Content writer at Hexnode. Fueled by good coffee and the occasional cat cuddle, I enjoy crafting content that informs, connects, and resonates. Nothing excites me more than knowing my words have been read, appreciated, and maybe even bookmarked.
