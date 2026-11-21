---
id: collect-261121-veille-ia/securite/artex-explained-how-ai-penetration-testing-tool-is-powering-cyberattacks-tech-news-the-s-bharat
title: "ARTEX explained: How AI penetration-testing tool is powering cyberattacks | Tech News - The S Bharat"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["artex", "framework", "testing", "security", "used", "cyberattacks", "agents", "systems"]
source: https://thesbharat.com/thesbharat9gmail-com/artex-explained-how-ai-penetration-testing-tool-is-powering-cyberattacks-tech-news/
source_anchor: ""
source_lines: [1, 97]
section: "Securite"
---
ARTEX, an artificial intelligence (AI)-powered penetration testing tool designed to help security teams identify vulnerabilities in computer systems, is being used by cybercriminals to automate attacks against organisations. The framework can scan networks, identify security weaknesses, exploit vulnerable systems and steal data with limited human involvement.


The US Federal Bureau of Investigation (FBI), in an advisory issued on November 13, warned that multiple threat actors have been using ARTEX since at least August 2026 to carry out cyberattacks across sectors, including banking, government, technology, healthcare, defence and telecommunications.

## What is ARTEX

ARTEX is an open-source, AI-driven penetration testing framework released in July 2026 by a developer using the name Autumn-27. It was designed to help cybersecurity teams test the security of networks, applications and other IT systems that they own or are authorised to assess.


The framework uses large language models (LLMs) to coordinate multiple AI agents. These agents can carry out different stages of a security assessment, including scanning systems, identifying vulnerabilities, testing possible ways to exploit them and documenting their findings.

Unlike conventional security testing tools that follow predefined scans or commands, ARTEX can interpret instructions given in natural language and decide what steps to take based on the results of earlier tests.


For instance, an operator can give the framework a target and an objective. ARTEX then breaks that objective into smaller tasks and assigns them to specialised AI agents. The agents use existing security tools to examine the target, identify weaknesses and attempt to exploit them.


The framework also has a dashboard through which operators can monitor testing activity, review findings, track network traffic and intervene when required.


ARTEX does not have its own AI model. It connects to external models, including Anthropic’s Claude, OpenAI’s GPT models, DeepSeek, GLM, Grok and Kimi. The model used depends on the configuration selected by the operator.

## Is ARTEX available

The original developer made the ARTEX GitHub repository unavailable on October 8, 2026, following reports that the framework was being used in cyberattacks.


However, taking down the original repository has not stopped its distribution. According to the FBI, ARTEX remains available through independent community forks, archived copies and localised versions. A commercial version, called ARTEX PRO, is also available.


Cybersecurity firm Check Point Research identified more than 1,600 publicly accessible ARTEX instances in its November investigation. The actual number of deployments could be higher because some instances operate behind firewalls or within private networks.


The continued availability of the framework also raises questions about the limits of restricting open-source cybersecurity tools after they have been adopted by a wider community.

## How is ARTEX being used in cyberattacks

The framework’s ability to automate multiple stages of penetration testing has made it useful to cybercriminals. Instead of manually searching for vulnerabilities and deciding how to exploit them, attackers can give ARTEX an objective and allow its agents to carry out much of the work.


A November 5 investigation by Check Point Research found that several threat actors were using ARTEX for financially motivated cybercrime. Researchers identified 2,557 systems that had hosted ARTEX dashboards, of which 1,608 were publicly accessible at the time of the investigation.


The researchers also gained access to the databases of 10 misconfigured ARTEX instances. These contained records of the AI models used, instructions given by attackers and the actions performed by the agents.

One operation involved targeting 10 online services that sold or distributed Apple iOS code-signing certificates. These certificates can be used to sign applications for distribution outside Apple’s official App Store.


The attackers instructed ARTEX to obtain the certificates and their associated passwords. Its agents exploited weaknesses such as hardcoded administrator passwords, missing authentication on web APIs and remote code execution vulnerabilities to gain access to the targeted services.


The agents then searched the compromised systems for certificate databases and checked whether the stolen certificates were usable. The operation resulted in the theft of more than 300 certificates and their passwords. Three passed the researchers’ validation checks.


Check Point Research found that the operator largely left the agents to carry out the work, intervening mainly to check progress and adjust priorities.


Another campaign involved attacks against South Korean financial institutions. According to the FBI, the attacks resulted in the theft of personal information belonging to approximately 67,000 people.


ARTEX has also been linked to a supply chain compromise involving ConnectWise Cloud Backup. The incident allowed attackers to access Microsoft 365 environments belonging to downstream customers. The FBI said the compromise enabled access to more than 2,250 downstream tenants.

## Why is ARTEX difficult to contain

One concern is that ARTEX can operate with limited human supervision after receiving its initial instructions. This allows attackers to run multiple operations at the same time without having to direct every step.


The FBI said the framework can translate high-level objectives into commands executed by specialised AI agents. These agents can move from reconnaissance and vulnerability discovery to gaining access, escalating privileges and extracting data.


The framework can also identify weaknesses in systems that organisations may not monitor as closely as their primary applications. These include employee portals, legacy services, mobile application backends, supplier networks and third-party integrations.


Another concern is that ARTEX can exploit misconfigurations alongside conventional software vulnerabilities. A system may not have a major software flaw but could still expose sensitive information through weak passwords, missing authentication or incorrectly configured access permissions.


CERT-EU, the cybersecurity service for European Union institutions, warned on November 5 that ARTEX can identify and exploit such weaknesses at scale. It also noted that the framework can work through long sequences of attack attempts without the limitations associated with manual testing.


There is also the question of controlling what the AI agents can do. ARTEX uses configurable rules and instructions to keep testing within the scope authorised by an operator. However, CERT-EU noted that these restrictions are not technically enforced by the framework. Its ability to avoid actions outside the authorised scope depends on the configuration and the AI model being used.


This creates a risk when the framework is operated by someone with malicious intent or is configured to target systems without permission.

## How can organisations protect themselves

The FBI has advised organisations to review internet-facing applications, employee portals, cloud infrastructure and third-party connections for vulnerabilities that could be exploited through automated attacks.


It has also recommended monitoring unusual network activity, suspicious login attempts, unexpected changes to user permissions and attempts to access privileged accounts.


CERT-EU has called for organisations to prioritise security updates for internet-facing systems, restrict access to administrative interfaces and implement multi-factor authentication (MFA) across critical services.


Organisations have also been advised to retain vulnerability findings, including those considered less severe, and address them as part of their security maintenance process.
