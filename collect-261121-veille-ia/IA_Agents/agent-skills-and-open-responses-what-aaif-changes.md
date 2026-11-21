---
id: collect-261121-veille-ia/ia-agents/agent-skills-and-open-responses-what-aaif-changes
title: "Agent Skills and Open Responses: What AAIF Changes"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "foundation", "four", "skills", "responses", "projects", "open", "anthropic"]
source: https://www.digitalapplied.com/blog/agent-skills-open-responses-aaif-portability
source_anchor: ""
source_lines: [1, 131]
section: "IA_Agents"
---
The Agentic AI Foundation, the Linux Foundation body that already governs the Model Context Protocol, announced on October 22, 2026 that it has taken on four more projects, bringing its total to ten in its first year. Anthropic contributed Agent Skills, the SKILL.md folder format; OpenAI contributed Open Responses, a multi-provider request and response specification based on its Responses API; Hugging Face contributed OpenEnv, a framework for reinforcement learning environments; and Alibaba contributed OpenSandbox, a sandbox platform for running agents. The announcement was timed for the foundation's AGNTCon and MCPCon conference in San Jose.

For a team choosing agent tooling the news lands as a question about portability. If a skill, a request format, a training environment and a sandbox all live under one neutral roof, does that mean they will work together, and that work built on one will move to another? The honest answer is layered. Neutral governance changes who controls a specification's future and lowers the risk that a single vendor bends it toward its own product. It does not by itself make a skill's scripts run in a second agent, a tool name resolve on a second platform, or a prompt produce the same output from a second model. Those seams stay where they were.

This post works through each of the four projects by the layer it occupies, using the foundation's own framing: how agents gain capabilities, how applications talk to models, how agents are trained and evaluated, and where they run. For each it describes what the specification actually fixes, where the portable part ends, and what to check before standardising on it. We covered the foundation's December launch with its first three projects; this is the first time its roster spans the whole stack.

1. 01Ten projects, four layers, four different contributors.The Linux Foundation's release lists Agent Skills (Anthropic), Open Responses (OpenAI), OpenEnv (Hugging Face) and OpenSandbox (Alibaba) joining A2A, Agent Router, agentgateway, AGENTS.md, goose and MCP. The total is ten, not three plus four from one company.
2. 02What moved is control, not compatibility.Anthropic transferred the Agent Skills name, logo, GitHub organisation and domain, and the standard is maintained by engineers from several companies as individuals. Nothing in that transfer makes a skill written for one agent run unchanged in another.
3. 03A skill's portable core is two text fields and a Markdown body.The specification requires only a name and a description; scripts depend on the agent's runtime, the allowed-tools field is marked experimental, and compatibility is a free-text note. The spec itself says support for pre-approved tools may vary by implementation.
4. 04Open Responses fixes the request shape, not the model.One schema, items as the unit of context, semantic streaming events and dated immutable releases with a compliance tester. A provider that passes the tests accepts the same request; it does not promise the same answer, tool behaviour or pricing.
5. 05The adoption numbers are vendor counts; treat them as such.More than 70 platforms, a million published skills and nearly 280 million installs are Anthropic's figures from its own post. They are plausible signals of momentum and not an audit, and they say nothing about how many of those skills port cleanly.

## 01 — The RosterFour projects, and *ten* in total.

The Linux Foundation release is precise about the count, which matters because adding the four to the three launch projects gives seven, not ten. The foundation launched in December 2025 with MCP, goose and AGENTS.md, has since taken on the Agent2Agent protocol, Agent Router and agentgateway, and now adds four more, for ten. The release groups the new four by the layer each addresses, and that grouping is the most useful thing in it for a reader deciding what to do.

An example of why the layers matter more than the count. A team that reads "Anthropic and OpenAI both donated to the same foundation" might expect a skill written for Claude Code to run through an Open Responses request to another provider's model with no changes. The two projects do not touch. Agent Skills describes a folder an agent reads; Open Responses describes the HTTP shape an application sends to a model. The agent that loads the skill might use Open Responses to call its model, or might not, and the specification of neither says anything about the other.

The failure case is the inverse assumption, that because the four are unrelated the announcement is just housekeeping. The same foundation that governs MCP now also governs the format for giving agents procedures, a candidate standard for the model-call interface, and two execution-environment projects. Where those specifications overlap in practice, in the tool calls a skill asks for and the sandbox a tool runs in, the foundation is the place a shared answer would be negotiated. That is a change in where to look, if not yet in what exists.

| Linux Foundation press release of October 22, 2026 and the four projects' own sites and repositories, read October 26, 2026. "Portable unit" is our reading of each specification. |  |  |  | 
|---|---|---|---|
| Project | Contributor and layer | What it specifies | Portable unit | 
|---|---|---|---|
| Agent Skills | Anthropic · how agents gain capabilities | A folder with a SKILL.md: YAML frontmatter, Markdown instructions, optional scripts, references and assets | The text; scripts depend on the runtime | 
| Open Responses | OpenAI · how applications talk to models | A request and response schema, streaming events and tool invocation patterns, published as dated OpenAPI releases | The request shape; not model behaviour | 
| OpenEnv | Hugging Face · training and evaluation | Gym-style reset, step and state calls over WebSocket; each environment a Docker image; MCP exposure in production mode | The environment container | 
| OpenSandbox | Alibaba · where agents run | A sandbox lifecycle and execution API with Docker and Kubernetes runtimes, SDKs, a CLI and an MCP connection | The sandbox protocol; runtimes vary | 

## 02 — GovernanceWhat a neutral home *actually* changes.

Anthropic's donation post lists what changed hands: the Agent Skills name and logo, the GitHub organisation and the agentskills.io domain. The standard is to be maintained by engineers from Anthropic, Google, OpenAI and other companies acting as individuals, with David Soria Parra and Mahesh Murag as lead maintainers, and Anthropic says it has proposed a roadmap of versioned releases, a formal proposal process and security guidance for people who write and install skills. The post also states that using or contributing to the standard does not require joining the foundation.

That is the same shape as the MCP transfer less than a year earlier, and the practical effect is the one MCP showed: a competitor can propose a change to the specification through a public process rather than by forking it, and a vendor cannot quietly add a field that only its product honours and call it the standard. For a team that has written fifty skills, the governance change is insurance on the format's future. It is not a change to the skills themselves, and it does not retroactively make them run anywhere they did not run on October 21.

The failure case is reading the donation as an interoperability guarantee between the donors. OpenAI's own agent products appear in Anthropic's list of platforms that integrated the format, and OpenAI engineers are among the maintainers, which is encouraging. But the specification still says that supported script languages depend on the agent implementation and that support for the pre-approved tools field may vary. Two maintainers from two companies agreeing on the frontmatter does not oblige either company's agent to run the other's scripts.

four different companies chose to contribute them to neutral governanceManik Surtani, CTO and co-founder, Agentic AI Foundation, in the Linux Foundation press release of October 22, 2026

## 03 — Skills LayerWhat a skill *carries*, field by field.

The specification is short enough to read in full, and its shortness is the point. A skill is a directory with a SKILL.md file whose YAML frontmatter requires two fields: a name of up to 64 lowercase characters that must match the directory, and a description of up to 1,024 characters that says what the skill does and when to use it. Four optional fields follow: a licence, a compatibility note of up to 500 characters for environment requirements, a free-form metadata map of strings, and an experimental allowed-tools string. The body is Markdown with no format restrictions, and the spec recommends steps, examples and edge cases, under 500 lines, with longer material split into referenced files.

The loading model is what makes the format cheap to carry. Agents read only the name and description at startup, around a hundred tokens per skill, pull in the full instructions when a task matches the description, and open scripts, references or assets only when the instructions call for them. An example: a team's invoice reconciliation skill carries a description that names the task, a body that walks the steps, and a references folder holding the chart of accounts. Any agent that honours the format loads the same text. Our earlier guide to the format covers writing the body well.

The failure case is the scripts folder. The spec says supported languages depend on the agent implementation and lists Python, Bash and JavaScript as common options, which means a skill whose value is in a Python script that calls an internal API is portable as text and not as behaviour. The compatibility field exists to say so, in prose, and nothing enforces it. A skill that works in one agent with a sandboxed Python runtime and network access will load in another agent that has neither, and fail at the step the text describes rather than at install.

- namerequired
- ≤ 64 charslowercase, hyphens; must match the directory
- descriptionrequired
- ≤ 1,024 charswhat it does and when to use it; loaded at startup
- compatibilityoptional
- ≤ 500 charsfree text; product, packages, network needs
- allowed-toolsoptional, experimental
- space-separatedsupport may vary between implementations
- SKILL.md bodyloaded on activation
- < 5,000 tokens advisedkeep under 500 lines; split references out
- scripts/optional
- runtime-dependentlanguages depend on the agent implementation

## 04 — Model LayerOpen Responses: one schema, *many* models.

Open Responses is the project most likely to be misread, because its name suggests an open model and it is an open interface. The repository describes a specification for multi-provider, interoperable language-model interfaces inspired by the OpenAI Responses API, with a shared request and response model, streaming semantics and tool-invocation patterns. The design centres on an agentic loop in which a model emits tool calls, receives results and continues; on items as the atomic unit of context, each with a state machine; and on semantic streaming events rather than raw text deltas, so a client can be written once against event types instead of against each provider's chunk format.

What makes it more than a document is the release discipline. The OpenAPI specification is published in dated, immutable releases, with a changelog, and the repository ships a compliance tester as both a web page and a command-line runner, with a thirty-second deadline per request and per WebSocket turn. A provider or a proxy can point the runner at its endpoint and get a pass or fail per test. For a team building a model-agnostic application, that is the artefact to ask vendors for: not "we support Open Responses" but the compliance output, dated.

The failure case is expecting the schema to carry behaviour. Two providers that both pass the compliance suite accept the same request and emit the same event types; they do not produce the same answer, call tools with the same judgement, honour the same reasoning controls or charge the same. The specification also allows provider-specific tools and item types as extensions, which is how it avoids fragmenting the core and also how a vendor-specific feature creeps back in. A request that uses an extension is portable only to providers that implement that extension.

## 05 — Runtime LayerOpenEnv and OpenSandbox: where agents *train* and run.

The two execution projects are easy to conflate and serve different jobs. OpenEnv, from Hugging Face, is a framework for reinforcement learning environments with a Gymnasium-style interface: reset, step and state calls over a WebSocket, each environment packaged as a Docker image that runs locally, on a cloud sandbox or as a Hugging Face Space. Its README counts more than forty ready environments and lists training-library integrations, and describes a Harbor integration that runs real coding harnesses such as Claude Code and Codex on tasks while capturing the exact tokens for training. Its audience is a team training or evaluating an agent, not one deploying it.

OpenSandbox, from Alibaba, is the deployment side: isolated environments where an agent executes code, runs commands, manages files and drives browsers or desktops, starting on Docker and scaling on Kubernetes through one lifecycle API. Its README describes a Firecracker-based fast runtime with creation times it quotes at 64 milliseconds median, network control through an ingress gateway and per-sandbox egress policies, a credential vault so agents call external services without holding real secrets, SDKs in five languages, a CLI and an MCP connection. We looked at it when Alibaba first open-sourced it; the governance move is the new part.

The failure case is treating either as a portability layer for skills or tools. Both expose MCP, which means an agent can reach tools inside them through the protocol the foundation already governs. Neither defines what tools exist, how they are named, or what credentials they hold. A skill that says "run the deploy script" works in a sandbox that has the script, the runtime and the credential; the sandbox protocol specifies how to start the box and run the command, and nothing about whether the command will mean the same thing in a differently provisioned one.

### Train and evaluate

reset, step and state over WebSocket; environments as Docker images; 40+ in the catalogue per the README; rewards and rubrics built in; harness capture for coding agents; MCP exposure in production mode.

### Run in isolation

Code, commands, files, browsers and desktops; Docker to Kubernetes; Firecracker fast runtime with the project's own latency figures; egress policies; credential vault; SDKs, CLI and MCP; extensible sandbox protocol.

## 06 — The SeamsWhere portability *stops* at every layer.

Put the four together and the same four seams appear at each layer. Tool identity: a skill names tools in an experimental field, Open Responses carries tool calls as items, and the sandboxes expose tools over MCP, but no project defines a shared registry of what a tool is called or what its arguments mean. Credentials: the skill's compatibility field can say network access is needed, OpenSandbox has a vault, and nothing passes a grant from one to the other. State: items in Open Responses are the unit of context for one conversation, and a skill's progress through a multi-step task lives nowhere the format specifies. Model behaviour: nothing in any of the four narrows the gap between what two models do with the same instruction.

An example that crosses all four. A procurement team packages an approval workflow as a skill, calls a vendor's model through an Open Responses gateway, and runs the approval tool inside an OpenSandbox box. They switch model providers. The skill text loads unchanged; the gateway request is accepted because both providers pass the compliance suite; the sandbox starts the same image. What changes is whether the second model calls the approval tool at the same step with the same arguments, and the first the team hears of the difference is an approval that fired twice. We described the same pattern for Agent Plugins in August, and the foundation's roster has not closed it.

The failure case is a migration plan that budgets for the layers the standards cover and not the seams. The portable parts move in an afternoon. The seams take an evaluation: the same skills and tasks run against the new model through the same gateway and sandbox, with tool-call traces compared step by step. That is where the time goes, and it is the work the announcement does not shorten.

##### Names and arguments

Skills name tools in an experimental field; Open Responses carries calls as items; sandboxes expose tools over MCP. What a tool is called is still per platform.

##### Who holds the secret

A compatibility note can say network access is needed; OpenSandbox vaults credentials. No project transfers a grant from skill to sandbox to model call.

##### Where progress lives

Open Responses items hold one conversation's context. A skill's position in a long task is not in the format; your harness owns it.

##### Same request, different judgement

Passing the compliance suite means the request is accepted. Which tool the model calls, and when, is the thing to measure after a switch.

## 07 — Before StandardisingWhat to check before you *commit* to a layer.

The practical response to the announcement is to decide, layer by layer, whether to adopt the foundation's project as your internal standard, and the decision differs by layer. For skills the format is already read by more than seventy agent platforms, by Anthropic's count, and the cost of adopting the frontmatter is near zero; the work is in keeping scripts thin and compatibility notes honest. For the model interface the question is whether your providers and gateways pass the compliance suite today, which is checkable in an afternoon. For environments and sandboxes the question is whether the lifecycle API matches your infrastructure, which is an engineering evaluation, not a policy.

An example of the skills decision done well. A team with a private skill library validates every skill with the reference checker the specification points to, moves anything that only works in one agent into the compatibility field in plain words, and treats the allowed-tools field as a hint rather than a control. When they add a second agent product, the skills that fail are the ones whose compatibility note said they would, and the list is short enough to fix. Our note on skill packs and supply-chain risk is the other half of that discipline: provenance for what you install, not only validity for what you write.

The failure case is adopting all four at once because they share a foundation. Each has a different maturity: Agent Skills has a year of adoption and a roadmap that is still a proposal; Open Responses has dated releases and a tester; OpenEnv and OpenSandbox are codebases with READMEs, and their governance under the foundation is days old. Our AI transformation team sequences these by where a client's lock-in actually is, which is usually the model interface first and the skills format last, because the skills already move.

## 08 — Open ItemsWhat the announcement does *not* settle.

Several things a standards body would normally settle are, on October 26, still proposals or absences. Agent Skills has no versioned release yet; Anthropic's post describes versioned releases, a formal proposal process and security guidance as the roadmap it has proposed, not as things that exist. The allowed-tools field is experimental in the specification's own words. We found no published count of how many of the million skills Anthropic cites validate against the reference checker, or how many run outside the product they were written for. The platform figure of more than seventy is Anthropic's, and the foundation's release does not restate it.

Open Responses has the tester, but we found no public register of who passes it; each team has to run it. The specification is based on one vendor's API design, which is a strength for adoption and a question for governance that the project's technical charter is meant to answer. OpenEnv's environment count and OpenSandbox's latency figures are the projects' own numbers from their READMEs, not measurements we or the foundation have reproduced. And the foundation's own site, as read on October 26, still presents a project directory and working groups rather than any cross-project interoperability statement.

None of these is a reason to wait. They are the list of things to re-check when the first versioned Agent Skills release lands, when the security guidance is published, and when the compliance register, if one appears, lets a team compare providers without running the suite itself. Until then the right posture is the one MCP taught over its first year under the same roof: adopt the format, measure the behaviour, and read the specification rather than the announcement.

**Verdict, October 26, 2026**The foundation now governs a format for procedures, a candidate model interface, a training framework and a sandbox platform, and that is a real change in who decides what each becomes. It is not a change in what ports. Adopt the skills frontmatter, test the model interface, evaluate the runtimes, and budget the migration for the four seams the specifications leave open.

### Standardise on the text that already moves, test the interface that claims to, and measure the behaviour nothing standardises.

On October 22, 2026 the Agentic AI Foundation went from three projects to ten, and the four new ones came from four companies at four layers of the stack: Agent Skills for what an agent knows how to do, Open Responses for how an application asks a model, OpenEnv for how an agent is trained and scored, and OpenSandbox for where it runs. Vendor-neutral governance means a proposal process, named maintainers acting as individuals, and a domain and organisation no single company owns. That is worth having, and it is what the announcement delivers.

What it does not deliver is interoperability across the seams the specifications leave open: tool identity, credentials, task state and model behaviour. A skill's text moves today, and its scripts move when the runtime matches. An Open Responses request is accepted by any provider that passes the tester, and answered differently by each. The migration a team should budget for is the evaluation across those seams, and the announcement shortens the rest of the work, not that part.
