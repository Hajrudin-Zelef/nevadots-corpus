---
id: collect-261121-veille-ia/securite/ai-powered-hacking-tool-knocks-the-european-commission-s-payroll-and-medical-systems-offline
title: "AI-powered hacking tool knocks the European Commission's payroll and medical systems offline"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["commission", "tool", "artex", "november", "first", "systems", "offline", "incident"]
source: https://aifrontierpost.com/articles/european-commission-artex-ai-cyberattack/
source_anchor: ""
source_lines: [1, 37]
section: "Securite"
---
Commission spokesperson Thomas Regnier confirmed the incident at a press briefing on November 19, almost four weeks after suspicious activity was first spotted. “As a protective and preventive measure, we temporarily took down several applications, and most of them have since then been restored,” he said.

## What went down#

The campaign was detected on October 22 and confirmed as a cyberattack on October 26, triggering mitigation steps and an investigation. It was not until November 19 — the day Politico first reported the incident — that the Commission spoke about it publicly.

The systems taken offline were the ones staff touch every day. Sysper, the human-resources platform used for payslips, leave requests and timesheets, went down, along with MyPMO, which handles medical reimbursements. Translation services were also hit, and Sysper reportedly crashed again a day after it was first restored. Staff could not book holidays or log their hours for much of the disruption, though salaries still arrived on time.

The incident reached the Commission's Corporate Management Board, its top internal crisis body, and tech commissioner Henna Virkkunen briefed her fellow commissioners. The European Data Protection Supervisor has been notified, and the Commission says it is working with CERT-EU, the cybersecurity service for EU institutions, while preventively checking its other systems.

## The tool at the centre of it: ARTEX#

Regnier confirmed the attack was “AI generated,” carried out with a publicly available tool: “AI assisted tools can be used and especially misused to detect vulnerabilities. Indeed we can confirm that this was done with AI tools.” He did not name the tool. Politico, citing CERT-EU's findings, identified it as ARTEX, a Chinese AI penetration-testing framework.

ARTEX was built for a legitimate purpose: helping security professionals find weaknesses before someone else does. Written in Go with a web console, it runs an offensive command-and-control platform that coordinates several AI agents and plugs into commercial language models. Give it an objective and its agents probe a network, test what they find, and decide the next step themselves, chaining small vulnerabilities into a path to compromise. The FBI says it is most effective against overlooked targets such as employee portals and legacy services — exactly the kind of system an internal HR tool is.

The framework has been in the wild since July 2026, when it first appeared on GitHub. Its developer, who posts as Autumn-27, pulled the repository on October 8 after reports of abuse — but forks, archived copies and a commercial ARTEX PRO edition keep it in circulation, and researchers had counted more than 1,500 exposed ARTEX control panels online by early November.

## The warnings came first#

The warnings were already public. On November 5, CERT-EU told EU bodies it had observed threat actors around the world using ARTEX to automate their campaigns. On November 13, the FBI issued a flash notice saying malicious actors had used ARTEX against organisations globally since at least August 2026. The Commission disclosed its own incident six days later.

ISACA's Chris Dimitriadis warned that AI lets attackers hack at the speed of intent, and said organisations will keep losing ground until they defend with AI as fast as criminals attack with it. The Commission is the case study: a tool anyone can download, aimed at a target with its own dedicated cyber defence team, still bought its operators more than ten days of disruption.

## Why this one matters#

The Commission runs one of the best-protected administrations in Europe, with its own cyber defence team. If an off-the-shelf tool can force it offline for more than a week, a hospital, a town hall or a mid-sized company is a softer target.

The affected platforms hold officials' names, pay grades, home addresses and contact details — records that could support targeted operations against individual staff even if no mass breach is ever confirmed. And disruption does not need data theft to count: taking payroll and medical systems offline for weeks is damage on its own.

Much is still unknown: no perpetrator named, no entry point disclosed, no account of how much of the work AI actually did versus a human operator. An EU official told Politico the Commission believes it has contained most of the attack, “but the full extent of any damage remains unclear.”

## What to watch#

Five things decide how big this becomes. First, whether the investigation finds staff data was accessed — “no evidence” so far describes what has been found, not proof nothing was taken. Second, whether the Commission formally names ARTEX or attributes the attack. Third, whether other EU institutions find ARTEX activity of their own after CERT-EU's warning. Fourth, whether the incident accelerates the EU's thinking on AI-assisted attacks. And fifth, what happens to ARTEX itself: with forks and a commercial edition in circulation, pulling one repository did not remove the tool — and the next campaign using it may not wait a month to surface.

**Sources:** Infosecurity Magazine, “European Commission Confirms AI-Driven Cyber-Attack Took Systems Offline,” November 19, 2026; EuropeSays, “Hackers Use FBI-Flagged ARTEX AI Tool to Knock EU Commission Staff Systems Offline,” November 2026; The Neuron, “EU AI Cyberattack: What Is ARTEX and How Does It Work?,” November 2026.
