---
id: collect-261121-veille-ia/deepseek/deepseek-harness-studio-readme-en-md-at-257d429156a243517a2cafdff777125cd3fb6d54-fufankeji-deepseek-
title: "deepseek-harness-studio/README.en.md at 257d429156a243517a2cafdff777125cd3fb6d54 · fufankeji/deepseek-harness-studio"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["agent", "deepseek-harness-studio", "desktop", "deepseek", "vision", "local", "session", "inspect"]
source: https://github.com/fufankeji/deepseek-harness-studio/blob/HEAD/README.en.md
source_anchor: ""
source_lines: [1, 171]
section: "DeepSeek"
---
**Official Website** · 中文 · **English**

**Built by Beyondata · Zero-code desktop enhancements for DeepSeek Harness**

**Vision enhancement + local models + Plugin Store + Preset Square · One-click deployment**

Automatically discover and surface new ecosystem plugins, with AI recommendations for useful capabilities; search, verify, install, enable, disable, and uninstall without the command line.

**Download the macOS arm64 development preview** · **Download the Windows x64 development preview**

This table lists capabilities that are present in source, included in the Desktop composition, and reachable through a user workflow. Longer-term ideas are kept separate.


| Capability | What it enables | 
|---|---|
| **Desktop workspaces and session management** | Open local projects through the native directory picker, organize and search sessions by Workspace, and rename, archive, fork, or resume session history. | 
| **Plugin Discovery and Agent recommendations** | Browse featured, recently updated, and ecosystem-popular plugins, filter or search by scenario, or describe a need and let the Agent shortlist the public `dsh-plugin` catalog. | 
| **Trusted plugin lifecycle and recovery** | Review exact versions, permissions, compatibility, and risk before installation; enable, disable, update, or uninstall later; automatically roll back or expose a retryable recovery flow for unfinished transactions. | 
| **Preset Square and seven built-in workflows** | Browse Fufan Official and community Agent Presets, inspect Skills, tools, and environment requirements, install them, and start a new session from the installed list. | 
| **Application Center and FF–LLM Wiki** | Launch complete AI applications with their own interface, data, and runtime flow from a first-class page, with an optional application shortcut in the sidebar. | 
| **Multiple providers and local inference** | Configure DeepSeek and compatible providers, or use the dedicated local-model flow for Ollama, vLLM, SGLang, and custom OpenAI-compatible services. | 
| **Native vision, compatible vision, and image attachments** | Send images directly to an image-capable DeepSeek model or use a verified cloud/self-hosted vision route; attachments persist and each image follows exactly one request path. | 
| **Plan, Goal, Todo, Jobs, and Workflow** | Enter plan mode, manage goals and todos, inspect background work in the current process, and review the members and outcomes of multi-stage Workflows in chat. | 
| **SubAgents and multi-Agent collaboration** | Create one-shot or continuable subagents, inspect parent/child lineage and runtime status, and continue or stop the current turn of supported child sessions. | 
| **Project rules, context references, and deliverables** | Load repository instructions, reference `@file` or`@session` context, and inspect, open, or reveal files the Agent actually produced. | 
| **Permissions, sandboxing, and human confirmation** | Select read-only, workspace-write, or full access for current or future sessions; confirm dangerous access, tool approvals, and Agent questions explicitly in the UI. | 
| **Themes and cross-platform Desktop delivery** | Use built-in or local backgrounds with adapted interface colors, and download macOS arm64 or Windows x64 previews from GitHub Releases. | 

These directions do not yet have complete first-class product journeys and are not counted as current features.


| Direction | Product work still required | 
|---|---|
| **Standalone capability center** | Discover, connect, and compose MCP servers, Skills, and tools that are not distributed as Bundles, with project-level management. | 
| **Visual Agent composition** | Add editors for custom Agents, role assignment, and team workflows on top of the existing Preset and SubAgent runtime. | 
| **Remote control and automation** | Add browser/desktop operation, mobile continuation, and notification channels behind explicit permission and audit boundaries. | 

DeepSeek Harness Studio uses Electron to host the DeepSeek Harness Web workspace. The desktop main process starts and manages a local `dsh web` service. This repository provides the complete development source so users can clone or download it, install dependencies, edit the code, launch the desktop app, and continue development.

Desktop installers are published only through this repository's GitHub Releases page, never through a third-party download site. Electron-validated macOS arm64 and Windows x64 development previews are available now, while the complete source remains available for local development.

- **Workspaces and sessions** : choose a local directory natively; group, search, or unregister Workspaces; rename, archive, or fork a session at its last completed turn.
- **Planning and work management** : organize work with Plan, Goal, and Todo. The Jobs panel shows background work owned by the current process; a process restart does not present those old jobs as still running.
- **Workflows and SubAgents** : chat reconstructs Workflow phases, members, and outcomes. The SubAgent directory exposes parent/child lineage, continuable conversations, and stopping the current turn of a running continuable child.
- **References and deliverables** :`@file` and`@session` supply context. Successfully produced files appear at the end of the answer and can be opened or revealed through the local Host.
- **Human collaboration** : Agents can ask structured single-choice, multiple-choice, or custom questions. Tool approvals, full access, and plan review require an explicit UI decision.
- **Security boundaries** : permission presets bind sandbox and approval policy to each session. Credentials are stored through a write-only flow; the page never reads back or displays a stored secret value.

Studio `0.1.0-rc.19` integrates the core and Web capabilities from DeepSeek Harness `0.1.1-rc.2` while retaining Beyondata's Plugin Center, Plugin Discovery, Preset Square, Application Center, themes, and desktop recovery flows. Studio and upstream Harness versions are managed independently; the download links above match this release.

- **Multimodal capability** : retains Pro and Flash text models while adding`DeepSeek-V4-Flash-Vision-Exp` , persistent image attachments, and reusable Files API uploads; stale references retry within bounds and file-resolution failure falls back for the whole request to bounded inline images.
- **Agent runtime** : integrates`@` file/session references, Plan, Goal, background Jobs, Workflow, SubAgents, concurrent Web Search, and persistent PowerShell PTY sessions on Windows.
- **Desktop adaptation** : starts the Host with`--no-open` , retains native directory selection, plugin transaction recovery, and existing user-data locations, and performs lockfile-free compatibility recovery for incompatible historical plugin locks.

Not sure where to find plugins, which ones were updated recently, or what the ecosystem is paying attention to? Open **Plugin Discovery** from the sidebar. The app reads the online catalog automatically and turns scattered packages into a recommendation page you can browse and act on directly.

  
  <sub>Real Desktop interface: catalog feature, recently updated, ecosystem popular, scenario filters, search, and install or management actions.</sub>

- **A fresh place to start every day** : see catalog features, recently updated entries, and ecosystem-popular plugins without searching repositories one by one.
- **Filter by scenario** : browse Agents and workflows, Web UI, browser and search, vision and media, memory and context, models and services, developer tools, or integrations and notifications.
- **Search for the answer directly** : search by plugin name, capability keyword, or publisher, then inspect its icon, summary, version, and update time.
- **Act as soon as you discover it** : start the trusted installation flow for a new plugin, or jump to Plugin Center management for an installed one.

When all you know is “I want a desktop pet,” you do not need to guess an npm package name first. Enter the need in **Plugin Discovery** and the app sends it to the current Agent as a `/find-plugins` request. The Agent loads the built-in Skill, performs a read-only search of the public `dsh-plugin` catalog, and returns the closest candidates with versions, publishers, update dates, and matching reasons in the current conversation.

  
  <sub>Real Desktop acceptance run: the conversation asks for a desktop-pet plugin; the Agent loads `find-plugins`, searches the public catalog, and lists five relevant candidates from eight results.</sub>

- **No catalog vocabulary required** : describe the outcome, use case, or problem in ordinary language.
- **Evidence stays inspectable** : each result includes the exact package name, version, publisher, update date, and a matching reason.
- **Search and installation remain separate decisions** : recommendations are public-catalog metadata only; after choosing a package, use**Plugin Center** for compatibility checks and installation confirmation.

  
  <sub>Real Desktop interface: plugin avatars, public catalog, Installed area, Install buttons, and three-dot management actions.</sub>

After choosing a plugin, open **Plugin Center** and search by short package name, full npm name, or an explicit GitHub repository for plugins and Skill Packs published to the public npm Registry in the DeepSeek Harness Bundle format. The `dsh-plugin` keyword is only a discovery signal, and GitHub is only used to map back to a published npm package; source is never installed directly.

- **Online discovery** : search public plugins and inspect versions, capabilities, permissions, compatibility, and risk.
- **One-click installation** : download and verify an exact package version, integrity metadata, and Bundle declaration; after confirmation, Desktop installs it, restarts the Harness Host, and verifies runtime state.
- **Installed management** : review system, public-catalog, and local sources together, then enable, disable, update, or uninstall an entry from its three-dot menu.
- **Safe removal** : uninstall retains configuration and plugin data by default; deleting data requires a separate user confirmation.

Open **Settings → Background** to switch built-in skins. For a custom image, the app performs the 1920×1080 WebP crop and interface color adaptation locally without uploading the original.

| **Whale Maid · Default**Two blue-and-white whale assistants frame a bright palace while the center remains clear for conversation. | **Cloud Cat**The original soft blue-and-white cat theme remains available as a calm, low-distraction option. | 

- **Permission selection** : the composer uses the Chinese`只读` ,`工作区写入` , and`完全访问` labels for the current session. General settings affect only new sessions, and enabling Full access requires an explicit risk confirmation.
- **Model and thinking modes** : the model and API key remain managed in Settings. The composer shows the current DeepSeek model and offers`关闭思考` ,`低强度思考` ,`深度思考` , and`最大思考` .

Studio can connect to model services running on the same machine or local network through standard OpenAI-compatible interfaces. Users start the inference server and load the model first; Studio does not download model weights, allocate deployment storage, or manage GPU runtime parameters.

| Framework | Default API base | Integration | 
|---|---|---|
| **Ollama** | `http://127.0.0.1:11434/v1` | Uses Ollama's OpenAI compatibility interface with the actual model ID; a local API key may be omitted. | 
| **vLLM** | `http://127.0.0.1:8000/v1` | Connects to a vLLM OpenAI-compatible server with its loaded model ID and optional API key. | 
| **SGLang** | `http://127.0.0.1:30000/v1` | Connects to an SGLang OpenAI-compatible endpoint with its running model ID and optional API key. | 
| **Custom service** | User supplied | Supports other HTTP(S) services compatible with `/v1/chat/completions` . | 

- **Conversation models** : open**Settings → Models → Add local model** , choose Ollama, vLLM, SGLang, or OpenAI-compatible, then fetch or enter a model id; the API base is prefilled and the local API key is optional.
- **Local vision models** : choose Ollama, vLLM, SGLang, or a custom service in vision-enhancement setup; the configuration is saved only after a real image succeeds.
- **No silent cloud fallback** : a failed self-hosted route never forwards the image to Bailian, OpenRouter, or another cloud provider.

When vision enhancement is enabled, the Host selects one route from the current exact model's capabilities: a model that declares image support receives the original image; otherwise, a configured and verified Bailian or OpenRouter-compatible provider supplies a traceable observation to the Agent. The routes are mutually exclusive, so each image is processed once.

- **Available in the composer** : use the “视觉增强” shortcut on the left side of the input bar; hover to see its purpose and current state.
- **Automatic path selection** : the shortcut reports`原生` or`兼容 · 提供方` ; native vision needs no extra key, while compatible vision requires a verified provider.
- **Explicitly off** : disabling the switch keeps images out of model-visible context while retaining attachment history in the interface.
- **Built for development work** : understand product screenshots, error dialogs, designs, charts, photos, and text in images, or inspect an image by its workspace path.

GitHub Releases provides Electron-validated macOS Apple Silicon and Windows x64 development previews. Running either desktop build requires no separate Node.js or pnpm installation. These remain development-preview assets; formal releases will provide platform-signed macOS `.dmg` and Windows x64 `.exe` installers.


**Download the macOS arm64 preview** · **Download the Windows x64 installer**

Development previews use a separate pre-release tag without triggering the formal installer workflow. Their public download area contains only the macOS ZIP and Windows installer; checksums, blockmaps, and platform-verification records remain in the corresponding GitHub Actions artifact so users do not mistake development files for installers. The formal workflow accepts only a `desktop-v*` tag that exactly matches the Desktop version, and publishes the macOS and Windows installers with `SHA256SUMS` only after both platform signatures pass verification.

Clone the repository with Git:

```
git clone https://github.com/fufankeji/deepseek-harness-studio.git
cd deepseek-harness-studio
```
You can also choose **Code → Download ZIP** on the GitHub repository page, extract the archive, and open the project directory.

- Node.js `^22.19.0 || >=24.0.0`
- pnpm `11.7.0`

Downloading the source, installing dependencies, and launching the desktop development environment do not require an API key. Configure the selected model provider and credentials in the application settings only when making model requests, and never commit credentials to Git.

Install the workspace dependencies:

`pnpm install`
Build the required modules and launch the desktop development environment:

`pnpm run dev:desktop`
The development launcher rebuilds when relevant source or build inputs change. To force a complete rebuild, run:

`pnpm run dev:desktop:rebuild````
deepseek-harness-studio/
├── apps/
│   ├── desktop/       # Electron main process, preload, Host lifecycle, and desktop build scripts
│   ├── web/           # DeepSeek Harness Web entry and desktop composition
│   └── cli/           # dsh CLI, runtime configuration, and Agent Presets
├── packages/          # Agent, model, tool, session, plugin, and client capability packages
├── native/            # Native sandbox helpers
├── python/            # Python SDK and related runtime
├── examples/          # Runnable examples and configurations
├── scripts/           # Build, validation, generation, and publishing scripts
├── website/           # Documentation site source
├── vendor/            # Pinned Cordis foundation source
└── assets/            # Project images used by the README
```
| Command | Purpose | 
|---|---|
| `pnpm run dev:desktop` | Build required modules and launch the Electron desktop app | 
| `pnpm run dev:desktop:rebuild` | Force a complete rebuild before launching the desktop app | 
| `pnpm run build` | Build the Host, client, Web, and desktop app | 
| `pnpm run package:desktop` | Create an unpacked desktop app for the current platform | 
| `pnpm run typecheck` | Run TypeScript type checks | 
| `pnpm run test` | Run the Vitest unit suite | 

1. `apps/desktop/src/main.ts` : desktop entry, window, tray, and local Host composition.
2. `apps/desktop/src/host-supervisor.ts` :`dsh web` startup, readiness detection, and shutdown.
3. `apps/desktop/src/preload.ts` : fixed desktop interfaces exposed to the renderer.
4. `apps/web/` : the Web workspace loaded by the desktop window.
5. `apps/cli/` and`packages/` : CLI composition and Harness capabilities.

This project continues desktop development from the Harness core, Cordis plugin system, and Web interface in deepseek-ai/deepseek-harness. This repository maintains the Electron desktop entry, local Host management, desktop interactions, and supporting development scripts.

This project uses the MIT License. Third-party license information is available in THIRD_PARTY_NOTICES.md.
