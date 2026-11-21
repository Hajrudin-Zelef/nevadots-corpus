---
id: collect-261121-veille-ia/deepseek/how-to-install-deepseek-harness-quick-start-from-source
title: "How to Install DeepSeek Harness: Quick Start & From Source"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["install", "node", "deepseek", "pnpm", "harness", "start", "source", "corepack"]
source: https://deepseekharness.io/install/
source_anchor: ""
source_lines: [1, 149]
section: "DeepSeek"
---
# How to Install DeepSeek Harness

DeepSeek Harness (what is it?) installs in one command. This guide walks the quick start end to end, then covers building from source, the first-run steps that trip people up, and what the v0.1 preview doesâand doesn'tâsupport for self-hosting.

`npx @deepseek-ai/dsh web`
## What You Need Before You Start

The list is short. If you have a recent Node.js, you can be inside the Web UI in under a minute; everything else can be added after launch.

### Node.js ^22.19.0 or >=24

dsh is a Node application. Any Node release matching ^22.19.0 or >=24.0.0 works; older versions fail the engine check.

### pnpm 11.7.0 (source builds only)

The repo pins pnpm 11.7.0 via Corepack. Only needed if you build from sourceârun corepack enable and the right version is used automatically.

### A DeepSeek API key

Required for model calls, not for installing. You add it in the Web UI under Settings â Models after first launch.

### Any OS with Node

macOS, Linux, or Windows: if Node runs, dsh runs. On Windows, use a shell with Node on PATH. Since 0.1.0-rc.8 the agent also gets a native persistent PowerShell tool instead of a bash-only one. If you'd rather not install Node, there is now an <a href="/deepseek-harness-desktop" class="text-primary font-semibold hover:underline">official desktop app</a> for Windows x64 and Apple silicon Macs.

## Install with npx (Recommended)

The npx path is the official quick start: no clone, no build, no global package. It downloads the published @deepseek-ai/dsh package and starts the Web UI server on your machine.

1.  1 ### Install Node.jsInstall Node ^22.19.0 or >=24.0.0 from nodejs.org or your version manager (nvm / nvm-windows). Open a fresh terminal so the new Node is on your PATH.
2.  2 ### Run dsh with npxOne command downloads the published package and starts the Web UI server. No global install, no clone, nothing to configure first. $`npx @deepseek-ai/dsh web`
3.  3 ### The browser opens itselfSince 0.1.0-rc.8, a local launch opens the Web UI in your default browser as soon as the server is ready. Port 3080 is the default; if it is busy, use the URL dsh prints. Pass --no-open to start the server without a browser, and expect an SSH launch to print the host URL only â your SSH client or editor owns the forwarded local address. $`http://127.0.0.1:3080`
4.  4 ### Choose a workspaceClick Choose workspace, add the project directory where you started dsh, and select it. The session composer stays disabled until this is done.
5.  5 ### Add your DeepSeek API keyGo to Settings â Models, enter your DeepSeek API key, and save. The model route becomes usable immediatelyâno server restart needed. Other providers are covered in the model configuration guide.

Something failed along the way? See the troubleshooting guide →

## The install is the smallest part of dsh

Everything in DeepSeek Harness is a plugin â models, tools, skills, sessions, sandboxes, storage, the agent loop, even the UI. That is why the interesting question is not how to install it, but what to put in it. Our directory indexes **5,627** community plugins across 12 categories, each with a written guide, screenshots from its own repository, and a copyable install command.

## Install from Source

Build from source if you want to read the code, hack on the harness, or write a plugin against the monorepo (`apps/` holds the CLI and web applications, `packages/` holds core, llm, mcp, sandbox, and dozens more). Prerequisites per the official development guide: Node 22.19+ or 24+, **Git 2.26 or newer**, and pnpm 11.7.0âpinned in package.json and delivered by Corepack, so enable Corepack before installing.

`$ git clone https://github.com/deepseek-ai/deepseek-harness.git$ cd deepseek-harness$ corepack enable$ pnpm install$ pnpm run build$ pnpm dsh web`
`corepack enable` is the step people skip. Corepack ships with modern Node and reads the pinned pnpm version from the repo, so you never install pnpm globally or fight version driftâone command and `pnpm install` just works with pnpm 11.7.0. After the build, `pnpm dsh web` starts the same Web UI as the npx path, served from your local checkout instead of the published package.

`pnpm install` does more than fetch packages: it wires up the repo's Lefthook git hooks and merge drivers automatically. To confirm the checkout is healthy before you start editing, run `pnpm run typecheck`âit's also what the pre-push hook runs, so a clean pass now saves a rejected push later.

From here, first run is identical to the quick start: let it open the printed URL, choose a workspace, add your key. If you are building from source because you want to extend the harness, the plugin ecosystem page maps what already existsâsince everything in dsh is a plugin, most extensions never require touching harness source at all.

### Run it headless (no Web UI)

The same checkout drives a no-browser agent. Put your key in a gitignored `.env` at the repo rootâ`DEEPSEEK_API_KEY=sk-...`, plus an optional `DEEPSEEK_BASE_URL` if you route through a proxyâthen hand it a task straight from the terminal:

`$ pnpm dsh --profile headless "summarize this workspace"`
`--profile` is the switch that makes one install serve several shapes: the web profile is the UI you saw above, the headless profile is this scriptable one-shot runner. In 0.1.2-alpha.1 the profile list grew to cover the SDK and ACP as well â the `demo:cordis` and `demo:acp` scripts that used to serve those roles are gone from the repository, replaced by real profiles. The entry modes table below lists all of them.

## First-Run Notes

Four things about the first five minutes that the install command does not tell you.

### Your workspace is where you launched dsh

The dsh process uses its invoking directory as the default filesystem location. Start it from your project root, then add and select that same directory as the workspace.

### Composer disabled? Pick a workspace

The session composer is intentionally unavailable until a workspace is selected. This is the single most common "is it broken?" moment on first runâit is not broken.

### Expect permission prompts

The agent can read and edit workspace files, run commands, delegate work, and maintain a plan. The Web UI asks before operations that require approval under the active permission policy, so keep the tab visible during early sessions.

### What a bare npx installs today

A bare npx run follows npm's latest dist-tag, which now serves 0.1.5-rc.3, published September 22, 2026; npx @deepseek-ai/dsh@next web takes the newer 0.1.7-rc.2. Three things come with the 0.1.5 line. Sessions it opens are upgraded to a V3 log format that older builds cannot read, so do not point a downgraded install at the same DSH_HOME. Its base bundle ships session telemetry as FEEDBACK_ONLY, so submitting /feedback uploads the relevant session records unless you set DSH_TELEMETRY_DISABLED. And every request to DeepSeek's official API carries the names and versions of your active plugin packages â metadata outside the model's input â unless a profile patch sets enabled: false on the plugin-package-inventory-deepseek row.

### This is a developer preview

DeepSeek Harness shipped on August 13, 2026 as a **v0.1 Developer Preview** (npm serves 0.1.5-rc.3, published September 22, 2026, as latest, with 0.1.7-rc.2 on `next` and the preview line on `alpha`), and the team's own warning is blunt: they are iterating rapidly and *"THERE WILL BE COMPATIBILITY-BREAKING CHANGES."* Expect commands, configuration, and behavior to shift between releases. If something that worked yesterday breaks today, re-running `npx @deepseek-ai/dsh web` picks up the latest published packageâand the troubleshooting page covers the recurring failure modes.

## One command, five applications

Since 0.1.2-alpha.1 the `dsh` command is the sole supported launcher, and a **profile** is an ordered stack of plugin-bundle patch layers sitting under your own overrides. SDK and ACP are profiles now, not separate binaries.

| Command | What it does | 
|---|---|
| `dsh web` | An alias of `--profile web` . Starts the browser application on`http://127.0.0.1:3080` and opens it for a local launch. | 
| `dsh --profile headless "job"` | Runs one fresh persisted session, prints the final answer, and exits. This is the scriptable one-shot runner. | 
| `dsh --profile sdk` | Serves SDK clients over JSON-RPC stdio until shutdown or disconnect. What the Python SDK starts by default. | 
| `dsh --profile sdk-minimal` | Serves SDK clients with a standalone minimal agent tree â a persistent shell and an editor, and nothing else. | 
| `dsh --profile acp` | Serves automation clients over ACP stdio until disconnect. In 0.1.1 this was a demo script; it is a first-class profile now. | 
| `dsh plugin --profile <name> ...` | Manages a profile's plugins by forwarding the remaining arguments to `pnpm` inside the profile directory. | 

The invoking directory is the default workspace root. The `web`, `headless`, `sdk`, `sdk-minimal`, and `acp` profiles auto-initialize on first use from shipped templates; any other profile has to be created through `dsh plugin` before it will boot.

The launcher parses only its own flags and hands everything after them to the booted profile. The first token it does not recognize starts the application's arguments â which is why `dsh --profile web --help` prints the web application's flags while `dsh --help` prints the launcher's. Invalid commands, options belonging to another mode, configuration errors, and boot failures all exit nonzero.

A profile directory holds a `package.json` (out-of-tree plugin dependencies plus the `dsh.profile` manifest with its ordered bundle list) and a `cordis.patch.yml` â your own patch layer. The tree composes over an empty root: each bundle's patch in order, then the profile patch, then `/cordis.patch.yml`, then any `--patch` overlays. Use `--dump-default-config` and `--dump-config` to inspect the composed tree without booting it.

Since `0.1.2-alpha.4` the shared base bundle mounts the web tool with fetch enabled, so `headless`, `sdk`, `acp`, and any custom profile that names only `@deepseek-ai/dsh-base` inherit both `web_search` and `web_fetch` with no application-level override â earlier builds enabled fetch per product and skipped ACP entirely. Fetch runs outside the shell and filesystem approval presets and needs no per-call approval; its provider accepts anonymous `http:` and `https:` requests to validated public destinations only, which bounds where a request may go, not what it may carry. Since `0.1.3-alpha.2` those same base-backed profiles also edit files through `read`, `write` and `edit` by default. The standalone `sdk-minimal` tree is unaffected, and a deployment that needs a different network policy has to restate the whole `tool-web` configuration in a later bundle or profile patch.

## Windows: A Real Shell Since rc.8

For its first week, dsh ran on Windows the way any Node application does â the engine check passes, `npx @deepseek-ai/dsh web` starts, the browser opens â but the agent's persistent shell tool was written for bash. **0.1.0-rc.8 closed that gap** with `@deepseek-ai/dsh-tool-pwsh-persistent`, a mirror of the bash tool for PowerShell: one owner-scoped persistent shell per agent, the current directory and exported environment variables surviving across calls, exact native exit codes, and the same timeout, cancel, and reset semantics.

You do not have to mount it yourself. The minimal agent preset now gates its persistent-shell stack by platform â the bash rows mount on POSIX, the pwsh rows on win32 â so a host always has exactly one persistent shell and it is the right one for the machine. The same release taught `dsh-subprocess` to inspect and signal Windows terminals, so a shell that has exited is detected rather than left hanging.

New since September 24, 2026: DeepSeek now ships a Windows x64 `.exe` installer for the desktop app, which bundles its own Node.js. If you want the terminal route, it is the same npx command or source build as everywhere else. Run it from a terminal that has a supported Node on `PATH`, and if a source build complains about pnpm, run `corepack enable` first. Windows-specific stumbles, such as PATH after a Node upgrade or a busy port 3080, are collected on the troubleshooting page.

## Signing In to a Provider: New in 0.1.1

Until 0.1.1 a credential in dsh could only be one thing: a value behind an environment-variable name. That describes an API key exactly and describes nothing else â which is why `openai-codex`, whose only authentication method is an OAuth login, had been withheld from the provider directory rather than offered and left broken. **0.1.1-rc.1 added the missing half.** `.credentials.yaml` gained a version and two sections: references, which still name environment variables, and records, which hold what a login produced. Boot upgrades the older flat pre-release file in place under the writer lock, so a key saved through the Models page in an earlier build keeps working without a hand edit.

The new `dsh-authorization` seam owns the conversation and never the protocol. A plugin that knows how to obtain its own credential registers a flow under the key it writes â `<plugin>/<id>`, scoped by the owning plugin's registered name rather than the provider's, so two plugins serving one provider cannot read each other's tokens. The flow performs the write itself, the seam allows one attempt per key at a time, and prompts travel with the request that started them: a login talks to exactly the page that asked, and a headless caller supplies an interaction that declines.

What it changes for you today: `openai-codex` is back in the provider picker, and a sign-in is offered for every installed provider that ships one â all 38 at the time of the release, of which 31 collect a key through pi-ai's own prompt, six offer that beside a subscription login, and Codex offers only the subscription login. What has not landed is the browser surface. The Models page still takes a typed key, the flows are reachable in-process only, an attempt is not durable so reloading the page mid-login abandons it, and signing out forgets the record locally without telling the issuer.

## Self-Hosting and Docker: The Honest Answer

As of the v0.1 preview, **DeepSeek Harness documents no official Docker image**, and we are not going to invent a Dockerfile here that the maintainers would have to support. The supported path is simply running the Node process yourself: `npx @deepseek-ai/dsh web` on your own machine, or the same command (or a source build) on a server you control that has a supported Node version.

That is less of a limitation than it sounds. dsh is a self-contained Node process serving a local Web UIâthere is no database to provision and no companion services to orchestrate in the documented setup. Given the pace of breaking changes in the preview, a hand-rolled container recipe would likely go stale within weeks anyway. If official container support lands, it will show up in the GitHub repo's docs first; GitHub Discussions and the Discord are the places to ask about deployment patterns in the meantime.

Worth noting for anyone comparing tools: this local-first, bring-your-own-process model is one of the practical differences from Anthropic's closed-source alternativeâour dsh vs Claude Code comparison walks through how the two approaches diverge on openness, models, and maturity.

## Installing dsh: Common Questions

Quick answers on Node versions, API keys, Windows, and first-run quirks.

The harness itself is free and open source under the MIT licenseâyou can install it, read the code, and modify it at no cost. Model calls are separate: when you plug in a DeepSeek API key, you pay DeepSeek API usage rates. Those rates rose at the August 13, 2026 launch (The Next Web reported the V4-Pro pricing increase) and were reduced again on September 10, when V4.1 Flash replaced V4 Flash. You can also plug in models from other providers, billed by whoever runs them.

Yes. dsh is a Node.js application, so it runs on any OS with a supported Node runtimeâmacOS, Linux, or Windows. On Windows, run the commands from a shell where Node is on your PATH (a fresh terminal after installing Node usually suffices). If a source build complains about pnpm, run corepack enable first so the pinned pnpm 11.7.0 shim is available. Windows support got materially better in 0.1.0-rc.8: the agent now has a native persistent PowerShell tool, and the minimal preset mounts it automatically on win32 instead of the bash-only shell.

Node ^22.19.0 or >=24.0.0. Anything older will fail the engine check. If your system Node is out of range, use a version manager (nvm on macOS/Linux, nvm-windows on Windows) to install a supported version, open a new shell, and re-run the install command.

Not to install itânpx @deepseek-ai/dsh web starts the Web UI without any key. You need a key (or another configured provider) before the agent can actually call a model. Add it under Settings â Models; the route becomes usable immediately without restarting the server.

The v0.1 preview docs don't pin down a storage location, and because storage itself is a plugin, the answer can change between preview releases. Rather than repeat a guess that goes stale, check the current user guide (docs/user/guide/index.md) in the GitHub repo, or ask in GitHub Discussions.

You haven't selected a workspace yet. The session composer stays unavailable until you click Choose workspace, add the project directory where you started dsh, and select it. Once a workspace is active, the composer unlocks and you can start a session.

Partly, since 0.1.1-rc.1. The harness gained a credential record store and an authorization seam, so a provider plugin can run a real login flow and keep what it produces â which is what brought openai-codex, an OAuth-only provider, back into the picker. The browser control that would start that login has not shipped yet, so in the Web UI you still add a key by typing it into Settings â Models. See the sign-in section above for what exists today and what does not.

## Get DeepSeek Harness on GitHub

The repo is the source of truth for a fast-moving developer preview: the README quick start, the development guide, and the release notes for each breaking change. Clone it, star it, and file issues where the maintainers actually look.
