---
id: collect-261121-veille-ia/gpt-openai/codex-cli-changelog-releases-summarized-nov-2026
title: "Codex CLI Changelog: Releases Summarized (Nov 2026)"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-29"]
keywords: ["codex", "releases", "skill", "model", "config", "changelog", "version", "gpt-6"]
source: https://codexskills.dev/guides/codex-changelog/
source_anchor: ""
source_lines: [1, 68]
section: "GPT_OpenAI"
---
This page summarizes the Codex CLI entries from OpenAI’s changelog so you can see what changed between the version you have and the newest release without reading the whole feed. It is maintained by hand from the official changelog and the openai/codex releases on GitHub; those two pages are the source of truth, and this one links to them rather than replacing them. Last checked 4 November 2026.

If you came here looking for a skill that *writes* release notes for your own project, that is the Release Notes skill.

## Check and update your version

```
codex --version                      # what you have
codex update                         # self-update, where supported
npm install -g @openai/codex@latest  # npm installs
brew upgrade --cask codex            # Homebrew installs
```
On macOS and Linux the curl installer from the CLI guide always installs the newest stable build. A version such as `0.162.0-alpha.18` is a pre-release from GitHub, not something the installer gives you.

## Stable releases

Newest first. Dates are the changelog’s publication dates. Entries that are not CLI releases (the desktop app, model availability) are included where they change what the CLI can do.

| Date | Release | What changed | 
|---|---|---|
| 29 Oct 2026 | CLI 0.166.0 | Keyboard-driven transcript selection with word, line and page motions, plus integrated search in the transcript. | 
| 27 Oct 2026 | CLI 0.165.0 | Built-in Google Cloud Vertex AI provider; GPT-6.1 Terra added to the model catalogs. | 
| 26 Oct 2026 | Models | GPT-6.1 Terra available in Codex on Plus, Pro, Business, Enterprise and Edu plans. | 
| 23 Oct 2026 | CLI 0.164.0 | Reusable prompts saved in numbered `/presets` slots; inline arguments for`/model` and`/permissions` . | 
| 15 Oct 2026 | CLI 0.163.0 | Searchable `/config` preferences panel, mid-response steering, and a configurable leader key. | 
| 14 Oct 2026 | Models | GPT-5.5 retired from Codex for ChatGPT sign-ins; API-key sessions unaffected. | 
| 9 Oct 2026 | CLI 0.162.1 | Fixed a TUI crash on multi-line async questions and startup failures caused by feature-setting mismatches. | 
| 8 Oct 2026 | CLI 0.162.0 | Managed Git worktree tools, pinned tasks in the Command Center, and `/copy` for transcript blocks. | 
| 8 Oct 2026 | Models | GPT-6.1 Sol Ultrafast available in Codex for eligible plans. | 
| 7 Oct 2026 | CLI 0.161.0 | GPT-6.1 Sol became the default model in the bundled and Bedrock catalogs; `/mcp login` for MCP sign-in from the TUI; microphone and speaker selection for voice. | 
| 5 Oct 2026 | CLI 0.160.1 | Preserved Windows startup environment variables when launching remote stdio MCP servers. | 
| 1 Oct 2026 | CLI 0.160.0 | “Show more” history paging in the agent command center; X11 middle-click paste. | 
| 30 Sep 2026 | CLI 0.159.3 | Optional account-security setup reminders for eligible ChatGPT sign-ins. | 
| 29 Sep 2026 | CLI 0.159.2 | Stopped console windows flashing on Windows during background processes. | 
| 29 Sep 2026 | CLI 0.159.1 | GPT-6.1 Sol as the default in the bundled catalog and the Bedrock Mantle and Runtime catalogs. | 

## What this means for skills and config

- **0.164’s `/presets`** store a prompt you reuse; a skill is the version you share and version-control. If a preset has grown past a sentence or two, it probably wants to be a skill: see write your first Codex skill.
- **0.163’s `/config` panel** edits the same`~/.codex/config.toml` the Codex config.toml skill and the CLI guide describe; nothing moved, it is just searchable now.
- **0.161’s `/mcp login`** removes the manual token dance for MCP servers that need OAuth; the Codex MCP guide covers the config side.
- **Model changes** (Sol as default, Terra on paid plans, GPT-5.5 retired) only affect the model a skill runs under. Skills are plain Markdown and do not pin a model, so none of them needed changes for these releases.

## Alphas and pre-releases

The GitHub releases page lists every build the team cuts, and on most days that is several `-alpha.N` tags with no release notes. They exist for internal testing and for people who opt in deliberately. The “Latest” label on GitHub marks the newest stable build, which is the one the changelog describes and the installer ships. If a feature you read about is missing from your install, check `codex --version` against the table above before assuming it is broken.

## Older releases

OpenAI’s changelog feed goes back to the first public Codex CLI and is filterable by product. Entries older than 29 September 2026 are not summarized here; use the official changelog with the Codex CLI filter, or the GitHub releases page for exact tags.

## Frequently asked questions

## Where is the official Codex changelog?

OpenAI publishes it at developers.openai.com/codex/changelog, which currently redirects to the combined ChatGPT & Codex changelog. Use its Codex CLI filter for CLI-only entries. Individual builds, including alphas, are on the openai/codex releases page on GitHub.

## How do I check which Codex CLI version I have?

Run codex --version. Compare it with the newest stable entry on this page; alphas carry a suffix such as 0.162.0-alpha.18 and are not what the installer gives you.

## How do I update the Codex CLI?

Run codex update if your install supports self-update, or rerun the installer for your platform: the curl script on macOS and Linux, npm install -g @openai/codex@latest, or brew upgrade --cask codex.

## Why does GitHub show a newer version than the changelog?

The GitHub releases page lists every build, and most are pre-release alphas with no notes. The changelog lists stable releases only. If a tag ends in -alpha.N it has not shipped to the installer yet.
