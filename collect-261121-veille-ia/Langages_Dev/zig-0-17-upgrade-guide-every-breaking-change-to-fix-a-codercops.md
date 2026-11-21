---
id: collect-261121-veille-ia/langages-dev/zig-0-17-upgrade-guide-every-breaking-change-to-fix-a-codercops
title: "Zig 0.17 Upgrade Guide: Every Breaking Change to Fix â CODERCOPS"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["build", "before", "const", "changes", "every", "after", "system", "compiler"]
source: https://blog.codercops.com/blog/zig-0-17-upgrade-guide-breaking-changes-2026
source_anchor: ""
source_lines: [1, 141]
section: "Langages_Dev"
---
Sponsored

Upgrading to Zig 0.17.0 will break your build, and most of the breakage is fixable in an afternoon. The release has just landed with 925 commits from 206 contributors, a build system split into two processes, and a pile of removals that the compiler will point you to one error at a time. The one real blocker is tooling: ZLS does not work with 0.17.0 yet.

This guide covers the changes that will actually stop your build, in the order you will hit them. Everything below comes from the official 0.17.0 release notes. If you want background on where the language stands, our Zig 0.16 review covers why teams adopt it in the first place.

## Zig 0.17 upgrade order: build.zig first, source second

Fix `build.zig` before anything else. The build system now compiles your project-specific configurer separately from a general maker, so a script that fails to compile blocks every other error message you would otherwise see. Once the configurer builds, the compiler can start reporting problems in your actual source, and those come in volume.

A sane branch plan:

1. Install 0.17.0 next to your current compiler. Do not replace it.
2. Pin the version in CI before you touch code, so a teammateâs toolchain bump does not collide with yours.
3. Get `zig build` to reach the point where it compiles your sources.
4. Work through source errors file by file.
5. Turn on incremental compilation last, once the tree is green.

## Build system changes that break build.zig

The configurer/maker split is the biggest structural change. Your `build.zig` is compiled and run to produce a compact serialized configuration, and a maker process, built once after installation, consumes it. That is why rebuilds get faster even when `build.zig` changes. It is also why ZLS broke, which we get to below.

Several APIs were renamed or consolidated. The ones most likely to appear in your script:

```
// Before
if (b.args) |args| run_cmd.addArgs(args);
// After
run_cmd.addPassthruArgs();
```
```
// Before
const fmt_include_paths = &.{ "lib", "src", "test" };
// After
const fmt_include_paths = b.pathList(&.{ "lib", "src", "test" });
```
`b.build_root` became `b.root`, and its type changed from a directory to a path. The argument helpers for artifacts, outputs, file contents and directories were folded together, so `addArtifactArg` and `addPrefixedArtifactArg` now go through `addArtifactArg2`. If you wrap these in your own helper functions, search for every caller.

There is also a new idea worth understanding: configure-time cache poisoning. Calling `findProgram()` poisons the configuration cache, because the result depends on the machine. `findProgramLazy()` avoids that. New dependency-tracking functions (`dependOnFileContents`, `dependOnFileMetadata`, `dependOnDirectoryContents`, `dependOnDirectoryMetadata`) let you declare what your configuration actually read. If your build script probes the filesystem, use them, or you will be rebuilding more often than you should.

Package management also moved into the build system. `zig fetch`, `zig init` and `zig libc` now compile in `-Osafe` instead of `-Ofast`.

## Language changes that break your source

These are the removals and semantic changes the compiler will flag.

### Array repetition with ** is gone

```
// Before
var result = [1]u8{0} ** window_size;
// After
var result: [window_size]u8 = @splat(0);
```
### @bitCast changed meaning

`@bitCast` now reinterprets the logical bit representation of a value in an endian-agnostic way. Casts that involve `extern struct` or `extern union` are no longer permitted. The common case is reading an integer out of a byte buffer:

```
// Before (now an error)
const int: u16 = @bitCast(bytes);
// After
const int_ptr: *align(1) const u16 = @ptrCast(&bytes);
const int = int_ptr.*;
```
If you were bit-casting an extern struct to read a wire format, you now have a design decision. A pointer cast works. So does a packed struct, if the layout rules fit your protocol.

### Enum conversion builtins were renamed

`@intFromEnum` is now `@backingInt` and `@enumFromInt` is now `@fromBackingInt`:

```
// Before
return @enumFromInt(graph.generated_files.items.len - 1);
// After
return @fromBackingInt(@intCast(graph.generated_files.items.len - 1));
```
A project-wide find and replace handles most of it. Watch for the `@intCast` that now appears at the call site when the argument type does not match the backing integer.

### Smaller removals

`void{}` is gone, so write `{}`. The `i0` type is gone, so use `u0`. The capture form of `errdefer` (`errdefer |err| ...`) was removed, and the release notes suggest splitting the work into an inner function and handling the error at the call site with `catch |err|`. The `internal` and `link_once` linkage kinds were removed in favor of `weak`. And `@hasDecl` now returns `true` only for public declarations, so a check that used to find a private helper will start returning `false` without any compile error. That one is silent, so grep for `@hasDecl` explicitly.

## C interop: @cImport is removed

This will hurt anyone who wraps a C library. `@cImport` is gone, and `std.Build.Step.TranslateC` is deprecated. The replacement is the external translate-c package, which you pull in as a build dependency and invoke from `build.zig`. The result is a module you import like any other.

Plan for this early. It touches your build graph, not just a few source lines, and it is the change most likely to need a real afternoon rather than a find-and-replace.

## Standard library renames you will hit

The notes list dozens. These show up in most codebases:

| Old | New | 
|---|---|
| `std.fmt.allocPrint(arena, ...)` | `arena.print(...)` | 
| `ArrayList.getLastOrNull()` | `last()` | 
| `ArrayList.getLast()` | `last().?` | 
| `std.heap.stackFallback(size, gpa)` | `std.heap.StackFallbackAllocator` with`.init` and`.allocator()` | 
| `DebugAllocator` | `SafeAllocator` (thread-safe, with leak and corruption checks) | 
| `IntegerBitSet` ,`ArrayBitSet` | `Integer` ,`Array` | 
| `OptimizeMode` tags`Debug` ,`ReleaseSafe` | `debug` ,`safe` ,`fast` ,`small` | 

Two behavior changes deserve a second look. `@typeInfo` for structs now returns arrays of field names and types instead of an array of field structs, so every `inline for` over `.fields` needs rewriting. And `mem.eql` and `mem.findDiff` no longer short-circuit on float slices, because NaN is not equal to itself.

`std.zon.fromSlice` now takes a struct of arguments (`.gpa`, `.arena`, `.source`, `.diagnostics`) instead of positional parameters, and parsed values allocate from the arena you pass.

## ZLS does not work with 0.17.0 yet

Say this loudly to your team before anyone bumps a toolchain. The release notes state that the configurer/maker separation breaks ZLS, and that the Zig and ZLS teams are working on the Build Server Protocol to fix it. They call restoring ZLS a top roadmap priority, but there is no date.

If your developers rely on in-editor errors and completion, that is a real cost. Our recommendation: upgrade CI and a small group first, keep everyone else on 0.16, and move the whole team when ZLS catches up. If you are weighing whether a Zig-heavy team is sustainable, the same question comes up when hiring a Rust developer, because pre-1.0 churn and tooling gaps decide how fast a new person becomes useful.

## Incremental compilation, and the regressions list

The upside: most x86_64-linux projects can now run `zig build -fincremental --watch` for much faster rebuilds, thanks to ELF linker work. The release also adds `zig objdump`, `zig cache-cat` and `zig fmt --complexity`.

The notes also list known regressions, including a soft-float problem in compiler-rt on x86, a `std.debug.simple_panic` compile failure, unreliable overriding of weak Zig libc symbols, broken response-file use in `std.Build.Step.Run`, and SPIR-V backend problems. If you target any of those, read the issue tracker before you commit to the upgrade.

## What to do this week

Create the branch, pin 0.17.0 in CI, and fix `build.zig` first. Budget a half day for a small project and several days if you depend on `@cImport`. Leave ZLS-dependent developers on 0.16 until the Build Server Protocol work lands. Pre-1.0 means this will happen again, so the habit worth keeping is a pinned compiler and an upgrade branch for every release.

## Frequently asked questions

- Should I upgrade to Zig 0.17 today?
- If you ship from the command line and CI, yes, once you have budgeted a day or two for build.zig and std renames. If you depend on ZLS for editor diagnostics and completion, wait. The release notes say ZLS cannot work with 0.17.0 until the Zig and ZLS teams finish work on the Build Server Protocol.
- How do I enable incremental compilation in Zig 0.17?
- Run zig build with -fincremental and --watch. The release notes say most x86_64-linux projects can use it now because of ELF linker improvements. Other targets are not covered by that claim.
- What replaces @cImport in Zig 0.17?
- The release notes deprecate std.Build.Step.TranslateC and point to the external translate-c package, which you add as a build dependency and call from build.zig to turn C headers into a Zig module.
- Why did my @bitCast stop compiling?
- Zig 0.17 changed @bitCast to reinterpret the logical bit representation in an endian-agnostic way, and casts involving extern struct or extern union are no longer allowed. For byte buffers, use @ptrCast to an unaligned pointer and read through it.

Sources

Sponsored

Sponsored

Sponsored
