---
id: collect-261121-veille-ia/langages-dev/typescript-7-ships-10x-faster-but-it-breaks-eslint
title: "TypeScript 7 Ships: 10x Faster, But It Breaks ESLint"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "announcement", "compiler", "microsoft", "compile", "vscode", "faster", "native"]
source: https://muhittincamdali.com/en/blog/typescript-7-go-derleyici-gecis-karari/
source_anchor: ""
source_lines: [1, 166]
section: "Langages_Dev"
---
TypeScript 7.0 went GA on July 8, 2026, and its Go port promises compile speedups of up to 11.9x on large projects like vscode. But the speed isn't free: TypeScript 7.0 ships with no programmatic API, so tools like typescript-eslint that depend on the compiler's JavaScript API don't work right now. This article covers the TypeScript 7 breaking changes with sources, how to run TypeScript 6 alongside it, and which projects should upgrade today versus wait for 7.1.

💡 **Pro Tip:** Before upgrading, run `npm ls typescript` to see which of your dependencies reach directly into the compiler's programmatic API — typescript-eslint is the tool most commonly broken by this.

In its official announcement, Microsoft said, "Today we are proud to announce the availability of TypeScript 7, a 10x faster native port of TypeScript!", making TypeScript 7.0 generally available on July 8, 2026. The "10x" figure is the marketing headline; the actually measured speedup ranges from 7.7x to 11.9x depending on the project.

This is the first fully native port of the TypeScript compiler, moving it from JavaScript to Go. Per the announcement's own wording, Microsoft spent the past year testing TypeScript 7 on real codebases with its own teams (Loop, Office, PowerBI, Teams, Xbox) and with numerous external teams; companies like Bloomberg, Canva, Figma, Slack, and Vercel were part of that validation process.

That scale of validation is no accident: changing the compiler's architecture isn't just "the same job, done faster" — it's a foundational shift that forces the language server (LSP), editor integrations, and build tools to all be retested. That's why it makes more sense to treat TypeScript 7 not as a "patch" but as a platform migration the ecosystem adopts gradually.

The TypeScript 7 migration breaking-change list is short but its impact is large: default compiler settings got stricter, some targets were removed outright, and — most importantly — the compiler's programmatic API isn't in this release. Together, these three changes answer the "should I upgrade now or wait for 7.1" question differently for every project.

The native port was developed in Microsoft's `microsoft/typescript-go` repository. During development the command was named `tsgo`; from the TypeScript 7.0 RC onward the command name reverted to `tsc` — so you don't need to relearn anything for day-to-day use.

The measurement table in the official announcement shows compile times measured on real projects:

| Project | TS 6.x time | TS 7.0 native time | Speedup | 
|---|---|---|---|
| vscode | 125.7s | 10.6s | 11.9x | 
| sentry | 139.8s | 15.7s | 8.9x | 
| bluesky | 24.3s | 2.8s | 8.7x | 
| playwright | 12.8s | 1.47s | 8.7x | 
| tldraw | 11.2s | 1.46s | 7.7x | 

You can verify these numbers aren't made up yourself:

`1// Source: devblogs.microsoft.com/typescript/announcing-typescript-7-0/2const before = 125.7; // vscode, TS 6.x compile time (seconds)3const after = 10.6; // vscode, TS 7.0 native compile time (seconds)4console.log((before / after).toFixed(1)); // "11.9"`
When you compile vscode with 8 type-checking workers (the announcement's "experimental" `--checkers` flag, `--checkers 8`; the default is 4), the time drops to 7.51 seconds and the speedup climbs to 16.7x — extra headroom on multi-core machines, but at the cost of higher memory usage, per the announcement.

The practical takeaway from this table: the announcement says full builds "typically yield speedups between 8x and 12x," but in the `--checkers` section it also warns "results will differ across projects and underlying machines" — so don't assume your own project's ratio without measuring it. The announcement is explicit about where the gains come from: "TypeScript 7.0 now performs many steps in parallel, including parsing, type-checking, and emitting."

There's a fact more important than speed that most "10x speed" articles gloss over: Microsoft is blunt in the announcement — "While TypeScript 7.0 is here, it does not ship with an API. We expect TypeScript 7.1 to ship with a new (and different) API." Every tool that programmatically reaches into the compiler via `import * as ts from "typescript"` cannot run on TypeScript 7.0.

The most visible casualty is typescript-eslint. The library needs direct access to the compiler's JavaScript API for code analysis; that dependency stays unresolved until the new API ships in 7.1. Similarly, tool chains built on the compiler API — ts-jest and ts-morph among them — hit the same wall.

This blocker is on record: typescript-eslint#12518, opened July 8, 2026, shows `typescript-eslint@8.63.0` with `typescript@7.0.2` producing an `ERESOLVE` on `npm ci` and crashing during linting with `TypeError: Cannot read properties of undefined (reading 'Cjs')`. The issue was closed same-day as "duplicate" (GitHub reason: not planned); the maintainer's stated reason: "typescript-eslint isn't compatible with TS 7 at this time, because there is no TS 7 API at this time."

The same blocker applies in editors: tools like Volar, which embed TypeScript inside their own compiler, can only rely on the 6.0 API, so the announcement says Vue, MDX, Astro, and Svelte projects should stay on 6.0 for now; for Angular, the recommendation is TypeScript 7 in the CLI but 6.0 in the editor.

In practice this shows up as "I upgraded to TypeScript 7 but `npm run lint` no longer works" — the compiler itself isn't broken; the API surface `typescript-eslint` expects simply isn't there. It's important to keep this distinction clear: the problem isn't in your code, it's that the tool chain depends on a layer TypeScript 7.0 doesn't yet expose.

The Go port fully replaces TypeScript's old JavaScript-based compiler body. The programmatic API is a layer that must be built separately on top of this new Go core — Microsoft deliberately kept it out of 7.0's scope and deferred it to 7.1. So the answer to "when will this be fixed" isn't a bug fix; it's the next minor release.

TypeScript 7.0's `tsconfig.json` defaults got noticeably stricter compared to previous versions. The official announcement's "Updates Since 5.x, and New Behaviors from 6.0" section lists eight notable default changes under "At a glance, the notable default changes to configuration are"; the announcement calls the `rootDir` and `types` changes the most "surprising" of the bunch. The table below collects four of them:

| Setting | New default | To restore old behavior | 
|---|---|---|
| `strict` | `true` | explicitly set `strict: false` | 
| `module` | `esnext` | write `commonjs` /`nodenext` as needed | 
| `types` | `[]` (empty) | write `types: ["*"]` | 
| `rootDir` | `./` | point at your source folder with `"rootDir": "./src"` | 

There's an important nuance that's easy to miss here: TypeScript 7.0 didn't invent any of the four defaults in the table — all four arrived with 6.0, as the announcement itself states: "TypeScript 7.0 adopts 6.0's new defaults." What TypeScript 7.0 itself adds is in the second half of that same sentence: "and provides hard errors in the face of any flags and constructs deprecated in TypeScript 6.0" — meaning flags and constructs deprecated in 6.0 now trigger hard compile errors. So the sentence "strict became the default in 7.0" is technically wrong; the accurate version is "the strict default arrived with 6.0, and 7.0 inherited it."

The `types` field being `[]` specifically stops global type packages (like `@types/node`) from being implicitly pulled in on Node.js projects — you now need to either import them or add them explicitly to the `types` array.

This is the harshest part of the TypeScript 7 migration breaking-change list: some old target and module settings now cause a compile error, not a warning. The official announcement gives the full list of these hard errors (`downlevelIteration` and `moduleResolution: classic` are on it too). In practice, the first four you should check are:

- **`target: es5`** : no longer supported (per the TypeScript 6.0 release notes, the lowest target is ES2015).
- **`module: amd` / `umd` / `systemjs` / `none`** : all removed.
- **`moduleResolution: node` / `node10`** : removed; the recommended values are`nodenext` and`bundler` .
- **`baseUrl`** : removed; you need to write`paths` values relative to the project root.

A `tsconfig.json` that uses any of these items won't compile under TypeScript 7.0 — this should be your first checkpoint before migrating:

`1{2  "compilerOptions": {3    "target": "ES2020",4    "module": "NodeNext",5    "moduleResolution": "NodeNext",6    "strict": true,7    "types": ["node"]8  }9}`
On an older project, `grep -inE '"(target|module|moduleResolution|baseUrl)"[[:space:]]*:' tsconfig*.json` lists the lines for these four settings with their line numbers. Because the pattern only matches the key name, it doesn't matter whether the value is written `"ES5"` or `"es5"` — you still need to read each matched line and compare its value against the removed list.

Microsoft didn't leave you empty-handed against the missing API: it published an official compatibility package that re-exports TypeScript 6.0's programmatic API. The announcement says: "we've published a new compatibility package, `@typescript/typescript6`. This package provides an executable named `tsc6`... re-exports the TypeScript 6.0 API." That means you can run the old compiler via the `tsc6` command and the new native compiler via `tsc`, both on the same machine.

Installation is exactly the command from the official announcement:

`1npm install -D typescript@npm:@typescript/typescript6`
You can combine this package with npm aliases to keep both TS7's speed advantage and tools that need the API (like typescript-eslint) in the same project:

`1{2  "devDependencies": {3    "typescript": "npm:@typescript/typescript6@^6.0.2",4    "@typescript/native": "npm:typescript@^7.0.2"5  }6}`
In this setup, tools that import the `typescript` package (typescript-eslint included) see the TS 6.0 API; `npx tsc` runs the TypeScript 7 native compiler via `@typescript/native`, while `tsc6` runs the TS 6.0 compiler. There's no conflict because the npm alias installs the packages into different folders, and the compatibility package ships a separate `tsc6` binary.

The biggest advantage of this approach is that it doesn't require an irreversible commitment: removing `@typescript/typescript6` and switching `typescript` straight to `npm:typescript@^7.0.2` once 7.1's API ships is a one-line `package.json` change. So moving to a side-by-side setup today doesn't lock you into a full native migration later — quite the opposite, it makes the migration gradual and reversible.

Frameworks like Next.js typically ran type checking during builds by calling the compiler's programmatic API — since that API doesn't exist in TypeScript 7.0, as of this article's publication the sturdiest approach is to split the build from type checking; the `experimental.useTypeScriptCli` flag landed in Next.js canary on July 10 as opt-in at that time. In practice, two steps work: use `typescript.ignoreBuildErrors: true` in `next.config` to decouple the build from type errors, then run `tsc6 --noEmit` (or plain `tsc --noEmit` on projects that don't need the API) as a separate CI step to preserve type safety.

`1# CI step: build with the fast native compiler, type-check in a separate process2next build3npx tsc6 --noEmit`
This is a temporary but solid bridge you can apply today without waiting for the framework's own integration. The principle isn't specific to Next.js — it applies anywhere any build tool (a bundler, a monorepo task runner, a deploy pipeline) calls the compiler API for type checking: split the build step onto the native compiler for speed and the verification step onto the tool that needs the API, a general pattern that doesn't leave you waiting for official framework support. If you want to see this "split the build from the CI pipeline" pattern applied end-to-end in a serverless TypeScript project, check out our serverless production setup with Hono.js.

Let's pull everything above into one flow. Applying the TypeScript 7 migration to a Node.js/Next.js project in this order significantly cuts your risk of running into surprise breaking changes:

1. **Take inventory.** Scan`npm ls typescript` and your`tsconfig.json` files; make sure nothing still uses`target: es5` ,`module: amd/umd/systemjs/none` ,`moduleResolution: node/node10` , or`baseUrl` .
2. **Identify API dependencies.** Check whether you're using typescript-eslint, ts-jest, ts-morph, or any custom scripts that reach directly into the compiler's`Program` /`LanguageService` objects.
3. **If there are none, upgrade directly.** Install`typescript@^7.0.2` without needing`npm install -D typescript@npm:@typescript/typescript6` , and review your`strict` /`module` /`types` fields in`tsconfig.json` against the new defaults.
4. **If there are dependencies, bridge them.** Add`@typescript/typescript6` as the`typescript` alias and the native compiler as the`@typescript/native` alias; update your build scripts to make it clear which command calls which executable.
5. **Split your CI.** Run the build step with native`tsc` and type checking (for tools that need the API) with`tsc6` in separate steps; this keeps your speed gains while keeping the old tool chain alive.
6. **Watch for 7.1.** Periodically check the`next` tag with`npm view typescript dist-tags` ; once 7.1 is stable, you can drop the`@typescript/typescript6` alias and move straight to`typescript@^7.1.0` .

None of these six steps requires an irreversible commitment — your project's CI stays green at every step, because the speed gain and the API dependency are managed in separate layers.

To boil all this down into a single decision, we built a table by project profile. The logic is simple: if you have no dependency that needs the compiler's programmatic API, you can upgrade today; if you do, bridge with `@typescript/typescript6` or wait for 7.1.

| Project profile | Depends on the API? | Recommendation | 
|---|---|---|
| Compiles only with `tsc` , lints with a separate tool (e.g. Biome) | No | Upgrade today | 
| Uses typescript-eslint | Yes | Bridge with `@typescript/typescript6` , or wait for 7.1 | 
| Test or codegen infrastructure built on ts-jest / ts-morph | Yes | Bridge with `@typescript/typescript6` | 
| Edge/serverless runtime service that doesn't use an external compiler API | No | Upgrade today | 
| Monorepo that only uses native `tsc` for speed in CI, with type checking split out | No (if already split) | Upgrade today | 
| Project using Vue, MDX, Astro, or Svelte | Yes (via Volar) | Stay on TS 6.0 for now | 

For projects that have already moved their lint layer to a tool that doesn't depend on TypeScript's compiler API (as we covered in our ESLint vs Biome comparison, Biome uses its own parser), this migration blocker is already largely gone. We also examined a similar transition — where an ecosystem has to adapt to a rebuilt compiler core — in our piece on the Kotlin 2.1 K2 compiler.

### GOLDEN TIP

The most valuable insight in this article

This tip holds the article's most important takeaway.

### Easter Egg

You found a hidden gem!

There's a hidden detail in this section. Want to uncover it?

### Reader Reward

Before you start your TypeScript 7 migration, we rounded up every step covered in this article into a single checklist. Follow it in order and you won't miss either the breaking changes or the API dependencies.

If your project doesn't use tools that depend on TypeScript's programmatic API — typescript-eslint, ts-jest, ts-morph — and only compiles with `tsc`, then yes: TypeScript 7.0 is GA and has been tested on real codebases by many large teams, including Microsoft's own. If you rely on tools that need the compiler API, run it side by side with `@typescript/typescript6` until 7.1's API arrives. My own preference: try it first on a small, independent service package, observe the CI-time gain, then roll it out to the main monorepo — gradual beats migrating a whole codebase at once.

The compiler, rewritten in Go, doesn't include the old JavaScript-based API in this first release — Microsoft states "TypeScript 7.0 does not ship with an API" and has announced a different API for 7.1. That's why tools reaching directly into the compiler's JavaScript API, like typescript-eslint, can't currently run on TypeScript 7.

Yes. Microsoft's published `@typescript/typescript6` package re-exports the TS 6.0 API and ships a separate executable called `tsc6`; with npm aliases, you can keep both `tsc` (TS 7 speed) and tools that need the old API in the same project at once.

The `strict: true` default actually arrived with TypeScript 6.0; 7.0 just inherits it. What 7.0 itself adds is turning settings deprecated in 6.0 (e.g. `target: es5`) into hard compile errors. Projects without an explicit `strict: false` may hit new type errors on upgrade.

### `@typescript/typescript6`?

`tsc` calls the native TypeScript 7.0 compiler; `tsc6` is the TS 6.0-compatible executable from `@typescript/typescript6`. Point tools needing the compiler API (like typescript-eslint) at `tsc6`, and keep using `tsc` for speed. Defining two commands in `package.json`'s `scripts` — `"build": "tsc"` and `"typecheck:legacy": "tsc6 --noEmit"` — makes it clear to teammates which command calls which compiler.

Once 7.1's API ships stable, finishing the migration is simple: remove the `@typescript/typescript6` alias from `package.json` and switch `typescript` to `npm:typescript@^7.1.0`. Tools like typescript-eslint may need a version upgrade to support the 7.1 API — so after the 7.1 announcement, the safer sequence is to check their changelogs first, then drop the alias.

This article was published on July 17, 2026; from that date through today (September 5, 2026) the technical picture hasn't changed, but there are two concrete developments:

- **npm dist-tags and the 7.1 timeline:** On`registry.npmjs.org` ,`latest` is still`7.0.2` while`next` is`7.1.0-dev.20260905.1` — 7.1 is still in development. But there's now an official timeline: the TypeScript team's 7.1 Iteration Plan, published July 31, 2026, plans Beta for October 6, RC for November 10, and 7.1 Stable for November 24, 2026. So "should I wait for 7.1" no longer has an unknown answer — the planned API stabilization is tied to this schedule.
- **`useTypeScriptCli` became the default in Next.js:** The flag landed opt-in on July 10 (PR #95639) and became default on August 3 (PR #96497). Per Next.js docs,`next build` now calls the project's`tsc` command for type checking — "By default,`next build` runs the project-local`tsc` command instead of loading the TypeScript JavaScript compiler API. This supports TypeScript 6 and enables TypeScript 7 while its JavaScript API is unavailable." This is the framework-level version of our "split build from type checking" advice above; turn it off on TypeScript 7 and, per the docs, "`next build` exits because the TypeScript JavaScript compiler API is unavailable." The docs' own caveat still stands: "This feature is currently experimental and subject to change, it's not recommended for production."

The TypeScript 7 breaking-change table boils down to a simple decision: if you don't need the compiler's programmatic API, upgrade today; if you do, bridge with `@typescript/typescript6` or wait for 7.1. Check the default changes (`strict`, `module`, `types`, `rootDir`) and removed targets (`es5`, `amd`/`umd`/`systemjs`, `node`/`node10`, `baseUrl`) before you upgrade.

The key takeaway: the up-to-11.9x speedup is real and measured, but treating it as "free" and upgrading without checking your tsconfig and dependency chain can turn your CI from green to red. Treat the migration as a small engineering project following the six-step checklist above, and you capture the speed gain today without breaking your team's tool chain.

To decouple your lint layer from the compiler API, see our ESLint vs Biome comparison. For pairing TypeScript's TS 6 compatibility package with a modern edge runtime, see Supabase Edge Functions with the Deno runtime; for similar type-safety discipline at the ORM layer, see Drizzle ORM and Turso SQLite. The Swift Testing migration guide covers a similar "run side by side, then cut over" strategy for a major compiler migration in test infrastructure, and our Expo 52 EAS Build production guide is a useful reference for managing this kind of split in a build pipeline.

- Official TypeScript 7.0 announcement — primary source for the GA date, speed tables, missing API, and the `@typescript/typescript6` compatibility package
- npm typescript dist-tags (live query) — real-time status of the `latest` /`next` version tags
- TypeScript 7.1 Iteration Plan (microsoft/TypeScript#63703) — official timeline with Beta/RC/Stable dates and the API stabilization target
- microsoft/typescript-go GitHub repository — the native port's development repo and the official record of the `tsgo` →`tsc` command transition
- typescript-eslint Issue #12518 — the record closed the same day as "duplicate"; contains the `ERESOLVE` , the`Cjs` crash, and the maintainer's reasoning
- TypeScript 6.0 release notes — the official record that the `strict: true` default actually arrived in 6.0
- Next.js useTypeScriptCli documentation — the official explanation of calling `tsc` during builds
- loke.dev: Running TypeScript 7 and typescript-eslint side by side — published August 2, 2026, a practical setup example with verified package versions

### Tags

### iOS Development News

Weekly Swift tips, SwiftUI tricks and iOS best practices. No spam, only valuable content.

Your subscription starts when you open the link in the confirmation email and press “Confirm my subscription”. The newsletter keeps open/click statistics; you can unsubscribe anytime with one click. Privacy
