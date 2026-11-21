---
id: collect-261121-veille-ia/claude-anthropic/claude-code-auto-mode-the-new-default-ssd-nodes
title: "Claude Code auto mode: the new default · SSD Nodes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["mode", "claude", "code", "auto", "default", "permission", "sessions", "start"]
source: https://www.ssdnodes.com/learn/claude-code-auto-mode-permissions
source_anchor: ""
source_lines: [1, 199]
section: "Claude_Anthropic"
---
## What auto mode changes on 14 August 2026

Claude Code auto mode runs tool calls without routine permission prompts, and uses a separate classifier model to review actions. Reads and working-directory edits outside protected paths skip Claude Code's own classifier requests, but server-side review can still block file edits and read-only shell commands. From 14 August 2026 it became the mode new sessions start in on Pro, Max, and Team plans. As of 27 October 2026 the documentation goes further: with Claude Code v2.1.283 or later, auto mode is the built-in starting mode for interactive terminal and VS Code sessions on every plan and provider. You can switch modes at any time, and a default you already set for yourself is not overwritten.

The documentation now states the change like this:

With Claude Code v2.1.283 or later, auto mode is the built-in starting permission mode for interactive terminal and VS Code sessions. On earlier versions, it's the built-in starting permission mode only on Pro, Max, Ultra, and Team plans.

Two rules on the same page matter more than the version number. A `defaultMode` you set in your own `~/.claude/settings.json` survives the change: Claude Code may ask once whether to switch it to auto mode, and keeps your setting if you decline. A default your organization deploys through managed settings survives it too. The announcement post said auto mode would stay opt-in on Enterprise plans and the Claude API at first, and an update to it on 22 October 2026 says Enterprise admins will get an email before auto mode becomes the default for their organization. From v2.1.283 the documentation no longer makes that exception for terminal and VS Code sessions. With the HIPAA configuration applied, eligible sessions start in Manual mode only on v2.1.285 through v2.1.325; terminal and VS Code sessions on v2.1.326 or later start in auto mode by default.

If you run Claude Code on a VPS (virtual private server), the change matters more than it does on a laptop. A permission prompt is a checkpoint that needs a person at the keyboard. On a remote box you are often not there, so the mode a session starts in is the mode it stays in for hours. Non-interactive `claude -p` runs follow a different rule: with v2.1.302 or later they start in `default` unless you set a mode, including on a third-party provider or with telemetry turned off. The permission-modes page documents this behaviour. Before v2.1.302, those two cases could start in `auto`, so run `claude --version` on the box if it might still be on an older release.

## The Claude Code permission modes, from most oversight to least

There are six modes. The name at the start of each line is the value you write in settings or pass to `--permission-mode`. All six are enforced by Claude Code itself rather than by the model, because deciding what a tool call is allowed to do is the job of the harness around the model.

- `default` : Claude asks before most actions that edit files, run shell commands, or reach the network. Reads inside your working directory still run without a prompt. The CLI (command line interface) labels this mode**Manual** and accepts`manual` as an alias.
- `plan` : Claude reads files and runs commands to explore, and does not edit your source. Edits stay blocked until you approve the plan, except in interactive terminal sessions with bypass permissions available: there, neither the classifier nor a prompt applies to planning commands.
- `acceptEdits` : file edits run without a prompt, along with the filesystem commands`mkdir` ,`touch` ,`rm` ,`rmdir` ,`mv` ,`cp` and`sed` . This applies only to paths inside your working directory or your`additionalDirectories` . Protected-path writes and recognized critical-path removals still prompt, and`touch` is auto-approved only with plain file paths. Every other shell command follows the regular permission rules.
- `auto` : routine permission prompts are replaced by classifier checks. Explicit`ask` rules still force a prompt.
- `dontAsk` : Claude Code auto-denies anything that would have prompted you. Apart from calls a mod you installed can approve, file reads inside your working directories, your`allow` rules, the built-in read-only Bash commands, preapproved documentation fetches, and calls a`PreToolUse` hook approves will run. The session never waits for input.
- `bypassPermissions` : prompts and safety checks are skipped, including writes to protected paths such as`.git` and`.claude` .

Press `Shift+Tab` during a session to cycle `default` to `acceptEdits` to `plan`. From `auto`, the first press drops you to `default`. The status bar shows where you landed, such as `⏵⏵ auto mode on` or a gray `⏸ manual mode on`. The other modes are not in that cycle by default. `auto` joins it once your account meets the requirements for it. `bypassPermissions` joins it only when the session was started with `--permission-mode bypassPermissions`, `--dangerously-skip-permissions` or `--allow-dangerously-skip-permissions`, or with `defaultMode: "bypassPermissions"` in user, `--settings` or managed settings. `dontAsk` never appears there, so set it with `claude --permission-mode dontAsk`.

Auto mode also needs a recent model, which is the usual reason it does not appear at all. As of 27 October 2026 the documentation lists Claude Opus 4.6 or later, Sonnet 4.6 or later, Haiku 5.5, or a Fable model on the Anthropic API, a shorter list on Amazon Bedrock, Google Cloud and Microsoft Foundry, and says older models such as Sonnet 4.5 are not supported on any provider. If Claude Code reports auto mode as unavailable, check those requirements and any `disableAutoMode` setting first. Anthropic can also turn auto mode off server-side, and a session that got that answer keeps auto mode off until it ends, so start a new session later rather than waiting inside the old one.

Two controls hold in every mode, including `bypassPermissions`: `deny` rules and explicit `ask` rules. Neither applies to `EndConversation` while another tool remains available. Those are the levers you keep whichever mode a session starts in.

## What the auto mode classifier blocks

The classifier is a second model that reads the pending action and decides whether it fits what you asked for. The documentation describes its job in one sentence:

A separate classifier model reviews actions before they run, blocking anything that escalates beyond your request, targets unrecognized infrastructure, or appears driven by hostile content Claude read.

Blocked by default, in the categories a server operator meets most often:

- Downloading and executing code, such as `curl | bash`
- Production deploys and migrations
- Force push
- Modifying shared infrastructure
- Opening a tunnel or reverse shell that makes a local service reachable from the public internet
- Printing a live credential or token into the transcript or a file

Allowed by default:

- Local file operations in your working directory
- Installing dependencies declared in your lock files or manifests
- Read-only HTTP requests
- Pushing to branches of the repository you are working in, including the default branch, except non-default branches whose names mark them as deploy or publication targets. The push's content still goes through the classifier's other rules.

Do not work from a summary like the one above. Run `claude auto-mode defaults` to print the full rule lists as JSON, and read the set that shipped with the version you have installed. `/auto-mode-setup` is a separate command: it drafts `autoMode.environment` entries from your project and your recent sessions in it, and writes them to `~/.claude/settings.json` if you accept the draft. That command, not `claude auto-mode defaults`, carries the version requirements. On Pro, Max, Ultra and Team plans `/auto-mode-setup` needs v2.1.228 or later, or v2.1.233 or later on native Windows. On an Enterprise plan or with a Claude Console API key it needs v2.1.283 or later. Before v2.1.304, `/auto-mode-setup` is missing from sessions that do not fetch feature flags, such as a box with telemetry turned off or one on a third-party provider, so update first if that describes your server.

Two documented limits matter before you lean on this. First, in the classifier requests Claude Code sends itself, the classifier sees your messages, the tool calls other than read-only lookups such as file reads and searches, and your `CLAUDE.md` content, and tool results are stripped out, so text inside a file or a web page Claude read cannot address the classifier directly. Second, when the classifier blocks an action 3 times in a row or 20 times in one session, auto mode pauses and Claude Code goes back to prompting you. Approving the prompted action resumes auto mode. Those thresholds are not configurable. A non-interactive run with the `-p` flag and no `--permission-prompt-tool` has nobody to prompt, and the documentation is explicit that Claude Code does not stop the run: the blocked action does not run and Claude keeps working.

That second behaviour is the one that bites on a remote box, in two different ways. An interactive session left running that trips the limit stops at a prompt and waits for a person who is not looking at the terminal. A `-p` run never waits, so a job can finish with the blocked steps left undone. A second Claude Code session on the same box is not a substitute for that person, since text sent from one session to another never counts as your consent and cannot answer a pending permission prompt, so a session paused at one still needs a human answer. Narrowing what the agent sets out to do is the other half of the fix, and a skill that pushes the agent toward the smallest change that works keeps a session from wandering into the sprawling actions the classifier stops. Where a skill steers one task, an output style edits the system prompt itself, so a style that asks for small, surgical changes holds for every turn of the session rather than the one job you invoked it for.

## Where the modes live in settings.json

Everything above is one object in a settings file.

```
{
  "permissions": {
    "defaultMode": "auto",
    "allow": [
      "Bash(npm run test *)",
      "Bash(git status)"
    ],
    "ask": [
      "Bash(git push *)",
      "Bash(docker compose up *)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./secrets/**)",
      "Bash(curl *)"
    ]
  }
}
```
Rules are evaluated in order: deny, then ask, then allow. The first match in that order decides the outcome, and a narrower rule does not beat a broader one that comes earlier. A deny rule for `Bash(aws *)` blocks `aws s3 ls` even when you also allowed that exact command, so an allow rule cannot carve an exception out of a deny rule. Read and Edit deny or ask rules do support gitignore negation: in one settings file's `deny` list, `Read(*.env)` followed by `Read(!sample.env)` exempts files named `sample.env` from that preceding rule. The exception reaches only earlier relative-path rules from the same settings source; it cannot override a deny from another source or reopen a file inside a directory blocked as a whole.

`ask` is the rule type that earns its keep in auto mode. Auto mode removes the routine prompt, and an `ask` rule puts one back for the specific command you want a person on. Your deploy command belongs there. So does `Bash(git push *)` if you want a checkpoint before code leaves the box. Keeping credential files in `deny` is the other half of that, and it pairs with keeping credentials out of an agent's reach in the first place.

The settings files themselves, lowest precedence first:

- `~/.claude/settings.json` : your user settings, applied in every project.
- `.claude/settings.json` : project settings, committed to the repository.
- `.claude/settings.local.json` : your own settings for one repository, git-ignored when Claude Code creates it; add it to`.gitignore` if you create it by hand.
- Managed settings, deployed by an administrator. On Linux that file is `/etc/claude-code/managed-settings.json` . Nothing overrides a managed permission rule, including a command line flag.

One trap here has a documented cause. `defaultMode: "auto"` does not take effect when it comes from `.claude/settings.json` or `.claude/settings.local.json`, so that a repository cannot grant itself auto mode by shipping a settings file. `bypassPermissions` is ignored from those two files as well. Set `auto` there and Claude Code falls back to its built-in default, skipping any `defaultMode` in `~/.claude/settings.json`. On a version or plan where that built-in default is Manual, the session quietly starts in `default` mode. Move the line to `~/.claude/settings.json`. Run `/permissions` to list every active rule next to the file it came from.

## The two switches that turn a mode off

Administrators get two kill switches, and both take the string `"disable"` rather than a boolean.

```
{
  "permissions": {
    "disableAutoMode": "disable",
    "disableBypassPermissionsMode": "disable"
  }
}
```
The documentation specifies that both settings accept the string `"disable"` in any settings file. These are most useful in managed settings where they can't be overridden.

`disableAutoMode` removes `auto` from the `Shift+Tab` cycle, and a session started with `--permission-mode auto` starts in Manual mode instead. `disableBypassPermissionsMode` does the same job for bypass mode, and it works from any scope, so you can set it in your own `~/.claude/settings.json` to lock yourself out of a mode you would rather not reach for at 2am on a live server. On a box other people use, put both in `/etc/claude-code/managed-settings.json` instead, because a user settings file belongs to the user and a managed one does not.

## Why auto mode on a VPS needs an isolation boundary

The classifier reviews one action at a time. It does not contain what an approved action then does. The documentation draws the line plainly:

The classifier is a per-action control, not an isolation boundary, so an isolation boundary still adds defense in depth for unattended runs, and is not required the way it is for `--dangerously-skip-permissions`.

So the pairing for a remote box is auto mode plus an environment you are willing to lose, not `bypassPermissions` plus hope. Bypass mode is documented for isolated environments only: containers, virtual machines, or dev containers without internet access, where Claude Code cannot damage your host system. A VPS running your database and your reverse proxy is none of those.

Three things carry most of the weight on a server. Run Claude Code as a normal user, never root. Give that user a working directory and nothing else worth reading. Rebuild the box rather than repair it, which is the argument for a disposable VM you throw away after each job. The hardening detail behind those, from user creation to firewall rules, is covered in the full safety pass for running Claude Code on a VPS, so it is not repeated here.

Claude Code enforces the root rule itself. On Linux and macOS it refuses to start in bypass mode under `sudo` or as root:

`--dangerously-skip-permissions cannot be used with root/sudo privileges for security reasons`
That check is skipped inside a recognized sandbox, which is why the documented answer for autonomous container work is a dev container that runs Claude Code as a non-root user. If you drive the agent from a phone or a laptop over SSH, the same reasoning applies to a long-running Claude Code session held open in tmux: nobody is watching the prompt while the session runs. None of it starts until the login itself works, so if the box turns you away with `Permission denied (publickey)`, the verbose SSH output will tell you which of the five faults you actually have.

## Set up the Bash sandbox on an Ubuntu VPS

The built-in sandbox restricts the filesystem and network access of sandboxed Bash commands Claude runs, and the operating system enforces that on child processes too. Excluded commands and approved unsandboxed retries run outside it. On Linux it needs two packages.

`sudo apt-get install bubblewrap socat`
Start Claude Code and run `/sandbox`. The panel opens with Mode, Overrides and Config tabs. If a required package is missing, a Dependencies tab is the only one shown, listing what to install. The dependency check runs at startup, so restart Claude Code after installing the packages or the panel will still report them as absent.

Ubuntu's AppArmor restriction on unprivileged user namespaces can stop bubblewrap from starting the sandbox. Ubuntu 25.04 introduced a bundled `bwrap-userns-restrict` profile that allows bubblewrap to create user namespaces and set up sandboxing. Check whether the restriction is enabled on your box:

`sysctl kernel.apparmor_restrict_unprivileged_userns`
A `0`, or an error saying the key does not exist, means this restriction is not the cause. A `1` means the restriction is enabled, but a suitable `bwrap` profile may already be installed. Add the following profile only if Claude Code runs `/usr/bin/bwrap` and `/sandbox` reports Ubuntu's AppArmor restriction, or sandboxed commands fail with `bwrap: setting up uid map: Permission denied` or `bwrap: loopback: Failed RTM_NEWADDR: Operation not permitted` while the restriction is enabled:

```
sudo tee /etc/apparmor.d/bwrap > /dev/null <<'EOF'
abi <abi/4.0>,
include <tunables/global>
profile bwrap /usr/bin/bwrap flags=(unconfined) {
  userns,
  include if exists <local/bwrap>
}
EOF
sudo systemctl reload apparmor
```
The profile applies to `bwrap` itself, not to the commands it runs inside the sandbox. Restart Claude Code and confirm that sandboxed commands work. Then narrow the boundary in settings:

```
{
  "sandbox": {
    "enabled": true,
    "filesystem": {
      "denyRead": ["~/"],
      "allowRead": ["."]
    },
    "network": {
      "allowedDomains": ["github.com", "*.npmjs.org"]
    }
  }
}
```
That block belongs in the project's `.claude/settings.json`, because `.` resolves to the project root only from project settings. Put the same lines in `~/.claude/settings.json` and `.` resolves to `~/.claude` instead, so the `denyRead` rule keeps your project files blocked and every command fails to read the code it is meant to edit.

Know what this does not cover. The sandbox constrains Bash and its child processes. The built-in file tools run inside the Claude Code process, and MCP (model context protocol) servers and hooks are separate processes that run unconstrained on the host. To put all of them behind one boundary, run the whole Claude Code process inside a container, a virtual machine, or the `@anthropic-ai/sandbox-runtime` package, which is a beta research preview at the time of writing.

## Which mode does each setup deserve?

### A solo repo on your own laptop

Use `auto`, with `ask` rules on the actions you want to see go past. You are at the keyboard, the classifier fallback can actually reach you, and the damage is bounded by one machine you control. This is the case the 14 August default was written for.

### A shared VPS

Use `auto` per user, set in each user's own `~/.claude/settings.json`, on a box where the account running Claude Code is not root and cannot read the other users' work. Deploy `disableBypassPermissionsMode` as `"disable"` in `/etc/claude-code/managed-settings.json`, together with the deny rules that protect shared paths. A shared box is the clearest case where `bypassPermissions` is wrong, because the isolation boundary that mode assumes does not exist: the other tenants are inside it.

### CI and unattended sessions

Use `dontAsk` with an explicit `allow` list of the commands the job needs. Auto-deny is the correct failure mode when no person will ever see a prompt. Auto mode runs non-interactively too, but a `-p` run that hits repeated classifier blocks does not stop: the blocked actions are skipped and Claude keeps working, so a job can finish with some of its steps left undone. From v2.1.319, you can also set `permissions.denyInsteadOfPrompting` to `true` in user or managed settings to deny calls that would otherwise prompt while using auto mode. With that setting on, 20 consecutive refused actions end the turn, and a `-p` run reports `error_during_execution`. Keep `bypassPermissions` for a container or virtual machine you rebuild from an image, and off any host that also runs something you care about.

## FAQ

### When does auto mode become the default in Claude Code?

It became the default on 14 August 2026 for new sessions on Pro, Max, and Team plans. As of 18 November 2026, the documentation says that with Claude Code v2.1.283 or later it is the built-in starting mode for interactive terminal and VS Code sessions on every plan and provider. With the HIPAA configuration applied, eligible sessions start in Manual mode only on v2.1.285 through v2.1.325; terminal and VS Code sessions on v2.1.326 or later start in auto mode by default. Non-interactive `claude -p` runs start in `default` unless you set a mode. Before v2.1.302, runs on a third-party provider or with telemetry off could start in `auto`; from v2.1.302 they start in `default` as well. A default you set yourself stays in place unless you accept the one-time switch prompt, and a default your organization manages is unchanged. Check what a session is actually in by looking at the status bar, which reads `⏵⏵ auto mode on` in auto mode.

### Should I use auto mode or bypassPermissions on a VPS?

Auto mode, paired with an isolation boundary. The classifier reviews actions before they run, with reads and working-directory edits outside protected paths skipping Claude Code's own classifier requests, although server-side review can still block file edits and read-only shell commands. The documentation is explicit that it is a per-action control and not an isolation boundary, so an unattended run still wants a container, a virtual machine, or a box you are willing to rebuild. `bypassPermissions` skips the checks entirely and is documented for isolated environments only. Claude Code refuses to start in that mode as root on Linux and prints `--dangerously-skip-permissions cannot be used with root/sudo privileges for security reasons`.

### How do I stop anyone on my server from using auto mode or bypass mode?

Set `permissions.disableAutoMode` and `permissions.disableBypassPermissionsMode` to the string `"disable"` in `/etc/claude-code/managed-settings.json`. Managed settings sit above every other scope, so no user settings file and no command line flag can override them. `disableAutoMode` removes `auto` from the `Shift+Tab` cycle, and a session started with `--permission-mode auto` starts in Manual mode instead. `disableBypassPermissionsMode` also works from any scope, so a single user can set it in their own `~/.claude/settings.json`.

### Why is my `defaultMode: "auto"` setting ignored?

Because it is in the wrong file. `defaultMode: "auto"` does not take effect when it comes from `.claude/settings.json` or `.claude/settings.local.json`, so that a repository cannot grant itself auto mode by shipping a settings file. Claude Code falls back to its built-in default instead, skipping any `defaultMode` in your user settings; where that built-in default is Manual, the session starts in `default` mode. Move the setting to `~/.claude/settings.json`, then run `/permissions` to confirm which file each active rule came from. If auto mode is still unavailable, check the model requirement: older models such as Sonnet 4.5 are not supported on any provider.
