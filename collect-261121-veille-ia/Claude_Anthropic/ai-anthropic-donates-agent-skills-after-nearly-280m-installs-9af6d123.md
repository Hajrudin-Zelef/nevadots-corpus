---
id: collect-261121-veille-ia/claude-anthropic/ai-anthropic-donates-agent-skills-after-nearly-280m-installs-9af6d123
title: "Ai anthropic donates agent skills after nearly 280m installs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["anthropic", "skills", "agent", "standard", "foundation", "says", "million", "linux"]
source: ""
source_anchor: ""
source_lines: [1, 64]
section: "Claude_Anthropic"
---
Anthropic said on 22 October 2026 that it is donating Agent Skills, the open standard that packages instructions and scripts an AI agent can load for a task, to the Agentic AI Foundation (AAIF) under the Linux Foundation. Anthropic says 1 million skills have been published and nearly 280 million installed, and that more than 70 agent platforms, including ChatGPT, Cursor, GitHub Copilot and VS Code, have integrated the standard.
The donation covers the Agent Skills name and logo, the GitHub organization and the agentskills.io domain, Anthropic says. It follows Anthropic’s December 2025 donation of the Model Context Protocol (MCP) to AAIF, which Anthropic co-founded with Block and OpenAI.
Business Pill · CONTEXT ENGINEERING
A short explainer on what a model sees before it answers. It teaches the general idea only and says nothing about any company in this piece.
The key insight: As we read it, the new figure is the install count: Anthropic says nearly 280 million skills have been installed from 1 million published, and it is now handing the standard to a foundation it co-founded with Block and OpenAI.
What Anthropic Announced
- The standard. “A skill is a folder containing a SKILL.md file that teaches an agent a specific task or domain, along with any scripts or reference material it needs,” Anthropic writes. It says it developed skills while working on Claude Code.
- The timeline. Anthropic says it launched skills in October 2025 and then published Agent Skills as an open standard; the standard was published in December 2025, it says.
- The donation. Anthropic says it is donating the Agent Skills name and logo, the GitHub organization and the agentskills.io domain, and that people can keep using or contributing to the standard without joining the AAIF.
- The maintainers. “Engineers from Anthropic, Google, OpenAI, and other companies will maintain the standard as individuals, with David Soria Parra and Mahesh Murag as Lead Maintainers,” Anthropic writes.
- The roadmap. Anthropic says it has proposed starting with versioned releases, a formal proposal process, and security guidance for people who write and install skills.
What Was Posted on X
Posting on 22 October, @choblin29 summarised the handover: Anthropic gives Agent Skills to the Linux Foundation’s Agentic AI Foundation after 1 million skills were published and nearly 280 million installed, with more than 70 agent platforms supporting it. The post notes it is the second major agent standard Anthropic has put under the foundation after MCP.
The Numbers
Anthropic and the Linux Foundation give the adoption figures below. The table lists them as stated, next to the figures Anthropic gave for MCP when it donated that standard.
| Measure | Figure | Source | 
|---|---|---|
| Agent Skills: skills published | 1 million | Anthropic, 22 Oct 2026 | 
| Agent Skills: skills installed | Nearly 280 million | Anthropic, 22 Oct 2026 | 
| Agent Skills: agent platforms integrated | More than 70 | Anthropic, 22 Oct 2026, including ChatGPT, Cursor, GitHub Copilot and VS Code | 
| MCP at its donation: active public servers | More than 10,000 | Anthropic, 9 Dec 2025 | 
| MCP at its donation: monthly SDK downloads | 97M+ | Anthropic, 9 Dec 2025, across Python and TypeScript | 
| AAIF projects | 10, up from 3 | Linux Foundation, 22 Oct 2026 | 
| AAIF member organizations | 305 | Linux Foundation, 22 Oct 2026 | 
Figures as stated by Anthropic and the Linux Foundation. The MCP figures are from Anthropic’s post of 9 December 2025, when it donated MCP; the Agent Skills figures are from its post of 22 October 2026. The two standards measure different things and the rows are not comparable as a ranking.
Four Projects in One Day
Agent Skills was one of four projects the AAIF announced on 22 October 2026, at AGNTCon + MCPCon North America in San Jose, according to the Linux Foundation. The others are Open Responses, contributed by OpenAI; OpenEnv, contributed by Hugging Face; and OpenSandbox, contributed by Alibaba. The additions bring AAIF to 10 hosted projects in its first year, up from its three launch projects in December 2025, the foundation says.
“Without the right context, even the most intelligent agents fall short,” David Soria Parra of Anthropic said in the Linux Foundation release. The same day, the foundation said its membership had reached 305 organizations, with 60 new member organizations joining this quarter.
The Security Question
VirusTotal has published figures on malicious skills. In Part III, published in October 2026, of a series that began in February 2026, it said it had analysed 35,878 AI agent skills submitted to the service and found that “62% of the 6,637 malicious skills contain no malicious code”, because their payload is written as plain-language instructions inside the skill documentation. VirusTotal’s sample is skills submitted to its service, not a sample of the 1 million published skills, so the two figures should not be combined.
Our piece on October’s supply-chain incidents carries VirusTotal’s figures in full, and its advice to defenders.
The Arithmetic
- About 280 installs per skill. Nearly 280 million installs across 1 million published skills is about 280 per skill on average, by our arithmetic. Anthropic does not give the distribution of installs across skills.
- AAIF more than tripled its projects. From 3 to 10 hosted projects in its first year, by our arithmetic from the Linux Foundation’s figures.
- About 18.5% of VirusTotal’s sample was flagged. 6,637 of 35,878 analysed skills, by our arithmetic. This describes VirusTotal’s submissions only.
Connecting the Coverage
- Our piece on AI labs as services firms covered how OpenAI, Anthropic and others are selling deployment help, from forward-deployed engineers to partner networks.
- Our piece on METR’s audit of OpenAI covered how outside researchers tested the monitoring of internal coding agents.
The Structural Read
As we read it, Anthropic is repeating the path it took with MCP: publish an open standard, let other platforms adopt it, then donate it to the Linux Foundation’s AAIF. Its post says more than 70 agent platforms, including ChatGPT and GitHub Copilot, have integrated Agent Skills.
As we read it, the foundation is becoming a shared home for rival labs’ standards. On the same day, the Linux Foundation said OpenAI, Hugging Face and Alibaba also contributed projects, bringing AAIF to 10.
As we read it, security is the open item. Anthropic’s proposed roadmap includes security guidance for people who write and install skills, and VirusTotal has published figures on malicious skills in its own sample.
Anthropic, 22 October 2026
“We’re committed to keeping Agent Skills open, community-driven, and vendor-neutral.”
Three Implications
FOR AGENT PLATFORMS Anthropic says people can use or contribute to Agent Skills without joining the AAIF, and that maintainers from Anthropic, Google and OpenAI will work on it as individuals.
FOR COMPANIES WRITING SKILLS Anthropic’s proposed roadmap includes versioned releases and a formal proposal process, so changes to the format would follow a published process.
FOR SECURITY TEAMS VirusTotal says 62% of the malicious skills in its sample contain no malicious code, only plain-language instructions; Anthropic’s proposed roadmap includes security guidance for people who write and install skills.
The Business Engineer Lens
This story maps onto the Business Engineer framework The Agentic Harness War.
The framework says: “If the SKILL.md is the line of code of the agentic era, then the format war is the platform war.” It names the load-bearing question as whether a skill is portable or locked to a harness.
As we read it, donating Agent Skills to a vendor-neutral foundation is a bet on the portable answer to that question. The framework was published in July 2026 and does not discuss this donation.
What Is Not Established
The adoption figures are Anthropic’s own and we have not seen an independent count. Anthropic does not say how many of the nearly 280 million installs are active, how many skills account for most installs, or how much revenue, if any, it earns from skills. The roadmap items are proposals, not adopted rules. VirusTotal’s figures describe skills submitted to its service and are not a measure of the published skills as a whole.
The Bottom Line
Anthropic is donating Agent Skills, which it says has 1 million published skills, nearly 280 million installs and more than 70 integrating platforms, to the Linux Foundation’s Agentic AI Foundation, about 10 months after it donated MCP there, by our count. The foundation now hosts 10 projects, including standards contributed by Anthropic, OpenAI, Hugging Face and Alibaba, and Anthropic says the proposed roadmap includes security guidance for people who write and install skills.
95,000+ executives read Business Engineer for the AI strategy frameworks cited by ChatGPT, Claude, and Perplexity.
A note on sourcing. This piece is based on Anthropic’s 22 October 2026 post and the Linux Foundation’s release on its four new projects, read in full; the opening of its release on membership; Anthropic’s 9 December 2025 post on MCP; the opening of VirusTotal’s October 2026 post on malicious skills, without technical detail; and our own earlier pieces. The adoption figures are Anthropic’s own. The per-skill average, the project growth, the 18.5% and the month count are our arithmetic. Nothing here is investment advice.
Unlock the rest of this article
You are reading Anthropic Donates Agent Skills After Nearly 280M Installs. Add your email to read on, and the book is yours too.
- The loop as the unit of work
- Goodhart is the physics
- Six properties, each answering a failure
Join 95,000+ operators reading FourWeekMBA.
