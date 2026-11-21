---
id: collect-261121-veille-ia/langages-dev/typescript-7-and-the-go-compiler-on-a-real-project-migration
title: "TypeScript 7 and the Go Compiler on a Real Project Migration"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "node", "compiler", "config", "migration", "code", "project", "bundler"]
source: https://zeroexp.dev/library/typescript-7-migration
source_anchor: ""
source_lines: [1, 62]
section: "Langages_Dev"
---
TypeScript 7 is the compiler rewritten in Go, and on this site's own frontend it is not a small speedup: the same type check went from 9 seconds to 0.8, with the config untouched. The migration work is not in the speed. It is in the options TypeScript 6 deprecated, which 7 refuses outright, and in two changed defaults that make code fail to find its own globals.

Settings worth keeping after the upgrade are explained in a TypeScript config you can defend line by line.

## How much faster is it on a real project?

A clean `tsc --noEmit --incremental false`, three runs each:

| Compiler | Wall time | Memory used | 
|---|---|---|
| TypeScript 5.9.3 | 8.83 to 8.95 s |  | 
| TypeScript 6.0.3 | 9.04 to 9.15 s | 638 MB | 
| TypeScript 7.0.2, default 4 checkers | 0.79 to 0.88 s | 426 MB | 
| TypeScript 7.0.2, `--checkers 1` | 1.15 to 1.21 s |  | 
| TypeScript 7.0.2, `--singleThreaded` | 1.48 to 1.62 s |  | 

About eleven times faster, inside the 8x to 12x range the TypeScript 7.0 announcement reports. Even single-threaded it is six times faster, so the gain is mostly native code, with parallel checking on top. The config passed on 7 with no edit, because it already used `moduleResolution: bundler`, `strict` and no `baseUrl`. Profiling builds is covered in finding what makes a TypeScript build slow.

## What does a legacy config hit?

TypeScript 6.0.3 already fails it, with a way out:

TypeScript 7.0.2 offers none:

So upgrade to 6 first: it lists every problem while `ignoreDeprecations` keeps the build green. Fix them there, then move to 7.

## What replaces each removed option?

| Removed | Replacement | 
|---|---|
| `target: es5` ,`downlevelIteration` | A modern target such as `es2022` ; a bundler handles old browsers | 
| `moduleResolution: node` or`classic` | `nodenext` for code Node runs,`bundler` for bundled code | 
| `baseUrl` | `paths` entries starting with`./` | 
| `module: amd` ,`umd` ,`systemjs` | `esnext` or`preserve` with a bundler | 
| `esModuleInterop: false` | Delete it; it is always on | 

One trap: under `nodenext`, the `paths` alias compiled cleanly once fixed but the output still imported `@lib/money`, and Node failed with `Cannot find package '@lib/money'`. TypeScript never rewrites import paths. For code Node runs, use `package.json` `imports` instead, which both understand; module resolution for Node and bundlers shows the working setup.

## Which new defaults bite?

**`types` is now `[]`.** Without it, the fixed project lost Node's globals:

Add `"types": ["node"]`, plus your test runner's types where it has globals. **`rootDir` defaults to the config's folder**, so a config beside `src` needs `"rootDir": "./src"` to keep `dist/index.js` rather than `dist/src/index.js`. And **`strict` is on by default**, which matters only for configs that never set it; strict mode flag by flag covers what it turns on.

One command line change caught the test setup for this post: with a `tsconfig.json` in the folder, `tsc file.ts` now stops with `error TS5112: tsconfig.json is present but will not be loaded if files are specified on commandline. Use '--ignoreConfig' to skip this error.`

## What about tools that use the compiler API?

TypeScript 7.0 has no programmatic API; the team expects a new one in 7.1. typescript-eslint and other tools that import `typescript` keep using 6. The TypeScript team's recommended setup installs both under aliases:

That installed a `tsc` reporting `Version 7.0.2` and a `tsc6` reporting `Version 6.0.3`, and tools importing `typescript` get the 6.0 API. Run `tsc` in CI for type checking and let the linter use 6.

## When does this not apply?

If your build uses a bundler or runner that strips types, such as Vite, esbuild or Node itself, TypeScript 7 changes only your type check and editor, never the output. Projects still on TypeScript 5 with `baseUrl` everywhere should treat 6 as the migration and 7 as a version bump after it. And editor support is separate: check that your editor uses the TypeScript 7 language server.

## Next steps

1. Upgrade to TypeScript 6 and fix every deprecation it reports.
2. Set `types` and`rootDir` explicitly.
3. Install 7 beside 6 with the aliases above if a tool needs the API.
4. Time `tsc --noEmit` before and after, on your CI runner.
