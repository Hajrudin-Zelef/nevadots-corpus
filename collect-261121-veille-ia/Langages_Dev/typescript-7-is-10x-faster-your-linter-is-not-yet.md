---
id: collect-261121-veille-ia/langages-dev/typescript-7-is-10x-faster-your-linter-is-not-yet
title: "TypeScript 7 is 10x faster. Your linter is not, yet"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-07-08"]
keywords: ["typescript", "compiler", "type", "faster", "everything", "runs", "next", "linter"]
source: https://godek.dev/blog/typescript-7-faster-linter-not-yet
source_anchor: ""
source_lines: [1, 70]
section: "Langages_Dev"
---
Microsoft rewrote the TypeScript compiler in Go, shipped it as TypeScript 7.0 on 8 July 2026, and the headline held: full type checks run 8 to 12x faster on their benchmarks, 6x on this site. Then you open the project, and ESLint, the Vue or Svelte checker and half your editor are still on TypeScript 6. That is not a bug. It is the deal 7.0 made, and 7.1 is the release that starts paying it back.

## What changed in TypeScript 7

The 7.0 announcement is the source for all of this. Compiler and language server were ported from JavaScript to Go, keeping the original structure so results match 6.0. Native code plus shared-memory multithreading: parsing, type checking and emit run in parallel, `--checkers` (default 4) sets the thread count. Same language, same `tsc`, same errors; code that compiles cleanly on 6.0 without `ignoreDeprecations` should compile identically on 7.0.

What 7.0 does not have is an API. In the post's words, it "does not ship with an API", and the team recommends it "in scenarios where language server plugins are not required". Everything that embeds the compiler as a library, instead of running `tsc`, stays on 6.0. That is the whole story of this note.

## The numbers, theirs and other people's

Microsoft's table, TypeScript 6 to 7, full build, default `--checkers 4`:

| Codebase | TS 6 | TS 7 | Speedup | 
|---|---|---|---|
| VS Code | 125.7 s | 10.6 s | 11.9x | 
| Sentry | 139.8 s | 15.7 s | 8.9x | 
| Bluesky | 24.3 s | 2.8 s | 8.7x | 
| tldraw | 11.2 s | 1.46 s | 7.7x | 

Memory went down 6 to 26%, and the post quotes Slack cutting CI type checking from 7.5 to 1.25 minutes.

Independent numbers are lower, and more useful. Prisma (9 July 2026) took a whole monorepo from about 74 s on TypeScript 5.8.2 to about 24 s on 7.0.2, roughly 3x, median of several GitHub Actions runs. A 728-file Next.js app (June 2026, 7.0 preview) went from 6.7 s to 2.1 s cold, 3.2x, with `skipLibCheck` already on; the native compiler also caught one overload error the old one had let through. The spread is the lesson: the 10x comes from huge, type-heavy codebases on many cores, a repo that is mostly `skipLibCheck` and I/O gets 3x. Both are worth having.

## Measured on this site: 2.3 s to 0.4 s

This site is a small Next.js 16 app: 227 TypeScript files of my own, 9,500 lines, plus about a thousand declaration files from `node_modules`. The repo is on TypeScript 5.9.3, so the honest comparison is 5.9 against 7.0.2. Apple M1 Pro, 10 cores, binaries called directly, the incremental `tsconfig.tsbuildinfo` deleted before every run so nothing is a no-op, seven runs, median:

|  | TypeScript 5.9.3 | TypeScript 7.0.2 | 
|---|---|---|
| `tsc --noEmit` , cold | 2.32 s | 0.37 s (6.3x) | 
| peak memory | 433 MB | 349 MB | 
| `tsc --noEmit --singleThreaded` |  | 0.69 s (3.4x) | 
| `next typegen && tsc --noEmit` | 2.9 s | 0.93 s | 

Both exited 0 with zero diagnostics, straight from 5.9 without a stop at 6.0. Single-threaded, the Go compiler alone is 3.4x; the threads buy the rest. And at this size the floor is now the tooling around `tsc`: `next typegen` takes longer than the type check. Two seconds per run is a rounding error for me. Across a day of CI on a real monorepo it is not, which is what Prisma's and Slack's numbers say.

The whole experiment, nothing installed into the repo:

Run each a few times, take the median. `npx` itself adds most of a second, which is why the table calls the binaries directly.

## Why your linter and your framework are still on 6

Everything that runs `tsc` as a command is fine. Everything that does `import ts from "typescript"` is not, because the thing it imports is gone. So most teams run a split brain in November 2026: TypeScript 7 for `tsc` in CI, TypeScript 6 for everything else. Microsoft ships `@typescript/typescript6` for exactly this, a `tsc6` binary plus the 6.0 API. The wiring, from the 7.0 post:

`npx tsc` now runs 7.0, anything resolving `typescript` gets 6.0. Who needs it, as of November 2026:

- **typescript-eslint.** Peer range`typescript >=4.8.4 <6.1.0` , so 7 as`typescript` is an`ERESOLVE` before you have linted anything. The TypeScript 7 PR is open: an opt-in backend that talks to the native compiler over IPC, and by the author's own numbers type-aware rules are not faster yet, on some rule sets slower, until 7.1 grows bulk APIs. Your typed lint will not get its 10x in 2026.
- **Vue.**`vue-tsc` embeds the API. The maintainer's bridge to the native compiler took the project's own typecheck from 12.8 to 4.7 s, and he was clear the full 10x cannot carry over while SFC parsing stays in JavaScript. The plan is a content mapper (below), then deprecating`vue-tsc` .
- **Svelte.**`svelte-check` 4.7.4 tolerates TypeScript 7 under the`@typescript/native` alias; the check itself runs on 6.
- **Astro.**`astro check` on TypeScript 7 prints instructions instead of checking. The team built an Astro content mapper, which needs 7.1.

## Next.js 16 and TypeScript 7

Next.js is the easy case: since 16.3 it does not need the API to type check. PR #96497 (August 2026, in 16.3.0) made the project-local `tsc` CLI the default checker for `next build`; `experimental.useTypeScriptCli` is `true` unless you turn it off, and turning it off with TypeScript 7 installed makes the build exit and tell you why. Next.js 15.5 rejects TypeScript 7 outright, so the path starts at 16.3, and you want 16.4.1 or later anyway. Per the docs, diagnostics are raw `tsc` output with no Next.js code frames, and `typescript.ignoreBuildErrors` skips the CLI checker too. `next typegen && tsc --noEmit` stays the CI recipe; it is the `typecheck` script on this site, and it passed on 7.0.2 unchanged.

The editor is the trade. The `{ "name": "next" }` entry in `tsconfig.json` is a language service plugin, and the native language server does not load third-party code into the Go process. With the TypeScript 7 extension (`TypeScriptTeam.native-preview`, the default route in the 7.0 post; VS Code's release notes through 1.145 do not mention it being built in) you lose the plugin's in-editor nudges about segment config and misplaced `"use client"`. You keep the generated route types, where the real errors are caught, and you get hover and go-to-definition that do not make you wait. I would take that trade on every Next.js project I have.

## What 7.1 unblocks, and when

The 7.1 beta shipped on 20 October 2026 with the two things tool authors were waiting for. An API, split into entry points (`typescript/sync`, `typescript/async`, `typescript/fs`, `typescript/ast`); it is what typescript-eslint's PR is built on. And content mappers: a package registered in `tsconfig.json` under `contentMappers` turns a `.vue`, `.svelte` or `.astro` file into virtual TypeScript with spans mapped back to the source, so plain `tsc` can check it. The post lists prototypes for Astro, CSS Modules, Ember, MDX, Svelte and Vue and calls Volar's model "a first-class citizen" now. A mapper is an external executable, so `tsc` runs them only with `--runExternalCode`; Next.js has an open PR to pass that flag during `next build`.

And 7.1 stable is not the finish line. Each tool needs its own release on top: typescript-eslint's PR has to merge, Vue's content mapper is not on npm, Astro's editor integration is a pending PR. Budget early 2027 for one compiler everywhere, and keep the alias until then.

## What I'd actually do

- **Upgrade now** if your type checking is plain`tsc` : Next.js 16.3+, React, Node services, libraries. Install`typescript@^7` , run`tsc --noEmit` , fix what 7 refuses (the options 6.0 deprecated, the odd stricter overload), bank the CI minutes. If you lint with type-aware rules, add the alias so`typescript` stays 6 for ESLint.
- **Wait** if your templates are the point: Vue, Svelte, Astro, Angular. Run 7 on the`.ts` files in CI under the alias if you want the speed, keep the framework checker on 6, and watch for your framework's content-mapper release after 7.1 stable.
- **Measure before you brag.** Delete the`.tsbuildinfo` , run seven times, report the median. My 6x is a two-second win on a small site. The 10x is real on the codebases where 10x is worth a migration.

The compiler jumped a decade in one release. The tools around it were promised the same in 7.1, and the beta is out. Until it lands, run two TypeScripts and do not apologise for it. If CI is where it hurts, that is the hour I'd start with.
