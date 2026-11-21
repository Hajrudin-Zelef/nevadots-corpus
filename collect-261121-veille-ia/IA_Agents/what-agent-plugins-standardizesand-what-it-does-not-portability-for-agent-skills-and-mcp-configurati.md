---
id: collect-261121-veille-ia/ia-agents/what-agent-plugins-standardizesand-what-it-does-not-portability-for-agent-skills-and-mcp-configurati
title: "What Agent Plugins Standardizes—and What It Does Not: Portability for Agent Skills and MCP Configuration — MIF LLC"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "specification", "skills", "configuration", "portability", "plugins", "does", "plugin"]
source: https://www.mifllc.com/ai-software/blog/agent-plugins-portability/
source_anchor: ""
source_lines: [1, 99]
section: "IA_Agents"
---
# What Agent Plugins Standardizes—and What It Does Not: Portability for Agent Skills and MCP Configuration

The more an organization invests in custom skills and connection configuration across multiple AI agent products, the more consequential their portability becomes. A new cross-vendor specification now addresses part of that problem.

For organizations using multiple AI agent products in parallel—such as ChatGPT, Cursor, and GitHub Copilot—a practical question arises: how much of the custom skills and connection configuration built for one product can be moved to another?

The more an organization invests in those extensions, the more consequential that question becomes.

A new cross-vendor specification now addresses part of that problem.

## What Agent Plugins 1.0.0 Defines

On August 6, 2026, Vercel published Agent Plugins 1.0.0, a specification for distributing AI agent extensions in a common format. Vercel proposed it, and representatives from AWS, Anysphere (the company behind Cursor), GitHub, Microsoft, OpenAI, and Vercel developed the specification together. The technical steering committee includes maintainers from AWS, Cursor, Microsoft, OpenAI, and Vercel, and the specification and its schemas are publicly available.

Version 1.0.0 focuses specifically on two component types: Agent Skills and MCP servers. MCP servers connect AI agents to external tools and services.

The structure is straightforward. A directory with a `plugin.json` at its root is treated as a single plugin. A plugin may also include a `skills/` directory containing Agent Skills and an `mcp.json` file describing MCP server configuration. `plugin.json` is required, while `skills/` and `mcp.json` are optional.

Among the portable components, a plugin may include Agent Skills, MCP configuration, or both; neither location is required. The specification states that if one of these fixed component locations is absent, a client must not treat that as an error. The manifest itself has only two required fields: `$schema`, which identifies the specification version, and `name`.

According to Vercel’s explanation, the problem until now was that using the same skill or MCP server across several products meant adapting it to each product’s different metadata conventions, discovery paths, and MCP configuration formats—and that fragmentation is what the specification addresses.

What the specification standardizes is not merely how directories are arranged. It defines a minimal common contract between clients that covers plugin validation, component discovery, loading of MCP configuration, isolation of failures when an individual component is invalid, and detection of the specification version.

As supporting clients, the announcement lists ChatGPT, Codex, Cursor, GitHub Copilot, Kiro, and VS Code. That said, the specification’s status is shown as “Working Draft” at the time of the 1.0.0 publication. Support from several major products is significant, but it does not yet establish that the specification has matured into a broadly adopted industry standard.

## Does a Common Format Alone Deliver Portability?

The arrival of a common specification agreed among major players is a welcome development for any organization that has invested in building extensions.

What follows is MIF’s view, based on the published specification.

The main risk is to read the announcement as meaning that switching has become easy. That interpretation overlooks the distinction between what the specification standardizes and what remains product- or organization-specific.

Extension portability can be considered at three levels.

**1. Package portability:** whether compatible clients can discover, validate, and load a plugin’s components through a common method. This is the level Agent Plugins primarily standardizes.

**2. Behavioral portability:** whether the same skill produces a comparable workflow and output quality across different models and clients. The package specification alone does not guarantee this; how skills are presented to users and models is left to each client.

**3. Operational portability:** whether authentication, permissions, secrets, installation, updates, distribution, sandboxing, and auditing can be recreated in another product. This level also sits outside the portable core the specification defines.

Loading the same plugin does not guarantee comparable output quality, equivalent permissions, or the same security and safety controls.

## Portability and Safety Are Also Separate

Another distinction worth drawing is between portability and safety. The fact that a plugin can be loaded in the Agent Plugins format does not mean that it is safe or appropriate to run in your environment. The specification explicitly leaves installation sources, registries and marketplaces, permission prompts, trust policy, sandboxing, and the handling of enablement and updates to each client’s design.

MCP connections deserve particular attention here. `mcp.json` can describe a local stdio server or a remote MCP endpoint, but **authentication is not standardized**. The specification defines no portable OAuth settings and no credential-reference fields, and states that authentication remains client-managed. It also treats configured headers as literal, visible package data and prohibits them from containing credentials or secrets.

In practice, then, switching products means identifying not only the connection settings to move, but the authentication, authorization, and audit controls as well.

## Separate the Portable Core from Product-Specific Extensions

One conclusion does not necessarily follow from the specification. MIF does not take the view that product-specific work should be avoided simply to reduce switching costs. A product’s distinctive capabilities can deliver substantial practical value, and abandoning them across the board would be a poor trade-off.

The specification itself does not reject product-specific functionality either. It provides a mechanism for placing client-specific data and files under reverse-domain namespaces—in an `extensions` object within `plugin.json`, or in a dedicated directory—and specifies that a client ignores namespaces it does not implement without validating their contents.

The extension mechanism supports a more practical design principle: **separate what can be shared from what must remain product-specific**. Manage the portable core—business rules, decision criteria, approved reference materials, and non-sensitive MCP configuration—separately from product-specific hooks, commands, screen interactions, and proprietary features.

The specification gives organizations a clearer basis for drawing that line.

## Start by Taking Inventory

The most practical first step is to inventory the skills and connection settings your organization has already built.

The table below is one example of how to begin. Adapt the items and the criteria to your own situation and risk posture.

| Item | Portable elements | Product- or environment-specific elements | What to check next | 
|---|---|---|---|
| (e.g.) Customer-inquiry drafting skill | Skill text, templates, reference material | Invocation method, model-specific output tendencies, product-specific tool names | Test against the same evaluation criteria on several clients | 
| (e.g.) Internal knowledge-search MCP connection | Server URL, transport, non-sensitive settings | OAuth, credentials, access permissions, user approval | Whether equivalent authentication and permission settings can be configured in another client | 
| (e.g.) Local MCP server | Command, arguments, files inside the plugin | Sandboxing, execution permissions, OS and runtime dependencies | Confirm the execution environment and permissions per client | 
| (e.g.) Commands and hooks | Not part of the 1.0.0 portable core | Product-specific namespaces and formats | Whether an alternative is sufficient or a client-specific port is required | 
| (e.g.) Distribution and updates | Not standardized by the specification | Marketplace, update approval, version management | Who approves distribution and updates | 

The columns and the level of detail are a starting template, not a prescription. The key is not to assume that conformance to the common format resolves every migration issue, but to identify precisely what can move unchanged and what must be reconfigured.

## Summary

Agent Plugins 1.0.0 provides a common package format for Agent Skills and MCP server configurations, allowing compatible clients to discover, validate, and load those components through a shared method.

By reducing product-specific differences in package structure, discovery paths, and MCP configuration, the specification may reduce the amount of rework required for each client.

The specification does not, however, make every part of an extension portable. Installation, authentication, permissions, sandboxing, updates, the way skills are presented to users and models, and product-specific functionality all remain with each client and each adopting organization.

Loading the same plugin does not guarantee comparable output quality, equivalent permissions, or the same security and safety controls.

The goal is not to avoid product-specific optimization altogether. It is to manage the portable core—business rules, decision criteria, approved reference materials, and non-sensitive MCP configuration—separately from product-specific hooks, commands, authentication, and screen interactions.

The publication of Agent Plugins 1.0.0 does not mean that switching has automatically become easy. It is better understood as a practical framework for inventorying what an organization has built: what can move through a common format, and what remains tied to a particular product or operating environment.

### Sources and Verification

This article was prepared using materials reviewed in August 2026, including Vercel’s announcement “Introducing Agent Plugins” and the official Agent Plugins documentation (the specification itself, the plugin authors’ guide, the client implementers’ guide, and the pages on MCP server configuration and client extensions).

The specification’s status was shown as Working Draft at the time of verification, and the list of compatible clients and the specification itself may change.

Before adoption, confirm the components you plan to use and each client’s current level of support against the latest official documentation.
