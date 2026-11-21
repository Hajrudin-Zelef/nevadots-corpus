---
id: collect-261121-veille-ia/dotnet-csharp/weekly-net-roundup-net-11-arrives-aspire-retains-run-history
title: "Weekly .NET Roundup: .NET 11 Arrives, Aspire Retains Run History"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["support", "runtime", "release", "net", "arrives", "aspire", "history", "work"]
source: https://tech.hub.ms/dotnet/roundups/weekly-dotnet-roundup-2026-11-16
source_anchor: ""
source_lines: [1, 318]
section: "DotNet_CSharp"
---
# Weekly .NET Roundup: .NET 11 Arrives, Aspire Retains Run History

.NET 11 is generally available with two years of support, C# 15 unions and closed hierarchies, and expanded libraries, while .NET 8 and .NET 9 have reached end of support. Azure Functions now supports .NET 11 for C# isolated workers as in-process support ends, making hosting and SDK changes part of the upgrade rather than just a target-framework edit. Aspire 13.6 adds SQLite-backed diagnostic history and a Native AOT dashboard, retaining more telemetry while reducing memory use and startup time in reported benchmarks. Visual Studio 2026 version 18.12 enables SDK-powered Copilot Chat by default, while LTSC 18.11 offers a two-year fixed-feature support window. The releases pair migration requirements with concrete tooling improvements, but Runtime Async, C# memory-safety work, and Blazor AI components remain separate preview capabilities.

## This Week's Overview

## .NET 11 arrives as .NET 8 and .NET 9 reach end of support

.NET 11 is generally available, while .NET 8 and .NET 9 reached end of support on November 10, 2026. The release brings language and library additions, but developers should distinguish supported capabilities from runtime and memory-safety work that remains in preview.

### Release availability and support

.NET 11 is an STS release with two years of support, and its SDK is available to download. C# 15 adds union types and closed hierarchies, while the SDK includes a Model Context Protocol (MCP) server template.

Not everything discussed alongside the release is generally available. C# memory-safety work remains in preview, as do Runtime Async and the preview or prerelease Blazor AI components. Android Native AOT, which compiles applications to native code ahead of time, remains experimental.

### November servicing and security fixes

The November 10 servicing releases for .NET 10, .NET 9, and .NET 8 include security and non-security fixes. Release resources cover .NET 11.0.0, .NET 10.0.15, .NET 9.0.23, and .NET 8.0.34. Listed advisories address elevation of privilege, denial of service, information disclosure, security feature bypass, and Visual Studio remote code execution vulnerabilities.

Teams should consult each advisory rather than treat the release table as a complete affected-product inventory. The advisories contain the full affected-product lists and remediation guidance, while deployment resources include installers, binaries, Linux package instructions, and container images through Microsoft Container Registry.

### Runtime Async and memory safety

Runtime Async moves asynchronous orchestration and code generation from the Roslyn compiler into RyuJIT in the .NET Runtime. The change is intended to improve performance by putting that work under runtime control, rather than establishing a measured gain for every application.

The updated memory-safety model introduces stricter compile-time enforcement and makes unsafe-code use easier to audit. These are separate areas to evaluate: one changes how asynchronous code executes, while the other strengthens how developers identify and constrain unsafe operations.

### Numeric types and tensor operations

.NET 11 numerics coverage includes IEEE 754 decimal floating-point types, BFloat16, generic complex numbers, and generic random-number APIs. The discussion connects these additions to generic math, with attention to precision, range, rounding, parsing, and formatting.

Layout-aware tensor operations address both performance and correctness across different shapes, overlapping memory, and special values. The session distinguishes improvements in .NET 11 from further tensor work planned for .NET 12.

### JSON serialization for new data models

System.Text.Json in .NET 11 adds serialization support for C# unions and closed hierarchies. Expanded source-generation capabilities and richer annotations for ordinary C# objects give developers more ways to describe serialization behavior, with the session explaining when to use those capabilities.

### Post-quantum cryptography

.NET 11 expands the platform's post-quantum cryptography capabilities. The accompanying session covers their present-day use and preparation for future cryptographic requirements, without making a specific migration prescription for every application.

## Aspire expands its developer loop and retains diagnostic history

The Aspire coverage spans releases 13.4 through 13.6, connecting broader language support with dashboard persistence and interactive tooling. Implementation reports explain how the dashboard uses SQLite and Native AOT to retain more diagnostic data while reducing memory use and startup time.

### TypeScript AppHosts and cross-language debugging

Aspire 13.4 makes TypeScript AppHosts generally available, using `apphost.mts` as the entry point and generated SDK modules under `.aspire/modules/`. Startup validation runs before the application, and first-party Go hosting adds OpenTelemetry, health checks, module helpers, and Delve debugging support. The `aspire logs` and `aspire otel` commands can search on the server before streaming results back.

The Visual Studio Code extension debugs C# and TypeScript AppHosts and coordinates debugging for C#, JavaScript, TypeScript, Python, and Go resources. Resource commands accept typed arguments, defaults, choices, validation, and secret inputs, which the extension collects through prompts. For AppHosts started from Visual Studio Code, automatic dashboard launch stopped beginning mid-June, though settings can restore it.

The broader toolchain discussions explain how .NET remains Aspire's orchestration and runtime foundation without limiting application languages to .NET. They also describe how supporting additional languages prompted changes to the original .NET-focused workflow, while distinguishing available improvements from work still to come.

- Aspire 13.4 is here
- Aspire in VS Code: the 13.4 developer loop
- How Aspire uses .NET to build an agent-first toolchain for any programming language
- What's New in Aspire for .NET Developers

### Dashboard persistence and run history

Aspire 13.6 stores resource snapshots and telemetry in SQLite databases, retaining up to 10 application runs with optional pinning. Persistence is enabled by default when an AppHost launches the dashboard, and default limits for console logs, structured logs, and traces increase from 10,000 to 100,000 entries each. The standalone dashboard instead defaults to `None`, with `Run` and `Resume` options for retaining data.

The implementation combines SQLite storage with Dapper queries and build-time query and mapping generation for Native AOT. In the reported benchmark, private memory usage fell from 1,007 MB in Aspire 13.5 to 241 MB in 13.6, a 76% reduction. That result accompanies higher retention limits rather than a reduction in the amount of diagnostic data available.

Persisted telemetry has no separate encryption or authorization layer, so the dashboard remains a development and short-term diagnostics tool. Preview database REPL support opens authenticated clients inside the dashboard and requires explicit opt-in because it grants full write access.

### Resource interactions and terminal sessions

Aspire 13.5 introduces a refreshed dashboard, file imports for resource commands, progress dialogs, and interactive terminal sessions. The Core Interaction Service and file input APIs are stable, while progress dialogs and terminal APIs remain experimental. Terminal resources currently run as plain processes, so developers must attach debuggers manually.

`WithTerminal()` and `withTerminal()` attach terminal emulators to projects, executables, and containers, and Aspire 13.6 adds docked terminals and richer emulation. Built-in REPL commands support Redis, Mongo, SQL Server, Postgres, Valkey, and MySQL. The experimental `aspire terminal tape play` command automates an existing terminal and prints its final screen, rather than producing a video or recording file.

### Native AOT dashboard publishing

Aspire 13.6 publishes its dashboard using Native AOT and targets .NET 11. Median page-visible startup falls from 1,429 ms to 491 ms, a 66% reduction, while the native dashboard totals 110.22 MB compared with approximately 183.08 MB for the managed dashboard and required runtime.

The dashboard executable no longer needs a separately installed .NET runtime, though the CLI still ensures one is available for other Aspire components. This should not be read as general Native AOT support for Blazor Web Apps: that publishing path remains experimental, despite the enabling Blazor changes landing in .NET 11 RC1.

### Repeatable adoption across repositories

The multi-repository rollout report describes more than 50 microservice repositories using Aspire by May 2026, up from approximately 15 in the previous report. Its MARS system combines Aspire, Microsoft Agent Framework, and GitHub Copilot SDK in a check, plan, fix, validate, and learn loop. Repository health comes from deterministic C# metrics, with cloud checks grounded in Azure DevOps pull request and build snapshots.

The team estimates that agentic onboarding removes 4-8 hours of manual setup per repository. More than 700 end-to-end tests run on every pull request against the rollout system, providing a concrete validation mechanism for the automation.

### Isolated end-to-end service testing

Azure Chaos Studio uses Aspire to run roughly 90 hermetic tests, isolated from shared environments, on every pull request in Azure Pipelines. Tests run in parallel across the real graph of four independently shipped services, using Cosmos DB and Storage emulators, a community Key Vault emulator, and WireMock where no usable emulator exists.

Coverage includes zone outages, identity outages, DNS failures, and geo-replication failures. The test fixture waits for real resource health checks before executing tests and tears down the application afterward, making readiness and cleanup explicit parts of the test lifecycle.

## Azure application migrations require hosting and SDK changes

Azure Functions now supports .NET 11, while Logic Apps Standard migration guidance targets .NET 10. Both require attention to project configuration and hosting behavior, not only a target-framework edit.

### Azure Functions adopts .NET 11 for isolated workers

.NET 11 is generally available in Azure Functions for C# isolated worker applications, while F# support is temporarily unavailable. Projects require `Azure.Functions.Sdk` version 1.0.1 or later, and `Microsoft.Azure.Functions.Worker` version 2.50.0 or later is recommended. The new SDK replaces the build SDK package, not the worker runtime package, and requires changing the project SDK declaration.

In-process model support ended on November 10, 2026, and Linux Consumption does not support .NET 10 or later. Container users must account for .NET 11 Functions base images being chiseled images without a shell or SSH server, with no non-chiseled or `appservice` tags published.

The Azure weekly update reinforces the support transition by covering the end of .NET 8 and .NET 9 support in Azure Static Web Apps. It also includes Azure Functions .NET 11 availability alongside broader Azure networking and operational updates.

### Logic Apps Standard connector migration

Following last week's Logic Apps Standard migration coverage, the new guidance addresses NuGet-based applications and custom built-in connectors. Migration preserves connector registration code and workflow definitions, but projects must target `net10.0` and run on a .NET 10 host. Prerequisites include .NET SDK 10.0.403 or later and Azure Functions Core Tools 4.16.0 or later.

Configuration differs between environments: local development uses `FUNCTIONS_WORKER_RUNTIME=dotnet-isolated`, while Azure retains `dotnet` and removes `LOGICAPP_INPROC_REDIRECT`. Extension bundle configuration must be removed from both `host.json` and Azure app settings so the packaged runtime can register custom connectors. Validation covered a sample connector action locally and in Azure, not triggers or applications combining NuGet-based and custom-code projects.

## Copilot changes its Visual Studio execution model

Visual Studio's SDK-powered Copilot experience changes how developers direct and inspect agent work. Related modernization coverage shows how that assistance fits into migrations without removing developer review.

### SDK-powered chat becomes the default

The new Copilot Chat experience is enabled by default in Visual Studio 2026 version 18.12. On 157 real C# tasks using GPT-5.6 Sol, it resolved 124 tasks compared with 103 for the previous experience. Developers can submit instructions during a task, inspect individual step outputs, and follow delegated subagents through separate cards.

Agent skills can work unchanged across Visual Studio, VS Code, Copilot CLI, and the GitHub Copilot app. Plan mode and chat exports are still being brought over, so developers who need the previous experience can disable the new one in settings and restart Visual Studio. The accompanying AI overview discusses the SDK-powered agent harness, model and provider access, and configuration controls.

### Agent-assisted modernization with review boundaries

The modernization walkthrough follows a real .NET migration from assessment of potential breaking changes through preparation for team review. It identifies where the agent performs work and where control returns to developers, making those handoffs part of the upgrade process. Custom skills extend the workflow when a codebase does not fit the default migration path.

## Visual Studio separates long-term servicing from feature delivery

Visual Studio 2026 LTSC 18.11 gives teams a longer fixed-feature support window. Separate sessions cover the editor, debugging, and diagnostic experiences available in the broader Visual Studio development workflow.

### LTSC 18.11 support and channel choice

Visual Studio 2026 LTSC 18.11 is available with support through November 14, 2028, extending LTSC support from one year to two. It includes .NET 10 LTS and the MSVC 14.50 and 14.52 toolsets. LTSC receives security and critical fixes rather than regular feature updates, and installations must remain current with servicing updates to stay supported.

Microsoft recommends the Stable channel for most customers, with monthly feature and quality updates required to remain supported. Existing Visual Studio 2026 Enterprise, Professional, and Build Tools product keys work on both channels without an additional purchase. The choice is therefore about update policy and feature cadence, not buying another license.

### Debugging and diagnostics

The debugging and diagnostics demonstrations cover identifying issues, understanding performance, and validating fixes. They include hands-free, AI-assisted debugging and continuing investigations inside the IDE, connecting assistance with the practical work of checking whether a change solved the problem.

### Editor and productivity improvements

The Visual Studio feature tour covers code navigation, editor customization, build and testing experiences, and working with Markdown and web files. The keynote provides a broader view of current capabilities and previews future directions, so its forward-looking material should be distinguished from the demonstrated feature tour.

## Database work targets connection startup and query capabilities

SqlClient and Entity Framework Core address different parts of database access: establishing connections under concurrent load and expressing queries and models. The SqlClient measurements are specifically about cold-start connection establishment, not faster SQL execution.

### SqlClient Pool V2 opens connections concurrently

Pool V2 is available starting with Microsoft.Data.SqlClient 7.1.0 and must be enabled before any database connections open. Unlike the legacy pool, which opens only one new connection at a time within a pool, Pool V2 creates connections concurrently. With 100 concurrent Linux callers, synchronous cold-start time fell from 630.1 ms to 117.5 ms, and asynchronous time fell from 604.1 ms to 92.0 ms.

Those benchmarks measure connection establishment and coordination, not query execution or reuse of an already warm pool. Pool V2 still makes synchronous network calls on managed thread-pool threads, so high-latency connections can increase thread-pool pressure. Teams should evaluate it against their connection-startup workload rather than expect a general database throughput improvement.

### EF Core 11 expands search, LINQ, and migration tooling

EF Core 11 coverage includes approximate vector search and full-text search on SQL Server, plus LINQ operators such as `FullJoin` and `MaxBy`/`MinBy`. A revised query pipeline generates leaner SQL, while modeling improvements cover complex types and JSON.

The `dotnet ef` tooling supports configuration files, file-based applications, and one-step migrations. The session also covers Cosmos DB bulk and batch support and upgrade considerations, giving developers both new capabilities to evaluate and migration details to review.

## C# develops closed data models and labeled control flow

Following last week's coverage of union types and closed hierarchies, the language discussions examine their modeling value alongside other C# additions. A separate demonstration focuses on labeled loop-control statements.

### Expressing a closed set of data shapes

The C# discussion covers union types, closed classes, and extension indexers. Mads and Dustin explain how unions express a closed set of data shapes, using wire protocols as a practical application, and how those models communicate intent to consuming code.

### Labeled break and continue

Nick Chapsas introduces labeled `break` and `continue` as a C# 15 feature. The demonstration contrasts them with `goto`, through which C# has long supported labels, to explain the distinction between the new control-flow statements and existing label-based jumps.

## .NET tooling revisits startup and build execution

Native compilation and execution-model changes appear in the CLI, C# Dev Kit, and MSBuild coverage. Each targets a different part of the development loop, and the reported results should not be treated as interchangeable build-speed claims.

### A native dotnet CLI

The .NET team describes making the `dotnet` CLI a native application in .NET 11. The session highlights responsiveness and flexibility, while covering transition problems and lessons relevant to developers building their own Native AOT applications.

### C# Dev Kit consolidates its backend

C# Dev Kit 11 replaces five backend processes with a single Native AOT server. In team benchmarks, large workspaces that previously took more than a minute to load now open in a few seconds. Persistent caches avoid recomputing data, and a new incremental build system accelerates common builds.

Support for other editors, command-line tools, and agent-driven workflows is discussed as a possible direction. It is not an announced capability of the release.

### Multithreaded MSBuild

The .NET team presents multithreaded MSBuild as a new execution mode for improving build performance. The session covers trying it, understanding its effect on existing build workflows, and preparing projects and tools for multithreaded execution.

## ASP.NET Core and Blazor address server resources and agent interfaces

The web-development coverage connects server-side rendering and resilient interactivity with experimental agent-facing components. The server improvements and AI interface experiments have different maturity levels and solve different problems.

### Server rendering and resilient sessions

Blazor in .NET 11 combines static server-side rendering with Blazor Server interactivity under the same component model. Static rendering supports client-side validation, temporary data, session state, and output caching without requiring an interactive circuit, the server-side connection and state used for interactivity.

Persistent component state and circuit pause and resume avoid duplicate work and preserve sessions across disconnections. Pausing inactive circuits frees resources and supports graceful deployments. The broader ASP.NET Core session adds asynchronous validation, richer API contracts, and deeper Aspire integration to the discussion of scalable and distributed web applications.

- Blazor Server in .NET 11: Fast, resilient, and scalable
- Modern web development with ASP.NET Core & Blazor in .NET 11

### Experimental agent-facing components

Experimental Blazor AI components work alongside the open AG-UI protocol in demonstrations using Microsoft Agent Framework, Microsoft Foundry, and Aspire. Examples cover streaming interactions, custom tool experiences, human-in-the-loop workflows, and shared application state. The session emphasizes showing agent progress and tool activity while keeping users in control.

## .NET MAUI changes its runtime and command-line workflow

.NET MAUI's .NET 11 coverage separates the move to CoreCLR from optional compilation choices and developer tooling. Application-level AI demonstrations then show how existing services and interface state can support tool-driven interactions.

### CoreCLR on mobile platforms

In .NET 11, .NET MAUI moves to CoreCLR on Android, iOS, Mac Catalyst, and tvOS. The runtime session compares Mono AOT, ReadyToRun, and NativeAOT, while making clear that NativeAOT's additional constraints are separate from the default runtime change.

Trimming and JSON source generation are covered as preparation for NativeAOT. Android and iOS startup and application-size results provide material for performance and compatibility testing rather than a reason to assume every application will behave identically.

### Run, test, and watch on devices

.NET 11 brings `dotnet run`, `dotnet test`, and `dotnet watch` to .NET MAUI mobile development. `dotnet run` selects a device or emulator, installs the application, and launches it, while `dotnet test` captures test results and `dotnet watch` updates a running application after code changes.

Android and iOS support still requires platform tooling, packaging, device communication, and application-process lifetime handling. The demonstrations cover local development, automated tests, and continuous integration using real .NET MAUI projects.

### AI tools that act on application state

A conference companion demonstration connects an assistant to tools for finding sessions, updating a personal agenda, and navigating application screens. It reuses existing services and view models so the conversation and interface share application state.

The implementation uses Microsoft.Extensions.AI and experimental .NET MAUI Labs tooling. Validation, confirmation before destructive actions, and handling requests the assistant cannot fulfill are part of the design, not separate concerns left outside the demonstration.

## Agent applications gain durable execution and deployment examples

Agent coverage ranges from a generally available Durable Task extension to experimental interfaces and hosting demonstrations. The practical distinctions are where state lives, how execution resumes, and which responsibilities remain with the application.

### Durable execution outside the worker process

The Durable Task extension for Microsoft Agent Framework is generally available and stores session and execution state outside the worker process. During replay, completed agent calls with recorded results reuse those results instead of calling the model again. Workflows can also wait for external approval events without keeping a worker active.

Applications still need to persist lightweight session references themselves and use the same Durable Task backend across requests. C# and Python applications can run through Azure Functions or self-hosting, with execution visibility in the Durable Task Scheduler dashboard. Durable execution therefore does not remove the application's responsibility for identifying and reconnecting to the correct session.

### Composing and observing distributed agents

The AlpineAI example combines .NET and Python specialist agents, a Go telemetry generator, a React frontend, and Microsoft Foundry resources in one Aspire AppHost. Its advisor exposes four A2A specialists and a Foundry prompt agent as five tools. The chat advisor is a Foundry hosted agent, while the voice advisor uses a .NET WebSocket service connected to Azure AI Voice Live.

Aspire models Azure Container Apps placement, Cosmos DB conversation storage, dependencies, and health checks. Dashboard traces connect advisor processing, specialist calls, model calls, and downstream HTTP requests. The broader .NET AI stack session provides context on composing model abstractions, retrieval, tools, agents, local inference, and infrastructure.

- Distributed multi-agent systems with Aspire and Microsoft Agent Framework
- The .NET AI Stack: What's New, What's Next, and How It Fits Together

### Foundry hosting and preview limitations

The hosted-agent session examines moving .NET agents from localhost into Microsoft Foundry using existing C# and .NET skills. A separate experiment replaces four hosted Microsoft Agent Framework wrappers with one Foundry-managed Copilot harness agent, while a React workbench and Node.js backend retain authentication, streaming, conversation ownership, and authorized downloads.

That experiment uses the signed-in user's context for Fabric access and the project's managed identity for Foundry IQ. Early tests returned correct sales figures but missed the requested explanatory document, with clarified instructions producing one successful retrieval from both sources. The harness is in public preview, and the demonstration does not establish production readiness, feature parity, or improved quality, latency, or cost.

- Beyond Localhost: .NET Agents in Foundry
- From Python Agents to Copilot Harness : Putting Microsoft Foundry to the Test

### MCP server implementation

The MCP C# SDK session covers SDK updates and the latest Model Context Protocol revision through a practical C# server. It demonstrates scalable implementation with stateless requests and collecting user input across multiple interactions. Existing MCP clients remain supported as the protocol evolves.

### A hardware companion backed by .NET services

The desk-companion demonstration splits responsibilities between C++ on M5Stack/StackChan hardware and a .NET 11 backend. The hardware controls the display, movement, and sensors, while Aspire orchestrates backend services and exposes telemetry.

Microsoft Foundry supplies chat and vision models, Microsoft Agent Framework supports agent behavior, and GitHub Copilot SDK supports tool-driven developer scenarios. The WorkIQ demonstration uses synthetic or sanitized work context, and sample code plus a setup guide accompany the project.

## Windows desktop development balances compatibility and new UI approaches

Windows desktop coverage spans compatibility-preserving WinForms changes, WinUI tooling, and an experimental C# interface framework. These are different adoption paths rather than a single replacement strategy.

### WinForms modernization preserves existing behavior

WinForms modernization in .NET 11 prioritizes behavioral and designer compatibility for existing business applications. An opt-in visual-styles generation updates control metrics and appearance while leaving compatibility as the default. The session covers toggle switches, flash-reduced startup, live Windows settings, focus and text-scaling improvements, and smoother batched interface updates.

The architectural guidance separates designer state, presentation, and infrastructure from application logic and irreplaceable business rules. It also distinguishes using AI to assist modernization from adding runtime features such as domain-specific copilots or local and connected AI services.

### WinUI project and packaging workflows

The WinUI session demonstrates new `dotnet new` templates and how `dotnet run` handles package identity for Windows applications. It covers building, running, debugging, testing, and packaging in Visual Studio and VS Code, with WinAppCLI and AI coding agents supporting repetitive development tasks. Native AOT is discussed as an option for delivering fast, efficient applications.

### Experimental native interfaces in C#

Microsoft.UI.Reactor applies the Model-View-Update pattern to WinUI 3 in an experimental C# framework. The demonstration starts a complete application with `dotnet run app.cs`, stores interface state in hooks, and renders native WinUI controls with the same runtime and theming as XAML applications.

The session covers lists, keyed reconciliation, and more than 60 Roslyn analyzers that detect interface bugs while typing. It also discusses limitations and how XAML and Model-View-ViewModel approaches fit alongside the experiment.

### Upcoming Windows Runtime projection redesign

CsWinRT 3.0 is an upcoming redesign of Windows Runtime projection support for .NET. The session describes a redesign based on developer feedback, rather than an already available migration target.

## Other .NET News

The Excel-reading guide compares forward-only streaming with ExcelDataReader, object mapping with ExcelMapper, and cell-level editing with ClosedXML, using samples compiled and run on .NET 10. Legacy `.xls` reading requires the code pages encoding provider, while the dependency-free XML example omits dates, inline strings, formulas, and skipped empty cells and is suitable only for known, simple files.

For teams maintaining .NET 10 code, the C# 14 reference covers the `field` keyword, null-conditional assignment, deferred LINQ execution, asynchronous I/O, nullable reference analysis, and project configuration. Its short examples provide a reference for that language baseline rather than the C# 15 release.

The AI-assisted testing session uses MSTest, Microsoft Testing Platform, and modern .NET tooling to identify coverage gaps, generate and improve tests, and validate changes. Developers retain responsibility for final decisions about tests, changes, and code quality.

## Also relevant this week

- Introducing Azure Container Apps as a target workload in Azure Migrate: Azure Migrate adds Azure Container Apps as a target workload for assessing discovered web applications. Full story in the Azure roundup.
- SCKit abuses ScreenConnect to deploy remote access and target technician sessions: Microsoft confirmed reported ScreenConnect abuse and observed attackers installing unauthorized remote-access clients and deploying SCKit on customer-managed endpoints. Full story in the Security roundup.
- Announcing PowerShell 7.7 GA Release: PowerShell 7.7 is a generally available Standard Term Support release built on .NET 11. Full story in the DevOps roundup.
- Bring Your Own Model is now generally available in Visual Studio: Bring Your Own Model is generally available in Visual Studio 18.12. Full story in the GitHub Copilot roundup.
- Azure's new lineup for .NET developers: The video presents Azure capabilities for .NET developers through live demonstrations. Full story in the Azure roundup.
- From Local Development to CI Workflows with the Azure Cosmos DB Emulator: FleetCharge uses Python, Microsoft Agent Framework and FastAPI, with Phi-3.5 inference running locally through Ollama. Full story in the DevOps roundup.
- Inspecting Local Data with Azure Cosmos DB Shell: The walkthrough uses Azure Cosmos DB Shell 1.1.280-preview to inspect FleetCharge dispatch records in the local Linux emulator. Full story in the Azure roundup.
- Canada’s 2026 time zone updates: guidance for Microsoft SQL: Affected Canadian locations will retain UTC−07:00, UTC−06:00, and UTC−05:00 rather than turn clocks back on November 1, 2026. Full story in the Azure roundup.
- 2 Secure 2 Curmudgeonly: The session reviews .NET security bugs from 2025–2026. Full story in the Security roundup.
- What’s new across Microsoft SQL in 2026 so far: the PASS Summit West roundup: Hyperscale elastic pools preview supports up to 50 databases and 128 TB per pool, depending on hardware and service objective. Full story in the Azure roundup.
- Small Models, Big .NET Energy: Session BRK037 demonstrates Foundry Local and small AI models running on a developer’s own hardware. Full story in the Artificial Intelligence roundup.
- Protect your PC from AI coding agents while modernising legacy .NET apps: The session discusses isolation options for AI coding agents using GitHub Copilot CLI modernisation plugins on legacy .NET applications. Full story in the Security roundup.
- Your AI Agent Never Sleeps! 🔥 Background Agents Explained: The video introduces background agents in Microsoft Agent Framework. Full story in the Artificial Intelligence roundup.
- Securing Your NuGet Dependencies in the Age of AI: Jon Douglas presents a session on securing NuGet dependencies in the age of AI. Full story in the Security roundup.

## Dates to know

### Deadlines

- **14 November 2028** : Visual Studio LTSC: Support ends for Visual Studio 2026 LTSC 18.11. (Announcing Visual Studio 2026 LTSC 18.11)
