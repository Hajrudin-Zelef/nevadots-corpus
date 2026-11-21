---
id: collect-261121-veille-ia/grok-xai/european-commission-cyberattack-artex-ai-tool-suspected-in-prolonged-disruption
title: "European Commission Cyberattack: ARTEX AI Tool Suspected in Prolonged Disruption"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["commission", "artex", "tool", "data", "european", "november", "staff", "confirmed"]
source: https://securityaffairs.com/203483/hacking/european-commission-cyberattack-artex-ai-tool-suspected-in-prolonged-disruption.html
source_anchor: ""
source_lines: [1, 27]
section: "Grok_xAI"
---
A cyberattack has disrupted internal European Commission systems for more than ten days, hitting payroll, leave requests and medical reimbursements. Politico’s Brussels Playbook **reports** the Corporate Management Board, the Commission’s top internal crisis body, met urgently over the incident, and tech commissioner Henna Virkkunen briefed the full College of Commissioners on 18 November. An EU official told Politico the Commission believes it has contained most of the attack, while the full extent of any damage remains unclear.

Sysper, the HR platform for payslips, timesheets and leave, crashed briefly again on 18 November after access had been restored earlier in the week. MyPMO, the medical reimbursement system, came back online around the same time. JSIS, the staff health insurance portal, was still down on 19 November according to a Commission employee cited by Euronews, and translation services went offline on 17 November, prompting one official to admit to “swearing a lot.”

Pay still landed on time, according to staff Politico spoke to, even as some couldn’t submit timesheets or request leave. Commission spokesperson Thomas Regnier confirmed the response to Politico: *“Individual systems were targeted and, in line with our standard practice, we have informed those concerned.”* 

Some staff still complained the internal communication said little, and an IT reply about the Sysper problem cited a general technical issue without mentioning an attack.

CERT-EU, the cybersecurity team that protects EU institutions, detected the activity and **identified** ARTEX, an AI-powered penetration-testing tool, as a possible tool used in the attack. An EU official told Politico that some threat actors misuse tools like ARTEX and have targeted the Commission. In a technical note published on 5 November, CERT-EU described ARTEX as a system that uses AI agents. An operator gives it an objective in plain language, and the software breaks it down into smaller tasks that the agents carry out using real hacking tools.

ARTEX does not include its own AI model. Instead, it connects to an external AI model selected by the operator. Its original developer removed the project from its repository in October after reports of misuse. However, independent copies are still available, so removing the original project does not prevent others from using it. So far, there is no public information explaining how the attackers entered the Commission’s systems. The reference to ARTEX therefore points to a suspected tool, not a confirmed attack method or the identity of those responsible.

The biggest unanswered question is whether staff data was stolen. This could include names, salary details, home addresses or health information. Investigators have not yet confirmed whether anyone accessed this data, which explains why the Commission has shared little information with employees.

Taking systems offline or restoring them does not prove that no data was stolen. Getting a service back online only shows that it is working again. It does not mean attackers did not copy information before the system was shut down.

Under EU Regulation 2018/1725, a confirmed personal data breach must normally be reported to the European Data Protection Supervisor within 72 hours. If the breach creates a high risk for individuals, they must also be informed without delay. However, these deadlines only start once a personal data breach has been confirmed.

This is not the Commission’s first security incident this year, although no link between the cases has been established. On January 30, attackers left traces in the system used to manage staff mobile devices. Some names and phone numbers were exposed. The Commission said it contained and cleaned up the incident within nine hours.

A more serious incident happened in March. Attackers compromised a software update for Trivy, a security tool, and used it to enter an AWS cloud environment. They stole around 91.7 GB of compressed data, including personal information and email content. The breach may also have affected at least 29 other EU institutions and bodies.

The timeline shows that the Commission restored its systems before it fully understood what had happened. Services came back online in stages, but key questions remain unanswered: where did the attack start, which systems were affected, and was any data stolen, and who is behind the attack?

**Follow me on Twitter:** **@securityaffairs** **and** **Facebook** **and** **Mastodon**

**(****SecurityAffairs** **– hacking, European Commission)**
