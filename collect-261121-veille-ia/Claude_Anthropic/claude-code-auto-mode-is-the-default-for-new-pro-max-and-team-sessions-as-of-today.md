---
id: collect-261121-veille-ia/claude-anthropic/claude-code-auto-mode-is-the-default-for-new-pro-max-and-team-sessions-as-of-today
title: "Claude Code auto mode is the default for new Pro, Max, and Team sessions as of today."
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-14"]
keywords: ["auto", "default", "claude", "sessions", "team", "mode", "prompt", "code"]
source: https://www.nofeed.dev/news/2026-08-14/
source_anchor: ""
source_lines: [1, 12]
section: "Claude_Anthropic"
---
Anthropic's permission-modes docs state it: starting 14 Aug 2026, new sessions on Pro, Max, and Team start in auto mode.<sup>1</sup> A one-time switch prompt appears if you never set a default; a `defaultMode` you already chose stays unless you accept that prompt; org-managed defaults do not move.<sup>1</sup>

Auto is a classifier, not YOLO. Deny and ask rules still win; unlisted actions go through a separate model that blocks escalation, unknown infrastructure, and prompt-injection-shaped moves.<sup>1</sup> Default surprise: pushes to any branch of the working repo — including main — and PR creation are allowed; force-push, secrets in commits, and history rewrites stay soft-blocked.<sup>1</sup> Claude Code 2.1.232 also turns subagent forking on by default and lets you `@` another live session on the same machine.<sup>2</sup>

If you never set a default, decide before the next session: keep auto and pin ask rules on `git push` / `gh pr create`, or set `"defaultMode": "manual"` in `~/.claude/settings.json`. Org admins who want the old baseline set `permissions.disableAutoMode` to `disable`.

## Deeper — who is affected and how to pin

- Only new sessions; existing `defaultMode` survives the one-time prompt unless you accept it.<sup>1</sup>
- Auto needs a supported model (Opus 4.6+, Sonnet 4.6+, Fable 5 on Anthropic; provider-specific floors on Bedrock/Foundry/gateway).<sup>1</sup>
- Per session: `Shift+Tab` or`claude --permission-mode manual` . Persistent:`~/.claude/settings.json` — repo settings cannot grant auto to themselves.<sup>1</sup>
- v2.1.232: `subagent_type: "fork"` inherits conversation + prompt cache; non-teammate spawns background by default; cross-session`SendMessage` by bare name.<sup>2</sup>
