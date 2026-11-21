---
id: collect-261121-veille-ia/claude-anthropic/opus-5-5-now-runs-in-tools-you-did-not-reconfigure-find-the-defaults-a-launch-week-moved-and-pin-the
title: "Opus 5.5 now runs in tools you did not reconfigure: find the defaults a launch week moved, and pin the ones that matter"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "default", "model", "claude", "code", "moved", "week", "tools"]
source: https://www.ainews.tech/blog/launch-week-moves-the-defaults-you-never-set
source_anchor: ""
source_lines: [1, 87]
section: "Claude_Anthropic"
---
A Claude Code settings file on one of our machines contains the line `"model": "opus[1m]"`. It has not been edited in weeks. This is what a headless run on it reports this morning:

```
claude -p "Reply with the single word ok." --output-format json | jq -r '.modelUsage | keys[]'
```
```
claude-opus-5-5[1m]
```
Before version 2.1.280, shipped on 22 September, the same alias resolved to Opus 5. The file did not change. The meaning of `opus` did, because Claude Code's model docs define the aliases as pointers that "point to the recommended version for your provider and update over time."

That is the ordinary, documented behaviour of an alias, and on its own it would not be worth a post. What made this week different is how many tools moved a default at once, and how many kinds of default moved. Claude Opus 5.5 and GPT-6 Sol and Luna all shipped on 22 September. Within four days, three agent tools had changed what they do for anyone who had not written down a choice, and a fourth had put a date on it.

## What moved between 22 and 25 September

Every row below comes from the vendor's own release notes or changelog. The column that matters is the last one: what you get if you never set the value yourself.

| Tool | Release | Default that changed | What an unset value now gives you | 
|---|---|---|---|
| Claude Code | 2.1.280, 22 Sep | Default model on Pro and Team Standard | Opus 5.5, previously Sonnet 5 | 
| Claude Code | 2.1.280, 22 Sep | The `opus` alias | Opus 5.5, previously Opus 5 | 
| Claude Code | 2.1.280, 22 Sep | Effort on newly released models | `medium` on Opus 5.5, even if you saved`high` before`/effort` became per-model | 
| Claude Code | 2.1.283, 25 Sep | Permission mode for interactive sessions on third-party providers or with telemetry off | Auto mode, when no permission mode is configured | 
| Cline | CLI 3.0.65 and extension 4.1.21, 24 Sep | Default model for 19 providers | Opus 5.5 on 11 of them, including GitHub Copilot and Vertex | 
| Codex | 0.156.0, 22 Sep | Voice and worktrees | On | 
| Codex | 0.157.0, 25 Sep | Daemon startup, fullscreen transcript, Guardian thread context | On | 
| GitHub Copilot | Announced 24 Sep, effective 22 Oct | Business and Enterprise policies left "Unconfigured", including MCP servers | Whatever your enterprise's global default says | 

Cline's release note is the most direct about it: "If you use one of those providers without pinning a model, expect a different default." GitHub's is the most precise about the rule underneath: "If you've explicitly enabled or disabled a feature, we won't override that choice."

Put the two sentences together and the pattern is clear. **An explicit choice is preserved. An unset value is a standing delegation to the vendor**, and a launch week is when vendors exercise it. Nothing on the list above is a bug, and nothing is hidden. Each change is one line in a changelog that shipped alongside a much louder model announcement.

## Three kinds of default, three different failures

The model default is the one people notice, because output changes. The other two are quieter.

**The model itself.** A Pro subscriber who tuned a workflow on Sonnet 5 is now on Opus 5.5, which also starts at a lower default effort than Sonnet 5. A Cline user on the Vertex or Requesty provider who never picked a model now sends every task to Opus 5.5, which Anthropic lists at $4 input and $20 output per million tokens. Neither of them changed anything. If you run the same task on two machines and get different results this week, check which model each one resolved to before you change the prompt.

**A setting that stopped applying.** Claude Code's docs spell it out: Opus 5.5 "starts at `medium` unless one of the sources above sets a level for it, and a top-level `effortLevel` in your user settings file doesn't count for Opus 5.5." If you set effort to `high` with the older form of `/effort`, that choice still applies to Opus 5 and earlier. When the default moved you to Opus 5.5, your choice stayed behind with the old model. Anthropic's own guidance is that Opus 5.5 at `medium` matches or beats Opus 5 at `high` on its evaluations, so this may be fine. It is still a decision someone else made for you, and you should know it happened.

**Behaviour that switched on.** Codex now starts a background daemon on its own. Interactive Claude Code sessions on a third-party provider such as Bedrock or Vertex, or with telemetry disabled, now start in auto mode when `permissions.defaultMode` is unset. For one developer on a laptop that is a convenience. On a shared development VM or a jump host where several people open sessions, it changes what the agent may do without asking, and it arrived in a point release.

## An allowlist is not a pin

The most useful fact of the week sits in the Claude Code docs, and it catches teams that believe they already locked their model down.

Enterprise admins restrict models with `availableModels` in managed settings. The entries match by prefix: "An `availableModels` entry such as `claude-opus-5` also permits later releases that extend it, such as Opus 5.5, as soon as Claude Code supports them." An allowlist that reads like a pin to Opus 5 has admitted Opus 5.5 since 22 September.

Claude Code 2.1.283 added the two keys that close this gap. Both are read from managed settings only:

```
{
  "availableModels": ["claude-opus-5", "claude-sonnet-5"],
  "availableModelsMatch": "exact",
  "enforceAvailableModels": true,
  "requiredMinimumVersion": "2.1.283"
}
```
With `"exact"`, each model ID permits only the version it names, so Opus 5.5 stays blocked until someone adds `claude-opus-5-5` to the list. The rule applies to model IDs only: a family alias such as `"opus"` or `"haiku"` in the same list still permits every version in that family, so write versions if you want versions held. `enforceAvailableModels` extends the list to the Default option, which otherwise keeps resolving to the plan's default. `requiredMinimumVersion` matters because earlier versions ignore the new keys entirely. If you would rather allow the family and hold back one release, `deniedModels: ["claude-opus-5-5"]` does that.

There is a timing detail here. On npm, Claude Code's `stable` channel is still 2.1.274 as of this morning, while `latest` is 2.1.283. A team that follows `stable` has not received the model change yet, and it has not received the keys to control it either. Both arrive together on a date nobody has announced.

## What to do, by where you sit

**If you use Claude Code alone, on a subscription or an API key:** decide whether you want the alias or the version. `"model": "claude-opus-5-5"` pins, and so does `ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-5-5` if you want `opus` to keep meaning today's model. Then run `/effort` once on the model you are using now, because a level saved before per-model effort does not carry over.

**If you administer Claude Code for a team:** check whether your `availableModels` entries are prefixes. If they are and you want new versions reviewed first, add `availableModelsMatch: "exact"`, set `requiredMinimumVersion`, and set `permissions.defaultMode` explicitly on any machine where people open interactive sessions against Bedrock or Vertex, or with telemetry disabled.

**If you use Cline with a provider that is not Anthropic itself:** open the provider settings and select a model by name. With the CLI, pass it on every scheduled or scripted run with `-m`. The eleven providers that moved to Opus 5.5 are listed in the 3.0.65 release notes.

**If you run Codex in CI or on shared machines:** check whether the auto-started daemon is acceptable there, and whether any config still names the `ultrafast` tier for `gpt-5.6-sol`, which 0.157.0 removed.

**If you administer GitHub Copilot Business or Enterprise:** you have until 22 October. Open AI Controls, then the Copilot subpage, and look at every policy still marked Unconfigured, especially "MCP servers in Copilot". Anything you set explicitly before then stays as you set it. Anything left Unconfigured follows the global default from that day on.

**If your own code calls the API:** none of the tool defaults above reach you, because Anthropic model IDs are pinned snapshots. The risk runs the other way. When you decide to move `claude-opus-5` to `claude-opus-5-5`, the string is not the whole migration: `thinking` set to `enabled` or `disabled` returns a 400 on Opus 5.5, and so does `tool_choice` of type `any` or `tool`. We covered the replacement for forced tool use when Fable 5.1 introduced the same restriction.

## Read the next launch's changelogs for one word

Next time a frontier model ships, the model announcement will be loud and the tool changes that follow it will be quiet. The quiet ones are easy to find if you search the release notes of the tools you run for the word "default" in the three days after the launch:

```
gh release view cli-v3.0.65 -R cline/cline --json body -q .body | grep -i default
curl -s https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md \
  | awk '/^## 2\.1\.274$/{exit} {print}' | grep -E '^- Changed' | grep -i default
```
The second command lists every "Changed" line mentioning a default between your installed version and the newest one. Replace `2.1.274` with whatever `claude --version` reports. Run against this week's releases, it returns seven lines, and the model change is only one of them.

Then confirm what runs, rather than what the settings file seems to say. For Claude Code, the `modelUsage` keys from a headless run, as at the top of this post. For your own API calls, the `model` field in the response, which we went through in the post on model IDs that change under you. If your agents run unattended, a one-line check in CI that fails when the resolved model is not the one you expect is cheaper than working out a week later why their output changed.
