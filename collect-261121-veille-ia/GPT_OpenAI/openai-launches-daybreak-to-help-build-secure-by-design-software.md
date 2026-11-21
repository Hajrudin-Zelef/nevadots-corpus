---
id: collect-261121-veille-ia/gpt-openai/openai-launches-daybreak-to-help-build-secure-by-design-software
title: "OpenAI Launches ‘Daybreak’ to Help Build Secure By Design Software"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "daybreak", "software", "said", "security", "models", "help", "build"]
source: https://www.infosecurity-magazine.com/news/openai-daybreak-secure-by-design/
source_anchor: ""
source_lines: [1, 67]
section: "GPT_OpenAI"
---
OpenAI has announced Daybreak, a new initiative based on its frontier large language models (LLMs) and its AI-coding assistant, Codex, to help developers build secure software from the ground up.

Unveiled on May 12, OpenAI said Daybreak builds from its Trusted Access for Cyber (TAC) program, a scheme that reserves access to certain frontier models to a selective number of organizations.

The initiative already includes three of OpenAI’s latest models: the general-purpose version of GPT‑5.5; GPT‑5.5 with TAC, which offers more precise safeguards for verified defensive work in authorized environments; and GPT‑5.5‑Cyber. It also features Codex Security, a code‑review assistant based on Codex that is currently available only as a research preview.

Where the TAC program is primarily focused on vetted users tapping into LLMs to identify and fix vulnerabilities, Daybreak aims to tackle the vulnerability problem from the start of the software development lifecycle.

Speaking to *Infosecurity*, Willie Tejada, SVP & GM of Cloud Native Security Fabric at Aviatrix, explained that OpenAI's press release is intentionally broad because Daybreak is "a platform play, not a model announcement."

He said the initiative aims to help cyber defenders do three things: build an editable threat model of a given code repository focused on realistic attack paths, discover and test vulnerabilities in an isolated environment and propose and validate patches directly in the repo.

"The pitch is that it compresses hours of manual security analysis into minutes," Tejada added.

In a series of short videos posted on social media, OpenAI shared some of the tasks that software developers and cybersecurity defenders can perform as part of the initiative. These include:

- Scanning a codebase using Codex Security’s 10 subagents, identifying vulnerabilities, fixing them and adding regression tests
- Triaging vulnerability backlog, prioritizing vulnerabilities that should be fixed (e.g. by severity, impact or exploitability) and deploying agents to open pull requests
- Automating vulnerability detection, validation and response (e.g. looking for the latest CVEs, deploying an agent to investigate their impact on the business, searching logs for exploitation evidence)

“The goal is simple: accelerate cyber defenders and continuously secure software,” the OpenAI announcement said. “Because those same capabilities can be misused, Daybreak pairs expanded defensive capability with trust, verification, proportional safeguards and accountability.”

According to Tejada, Daybreak is "OpenAI's bid to own the security developer toolchain the same way GitHub Copilot captured the coding assistant market."

The company also said it will soon deploy new “cyber-capable models” in cooperation with industry and government partners.

As of May 2026, OpenAI said its TAC program includes hundreds of organizations and "thousands of individual defenders."

These include IT and cybersecurity organizations like Akamai, Cisco, Cloudflare, CrowdStrike, Fortinet, NVIDIA, Oracle, Palo Alto Networks, Sophos and Zscaler.

The TAC also includes large enterprises, especially in finance and private equity, such as Bank of America, BBVA, BlackRock, BNY, Citibank, Goldman Sachs, JPMorgan Chase, Morgan Stanley and US Bank.

While only a handful of government-linked research organizations, like the US Center for AI Standards and Innovation (CAISI) and the UK AI Security Institute (UK AISI) are currently part of the TAC program, OpenAI confirmed in early May its intention to expand it to more government agencies.

Anthony Grieco, SVP, chief security and trust officer at Cisco, believes frontier models like GPT 5.5 are “powerful force multipliers for defenders.”

“They are fundamentally changing the velocity of our operations, enabling us to move faster on everything from incident investigation to proactive exposure reduction,” he said.

“But speed cannot be traded for trust. The true value of this technology isn't found in the model alone, but in the enterprise-ready framework we wrap around it. A framework that helps us make more secure products. Our focus is on transforming our secure development and operations processes with these new capabilities. For us, it's about enabling innovation that is as reliable as it is fast.”

## **Experts Raise Concerns About AI-Powered Vulnerability Research** 

While many experts regard the launch of Daybreak as a step in the right direction in a bid to use frontier AI models to help fix vulnerabilities alongside other software development tasks, it also raised a lot of concerns.

David Stuart, a cybersecurity evangelist at data security solutions provider Sentra, warned that to unlock the capabilities of frontier AI agents, organizations must grant these systems access to their environment.

“That may include code repositories, infrastructure configurations and build pipelines. Before introducing these tools, organizations need to understand what sensitive data lives in those environments and whether it is governed well enough for an AI agent to interact with it,” he said.

“The same access that makes these tools useful also makes them part of the data attack surface. That governance work needs to happen before the agent is deployed.”

Meanwhile, Andrew Wesie, a vulnerability researcher and CTO of AppSec company Xint.io, said that vulnerability researchers should also ensure they’re aware of the details of security offerings from GenAI companies like OpenAI and Anthropic if they don’t want to be trapped in an expensive ecosystem that will lock them in.

“For example, how many tokens are burned during [Daybreak] assessments, what is the false positive rate and how will pricing work for enterprise code bases that have millions of lines of code? Without this information, it’s hard to know if teams should build their AppSec pipelines around monolithic models,” he cautioned.

Many believe the democratization of AI-powered vulnerability research is to be welcomed. However, Melissa Bischoping, head of threat research and intelligence at Tanium, warned it also “tightens the bottleneck around remediation.”

As software companies find and develop bug fixes at an “unprecedented pace,” consumers of the software may not necessarily be ready to deploy patches.

“Many organizations today still struggle with the ‘old way’ of monthly patching at the scale of the last few years. That ship has sailed, and we’ve got to rethink and rebuild our patching systems for this era,” she said.

She argued that patch management teams will have to deal with “dozens or hundreds of micro-patches per week” as bugs are uncovered.

Clyde Williamson, a senior product security architect at Protegrity, which provides data security solutions designed for AI workflows, noted that finding vulnerabilities has never been the hardest problem, prioritization is.

*This article was updated on May 13 to add comments from cybersecurity professionals.*

*Image credits:  Thrive Studios ID / TY Lim / Shutterstock.com*
