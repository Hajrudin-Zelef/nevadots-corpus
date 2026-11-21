---
id: collect-261121-veille-ia/dotnet-csharp/net-11-is-out-what-actually-matters-if-you-build-ai-apps-in-c
title: ".NET 11 Is Out: What Actually Matters If You Build AI Apps in C#"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["union", "tool", "build", "apps", "types", "server", "string", "actually"]
source: https://medium.com/@satyamyadavaiml/net-11-is-out-what-actually-matters-if-you-build-ai-apps-in-c-371c72fa2264
source_anchor: ""
source_lines: [1, 79]
section: "DotNet_CSharp"
---
# .NET 11 Is Out: What Actually Matters If You Build AI Apps in C#

*Union types, a built-in MCP server template, and the upgrade deadline you should not ignore.*

.NET 11 shipped on November 10 at .NET Conf. The release notes are long, and most of it is runtime and tooling work you get for free just by upgrading. But I build AI agents for a living, and I also spend time in the MCP C# SDK repo, so I read the announcement with one question in mind: what changes how I write AI-powered .NET apps on Monday morning?

Here are the four things I think are worth your time, plus one deadline.

## 1. C# 15 union types are a natural fit for agent tool results

The headline language feature in C# 15 is union types. A union says “this value is exactly one of these types”, and the compiler checks that your `switch` handles every case.

If you have built agents, you know the problem this solves. A tool call does not just succeed or fail. It can succeed, it can fail in a way that is worth retrying, or it can stop and wait for a human to approve the next step. Until now, I modelled that with a base class, a status enum, or a result object full of nullable fields. All of these let you forget a case.

With C# 15 you can write this:

```
public sealed record ToolSuccess(string Output);
public sealed record ToolNeedsApproval(string Action, string Reason);
public sealed record ToolFailed(string Error, bool Retryable);
```
public union ToolOutcome(ToolSuccess, ToolNeedsApproval, ToolFailed);static string NextStep(ToolOutcome outcome) => outcome switch

{

ToolSuccess s => $"Send result back to the model: {s.Output}",

ToolNeedsApproval a => $"Pause and ask a human: {a.Action} ({a.Reason})",

ToolFailed { Retryable: true } f => $"Retry: {f.Error}",

ToolFailed f => $"Stop and report: {f.Error}",

};

The case types convert to the union automatically, so a tool method can simply `return new ToolNeedsApproval(...)`. If someone later adds a fourth outcome, every `switch` that does not handle it becomes a compiler problem instead of a production surprise.

In agent systems, the “needs approval” path is the one that tends to get forgotten. Making it a first-class case that the compiler forces you to handle is exactly the kind of guardrail I want.

Two related points from the release notes: `System.Text.Json` now supports C# unions, and ASP.NET Core supports them in JSON request and response bodies, with OpenAPI describing the alternatives as `anyOf`. That support is for JSON bodies only, not route, query or form binding. If two cases have the same JSON shape, you need to configure a classifier so deserialization can tell them apart.

## 2. The MCP server template now ships with the SDK

This one is small but I am happy about it. The .NET 11 SDK includes an MCP server template built on the official MCP C# SDK, with a working sample tool:

`dotnet new mcpserver -o MyMcpServer`
No separate template package to find and install first. For teams that want to expose existing .NET business logic to AI clients, this lowers the first step to one command.

It is also worth knowing where the SDK itself is. The MCP C# SDK reached v2.0 in July, implementing the 2026–07–28 MCP spec revision. The big change is that the HTTP transport is stateless by default: there is no initialize handshake and no `Mcp-Session-Id` header. If your server needs state across calls, the recommended pattern is to return an explicit handle (for example, a basket ID) from one tool and have the model pass it into the next. If you are upgrading an older server, read the v2 notes before you assume sessions still work the way they did.

## 3. The AI building blocks are clearer now

Microsoft is now presenting a simple three-layer story for AI in .NET:

- **Microsoft.Extensions.AI** (MEAI) for model access through abstractions like`IChatClient` and`IEmbeddingGenerator` , composed with dependency injection, caching and OpenTelemetry.
- **MCP** for tool integration.
- **Microsoft Agent Framework (MAF)** for agents that call tools, keep conversation state, and run workflows.

The practical point: MEAI and MAF ship as NuGet packages. You do not have to move your whole app to .NET 11 to use them. If you are on .NET 10 LTS and want to add AI features, you can start today.

There are also new Blazor AI components for streaming chat, tool rendering and approval flows, which can connect to a remote agent through AG-UI. I like that approval flows are part of the component set. But be careful: the package is experimental and stays prerelease throughout .NET 11. Treat it as something to prototype with, not something to ship to customers yet.

## 4. EF Core 11 adds vector search for SQL Server (experimental)

If you build RAG on top of SQL Server, EF Core 11 adds support for vector indexes and approximate vector search. This could let some teams keep embeddings next to their relational data instead of running a separate vector database.

Again, the SQL Server features and the EF APIs are marked experimental. I would test it against your real data size and latency needs before choosing it over a dedicated vector store. EF Core 11 also requires .NET 11, so this one does depend on the framework upgrade.

## The deadline: .NET 8 and .NET 9 are out of support

.NET 8 and .NET 9 reached end of support on November 10, 2026, the same day .NET 11 shipped. If you still run production services on either, you are no longer getting fixes.

You have two sensible choices. .NET 11 is a Standard Term Support release with two years of support and the newest features. .NET 10 is the LTS option if you want a longer runway. For the upgrade itself, the official checklist is practical: update `global.json` if you pin the SDK, change your target framework, review the breaking changes (including new minimum CPU requirements), then run your tests and measure under real load.

## My takeaway

For AI developers, .NET 11 is not about one big feature. It is about the pieces fitting together: unions to model agent outcomes safely, a built-in MCP template, clear AI abstractions, and a database layer that is starting to understand vectors.

My plan is simple. I will try union types on a tool-result model first, because that is low risk and the payoff is immediate. I will try the Blazor AI components and EF Core vector search in a side project, not in production. And if you are still on .NET 8 or 9, that upgrade should be at the top of your list this month.
