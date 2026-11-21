---
id: collect-261121-veille-ia/muse/muse-code
title: "Muse Code"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "code", "session", "agent", "runs", "model", "build", "install"]
source: https://dev.meta.ai/docs/muse-code?team_id=1741581793652561&project_id=29035399122726954
source_anchor: ""
source_lines: [1, 21]
section: "Muse"
---
Muse Code is Meta's coding agent for the terminal and CI, built for Muse Spark. Run it in a project and it plans, edits, and runs commands to do a task, with approvals and an OS sandbox on from the first run.

Muse Code and the Model API are two ways to use the same model. Call the API directly when you build your own agent or app. Run Muse Code when you want a ready-made coding agent at the command line or in a pipeline.

Workflows coordinate parallel and staged agent work when the installed build includes the workflow engine and the rollout is enabled. They are not available on every build or platform. Session messaging lets independent live sessions exchange handoffs, review requests, and status updates.

Install Muse Code with the one-line installer. It puts a native `muse` binary on your path.

Confirm it's on your path:

Run `muse` in any project directory to start an interactive session. On first entry Muse Code asks whether to trust the workspace (which loads its skills, rules, and hooks) and prompts you to authenticate with a browser sign-in or an API key. The default model is `muse-spark-1.2`.

Reopen the sign-in options any time from an interactive session with `/login`. For a non-interactive environment such as CI, set `META_API_KEY` instead. See authentication and billing for the full sign-in and billing setup flows.

The same binary runs two ways:

Muse Code runs on macOS, Linux, and Windows from one codebase. A few capabilities work differently on Windows:

- **PowerShell replaces Bash.** Agent-run commands use PowerShell syntax and Windows command-line tools.

Beyond the CLI, you can drive a Muse Code session from your own program over a stable, versioned session protocol. `muse serve` runs the protocol, and `muse schema` prints its JSON schema. The TypeScript SDK wraps the protocol: install it with `npm install @muse-code/sdk`. A quickstart, cookbook recipes, and a generated API reference are in the Muse Code SDK docs(opens in new tab).
