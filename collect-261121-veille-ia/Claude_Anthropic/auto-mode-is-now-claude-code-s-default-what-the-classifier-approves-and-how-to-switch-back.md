---
id: collect-261121-veille-ia/claude-anthropic/auto-mode-is-now-claude-code-s-default-what-the-classifier-approves-and-how-to-switch-back
title: "Auto mode is now Claude Code's default: what the classifier approves, and how to switch back"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["classifier", "mode", "auto", "default", "switch", "claude", "prompt", "pushes"]
source: https://dev.to/rulestack/auto-mode-is-now-claude-codes-default-what-the-classifier-approves-and-how-to-switch-back-4j2j
source_anchor: ""
source_lines: [1, 197]
section: "Claude_Anthropic"
---
Starting today — August 14, 2026 — **auto mode is the default permission mode for new Claude Code sessions** on Pro, Max, and Team plans. If you've never touched your permission settings, your next session starts with a one-time switch prompt, and after that Claude runs most actions without asking you first.

That's a real behavior change, not a UI tweak. This post covers what the classifier actually approves, the defaults most people find surprising (pushes to `main` are allowed), how to keep human checkpoints where you want them, and how to switch back. Everything below is from the official docs as of today.

The permission modes doc states the change plainly: as of August 14, 2026, new sessions on Pro, Max, and Team plans start in auto mode. You can still switch modes whenever you want, an existing default you set yourself only changes if you accept a one-time switch prompt, and organization-managed defaults don't move at all.

Three details worth pulling out of that:

- 
**Only new sessions** are affected, and only on Pro, Max, and Team.
- 
**A `defaultMode` you set yourself survives.** If you already have`"defaultMode": "plan"` (or anything else) in your settings, nothing changes unless you accept the switch prompt.
- 
**Org-managed defaults are untouched.** If your admin distributes a mode via managed settings, today changes nothing for you.

Auto mode is not "skip permissions." It routes actions through a **separate classifier model** that reviews each action before it runs. The classifier blocks anything that escalates beyond your request, targets infrastructure it doesn't recognize as yours, or appears driven by hostile content Claude read (prompt injection).

Two rule layers still run **before** the classifier is ever consulted:

- 
`permissions.deny` rules block outright. Neither the classifier nor your stated intent can override them.
- 
`permissions.ask` rules force a prompt. An explicit ask rule is your stated intent to be asked, so the classifier cannot auto-approve a matching action.

So auto mode changes the *default* for unlisted actions, not your explicit rules.

**Pushes and PRs are allowed by default.** Auto mode allows pushes to any branch of the repository you're working in — *including the default branch* — and pull request creation. (Before v2.1.211 the classifier only allowed your working branch, branches Claude created, and routine pushes to the default branch; the current default is broader.)

There are still guardrails inside that: a branch whose name marks it as a deploy target (`production`, `release`, `gh-pages`) is judged on its own terms, including as a production deploy. Force pushes, a secret entering the commit, and history rewrites stay soft-blocked.

**Soft blocks can be cleared by explicit intent.** The classifier distinguishes "clean up the repo" (does not authorize a force-push) from "force-push this branch" (does). General requests don't count as explicit intent; naming the exact action does.

**Narrow allow rules bypass the classifier entirely.** A rule like `Bash(npm test)` carries into auto mode and resolves *before* the classifier — only broad rules like `Bash(*)` are suspended. A narrow prefix rule can therefore let a destructive argument through unseen. If you want every shell command classified regardless of your allow list, set:


```
{
  "autoMode": {
    "classifyAllShell": true
  }
}
```
(Requires v2.1.193+. It trades latency for coverage.)

If you like auto mode for everything except pushes and PRs, add content-scoped ask rules — they're evaluated before the classifier and always prompt:


```
{
  "permissions": {
    "ask": [
      "Bash(git push *)",
      "Bash(gh pr create *)"
    ]
  }
}
```
For a boundary that must *never* be crossed, use `permissions.deny` (ideally in managed settings for a team). Stating a boundary in chat ("don't push until I review") also works — the classifier reads it — but it can be lost when context compaction removes the message. Use an ask or deny rule for anything durable.

Per session: press `Shift+Tab` to cycle modes, or start with `claude --permission-mode manual`.

As a persistent default, set it in `~/.claude/settings.json`:


```
{
  "permissions": {
    "defaultMode": "manual"
  }
}
```
`manual` is the alias for the config value `default` (v2.1.200+; both work). One placement gotcha: `"defaultMode": "auto"` is **ignored** when it comes from a repo's `.claude/settings.json` or `.claude/settings.local.json` — since v2.1.142 a repository cannot grant itself auto mode. Your own opt-in or opt-out belongs in user settings. (If you're unsure which of your settings files wins, I wrote up the full precedence order.)

Org admins can disable auto mode for everyone by setting `permissions.disableAutoMode` to `"disable"` in managed settings.

Auto mode denials are recorded: open `/permissions` and check the **Recently denied** tab. Press `r` on a denial to let Claude retry it. Repeated denials for the same destination usually mean the classifier doesn't know that infrastructure is yours — name it in `autoMode.environment` in user or managed settings, then verify with:


```
claude auto-mode config
```
`claude auto-mode defaults` prints the built-in rule lists, and `claude auto-mode critique` reviews custom rules you've written. One warning from the config reference: if you set `environment`, `allow`, `soft_deny`, or `hard_deny` without including the literal `"$defaults"` entry, you **replace** the entire built-in list for that section — including the force-push and `curl | bash` soft blocks. Keep `"$defaults"` in the array unless you intend to own the whole list.

Auto mode's classifier also changes plan mode: with auto mode available, the `useAutoModeDuringPlan` setting is on by default, so shell commands during planning are reviewed by the classifier instead of prompting you. If you want the exact semantics of what plan mode blocks and what approving a plan switches you into, that's yesterday's post. And if your mental model of allow/ask/deny matching is fuzzy, this one covers the rule syntax.

The honest framing from the docs themselves: auto mode reduces prompts, it doesn't guarantee safety. Trust it with tasks where you trust the general direction — and put ask rules on the actions you'd want to see either way.

*More from this account on the same problem:*

- Claude Code permission rules: Bash(git push:*) stopped 8 of 14 ways to push, and 5 reached the remote
- Claude Code's Read deny rules let 4 of 11 routes through: grep -r, a Python one-liner and two CLAUDE.md @imports

*If the classifier approved or blocked something in your sessions that this post does not explain, please put the command and your Claude Code version in the comments below; I'll answer each one there. For a daily changelog-watch on AI coding tools.*

Everything below was accurate against the docs on publication day. Since then, 18 Claude Code releases have shipped, and a few of them refine sections of this post. The core claims — auto mode is the Pro/Max/Team default, pushes and PR creation are allowed by default including to `main`, and `permissions.deny`/`permissions.ask` still run before the classifier — are unchanged as of today's docs. Here's what moved.

**"Narrow allow rules bypass the classifier entirely" now has one fewer exception.** At publication, only broad rules (`Bash(*)`, wildcarded interpreters, package-manager run commands, `Agent` rules) were suspended on entering auto mode; a `Monitor` allow rule stayed in effect and let Monitor commands run unreviewed, since Monitor executes through the shell. **v2.1.236** closed that gap: "`Monitor` allow rules are now set aside while auto mode is active, so Monitor commands are reviewed the same way Bash commands are." The permission modes doc confirms this under "How the classifier evaluates actions": Monitor allow rules are now in the suspended list alongside the broad Bash/PowerShell rules.

**You no longer need to hand-edit `~/.claude/settings.json` to manage classifier rules or switch into auto mode.** Two additions:

- 
**v2.1.246** added an Auto mode tab to`/permissions` for viewing and editing the`allow` ,`soft_deny` ,`hard_deny` , and`environment` classifier rules directly, including toggling the built-in rules for a section on or off.
- 
**v2.1.247** added a one-keystroke "Yes, and switch to auto mode" option on a Bash command's permission prompt in Manual and`acceptEdits` mode — approve the command and flip the session into auto mode in one step (not offered for PowerShell prompts, or for prompts an`ask` rule or hook forced).

The article's "how to switch back" section (Shift+Tab, `--permission-mode manual`, `defaultMode` in settings) is still exactly how it works; these are additive shortcuts for the other direction.

**Auto mode's default block list grew.** The article's "blocked by default" list (from the docs at the time) didn't yet include a category the docs now call Containment Escape. **v2.1.257** added it: "cloud metadata-credential fetches, egress evasion, and cross-tenant reach are no longer auto-approved unless your environment marks them expected." The current auto mode configuration doc pairs this with a new environment slot, Host containment, for describing a container/VM/pod setup where these should be allowed. The same release added a one-time prompt before Claude's first file read outside the working directories, with a `permissions.blockReadsOutsideWorkingDirectories` setting to block such reads outright — this sits alongside, not inside, the classifier's own checks.

**A real gap in the "ask rules always force a prompt" claim got fixed, not just documented differently.** The article states explicit `ask` rules always prompt in auto mode, which was the intended behavior but had an edge case: **v2.1.257** "Fixed a `permissions.ask` rule being skipped in auto mode when the matching command ran inside a compound command or subshell, letting it run without the confirmation prompt." If you added the `Bash(git push *)` / `Bash(gh pr create *)` checkpoint rules from this post's "Keeping human checkpoints" section, they're more reliable now than they were at publication, since a push wrapped in `&&` or a subshell could previously have slipped past them.

**The org kill switch is now enforced live, not just at session start.** The article says an admin can set `permissions.disableAutoMode` to `"disable"` in managed settings. At publication, a session already running in auto mode kept it until it ended even after that policy arrived. **v2.1.251** fixed managed-settings `disableAutoMode` arriving mid-session to move an already-running auto-mode session back to default mode; the current docs note this explicitly ("Before v2.1.251, a running session kept auto mode until it ended").

No changes were found to the push/PR defaults, the `$defaults` splicing syntax for `environment`/`allow`/`soft_deny`/`hard_deny`, `autoMode.classifyAllShell`, or the rule that `defaultMode: "auto"` doesn't take effect from a repo's `.claude/settings.json` or `.claude/settings.local.json` — everything else in the original post stands as written.

Primary sources for this update:

- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/auto-mode-config
- https://code.claude.com/docs/en/settings

This is the second refresh of this post. The first (September 7) covered v2.1.232 through v2.1.257. This one re-reads the official CHANGELOG from v2.1.232 (the first release after this post went up) through **v2.1.270** — 30 releases — plus the current permission modes doc. The headline claims still hold as of v2.1.270: auto mode is the built-in starting mode on Pro, Max, and Team; pushes and PR creation are allowed by default, including to the default branch; `permissions.deny` and `permissions.ask` resolve before the classifier. What follows is every entry that qualifies a sentence in this post, including a few from the earlier range that the first refresh did not call out.

**"Only new sessions are affected."** As of v2.1.251 the one-time "make auto mode your default" offer waits until you've sent or cleared what you're typing, so the Enter that sends your prompt can't answer it, and the offer no longer appears in unattended sessions (for example agent-team teammate panes), where a stray keypress could accept it unread. v2.1.252 fixed the first launch on a fresh install starting in default mode instead of auto mode for accounts whose startup default is auto mode. The permission modes doc now also states a version floor this post never mentioned: the auto default requires v2.1.228 or later on macOS, Linux, and WSL, and v2.1.233 or later on native Windows; earlier versions start in Manual.

**"The classifier blocks anything that escalates beyond your request, targets infrastructure it doesn't recognize as yours, or appears driven by hostile content."** As of v2.1.261 this list has one more category: a link that packs content into a public diagram renderer's URL is treated as an upload to that site and is no longer auto-approved unless you asked for it. (v2.1.257's Containment Escape rule, covered in the first refresh, remains in place.) Two v2.1.236 entries the first refresh skipped: on Bedrock, Vertex AI, and Foundry, and when telemetry is disabled, the classifier now uses the same defaults as on the Claude API, including severity-scored classification; and the git status check Claude Code runs before a destructive command can no longer be fooled by a repo's `status.showUntrackedFiles=no` into reporting a clean tree.

**"`permissions.deny` rules block outright."** Still true, and the matcher got tighter in ways that matter if a deny rule is your boundary. As of v2.1.257, Bash `Read()`/`Edit()` deny rules apply to `< file` redirects and to reader commands like `tac` and `egrep`. As of v2.1.268, deny and ask rules on symlinked directories (`/etc`, `/tmp`, `/var` on macOS; `/bin` on Linux) apply when the path is given by its real location, Bash commands no longer ignore deny rules written on a symlinked path spelling, and a Read or Edit deny rule applies even when an `env -C`, `eval`, or similar command the checker can't analyze is on the same line. As of v2.1.269, `Edit()` deny rules and the write-path check cover the file a Bash `tee` writes, and a `Bash(tee:*)` allow rule no longer covers destinations outside the working directories. One entry went the other way: v2.1.259 extended Bash `Read()` deny rules to option values, `git diff`/`git grep` file operands, and `cd DIR && cat FILE` compounds, then v2.1.260 reverted "the 2.1.259 change applying `Read()` deny rules to Bash arguments" because it denied `npm run build` under a `Read(./**/build/**)` rule in every mode and made `cd … && grep` prompt even in auto mode. If you were on v2.1.259 exactly, that is why auto mode started prompting on ordinary commands.

**"Narrow allow rules bypass the classifier entirely."** Unchanged for `Bash(...)` rules; the Monitor exception closed in v2.1.236 (first refresh). v2.1.246 also added a startup warning for Bash allow rules with a wildcard before the subcommand, such as `Bash(git * main)`, since they also match options inserted before the subcommand — worth heeding here, because such a rule resolves before the classifier ever sees the command.

**"`"defaultMode": "auto"` is ignored when it comes from a repo's `.claude/settings.json` or `.claude/settings.local.json`."** As of v2.1.257 this applies to `"bypassPermissions"` too: set it in user or managed settings, or pass `--permission-mode`. The doc adds that a repo-level `"bypassPermissions"` doesn't take effect and the session starts in Manual mode.

**"Auto mode denials are recorded … Press `r` on a denial to let Claude retry it."** Unchanged. As of v2.1.268 the message Claude itself receives on a denial names the rule that blocked the action and asks Claude to try a safer method and finish unrelated work before stopping to ask you, so a single denial should interrupt less. v2.1.260 fixed `permissions.blockReadsOutsideWorkingDirectories` (introduced in v2.1.257, first refresh) on macOS hiding the user's git config from sandboxed git and hiding a worktree-isolated sub-agent's own checkout.

**Headless.** New since this post: as of v2.1.259, `--permission-prompts none` is available for unattended headless hosts. Anything that would prompt is denied automatically, while the active permission mode, auto mode included, keeps deciding. That is the flag to reach for if you run auto mode in CI with `ask` rules and want a hard "never wait" rather than a hung run.

No changes were found in v2.1.232 through v2.1.270 to `Shift+Tab` / `--permission-mode manual` / `"defaultMode": "manual"`, to `autoMode.classifyAllShell`, to the `"$defaults"` splicing rule for `environment` / `allow` / `soft_deny` / `hard_deny`, to `claude auto-mode config|defaults|critique`, to `useAutoModeDuringPlan` being on by default, or to `permissions.disableAutoMode` beyond the v2.1.251 live-enforcement fix already noted.

Primary sources for this update:

- https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md
- https://code.claude.com/docs/en/permission-modes

**What the classifier costs now depends on where it runs.** This post never covered the classifier's own bill, and a September 19 change makes it worth a paragraph. As of **v2.1.278**, Claude Code asks for server-side checks by default on Enterprise plans and accounts that use the Claude API, and on Amazon Bedrock, Google Cloud's Agent Platform and Microsoft Foundry. The changelog entry says the server-side classifier "does not charge for classifier overhead".

Where a platform, region or gateway can't deliver those checks, Claude Code falls back to its own classifier requests, which the docs describe as "billed as token usage as before", and it shows a notice before continuing. Pro, Max and Team plans never show the notice. In `-p` runs the text goes to stderr instead of holding the action.

Two things to check on your own setup:

- Run `/status` in an auto mode session. The**Auto mode server** row reads Enabled while the server's checks decide the session's actions, and Disabled once the session has fallen back.
- If you route Claude Code through an LLM gateway or proxy, that is the most common cause of a fallback according to the docs: one that strips or rewrites request headers, or drops request fields it doesn't recognize.

`CLAUDE_CODE_AUTO_MODE_SERVER=0` opts out on Bedrock, Vertex, Foundry and gateways. Nothing else in this post changes: `permissions.deny` and `permissions.ask` still run before the classifier.

Primary sources for this update:

- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/auto-mode-classifier-billing

A re-read of the CHANGELOG through v2.1.289 and the current permission modes doc found three places where this post is now out of date, and one we got wrong:

- 
**Not just Pro, Max and Team any more.** v2.1.284: "Changed interactive terminal and VS Code sessions to start in auto mode when no permission mode is configured, on every plan and provider;`permissions.defaultMode` still overrides it." The doc dates this to v2.1.283. On earlier versions, auto mode is the built-in starting mode only on Pro, Max and Team.
- 
**A notice, not a prompt, if you never touched your settings.** Per the doc, the first time the built-in default starts one of your sessions in auto mode, Claude Code "shows a notice that links to this page". The ask-once offer is for people whose`~/.claude/settings.json` already sets another`defaultMode` ; if you decline it, your setting stays.
- 
**The `env -C` / `eval` part of the September 15 deny-rule paragraph no longer holds.** v2.1.273: "Reverted a 2.1.268 change that checked Read and Edit deny rules on Bash lines the permission checker can't analyze (`eval` ,`env -C` )". Such lines prompt again instead of being denied.
- 
**Our version number was wrong.** The fresh-install fix the September 15 update credits to v2.1.252 is listed under v2.1.251.

We also could not find the v2.1.142 entry this post cites for a repository not being able to grant itself auto mode. The behavior itself matches the current settings reference: `auto` and `bypassPermissions` don't take effect from project or local settings.

Primary sources for this update:

## Top comments (10)

The classifier layer is a useful default, but I still treat pushing to main as a human decision even when auto mode would allow it. What has bitten me is not the flashy deny cases. It is the quiet approve of a rewrite that looks intentional and is wrong. I keep auto mode for reads and tests, then force an ask rule on git write and anything that mutates shared branches, because the model can be confident and the diff is where I decide whether that confidence was earned.

I've never managed to express 'mutates shared branches' as a clean match — the branch name usually isn't in the command string for the dangerous forms (

`git reset --hard`on whatever is checked out). Do you match on command shape and eat the false positives, or wrap the git calls in something matchable?
The compaction point is the one I'd worry about most. "Don't push until I review" is a useful instruction for a human, but it's a pretty weak control if it only lives in chat history. Anything that's actually a boundary probably belongs in permissions, hooks or config where it survives the rest of the session.

Same conclusion on our side. Our push goes through one script that re-runs the tests first, but picking that script is still just a CLAUDE.md line, not a deny rule.

Auto mode needs very plain audit language. If a classifier is deciding what is safe, the user needs to know which class was assigned and what capability that class actually unlocked.

Right — you get the outcome without the class, and the class is the half you'd need for review.

Right, and that missing class is where review gets fuzzy. If the system only records the outcome, you can tell what happened, but not whether the classifier boundary itself was reasonable.

I keep a short list of commands I expect to be blocked and re-run it after upgrades — it tells me when the boundary moved, which isn't the same as telling me it was ever in the right place. I'd been collapsing 'was it reasonable' into 'did it fire' until you split them.

That regression list is a good practical test. It does not prove the policy is ideal, but it catches silent boundary drift. I would pair it with one positive list too: commands you expect to pass. Otherwise an upgrade can make the system safer-looking by blocking work it was supposed to allow.

You're right, and it stings a little — the deny list alone was me testing my fears, not the boundary. If I revise the piece, the expect-pass set goes in first: an upgrade that over-blocks reads as "safer" on every metric except throughput, and throughput has a dozen other explanations.
