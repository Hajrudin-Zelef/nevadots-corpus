---
id: collect-261121-veille-ia/claude-anthropic/claude-marketplace-anthropic-centralizes-connectors-and-agents-mail-studio
title: "Claude Marketplace: Anthropic centralizes connectors and agents | Mail Studio"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-23"]
keywords: ["marketplace", "claude", "connectors", "anthropic", "agents", "agent", "three", "consulting"]
source: https://www.mail-studio.com/en/ai/claude-marketplace-connectors-agents/
source_anchor: ""
source_lines: [1, 42]
section: "Claude_Anthropic"
---
Anthropic launched the Claude Marketplace on September 23, 2026, a single place to discover connectors, agents and service partners built around Claude. The announcement organizes into three blocks an ecosystem that had until now grown in a scattered way, spread across homegrown MCP extensions, third-party integrations and consulting offers. For technical teams already running Claude in production, the main change is how they discover and justify adding a new tool.

## Three categories, one shared standard

The marketplace lists more than 2,000 connectors and extensions, built on the open Model Context Protocol and Agent Skills standards, with integrations for Atlassian, Google, Microsoft, Notion and Salesforce. A second category gathers Claude-powered software from vendors such as CrowdStrike, Cursor, Harvey, Legora, Lovable and Snowflake. The third brings together consulting and integration firms from the Claude Partner Network, including Accenture, Boston Consulting Group and Deloitte.

## Already-committed spend, usable directly

Customers who hold a spend commitment with Anthropic can put part of it toward buying agents and products listed on the marketplace, with no separate purchasing process. That’s the main practical change for a team evaluating a third-party agent: the budget barrier and the discovery barrier fall away at the same time.

## How a vendor publishes a connector

For a developer or product team, publishing follows three distinct paths depending on the type of offering.

| Offering type | Publishing path | Technical standard | 
|---|---|---|
| Connector / plugin | Direct submission | Model Context Protocol, Agent Skills | 
| Agent / product | Marketplace application | Depends on the integration | 
| Consulting service | Claude Partner Network application | Not applicable | 

A minimal MCP server skeleton, of the kind expected for a connector submission, fits in a few lines:

```
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
const server = new McpServer({ name: "example-connector", version: "1.0.0" });
server.registerTool("search", {
  description: "Searches the target system",
  inputSchema: { query: "string" },
}, async ({ query }) => {
  return { content: [{ type: "text", text: `Result for ${query}` }] };
});
```
More than 2,000 connectors listed at launch: the marketplace doesn’t create the MCP ecosystem, it simply makes it visible in one place.


## What to remember

The Claude Marketplace doesn’t change the technical standards themselves: MCP and Agent Skills remain the foundation. What changes is distribution and funding, with a single storefront and the option to pay through an already-signed spend commitment. Teams already running editorial agents in production gain an extra distribution channel for their own internal connectors, should they choose to open them up.

A centralized marketplace is convenient for finding a connector in five minutes instead of digging through GitHub, but it also shifts the power of visibility toward Anthropic: being well ranked across those three categories will quickly become a business concern for third-party vendors, not unlike ranking in a mobile app store. Worth watching over time, not underestimating just because the underlying protocol stays open — **Simon Janvier**.

Further reading: the full announcement on Anthropic’s blog.
