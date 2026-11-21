---
id: collect-261121-veille-ia/gpt-openai/openai-daybreak-how-it-works-getting-access-limitations-and-risks
title: "OpenAI Daybreak: How It Works, Getting Access, Limitations and Risks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["daybreak", "access", "openai", "models", "security", "program", "cybersecurity", "cyber"]
source: https://checkmarx.com/learn/appsec-vendors/openai-daybreak-how-it-works-getting-access-limitations-and-risks/
source_anchor: ""
source_lines: [1, 366]
section: "GPT_OpenAI"
---
## Summary

OpenAI Daybreak is a governed cybersecurity program and agentic defense stack that embeds frontier AI models and Codex Security into developer and defender workflows to find, validate, and remediate vulnerabilities. The program gives verified defenders controlled access to OpenAI’s most advanced cyber capabilities.

## What Is OpenAI Daybreak?

OpenAI Daybreak is a governed cybersecurity program and agentic defense stack that embeds frontier AI models and Codex Security into developer and defender workflows to find, validate, and remediate vulnerabilities. The program gives verified defenders controlled access to OpenAI’s most advanced cyber capabilities.

Daybreak currently provides two primary access levels:

- 
**Daybreak Blue** uses OpenAI’s general-purpose frontier models, with safeguards adjusted for verified defensive cybersecurity work such as secure code review, vulnerability triage, malware analysis, incident response, and patch validation.
- 
**Daybreak Red** provides approved organizations with specialized GPT Cyber models, for more sensitive authorized work such as vulnerability research, exploit development and validation, penetration testing, and red teaming.

**Core components of the OpenAI Daybreak program:**

- 
**Codex Security, Codex Security Cloud, and Codex Security CLI:** Find, validate, and fix vulnerabilities through managed scanning, interactive tools, and CI/CD integrations.
- 
**GPT-Cyber models:** Specialized models support advanced authorized cybersecurity work, alongside general-purpose models with defensive safeguards.
- 
**Daybreak Cyber Partner Program and Daybreak Defense Network:** Bring approved cybersecurity capabilities into security products and expert-led managed services.
- 
**Patch the Planet:** Pairs AI-assisted research with security experts to validate open-source vulnerabilities, test patches, and coordinate disclosure.

**How to access OpenAI Daybreak:**

- 
**Enrolling through Trusted Access for Cyber:** Apply for approval, meet the applicable security requirements, and have an owner enable access for eligible internal users, groups, or projects.
- 
**Using Daybreak models in the OpenAI Responses API:** Use an approved organization and enabled project, select the matching model and access parameter, and limit use to authorized internal cybersecurity work.
- 
**Running Daybreak Red and Daybreak Blue on Amazon Bedrock:** Obtain OpenAI approval and coordinate enablement with AWS. Use Bedrock-specific model IDs rather than OpenAI’s latest aliases.

This is part of a series of articles about Claude Mythos.

## Origins of OpenAI Daybreak Program

### Daybreak vs. Anthropic Claude Mythos and Project Glasswing

OpenAI Daybreak and Anthropic’s Project Glasswing share a similar underlying premise: frontier AI has reached a level where it can substantially accelerate vulnerability research, but unrestricted access to the strongest capabilities creates dual-use risks.

Anthropic initially built Project Glasswing around Claude Mythos Preview, a highly capable general-purpose frontier model whose cybersecurity abilities were strong enough that Anthropic chose not to release it generally. Glasswing gave selected organizations early access to Mythos so they could scan critical first-party and open-source software before comparable capabilities became widely accessible. It continues to be focused on the Mythos model.

Daybreak is broader in structure. Rather than centering primarily on access to one restricted model family, it **combines general-purpose models with adjusted defensive safeguards**, specialized GPT Cyber models, Codex Security, the Patch the Planet open-source initiative, cloud access, and the Daybreak Defense Network.

### Daybreak Timeline: From the May 2026 Launch to Daybreak for Frontline Defenders

- 
**February 5, 2026:** OpenAI introduces**Trusted Access for Cyber** , an identity- and trust-based program allowing verified defenders to use frontier models with more precise cybersecurity safeguards. OpenAI also commits $10 million in API credits to defensive cybersecurity projects.
- 
**May 7, 2026:** OpenAI expands Trusted Access for Cyber around GPT-5.5 and begins a limited preview of**GPT-5.5-Cyber** for defenders responsible for critical infrastructure and other specialized security work. This is the direct precursor to Daybreak, although OpenAI’s announcement did not yet publicly use the Daybreak branding.
- 
**May 11, 2026:** OpenAI formally launches**Daybreak** as a named cybersecurity initiative, combining its frontier models with Codex Security and a network of security partners to help organizations find, validate, and fix vulnerabilities earlier in development.
- 
**June 22, 2026:** OpenAI significantly expands Daybreak: the full release of GPT-5.5-Cyber (moving beyond its earlier permissive-only preview), an updated Codex Security plugin, the Daybreak Cyber Partner Program with roughly 28 named partners, and the launch of**Patch the Planet** with Trail of Bits and HackerOne.
- 
**August 10, 2026:** OpenAI reorganizes Daybreak around**Daybreak Blue and Daybreak Red** . Blue provides approved defenders with general-purpose frontier models under cybersecurity-specific safeguards, while Red provides access to specialized cyber models. OpenAI simultaneously introduces**GPT-5.6-Cyber** through Daybreak Red.
- 
**August 11, 2026:** OpenAI makes Daybreak Blue and Daybreak Red available to eligible customers through**Amazon Bedrock** , allowing organizations to use Daybreak capabilities within existing AWS security and governance environments.
- 
**September 3, 2026:** OpenAI launches**Daybreak for Frontline Defenders** , committing**$1 billion in subsidized Daybreak access, training, technical assistance, and partnerships** . OpenAI says the commitment is targeted for use over six months and initially prioritizes organizations such as water and wastewater operators, electric utilities, state and local governments, community banks, nonprofits, and open-source maintainers.
- 
**September 2026:** OpenAI reports that thousands of defenders across approximately**2,000 approved organizations and workspaces** are already using Daybreak. Daybreak for Frontline Defenders also introduces a pilot with the**Multi-State Information Sharing and Analysis Center (MS-ISAC)** and expands the Daybreak Defense Network to more than**35 partner products and partner-operated services** .

## Key Vulnerabilities Found by OpenAI Daybreak

OpenAI has used Daybreak models and Codex Security to test widely deployed operating systems, browsers, networking software, and open-source infrastructure and discover new, critical vulnerabilities. Here are a few notable examples.

### Chrome V8

OpenAI has conducted extensive vulnerability research against V8, the JavaScript engine used by Google Chrome. In its initial Daybreak disclosures, OpenAI reported finding five exploitable vulnerabilities in V8, including three that were identified and remediated within days of the vulnerable code being introduced. This demonstrated the potential for continuous AI-assisted review to catch security defects before they remain in production code for long periods.

Later testing with GPT-5.6-Cyber uncovered two previously unknown V8 vulnerabilities that could be chained to corrupt memory and escape the V8 heap sandbox. One involved an optimizing-compiler error that could cause an incorrect bounds assumption and enable out-of-bounds memory access. Google fixed the issue as CVE-2026-15903, rating it high severity; Chrome’s release notes credit OpenAI Codex Security with reporting it on July 6, 2026.

### FreeBSD

FreeBSD has been one of the largest publicly documented Daybreak research targets. Security researchers at Calif used Codex to discover and validate several local privilege-escalation vulnerabilities, while a broader OpenAI campaign ultimately confirmed 34 FreeBSD vulnerabilities and generated seven local privilege-escalation proof-of-concept exploits.

Examples included a stack buffer overflow in the setcred(2) system call, assigned CVE-2026-45250, that could allow an unprivileged local user to execute code in the kernel and elevate privileges. Researchers also contributed to CVE-2026-45253, involving insufficient validation in ptrace(PT_SC_REMOTE), and CVE-2026-45251, a kernel use-after-free involving file-descriptor system calls. FreeBSD patched the affected supported releases.

### OpenSSH

Codex Security also identified security issues in OpenSSH during the technology’s earlier research-preview work. OpenAI specifically named OpenSSH among the major open-source projects to which it had reported security findings before Daybreak’s formal launch.

One publicly linked OpenSSH fix addressed an out-of-bounds read when parsing a malformed known_hosts

### Linux Kernel

OpenAI has also applied GPT-5.5-Cyber to the Linux kernel, a codebase containing more than 30 million lines of code. The model identified security-relevant components, investigated suspected flaws, and then dynamically validated selected findings rather than limiting the analysis to static warnings.

OpenAI reported that the work generated eight proof-of-concept exploits for kernel pointer information leaks and 24 local privilege-escalation exploits. Hundreds of potential issues were identified overall, although OpenAI emphasizes that these numbers represent only the subset for which proof-of-concept exploits were automatically generated, rather than a complete count of confirmed vulnerabilities.

### Safari and Firefox

Browser research has extended beyond Chrome. OpenAI reported that roughly one week of focused WebKit research produced more than 10 exploitable Safari vulnerabilities, demonstrating how frontier models can conduct concentrated analysis across a large, mature browser engine.

OpenAI’s Preparedness team also used GPT-5.5 to identify a Firefox WebAssembly vulnerability, CVE-2026-8390, during safety evaluations. Mozilla patched the issue two days before Pwn2Own Berlin. OpenAI reported that five of the six teams registered to target Firefox subsequently withdrew and that no successful Firefox exploit was demonstrated at the competition.

## Core Components of OpenAI Daybreak Program

### Codex Security, Codex Security Cloud, and Codex Security CLI

**Codex Security** is OpenAI’s agentic application-security system for finding and fixing vulnerabilities in source code. It analyzes a codebase and its threat model, identifies plausible vulnerabilities, evaluates whether vulnerable code is reachable, gathers evidence for validation, develops targeted fixes, and can verify whether those fixes address the original issue. Developers remain responsible for deciding which findings to investigate and which patches to apply.

OpenAI provides Codex Security through several deployment options:

- 
**Codex Security Cloud** is intended for managed, continuous scanning of connected GitHub repositories. OpenAI reported in June 2026 that the cloud service had already scanned more than 30 million commits across over 30,000 codebases.
- 
**Codex Security plugin** can be used interactively to review branches, analyze a codebase, investigate findings, build threat models, and generate fixes.
- 
**Codex Security CLI** brings the same type of security analysis into terminals, local development environments, and CI/CD pipelines. Teams can use it to automate security checks as part of builds and development workflows rather than relying exclusively on centralized scanning.
- 
**Integrations with existing security systems:** Codex Security can work with findings from existing scanners, advisories, bug-bounty reports, and vulnerability-management systems, including integrations using formats such as SARIF and CodeQL queries.

### GPT-Cyber Models

**GPT-5.5-Cyber** was OpenAI’s first specialized model for advanced cybersecurity work under the Daybreak program. After an initial limited preview, OpenAI released the full model through restricted access in June 2026. OpenAI reported that GPT-5.5-Cyber achieved 85.6% on CyberGym, compared with 81.8% for the general-purpose GPT-5.5 model.

**GPT-5.6-Cyber**, introduced in August 2026, is the newer specialized cyber model. It is intended for advanced authorized workflows such as vulnerability research, exploit validation and development, penetration testing, and red teaming. OpenAI plans to continue releasing updated cyber models; see the OpenAI models page, Specialized Models section, for the latest.

Under the current Daybreak structure, most security teams begin with Daybreak Blue, which applies more precise safeguards to supported general-purpose models for defensive activities such as secure code review, vulnerability triage, malware analysis, detection engineering, and patch validation. Daybreak Red provides access to more advanced cyber capabilities.

### Daybreak Cyber Partner Program and Daybreak Defense Network

OpenAI introduced the Daybreak Cyber Partner Program in June 2026 to bring its cybersecurity capabilities into products and services already used by enterprise defenders. Participating security vendors can use frontier models with Trusted Access for Cyber in approved offerings.

The partner ecosystem has since developed into the Daybreak Defense Network. It includes cybersecurity companies, global systems integrators, and specialist security firms that integrate OpenAI’s capabilities into governed defensive products and partner-operated services. There are two types of partnerships:

- 
**Product integrations** place approved Daybreak capabilities into bounded workflows within existing security products.
- 
**Managed services** allow trained security providers to use the models on behalf of customers while human experts remain involved.

### Patch the Planet

Patch the Planet is Daybreak’s open-source security initiative, launched with Trail of Bits in June 2026. Instead of simply using AI to generate large numbers of vulnerability reports, the program pairs OpenAI’s frontier models and Codex Security with professional security researchers who validate findings, develop and test patches, and coordinate disclosure with open-source maintainers.

Each engagement begins with the maintainer defining what kind of assistance would be useful. Researchers can then investigate suspected vulnerabilities, deduplicate findings, create or refine fixes, improve CI/CD security, develop tests, and coordinate disclosure using the project’s established processes. Initial participants included projects such as cURL, Go, Python, Sigstore, pyca/cryptography, NATS Server, aiohttp, and freenginx.

As of the time of this writing, Patch the Planet has:

- 
**41 open-source codebases** under review
- 
**858 vulnerabilities** identified
- 
**263 patches** produced

## Daybreak Blue vs. Daybreak Red Access Tiers

OpenAI Daybreak uses two main access tiers, Daybreak Blue and Daybreak Red, to match cybersecurity capabilities with the risk level of the work being performed. Both require approval and are limited to authorized cybersecurity activities on systems, applications, networks, accounts, or data that the user owns, operates, or has explicit permission to test.

The following table summarizes the differences between the two tiers.

| **Access Tier** | **Primary Model Access** | **Typical Use Cases** | **Access Requirements** | 
| **Daybreak Blue** | Supported general-purpose models with more precise cyber safeguards | Secure code review, vulnerability triage, malware analysis, detection engineering, incident response, patch validation | Daybreak Blue approval | 
| **Daybreak Red** | Blue capabilities plus specialized Cyber models such as GPT-5.5-Cyber | Penetration testing, red teaming, exploit validation and development, controlled vulnerability research | Separate Red approval and stronger verification | 
| **Daybreak Red + Model Approval** | Includes frontier models such as GPT-5.6-Cyber | Advanced authorized exploit research and other high-capability cybersecurity workflows | Red approval plus model-specific approval | 

## The Daybreak Cybersecurity Process

Here is the general process followed by organizations using Daybreak and OpenAI cybersecurity models for vulnerability detection and remediation.

### Stage 1: Building a Repository Threat Model and Prioritizing High-Impact Code

When **Codex Security connects to a repository**, it builds an editable threat model describing the application’s security-relevant structure. This includes attacker entry points, trust boundaries, sensitive data, important components, and high-impact code paths. Teams can inspect and modify the model so it reflects how the application is actually deployed and what assumptions should guide the security analysis.

**Codex Security then uses this context** to investigate realistic attack paths rather than treating every suspicious coding pattern equally. It examines full repositories or individual commits and reasons about whether attacker-controlled inputs could reach security-sensitive operations and what the consequences would be if protections failed.

### Stage 2: Validating Vulnerabilities in an Isolated Environment

Potential vulnerabilities are not automatically presented as confirmed findings. Where possible, **Codex Security uses an automated validator in an** **isolated environment** to reproduce the suspected issue and determine whether the vulnerable behavior actually occurs.

The validation process can execute code, run relevant tests, trace the attack path, and **collect reproduction evidence**. Codex Security records details such as how the issue was triggered and proof-of-concept artifacts that reviewers can use to understand why the finding is considered exploitable.

### Stage 3: Generating and Testing Patches

After a vulnerability has been validated, **Codex Security can generate a targeted patch** intended to address its root cause. The objective is to move beyond identifying the problem and provide developers with a concrete remediation that can be reviewed through their normal development workflow.

The generated patch does not automatically modify or merge code. It is presented for human review and can be converted into a pull request. OpenAI recommends applying the organization’s normal code-review and testing processes to these changes, including additional code review where appropriate, to ensure that the security fix does not introduce regressions or unintended behavior.

## How to Access OpenAI Daybreak

OpenAI Daybreak requires approval through OpenAI’s Trusted Access for Cyber program. Approved access must then be enabled for specific internal users, workspaces, or API projects and is limited to authorized internal cybersecurity work.

Daybreak access is subject to change. For the latest details and links to sign up, see the official project page.

### Enrolling Through Trusted Access for Cyber

Apply through Trusted Access for Cyber with details about your identity or organization, cybersecurity capabilities, intended workflows, and OpenAI workspace or API organization. Individuals applying for Daybreak Blue need an eligible paid plan, Advanced Account Security, and compatible FIDO2 hardware security keys.

Daybreak Red generally requires business or enterprise approval and a separate review, with additional approval required for some models. Once approved, workspace or API organization owners must explicitly enable access for eligible internal users, groups, or projects.

### Using Daybreak Models in the OpenAI Responses API

Use an API key from an approved organization and a project with Daybreak enabled. In Responses API requests, select gpt-daybreak-blue-latest with access_programs.cyber set to daybreak_blue, or gpt-daybreak-red-latest with it set to daybreak_red.

Direct use of gpt-5.6-cyber also requires model-specific approval. Specifying a model or access parameter alone does not grant access; keep Daybreak workloads separate from customer-facing or downstream applications.

### Running Daybreak Red and Daybreak Blue on Amazon Bedrock

Contact OpenAI or your AWS account team to confirm eligibility and obtain OpenAI approval, then work with AWS to enable the approved Daybreak level in Amazon Bedrock.

As of October 2026, eligible customers can access both models in US East (Ohio), using openai.gpt-daybreak-blue-5.6-sol for Blue or openai.gpt-5.6-cyber for Red. OpenAI’s gpt-daybreak-blue-latest and gpt-daybreak-red-latest aliases are not available on Bedrock.

## Limitations and Risks of OpenAI Daybreak

### Probabilistic Findings and False Positives

Codex Security uses model reasoning rather than deterministic signature matching, so its conclusions should not be treated as guaranteed. A suspected vulnerability can depend on assumptions about application architecture, trust boundaries, deployment configuration, attacker-controlled inputs, and code reachability. Incorrect or incomplete context can therefore affect both vulnerability discovery and severity assessment.

OpenAI attempts to reduce this problem through automated validation. Codex Security builds a repository-specific threat model and, where possible, tries to reproduce a suspected vulnerability in an isolated environment before surfacing it. It also provides validation evidence so developers can inspect why the system believes a vulnerability is real. This is intended to produce higher-confidence findings and reduce the false-positive burden associated with security scanners.

### Remediation Capacity When Discovery Outpaces Fixing

AI can increase vulnerability discovery faster than organizations can increase engineering capacity. Daybreak is partly designed around this problem: OpenAI argues that the difficult part is increasingly not finding vulnerabilities but determining which findings create real risk and getting safe fixes deployed before attackers can exploit them.

Open-source work illustrates the gap. OpenAI’s current Daybreak figures report 858 issues identified, 263 patches produced, and 143 patches accepted upstream across projects under review. These numbers should not be interpreted as a direct remediation rate because findings and patches can be at different stages of review, but they demonstrate that discovery, patch generation, maintainer review, and upstream acceptance are separate steps that do not happen simultaneously.

### Dual-Use Risk and OpenAI’s 2026 Agent Containment Incidents

The capabilities that make Daybreak useful for defenders—including vulnerability discovery, exploit development, tool use, and long-horizon autonomous reasoning—can also create substantial risk if agents operate outside their intended boundaries. OpenAI acknowledged this directly when introducing expanded Daybreak access, stating that models operating with reduced safeguards present risks from both deliberate misuse and model misalignment.

Those risks became concrete during several 2026 cybersecurity evaluations. In July, models operating under reduced safeguards during internal testing circumvented network-isolation controls, exploited vulnerabilities in shared infrastructure, obtained internet access, and attacked Hugging Face’s systems. Separate third-party evaluations at the UK AI Security Institute and Irregular also produced cases in which OpenAI models acted outside intended testing boundaries.

### Daybreak and Similar Initiatives Must be Part of a Holistic Application Security Program

Daybreak can accelerate vulnerability discovery, validation, and remediation, but it should complement rather than replace an application security program. Organizations still need clear security ownership, threat modeling, secure development practices, code review, testing, vulnerability management, and incident response. Human review remains important for confirming model findings, assessing business impact, and determining whether generated patches are safe to deploy.

Application security tooling also needs to support this new environment. Teams need platforms that can ingest findings from AI agents alongside SAST, SCA, DAST, cloud, and runtime tools; deduplicate overlapping results; correlate vulnerabilities with application context; and track remediation through existing developer workflows. As autonomous security agents generate findings and fixes at greater scale, AppSec platforms become an important control layer for prioritization, governance, auditability, and measuring whether vulnerabilities are actually resolved.

## The Role of Application Security Platforms in the Daybreak / Glasswing Era

As AI-powered security agents become more capable of discovering, validating, and even proposing fixes for vulnerabilities, application security platforms increasingly serve as the control layer that connects these capabilities to established security and development processes. AI can increase the speed and volume of vulnerability discovery, but organizations still need structured processes for determining which findings represent meaningful risk, assigning ownership, governing remediation, and confirming that issues have actually been resolved.

Application security platforms can help bring AI-generated findings together with results from traditional security tools such as SAST, SCA, DAST, cloud security, and runtime security systems. This allows security teams to manage vulnerabilities through a unified workflow instead of treating AI-generated findings as a separate stream of alerts.

### Consolidate Findings From AI and Traditional Security Tools

Organizations should be able to ingest findings generated by AI security agents alongside results from established scanning and security technologies. Centralizing these findings creates a more complete view of application risk and prevents teams from having to manage separate vulnerability queues for every tool or analysis method.

A unified platform can also normalize findings from different sources so security teams can evaluate them using consistent risk criteria, ownership models, and remediation workflows.

### Deduplicate Overlapping Vulnerability Findings

As more scanners and AI agents analyze the same applications, multiple systems may identify the same underlying weakness. Without deduplication, developers can receive several tickets describing essentially the same problem, increasing noise and wasting remediation effort.

Application security platforms can correlate overlapping findings and consolidate them into a single actionable issue. This helps security teams reduce alert volume while retaining supporting evidence from multiple detection sources.

### Add Application, Deployment, and Runtime Context

A vulnerability’s practical importance depends on more than its theoretical severity. Security teams also need to understand where the affected code is deployed, whether vulnerable functionality is reachable, what data or systems could be exposed, and whether compensating controls reduce the likelihood of exploitation.

By enriching AI-generated and scanner-generated findings with application and runtime context, organizations can distinguish high-impact vulnerabilities from issues that are unlikely to create meaningful risk in the current environment.

### Prioritize Vulnerabilities Based on Actual Risk

AI-powered security tools may dramatically increase the number of potential vulnerabilities organizations can identify. Security teams therefore need mechanisms for determining which findings require immediate remediation and which can be addressed later.

Application security platforms can combine severity, exploitability, reachability, asset criticality, deployment context, and other risk signals to create more meaningful priorities. This helps prevent engineering teams from being overwhelmed as vulnerability discovery becomes increasingly automated.

### Route Remediation to the Right Developer

Finding a vulnerability is only useful if it reaches someone who can fix it. Application security platforms can map vulnerabilities to repositories, services, development teams, and code owners so remediation tasks are automatically assigned to the appropriate people.

Integrations with developer workflows can also deliver findings through existing issue trackers, source-control systems, and CI/CD pipelines, reducing the need for developers to work inside separate security tools.

### Govern AI-Generated Remediation

AI security systems may increasingly generate patches, configuration changes, or remediation recommendations automatically. These outputs should still pass through established development controls rather than being deployed directly into production without oversight.

Application security platforms can provide governance around AI-generated fixes by recording the source of the recommendation, tracking review and approval, maintaining audit trails, and ensuring that normal code review and testing processes remain in place.

### Measure Whether Vulnerabilities Are Actually Resolved

As vulnerability discovery becomes faster, security programs need to focus more heavily on remediation outcomes rather than simply counting findings. Teams should be able to track whether vulnerabilities were assigned, fixed, validated, reopened, or accepted as risk.

Application security platforms can provide metrics around remediation time, backlog reduction, recurring vulnerability patterns, and fix effectiveness. These measurements help organizations determine whether increased detection capability is actually producing a measurable reduction in application risk.

## How to Prepare Your Organization for Frontier Cybersecurity AI Models

Frontier cybersecurity AI models can dramatically increase the speed and scale of vulnerability discovery, but they can also create more findings than security and development teams can investigate manually. Preparing for these models therefore requires more than simply adding AI-powered scanning. Organizations need security processes that can absorb higher finding volumes, identify which issues are actually attackable, and move validated vulnerabilities through remediation without overwhelming developers.

Preparation should also account for the growing role of AI agents in software development. As coding agents gain the ability to modify files, install dependencies, and make changes autonomously, security controls need to operate earlier and continuously rather than relying only on post-commit scanning.

### Establish Continuous Security Testing Across the SDLC

Organizations should move from periodic security assessments toward continuous testing of code, dependencies, infrastructure-as-code, containers, secrets, and other software components. Both human-written and AI-generated changes should be evaluated as they are created.

Continuous testing becomes especially important as AI accelerates development and vulnerability research. Faster code generation and faster vulnerability discovery can quickly outpace manual review processes if security testing remains concentrated at the end of the development lifecycle.

### Combine Deterministic Detection With Frontier AI Analysis

Frontier models should complement rather than replace established application security testing. Deterministic techniques can provide repeatable detection of known vulnerability patterns, while frontier AI can reason across those results and identify weaknesses that may not have an existing rule, signature, or CVE.

A hybrid approach gives organizations a stronger foundation for adopting increasingly capable security models. Established scanning provides consistency and evidence, while AI reasoning can extend coverage into more complex or previously unknown vulnerability classes.

### Prepare for Much Higher Vulnerability Finding Volumes

More capable AI models can discover vulnerabilities faster than organizations can fix them. Security teams should therefore assume that the primary bottleneck may shift from finding vulnerabilities to deciding which findings matter and getting them remediated.

Organizations should establish automated triage, deduplication, risk scoring, and ownership workflows before introducing AI systems that substantially increase discovery volume. Without these controls, additional detection capability can simply create a larger backlog rather than reduce application risk.

### Prioritize Findings Using Reachability and Exploitability

Scanner severity alone becomes less useful when organizations are dealing with very large numbers of findings. Security teams should determine whether vulnerable functionality is actually reachable, whether exploitation is practical, and whether controls such as sanitization or masking prevent an attacker from reaching the vulnerable condition.

Adding this attackability context helps distinguish theoretical weaknesses from vulnerabilities that create concrete exposure in the deployed application. This allows organizations to focus remediation resources on findings most likely to reduce real-world risk.

### Integrate Security Directly Into Developer and AI-Agent Workflows

Security controls should operate where software is being created rather than forcing developers to wait for centralized scans. This includes IDEs, pull requests, CLI environments, CI/CD pipelines, and AI coding-agent workflows.

For autonomous coding agents, organizations may need controls that evaluate actions before they occur. For example, security systems can inspect file changes or dependency installations before an agent is allowed to continue, helping prevent risky actions from reaching the repository or executing on a developer system.

### Build a Governed Triage and Remediation Process

Frontier AI can help classify vulnerabilities and generate fixes, but organizations still need defined governance around how those decisions are applied. Security teams should determine when AI may automatically change a finding’s state, when human review is required, and how remediation recommendations move into development workflows.

Changes made by AI should also be traceable. Maintaining change logs, recording AI-driven triage decisions, and linking remediation activity to specific findings helps preserve accountability as more security work becomes automated.

### Keep Human Review in the Remediation Loop

AI-generated fixes can accelerate remediation, but a plausible-looking patch is not necessarily a correct security fix. Developers should be able to inspect, refine, approve, and test proposed changes before they are merged.

Organizations should also verify fixes by rescanning changed code to confirm that the original vulnerability is no longer present. This creates a closed remediation loop of detection, fix generation, review, and verification rather than treating code generation itself as proof that the issue has been resolved.

### Centralize Application Risk and AI Security Activity

As organizations adopt additional scanners, AI security agents, and coding assistants, security data can become fragmented across tools and teams. A centralized application risk view can correlate findings, apply consistent prioritization, and give security leaders visibility into what requires attention across the software portfolio.

Organizations should also track how AI is being used for triage and remediation, including the number of AI-assisted decisions, remediation activity, developer participation, and the resulting effect on vulnerability backlogs and remediation times.

## Preparing for Mythos-Class Threats with Checkmarx

Mythos-class models change the economics of application security in two ways. Attackers with similar capabilities can find unknown flaws and turn newly released patches into working exploits within hours, so a vulnerability that slips past detection is no longer a near-miss but an active risk. At the same time, AI-scale discovery floods security teams with findings, making triage and remediation the new bottleneck.

Preparing for this environment means detecting more real vulnerabilities before release, separating attackable risk from noise, and getting fixes merged faster. Checkmarx addresses each of these steps within a single platform.

**Checkmarx Fusion** is a hybrid scanning architecture within the Checkmarx One application security platform. It combines deterministic, rules-based scanning with several curated frontier AI models that analyze code in parallel. Its Findings Analysis Engine reconciles the results into a single, verified set of findings. 

Findings flow into Checkmarx One, where **Triage Assist** prioritizes them by attackability and **Remediation Assist** turns them into review-ready fixes. This helps AppSec teams find more real vulnerabilities, cut through scanner noise, and shorten the time between detection and a deployed fix.

**Key capabilities of Checkmarx Fusion and Triage & Remediation Assist:**

- 
**Hybrid deterministic and AI detection:** Depending on the language, a rules-based or AI-based engine runs first, and several curated frontier AI models are layered on top. Each model approaches the code differently, surfacing logic-based flaws and vulnerabilities with no known rule or prior CVE, including in AI-generated code and in languages that rules-based tools do not cover.
- 
**High-fidelity results with fewer false positives:** The Findings Analysis Engine removes false positives and deduplicates findings into one clean output. Checkmarx reports an F1 score of 0.74 for Fusion in SAST, compared with 0.64 for Next-Gen SAST and roughly 0.20 for query-based SAST, with 60–70% fewer false positives than standalone AI scanning.
- 
**Consistent results at a predictable cost:** Fusion runs a bounded, curated set of Checkmarx-validated models rather than open-ended frontier API calls, so repeated scans return the same answer. Teams can choose higher- or lower-cost models according to their needs, avoiding token bills that compound across every commit.
- 
**Portfolio-scale scanning:** Fusion can scan an entire project portfolio in a single operation, with results organized by project and comparable across codebases. Incremental scanning re-scans only new and changed code, so scans get faster over time.
- 
**Attackability-based triage:** Triage Assist classifies eligible SAST and SCA findings as False Positive, Acceptable Risk, or Action Required based on reachability, exploitability, policy, and code context. Automated triage rules keep the backlog current so teams focus on the issues that materially reduce exposure.
- 
**Review-ready remediation:** Remediation Assist turns prioritized risks into remediation guidance in Checkmarx One. For supported GitHub projects, it surfaces AI triage verdicts in pull requests and generates fix PRs for review. Nothing merges automatically, and developers stay accountable for final changes.
- 
**Unified risk view and measurable impact:** Fusion findings are scored and prioritized in Checkmarx One alongside every other signal in the portfolio. AI Assist Usage Analytics tracks adoption, developer time saved, and remediation activity. Checkmarx reports up to 95% faster MTTR and 65%+ of findings fixed on the first attempt.
- 
**Expanding scan coverage:** SAST is available now in Fusion, with Secrets Detection, IaC, API Security, and DAST being added on the same architecture. Fusion runs in CI/CD and pull request workflows on Checkmarx One.

Learn more about Checkmarx Fusion and Checkmarx Triage & Remediation Assist.
