---
id: collect-261121-veille-ia/langages-dev/typescript-7-1-beta-5-changes-to-test-before-the-rc
title: "TypeScript 7.1 Beta: 5 Changes to Test Before the RC"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "typescript", "beta", "extends", "error", "import", "text", "notes"]
source: https://certificates.dev/blog/typescript-71-beta-5-changes-to-test-before-the-rc
source_anchor: ""
source_lines: [1, 188]
section: "Langages_Dev"
---
TypeScript 7.1 Beta restores a compiler API, adds typed import attributes and ES2026 types, tightens conditional types, and improves speed and memory use; test for breaking changes in CI before upgrading.

Martin Ferret

*November 18, 2026*

TypeScript 7.0 made `tsc` fast. It also left a hole: no API.

So typescript-eslint, Vue, Svelte and friends stayed on 6.0. TypeScript 7.1 Beta, released on October 20, 2026, is the release that starts closing that hole. It also adds one new type error that can break a green build.

Every snippet below was compiled with `tsc --strict` and run with Node 24.22.1 on November 2, 2026, using `typescript@7.1.0-beta` unless another version is named. The outputs are pasted as they came out.

TypeScript 6.0 shipped on March 23, 2026, as the last release on the JavaScript codebase. The latest 6.x on npm is 6.0.3. TypeScript 7.0, the native port written in Go, shipped on July 8, 2026, and `latest` on npm is now 7.0.2.

The 7.1 Beta installs with `npm install -D typescript@beta`. The iteration plan on GitHub targets the RC for November 10 and the stable release for November 24, 2026. The beta itself landed two weeks after its planned date, so treat those dates as targets, not promises.

A `declare module` block can now carry a `with { type: "..." }` clause. The type of an import depends on its attribute, not on its file extension.

Before, you wrote one wildcard per extension (`"*.md"`, `"*.txt"`), and nothing stopped an import that forgot the attribute. In 7.0.2, the new syntax does not even parse:

```
text.d.ts(1,20): error TS1005: ';' expected.
```
In 7.1, one declaration covers every text import:

```
// text.d.ts
declare module "*" with { type: "text" } {
  const content: string;
  export default content;
}
```
```
// main.ts
import notes from "./notes.md" with { type: "text" };
const lines = notes.trim().split("\n");
console.log(`${lines.length} lines, first: ${lines[0]}`);
```
Run with `node --experimental-import-text`, it prints:

```
3 lines, first: # Release notes
```
And the same import without the attribute now fails:

```
bad.ts(1,19): error TS2307: Cannot find module './notes.md'
or its corresponding type declarations.
```
The runtime always cared about the attribute. Now the types do too.

Two limits. TypeScript does not check that the file exists, and it does not ship these declarations: you write them yourself. And on Node 24, text imports still need the experimental flag.

This is the one that can turn a green build red.

Inside a distributive conditional type, the checked type parameter now counts as a narrower, new type. Other parameters constrained to the original no longer relate to it automatically.

This compiles without a single error in 7.0.2:

```
type Show<A extends number, B extends A> =
  `${B} extends ${A}`;
type Issue<A extends number, B extends A> =
  A extends B ? never : Show<A, B>;
const uhOh: Issue<0 | 1, 0> = "0 extends 1";
console.log(uhOh);
```
It prints `0 extends 1`, exactly what the `Show` constraint was meant to forbid. The type lies, and 7.0 lets it.

7.1 Beta rejects it:

```
issue.ts(5,33): error TS2344: Type 'B' does not satisfy the
constraint 'A'.
  'B' is only assignable to the non-distributed 'A', but 'A'
  has been distributed here.
```
One fix I tested is turning distribution off with tuple brackets, `[A] extends [B]`. It compiles under both versions and prints the same `0 extends 1`. The release notes also suggest removing the constraint if you do not need it, or adding extra conditional checks.

Only reach for tuple brackets if you do not need distribution. Otherwise you change what the type means.

`-target es2026` arrives, but only as types
`es2026` is now a stable `--target` and `--lib` value. It types methods like `Math.sumPrecise`, `Error.isError` and `Map.prototype.getOrInsert`.

7.0.2 rejects the value outright:

```
error TS6046: Argument for '--target' option must be: 'es6',
... 'es2024', 'es2025', 'esnext'.
```
In 7.1, this probe compiles cleanly:

```
const has = (name: string, value: unknown) =>
  console.log(name.padEnd(20), typeof value);
has("Error.isError", Error.isError);
has("Math.sumPrecise", Math.sumPrecise);
has("Map#getOrInsert", Map.prototype.getOrInsert);
has("Uint8Array.fromHex", Uint8Array.fromHex);
has("Array.fromAsync", Array.fromAsync);
```
And on Node 24.22.1, it prints:

```
Error.isError        function
Math.sumPrecise      undefined
Map#getOrInsert      undefined
Uint8Array.fromHex   undefined
Array.fromAsync      function
```
Three of the five methods do not exist at runtime.

`lib` describes what you promise the runtime has. TypeScript does not polyfill. In my first version of this probe, a call to `hits.getOrInsert()` compiled fine and then crashed with `TypeError: hits.getOrInsert is not a function`.

TypeScript 7.0 shipped with no API. The workaround was running 6.0 side by side through `@typescript/typescript6`, which is why so many tools never left 6.0.

7.1 exposes a new API, split into entry points such as `typescript/sync`, `typescript/async`, `typescript/fs` and `typescript/ast`, built around snapshots of the project state. This in-memory check runs against the native compiler:

```
import { API } from "typescript/sync";
import { createFileSystemWithLib } from "typescript/fs";
import { resolve } from "node:path";
const file = resolve("virtual.ts");
using api = new API();
using snap = api.createSnapshot({
  fileSystem: createFileSystemWithLib([
    [file, 'export const n: number = "42";'],
  ]),
  createPrograms: [
    { rootFiles: [file], compilerOptions: { strict: true } },
  ],
});
using program = snap.operation.createdPrograms[0];
for (const d of program.getSemanticDiagnostics()) {
  console.log(`TS${d.code}:`, d.text);
}
```
It prints:

```
TS2322: Type 'string' is not assignable to type 'number'.
```
Linters, codemods and framework tooling can finally start moving to the native compiler. 7.1 also adds content mappers, a plugin point meant for `.vue`, `.svelte`, `.astro` or `.mdx` files.

But it is a different API, not a port of the 6.0 one. My first try used `d.messageText`, and `tsc` rejected it: the field is `d.text`. Objects must be disposed, hence `using`, or they leak on the Go side. Content mappers also need the explicit `--runExternalCode` flag. The announcement said its example package, `@vue/content-mapper`, was not on npm yet. A `4.0.0-alpha.0` appeared there on October 23, 2026. I did not test it.

The team lists about 50 optimizations. On its own `--noEmit` benchmarks, 7.1 Beta runs 1.2x to 1.9x faster than 7.0 and uses 15.8% to 49.6% less memory.

I ran my own test on the `packages/zod` sources of the zod repo: 171 files, 93,887 lines including lib files. One warm-up run, then 10 timed runs per compiler, on a shared cloud VM with 8 vCPU and 15 GiB of RAM. All three compilers reported zero errors. Median results:

| Compiler | Wall time | Peak memory | 
|---|---|---|
| TypeScript 6.0.3 | 3.959 s | 563 MiB | 
| TypeScript 7.0.2 | 0.756 s | 344 MiB | 
| TypeScript 7.1.0-beta | 0.702 s | 279 MiB | 

Benchmark chart: TypeScript 6.0.3 vs 7.0.2 vs 7.1 Beta

7.1 Beta is about 7% faster than 7.0.2 and uses about 19% less memory. Against 6.0.3, it is about 5.6x faster, but most of that gain already came with 7.0. On memory-limited CI runners, the memory drop may matter more than the speed.

A sub-second build is too small to show the big gains Microsoft reports on huge codebases, and a shared VM adds noise: one 7.1 run took 1.280 s. Measure your own repo.

That is right for production builds. The API is new, the team is still tuning it with partner teams, and the RC dates are targets. If your build is already fast on 7.0, the speed gain alone does not justify any churn.

Where it is wrong: the distributive check is a breaking change, and a beta is the cheapest moment to find out whether it breaks you. One extra CI job ships nothing.

If you stayed on 6.0 for typescript-eslint, Vue, Svelte or a custom tool, try the API now and report gaps while they can still be fixed.

If your codebase has heavy conditional type utilities, add one CI job on `typescript@beta` today.

If you are on 7.0 with no plugins and a fast build, wait for the RC.

Run the beta in CI, not in production. Let it break there first.

`typescript` dist-tags: https://www.npmjs.com/package/typescript?activeTab=versions`@vue/content-mapper`: https://www.npmjs.com/package/@vue/content-mapper
Keep learning TypeScript. Then get certified when you are ready.

TypeScript 7.1 Beta: 5 Changes to Test Before the RC

Coordinating Optimistic Updates in React

Learn how to combine React’s useActionState, useOptimistic, and useTransition to implement ordered, optimistic UI updates for a channel‑sidebar layout, handling saves, rollbacks, and seamless user interaction.

Aurora Scharff

How to Make a Useful Open Source Contribution

Learn how to make useful open source contributions by identifying reproducible problems, testing and documenting fixes, preparing clear pull requests, and working constructively with maintainers through review.
