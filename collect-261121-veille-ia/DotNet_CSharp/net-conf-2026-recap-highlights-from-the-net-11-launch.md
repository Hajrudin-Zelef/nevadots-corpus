---
id: collect-261121-veille-ia/dotnet-csharp/net-conf-2026-recap-highlights-from-the-net-11-launch
title: ".NET Conf 2026 Recap: Highlights from the .NET 11 Launch"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["support", "november", "runtime", "microsoft", "net", "conf", "recap", "launch"]
source: https://Ironsoftware.com/news/industry-news/net-conf-2026-recap/
source_anchor: ""
source_lines: [1, 104]
section: "DotNet_CSharp"
---
# .NET Conf 2026 Recap: Highlights from the .NET 11 Launch

Microsoft shipped .NET 11 on November 10, 2026, opening .NET Conf 2026, a free four-day virtual event that ran November 10-13. The first two days belonged to Microsoft's product teams, with a .NET 11 launch keynote and a Visual Studio keynote; the last two were Community Days.

Our engineering team followed along. This recap covers the announcements we think matter most to .NET developers, plus a few notes on what caught our attention.

## At a glance

- .NET 11 is out. It's a Standard Term Support release with two years of support.
- .NET 8 and .NET 9 reached end of support on November 10, 2026.
- C# 15 adds union types, closed hierarchies and arguments in collection expressions.
- Performance and tooling: many runtime optimizations, a faster dotnet CLI and new supply-chain settings.
- AI: an MCP server project template, Microsoft.Extensions.AI and the prerelease Microsoft Agent Framework.
- Across the stack: updates to ASP.NET Core, Blazor, .NET MAUI, EF Core 11 and Windows desktop.

## .NET 11 release and support timeline

.NET 11 shipped on November 10, 2026, the same day .NET 8 and .NET 9 reached end of support. Apps still running on those versions no longer receive security updates.

| Version | Support type | Status | 
|---|---|---|
| .NET 11 | Standard Term Support (2 years) | Released November 10, 2026 | 
| .NET 10 | Long Term Support (3 years) | Supported | 
| .NET 9 | Standard Term Support | End of support November 10, 2026 | 
| .NET 8 | Long Term Support | End of support November 10, 2026 | 

Microsoft notes that STS and LTS releases get the same quality and servicing; only the length of support differs. For teams planning an upgrade, both .NET 10 and .NET 11 are supported targets.

## What this means for Iron Software users

If you're moving your application to .NET 10 or .NET 11, review your Iron Software library versions at the same time. Keeping your runtime and dependencies current helps reduce compatibility issues and gives you access to the latest fixes, platform support, and product improvements. Check the relevant product changelog before migrating.

## C# 15

The biggest language addition is union types: a value that is exactly one of a declared set of types. The compiler checks that a switch expression handles every case.

For syntax and examples, see Microsoft's What's new in C# 15 and the .NET Blog post Explore union types in C# 15.

Other C# 15 updates:

- Closed hierarchies: a closed class can only be derived from inside its own assembly.
- Collection expression arguments: you can pass constructor arguments, such as a comparer, inside a collection expression.
- Extension indexers and labeled break and continue.

System.Text.Json supports C# unions and F# discriminated unions, and ASP.NET Core can describe union results with anyOf schemas in OpenAPI. F# 11 also shipped, focused on compiler memory use and fewer allocations.

Unions caught our team's attention because they give API authors another way to model results besides exceptions and Try* methods. We'll be experimenting with them internally to see where they fit.

## Runtime and libraries

.NET 11 includes hundreds of runtime and library optimizations, with JIT improvements to bounds-check removal, devirtualization and constant folding. Some other runtime changes:

- Native AOT interface dispatch now uses a shared helper, and Half (FP16) operations have broader support.
- Crash reports can be logged in-process on Linux and macOS, not only on mobile.
- Runtime Async is available as a preview. It moves async suspension into the runtime and is turned on with runtime-async=on.
- Minimum hardware requirements went up for x86/x64 and Arm64.

New in the libraries:

- Built-in Zstandard compression in System.IO.Compression, and CRC32 validation for ZIP entries.
- Four in-memory stream types: ReadOnlyMemoryStream, WritableMemoryStream, ReadOnlySequenceStream and StringStream.
- An expanded Process API that runs a program and captures its output in one call.
- LINQ FullJoin.

As a team that works with a lot of file formats, we're looking forward to trying the new stream types and Zstandard support.

- Faster CLI: the dotnet CLI now starts as a Native AOT host and keeps a warm MSBuild server running. Both are on by default.
- File-based apps can use #:include, and dotnet run -e sets environment variables from the command line.
- dotnet watch integrates with Aspire app hosts, recovers from crashes and can select mobile devices.
- dotnet test on Microsoft.Testing.Platform adds run-level timeouts and failure limits.
- Containers: publishing produces reproducible image digests and skips uploading layers that haven't changed.
- NuGet adds a minimum package publication age setting, so builds can skip packages published very recently.
- dotnetup (public preview) manages side-by-side SDKs and runtimes.

The NuGet and container changes are small, but they're a welcome step for teams paying closer attention to software supply-chain security.

## AI in .NET

AI came up across many sessions, from the .NET AI stack on day one to AI in Visual Studio and the MCP C# SDK on day two. The main announcements:

- dotnet new mcpserver creates a Model Context Protocol (MCP) server project using the official MCP C# SDK, so .NET code can be exposed as tools for AI assistants.
- Microsoft.Extensions.AI provides common IChatClient and IEmbeddingGenerator abstractions for working with different model providers.
- Microsoft Agent Framework (prerelease) adds agents, tool use, workflows and AG-UI hosting.
- .NET Skills gives coding agents .NET-specific guidance.
- .NET MAUI DevFlow (experimental) lets agents inspect a running app's visual tree and take screenshots.
- .NET Upgrade canvas shows the progress of GitHub Copilot upgrade sessions visually.

The MCP template stood out to us. It makes it much easier to try connecting existing .NET code to AI assistants, and we expect to see a lot of experimentation here over the next year.

## Web, mobile, data and desktop

| Area | Highlights | 
|---|---|
| ASP.NET Core | OpenAPI 3.2 by default, async validation for Minimal APIs, union support in JSON, and SignalR authentication refresh without reconnecting | 
| Blazor | CacheView for static server-rendered output, opt-in circuit pause for inactive tabs, client-side validation for static forms, jump-to-item in QuickGrid and Virtualize, and experimental AI components | 
| .NET MAUI | CoreCLR as the default runtime, XAML Incremental Hot Reload (preview), implicit XAML namespaces, and removal of the Compatibility package | 
| EF Core 11 | Fewer unnecessary joins, better GroupBy translation, FullJoin support, and experimental SQL Server vector search | 
| Windows desktop | Opt-in modern visual styles in Windows Forms, reliability fixes in WPF, and WinUI 3 recommended for new apps | 

## Closing thoughts

.NET Conf 2026 had plenty for every kind of .NET developer, from language features to tooling to AI. For many teams, the most practical next step will be planning the move off .NET 8 and 9. After that, union types, the new libraries and the MCP tooling all look worth exploring.

If you missed the live sessions, the .NET Conf 2026 agenda lists every talk, and the .NET 11 announcement covers the release in full detail.
