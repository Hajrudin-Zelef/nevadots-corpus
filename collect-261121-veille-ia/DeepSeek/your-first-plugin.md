---
id: collect-261121-veille-ia/deepseek/your-first-plugin
title: "Your first plugin"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["plugin", "context", "cordis", "export", "apply", "function", "tools", "import"]
source: https://deepseek-harness.github.io/deepseek-harness/en/develop/basic/
source_anchor: ""
source_lines: [1, 118]
section: "DeepSeek"
---
# Your first plugin 

This tutorial creates a minimal Harness plugin and loads it into the Web UI. Start from a repository checkout that has completed the run-from-source path.

## Create a local project 

From the repository root, create a scratch project for the tutorial:

`mkdir -p scratch-plugin/src`
## What is a plugin? 

In Harness, a plugin is a TypeScript module that exports an `apply` function. The framework calls `apply` when loading the plugin and passes a `ctx` context object through which the plugin registers capabilities:

```
import type { Context } from '@deepseek-ai/cordis'
export const name = 'my-plugin'
export function apply(ctx: Context) {
  // Register capabilities here.
}
```
That is the complete configuration.

## Create the plugin file 

Create `scratch-plugin/src/my-plugin.ts`:

```
import type { Context } from '@deepseek-ai/cordis'
export const name = 'hello-plugin'
export function apply(ctx: Context) {
  // Required dependencies are ready before apply runs.
  console.log('[hello-plugin] plugin loaded!')
}
```
## Register it in cordis.yml 

Run `pwd` from the repository root, then create `scratch-plugin/cordis.yml` as a Web overlay that inserts the local plugin. Replace `/absolute/path/to/deepseek-harness` below with the printed path:

```
- insert:
    - id: hello
      name: '/absolute/path/to/deepseek-harness/scratch-plugin/src/my-plugin.ts'
```
The plugin path must be absolute. A patch file contributes configuration but does not change the profile directory from which the loader resolves module paths.

Start the Web UI with that overlay:

`pnpm dsh web --patch ./scratch-plugin/cordis.yml`
Open `http://127.0.0.1:3080`. The terminal prints `[hello-plugin] plugin loaded!` during startup.

## Automatic cleanup 

Anything registered through `ctx`—event listeners, tools, or timers—is cleaned up when the plugin unloads. You do not need to call removeListener or clearInterval manually.

For a resource that needs explicit cleanup, such as a network connection, use `ctx.effect()` to provide its disposer:

```
import type { Context } from '@deepseek-ai/cordis'
export function apply(ctx: Context) {
  ctx.effect(() => {
    const timer = setInterval(() => {
      console.log('heartbeat')
    }, 5000)
    // The returned function runs when the plugin unloads.
    return () => clearInterval(timer)
  })
}
```
## Declare dependencies 

If the plugin consumes another service such as `tools` or `llm`, declare it in `inject`:

```
import type { Context } from '@deepseek-ai/cordis'
export const name = 'my-tool-plugin'
export const inject = ['tools']
export function apply(ctx: Context) {
  // ctx.tools is ready here.
  ctx.tools.register(/* ... */)
}
```
The framework waits for every required service before loading the plugin.

## Three plugin forms 

In addition to a function module, a plugin can use object or class form.

### Object form 

```
import type { Context } from '@deepseek-ai/cordis'
export default {
  name: 'my-plugin',
  inject: ['tools'],
  apply(ctx: Context) {
    // ...
  },
}
```
### Class form 

```
import { Service, type Context } from '@deepseek-ai/cordis'
export default class MyService extends Service {
  static inject = ['tools']
  constructor(ctx: Context) {
    super(ctx, 'myService')
    // Perform synchronous initialization in the constructor.
  }
}
```
Function form is sufficient in most cases. Use class form when the plugin provides a service to other plugins; see services and dependencies.

## Next steps 

- Build a tool — learn the tool definition DSL
- Plugin configuration — accept user configuration
- Cordis tutorial — the plugin framework underneath, built from a scratch directory with no API key
