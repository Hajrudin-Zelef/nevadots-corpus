---
id: collect-261121-veille-ia/copilot-microsoft/net-conf-2026-recap-the-announcements-that-actually-matter
title: ".NET Conf 2026 Recap: The Announcements That Actually Matter"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["more", "conf", "code", "announcements", "release", "recap", "actually", "matter"]
source: https://www.igniscor.com/post/net-conf-2026
source_anchor: ""
source_lines: [1, 167]
section: "Copilot_Microsoft"
---
.NET Conf 2026 Recap: The Announcements That Actually Matter

35 minutes ago

9 min read

If your upgrade plan for the next two years rests on three letters – LTS – .NET Conf 2026 just gave you a reason to read the fine print. Somewhere between the keynote demos and the release notes sits a detail that can quietly push part of your stack out of support a year earlier than you expected.

The conference ran from November 10 to 13, opened with the launch of .NET 11, and packed four days with sessions on the runtime, ASP.NET Core, .NET MAUI, Aspire, Visual Studio, and AI tooling. That’s a lot to take in, and most of it won’t change how you build software next quarter. Some of it will.

We went through the announcements, release notes, and session materials to separate the headline features from the changes that affect real projects. Below, you’ll find what got faster on the server without rewriting your code, what changed underneath your mobile apps, and which AI tools are ready for production and which are still experiments. And yes – we’ll get to that fine print and what it means for your upgrade plan.

Four Days, One Release: .NET Conf 2026 at a Glance

For the 16th year in a row, .NET Conf followed a familiar rhythm: a free, online event in November that ships the next version of .NET. This year’s edition stretched to four days. November 10 and 11 belonged to Microsoft’s product teams – the .NET 11 launch keynote on day one, a Visual Studio keynote from Mads Kristensen on day two – while November 12 and 13 were handed over to Community Days, with talks from developers building real-world projects.

The release itself is the work of more than 7,000 contributors, and it lands in an ecosystem that now counts over 589,000 unique packages on NuGet and more than 1.1 trillion downloads. The headline language features – C# 15 union types, the MCP C# SDK, OpenAPI 3.2 by default – have been visible in previews for months, so we’ll only touch on them where they change something practical.

The more interesting story is what happened below the language level. Here’s a quick map of where the changes landed:

For server-side teams, .NET 11 is less about new syntax and more about getting more from the code you already have. The biggest wins land in places you rarely touch directly: the JIT, the build pipeline, the way async code shows up in a profiler, and the SQL your ORM generates. Together, they add up to services that run faster, fail more transparently, and can cost less to build and operate – often without changing a single line of application code.

Runtime and SDK: Speed You Get for Free

Stephen Toub’s annual deep dive counts hundreds of runtime and library optimizations in this release. Most of them require no code changes at all – retarget, rebuild, and measure on your own workload before promising anyone a number.

JIT compiler. It removes more redundant bounds checks and branches, devirtualizes more calls, and folds more operations into constants. Hot paths in existing services get faster after a simple rebuild.

Native AOT. Interface calls now go through a shared helper that cuts the code needed at each call site. Services built around abstractions benefit the most.

Hardware support. FP16 instructions now power Half operations on supported x64 and Arm64 CPUs, which matters for numeric and ML-adjacent workloads.

CLI and build. The dotnet CLI runs on a Native AOT host by default, and the MSBuild server stays warm between invocations. Each command saves a little, and across hundreds of CI runs it adds up.

Diagnostics. In-process crash reports now work on Linux and macOS, capturing the managed stack trace, module information, and runtime state before the process exits – exactly what you need when a container restarts at 3 a.m.

Libraries. Built-in Zstandard compression, one-call process execution with captured output and exit code, and JSON Lines output in System.Text.Json mean less custom plumbing and fewer third-party packages.

The most ambitious change is still in preview. Runtime Async moves async suspension and resumption from compiler-generated state machines into the runtime itself. The payoff is a cleaner live call stack: profilers and debuggers show your actual async methods instead of the plumbing that runs them. Opting in takes one property:

It’s worth experimenting with now, but test your dependencies and diagnostics before turning it on anywhere that matters.

ASP.NET Core and EF Core 11: Less Boilerplate, Cleaner SQL

The web stack picks up the new C# 15 features and closes a few long-standing gaps:

Unions in APIs. ASP.NET Core supports unions in JSON request and response bodies for both Minimal APIs and MVC, and OpenAPI describes them with an anyOf schema. An endpoint can return either an order or a “not found” result as one type, and API clients see both outcomes in the contract. The support covers JSON bodies only, not route, query, or form binding.

Async validation. Validation rules can await a database lookup or a remote call instead of blocking a thread. Checks like “is this email already taken?” finally fit naturally into Minimal APIs and Blazor forms.

OpenAPI 3.2 by default. Generated documents can now describe the payload of each event in a server-sent event stream. If your client generator isn’t ready, you can still select an earlier version.

SignalR. Long-lived connections can refresh authentication without reconnecting, and Blazor Server components can react when the user’s claims change.

Blazor Server. An opt-in circuit pause frees server memory and the SignalR connection held by inactive browser tabs – useful for apps where many users leave a tab open all day.

EF Core 11, in turn, focuses on the SQL it generates:

Leaner queries. It removes unnecessary joins in more navigation and split-query scenarios and translates GroupBy better, so more queries can stay in LINQ.

FullJoin. The new LINQ operator keeps unmatched rows from both sides and translates to SQL on supporting relational providers – handy for reconciliation tasks like comparing two inventories.

Flexible modeling. Complex types and JSON columns now work with table-per-type and table-per-concrete-type inheritance.

Vector search. SQL Server gets vector indexes and approximate vector search, a first step toward semantic search inside your existing database. Both the database features and the EF APIs are still experimental.

One planning note: EF Core 11 requires .NET 11, so the ORM and runtime upgrades travel together. Check your database provider’s compatibility before you schedule either.

Mobile: The Runtime Under Your MAUI App Has Changed

Since Microsoft acquired Xamarin in 2016, Mono has quietly powered every Xamarin and .NET MAUI app. With .NET 11, that era ends: mobile apps now run on CoreCLR, the same runtime behind ASP.NET Core and desktop .NET. For teams that build both the app and the backend, this means one runtime, one set of diagnostics, and one CLI across the entire product.

CoreCLR Replaces Mono

The switch didn’t happen overnight. CoreCLR became the default for MAUI on Android, iOS, and Mac Catalyst in Preview 4 back in May, and by Preview 6 in July it was the only option – the property for selecting Mono was removed. The final release keeps it that way.

Here’s what that means in practice:

On Android, CoreCLR uses tiered compilation: methods start with fast, lightly optimized code and get recompiled more aggressively as they become hot.

On iOS and Mac Catalyst, where Apple doesn’t allow JIT compilation, the runtime relies on precompiled code and an interpreter instead of generating native code at runtime.

Native AOT remains an opt-in publishing alternative. On Android, it’s still experimental, and conference session notes point out that p90 startup is still slow – not something to ship to production just yet.

Handlers changed too. Android Shell moves to a handler-based architecture, and Windows now uses the new CollectionView2 handler by default.

Microsoft hasn’t published official startup or app-size numbers for MAUI on CoreCLR, and results will depend heavily on your app. Before upgrading, build your own baseline: measure cold and warm start on physical devices, compare APK/AAB and IPA sizes against your .NET 10 build, and walk through navigation and custom handlers.

A Faster Inner Loop

The other half of the story is the developer experience. MAUI is moving closer to the standard .NET CLI, and the daily build-deploy-check cycle gets shorter:

dotnet run, test, and watch now work with Android and iOS projects, including device selection. dotnet watch also gains crash recovery, so a failed reload no longer ends your session.

XAML Incremental Hot Reload (preview) updates live pages and controls – including supported structural edits – without making you navigate away and back. It’s enabled by default in Debug builds.

Less XAML boilerplate. Implicit namespace declarations remove the standard headers you used to repeat at the top of every file.

Testing from the CLI. New mobile test templates and device selection in dotnet test bring device tests into the same command-line workflow as the rest of your tests.

For mixed teams, that last point matters most. When the mobile app and the API build, test, and run through the same commands, it becomes much easier to keep both in one CI pipeline.

AI Across the Stack

AI ran through almost every part of .NET Conf 2026, from the opening keynote to the Aspire sessions. For most teams, the real question isn’t whether .NET can do AI – it’s which pieces are ready for production and which are still experiments.

On the backend, the building blocks now form a clear stack. Microsoft.Extensions.AI provides abstractions like IChatClient and IEmbeddingGenerator that plug into dependency injection, caching, and OpenTelemetry. Microsoft Agent Framework builds on them with agents that call tools and keep conversation state. And the SDK now ships an MCP server template, so exposing your service’s capabilities to AI clients starts with a single command:

dotnet new mcpserver -o MyMcpServer

On the tooling side, agents are getting closer to your running code. .NET MAUI DevFlow lets an agent inspect a live app’s visual tree, take screenshots, and interact with the UI instead of reasoning from source alone. .NET Skills gives coding agents .NET-specific guidance, and the Aspire team presented its toolchain as agent-first, extending beyond .NET to TypeScript, Python, and Java.

Before you build a roadmap around any of this – or anything else in this article – check its status:

Status

What’s in it

Ready to use

Microsoft.Extensions.AI, MCP server template

Preview

Runtime Async, XAML Incremental Hot Reload, dotnetup

Experimental

Blazor AI components, AG-UI hosting, MAUI DevFlow, EF Core vector search, Native AOT on Android

Planning Your Upgrade

Now for the fine print we promised at the start. Upgrading to .NET 11 is straightforward on paper, but the right timing depends on what you ship – and servers and mobile apps don’t follow the same calendar.

.NET 10 or .NET 11 – and the MAUI Exception

.NET 11 is a Standard Term Support release with two years of updates, while .NET 10 is the current LTS. Here’s the twist: both reach end of support on November 14, 2028. For backend teams, that makes the choice purely about features – take .NET 11 for C# 15 and the new tooling, or stay on .NET 10 and upgrade every second year. (And if you’re still on .NET 8 or .NET 9, both went out of support on November 10, 2026 – the upgrade is no longer optional.)

Mobile is different. .NET MAUI has its own support policy: each major version is supported for at least six months after its successor ships. That puts the end of support for MAUI 10 at May 11, 2027 – a year and a half before the .NET 10 runtime underneath it. This isn’t new: MAUI 9 lost support in May 2026, six months before .NET 9 did. The takeaway is simple: for MAUI apps, LTS doesn’t mean three years. Plan for a yearly upgrade cycle, because the tooling behind your app – Xcode and the Android SDK – moves at that pace too.

What to Check Before You Migrate

Installing the new SDK doesn’t retarget anything on its own. Before you switch, walk through this list:

Breaking changes. Review the .NET 11 breaking changes, including updated minimum CPU requirements for your servers and build agents.

SDK pinning. Update global.json if your repository pins the SDK, and install the matching version on every developer machine and CI agent.

MAUI compatibility. The Microsoft.Maui.Controls.Compatibility package no longer ships, so apps that still reference it need to migrate first.

Data access. Check your database provider’s support for EF Core 11 before scheduling the runtime upgrade.

Supply chain. NuGet can now require a minimum publication age before new package versions become eligible, and container publishing produces reproducible image digests. Both are worth turning on during the upgrade.

Real workloads. Run your tests and measure under representative load, using your actual publishing mode, container images, and deployment hosts.

Conclusion

It’s tempting to judge a .NET release by its keynote demos. This year, a quieter lens is more useful: what changes for the systems you already run, and what that asks of your team.

For the server, the answer is reassuring. .NET 11 rewards a simple rebuild with faster, more transparent services, and ASP.NET Core and EF Core 11 smooth out problems teams have been working around for years. For mobile, the move to CoreCLR finally puts apps and backends on one runtime and one toolchain – a bigger shift than any single feature. AI is clearly becoming part of the platform itself, but its honest status labels are a reminder to build on what’s ready, not on what’s demoed.

The fine print ties it all together. A server and a mobile app built on the same .NET can still age on different schedules. Teams that treat upgrades as part of the product’s lifecycle, rather than a chore to postpone, will get the most out of this release – and the next one.

Let’s Make .NET 11 Work for You

Upgrading a backend and a mobile app at the same time is exactly where calendars, dependencies, and priorities start pulling in different directions. Our .NET teams work on both sides of that line every day – from tuning ASP.NET Core services to moving MAUI apps onto CoreCLR without surprises in production. If you want an upgrade plan that covers your whole stack, not just the runtime, let’s map it out together.
