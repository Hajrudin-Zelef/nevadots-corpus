---
id: collect-261121-veille-ia/securite/fbi-warns-cybercriminals-are-using-ai-tool-artex-for-automated-attacks-across-critical-sectors-cyber
title: "FBI Warns Cybercriminals Are Using AI Tool ARTEX for Automated Attacks Across Critical Sectors | #cybercrime | #infosec | Blog of National Cyber Security Training Academy"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["artex", "tool", "security", "automated", "across", "using", "cybercriminals", "attacks"]
source: https://blog.nationalcybersecuritytrainingacademy.com/fbi-warns-cybercriminals-are-using-ai-tool-artex-for-automated-attacks-across-critical-sectors-cybercrime-infosec/
source_anchor: ""
source_lines: [1, 29]
section: "Securite"
---
Follow us on Google News

ARTEX, an open-source artificial intelligence tool originally created to streamline cybersecurity testing, has been weaponized by cybercriminals to carry out large-scale, automated attacks with minimal human supervision. In an advisory issued on November 13, the Federal Bureau of Investigation (FBI) warned that threat actors have been actively exploiting the framework since at least August 2026 across multiple critical sectors. Targeted industries include banking, government agencies, technology firms, healthcare services, defense, and telecommunications. According to the FBI, attackers simply define a target and an objective, allowing autonomous AI agents to scan networks, identify vulnerabilities, breach systems, and exfiltrate sensitive data.

**How the ARTEX Framework Operates**

Released in July 2026 by a developer using the alias Autumn-27, ARTEX was designed as an AI-driven penetration testing framework to assist security teams in evaluating authorized networks and applications. Unlike conventional testing software that relies on rigid, pre-programmed commands, ARTEX interprets plain-language instructions and divides tasks among multiple specialized AI agents using large language models. The framework does not include its own native AI model, instead connecting to external platforms such as Anthropic’s Claude, OpenAI’s GPT models, DeepSeek, GLM, Grok, and Kimi.

Although the original developer removed the ARTEX repository from GitHub on October 8, 2026, following reports of malicious abuse, the tool remains widely accessible through independent community forks, archived versions, local builds, and a commercial edition called ARTEX PRO. In November, cybersecurity firm Check Point Research identified 2,557 hosted ARTEX dashboards online, including 1,608 publicly accessible instances, noting that the true figure is likely higher due to deployments behind private firewalls.

**Major Breaches in South Korea and Digital Certificate Theft**

During a November 5 investigation, Check Point Research accessed databases from 10 misconfigured ARTEX instances, uncovering records of financially motivated cybercrime campaigns. In one operation, attackers targeted 10 online services that sell or distribute Apple iOS code-signing certificates, which allow applications to be signed outside Apple’s official App Store. Instructed to harvest certificates and passwords, the AI agents exploited hardcoded administrator passwords, unauthenticated web APIs, and remote code execution flaws to steal more than 300 certificates, three of which remained actively usable. Investigators noted that the human operator rarely intervened, mostly monitoring progress and adjusting priorities.

The FBI also linked ARTEX to cyberattacks against financial institutions in South Korea, resulting in the theft of personal data belonging to approximately 67,000 individuals. Additionally, the tool was connected to a supply chain compromise involving ConnectWise cloud backups, which granted attackers access to customer Microsoft 365 environments and impacted more than 2,250 downstream tenants.

**Limited Safeguards and Recommended Defenses**

Security agencies warn that ARTEX can execute multi-stage intrusions across lightly monitored infrastructure, such as employee portals, legacy software, mobile app backends, and third-party vendor networks. On November 5, the European Union’s cybersecurity service, CERT-EU, cautioned that ARTEX lacks strict technical guardrails, meaning its boundaries depend entirely on operator configuration and the underlying AI model used.

To counter these automated threats, the FBI advised organizations to audit internet-facing applications, cloud infrastructure, and third-party connections while monitoring for unusual network traffic, suspicious login attempts, and unauthorized privilege changes. CERT-EU similarly urged institutions to prioritize security patches for public-facing systems, restrict access to administrative interfaces, enforce multi-factor authentication on critical services, and remediate lower-severity vulnerabilities routinely.

Click Here For The Original Source.

——————————————————–

…………

### *Related*
