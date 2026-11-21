---
id: collect-261121-veille-ia/claude-anthropic/claude-code-v2-1-323325-code-mode-a-sandbox-that-asks-again-and-1m-context-by-default
title: "Claude Code v2.1.323–325: Code Mode, a Sandbox That Asks Again, and 1M Context by Default"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "claude", "sandbox", "mode", "context", "model", "default", "changelog"]
source: https://sdd.sh/2026/11/claude-code-v2-1-323-325-code-mode-sandbox-permission-flow/
source_anchor: ""
source_lines: [1, 52]
section: "Claude_Anthropic"
---
Three releases landed between November 10 and 13, and the changelog entries are short. Short entries hide behavior changes that will surprise unattended setups. This post is based only on the official Claude Code changelog. I haven’t run these versions, and the page was too large to read end to end, so I only covered the entries at the top. Version 2.1.324 did not appear in what I read, so it isn’t covered here.

## Code Mode (v2.1.323): experimental, opt-in#

The headline addition is **Code Mode**: Claude writes JavaScript to call its tools instead of emitting one tool call at a time. The changelog line is a single sentence, so I can’t say how tool results are returned, how it interacts with permissions, or what it costs in tokens. Anthropic hasn’t documented any of that in what I read.

The general idea is familiar from the MCP ecosystem. Loops, filters and joins over tool output are cheaper in code than as a chain of model turns. The open question is the security model: if the model writes a script that calls five tools, does each call still hit the permission layer individually? Until the docs say so, test it on a throwaway repo before turning it on in anything unattended.

## The sandbox stops auto-approving (v2.1.325)#

This is the change most likely to break a workflow. **Sandboxed Bash now follows the normal permission flow by default.** If you relied on sandboxed commands running without prompts, you now have to opt back in:

```
{
  "sandbox.autoAllowBashIfSandboxed": true
}
```
Two related changes point the same direction:

- **Scheduled tasks and `/loop`** now ask before running a skill that declares`allowed-tools` or hooks. For unattended runs, add`Skill(name)` to`permissions.allow` .
- **`/security-review`** (v2.1.323) now asks before Claude runs it.

The pattern is consistent: fewer implicit grants, more explicit allow rules. That’s good for a spec-driven setup, because the permission list becomes a reviewable artifact in the repo. It’s a nuisance if you had a nightly `/loop` that suddenly sits waiting for a human. Check any scheduled jobs after upgrading.

Also in v2.1.323, the Bash sandbox on macOS, Linux and WSL **ignores write access granted through a symlink** that leads elsewhere or to a missing path. If your sandbox config granted a symlinked path, grant the real path instead.

## 1M context assumed for unknown models (v2.1.323)#

The assumed context window for unrecognized model IDs changed from 200K to 1M. This matters for gateway and custom-model users, since an unrecognized ID is exactly what a proxy produces. If your backend actually serves 200K, Claude Code may now let a session grow past what the model accepts. To restore the old behavior:

`export CLAUDE_CODE_DISABLE_1M_CONTEXT=1`
## Plugins and platform notes#

- `engines["claude-code"]` in`plugin.json` lets a plugin declare a supported Claude Code version range; plugins outside it aren’t installed or loaded.
- MCP bundles (`.mcpb` ,`.dxt` ) now unpack under the plugins directory, so read-only plugin folders load (v2.1.325).
- Fast mode is available on Vertex in the global, us and eu regions, falling back to standard speed if Vertex refuses it (v2.1.322).
- The changelog reports 195 bug fixes in v2.1.323 and 88 in v2.1.325, including a security fix in how zstd-compressed responses are decoded. I haven’t seen further detail, so I can’t assess severity.

## What to do this week#

1. **Audit scheduled tasks and `/loop` jobs.** Add`Skill(name)` rules for the ones that must run unattended.
2. **Decide on `sandbox.autoAllowBashIfSandboxed` deliberately.** Setting it to true restores the old convenience; the new default is the safer one.
3. **Gateway users:** set`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` if your model is really 200K.
4. **Try Code Mode on a scratch repo only** , and watch the permission prompts to learn how it behaves.

## Verdict#

None of this is flashy, which is the point. Anthropic is tightening defaults while adding an experiment (Code Mode) that, if it works, makes agents cheaper per task. Terminal-native agents live or die on their permission model, and this is the kind of unglamorous work that separates them from tools that bolt autonomy on. My caveat stands: this is the changelog’s word, not mine, and I’d want the Code Mode documentation before saying more.

## Sources#

- Claude Code changelog, entries for v2.1.322, v2.1.323 and v2.1.325
