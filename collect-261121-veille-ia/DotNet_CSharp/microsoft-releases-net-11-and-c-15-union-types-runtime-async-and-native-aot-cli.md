---
id: collect-261121-veille-ia/dotnet-csharp/microsoft-releases-net-11-and-c-15-union-types-runtime-async-and-native-aot-cli
title: "Microsoft Releases .NET 11 and C♯ 15 - Union Types, Runtime Async and Native AOT CLI"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-10"]
keywords: ["microsoft", "async", "runtime", "native", "types", "union", "class", "server"]
source: https://www.kunal-chowdhury.com/2026/11/dotnet-11-csharp-15-release-features.html
source_anchor: ""
source_lines: [1, 76]
section: "DotNet_CSharp"
---
Add **kunal-chowdhury.com** on Google to see more of our coverages in your search results.

For more than a decade, C# architects have begged Microsoft for native discriminated unions and an end to the hidden memory allocations caused by compiler-generated `async/await` state machines. At .NET Conf 2026, Microsoft delivered both in a single release by shipping **.NET 11 and C# 15** into general availability.


Backed by 24 months of Standard Term Support (STS) through November 2028 and shaped by over 7,000 community contributors, this release replaces async state-machine boilerplate with 0-byte Runtime Async execution, turns on a **Native AOT CLI host** by default, and moves **.NET MAUI onto CoreCLR**. At the same time, November 10, 2026 marks the official End of Support for both .NET 8 and .NET 9—making your upgrade roadmap an immediate priority.




`public union` return types and assembly-sealed `closed class` hierarchies with full compiler exhaustiveness checking in `switch` expressions.`runtime-async=on`, slashing synchronous async completion from 21.2 ns (144 B) down to 6.2 ns (0 B allocated) and cleaning up live debugger call stacks.`dotnet new mcpserver` template, and introduces the preview `dotnetup` toolchain manager.


As confirmed in Microsoft's official Announcing .NET 11 blog post, **C# 15** is the default language version for all `net11.0` projects. Its headline addition is native **union types**—letting a method return any of a declared set of case types without boxing into `object` or inventing artificial base classes:



When you own the class hierarchy yourself, C# 15 also introduces **closed hierarchies** (`closed class`), which prevent external assemblies from subclassing your type so the compiler can guarantee exhaustive pattern matching. In addition, C# 15 adds **collection expression arguments**—such as `HashSet<string> names = [with(StringComparer.OrdinalIgnoreCase), "Alice", "ALICE", "Bob"];`—alongside extension indexers and labeled `break` and `continue` statements.



Ever since C# 5 introduced `async` and `await`, the Roslyn compiler has rewritten every async method into a complex state-machine struct that clutters live profiler call stacks and allocates extra heap memory on awaits. In .NET 11, **Runtime Async** moves suspension and resumption directly into the CoreCLR virtual machine—and Microsoft has already compiled the entire **.NET 11 Base Class Library (BCL) with runtime-async=on**.


You can opt your own `net11.0` application code into Runtime Async inside Visual Studio 2026 v18.11 simply by adding `<Features>runtime-async=on</Features>` to your project file (no preview flag required). The speed and memory gains across Microsoft Learn's .NET 11 runtime benchmarks are dramatic:



Daily terminal workflows on a modern Windows 11 developer workstation get an immediate responsiveness boost because the .NET 11 SDK enables a **Native AOT CLI host by default** and keeps the background **MSBuild server warm** between invocations. Version queries, help commands, and local tool launches execute instantaneously without spinning up the managed CLR, while the new preview **dotnetup toolchain manager** simplifies user-level SDK installations.


For AI engineers building agentic systems, .NET 11 bakes Model Context Protocol (MCP) server architecture right into the SDK. Running `dotnet new mcpserver -o MyMcpServer` scaffolds a production-ready C# MCP tool server in seconds, which pairs cleanly with **Microsoft.Extensions.AI (MEAI)**, the **Microsoft Agent Framework (MAF)**, Microsoft Foundry IQ deployments, and the new **.NET Upgrade Canvas** inside GitHub Copilot.



Web, cloud, and data-access workloads receive deep stack-wide integration for C# 15 and high-density server hosting. Here is how the major workload upgrades break down across the .NET 11 stack:




On the client side, **.NET MAUI 11** completes a foundational architectural shift by replacing the legacy Mono runtime with **CoreCLR as its default runtime** across mobile and desktop platforms (using precompiled code and the interpreter on Apple iOS/iPadOS where JIT is restricted). That unification means your mobile apps now share the exact same runtime semantics and base library optimizations as your cloud services.


Developer iteration in MAUI and Windows desktop frameworks is also much smoother. .NET MAUI 11 enables **XAML Incremental Hot Reload** by default in Debug builds, supports implicit XAML namespace declarations, switches Windows to the faster `CollectionView2` handler, and pairs with the experimental **.NET MAUI DevFlow** agentic UI inspector—while **Windows Forms** gains an opt-in modern visual styles mode and suspended bulk painting.



There is a critical security deadline buried inside the .NET 11 launch announcement: both **.NET 8 (LTS) and .NET 9 (STS) reached End of Support on November 10, 2026**. As we saw during last month's emergency out-of-band .NET security update for CVE-2026-70197, running unpatched runtimes in production exposes web APIs and Kestrel servers to severe risk.


If your enterprise policy mandates a Long-Term Support track, migrate your remaining .NET 8 and .NET 9 services to **.NET 10 (LTS)** immediately. If you want C# 15 union types, 0-byte Runtime Async, and the 24-month STS support window through November 2028, update your `global.json` and target frameworks to `net11.0`, verify your container base images against .NET 11's updated minimum x64/Arm64 CPU instruction baselines, and test your deployment pipeline today.














Between C# 15 union types, Runtime Async cutting synchronous await overhead to 0 bytes, a snappy Native AOT CLI host, and CoreCLR becoming the default runtime for .NET MAUI, **Microsoft's .NET 11 release** ranks among the most impactful upgrades in the platform's history. Are you migrating your production workloads straight to `net11.0` for C# 15 unions, or holding steady on .NET 10 LTS? Let me know in the comments below!



Solution Architect & Former Microsoft MVP

**Kunal Chowdhury** is an enterprise solution architect and former multi-year Microsoft MVP. He is the author of three technical books: *Windows Presentation Foundation Development Cookbook*, *Mastering Visual Studio 2017*, and the *Mastering Visual Studio 2019*.

He publishes technical articles on Kunal-Chowdhury.com.

-->
