---
id: collect-261121-veille-ia/dotnet-csharp/net-11-ships-with-c-15-unions-faster-tools-and-coreclr-for-maui-visual-studio-magazine
title: ".NET 11 Ships with C# 15 Unions, Faster Tools and CoreCLR for MAUI -- Visual Studio Magazine"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-10"]
keywords: ["faster", "unions", "types", "ships", "tools", "coreclr", "maui", "union"]
source: https://visualstudiomagazine.1105cms01.com/articles/2026/11/10/net-11-ships-with-csharp-15-unions-faster-tools-and-coreclr-for-maui.aspx
source_anchor: ""
source_lines: [1, 74]
section: "DotNet_CSharp"
---
News

        
        ### .NET 11 Ships with C# 15 Unions, Faster Tools and CoreCLR for MAUI

        
        
        
        
Microsoft released .NET 11 on Nov. 10, bringing C# 15 union types, faster command-line tooling and substantial changes for web and mobile developers, alongside an expanding stack for building AI agents and their interfaces.

The general availability announcement opens .NET Conf 2026 with a release whose practical appeal extends from compiler checks that catch missing application states to less waiting between CLI commands. .NET 11 is a Standard Term Support release with two years of support; .NET 10 remains the supported Long Term Support option.

During the livestreamed .NET 11 announcement at the conference, Damian Edwards illustrated how .NET has progressed through five eras.

For developers deciding where to spend their upgrade effort, three themes stand out: expressing application contracts more precisely, reducing development and execution overhead, and improving the foundations of interactive applications. The release also draws an important boundary: several prominently featured AI and runtime capabilities remain experimental or preview even though .NET 11 itself has shipped.

**C# 15: Make Application Outcomes Explicit**

The most consequential language addition is the ability to declare a union whose value can be one of a fixed set of existing types. A service can describe its possible outcomes in its return type, giving callers a contract the compiler can inspect.

    
In Microsoft's earlier union types walkthrough, Bill Wagner, C#/.NET principal content developer, explained the benefit: "That's one core value: the compiler catches missing cases at build time, not at runtime." Adding another case to a union can produce warnings in switch expressions that no longer handle every possibility.

That makes unions useful for results such as success, missing data or a validation failure, without requiring those types to inherit from an artificial common base. There are implementation details to consider in performance-sensitive code: the shorthand declaration generates a struct whose object-valued storage boxes value types. Libraries can implement a recognized custom-union pattern with non-boxing accessors.

The C# 15 documentation also describes closed hierarchies. Marking a class `closed` restricts direct derivation to its assembly, allowing exhaustiveness checking over that known set of descendants. The restriction is on direct descendants; an intermediate descendant must itself be closed to extend that checking farther down a hierarchy.

Smaller additions reduce everyday bookkeeping. Collection expressions can pass constructor or factory arguments through a leading `with(...)` element, for example to supply a case-insensitive comparer. Extension indexers let developers add indexing syntax to a receiver, while labeled `break` and `continue` can target an enclosing loop directly. C# 15 is the default for `net11.0` projects, and these features do not require the preview language setting. The separate memory-safety work still does.

**Faster Feedback, Then Faster Applications**

The SDK and tooling changes target repeated costs in the development loop. The CLI host uses Native Ahead-of-Time compilation by default, allowing commands such as help and version queries to avoid managed CLI startup. Commands that need managed execution continue to use it. MSBuild also keeps its server warm between CLI invocations by default.

Other tooling improvements include smaller Linux and macOS installers through assembly deduplication, multi-file support for file-based apps through `#:include`, and additional control over test execution with Microsoft.Testing.Platform. The latter's options should be checked against the selected runner rather than assumed to match VSTest switches.

Package policy is another practical upgrade consideration. As *Visual Studio Magazine* reported in its RC2 coverage, NuGet can enforce a minimum publication age when selecting eligible versions. That provides a way to delay consumption of newly published packages, alongside existing dependency review and vulnerability checks. RC2 also brought faster MSBuild wildcard matching and MAUI C# UI templates.

Application performance receives a broader set of optimizations. In his performance deep dive, Microsoft Distinguished Engineer Stephen Toub described benefits from JIT improvements arriving "often with no source changes or recompilation of the application itself." His examples examine how the runtime eliminates abstraction costs, unnecessary checks and allocations when it can prove they are unnecessary.

The extensive microbenchmarks are useful starting points for investigation, rather than a single speedup figure to apply to every application. Teams should compare throughput, allocation, startup and publishing size using their own workloads and deployment settings.

The runtime release notes identify a separate experiment worth watching: Runtime Async moves suspension and resumption work from compiler-generated state machines into the runtime. It remains a preview feature for application code, enabled with `<Features>runtime-async=on</Features>`. A `net11.0` project no longer needs `EnablePreviewFeatures` for this opt-in.

The immediate diagnostic benefit concerns live execution stacks seen by debuggers and profilers. Real async methods appear more directly, with fewer state-machine infrastructure frames. Exception stack traces already received cleanup under the existing implementation, so developers should distinguish those two scenarios when evaluating the change.

**Web Apps: Async Validation and Agentic Interfaces**

For web developers, ASP.NET Core 11 addresses work that commonly crosses a process or network boundary. Minimal APIs and Blazor support asynchronous validation, including rules that query a database or call another service. Blazor's form submission pipeline awaits async validators, with APIs for field-level work, cancellation and pending-state reporting.

OpenAPI generation now defaults to version 3.2. Teams whose downstream tooling requires an older specification can explicitly select it. SignalR adds authentication refresh without reconnecting, while Blazor gains additional control over rendering and long-running UI sessions. Those operational changes deserve testing alongside business logic during an upgrade.

The more exploratory web theme is agentic UI. Microsoft's Blazor AI components walkthrough shows how streamed responses, tool activity, approvals and shared state can be rendered through familiar Razor components. Principal Product Manager Daniel Roth described their role: "They turn agent interactions into content blocks and observable state that Blazor components can render and control."

The sample brings together Microsoft.Extensions.AI, the AG-UI .NET SDK, Microsoft Agent Framework, ASP.NET Core, Microsoft Foundry and Aspire. A `UIAgent` supplies interaction state to the components, while AG-UI provides an event-based connection to remote agents. The Blazor components remain experimental and are distributed in a prerelease package; the runtime's GA milestone does not make that UI package production-stable.

**MAUI's Runtime Transition**

For cross-platform developers, MAUI's move to CoreCLR is a fundamental platform change. CoreCLR is the default across MAUI platforms, bringing the mobile stack closer to the runtime used by server and desktop applications. On Apple platforms, that does not imply enabling JIT compilation: execution combines precompiled code and interpretation. Native AOT remains an opt-in publishing choice, with Android support experimental.

In an earlier transition update, Principal Product Manager David Ortinau advised: "Don't assume universal improvement, and don't assume universal regression either. Measure your app, then tell us what you see." That remains useful guidance for checking startup, package size, platform integrations and third-party dependencies on actual devices. Blazor WebAssembly continues to use Mono.

MAUI also adds implicit XAML namespaces and previews incremental XAML Hot Reload. Its servicing lifecycle follows a separate MAUI support policy, which mobile teams should consult when planning their upgrade schedule.

**More Changes to Put on the Upgrade List**

Beyond those main themes, the release includes updates across data access, libraries, desktop development and modernization:

- **Libraries:** The library additions include built-in Zstandard compression and one-shot process APIs for launching a command and capturing output and exit status. New stream adapters expose existing memory and sequences without first copying them. System.Text.Json gains C# union and F# discriminated-union support, and LINQ adds FullJoin.
- **Data access:** EF Core 11 improves queries, supports FullJoin translation where providers implement it, and extends complex types and JSON columns to entities with table-per-type or table-per-concrete-type inheritance. SQL Server vector APIs remain experimental. EF Core 11 requires the .NET 11 runtime.
- **Windows desktop:** Windows Forms adds opt-in modern visual styles, appearance and accessibility handling, deferred form display and bulk-painting controls. WPF work covers reliability, backdrop support and deployment dependencies.
- **Modernization:** Microsoft's Copilot upgrade canvas displays assessment, planning, implementation tasks and results. It gives developers a place to follow and steer a migration as dependency and build issues change the work required.

The GA release follows the production-testing phase covered in VSM's RC1 report, when .NET 11 gained go-live support. Teams moving from those candidates should update SDK pins, target frameworks, packages, workloads and deployment images, then check breaking changes and the updated minimum CPU requirements. Those hardware baselines can matter for older deployment targets even when the application itself compiles successfully.
