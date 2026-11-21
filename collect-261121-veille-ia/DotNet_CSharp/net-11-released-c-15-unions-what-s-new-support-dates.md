---
id: collect-261121-veille-ia/dotnet-csharp/net-11-released-c-15-unions-what-s-new-support-dates
title: ".NET 11 Released: C# 15 Unions, What's New & Support Dates"
domain: dotnet-csharp
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-10"]
keywords: ["support", "product", "release", "november", "union", "default", "version", "released"]
source: https://tecadmin.net/dotnet-11-released-whats-new/
source_anchor: ""
source_lines: [1, 113]
section: "DotNet_CSharp"
---
Microsoft released **.NET 11** on 10 November 2026. The headline feature is **union types in C# 15**, and the release also brings faster command-line tooling, OpenAPI 3.2 by default in ASP.NET Core, CoreCLR as the default runtime for .NET MAUI, and an MCP server template in the SDK.

.NET 11 is a **Standard Term Support (STS)** release with two years of updates, until 14 November 2028. If you need the longest support window, .NET 10 remains the current long-term support (LTS) release. The same day, **.NET 8 and .NET 9 reached end of support**, so apps still on those versions should move to .NET 10 or .NET 11.

## .NET 11 at a Glance

| Release date | 10 November 2026 | 
|---|---|
| Support type | Standard Term Support (STS), 2 years | 
| Supported until | 14 November 2028 | 
| C# version | C# 15 | 
| F# version | F# 11 | 
| Target framework | net11.0 | 
| Current LTS release | .NET 10 (supported until 14 November 2028) | 
| Ended on release day | .NET 8 and .NET 9 | 

## What’s New in .NET 11

### C# 15: union types

A union holds exactly one value from a fixed set of existing types. The compiler converts each case type to the union automatically and checks that a **switch** expression handles every case, so you get a warning if you forget one. This is the example from Microsoft’s announcement:

```
```
```
ProductLookup result = new Product(42, "Mechanical keyboard");
string message = result switch
{
    Product product => $"Found {product.Name}",
    ProductNotFound missing => $"No product with ID {missing.Id}",
};
public sealed record Product(int Id, string Name);
public sealed record ProductNotFound(int Id);
public union ProductLookup(Product, ProductNotFound);
```
Unions also work with System.Text.Json, so ASP.NET Core Minimal APIs and MVC can accept and return them in JSON bodies. OpenAPI describes them with an **anyOf** schema.

### More C# 15 features

- **Closed hierarchies:** mark a class as**closed** to allow derived classes only in the same assembly. The compiler can then check that a switch covers every subclass.
- **Collection expression arguments:** pass constructor arguments inside a collection expression, for example**[with(StringComparer.OrdinalIgnoreCase), “Alice”, “Bob”]** to build a case-insensitive set.
- **Extension indexers** and**labeled break and continue** for nested loops.

C# 15 is the default language version for projects that target net11.0. The separate memory safety work is still in preview.

### Faster SDK and CLI

- The **dotnet** CLI host is now compiled with Native AOT, so commands like**dotnet –version** and help output start almost instantly.
- The **MSBuild server** stays running between builds by default, which speeds up repeated builds.
- SDK installers for Linux and macOS are smaller.
- File-based apps (single **.cs** files run with**dotnet run** ) can now span several files with**#:include** .
- **dotnet test** on Microsoft.Testing.Platform gets run-level timeouts, failure limits and clearer progress output.
- Container publishing produces reproducible image digests.
- A new **dotnetup** tool (public preview) installs and updates SDKs and runtimes per user.

### Runtime and libraries

- The JIT removes more redundant bounds checks and branches and devirtualizes more calls.
- **Runtime Async** , which moves async/await handling into the runtime for clearer call stacks in debuggers and profilers, is still a preview. You can opt in with**<Features>runtime-async=on</Features>** in your project file.
- New in the libraries: **Zstandard** compression in System.IO.Compression, a LINQ**FullJoin** operator, in-memory stream adapters such as**StringStream** , and JSON Lines support in System.Text.Json.

### ASP.NET Core 11 and Blazor

- **OpenAPI 3.2** is now the default document version.
- Asynchronous validation for Minimal APIs and Blazor forms.
- SignalR can refresh authentication without reconnecting.
- Blazor adds a **CacheView** component that caches rendered output during static server rendering, client-side validation for static forms, and an opt-in pause for inactive Interactive Server circuits.

### EF Core 11, .NET MAUI and AI

- **EF Core 11** generates fewer unnecessary joins, translates GroupBy better, and supports complex types and JSON columns with TPT and TPC inheritance. SQL Server vector search is experimental.
- **.NET MAUI** now uses**CoreCLR** as its default runtime on all platforms, and XAML hot reload is incremental. The old Microsoft.Maui.Controls.Compatibility package no longer ships.
- The SDK includes an **MCP server template** (**dotnet new mcpserver** ) built on the official MCP C# SDK, alongside Microsoft.Extensions.AI and the Microsoft Agent Framework.

## Support Timeline

.NET 11 is supported until 14 November 2028, the same date as .NET 10 LTS. In practice the choice between them comes down to features: pick .NET 11 if you want C# 15 and the new tooling, and stay on .NET 10 if you prefer to upgrade only every second year. See the full lifecycle on the .NET 11 release page and every version on the .NET release history.

## How to Install .NET 11

On Windows and macOS, download the SDK installer from the official .NET 11 download page. Visual Studio users need an up-to-date Visual Studio 2026.

On Linux, the quickest way is Microsoft’s install script, which installs the SDK into your home directory:

`wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh`
`chmod +x dotnet-install.sh`
`./dotnet-install.sh --channel 11.0`

Add the SDK to your PATH and check the version:

`export DOTNET_ROOT=$HOME/.dotnet`
`export PATH=$PATH:$DOTNET_ROOT:$DOTNET_ROOT/tools`
`dotnet --version`

Add the two **export** lines to your **~/.bashrc** to make them permanent. For step-by-step package installs, see our guides for Ubuntu 24.04 and Debian 12, and how to run multiple .NET versions side by side.

## How to Upgrade a Project to .NET 11

Installing the SDK does not change your existing projects. To move a project to .NET 11:

1. Change the target framework in your **.csproj** file to**net11.0** :```
<TargetFramework>net11.0</TargetFramework>
```
2. If a **global.json** file pins an older SDK, update or remove it.
3. Update Microsoft.* NuGet packages (ASP.NET Core, EF Core and so on) to their 11.0 versions.
4. Rebuild, run your tests, and review the .NET 11 breaking changes.
5. If you deploy with Docker, switch your base images to the 11.0 tags. Our multi-stage Dockerfile guide for .NET shows the pattern.

## Should You Upgrade?

Upgrade now if you are still on **.NET 8 or .NET 9**: both stopped receiving security updates on 10 November 2026. Moving to .NET 11 gets you the newest features, and moving to .NET 10 gets the same support end date with fewer changes. If you are already on .NET 10, there is no rush. Try .NET 11 on a branch to evaluate union types and the faster tooling, and upgrade when your dependencies support it.

How did it make you feel?
