---
id: collect-261121-veille-ia/claude-anthropic/major-updates-in-claude-code-v2-1-321-introduction-of-claude-fable-5-2-and-a-fix-for-missing-enforce
title: "Major Updates in Claude Code v2.1.321 - Introduction of Claude Fable 5.2 and a Fix for Missing Enforcement of Deny Rules on cp/mv | DevelopersIO"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["claude", "fable", "changes", "settings", "deny", "rules", "code", "security"]
source: https://dev.classmethod.jp/en/articles/20261110-cc-updates-v2-1-321/
source_anchor: ""
source_lines: [1, 279]
section: "Claude_Anthropic"
---
I'm Ishikawa from the Cloud Business Division. Claude Code v2.1.321 (released November 9, 2026) is out. v2.1.320 does not exist in either npm or the CHANGELOG, so the difference from the previous v2.1.319 is a single version, v2.1.321. Because there are many changes (118), I'll focus on the notable items and the changes that affect existing settings.

Here is the previous update article.


## Update Summary

v2.1.321 includes 118 changes. By my own classification: 65 fixes, 14 security items, 12 breaking changes, 23 improvements/developer experience/performance items, 3 new features, and 1 documentation item. Fixes account for more than half, and there are many security fixes related to permissions and authentication for Bash and MCP, as well as behavior changes related to mod and gateway settings.

## Notable Updates

### New feature: Claude Fable 5.2 arrives

Claude Fable 5.2 (`claude-fable-5-2`) has been added and is now the default Fable model. **The context is 1M, and the price per million tokens is $8 for input / $40 for output, with cache reads at $0.20. Compared with Fable 5.1 (1M context, $10 input / $50 output, $0.25 cache reads), the context is the same and every unit price is 20% lower.** In addition, saved Fable 5.1 model selections are migrated to `fable` (see "Breaking Changes and Deprecations" for details).



###  New feature: `--settings` for `claude remote-control`

**`--settings <file>` has been added to `claude remote-control`**. It applies the specified settings file to every session that `claude remote-control` starts. If you change the settings file, new sessions will not start until you restart `claude remote-control`.

I think this option suits those who want to manage the settings for sessions launched from Remote Control in a single file.

### Security: Fix for MCP OAuth sending secrets

Fixed an issue where, when a server redirected, MCP OAuth requests sent secrets such as refresh tokens to a different host.

If you use MCP servers authenticated with OAuth, I think this is a fix you'll want to update for early.

### Security: Fix for cases where Bash permission prompts did not appear

Fixed a case where read-only Bash commands ran outside the sandbox without a permission prompt, and a case where Bash permission checks judged file writes against the wrong folder and did not ask for confirmation.

For those who delegate to Bash on the premise of the sandbox and permission prompts, I feel this is an important fix that makes prompts appear where they should.

### Security: Fix for deny rules not being applied to cp / mv

Fixed an issue where deny rules and protected-file checks were not applied to files written by `cp` / `mv` with a directory as the destination. Also, `cp` and `mv` specified by path, such as `/bin/cp`, now get the same path checks as `cp` / `mv`.

If you protect files with deny rules or protected-file settings, I think this is a fix you'll want to update for.

## Supplement: Changes in Claude Fable 5.2

Based on Anthropic's announcement page (published November 9, 2026), I've summarized the changes from Fable 5.1 divided into cost, performance, and tendencies.


### Cost

- As mentioned above, the unit price is **20% lower than Fable 5.1** .**Output generation is up to 30% faster than Fable 5.1**
- **A fast mode that speeds up token generation by up to 2.5x is also available** . Fast mode pricing is $16 input / $80 output per million tokens
- At **Medium effort, which is the Claude Code default, it is said to complete difficult tasks with fewer tokens than Fable 5.1**
- Regarding cost per task, the following comparisons with Opus 5.5 are shown
  - FrontierCode 2.0: At High effort it scores 45.3%, **almost the same result as Opus 5.5's best score (45.5%) at about half the cost**
  - WANDR: At Medium effort, it achieves **a result equivalent to Opus 5.5's best score at about 40% of the cost**
  - Chartography: At Medium effort it scores 66.4%, **exceeding Opus 5.5 at Max effort (64.4%)** at less than one-tenth of the cost
- FrontierCode 2.0: At High effort it scores 45.3%, 

### Performance

From the benchmark table on the announcement page, I extracted the items that can be compared with Fable 5.1. Opus 5.5 is included for reference. Scores are with safeguards enabled, and unless noted otherwise, they are results at each model's highest effort.

| Benchmark (Domain) | Fable 5.2 | Fable 5.1 | Opus 5.5 | 
|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | **49.1% (Xhigh)** | 32.4% (Xhigh) | 45.5% | 
| FrontierSWE v2 (agentic coding) | **69.9%** | 56.3% | 62.3% | 
| AutomationBench (business workflows) | 51.4% | 31.4% | 42.5% | 
| Terminal-Bench-Science 0.1 (scientific research) | **74.6%** | 52.6% | 58.7% | 
| WANDR relaxed score (agentic search) | **82.1%** | 68.7% | 72.3% | 
| OSWorld 2.1 offline set, partial (computer use) | **89.3%** | 83.5% | 87.2% | 
| RoboDojo Sim (robotics) | **64.9%** | 27.6% | 39.4% | 
| AA-Briefcase v1.1 (knowledge work) | **66.1% (rubric)** / 1785 Elo | 61.5% (rubric) / 1669 Elo | 61.2% (rubric) / 1800 Elo | 
| Remote Labor Index (knowledge work) | **41.3% (High)** | 17.9% | 21.3% | 
| Humanity's Last Exam, with tools (interdisciplinary reasoning) | **74.2%** | 65.6% | 67.7% | 
| Chartography, no tools (chart reading) | **73.4%** | 44.8% | 64.4% | 

The cybersecurity and protein design items are comparisons of Mythos 5.2 and Mythos 5.1 rather than Fable, so I excluded them from the table.

### Tendencies

- According to testers, Fable 5.2 communicates more clearly than past Fable and Mythos models
- Early testers of research and analysis tasks evaluate that Fable 5.2 can handle more ambiguous and data-heavy tasks than previous models, with better judgment especially on ambiguous tasks that require abstract reasoning or design
- As an example of work using many agents, in an experiment by Anthropic engineers, Fable 5.2 ran more than 1,000 subagents for 8 days starting from an empty folder and built a prototype OS that boots on a laptop
- Comments from engineers at companies using it, posted on the announcement page, include evaluations such as "it doesn't stop at what looks plausible and keeps going until the work is truly finished" (Uber) and "it says it doesn't know when it doesn't know, without hallucinating" (Cognition)
- The cybersecurity and biology safeguards are of the same strength as Fable 5.1, with fewer unnecessary blocks. In early testing on claude.ai, Claude Code, and Cowork, blocks were reduced by about 55% compared with Fable 5.1 (about 60% for cybersecurity and about 40% for biology). For blocked queries, Opus 4.8 responds instead for cybersecurity, and Opus 5 responds instead for biology
- Fable 5.2 and Mythos 5.2 are said to voice the suspicion that "I may be being evaluated" more often than any recent model. Anthropic explains that this tendency hinders evaluating behavior in real environments

## Update Details

### New Features

- Claude Fable 5.2 (`claude-fable-5-2` ) has been added and is now the default Fable model (1M context, $8 input / $40 output per million tokens, $0.20 cache reads)
- `--settings <file>` has been added to`claude remote-control` . It applies the settings file to every session it starts
- [VS Code] When both an auth token and an API key are set, a warning similar to the terminal startup warning is now shown above the prompt field

### Security

- **Fixed MCP OAuth secrets being sent** : Fixed an issue where secrets such as refresh tokens were sent to a different host when a server redirected
- **Fixed read-only Bash running outside the sandbox** : Fixed a case where read-only Bash commands ran outside the sandbox without a permission prompt
- **Fixed incorrect Bash write determination** : Fixed a case where file writes were judged against the wrong folder and no confirmation was requested
- **Fixed deny rules not being applied to cp / mv** : Fixed an issue where deny rules and protected-file checks were not applied to files written by`cp` /`mv` with a directory destination
- **Fixed missing checks for path-specified cp / mv** :`cp` and`mv` specified by path, such as`/bin/cp` , now get the same path checks as`cp` /`mv`
- **Fixed loss of ask in PreToolUse hooks** : Fixed an issue where a PreToolUse hook's`ask` was lost when another hook's`defer` completed first and could not be applied
- **Fixed mcp_tool hooks being skipped for large responses** : Fixed an issue where`PreToolUse``mcp_tool` hooks for stdio / HTTP MCP servers were skipped when the response exceeded 16 MiB. The tool call is now denied
- **Fixed files with control characters in names appearing under a different name** : Fixed an issue where hidden text-direction control characters or control characters in file names made sent files appear under a different name in the Claude app. Sending such files is now denied
- **Policy enforcement for app-provided Claude in Chrome tools** : Claude in Chrome tools provided by apps that run Claude Code, such as the Code tab of the Claude desktop app, now follow the organization's Claude in Chrome policy
- **Credentials excluded from gateway error messages** : Error messages from the Claude apps gateway now display the gateway address without the username and password
- **Narrowed the scope of the email address check** : Only shell commands that contain the user's own email address and specify a User-Agent are now denied without confirmation. Other commands and scheduled runs no longer stop at this check
- Also included: rejection of skill and command names containing Bamum characters that render as blank in some fonts, a fix so that VS Code plugin permission requests with names containing hidden characters are shown as "a plugin", and rejection of permission answers that set the undocumented `afkTimeoutMs` in`AskUserQuestion` for`claude -p` and the SDK.

### Improvements

- In the `/model` picker, "More models" is now placed at the bottom as an unnumbered row, and older models are shown below it without renumbering the other entries
- `claude install` and the install script now finish 2 to 4 seconds faster
- Sped up loading of trusted TLS certificates, shortening startup time
- On Windows, programs on PATH are now located without launching a helper process, shortening startup and the time to run the first command
- On Windows, standard Git for Windows installations no longer check git.exe on every startup, shortening startup time
- In auto mode for headless sessions, the desktop app, and VS Code, reports from multiple waiting background subagents are now answered with a single model call, as in the terminal
- In sessions with so many images that they do not fit in a single request, Claude is now told whether it has already seen the excluded images, and re-reading of unchanged images is limited
- For commands longer than 1,000 characters, background command notifications and TaskStop results now show Claude only the beginning and end of the command, reducing context consumption
- In headless sessions, mod files in `.claude/skills` are now reloaded when they change. This can be disabled with`CLAUDE_CODE_PLUGIN_DIR_WATCH=0`
- In OpenTelemetry hook events for non-interactive sessions, two built-in hooks are now excluded from `num_hooks` and`num_success` after Bash and PowerShell calls
- Also included: improvements to Claude apps gateway messages and usage-limit handling, the first message in Remote Control, error display in the agent view, the fast mode label, and the addition of Ultra to plan names.

### Fixes

- **Fixed incorrect edits by Edit / NotebookEdit on network paths** : Fixed issues on UNC shares, WSL folders, and`/net` mounts where Edit would change the first of multiple matches, replace the entire file with an empty`old_string` , or edit a notebook as text, and an issue where`NotebookEdit` without a specified cell would delete or overwrite the notebook's first cell
- **Fixed wrong working folder for parallel Bash** : Fixed an issue where, when Claude sent multiple Bash commands at the same time and an earlier command changed folders, commands could run in the wrong folder
- **Fixed turns ending on TLS errors** : Fixed an issue where a turn ended with "API Error: EPROTO: The TLS handshake failed" when the connection dropped mid-response. As before 2.1.302, it now either keeps the partial response or retries the request
- **Fixed fast mode fallback issues** : Fixed issues such as losing the response, switching models, or retrying for several minutes when fast mode availability was in a rate-limited, overloaded, or outage state. It now first tries the standard speed of the same model
- **Fixed loss of background subagent reports during compaction** : Fixed an issue in auto mode where, if the conversation was compacted before Claude responded, the report from the background subagent that started the turn was lost
- **Fixed missing skills, commands, and agents after git stash** : Fixed an issue where project skills, commands, and agents remained missing when a folder was temporarily deleted and restored, such as by`git stash` (including after entering a worktree)
- **Fixed git status failures being reported as clean** : Fixed an issue where the git status given to Claude at session start read as clean when git status failed. It now shows unavailable
- **Fixed unsaved messages and hangs on low memory** : Fixed an issue where, if memory ran out while loading the transcript, new messages were not saved and`claude -p` , SDK, and VS Code sessions hung
- **A quietly welcome fix** : Fixed an issue where Bash read commands such as`grep -r foo "$PWD/src"` asked for permission even though the same command with the path written directly would run without confirmation. Since you'll respond to confirmation dialogs less often, I feel this is a quietly welcome fix.
- Also fixed: many minor bugs in Remote Control, cloud sessions, the Code tab of Claude Desktop, screen reader mode, and MCP server connection display.

### Breaking Changes and Deprecations

There are 12 changes that affect existing settings and operations. No items have been deprecated. The "before" column is written only to the extent it can be read from the CHANGELOG's description of the change, and anything not stated explicitly is marked as "inferred".

| Target | Before (up to v2.1.319) | After (v2.1.321 onward) | 
|---|---|---|
| Saved Fable 5.1 model selection | Stays on Fable 5.1 | Migrated to `fable` (immediately for Pro and Max plans; for Team and Enterprise plans, once the organization allows it) | 
| Mods written by Claude (auto and bypass modes) | Asks for confirmation (inferred) | Enabled without confirmation. The same applies even with `allowUnsandboxedCommands: false` or`allowManagedDomainsOnly` in managed settings | 
| Mods written by Claude ( `dontAsk` mode) | Loaded (inferred) | Not loaded | 
| A mod's own tool calls ( `$.tool.call` ,`$.mcp.call` ) | Permission prompts appear due to ask rules and so on (inferred) | No permission prompts even when the user has their own ask rules or in `dontAsk` /`-p` runs. Deny rules and organization policies still apply | 
| `.claude/skills` inside`~/.claude/plugins` | Loaded when Claude works with files there | Not loaded | 
| Self-hosted runner (on-demand runner rejected by the server as unnecessary because the session ended, was paused, or the work order was replaced) | Exit code 1 | Exit code 0 | 
| Search for programs that Claude Code itself launches on Windows (git, etc.) | Relative paths, UNC PATH entries, and so on are also searched (inferred) | Skips relative paths, UNC PATH entries, and links to network shares. Reflects PATH changes made through settings | 
| Managed `extraKnownMarketplaces` | Not stated | Declared names cannot be added from another source. Entries from elsewhere are hidden. Plugins listed by a declared marketplace are reinstalled from there | 
| Takeover from undeclared sources by managed `extraKnownMarketplaces` | Not stated | Saved data is set aside for the `cleanupPeriodDays` period; options and secrets are set aside until you run`claude plugin restore-set-aside` | 
| Claude apps gateway `policy_mode: layered` | Managed MCP servers are not the sum of all matching policies (inferred) | The sum of all matching policies, including `match: {}` . Gateways already using this mode will serve more servers after upgrading | 
| Gateway Bedrock, Vertex, and Foundry upstreams | Uses the `ANTHROPIC_*_BASE_URL` variables in the gateway's environment (inferred) | Ignored. Set a different endpoint with the upstream's `base_url` | 
| PostToolUse hooks for Cowork cloud sessions | Can replace AskUserQuestion answers or the plan in ExitPlanMode results | Cannot replace them. Claude reads the original content | 
| Remote Control session folder | A connected client can move the session to a different folder | Keeps the starting folder and cannot be moved to a different folder | 

Below are examples of four of these that can be contrasted through settings or commands.

#### Saved Fable 5.1 model selection

The following is an example.

Before (up to v2.1.319):

Saved model selection: Fable 5.1 (claude-fable-5-1)


After (v2.1.321 onward):

Saved model selection: fable (the default Fable model in v2.1.321 is Claude Fable 5.2)


To go back to Fable 5.1, use the following command.

```
/model claude-fable-5-1
```
#### Gateway Bedrock, Vertex, and Foundry upstreams

The following is a Bedrock upstream configuration example based on the gateway configuration example in the official documentation.

Before (up to v2.1.319):

```
# Specify the endpoint with the gateway's environment variable (ignored in v2.1.321)
export ANTHROPIC_BEDROCK_BASE_URL=https://bedrock-runtime-fips.us-east-1.amazonaws.com
```
After (v2.1.321 onward):

```
upstreams:
  - provider: bedrock
    region: us-east-1
    auth: {}
    base_url: https://bedrock-runtime-fips.us-east-1.amazonaws.com
```
####  `.claude/skills` inside `~/.claude/plugins`

The following is a placement example.

Before (up to v2.1.319):

~/.claude/plugins/<any folder>/.claude/skills/

→ Loaded when Claude works with files under ~/.claude/plugins


After (v2.1.321 onward):

~/.claude/plugins/<any folder>/.claude/skills/

→ Not loaded even when Claude works with files under ~/.claude/plugins


#### Self-hosted runner exit code

The following is an example.

Before (up to v2.1.319):

On-demand runner rejected by the server as unnecessary → exit code 1


After (v2.1.321 onward):

On-demand runner rejected by the server as unnecessary → exit code 0


## Trying Claude Fable 5.2

I specified `--model claude-fable-5-2` with `claude -p` and checked whether a response came back from Fable 5.2 and what model information the JSON output contained. Following the description in `claude --help`, I disabled all built-in tools with `--tools ""` and used `--no-session-persistence` so the session is not saved.

```
% claude --version
2.1.321 (Claude Code)
% claude -p --model claude-fable-5-2 --max-turns 1 --tools "" --no-session-persistence --output-format json "動作確認です。「こんにちは」とだけ返してください。" > fable52.json
% jq '{result, is_error, num_turns, total_cost_usd, modelUsage}' fable52.json
{
  "result": "こんにちは",
  "is_error": false,
  "num_turns": 1,
  "total_cost_usd": 4.122216,
  "modelUsage": {
    "claude-fable-5-2": {
      "inputTokens": 2,
      "outputTokens": 7,
      "cacheReadInputTokens": 0,
      "cacheCreationInputTokens": 257620,
      "webSearchRequests": 0,
      "costUSD": 4.122216,
      "contextWindow": 1000000,
      "maxOutputTokens": 128000,
      "thinkingTokens": 0,
      "cacheCreationEphemeral5mInputTokens": 0,
      "cacheCreationEphemeral1hInputTokens": 257620,
      "canonicalModel": "claude-fable-5-2",
      "provider": "firstParty",
      "costBasis": "list"
    }
  }
}
```
The `modelUsage` key and `canonicalModel` are `claude-fable-5-2`, so the response came back from Fable 5.2. `contextWindow` is `1000000`, which matches the 1M context stated in the CHANGELOG.

`cacheCreationInputTokens` was 257,620 tokens, all of which were writes to the 1-hour cache (`cacheCreationEphemeral1hInputTokens`). I assume this amount for a response of just "Hello" is because my environment's CLAUDE.md, skills, MCP settings, and so on were included in the system prompt. The `total_cost_usd` of 4.122216 is an estimate with a `costBasis` of `list`, and almost all of it is for this cache write.

2 input tokens × $8 and 7 output tokens × $40 (both per million tokens) total $0.000296, and the remaining $4.12192 is for the cache write. The CHANGELOG does not state a unit price for cache writes, so the unit price below is my own back-calculation from this log.

I found it convenient that simply specifying the model ID lets you use Fable 5.2 with 1M context right away. Back-calculating from the log, the 1-hour cache write is computed at $16 per million tokens, and since the gap with the $0.20 cache read is large, I expect the cache benefit to grow the longer you continue a conversation in the same context.

Incidentally, the back-calculated $16 matches the 1-hour cache write unit price for Fable 5.2 listed on Anthropic's pricing page ($16 per million tokens).

## Conclusion

With Fable 5.2 becoming the default and the security fixes around MCP OAuth and Bash permissions arriving at the same time, I feel this is a release with clear reasons to update.

If you use Fable 5.1, mods, or the Claude apps gateway, why not check the breaking changes section as well, then update and try it out?

## References
