---
id: collect-261121-veille-ia/securite/ten-projects-one-stack-why-the-aaif-consolidation-reframes-the-security-question
title: "Ten Projects, One Stack: Why the AAIF Consolidation Reframes the Security Question"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "projects", "security", "aaif", "agents", "foundation", "skills", "question"]
source: http://dev.to/saaro_net/ten-projects-one-stack-why-the-aaif-consolidation-reframes-the-security-question-60l
source_anchor: ""
source_lines: [1, 38]
section: "Securite"
---
On October 22, 2026, the Agentic AI Foundation (AAIF) added four more projects: Agent Skills (Anthropic), Open Responses (OpenAI), OpenEnv (Hugging Face), and OpenSandbox (Alibaba). The foundation, launched in December 2025 with three projects, now hosts ten after eleven months. Good for adopting teams – and a reason for a question that often gets overlooked: What does it mean when a large part of the trust assumptions of the agent stack is under one roof?

The AAIF is a project of the Linux Foundation, founded on December 9, 2025, with MCP, goose, and AGENTS.md as founding projects. By August 2026, it had grown to more than 250 members. The ten projects cover almost every layer of an agent operation: MCP for tool and data access, A2A for agent communication, Open Responses as a model interface, AGENTS.md and Agent Skills as instruction-carrying formats, agentgateway and Agent Router as a traffic control plane, goose as a runtime, and OpenEnv and OpenSandbox for evaluation and isolation. This is a map of dependencies.

MCP connects an agent with things that are not agents: databases, file systems, internal APIs. A2A connects an agent with another agent – potentially from another company, on another model, in another organization. A2A defines discovery via Agent Cards, task delegation, and result exchange.

A2A reached the stable v1.0 specification in March 2026 and has been a Growth Stage project of the AAIF since August 17, 2026 – in the same house as MCP, but with a separate specification. The foundation names more than 150 supporting organizations and production deployments in supply chains, financial services, and mobile platforms. Shared governance does not mean shared specification: the protocols remain independent.

Since October 1, 2026, there is the official A2A CLI – an interface for scripts, CI pipelines, and coding agents:


```
# Read the Agent Card of a remote agent
a2a card get https://agent.example.com
# Send a task and wait for the result
a2a send -a https://agent.example.com "Summarize this document"
```
JSON outputs and defined exit codes make it automatable; `a2a server --exec` turns any stdin/stdout program into an A2A server – for testing, not for production.

This is where the analysis by the Cloud Security Alliance (CSA) from October 29, 2026, begins: The AAIF solves the governance problem, not the security problem. The public project list lists ten projects without maturity or security labels. Unlike the CNCF, whose graduation requires an external security review, the CSA found no published foundation-wide baseline – no mandatory threat model, no uniform disclosure process, no third-party audit for all projects.

Added to this is a silent migration risk: The MCP revision from July 28, 2026, removed sessions at the protocol level, including `Mcp-Session-Id`. Tooling that correlates activity via session IDs or makes policy decisions during the initialization handshake can lose visibility without reporting an error. New attachment points are the headers `Mcp-Method` and `Mcp-Name` as well as OpenTelemetry trace propagation.

Most uncomfortable: the instruction-carrying formats. A2A v1.0 introduced signed Agent Cards, with which identity and advertised capabilities can be cryptographically verified. The Agent Skills specification defines mandatory fields for `name` and `description`, optional metadata, and an experimental `allowed-tools` field – but nothing for signature, hashing, publisher identity, or provenance. AGENTS.md is deliberately kept even lighter. Both carry instructions that an agent with significant rights executes – without built-in verification of who wrote them.

A Snyk study from February 2026 examined 3,984 skills from public marketplaces: 534 (13.4 percent) had at least one critical problem, 1,467 (36.8 percent) at least one security problem, 76 malicious payloads were manually confirmed. For MCP registries, an OX Security analysis across 15,465 servers found no consistent authorization model and no uniform lifecycle responsibility. Those who treat skills and servers as configuration rather than software dependencies have an open supply chain.

Two of the ten projects are gateways – a position with unusual leverage. agentgateway routes LLM traffic via an OpenAI-compatible API, supports MCP tool servers and A2A, and enforces JWT, API key, and MCP authentication as well as role-based authorization in CEL. Agent Router, originating from the Envoy AI Gateway and moved to the AAIF on September 10, 2026, connects applications with models and MCP tools and manages backend credentials via its own policy resources.

A gateway in front of agent traffic is a common recommendation. However, one that terminates MCP, A2A, and model traffic simultaneously becomes the point where a parsing error or a credential leak affects all protocols. Since OpenEnv provides MCP interfaces and OpenSandbox provides execution APIs, a vulnerability in shared components can affect the gateway, evaluation environment, and runtime at the same time. Coordinated cross-project vulnerability handling at the foundation level is not publicly documented.

First, an inventory: Which of the ten projects are present in your own environment, even indirectly via vendor products, and which revision does each component implement? AGENTS.md files and skills deserve the same attention as code – review on changes, installation only from an allowlist, scanning for embedded scripts and instruction injection. Then: route MCP and A2A traffic through a policy-enforcing gateway and patch the gateway itself; prefer signed Agent Cards for A2A and reject unsigned cards from external parties; rebuild monitoring from session IDs to headers and trace context; place agent code within an isolation boundary with minimal privileges.

The consolidation is strategically correct: It takes away manufacturers' ability to create lock-in via a proprietary connector and gives architecture committees a neutral contracting party. Only no security promise follows from this. The foundation manages ten artifacts with their own maintainers, release cycles, and security histories; the maturity is visibly uneven.

The next few months are crucial in two areas: whether Agent Skills gets a signature and provenance extension, comparable to A2A Agent Cards, and whether a foundation-wide baseline of threat model, disclosure process, and conformance tests emerges. As long as both are missing, the AAIF stack remains a map of dependencies – and an invitation to check each one yourself.

## Top comments (0)
