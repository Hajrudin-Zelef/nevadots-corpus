---
id: collect-261121-veille-ia/dev-langages/deepseek-harness-install-modes-and-first-plugin-guide
title: "DeepSeek Harness: Install, Modes and First Plugin Guide"
domain: dev-langages
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-03"]
keywords: ["plugin", "harness", "cordis", "repository", "install", "path", "pnpm", "deepseek"]
source: https://www.sitepoint.com/deepseek-harness-developer-preview/
source_anchor: ""
source_lines: [1, 75]
section: "DeepSeek"
---
DeepSeek Harness is an open-source agent harness built around Cordis plugins. Models, tools, sessions, storage, loops and the UI are plugins, so developers can compose capabilities without treating the default model provider as a special case.

**Reviewed October 3, 2026.** This article replaces an earlier Python tutorial that used unverified package names, commands and APIs. The official project uses a Node.js/TypeScript toolchain and Cordis. The repository still describes the project as a developer preview and warns of compatibility-breaking changes.

## What the preview provides

The official overview describes four modes. Standard includes the full toolset. Code mode lets model-generated code coordinate tool calls. Minimal keeps a shell tool and file editor for benchmarking. Creator adds runtime inspection, plugin experiments and preset-authoring features. These are modes of the harness, not four separate model products.

The repository is MIT-licensed. Its preview status matters more than a feature list: pin the source revision you test, keep a copy of your configuration and expect API changes. Review the repository's safety notice before running tools with access to files or credentials.

## Start the Web UI

Install Node.js using a supported method for your operating system. The official quickstart's npm entry point is:

`npx @deepseek-ai/dsh web`
This starts the Web UI at http://127.0.0.1:3080 by default. A local launch opens it in the default browser; an SSH launch prints the host URL. Add --no-open if you do not want a browser to open. Follow the Web UI guide linked from the repository for provider configuration. Do not paste provider secrets into public examples.

The old pip install deepseek-harness, harness init and deepseek_harness.Harness examples have been removed. They are not the installation route documented by this project.

## Run from source for plugin development

The repository gives this source-build path:

```
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```
Set up pnpm according to the repository's current requirements before running those commands. The build prepares the repository artifacts; pnpm dsh web uses them without rebuilding. Record the Git commit you used when reporting a bug so another developer can reproduce it.

## Your first Cordis plugin

The official first-plugin tutorial starts from a working source checkout. A minimal TypeScript plugin exports a name and an apply function. Harness passes a Cordis context to apply when the plugin loads.

`mkdir -p scratch-plugin/src`
Create scratch-plugin/src/my-plugin.ts:

```
import type { Context } from '@deepseek-ai/cordis'
export const name = 'hello-plugin'
export function apply(ctx: Context) {
  console.log('[hello-plugin] plugin loaded!')
}
```
Next create scratch-plugin/cordis.yml. Replace the absolute path with the path printed by pwd from the repository root. This is a configuration overlay; the plugin module path must be absolute.

```
- insert:
    - id: hello
      name: '/absolute/path/to/deepseek-harness/scratch-plugin/src/my-plugin.ts'
```
Start the UI with the overlay:

`pnpm dsh web --patch ./scratch-plugin/cordis.yml`
Open http://127.0.0.1:3080. The terminal should print [hello-plugin] plugin loaded! during startup. If it does not, check the absolute path and the overlay syntax against the current tutorial before adding more features.

## Lifecycle and dependencies

The Cordis context connects a plugin to the runtime. The tutorial explains that event listeners, tools and timers registered through ctx are cleaned up when the plugin unloads. A plugin consuming another service, such as tools or llm, declares its required services in inject. The framework waits for those services before loading the plugin.

Start with the documented function form. Add a tool or service only after the minimal plugin loads. Read the current service and dependency guides before creating a database or network plugin; a guessed Python register method is not a substitute for the actual contract.

## Safety before a real project

An agent harness can run shell commands and edit files. Test it in a disposable project with no production credentials. Give it only the files and network access it needs. If you add a database plugin, use a read-only account initially and keep query validation at the database boundary. Plugin composability does not supply access control automatically.

Do not expose the local UI to the public internet merely to access it remotely. Use an appropriate secured remote-access setup and read the project's safety guidance. Provider configuration and plugins may send data to external services, so local execution of the harness alone does not establish that all data stays local.

## When to adopt it

The preview is useful when your goal is to explore composable agent infrastructure or contribute a plugin. For a production dependency, first test the exact capabilities you need, recovery after interruption, configuration compatibility and secret handling. The developer-preview warning is a reason to keep the experiment isolated and reproducible.

The next useful milestone is small: run the Web UI, load one documented plugin and verify its lifecycle. Then build outward from the current docs rather than from invented provider names or a speculative roadmap.
