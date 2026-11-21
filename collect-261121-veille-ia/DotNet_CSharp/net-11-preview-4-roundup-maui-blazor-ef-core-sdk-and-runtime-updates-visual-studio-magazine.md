---
id: collect-261121-veille-ia/dotnet-csharp/net-11-preview-4-roundup-maui-blazor-ef-core-sdk-and-runtime-updates-visual-studio-magazine
title: ".NET 11 Preview 4 Roundup: MAUI, Blazor, EF Core, SDK and Runtime Updates -- Visual Studio Magazine"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-05-21"]
keywords: ["maui", "dotnet", "preview", "core", "updates", "runtime", "visual", "studio"]
source: https://visualstudiomagazine.com/articles/2026/05/21/net-11-preview-4-roundup-maui-blazor-ef-core-sdk-and-runtime-updates.aspx
source_anchor: ""
source_lines: [1, 75]
section: "DotNet_CSharp"
---
News

        
        ### .NET 11 Preview 4 Roundup: MAUI, Blazor, EF Core, SDK and Runtime Updates

        
        
        
        
Microsoft released .NET 11 Preview 4, a broad update covering runtime, SDK, libraries, ASP.NET Core, .NET MAUI, C# and Entity Framework Core.

The company said the fourth preview includes improvements across the .NET Runtime, SDK, libraries, ASP.NET Core, .NET MAUI, C#, Entity Framework Core and other areas. The notes don't appear to feature any major, standout headline features. For Windows developers using Visual Studio, Microsoft recommends installing the latest Visual Studio 2026 Insiders build. The company also said developers can use Visual Studio Code with the C# Dev Kit extension for .NET 11 development.

**MAUI Moves to CoreCLR by Default**

One of the more significant changes for .NET client developers is in .NET MAUI, where CoreCLR is now the default runtime on all MAUI platforms for projects built with and targeting .NET 11 Preview 4.

The company deemed this worthy of its own blog post. "This isn't entirely new ground," said David Ortinau in that post. "We have been adding platforms to CoreCLR since .NET Framework starting with Windows and then Linux, macOS (AppKit), and Android. What's new in .NET 11 is we're extending that same runtime to Android, iOS, and Mac Catalyst. These were the last .NET MAUI platforms that were still on Mono."

    
The release notes describe the change as a runtime unification effort with benefits for debugging, profiling, Hot Reload, app size and app performance. However, the same notes include an important tooling caveat: there is not yet a debugger available for CoreCLR on mobile platforms. Developers who need to debug iOS or Android apps in Visual Studio or Visual Studio Code must opt back into Mono by setting `UseMonoRuntime`.

Preview 4 also adds .NET MAUI support for `dotnet watch` on Android devices and emulators, with deployment and Hot Reload changes applied as developers edit. On iOS, Microsoft said several long-standing issues were fixed to make `dotnet watch` usable end to end on a `dotnet new maui` project running in the iOS Simulator. The release notes still list a known issue requiring `MtouchLink=None` for iOS Simulator projects.

Other .NET MAUI updates include Material 3 styling across several Android controls, an `x:Code` directive for inline C# in XAML, compiled binding fixes inside `DataTemplate` elements, support for Android adaptive-icon monochrome layers and Apple platform updates tied to Xcode 26.4 Stable.

**SDK Updates Focus on Mobile Workflow and CLI Plumbing**

The .NET SDK release notes expand on the MAUI workflow changes, noting that `dotnet watch` now mirrors the `dotnet run` device-selection flow for MAUI and mobile targets. After target-framework selection, `dotnet watch` can call the `ComputeAvailableDevices` MSBuild target, auto-select a device when only one is available or show an interactive device-selection prompt when several are available.

The SDK also includes reliability fixes for `dotnet watch`, including fixes for the target-framework picker, Ctrl+C/Ctrl+R restart handling and an iOS Hot Reload deadlock. Other command-line changes include fish shell completion updates, current-directory fallback behavior for `dotnet reference add` and `dotnet reference remove`, restored NuGet CLI help forwarding and moving the `Using launch settings from ...` message from stdout to stderr.

Preview 4 also lays groundwork for a NativeAOT-compiled `dotnet` CLI host. The new entry point is not yet the default `dotnet` binary, but Microsoft said the goal is near-instant startup for common CLI invocations while preserving full functionality for other commands.

**ASP.NET Core Adds MCP Template and Blazor Updates**

ASP.NET Core in Preview 4 includes several web-development updates, including a change likely to draw attention from developers building AI-related tooling: the `mcpserver` project template now ships as a bundled template in the .NET SDK.

The ASP.NET Core release notes say the Model Context Protocol server template was previously available only by installing `Microsoft.McpServer.ProjectTemplates`. Developers can now create a project with `dotnet new mcpserver -o MyMcpServer`, and the template is discoverable from `dotnet new list`.

Blazor also gets several updates. The `Virtualize` component no longer shifts visible content when items above the viewport change height. A new `AnchorMode` parameter controls viewport behavior when items are prepended or appended, with documented scenarios including chat UIs, news feeds and log viewers.

Other Blazor changes include a new `SupplyParameterFromTempData` attribute for server-side rendering components, a server-initiated Blazor Server circuit pause API, a Blazor WebAssembly service-defaults template with Aspire integration and updates to the Blazor Web Worker template, including `InvokeVoidAsync`, cancellation and timeout support.

For API developers, ASP.NET Core OpenAPI document generation now recognizes HTTP QUERY as a known operation type. The release notes describe QUERY as a proposed safe, idempotent method that allows a request body for search scenarios where a query is too large or structured to fit in a URL.

**EF Core Adds SQL Server Vector Search Support**

Entity Framework Core in .NET 11 Preview 4 adds approximate nearest-neighbor vector query translation for SQL Server 2025. Calling `VectorSearch()` returns rows ordered by vector distance, while `WithApproximate()` instructs SQL Server to use a vector index and translates to the `WITH APPROXIMATE` clause introduced in SQL Server 2025.

The EF Core release notes say exact search remains available when `WithApproximate()` is not used, which is useful for verifying recall on small data sets. Microsoft said the feature builds on SQL Server 2025 vector index configuration added in earlier .NET 11 previews.

EF Core also now treats JSON columns as first-class citizens in the relational model, with structured JSON path representation intended to improve diagnostics, partial-update SQL and migration/compiled-model behavior. SQL Server temporal table period columns can now map to regular CLR properties, and several `DateTimeOffset` operations now translate to SQL Server queries.

For tooling, `dotnet ef` can now read default values for common options from a `.config/dotnet-ef.json` file. The release notes say discovery walks up from the working directory, similar to `.editorconfig` or `global.json`, while command-line arguments still take precedence.

**Libraries Get Process API Expansion**

The .NET libraries update includes a substantial expansion of `System.Diagnostics.Process`. Microsoft published a separate Process API Improvements in .NET 11 post describing the work as especially significant.

"The `System.Diagnostics.Process`
 class is the primary way to create and interact with processes with .NET," explained Adam Sitnik." We made the biggest update to it in years, with .NET 11. The changes add high-level APIs that make it easy to start a process and capture its output without deadlocks, give you full control over handle inheritance and standard handle redirection, introduce lifetime management features like `KillOnParentExit`
 , and include a lightweight `SafeProcessHandle`-based API surface that is more trimmer-friendly."

New APIs include `Process.Run`, `Process.RunAsync`, `Process.RunAndCaptureText` and `Process.RunAndCaptureTextAsync` for one-shot process execution and output capture. Additional APIs cover deadlock-free stdout/stderr reads, fire-and-forget launch scenarios, process lifetime management, inherited-handle control, detached processes and lower-level `SafeProcessHandle` lifecycle operations.

The libraries release notes also include span-based Deflate, ZLib and GZip encoder/decoder APIs, floating-point hexadecimal formatting and parsing, UTF validation APIs, built-in OpenTelemetry metrics for `MemoryCache` and `System.Text.Json` support for F# discriminated unions.

**Runtime Work Includes Runtime-Async and JIT Optimizations**

The runtime release notes say runtime libraries are now built with `runtime-async=on`, meaning they rely on runtime-provided async support rather than compiler-generated state machines. Microsoft said it expects runtime async to deliver throughput and library-size improvements, with results varying by async usage.

The runtime notes also list Just-In-Time compiler optimizations, including constant folding for `string.Equals` and `ReadOnlySpan.SequenceEqual` when operands are constants, bounds-check elimination after empty-span guards, redundant branch folding and hardware-intrinsics work. Other runtime items include ReadyToRun improvements, Browser/WebAssembly CoreCLR enablement and platform support for systems with more than 1,024 CPUs.

Microsoft said developers can get started by installing the .NET 11 SDK. As a preview release, the features and APIs remain subject to change before general availability.
