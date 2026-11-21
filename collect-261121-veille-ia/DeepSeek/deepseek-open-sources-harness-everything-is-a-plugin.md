---
id: collect-261121-veille-ia/deepseek/deepseek-open-sources-harness-everything-is-a-plugin
title: "DeepSeek Open-Sources Harness: Everything Is a Plugin"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "harness", "august", "benchmark", "license", "everything", "plugin", "deepseek-ai"]
source: https://www.digitalapplied.com/blog/deepseek-harness-open-source-agent-framework-2026
source_anchor: ""
source_lines: [1, 144]
section: "DeepSeek"
---
DeepSeek Harness is the open-source agent framework DeepSeek’s own benchmark tables have been quietly naming for weeks. On August 13, 2026, the deepseek-ai/deepseek-harness repository went public under an MIT license — command name `dsh` — the same day DeepSeek declared V4-Pro generally available. Its architectural bet is stated in the README’s opening lines: everything is a plugin.

That framing matters beyond the launch-day novelty. When DeepSeek published its V4-Flash-0731 and V4-Pro GA benchmark tables, the methodology footnotes credited the minimal mode of DeepSeek Harness as the agent framework — a harness nobody outside DeepSeek could inspect. As of August 13, they can. The published half of that benchmark methodology is now an MIT-licensed repo anyone can clone, read, and rerun.

This post covers what actually shipped: what “everything is a plugin” concretely means in DeepSeek’s own nouns, the four runtime modes and what each is for, the append-only session log that makes runs replayable and forkable, the honest version status behind the “v0.1” label, the corrected install recipe (the one circulating on social media is missing a required step), the benchmark tie-in, and where this sits against Claude Code, Codex, and the open framework landscape.

1. 01It is real, it is DeepSeek's, and it is MIT.The repo is owned by the deepseek-ai GitHub organization, created August 13, 2026, and carries the full MIT license text in its LICENSE file — verified at the file level, not just a README badge. DeepSeek also runs a dedicated product page for it at deepseek.com/harness.
2. 02“Everything is a plugin” has a precise noun list.DeepSeek's own list: models, tools, skills, sessions, sandboxes, storage, loops, scheduling, and the UI. A circulating social-media version says “memory” and “file system” — filesystem access is a swappable seam under the hood, but neither term is DeepSeek's own phrasing, and “memory” appears nowhere in its docs.
3. 03Minimal mode is the published benchmark methodology.The two-tool Minimal mode — persistent bash plus a file editor — is the exact agent framework DeepSeek's model-card footnote names for V4-Pro-0813's vendor-run code-agent benchmarks, and the repo's own BENCHMARK.md documents the eval path. The numbers stay vendor-stated; the harness behind them is now inspectable.
4. 04“v0.1” is a press label, not a shipped version.package.json reads 0.1.0-rc.5 — a release candidate — and the repo has zero GitHub releases and zero tags as of August 14. The README's own words: this is a developer preview, and there will be compatibility-breaking changes. Early software, deliberately so.
5. 05The viral install recipe is missing a step.The circulating clone → install → run sequence skips the required pnpm run build before pnpm dsh web serves the UI at 127.0.0.1:3080. The README also documents a simpler path the social posts omit: npx @deepseek-ai/dsh web, no clone needed.

## 01 — The ReleaseWhat DeepSeek *actually* shipped.

The facts that survive primary-source checking: the repository `deepseek-ai/deepseek-harness` was created at 11:56 UTC on August 13, 2026, under the deepseek-ai GitHub organization — not a fork, not a personal account. The license is MIT, confirmed by fetching the LICENSE file itself (“MIT License / Copyright (c) 2026 DeepSeek”). The command is `dsh`. And DeepSeek treats it as a product, not a code dump: there is a dedicated landing page at deepseek.com/harness and a separate developer-docs site. The landing page ships in both English and Chinese.

The launch drew immediate independent coverage: VentureBeat reported the release same-day, framing it as an open-source rival to the infrastructure under Claude Code and Codex. Interest is measurable but moving too fast to quote as a stable figure: VentureBeat’s August 13 snapshot counted roughly 27,500 GitHub stars; by our own API query on August 14 the count read 88,975 — both numbers are snapshots at stated times, not adoption metrics.

Two smaller signals say something about how DeepSeek expects this to be used. GitHub Issues are disabled — feedback routes through Discussions and a Discord — and the repo’s root `CLAUDE.md` file contains exactly one line of text: “AGENTS.md”. DeepSeek is pointing competitors’ coding agents at its own agent-instructions file, a quiet acknowledgment that Claude Code and its peers will be working inside this codebase from day one.

##### Verified at the LICENSE file

Full standard MIT text, copyright 2026 DeepSeek — checked against the raw file, not just the repo badge. The permissive license is what makes the fork-and-adapt path real rather than theoretical.

##### Nothing pins the “v0.1” label

The /releases and /tags endpoints both return empty as of August 14. The only version marker anywhere is package.json's 0.1.0-rc.5 — a release candidate. “v0.1” is how DeepSeek and the press talk about it, not a shipped cut.

##### Local-first by default

pnpm dsh web serves the interface at 127.0.0.1:3080 per the README. Primary surfaces are a local web UI, a headless command, and a Python SDK — with no DeepSeek-managed hosted service documented for this release.

## 02 — Architecture“Everything is a plugin” — in DeepSeek’s *own* nouns.

The phrase reads like marketing until you see the enumeration. DeepSeek’s own product page lists what is swappable: models, tools, skills, sessions, sandboxes, storage, loops, scheduling, and the UI. The architecture doc goes further — every part of the product is a plugin, including the model adapter, the tool registry, the session log, and the agent loop itself, so every part is replaceable from configuration. Want a different LLM? Register an adapter. Different tool surface? Swap the registry. Different orchestration logic entirely? The loop is a plugin too.

The machinery underneath is not DeepSeek’s invention, and the README says so: Harness is built on Cordis, a pre-existing open-source plugin kernel with its own published design paper. DeepSeek’s contribution is the agent product assembled from that kernel — and a plugin ecosystem play to go with it: the README asks third-party authors to tag their repos with the `dsh-plugin` GitHub topic for discoverability.

The second architectural commitment is the session model. Everything the model sees is recorded in an append-only session log — an event stream that resume, fork, search, and replay all operate on. The architecture doc is explicit that the log is the source of the context the model sees, and that transcripts, telemetry, and persistence all derive from the same stream; there is a first-class API to fork a live session at a boundary. For anyone who has debugged an agent by squinting at scrollback, an append-only, replayable, forkable event log as the core abstraction is the genuinely modern part of this design — the same direction other 2026 harnesses have converged on, as we covered in our look at harness co-training and why the execution environment now shapes model behavior.

*memory*and the

*file system*. Filesystem access is in fact a swappable seam under the hood — the architecture doc routes it through a provider — but neither term is DeepSeek’s own phrasing, and “memory” as a named component appears nowhere in its docs or marketing copy; the closest analog is the session-persistence layer. When citing the architecture, use DeepSeek’s own list: models, tools, skills, sessions, sandboxes, storage, loops, scheduling, and the UI.

## 03 — Runtime ModesFour modes — verified down to the *file system*.

Harness ships four runtime modes, and this is one claim you do not have to take from the marketing page: the repo contains exactly four agent-preset directories on disk, each with an `order` field matching the order DeepSeek presents them. One naming curiosity survives the trip from marketing copy to code: the on-disk preset folder for Creator mode is named `cordis` — after the plugin kernel — not `creator`.

##### *Standard* mode

The full coding agent: file editing, shell, file and web search, skills, planning, goals, subagents, and workflows. This is the daily-driver configuration — the closest analog to what Claude Code or Codex present by default.

##### *Code* mode

Everything Standard has, but tools are exposed through a Code Mode SDK: the model writes one TypeScript program against generated bindings instead of making discrete tool calls. In the registry, run_code is the only wire-level tool under this mode.

##### *Minimal* mode

A deliberately stripped two-tool agent: persistent bash plus str_replace_editor, nothing else. DeepSeek's stated purpose is benchmarking models in a minimal environment — and this is the mode its own published benchmark methodology names.

##### *Creator* mode

Standard's capabilities plus runtime inspection, in-memory plugin experiments, and preset-authoring guidance — the workshop where you inspect the current runtime and combine plugins into new modes. Its on-disk folder is named after the Cordis kernel.

| The four DeepSeek Harness runtime modes cross-referenced against their on-disk preset folders and order fields, their tool surfaces, and whether DeepSeek uses each mode for its published benchmark methodology. |  |  |  | 
|---|---|---|---|
| Mode | On-disk preset | Tool surface | In published benchmarks? | 
|---|---|---|---|
| Standard | `standard` · order 1 | File editing, shell, file + web search, skills, planning, goals, subagents, workflows | Not cited | 
| Code | `code` · order 2 | Standard's capabilities via a Code Mode SDK; run_code is the registry's only wire-level tool | Not cited | 
| Minimal | `minimal` · order 3 | Two tools only: persistent bash + str_replace_editor | Yes — named in the V4-Pro-0813 model-card footnote and the repo's BENCHMARK.md (numbers vendor-stated) | 
| Creator | `cordis` · order 4 | Standard's capabilities + runtime inspection, in-memory plugin experiments, preset authoring | Not cited | 

One honest limit on precision: Code mode carries an internal preset label in Chinese whose abbreviation is not expanded anywhere in the repo’s docs or code. DeepSeek’s public English term is simply “Code mode,” and that is the term worth using — we are not going to guess at an expansion the vendor has not published. The four-mode picture that emerges is a spectrum of control: Minimal strips the agent to two tools for measurement, Standard and Code are the working configurations, and Creator exists so the four shipped presets are a starting point rather than a ceiling.

## 04 — The Tie-InMinimal mode is the *published* benchmark methodology.

Here is the connection that makes this release more than another framework launch. When DeepSeek published its V4-Pro GA changelog entry on the same August 13, the benchmark table came with a methodology footnote — repeated on the Hugging Face model card — stating that the public code-agent tasks were evaluated with the minimal mode of DeepSeek Harness as the agent framework. The V4-Flash-0731 release in July carried the same DeepSeek-harness framing. For the two weeks that followed, that was a label pointing at software nobody outside DeepSeek could examine. Now the two-tool agent behind those rows is public, and the repo’s own `BENCHMARK.md` documents the eval path: install the Python SDK, run the minimal JSON-RPC agent variant, use separate workspaces and session IDs per benchmark task.

VentureBeat drew the sharp version of the implication: some of the agent results arriving alongside Harness are not purely model benchmarks — they measure the model operating inside an agent execution environment, precisely the software layer DeepSeek is now releasing. That cuts both ways. The skeptical reading is that DeepSeek grades its models inside its own gym, and every number in those tables remains vendor-stated with no independent reproduction we are aware of as of this writing. The charitable reading is that DeepSeek has now published the gym: open weights plus an open, MIT-licensed harness make the vendor’s methodology reproducible in principle by anyone, which is more than most vendor benchmark tables offer. The full GA benchmark story — tables, caveats, and the 32-hour release choreography — lives in our V4-Pro GA coverage and the V4-Flash-0731 release analysis; we will not retell it here.

There is a practical angle for teams that run their own model evals: a vendor-published, deliberately minimal, two-tool harness is a useful reference point to calibrate against, whether or not you adopt it. If you are building an eval rig from scratch, start from our guide to building an LLM eval harness — and note that DeepSeek’s design choice validates that guide’s core argument: keep the benchmarking environment small enough to hold constant.

## 05 — Version StatusBilled as v0.1 — actually *0.1.0-rc.5*, untagged.

The launch coverage — DeepSeek’s own page included — talks about “DeepSeek Harness v0.1.” The repository tells a more precise story. The `package.json` version field reads `0.1.0-rc.5`: a release-candidate suffix for a version that has not been finalized. And there is no GitHub Release and no git tag pinning any version at all — both endpoints return empty as of August 14. The only version marker that exists lives in a manifest file. “v0.1” is rounding, not a cut.

None of this is a gotcha — it is DeepSeek’s own framing once you read past the headline. The README states the status in capital letters:

“DeepSeek Harness is currently in developer preview and is iterating rapidly. THERE WILL BE COMPATIBILITY-BREAKING CHANGES.”— DeepSeek Harness README, GitHub, August 2026

Read that as the terms of engagement. Plugin interfaces may shift, presets may be renamed, and anything you build against today’s APIs is provisional. The practical requirements are equally concrete: Node 22.19+ (or 24+), pnpm as the package manager, and a tolerance for a project that has shipped five release candidates without tagging one. For experimentation and for reading the benchmark methodology, none of that matters. For production automation, all of it does.

## 06 — Getting StartedThe install recipe, *corrected*.

The install sequence circulating on social media — clone, `pnpm install`, `pnpm dsh web` — is missing a required step. Per the repo’s own README and the CLI package’s docs, production runs require built package and frontend artifacts, which means an explicit build before the web command works. The verified from-source sequence:

1. `git clone https://github.com/deepseek-ai/deepseek-harness.git`
2. `cd deepseek-harness`
3. `pnpm install`
4. `pnpm run build` — the step the viral recipe omits
5. `pnpm dsh web` — then open`http://127.0.0.1:3080`

The README also documents a simpler path the social posts skip entirely: `npx @deepseek-ai/dsh web` runs the published package with no clone and no build. If you just want to see the product, that is the two-minute route; clone from source when you intend to read or modify the plugins. One trap for anyone scripting against the repo directly: the default branch is `master`, not `main` — raw-file fetches against a `/main/` path 404.

## 07 — The LandscapeAn alternative to the *infrastructure*, not the product.

The most level-headed positioning of the launch came from VentureBeat’s side-by-side comparison: Harness is an open-source, model-agnostic alternative to the agent infrastructure underlying Claude Code and Codex — not yet a full replacement for either product’s broader developer experience. The gaps their table records are real. Harness’s primary interfaces are a local web UI, a headless command, and a Python SDK; Claude Code spans terminal, IDEs, desktop, browser, mobile, and Slack, and Codex has its own multi-surface spread. Neither a DeepSeek-managed hosted background-agent service nor a finished GitHub-native PR workflow is documented for Harness as of the launch.

What Harness offers instead is the thing the closed products structurally cannot: the harness itself is open. Model choice spans DeepSeek, Anthropic, OpenAI, and custom compatible endpoints — any adapter registered on the model seam — and every layer above the model is replaceable and inspectable. VentureBeat put the strategic weight on exactly this layer: models can increasingly be swapped behind standardized interfaces, while the harness that controls how an agent reasons, invokes tools, edits software, and persists across a workflow is much harder to replace. That is the lock-in argument we examined when Codex started importing Cursor skills — portability efforts inside closed harnesses are one answer to it; an MIT-licensed harness where the loop itself is a plugin is the more radical one.

Against the open frameworks, the distinction is altitude. MCP standardizes how tools connect to agents; LangChain and CrewAI give you libraries and abstractions to assemble your own orchestration — our agent-framework comparison maps that terrain. Harness arrives as a different shape: a complete, working agent product — UI, presets, session infrastructure — that happens to be decomposable into plugins all the way down. You start from a finished agent and swap parts, rather than starting from parts and assembling an agent. Whether that inversion wins depends on how stable the plugin interfaces prove — which is exactly what a rc.5 with breaking-changes warnings does not yet promise.

## 08 — The DecisionWho should try it *now* — and who should wait.

The version status makes this an easy call to structure: the question is not whether Harness is good, but whether your use case tolerates a developer preview that promises breaking changes.

##### Try it *now*

Minimal mode is the harness behind DeepSeek's published code-agent numbers, and BENCHMARK.md documents the path. If you evaluate models, being able to run — or at least read — the vendor's own methodology is immediately useful, breaking changes or not.

##### Try it, expect churn

The plugin-kernel design, the append-only forkable session log, and Creator mode are genuinely interesting architecture to study. Build experiments, not dependencies — the README promises compatibility breaks in capital letters, and nothing is tagged.

##### Wait for a tagged release

0.1.0-rc.5, zero releases, zero tags, no hosted service, no documented GitHub-native PR workflow. Teams running agents in production should watch for a first tagged release and interface stabilization before committing anything that matters.

##### No reason to switch yet

Harness does not match either product's surface breadth or hosted workflows today. The reason to care is leverage: an open, model-agnostic harness existing at all changes the negotiating position around closed-harness lock-in, even if you never run it.

The forward-looking read: DeepSeek shipping its eval harness as an open product, the same day as a flagship GA, is a statement about where it thinks the moat is — and is not. If the `dsh-plugin` ecosystem materializes, the interesting competition in 2027 may be between harness ecosystems rather than between models, with the model reduced to one more plugin behind an adapter seam. That is a future worth preparing for deliberately. For organizations deciding how agent tooling fits their stack — which harness, which models, what stays swappable — our AI transformation engagements start exactly there: evidence-first evaluation on your own workloads, with lock-in treated as a cost you price, not a surprise you discover.

## 09 — ConclusionThe gym is now *open*.

### DeepSeek published the harness its own benchmarks were already naming.

Strip the launch noise and three facts carry the story. DeepSeek open-sourced a real, MIT-licensed agent harness in which every layer — models, tools, skills, sessions, sandboxes, storage, loops, scheduling, the UI — is a plugin. Its four modes include the deliberately minimal two-tool configuration that DeepSeek’s own published benchmark methodology names, which converts those vendor tables from claims about a black box into claims about inspectable software. And the version underneath the “v0.1” label is 0.1.0-rc.5 with zero tags and a capital-letters warning about breaking changes — a developer preview by the vendor’s own words, not a 1.0 by anyone’s.

The honest scorecard for the circulating hype: almost everything checks out, with corrections at the edges — the plugin noun list is DeepSeek’s, not the paraphrase; the install recipe needs a build step the viral version omits; and the version label is softer than the coverage implies. That is a better hit rate than most launch-day virality earns, and the corrections are the part you can only get by reading the primary sources.

What happens next is the real test: whether independent teams rerun DeepSeek’s minimal-mode methodology against the open V4-Pro weights, and whether a plugin ecosystem forms around dsh. An open harness makes both possible. Possibility, as always, is where the vendor’s job ends and the community’s begins.
