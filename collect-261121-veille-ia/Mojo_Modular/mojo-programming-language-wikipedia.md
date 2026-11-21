---
id: collect-261121-veille-ia/mojo-modular/mojo-programming-language-wikipedia
title: "Mojo (programming language) - Wikipedia"
domain: mojo-modular
role: reference
task: article
actors: []
dates: ["2026-06-12"]
keywords: ["mojo", "language", "modular", "programming", "compiler", "python", "llvm", "mlir"]
source: https://en.wikipedia.org/wiki/Mojo_(programming_language)
source_anchor: ""
source_lines: [1, 124]
section: "Mojo_Modular"
---
| Mojo |  | 
|---|---|
| Paradigms |  | 
| Family | Python | 
| Designed by | Chris Lattner <sup>[1]</sup> | 
| Developer | Modular Inc. | 
| First appeared | 2023 | 
| Stable release |  | 
| Typing discipline |  | 
| OS | Cross-platform: Linux, macOS | 
| License | Apache 2.0 with LLVM exceptions (language, <sup>[3]</sup><sup>[4]</sup><sup>[5]</sup> standard library<sup>[6]</sup> ) | 
| Filename extensions | .mojo alternatively .🔥 (the fire emoji/U+1F525 Unicode character) | 
| Website | mojolang | 
| Influenced by |  | 
| Python, Cython, C, C++, Rust, Swift, Zig, CUDA, MLIR <sup>[7]</sup><sup>[8]</sup><sup>[9]</sup> |  | 

**Mojo** is a programming language for Linux and macOS.<sup>[10]</sup><sup>[11]</sup> It is a system programming language with semantics inspired by Rust such as static typing and a borrow checker,<sup>[12]</sup><sup>[13]</sup> but with syntax based on Python (like Nim, Julia).<sup>[14]</sup><sup>[15]</sup> It was originally intended to be a superset of Python, but this goal has been either abandoned or postponed indefinitely as of March 2026.<sup>[16]</sup> In May 2026, Modular launched a website for the language<sup>[17]</sup> and in August 2026 Modular released Mojo 1.0,<sup>[18]</sup> declaring it free and open-source software.<sup>[3]</sup><sup>[5]</sup>

Mojo builds on the Multi-Level Intermediate Representation (MLIR) compiler software framework, instead of directly on the lower level LLVM compiler framework used by languages like Julia, Swift, C++, and Rust.<sup>[15]</sup><sup>[19]</sup> MLIR is a newer compiler framework that allows Mojo to exploit higher level compiler passes unavailable in LLVM alone, and allows Mojo to compile down and target more than only central processing units (CPUs), including producing code that can run on graphics processing units (GPUs), Tensor Processing Units (TPUs), application-specific integrated circuits (ASICs) and other accelerators. It can also often more effectively use certain types of CPU optimizations directly, like single instruction, multiple data (SIMD) with minor intervention by a developer, as occurs in many other languages.<sup>[20]</sup><sup>[21]</sup> According to Jeremy Howard of fast.ai, Mojo can be seen as "syntax sugar for MLIR" and for that reason Mojo is well optimized for applications like artificial intelligence (AI).<sup>[8]</sup>

The Mojo language was created by Modular Inc, which was founded by Chris Lattner, the original architect of the Swift programming language and LLVM, and Tim Davis, a former Google employee.<sup>[22]</sup> The intention behind Mojo is to bridge the gap between Python’s ease of use and the fast performance required for cutting-edge AI applications.<sup>[23]</sup>

According to public change logs, Mojo development goes back to 2022.<sup>[24]</sup> In May 2023, the first publicly testable version was made available online via a hosted playground.<sup>[25]</sup> By September 2023 Mojo was available for local download for Linux<sup>[26]</sup> and by October 2023 it was also made available for download on Apple's macOS.<sup>[27]</sup> The SDK included a command-line driver, a Visual Studio Code extension, and a Jupyter kernel.<sup>[11]</sup>

In March 2024, Modular open sourced the Mojo standard library and started accepting community contributions under the Apache 2.0 license.<sup>[28]</sup><sup>[29]</sup>

In December 2025, Modular published a roadmap toward a 1.0 release, stating that Mojo 1.0 would stabilize core language features and that additional features, such as a match statement and enums, would follow in 1.x releases.<sup>[30]</sup> On 7 May 2026, Modular released Mojo 1.0.0 beta 1 and launched the language's website, mojolang.org.<sup>[17]</sup><sup>[31]</sup> On 11 August 2026, Modular released Mojo 1.0.0,<sup>[18]</sup><sup>[32]</sup> and one week later on 18 August 2026, Modular open-sourced the Mojo compiler under the Apache 2.0 license.<sup>[3]</sup><sup>[5]</sup>

Mojo was created for an easy transition from Python. The language has syntax similar to Python's, with inferred static typing,<sup>[33]</sup> and allows users to import Python modules.<sup>[34]</sup><sup>[35]</sup> It uses LLVM and MLIR as its compilation backend.<sup>[15]</sup><sup>[36]</sup><sup>[37]</sup> Mojo programs can import and call Python modules via the CPython runtime, and Mojo functions can be called from Python through C-compatible bindings.<sup>[17]</sup> The language is not source-compatible with Python 3. It does not support Python's class system; instead it provides struct types, memory-optimized alternatives to classes that support methods, fields, operator overloading, and decorators.<sup>[38]</sup><sup>[17]</sup> Python-style list, set, and dictionary comprehensions are supported.<sup>[39]</sup>

The language also provides a borrow checker, an influence from Rust.<sup>[40]</sup> Function arguments are passed as immutable references by default (the read convention). Functions may instead declare arguments as mutable references (mut) or take ownership of them, and ownership can be transferred with the ^ operator.<sup>[17]</sup><sup>[41]</sup>

Both the Mojo language compiler and the Mojo standard library are open source under the Apache 2.0 license with LLVM Exceptions.<sup>[3]</sup><sup>[5]</sup>

Mojo supports GPU programming through a gpu package in its standard library.<sup>[17]</sup> A 2025 study by Oak Ridge National Laboratory researchers presented at the SC25 WACCPD workshop found Mojo's GPU kernels broadly competitive with CUDA and HIP for memory-bound workloads on Nvidia and AMD accelerators, while noting performance gaps for atomic operations and fast-math compute-bound workloads on AMD GPUs.<sup>[42]</sup>

Functions in Mojo are declared with def. Earlier versions also provided an fn keyword for stricter, statically typed functions; fn was deprecated in 2026 in favor of using def for all functions.<sup>[43]</sup><sup>[44]</sup> Functions may declare typed parameters and return types:

```
def add(x: Int, y: Int) -> Int:
    """A typed addition."""
    return x + y
```
Variables may be declared explicitly with var, which gives block-level scope, or implicitly by assignment, which gives function-level scope matching Python's behavior:<sup>[45]</sup>

```
def main():
    var total = add(1, 2)  # explicitly declared
    doubled = total * 2    # implicitly declared
    print(doubled)
```
Mojo originally included a let keyword for immutable bindings;<sup>[38]</sup> it was removed from the language in 2024.<sup>[46]</sup>

Structs are Mojo's compile-time-laid-out alternative to Python classes. The @fieldwise_init decorator synthesizes a constructor from the declared fields, and traits such as Copyable declare supported behaviors:<sup>[17]</sup>

```
@fieldwise_init
struct Point(Copyable, Movable):
    var x: Int
    var y: Int
    def magnitude_squared(self) -> Int:
        return self.x * self.x + self.y * self.y
def main():
    p = Point(3, 4)
    print(p.magnitude_squared())  # 25
```
Mojo programs can import and use Python modules through the CPython runtime:<sup>[35]</sup>

```
from std.python import Python
def main() raises:
    math = Python.import_module("math")
    print(math.sqrt(2.0))
```
1. ↑ Sullivan, Mark (19 March 2024). "How Modular simplified AI software infrastructure". *Fast Company* . Retrieved 19 August 2024.
2. ↑ "Mojo v1.2.0". Modular. 22 October 2026. Retrieved 30 October 2026.
3. 1 2 3 4 *Mojo🔥 is now open source!* , 18 August 2026, retrieved 18 August 2026
4. ↑ "Modular Platform".
5. 1 2 3 4 "Mojo FAQ". Retrieved 18 August 2026.
6. ↑ "Welcome to the Mojo standard library".
7. ↑ 
  - Donovan, Ryan (2 October 2023). "No surprises on any system: Q&A with Loris Cro of Zig". *stackoverflow.blog* . Retrieved 19 August 2026.
  - McKenzie, Cameron (6 November 2024). "What is Mojo programming language and what is it used for?". *TheServerSide.com* . Retrieved 19 August 2026.
  - Gaurav, Dr Kumar (4 April 2024). "Programming Languages for AI Applications and Why Mojo is Among the Best". Retrieved 19 August 2026.
8. Donovan, Ryan (2 October 2023). "No surprises on any system: Q&A with Loris Cro of Zig". 
9. 1 2 Howard, Jeremy (4 May 2023). "fast.ai - Mojo may be the biggest programming language advance in decades". *fast.ai* . Retrieved 28 May 2024.
10. ↑ sylvaticus (14 March 2024). "Advantages of Julia vs Mojo". *Julia Programming Language* . Retrieved 19 August 2026.<sup>[*self-published source*]</sup>
11. ↑ Deutscher, Maria (7 September 2023). "Modular makes its AI-optimized Mojo programming language generally available". *Silicon Angle* . Retrieved 11 September 2023.
12. 1 2 Krzaczyński, Robert (9 November 2023). "Mojo Language SDK Available: Mojo Driver, VS Code Extension, and Jupyter Kernel". *InfoQ* .
13. ↑ "Mojo: Programming language for all of AI". *Modular.com* . Retrieved 28 February 2024.
14. ↑ Krzaczyński, Robert (19 July 2023). "Introduction to Mojo Programming Language". *InfoQ* .
15. ↑ "Mojo programming manual". *docs.modular.com* . Modular. 2023. Retrieved 26 September 2023.Mojo is a programming language that is as easy to use as Python but with the performance of C++ and Rust. Further, Mojo provides the ability to leverage the entire Python library ecosystem.
16. 1 2 3 Krill, Paul (4 May 2023). "Mojo language marries Python and MLIR for AI development". *InfoWorld* . Retrieved 28 May 2024.
17. ↑ "Fn deprication as a Python superset". *Modular* . 16 March 2026. Retrieved 28 July 2026.
18. 1 2 3 4 5 6 7 Yegulalp, Serdar (20 May 2026). "First look: Mojo 1.0 mixes Python and Rust". *InfoWorld* .
19. 1 2 Bensen, Earl (12 August 2026). "Mojo Hits 1.0: AI Systems Language Locks In Stable API, Ending Three Years of Churn". *Tech Times* .
20. ↑ "Should Julia use MLIR in the future?". *Julia Programming Language* . 20 February 2024. Retrieved 28 May 2024.
21. ↑ "Modular Docs: Why Mojo". *docs.modular.com* . Retrieved 28 May 2024.
22. ↑ "Mojo - A system programming language for heterogenous computing" (PDF). Archived from the original (PDF) on 28 May 2024.
23. ↑ Claburn, Thomas (5 May 2023). "Modular finds its Mojo, a Python superset with C-level speed". *The Register* . Retrieved 8 August 2023.
24. ↑ Thomason, James (21 May 2024). "Mojo Rising: The resurgence of AI-first programming languages". *VentureBeat* .
25. ↑ "Mojo changelog". 13 February 2025.
26. ↑ "A unified, extensible platform to superpower your AI". *Modular.com* . Retrieved 14 April 2024.
27. ↑ "Mojo - It's finally here!". *Modular.com* . Retrieved 14 April 2024.
28. ↑ "Mojo is now available on Mac". *Modular.com* . Retrieved 14 April 2024.
29. ↑ "Modular open-sources its Mojo AI programming language's core components". *SiliconANGLE* . 28 March 2024. Retrieved 28 May 2024.
30. ↑ "mojo/stdlib/README.md at nightly · modularml/mojo". *GitHub* . Retrieved 28 May 2024.
31. ↑ "Modular: The path to Mojo 1.0". Modular. 5 December 2025. Retrieved 12 June 2026.
32. ↑ "Mojo releases". Modular. Retrieved 12 June 2026.
33. ↑ "Mojo v1.0.0". Modular. 11 August 2026. Retrieved 12 August 2026.
34. ↑ "Modular Docs - Mojo programming manual". *docs.modular.com* . Retrieved 19 October 2023.
35. ↑ "Modular Docs - Mojo programming manual". *docs.modular.com* . Retrieved 31 October 2023.
36. 1 2 Yegulalp, Serdar (12 November 2025). "Revisiting Mojo: A faster Python?". *InfoWorld* .
37. ↑ Lattner, Chris; Pienaar, Jacques (2019). *MLIR Primer: A Compiler Infrastructure for the End of Moore's Law* (Technical report). Retrieved 30 September 2022.
38. ↑ Lattner, Chris; Amini, Mehdi; Bondhugula, Uday; Cohen, Albert; Davis, Andy; Pienaar, Jacques; Riddle, River; Shpeisman, Tatiana; Vasilache, Nicolas; Zinenko, Oleksandr (29 February 2020). "MLIR: A Compiler Infrastructure for the End of Moore's Law". arXiv:2002.11054 [cs.PL].
39. 1 2 Yegulalp, Serdar (7 June 2023). "A first look at the Mojo language". *InfoWorld* .
40. ↑ "Mojo v25.4 release notes". *GitHub* . Modular. 18 June 2025. Retrieved 12 June 2026.
41. ↑ "Modular Docs: Ownership and borrowing". Modular. Retrieved 29 February 2024.
42. ↑ "Ownership". *mojolang.org* . Modular. Retrieved 12 June 2026.
43. ↑ Godoy, William F.; Melnichenko, Tatiana; Valero-Lara, Pedro; Elwasif, Wael; Fackler, Philip; Ferreira Da Silva, Rafael; Teranishi, Keita; Vetter, Jeffrey S. (25 September 2025). "Mojo: MLIR-Based Performance-Portable HPC Science Kernels on GPUs for the Python Ecosystem". arXiv:2509.21039 [cs.DC].
44. ↑ "Functions". *mojolang.org* . Modular. Retrieved 12 June 2026.
45. ↑ "Mojo v26.2 release notes". *GitHub* . Modular. 19 March 2026. Retrieved 12 June 2026.
46. ↑ "Variables". *mojolang.org* . Modular. Retrieved 12 June 2026.
47. ↑ "Mojo v24.1 release notes". *GitHub* . Modular. 29 February 2024. Retrieved 12 June 2026.
