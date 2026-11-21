---
id: collect-261121-veille-ia/langages-dev/typescript-7-1-beta-migration-guide-install-benchmark-fix-diego-rodriguez
title: "TypeScript 7.1 Beta Migration Guide: Install, Benchmark, Fix | Diego Rodriguez"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-20"]
keywords: ["typescript", "beta", "import", "source", "compiler", "imports", "webassembly", "stable"]
source: https://www.diego-rodriguez.work/blog/typescript-7-1-beta-2026-migration-guide
source_anchor: ""
source_lines: [1, 361]
section: "Langages_Dev"
---
TypeScript 7.1 beta shipped on 20 October 2026. It brings back what 7.0 left out in July: a programmatic API for tools built on top of the compiler, plus a plugin point called content mappers that lets tsc check .vue, .svelte or .mdx files. It also adds import attributes in ambient modules, source phase imports for WebAssembly, a stable es2026 target and two behavioural changes that can turn a green build red.

I installed 6.0, 7.0 and 7.1 beta side by side in a throwaway project, reproduced both kinds of breakage, tested the fixes on all three compilers and wrote a small benchmark script. This is the guide I’d hand a Next.js or Node team: how to try the beta without breaking the editor or CI, how to measure your own speedup, and when to wait.

## 1. The Problem: 7.0 Was Fast, but Half the Toolchain Couldn’t Follow

TypeScript 7.0 shipped on 8 July 2026 as the native Go port of the compiler. Microsoft reported typical speedups of 8–12× on full builds; the VS Code codebase went from 125.7 s to 10.6 s with the default four type-checker threads. The catch was spelled out in the announcement: 7.0 shipped without an API. Anything that imports the compiler as a library — typescript-eslint’s type-aware rules, Volar for Vue, svelte-check, Angular’s template checking — had to stay on 6.0.

### How we got here (verified dates)

### Names and dates that circulate wrong

Some coverage dates the beta to 6 October; the devblog post and the npm publish time both say 20 October 2026. The package is the regular typescript package (npm install -D typescript@beta resolves to 7.1.0-beta) and the binary is still tsc. @typescript/native-preview was the pre-7.0 nightly channel; nightlies now ship as typescript@next. The 10 November RC and 24 November stable dates you may have seen come from third-party posts — Microsoft only says “about a month”.

So the question for a Next.js or Node team isn’t whether 7.x is fast — it is. It’s whether your whole toolchain can run on the same compiler, and what 7.1 changes in code that compiled cleanly on 7.0.

## 2. Minimal Concepts: What 7.1 Actually Changes

Six things, in the order they will reach you. The first two are why this release exists; the last one is why your build might fail.

### A new programmatic API

typescript/sync · /async · /fs · /ast

Not the 6.0 API ported over: separate entry points for a synchronous and a Promise-based client, virtual file systems, snapshots and AST helpers. API, Program and Snapshot objects live partly on the Go side, so you dispose them (using, or close() and dispose()). It doesn’t read tsconfig.json or support --project.

### Content mappers

contentMappers in tsconfig · --runExternalCode

A plugin registers for extensions like .vue and hands tsc a virtual TypeScript view with mappings back to the original file. Microsoft lists prototypes or implementations for Astro, CSS Modules, Ember, MDX, Svelte and Vue. Because they execute package code, tsc only runs them with --runExternalCode.

### Import attributes in ambient modules

declare module "*" with { type: "css" }

Type imports by their attribute instead of their file extension: CSSStyleSheet for type: "css", string for "text", Uint8Array for "bytes". An import that omits the attribute fails with TS2307. You write these declarations yourself; they aren’t in the built-in libs.

### Source phase imports

import source · import.source() · WebAssembly

import source X from "./x.wasm" gives you a WebAssembly.Module before it’s instantiated, ready for WebAssembly.instantiate. Requires module esnext, nodenext or preserve plus the esnext lib or target.

### Stable es2026 target and lib

--target es2026 · --lib es2026

Math.sumPrecise, Error.isError, Array.fromAsync, Iterator.concat, JSON.rawJSON, Map and WeakMap getOrInsert and the Uint8Array base64/hex methods move into a stable lib. 6.0 and 7.0 reject the value with TS6046.

### Two behavioural changes

DOM lib.d.ts · distributive type parameters

Regenerated DOM types can surface new errors, and inside a distributive conditional type the checked type parameter is now treated as a new, narrower one, so constraints that leaned on the original fail with TS2344.

### Microsoft’s numbers: 7.0 vs 7.1 beta

Measured by Microsoft with --noEmit and the default four type-checker threads. These compare 7.1 against 7.0, not against 6.x: the big jump already happened in July. Vendor benchmarks on well-known repos are a reference, not a promise for yours.

| Codebase | 7.0 | 7.1 beta | Speedup | Memory 7.0 → 7.1 beta | 
|---|---|---|---|---|
| mui-docs | 22.2 s | 11.9 s | 1.9× | 8,093 → 4,079 MiB (−49.6%) | 
| t3code-server | 26.5 s | 19.1 s | 1.4× | 6,894 → 5,197 MiB (−24.6%) | 
| sentry | 19.6 s | 15.1 s | 1.3× | 6,587 → 5,216 MiB (−20.8%) | 
| vscode | 21.6 s | 17.7 s | 1.2× | 8,387 → 6,943 MiB (−17.2%) | 
| bluesky | 4.48 s | 3.89 s | 1.2× | 2,017 → 1,698 MiB (−15.8%) | 

The vscode row doesn’t match July’s 10.6 s, and Microsoft doesn’t explain the difference between the two runs. Compare rows within one table, never across posts.

## 3. Tutorial: 7.1 Beta in a Next.js Project Without Breaking Anything

Everything below ran in my sandbox on Windows 11 with Node 22.22 and npm 11.11, against typescript 7.1.0-beta, 7.0.2 and @typescript/typescript6 6.0.2 (whose tsc6 reports 6.0.3). I only tested npm; the outputs in the comments are the ones I got.

### Step 1 — Record a baseline before you change anything

Write down which compiler really runs today and save its error list. You’ll diff against it after every change, and the version line catches a surprise I describe in the mistakes section.

node -v
npx tsc -v                  # the compiler your scripts really run
npm ls typescript           # who else pulls in a TypeScript
npx tsc --noEmit --pretty false > typecheck-before.txt

### Step 2 — Install 7.1 beta next to what you have

Pick the path that matches your lint setup. Path A suits projects already green on 7.0 with no type-aware linting. Path B is the alias layout Microsoft recommended for 7.0, pointed at the beta: the typescript name resolves to 6.0 for tools that import it (typescript-eslint, Angular), while the tsc binary comes from 7.1. Pin the exact version either way, because the beta tag will move to the RC.

```
# Path A: already green on 7.0, no typescript-eslint type-aware rules
npm install -D --save-exact typescript@7.1.0-beta
# Path B: "typescript" stays 6.0 for tools, tsc comes from 7.1
npm install -D --save-exact "typescript@npm:@typescript/typescript6@6.0.2" "@typescript/native@npm:typescript@7.1.0-beta"
npx tsc -v     # Version 7.1.0-beta
npx tsc6 -v    # Version 6.0.3   (Path B only)
// package.json after Path B
"devDependencies": {
  "@typescript/native": "npm:typescript@7.1.0-beta",
  "typescript": "npm:@typescript/typescript6@6.0.2"
}
```
### What next build does with all this

Per the current Next.js docs, next build runs the project-local tsc CLI by default instead of the JavaScript compiler API (experimental.useTypeScriptCli), which is how it supports TypeScript 7. Diagnostics then come straight from tsc without Next.js code frames, the whole project selected by your tsconfig is checked, and typescript.tsconfigPath still picks the config. If you set useTypeScriptCli to false while on TypeScript 7, the build exits because the API it needs isn’t there. The docs don’t say which binary wins in an alias layout like Path B, so I run tsc --noEmit as its own CI step and read the version line instead of assuming.

### Step 3 — Scripts and a CI job that can’t block you

Run the beta as a separate, allowed-to-fail job until 7.1 is stable, and print the version first so the log proves which compiler ran. Microsoft suggests fewer checkers on machines with fewer cores and less memory, such as CI runners; match --checkers to yours.

```
// package.json
"scripts": {
  "typecheck": "tsc --noEmit",
  "typecheck:6": "tsc6 --noEmit",
  "bench:tsc": "node scripts/bench-tsc.mjs 5 tsconfig.json"
}
# .github/workflows/ci.yml — an extra job next to your normal build
  typecheck-ts71-beta:
    runs-on: ubuntu-latest
    continue-on-error: true          # informative until 7.1 is stable
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-node@v5
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npx tsc -v
      - run: npx tsc --noEmit --checkers 2
```
### Step 4 — Measure your own speedup

Microsoft’s tables are for their repos. This script runs each installed compiler by explicit path, never through node_modules/.bin, discards a warm-up run, disables incremental builds (Next.js tsconfigs ship incremental: true, which would hand later runs a cache) and prints the median wall time plus the memory each compiler reports. Add "ts70": "npm:typescript@7.0.2" temporarily if you want the 7.0 row too, then remove it. On my two-file sandbox the gap was mostly process start-up (about 0.9 s for 6.0 against 0.27 s for 7.x), which tells you nothing: run it on your real repo, and once more with CHECKERS=2.

```
// scripts/bench-tsc.mjs  ·  usage: node scripts/bench-tsc.mjs [runs] [tsconfig]
import { spawnSync } from "node:child_process";
import { existsSync } from "node:fs";
const RUNS = Number(process.argv[2] ?? 5);
const PROJECT = process.argv[3] ?? "tsconfig.json";
const CHECKERS = process.env.CHECKERS; // e.g. CHECKERS=2 to mimic a small CI runner
// Explicit paths: never trust whatever node_modules/.bin/tsc points at today.
const compilers = [
  { label: "6.0", bin: "node_modules/typescript/bin/tsc6" },
  { label: "7.0", bin: "node_modules/ts70/bin/tsc" },
  { label: "7.1-beta", bin: "node_modules/@typescript/native/bin/tsc" },
].filter((c) => existsSync(c.bin));
const median = (xs) => [...xs].sort((a, b) => a - b)[Math.floor(xs.length / 2)];
for (const { label, bin } of compilers) {
  const args = [bin, "-p", PROJECT, "--noEmit", "--incremental", "false", "--extendedDiagnostics"];
  if (CHECKERS && label !== "6.0") args.push("--checkers", CHECKERS);
  const version = spawnSync(process.execPath, [bin, "-v"], { encoding: "utf8" }).stdout.trim();
  const wall = [];
  const mem = [];
  let exit = 0;
  for (let i = 0; i <= RUNS; i++) {
    const t0 = performance.now();
    const r = spawnSync(process.execPath, args, { encoding: "utf8", maxBuffer: 256 * 1024 * 1024 });
    const ms = performance.now() - t0;
    exit = r.status;
    if (i === 0) continue; // warm-up run: fills the OS file cache, not counted
    wall.push(ms);
    const m = /Memory used:\s+(\d+)K/.exec(r.stdout);
    if (m) mem.push(Number(m[1]) / 1024);
  }
  console.log(
    label.padEnd(9),
    version.padEnd(20),
    "median " + (median(wall) / 1000).toFixed(2) + " s",
    "| memory " + (mem.length ? median(mem).toFixed(0) + " MiB" : "n/a"),
    "| exit " + exit,
  );
}
// Path A: the default path above points at node_modules/typescript/bin/tsc instead.
```
### Step 5 — Fix breaking change #1: distributive type parameters

This is Microsoft’s example. It compiles on 6.0 and 7.0 and fails on 7.1 with exactly the error below; I reproduced it. The three fixes compile on all three compilers, but they don’t mean the same thing, so choose by intent and pin the result with a type test.

```
type Show<A extends number, B extends A> = `${B} extends ${A}`;
type Issue<A extends number, B extends A> =
    A extends B ? never : Show<A, B>;
// 7.1: error TS2344: Type 'B' does not satisfy the constraint 'A'.
//   'B' is only assignable to the non-distributed 'A', but 'A' has been distributed here.
// Fix A: re-check the relation inside the branch (unsound combinations become never)
type FixA<A extends number, B extends A> =
    A extends B ? never : B extends A ? Show<A, B> : never;
// Fix B: drop a constraint the helper never needed (keeps the 7.0 result)
type ShowLoose<A extends number, B extends number> = `${B} extends ${A}`;
type FixB<A extends number, B extends A> =
    A extends B ? never : ShowLoose<A, B>;
// Fix C: stop distributing, if you meant the whole union (the result changes)
type FixC<A extends number, B extends A> =
    [A] extends [B] ? never : Show<A, B>;
// FixA<0 | 1, 0> = never
// FixB<0 | 1, 0> = "0 extends 1"
// FixC<0 | 1, 0> = "0 extends 0" | "0 extends 1"
```
### Step 6 — Fix the config and lib errors

If you jump from 6.x, the options 6.0 deprecated are hard errors on every 7.x, and older Next.js tsconfigs often carry both baseUrl and moduleResolution: node. paths resolve relative to the tsconfig, so dropping baseUrl usually needs no other change; with those two lines fixed, my sandbox config passed on 6.0, 7.0 and 7.1, and the plugins entry for Next’s editor plugin was harmless on the CLI. On 7.x, types defaults to [], so if you rely on globals such as jest, list their @types packages explicitly. For the regenerated DOM types, Microsoft points to issue #63702 instead of listing every change: diff typecheck-before.txt against the new output and fix the DOM-heavy call sites one by one rather than pinning an old lib.

```
// What 7.1 printed for a legacy Next.js tsconfig in my sandbox
// error TS5108: Option 'moduleResolution=node10' has been removed. Please remove it from your configuration.
// error TS5102: Option 'baseUrl' has been removed. Please remove it from your configuration.
//   Use '"paths": {"*": ["./*"]}' instead.
// tsconfig.json
{
  "compilerOptions": {
-   "moduleResolution": "node",
+   "moduleResolution": "bundler",
-   "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] },
+   "types": ["node", "jest"],       // only if you rely on these globals
    "plugins": [{ "name": "next" }]
  }
}
```
### Step 7 — Try the new API: type-check generated code in memory

The API is the headline, and it isn’t only for framework authors. This script type-checks a snippet — say, code an LLM just generated — with strict settings on a virtual file system, without writing anything to disk. It ran on Node 22 with Path B, hence the @typescript/native imports; on Path A, import from typescript/sync, typescript/fs and typescript/ast. Note the new shape: diagnostics expose fileName, code and text, not 6.0’s file and messageText.

```
// check-snippet.mjs: type-check generated code in memory with the TypeScript 7.1 API
import { API } from "@typescript/native/sync";
import { createFileSystemWithLib } from "@typescript/native/fs";
import { ScriptTarget } from "@typescript/native/ast";
import { resolve } from "node:path";
export function typecheck(code) {
  const fileName = resolve("virtual/generated.ts"); // never written to disk
  const api = new API();
  const snapshot = api.createSnapshot({
    fileSystem: createFileSystemWithLib([[fileName, code]]),
    createPrograms: [
      { rootFiles: [fileName], compilerOptions: { strict: true, noEmit: true, target: ScriptTarget.ES2022 } },
    ],
  });
  const program = snapshot.operation.createdPrograms[0];
  try {
    return program.getSemanticDiagnostics().map((d) => "TS" + d.code + ": " + d.text);
  } finally {
    program.dispose(); // these objects live on the Go side: release them explicitly
    snapshot.dispose();
    api.close();
  }
}
console.log(typecheck('export const total = (xs: number[]): number => xs.reduce((a, b) => a + b, "0");'));
// [ "TS2322: Type 'string' is not assignable to type 'number'." ]
console.log(typecheck("export const ok = (xs: number[]): number => xs.length;"));
// []
```
### Golden rule for the beta

One compiler is the source of truth per job, and its version is printed in the log. Adopt 7.1-only syntax only when every tool that reads those files runs 7.1.

## 4. Ecosystem Status (26 October 2026) and the Decision Table

I checked npm metadata and the tracking issues today. A peer range says what a package accepts, not that it works, but a range that excludes 7.x means npm will complain and the tool will usually refuse to start.

### typescript-eslint

8.72.0 still declares typescript >=4.8.4 <6.1.0. Tracking issue #10940 is open; on 29 September a maintainer wrote that 7.1 support is “looking more and more stable”, with PR #12803 in internal review, and that they don’t expect it to be significantly faster than on TS6 at first. Keep Path B.

### Next.js

16.3 (3 August) added TypeScript 7 type checking to next build, and the current docs make the local tsc CLI the default; the latest stable today is 16.4.3. Next.js’s IDE plugin is a separate, editor-side TypeScript plugin, and Microsoft’s 7.0 post recommended TypeScript 7 where language server plugins aren’t required, so check that its hints still appear in your editor.

### Vue

@vue/content-mapper 4.0.0-alpha.0 landed on npm on 23 October (peer typescript >=7.1.0-beta). The Vue Language Tools v4 pull request (#6170) is still a draft: it replaces vue-tsc with tsc --runExternalCode and lists known regressions, such as renames that miss template shorthands and some go-to-definition targets.

### Svelte and Astro

Microsoft links content-mapper work for both (Svelte language-tools PR #3111, Astro PR #17835), but the released checkers still pin TypeScript 5 or 6: svelte-check 4.7.7 and @astrojs/check 0.9.12 declare ^5.0.0 || ^6.0.0.

### Angular

@angular/compiler-cli 22.2.3 requires typescript >=6.0 <6.1. Microsoft’s 7.0 advice still applies: tsc 7 on the command line for fast project-wide errors, 6.0 for editor support.

### VS Code

Per Microsoft, the TypeScript 7 extension becomes the default once installed and is toggled with the Enable/Disable TypeScript 7 Language Server commands. Issue #64565, still open, reports that it doesn’t detect a workspace typescript 7.x and runs its bundled compiler; a commenter says setting js/ts.tsdk.path to node_modules/typescript works around it. For the beta, trust CI over squiggles.

### Your editor and CI can disagree

With Path B, typescript-eslint and the 6.0 language service see one compiler and CI’s tsc sees another. That’s fine for a trial, and it’s exactly why the beta job stays non-blocking and why 7.1-only syntax waits until everything moves.

### Decision table: try the beta, wait for stable, or stay on 6.x

| Your situation | Verdict | Why | 
|---|---|---|
| Next.js or Node app, already green on 7.0, no type-aware lint | Try now (CI job) | Path A in a non-blocking job. Microsoft measured 1.2–1.9× faster checks than 7.0 and 16–50% less memory; your risk is the two behavioural changes. | 
| You build tools on the compiler (codemods, generators, LLM code checks) | Try now (branch) | This is the release the API lands in, and Microsoft is asking for feedback before the RC. Expect the surface to change. | 
| Large monorepo with heavy type-level code | Try now (CI job) | The distributive change hits type-level helpers first. Better to find those errors while the beta still takes bug reports. | 
| typescript-eslint type-aware rules in the pipeline | Wait for stable | Its peer range stops at <6.1.0. Use Path B for tsc only and move fully when typescript-eslint ships 7.1 support. | 
| Vue, Svelte, Astro or MDX | Wait for stable | Content mappers are the fix, but Vue’s is an alpha with known editor regressions and the Svelte and Astro checkers still pin 5–6. Experiment in CI only. | 
| Angular | Stay on 6.x | @angular/compiler-cli 22.2.3 requires typescript >=6.0 <6.1. tsc 7 can still run as a fast extra check. | 
| Still on 5.x, or on 6.x with ignoreDeprecations | Stay on 6.x | Clear the 6.0 deprecations first (baseUrl, moduleResolution node, target es5…). They’re hard errors on every 7.x, so 7.1 adds nothing until they’re gone. | 

## 5. Common Mistakes (Most of Them I Hit in the Sandbox)

Roughly in order of how much time they cost.

### 1. Trusting npx tsc

When several installed packages expose a tsc binary, node_modules/.bin/tsc is whichever got linked. In my sandbox, adding a third alias (7.0.2) silently flipped npx tsc from 7.1.0-beta to 7.0.2, and another layout resolved it to the 6.0.3 compiler that @typescript/typescript6 pulls in. Print tsc -v in CI and call binaries by path in scripts.

### 2. Installing the floating beta tag

typescript@beta moves when the RC ships, so the same install can give you a different compiler next month. Use --save-exact 7.1.0-beta and bump on purpose.

### 3. Writing 7.1-only syntax while older compilers read the files

6.0 and 7.0 can’t parse declare module "*" with { … } (TS1005) or import source, and they reject target es2026 (TS6046). With Path B, typescript-eslint and your editor are still on 6.0, so they break first.

### 4. Using import source with the es2026 target

es2026 is the stable lib; source phase imports live in esnext. With target es2026 I got TS2318: Cannot find global type 'AbstractModuleSource'. Use the esnext lib or target, with module esnext, nodenext or preserve.

### 5. Benchmarking a warm incremental cache

Next.js tsconfigs ship incremental: true, so a second run reads .tsbuildinfo and looks magically fast. Pass --incremental false, discard the first run, compare medians and use the same --checkers value on every machine.

### 6. Expecting faster linting

typescript-eslint’s maintainer expects its first TS 7 support to perform roughly on par with TS6. The win is in tsc and the editor, not necessarily in ESLint.

### 7. Porting 6.0 API code line by line

The 7.1 API is new: diagnostics carry fileName and text, it doesn’t load tsconfig.json or support --project, and API, Program and Snapshot objects must be disposed or they leak on the Go side.

### 8. Treating --runExternalCode as a harmless flag

Microsoft calls content mappers “effectively arbitrary executables”. Only enable the flag for mapper packages you’d trust with an install script, and pin their versions.

## Conclusion

TypeScript 7.1 is the release that turns 7.x into a complete toolchain instead of a fast tsc running next to a 6.0 one: the API and content mappers are what typescript-eslint, Vue, Svelte and Astro were waiting for. On top of that, Microsoft’s own numbers show another 1.2–1.9× over 7.0 with noticeably less memory.

It’s still a beta, and the ecosystem hasn’t crossed over yet. Install it side by side, pin it, run it in a CI job that can’t block you, measure on your own repo, and fix the distributive-type errors now, while reports can still shape the RC.

### 7.1 beta in one screen

Do now

- • Pin 7.1.0-beta side by side
- • Non-blocking CI job + tsc -v
- • Benchmark on your own repo

Fix

- • TS2344 distributive constraints
- • baseUrl and node10 removals
- • DOM lib diffs

Wait

- • typescript-eslint 7.1 support
- • Stable Vue and Svelte mappers
- • 7.1-only syntax in shared files
