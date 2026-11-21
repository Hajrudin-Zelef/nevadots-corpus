---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-releases-net-11-with-c-15-and-coreclr-for-maui-itdaily
title: "Microsoft releases .NET 11 with C# 15 and CoreCLR for MAUI - ITdaily"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "runtime", "support", "maui", "default", "releases", "coreclr", "types"]
source: https://itdaily.com/news/software/net-11-csharp-15-coreclr-maui/
source_anchor: ""
source_lines: [1, 27]
section: "Copilot_Microsoft"
---
**Microsoft has launched .NET 11, featuring union types in C# 15, a Native AOT host for the CLI and new components for AI agents. It is an STS release with two years of support.**

Microsoft has released .NET 11 at .NET Conf 2026. The release includes updates for C# 15, ASP.NET Core, .NET MAUI and Windows development, plus hundreds of optimisations in the runtime.

.NET 11 is a Standard Term Support (STS) release and gets two years of support. Long Term Support (LTS) releases such as .NET 10 get three years. Quality and maintenance are the same for both, only the duration differs. More than 7,000 contributors worked on the project.

## What’s new in C# 15 and the SDK

C# 15 is the default language version for projects targeting .NET 11. Union types let a single value be one of several predeclared types, with exhaustiveness checking in switch expressions. Closed hierarchies are also added, in which a closed class can only be derived within its own assembly. Smaller additions include arguments for collection expressions, extension indexers and labelled *break* and *continue*.

The SDK uses a Native AOT host for the CLI by default. Commands such as help and version queries therefore run without starting the managed CLI. The MSBuild server also stays warm between consecutive calls, so startup repeats less work.

For the runtime, Microsoft points to improvements in the JIT compiler, which removes redundant bounds checks and branches. Runtime Async, which moves the suspending and resuming of asynchronous code to the runtime, remains a preview feature. Developers can enable it with the runtime-async=on setting.

## AI agents and web development

The .NET 11 SDK includes a template for an MCP server. For applications that use AI services, Microsoft.Extensions.AI provides abstractions for model access. Microsoft Agent Framework builds on this with agents that call tools and keep track of conversation state.

ASP.NET Core gets asynchronous validation and OpenAPI 3.2 as the default for generated documents. Blazor gets a component that caches rendered output and an option to automatically pause inactive circuits. Experimental Blazor AI components work with the Agent User Interaction Protocol (AG-UI) and remain a prerelease throughout .NET 11.

.NET MAUI now uses CoreCLR as its default runtime. On Apple platforms, where JIT compilation is not allowed, the runtime runs on precompiled code and an interpreter. XAML Incremental Hot Reload is available as a preview feature and is enabled by default in debug builds.

## Upgrading to .NET 11

Developers can download and use the .NET 11 SDK with Visual Studio 2026 or Visual Studio Code with C# Dev Kit. For an existing application, installing the SDK alone is not enough, as the target framework must also be changed. Microsoft recommends reading the breaking changes, including the new minimum CPU requirements.

.NET 8 and .NET 9 lose their support on 10 November 2026. Organisations running those versions must therefore choose between .NET 11 and the LTS alternative, .NET 10. The NuGet ecosystem now counts more than 589,000 unique packages and more than 1.1 trillion downloads.
