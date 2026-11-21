---
id: collect-261121-veille-ia/langages-dev/why-typescript-7-is-written-in-go-not-c-43-years-of-anders-hejlsberg
title: "Why TypeScript 7 Is Written in Go, Not C#: 43 Years of Anders Hejlsberg"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-07-08"]
keywords: ["typescript", "hejlsberg", "code", "pascal", "compiler", "turbo", "program", "javascript"]
source: http://dev.to/vladut02/why-typescript-7-is-written-in-go-not-c-43-years-of-anders-hejlsberg-ko2
source_anchor: ""
source_lines: [1, 60]
section: "Langages_Dev"
---
On July 8, 2026, TypeScript 7 shipped: the same compiler, ported to a new language, and about ten times faster. On the VS Code codebase, a full build went from 125.7 seconds to 10.6 seconds.

The port had been announced in March 2025 by Anders Hejlsberg, the man who designed C#. And the new language wasn't C#. It was Go.

To see why that makes sense, it helps to go back 43 years. Across four languages, one idea keeps coming back: the time between typing your code and seeing what it does.


In the early 1980s, Hejlsberg wrote a Pascal compiler for small personal computers in Denmark. It was sold as Compass Pascal, and later as Poly Pascal. A young California company, Borland, licensed it, and Hejlsberg stayed in Denmark: according to Dr. Dobb's, Borland sent him 9600-baud modems so he could send each new build to California.

On November 20, 1983, it shipped as **Turbo Pascal**. The numbers, again from Dr. Dobb's:

- 
**$49.95** , when its nearest competitor cost nearly $500
- 
**one floppy disk** , ten files
- 
`TURBO.COM` , the main program, was**33,280 bytes**

The editor and the compiler lived in one program. You typed, compiled and ran without leaving it. Hejlsberg later wrote that back in the old Turbo Pascal days, it was possible for one person to write and maintain an entire product.

Windows brought windows, buttons and menus, and building them by hand meant pages of code. On February 14, 1995, Borland launched Delphi in San Francisco, with Hejlsberg as chief architect. You drew a window, dropped a button on it and wrote what should happen on click. Underneath, it was still Pascal, grown out of Turbo Pascal.

In 1996 he joined Microsoft. In June 2000, Microsoft announced C#, with Hejlsberg as lead designer (the ECMA-334 standard names him first among its principal inventors). The goal, as described at the time: the power of C++ with the ease of Visual Basic.

That Delphi button followed him. In C#, a click calls your method through a delegate, and every event you write works that way. (How delegates ended up in C# is its own story, involving Sun and a court order: Why C# Exists.)

He stayed with C# as it grew: generics in 2005, LINQ in 2007, async/await in 2012.

By 2012, people were building JavaScript applications of a size the language was never planned for. Some teams inside Microsoft used tools like Script# to write C# and compile it to JavaScript. TypeScript went the other way: don't hide JavaScript, put types on top of it.

Microsoft introduced TypeScript on October 1, 2012. Every JavaScript program was already a TypeScript program, and the types are erased when you compile. So what are they for? For the tools: errors as you type, autocomplete, safe renames. It's the Turbo Pascal idea again. The fastest feedback is the one you get before you run the code.

TypeScript took over. By 2025, VS Code alone was about **1.5 million lines** of it. But the compiler was itself written in TypeScript, running as JavaScript. On VS Code, `tsc` took **77.8 seconds**, and just opening the project in the editor took about **9.6 seconds**.

In March 2025, Hejlsberg wrote the announcement: a native port of the compiler. Not a rewrite. A port: the same logic, moved over piece by piece, so it behaves the same.

One thing worth stating plainly: **your TypeScript code didn't change**. Only the program that checks it did.

Plenty of C# developers asked. The team answered in public, in a “Why Go?” FAQ:

- 
**It's a port, so the code shape matters most.** "Idiomatic Go strongly resembles the existing coding patterns of the TypeScript codebase", which is mostly plain functions and data structures rather than classes.
- 
**Memory control with a garbage collector.** Go lets them control how data is laid out in memory, without managing memory by hand.
- 
**Batch builds can skip GC.** "Batch compilations can effectively forego garbage collection entirely, since the process terminates at the end."

In an interview with Michigan TypeScript in March 2025, as reported by devclass, Hejlsberg said they did consider C#, but that C# is bytecode-first and its native (AOT) compilation wasn't available on every platform they needed.

So it wasn't a verdict on C#. It was the tool that fit this one job.

TypeScript 7 shipped on July 8, 2026, with full builds typically **8 to 12 times faster**. On VS Code, a full build went from 125.7 s to 10.6 s, and the time until the first error appears in the editor dropped from about 17.5 seconds to under 1.3.

Turbo Pascal, Delphi, C#, TypeScript. Different companies, different platforms. Each time, the tool around the language mattered as much as the language. And when his own compiler got slow, his team didn't reach for his own language. They reached for the one that fit the job.

Would you have ported it to C#? Tell me in the comments.

*I make Vlad's Stack — short explainers and stories about how the tools you use every day actually work, for people who write code (.NET-flavored). The video version of this story: https://www.youtube.com/channel/UCUO8Uo5LsEy1b9eRkrH6JNg*

## Top comments (0)
