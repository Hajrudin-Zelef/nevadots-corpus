---
id: collect-261121-veille-ia/langages-dev/typescript-7-0-what-changed-and-what-should-be-expected
title: "TypeScript 7.0: What changed and what should be expected."
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "should", "changed", "projects", "watch", "changes", "expected", "microsoft"]
source: https://medium.com/@etosin70/typescript-7-0-what-changed-and-what-should-be-expected-2de0d73f4f01
source_anchor: ""
source_lines: [1, 191]
section: "Langages_Dev"
---
# TypeScript 7.0: What changed and what should be expected.

Summary of the Newly Released TypeScript 7.0

Microsoft has officially announced **TypeScript 7.0**, and the biggest message is clear: TypeScript is entering its **native-performance era**. Unlike previous versions, TypeScript 7.0 is built on a new native codebase written in **Go**, designed to make the compiler and editor tooling faster while remaining as compatible with existing TypeScript behavior as possible. Microsoft describes it as a **10x-faster native port of TypeScript**, with real-world full-build speedups typically between **8x and 12x**.

## What Changed in TypeScript 7.0?

The most important change is performance. TypeScript 7.0 brings native-code speed, shared-memory multithreading, and several optimizations that make building, checking, and navigating large projects much faster. In Microsoft’s benchmarks, projects such as VS Code, Sentry, Bluesky, Playwright, and tldraw saw major compile-time improvements. For example, VS Code build time dropped from **125.7 seconds in TypeScript 6** to **10.6 seconds in TypeScript 7**.

This speed improvement is not only about running `tsc`. It also affects the developer experience inside editors. TypeScript 7.0 improves project loading, auto-completion, find-all-references, diagnostics, and watch mode. Microsoft reports that opening a file with an error in the VS Code codebase previously took about **17.5 seconds** before showing the first error, but with TypeScript 7, it takes under **1.3 seconds**.

Another major change is that TypeScript 7.0 now performs several compiler operations in parallel. Parsing, type-checking, emitting, and project-reference builds can now take advantage of modern multi-core machines. Developers can control this behavior using new flags such as:

```
tsc --checkers 8
tsc --builders 4
tsc --singleThreaded
```
The `--checkers` flag controls the number of type-checking workers, while `--builders` controls how many project reference builders run in parallel. The `--singleThreaded` flag disables parallelization, which may be useful for debugging or when using TypeScript in a low-resource environment.

## Improved Watch Mode

TypeScript 7.0 also ships with a rebuilt `--watch` mode. The new watch mode is based on a Go port inspired by Parcel’s file-watching foundation. This means large projects should experience more stable and efficient file watching, especially when dealing with many files and dependencies. For developers who constantly run:

`tsc --watch`
This should lead to faster feedback after file changes and less resource pressure during development.

## Configuration and Breaking Changes

Although TypeScript 7.0 aims to be compatible with TypeScript 6.0, it adopts several new defaults and turns some deprecated options into hard errors. This means some projects may need configuration updates before upgrading.

Important default changes include:

```
{
  "compilerOptions": {
    "strict": true,
    "module": "esnext",
    "noUncheckedSideEffectImports": true,
    "libReplacement": false,
    "stableTypeOrdering": true,
    "rootDir": "./",
    "types": []
  }
}
```
Two changes developers should pay special attention to are `rootDir` and `types`. If your `tsconfig.json` is outside your `src` folder, you may need to explicitly set:

```
{
  "compilerOptions": {
    "rootDir": "./src"
  },
  "include": ["./src"]
}
```
Also, because `types` now defaults to an empty array, projects that rely on global type declarations should list them manually:

```
{
  "compilerOptions": {
    "types": ["node", "jest"]
  }
}
```
Some older options are no longer supported, including `target: es5`, `downlevelIteration`, `moduleResolution: node/node10`, `moduleResolution: classic`, and older module formats such as `amd`, `umd`, `systemjs`, and `none`. Microsoft recommends using modern options like `nodenext`, `bundler`, `esnext`, or `preserve` depending on the project setup.

## JavaScript and JSDoc Changes

TypeScript 7.0 also changes how JavaScript files are analyzed. The goal is to make JavaScript support more consistent with TypeScript files. Some older JSDoc and Closure-style patterns are no longer supported.

For example, values can no longer be used directly where types are expected. Developers should use `typeof` instead. Also, Closure-style function syntax is no longer supported; developers should use TypeScript-style function type syntax.

This mainly affects projects that use TypeScript to check JavaScript files with JSDoc comments. Pure TypeScript projects may not feel much impact here.

## Template Literal Type Improvement

TypeScript 7.0 now handles Unicode code points better in template literal type inference. Previously, some Unicode characters, such as emoji, could be split into invalid surrogate pairs during type-level string manipulation. TypeScript 7.0 now treats these characters more naturally, closer to how JavaScript behaves when using `for...of` or spreading a string.

```
type HeadTail<S> = S extends `${infer Head}${infer Tail}` ? [Head, Tail] : never;
type Result = HeadTail<"😀abc">;
//   ^
// In 7.0: ["😀", "abc"]
// Previously: ["\ud83d", "\ude00abc"]
```
This is a useful improvement, but it can be a breaking change for advanced type utilities that intentionally depended on UTF-16 code-unit behavior.

## How to Install and Use TypeScript 7.0

For most projects, installation remains simple:

`npm install -D typescript`
Then run:

`npx tsc`
For watch mode:

`npx tsc --watch`
For larger codebases, teams may experiment with parallelization:

`npx tsc --checkers 8`
For monorepos using project references:

`npx tsc --build --builders 4`
However, increasing `--checkers` and `--builders` can increase memory usage, so teams should test these settings carefully in local development and CI environments.

## Migration Guide for Developers

A practical migration path would look like this:

First, upgrade to TypeScript 6.0 before jumping to 7.0. Microsoft specifically recommends this because TypeScript 7.0 is designed to align closely with TypeScript 6.0 behavior.

Next, remove deprecated compiler options such as:

```
{
  "target": "es5",
  "moduleResolution": "node",
  "downlevelIteration": true,
  "baseUrl": "./src"
}
```
Then update your config to modern alternatives:

```
{
  "compilerOptions": {
    "target": "es2024",
    "module": "esnext",
    "moduleResolution": "bundler",
    "strict": true,
    "rootDir": "./src",
    "types": ["node"]
  },
  "include": ["src"]
}
```
For frontend projects using tools like Vite, Next.js, or modern bundlers, `moduleResolution: "bundler"` will often make sense. For Node.js projects using modern ESM behavior, `moduleResolution: "nodenext"` may be more appropriate.

## Important Limitation: No Stable API Yet

One major thing developers should know is that **TypeScript 7.0 does not yet ship with a stable programmatic API**. Microsoft expects TypeScript 7.1 to introduce a new API. Until then, tools that depend deeply on the TypeScript compiler API may still need TypeScript 6.0.

This affects tools such as `typescript-eslint`, language-service plugins, and frameworks or ecosystems that embed TypeScript internally.

Microsoft provides a compatibility package called:

`@typescript/typescript6`
This allows teams to run TypeScript 7.0 for `tsc` while keeping TypeScript 6.0 available for tools that still depend on the old API.

Because typescript-eslint expects to import from `typescript` directly via peer dependencies, it is recommended to achieve this via npm aliases. You should be able to run the following command

`npm install -D typescript@npm:@typescript/typescript6`
or modify your `package.json` as follows:

```
{
  "devDependencies": {
    "typescript": "npm:@typescript/typescript6@^6.0.2",
  }
}
```
Note that doing this will leave you only with a `tsc6` executable. To get 7.0’s `tsc`, you can add another alias for TypeScript 7 and `npx tsc` will just work with 7.0:

```
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```
## What Developers Should Expect

Developers should expect TypeScript 7.0 to make day-to-day development feel much faster, especially in large projects. Opening projects, seeing type errors, running builds, using auto-complete, and navigating code should become significantly smoother.

However, developers should also expect some migration work. Projects using old compiler options, ES5 targets, older module systems, custom JSDoc patterns, or TypeScript compiler API-based tooling may need updates.

Framework-specific users should be careful. Microsoft notes that workflows involving Vue, MDX, Astro, Svelte, and specialized Angular template type-checking may not fully benefit from TypeScript 7 yet because some of these tools rely on the TypeScript API, which is not stable in 7.0. For now, some teams may need to use TypeScript 7 for CLI checks and TypeScript 6 for certain editor or framework tooling.

## Conclusion

TypeScript 7.0 is less about adding flashy new syntax and more about transforming the speed and scalability of the TypeScript toolchain. Its native Go-based implementation, multithreaded compiler, faster editor support, improved watch mode, and reduced memory usage make it one of the most significant TypeScript releases so far.

For developers, the main benefit is a faster feedback loop. For teams, especially those working in large repositories or CI-heavy environments, the upgrade could save substantial development time. Still, migration should be handled carefully: update deprecated configuration, test tooling compatibility, check framework support, and consider running TypeScript 6 and 7 side-by-side where needed.

**Happy Coding!**

**Source:** https://devblogs.microsoft.com/typescript/announcing-typescript-7-0
