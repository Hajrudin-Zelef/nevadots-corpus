---
id: collect-261121-veille-ia/ia-agents/agent-plugins-standard-what-it-means-for-qa-teams
title: "Agent Plugins Standard: What It Means for QA Teams"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "plugin", "skills", "plugins", "json", "standard", "files", "client"]
source: https://katalon.com/resources-center/blog/agent-plugins-standard
source_anchor: ""
source_lines: [1, 148]
section: "IA_Agents"
---
Katalon's library of testing skills for AI agents holds 13 skills. Building it produces 124 files.

The 13 skills are the actual work: how to plan a test cycle, how to design cases from a requirement, how to read a failing run and decide whether a release is safe. The other 111 files are packaging. Four of them arrived this month and they are the subject of this post. The remaining 107 exist for one reason: until August 2026, every agent client wanted that same knowledge handed to it in a slightly different shape.

```
skills/  (13 SKILL.md files, the only real content)
   |
   +--> plugins/          Claude Code, Codex, Cursor plugin
   +--> .cursor/rules/    *.mdc, its own frontmatter
   +--> .kiro/steering/   inclusion: manual
   +--> .github/skills/   plus .github/prompts/, separately
   +--> .windsurf/rules/  trigger: model_decision
   +--> .clinerules/      no frontmatter at all
   +--> .continue/rules/  name + description + alwaysApply
   +--> AGENTS.md         the fallback for everyone else
                                    107 files of transcription
```
None of that is interesting work. It is transcription. Every agent client that gains traction means another emitter to write and another 13 files to keep from drifting out of sync with the skills they are supposed to mirror. Anyone who maintains a shared prompt or rules library across a QA team knows the shape of this problem, whether it holds 13 skills or 3.

Then on August 6, 2026, five companies that compete with each other agreed on a format. Vercel announced Agent Plugins 1.0.0, with Amazon, Cursor, Microsoft, and OpenAI on the steering committee and GitHub in the room for the drafting. Katalon adopted it the next day in katalon-labs/true-skills.

## What is the Agent Plugins standard?

Agent Plugins is an open, vendor-neutral standard for packaging reusable AI agent components into a portable plugin. Version 1.0.0 was published on August 6, 2026 at agent-plugins.org. It defines a directory layout that any conformant agent client can load, so a plugin author packages once instead of once per client.

It is a packaging format and nothing else. The full specification reads in one sitting, which is the first thing worth liking about it.

A plugin is a directory:

```
my-plugin/
|-- plugin.json          identity + which spec version
|-- skills/
|   `-- summarize/
|       |-- SKILL.md
|       |-- scripts/
|       `-- references/
|-- mcp.json             MCP servers this plugin ships
|-- com.example.client/  optional, client-specific
`-- LICENSE
```
## What does plugin.json require?

plugin.json requires exactly two fields: $schema, which pins the spec version, and name. Everything else is optional metadata that looks like a package.json you have written a hundred times: version, description, author, homepage, repository, license, keywords.

```
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "katalon-true-skills",
  "version": "0.3.0",
  "license": "MIT"
}
```
Two required fields. That is the whole barrier to entry.

The skills themselves are not redefined. The spec says they must conform to the Agent Skills specification, the SKILL.md format Anthropic built and released as an open standard, which Cursor, Copilot, VS Code, Gemini CLI, Kiro, and a long list of others already read. Agent Plugins does not compete with that. It says where skills live and how they travel.

mcp.json describes the MCP servers a plugin brings, in three transports: stdio, streamable-http, and sse for the legacy case. Two environment variables are guaranteed, PLUGIN_ROOT and PLUGIN_DATA, and they expand inside args, env values, and cwd. They do not expand inside command, which stays one executable token. That restriction is small and worth appreciating: a manifest cannot smuggle a shell expression into the thing your machine is about to run. If MCP itself is still new ground for your team, our walkthrough of what an MCP server means for testing covers the runtime half of this picture.

## What the spec deliberately leaves out

The most interesting part of Agent Plugins 1.0.0 is what it refuses to do. No registry. No conformance badge. Nobody to register with. Conformance is self-declared by implementing the behaviors.

Client-specific features go into reverse-domain namespaces, both as a key in the manifest and as a top-level directory:

```
my-plugin/
|-- plugin.json
`-- com.example.client/
    `-- hooks/
        `-- hooks.json
```
Clients ignore namespaces they have not implemented, without validating them. That one rule is what lets the format stay this small and still leave room for clients to differentiate. Hooks, telemetry config, some future thing nobody has built: all of it fits without a spec revision, and none of it breaks a client that has never heard of it.

The repo also ships a FUTURE_CONSIDERATIONS document listing what was deliberately deferred from 1.0.0: permission and consent flows, cryptographic provenance, secret handling, enterprise allowlists, audit-trail schemas, dependency resolution between plugins, and conformance test suites. Seven genuinely hard problems. As Tieu puts it, "a 1.0 that tried to answer them would have landed in 2027 and been wrong about most of them."

Those seven are the list to watch if you work somewhere with a security review. Nothing in 1.0.0 tells you whether a plugin is safe to install, only how it is shaped.

## Who governs the Agent Plugins standard?

The standard is governed by a Technical Steering Committee. Its Core Maintainers are Clare Liguori (Amazon), Roshan Sadanani (Cursor), Harald Kirschner (Microsoft), Gav Verma (OpenAI), and Jonathan Hefner (Vercel), who serves as Lead Core Maintainer. Vercel initiated the proposal, and AWS, Anysphere, GitHub, Microsoft, and OpenAI worked on the specification before publication.

The names matter less than the governance charter, which makes promises about what cannot happen:

```
no single vendor may hold a majority of Core Maintainer seats
no seats are reserved for specific companies
project assets held in trust by a neutral entity
spec licensed CC-BY-4.0, code Apache 2.0
maintainers selected partly on "multi-vendor neutrality"
```
The way a format like this dies is not by failing to launch. It launches, gets adopted, and then bends toward whoever controls the reference implementation until conformance quietly means compatibility with one company's product. Writing the neutrality constraints into the charter before adoption is the only real defense, and it is cheap to write while a project is new and expensive to retrofit once a sponsor has something to protect.

## How Agent Plugins differs from MCP and Agent Skills

The three sit in a stack and do not overlap.

```
Agent Skills   what a capability IS      SKILL.md + scripts/ + references/
MCP            how an agent CALLS it     stdio / streamable-http / sse
Agent Plugins  how you SHIP both         plugin.json + skills/ + mcp.json
```
MCP standardized how an agent reaches a tool at runtime, and tool builders stopped writing one integration per assistant. It says nothing about distribution, so you could build an excellent MCP server and a good set of skills and still be stuck on how to hand the bundle to someone who uses a different editor. Agent Plugins fills that gap and nothing else. One is the wire protocol, the other is the shipping container.

## What adoption actually cost

Less than expected in one repo. More than expected in the other.

For the skills library it was genuinely small. The emission was added to the build script that already generates the other eight formats, so a plugin.json and an mcp.json now fall out at the repo root and inside the plugin directory. The MCP config needed translating rather than copying, because Claude's .mcp.json shape and the standard's are not the same file under a different name: the standard wants an explicit "type": "stdio" and its own $schema. Both files were then validated against the published schemas with ajv before anyone trusted them:

```
$ npx ajv-cli validate --spec=draft2020 \
    -s plugin.schema.json -d plugin.json
plugin.json valid
```
That took an afternoon, most of it reading.

The personal repo was messier, because its canonical skills live in .claude/skills/ and the spec wants them at skills/. Keeping two hand-maintained copies is the exact disease the standard is supposed to cure. So the root skills/ became a generated mirror, with a one-command rebuild script and a packaging validator that fails if the two trees differ at all. Edit the mirror by hand and the build tells you off. That is the right shape: the standard gets a real directory, and the source of truth does not move.

The unglamorous part was version bookkeeping. The repo shipped a release mid-work, the branch collided with it on rebase, and the version already written into four different manifests was taken. The validator now checks version alignment across all four manifests plus the version file, and the release went out one number higher than planned.

## Nothing got deleted, and that is the honest part

All eight adapters are still there.

Every client in that list has users today, and several have not implemented the standard yet. Adopting on day one meant adding a ninth surface, not removing eight. The consolidation is real, but it arrives later, when clients catch up. Treat any claim that a one-day-old standard let a team delete its compatibility layer with suspicion.

```
before this week:  8 formats                120 files
now:               8 formats + 1 standard   124 files
later, maybe:      just the standard         ~15 files
                   ^ this is the bet, not the result
```
So it is a bet, and it is worth naming as one. Specifications get abandoned all the time. What made this one worth taking was the combination: a spec small enough to implement in an afternoon, a charter that constrains its own sponsors, and a scope that stops at packaging instead of reaching for the whole problem. If it fails, the cost was a manifest file and about 40 lines in a build script.

## What this means if you maintain testing skills

The near-term math is simple. If you maintain agent skills for more than two clients, one manifest and a small build change buy you a portable surface for roughly an afternoon of work, and you lose almost nothing if the standard stalls. If you maintain them for exactly one client, there is no urgency yet, and waiting to see which clients ship support costs you nothing either.

The longer-term change is the interesting one. When packaging stops being a differentiator, the differentiator becomes content: the quality of the skills themselves, the judgment encoded in them, and whether they reflect how testing actually works. The small, specific plugin that nobody builds today, because packaging it for six clients costs more than writing it, becomes worth building. A test-data plugin for one regulated domain. A regression triage skill tuned to one team's flaky suite. Those are the pieces that never justified the transcription tax.

That is also why this matters to a testing platform and not only to a repo maintainer. The same logic sits behind how Katalon True exposes its agents and its own MCP server: the useful part is the testing judgment, not the wrapper it ships in.

A published spec is not a working one. It starts working when enough libraries package for it that clients have a reason to read it. Katalon packaged for it. We will see who else does.

## Test everything. Ship with confidence.

From generating test cases to spotting failures, Katalon's AI works across your entire pipeline, automatically.
