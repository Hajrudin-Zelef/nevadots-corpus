---
id: collect-261121-veille-ia/langages-dev/typescript-7-is-fast-your-migration-cost-is-in-the-tools-that-import-it-makmel
title: "TypeScript 7 Is Fast. Your Migration Cost Is in the Tools That Import It — Makmel"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "cost", "compiler", "tools", "post", "migration", "editor", "defaults"]
source: https://makmel.info/blog/typescript-7-migration-cost-is-tooling
source_anchor: ""
source_lines: [1, 72]
section: "Langages_Dev"
---
On July 8, the TypeScript team published Announcing TypeScript 7.0, the native port of the compiler to Go. The headline number is the one everybody quoted: full builds "typically yield speedups between 8x and 12x." The VS Code codebase went from 125.7 seconds to 10.6.

A few paragraphs further down is the sentence that decides most teams' schedules: "While TypeScript 7.0 is here, it does not ship with an API."

That's the real migration story. Your application code probably needs a few tsconfig edits. The things that break are the tools that load the compiler as a library: the type-aware linter, the editor plugin, the webpack loader, the script that walks the AST to generate client code. Four months after 7.0, the replacement API is still in beta.

## Two releases, two kinds of cost

**TypeScript 6.0 carries the config cost.** It shipped on March 23 as the bridge release, and it's where the defaults moved. `strict` is now `true` by default, `module` defaults to `esnext`, and `types` defaults to an empty array. `rootDir` now defaults to the directory holding `tsconfig.json`. Deprecated: `target: es5`, `downlevelIteration`, `--moduleResolution node` (that is, `node10`), and `baseUrl` as a lookup root. Gone outright: `moduleResolution: classic`, `--outFile`, and the `amd`, `umd` and `systemjs` module values. `esModuleInterop` can no longer be set to `false`.

You can silence the deprecations with `"ignoreDeprecations": "6.0"`. The 6.0 post is clear that this only buys time, because 7.0 "will not" support those options. The 7.0 post confirms it: 7.0 "provides hard errors in the face of any flags and constructs deprecated in TypeScript 6.0."

This part is tedious, but it's bounded. There's even an experimental `ts5to6` tool that "can automatically adjust `baseUrl` and `rootDir` across your codebase." The 7.0 post calls the `rootDir` and `types` changes the most "surprising" but says "they can be mitigated easily." I'd agree. A missing `"types": ["node"]` shows up as a wall of errors on your first build, not three weeks later in production.

**TypeScript 7.0 carries the tooling cost.** 6.0, per its own announcement, "continues to be API compatible with TypeScript 5.9." 7.0 isn't API compatible with anything, because it doesn't have an API. The December 2025 progress update had already said it plainly: "Corsa/TypeScript 7.0 will not support the existing Strada API." Strada is the JavaScript compiler that `import ts from "typescript"` has handed you for a decade.

## Who imports the compiler

Most teams don't think of themselves as compiler API users. The API is buried in their dev dependencies.

**Type-aware linting.** typescript-eslint builds a TypeScript program to power rules like no-floating-promises. On 7.0's release day, a maintainer answered the support request in issue #12518: "typescript-eslint isn't compatible with TS 7 at this time, because there is no TS 7 API at this time."

**Embedded languages.** The 7.0 post says "workflows that use Vue, MDX, Astro, Svelte, and others will likely not yet be able to leverage TypeScript 7," and that tools such as Volar, which embed TypeScript into their own language services, "can only currently rely on TypeScript 6.0." Angular's template checking is in the same position. The suggested split for Angular is 7.0's `tsc` on the command line and 6.0 in the editor.

**Language service plugins.** The team recommends using 7.0 "in scenarios where language server plugins are not required." If your editor experience depends on a plugin listed in `compilerOptions.plugins`, that's a 6.0 editor for now.

**Everything else that calls `ts.createProgram`.** Bundler loaders, test transformers, OpenAPI and GraphQL codegen that reads your types, internal scripts that use the checker to enforce an architecture rule, documentation generators. None of these is named in the 7.0 post, and that's the point. They're your inventory to take, not the TypeScript team's.

Custom transformers deserve a separate line. The 7.1 beta announcement doesn't mention them, and neither does the 7.0 post. Tools that inject transforms into emit, including those that patch the compiler package itself, were written against a JavaScript compiler running in the same process. My reading is that in 7, the compiler is a Go binary and there's nothing in your process to patch. Until a transform hook is documented, assume you'll need another route, such as a bundler plugin or a separate build step.

## The 7.1 API is a different shape

The 7.1 beta shipped on October 20 with the first real API. It isn't the old one with a new package name. It's split into entry points: `typescript/sync`, `typescript/async`, `typescript/ast`, `typescript/fs` and others. The objects you work with live on the Go side, and the post warns that "we can't just let the garbage collector deal with them or else we'll leak on the Go side." `API` and `Program` objects are `Disposable`, so you release them with `using` or by hand. Programs are taken from snapshots, and `snapshot.update()` derives a new one incrementally. File watching, if you need it, "currently needs to be done separately."

There's also a new extension point, content mappers, for non-TypeScript file formats. They run only with `--runExternalCode`, because they "are effectively arbitrary executables that TypeScript dispatches to."

Read that as a tool author and the scope is clear. Code that held a `ts.Program` for the life of a process, walked nodes freely and let the GC clean up needs to be restructured around explicit lifetimes, not just re-imported. As for timing, the beta post promises "a Release Candidate in about a month from now, followed by a final release shortly thereafter." That puts 7.1 stable near the end of the year at best. After that, each tool you depend on still has to port to it and release.

## Running both while you wait

The 7.0 release gave you a way to take the speed without waiting for the ecosystem. The `@typescript/typescript6` package provides a `tsc6` binary and re-exports the 6.0 API. Because tools like typescript-eslint import `typescript` through a peer dependency, the post recommends npm aliases. Keep `typescript` pointing at 6.0 for the tools, and give 7.0 a different name:

```
{
  "devDependencies": {
    "@typescript/native": "npm:typescript@^7.0.2",
    "typescript": "npm:@typescript/typescript6@^6.0.2"
  }
}
```
CI type-checks with 7.0. The linter, the loader and the editor plugins keep loading 6.0. That works because 7.0 is designed to match 6.0's type-checking behavior, which is why getting clean on 6.0 first matters. The cost is two compilers that can disagree, plus a `package.json` future maintainers will need explained. Write the reason down next to it.

This is the same lesson as the Node 26 upgrade: the version bump is the easy part, and the work is in what sits around it. It also argues for taking upgrades in small installments. 6.0 now, the alias for 7.0 next, each tool when its maintainers ship.

## Before you bump to 7

1. Does `tsc` 6.0 build with zero deprecation warnings and no`ignoreDeprecations` in any tsconfig?
2. Did you set `types` and`rootDir` explicitly instead of relying on the new defaults?
3. Which packages in your lockfile have `typescript` as a peer or direct dependency? That list is your real migration scope.
4. Do any of them need a `ts.Program` or the type checker: type-aware lint rules, loaders, test transformers, codegen?
5. Do you use Vue, Svelte, Astro, MDX or Angular templates, or any entry in `compilerOptions.plugins` ?
6. Does any build step apply custom transformers or patch the compiler package?
7. If you split 6.0 and 7.0 with aliases, does CI show both compilers' results, and is there a note explaining why both are there?
8. For internal scripts that use the compiler API, has someone read the 7.1 beta API and estimated the rewrite?

If items 3 through 6 are all empty, you can take the 10x today. If they aren't, the speedup is real but scheduled, and the schedule belongs to your tools.

## Thoughts on this post?

Questions, disagreements, or a related problem you're working through? I'd love to hear from you.

Get in touch →
