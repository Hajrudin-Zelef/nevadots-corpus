---
id: collect-261121-veille-ia/dotnet-csharp/net-11-lands-as-net-8-and-9-hit-end-of-support-and-drops-cpus-without-sse4-2
title: ".NET 11 lands as .NET 8 and 9 hit end of support, and drops CPUs without SSE4.2"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-10"]
keywords: ["support", "dotnet", "net", "more", "default", "async", "drops", "cpus"]
source: https://blog.jorgeturbica.dev/en/net-11-lands-as-net-8-and-9-hit-end-of-support-and-drops-cpus-without-sse4-2
source_anchor: ""
source_lines: [1, 85]
section: "DotNet_CSharp"
---
.NET 11 shipped on 10 November 2026, the same day .NET 8 and .NET 9 went out of support. If you run services on either, the calendar no longer leaves you any slack: you either move to .NET 10 (LTS) or to .NET 11, an STS release with two years of support that also refuses to start on x86 processors older than x86-64-v2.

The release brings C# 15, runtime-managed async (in preview), a smaller SDK with a faster CLI, and a handful of behavioural changes that a web service will notice more than any of the headline features.

## What a production service gets

**Runtime-native async.** This is the biggest change under the hood. Instead of the compiler emitting a state machine for every `async` method, the runtime itself handles suspension and resumption. It is still in preview and you turn it on per project with `<Features>runtime-async=on</Features>`, no longer needing `EnablePreviewFeatures`. The interesting part is that the runtime libraries already ship compiled this way, so Microsoft is validating it at scale. The first thing you will notice is live stack traces: in the documentation's example, a chain of three `async` methods drops from 13 stack frames to 5, with no `AsyncMethodBuilderCore` noise in between. For a profiler, or a dump taken mid-incident, that is diagnosis time saved.

**A JIT that does less pointless work.** It removes more bounds checks (`i + cns < len`, `values[^1]`, the first element after an empty-`Span` guard), devirtualizes generic virtual methods, and folds constant string comparisons after inlining. In ReadyToRun images, `Comparer<T>.Default` and `EqualityComparer<T>.Default` are now specialized, and Microsoft measures up to 20x better throughput in collections that rely on the default comparer.

**Operational fixes you will appreciate.** The runtime now starts on machines with more than 1,024 logical CPUs (it used to fail during initialization), reads cgroup v2 memory limits correctly at the root of the hierarchy, and `Guid.NewGuid()` uses `getrandom()` on Linux for about 12% more throughput. In-process crash reporting also reaches Linux and macOS: with `DOTNET_EnableCrashReport=1` the process writes its managed stack and module list before it dies.

**SDK and CLI.** The Native AOT-compiled CLI host is on by default, and tools such as `dotnet ef` skip the 600–700 ms startup of the managed CLI. The MSBuild server now stays warm between invocations. On Linux x64 the SDK tarball shrinks from 230 to 189 MB, and container images by 8–17%. Two additions help with supply chain hygiene: reproducible container images with `SOURCE_DATE_EPOCH`, and a minimum publication age for NuGet packages.

```
# Same inputs, same image digest
dotnet publish /t:PublishContainer \
  -p:ContainerRegistry=registry.example.com \
  -p:SOURCE_DATE_EPOCH="$(git log -1 --pretty=%ct)"
# Ignore package versions published less than 24 hours ago
dotnet nuget add source https://api.nuget.org/v3/index.json --name public \
  --min-publish-age-hours 24 --configfile nuget.config
```
The minimum age is applied by `dotnet package add` and `dotnet package update` when they pick a version; a normal restore only enforces it for floating references, where it fails with NU1020.

On the language side, C# 15 adds union types, closed hierarchies, collection expression arguments and labelled `break`/`continue`. The libraries gain Zstandard in `System.IO.Compression`, LINQ `FullJoin` and union support in `System.Text.Json`. ASP.NET Core now generates OpenAPI 3.2 by default.

## What can break

The riskiest change is not in your code but in your hardware. The x86/x64 baseline moves from x86-64-v1 to **x86-64-v2** on every OS (SSE4.2, POPCNT and CX16 are now required), and Windows on Arm64 now requires LSE. On a CPU that falls short, the app will not start and prints `The current CPU is missing one or more of the baseline instruction sets`. Microsoft points out that the last chips without these instructions dropped out of Intel and AMD support around 2013, but old virtualization setups or emulated CPU models can still trip over it.

For a service built on ASP.NET Core or the generic host, these are the behavioural changes I would check first:

- **`IHost.RunAsync` and `IHost.StopAsync` now throw when a `BackgroundService` fails.** If your process used to stay up with a dead worker, whatever launches it will now find out.
- **An empty string in a later configuration provider overrides earlier ones.** An empty environment variable can wipe out a value from`appsettings.json` .
- **Named mutexes default to user scope on Unix** , which changes coordination between processes running as different users.
- **Server-side `SslStream` no longer downloads intermediate certificates via AIA** by default. If your TLS server does not send the full chain, expect validation errors.
- **High-cardinality `HttpClient` metrics move to observable counters** : check your dashboards if you consume them through OpenTelemetry.
- **ZIP reads validate CRC32 and TAR reads validate header checksums** : corrupt archives that used to slip through now fail.

If you used the `DOTNET_RuntimeAsync` or `UNSUPPORTED_RuntimeAsync` environment variables to try runtime async, they are gone; to opt a project out, use `<UseRuntimeAsync>false</UseRuntimeAsync>`.

## How to upgrade to .NET 11

The SDK is 11.0.100 and the runtime 11.0.0. With the official install script:

```
curl -sSL https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
chmod +x dotnet-install.sh
./dotnet-install.sh --channel 11.0
```
For containers, the images are already on Microsoft's registry with tags such as `11.0`, `11.0-alpine` and `11.0-azurelinux3.0`:

```
FROM mcr.microsoft.com/dotnet/sdk:11.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app
FROM mcr.microsoft.com/dotnet/aspnet:11.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "MyService.dll"]
```
In the project, the minimum change is `<TargetFramework>net11.0</TargetFramework>` and, if you pin the SDK in `global.json`, version `11.0.100`. On Windows you need an up-to-date Visual Studio 2026, or VS Code with C# Dev Kit.

## .NET 10 LTS or .NET 11 STS

For anyone rushing off .NET 8 or 9, this is the real decision. .NET 10 is LTS and still supported; .NET 11 gets two years. If your team upgrades rarely, .NET 10 is the safe bet. If you were going to work through the behavioural changes above anyway and want the new JIT, the lighter SDK and reproducible images, .NET 11 is worth it, knowing that another jump will be due in two years.

## Checklist before production

1. Confirm your nodes (and the CPU model your hypervisor exposes) meet x86-64-v2, or LSE on Windows Arm64.
2. Build against `net11.0` and go through the new obsoletion warnings (`SYSLIB` ).
3. Review configuration: empty values in environment variables and chained providers.
4. Make a `BackgroundService` fail on purpose in staging and watch how the process exits.
5. If you terminate TLS with `SslStream` or Kestrel, make sure the full certificate chain is sent.
6. Compare your `HttpClient` metrics and dashboards before and after.
7. Measure latency and memory under real load before enabling `runtime-async=on` , starting with a non-critical service.

## Sources

- Announcing .NET 11, .NET Blog, 10 November 2026.
- What's new in .NET 11 runtime, Microsoft Learn.
- What's new in the SDK and tooling for .NET 11, Microsoft Learn.
- Breaking changes in .NET 11, Microsoft Learn.
