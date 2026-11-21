---
id: collect-261121-veille-ia/langages-dev/typescript-7-1-beta-the-api-is-back-and-vue-svelte-and-angular-can-follow-aliz-web-hub
title: "TypeScript 7.1 Beta: The API Is Back, and Vue, Svelte, and Angular Can Follow | Aliz Web Hub"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "beta", "stable", "svelte", "back", "file", "mapper", "point"]
source: https://web-hub.aliz.ai/blog/typescript-7-1-beta
source_anchor: ""
source_lines: [1, 138]
section: "Langages_Dev"
---
TypeScript 7.1 Beta landed on October 20, 2026. When TypeScript 7.0 shipped in July, the Go-based compiler was fast but had one big gap: there was no programmatic API. 7.1 closes it. It brings a new, stable-track compiler API, a plugin point called **content mappers** for non-TypeScript files like `.vue` components, and another big round of performance work. It also adds a few language and lib features. This post covers what changed, what it unblocks, and whether you should try the beta now.

## Try it

```
npm install -D typescript@beta
npx tsc --version
```
The release candidate and stable dates are tracked in the 7.1 iteration plan. The plan originally put the beta on October 6, RC on November 10, and stable on November 24. The beta ended up two weeks late, so treat the later dates as tentative.

## The compiler API returns

Tools that need more than `tsc` on the command line have been stuck on TypeScript 6 since July. That includes Vue's Volar, Svelte, Astro, Angular templates, ts-morph, and custom transformers. 7.1 is the release meant to fix that.

The old monolithic `typescript` module, with `ts.createProgram()` and friends, is not coming back in the same shape. The new API is split into focused entry points:

| Entry point | Purpose | 
|---|---|
| `typescript/sync` | Synchronous API client | 
| `typescript/async` | Promise-based version of the same client | 
| `typescript/fs` | Request-scoped file systems | 
| `typescript/ast` (plus`is` and`factory` submodules) | Syntax tree types, type guards, and node factories | 

```
import { API } from "typescript/sync";
```
These paths used to live under `typescript/unstable/*`. The prefix was dropped for the beta, and that signals intent. According to the API status issue, the stability promise works like this:

- After 7.1 stable, the API stays **backward compatible across all of 7.x** for consumers that use the provided client.
- The client/server protocol underneath is an implementation detail. It can change in minor releases, so don't talk to it directly.
- Breaking changes are still possible between beta and stable.
- Watch mode, incremental, and build orchestration APIs are **not** part of 7.1. They may come later.
- Some utility functions and checker methods from TS 6's `typescript.d.ts` are still missing. The team is mapping old functions to the new API to decide what gets added before stable.

Full API documentation is planned for 7.1 stable. It will focus on the differences from the TS 6 API. Until then, expect tool authors to be the early adopters, not application teams.

## Content mappers: a first-class answer for `.vue`, `.svelte`, and friends

Framework tooling has always type-checked custom file formats with a workaround. The language server is patched or wrapped so that TypeScript sees a generated `.ts` view of each component. Content mappers turn that into an official plugin point.

A content mapper is registered for one or more file extensions. When TypeScript meets such a file, it asks the mapper for a **virtual TypeScript representation**. The mapper also returns mappings back to the original source, so error spans and editor features point at the right place in the `.vue` file, not in generated code.

Mappers are configured in `tsconfig.json`:

```
{
  "contentMappers": [
    {
      "package": "@vue/content-mapper",
      "extensions": [".vue"]
    }
  ],
  "include": ["src"]
}
```
The announcement uses `@vue/content-mapper` as a hypothetical example. That package is not on npm yet. Community SDKs such as ts-content-mapper already let you write and test your own mapper.

A mapper runs as a separate process. That means it is arbitrary code, so TypeScript won't start it unless you opt in:

```
npx tsc --noEmit --runExternalCode
```
`--runExternalCode` runs whatever package your `tsconfig.json` names. Treat it like adding a build plugin: pin the version and review the dependency. In the editor, the same opt-in goes through workspace trust instead of a CLI flag.

The content mapper protocol only changes in backward-compatible ways during 7.x. That gives framework authors a stable base to build on. The beta still has rough edges, though. For example, open issues report that mapped extensions are not probed for extensionless imports in `bundler` resolution (#64546), and that `moduleSuffixes` resolves to the wrong file (#64548).

## Performance: another big step

7.0 was already 8–12x faster than TS 6. 7.1 adds optimizations aimed mostly at memory pressure. The iteration plan lists faster union type construction, lazily collected source file identifiers, fast paths for narrowing on unions, and leaner checker side-tables.

The headline benchmark from the announcement is the MUI docs codebase, type-checked with `--noEmit` and 4 checker threads:

| Metric | TS 7.0 | TS 7.1 Beta | Change | 
|---|---|---|---|
| Type-check time | 22.2 s | 11.9 s | ~1.9x faster | 
| Memory | 8,093 MiB | 4,079 MiB | ~50% less | 

Not every project gains that much. Codebases like VS Code and Sentry see closer to 1.2–1.3x. The more union-heavy your types, the more you are likely to see. Still, halving memory matters a lot for CI runners and large monorepos that came close to out-of-memory crashes on 7.0.

## Language and lib updates

### Import attributes in ambient module declarations

You can now scope a wildcard `declare module` to a specific import attribute. This lets TypeScript give the right type to CSS module scripts, text imports, and similar non-JS imports:

```
declare module "*" with { type: "css" } {
  const sheet: CSSStyleSheet;
  export default sheet;
}
declare module "*" with { type: "text" } {
  const content: string;
  export default content;
}
```
```
import styles from "./button.css" with { type: "css" };
import template from "./banner.txt" with { type: "text" };
document.adoptedStyleSheets = [styles];
```
How it works (PR #63931):

- An import matches when its attributes are assignable to the declaration's attributes type. If several declarations match, the most specific one wins.
- If an import has attributes, matching pattern declarations take priority over normal file resolution. Imports without attributes behave exactly as before.
- Attribute values must be string literal types.
- TypeScript does not ship these declarations in `lib.d.ts` . You write them yourself, or a framework or bundler provides them.

### Source phase imports

TypeScript now understands the TC39 source phase imports proposal. A `source` import gives you a compiled but not-yet-linked module instead of an instance. The main use case today is WebAssembly:

```
import source mathModule from "./math.wasm";
const instance = await WebAssembly.instantiate(mathModule, {});
```
The dynamic form, `import.source("./math.wasm")`, is supported too. The binding is typed as the new global `AbstractModuleSource` type. Source imports require `--module` set to `esnext`, `nodenext`, or `preserve`.

### `es2026` target and lib

`es2026` is now a stable value for both `target` and `lib`. It brings declarations for ES2026 additions, such as `Math.sumPrecise()`, `Error.isError()`, and the `Uint8Array` base64/hex helpers. 7.1 also adds declarations for in-progress proposals, including `Promise.allKeyed` and `Iterator.zip`. Check our ECMAScript Versions reference for what each edition adds.

### Watch for DOM lib changes

The 7.1 iteration plan includes a `lib.d.ts` refresh for DOM types. Lib updates like this can surface new type errors without any change to your code. If the beta reports errors in code that only touches browser APIs, check the DOM types first.

## What it means for your projects

- **Vue, Svelte, Astro, and Angular projects:** Keep the TS 6 + TS 7 dual install for now. 7.1 gives framework teams what they need, but you can drop TS 6 only after your framework's tooling ships 7.1 support, such as a content mapper or a new language-tools release.
- **Tool and library authors:** Start porting to`typescript/sync` or`typescript/async` now, and report missing APIs during the beta. This is the window where gaps can still be filled before stable.
- **Plain TypeScript apps already on 7.0:** Try the beta in a CI branch. A minor version bump with a possible 2x memory cut is worth one pipeline run. Do not ship to production until 7.1 stable.
- **Teams still on TS 5:** Nothing changes. The path is still TS 5 → TS 6 → TS 7, and the 7.0 post covers it.

**Further reading:**

- Announcing TypeScript 7.1 Beta — the official announcement
- TypeScript 7.1 Iteration Plan — schedule and full work item list
- API status as of 7.1 beta — stability and versioning plan for the new API
- TypeScript 7.0: The Go Rewrite Lands — our earlier post on the 7.0 release and migration
