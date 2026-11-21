---
id: collect-261121-veille-ia/langages-dev/typescript-7-in-2026-go-compiler-migration-guide
title: "TypeScript 7 in 2026: Go Compiler Migration Guide"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "compiler", "migration", "defaults", "guide", "type", "release", "types"]
source: https://mameara.com/typescript-7-native-compiler-2026/
source_anchor: ""
source_lines: [1, 167]
section: "Langages_Dev"
---
Sharing is caring!

My CI type-check used to be the part of the pipeline I scheduled around. Not because it failed often, but because it took long enough that I would start it, switch tasks, and come back with the mental stack half gone. Last week the same check finished before I could open the diff tab. That is not a benchmark chart talking. That is the day-to-day difference TypeScript 7 makes.

TypeScript 7.0 landed on July 8, 2026. It is the first stable release built on the Go port of the compiler — the project formerly known as `tsgo` — and Microsoft reports 8–12x speedups on full builds. But “the compiler got faster” undersells the release and oversells what you need to do. This guide is the migration I actually ran, including the parts that broke.

## What TypeScript 7 is, and what it is not

It is a native rewrite of the same type checker. The team ported the existing codebase to Go rather than redesigning the type system, so the diagnostics you get are intentionally identical to TypeScript 6.0. Your types did not become smarter or stricter in 7.0. They became cheaper to check.

It is not a runtime, and it does not remove your build step. A common misunderstanding in 2026: “TypeScript is native now, so Node runs my types.” No. Node’s built-in type stripping executes `.ts` files by erasing annotations without checking anything. The compiler checks everything and emits JavaScript. They are complementary, not replacements — more on that below.

It is also not a drop-in for every tool in your stack yet. TypeScript 7.0 shipped without a stable programmatic compiler API. That API is expected in 7.1. Until then, tooling that imports the compiler — parts of typescript-eslint, and framework integrations for Vue, Svelte, Angular and MDX — may still need the JavaScript-based compiler. Microsoft publishes `@typescript/typescript6` for exactly this: it provides a `tsc6` binary and re-exports the 6.0 API so you can run both side by side.

## The numbers, briefly

I do not trust vendor benchmarks as a planning tool, but the shape of these is useful:

| Workload | TypeScript 6.0 | TypeScript 7.0 | Change | 
|---|---|---|---|
| VS Code full build (2.3M LOC) | 125.7s | 10.6s | ~11.9x | 
| VS Code aggregate memory | baseline | ~18% lower | — | 
| Open file with an error (VS Code) | ~17.5s | under 1.3s | ~13x | 
| Slack CI type-check | ~7.5 min | ~1.25 min | ~6x | 

The honest takeaway is not “10x everywhere.” On a 40k-line app the wall-clock difference is seconds, not minutes. The bigger win is interactive: the language server feels instant, and type errors stop being a context switch. If you maintain a monorepo or a large design-system package, the CI savings are real money.

## Do not skip TypeScript 6.0

This is the single most important line in the release notes: TypeScript 7.0 is compatible with 6.0’s behaviour, and 6.0 is where the deprecations became real. Jumping from 5.x straight to 7 turns a boring upgrade into a debugging session.

TypeScript 6.0, released in March 2026, was the last version on the JavaScript codebase. It set the defaults that 7.0 inherits:

- `strict` is on by default.
- Long-deprecated compiler options are removed rather than warned about.
- The language moves toward modern module and target defaults instead of legacy compatibility modes.

Practical advice: upgrade to 6.0 first, fix everything, ship it, then move to 7.0. That is exactly what the TypeScript team recommends, and it is the only migration path I would sign off on for a production repo.

## The tsconfig diff nobody warns you about

TypeScript 7 changes two defaults that break real projects in quiet ways.

**`rootDir` now defaults to `./`.** If your sources live in `src/`, declaration output paths can shift. Be explicit:

```
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist"
  },
  "include": ["./src"]
}
```
**`types` now defaults to `[]`.** Anything that relied on automatic inclusion of global declaration packages — Node, Jest, Bun, Mocha — must be listed. This is the change that produces “Cannot find name `process`” on a codebase that compiled minutes earlier.

```
{
  "compilerOptions": {
    "types": ["node", "jest"]
  }
}
```
A sane updated config for a modern app looks like this. Note the explicit module settings and `verbatimModuleSyntax`, which I consider non-negotiable in 2026:

```
{
  "compilerOptions": {
    "target": "ES2023",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "erasableSyntaxOnly": true,
    "skipLibCheck": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "types": ["node"]
  },
  "include": ["./src"]
}
```
I keep `erasableSyntaxOnly` on even in projects that compile with `tsc`, because it keeps the door open for running the same files directly under Node during development. If a dependency forces `enum` or a parameter property, you will find out at the boundary instead of in a confusing runtime error.

## The new parallelism flags

The Go compiler can use shared-memory multithreading, which the old JavaScript implementation structurally could not. By default it picks a sensible number of threads; you can tune it:

```
# More parallel type checkers on a beefy CI box
npx tsc -p tsconfig.json --checkers 8
# Parallel project builds in a monorepo
npx tsc -b --builders 4
# Constrained containers: disable threading entirely
npx tsc -p tsconfig.json --singleThreaded
```
On our CI runners, `--checkers 8` cut the check from about 9 seconds to 4 on a 90k-line app. Diminishing returns show up fast; throwing 32 checkers at a small repo mostly adds coordination overhead. Measure before you pin a number in the pipeline.

## Keeping the old compiler around without pain

You will almost certainly have one tool that is not ready. Install the compatibility package and keep it out of the main path:

```
npm i -D typescript @typescript/typescript6
# Native compiler
npx tsc -p tsconfig.json
# Legacy compiler when a tool needs the 6.0 API
npx tsc6 -p tsconfig.tools.json
```
Name the scripts honestly — `typecheck` and `typecheck:tools`, not `tsc` and `tsc2`. Six months from now the second script should be deletable, and a clear name makes that decision easy.

## Node type stripping did not replace tsc

Since Node 22.18 and on the current LTS line, Node can execute TypeScript directly by stripping types. It is excellent for scripts, tests, and small services. It checks nothing. A file that runs perfectly under Node can still be full of type errors that only `tsc --noEmit` will find.

The combination I use in 2026:

- **Development:** Node runs`.ts` directly for scripts and local servers, with`erasableSyntaxOnly` enforcing that the syntax is strippable.
- **Editor:** the TypeScript 7 language server, for instant feedback.
- **CI:**`tsc --noEmit` with parallel checkers. This is the gate.

Two modern options that pair well with this: `--erasableSyntaxOnly` (TypeScript 5.8+) rejects syntax that cannot be erased, and `import defer` (5.9+) lets you defer module evaluation without turning everything into a dynamic import. If you have not revisited your import strategy since 2023, both are worth an afternoon.

## The migration playbook I would follow again

1. **Pin Node and TypeScript versions in CI.** Do not let a floating`typescript@latest` upgrade your compiler mid-sprint.
2. **Go to 6.0 first.** Remove deprecated options, set`strict` explicitly, and fix the fallout. Ship it.
3. **Add explicit `types` and `rootDir`** before installing 7.0. This avoids a noisy first run.
4. **Install 7.0 and run `tsc --noEmit` on the whole workspace.** Expect zero type errors if step 2 was clean. If you see hundreds, you skipped step 2.
5. **Measure the editor, not just CI.** Track time-to-first-error on a large file. That is where developers feel the upgrade.
6. **Keep `tsc6` for lagging tools** and write down which ones. A short list beats tribal knowledge.
7. **Remove `skipLibCheck` workarounds last.** Faster checking makes it tempting to turn off, but it hides dependency bugs.

One thing I would not do: enable every strict flag in the same commit as the compiler upgrade. Two variables, one failure. Upgrade first, tighten second.

## What still needs work

The missing stable API is the real gap. If your build depends on custom transformers or a tool that imports `typescript` internals, 7.0 is not ready for that part of the pipeline. The compatibility package is a bridge, not a destination. Plan for 7.1, and keep an eye on the changelogs of typescript-eslint and your framework tooling rather than assuming “TypeScript 7 support” means the whole ecosystem moved on day one.

Also budget for declaration emit. Libraries that publish `.d.ts` files should verify output against 6.0 before switching the publishing pipeline. Type behaviour is identical, but path defaults change, and a broken declaration path is the kind of bug users report as “your package has no types” three weeks later.

## Verdict

TypeScript 7 is the rare upgrade that makes the tool disappear. You stop thinking about type-check time and start thinking about the code again. The migration is mostly a 6.0 cleanup plus two tsconfig defaults — boring work, and worth doing properly.

If you are modernising the rest of the front-end stack at the same time, pair this with Next.js 16 and Cache Components. And if your team is still deciding how much AI assistance belongs in the loop, the honest comparison in AI code assistants is a better starting point than another vendor demo.

## FAQ

### Do I need to rewrite anything for TypeScript 7?

No. The type checker was ported faithfully; code that compiled cleanly on 6.0 should compile identically on 7.0. The work is configuration and ecosystem compatibility, not application code.

### Will my build get 10x faster?

Full builds typically see 8–12x in large codebases. Small projects see seconds saved, not minutes. The bigger everyday win is editor responsiveness.

### Can I use TypeScript 7 with ESLint today?

Often not with the type-aware rules, because 7.0 has no stable programmatic API until 7.1. Use `@typescript/typescript6` with `tsc6` for those tools while the native compiler handles builds and type-checks separately.

### Is Node’s type stripping enough to drop tsc?

No. Type stripping erases annotations without checking them. Keep `tsc --noEmit` in CI as the correctness gate.

*Images: Lisa Fotios (CC0), Remko van Dokkum (CC BY 2.0), One Idea LLC (CC0) via StockSnap and Openverse.*
