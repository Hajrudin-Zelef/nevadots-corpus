---
id: collect-261121-veille-ia/langages-dev/tsc-typescript-6-and-earlier-vs-typescript-7-native-tsc
title: "tsc (TypeScript 6 and earlier) vs TypeScript 7 (native tsc) |…"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-06"]
keywords: ["typescript", "microsoft", "tsc", "earlier", "native", "javascript", "tools", "compiler"]
source: https://assemblyline.org/software/replace/tsc
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
# Instead of tsc (TypeScript 6 and earlier): TypeScript 7 (native tsc)

Upgrade to TypeScript 7. It is the same tsc command and the same type system, ported from JavaScript to Go, and Microsoft measures full builds at 8x to 12x faster with less memory. Stay on TypeScript 6 only where a tool needs the compiler's programmatic API, which TypeScript 7.0 does not ship.

Both are for type-checking TypeScript projects and compiling them to JavaScript.

## Why TypeScript 7 (native tsc) is better

- Full builds of large open-source codebases drop sharply in Microsoft's benchmarks, with VS Code going from 125.7s to 10.6s, Sentry from 139.8s to 15.7s.
- The editor gets faster too. Opening a file with an error in the VS Code codebase showed the first error after about 17.5 seconds with TypeScript 6 and under 1.3 seconds with TypeScript 7.
- Parsing, type-checking and emit run in parallel, tunable with --checkers and --builders, and --build can compile several project references at once.
- Microsoft reports the new language server cut failing commands by over 80% and crashes by over 60% compared with TypeScript 6.0.
- Slack reported CI type-checking falling from about 7.5 minutes to 1.25 minutes.

## When to keep tsc (TypeScript 6 and earlier)

Keep TypeScript 6 alongside 7 where tools import the compiler API, such as typescript-eslint, or where the editor setup depends on language server plugins, as with Vue, Svelte, Astro, MDX and Angular templates. Microsoft's @typescript/typescript6 package installs it as tsc6 so the two run side by side.

## Switching

npm install -D typescript gets version 7. Upgrade to TypeScript 6.0 first and clear its deprecation warnings, because 7.0 turns them into hard errors. Target es5, downlevelIteration, moduleResolution node10 and the amd, umd and systemjs module formats are gone. New defaults apply too, including strict set to true and module set to esnext.

## Watch out

There is no programmatic API in 7.0. TypeScript 7.1, in beta since October 2026, adds a new and different API, so tools that call the compiler have to be rewritten for it. JavaScript files checked through JSDoc follow some revised inference rules. The staging repository microsoft/typescript-go is archived, and development now happens in microsoft/TypeScript.

## More newer, better JavaScript bundlers, compilers and build tools

## Sources

Judged on 2026-11-06. See every javaScript bundlers, compilers and build tools, or all 66 replacements.
