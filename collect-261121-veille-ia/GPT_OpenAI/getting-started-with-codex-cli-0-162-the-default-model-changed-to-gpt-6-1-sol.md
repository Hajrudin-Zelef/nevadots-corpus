---
id: collect-261121-veille-ia/gpt-openai/getting-started-with-codex-cli-0-162-the-default-model-changed-to-gpt-6-1-sol
title: "Getting Started with Codex CLI 0.162: The Default Model Changed to GPT-6.1 Sol"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "model", "default", "gpt-6", "catalog", "changed", "release", "releases"]
source: https://zenn.dev/kai_kou/articles/1228-codex-cli-0161-gpt-61-sol-default-guide?locale=en
source_anchor: ""
source_lines: [1, 222]
section: "GPT_OpenAI"
---
## Introduction

After bumping Codex CLI with `npm update`, many of us probably start using it right away without reading the release notes. However, in 0.161.0, the default model in the bundled model catalog has been swapped to GPT-6.1 Sol. If you haven't configured anything locally, there's a chance you've been talking to a different model from the moment you updated.

So where should you look after updating to confirm "what changed" in your own environment? The model lineup, the default values of opt-in features, the MCP authentication path, and the conditions for the startup failure fixed in the previous patch can all be checked with CLI commands. In this article, I've organized these into 5 verification steps.

The target audience is developers who use Codex CLI daily and have updated (or are about to update) to the 0.161–0.162 series.

### What this article helps you solve

- You can grasp the changes across the 3 releases from 0.161.0 to 0.162.1 in the order you should verify them
- You can check the default model lineup and the context length value with `codex debug models`
- You can check how opt-in features like Daybreak are configured by default with `codex features list`

### Environment used

Verification was performed in a Linux cloud execution environment. I installed `@openai/codex@0.162.1` locally into a temporary directory outside the repository, and isolated `HOME` and `CODEX_HOME` under it as well. This was a state with no configuration file, no ChatGPT sign-in, and no API key set. Therefore, features that require launching the TUI for interaction (`/mcp login`, voice device selection, task pinning, etc.) were not exercised, and are treated in this article as descriptions from the release notes.

## TL;DR

After completing the steps in this article, you'll be able to determine the following in your own environment.

1. Whether the installed Codex CLI is `latest` 0.162.1, or whether you've accidentally picked up an alpha
2. That GPT-6.1 Sol is at the top (priority 1) in the bundled catalog, and that its `context_window` is 272,000 (a different value from the 1,050,000 on the API model page)
3. That Daybreak is off by default, and you need to explicitly set `cli_daybreak` to enable it
4. That you can authenticate MCP servers from the CLI's `codex mcp login`
5. That the startup failure caused by a feature configuration mismatch with the background server has been fixed in 0.162.1

## What changed in 0.161.0–0.162.1

The three releases came out in succession from October 7 to 9. The primary sources are the GitHub Releases for openai/codex (`rust-v0.161.0` / `rust-v0.162.0` / `rust-v0.162.1`). At the time of my check, I couldn't find entries for these three versions on Codex's official changelog page.

| Version | Public display on GitHub Releases | Main changes (summary of release notes) | 
|---|---|---|
| 0.161.0 | October 7 | Default model in the bundled catalog and Amazon Bedrock catalog changed to GPT-6.1 Sol. `/mcp login \u003cname\u003e` , voice device selection, Daybreak made opt-in | 
| 0.162.0 | October 8 | Managed Git worktree tools, task pinning in agent Command Center, transcript copying, clickable URLs in TUI, capability settings for custom providers ( `remote_compaction` and`external_web_access` in`[model_providers.\u003cid\u003e.capabilities]` ) | 
| 0.162.1 | October 9 | Fixed TUI crash on multi-line async questions, fixed startup failure caused by feature configuration mismatch with background server | 

Note that I can't definitively say "0.161.0 was the first to make GPT-6.1 Sol the default." Two third-party release mirrors state that it was preceded by a backport in 0.159.1 (September 29) (I couldn't retrieve the GitHub Releases, and haven't confirmed it in the primary source). The 0.161.0 row in the table is a summary limited to the release notes' description of the bundled catalog and Bedrock catalog defaults.

The `capabilities` key names were confirmed by having 0.162.1 read a `config.toml`. Entering an invalid value for `remote_compaction` caused startup to fail with the error `expected unsupported or v2`, and `wire_api = "chat"` failed with `no longer supported`. The default values and meanings are based on the description in the official PR (openai/codex#50459), and I have not confirmed the actual behavior.

What directly affects users are the two changes: the default model change in 0.161.0 and the startup failure fix in 0.162.1. The rest are additional features that only those who use them need to be aware of, so in the steps below I'll check the default model and startup-related items first, and touch on the additional features only in the relevant sections.

## Steps

The 5 checks flow from top to bottom: "installed version," "model used," "default features," "authentication," and "known issues."

### 1. Check the version and dist-tag

First, confirm which version your local CLI is. Looking at npm's dist-tags as well will tell you whether you've unintentionally installed an alpha.

```
npm view @openai/codex dist-tags --json
npm view @openai/codex time --json
```
The excerpted execution results are as follows.

```
$ npm view @openai/codex dist-tags --json   (excerpt)
  "latest": "0.162.1",
  "alpha": "0.163.0-alpha.5",
$ npm view @openai/codex time --json   (excerpt)
  "0.162.1": "2026-10-09T19:50:30.512Z",
  "0.163.0-alpha.5": "2026-10-10T00:48:26.025Z",
```
The npm publication of 0.162.1 was October 9 at 19:50 UTC, which is October 10 at 04:50 Japan time. The display on the GitHub Releases side is October 9 at 19:44, and while I haven't confirmed the timezone of that display, it appears to be a difference of a few minutes. About 5 hours later, `0.163.0-alpha.5` was released under the `alpha` tag, so if you install with `@alpha` or a range specification, you may end up with a version that isn't in the 0.162 series.

After installation, you can check the version with `codex --version`.

```
$ npm install @openai/codex@0.162.1
added 2 packages
$ codex --version
codex-cli 0.162.1
```
If you normally install globally, specifying the version explicitly like `npm install -g @openai/codex@0.162.1` makes it easier to match the article's content against your local behavior. The subcommand list in `codex --help` also includes `update` and `doctor`, but I haven't verified the behavior of these two in this verification.

### 2. Look at the model lineup in the bundled catalog

The default model change can be confirmed with `codex debug models`. This subcommand outputs the model catalog bundled with the CLI, and it could be run without authentication or a configuration file. The output is JSON, so I extracted the main fields from it into a table (excerpted rows from the `codex debug models` output, not all entries).

| slug | display_name | visibility | priority | context_window | default_reasoning | 
|---|---|---|---|---|---|
| gpt-6.1-sol | GPT-6.1-Sol | list | 1 | 272000 | low | 
| gpt-6-astra | GPT-6-Astra | list | 2 | 272000 | low | 
| gpt-6-sol | GPT-6-Sol | list | 3 | 272000 | medium | 
| gpt-6-luna | GPT-6-Luna | list | 4 | 272000 | medium | 
| gpt-5.6-sol | GPT-5.6-Sol | list | 5 | 272000 | low | 
| gpt-5.6-terra | GPT-5.6-Terra | list | 8 | 272000 | medium | 
| gpt-5.6-luna | GPT-5.6-Luna | list | 9 | 272000 | medium | 
| gpt-daybreak-blue-latest | Daybreak Blue | hide | 11 | 272000 | low | 
| gpt-daybreak-red-latest | Daybreak Red | hide | 12 | 372000 | medium | 
| gpt-5.5 | GPT-5.5 | list | 13 | 272000 | medium | 
| codex-auto-review | Codex Auto Review | hide | 43 | 272000 | medium | 

`gpt-6.1-sol` is at the top with priority 1, and `gpt-6-astra` follows right below. However, this output doesn't explicitly state "this is the default model." The idea that "the one with the smallest priority becomes the default" is an inference from the catalog ordering, and what I could confirm is limited to the fact that it matches the release notes' description that "the default model became GPT-6.1 Sol." Note that I did not authenticate, launch the TUI, and visually confirm which model is actually selected by default.

Another thing I noticed in the table is the difference in `default_reasoning`. `gpt-6.1-sol` is `low`, and `gpt-6-sol` is `medium` (the column names are organized by me, and are not the actual key names in the JSON). I haven't confirmed whether the previous default was `gpt-6-sol`, but if the model changes, the reasoning strength when nothing is specified can also change in the catalog. If you feel the response speed or depth differs from before the update, consider this value as well, not just the model.

#### The catalog's 272,000 and the API's 1,050,000

What caught my attention most in this catalog was the value of `context_window`. The GPT-6.1 Sol model page in OpenAI's API documentation states the following.

| Item | Value (API model page) | 
|---|---|
| model ID | `gpt-6.1-sol` | 
| input | $2 / 1M tokens | 
| cached input | $0.10 / 1M tokens | 
| cache writes | $2.50 / 1M tokens | 
| output | $10 / 1M tokens | 
| input over 272K | Input and cache pricing doubled, output 1.5x (applied to the entire request) | 
| context window | 1,050,000 tokens (max input 922,000) | 
| max output | 128,000 tokens | 
| knowledge cutoff | 2026-04-30 | 

Source: GPT-6.1 Sol (OpenAI API documentation)

The API page says 1,050,000, but the Codex CLI catalog says 272,000. That's about 1/3.9 of the value, and if you look only at the API specs and think "Codex can also handle million-token-class context," your assumptions may be off. In the rows excerpted in the table above, nearly all models are 272,000, with only Daybreak Red at 372,000. It reads as not a uniform fixed value, but a value held on the CLI side per model.

Where this difference takes effect (whether compression or truncation occurs on the CLI side at the 272,000 boundary when passing long context) hasn't been verified because I haven't authenticated and actually streamed a long context. What can be said is limited to the point that "the value the CLI catalog holds is a different thing from the API's maximum value." When estimating costs too, it's safer not to assume the context length from the API page as-is. Incidentally, the same model page states that "prompts with input exceeding 272K tokens have input and cache pricing doubled for the entire request, and output at 1.5x." This 272K is the same number as the CLI catalog's `context_window` of 272,000. The fact that they match is true, but whether the CLI decided its value to align with this boundary is not stated officially, and I haven't confirmed it either.

#### If you want to pin to the previous model

If you want to avoid switching to GPT-6.1 Sol, specify the model explicitly. Codex's changelog (the September 22, 2026 GPT-6 Sol/Luna rollout entry) introduces two ways to specify the model in the CLI: the `/model` command and `codex --model gpt-6-sol`. If you want your team to use the same model, specifying it explicitly rather than leaving it to the default will help avoid behavior changes on every update.

Note that what gets selected when you don't specify a model also depends on your subscription plan. The official models page doesn't write a default name but expresses it as "recommended model," and GPT-6.1 Sol is stated as not available for Free and Go (available for Plus / Pro / Business / Enterprise / Edu). Also, according to the same page, GPT-5.5 will be retired on all plans on October 14, 2026 (API excluded). Both are descriptions from official documentation, and I haven't confirmed them. Source: Codex models (official)

### 3. Check the default values of opt-in features

Next, check the default values of feature flags with `codex features list`. The output has 3 columns: "name / stage / enabled by default." I extracted only the rows relevant to this update.

```
$ codex features list   (excerpt)
api_key_cyber_access_programs            stable             false
cli_daybreak                             under development  false
in_app_voice                             stable             true
worktrees                                stable             true
```
`cli_daybreak` is `under development` and off by default. The 0.161.0 release notes also explain that Daybreak is opt-in, and enabling it requires `--enable cli_daybreak` or `features.cli_daybreak=true`, and that `daybreak=true` alone does not enable it. In the catalog too, Daybreak Blue and Daybreak Red have `visibility` set to `hide`, which is consistent with them not appearing by default. According to the release notes, using automatic Cyber routing also requires a combination of a supported ChatGPT sign-in and the OpenAI provider. I haven't tried the behavior with Daybreak enabled this time.

`in_app_voice` is stable and on by default. The microphone, speaker, and input channel selection for voice conversation introduced in 0.161.0 is, according to the release notes, saved locally. Since it's a feature that requires voice devices, I haven't confirmed the actual behavior.

`worktrees` is also stable and on by default. The managed Git worktree tools in 0.162.0 are described in the release notes as "available for trusted local projects when the worktrees feature is enabled." Since it's on by default, it reads as being applicable without additional configuration in projects you treat as trusted. The usage of worktrees themselves and the differences from manual `git worktree` are covered in a separate article, Codex CLI worktree introduction, so here I'll limit this to confirming that it's "on by default." The storage location (`$CODEX_HOME/worktrees`) and retention count in the official worktree documentation are descriptions for the desktop app, so I won't treat them as CLI specifications here.

The overall stage and default value trends of feature flags were organized in a previous feature flags article. There's no need to review all of them on every update; looking up just the names that appear in the release notes with `codex features list` is sufficient.

### 4. Check the MCP server authentication path

In 0.161.0, the release notes state that MCP server authentication became possible during a session with `/mcp login \u003cname\u003e`. I haven't launched this TUI-side command this time, but I could confirm that there's also an authentication entry point as a CLI subcommand.

The subcommand list in `codex mcp --help` included the following (a formatted summary of the output).

```
list / get / add / remove / login / logout
```
Since `codex mcp login` / `codex mcp logout` are listed, there's also a path to complete MCP server authentication from the shell without opening the TUI. I didn't actually run `codex mcp login`, only confirmed its existence in the help. The official MCP documentation states that this subcommand has `--no-browser`, `--scopes`, and `--oauth-client-registration \u003cAUTO|CIMD|DCR\u003e`, which I could also confirm with `codex mcp login --help` in 0.162.1 (whether the TUI's `/mcp login` behaves the same is unverified). Whether it can be run non-interactively is unverified, so please try it yourself to see if it can be used in scripts or CI.

Codex's own authentication is on the `codex login` side, and the help showed a `status` subcommand and options such as `--with-api-key` and `--with-access-token`. When you want to first check whether authentication is still alive after an update, starting with `codex login status` makes it easier to separate model or MCP issues from authentication expiration (I only confirmed this command's existence in the help and haven't run it either).

Incidentally, Codex's changelog notes that `codex mcp-server` was deprecated on 2026-08-24 and removed on 2026-09-05. If an old procedure document uses `codex mcp-server`, that will need to be reviewed separately from this update.

### 5. Know the conditions of the startup failure fixed in 0.162.1

Finally, known issues. The 0.162.1 release notes list two fixes.

- Fixed a TUI crash on multi-line async questions (preserving newlines and full hyperlink destinations, #51866)
- A fix to allow startup even when the feature configuration of a running background server differs from the CLI's defaults. The compatibility check is applied only when features are explicitly overridden in the CLI (#52648)

The second one relates to step 3. If the feature configuration differs between the server running in the background and your local CLI, startup itself could fail before the fix. In 0.162.1, this mismatch alone no longer causes a stop, but as far as the release notes' explanation goes, explicitly overriding features with `--enable` etc. is still subject to the compatibility check. If you use something like enabling Daybreak with `--enable cli_daybreak` at startup, being mindful of whether it matches the background server's configuration will speed up troubleshooting.

0.162.0 also includes other fixes such as the handling of server-side model/reasoning defaults in new TUI threads, CRLF preservation in `apply_patch`, Linux and Windows sandbox, `Retry-After` handling, and the PowerShell installer checksum. Even if you're staying on the 0.161 series, upgrading to 0.162.1 lets you take in these fixes all at once.

## Troubleshooting

### 
 Q1: `codex --version` is still old after updating

It's possible that it's installed both globally and locally, and the older one is found first on PATH. Check which binary is being invoked with `which codex`, and reinstall with an explicit version like `npm install -g @openai/codex@0.162.1` (these two commands are general guidance and were not run in this verification).

### Q2: Response tendencies changed from before the update

As in step 2, the default model became GPT-6.1 Sol, and in the catalog `default_reasoning` differs: `gpt-6.1-sol` is `low` and `gpt-6-sol` is `medium`. If you want to return to the previous behavior, specify it explicitly with `codex --model gpt-6-sol`, and if differences remain, check the reasoning settings as well.

### Q3: Daybreak models don't appear in the list

Since it's off by default, not appearing is normal. As per the release notes, specify `--enable cli_daybreak` or `features.cli_daybreak=true`. The name `daybreak=true` will not enable it.

### Q4: I want to know the cost of GPT-6.1 Sol for long context

I could confirm the base pricing listed on the official model page (input $2, cached input $0.10, output $10 / 1M tokens), but the official model page states that for prompts with input exceeding 272K tokens, input and cache pricing is doubled for the entire request, and output is 1.5x. How the CLI behaves when this boundary is crossed is unverified. If you're estimating based on long context, check the pricing table on the latest model page.

## Summary

The answer to the opening question "where should you look after updating to confirm what changed" is the following 5 points.

- Use `npm view` dist-tags and`codex --version` to see whether`latest` 0.162.1 is installed
- Use `codex debug models` to confirm that GPT-6.1 Sol is at priority 1 and that`context_window` is 272,000
- Use `codex features list` to confirm that Daybreak is off by default, and worktrees and voice are on by default
- Use `codex mcp --help` and`codex login --help` to confirm that there are authentication entry points for MCP and the main body (login itself not executed)
- If the startup failure conditions apply, upgrade to 0.162.1 (feature configuration mismatch alone will no longer cause a stop)

Among these, what wasn't visible just from reading the release notes was the second point, the context length. A model that is 1,050,000 tokens on the API model page is treated as 272,000 in the Codex CLI catalog. If your local workflow passes long context, first look at the value for the model you're using with `codex debug models`. Including the point that the API's surcharge rate boundary (272K) is the same number as the CLI catalog value, how the CLI actually behaves when exceeding 272,000 remains an unresolved question I couldn't fully verify this time.

## Related articles

- Integrating Codex CLI's exec into CI with API key authentication
- Codex CLI's token budget, 11,000 tokens used even with an upper limit of 5
- Introduction to GPT-Rosalind — The full picture of OpenAI's first life science AI specialized for drug discovery and genome research

## Reference links

- openai/codex rust-v0.161.0 (GitHub Releases)
- openai/codex rust-v0.162.0 (GitHub Releases)
- openai/codex rust-v0.162.1 (GitHub Releases)
- GPT-6.1 Sol (OpenAI API documentation)
- Codex models (official)
- Codex MCP (official)
- 
Codex changelog (entries on the GPT-6 Sol/Luna rollout and the deprecation and removal of `codex mcp-server` )

## Discussion
