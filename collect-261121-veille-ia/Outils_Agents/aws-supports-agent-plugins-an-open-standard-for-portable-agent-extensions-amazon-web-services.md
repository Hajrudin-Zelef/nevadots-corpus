---
id: collect-261121-veille-ia/outils-agents/aws-supports-agent-plugins-an-open-standard-for-portable-agent-extensions-amazon-web-services
title: "AWS Supports Agent Plugins: An Open Standard for Portable Agent Extensions | Amazon Web Services"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "plugins", "open", "extensions", "standard", "client", "package", "skills"]
source: https://aws.amazon.com/blogs/opensource/aws-supports-agent-plugins-an-open-standard-for-portable-agent-extensions/
source_anchor: ""
source_lines: [1, 49]
section: "Outils_Agents"
---
# AWS Supports Agent Plugins: An Open Standard for Portable Agent Extensions

If you’ve ever built an MCP server or agent skill, you know that packaging it means adapting for one client, re-writing for another, and repeating that for every tool your team uses. Agent Plugins 1.0.0 is an open source, vendor-neutral specification that gives AI agent extensions a common packaging format so you can package an extension once and ship it to any client, including Kiro, VS Code, Cursor, or any tool that implements the spec.

AWS is a founding member of the Agent Plugins Technical Steering Committee along with Cursor, Microsoft, OpenAI, and Vercel, and we’re excited to support this standard enabling portability for skills and MCP packaging across agents. We’ve already made AWS Agent Toolkit compatible and Kiro is rolling out support for the Agent Plugins spec.

Extensions are reusable components like skills, MCP servers, hooks, and sub-agents that give an AI agent new capabilities, connect it to external tools and data, or automate tasks on its behalf. This first version of Agent Plugins standardizes the two component types with the most adoption today: Agent Skills and MCP servers. Additional extension types like hooks and custom agents are on the roadmap for future versions.

While model choice gives developers flexibility on which AI you use, Agent Plugins gives you flexibility on which tools your AI uses without tying that decision to a specific client or provider.

## Why open standards matter for agentic AI

To date, managing agent extensions has been a bit like distributing libraries before package managers or shipping applications before standard container formats. Once JavaScript converged on package.json, tools like npm, yarn, and pnpm could all install the same package, ending manual script downloads and copy-paste dependency management. Similarly, OCI gave container images a vendor-neutral format so that a single build runs in Docker, containerd, Podman, or any compliant runtime, replacing Docker’s proprietary image spec. Open standards give everyone a shared foundation to build on.

At AWS, we believe that building with open source and open standards keeps AI architectures flexible and interoperable. Open standards give developers the freedom to compose, extend, and migrate their workloads on their own terms. When we invest in standards like the Model Context Protocol (MCP), Agent Client Protocol (ACP), x402, and now Agent Plugins, we’re making sure no single decision you make, from your model, to your tools or your client, locks you into a vendor.

## What Agent Plugins is

Agent Plugins defines how to package agent extensions so they’re easy to share, version, and install across tools. Under the hood, a plugin is a directory with a JSON manifest that declares the plugin’s identity and entry points, plus fixed locations for its components. A compatible client discovers what’s inside by scanning the directory structure. If there’s a skills/ folder, it loads skills; if there’s an MCP config, it connects to those servers.

The format is deliberately small. Version 1.0.0 standardizes Agent Skills (reusable instructions and resources for agents) and MCP servers (connections to external tools and data). How clients install, surface, or distribute plugins is intentionally outside the spec. Those are areas where clients can differentiate while sharing a common format underneath.

This is the right design philosophy. A shared format should define the essential components for interoperability, then get out of the way. Extension authors package once, then compatible clients discover and load components from the same structure. Clients retain full freedom to add proprietary capabilities through namespaced extensions without cluttering up the portable core.

## Agent Plugins is a community effort

Agent Plugins emerged from a practical problem that every team building agent tooling encountered independently. Extension authors were doing redundant work adapting the same components for different clients, and developers were losing time navigating incompatible packaging conventions.

Vercel released the initial specification draft, then formed a working group of companies whose teams had each been solving this problem in their own way. Representatives from AWS, Cursor, Microsoft, OpenAI, and Vercel refined the specification together, drawing on their respective experiences of building agent clients that developers use every day. The group established a governance structure that ensures no single company’s product roadmap dictates the format’s direction.

The result is a specification that reflects the actual needs of both extension authors and client implementers because both perspectives were in the room. The Technical Steering Committee includes core maintainers from all five founding companies, and the project’s contribution process and technical decisions are fully public.

This is how the best standards emerge. Not from a single vendor’s vision imposed on the ecosystem, but from the convergence of shared problems solved together. It’s the same pattern we’ve seen with MCP for tool connectivity, with OpenTelemetry for observability, and with container standards before them.

## How AWS supports Agent Plugins

We’re shipping Agent Plugins support at-launch across Kiro and the AWS Agent Toolkit, two key offerings where developers build and extend AI agents on AWS.

Kiro is an agent harness with steering mechanisms like specs, hooks, and automated tests that keep agents aligned with developer intent throughout the development lifecycle. With this launch, Kiro Powers, which extends Kiro through installable packages, now supports the Agent Plugins spec natively. Developers can install plugins that bundle Skills and MCP servers together, getting capabilities that work seamlessly without manually configuring each component.

AWS Agent Toolkit is a collection of official AWS-supported MCP servers, skills, and agent plugins that enable AI coding agents to build on AWS reliably. These are available through different packaging formats and bundle 30+ curated skills across multiple plugins covering services like Lambda, S3, DynamoDB, CDK, and more. Supporting the new Agent Plugins specification enables different harness / code assistants to utilize the same packaging format, ultimately reducing the fragmentation for how agents consume packages of agent extensions, like the AWS Agent Toolkit.

## Looking ahead

Agent Plugins 1.0.0 is a starting point. The specification will evolve as the ecosystem identifies additional component types that benefit from portability. The Technical Steering Committee is already considering how hooks, sub-agents, and other extension types might join Plugins in future versions.

We’re committed to this work because we’ve seen what open standards do for developer productivity. They remove the friction that prevents good tools from reaching the people who need them. Today it’s agent extensions. We expect more standards to emerge as the agentic AI ecosystem continues to evolve.

Get started with the specification at agent-plugins.org, explore the GitHub repository, and try installing your first plugin in Kiro.
