---
id: collect-261121-veille-ia/langages-dev/typescript-7-upgrade-guide-what-breaks-eslint-next-js-and-real-benchmarks
title: "TypeScript 7 Upgrade Guide: What Breaks, ESLint, Next.js and Real Benchmarks"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-07-08"]
keywords: ["typescript", "compiler", "next", "upgrade", "guide", "code", "server", "three"]
source: http://dev.to/fabwaseem/typescript-7-upgrade-guide-what-breaks-eslint-nextjs-and-real-benchmarks-3m16
source_anchor: ""
source_lines: [1, 248]
section: "Langages_Dev"
---
I tested the TypeScript 7 upgrade and ran into three catches that are easy to miss: removed tsconfig options, a `typescript-eslint` break, and a side-by-side setup that changes which compiler `next build` uses. Here is the full guide with the exact errors and fixes.

TypeScript 7 is the native, Go-based rewrite of the TypeScript compiler, and it has been the `latest` version on npm since July 8, 2026. It type-checks the same code as TypeScript 6 but much faster. On this box Hono's source built **5.2x faster** (3.01 s to 0.58 s), with identical JavaScript output.

The upgrade is usually one `npm install`, with three real catches. Every option deprecated in TypeScript 6 is now a hard error. TypeScript 7 has no programmatic API, so `typescript-eslint` refuses to run. And the side-by-side workaround quietly changes which compiler `next build` uses. I hit all three while testing, so this guide gives the exact errors and fixes, cited to the official TypeScript 7 announcement.

TypeScript 7 is a faithful port of the TypeScript compiler and language service from TypeScript to Go. It ships as the normal `typescript` npm package with the same `tsc` command. Microsoft says it "typically" yields "speedups between 8x and 12x on full builds" through native code and shared-memory multithreading. The type system and emitted JavaScript match TypeScript 6.0.

Key facts at a glance:

|  | TypeScript 6.0 | TypeScript 7.0 | 
|---|---|---|
| Written in | TypeScript (runs on Node.js) | Go (native binary, multithreaded) | 
| npm package | `typescript@6` /`@typescript/typescript6` | `typescript@7` (`latest` , 7.0.2) | 
| Type checking | Reference behavior | Same semantics, with `stableTypeOrdering` always on | 
| Deprecated options | Error, silenced by `ignoreDeprecations: "6.0"` | Hard error, cannot be silenced | 
| Programmatic API | Yes ( `ts.createProgram` , ...) | None yet; planned for 7.1 | 
| Editor | Built-in TS server | LSP server, VS Code extension | 

The announcement adds that "practically any TypeScript code that compiles cleanly with TypeScript 6.0 (with the stableTypeOrdering flag on, and without any ignoreDeprecations flag set) should compile identically in TypeScript 7.0."

Yes, for compiling and type-checking. TypeScript 7.0 is a stable release, and the team tested it with companies including Bloomberg, Canva, Figma, Google, Linear, Notion, Sentry, Slack and Vercel. It is not ready for tools that need the compiler API, such as `typescript-eslint` and Vue, Svelte, Astro or MDX tooling, until TypeScript 7.1 ships a new API.

Quotable results from the announcement:

- Slack's CI type-checking went "from about 7.5 minutes to 1.25 minutes."
- The new language server "reduced failing language server commands by over 80%, and reduced server crashes by over 60%" compared with 6.0.
- Opening a file with an error in the VS Code codebase dropped from about 17.5 seconds to under 1.3 seconds.

The TypeScript 7.1 iteration plan lists "Stabilize API" as its first item. It scheduled the 7.1 beta for October 6 and the stable release for November 24, 2026. On October 17, 2026 npm still had no 7.1 beta, only nightly `7.1.0-dev` builds under the `next` tag, so treat the date as a target.

On an 8-vCPU Linux box, TypeScript 7.0.2 built Hono's source (189 files, about 26,000 lines, declarations plus JavaScript) in a median 0.58 seconds, against 3.01 seconds for TypeScript 6.0.3. That is a 5.2x speedup with 27% less peak memory. Both compilers emitted byte-identical JavaScript for all 188 output files.

The run: `tsc -p tsconfig.build.json` on Hono at commit `5ca49b0`, three runs each, Node.js 24.22.0, measured with `os.wait4` for wall time and peak RSS.

| Compiler | Median | Runs | Peak memory | 
|---|---|---|---|
| TypeScript 6.0.3 | 3.01 s | 2.85, 3.01, 3.08 | 528 MB | 
| TypeScript 7.0.2 (default, 4 checkers) | **0.58 s** | 0.58, 0.50, 0.59 | 383 MB | 
| TypeScript 7.0.2 `--checkers 8` | 0.61 s | 0.61, 0.60, 0.62 | 416 MB | 
| TypeScript 7.0.2 `--singleThreaded` | 0.98 s | 0.98, 1.03, 0.89 | 250 MB | 

My 5.2x is below Microsoft's 8x to 12x because this project is small. Startup time weighs more on a 3-second build than on VS Code's 125-second one, so expect larger gains on larger codebases. Two things surprised me:

- 
**More checkers didn't help here.**`--checkers 8` was no faster than the default of 4 and used more memory. The announcement says extra checkers help "larger codebases" at "the cost of increased memory usage."
- 
**Even single-threaded, TypeScript 7 was 3x faster** and used half the memory. On small CI runners`--singleThreaded` or`--checkers 1` is a reasonable choice.

The JavaScript was identical. The `.d.ts` files were not: 25 of 188 declaration files differed. In 16 the only change was quote style (`'/'` instead of `"/"`). In 9, TypeScript 7 wrote a reference such as `toArray: typeof toArray` where TypeScript 6 had inlined the full function type:


```
-    toArray: (children: Child) => Child[];
+    toArray: typeof toArray;
```
These are equivalent types, but if you publish a library, diff your generated declarations or run your API report tool before releasing a version built with TypeScript 7.

Three things break most often. Every compiler option deprecated in TypeScript 6.0 is now a hard error that `ignoreDeprecations` no longer silences. The new defaults (`types: []`, `rootDir: "./"`, `strict: true`) surface errors in existing configs. And any tool that imports the `typescript` API fails, because 7.0 ships without one.

I ran a deliberately old `tsconfig.json` through both compilers. TypeScript 6 already reports these as errors (`TS5101`/`TS5107`, "will stop functioning in TypeScript 7.0"), but you can silence them with `"ignoreDeprecations": "6.0"`. TypeScript 7 reports `TS5102`/`TS5108` and ignores that flag (with it set, 7.0.2 still exited 1):


```
tsconfig.json(3,15): error TS5108: Option 'target=ES5' has been removed. Please remove it from your configuration.
tsconfig.json(5,25): error TS5108: Option 'moduleResolution=node10' has been removed. Please remove it from your configuration.
tsconfig.json(6,5): error TS5102: Option 'baseUrl' has been removed. Please remove it from your configuration.
  Use '"paths": {"*": ["./*"]}' instead.
tsconfig.json(7,24): error TS5090: Non-relative paths are not allowed. Did you forget a leading './'?
tsconfig.json(8,5): error TS5102: Option 'downlevelIteration' has been removed. Please remove it from your configuration.
tsconfig.json(9,24): error TS5108: Option 'esModuleInterop=false' has been removed. Please remove it from your configuration.
```
The full list of removals and their replacements, from the announcement:

| Removed in TypeScript 7 | Use instead | 
|---|---|
| `target: "es5"` ,`downlevelIteration` | `es2015` or later (a bundler can down-level if you must) | 
| `moduleResolution: "node"` /`"node10"` /`"classic"` | `"nodenext"` (Node) or`"bundler"` (Vite, Next.js, webpack) | 
| `module: "amd"` ,`"umd"` ,`"systemjs"` ,`"none"` | `"esnext"` or`"preserve"` | 
| `baseUrl` | `paths` with`./` -relative entries | 
| `esModuleInterop: false` ,`allowSyntheticDefaultImports: false` | Remove (always on) | 
| `alwaysStrict: false` | Remove (always on) | 
| `module Foo {}` namespace syntax | `namespace Foo {}` | 
| `import ... assert { type: "json" }` | `import ... with { type: "json" }` | 
| `tsc file.ts` inside a folder with`tsconfig.json` | `tsc --ignoreConfig file.ts` | 

After removing those options, two TypeScript 6 defaults (which 7 keeps) produced new errors:


```
error TS5011: The common source directory of 'tsconfig.json' is './src'. The 'rootDir' setting must be
explicitly set to this or another path to adjust your output's file layout.
src/index.ts(2,19): error TS2591: Cannot find name 'process'. Do you need to install type definitions for node?
Try `npm i --save-dev @types/node` and then add 'node' to the types field in your tsconfig.
```
`rootDir` now defaults to `./`, and `types` now defaults to `[]`, so `@types/*` packages are no longer included automatically. This is the config that compiled and ran on TypeScript 7.0.2:


```
{
  "compilerOptions": {
    "target": "es2022",
    "module": "nodenext",
    "outDir": "dist",
    "rootDir": "./src",
    "types": ["node"]
  },
  "include": ["src"]
}
```
Other defaults to know: `strict` is `true`, `module` is `esnext`, `noUncheckedSideEffectImports` is `true`, and `stableTypeOrdering` is always on.

Template literal inference now keeps Unicode code points whole. This compiles on TypeScript 7 and fails on 6, which also makes it a quick way to prove which compiler a tool is running:


```
// lib/which-ts.ts — compiles only on TypeScript 7
type HeadTail<S> = S extends `${infer Head}${infer Tail}` ? [Head, Tail] : never
export const check: HeadTail<'😀abc'> = ['😀', 'abc']
```
TypeScript 6 reports `Type '"😀"' is not assignable to type '"�"'`, because it splits the emoji into two UTF-16 halves.

JSDoc-typed JavaScript is also stricter: `@enum`, `@class`, a bare `?` type and Closure-style `function(string): void` are no longer recognized. The CHANGES.md file tracks the full list.

No. `typescript-eslint` 8.71.2 (the latest on October 17, 2026) declares `typescript >=4.8.4 <6.1.0` as a peer dependency and throws at startup when it finds TypeScript 7. Because `eslint-config-next` uses it, `npm run lint` fails in every Next.js app that upgrades. The fix is to keep TypeScript 6 installed under the `typescript` name for the API.

This is the exact error from `npx eslint .` in a Next.js 16.4.2 app after `npm i -D typescript@7.0.2`:


```
typescript-eslint does not support TS 7.0.
Please see https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/#running-side-by-side-with-typescript-6.0 to run typescript-eslint using the TS 6 API.
See also https://github.com/typescript-eslint/typescript-eslint/issues/10940 for tracking typescript-eslint's support for TS >=7.1
Oops! Something went wrong! :(
ESLint: 9.39.5
Error: typescript-eslint does not support TS 7.0.
```
Support is tracked in typescript-eslint issue #10940 and depends on the 7.1 API.

Install TypeScript 7 under an alias such as `@typescript/native`, and install `@typescript/typescript6` under the `typescript` name. Tools that import `typescript` (ESLint, Vue, Svelte) get the 6.0 API, while `npx tsc` runs the TypeScript 7 binary. This is the setup the TypeScript team recommends in its announcement.


```
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```
What I saw after `npm install`:


```
$ npx tsc -v
Version 7.0.2
$ npx tsc6 -v
Version 6.0.3
$ npx eslint .
(no output, exit 0)
```
The compatibility package ships a `tsc6` binary, so the two never fight over the name `tsc`.

With the alias setup, `next build` type-checks with **TypeScript 6**, not 7. Next.js resolves the package named `typescript`, finds the compatibility package and runs its `tsc6`. I proved it with the emoji file above: `npx tsc` passed, but `next build` failed with the TypeScript 6 error:


```
  Running TypeScript ...
lib/which-ts.ts(3,42): error TS2322: Type '"😀"' is not assignable to type '"�"'.
Failed to type check.
```
So pick one of two setups until typescript-eslint supports 7.1:

| Setup | `next build` checks with | ESLint (type-aware) | Best for | 
|---|---|---|---|
| **A.**`typescript@7` only | TypeScript 7 (fast) | Broken | Projects that lint with Biome or oxlint, or no type-aware lint | 
| **B.** Alias:`typescript` = TS 6,`@typescript/native` = TS 7 | TypeScript 6 | Works | Projects on `eslint-config-next` | 

With setup B, add `npx tsc --noEmit` as its own CI step to get TypeScript 7's speed for type checking. Next.js's `typescript.ignoreBuildErrors` would skip the slower check in `next build`, but only use it if that `tsc` step blocks the merge, because the Next.js docs call that option "very dangerous" on its own.

Yes. Since Next.js 16.3, `next build` can type-check with TypeScript 7, and in 16.4 it uses the project's `tsc` CLI by default (`experimental.useTypeScriptCli: true`), which works without the compiler API. Install `typescript@^7` and it is used automatically. In my test the "Running TypeScript" step dropped from 1,368 ms to 231 ms.

The Next.js 16.3 release post says: "To start using TypeScript 7 for type checking during next build, just bump your project's local dependency." The TypeScript config docs add the trade-off: CLI type checking "prints the native tsc diagnostics" and "does not apply Next.js-specific code frames," and it checks every file your `tsconfig.json` includes, test files too.

My measurements on the Next.js 16.4.2 app from my use cache guide:

|  | TypeScript 5.9.3 (create-next-app default) | TypeScript 7.0.2 | 
|---|---|---|
| `npx tsc --noEmit` | 1.37 s | 0.38 s | 
| `next build` "Finished TypeScript in" | 1,368 ms | 231 ms | 
| Real type error caught by `next build` | Yes | Yes ( `TS2322` , build fails) | 

`create-next-app` 16.4.2 still writes `"typescript": "^5"` into new projects, so new apps start on 5.9 until you upgrade. The app's default tsconfig (`moduleResolution: "bundler"`, no `baseUrl`) needed no changes for TypeScript 7.

To upgrade safely, get to TypeScript 6.0 with zero deprecation errors first, then install TypeScript 7, fix the `types` and `rootDir` defaults, decide how to keep the 6.0 API for ESLint and framework tooling, and add TypeScript 7 to CI. Library authors should also diff their generated `.d.ts` files before publishing.

1. 
**Upgrade to TypeScript 6.0** and remove`ignoreDeprecations` . Fix every`TS5101` /`TS5107` deprecation error; each becomes a hard TypeScript 7 error.
2. 
**Replace `baseUrl`** with`./` -relative`paths` , and`moduleResolution: "node"` with`"bundler"` or`"nodenext"` .
3. 
**Install TypeScript 7** :`npm i -D typescript@^7` (or the alias setup if step 6 applies).
4. 
**Add `"types": ["node"]`** (plus`jest` ,`bun` and so on) if globals like`process` stop resolving.
5. 
**Set `rootDir`** when`tsconfig.json` sits above`src` and you emit files.
6. 
**Check API-dependent tools** : typescript-eslint,`eslint-config-next` , Vue/Volar, Svelte, Astro, MDX, Angular templates. If you use any, keep`typescript` pointed at`@typescript/typescript6` .
7. 
**Add `npx tsc --noEmit` to CI** and check that it runs version 7 (`npx tsc -v` ).
8. 
**Tune parallelism** : try`--checkers 1` or`--singleThreaded` on small CI runners and higher`--checkers` on large monorepos, and measure.
9. 
**Libraries: diff `.d.ts` output** between 6 and 7 before publishing.
10. 
**Editor** : install the TypeScript 7 VS Code extension, and use "Disable TypeScript 7 Language Server" for Vue or Svelte projects.

The most common mistakes are upgrading straight from 5.x without passing through 6.0's deprecation errors, assuming the alias setup makes `next build` use TypeScript 7, forgetting the new `types: []` default, and trusting a passing build while lint is silently skipped. Each produces either a confusing error or a slower build than you expected.

| Mistake | Symptom | Fix | 
|---|---|---|
| Skipping TypeScript 6 | Wall of `TS5102` /`TS5108` errors | Upgrade to 6.0 first and clear its deprecations | 
| Keeping `ignoreDeprecations: "6.0"` | Still errors on 7 | Remove it and fix the options | 
| `typescript@7` with`eslint-config-next` | "typescript-eslint does not support TS 7.0" | Alias setup B | 
| Alias setup but expecting fast `next build` | Build still uses `tsc6` | Run `npx tsc --noEmit` in CI as well | 
| Relying on automatic `@types/*` | `Cannot find name 'process'` (TS2591) | `"types": ["node"]` | 
| Upgrading a Vue, Svelte or Astro app's editor | Lost template type checking | Keep TypeScript 6 for the editor | 
| Publishing a library without checking `.d.ts` | Different declaration text | Diff and review before release | 

If you deploy through Docker, rebuild without cache after changing the compiler. My Next.js Dockerfile guide shows a multi-stage build where `npm run build`, and with it the type check, runs in the builder stage.

**Related:** Prisma 8 needs TypeScript 5.9 or newer, and its client type-checked cleanly on TypeScript 7.0.2 in my test. See Prisma 8 vs Prisma 7: should you upgrade?

**Related:** on NestJS, TypeScript 7 isn't usable through the Nest CLI yet: `nest build` asks for TypeScript 6 until 7.1. Plain `tsc` 7 works. Details in my NestJS 12 migration guide.

**Related:** Oxlint's type-aware mode runs on typescript-go too. On this site's code it matched all 80 typescript-eslint findings in 1.29 s vs 20.5 s; details in Oxlint vs Biome vs ESLint.

- TypeScript 7 is the native Go port of TypeScript, `latest` on npm since July 8, 2026 (7.0.2), with the same type system as 6.0.
- On Hono's source it built 5.2x faster than TypeScript 6 on my 8-vCPU box, with identical JavaScript output.
- Everything deprecated in TypeScript 6 is a hard error in 7: es5, node10 resolution, baseUrl, AMD/UMD modules, esModuleInterop false.
- New defaults bite: types is [] and rootDir is ./, so add "types": ["node"] and an explicit rootDir.
- typescript-eslint does not support TypeScript 7; keep @typescript/typescript6 under the typescript name until 7.1 ships an API.
- Next.js 16.3+ type-checks with TypeScript 7 automatically, but the alias setup makes next build use TypeScript 6, so run npx tsc in CI too.
- Library authors should diff .d.ts output, which can differ in quote style and typeof references.

*Originally published on fabwaseem.com.*

## Top comments (0)
