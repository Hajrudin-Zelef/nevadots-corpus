---
id: collect-261121-veille-ia/langages-dev/typescript-7-migration-adopt-the-go-compiler-before-7-1-ships
title: "TypeScript 7 Migration: Adopt the Go Compiler Before 7.1 Ships"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-07-08"]
keywords: ["typescript", "microsoft", "compiler", "migration", "only", "adopt", "first", "ships"]
source: https://loopyazilim.net/blog/typescript-7-migration-before-7-1
source_anchor: ""
source_lines: [1, 135]
section: "Langages_Dev"
---
A TypeScript 7 migration is no longer homework for later. Microsoft released TypeScript 7.0, the compiler rewritten in Go, on 8 July 2026, and published the 7.1 beta on 20 October 2026. Teams writing React, Next.js or Node in TypeScript will meet it soon: either by choice, or because a dependency update pulls in 7.0 and `npm ci` stops with an ERESOLVE error, or the lint step crashes.

Our take is simple: adopt 7.0 now, but only as your type-checker. Keep every tool that uses the compiler as a library on the TypeScript 6 compatibility package until that tool announces 7.1 support. The steps below show how to do that without breaking linting or framework tooling.

## Why a TypeScript 7 migration means two compilers for now

The payoff is real. Microsoft's 7.0 announcement cites Canva, where the time to the first error in the editor fell from about 58 seconds to about 4.8 seconds. For pipelines that spend minutes on `tsc --noEmit`, a speed-up of that kind changes how often you can afford to type-check.

The catch is stated just as clearly in the same announcement: TypeScript 7.0 ships without an API. Microsoft expects 7.1 to bring a new and different API. Until then, it has made it a priority that 7 can run side by side with 6.0 for tools that need programmatic access, with typescript-eslint as the obvious example. The `@typescript/typescript6` package supplies a `tsc6` executable and re-exports the 6.0 API, so `tsc` can be TypeScript 7 while the rest of your tooling keeps relying on 6.0.

What happens if you skip this is recorded in a typescript-eslint issue opened on 8 July 2026. With typescript-eslint 8.63.0, `npm ci` fails because the peer dependency range only allows TypeScript below 6.1.0. Installing anyway with `npm install` doesn't fix it: ESLint then crashes inside `@typescript-eslint/typescript-estree`. Forcing the install only moves the failure from install time to the first lint run.

## Step 1: Compile cleanly on TypeScript 6.0 first

TypeScript 6.0 is the bridge. Microsoft describes it as API compatible with 5.9 while introducing the breaking changes and deprecations that set the stage for 7.0. You can silence those deprecations with `"ignoreDeprecations": "6.0"`, but 7.0 supports none of the deprecated options, so that setting only postpones the work.

The compatibility contract in Microsoft's 7.0 announcement is precise: code that compiles cleanly on 6.0 with `stableTypeOrdering` enabled and without `ignoreDeprecations` should compile identically on 7.0. Beyond that, 7.0 adopts 6.0's new defaults and turns 6.0 deprecations into hard errors. `strict` is on by default, `module` defaults to `esnext`, and `stableTypeOrdering` is always on, with no option to disable it.

So the first milestone is a clean 6.0 build that meets that contract:

- Remove `ignoreDeprecations` and fix whatever surfaces.
- Turn on `stableTypeOrdering` .
- Set `types` explicitly. Microsoft notes many projects will need`["node"]` ;`["*"]` restores 5.9 behaviour, but an explicit list is recommended for performance and predictability.
- Set `rootDir` explicitly. The experimental`ts5to6` tool can adjust`baseUrl` and`rootDir` across a codebase.
- Write `strict` and`module` out explicitly too, so the move to 7.0's defaults changes nothing you didn't choose.

```
{
  "compilerOptions": {
    "strict": true,
    "module": "esnext",
    "types": ["node"],
    "rootDir": "./src",
    "stableTypeOrdering": true
  }
}
```
Adjust `types`, `rootDir` and `module` to your project. The point is that nothing important is left to a default that is about to change.

## Step 2: Find everything that imports `typescript` as a library

The `tsc` command is only one consumer of TypeScript. Anything that imports the `typescript` package and calls its API will not work against 7.0. Start with the dependency tree and your own scripts:

```
# Which installed packages depend on typescript?
npm ls typescript --all
# Which of your own files import the compiler?
grep -rln -e "'typescript'" -e '"typescript"' scripts tools src
```
Sort what you find into a short table:

| Category | Typical examples | What to do before 7.1 | 
|---|---|---|
| Linting | typescript-eslint | Keep on the 6.0 API | 
| Framework language tools | Vue, Svelte, Astro, Angular tooling | Keep on the 6.0 API until 7.1 support is announced | 
| Codegen | API clients, schema-to-type generators | Keep on 6.0, test against 7.1 later | 
| Custom AST scripts | codemods, doc generators, transformers | Keep on 6.0, plan a port to the new API | 
| Plain type-checking | `tsc --noEmit` in CI | Move to 7.0 now | 

If the list contains nothing but `tsc` itself, you can move straight to 7.0 after Step 1. Most real projects have at least typescript-eslint, which is why the next step matters.

## Step 3: Run TypeScript 7 side by side with the 6.0 API

If Step 2 found library consumers, use the side-by-side setup Microsoft documents in the 7.0 announcement: `tsc` runs TypeScript 7, while `@typescript/typescript6` provides the 6.0 API to the tools that need it. Because tools such as typescript-eslint import `typescript` directly through peer dependencies, Microsoft recommends npm aliases: point `typescript` at the compatibility package and add a second alias for TypeScript 7.

```
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```
With only the first alias you get just a `tsc6` executable; the second alias is what makes `npx tsc` run 7.0.

Then make the split visible in CI. Type-checking on 7 becomes its own step, and the job prints which compilers it actually resolved:

```
- name: Show compiler versions
  run: |
    npx tsc --version
    npx tsc6 --version
- name: Type-check with TypeScript 7
  run: npx tsc --noEmit
- name: Lint (tooling on the TypeScript 6.0 API)
  run: npx eslint .
```
The version printout is cheap insurance. If `npx tsc --version` reports a 6.x release, the job is not testing what you think, and you want to learn that from a log line rather than from a slow build weeks later.

The trade-off: for a while you carry two compiler versions. Treat the 7 step as your type-check gate and the 6.0 copy as a dependency that exists only to keep tools running.

### Next.js: check what `next build` actually runs

According to the Next.js docs, `next build` by default runs the project-local `tsc` command instead of loading the TypeScript JavaScript compiler API. That default supports TypeScript 6 and is what makes TypeScript 7 usable while its JavaScript API is unavailable. If you opt out with `experimental.useTypeScriptCli: false` while on TypeScript 7, `next build` exits.

The docs also spell out the trade-offs of CLI checking: you get native `tsc` diagnostics without Next.js-specific code frames, and the check covers the complete project selected by your tsconfig, including test files and `.next/dev/types` when they are included. The option is experimental and its behaviour may change.

Practical checks:

- Don't set `useTypeScriptCli: false` on a TypeScript 7 project.
- Run `npx tsc --version` in the same CI job as`next build` , because the build calls that local`tsc` .
- Expect `next build` to fail on type errors in test files if your tsconfig includes them. Fix them, or narrow`include` on purpose rather than by accident.

## Step 4: Try TypeScript 7.1 on a branch, not in production

Microsoft's 7.1 beta announcement ships a new compiler API and new features, and says the team hopes to deliver even better speed with a leaner memory footprint by the final release. It adds a new plugin point called content mappers, and one behaviour change to watch: 7.1 is stricter about relating type parameters inside distributive conditional types. The official iteration plan also includes DOM `lib.d.ts` updates, which can raise new type errors in code you haven't touched.

That makes 7.1 worth testing early and not worth shipping yet:

```
git switch -c try-ts-7.1
npm install -D typescript@beta
npx tsc --noEmit
```
Triage the errors into three groups: conditional-type changes, DOM typings and everything else. Fix genuine bugs on your main branch, where the fix still has to compile on 7.0, and keep the rest as notes for the final release.

Two rules until your tooling catches up:

- Don't remove `@typescript/typescript6` until typescript-eslint and your framework's language tooling announce 7.1 support in an actual release. An API in beta is an invitation to experiment, not a guarantee.
- Don't commit to a hard cutover on a specific day.

## Treat 24 November as a target, not a promise

The 7.1 iteration plan lists the RC for 10 November 2026 and the stable release for 24 November 2026. The beta announcement says an RC should follow in about a month, with the final release shortly after. However, on 1 September 2026 a maintainer wrote in the same plan that the team was looking at pushing the beta out by about two weeks, with details to follow on what that means for the RC and final dates. The plan's table still showed the beta for 6 October; it actually shipped on 20 October, and the beta announcement's "about a month" points to an RC around 20 November rather than 10 November. We recommend treating both November dates as tentative.

In planning terms: schedule the 7.1 evaluation, not the 7.1 rollout. Put a recurring check on your team's calendar for typescript-eslint and framework release notes, and keep the side-by-side setup as your stable state until both have shipped 7.1 support.

## Key takeaways

- Use TypeScript 7.0 now for type-checking only; it ships without a compiler API.
- Get clean on 6.0 first: no `ignoreDeprecations` ,`stableTypeOrdering` on, explicit`types` and`rootDir` .
- Inventory every tool that imports `typescript` and keep those on`@typescript/typescript6` .
- Forcing typescript-eslint onto 7.0 just moves the failure from install to lint.
- In Next.js, keep CLI type checking on and print `tsc --version` in CI.
- Test the 7.1 beta on a branch and treat 10 and 24 November 2026 as dates that may move.
