---
id: collect-261121-veille-ia/langages-dev/highlights-from-typescript-7-1
title: "Highlights from TypeScript 7.1"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["import", "source", "type", "typescript", "imports", "module", "content", "const"]
source: https://medium.com/@onix_react/highlights-from-typescript-7-1-a4629b80a275
source_anchor: ""
source_lines: [1, 270]
section: "Langages_Dev"
---
The TypeScript team has released the **7.1 Beta**. It builds on the Go-based compiler from 7.0 and adds import attributes in ambient modules, source phase imports, a stable es2026 target, a brand-new programmatic API, and content mappers for single-file component formats like Vue and Svelte.

Let’s go through what’s new and how to try it today.

Subscribe to our **Telegram channel** to stay updated in real time! We post the latest updates and interesting content related to React, JavaScript, TypeScript, and more.


Install the beta:

`npm install -D typescript@beta`
## Import Attributes in Ambient Modules

Import attributes are metadata attached to an import statement. They tell runtimes and bundlers how to interpret the imported file, since a browser can’t rely on the `.json` extension alone:

`import myData from "./data/someData.json" with { type: "json" };`
The type attribute has become the standard way to do this. Beyond `json`, there are proposals for `type: "text"` (the file as a string) and `type: "bytes"` (a `Uint8Array`), and browsers already support `type: "css"` for importing a `CSSStyleSheet`.

Until now, typing such files meant wildcard ambient declarations like `declare module "*.css"`. That approach has real downsides:

- It ignores import attributes, so you can forget `with { type: "text" }` and TypeScript won’t complain.
- It’s repetitive, because you redeclare the same shape for `.txt` and`.md` even though the runtime cares about the attribute, not the extension.
- Runtimes may behave differently depending on the attribute. A bundler might treat .css as a CSS Module by default, while `type: "css"` forces a CSSStyleSheet.

TypeScript 7.1 lets ambient module declarations specify import attributes:

```
declare module "*" with { type: "css" } {
    const content: CSSStyleSheet;
    export default content;
}
declare module "*" with { type: "text" } {
    const content: string;
    export default content;
}
declare module "*" with { type: "bytes" } {
    const content: Uint8Array;
    export default content;
}
```
Now you get an error if the required attribute is missing, and the import gets the correct type based on the attribute.

⚠️ **Keep in mind:**

- Imports matching these declarations are not checked against files on disk. Failed lookups still show up at runtime or in your bundler.
- These declarations are not part of the built-in TypeScript libraries. Libraries shouldn’t ship them, and you should declare them in your own project, so you stay in control of your global scope (for example, `lib.dom.d.ts` versus`@types/node` ).

## Source Phase Imports

Source phase imports are a proposed ECMAScript feature that adds a `source` modifier after import. Only default imports are supported:

`import source SomeModule from "some-module";`
With import source, you receive an “inert” object representing the module after it has been parsed, but before its dependencies are loaded or linked and before any of its code runs. For `WebAssembly`, that object is a `WebAssembly.Module`, which you can pass straight to `WebAssembly.instantiate`:

```
import source SuperFastThing from "./super-fast-thing.wasm";
import { WASI } from "node:wasi";
const wasi = new WASI({ /*...*/ });
const fastThingInstance = await WebAssembly.instantiate(SuperFastThing, {
    wasi_snapshot_preview1: wasi.wasiImport
});
wasi.start(fastThingInstance);
```
WebAssembly source imports already work in recent Node.js versions. For JavaScript files, there isn’t much you can do yet. The proposal hints that you may eventually pass a module source to `import()`, but many tools don’t support JS source imports at all today.

Dynamic source imports use `import.source`:

`const SuperFastThing = await import.source("./super-fast-thing.wasm");`
Source imports also keep their identity, so you can safely compare them or use them as keys in a `Map` or `Set`:

```
import source source1 from "./plugin.wasm";
import source source2 from "./plugin.wasm";
const source3 = await import.source("./plugin.wasm");
console.log(source1 === source2); // true
console.log(source2 === source3); // true
```
## — target es2026 and New Library Methods

`es2026` is now a stable value for `--target` and `--lib`. The practical impact is on library declarations, which were added or moved over from esnext as proposals reached ECMAScript 2026. They include:

- `Math.sumPrecise()`
- `Iterator.concat()`
- `JSON.rawJSON()` and a`JSON.parse()` overload that exposes the original source text
- `Error.isError()`
- `getOrInsert()` and`getOrInsertComputed()` on`Map` and`WeakMap`
- `Uint8Array` base64 and hex helpers:`toBase64()` ,`setFromBase64()` ,`toHex()` ,`setFromHex()` ,`Uint8Array.fromBase64()` and`Uint8Array.fromHex()`
- `Array.fromAsync()`

`--lib esnext` also picks up a few newer proposals:

- `Promise.allKeyed` and`Promise.allSettledKeyed`
- `Iterator.zip` and`Iterator.zipKeyed`
- `Iterator.prototype.chunks` ,`.join` and`.includes`

## TypeScript 7.1’s New Compiler API

TypeScript 7.1 ships a new API for building tools on top of its syntactic and semantic analysis. Instead of one monolithic module, it’s split into focused entry points:

- `typescript/sync` : Synchronous compiler client with compiler options, type-checker and program APIs
- `typescript/async` : Promise-based equivalent of the sync client
- `typescript/fs` : Request-scoped full and layered file systems
- `typescript/ast` : AST node types and enums such as`SyntaxKind` and`ScriptTarget`
- `typescript/ast/is` : Node type guards
- `typescript/ast/factory` :`create*` and immutable`update*` functions

Here’s a simple compilation with explicit filenames and options. Unlike `tsc`, it does not discover `tsconfig.json` and doesn’t support `--project`:

```
import { API, formatDiagnosticsWithColorAndContext, type Diagnostic } from "typescript/sync";
using api = new API();
const reportErrors = (diagnostics: readonly Diagnostic[]) => {
    console.log(formatDiagnosticsWithColorAndContext(diagnostics, api));
};
// Parse command-line filenames and compiler options.
const commandLineArgs = process.argv.slice(2);
const parsedCommandLine = api.parseCommandLine(commandLineArgs);
if (parsedCommandLine.errors.length > 0) {
    reportErrors(parsedCommandLine.errors);
    process.exit();
}
// Create the program from the entry points.
using program = api.createProgram(parsedCommandLine.fileNames, parsedCommandLine.options);
// Collect diagnostics before emitting.
const diagnostics = [
    ...program.getGlobalDiagnostics(),
    ...program.getProgramDiagnostics(),
    ...program.getSyntacticDiagnostics(),
    ...program.getSemanticDiagnostics(),
];
if (diagnostics.length > 0) {
    reportErrors(diagnostics);
}
// Emit respects `noEmitOnError`.
const emitResult = program.emit();
if (emitResult.diagnostics.length > 0) {
    reportErrors(emitResult.diagnostics);
}
```
### Why `using`?

`API` and `Program` instances are `Disposable`. They allocate memory that must be tracked across the Go and JavaScript runtimes, so relying on the garbage collector would leak memory on the Go side. You should either:

- dispose of them manually (`api.close()` ,`program.dispose()` or`api[Symbol.dispose]()` ), or
- declare them with `using` so they’re disposed when they leave scope.

Once disposed, an object can’t be used again.

### Snapshots

Many workflows aren’t one-off builds. Files change over time, on disk or as the user types. The API models this with snapshots: a version of all projects and programs at a point in time. You can create a fresh one with `api.createSnapshot`, or derive a new one from an existing snapshot with `snapshot.update()`, which saves work for both you and the compiler.

```
import { ScriptTarget } from "typescript/ast";
import { createFileSystemLayer, createFileSystemWithLib } from "typescript/fs";
import { API } from "typescript/sync";
import { resolve } from "node:path";
const fileName = resolve("snapshot-example.ts");
using api = new API();
using before = api.createSnapshot({
    fileSystem: createFileSystemWithLib([
        [fileName, 'export const answer: number = "forty-two";'],
    ]),
    createPrograms: [
        {
            rootFiles: [fileName],
            compilerOptions: {
                strict: true,
                noEmit: true,
                target: ScriptTarget.ES2022,
            },
        },
    ],
});
using beforeProgram = before.operation.createdPrograms[0];
console.log("before:", beforeProgram.getSemanticDiagnostics().length);
using after = before.update({
    fileSystem: createFileSystemLayer([
        [fileName, "export const answer: number = 42;"],
    ]),
    ensurePrograms: [beforeProgram.id],
});
using afterProgram = after.getProgram(beforeProgram.id);
if (!afterProgram) {
    throw new Error("The updated program was not found.");
}
console.log("after:", afterProgram.getSemanticDiagnostics().length);
console.log("before again:", beforeProgram.getSemanticDiagnostics().length);
// before: 1
// after: 0
// before again: 1
```
**A few details worth knowing:**

- Programs and projects keep their `id` across snapshots.`ensurePrograms` lists the programs whose updates you want to observe. By default updates are lazy, so you can chain several`update()` calls without forcing program rebuilds.
- The previous snapshot stays usable, so you can still read diagnostics from both `beforeProgram` and`afterProgram` .
- Don’t mix objects from different snapshots (programs, projects or checkers).
- Snapshots are `Disposable` too, so use`using` or call`dispose()` .

### File System APIs

By default the API reads from the host file system. You can replace or layer it with virtual ones:

- `createFileSystem(files)` : Defines a complete file system. Unlisted files are absent unless explicitly mounted.
- `createFileSystemWithLib(files)` : Defines a complete file system and mounts the compiler’s default library directory read-only.
- createFileSystemLayer(files)`: Overrides selected files and falls back to the base snapshot’s file system, or the host file system.

In the example above, the initial snapshot used `createFileSystemWithLib` for a partial virtual file system, and the update used `createFileSystemLayer` for only the changed file. Everything else, including `lib.d.ts`, resolved through the base layer.

You can also emit entirely in memory:

`const emitResult = program.emitToString();`
Some extra controls exist, such as `removedPaths` to hide files from the underlying system. File watching is not built in. You need to report creations, deletions and modifications yourself via the `fileNotifications` option of `snapshot.update()`, and supply changes that don’t exist in the host file system through another layer.

## TypeScript 7.1: Content Mappers

TypeScript has always provided excellent tooling for `.ts` and `.js` files. But what about frameworks like Vue, Astro, Ember, and Svelte, where HTML, JavaScript, and CSS coexist in a single file?

**Until now, supporting these formats came with limitations:**

- **Editor integration:** Tools like Volar had to embed TypeScript or rely on complex workarounds.
- **CLI limitations:** Command-line type-checkers couldn’t take advantage of the same integrations.
- **Extra complexity:** Maintaining accurate diagnostics across embedded code was challenging.

### What’s New?

TypeScript 7.1 introduces **content mappers**. A content mapper is registered for specific file extensions and produces a virtual representation of the file for TypeScript to analyze. That representation includes regions mapped back to the original file, usually where the JavaScript or TypeScript code lives, so errors and spans stay accurate and tooling works inside the embedded code.

Register a mapper in `tsconfig.json`. For example, with a hypothetical `@vue/content-mapper` package (not yet on `npm`):

```
{
  "compilerOptions": {
    // ...
  },
  "contentMappers": [
    {
      "package": "@vue/content-mapper",
      "extensions": [".vue"],
      "options": {
        "target": 3.5
      }
    }
  ]
}
```
Whenever a matching file enters the program, either through `files`/`include` or an import, TypeScript asks the mapper for its content and mappings. Then you can run from the command line: `tsc --runExternalCode`

The `--runExternalCode` flag is required because content mappers are effectively arbitrary executables that TypeScript dispatches to, and the flag makes that explicit.

In VS Code, content mappers work out of the box, but they need both workspace trust and content mapper permissions. You can disable them entirely with the new `js/ts.contentMappers.enabled` setting.

Mappers have already been prototyped or implemented for:

- Astro
- CSS Modules
- Ember
- MDX
- Svelte
- Vue

**For developers, this means:**

- **Better type checking:** Catch errors directly in Vue, Svelte, Astro, and other supported files.
- **Consistent tooling:** Use TypeScript analysis across editors and command-line workflows.
- **Accurate diagnostics:** See errors mapped to the correct locations in your original files.
- **Simpler integrations:** Framework tooling can rely on a shared TypeScript extension mechanism instead of complex workarounds.

It’s an important step toward a more consistent development experience, regardless of the framework you use.

Stay tuned for more TypeScript updates, and follow **@onix_react** for the latest frontend development news! 🚀
