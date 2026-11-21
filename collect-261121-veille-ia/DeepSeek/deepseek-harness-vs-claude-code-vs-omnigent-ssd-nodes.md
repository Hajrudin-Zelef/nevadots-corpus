---
id: collect-261121-veille-ia/deepseek/deepseek-harness-vs-claude-code-vs-omnigent-ssd-nodes
title: "DeepSeek Harness vs Claude Code vs Omnigent · SSD Nodes"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-18"]
keywords: ["deepseek", "harness", "claude", "code", "profile", "omnigent", "plugin", "pnpm"]
source: https://www.ssdnodes.com/learn/dsh-vs-claude-code-vs-omnigent
source_anchor: ""
source_lines: [1, 156]
section: "DeepSeek"
---
## DeepSeek Harness vs Claude Code vs Omnigent: which one you need

DeepSeek Harness, Claude Code and Omnigent are all called agent harnesses, and they solve different parts of the problem. DeepSeek Harness is a plugin runtime with a web UI (user interface) where you supply the model. Claude Code is a terminal program from Anthropic that runs Anthropic models. Omnigent is an orchestrator from Databricks that drives other harnesses, including Claude Code, instead of implementing an agent loop of its own. None of them is a drop-in replacement for another, so the question worth answering is which layer you are missing. If that framing is new, what an agent harness actually is sets out the parts these three divide up differently: the loop, the tools, the permissions and the session state.

The axes that actually differ are architecture, model coupling, interface, maturity and licence. Versions and dates below were first checked on 18 August 2026 and rechecked against each project's own docs, release pages and package registries on 20 October 2026. Version numbers, release status and runtime requirements were checked again on 27 October 2026. Where a sentence gives its own date, that date applies. Two of the three move fast enough that you should check again before you commit to one.

## What is DeepSeek Harness, and what does the plugin model buy you?

DeepSeek published DeepSeek Harness, or `dsh`, at github.com/deepseek-ai/deepseek-harness in August 2026. The repository description states the whole design in four words: everything is a plugin. The model adapter, the tool registry, the session log, the sandbox and the agent loop itself are all plugins, loaded by a runtime built on Cordis. Adoption was unusually fast, and the repository passed 150,000 stars within days of publication (checked 18 August 2026).

DeepSeek Harness is in developer preview and iterating rapidly. THERE WILL BE COMPATIBILITY-BREAKING CHANGES.

That is the README's own warning, and it is the most important sentence in this comparison. Anything you write against the plugin API (application programming interface) today is something you should expect to rewrite.

Start it with npx, which needs Node.js installed:

`npx @deepseek-ai/dsh web`
The command prints its URL, and on a local launch it also opens that URL in your default browser. By default the web UI is served at `http://127.0.0.1:3080`, and the printed URL carries a fresh process token that signs the browser in, so open that exact URL rather than typing the bare address. The address is a deliberate bind rather than a cosmetic default, and what binding to 127.0.0.1 means for reaching the UI starts to matter the moment `dsh` is running on a different machine from your browser. As of 27 October 2026, every published build was a prerelease, either a release candidate or an alpha, and none was a stable release (when npm's `latest` tag still pointed at `0.2.1-rc.2` and its `next` tag had moved ahead to `0.2.1-rc.4`). So if that command fails or boots a version you did not expect, pinning an exact `dsh` version and clearing the npx cache is the fastest way out. Building from source uses pnpm instead:

```
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```
`dsh web` is shorthand for `dsh --profile web`. A profile is a directory under `$DSH_HOME/profiles/`, and the launcher boots whichever one you name. That matters because there is a headless profile as well: `dsh --profile headless "job"` runs one persisted session, prints the result and exits, which is the shape you want in a cron job or a build step. Launcher flags such as `--profile` and `--patch` must come before the application's own arguments, because the launcher stops parsing at the handoff. A flag like `--port` belongs to the booted profile, not to the launcher, so its position matters. Plugins are managed by forwarding arguments to pnpm, as in `dsh plugin --profile web add <package>`. A plugin loads into the same runtime as the agent and inherits its permissions, so check what a plugin can reach before you install it.

## What is Claude Code, and why is it a terminal program?

Claude Code is Anthropic's own CLI (command line interface) agent. The CLI has no web UI of its own. Anthropic also offers Claude Code on the web, in a desktop app and in IDE extensions, but those are separate surfaces, and on a server the CLI is what you run. It runs in your terminal, in the directory you want it to work on, which is why it fits a server session so well. With no interface to configure, you shape how it answers with text rather than settings panels, and an output style swaps out its system prompt so every reply in the session comes back in a different shape. Installation is one command on macOS, Linux and WSL:

```
curl -fsSL https://claude.ai/install.sh | bash
claude --version
```
A working install prints a version and the product name, for example `2.1.211 (Claude Code)`. If that fails, `claude doctor` prints read-only installation and settings diagnostics without starting a session. There is an npm package too, `npm install -g @anthropic-ai/claude-code`, which requires Node.js 22 or later. It installs the same native binary, so the installed `claude` does not use your Node.js at runtime. Anthropic also publishes signed apt, dnf and apk repositories for Debian, Ubuntu, Fedora, RHEL and Alpine. On a server you manage with configuration management those are the better choice, because updates then arrive through your normal upgrade workflow.

Anthropic's setup docs say Claude Code requires a Pro, Max, Ultra, Team, Enterprise or Console account, or access through a supported cloud provider: Amazon Bedrock, Google Cloud's Agent Platform or Microsoft Foundry. The free Claude.ai plan does not include access. Ollama also documents a route, `ollama launch claude`, that points Claude Code at local models through Ollama's Anthropic-compatible API with no Anthropic credential in the configuration, but you are then running someone else's model rather than Claude. For Anthropic's own models, deciding between a Claude subscription and API billing changes what you pay far more than any setting inside the tool does.

## What is Omnigent, and what does a meta-harness orchestrate?

Databricks announced Omnigent in June 2026 and released it under the Apache 2.0 licence. It calls itself a meta-harness. It does not implement an agent loop. It drives the ones that already exist: Claude, Codex, Cursor, OpenCode, Hermes, Pi and OpenAI's agents SDK (software development kit), and by October 2026 the README also named GitHub Copilot, Antigravity, Devin, Kiro and Grok Build. Several of those appear twice in the harness list, once in a form that talks to the vendor's SDK and once in a native form that wraps the vendor's real terminal program.

An agent is a YAML file, YAML being a plain text configuration format:

```
name: my_agent
prompt: You are a helpful data analyst.
executor:
  harness: claude-sdk
tools:
  word_count:
    type: function
    callable: mypackage.mymodule.word_count
  docs:
    type: mcp
    url: https://example.com/mcp
  researcher:
    type: agent
    prompt: Search for relevant information.
```
The `executor.harness` line is the point of the whole project. Change `claude-sdk` to `codex` and the same prompt with the same tools runs on a different vendor. A tool is a Python function, an MCP (model context protocol) server, or another agent, and that last case is how multi-agent work is expressed: a sub-agent is simply a tool. If you have not settled where skills, MCP servers and rules files each belong, the difference between agent skills, MCP and rules files is worth reading first, because Omnigent inherits whichever of those the harness underneath supports.

The shipped example makes the idea concrete. Polly is a coding orchestrator that writes no code itself. It plans, delegates work to coding sub-agents in parallel git worktrees, then routes each diff to a reviewer running on a different vendor's model.

Install it with the published script:

`curl -fsSL https://raw.githubusercontent.com/omnigent-ai/omnigent/main/scripts/install_oss.sh | sh`
Or install it manually, which needs Python 3.12 or newer:

```
uv tool install omnigent
omnigent setup
omnigent start
```
`pip install "omnigent"` works as well, and extras are selected the usual way with `uv tool install "omnigent[databricks,modal]"`. `omnigent setup` collects credentials and defaults. `omnigent start` starts the local server, registers the machine as a host, and prints the URL to open. `omnigent server status` and `omnigent stop` are the other two commands you will use daily. A bare `omnigent` does not launch an agent. Both `omnigent` and `omni` invoke the same entry point: in an interactive terminal, the bare command starts the local server and host in the background and returns; without a TTY, it prints help. Launch an agent with `omni claude`, `omni polly` or `omni run agent.yaml`.

The dependency list is longer than the one-line installer suggests: Python 3.12 or newer, uv, git, Node.js 22 LTS (long term support) or newer with npm and pnpm, tmux for the native terminal wrappers, and bubblewrap (`bwrap`) for the Linux sandbox. That is a direct consequence of the design. Omnigent supervises vendor CLIs and installs some automatically, while others require separate installation, so it is Python software whose job includes running Node.js software.

## Who supplies the model, and who pays for it?

This is the axis with the largest practical consequences, and the three answers are genuinely different.

DeepSeek Harness is bring-your-own. The installed provider catalogue ships providers such as Anthropic, OpenAI, Kimi (`moonshotai`) and GLM (`zai`), next to a card for DeepSeek's own API. It also lists providers without native key authentication, such as `openai-codex`, which show no key field unless the profile explicitly names a credential reference (checked 20 October 2026). A custom provider entry takes a lowercase provider id, a display name, a base URL, an API protocol, a credential and at least one model, which is how you point it at a company gateway or a self-hosted server. Keys are written to `$DSH_HOME/.credentials.yaml`, and the provider settings in the profile's `cordis.patch.yml` (for `dsh web`, `$DSH_HOME/profiles/web/cordis.patch.yml`) keep only a reference to them, which means the settings file is safe to share and the credentials file is not. A gateway entry can name an environment variable instead, with a line such as `apiKeyEnv: GATEWAY_API_KEY`. Despite the project name, nothing in it forces you onto DeepSeek's own models. If you want to see those files filled in for a real key or aimed at a local Ollama endpoint, wiring up dsh keys, models and endpoints walks through each mode and what does or does not leave your machine in it.

Claude Code is built around one vendor's models. You log in with a paid account, or you set `ANTHROPIC_API_KEY` and Claude Code prompts you once to approve that key instead of opening a browser. That second path is the one that works on a headless server. Organisations that already buy capacity elsewhere can route through Amazon Bedrock, Google Cloud's Agent Platform or Microsoft Foundry. Ollama's Anthropic-compatible API is the documented way to point it at local models instead, at the cost of giving up Claude itself.

Omnigent needs whatever the harness underneath needs, gathered by `omnigent setup`. It accepts API keys, existing CLI subscriptions, OpenAI-compatible or Anthropic-compatible gateways, and Databricks workspaces. That is the real cost of a meta-harness: one machine ends up holding every vendor's credentials. Read how to keep secrets out of an AI agent's reach before that machine goes anywhere near the public internet.

## How mature is each one, as of August 2026?

Claude Code has been generally available since 2025 and was still on the 2.1.x line on 29 October 2026, when the newest changelog entry was 2.1.312. It auto-updates in the background, offers `latest` and `stable` release channels, publishes GPG-signed release manifests, and documents a `minimumVersion` pin for organisations that need a floor. That is the profile of a product people already run in production.

Omnigent has been public since June 2026 and is still in the 0.x series. On 29 October 2026 the newest release on PyPI was 0.20.0, uploaded on 28 October. PyPI lists those numbered versions, 0.1.0 onwards, as ordinary releases rather than prereleases, with release candidates published separately. Even so, the README labels the project alpha. Expect both the harness list and the YAML schema to keep moving. It is usable, and it is not yet stable.

DeepSeek Harness is the youngest by a wide margin. It was published in August 2026, days before the first version of this comparison was written. Its own README promises compatibility-breaking changes, and as of 27 October 2026 every build published to npm was still a prerelease. A very large star count tells you many people are watching it. It tells you nothing about whether the plugin API has settled.

## What licence does each one carry?

DeepSeek Harness is MIT, with third-party licences disclosed in `THIRD_PARTY_NOTICES.md`. Omnigent is Apache 2.0. Claude Code is not open source: it is Anthropic's commercial product, and running it against Anthropic's models requires a paid plan, billed API access or a supported cloud provider. If your requirement is that you can read and patch every line, Claude Code is ruled out before any feature comparison begins. If your requirement is that the tool keeps working next month without your attention, that requirement points the other way.

## What does it take to run each one on a VPS?

Claude Code is the simplest of the three. Install it, log in, and run it inside tmux so the session survives a dropped SSH (secure shell) connection. Anthropic documents 4 GB of RAM (random access memory) or more and an x64 or ARM64 processor, on Ubuntu 20.04 or later, Debian 10 or later, or Alpine 3.19 or later. On Alpine you also need `bash` and `curl` installed first, because the install command fails with a `not found` error without them. Running Claude Code on a VPS inside tmux covers the session handling in detail.

DeepSeek Harness needs Node.js and a browser pointed at its web UI. The README asks for Node.js without naming a minimum version. On 27 October 2026 the repository's root `package.json` declared `>=24.9.0`, raised from the `^22.19.0 || >=24.0.0` range that earlier tagged releases carry, so Node 22 is no longer enough and Node 24.9 or newer is the safe pick. Do not publish port 3080. Leave the server on `127.0.0.1` and forward the port from your laptop:

`ssh -N -L 3080:127.0.0.1:3080 you@your-vps`
Then open the URL `dsh` printed, token included, in your local browser. Over SSH it does not try to open a browser for you. A full DeepSeek Harness install on a VPS covers the profile layout and the plugin commands.

Omnigent is the heaviest, because it installs and supervises other people's CLIs. It wants Python 3.12, uv, git, Node.js 22, npm, pnpm, tmux and bubblewrap on the same box, and some vendor CLIs require separate installation. For native Kiro, install and log in to `kiro-cli` first: the launcher fails if the CLI is missing from `PATH`, unless you point `OMNIGENT_KIRO_PATH` at it. Its README says the local UI can be opened from a phone at the machine's LAN address, so on a VPS do not assume port 6767 is private: block it in your firewall and forward it the same way:

`ssh -N -L 6767:127.0.0.1:6767 you@your-vps`
For a server other people use, the supported path is the Docker deployment under `deploy/`, which enables multi-user accounts by default. The same behaviour is available on a manually started server by setting `OMNIGENT_AUTH_ENABLED=1`, and OIDC (OpenID Connect) login works against Google, GitHub, Okta and Microsoft. The Omnigent multi-agent harness guide covers the sandbox providers, which start to matter as soon as agents write to disk.

## Why is the web UI blank on your server's public IP address?

The obvious move is to bind a web harness to a public address and open the port. The two behave differently here. DeepSeek Harness now blocks the worst version of that: its `--host` flag takes one concrete local address, and a wildcard such as `0.0.0.0` is rejected at startup. Its printed URL carries a process token, and that token plus a signed session cookie authenticates every API call, which also means that over plain HTTP the token crosses the network unencrypted. Omnigent's bare local server leaves multi-user accounts off, so an open Omnigent port is an open shell on your VPS.

The second thing that breaks is a browser rule that surprises people. `crypto.randomUUID()` is only defined in a secure context, which means HTTPS (hypertext transfer protocol secure), `localhost` or `127.0.0.1`. When this comparison was first written in August 2026, loading the DeepSeek Harness UI over a plain address such as `http://203.0.113.10:3080` made the browser console report:

`TypeError: crypto.randomUUID is not a function`
The page then rendered partly or not at all, because session creation failed before anything could be drawn. Nothing was broken on the server, and the logs showed a normal request. The origin was not trusted by the browser, so a function the front end depended on was never defined. An SSH tunnel avoids this with no certificate, because `127.0.0.1` counts as a secure context. A reverse proxy holding a real TLS (transport layer security) certificate works too and keeps the `dsh` launch token off the wire, and in front of Omnigent you also have to turn authentication on yourself.

## Which one fits you?

Pick Claude Code if you want a coding agent that works today, you are willing to pay Anthropic, and the terminal is where you already work. It is the only one of the three that is a finished commercial product rather than a preview.

Pick DeepSeek Harness if the plugin model is the point: you want to replace the model adapter, the sandbox or the agent loop, and you accept that the interface you write against will change. Its provider catalogue covers Anthropic and OpenAI, so choosing it is a bet on the runtime, not on DeepSeek's models.

Pick Omnigent if you already run more than one agent CLI and your actual problem is coordination: policy, sandboxing, review by a second vendor's model, and a shared session other people can watch. It adds a layer to your stack, and a layer you do not need is a layer that can break. If your coordination problem is smaller than that, two Claude Code sessions on the same box can send messages to each other without an orchestrator in between.

If you are still shortlisting, the Mimo Code AI coding agent sits in the same terminal-agent category as Claude Code, and running coding agents in a disposable VM is worth reading whichever one you choose, because none of these three makes a careless `rm -rf` safe on its own. None of the three decides how large a diff the agent proposes either, which is a prompt-level question, so a skill that pushes an agent towards the smallest change that works is worth layering on top of whichever harness you land on.

## FAQ

### Do I need a paid Anthropic plan to use Claude Code?

For Anthropic's models, yes, or a supported cloud provider. Anthropic's setup docs say Claude Code requires a Pro, Max, Ultra, Team, Enterprise or Console account, and the free Claude.ai plan does not include access. Organisations with capacity elsewhere can point it at Amazon Bedrock, Google Cloud's Agent Platform or Microsoft Foundry instead. On a headless VPS, set `ANTHROPIC_API_KEY` before the first run: Claude Code then prompts you once to approve that key instead of trying to open a browser. Ollama documents a separate route, `ollama launch claude`, that runs Claude Code against local models through Ollama's Anthropic-compatible API with no Anthropic credential involved, but then the model doing the work is not Claude.

### Can DeepSeek Harness run Claude or OpenAI models?

Yes. The installed provider catalogue includes Anthropic and OpenAI among others, and you add a key per provider under Settings then Models. Anything outside the catalogue goes in as a custom provider, which needs a lowercase provider id, a base URL, an API protocol, a credential and at least one model. The catalogue also lists providers without native key authentication, such as `openai-codex`, and those show no key field unless the profile explicitly names a credential reference (checked 20 October 2026). Keys are stored in `$DSH_HOME/.credentials.yaml` while the profile's `cordis.patch.yml` holds only a reference, so back the credentials file up separately and keep it out of version control.

### Does Omnigent replace Claude Code?

No. Omnigent runs Claude Code. Its harness list includes Claude, Codex, Cursor, OpenCode, Hermes and Pi, and an agent selects one with a single `harness:` line in its YAML file. You still authenticate the underlying CLI, and some vendor CLIs require you to install them separately. Omnigent adds orchestration, policy and sandboxing above that, so it earns its dependencies only when you are running more than one agent or more than one vendor.

### Why is the DeepSeek Harness web UI blank on my server's public IP address?

In August 2026 the cause was that a plain HTTP origin is not a secure context, so `crypto.randomUUID()` is undefined there and the browser console reported `TypeError: crypto.randomUUID is not a function`. Session creation failed, so the page rendered partly or not at all while the server logs looked normal. Current builds also reject a wildcard `--host` and put a process token in the printed URL, so the bare address alone will not sign you in. Forward the port over SSH with `ssh -N -L 3080:127.0.0.1:3080 you@your-vps` and open the printed URL at `http://127.0.0.1:3080`, since loopback counts as a secure context. A reverse proxy with a real TLS certificate also works, and it keeps the launch token from crossing the network in plain text.

### How much RAM does a VPS need for these harnesses?

Anthropic documents 4 GB or more for Claude Code. For DeepSeek Harness and Omnigent, measure rather than guess. With remote models, the harness itself is doing process supervision and file work rather than inference; hosting a local model adds its own memory requirements. What really sets the size of the box is the work the agent performs on it, such as a build and a test suite. Omnigent asks for the most, because Python, Node.js, tmux and every vendor CLI you enable run on the same machine.
