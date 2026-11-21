---
id: collect-261121-veille-ia/dotnet-csharp/microsoft-releases-net-11-with-c-15-union-types-a-faster-sdk-and-ef-core-11
title: "Microsoft Releases .NET 11 With C# 15 Union Types, a Faster SDK and EF Core 11"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["core", "types", "union", "microsoft", "runtime", "version", "support", "closed"]
source: https://vinish.dev/dotnet-11-released-csharp-15-union-types
source_anchor: ""
source_lines: [1, 71]
section: "DotNet_CSharp"
---
Microsoft shipped .NET 11 on November 10, 2026, on the opening day of .NET Conf 2026. The release brings C# 15 as the default language version, a quicker command-line SDK, many runtime speedups, and updates to ASP.NET Core, Blazor, Entity Framework Core and .NET MAUI.

.NET 11 is a Standard Term Support (STS) release, so it gets two years of fixes. The timing matters: .NET 8 and .NET 9 reached end of support on the same day, so teams still on those versions now need to move to .NET 10 or .NET 11 to keep receiving patches.

## C# 15 Brings Union Types and Closed Hierarchies

The headline language feature is union types. A union declares that a value is exactly one of a fixed list of types, and the compiler checks that a switch expression handles every case. Microsoft's example is a lookup that returns either a found product or a not-found record, with no shared base class needed.

C# 15 also adds closed classes. A closed class can only be derived from inside the assembly that declares it, which lets the compiler verify pattern matching over that hierarchy. Unions suit combining types you already have, while closed hierarchies suit models where you control the inheritance.

Several smaller additions round out the release:

- Collection expressions can now take arguments, for example passing a case-insensitive comparer when building a HashSet.
- Extension indexers join the extension member family.
- Labeled break and continue make it easier to exit nested loops.

None of these need the preview language setting. The separate memory safety work in C# stays in preview. F# 11 also ships, with lower compiler memory use and new ways to cap concurrent asynchronous work.

## A Faster SDK and Runtime

The SDK now uses a Native AOT host for the dotnet command by default. Simple commands such as help or version checks no longer start the managed CLI, and the MSBuild server stays running between builds to cut repeated startup cost. Linux and macOS installers are also smaller.

Other tooling changes include:

- File-based apps can pull in extra source files with the #:include directive.
- dotnet watch now works with Aspire app hosts, recovers from crashes and lets you pick a target device for mobile work.
- Container publishing can produce reproducible image digests based on SOURCE_DATE_EPOCH.
- NuGet can require a minimum age before a newly published package version is eligible, a useful supply chain control.
- A new preview tool called dotnetup installs and updates SDKs and runtimes per user.

On the runtime side, the JIT removes more bounds checks, devirtualizes more calls and folds more constants. Native AOT interface calls use less code, and Half values can use FP16 hardware instructions on supported x64 and Arm64 chips. Runtime Async, which moves async suspension into the runtime and produces cleaner live call stacks, remains a preview opt-in.

## Libraries and EF Core 11

System.IO.Compression now supports Zstandard. The Process API gains one-call methods that run a program and return its output and exit code. Four new stream adapters let code pass existing memory, sequences or strings to Stream-based APIs without copying, and LINQ adds a FullJoin operator that keeps unmatched rows from both sides.

For database developers, Entity Framework Core 11 is the key piece. It drops needless joins in more navigation and split-query cases, translates GroupBy better, and maps the new FullJoin to SQL on providers that support it. Complex types and JSON columns now work with table-per-type and table-per-concrete-type inheritance.

EF Core 11 also adds experimental support for SQL Server vector indexes and approximate vector search. It requires .NET 11, so it cannot be used on .NET 10 projects.

## Web, Mobile and Desktop Updates

ASP.NET Core understands C# unions in JSON request and response bodies and describes them in OpenAPI output. OpenAPI 3.2 becomes the default document version, and validation can now run asynchronously, so a rule can check a database without blocking a thread. SignalR connections can refresh authentication without reconnecting.

Blazor adds a CacheView component that caches rendered markup during static server rendering, plus an opt-in pause for inactive Interactive Server circuits that frees server memory. An experimental Blazor AI components package covers chat, tool output and approval flows.

.NET MAUI now runs on CoreCLR by default and adds incremental XAML hot reload in preview. The old Microsoft.Maui.Controls.Compatibility package is gone. Windows Forms gains an opt-in modern visual style, while WPF gets reliability fixes.

## What It Means for Developers

The most urgent point is not a feature but a date. Any production app on .NET 8 or .NET 9 is now unsupported, so plan a move. Pick .NET 10 if you want a long support window, or .NET 11 if you want the new features and can upgrade again within two years.

Union types are worth learning early. They make result and error handling explicit, and since System.Text.Json and ASP.NET Core already support them, they can flow straight through an API. Teams using EF Core should read the breaking changes list and compare generated SQL before and after the upgrade, since query translation has changed.

Developers following language trends can see how C# compares with other options in our guide to the best programming language to learn in 2026.

## How to Upgrade

Install the .NET 11 SDK and use an up-to-date Visual Studio 2026 or VS Code with C# Dev Kit. Then update global.json if the repository pins an SDK, change target frameworks to net11.0, and bump packages and workloads to matching versions.

Microsoft advises checking the breaking changes, which include higher minimum CPU requirements, before rollout. Run tests and measure under real load, and check container base images and native dependencies. Anyone on a .NET 11 release candidate should move to the final build.

## Conclusion

.NET 11 is a broad release with union types in C# 15, faster everyday tooling and a smarter EF Core. Arriving on the same day that .NET 8 and .NET 9 lose support, it gives many teams a clear reason to upgrade now.

## Sources

**Get more Vinish.dev tutorials on Google**

See my articles more often when you search Google.
