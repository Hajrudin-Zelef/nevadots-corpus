---
id: collect-261121-veille-ia/ia-agents/agent-plugins-1-0-what-the-standard-actually-fixes
title: "Agent Plugins 1.0: What the Standard Actually Fixes"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-08-06"]
keywords: ["agent", "plugins", "plugin", "standard", "specification", "github", "json", "trust"]
source: https://www.digitalapplied.com/blog/agent-plugins-1-0-open-standard-portable-ai-skills
source_anchor: ""
source_lines: [1, 237]
section: "IA_Agents"
---
Agent Plugins 1.0 is an open, vendor-neutral standard for packaging reusable agent components into portable plugins, announced on August 6, 2026 by a Technical Steering Committee of Core Maintainers from Amazon, Cursor, Microsoft, OpenAI and Vercel — with Google announcing the same day that it is joining that group. A plugin is a directory: a plugin.json, an optional skills folder, an optional mcp.json. The restraint is the design.

That is the announcement. What sits underneath it is more interesting. Two of the five founding vendors still ship their own, richer plugin formats — Cursor has `.cursor-plugin/plugin.json` and OpenAI has `.codex-plugin/plugin.json`. Anthropic’s `.claude-plugin/plugin.json` stays a first-class format inside VS Code too, even though Anthropic is not on the steering committee. The specification page reads Status: Working Draft while the repository README calls 1.0.0 the current published release, and there is no git tag and no GitHub Release behind either statement.

This guide works through what the standard normatively defines, the five launch client entries and the transport asymmetry already visible between them, the seven areas the project openly defers, the trust gap that opens when a portable package format ships before a provenance model, and how the governance charter holds up against what is observably true. Every fact below is sourced to the specification, the Technical Charter, the GitHub API, or a named vendor’s own property.

1. 01A plugin is a directory, and only two things travel.Agent Plugins v1 defines exactly two portable component types — skills and MCP servers. The manifest schema is closed at ten permitted top-level fields, only two of which are required, and component locations are fixed and cannot be overridden.
2. 02Five client entries at launch, covering six products.The canonical compatible-clients page lists VS Code, Cursor, GitHub Copilot, ChatGPT & Codex as one combined entry, and Kiro. Google is not on that list — it shipped two plugin producers, not a client.
3. 03Six plugin formats, and the standard is one of them.VS Code documents four manifest formats side by side — Agent Plugins 1.0, Copilot, Claude and Legacy OpenPlugin — and Cursor and Codex carry two more outside that list. The portable format is a floor added beneath the existing ones, not a replacement for them.
4. 04No trust model in v1, and install is implicit trust.The project's future-considerations document names provenance verification as unaddressed, while the VS Code documentation states that plugin MCP servers are implicitly trusted at install with no separate startup trust prompt. Read together, portability expands blast radius before trust infrastructure exists.
5. 05Independently governed — and not an AAIF project.The Agentic AI Foundation's own post states that Agent Plugins is not an AAIF project and has not submitted a proposal to become one. MCP, by contrast, is one. Agent Plugins packages MCP; it does not replace it.

## 01 — What LandedAn *announcement*, not a creation date.

August 6, 2026 was a coordinated announcement day. Google’s developer blog, the AWS Open Source Blog and Vercel’s blog and changelog all carry that date. But the specification text itself was committed on July 24, 2026 under the message Publish Agent Plugins Specification 1.0.0, the Agentic AI Foundation had already published a guest post about the format on August 4, and the `agentplugins` GitHub organisation was created back on March 16, 2026. If you are trying to judge how mature this is, the announcement date is the least informative of those four.

The maturity signals contradict each other, and both halves belong on the record. The specification page is labelled Spec Version: 1.0.0 alongside Status: Working Draft. The repository README simultaneously describes Agent Plugins Specification 1.0.0 as the current published release. Checked against the GitHub tags endpoint, there is no git tag for 1.0.0 and no GitHub Release — the array comes back empty. Read together, that is a published, implemented, still-labelled-draft specification: a normal place for a young standard to be, and an abnormal thing for coverage to gloss over.

##### Publish Specification 1.0.0

The commit that landed the 1.0.0 text predates the coordinated announcement by nearly two weeks. The initial maintainer roster was recorded July 20, the Lead Core Maintainer July 27, and the AAIF guest post went up August 4.

##### Git tags and GitHub Releases

The tags endpoint on the specification repository returns an empty array at the time of writing. Do not expect to pin a dependency to a tagged 1.0.0 — the version lives in the schema URL and the document header, not in a release artefact.

##### CC-BY-4.0 plus Apache 2.0

Specification text, documentation, examples and diagrams are CC-BY-4.0; schemas, source code and scripts are Apache 2.0. The GitHub API reports the repository licence as NOASSERTION precisely because of the split, so calling it flatly Apache 2.0 licensed is wrong.

The provenance story is consistent across vendors. The rosters are not. Vercel says it initiated the proposal and names six legal entities as refining it collaboratively: Amazon Web Services (AWS), Anysphere, GitHub, Microsoft, OpenAI, and Vercel. The AWS Open Source Blog describes the same process but names five products: AWS, Cursor, Microsoft, OpenAI, and Vercel. Anysphere is the company behind Cursor and GitHub is a Microsoft subsidiary, so the two lists are reconcilable in spirit — but they are not the same list, and no primary reconciles them. The repository’s MAINTAINERS.md and the project site both say Amazon where both vendor blogs say AWS.

One small canonical detail that costs people time: the project site is `agent-plugins.org`, hyphenated. The unhyphenated variant resolves in DNS to different addresses and fails the TLS handshake outright. It is not the project.

*announcement*date, not the creation date. The specification text was committed July 24, the AAIF guest post published August 4, and the GitHub organisation was created in March 2026. Google’s stated expectation to bring Agent Plugins support to more of its products is an announced intention, not shipped behaviour.

## 02 — AnatomyA plugin is a *directory*, not an archive.

A directory beat a `.zip`, a `.tar.gz` and a registry bundle so that plugins stay inspectable with standard tools such as `ls`, `cat` and `git`, editable in place during development, and compatible with version control — the specification’s design-decisions section says so in as many words. That single choice explains most of the rest of the format.

Component locations are fixed and cannot be overridden. The specification states that clients must discover each supported component type from its fixed location, and that `plugin.json` cannot override those locations or contain inline component configuration. Skills live in immediate subdirectories of `skills/` that contain a `SKILL.md` — there is no recursive search. MCP servers live in a single `mcp.json` at the plugin root. If a fixed location is simply absent, a client must not treat that as an error.

##### *plugin.json*

Only $schema and name are required, and $schema must be exactly the canonical 1.0.0 plugin schema URL. The other eight permitted top-level fields are version, description, author, homepage, repository, license, keywords and extensions. Names are 1 to 64 characters, lowercase alphanumerics with hyphens and dots, starting and ending alphanumeric, with no doubled hyphen or dot sequences.

##### skills/

Each immediate subdirectory containing a SKILL.md is one skill. Agent Plugins does not define the skill format at all — it defers entirely to the Agent Skills specification, which it names as the source of truth for the SKILL.md format, frontmatter fields and directory layout.

##### mcp.json

Three transports are described: stdio, streamable-http, and sse, the last explicitly labelled the deprecated legacy HTTP+SSE transport from the 2024-11-05 MCP specification. A conformant MCP-capable client must support at least one of stdio or streamable-http and should support both; sse support is optional.

##### com.example.client/

The escape hatch for client-specific data that the portable format does not cover. It is part of the 1.0.0 layout, but the VS Code documentation states that VS Code currently ignores client extension data and directories in Agent Plugins 1.0 packages.

"A plugin is a directory. That's the whole idea, and the restraint is the point."— Kevin Hou, Haoyu Wang & Alan Blount, Google Developers Blog, Aug 6, 2026

Two runtime details matter once you are writing a plugin rather than reading about one. First, the specification reserves exactly two environment variables — `${PLUGIN_ROOT}` and `${PLUGIN_DATA}` — and clients that launch plugin subprocesses must provide both. `${PLUGIN_DATA}` is a client-managed writable directory that must persist across plugin updates. Expansion is single-pass and non-recursive, and applies only to `args`, `env` values and `cwd` — never to `command`, to `env` keys, or to the fixed component locations.

Second, failure isolation is normative rather than a quality-of- implementation nicety. A failed MCP server does not disable the plugin’s skills, and an invalid skill is skipped rather than fatal. Google restates this as independent components failing independently; Vercel restates it as one invalid component not disabling unrelated ones. Path containment is normative too: every plugin-supplied path must resolve within the plugin root, `../` escapes are invalid, and symlinks may only resolve to targets inside the root.

Secrets are explicitly out of scope, and the specification says so twice. Header values are described as visible package data rather than a portable secret mechanism, with plugins told they must not embed credentials or other secrets in headers; configured `env` values carry the same warning. There is no OAuth configuration and no portable credential-reference field in v1 — authorization discovery, user interaction and credential storage are all client-managed. If you are building the MCP server that a plugin will wrap, our TypeScript MCP server walkthrough covers the auth surface the packaging layer deliberately leaves alone.

## 03 — Launch ClientsFive client entries — and the transports already *diverge*.

The canonical compatible-clients page lists five entries: VS Code, Cursor, GitHub Copilot, ChatGPT & Codex as a single combined entry, and Kiro. Six products are named across five rows, and Vercel’s blog matches the five-entry count with five bullets. Coverage that prints six clients is counting products, not entries — and the combined entry matters, because it is the one with the transport gap.

All five entries list Agent Skills support. Four of them list stdio, Streamable HTTP and legacy SSE. The ChatGPT & Codex entry lists stdio and Streamable HTTP only. That is a concrete, day-one consequence of the specification’s own rule that an MCP-capable client need only support one of stdio or Streamable HTTP: a plugin author who ships an `sse`-only server already has a portability hole in that one entry.

| Launch client capability matrix for Agent Plugins 1.0, assembled from the canonical compatible-clients page, the VS Code documentation and the founding vendors’ own plugin repositories, showing Agent Skills support, MCP transports listed, treatment of client extension directories, and whether the vendor still ships a separate non-portable plugin format. |  |  |  |  | 
|---|---|---|---|---|
| Client entry | Agent Skills | MCP transports listed | Client extension directories | Separate non-portable format | 
|---|---|---|---|---|
| VS Code | Listed | stdio, Streamable HTTP, legacy SSE | Currently ignored, per the VS Code documentation | Also auto-detects Copilot, Claude and Legacy OpenPlugin manifests | 
| Cursor | Listed | stdio, Streamable HTTP, legacy SSE | Not stated | .cursor-plugin/plugin.json | 
| GitHub Copilot | Listed | stdio, Streamable HTTP, legacy SSE | Not stated | Not stated | 
| ChatGPT & Codex | Listed | stdio, Streamable HTTP — no legacy SSE | Not stated | .codex-plugin/plugin.json | 
| Kiro | Listed | stdio, Streamable HTTP, legacy SSE | Not stated | Not stated | 

Six cells read not stated rather than an inference. Only the VS Code documentation describes what a client does with client extension directories, and no primary establishes a separate non-portable plugin format for GitHub Copilot or Kiro. Filling those cells by analogy would be the exact failure mode this table exists to avoid.

Kiro’s launch status is itself reported two ways, and the gap between them is small enough to be easy to miss. The AWS Open Source Blog says Kiro is rolling out support for the Agent Plugins spec, and separately that Kiro Powers now supports the spec natively. Vercel’s blog says that at launch, Agent Plugins are supported across a list that includes Kiro. Both are first-party statements; they are not quite the same claim. AWS also says its Agent Toolkit bundles 30+ curated skills across multiple plugins covering services such as Lambda, S3, DynamoDB and CDK — an AWS-stated figure carried by no other source.

*producers*, not a client: its Agents CLI and its Data Agent Kit. Google does not appear on the compatible-clients page. Any claim that Gemini CLI or Antigravity reads Agent Plugins is unsupported by any primary — what Google actually said is that it expects to bring Agent Plugins support to more of its products that already work with Skills and MCP servers.

## 04 — Manifest PathsOne standard that *added* a format.

Here is the finding that most coverage skipped. VS Code auto-detects the plugin format from the manifest path and schema, and its documentation describes four formats supported side by side: Agent Plugins 1.0 (`plugin.json` carrying the canonical `$schema`), Copilot (`plugin.json` without it), Claude (`.claude-plugin/plugin.json`), and Legacy OpenPlugin (`.plugin/plugin.json`). Outside that list, Cursor and Codex keep two more in active use. Six formats across five distinct paths — the portable one shares its filename with Copilot’s and is told apart only by the `$schema` value.

| Six plugin manifest formats in active use, spread across five distinct manifest paths, grouped into the four formats the VS Code documentation says VS Code auto-detects and the two founding-vendor formats outside that list, with each format’s manifest path, distinguishing marker and portability status. |  |  |  | 
|---|---|---|---|
| Format | Manifest path | How a client tells it apart | Portable under the standard? | 
|---|---|---|---|
| Auto-detected by VS Code — four formats, per its documentation |  |  |  | 
| Agent Plugins 1.0 | plugin.json | Carries the canonical 1.0.0 `$schema` value | Yes — this is the portable format | 
| Copilot | plugin.json | Same filename, no canonical `$schema` | No — client-specific | 
| Claude | .claude-plugin/plugin.json | Directory path | No — Anthropic-origin, still first-class in VS Code | 
| Legacy OpenPlugin | .plugin/plugin.json | Directory path | No — legacy | 
| Founding-vendor formats outside the VS Code list |  |  |  | 
| Cursor | .cursor-plugin/plugin.json | Directory path | No — the cursor/plugins repository was still receiving pushes at the time of writing | 
| Codex | .codex-plugin/plugin.json | Directory path | No — sits alongside .app.json, .mcp.json, agents/, commands/, hooks.json and assets/ | 

The gap is not just filesystem trivia. OpenAI’s own Codex plugin documentation describes six plugin parts: skills, connectors, MCP servers, browser extensions, hooks, and scheduled task templates. Agent Plugins standardises two of those six. Everything that makes a vendor’s plugin experience distinctive — hooks, scheduled tasks, browser extensions, connectors — stays outside the portable envelope by design.

That is a defensible architecture, and the specification says so outright: portability is an interoperability floor, not a replacement layer. But it is why the marketing framing of one format for every agent does not survive contact with the filesystem. What actually happened is that a sixth format was added, one that several clients now also read. Whether the proprietary formats eventually thin out is the thing to watch over the next few releases, and today nobody has published evidence either way — there are no adoption numbers, plugin counts or install figures from any primary source in this story.

## 05 — ScopeDeferred is not the same as *excluded*.

All four vendor posts blur two very different categories: things the project intends to address later, and things it has decided belong permanently to the client. Which bucket a gap falls into is what actually tells a team whether to adopt now or wait, so the table below keeps them apart.

| Agent Plugins 1.0 capability scope, grouped into behaviour that is normative in the 1.0.0 specification, areas named in the project’s future-considerations document as unaddressed, and responsibilities the project states are client-owned or out of scope, with the source document for each. |  |  | 
|---|---|---|
| Capability | Status | Where it is stated | 
|---|---|---|
| Normative in 1.0.0 — a conformant client must do this |  |  | 
| Manifest and its closed field set | Ten permitted fields, two required | Spec §5.2, §5.3 | 
| Skills discovery | Fixed location, not overridable, no recursive search | Spec §6.1 | 
| MCP server configuration | Fixed location; at least one of stdio or Streamable HTTP | Spec §6.1, §7.2.1 | 
| Path containment | Required — no escapes above the plugin root | Spec §4.1 | 
| Failure isolation | Required — one broken component does not disable the rest | Spec §7.2.2, §11.3 | 
| Plugin variables | Both reserved variables must be provided to subprocesses | Spec §9 | 
| Named as unaddressed — seven areas, none committed to a release |  |  | 
| Permission and approval UX | Deferred | FUTURE_CONSIDERATIONS.md | 
| Provenance verification | Deferred | FUTURE_CONSIDERATIONS.md | 
| Secret handling | Deferred | FUTURE_CONSIDERATIONS.md | 
| Enterprise controls | Deferred | FUTURE_CONSIDERATIONS.md | 
| Audit-trail standardisation | Deferred | FUTURE_CONSIDERATIONS.md | 
| Dependency resolution | Deferred — plugins cannot depend on other plugins in v1 | FUTURE_CONSIDERATIONS.md | 
| Testing and validation | Deferred — no test harness or validation tool is specified | FUTURE_CONSIDERATIONS.md | 
| Stated as client-owned or out of scope |  |  | 
| Installation and distribution | No install mechanism, no distribution protocol | Google Developers Blog; AAIF | 
| Sandboxing | No sandboxing requirements defined | Google Developers Blog | 
| OAuth and credential storage | Client-managed; no portable credential-reference field | Spec §7.2.1 | 
| Secrets in headers and env | Out of scope — visible package data, stated twice | Spec §7.2.1, §9.2 | 
| Trust and execution | Publishers, provenance systems and client policy | AAIF responsibility split | 

## 06 — Trust GapPortability arrives *before* provenance.

Two documents, both public, both from parties to the standard, say things that only become alarming when you read them together. The project’s future-considerations file names provenance verification as an unaddressed area of v1, alongside permission and approval UX, enterprise controls and audit-trail standardisation. The VS Code documentation, describing the shipped client behaviour, states that plugin MCP servers are implicitly trusted at install and do not show a separate trust prompt at startup the way workspace MCP servers do.

*both*gets you to the operational risk.

The mechanism is straightforward. A portable package format lowers the cost of moving an extension between clients, which is the entire point. It also lowers the cost of moving a malicious extension between clients, and v1 ships no signature, no publisher identity binding and no provenance chain to distinguish the two. Install becomes the trust decision, made once, by a human reading a directory listing. That is the same posture that has repeatedly gone wrong in package ecosystems with far more mature tooling — and it landed in the same week that a compromised maintainer account seeded an npm worm whose payload specifically targeted AI tool configuration directories.

To be fair to the specification, it is not defenceless. Path containment is normative, symlink escapes are invalid, secrets are explicitly declared out of scope rather than half-supported, and failure isolation limits how much one bad component can take down. Those are real controls, and they are the right ones for a packaging format to own. The gap is that everything above them — who published this, has it been tampered with, what is it allowed to touch — is client policy, and client policy is not uniform across five entries that already disagree about transports. Teams that want identity and audit trails around agent activity are, for now, building that themselves — which is why self-hosted agent workspaces with first-class agent identity are getting attention at the same moment as a packaging standard that leaves identity out.

##### Read the *directory*

A plugin being a directory is a security asset here: ls, cat and git diff work on it. Review mcp.json for what commands the client will launch, and check whether any client extension directory is carrying behaviour your client honours but you did not expect.

##### Treat install as code execution

VS Code documents that plugin MCP servers are implicitly trusted at install. Whatever your client does, assume that installing a plugin is equivalent to running its author's code, and gate it the way you gate a new dependency rather than the way you gate a settings change.

##### Pin and vendor your sources

Plugins cannot declare dependencies on other plugins in v1, so there is no transitive resolution to police — but there is also no lockfile and no registry. Keep an internal, reviewed copy of the plugins your team relies on rather than pulling from community marketplaces at install time.

None of this is an argument against the standard. It is an argument for sequencing: adopt the format for the plugins you author, and keep third-party installs behind the same review you would apply to any dependency until the provenance work in the future-considerations list actually lands. The failure patterns are well documented — our write-up of what goes wrong when plugin trust is assumed is the closest analogue, and plugin governance in an enterprise setting covers the controls a platform team ends up building anyway.

## 07 — GovernanceVendor-neutral, in the *charter*. Now test it against the record.

Governance lives in a Technical Charter kept separate from the format specification, which is a good sign on its own. The charter defines four roles — Contributors, Maintainers, Core Maintainers and a Lead Core Maintainer — and states that the Technical Steering Committee consists of all Core Maintainers together with the Lead Core Maintainer. That sentence settles a point that a lot of secondary coverage has garbled: Core Maintainer is the TSC seat, not a lesser rank beneath it.

The initial roster is five people, named with their affiliations: Clare Liguori (Amazon), Roshan Sadanani (Cursor), Harald Kirschner (Microsoft), Gav Verma (OpenAI) and Jonathan Hefner (Vercel), with Hefner also recorded as Lead Core Maintainer. When Google announced on August 6 that it is joining as a Core Maintainer, it was announcing that it is joining the TSC. The two real distinctions are timing — Google is not a founding member, and announced its intention to join on announcement day — and roster state: Google is not yet listed in MAINTAINERS.md at the time of writing.

*no primary names*, so neither will we.

#### Governance thresholds · each shown against its own stated denominator

Source: Agent Plugins Technical Charter (GOVERNANCE.md §§5, 7, 11)
Now the other half, because a charter is a claim and not a state of the world. Vercel wrote the initial specification draft — the git history corroborates it, with early pull requests merging from Vercel-owned head branches before the project moved to its own organisation. The Lead Core Maintainer, who chairs the TSC and casts the tie-break, is a Vercel employee. Neither fact violates a charter clause: one company holding one of five Core Maintainer seats is not a majority, and the removal threshold is deliberately high in both directions. But readers deserve both halves rather than the adjective alone.

What makes this charter more testable than most is that it publishes thresholds you can re-check later. Proposals must start in public GitHub Discussions and demonstrate a concrete cross-client need plus evidence that implementers are prepared to support the same semantics. Come back in six months and the questions are mechanical: has the roster grown beyond the founding five, is Google listed, has any single employer accumulated more than one seat, and did the proposals that shipped go through public discussion first. That is a more useful vendor-neutrality audit than any statement of intent.

## 08 — Where It SitsIt packages MCP. It does *not* replace it.

The single most common misreading of this launch is that a new standard has arrived to compete with the Model Context Protocol. It has not, and the clearest statement of that comes from the Agentic AI Foundation’s own write-up: “Agent Plugins does not replace Agent Skills or MCP. It packages them without changing their native formats.” The specification backs this structurally — it defers the entire skill format to the Agent Skills specification, which it names as the source of truth for the SKILL.md format, frontmatter fields and directory layout, and it configures MCP servers using MCP’s own transports rather than inventing new ones.

##### Agentic Resource Discovery

Google positions discovery as the first layer, and notes that its resource-discovery work already treats a Plugin as a first-class agentic resource type. This four-layer stack is Google's own model rather than a project-level definition.

##### AI Catalog

The layer that answers what a resource is and where it came from. Notably, catalogues and registries are also where the AAIF responsibility split puts discovery — explicitly outside Agent Plugins itself.

##### *Agent Plugins*

The only layer this standard occupies. It is a packaging contract: a fixed directory shape that lets a client find components it already knows how to run, without altering how those components are written.

##### MCP and Agent Skills

Execution stays with the specifications that already own it. Agent Skills has its own home and its own GitHub organisation; MCP has its own specification and, unlike Agent Plugins, is an Agentic AI Foundation project.

That last point is the one to get right. The Agentic AI Foundation published a guest post about Agent Plugins on August 4, and the same post carries an explicit disclaimer: Agent Plugins is an independently governed open specification, it is not an AAIF project, and it has not submitted a proposal to become one. MCP is an AAIF project. So any framing that treats this as another foundation-backed standard, or that borrows AAIF’s governance credibility for it, is simply wrong. If you want the contrast, our explainer on the Agentic AI Foundation covers what foundation stewardship actually involves, and the wider agent-protocol map shows where a packaging layer slots into a landscape that predates it.

AAIF frames the same division as a five-way responsibility split: reusable instructions belong to Agent Skills; runtime connections belong to MCP; packaging belongs to Agent Plugins; discovery belongs to catalogues and registries; and trust and execution belong to publishers, provenance systems and client policy. Read that last row next to the trust gap in section 06 and the shape of the near-term work becomes obvious — the row with the most owners is the row with the least specification.

One absence is worth stating and not explaining. Anthropic is not on the Technical Steering Committee and Claude Code is not among the launch client entries — yet Agent Skills, the specification Agent Plugins defers to entirely, and the `.claude-plugin/plugin.json` format, still first-class inside VS Code, both originate at Anthropic. No primary source gives a reason for the absence, and none of the parties has commented on it. Anything beyond the fact would be invention. If you are working with the format Agent Plugins wraps, our Agent Skills framework guide is the prerequisite read, and the current MCP specification covers the transport rules the plugin manifest simply points at.

## 09 — What To DoAdopt the *floor*, gate the ceiling.

The practical read is that this standard is cheap to adopt and cheap to be wrong about, which is unusual and mostly good. Adding a conformant `plugin.json` to an extension you already ship costs two required fields and does not force you to abandon your existing vendor-specific manifest — VS Code already reads several side by side. The asymmetry runs the other way: the cost of installing someone else’s plugin is where the unpriced risk sits, because v1 defines nothing between the download and the execution.

##### Add the manifest now

Two required fields, a closed schema and no requirement to drop your existing format. If your skills already sit in a skills/ directory with SKILL.md files, conformance is mostly a naming exercise. There is no downside to being readable by five client entries instead of one.

##### Check your *transport*

The ChatGPT & Codex entry lists stdio and Streamable HTTP with no legacy SSE, while the other four entries list all three. An sse-only server is portable in name and unportable in that entry. Move to Streamable HTTP or ship stdio alongside before you advertise portability.

##### Gate it like a dependency

There is no provenance verification, no signature and no publisher identity binding in v1, and VS Code documents that plugin MCP servers are implicitly trusted at install. Review the directory, review the publisher, keep an internal vendored copy, and do not treat a community marketplace as a trust boundary.

##### Plan for client-side, not spec-side

Permission and approval UX, enterprise controls and audit-trail standardisation are named in the future-considerations document as unaddressed, with no committed release. Build the controls you need in your client policy and platform tooling, and treat any future spec coverage as a bonus rather than a plan.

The most likely near-term trajectory is not consolidation but layering. The vendors that already ship richer formats have every commercial reason to keep them — hooks, scheduled tasks, connectors and browser extensions are exactly the surfaces where a client differentiates — and the standard was carefully written not to threaten them. What that produces is a portable core that travels and a proprietary shell that does not, which is close to how container images and language package manifests settled. AWS reaches for both analogies in its own post, and the comparison is fair in a way AWS probably did not intend: package.json gave npm, yarn and pnpm a common format and those three tools still differ substantially.

The thing that would change this assessment is provenance. If the deferred work lands — signatures, publisher identity, a verification story that clients agree on — then a portable format plus a trust chain starts to look like a real distribution ecosystem rather than a shared directory layout, and the calculation for third-party installs flips. Until then, the standard is genuinely useful for what you author and genuinely underspecified for what you consume. If you are working out where an agent packaging layer fits into a production stack, that is the sort of question our AI transformation engagements open with, and our engineering work is where the client-side controls get built.

## 10 — ConclusionA floor worth standing on, honestly *labelled*.

### The standard fixes packaging. It does not yet fix trust — and it says so.

Agent Plugins 1.0 does something narrow and does it well. A plugin is a directory, exactly two component types travel, the manifest schema is closed, component locations are fixed, and failure isolation and path containment are normative rather than aspirational. Five client entries read it at launch. That is a real interoperability floor, and it arrived with a *published charter* containing testable anti-capture thresholds, which is more than most vendor consortia manage.

The caveats are all on the record, mostly in the project’s own documents. The specification page still reads Working Draft and there is no git tag behind the 1.0.0 label. Cursor and Codex still ship their own richer formats, and VS Code documents four manifest formats side by side. Transport support already diverges between the five client entries. Seven areas are named as unaddressed, with provenance verification among them — while shipped client behaviour makes install an implicit trust decision. And it is not an Agentic AI Foundation project, whatever the surrounding ecosystem framing suggests.

So the useful posture is asymmetric. Adopt the manifest for what you publish, because it costs almost nothing and widens your reach. Treat what you install with the same suspicion you would apply to any unsigned dependency, because the standard has explicitly not solved that yet and has told you so in writing. The vendors got the hard political part done — a shared format nobody owns. The technically harder part, making that format safe to consume at scale, is the next release’s problem, and there is no published date for it.
