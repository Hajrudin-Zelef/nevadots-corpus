---
id: collect-261121-veille-ia/zig-lang/zig-programming-language-wikipedia
title: "Zig (programming language) - Wikipedia"
domain: zig-lang
role: reference
task: article
actors: []
dates: ["2016-02-08"]
keywords: ["language", "programming", "github", "designed", "platform", "generative", "andrew", "kelley"]
source: https://en.wikipedia.org/wiki/Zig_(programming_language)
source_anchor: ""
source_lines: [1, 232]
section: "Zig_Lang"
---
| Zig |  | 
|---|---|
| Paradigms | Multi-paradigm: imperative, concurrent, procedural, functional | 
| Designed by | Andrew Kelley | 
| Developer | Zig Software Foundation | 
| First appeared | 8 February 2016 <sup>[1]</sup> | 
| Stable release |  | 
| Typing discipline | Static, strong, inferred, nominal, generic | 
| Memory management | Manual | 
| Implementation language | Zig | 
| Platform | x86-64, ARM64, WebAssembly Tier 2: ARM, IA-32, RISC-V, MIPS64, POWERPC64, SPARC64, some tier-2 platforms have tier-1 support for *standalone programs* | 
| OS | Cross-platform: Linux, macOS, FreeBSD, Windows | 
| License | MIT <sup>[3]</sup> | 
| Filename extensions | .zig, .zir, .zigr, .zon | 
| Website | ziglang | 
| Influenced by |  | 
| C, C++, LLVM IR, Go, Rust <sup>[4]</sup><sup>[5]</sup><sup>[6]</sup><sup>[7]</sup><sup>[8]</sup> |  | 

**Zig** is a general-purpose system programming language that emphasizes performance, simplicity and metaprogramming.<sup>[9]</sup> The compiler is free and open-source software, released under an MIT License on Codeberg.<sup>[10]</sup>

Zig was designed to replace the C programming language while maintaining C ABI compatibility.<sup>[11]</sup><sup>[12]</sup> Unlike C, which supports metaprogramming through preprocessor macros, Zig supports metaprogramming through compile-time code execution.<sup>[13]</sup> At compile time types are first class and can be passed to functions, this system is used for generics.<sup>[13]</sup><sup>[14]</sup> A core goal of the language is to have no hidden memory allocations or control flow<sup>[9]</sup>.  Low-level programming features include packed structs, arbitrary-width integers<sup>[15]</sup> and multiple pointer types.<sup>[13]</sup>

Zig was designed by Andrew Kelley and first announced in 2016.<sup>[1]</sup> Development is funded by the Zig Software Foundation (ZSF), which receives corporate sponsorships as well as private donations.<sup>[16]</sup><sup>[17]</sup>

Andrew Kelley began making a digital audio workstation but was unsatisfied with Go, Rust and C++. He decided to make a new language with the advantages of C while improving on its weaknesses.<sup>[18]</sup> The name *Zig* was chosen using a script that generated random combinations of letters starting with the letter *z*.<sup>[19]</sup>

The previous bootstrapping compiler was written in Zig and C++ and it used LLVM as a back-end.<sup>[20]</sup> It was replaced with a fully Zig version in the 0.11.0 release. Zig now uses a prebuilt WebAssembly binary to bootstrap itself.<sup>[21]</sup><sup>[22]</sup>

On 26 November 2025, Zig development migrated from GitHub to Codeberg, citing GitHub's declining reliability under Microsoft ownership, particularly due to poor use of Generative AI on the platform, as a primary reason for this change.<sup>[23]</sup><sup>[24]</sup> One cited example of the platform's declining reliability was the apparent unreliability and delays towards the fixing of bugs in GitHub's CI platform, GitHub Actions.<sup>[23]</sup><sup>[24]</sup> In addition, the Zig development team had concerns with the Generative AI direction of GitHub, which was in contrast to the Zig project's policy against Generative AI.<sup>[25]</sup> In 2026, Codeberg would introduce a policy against Generative AI<sup>[26]</sup><sup>[27]</sup>, similar to the policy of the Zig project.

Below is a "Hello, World!" program in Zig.

```
const std = @import("std");
pub fn main() void {
    std.debug.print("Hello, World!\n", .{});
}
```
The primary goal of Zig is to be a better solution to the sorts of tasks that are currently solved with C. A primary concern in that respect is readability; Zig attempts to use existing concepts and syntax wherever possible, avoiding the addition of different syntax for similar concepts. Further, its goal is to be a language designed for "robustness, optimality and maintainability". The small and simple syntax is an important part of that maintainability, as one of the language's goals is to allow maintainers to debug code written in Zig without having to learn the intricacies of a language they might not be familiar with.<sup>[28]</sup> Even with these changes, Zig can compile into and against existing C code; C headers can be included in a Zig project and their functions called, and Zig code can be linked into C projects by including the compiler-built headers.<sup>[29]</sup>

Error handling is handled through error types and can be handled with `catch` or `try`. Generics are achieved through compile time code generation and accommodating a form of duck typing with the `comptime` directive.<sup>[30]</sup>

Unlike C, Zig uses labeled `break` and `continue` instead of goto statements. These act similarly to `goto`, but can only jump to the top of code blocks and prevent jumping past variable initialization.<sup>[*citation needed*]</sup>

Memory allocation in the Zig standard library follows the convention that allocation is handled through `struct`s describing the action, as opposed to calling the memory management functions in libc. For instance, in C if one wants to write a function that makes a string containing multiple copies of another string, the function might look like this:

```
const char* repeat(const char* original, size_t times);
```
In the code, the function would examine the size of `original` and then malloc `times` that length to set aside memory for the string it will build. That malloc is invisible to the functions calling it; if they fail to later release the memory, a leak will occur. In Zig, this might be handled using a function like:

```
const Allocator = std.mem.Allocator;
fn repeat(allocator: Allocator, original: []const u8, times: usize) Allocator.Error![]const u8;
```
In this code, the `allocator` variable is passed a struct that describes what code should perform the allocation, and the `repeat` function returns either the resulting string or, using the error union type as indicated by the `!`, an Allocator.Error. By directly expressing the allocator as an input, memory allocation is never "hidden" within another function, it is always exposed to the API by the function that is ultimately calling for the memory to be allocated. No allocations are performed inside Zig’s standard library. Further, as the struct can point to anything, one can use alternative allocators, even ones written in the program. This can allow, for instance, small-object allocators that do not use the operating system functions that normally allocate an entire memory page.<sup>[31]</sup>

The optional type is also an example of a language feature that offers general functionality while still being simple and generic. They do not have to be used to solve null pointer problems; they are also useful for any type of value where "no value" is an appropriate answer. Consider a function `countTheNumberOfUsers` that returns an integer, and an integer variable, `theCountedUsers` that holds the result. In many languages, a magic number would be placed in `theCountedUsers` to indicate that `countTheNumberOfUsers` has not yet been called, while many implementations would just set it to zero. In Zig, this could be implemented as an `var theCountedUsers: ?i32 = null` which sets the variable to a clear "not been called" value.<sup>[31]</sup>

Another more general feature of Zig that also helps manage memory problems is the concept of `defer`, which marks some code to be performed at the end of a scope no matter what happens, including possible runtime errors. If a particular function allocates some memory and then disposes of it when the operation is complete, one can add a line to defer a `free` to ensure it is released no matter what happens.<sup>[31]</sup>

The Zig standard library avoids hidden allocations. Allocation is not managed in the language directly. Instead, heap access is done via the standard library, explicitly.<sup>[32]</sup>

Zig promotes a gradual approach to portability by providing direct interoperability of Zig code with C code, allowing code from one language to call code from the other. Since 0.16.0, this is implemented through Zig's build system, by enabling C libraries to be defined within the `build.zig` file of the Zig project, to enable for the Zig code to import the C libraries through the use of the `@import` keyword.<sup>[33]</sup>

For inclusion of C code in a primarily Zig project, this is typically implemented in the `build.zig` file, in a similar fashion to the following example (note that this example is only of the relevant code):

```
const translate_c = b.addTranslateC(.{
    .root_source_file = b.path("src/cheaders.h"),
    .target = target,
    .optimize = optimize,
});
const exe = b.addExecutable(.{
    .name = "example",
    .root_module = b.createModule(.{
        .root_source_file = b.path("src/main.zig"),
        .optimize = optimize,
        .target = target,
        .imports = &.{
            .{
                .name = "c",
                .module = translate_c.createModule(),
            },
        },
    }),
});
```
The C headers can then be called in Zig code with the following code:

```
const c = @import("c");
c.exampleFunction();
```
This is in line with how Zig imports its own libraries, similarly with the `@import` directive, typically in this fashion:

```
const std = @import("std");
```
For the alternate method of utilising and calling Zig code inside a primarily C program, this is more difficult than the aforementioned, as the Zig build system focusses on a Zig-based environment. However, it is a similar setup, as the Zig build system is still utilised for such a purpose in most scenarios. As C code naturally uses C header files (`.h`) and macros, all Zig declarations, such as functions, would need to be expressed either within header files or `extern` declarations, as would be done for C interoperability with programming languages other than Zig. The following code is an example for a C project which uses Zig code, where the shared declarations between C and Zig code are contained within an `extern.h` file:

```
    const sharedheader = b.addTranslateC(.{
        .root_source_file = b.path("./src/extern.h"),
        .target = target,
        .optimize = optimize,
        .link_libc = true,
    });
    const zig_code = b.addLibrary(.{
        .name = "zigcodelib",
        .linkage = .static,
        .root_module = b.createModule(.{
            .root_source_file = b.path("src/zigcode.zig"),
            .target = target,
            .optimize = optimize,
            .link_libc = true,
            .imports = &.{
                .{ .name = "flags.h", .module = sharedheader.createModule() },
            },
        }),
    });
    const main_c_module = b.addModule("main", .{
        .target = target,
        .optimize = mode,
        .link_libc = true,
    });
    main_c_module.addCSourceFile(.{
        .file = b.path("src/main.c"),
        .flags = &.{ "-Wall", "-pedantic" },
    });
    const exe = b.addExecutable(.{
        .name = "example",
        .root_module = main_c_module,
    });
    exe.root_module.linkLibrary(zig_code);
    b.installArtifact(zig_code);
    b.installArtifact(exe);
```
Zig 0.16.0 deprecated `@cImport`, going forward C Translation will be handled via the Build System instead of using the `@cImport` language builtin, which has been 'deleted' from the language. Additionally the release deprecated `std.Build.Step.TranslateC`  in favour using an explicit package dependency of the official translate-c package, which is the same implementation as the previous build step version with additional configuration options.<sup>[33]</sup>

Prior to 0.16.0, the system used for C interoperability did not use the Zig build system, and instead used a `@cImport` directive only used for calling C code. This directive would often wrap an additional `@cInclude` directive, which mirrored the `#include` macro that would be seen within C code. In these prior versions of Zig, C libraries are imported in the following manner:

```
const c = @cImport(@cInclude("soundio/soundio.h"));
```
This Zig code would be functionally equivalent to the following C code with the main difference being that the library is accessed through calls to the constant identifier `c`, as seen in the current 0.16.0 example.

```
#include <soundio/soundio.h>
```
The Zig code would then be able to call functions from soundio library in the same manner as calling functions from other Zig code, As Zig uses new data types that are explicitly defined, unlike C’s more generic `int` and `float`, a small number of directives are used to move data between the C and Zig types, including `@intCast` and `@ptrCast`.<sup>[31]</sup> This is notably different from the current implementations, as Zig code would directly be able to import code from system header files, whereas the current implementation only allows for Zig code to import C code from local libraries defined within the users code.

Zig can evaluate sections of code at compile time instead of runtime using the `comptime` keyword. Running code at compile time provides functionality similar to macros and conditional compilation, without the use of a separate preprocessor language.<sup>[34]</sup>

Types become first-class citizens at compile time, allowing compile-time duck typing,<sup>[35]</sup> which Zig uses to implement generic types. The example below declares a generic linked list:

```
fn LinkedList(comptime T: type) type;
```
The function takes the type `T` and produces a concrete linked-list type based on `T`.

Starting with version 0.16.0, all standard library input/output functionality in Zig requires passing an explicit `Io` instance, rather than relying on globally available I/O functions.<sup>[36]</sup> The change addresses the "what color is your function" problem common to async programming, in which functions written to support asynchronous execution are typically incompatible with synchronous callers and vice versa. Zig's approach makes an `Io` implementation an explicit parameter rather than an implicit property of a function, allowing the same code to run under different execution strategies—including a thread-based implementation (`Io.Threaded`) and an experimental, work-in-progress stackful coroutine implementation (`Io.Evented`)—without requiring separate sets of functions for synchronous and asynchronous use.<sup>[36]</sup>

The `Io` interface also provides task-level concurrency primitives, including `Future` (for launching and awaiting individual asynchronous tasks) and `Group` (for managing many tasks that share a lifetime), along with a unified cancellation mechanism that allows in-flight operations, such as networking or file system calls, to be interrupted.<sup>[36]</sup>

Zig also includes a C and C++ compiler, and can be used with either or both languages<sup>[37]</sup> The compiler provides many headers including the C standard library (libc) and C++ Standard Library (libcxx) for many different platforms. This allows Zig’s `cc` and `c++` sub-commands to act as cross compilers similarly to Clang.<sup>[38]</sup><sup>[39]</sup>

Zig treats cross-compiling as a first-class use-case of the language.<sup>[40]</sup> This means any Zig compiler can compile runnable binaries for any of its target platforms. The toolchain can compile to any of these targets without installing additional software.<sup>[31]</sup>

Cross-compilation is also available for variety of the operating systems. Popular UNIX-like ones and Windows are officially supported.<sup>[41]</sup> Minimal applications can and have been made for Android (with Android NDK) or iOS.<sup>[*citation needed*]</sup>

The LLVM backend is the default for most targets, except for SPIR-V, and debug-mode x86-64.<sup>[42]</sup> Zig also supports a self-hosted backend which can be enabled by using `-fno-llvm`.<sup>[43]</sup>

Version 0.11.0 bundles an experimental package manager, but no official package repository is available. Instead a package is simply a URL that points to a compressed file, or a Git repository. Each package ideally includes a standard `build.zig` file (that the Zig compiler uses by convention to compile the source code) and a `build.zig.zon` file containing metadata with name and version of the package.<sup>[44]</sup>

Projects that use (or have previously used) Zig include:

- Bun, a JavaScript and TypeScript runtime originally written in Zig (until v1.3.14) and rewritten in Rust (starting with v1.4.0) in 2026, using Safari's JavaScriptCore virtual machine.<sup>[45]</sup>
- TigerBeetle, a financial transaction database<sup>[46]</sup><sup>[47]</sup>
- Ghostty, a GPU accelerated terminal emulator<sup>[48]</sup>

1. 1 2 Kelley, Andrew (8 February 2016). "Introduction to the Zig Programming Language". *andrewkelley.me* . Retrieved 8 November 2020.
2. ↑ "0.17.1 Released". 3 November 2026. Retrieved 18 November 2026.
3. ↑ "LICENSE". Retrieved 6 October 2026.
4. ↑ "Why Zig when There is Already C++, D, and Rust? ⚡ Zig Programming Language".
5. ↑ "No surprises on any system: Q&A with Loris Cro of Zig - Stack Overflow". 2 October 2023.
6. ↑ "What's Zig got that C, Rust and Go don't have? (With Loris Cro)". *YouTube* . 15 November 2023.
7. ↑ "Overview ⚡ Zig Programming Language".
8. ↑ "After a day of programming in Zig". 29 December 2023. Archived from the original on 11 February 2025. Retrieved 31 December 2024.
9. 1 2 "Home ⚡ Zig Programming Language". *ziglang.org* . Retrieved 5 October 2026.
10. ↑ "ziglang/zig". *Codeberg* . Retrieved 11 February 2020.
11. ↑ "Introduction to the Zig Programming Language - Andrew Kelley". *andrewkelley.me* . Retrieved 5 October 2026.
12. ↑ "New challenger joins Rust to topple C language". *InfoWorld* . Retrieved 7 October 2026.
13. 1 2 3 "Documentation - The Zig Programming Language". *ziglang.org* . Retrieved 6 October 2026.
14. ↑ Edge, Jake (6 October 2020). "Zig heading toward a self-hosting compiler". *LWN.net* . Retrieved 6 October 2026.
15. ↑ Anderson, Tim (24 April 2020). "Keen to go _ExtInt? LLVM Clang compiler adds support for custom width integers". *The Register* . Retrieved 30 December 2024.
16. ↑ "Announcing the Zig Software Foundation". *Ziglang.org* . Retrieved 28 May 2021.
17. ↑ "Pledging Another $400,000 to the Zig Software Foundation". *Mitchell Hashimoto* . 21 June 2026. Retrieved 7 July 2026.
18. ↑ Anderson, Tim (28 May 2026). "Zig creator seeks 'uncompromising perfection' before blessing 1.0". *theregister* . Retrieved 7 October 2026.
19. ↑ andrewrk (13 March 2024). "origin of the zig programming language name. by @andrewrk". Retrieved 13 March 2024.
20. ↑ "A Reply to _The Road to Zig 1.0_". *www.gingerbill.org* . 13 May 2019. Retrieved 11 February 2020.
21. ↑ "0.11.0 Release Notes ⚡ The Zig Programming Language". *ziglang.org* . Retrieved 7 October 2026.
22. ↑ "Goodbye to the C++ Implementation of Zig ⚡ Zig Programming Language". *ziglang.org* . Retrieved 7 October 2026.
23. 1 2 Kelley, Andrew (26 November 2025). "Migrating from GitHub to Codeberg". *ziglang.org* . Archived from the original on 27 November 2025. Retrieved 27 November 2025.
24. 1 2 Claburn, Thomas (2 December 2025). "Zig quits GitHub, gripes about Microsoft's AI obsession". *theregister* . Retrieved 8 August 2026.
25. ↑ Dees, Mels (2 December 2025). "Zig project leaves GitHub due to excessive AI". *Techzine Global* . Retrieved 8 August 2026.
26. ↑ Claburn, Thomas (23 July 2026). "Codeberg gives vibe-coded projects the toss, promotes human FLOSS". *theregister* . Retrieved 8 August 2026.
27. ↑ "Codeberg Has Drawn a Hard Line on Use of AI With Community Backing". *It's FOSS* . 24 July 2026. Retrieved 8 August 2026.
28. ↑ Elizabeth 2017.
29. ↑ Yegulalp 2016.
30. ↑ "Documentation – The Zig Programming Language (0.15.1): Compile-Time Parameters". *ziglang.org* . Retrieved 4 November 2026.
31. 1 2 3 4 5 "Allocators". 11 September 2023.
32. ↑ Tyson, Matthew (9 March 2023). "Meet Zig: The modern alternative to C". *InfoWorld.com* .
33. 1 2 "Zig 0.16.0 Release Notes". *Zig Release Notes* . 14 April 2026. Retrieved 24 April 2026.
34. ↑ *The Road to Zig 1.0 - Andrew Kelley* . ChariotSolutions. 9 May 2019 – via YouTube.
35. ↑ "Zig Language Reference: Compile-Time Parameters".
36. 1 2 3 "0.16.0 Release Notes". *Zig Release Notes* . 14 April 2026. Retrieved 21 August 2026.
37. ↑ "0.6.0 Release Notes". *Ziglang.org* . Retrieved 19 April 2020.
38. ↑ ". *andrewkelley.me* . Retrieved 28 May 2021.
39. ↑ "Zig Makes Go Cross Compilation Just Work". *DEV Community* . 24 January 2021. Retrieved 28 May 2021.
40. ↑ "The Zig Programming Language". *Ziglang.org* . Retrieved 11 February 2020.
41. ↑ "Platform Support ⚡ Zig Programming Language". *ziglang.org* . Retrieved 7 October 2026.
42. ↑ "0.15.1 Release Notes ⚡ The Zig Programming Language". *ziglang.org* . Retrieved 3 September 2025.
43. ↑ "0.14.0 Release Notes ⚡ The Zig Programming Language". *ziglang.org* . Retrieved 4 November 2026.
44. ↑ "Zig Build (zig.guide)".
45. ↑ Sumner, Jarred (8 July 2026). "Rewriting Bun in Rust". *Bun Blog* .
46. ↑ Greef, Joran Dirk (4 August 2023). "A New Era for Database Design with TigerBeetle". *InfoQ* . Retrieved 28 January 2026.
47. ↑ *tigerbeetle/tigerbeetle* , TigerBeetle, 28 January 2026, retrieved 28 January 2026
48. ↑ Proven, Liam (8 January 2025). "Just when you thought terminal emulators couldn't get any better, Ghostty ships". *The Register* .

- Elizabeth, Jane (19 October 2017). "Tired of C? New programming language Zig aims to be more pragmatic and readable". *jaxenter* . Archived from the original on 1 October 2020. Retrieved 22 April 2020.
- Yegulalp, Serdar (29 August 2016). "New challenger joins Rust to topple C language". *InfoWorld* . Retrieved 11 February 2020.
