---
id: collect-261121-veille-ia/ia-agents/agent-plugins-1-0-six-vendors-agree-on-how-to-package-an-agent-capability-361-network
title: "Agent Plugins 1.0: six vendors agree on how to package an agent capability — 361 Network"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "plugins", "copilot", "servers", "august", "capability", "plugin", "vendors"]
source: https://www.361network.com/en/news/agent-plugins-1-0
source_anchor: ""
source_lines: [1, 40]
section: "IA_Agents"
---
A standard backed by AWS, Anysphere, Microsoft, OpenAI, Vercel and Google is not a normal changelog entry. Agent Plugins 1.0 was published on 6 August; GitHub shipped support on 12 August across VS Code, the Copilot CLI, the Copilot SDK and the Copilot app.

The problem it solves is unglamorous and real. Skills and MCP servers already worked. What did not work was packaging: publishing the same capability to several agent clients meant separate manifests and separate directory structures for each, even though the skill and the server underneath were identical.

## What a plugin actually is

A plugin is a bundle. It can contain:

- Skills — the capabilities an agent can use
- MCP servers — the tool integrations those capabilities talk to
- Vendor-specific components, kept in namespaced directories such as `com.github.copilot/`

## Why the namespaced directories are the clever part

The namespace convention is what makes a shared standard survive contact with six competing vendors. Everything portable lives at the top level; anything that only makes sense in one client lives in that client's directory and is ignored by the others.

That means a vendor can add a proprietary feature without forking the standard, and a plugin author can support one client deeply without breaking the rest. Standards usually die at exactly this point — the moment one participant needs something the others do not — and this design routes around it rather than pretending it will not happen.

## Where plugins come from

The Awesome Copilot marketplace is the default source in VS Code, the Copilot CLI and the Copilot app. Enterprises can add their own marketplaces through enterprise settings.

A default marketplace is the feature that makes this real rather than theoretical — the reason MCP took off was not the protocol, it was that servers became easy to find and install. Expect the same dynamic here, including the less welcome half of it: the moment installing a capability takes one click, people install capabilities.

## The admin controls, and why to set them now

Copilot Business and Enterprise can govern plugins from `managed-settings.json`, using three keys alongside the MCP allowlists that reached GA on 6 August:

- `enabledPlugins` — auto-install specific plugins, or block them
- `extraKnownMarketplaces` — add marketplaces beyond the default
- `strictKnownMarketplaces` — restrict installation to approved sources only
- MCP allowlists — approve or block the servers inside plugins, by URL, command or name

## What this changes about your risk model

Read the two August announcements together and the shape becomes clear. On 6 August GitHub gave enterprises a way to allowlist MCP servers. On 12 August it made it dramatically easier to acquire and install bundles that contain MCP servers.

That is not a contradiction — it is the correct order. The control shipped before the distribution channel. But it does mean an allowlist written last week against the servers you knew about is now guarding a door that a lot more traffic is about to come through.

The practical consequence: a plugin is a supply-chain artifact. It is authored by someone, it bundles executable capability, and it installs in one click. Everything your organisation already believes about dependencies applies here, and almost nobody has written it down for plugins yet.
