---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-319-and-2-1-321-close-a-worktree-sandbox-escape-and-an-mcp-oauth-token-leak-upgrade-
title: "Claude Code 2.1.319 and 2.1.321 Close a Worktree Sandbox Escape and an MCP OAuth Token Leak. Upgrade Now"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-06"]
keywords: ["sandbox", "claude", "code", "oauth", "fixes", "escape", "upgrade", "permission"]
source: https://neomanex.com/news/claude-code-2-1-319-2-1-321-worktree-sandbox-escape-bash-bypass-mcp-oauth-fixes
source_anchor: ""
source_lines: [1, 59]
section: "Claude_Anthropic"
---
# Claude Code 2.1.319 and 2.1.321 Close a Worktree Sandbox Escape and an MCP OAuth Token Leak. Upgrade Now

- Claude Code 2.1.319 (6 November) fixes a worktree sandbox escape and Bash permission bypasses and changes auto mode behind gateways; 2.1.321 (9 November) fixes MCP OAuth refresh tokens sent to a redirect host and more Bash bypasses.
- These are security fixes announced only as changelog lines, with no CVE or advisory, so vulnerability scanners and advisory feeds are unlikely to flag older versions.
- Upgrade to 2.1.321 everywhere (it includes 2.1.319), rotate MCP OAuth refresh tokens if a third-party server may have redirected, and check the auto mode change if you run behind a gateway.

**Verdict: upgrade to 2.1.321 today (it includes 2.1.319).** The Claude Code changelog lists eight security fixes in two releases three days apart: a sandbox escape, a token leak and six permission or sandbox bypasses. None has a CVE or an advisory, so if you track Claude Code security through advisory feeds or scanners, you have not been told. Our verdict on `claude-code` stays **conditional**: the fixes are welcome, but security notice by changelog line is exactly the reason it is not higher.

Anthropic shipped Claude Code 2.1.319 on 6 November 2026 and 2.1.321 on 9 November 2026. Version 2.1.320 does not appear in the changelog. When we checked the changelog on 10 November, it listed these fixes, quoted as written:

**2.1.321 (9 November)**

- "Fixed MCP OAuth requests sending secrets such as refresh tokens to another host when a server redirected them"
- "Fixed a case where a read-only Bash command ran outside the sandbox without a permission prompt"
- "Fixed a case where Bash permission checks judged a file write from the wrong folder and didn't prompt"
- "Fixed deny rules and protected-file prompts not applying to the file that a `cp` or`mv` into a directory writes"

**2.1.319 (6 November)**

- "Fixed a sandbox escape when creating a worktree"
- "Fixed a Bash permission bypass where a crafted environment prefix could hide part of a command from permission checks"
- "Fixed a case where a `sed` command could skip the permission check in accept-edits and auto mode"
- "Fixed a case where `/rewind` could restore or delete a file through a directory symlink"

2.1.319 changes auto mode when telemetry or feature flags are off, including behind LLM gateways: "calls the server can't check are asked about or denied, with no fallback to the built-in classifier". Setting `CLAUDE_CODE_AUTO_MODE_SERVER=0` keeps the built-in classifier. The changelog says Bedrock, Vertex and Foundry are unchanged.

Again, these are security fixes shipped as ordinary changelog lines. Three of these deserve priority.

- **MCP OAuth redirect.** If you connect to MCP servers you do not run, a redirect could carry your refresh token to another host. That is a credential exposure, not just a prompt you missed.
- **Worktree sandbox escape.** Anyone who relies on the sandbox while running parallel agents in worktrees was less contained than they assumed.
- **Permission bypasses.** The environment prefix,`sed` , wrong-folder and`cp` /`mv` fixes all describe commands that could slip past the checks that accept-edits and auto mode depend on.

| You are | Do this | 
|---|---|
| Any Claude Code user | Upgrade to 2.1.321 (which includes 2.1.319) on every machine and CI runner. | 
| Using third-party MCP servers with OAuth | Check where they redirect, and rotate refresh tokens where you cannot rule out a redirect to another host. | 
| Running agents in worktrees with the sandbox on | Upgrade before your next parallel run; treat earlier runs as less contained than intended. | 
| Behind an LLM gateway with telemetry or flags off | Expect more prompts or denials in auto mode, or set `CLAUDE_CODE_AUTO_MODE_SERVER=0` . | 

**Is there a CVE for any of these fixes?**
No. The changelog is the only notice, so vulnerability scanners and advisory feeds are unlikely to flag older versions.

**Do I need 2.1.319 first?**
No. Installing 2.1.321 includes everything in 2.1.319.

**Does the auto mode change affect Bedrock, Vertex or Foundry?**
No. The changelog says those providers are unchanged.

## What to do

1. 1 Upgrade Claude Code to 2.1.321 (which includes 2.1.319) on every machine and runner.
2. 2 If you use MCP servers with OAuth, check which hosts they redirect to and rotate refresh tokens if a server you do not control may have redirected requests.
3. 3 Behind an LLM gateway with telemetry or feature flags off, expect auto mode to ask or deny where it used to fall back to the built-in classifier, or set CLAUDE_CODE_AUTO_MODE_SERVER=0 to keep the old behaviour.

## Affected tools & models

Never need to catch up again

The weekly delta: only verdict changes and act-now items. No digest filler.
