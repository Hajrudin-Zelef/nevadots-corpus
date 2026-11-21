---
id: collect-261121-veille-ia/langages-dev/typescript-7-1-beta-what-s-new-and-what-to-test-first
title: "TypeScript 7.1 Beta: What's New and What to Test First"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "import", "typescript", "beta", "module", "ambient", "imports", "attribute"]
source: https://www.digitalapplied.com/blog/typescript-7-1-beta-whats-new-what-to-test
source_anchor: ""
source_lines: [1, 106]
section: "Langages_Dev"
---
On October 20, 2026 Daniel Rosenwasser of the TypeScript team announced the TypeScript 7.1 beta. It installs with one command and changes six things that matter to a working team: how ambient modules match imports, a new import form for WebAssembly, a stable es2026 target, a new programmatic API, a plugin point called content mappers, and a batch of compiler optimisations Microsoft measured on five public codebases. The announcement also lists two behavioural changes that can add errors to a build that passes today.

This is a point release on the native compiler, not a second migration. If your project is already on 7.0 the upgrade should be dull, and the purpose of a beta is to make sure of that before the release candidate. Microsoft says the candidate will follow in about a month, with the final release shortly after; it gives no dates. The migration itself is covered in our TypeScript 7.0 migration playbook. This post is about what is new in 7.1 and what to run against the beta this week.

1. 01Ambient modules can now match on import attributes.A declaration like declare module "*" with { type: "css" } gives the right type to any import carrying that attribute and errors on an import that forgets it. Microsoft asks libraries not to ship these declarations; projects declare them locally.
2. 02Source phase imports arrive for WebAssembly.import source X from "./x.wasm" returns a WebAssembly.Module you can pass straight to WebAssembly.instantiate. It needs an esnext, nodenext or preserve module setting and the esnext lib. JavaScript source imports exist in the proposal but do little yet.
3. 03es2026 is a stable target, and esnext gained new methods.Math.sumPrecise, Iterator.concat, Error.isError, Map and WeakMap getOrInsert, the Uint8Array base64 and hex methods and Array.fromAsync are now in es2026, some moved from esnext and some newly added. Promise.allKeyed, Iterator.zip, chunks, join and includes land in esnext.
4. 04A new API with snapshots, virtual file systems and content mappers.Six entry points replace the monolithic API. Snapshots derive state from earlier state instead of rebuilding; API, Program and Snapshot objects are Disposable and should be declared with using. Content mappers let Vue, Svelte, Astro and MDX files be checked by tsc behind --runExternalCode.
5. 05Two changes can add errors: DOM typings and distributive type parameters.Updated lib.d.ts DOM types may change type-checking results, and conditional types that distribute over a union now treat the distributed parameter as unrelated to other parameters constrained to the original. Code that leaned on that relation will need a constraint removed or a check added.

## 01 — ModulesImport attributes in *ambient* modules.

Import attributes are the metadata after the with keyword on an import, such as type: "json". Runtimes use them to decide how to load a file, because a browser cannot infer from an extension alone that a file is JSON, text, raw bytes or a stylesheet. Proposals for type: "text" and type: "bytes" exist alongside the json and css types browsers already handle.

Until now a TypeScript project declared the type of such imports by file extension, with a wildcard ambient module per extension. The announcement names three problems with that. It lets a developer forget the attribute without an error. It repeats a declaration for every extension that shares a type. And it is wrong whenever the runtime's behaviour depends on the attribute rather than the extension, such as a bundler that treats .css as CSS Modules by default but as a CSSStyleSheet when type: "css" is present.

TypeScript 7.1 lets an ambient module declaration carry the attribute itself: declare module "*" with { type: "css" } and so on. An import that matches gets the declared type, and an import that omits the required attribute is an error. Two limits are stated. Matching imports are not checked against files on disk, so a missing file still fails at runtime or bundle time, as ambient matches always have. And the declarations are not shipped in the built-in libraries; Microsoft recommends that packages leave them out and that end users declare them per project, so that the global scope stays under the project's control and future built-in declarations do not conflict.

## 02 — WebAssemblySource phase imports, mostly for *.wasm* today.

Source phase imports are a proposed ECMAScript feature with a new source modifier after import. Only default imports can use it. The value you get back represents the module after the runtime has parsed and analysed it but before its own imports are loaded or any of its code has run. The announcement calls the object inert: an AbstractModuleSource that is meant to be instantiated or imported later.

The useful case now is WebAssembly. A source import of a .wasm file yields a WebAssembly.Module, which can go straight into WebAssembly.instantiate with an import object, such as a WASI instance in Node. Recent Node versions already support this at runtime. A dynamic form, import.source(), does the same thing at call time, and source imports keep identity, so two imports of the same file compare equal and can be used as Map or Set keys.

For JavaScript files the feature is mostly a placeholder: the proposal implies a module source could one day be passed to import(), but the announcement says many environments do not yet support JavaScript source imports at all. The type AbstractModuleSource is new in lib.esnext.d.ts, and the feature requires a module setting of esnext, nodenext or preserve plus the esnext lib or target. The failure case is a team that adopts import source for .wasm under a commonjs or es2022 module setting. The beta names the cause, saying source phase imports need a module setting of esnext, nodenext or preserve, but changing an existing project's module setting is a larger change than the import it enables.

## 03 — TargetsA stable *es2026* target and new esnext methods.

es2026 is now a stable value for --target and --lib. The announcement says only the lib side is significant: declarations have been added or moved out of esnext as the underlying features stabilised in ECMAScript 2026. The practical effect is that a project which pinned lib: esnext to get these methods can pin a fixed year instead and stop absorbing whatever esnext adds next.

The methods now in es2026 and the proposals now in esnext are listed below, as the announcement lists them. The esnext set is still proposal-stage; a project should not ship code that depends on those methods without a runtime that implements them or a polyfill.

##### Number, JSON and Error helpers

The JSON.parse overload exposes the original source text during parsing. Error.isError moves over from esnext; the Math.sumPrecise and JSON declarations are newly added in this lib.

##### Collections and iterators

getOrInsert removes the has-then-set dance for maps. Iterator.concat joins iterators without spreading them into arrays.

##### Uint8Array encoding

Native base64 and hex conversion on typed arrays, which removes a common small dependency in browser and edge code.

##### Promise and Iterator proposals

Proposal-stage. Typed so you can write against them, but check your runtime before relying on any of them in production.

## 04 — ToolingThe new compiler API: *snapshots* and virtual files.

This is the section that matters most to anyone who builds tooling, including the editors and AI coding assistants that call the compiler many times a second. The 7.0 compiler is written in Go, and the old monolithic JavaScript API did not survive the move. TypeScript 7.1 ships its replacement, organised as six entry points: typescript/sync for a synchronous client with compiler options, program and type-checker APIs; typescript/async for a Promise-based equivalent; typescript/fs for request-scoped and layered file systems; typescript/ast for node types and enums; and typescript/ast/is and typescript/ast/factory for type guards and create or update functions.

Two ideas carry the design. The first is disposal. API and Program instances hold memory on the Go side, so they cannot be left to the JavaScript garbage collector. They are Disposable objects: either call close() or dispose() explicitly, or declare them with the using keyword so they are released when they leave scope. A disposed program cannot be reused. The second is the snapshot. A snapshot is the state of every project and program at one moment, and snapshot.update() derives a new snapshot from an old one so that a single-file edit does not rebuild the world. Program and Project objects keep their ids across snapshots, ensurePrograms says which ones to refresh, and both old and new snapshots stay queryable, as long as you do not mix objects from one snapshot's checker with another's.

The file-system layer is what makes this usable without touching disk. createFileSystem defines a complete virtual file system; createFileSystemWithLib does the same and mounts the compiler's default library read-only; createFileSystemLayer overrides selected files and falls back to the base snapshot or the host. Combined with emitToString, a tool can type-check and emit an in-memory file without writing anything. File watching is not included: creations, deletions and changes go in through a fileNotifications option on update. The example in the announcement checks a file with a type error, layers a corrected version over it, and reads one diagnostic before and zero after. The failure case is a long-running tool that creates snapshots in a loop and never disposes them, which leaks on the Go side where the JavaScript heap profiler cannot see it.

## 05 — FrameworksContent mappers put Vue and Svelte files *inside tsc*.

Single-file component formats embed TypeScript inside HTML-like files: Vue, Svelte, Astro, Ember and MDX all do it. The Volar project has provided editor support for them by generating a virtual TypeScript view of each file and mapping errors back. The announcement is candid that this worked well in editors and badly everywhere else, because command-line type-checkers for those formats used different APIs and Volar had to either embed TypeScript entirely or work around its limits.

TypeScript 7.1 makes that model a first-class plugin point. A content mapper registers for a file extension, produces a virtual representation of the file, and supplies regions that map back to the original so that error spans land in the right place. It is configured in tsconfig.json under a contentMappers array with a package name, extensions and options, and runs whenever a matching file enters the program by include or by import. Mappers have been prototyped or implemented for Astro, CSS Modules, Ember, MDX, Svelte and Vue; the announcement notes that the Vue package name in its example is not yet on npm.

Content mappers are effectively arbitrary executables that TypeScript dispatches to, so the --runExternalCode flag makes that explicit.Daniel Rosenwasser, TypeScript 7.1 Beta announcement, October 20, 2026

That flag is the part to notice. Running tsc with a content mapper requires --runExternalCode, and in VS Code the feature needs both workspace trust and a content mapper permission, with a setting, js/ts.contentMappers.enabled, to switch it off. The reason is plain: a mapper is code from a package that the compiler will execute, which is a new supply-chain surface in a tool that previously only read files. The example to run is a Vue or Svelte project checked by the beta's tsc with a prototype mapper, to see whether the command-line errors match the editor's. The failure case is a CI pipeline that adds --runExternalCode globally for one project and thereby lets any future mapper dependency execute during every type-check.

## 06 — PerformanceWhat Microsoft *measured* on five codebases.

TypeScript 7.0 claimed roughly ten times the speed of TypeScript 6. The 7.1 beta adds several dozen smaller optimisations, listed by pull request in the announcement, aimed at memory pressure and build time: fast paths for narrowing on unions, denser symbol and type records, batched allocations, better program reuse after a single-file change, and a fix for a stall when setting up build mode on large solutions. Microsoft ran the beta against 7.0 on five public codebases with the default of four checker threads and --noEmit. Both tables are Microsoft's; the bars show the wall-clock reduction.

#### Type-check wall-clock reduction, TypeScript 7.0 → 7.1 beta

Source: Microsoft, TypeScript 7.1 Beta announcement, October 20, 2026. Vendor-run, --noEmit, four type-checker threads, TypeScript 7.0 versus 7.1 beta.
| Source: Microsoft, TypeScript 7.1 Beta announcement, October 20, 2026. Vendor-run memory usage under --noEmit. Read October 21, 2026. |  |  |  | 
|---|---|---|---|
| Codebase | TypeScript 7.0 | 7.1 beta | Memory reduction | 
|---|---|---|---|
| bluesky | 2,017 MiB | 1,698 MiB | 15.8% | 
| mui-docs | 8,093 MiB | 4,079 MiB | 49.6% | 
| sentry | 6,587 MiB | 5,216 MiB | 20.8% | 
| t3code-server | 6,894 MiB | 5,197 MiB | 24.6% | 
| vscode | 8,387 MiB | 6,943 MiB | 17.2% | 

The example that matters is mui-docs, where the memory halved and the time nearly did. That codebase is documentation-heavy with large generated types, which suggests the biggest wins land on projects with wide unions and many instantiations rather than on plain application code, where bluesky's 13% is the more realistic expectation. The failure case is reading 1.9x as the headline and planning CI capacity around it. Measure your own repository with the beta at four threads and --noEmit, which is the only configuration these numbers describe.

## 07 — BehaviourTwo changes that can turn a green build *red*.

The announcement's notable-changes section lists two items. The first is routine: the generated DOM typings in lib.d.ts have been updated, and the announcement says they may affect type-checking, with the specifics tracked in a linked issue on the TypeScript repository. Every TypeScript release carries this, and the fix is usually a type assertion or a removed one.

The second is subtler and worth understanding before the errors arrive. A conditional type that tests a type parameter, such as T extends X ? A : B, distributes over a union supplied for T: each member is checked separately and the results are joined. Inside each branch, T is then a narrower type than the original. Earlier versions did not distinguish the two, so code could pass the distributed T to another type whose constraint was written against the original parameter. The announcement's example is a helper that requires its second parameter to extend its first, called from the false branch of a distributive conditional with the distributed parameter; under 7.0 that checked, and under 7.1 it is an error reading that the type is only assignable to the non-distributed parameter, which has been distributed here.

The announcement's own analogy is the clearest statement of the rule: a Cat is a subtype of Animal but not of every kind of Animal. Code that hits the new error needs either the original constraint removed, if it was not load-bearing, or an extra conditional check added so the narrowed parameter satisfies the constraint it is passed to. The failure case is a shared types package that compiles under 7.0, is published, and breaks every consumer that upgrades to 7.1 first; this is the change to run across a monorepo before the release candidate rather than after.

## 08 — ChecklistWhat to *test* against the beta this week.

A beta is cheap to install on a branch and expensive to ignore until the release lands in a dependency update. The routing below is in order of how likely each item is to break a build, judged from the announcement alone. Projects still on TypeScript 6 have a different first step, covered in our early-adopter readiness guide.

One example of a result worth acting on: if the beta raises zero new errors across a monorepo, record that and move on, because the release candidate is the next point at which anything can change. The failure case is skipping the beta, taking the final release through an automated dependency bump, and discovering the distributive-type change in a Friday deploy. Teams on Next.js should also note that a framework upgrade is a separate decision, handled in our Next.js 15 to 16 migration playbook, and that our web development practice runs these upgrade passes for clients on a fixed scope.

## 09 — ConclusionA point release that is mostly *plumbing*, with two sharp edges.

### Install the beta on a branch, run tsc --noEmit across every package, and fix the distributive-type errors before the release candidate arrives.

TypeScript 7.1 beta, published October 20, 2026, is a tooling release more than a language one. Typed import attributes on ambient modules, source phase imports for WebAssembly and a stable es2026 target are small conveniences. The new compiler API, with its six entry points, disposable objects, snapshots and virtual file systems, and the content mapper plugin point behind --runExternalCode, are the pieces that editors, framework type-checkers and AI coding tools will be built on for the next several releases. Microsoft's own benchmarks show 1.2x to 1.9x faster type-checks and 16% to 50% less memory on five codebases, measured by Microsoft under one configuration.

The two behavioural changes are the reason to test now rather than later. Updated DOM typings and the stricter treatment of distributive type parameters can both add errors to code that passes under 7.0, and the second one propagates through published types packages to every consumer. Microsoft plans a release candidate in about a month and a final release shortly after, with no dates stated. A branch with typescript@beta and a clean tsc run is the whole of the preparation.
