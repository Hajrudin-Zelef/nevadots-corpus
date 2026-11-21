---
id: collect-261121-veille-ia/claude-anthropic/control-what-claude-code-can-do-without-asking
title: "Control what Claude Code can do without asking"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "mode", "code", "session", "auto", "plan", "manual", "project"]
source: https://guides.rogerle.com/claude-code/permissions
source_anchor: ""
source_lines: [1, 182]
section: "Claude_Anthropic"
---
## On this page10 sections

Sooner or later every Claude Code user hits the same wall. Claude asks before every command, you answer "Yes" fifty times a day, and eventually you either stop reading the prompts or turn every check off. Both are worse than setting it up properly once.

This guide explains how Claude Code decides what it may do on its own, how to change that for a session or for good, and how to write rules that approve your routine commands and block the ones you never want run.

## How Claude Code decides

Every action Claude wants to take (reading a file, editing one, running a command, fetching a web page) goes through two layers:

- **The permission mode** sets the baseline for the whole session: ask about almost everything, accept file edits, only plan, or let a safety classifier decide.
- **Permission rules** sit on top of the mode. They approve, force a question about, or block specific tools and commands, whatever the mode.

Rules are enforced by Claude Code itself, not by the model. Writing "never push to git" in your CLAUDE.md changes what Claude tries to do, but it does not stop anything. A deny rule does.

## The permission modes

- **Manual** (`default` ): Claude reads files in your project without asking, and asks before editing files, running most commands or using the network. A small set of read-only commands, such as`ls` ,`cat` ,`grep` and read-only`git` commands, never ask. The setting is called`default` because Manual was the starting mode before Claude Code 2.1.228. On most plans, sessions now start in auto mode (see the note below).
- **Accept edits** (`acceptEdits` ): file edits inside your project run without asking, and so do common file commands such as`mkdir` ,`touch` ,`mv` ,`cp` and`rm` . Everything else still asks. Good when you review the changes afterwards with`git diff` .
- **Plan** (`plan` ): Claude reads and explores but edits nothing until you approve its plan. Good for unfamiliar code, or before a large change.
- **Auto** (`auto` ): Claude works without routine prompts, and a separate model, the classifier, checks each risky action before it runs. More on it below.
- **Don't ask** (`dontAsk` ): anything that would ask is refused instead. Only your pre-approved rules run. Meant for scripts and automation, where nobody is there to answer.
- **Bypass permissions** (`bypassPermissions` ): no permission prompts. Deny rules still block, and your ask rules and critical deletes still ask. Only for containers or virtual machines that Claude cannot damage.

Note

On Pro, Max and Team plans, a new terminal session starts in auto mode. On an Enterprise plan, with an API key, or in a `claude -p` run, it starts in Manual. The footer at the bottom of the screen always shows the current mode, for example `⏸ manual mode on` or `⏵⏵ auto mode on`.

## Switch modes

During a session, press `Shift+Tab` to move through the modes. From auto mode, the first press goes to Manual, and then the cycle runs Manual, Accept edits, Plan, and back. Auto mode appears at the end of the cycle when it is available to you.

To start a single session in a given mode, use the flag:

`claude --permission-mode plan`
To make a mode the default for every session on your machine, set `defaultMode` in your user settings:

```
{
  "permissions": {
    "defaultMode": "acceptEdits"
  }
}
```
Warning

`"defaultMode": "auto"` only works in `~/.claude/settings.json`. In a project's `.claude/settings.json` or `.claude/settings.local.json` it is ignored, so a repository you clone cannot switch your session into auto mode. The same goes for `"bypassPermissions"`: in our test, the session in a project that set it started in Manual mode. The other modes do apply when a project's settings set them. In a project that set `"acceptEdits"`, the session started in Accept edits mode, even in a folder that had never been trusted.

## Answering a prompt

When Claude wants to run a command that is not approved yet, it asks:

```
Run the project's lint script
 ❯ 1. Yes
   2. Yes, and don't ask again for: npm run *
   3. Yes, and switch to auto mode · auto mode handles these prompts for you
   4. No
```
- **Yes** approves this one command.
- **Yes, and don't ask again** saves a rule, so the same kind of command runs without asking from now on in this project. Read the rule before you pick it: the prompt above offered`npm run *` , which approves every npm script in the project, not only the lint script.
- **No** refuses. Press`Tab` on Yes or No to add a comment, such as "use the test database instead", and Claude reads it.

The saved rule goes into `.claude/settings.local.json` at the root of your git repository. Claude Code keeps that file out of your commits, so the rule stays yours. An approval for a file edit is different: it lasts only until the session ends.

## Write your own rules

Rules live in a settings file, in three lists: `allow`, `ask` and `deny`. Here is a sensible starting point for a project:

```
{
  "permissions": {
    "allow": [
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(git commit *)"
    ],
    "ask": [
      "Bash(git push *)"
    ],
    "deny": [
      "Read(.env)",
      "Read(.env.*)",
      "Read(secrets/**)"
    ]
  }
}
```
With this file, Claude runs the tests, the linter and commits without asking. It always asks before a push, even in auto mode. It cannot read any `.env` file in the project, or anything in a `secrets` folder. A `Read` deny rule also stops Claude's file tools from editing or overwriting those files.

Claude Code checks the lists in a fixed order: deny first, then ask, then allow. The first match decides. A deny rule therefore always wins, even over a more specific allow rule. We tested this with `Bash(git *)` in allow and `Bash(git push *)` in deny: `git tag` ran without asking, and `git push` was refused with:

Output

`Permission to use Bash with command git push origin master has been denied.`
### How the patterns match

- `Bash(npm test)` without a`*` matches exactly that command and nothing else.`npm test --watch` still asks.
- `*` matches any text, including spaces.`Bash(git commit *)` matches every`git commit` command, and also a bare`git commit` .
- Put the `*` after the subcommand.`Bash(git log *)` allows only`git log` , but`Bash(git *)` allows every git command, including`git push` and`git reset --hard` .
- The space before `*` matters.`Bash(ls *)` matches`ls -la` but not`lsof` .`Bash(ls*)` matches both.
- Claude Code understands `&&` ,`||` ,`;` and`|` . An allow rule for`npm test` does not approve`npm test && rm -rf build` : each part must be allowed on its own. Deny rules apply to every part, even inside a subshell.
- File rules (`Read` and`Edit` ) use gitignore-style patterns, relative to the folder where you started Claude. A bare file name such as`Read(.env)` matches that name in any subfolder too. For an absolute path, start with two slashes, as in`Read(//etc/**)` . In a project's settings, a single leading slash, as in`Edit(/src/**)` , means the project folder, not the root of the disk.
- Web rules use the domain: `WebFetch(domain:docs.python.org)` .

Warning

Bash rules match the command as Claude writes it. They are not a wall around the program. A deny rule for `Bash(curl *)` stops `curl https://example.com`, but not `/usr/bin/curl https://example.com` or `sh -c 'curl https://example.com'`. A `Read` deny rule does not stop a Python script from opening the file. When a restriction must really hold, turn on the sandbox with `/sandbox`, which enforces file and network limits at the operating system level.

## Where rules live

- **`~/.claude/settings.json`** , your user settings: every project on your machine. Good for personal defaults, such as`defaultMode` , or a deny rule for`Read(~/.ssh/**)` .
- **`.claude/settings.json`** in a project: shared with everyone who works on it, if you commit it to git. Good for the project's test and build commands.
- **`.claude/settings.local.json`** in a project: only you, only this project. This is where "Yes, and don't ask again" writes.
- **Managed settings** , deployed by an organization: they override everything else and cannot be changed by users.

When more than one file has rules, the lists are combined, not replaced. A deny rule in any file blocks the action, whatever the other files allow.

Type `/permissions` in a session to see every rule in force and the file it comes from. You can add and remove rules there too, even while Claude is working.

## The trust dialog guards project rules

A project's `.claude/settings.json` could approve anything, so Claude Code does not apply its allow rules until you trust the folder. The first time you start Claude Code in a folder with such rules, the trust dialog lists them:

```
⚠ This folder pre-approves 2 tool permissions in .claude/settings.json:
Bash(npm test) and Bash(git commit *)
These will apply without asking. Only proceed if you trust this configuration.
```
Deny and ask rules apply right away, trusted or not, because they only restrict. In a `claude -p` run, where there is no dialog, an untrusted folder's allow rules are skipped and Claude Code warns about it on the error output:

Output

`Ignoring 2 permissions.allow entries from .claude/settings.json: this workspace has not been trusted.`
If you run `claude -p` in a repository you did not write, add `--setting-sources user` so that Claude Code reads only your own settings and ignores the project's files.

## Auto mode

In auto mode Claude works without routine prompts. Reads and edits inside your project run straight away. Other actions, such as shell commands, network requests and edits outside the project, go to the classifier first, which checks them against what you asked for.

By default the classifier blocks, among other things:

- downloading and running code, such as `curl ... | bash`
- sending sensitive data to outside services
- production deploys and database migrations
- force pushes and `git reset --hard` on uncommitted work
- changes to shared infrastructure
- deleting files that existed before the session

When it blocks something, Claude sees the reason and tries another way, or asks you. A real example: when the request was only "do both", without saying what to change, auto mode refused one of the two, a change to a web server's main configuration file, with the reason `Modify Shared Resources`. When a later message named the file and the exact change, the classifier let it through. That matches the documentation: a request in the conversation clears a block only when it names the action and what makes it risky.

Things to know:

- Boundaries you state in the conversation count. If you say "don't push", the classifier blocks pushes. But it reads them from the conversation, so they can be lost when a long session is compacted. For a hard rule, use a deny rule.
- `ask` rules still prompt you in auto mode. That makes them the right tool for "go ahead, but check with me before pushing".
- On entering auto mode, broad allow rules such as `Bash(*)` or`Bash(python*)` are switched off, because they would let anything past the classifier. Narrow rules such as`Bash(npm test)` stay. They come back when you leave auto mode.
- If the classifier blocks 3 actions in a row, or 20 in a session, auto mode pauses and Claude Code starts asking you again.
- Blocked actions are listed in `/permissions` under**Recently denied** , where you can press`r` to retry one with your approval.

Warning

Auto mode lowers the number of prompts. It does not make every action safe. Use it for work where you trust the general direction, and stay in Manual for sensitive work on a live server.

## What no rule can approve

Some actions always go through a check, even with an allow rule that matches them:

- **Protected paths.** Writes to`.git` ,`.claude` ,`.vscode` ,`.bashrc` ,`.zshrc` ,`.gitconfig` ,`.mcp.json` and a few other configuration files are never approved by allow rules. Manual mode asks, auto mode sends them to the classifier.
- **Critical deletes.**`rm` or`rmdir` aimed at the root of the disk, a top-level directory such as`/etc` or`/usr` , your home directory, or the project directory itself are never approved by allow rules either.

Only `bypassPermissions` skips the protected-path check, and even it still asks before a critical delete. Do not use it on a machine you care about. On Linux and macOS, Claude Code refuses to start in that mode as root or under `sudo`.

## Sources

- Configure permissions, Claude Code documentation
- Choose a permission mode, Claude Code documentation
- Settings files and precedence, Claude Code documentation
- Sandboxing, Claude Code documentation

Checked on

Claude Code 2.1.280 on AlmaLinux 9.8. The rules in this guide were tested in a throwaway git project: allow, deny, file patterns in subfolders and the deny-over-allow order in `claude -p` runs, the trust dialog and "Yes, and don't ask again" in an interactive session. The prompt and dialog texts above were copied from those sessions. Auto mode behavior is taken from the documentation, apart from the one real block described.
