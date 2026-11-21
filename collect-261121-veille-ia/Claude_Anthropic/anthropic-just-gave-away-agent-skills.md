---
id: collect-261121-veille-ia/claude-anthropic/anthropic-just-gave-away-agent-skills
title: "Anthropic Just Gave Away Agent Skills"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2025-12-18"]
keywords: ["skills", "agent", "anthropic", "standard", "just", "claude", "code", "open"]
source: https://dev.to/max_quimby/anthropic-just-gave-away-agent-skills-4g3k
source_anchor: ""
source_lines: [1, 136]
section: "Claude_Anthropic"
---
On October 22, 2026, Anthropic announced it was donating the Agent Skills standard — the name, the logo, the GitHub organization, and the agentskills.io domain — to the Agentic AI Foundation (AAIF) under the Linux Foundation. One million skills published. Nearly 280 million installed. Over 70 agent platforms integrated, including ChatGPT, Cursor, GitHub Copilot, and VS Code.

This is not philanthropy. This is the MCP playbook running for the second time. And if you build, ship, or consume agent skills, you need to understand what just happened — and what it means for you.

Boris Cherny, Anthropic's Product Manager for Agent Skills, shared the news on X with 2,249 likes and 177K views: "Agent Skills started as an experiment in Claude Code. Many across the industry have come to adopt it, so we at Anthropic found it more fitting to donate it to the @AgenticAIFdn, next to MCP."

ℹ️ **By the Numbers:** Anthropic reports 1 million published skills, 280 million installs, and 70+ integrated platforms since the December 2025 open standard launch — just 10 months of adoption.


Anthropic has now run the same strategy twice. The pattern is unmistakable:

**Step 1 — Build it proprietary.** Agent Skills launched in October 2025 as a Claude Code feature. Skills were folders of instructions, scripts, and resources that Claude could discover and load dynamically. They worked great — but only on Claude.

**Step 2 — Open the standard.** On December 18, 2025, Anthropic published the specification as an open standard with a reference SDK at agentskills.io. OpenAI added Agent Skills support to ChatGPT and Codex within two months. Cursor, GitHub Copilot, VS Code, Gemini CLI, and more than a dozen others followed. By March 2026, the ecosystem report counted 16+ platforms and hundreds of thousands of skills on public directories.

**Step 3 — Donate governance to a foundation you co-founded.** The AAIF is the same foundation Anthropic established with Block and OpenAI in December 2025 — the one that already hosts MCP. Engineers from Anthropic, Google, OpenAI, and other companies now maintain the standard, with David Soria Parra and Mahesh Murag as lead maintainers. The governance moved. The spec-setters stayed.

Simon Willison called it in December 2025: when Anthropic first released skills as an "open standard," he predicted they would end up in the AAIF. Ten months later, that is exactly what happened. MCP to AAIF in December 2025. Agent Skills to AAIF in October 2026. The playbook is legible.

The Hacker News community discussed the open standard when it launched, with debate around whether a markdown-based spec could scale to enterprise use cases:

The broader industry convergence on agent standards also drew attention, with commenters noting that five major AI companies agreeing on a single standard is historically unusual:


Want proof that skills have crossed from experiment to infrastructure? Open GitHub Trending today. Five of the top 15 repositories are skills or skill-adjacent:

| Rank | Repository | Stars | What It Is | 
|---|---|---|---|
| #1 | cloudflare/security-audit-skill | 41,497 | Multi-phase security audit skill — turns any coding agent into a security auditor | 
| #2 | Graphify-Labs/graphify | 133,761 | Knowledge-graph skill for Claude Code, Cursor, Codex, Gemini CLI | 
| #12 | video-shotcraft | 13,199 | AI video production skill for Claude Code and Codex | 
| #13 | SnailSploit/Claude-Red | 8,255 | Offensive security skills library for Claude | 
| #15 | genspark-ai/genoffice | 10,644 | AI Office suite with agent skill integration | 

That is not a coincidence. A third of the most-starred new repositories on the planet are built on a spec that Anthropic just donated. The standard has become the default packaging format for agent capabilities — and Cloudflare's security-audit-skill reaching #1 with 41K stars is especially notable because it demonstrates enterprise adoption, not just developer hobbyism. Cloudflare runs the underlying harness against 128 internal repositories continuously, using a six-phase architecture where the agent that finds a vulnerability is never the agent that confirms it.

We tracked the skills marketplace land grab when repos started racing past 250K stars in September. The AAIF donation is not the cause of this adoption — it is the consequence. Anthropic waited for the standard to win, then cemented the win with institutional governance.

The comparisons to MCP are obvious. Both started as Anthropic products. Both became open standards. Both landed in the AAIF. But the skills donation is different in one critical way: **the security surface is far larger.**

MCP connects agents to data sources and tools. A malicious MCP server is dangerous, but the attack requires the user to explicitly connect to it. Skills are different. Skills are markdown files that agents are designed to trust and follow. They load implicitly based on matching context — often without the user choosing them specifically. A skill does not need to trick a firewall or exploit a CVE. It just needs to be in the right folder when the agent starts.

The Snyk ToxicSkills audit in February 2026 quantified the problem: out of 3,984 skills scanned on ClawHub and skills.sh, **76 contained confirmed malicious payloads**, 534 (13.4%) had critical-severity issues, and 1,467 (36.82%) had at least one security flaw. The attack patterns are sophisticated — base64-encoded exfiltration targeting AWS credentials, systemctl backdoors, and prompt injection combined with malware in 91% of malicious samples. Three named threat actors (zaycv, Aslaep123, aztr0nutzs) were publishing malicious skills at scale.

The Cloud Security Alliance's CISO Briefing framed it bluntly: "The attack surface is not a code path — it is a document that agents are designed to trust and follow." A separate typosquatting campaign, "clawdhub," targeted 341 skills with reverse shells and data exfiltration payloads aimed at OpenClaw users.

OWASP responded by creating an entire Agentic Skills Top 10, with AST01 — Malicious Skills — at the top. Their analysis found a separate campaign, ClawHavoc, with 1,184 malicious skills across 12 publisher accounts. The core problem: agent skills execute with the full permissions of the host agent — shell access, API keys, filesystem writes. We analyzed this exact attack surface in our skills supply-chain security deep dive, and the skills-vs-MCP architecture comparison explains why the trust models differ so fundamentally.

⚠️ **The Unsolved Problem:** Anthropic's proposed roadmap includes "security guidance for skill authors and installers," but the standard itself has no signing, no provenance chain, and no runtime sandboxing. The iOS App Store had review gates from day one. Skills directories have GitHub stars and README files. We explored this gap in depth in our audit analysis.


Let us be precise about what the AAIF donation does and does not change.

**What changes:**

- 
**Spec ownership** moves from Anthropic's GitHub org to the AAIF. No single company can unilaterally change the standard.
- 
**Formal process** for spec changes: Anthropic proposed versioned releases and a proposal process (similar to Python PEPs or TC39 for JavaScript).
- 
**Multi-vendor maintainers:** Engineers from Anthropic, Google, OpenAI, and others maintain the standard in their individual capacities — not as corporate delegates.
- 
**Legitimacy signal** for enterprises considering adoption. Foundation governance under the Linux Foundation is the same model that governs Kubernetes, Node.js, and GraphQL.

**What does not change:**

- 
**No security framework.** The proposed roadmap includes "security guidance" but no mandatory signing, scanning, or audit requirements.
- 
**No quality gates.** The spec defines a format, not a minimum quality bar. The ecosystem benchmarks show the average public skill scores 6.2 out of 12 on quality metrics.
- 
**No registry governance.** Skills.sh has 288K development skills. SkillsMP indexes 1.9 million scraped from GitHub. Neither has security review. The AAIF governs the spec, not the distribution channels.
- 
**Anthropic's influence.** The company that designed the standard, built the reference implementation, and co-founded the foundation will continue to shape its direction. The lead maintainers are Anthropic engineers. This is open governance, not absent governance.

As the Dev.to analysis noted: "Foundation governance should make it harder for one vendor to change the rules. But MCP was already being adopted before the donation, so a foundation home alone does not drive adoption." The same applies here. The standard won on merit — the governance change makes the win durable.

The raw numbers are staggering. The Agent Skills Ecosystem Report maps the current landscape:

- 
**Skills.sh** launched in January 2026 and now hosts 288,811 development skills alone, with 86,948 in product management and 74,510 in marketing.
- 
**SkillsMP** indexes 1.9 million skills scraped from public GitHub — no security review.
- 
**SkillHub** offers 7,000+ AI-evaluated skills.
- 
**Agensi** runs a curated marketplace with 8-point security scans and a 70/30 revenue split for creators.
- 
**Anthropic's official directory** remains small and manually curated — quality over quantity.

Roughly 40 products sit on the agentskills.io showcase as of mid-2026, spanning coding agents (Codex, Copilot, Cursor, Gemini CLI), open-source agents (Goose, OpenCode, Amp), and enterprise platforms (Databricks Genie Code, Snowflake Cortex Code). A Digital Applied survey found that 68% of production agent deployments have adopted MCP or an equivalent tool standard. Skills are following the same adoption curve.

But distribution has outpaced security. The gap between "1.9 million indexed skills" and "76 confirmed malicious" is not a safety margin — it is an audit deficit. The confirmed malicious skills were found by Snyk in a sample of just 3,984 out of nearly 2 million. Scale the detection ratio and the number gets concerning fast.


If you are building or shipping agent skills today, the governance change creates a concrete set of implications:

💡 **The Practitioner Playbook:**


**The spec is stable enough to bet on.** Foundation governance signals that the SKILL.md format, the YAML metadata schema, and the progressive-loading model are not going to change underneath you. If you have been holding off because the standard was "Anthropic's thing," that objection is gone.

**Security is your problem, not the spec's.** The AAIF governs the format, not the supply chain. If you install skills from public directories, you need your own vetting process. Snyk's `uvx mcp-scan@latest --skills` is a starting point. Check our audit landscape analysis for a deeper look at vetting options.

**The directory land grab is wide open.** With 1.9 million skills indexed and no dominant curated marketplace, the opportunity is in quality, not quantity. Agensi's security-scan-plus-revenue-split model, SkillsBench's quality benchmarks, and Cloudflare's validation approach all point toward the same thesis: curation wins.

**Cross-platform is now the default.** Skills that only work on one agent are leaving adoption on the table. The spec is implemented by 70+ platforms — design for portability and test on at least three agents before publishing.

**Learn the playbook.** Anthropic's strategy — build proprietary, prove value, open the standard, donate governance — is the template for setting AI infrastructure standards. If you are building anything that could become a standard (protocols, formats, runtime conventions), this is the path to adoption without getting forked.


Here is what makes Anthropic's approach effective, and why it keeps working:

**Lock-in without lock-in.** By the time skills reached the AAIF, 70 platforms had already built integrations against Anthropic's specification. Donating governance does not reduce Anthropic's influence — it makes the influence permanent. The spec will evolve under the AAIF, but it will evolve from the foundation Anthropic laid. Every platform that adopted the standard is now building on Anthropic's design decisions.

**The MCP precedent removes objections.** When MCP was donated to the AAIF in December 2025, skeptics asked whether foundation governance was real or performative. Ten months later, Google's A2A protocol joined as a hosted project, and the AAIF membership grew past 250 organizations including Google, Microsoft, AWS, Cloudflare, and Bloomberg. The foundation has weight. Donating Agent Skills to the same body is a lower-risk bet for everyone involved.

**Timing matters.** Anthropic did not donate early, when the standard needed credibility. It donated after adoption proved the standard works. The million-skill, 280-million-install numbers are the justification — not the goal — of the donation. This is a victory lap with institutional clothing.

The proposed roadmap includes versioned releases, a formal proposal process, and security guidance. Feedback goes through the project's Discord. Three predictions for the next 12 months:

**Prediction 1: Security scanning becomes mandatory in major directories.** The Snyk and OWASP findings have made the problem undeniable. Expect Skills.sh and SkillsMP to add at minimum automated scanning — and expect enterprises to require it. The skills standard itself may gain a `security` metadata field for declaring permissions and data access.

**Prediction 2: The AAIF becomes the de facto governance body for the agent infrastructure stack.** MCP (data and tools), Agent Skills (capabilities), A2A (inter-agent delegation) — the foundation now hosts all three layers. Whoever proposes the next protocol addition has a clear path: the AAIF.

**Prediction 3: Skills specialization accelerates.** The scientific skills library pattern was early evidence that domain-specific skills outperform generic ones. With foundation governance signaling long-term stability, expect more vertical investment — legal, medical, financial, compliance — from organizations that were waiting for the standard to settle. The ecosystem already has 382K downloads on some bundles, and the race to own vertical niches is just beginning.

The Agent Skills standard is now infrastructure. It is governed by a foundation, implemented by 70+ platforms, and stress-tested by 280 million installs. Anthropic set the spec, proved it worked, and handed over the keys — to a building it helped design. That is not generosity. That is strategy. And it is the playbook for anyone building the next piece of AI infrastructure.

*The Agent Skills donation was announced on October 22, 2026. GitHub Trending data is from the AgentConn research digest for October 23, 2026.*

*Originally published at AgentConn*

## Top comments (1)

At a million skills, supply stops being the bottleneck and trust starts being it. A skill description is a claim, not evidence — the check that matters is reading what the skill says it will not do, then running it once on a real task before it becomes part of your daily setup. The standard being open helps everyone write skills; it does not help anyone choose between them, and that choosing step is now the whole job.
