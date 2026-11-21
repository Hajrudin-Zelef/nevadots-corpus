---
id: collect-261121-veille-ia/claude-anthropic/claude-code-changelog-claude-code-docs-1
title: "Claude Code changelog - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["changed", "code", "claude", "added", "tasks", "mode", "scheduled", "most"]
source: https://code.claude.com/docs/en/changelog
source_anchor: ""
source_lines: [1, 1500]
section: "Claude_Anthropic"
---
`claude --version` to check your installed version.
**Added**

- Added a trust dialog warning that says where a folder’s hooks are declared
- Added a warning in the conversation, in Claude Desktop and other non-interactive hosts, when hooks in managed settings fail to load
- Added this session’s scheduled tasks, `/loop` wakeups, the goal and this conversation’s forks to`/tasks` ; x stops one after asking
- Added a “Server message” row to a server’s page in `/mcp` that shows the JSON-RPC error message of a remote HTTP MCP server that refuses the connection
- Added a “Marketplace auto-update” row to `/status` that names managed marketplaces that get no automatic updates, and why
- Added support for turning on Code Mode while OpenTelemetry events and metrics are enabled; each tool call the code makes sends its events; tracing still keeps it off
- [VSCode] Added a line above the message box after you decline the trust prompt, with Review trust to bring the prompt back without reloading the window

**Changed**

- Updated computer use on macOS to newer safety rules, so some actions are refused and some apps have less access
- Changed saved scheduled tasks: in new signed-in sessions, most saved by earlier versions need scheduling again, though `--resume` of their own session still runs them
- Changed `/code-review` with no target on most models to review the whole branch against the default branch, and fixed it losing the diff on multi-commit branches
- Changed auto mode to run `git status` less often so most requests wait less in large repositories; a git command that discards changes may be blocked until the working tree is checked
- Changed `/code-review <pr number> --fix` , and`/code-review` given a pull request URL, to stop without reviewing when`gh pr diff` is refused or fails
- Changed ‘Exit and stop tasks’ to end this session’s scheduled tasks so they don’t run again on resume, unless you exit while Claude is responding
- Changed some teammate panes to start through /bin/sh, so they may not read shell start-up files
- Changed computer use on macOS to give up on an app launch that has not finished after 30 seconds
- Gateway: Changed the start-up warning shown while `access_control.allow_cidrs` is empty to a notice at every start, boxed in text logs: never expose the gateway to the public internet
- Self-hosted runner: Changed a second host name’s clone of the same owner/name to go under `_repos/clones/<host>/` in the base folder; no clone moves
- Self-hosted runner: Changed repositories with unusual characters in their address path to clone anew in a new folder; rules and volumes naming the old one stop applying, and the old clone stays put
- Self-hosted runner: Changed `--repository-folder` to take most folders in the base folder at`--capacity 1` (above 1, one there stops start-up); another repository’s session overlapping one is refused
- Changed Code Mode so a PreToolUse hook that errors no longer refuses a script’s tool call, as outside Code Mode; set `"onFailure": "block"` on a command, HTTP or MCP tool hook that must refuse
- Changed Code Mode to refuse a script’s read of a line range over 256 MiB, which could use gigabytes of memory, and fixed reads of very long lines timing out
- Changed Code Mode to also start when telemetry, nonessential traffic or feature flags are turned off; an administrator can keep it off in managed settings
- Changed Code Mode to run a script that declares its own `log` ,`preview` or`sleep` , which it used to refuse
- Changed macOS login renewal to test the Keychain with a `Claude Code-write-check` item
- Changed the `/tasks` header and the footer’s background count to one number covering shells, monitors, MCP calls, agents, workflows and scheduled tasks
- Changed Stop and SubagentStop hooks to list a Monitor’s script as `monitor` , not`shell` , in`background_tasks`
- Removed 2.1.326’s automatic retry at the limit named in a `max_tokens` error; behind a gateway whose model takes fewer output tokens, set`CLAUDE_CODE_MAX_OUTPUT_TOKENS` to that limit
- [VSCode] Changed Down to stop at the last item of a permission prompt instead of wrapping to Yes
- [VSCode] Changed command output to show hidden characters and control sequences other than plain colors as symbols, so Persian, Khmer, Lao, Sinhala and Malayalam text can look broken or out of order

**Added**

- Added auto mode for Claude Opus 4.6 and Sonnet 4.6 on Amazon Bedrock, Google Cloud’s Agent Platform, Microsoft Foundry and Claude apps gateway; interactive sessions with no permission mode start in it
- Added `"onFailure": "block"` for`mcp_tool` hooks, as for command and HTTP hooks: a hook whose call fails blocks the action
- Added a warning when a message from a session that skips permission prompts may be held and dropped by a cloud session
- Added a notice in `/status` and`claude doctor` when an OpenTelemetry exporter has no endpoint and its data would likely be lost
- Added `CLAUDE_CODE_DISABLE_DEFERRED_TOOL_RESUME` , which lets a host stop`claude -p --resume` and`--continue` from running a tool call that a hook deferred
- Added support for apps that embed Claude Code to switch on the policy plugin’s `allowManagedModsOnly` option; an admin’s managed settings still win
- Added a startup warning to the Claude apps gateway when a policy’s `maxEffortLevel` is not a valid effort level
- Self-hosted runner: Added `--repository-folder` to name the folder that sessions use for a repository at`--capacity 1` , in place of its path under the base folder, which can’t be a symbolic link
- Self-hosted runner: Added a start-up warning when the host’s settings.json sets a default permission mode that sessions ignore
- Self-hosted runner: Added `--hook-label` to`claude self-hosted-runner orchestrator` , to hand chosen session labels to the`spawn-runner` hook
- Self-hosted runner: Added `--metadata key=value` to`claude --environment` , to label a session for the`spawn-runner` hook
- Self-hosted runner: Added `claude_code_self_hosted_orchestrator_session_queue_wait_by_reason_seconds` to the orchestrator’s`/metrics` , so a re-opened session’s wait can be told from a new one’s
- Self-hosted runner: Added an orchestrator metric, `claude_code_self_hosted_orchestrator_sessions_given_up_total` , that counts by cause how often Anthropic gave up starting a runner for a session
- [VSCode] Added a notice when a message from another session is held, with a link to the settings file that can allow it

**Changed**

- Changed plan mode and Bash prompts for Opus 4.6 and Sonnet 4.6 on third-party providers to use and offer auto mode, as on newer models; `disableAutoMode` turns both off
- Changed Code Mode to no longer run UserPromptSubmit hooks, managed ones included, on scripts’ output; to redact that output, use a PostToolUse hook with `updatedToolOutput`
- Changed `/code-review` on most models to report only bugs and CLAUDE.md violations; run`/simplify` for reuse, simplification and efficiency cleanups
- Changed `/code-review` to mark each finding Important, Pre-existing or Nit on most models, and to end each summary with a suggested fix; a clean review’s list now reads “Code review found no issues.”
- Changed `deniedMcpServers` entries of the form`claude.ai <name>` to apply to that connector in cloud and self-hosted runner sessions too
- Changed managed `PostToolUse` hooks to no longer fire for calls refused before they ran: no log entry, rewrite, block or stop there;`PostToolBatch` gets most, not marked as refused
- Changed `/bug` to ask Yes or No before sending, so repeated Enters no longer send a report or open GitHub
- Changed subagents in `claude -p` , the Agent SDK, desktop and VS Code to wait for their own subagents by default when`CLAUDE_CODE_FORK_SUBAGENT=0` is set
- Changed `claude -p` with`CLAUDE_CODE_FORK_SUBAGENT=0` to count a background subagent and those it waits for against one 10-minute wait; raise`CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS` for longer chains
- Changed a shell command rewritten by a PermissionRequest hook to a sandbox-excluded command to run outside the sandbox
- Changed the Claude in Chrome welcome screen and `/chrome` to say that Claude Code asks before Claude acts on a site, unless your permission mode or a rule in`/permissions` answers
- Changed Claude Desktop and Agent SDK sessions on third-party providers or with telemetry or feature flags off to end the turn when closed with a question open, instead of continuing without an answer
- Changed pasted text to be marked for Claude as pasted, on third-party providers or with telemetry or feature flags turned off
- Changed subagent progress summaries in terminal sessions on the Anthropic API to be written by the default small fast model from short notes, also with telemetry off, using far fewer tokens
- Changed how a sandbox that can’t start is reported, unless `sandbox.failIfUnavailable` is set: the next sandboxed Bash command is refused and says that the sandbox is off for the session
- Changed `/list-agents` to show its list to you alone; Claude lists sessions itself when needed
- Self-hosted runner: Changed the built-in `gh api` to be off by default where settings guard Claude’s GitHub tools by name;`--use-anthropic-gh true` turns it on
- Self-hosted runner: Changed a `claude` that a wrapper script runs before the session’s own`claude` is up, or after it has ended, to have no Anthropic-managed git
- Self-hosted runner: Changed `CLAUDE_RUNNER_SESSION_UUID` and`CLAUDE_CODE_REMOTE_SESSION_UUID` to both be set for the session and its`checkout` ,`post-session` and`spawn-runner` hooks
- Changed self-hosted runners at `--capacity 1` to refuse a session earlier when the path of a repository it pushes to is a symbolic link

**Added**

- Added a confirmation before `/install-github-app` changes a repository
- Added `claude mcp add-json <name> -` to read the server’s JSON from stdin, so tokens can stay off the command line; not available inside a Claude Code session
- Added a line when a resumed session’s earlier messages can’t be loaded
- Added a list in `/status` of broken agent, skill, command and output style files, keybinding errors, an unavailable sandbox, ignored permission settings and hook matchers that never match
- Added a note in `/config` when other settings override a row
- Added `x-claude-code-managed-settings-checksum` to the gateway hint headers; sent where managed settings set`CLAUDE_CODE_GATEWAY_HINT_HEADERS=1` , including where they already do
- Added support for naming a session’s billing group with the `attributionProject` setting, for Claude apps gateways that set`admin.billing_group_tag`
- Added `/tui classic` as the name for the non-fullscreen renderer;`/tui default` still works
- [VSCode] Added “Claude Code: Add to Chat” to the Explorer’s right-click menu, which @-mentions the selected files and folders in the chat

**Changed**

- Changed some pre-started background sessions to be discarded after an account change
- Changed a workflow paused at a usage limit in the terminal to stay paused after a login, until the earlier login returns or you type a prompt
- Bug fixes and reliability improvements
- Changed `ExitWorktree` to ask before removing a worktree that may hold unsaved work; refused in don’t-ask mode or with nobody to answer, unless an allow rule, auto or bypass permissions mode allows it
- Changed `claude mcp add --scope project` to stop, instead of overwriting the file, when`.mcp.json` lists its servers at the top level with no`mcpServers` key
- Changed the arrow keys on most permission prompts to stop at No instead of wrapping around; a number past the last answer moves to No
- Changed path checks to refuse some symbolic links
- Changed what counts as inside a working directory, so some paths now ask for permission
- Changed `permissions.blockReadsOutsideWorkingDirectories` to ask about more Bash commands in bypass and auto modes, and to deny some in auto mode
- Changed `permissions.blockReadsOutsideWorkingDirectories` to refuse more Bash commands that name a path under a Read or Edit deny rule, where it used to ask
- Changed auto mode’s notice about falling back to billed classifier requests to say that an upcoming release removes the fallback; each action it would have checked will be asked about or denied
- Changed auto mode to ignore more allow rules that run any package, like `Bash(pnpm dlx *)` , and path or version forms of ones it already ignored (`/usr/bin/npm run:*` ); the safety check decides those
- Changed `/sandbox` and most`/config` value lists to be unnumbered outside screen reader mode; use the arrow keys and Enter
- Changed Bash commands whose variables expand to very long text to be denied, with a message that says to use shorter values or a file
- Changed the PowerShell tool to ask before one more command, even under a rule naming it; if nothing answers, it’s refused unless a whole-tool rule, auto mode or bypass permissions mode allows it
- Changed a subagent resumed by a sibling’s message to follow the session’s permission settings; it may now ask, or be refused where nobody can answer
- Changed `--worktree` with a relative`--add-dir` beside the launch folder to give the Bash sandbox that folder, not one beside the worktree
- Changed some `--worktree` starts with a relative`--add-dir` above the launch folder, such as`..` , to give the Bash sandbox, and a`--bg` session, only the worktree
- Changed Claude in Chrome to refuse uploading some credential files; attach those to the page yourself
- Changed artifact approval in organizations that require it for each new artifact: an artifact that only a plugin created or allowed can be refused
- Changed sessions with nobody to ask to refuse a new artifact at once, with no permission prompt, in organizations that require approval of each one
- Changed the Artifact tool to refuse calls once settings turn Artifacts off, in sessions already running too
- Changed the Artifact publish prompt to stop offering “don’t ask again” in subagents and after a background task result, someone else’s message or a compaction
- Changed `claude plugin eval` to keep its deny rules over a plugin’s permission hook, and to flag calls the hook let through
- Changed MCP server logs to record the mode, action, and field names of elicitation requests and hook answers, not their full contents
- Changed most prompts to ignore answers other than Esc for a moment after resuming with `fg`
- Changed Code Mode to no longer refuse to start when an installed mod hooks tool calls; an administrator can still keep Code Mode off in managed settings
- Changed subagents launched without a model to stay on the model they started on when resumed, including after `/model`
- Changed the first subagent on a model to wait up to 1.5 seconds (10 in `claude -p` ) for that model’s settings from Anthropic, instead of starting without them
- Changed restarted background subagents, and turns that start by themselves, to wait for plugins to load
- Changed prompt history to leave out most launch prompts of sessions that another Claude Code session started, such as `claude --bg` run by Claude
- Changed subagent progress summaries in terminal sessions on Amazon Bedrock, Google Vertex AI and Microsoft Foundry to be written from short notes, not the whole conversation, using far fewer tokens
- Changed non-interactive (`-p` ) mode to run fewer git commands before the first request
- Changed first-run setup to skip terminal setup in remote VS Code, Cursor and Devin Desktop sessions, where it cannot change settings
- Changed fullscreen text selection to leave out the glyph of system, thinking and inbound message lines
- Changed Anthropic-hosted cloud sessions to ignore more of a repository’s flag and plugin delivery settings; its `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` now acts at most as`DISABLE_TELEMETRY`
- [VSCode] Changed a chat edit’s diff to stay open and keep asking for approval when a terminal session in the same window sends a prompt
- [VSCode] Removed unused tools from the IDE connection (openFile, saveDocument, getOpenEditors and related)

**Added**

- Added support for turning on Code Mode while agent teams are enabled; if you already turned it on, sessions with teams on now start in Code Mode
- Added a startup warning when an `--allowedTools` ,`--disallowedTools` or`--tools` value has an entry that doesn’t look like a complete rule
- Added a message explaining why MCP servers stop loading when an administrator adds `managed-mcp.json` during a session
- Added a row to `/status` that says when your provider or an environment variable such as`DISABLE_TELEMETRY` turns features off
- Added a note in `/plugin` and`claude plugin list` when a malformed`plugin.json` field such as`author` or`homepage` is ignored
- Added Ctrl+O to show hidden startup notices
- Added a hint in the empty prompt that says what to do after a request fails
- Added an `esc to stop` footer hint in a subagent’s view where Esc would stop it
- Added `OTEL_RESOURCE_INCLUDE_HOST` to include`host.name` and`host.id` in OpenTelemetry resource attributes (off by default)
- Added `overflowX="scroll"` on a mod’s`Box` , and`maxLines` and`wrap="clip"` on its`Text`
- [VSCode] Added a `claudeCode.promptSuggestions` setting that turns prompt suggestions off

**Changed**

- Changed edits in a session’s mods folder to ask more often, and sandboxed commands to write to fewer folders; if every edit asks, remove any link there to an unreadable or network path
- Changed terminal and VS Code sessions in organizations with the Claude Code + Cowork HIPAA configuration to start in auto mode by default; `disableAutoMode` turns it off
- Changed terminal and VS Code sessions with no mode set to start in auto mode, not Manual mode, when your organization’s policy hasn’t loaded
- Changed a `managed-mcp.json` added mid-session so it may apply at the next check; restart open sessions for it to apply in full
- Changed Code Mode’s `$` shell helper to refuse some values with an error; nothing runs
- Changed auto mode to stop some shell commands that ran before
- Changed some Bash commands on macOS that ran before to ask or be refused
- Changed a hook’s `"decision": "block"` to hold in more cases, so a hook that always prints one may start blocking calls
- Changed sessions that start in auto by default to take a server-managed `defaultMode` once settings load, or Manual if settings fail with no stored copy and policy fails or requires it
- Changed auto mode for organizations with the Claude Code + Cowork HIPAA configuration: on a direct Anthropic API connection, the server-side classifier now checks actions, as for other organizations
- Changed the Bash sandbox to keep its state in some cases during a session, until you run `/sandbox` or`/cd` ; Bash may ask first
- Changed sandboxed commands on macOS, Linux and WSL to no longer write to some sensitive home folders by default; some can be allowed with `sandbox.filesystem.allowWrite` in user or managed settings
- Changed the Bash sandbox to set aside unexpected entries in a repository’s `.git` folder after a command
- Changed the Bash sandbox on Linux and WSL to create some missing `--add-dir` folders before each sandboxed command, so commands that create them can fail, skip, or land inside them
- Changed commands that a permission hook or SDK host rewrites while approving to stay in the sandbox if the original was checked as sandboxed; the approver can add `dangerouslyDisableSandbox: true`
- Changed `claude mcp serve` to ignore`sandbox.excludedCommands`
- Changed `/rewind` on macOS, Linux and WSL to leave some files untouched and report them as skipped when the sandbox is on
- Changed the Bash sandbox to ask before some lines that set environment variables when auto-allow is off; unattended runs can refuse them
- Changed Claude’s file tools to refuse writes to Claude Code’s sign-in files, such as `.credentials.json`
- Changed the Bash permission check to ask before more commands reach Claude Code’s sign-in files
- Changed some file tools to refuse some device and process files
- Changed some `cp` and`mv` commands to ask first
- Changed some tool calls in bypass permissions mode to ask for approval, or be denied where no one can be asked
- Changed the Bash permission check to ask about some commands with very long values it can’t finish checking, or to refuse them where a deny rule may apply or no one can answer
- Changed subagents isolated in a worktree to refuse, in any repository, some git commands in Bash that can rewrite branches shared with your checkout; sessions are not affected
- Changed `ExitWorktree` to refuse removing a worktree while a background subagent or shell command may still be running in it, unless Claude passes`discard_changes`
- Changed the computer use macOS permissions panel to ignore j and k; the arrow keys and Ctrl+P/Ctrl+N still move the selection
- Changed the PowerShell tool to ask before some more commands; unattended runs refuse them unless an allow rule, auto mode or bypass permissions mode allows them
- Changed the PowerShell tool to ask about some more commands, even in bypass permissions mode, and to refuse some of them
- Changed some malformed PowerShell commands to be denied
- Changed some PowerShell tool commands that define a function named like a built-in command to be refused, so Claude can rename it
- Changed auto mode to ignore some wildcard shell allow rules, such as ones that name no program or that start with `$HOME` , so the safety check decides those commands
- Changed auto mode to judge wildcard allow rules whose program is a Windows backslash path, so those calls can be blocked; exact rules keep their allow; we plan to restore rules that start with `.\`
- Changed a managed `autoMode.environment` list without`"$defaults"` to replace every built-in entry in Claude Desktop and Agent SDK apps too; add`"$defaults"` or copy the sensitive-target ones
- Changed `--add-dir` and`/add-dir` in local terminal sessions to show the trust prompt for a folder you have not trusted yet;`claude --bg` refuses such a folder
- Changed self-hosted runners under `--trust-workspace false` to ignore more repository settings; set such values in user or managed settings
- Changed how permission answers over Remote Control and on self-hosted runners save “always allow” choices
- Changed enabled plugins that were refused for a malformed `plugin.json` display field: they now load, with their hooks and MCP servers; administrators should check the plugins they deploy
- Changed how `allowedHttpHookUrls` and`httpHookAllowedEnvVars` apply to hooks from managed settings
- Changed HTTP hooks to ignore `Content-Length` and`Transfer-Encoding` in their`headers` setting; the request’s length comes from its body
- Changed plugin uninstall to also delete the files extracted from the plugin’s MCP bundles (`.mcpb` ,`.dxt` ), unless another plugin uses the same bundle
- Changed some worktree sessions to skip some plugin settings; a notice names what was skipped
- Changed `/plugin` uninstall, remove and switch questions to lists answered with the arrow keys and Enter, or Esc to cancel;`y` ,`n` and`u` no longer answer them
- Changed automatic compaction in plan mode to use the model answering you, such as Opus with `opusplan` , so it can cost more
- Changed Cowork cloud sessions to hold an organization’s OpenTelemetry export until the first turn; a session that gets no message now sends nothing, so `claude_code.session.count` can fall
- Changed Claude’s messages to a session on another machine to be refused when your session uses an API key, auth token, profile or workload identity beside a saved claude.ai login
- Changed `--bare` sessions on an unapproved API key: moving one to the background is now refused in most setups, and`claude --bare --bg` exits with an error if the background service lacks the key
- Changed “Continue automatically at usage limit” to stop with a notice, sending nothing, when your login changes or ends during the wait
- Remote Control: Changed how it connects through a proxy: requests now use a CONNECT tunnel, so a proxy that refuses CONNECT stops Remote Control with an error
- Remote Control: Changed the session debug logs written under `--verbose` to go to`~/.claude/child-logs/`
- Self-hosted runner: Changed Linux and macOS sessions with no `gh` to get a built-in`gh api` by default, via Anthropic, which adds their GitHub sign-in, if any;`--use-anthropic-gh false` turns it off
- Changed agent view so that quitting with Ctrl+C twice, Ctrl+D twice or `/exit` after pressing`←` in an empty conversation ends that session, instead of leaving it running for 5 minutes
- Changed `--await-initialize` to be refused with`--bg` or`--worktree --tmux` , which never read the initialize request, instead of starting without the fields it carries
- Changed worktree creation, `/teleport` and IDE checkouts to run git with a stricter`PATH` ; a git filter or helper may need its full path
- Changed the card for approving a subagent’s plan to two choices, “Yes, and switch to default (ask each time) for this session” and “No”, chosen with the arrow keys and Enter (typed text moves to No)
- Changed the `/claude-api` skill to build agents on Managed Agents by default, unless you want to host the agent loop yourself
- Changed `/usage` in fullscreen mode to show plan limit bars before session cost for subscribers;`/cost` still starts with cost
- Changed the wording of some messages that `claude` subcommands print; JSON output is unchanged
- Changed first-run setup to preselect the “Auto (match terminal)” theme instead of Dark mode
- Changed fullscreen `/cd` to refuse “Yes” while its trust prompt doesn’t fit, with a line saying to make the window taller
- Removed the tmux notice that said the mouse wheel needs `set -g mouse on` in fullscreen; the wheel scrolls without it
- Removed the spinner tip that suggested `/clear` 30 minutes into a running turn
- [VSCode] Changed a running command’s live output in the chat to look like the command card’s OUT row
- [VSCode] Changed the text of extension errors in event listeners, views and activation to stay out of the editor’s own error telemetry

**Added**

- Added a spinner tip in VS Code and similar integrated terminals about re-running a notebook cell, shown once Claude has read or edited a Jupyter notebook
- Added a line under the error shown when your organization’s policy blocks a request, saying how to continue: run `/rewind` and change the message, or run`/clear`
- Added a note for Claude when a sandboxed `git checkout` ,`git stash` or autostash pull cannot update protected files such as those in`.claude`
- [VSCode] Added prompt suggestions wherever the terminal shows them: Tab accepts the suggested next prompt; `promptSuggestionEnabled: false` in Claude Code’s settings.json turns them off
- [VSCode] Added a Console sign-in that creates no API key
- [VSCode] Added a field to paste the redirect URL when signing in to an MCP server from the panel in SSH, WSL and container windows

**Changed**

- Changed sandboxed Bash commands to follow the normal permission flow by default; set `sandbox.autoAllowBashIfSandboxed: true` to keep running them without prompts
- Changed managed settings that turn the sandbox on: to keep commands running without prompts, set `sandbox.autoAllowBashIfSandboxed: true` in your highest-ranking managed source
- Changed read-only git commands to ask first in a repository that isn’t the project’s or one you trusted; name it with `--add-dir` , not the`additionalDirectories` setting, to run them without prompts
- Changed scheduled tasks and `/loop` to ask before running a skill that declares`allowed-tools` or hooks; add`Skill(name)` to`permissions.allow` to run it unattended
- Changed “Always allow” in Desktop, IDE and SDK hosts: a rule alone is offered only when saving it would allow that command again, so a scheduled task that relied on one can now wait for you
- Changed the sandbox to refuse sandboxed writes to `.claude` settings, skills, agents, commands and hooks in added directories, as it does in the launch folder
- Changed worktree isolation: an isolated worktree session or agent is now refused `git bisect run` ,`git maintenance register|start` and`scalar` ’s set-up commands, and is told what still works
- Changed plugin MCP bundles (`.mcpb` ,`.dxt` ) to unpack under the plugins directory, not`.mcpb-cache` in the plugin’s folder, so read-only plugin folders load; one fetched by URL downloads again once
- Changed plugins of a remote marketplace that managed settings declare, enable and list in `prependPlugins` or`appendPlugins` to count as the organization’s;`pluginDigests` is deprecated and ignored
- Changed plugin loading to skip, and `claude plugin validate` to report, some unsafe paths and links in a plugin’s folder
- Changed nested skill loading: `.claude/skills` folders in`--plugin-dir` plugins no longer load from a session started above them; start Claude Code in the plugin folder instead
- Changed EnterWorktree to refuse a `WorktreeCreate` hook path that goes through a symlink inside the project; have the hook print the real path
- Changed tmux and iTerm2 teammates, and new sessions from the agents view opened with ←, to start in the session’s folder instead of where the shell last ran `cd`
- Changed prompt suggestions in the terminal to refresh less often after 8 turns in a row started by background or scheduled tasks, saving usage until you press a key or return to the terminal
- Changed Esc on a background MCP task’s question to “answer later”: nothing is sent to the server, and the question stays in `/tasks`
- Changed `CLAUDE_CODE_MAX_CONTEXT_TOKENS` to also set the context window of unrecognized model IDs that start with`claude-` , such as a gateway’s`claude-sonnet-latest`
- Changed `/login` to ask for permission to change your privacy settings
- Changed `/cost` ,`$.session.usage()` ,`--max-budget-usd` and the cost and token metrics to count a mod’s`$.model.complete` and`$.model.classify` calls
- Changed the programs that Claude Code’s sandbox starts for its own use, such as its network bridge, to run without Claude Code’s sign-in tokens and API keys in their environment
- Changed ripgrep, local git commands and the sandbox runtime’s own helper programs to run without the mTLS client key’s passphrase in their environment
- Remote Control: Changed `/resume` and`/teleport` to disconnect first when the connected session has a share link, or when that can’t be checked; run`/remote-control` to connect again
- Changed Claude Code on the web, cloud scheduled tasks and Cowork cloud sessions to start despite `sandbox.failIfUnavailable` : the hosted container has no command sandbox
- Changed requests for settings, permission rules, hooks or status over a cloud or self-hosted session’s connection to return less or be refused; what applies in the session is unchanged
- Changed `claude -p` with no cached account data to wait for that data before its first request, so a first run in a clean environment can start a few seconds later
- Changed Claude’s instructions to keep files you’ll want out of its temporary scratchpad folder, which can be cleared
- Changed marketplace `ref` to accept the full ID of any commit on a branch or tag; clients before this version can’t clone such a pin, so a marketplace pinned this way stops loading there
- Changed plugin marketplace and plugin downloads to use a CONNECT tunnel when `ALL_PROXY` is the only proxy set; a proxy that refuses CONNECT now fails them
- Changed `-p` and SDK runs in a worktree or submodule mounted into a container to apply`.claude/settings.local.json` allow rules only once the folder is trusted
- Changed self-hosted runners to refuse a session whose session folder or repository path is a symbolic link, other than a `checkout` hook’s checkout
- Removed “ask your admin to raise it” from spend limit and shared budget errors in cloud sessions and under Remote Control
- [VSCode] Changed the install link’s marketplace confirmation button to “Trust marketplace and continue”

**Added**

- Added Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools; see https://code.claude.com/docs/en/code-mode to turn it on, or to turn it off for an organization
- Added `engines["claude-code"]` to`plugin.json` : plugins outside their stated Claude Code version range are not installed, updated or loaded, and install and update pick the newest version that fits
- Added type checking of a plugin’s TypeScript hooks module to `claude plugin validate` ; each finding is a warning, and`--strict` fails on it
- Added to `claude plugin validate` the`$` nouns a mod’s`engine.create` hook adds, withholds or replaces
- Added support for `CLAUDE_CODE_AUTO_MODE_MODEL` in managed settings for Claude Desktop sessions on third-party providers (takes effect once Claude Desktop includes this version)
- Added an options menu (stop, switch to usage credits, upgrade) when a model needs usage credits that are turned off or used up
- Added a progress heartbeat every 30 seconds while hooks run long in `claude -p` and SDK sessions, so a host such as Claude Desktop won’t end the turn as hung (once it includes this version)
- Added `.docx` support to Artifact tool asset uploads
- Added a `/diff to close` hint to the diff panel, and the keys that focus and close a panel to the`?` shortcut guide
- Added a start-up warning for Bash allow rules with a `*` in a variable’s value, such as`Bash(PORT=* npm run dev)`
- Added a warning when a plugin’s language server can’t start, naming the server and what to check
- Added a one-time startup notice naming `CLAUDE_CODE_PERFORCE_MODE` when Perforce settings are found and the mode is off
- Added `CLAUDE_CODE_DISABLE_AGENT_PROGRESS_SUMMARIES` (available since 2.1.318): set it to`1` to turn off subagent progress summaries and the requests that write them (anthropics/claude-code#86879)
- Added `CLAUDE_CODE_RESUME_HINT_COMMAND` , which a program that starts Claude Code can set to have the exit hint name its own resume command
- Added `unsetOptionCount` to the`claude plugin install --json` result: how many`userConfig` options are not yet set
- Added `isPromptCovered` to the props a mod’s pane is drawn with, telling it when a dialog or a field above the prompt covers the prompt
- [VSCode] Added a Claude Code trust prompt before the first session in a folder you haven’t trusted; declining leaves the folder’s settings and MCP servers unused
- [VSCode] Added the live output of a running Bash or PowerShell command to the chat, under the command’s card
- [Claude Code on the web] Added a failure notification when a scheduled task’s run can’t start because its settings require a sandbox that isn’t available where it runs
- [Claude Code on the web] Added the No repository option to the repository picker for new sessions on a self-hosted environment
- [Claude Code on the web] Added a warning to a cloud session’s Delete dialog when deleting it will pause scheduled tasks that run in that session
- [Remote Control] Added a Try again button to the error shown when the computer running your session is signed out of Claude Code
- [Claude Tag] Added Claude Tag sessions started in Slack to the Compliance API on Enterprise plans, in beta: list the sessions and read their transcripts
- [Claude Tag] Added a confirmation step when an admin turns off Claude Tag for the organization, with a dialog that says what stops
- [Claude Tag] Added a Retry button to a Slack channel’s Configure page for when its settings fail to load
- [Claude Tag] Added a Scope filter to the Network events tab of Claude Tag’s Activity page, so admins can download one scope’s events instead of the whole organization’s
- [Code Review] Added a Download CSV button to Code Review admin settings: export review costs per repository by month, week or day, for one repository, one GitHub organization or all

**Changed**

- Self-hosted runner: Reverted the exit code of an on-demand runner whose work order was superseded to 1; 2.1.321 and 2.1.322 had changed it to 0
- Changed `/security-review` to ask before Claude runs it, and skills’ inline git commands to stop in bare-repository worktrees and similar folders until you allow them, outside auto mode and bypass
- Changed how many messages each of your sessions on the same machine can send the others without a prompt from you, from about 20 to 100,000
- Changed the number of messages a Claude Desktop Code tab session can send to your other sessions before it waits for you to type again from 20 to 100,000
- Changed organization settings and policy requests to no longer use a proxy set only by `ALL_PROXY` ; set`HTTPS_PROXY` if your network needs a proxy for them
- Changed the context window assumed for a model ID that Claude Code doesn’t recognize from 200K to 1M; use `/autocompact` to compact earlier, or`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep 200K
- Changed sessions on other vendors’ models with a smaller window: once full, they can fail every turn until `/clear` , so set`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` before they fill
- Changed Claude Code’s own ripgrep and most internal git commands to run without most of its sign-in tokens and API keys, so a credential helper or hook under such a git command no longer finds them
- Changed `MCP_SDK_GENERATION=v1` to run the v2 MCP client in a compatibility mode (earlier handshake, relaxed OAuth sign-in checks), because the v1 MCP client is removed
- Changed the tool Claude uses to finish an MCP server sign-in so that it asks you to confirm, even in bypass and auto modes; where nobody can be asked, such as in `dontAsk` mode, use`/mcp`
- Self-hosted runner: Changed a session at a connector’s approval prompt or sign-in inside a running tool to be released as paused at the idle or lifetime limit; a late answer resumes from a fresh clone
- Changed artifact updates to ask first, even under an allow rule, when people outside your organization can see the artifact or Claude can’t check who can
- Changed `glab config set` without`--global` or`--host` , and`glab config edit --local` , to fail read-only inside the Bash sandbox; run them outside it
- Changed the Bash sandbox on macOS, Linux and WSL to ignore write access granted through a symlink in a writable folder that leads elsewhere or to a missing path; grant the real path instead
- Changed “don’t ask again” so an allow rule that looks like it holds a secret, or would be saved to a `settings.local.json` that git tracks, lasts for the session only and isn’t saved
- Changed the sandbox on macOS and Linux: `git worktree add` ,`move` and`remove` now fail read-only, as do commits in a worktree the session wasn’t started in or given, even one inside the project
- Changed start-up to stop when a `--settings` file isn’t valid JSON and has a`sandbox` key, even one that turns the sandbox off
- Changed what happens when a tool’s permission check fails with an error after a `PreToolUse` hook returned “allow”: Claude Code now decides the call as if no hook had answered
- Changed what happens when a tool’s permission check fails on input that a `PermissionRequest` hook rewrote: Claude Code now asks again, or refuses the call where nobody can answer
- Changed the GitLab merge request badge to show only for the repository a sandboxed session opened in
- Changed the `claude mcp add --help` example to a local command
- Changed mods: a `*` , namespace or negated hook on`engine.create` now only observes. To add or withhold a`$` noun, hook`engine.create` by name
- Changed `claude plugin validate` to fail a mod whose`engine.create` hook isn’t written in the one accepted form; installed mods keep loading
- Changed marketplace and plugin installs to fail when a git submodule isn’t on the commit its repository records
- Changed project (`.mcp.json` ) MCP server approvals to match the server’s exact name; an approval entry written in another spelling now asks again
- Changed self-hosted runners under `--trust-workspace false` and VS Code sessions in an untrusted folder: the repository’s settings can no longer disable your hooks or turn plugins on or off
- Changed `tool.list` plugin hooks so the tools they add or remove apply to the model and every agent
- Changed `claude plugin install` and`/plugin` to show a warning instead of success when a plugin’s install command writes to stderr
- Changed claude.ai chats and Cowork tasks to check a plugin file for secret keys before sending it, and to ask first if it may hold one
- Changed `claude rm` and the agents view to say a session is removed from the list, not deleted;`claude rm` help says the conversation is kept and`claude --resume` lists it
- Changed `/stickers` to say that the sticker pack is all claimed for now, instead of opening a page in the browser
- Changed a managed settings `policyHelper` to no longer take environment variables from user or`--settings` files; set what it needs in managed`env` or the launching shell
- Changed a `/skill` or`@file` typed between two files picked in claude.ai or the desktop app, or after a picked file and a link, to no longer take effect
- Changed split-pane agent teammates to check that requests come from their team lead: one that outlives its lead has its asks refused, and plan approvals must be re-sent after `/clear` or`/resume`
- Changed worktree creation to refuse checkouts whose `.git` folders are links or hold hard links, such as git-new-workdir layouts and`cp -al` copies; use a plain clone
- Changed what Claude is told after safeguards stop a forked skill or a workflow agent: retry with a narrower prompt or tell you, instead of doing the work itself
- Changed browser tool calls (Claude in Chrome, the desktop browser pane) to refuse some escaped or JSON-text inputs and tell Claude how to respell them
- Changed workflows so that a prompt an app marks as not typed no longer reaches their agents as your request
- Changed computer use on macOS to refuse key chords that hold Space with Control or Option, and more system shortcuts that need your grant
- Changed first-run setup in Terminal.app to name the “Use Option as Meta key” setting, what Option stops typing, and how to undo it before you agree
- Changed the maximum of `CLAUDE_CODE_OVERLOADED_RETRY_MAX_DELAY_MS` from 64000 to 300000
- Changed workflows in the desktop app’s Code tab to pause at a usage limit and continue after it resets
- Changed the spend limit and 1M context errors, and `/usage-credits` , in cloud sessions and under Remote Control: they no longer give a web address
- Changed which MCP servers are reported by name in OpenTelemetry exports to follow the current connector directory
- Changed `/logout` in a background session, which had no effect: it now signs out every session on the computer, deletes stored MCP logins and plugin secrets, and ends the session, as in a terminal
- macOS: Changed the Bash sandbox so `lsof` and similar tools may show less about processes a command did not start
- Changed cloud scheduled tasks: a `PostToolUse` hook from your own settings can no longer replace the plan in`ExitPlanMode` ’s result
- Changed `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` in Anthropic-hosted cloud sessions to stop applying to auto-memory extraction and, in some sessions, to`/btw` ; the main conversation still follows it
- Removed the `subagentStatusLine` setting: subagent rows always use the default look, and settings files that still hold the key load as before
- Deprecated a plugin’s `agent` setting: it still applies, and now warns at startup, in`claude plugin list` and in`claude plugin validate`
- [VSCode] Changed opening a Claude Code on the web session in a folder where Claude Code’s sandbox is on: the extension no longer switches to the session’s branch for you, and the dialog says why

- Added fast mode on Vertex in the global, us and eu regions, falling back to standard speed if Vertex refuses it

**Added**

- Added Claude Fable 5.2 (`claude-fable-5-2` ), now the default Fable model — 1M context, $8/$40 per Mtok with $0.20/Mtok cache reads
- Added `--settings <file>` to`claude remote-control` , which applies a settings file to every session it starts; if the file changes, no new session starts until you restart`claude remote-control`
- [VSCode] Added a warning above the prompt box when both an auth token and an API key are set, matching the terminal’s startup warning

**Changed**

- Updated the session-feedback message and the `allowedProviders` setting description to name Ultra alongside Pro and Max, and the`forceLoginMethod` description to cover every paid Claude plan
- Updated the bundled dataviz skill to suggest a list of meters, one row per item, for several ratios that each have their own limit
- Updated the “Upgrade to Max” options to say “Upgrade to Max or Ultra”
- Changed mods Claude writes to turn on without asking in auto and bypass modes, even with managed `allowUnsandboxedCommands: false` or`allowManagedDomainsOnly` , and not to load in`dontAsk` mode
- Changed mods’ own tool calls (`$.tool.call` ,`$.mcp.call` ) to raise no permission prompt, even under your own ask rules or in`dontAsk` and`-p` runs; deny rules and organization policy still apply
- Changed saved Fable 5.1 model picks to move to `fable` : on Pro and Max plans at once, on Team and Enterprise plans once your organization allows it; use`/model claude-fable-5-1` to switch back
- Changed the fast mode labels in `/fast` ,`/config` and`/model` to name your current model when it supports fast mode, and removed “(Opus only)” from the VS Code row
- Changed nested skill loading: `.claude/skills` folders inside`~/.claude/plugins` no longer load when Claude works on files there
- Self-hosted runner: Changed an on-demand runner that the server refuses because it is not needed (session ended or paused, work order superseded) to exit 0 instead of 1
- Windows: Changed how Claude Code finds programs it starts itself, such as git, to skip relative and UNC PATH entries and links onto a network share, and to pick up PATH changes from settings
- Changed OpenTelemetry hook events in non-interactive sessions to leave two built-in hooks out of `num_hooks` and`num_success` after Bash and PowerShell calls
- Changed the number of messages a Claude Desktop Code tab session can send to your other sessions before it waits for you to type again from 10 to 20
- Changed `AskUserQuestion` in`claude -p` and SDK sessions to deny a host’s permission answer that sets the undocumented`afkTimeoutMs` input field
- Changed managed `extraKnownMarketplaces` : a declared name can’t be added from another source, an entry from elsewhere is hidden, and plugins the declared marketplace lists are reinstalled from it
- Changed Claude in Chrome tools that an app running Claude Code provides, such as the Claude desktop app’s Code tab, to follow your organization’s Claude in Chrome policy
- Changed `policy_mode: layered` in the Claude apps gateway: managed MCP servers now add up over every matching policy,`match: {}` included, so a gateway already on it serves more after the upgrade
- Gateway: Changed Bedrock, Vertex and Foundry upstreams to ignore the `ANTHROPIC_*_BASE_URL` variables in the gateway’s environment. Set`base_url` on the upstream to use another endpoint
- Changed headless sessions to reload mods in `.claude/skills` when their files change; set`CLAUDE_CODE_PLUGIN_DIR_WATCH=0` to turn this off
- Changed the Cyber Verification Program sentence on a cyber safeguard block to use the same words as claude.ai
- Changed Cowork cloud sessions: a PostToolUse hook can no longer replace the answer to AskUserQuestion or the plan in ExitPlanMode’s result; Claude reads the originals
- Changed `cp` and`mv` spelled by a path, such as`/bin/cp` , to get the same path checks as`cp` and`mv`
- Changed background command notifications and TaskStop results to show Claude only the start and end of a command over 1,000 characters, which saves context
- Changed artifact design guidance to treat Inter and Space Grotesk as good default typefaces
- Changed the account email check to refuse only a shell command that holds your email and names a User-Agent, without asking, so other commands and scheduled runs no longer stop at its prompt
- Changed Remote Control sessions to keep the folder they were started in: a connected client can no longer move a session to another folder
- Changed managed `extraKnownMarketplaces` take-overs from an undeclared source: saved data is set aside for`cleanupPeriodDays` , and options and secrets until`claude plugin restore-set-aside`

**Added**

- Added support for MCP tasks in the terminal: when an MCP server runs a tool call as a task, Claude Code moves it to the background right away and tells Claude the result when it finishes
- Added the `CLAUDE_CODE_DISABLE_MCP_TASK_BACKGROUND` environment variable to make Claude Code wait for an MCP task inside its tool call instead of moving it to the background right away
- Added `permissions.denyInsteadOfPrompting` setting: a tool call that would wait on a permission prompt is denied and the turn continues, in terminal sessions, subagents and teammates
- Added `/claude-api latency-optimize` to find where your Claude API code makes people wait and try faster settings against your eval
- Added the `CLAUDE_CODE_DISABLE_FEATURE_FLAGS` environment variable, which stops feature-flag fetching without turning off telemetry; every flag then uses its built-in default
- Added spend for today, this week and this month to `/usage` for developers behind a Claude apps gateway who have no spend limit, when the gateway runs this version or later
- Added `admin.pooled_conflict_rule` and`admin.pooled_priority_order` to the Claude apps gateway, to set which pooled group budget pays first for a developer in several groups
- Added an optional `login_notice` setting to the Claude apps gateway that shows an operator-defined message on the gateway’s browser verification page and on Claude Code’s sign-in screen
- Added `CLAUDE_GATEWAY_LOG_FORMAT=json` to the Claude apps gateway: operational log lines become JSON with`severity` ,`time` and`message` , for log collectors that read the level from a field
- Added a Claude apps gateway boot warning for a case where another policy’s `availableModels` also allows a custom model id that one policy lists by name
- Added start-up warnings to the Claude apps gateway for policies with no `desktopHome` that rely on`coworkTabEnabled: false` or`chatSessionRetentionDays` , ahead of Claude Desktop’s November 10 change
- Added a warning when your claude.ai login ends and Claude Code continues on the sign-in from `ant auth login` , which bills usage to your Claude Console organization
- Added a line at startup naming the Claude Console organization that is billed in a case where Claude Code uses the sign-in from `ant auth login`
- Added an “Earlier in this session” row to `/tasks` that lists finished subagents, so you can reopen one and message it even after it leaves the list or you restart
- Added a note to `/loop` confirmations in cloud sessions that a timer stops with its container, with an offer of a loop that survives
- Gateway: Added startup warnings for upstreams that mix AWS GovCloud with other upstreams, and for base URL variables in the gateway’s environment
- Gateway: Added a start-up log warning when managed settings name an Amazon Bedrock guardrail in a header, which the gateway doesn’t forward, and no `bedrock` upstream sets a`guardrail`
- [Claude Tag] Added a Claude Tag row to the Models admin page for Enterprise orgs with Claude Tag connected, linking to where its model defaults are managed

**Changed**

- Changed auto mode with telemetry or feature flags off, including behind LLM gateways: calls the server can’t check are asked about or denied, with no fallback to the built-in classifier; set `CLAUDE_CODE_AUTO_MODE_SERVER=0` to keep using it (Bedrock, Vertex and Foundry are unchanged)
- Changed auto mode behind an LLM gateway: `CLAUDE_CODE_AUTO_MODE_SERVER=1` turns on the server-side check even with`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` set; if managed settings set the latter, they must set the`1` too
- Changed MCP tasks to work in the terminal without feature-flag fetching (Bedrock, Vertex, Foundry, LLM gateways, `DISABLE_TELEMETRY` ); set`CLAUDE_CODE_DISABLE_MCP_TASKS=1` to turn them off
- Changed auto mode so that a project’s settings files or a plugin can no longer switch its server-side check back on once you have switched it off outside the project
- Changed Monitor’s WebSocket watch to follow a rule on every Bash command: a deny refuses it, and under an ask rule an allow on Monitor, bypass mode or auto mode no longer skips the prompt
- Changed `CLAUDE_CODE_PROXY_RESOLVES_HOSTS` so that settings files can no longer set it where the host manages the network; set it in the process environment
- Changed a session’s exact Bash or PowerShell ask rule to be refused, not asked about, when a broader deny rule covers the command
- Changed the Bash tool to ask for permission before running `which` with flags outside a lookup/display set
- Changed `--add-dir` directories to register plugin marketplaces only once that directory’s own trust dialog has been accepted
- Changed nested skill loading: with the Bash sandbox on, skills in `.claude/skills` folders below the starting folder load only if you start Claude Code there or`/cd` there
- Changed self-paced `/loop` to end when a turn finishes without rescheduling, instead of scheduling a fallback wakeup; set`CLAUDE_CODE_LOOP_KEEPALIVE=1` to keep the old behavior
- Changed PostToolUse hooks to run, with up to 10 seconds to finish, on tool calls that finish after a new message interrupts the turn
- Changed what Claude is told after safeguards stop a subagent: retry with a narrower prompt or tell you, instead of doing the work itself
- Changed cloud sessions to drop session-scoped allow rules at a move to a new worker when their deny and ask rules can’t all be kept, so you may be asked again
- Changed the automatic certificate repair behind a TLS-inspecting proxy: it no longer runs when `ALL_PROXY` is the only proxy variable set. Set`HTTPS_PROXY` to the proxy’s http:// URL instead
- Changed artifact database saves of rows others may have written: each file gets a hidden marker file, and existing files without one, earlier versions’ exports among them, are left alone and reported
- Updated `/claude-api managed-agents-onboard` templates to match the Claude Console: removed Performance tuner, renamed two, and turned off web search and fetch in five whose prompts don’t use them
- Removed `shortcuts_list` and`shortcuts_execute` , which listed and started shortcuts saved in the extension, from Claude Code’s built-in Claude in Chrome tools
- Self-hosted runner: Changed `CLAUDE_RUNNER_CHECKOUT_PATH` so that each repository in a session gets its own folder: a folder name already taken in the session gets`-2` ,`-3` , … at the end
- Self-hosted runner: Removed the end-of-session upload of a session’s debug log; a failed session’s log stays on the runner host unless `--remove-session-state` is set
- [VSCode] Changed links in Claude’s replies that point at Claude sign-in, connector or API addresses to show as text with the address, instead of opening in the browser
- [Code Review] Changed unlinking a GitHub organization to also turn off its All repositories setting in Code Review; it stays off after you relink until you turn it back on

**Added**

- Added a record of your answer when Claude Code asks to turn on the mods it writes in a session
- Added `assume_role` on`mantle` upstreams of the Claude apps gateway, for a role in another AWS account and for per-developer AWS cost attribution
- Added an `effort` option to a mod’s`$.model.fork` , so a quick side job can run at`low` whatever effort the session uses; the result reports the effort sent
- Added the date a stored claude.ai sign-in ends to `claude auth status` (`loginExpiresAt` ) and to`claude -p` JSON results (`credential_expires_at` )
- Added a one-line stderr warning to `claude -p` text output, starting 10 days before a stored claude.ai sign-in ends
- Gateway: Added a log warning when the Google access token from a federation (`external_account` ) credentials file is about to expire
- Self-hosted runner: Added `CLAUDE_RUNNER_SPAWN_REASON` to the orchestrator’s spawn-runner hook environment: why Anthropic sent the request (e.g.`runner_lost` ,`resumed` ); unset when none was sent
- Self-hosted runner: Added a startup warning when `--release-idle-session-min` is the last argument with no value (still ignored) or has a value of only spaces (still read as`0` )
- [Cloud sessions] Added a No repository option: a cloud session on an Anthropic-hosted environment can now start without picking a repository
- [Cloud sessions] Added the usual Chrome names to cloud sessions, so tools other than Playwright can find the built-in Chromium through google-chrome and CHROME_PATH
- [Cloud sessions] Added a New session button to the card shown when a cloud session’s environment was deleted, where the message box is hidden

**Changed**

- Changed Claude Code to no longer install or update the IDE extension when started in an IDE terminal; use `/ide` from an outside terminal or your editor’s extensions view
- Changed subagents to stop instead of switching models when “Switch models when a message is flagged” is set to “Ask each time”
- Changed background subagents to move a shell command to the background after 3 minutes on all providers, so the prompt cache doesn’t expire while it runs
- Changed subagent progress summaries on Bedrock, Vertex, Foundry, or with telemetry off: on a cold prompt cache, one can now wait up to 5 minutes for the subagent’s pending response
- Changed what a mod attaches at `prompt.submit` to stay in the user’s turn rather than go out as a system message
- Changed `deniedMcpServers` URL entries to also block Anthropic-provided servers whose upstream URL matches, and connectors in cloud sessions and on self-hosted runners, where the session is given the connector’s own URL; otherwise deny a connector by name
- Changed the `/permissions` rule details, built-in rules and delete confirmation views to use the standard dialog; pressing Ctrl+C twice there now goes back or cancels instead of quitting
- Changed `/claude-api managed-agents-onboard` to give a new agent’s built-in tools, and a quickstart template’s MCP tools, the`auto` permission policy (“Judge risky actions”) by default, matching the Claude Console; say “run without asking” to opt out
- Removed the 2.1.288 fix that kept the model’s earlier thinking when resuming a conversation started on 2.1.286 or earlier; that thinking can now be dropped
- Self-hosted runner: Changed the orchestrator’s log lines: `ok session=…` now prints at every log level on stdout (was debug only, stderr) with`reason=…` after`jti=…` , and`nack session=…` ends its fields with`reason=…` after`retryable=…` ; update scripts that match`retryable=true:` or`retryable=false:`
- [Cloud sessions] Changed connector search results to tell Claude that the search covers only part of the connector directory

**Added**

- Added a warning to `/schedule` when a new cloud scheduled task gets connectors you did not ask for, with how to remove them
- Added a check that asks before a web or shell tool call sends your account email address to the network
- Added `onFailure: "block"` for Agent SDK hook callbacks, so a callback that fails blocks the action instead of letting it run
- Added a notice in Claude Desktop’s Code tab when your organization’s remote managed settings haven’t loaded for the session, and another once they do
- Added a notice when auto mode’s check has no answer for a subagent’s tool call, which used to leave nothing in the main view
- Added `anthropic.plan` ,`anthropic.seat_type` ,`anthropic.provider` and`anthropic.custom_base_url` to OTel metrics, events and spans
- Added a cost report to the Claude apps gateway admin API, `GET /v1/organizations/analytics/cost` , for reading past spend over a date range in total, per developer, per day or per IdP group
- Added a daily record of each developer’s IdP groups on Claude apps gateways with `admin:` set, kept for`admin.identity_retention_days` , which the cost report groups by
- Added `CLAUDE_CODE_DISABLE_NESTED_SESSION_ID` : set it to`1` on a self-hosted runner to have`claude -p` run inside a session use that session’s ID and transcript, as in earlier versions
- [Cloud sessions] Added a link card to cloud sessions, so you can open or decline a link an MCP server asks you to visit instead of the request going unseen

**Changed**

- Changed SendMessage so a message to a running background agent arrives earlier: its running command moves to the background, without being stopped
- Changed subagents and teammates on a different model from the main conversation to use that model’s saved effort level or its default, instead of the session’s effort level
- Changed exit to skip the “Background work is running” prompt for plugin monitors
- Changed the built-in `plugin-authoring` skill to offer once, after a mod you built works, to help share the mod
- Removed the Read tool’s “file unchanged since your last Read” stub: reading an unchanged file again returns its content
- [Cloud sessions] Changed the organization environment dialog in admin settings to the personal dialogs’ short layout: each section opens from a row, and unsaved input asks before closing
- [Claude Tag] Changed newly pasted artifact links in Slack to show a simpler card with an Open button that opens the artifact in Claude, instead of Slack’s side panel

**Added**

- Added `CLAUDE_CODE_DISABLE_ULTRATHINK` , which turns the`ultrathink` keyword off
- Added pooled group budgets to the Claude apps gateway: one monthly budget that a group’s members share, on top of each member’s own limit, set with `pooled_budget` on the spend limits admin API
- Added `CLAUDE_CODE_DISABLE_LOOPED_CROSS_CERT_DROP` to keep system cross-certificates that certify each other in a loop, for servers whose private CA needs one
- Added `CLAUDE_CODE_SUBAGENT_EFFORT` to set the effort level subagents run at when nothing else names one, instead of inheriting the session’s
- Added `managed.policy_mode: layered` to the Claude apps gateway: every policy that matches a developer applies, each merged over the ones listed below it, not only the first
- Added `guardrail.input_tagging` on Claude apps gateway Bedrock upstreams, which wraps prompts in the input tags that an Amazon Bedrock guardrail’s prompt attacks filter needs;`all_messages_and_system` also tags the system prompt
- [VSCode] Added sign-in to a Claude apps gateway from the chat panel, with no terminal needed
- [Cloud sessions] Added a form card to cloud sessions, so you can fill in or decline a form an MCP server asks for instead of the request being canceled without you seeing it
- [Cloud sessions] Added a “Catching up…” line that shows while a cloud or Remote Control session you switch back to is still bringing its messages up to date
- [Code Review] Added a findings-by-severity card to the Code Review analytics page, showing the period’s total findings, a daily chart stacked by severity, and each severity’s count

**Changed**

- Deprecated fast mode for Opus 4.8 as of October 30, 2026; it will be removed on November 30, 2026. After removal, fast mode requests on Opus 4.8 return an error and do not fall back to standard Opus 4.8. Migrate to Opus 5.5 to keep the speedup
- Changed auto mode so that an `autoMode.allow` list without`"$defaults"` no longer removes the built-in rule on scheduled-task prompts
- Changed server-managed settings for Team and Enterprise logins: settings fetched from Anthropic’s API now apply without an approval prompt, and a stored copy that doesn’t verify is refused
- [VSCode] Changed `ultrathink` to run its turn at`xhigh` effort and show in a rainbow in the chat box, as in the terminal
- [Cloud sessions] Changed network secrets so Enterprise members can add and delete them on their own cloud environments, instead of only viewing them
- [Claude Tag] Changed a Slack channel’s Respond automatically switch to a “How Claude participates” choice on the channel’s Configure page and in Claude Tag admin settings

**Added**

- Added `permissions.blockWritesOutsideWorkingDirectories` : Write, Edit and NotebookEdit refuse paths outside the working directories in every permission mode
- Added an optional `groups` list to Claude apps gateway upstreams: only developers in the listed IdP groups are sent to that upstream, on failover too
- Added `guardrail_trace` to Claude apps gateway Bedrock upstreams, to log which Amazon Bedrock guardrail filter fired on a request, without the matched text
- Added idle compaction to VS Code and Claude Desktop’s Code tab: a 200K+ token conversation left idle is summarized, and the summary is used on your next message if the prompt cache has expired
- [Cloud sessions] Added an Include completed button to the Scheduled tasks page when finished one-off tasks are the only ones hidden, so you can show them without opening the Sort menu
- [Claude Tag] Added a dismissible link to the Claude Tag API docs on the Bundles and Channels tabs in admin settings, for Enterprise organizations with Slack connected

**Changed**

- Updated the `/claude-api` skill: its Datadog setup for API tracing now sends the`dd-llmobs-enabled: true` header, as the Console does, and its`ant` install instructions drop the outdated macOS`xattr` step
- Updated the bundled dataviz skill to advise printing y-axis tick labels in full under 10,000, and horizontal ones in full under a million where they fit
- Updated the bundled dataviz skill to answer donut and pie chart requests with ranked bars or a segmented bar that prints its values
- Changed Claude’s file tools to ask before editing the local copy of your organization’s server-managed settings, as they do for your settings files
- Changed `/ultrareview` to check workspace trust before it runs git, as`claude ultrareview` does
- Changed the Bash tool’s shell to clear any inherited `which` alias or function and to stop passing exported`which` /`unsetenv` functions to commands, so`which` is the system command or zsh builtin
- Changed `/permissions` Recently blocked to refuse approving a call you can’t read in full, including one the terminal is too short to show
- Changed auto mode in Cowork cloud sessions: a hook’s allow of an MCP tool call now counts as no answer, so the classifier or a permission prompt decides
- Changed hook `if` conditions such as`Monitor(tail *)` to match the Monitor tool’s shell commands as`Bash(tail *)` matches Bash’s
- Changed the `claude-api` skill to ask to read a Managed Agents workflow run’s workflow before changing the agent to improve that run
- Changed flagged requests to retry on another model, not end in a refusal, when `ANTHROPIC_DEFAULT_OPUS_MODEL` ,`availableModels` or`deniedModels` leaves only an older Opus model, or none
- Changed idle compaction to also run for subscribers with telemetry turned off, and on Bedrock, Vertex, Foundry and LLM gateways once the one-hour prompt cache is on
- Changed `FORCE_HYPERLINK=false` (also`no` and`off` ) to turn hyperlinks off instead of on
- macOS: Changed `git grep` in most commands that Claude runs to use the C locale, to avoid a reported memory leak in macOS’s regex library on files that aren’t valid UTF-8
- [VSCode] Changed `CLAUDE_CODE_DISABLE_CFC_PROMPT` to also stop the extension adding the Claude in Chrome instructions to a session’s messages; the browser tools stay available
- [Cloud sessions] Changed cloud sessions from the web, desktop and mobile apps to start even when a setup script exits with an error, instead of failing to start

**Added**

- Added `oidc.user_id_claim` to the Claude apps gateway, to identify people by an id_token claim other than`sub` , such as Microsoft Entra ID’s`oid`
- Added click to send: in fullscreen, clicking the “to send now” hint under a queued message sends it right away, as its key does
- [Cloud sessions] Added an effort level to the model picker when you create or edit a routine that runs in the cloud
- [Cloud sessions] Added a Bash editor with colors, line numbers and a Copy button to the setup script box in cloud environment settings

**Changed**

- Changed Bedrock, Vertex, Foundry and Mantle to use the shorter system prompt for model names Claude Code doesn’t recognize, such as custom deployment names and inference profile ARNs, even when they front Sonnet or Haiku; set `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0` to keep the full prompt
- Changed automatic model switches after a flagged message to also cover model names Claude Code doesn’t recognize, such as custom deployment names; set `CLAUDE_CODE_DISABLE_REFUSAL_FALLBACK=1` to turn them off
- Changed usage limit messages to call the 5-hour window the “5-hour limit” instead of the “session limit”
- Changed cloud sessions started from the CLI to get their names and branch names from the server
- Changed prompt and agent hooks to run at most 24 at a time per session, sub-agents included, plus at most 12 that have waited longer than their event’s timeout; a `timeout` under 1 second now counts as 1 second
- Changed a subagent’s `autoCompactWindow` to lower the window set by`CLAUDE_CODE_AUTO_COMPACT_WINDOW` , where it had no effect before; it still never raises that window
- Changed OpenTelemetry `mcp_server.name` to report the real name of fixed-URL remote MCP servers declared by plugins from the official Anthropic marketplace, instead of`custom`
- Changed Claude Code to stop sending the `claude_code.safety_event` OpenTelemetry event
- [VSCode] Changed usage limit resets to show a clock time (“Resets at 3:10 PM”) and to say “5-hour limit”

**Improved**

- Improved the prompt’s effort indicator: a change made mid-task now says it may wait for your next message, and the note clears when it no longer applies
- Improved WebFetch errors in cloud sessions: a host that does not resolve is now explained as possibly outside the allowed network domains, so Claude names the host to allow
- [Cloud sessions] Improved the step shown when Claude adds a repository to a cloud session: opening it now shows a short table of what happened instead of raw JSON
- [Claude Tag] Improved Claude Tag’s retry notice in Slack to name the model in use, or say rate limits were hit, instead of a generic API error

**Added**

- Added `--hold-idle-file` and`--max-hold-min` to`self-hosted-runner` , to keep an idle session while your host reports processes it left running
- Added a tip for the Explanatory output style, shown when you ask Claude for explanations
- [VSCode] Added syntax coloring to code blocks in Claude’s replies, in light, dark and high contrast dark themes
- [VSCode] Added a “Running UserPromptSubmit hooks” spinner status and screen reader announcement for slow UserPromptSubmit hooks
- [Claude Tag] Added a Skills view to Claude Tag analytics showing how often Claude runs each attached skill in Slack channels and which go unused, with Usage links in admin settings

**Changed**

- Changed `ultrathink` in the interactive terminal to run its turn at xhigh effort on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1 through Anthropic’s API; typed while Claude is working, it raises the rest of that turn
- Changed the `ultrathink` rainbow to appear only where the keyword can raise effort, and not when the word is quoted or part of a file path
- Changed Ctrl+D in agent view to match the prompt: on an empty input, pressing it twice exits
- Changed `--output-format json` and`stream-json` output to write DEL, C1 control and Unicode format characters inside strings as`\uXXXX` escapes; parsed values are unchanged
- Changed model names and IDs to drop the `[1m]` suffix and its label on models where the suffix has no effect; the suffix is still accepted
- Changed startup warnings about permission rules in settings files to show once per user until the rule changes
- Changed `/code-review` to tell Claude to put the effort level first and use max only when asked
- Changed `DISABLE_COMPACT` in settings files and in a mod’s`$.env.set` so they can only turn compaction off, not back on
- Changed Claude Code on Bedrock, Mantle, Vertex, and Foundry to load model settings from a signed file on downloads.claude.ai; set CLAUDE_CODE_MODEL_CATALOG=off to turn this off
- Changed sessions that set `CLAUDE_CODE_SYNC_SKILLS` with feature-flag fetching off to reuse a claude.ai skills list up to 10 minutes old that another session on the machine fetched
- Changed clicks on scrollbar arrows and bars in fullscreen lists to act when the mouse button is released, so moving off the scrollbar before letting go cancels the click
- Changed `claude purge` to say it deletes data only on this computer, not anything stored by Anthropic
- [VSCode] Changed the extension to start Claude Code sessions with `DISABLE_TELEMETRY` set while VS Code’s telemetry setting is off, so they send no usage metrics or error reports and fetch no feature flags
- [VSCode] Changed the session list side bar to load Web sessions when the Web tab is first opened, not at startup
- [Claude Tag] Changed Claude Tag admin settings to show a lock instead of # for private Slack channels that your own Slack account can see

**Added**

- Added the Monitor tool and plugin monitors on Bedrock, Vertex, and Foundry, and when telemetry or nonessential traffic is disabled; long foreground sleeps are now blocked there too
- [Cloud sessions] Added Claude Code cloud sessions and Remote Control permissions to Enterprise custom roles, so Owners can control access to each by role
- [Cloud sessions] Added a Choose file button to load a cloud environment’s setup script from a file

**Changed**

- Changed the error for an MCP tool call that times out in a non-interactive session while the server’s elicitation is unanswered: it now tells Claude that no answer came and not to repeat the call
- [Cloud sessions] Changed turning on Cloud sessions or Remote Control in admin settings to ask for confirmation and list the custom roles that have that permission
- [Cloud sessions] Changed the cloud environment dialog to open network access, setup script and environment variables from rows instead of listing every editor at once

**Added**

- [VSCode] Added a log warning that says what to do when dictation or the extension’s own requests fail certificate verification, such as behind a TLS-inspecting proxy

**Changed**

- Updated the bundled dataviz skill to accept APCA as a text-contrast measure (keeping WCAG 2 ratios where AA conformance is required), add an `apca(text, background)` function to its palette validator scripts, and say whether a label inside a colored chart fill should be white or near-black
- Updated the /claude-api skill: service account keys and Workload Identity Federation tokens can now manage OpenTelemetry destinations
- Changed `sed -i` to get the standard Bash prompt and run as written, not a simulated file-edit preview; like other Bash edits, it is no longer restored by`/rewind`
- Changed `claude ultrareview` to require workspace trust: it asks on a terminal, else refuses; scripts pass`--trust-workspace` , which takes no value (put a PR number or base branch before it), or set`CLAUDE_CODE_DISABLE_ULTRAREVIEW_TRUST_CHECK=1`
- Changed the dangerous `rm` and`rmdir` prompt in bypass permissions mode in the Desktop app and VS Code to deny the command at once with a rewrite hint, so unattended sessions keep going
- Changed commands the settings exclude from the sandbox to ask before writing to its write-list paths while the sandbox is enabled but cannot run; the unsandboxed fallback is refused there
- Changed worktree creation and reuse so git no longer fetches objects on demand, in ordinary clones and in partial clones with a Bash sandbox configured; such a partial clone fetches them first
- Changed the `/claude-api` skill to build Managed Agents subagent teams on`multiagent_20261001` with dynamic workflows on by default, instead of`coordinator`

**Added**

- Added `/claude-api api-version` to move Claude API code to API version`2026-10-21`
- Added the `claude_code.safety_event` OpenTelemetry event for each tool call a permission rule, auto mode, a safety check, a hook or the sandbox stopped, prompted or sent back
- [VSCode] Added a notice on the microphone button when voice dictation stops by itself (15 seconds without recognized speech, or the 2-minute limit), saying why and that the text so far is kept
- [Code Review] Added `@claude review stop` (also`off` and`pause` ) to Code Review, ending an`@claude review always` subscription on a pull request without starting a new review

**Changed**

- Changed the notice Claude gets when the WebSearch budget is used up to also tell it not to query search engines in a browser or through computer use
- Changed the `axScreenReader` setting and`CLAUDE_AX_SCREEN_READER` in a settings`env` block to apply only from user, managed or`--settings` settings (since 2.1.305)
- Self-hosted runner: Changed checkout hooks to be stopped after 30 minutes by default, failing the session’s setup; if yours needs longer, raise `--checkout-hook-timeout-min` or set it to`0`
- Self-hosted runner: Changed the certificate file built for Anthropic-managed git to filter its trusted certificates as the CLI does; a `[runner:warn]` line gives the number left out
- [VSCode] Changed the “still open somewhere else” notice to also cover a Claude process left over from an extension restart, which usually ends on its own within about 10 minutes
- [VSCode] Changed voice dictation to filter its trusted certificates as the CLI does
- [VSCode] Changed the extension’s own requests to filter their trusted certificates as the CLI does, and to trust the `NODE_EXTRA_CA_CERTS` file named in the environment or in user settings

- Fixed `claude -p` inside cloud sessions reporting “Not logged in” when started without the session’s environment variables
- Fixed an API error being replaced by “prompt is too long”, and an unneeded compaction, in sessions with many MCP tools

**Added**

- Added `sandbox.allowNestedUserNamespaces` so sandboxed commands on Linux can create user namespaces (Chromium, rootless podman); this lets them get around the sandbox’s file protections
- Added `CLAUDE_CODE_RETRY_WATCHDOG_USAGE_LIMIT_RECHECK_MS` so unattended retry mode (`CLAUDE_CODE_RETRY_WATCHDOG` ) notices a usage limit that was lifted before its reset
- [VSCode] Added a “Running start-up hooks” spinner status, with a screen reader announcement, while SessionStart or Setup hooks run
- [Claude Tag] Added public Slack channels Claude hasn’t joined to the Claude Tag admin Add place search, so admins can set one up from there

**Changed**

- Changed `allowedMcpServers` to stop filtering`managed-mcp.json` stdio servers that use`${VAR}` only in`env` : one your allowlist left out now loads on upgrade; use`deniedMcpServers` to keep it off
- Changed plugins that Claude Desktop mounts by path to no longer keep their `allowed-tools` when your organization’s managed settings set`allowManagedPermissionRulesOnly`
- Changed the `/artifacts` delete confirmation to take 1 or Enter to keep, 2 to delete, and Esc to cancel;`y` and`n` no longer answer it
- Changed a mod’s docked pane to stay within 45% of the terminal’s width, including a width kept from a wider terminal
- [VSCode] Changed the message shown when a session does not start in time and no hook is running: it now leads with the output logs and lists causes instead of pointing to sign-in and network first
- [Code Review] Changed Code Review re-reviews so they no longer open with LGTM while earlier blocking findings are still open

**Added**

- Added a transcript line when a plugin that runs first skips one of your mod’s hooks, naming the event and the plugin
- Added `decider: "person"` to a mod’s`tool.check` answer: only the permission dialog or a host’s callback can approve the ask, and it is refused with the mod’s reason where nobody can be asked
- Added `e.isReadOnly` to a mod’s`tool.check` event, set when the tool call is read-only
- Added `/auto-mode-setup` : a guided setup that reads your project and recent sessions to teach auto mode about your environment, now also available on Bedrock, Vertex, Foundry, Claude apps gateway and telemetry-disabled sessions
- Added `/claude-api managed-agents-secure` , which compares a Managed Agents agent’s settings with Anthropic’s published security guide, without changing anything
- Added the names of the tools called earlier to the compaction summary in the transcript view (Ctrl+O)
- Added a warning on Pro and Max plans when resuming an inactive conversation would use a noticeable share of your 5-hour usage limit: resume anyway, or start a new conversation and Claude can look up the previous one if you refer to it
- [VSCode] Added mod drawing in the chat panel for sessions on Amazon Bedrock, Google Vertex AI and Microsoft Foundry, behind an LLM gateway, or with telemetry turned off
- [Code Review] Added review levels to Code Review: Owners choose Quick, Standard (the default), or Deep for the organization, and can give individual repositories their own level
- [Code Review] Added an option in Code Review admin settings to review every repository in a GitHub organization, including ones added later, without selecting them one by one

**Changed**

- Changed mods’ `settings.read` hooks to receive each settings layer and rewrite the merged settings, so a managed mod can make an organization’s setting a ceiling, including the`cleanupPeriodDays` the retention sweep uses
- Changed the `/artifacts` delete confirmation to show numbered choices: 1 or Enter keeps the artifact, 2 deletes it, and y, n and Esc still work
- Changed the question that turns on the mods Claude writes in a session to plain text with no link or session id; it stays up when a new turn starts, and Claude tells you what loaded when you turn them on
- Changed tool search to be on by default behind a custom `ANTHROPIC_BASE_URL` ; a 400 or 422 from the gateway is retried without it
- Changed compaction to send Claude the skill list again afterwards; `/reload-skills` now sends only new or changed skills
- Changed skill hooks: the skills of one source (a repo, your own folder, or a plugin) can register at most 1,024 hooks and 16,384 matcher characters per session
- Changed skills under a policy that allows hooks from plugins only: a skill used in the main conversation whose hooks are kept out no longer gets its `allowed-tools`
- Updated the /claude-api skill to use the `ant` CLI on Claude Platform on AWS instead of falling back to the SDK or curl
- Removed the notes from `claude plugin validate` that predicted where a mod’s hooks would run
- Removed the “(research preview)” label from the `/fast` dialog title on Opus 5.5
- [VSCode] Changed background sub-agents to show as one status line under their Agent card instead of streaming every step into the chat; the full transcript stays in /tasks

**Added**

- Added `CLAUDE_CODE_AUTO_MODE_MODEL` to choose the model the client-side auto mode classifier uses; it does not change the model for server-side auto mode
- Added an `Idle compaction` row to`/config` for turning idle compaction off or back on; the notices shown after an idle compaction now say how to turn it off
- Added a notice to `/design-sync` and`/design-login` that standalone Claude Design (claude.ai/design) closes by December 14, 2026
- [Cloud sessions] Added a banner on a routine’s page that says why its last run didn’t start, such as your organization reaching its limit on concurrent cloud sessions

**Changed**

- Changed `claude -p` and Agent SDK sessions with no permission mode to start in`default` mode on third-party providers, with telemetry off or on a fresh install;`--permission-mode auto` keeps auto
- Changed fork subagents (`subagent_type: "fork"` ) to be on by default in`claude -p` , the Agent SDK, the desktop app and VS Code, as they already are in the terminal;`CLAUDE_CODE_FORK_SUBAGENT=0` turns them off, and`claude mcp serve` never offers them
- Changed the desktop app and VS Code to always run the main conversation’s subagents in the background, as the terminal does, rather than let Claude choose; a subagent’s own Agent calls in `claude -p` , the Agent SDK, the desktop app and VS Code now run in the foreground by default
- Changed the `← for agents` footer hint to hide 10 seconds after a session opens; the count of agents waiting on you still shows
- Changed where the Claude in Chrome instructions sit in the system prompt so that new conversations can read them from the shared prompt cache
- Changed prompt and agent hooks so that at most 12 of one event run at a time; the rest wait their turn
- Changed the message shown when your organization’s policy turns off `/feedback` : it now points to GitHub issues instead of an organization admin
- [Claude Tag] Changed Claude Tag admin settings to organize Claude’s access into four tabs: Bundles, Connectors, Skills and plugins, and Channels
- [Claude Tag] Changed the Roles page in organization settings to group the roles created for Claude Tag channel managers in a collapsed section below your organization’s own roles

**Added**

- Added `CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS` so unattended retry mode (`CLAUDE_CODE_RETRY_WATCHDOG` ) ends a request at a usage limit that resets too far away instead of waiting
- Added OpenTelemetry tracing guidance to the `claude-api` skill, including setting up trace destinations with the`ant` CLI
- Added the `%` motion to vim mode: jump to the matching bracket, alone or with d/c/y
- Added `rows` to a mod’s`Input` : a text field of several lines in the terminal, where Enter submits and Shift+Enter or a backslash before Enter starts a new line
- Added `classifier_verdict` and`permission_mode` to OpenTelemetry`tool_decision` events, so auto mode classifier decisions can be told apart from permission rules
- Added `classifier_category` to OpenTelemetry`tool_decision` events, naming the built-in rule a classifier block matched, when`OTEL_LOG_TOOL_DETAILS=1`
- [VSCode] Added LaTeX math rendering in Claude’s responses, including `$$` and multi-line`\[ … \]` blocks and inline`$…$` and`\(…\)` math
- [VSCode] Added a “Get more usage” link to the usage-limit banner for Pro and Max plans in the VS Code and JetBrains panels

**Changed**

- Changed a mod hook’s `.catch` handler to run only when the hook throws or times out; events raised beneath the hook’s own calls now skip that hook without asking its`.catch`
- Changed a mod’s `$.env.set` to resolve with`{}` once the variable is set or unset, or with`{ deny }` saying why when the write is not made

**Improved**

- Improved switching to a model with a smaller context window: a conversation too long for the new model is now summarized by the previous model first
- Improved rendering performance while a long-running subagent works: its progress trail now updates incrementally instead of being reprocessed in full on every render
- Improved artifacts: pages Claude writes now paint the band above a sticky header, so scrolled text no longer shows there in the mobile apps
- Improved mods: in VS Code and the desktop and mobile apps, a tree drawn above the question dialog can now hold Markdown and up to 48 rows; the terminal keeps 12 plain rows

**Added**

- Added `virtualText` to a mod’s prompt decorations: text drawn in the prompt box that never enters the draft or the submitted prompt
- Added `cache_creation_5m_tokens` and`cache_creation_1h_tokens` to the OpenTelemetry`api_request` event, splitting cache writes by lifetime
- Added `$.ui.view({ agentId })` for mods: shows an agent’s transcript in the main view when you ask, and`$.ui.view({})` goes back
- Added a `dir` prop on a mod’s`Text` for right-to-left text: on Windows and in VS Code’s terminal its rows read in that direction and sit flush right
- Added `/advisor` on Claude Platform on AWS; no advisor runs until you pick one with`/advisor` ,`advisorModel` or`--advisor` , and`CLAUDE_CODE_DISABLE_ADVISOR_TOOL=1` turns it off
- Added OpenTelemetry GenAI semantic-convention attributes to trace spans, such as `gen_ai.provider.name` ,`gen_ai.conversation.id` ,`gen_ai.tool.name` and`error.type`
- Added support for rewriting a workflow agent’s `prompt` and`model` in the`agent.spawn` mod hook, which could only refuse workflow agents before
- Added the `prompt.change` mod hook, raised when the prompt box’s text changes without a typed edit: a recalled history entry, a clear, an undo, or a plugin’s fill
- [VSCode] Added the `claudeCode.attachSelection` setting to stop text selected in the editor from being added to your messages
- [Claude Tag] Added support for Slack Connect channels: an Owner or Claude Tag admin can let Claude in Slack answer in channels shared with another organization; it’s off by default
- [Claude Tag] Added a private Allow once / Deny card in Slack threads for connector actions that need your approval, so Claude asks instead of refusing; others only see that it’s waiting on you
- [Claude Tag] Added support for Slack voice clips in Claude in Slack: Claude reads Slack’s transcript of a clip when Slack has one

**Changed**

- Changed `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` sessions to ignore CA-bundle, tool-proxy, git TLS and git config variables like`SSL_CERT_FILE` in settings; under Claude Desktop, only in project settings
- Changed exit to force-kill background commands still running 1.5 seconds after being asked to stop; in fullscreen mode they could previously be left running
- Changed Claude Desktop’s Code tab to apply managed settings when `forceLoginGatewayUrl` names an Anthropic-operated gateway that serves the session’s hostname; that gateway must be reachable at start
- Changed `claude gateway` to stop at start, naming the key, on a`desktop:` policy key that Claude Desktop cannot use from a gateway, where it earlier started with a warning
- Changed large requests to api.anthropic.com to use gzip compression even when telemetry is disabled; set `CLAUDE_CODE_GZIP_REQUEST_BODIES=0` to turn it off
- Changed the `claude-api` skill to leave Managed Agents sessions in place unless you ask to archive or delete them
- Changed background sessions to resume a carried workflow through the Workflow tool’s approval instead of running it from the handoff file
- Changed local session transcripts to also record each API request’s system prompt, tools and parameters, so a transcript you choose to share can reproduce a request exactly
- Changed the `/sandbox` mode options to read “Sandbox commands” instead of “Sandbox BashTool”
- Changed `PermissionRequest` hooks in Cowork cloud sessions: a`setMode` update in a hook’s allow is now ignored; the rest of the approval still applies
- Changed hooks: `updatedInput` from PreToolUse and PermissionRequest hooks is now validated like Claude’s own input: unknown keys are dropped, and input the tool rejects denies the call
- Removed the `--sandbox` and`--no-sandbox` flags from`claude remote-control` , which had no effect; passing either now exits with an error. Set`sandbox.enabled` in settings to sandbox sessions
- [Claude Tag] Changed access bundles an admin attached to a Slack channel to keep applying once the channel becomes a Slack Connect channel, with no need to remove and re-attach them

**Improved**

- Improved WebFetch in Cowork cloud sessions to tell Claude that a page failed moments ago instead of fetching it again
- Improved mod sharing in the built-in `plugin-authoring` skill: Claude scans a mod’s files for keys before opening them, and asks before scanning a folder outside the project
- Improved the note Claude 3 models get about an @-mentioned PDF, so they less often answer without reading it
- Improved artifact updates: Claude now applies a small change and republishes the artifact in one call, and makes larger edits and republishes within a single response

**Added**

- Added `CLAUDE_CODE_DISABLE_FILE_MENTIONS` environment variable: Claude gets the paths of`@` file mentions to read with its file tools, not the files’ contents
- Added idle compaction in the terminal: a 200K+ token conversation left idle for about 54 minutes is compacted before its one-hour prompt cache expires, so your next message doesn’t reprocess it all
- Added `CLAUDE_CODE_IDLE_COMPACT_MIN_TOKENS` to change idle compaction’s 200K-token minimum;`"idleCompaction": false` in settings.json turns it off. Both need v2.1.290 or later
- Added `$.prompt.media` so a`prompt.submit` hook of a mod installed by managed settings can read the bytes of an image or document attached to the prompt
- Added `$.session.identity()` for mods installed by managed settings: how the session is signed in, and its organization and email when managed`forceLoginOrgUUID` lists that organization
- Added `size` ,`weight` ,`mono` and`tabular` to a mod’s`Text` and named spacing steps (`xs` to`xl` ) to its`Box` ;`bold` is deprecated in favor of`weight`
- Added `--shutdown-drain-sec` to`claude self-hosted-runner orchestrator` : on SIGTERM it stops polling and gives the poll in flight and running spawn hooks up to that many seconds to finish
- Added the skill’s frontmatter, source, plugin name and file path to what a mod’s `skill.prompt` hook receives
- Added `/effort` and`/fast` in agent view to set what new sessions start with; a bare`/model` ,`/effort` or`/fast` there now opens a picker
- Added a “safe mode” line to the agents view peek panel for background sessions started with `--safe-mode`
- Added a one-time note when plan mode won’t ask before most edits or commands because bypass permissions is available
- Added `--disallow-tools` and a`disallowed_tools` case field to`claude plugin eval` to keep specific built-in tools, such as Agent, out of eval runs
- Added support for turning off the attribution block in Claude Desktop’s Code tab: set `CLAUDE_CODE_ATTRIBUTION_HEADER=0` in the`env` of a Claude apps gateway’s`code` policy
- Added a field to the attribution block in Claude Code sessions behind a Claude apps gateway; remove the block with `CLAUDE_CODE_ATTRIBUTION_HEADER=0`
- Added a `queue` door to the`session.append` mod hook, so a mod can make the transcript’s queue records differ from the prompt as sent
- Added a note in `/plugin` and`claude plugin validate` naming a mod’s hooks that are skipped where an organization manages Claude Code
- Added `/usage` warnings for max effort, workflows, always-on context and large tool results
- [Claude Tag] Added the ability for Claude in Slack to delete a canvas it created in a channel when someone asks it to

**Changed**

- Changed plugin hooks and workflow scripts to refuse values they hand back that nest deeper than 28,000 levels, with an error naming the limit (workflow scripts allowed 100,000)
- Changed actions a plugin hook can block to be refused, not let through, when the hook cannot be handed them or cannot hand back its answer; a tool that already ran has its result withheld
- Changed requests to claude.ai’s connector proxy and to Anthropic-hosted MCP servers to no longer follow HTTP redirects; redirects by a connector’s own server are unaffected
- Changed MCP requests in cloud and self-hosted runner sessions to no longer follow HTTP redirects from the session’s MCP proxy
- Changed skill and command loading: one whose file or folder name has hidden, direction-changing or blank-like characters, `<` or`>` is no longer offered until renamed; such a`name:` is ignored
- Changed `ui.render` trees in`claude plugin test` : each mod’s Box or Text root now carries`part: { plugin }` naming the mod, so exact-tree assertions must include it
- Changed WebFetch page reads to pick their effort as web searches do: the caller’s level on the caller’s own model, otherwise the level settings give the small model
- Changed `claude plugin test` to fail a call to a mod’s served`$` method that has not answered within ten seconds, as a session does
- Changed MCP allow rules to reach only the server they name: `mcp__a` no longer allows a server named`a__b` or`a_` ; write`mcp__<server>__*` with the server’s whole normalized name
- Changed agent `tools:` lists to take only the named MCP server’s tools; list a server whose name contains`__` as`mcp__<whole name>__*`
- Changed Claude in Chrome in Cowork cloud sessions outside auto mode: an approved-site action the safety classifier flags now asks you instead of being refused, unless managed settings apply
- Changed cloud session and Remote Control MCP connectors to negotiate MCP protocol version 2026-07-28 by default on installs that fetch no flags; `MCP_PROTOCOL_NEGOTIATION=legacy` opts out
- Changed MCP tool descriptions and server instructions from Anthropic’s first-party MCP servers at `api.anthropic.com` to reach Claude in full instead of being cut at 4,096 characters
- Changed auto mode in Cowork cloud sessions: a `PermissionRequest` hook can no longer approve shell commands or subagents; those prompts wait or are denied
- Changed the resume command printed at exit: it names the session by its ID and no longer by its name, so that nothing in a session name can run when the line is pasted
- Changed the resume command printed at exit of a worktree session: it uses `-w` and`-r` if the long flags do not fit the terminal. If neither fits a kept worktree, rows above it say where to run it
- Changed `/btw` side questions in non-interactive sessions to return a timeout error when still unanswered after 11 minutes
- Changed `UserPromptSubmit` hooks to run before a skill or custom slash command expands, so a hook that blocks the prompt now also stops its embedded shell commands
- Changed a skill’s own front-matter `UserPromptSubmit` hook to first run on the prompt after the one that invokes the skill
- Changed the `turn.start` mod hook to also hear subagents’ and teammates’ turns, not only the main loop’s; tell them apart by`e.agentId` (absent on the main loop)
- Changed the errors shown when `/ultrareview` can’t upload a repository to stop suggesting`git add .` , which could stage or commit untracked files such as`.env`
- Changed the cyber safeguard notice to no longer ask people to apply to the Cyber Verification Program
- [Cloud sessions] Changed deleting a self-hosted environment to turn off its routines right away, with the reason shown on each routine, instead of leaving their runs to fail
- [Claude Tag] Changed how Claude in Slack corrects a message it got wrong: it edits the message in place and adds a short “Edited” note instead of striking through the old text
- [Claude Tag] Changed Claude in Slack to join channels with more than 100 members quietly, with no intro message

**Added**

- Added a `code` key to the Claude apps gateway’s`managed.policies[]` : the same settings as`cli` , also applied in Claude Desktop’s Code tab; beside`desktop` , it turns on Claude Desktop’s gateway mode
- Added `autoCompactWindow` to subagent frontmatter and`--agents` definitions, so a subagent can auto-compact earlier than the main conversation’s window
- Added `CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` to run every workflow agent on one model while other subagents keep theirs
- Added `CLAUDE_CODE_OVERLOADED_RETRY_MAX_DELAY_MS` environment variable to set a longer maximum delay for the backoff when retrying an overloaded (529) request
- Added a note in `/plugin` on a plugin whose hooks are left out because another enabled plugin has the same name
- Added an `allow_large` option to the Read tool so Claude can read a text file past the usual size limits in one call when it needs the whole file and the context has room
- [Cloud sessions] Added a status filter to the Sessions and Runners lists on a self-hosted environment’s Activity tab in admin settings; you can pick several statuses at once
- [Claude Tag] Added creating, editing and deleting workspace and channel memory files on the Activity page’s Memory tab for members with the Claude Tag Admin permission

**Changed**

- Updated `/cost` , the status line,`--max-budget-usd` and the SDK’s cost figures to price Sonnet 5.5 cache reads at $0.10 per million tokens (was $0.20)
- Updated the bundled dataviz skill: lighter violet for the seventh series in light mode, softer primary text in dark mode, and compact y-axis labels like 1K
- Changed the default limit on MCP tool descriptions sent up front and on MCP server instructions from 2,048 to 4,096 characters
- Changed `←` : a turn or`!` command started while the session moves to the background is now stopped instead of finishing out of sight
- [VSCode] Changed Claude in Chrome to ask before browser actions in every session, including one connected with `@browser` , as the terminal does; allowing a site for the session stops repeat asks
- [Claude Tag] Changed Claude in Slack to show the footer with the session link, model and cost only on its newest reply in a thread; the reply before it drops the footer

**Added**

- Added `onFailure: "block"` for command and HTTP hooks: a hook that can’t start, times out, or exits with an unexpected code blocks the action instead of letting it through
- Added Program Status Protocol (OSC 7501) support: terminals that implement it can show whether Claude Code is working, waiting on you, or done
- Added quoted text to the `/copy` picker, so a drafted message copies without its`>` markers
- Added a warning to `claude plugin install` ,`enable` ,`disable` and`marketplace add` when the settings file they write to does not load
- Added a line on stderr, when it is a terminal, that says what a `claude -p` run is waiting for when it stays open after its last turn
- Added support for `timeouts.upstream_ttfb_ms` on the Claude apps gateway’s Bedrock, Vertex, Foundry and other cloud upstreams: a value you set now limits how long a stream may take to start there, after which it fails over or gets a 502
- Added a “Backgrounding cancelled” message when you stop the turn while `←` is waiting for the current tool to finish
- Added an optional `models` list to every Claude apps gateway upstream: only the listed models are sent there, on failover too, and one`*` in an entry is a wildcard
- Added support for `forceLoginMethod: "gateway"` and`forceLoginGatewayUrl` in your own user settings on machines with no managed settings, so`/login` opens on that Claude apps gateway
- Added advice to `claude plugin validate` when a plugin’s README has no install line: it prints the line to paste and never changes the exit code, even with`--strict`
- Added `upstream_request_id` to the Claude apps gateway’s`inference` audit event: the request ID from Amazon Bedrock, the Anthropic API or another upstream, for support cases
- Added `$.ui.notify` for mods: raises a native notification through your own notification setting and says which channel sent it
- Added children to a mod’s `Button` : strings and`Text` , so a row of a list is one pressable with a chip or a dim detail inside
- Added `CLAUDE_CODE_RETRY_WATCHDOG_MAX_WAIT_MS` to limit how long unattended retry mode (`CLAUDE_CODE_RETRY_WATCHDOG` ) waits out 429 and 529 errors
- Added a `request-id` header to the Claude apps gateway’s successful inference responses, so`request_id` in Claude Code telemetry matches the gateway’s audit log
- [VSCode] Added a chat row for files Claude sends you: click a file’s name to open it in the editor; Claude’s caption shows underneath, and the row stays visible in Focus view
- [Claude Tag] Added a confirmation in Claude Tag admin settings before adding a channel manager moves that member to Custom roles, which can remove their other access

**Changed**

- Changed MCP tool descriptions the model loads through tool search to be cut at 16,384 characters instead of 2,048
- Changed subagents to preload at most 32 skills from the `skills` field, each once; a subagent with the Skill tool can still invoke the rest
- Changed Ctrl+C at the idle prompt of an attached background session to leave a pending `/loop` wakeup alone, so pressing it twice detaches and the loop keeps running; press Esc to stop it
- Changed `claude agents` stopped with its background service (macOS, or Linux without the service installed): running sessions now stop in about a minute unless it is run again, and a notice says so
- Changed claude.ai connectors to negotiate MCP protocol version 2026-07-28 by default on installs that fetch no flags; `MCP_PROTOCOL_NEGOTIATION=legacy` opts out
- Changed Artifact pages with no background of their own to show on white instead of off-white
- Changed what Claude and you read when a mod denies a tool call after the tool ran: it now says the tool ran and a plugin withheld its result
- Changed tab stops to count from where a text starts instead of from the screen’s left edge: an answer indented by 2 has its first stop 8 cells in, where it was 6
- Changed the Artifact tool’s permission prompts with telemetry disabled to the same five questions other installs get, instead of asking before each read of someone else’s artifact and each data edit
- Changed scheduled and Run now routine runs in installs with telemetry disabled to publish new private artifacts, and update your own, without asking, as in other installs
- Changed toasts from your organization’s mods to show ahead of other mods’ toasts instead of waiting behind them
- Changed the Claude apps gateway to start with a warning and serve a `desktop` policy key its bundled Claude Desktop schema doesn’t know, so new Desktop settings need no gateway upgrade
- Changed WebSocket (`ws` ) MCP servers: a message over 16 MiB is no longer parsed and closes the connection, the limit the other transports already have
- Self-hosted runner: Changed the runner to stop passing `CCR_AUTO_MODE_ALLOW` ,`CCR_AUTO_MODE_ENVIRONMENT` and`CCR_AUTO_MODE_SOFT_DENY` into a session’s environment
- [Claude Tag] Changed the settings confirmation cards Claude posts in a Slack channel to stay open for 30 minutes instead of 10
- [Claude Tag] Changed Claude’s notice that a channel shared across Enterprise Grid workspaces uses organization defaults to post only when someone mentions Claude, at most once a month

- Fixed `prompt` and`agent` hooks written as instructions (such as “Block commands that…”) allowing what they should block
- Improved how `prompt` hooks on Stop and SubagentStop written as instructions (such as “Carry on if the build is broken”) are judged, so Claude is less likely to stop early

**Added**

- Added Claude Haiku 5.5 (`claude-haiku-5-5` ), now the default Haiku model on the Anthropic API — 1M context, $0.10/$0.50 per Mtok ($0.50/$2.50 for prompts over 100K)
- Added `agentType` to the`subagentStatusLine` payload, so scripts can tell custom subagent types apart
- Added `isDeferred` to`$.tool.register` for mods:`false` lists the tool’s schema in the prompt from the start instead of behind tool search

**Changed**

- Reverted the auto mode denial message change from 2.1.281 that told Claude a denial covers the outcome, not only the exact command
- Reverted the 2.1.290 fix for cloud sessions staying asleep after a container restart lost a pending `/loop` wakeup or scheduled task; Claude is no longer told, and the session stays asleep
- Changed claude.ai skill syncing to check for changes about every 40 minutes, instead of every 10, while no session is in use
- Changed the order of agent lists and of MCP servers announced to the model: names with non-ASCII characters now sort after ASCII names
- Changed OpenTelemetry `claude_code.at_mention` logging to emit at most 100 agent and 100 MCP-resource events each time a prompt is read
- Self-hosted runner: Changed the orchestrator to sleep 4 to 6 seconds between polls instead of a constant 5, so several replicas for one environment stop polling at the same moment
- [Claude Tag] Changed Claude in Slack to join announcement channels, where only admins can post, without posting its intro
- [Claude Tag] Changed the limit on channel rules in Claude Tag admin settings from 20 to 50 for each workspace and for the organization-wide Slack page

**Added**

- Added `--marketplace <source>` to`claude plugin install` : adds the marketplace if needed, under the same policy checks as`claude plugin marketplace add` , then installs the plugin from it
- Added an `effort` parameter to the Agent tool, so Claude runs a sub-agent at the effort level you ask for
- Added `CLAUDE_CODE_OVERLOADED_RETRY_BASE_DELAY_MS` environment variable to set a longer base delay for the backoff when retrying an overloaded (529) request
- Added `prompt.autocomplete` , an event a mod hooks to add its own rows to the prompt box’s autocomplete list
- Added prompt caching to `$.model.complete` for mods:`prompt` and`system` take blocks of text, and`cache: true` on a block caches the request up to it
- Added workflow agents to the `agent.spawn` mod hook, with their run and index, so a mod can refuse them
- [Claude Tag] Added an Edit button to the Allowed domains card on a channel’s Configure page, so Enterprise admins can open the access bundle that sets the channel’s domains
- [Code Review] Added the period’s total with its change from the previous period, and a breakdown by repository, to the PRs reviewed chart in Code Review analytics

**Changed**

- Changed local (stdio) MCP server connections to negotiate protocol version 2026-07-28 by default on every install, including Bedrock, Vertex and Foundry; `MCP_PROTOCOL_NEGOTIATION=legacy` opts out
- Changed `claude plugin test` : a failed`expect` inside a hook the test registered, or a stub answer the engine refuses, now fails the test instead of passing silently
- Changed usage limit messages to write claude.ai settings links with https:// so terminals and apps can make them clickable
- Changed scheduled and Run now routine runs to publish a new artifact only you can see without asking for approval; artifacts that request connectors or other access still ask
- Changed agent names to allow at most 256 characters: a longer one is rejected, and a skill’s or a plugin file’s `name` longer than that is ignored
- [Claude Tag] Changed the first message of a Slack thread continued with `!fork` to a card showing where it came from, the request, and who asked, with a link to the original thread
- [Claude Tag] Changed the organization-wide and default spend limit boxes on Claude Tag’s spend limits page in admin settings to save only when you press Save or Enter, not when you click away

- Fixed a regression in 2.1.290 where cloud sessions could drop answers to permission prompts
- Fixed a regression in 2.1.288 where the last messages of a session could be lost when quitting

**Added**

- Added `serverToolUses` to the result of a mod’s`turn.step` hook: the tool calls the API ran itself (the advisor), each with its id, name, input, start and end
- Added `agentId` to the`tool.check` event of plugin hooks, so a hook can tell a subagent’s permission check from the main session’s
- Added `ceiling` to the question and verdict a mod’s`tool.check` hook reads, naming the approval an organization requires for a tool
- Added `ThemeKey` and`Color` types to the plugin hooks typings, so an editor lists the theme colors a mod’s drawing can name
- Added to `claude plugin validate` : each hook a mod registers at a gating site is listed with whether it has a`.catch` (`gatingHooks` under`--json` )
- Added a Deny button to the Claude apps gateway’s sign-in approval page: it ends the pending sign-in, so the waiting terminal stops within seconds
- Added `claude attach <name>` and`claude logs <name>` : part of a session name works in place of the id
- Added `/claude-api managed-agents-onboard <url>` to set up the Managed Agents pattern a page describes as`ant apply` files
- Added `/claude-api managed-agents-onboard <quickstart-name>` to build a Console quickstart template, such as`deep-researcher` , with the`ant` CLI
- Added a warning when a managed settings file is a link to a file outside the managed settings folder
- Added a /status and doctor warning when managed settings ignore user-configured sandbox allowRead paths or allowed domains
- [VSCode] Added a screen reader announcement, “Message queued.”, when you send a message while Claude is working
- [VSCode] Added a way to review and run a plugin marketplace’s install or update command from the Manage plugins dialog
- [Claude Tag] Added fast mode in Slack: mention Claude with `!fast` to switch a thread to fast mode, moving it to Opus if needed, and`!fast off` to switch back; replies show (fast) while it’s on
- [Claude Tag] Added the optional Path prefixes field when creating a custom connection in an access bundle, so its allow rule can cover only those paths instead of the whole host

**Changed**

- Changed `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` to also skip the startup connection warm-up
- Changed Claude in Chrome so that a project’s settings files can no longer turn it on; use `--chrome` ,`/chrome` or your user settings
- Changed the Bash tool to ask for permission before running `pyright` , which is no longer treated as a read-only command
- Changed what a mod’s `$.process.spawn` rejects with when another mod denies it after the child ran: it now says the call ran and a plugin withheld its result
- Changed the background daemon’s log to write a multi-line message as one JSON-quoted line
- Changed skills and custom commands to refuse a `!` shell command that contains raw control characters other than tab and newline, with a message that shows where they are
- Changed `/artifacts` : opening an artifact in your browser now closes the list
- Changed Bash permission checks so that more forms of the `ps` command ask for approval instead of running without asking
- Changed plugin hooks so long text is clipped and logged instead of being refused or dropped silently
- Changed background sessions whose scheduled task is gone: they now move to Completed about 20 seconds later and can be updated or shut down when idle
- Changed the “Press ← again” confirm on a just-cleared prompt: a second ← no longer has to wait a second before it switches, and holding ← down now switches too
- Changed the errors shown when the `/ultrareview` upload fails at a git step: they name the step and what to try, and no longer repeat git’s own error text
- Changed `/code-review` at medium effort to also report cleanup and CLAUDE.md conventions findings on models without tuned review settings, including Opus 5.5 and Sonnet 5.5
- Changed an in-process teammate’s `agent_id` in Agent results to its agent ID (its`name@team` address stays in`teammate_id` ); TeammateIdle hooks no longer fire from its subagents or forks
- Changed background sessions waiting on a scheduled wakeup (`/loop` ): they are now left running through updates and low memory, where being restarted or shut down could silently lose the wakeup
- Changed `/model` ,`/effort` and`/rename` sent from`claude agents` to a busy background session to apply right away, without a confirmation, instead of when the turn ends
- Changed the Claude apps gateway’s minimum supported PostgreSQL version from 14 to 11
- Changed the interactive session’s WebSearch budget to refill over time (100 calls/hour; `CLAUDE_CODE_WEB_SEARCH_REFILLS_PER_HOUR` sets the rate, 0 turns it off) instead of ending after 200 calls
- Changed `CLAUDE_CODE_DISABLE_ATTACHMENTS` so a repository’s`.claude/settings.json` or`.claude/settings.local.json` can no longer set it; shell, user and managed settings still can
- Changed `claude plugin update` on a plugin loaded from a directory to print just its reason, without the “Failed to update plugin” prefix, as for built-in plugins
- Changed the built-in `gh api` in cloud sessions: a host other than github.com set in`GH_HOST` or`GH_REPO` is now refused (use`--hostname` or a full URL), and stderr notes requests to other hosts
- Changed the `claude-api` skill’s Managed Agents examples to turn off the web tools unless the agent needs them and to use the`auto` permission policy
- Self-hosted runners: Changed `claude --environment <id>` to create its session through the current Sessions API; printed and JSON session ids keep their session_… form
- [VSCode] Changed message timestamps to show by default (turn them off with the Claude Code: Show Message Timestamps setting)
- [Claude Tag] Changed the channel instructions limit to 8,192 characters instead of bytes, so non-English text gets the same room, and added a character count beside Save on the Configure page

**Added**

- Added `agent.spawn` for teammates, one agent id across plugin hook events, and idle and waiting states in`$.agent.list()`

**Changed**

- [VSCode] Reverted a 2.1.288 change to `claude auth status` that may have made sign-outs more frequent

**Improved**

- Improved how quickly large files open in a plugin code pane by laying the highlighted view out once at its final width
- Improved the line a mod’s author sees when its band or pane fails to draw: it names the mod and says nothing was drawn

**Added**

- Added `$.ui.selection()` for mods: returns the text you last selected in fullscreen mode and, when the selection lies within one transcript row, that row
- Added a built-in `gh api` to cloud sessions whose image has no GitHub CLI, and fixed the built-in sending control characters from file names, jq filters or GitHub errors to the terminal
- Added recovery for a prompt cleared with Ctrl+C: pressing Up on the empty prompt brings the draft back, including pasted text and images
- Added a re-authenticate prompt when an MCP server asks for more OAuth scope during a tool call
- Added `--max-findings <n>|all` to /code-review to report more or fewer findings than the usual limit; the choice is reused until you pass`--max-findings default`
- Added Ctrl+F to find a session by name and Alt+↑/↓ to jump between groups in the agents view; both, and rename, can be rebound in keybindings.json
- Added a screen reader mode announcement of the new permission mode when you approve a plan, including with Shift+Tab

**Changed**

- Changed the background command time limit to apply only in unattended sessions (`-p` , Agent SDK, CI, cloud); terminal, desktop app and VS Code sessions have no limit
- Changed the client-side auto mode classifier to ignore an `ANTHROPIC_DEFAULT_SONNET_MODEL` pin that names Claude Sonnet 5.5 or Opus 5.5 and use Claude Sonnet 5 instead
- Changed `claude project purge` to`claude purge` ; the old name still works and prints a notice
- Changed the agents view `n:` filter (and Ctrl+F search) so Enter opens the session whose name matches best instead of the top row
- Changed `/autocompact` to save the auto-compact window per model, so each model keeps its own setting when you switch
- Changed MCP URL prompts from servers that can’t report when you’re done to wait for “I’m done, continue” before the tool call continues, so you can finish in the browser first

**Added**

- Added Claude Mods: plugins may now modify deeper behavior
- Added You should know, a built-in mod where a side agent watches your back and flags things you or Claude might miss. Turn it on with `/plugin enable cc-plugin-you-should-know@builtin` (for first-party sessions with telemetry on)
- Added an `n:<text>` filter to the agents view that matches session names and tasks; a filter now shows matches in collapsed sections and Enter opens the first match
- Added `prompt_text` to the OpenTelemetry`user_prompt` event, a copy of`prompt` for backends that nest dotted keys; drop or mask it wherever you drop or mask`prompt` (anthropics/claude-code#70763)
- Added URL prompts from MCP servers on the 2025-11-25 protocol, for example to sign in. If a server no longer connects after this update, add “bareElicitationCapability”: true to its MCP config entry
- Windows: Added a startup warning when denying the Bash tool also turns off the PowerShell tool, so Claude has no shell tool
- Self-hosted runner: Added a built-in `gh api` (REST only) for sessions that use Anthropic-managed git on macOS and Linux machines where the GitHub CLI is not installed
- [VSCode] Added “Run in background” to a running command or sub-agent, to move it to the background and keep working
- [VSCode] Added the output of background shells and Monitors to their cards in the agent map

**Changed**

- Changed a shell write through a repo-committed symlink onto a sensitive file or out of the working tree to name where it lands and wait for a person, on lines with a `~` target too
- Changed Opus 4.7+ and Fable to use a 1M context window by default on Bedrock, Vertex, Foundry and the Claude apps gateway, with no `[1m]` suffix (`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` keeps 200K)
- Changed replies from `claude agents` to arrive as queued messages; slash commands other than`/stop` sent while a turn is running now run when it ends
- Changed whole-tool `Bash` allow rules and allowing hooks to prompt for, not run, shell writes to files Claude Code’s file tools refuse outright (the Anthropic profile store, the host credentials file)
- Changed right-click paste on Windows and Linux, and middle-click paste on Linux, to happen when the button is released; moving the pointer away before releasing cancels it
- Changed MCP server `alwaysLoad: false` to defer all of that server’s tools behind tool search
- Changed screen reader mode to write new or changed lines without first pausing with the cursor at the start of the line; set `CLAUDE_AX_PREPARK_MS=50` to restore the pause
- Changed automatic model switches after a flagged message to keep your current effort level instead of the new model’s default
- Changed waiting permission prompts to show oldest first, so a new prompt no longer covers the one you’re reading (prompts with a countdown still open on top)
- [VSCode] Changed the Claude in Chrome “Enabled by default” switch to also connect the editor’s own sessions, which still ask before browser actions

**Added**

- Added a count such as “2 of 5” to the permission prompt when several permission requests stack up
- Added mouse support for the “N more” rows of lists in fullscreen mode: click one to jump to that end of the list, with hover and pressed states
- [VSCode] Added bookmarks: save Claude’s responses and keep them in view in a Bookmarks side panel
- [VSCode] Added the questions Claude asks and your answers to the conversation: after you answer a question card, a Questions row shows each question with your picks
- [VSCode] Added option previews to question cards in the chat panel: the highlighted choice’s mockup or snippet shows beside or under the options
- [VSCode] Added rows under a message that open to the terminal output, browser tab, browser instructions and selected code sent with it
- [Claude Tag] Added an Add channel button to Claude Tag’s spend limits page in admin settings, so a limit can be set on any channel, including a private one, from its channel ID or Slack link

**Changed**

- Changed prompts sent while nothing is running or queued to show in the normal text color right away instead of gray
- Changed how failed API requests are retried: one limit now covers a whole model call, so with the default retry settings a failing call sends at most 14 requests
- Changed `--bare` to connect only the MCP servers named on the command line, send the model no system reminders, and start no background tasks; under`--bare` , a shell command that reaches its timeout now stops instead of moving to the background
- Changed the send-now key (ctrl+enter) to move a skill’s own shell command to the background instead of ending it
- Changed the WebFetch error for a rate-limited domain safety check to tell Claude not to retry it in a loop
- Changed plugin installs to refuse npm sources that are git repositories or folders, and to install plugin dependencies only from registry packages
- Changed list screens (`/artifacts` ,`/mcp` ,`/skills` ,`/hooks` and others) to always line up each row’s details in one column after the names
- Changed the overflow rows of lists to read ”↑ N more” / ”↓ N more” instead of “N more above” / “N more below”
- Changed `/hooks` to open on one list of your configured hooks grouped by event, so viewing a hook takes one Enter instead of three
- Changed the theme picker to a scrolling list that fits your terminal instead of pushing the preview off screen; number keys no longer pick a theme
- Changed `/exit` ’s Remove worktree to run after Claude Code stops the servers and shells it started there, which on Windows could keep the folder from being deleted
- Changed the `claude-api` skill’s Managed Agents examples to create environments with limited networking
- Removed the browser link from `/ultrareview` and`claude ultrareview` output
- [VSCode] Changed Stop and Escape to end only the current turn; background agents keep running and can be stopped one by one from the agent map
- [VSCode] Changed the ”✻ Claude Code” status bar item to show in every window, so you can open Claude when no file is open
- [Cloud sessions] Changed a routine’s page to read “Due” with the scheduled time, instead of a next run time in the past, when a scheduled run is late and hasn’t started

**Added**

- Added `CLAUDE_CODE_DISABLE_WEB_FETCH` environment variable to turn off the WebFetch tool
- Added `claude --desktop` to open the Claude desktop app on the current directory, or on a session with`--continue` /`--resume <id>`
- Added `claude plugin configure <plugin>` to show a plugin’s options and which are unset, or save new values read from stdin with`--values-stdin`
- Added `<server>.<key>=<value>` to`claude plugin install --config` , so a bundled`.mcpb` MCP server’s own settings can be set at install time and it starts without visiting`/plugin` → Configure
- Added `allowedProviders` managed setting to limit which API providers a machine may use (Anthropic API, a custom endpoint, Bedrock, Mantle, Vertex AI, Foundry, Claude Platform on AWS, or a Cloud gateway)
- Added `CLAUDE_CODE_NONSTREAMING_TIMEOUT_RETRIES` environment variable to cap re-sends of a non-streaming fallback request that timed out
- [VSCode] Added a note under a restored tab’s last message when a window reload interrupted it and no reply will follow
- [VSCode] Added a plugin options form to Manage plugins: installing a plugin that has options asks for the unset ones, and a gear on its row changes them later
- [VSCode] Added an on-demand diagnostics tool so Claude in the panel can read the Problems panel’s current errors and warnings at any time, not only right after it edits a file
- [Claude Tag] Added direct messages with Claude for members on an Enterprise plan Standard or Usage-Based Chat seat who also have Cowork; a seat that includes Claude Code is no longer required

**Changed**

- Changed MCP tools so a tool that sets its own `_meta['anthropic/alwaysLoad']` to false stays deferred when its`--mcp-config` , Agent SDK or plugin server is set to`alwaysLoad`
- Changed background Bash and PowerShell commands to stop after a time limit (their `timeout` with`run_in_background` , default 30 min, max 2 h); Claude is notified when one is stopped
- Changed Code Review’s pull request reviews and `/ultrareview` to run when`disableWorkflows` is on, unless the machine running the review has it set by its own administrator (MDM or the managed-settings file)
- Changed sessions behind a custom `ANTHROPIC_BASE_URL` to use the 1M context window of models that have one (Opus 4.7+, Sonnet 5+, Fable); run`/autocompact 200k` if your gateway stops at 200K
- Changed Team and Enterprise sessions, and sessions whose sign-in plan Claude Code can’t determine, to withhold WebFetch until the organization policy loads if it couldn’t be loaded at startup
- Changed /memory so that Auto-memory can no longer be turned on from a background session or from a session one of Claude Code’s own tools started; turning it off there still works
- Changed the one-time offer to make auto mode your default permission mode to also show on third-party providers and with telemetry off, when your user settings default to another mode
- Changed `claude -p` and Python Agent SDK sessions on third-party providers or with telemetry off to start in auto mode when no permission mode is configured, like interactive sessions;`--permission-mode` still overrides it
- Changed Bedrock, Mantle and Claude Platform on AWS requests to a base URL with a non-default port to include the port in the SigV4-signed Host header
- Changed the MCP server name `widgets` to be reserved in cloud sessions and on self-hosted runners: your own server under it, or a close spelling such as`widgets_` , no longer loads, so rename it
- Changed `/ultrareview` on macOS and Linux to leave symbolic refs out when uploading a local checkout; a checkout whose current branch is a symbolic ref is now refused with an explanation
- Windows: Changed project and local settings `env` to no longer set`ALLUSERSPROFILE` ,`SystemDrive` , or the`CommonProgramFiles` variables; set them in user or managed settings instead
- Changed `/tasks` to fold background work Claude Code runs for itself under one “System tasks” row; press Enter on it to show those tasks
- Changed `/ultrareview` on macOS and Linux to require git 2.31 or newer to upload a local repository; checkouts made with`--separate-git-dir` are now refused instead of being uploaded with an older method
- Changed `/ultrareview` uploads on macOS and Linux to send a partial clone as a working-tree snapshot on git 2.31 or newer, instead of falling back or refusing when git’s version looked too old
- Changed `/ultrareview` uploads on macOS and Linux to refuse, instead of fetching, a partial clone missing some of its working tree’s files on older git versions; a clone made without`--filter` uploads
- Changed Bedrock, Vertex and Mantle start-up model checks to identify themselves as Claude Code, like other Claude Code requests
- Changed `claude mcp get` to hide the command, arguments, and environment values of stdio MCP servers provided by plugins; variable names are still shown
- Changed `/claude-api` so it can no longer be run from Remote Control clients
- Changed `/config chrome=true` to direct you to the /config panel instead of enabling Claude in Chrome by default;`/config chrome=false` still turns it off when it was on
- Changed sandbox settings so project settings cannot widen or turn off an admin-required sandbox, replace the proxy behind a managed deny list, extend a strict allowlist, or reopen managed read-denies
- [VSCode] Changed the Manage plugins dialog to ask before removing a marketplace or turning off a plugin that your project’s shared `.claude/settings.json` turns on
- [Cloud sessions] Changed `MCP_DISCOVERY_CACHE=1` , when set in your cloud environment’s variables rather than a settings file, to reuse your connectors’ tool lists after a session restart; other MCP servers are no longer cached and connect at startup

**Added**

- Added Claude Sonnet 5.5 (`claude-sonnet-5-5` ), now the default Sonnet model on the Anthropic API — 1M context, $2/$10 per Mtok with $0.20/Mtok cache reads
- Added a “Yes, but ask again next time” answer to auto mode’s prompt before a read outside the working directories, so you can allow that one read and still be asked about later ones
- Added dollar amounts to the Claude apps gateway spend limit in `/usage` and the status line (for example “$271.40 / $500.00 spent this month”) when the gateway runs this version or later; the status line’s`rate_limits.spend_limit` also gains`used_usd` ,`limit_usd` and`period`
- Added `effortSlider:decreaseEffort` ,`increaseEffort` and`toggleUltracode` keybinding actions, so the`/effort` slider’s arrow and Tab keys can be rebound in`keybindings.json`
- Added `/rate-limit-options` to`/help` and the command menu for claude.ai subscribers, so the usage-limit notices that mention it point to a command you can find
- Added `/mcp reconnect all` in the interactive terminal to retry every MCP server that failed to connect or needs authentication at once
- Added Claude apps gateway startup warnings when a managed policy’s `availableModels` is empty, or leaves out the model Claude Code starts on without setting`model` or`enforceAvailableModels`
- Added `auth: { google: {} }` for Claude apps gateway`telemetry.forward_to` destinations, so telemetry can be exported straight to Google Cloud’s OTLP endpoint using the gateway’s Google Cloud credentials
- Added certificate client authentication (`private_key_jwt` ) between the Claude apps gateway and its identity provider, for identity providers that issue certificate credentials instead of client secrets
- [VSCode] Added an optional time above each prompt and response, with a date line where the day changes (Claude Code: Show Message Timestamps setting, off by default)
- [VSCode] Added plugin load errors and notes to the Manage plugins rows, with a popup to disable, uninstall or copy the error
- [VSCode] Added an Ultracode on/off switch under the Effort slider, replacing the slider’s Ultracode stop; the model pill shows ”· Ultracode” at any effort level
- [Claude Tag] Added model family choices such as “Opus (latest)” for a thread, a channel default or your DM, so the choice follows the newest model in that family
- [Claude Tag] Added the spend that counts toward your organization-wide limit to the analytics spend projection chart, with how much of the limit is used

**Changed**

- Changed interactive terminal and VS Code sessions to start in auto mode when no permission mode is configured, on every plan and provider; `permissions.defaultMode` still overrides it
- Changed Ultracode into its own toggle in `/effort` (Tab, or`/effort ultracode [on|off]` ): it no longer forces xhigh effort and stays on at any effort level
- Changed retries after a dropped connection mid-response to share one budget with the rest of the request’s retries, so a failing request gives up sooner
- Changed the notice shown when a Sonnet model’s safeguards flag a message to explain why it happened and to offer editing and retrying
- Changed safety-related model switches in sessions that pin an Opus model with `ANTHROPIC_DEFAULT_OPUS_MODEL` or`modelOverrides` : on the Anthropic API, the API now picks the model to switch to for each kind of flag, not the pinned model
- Changed the non-interactive first turn to still wait up to 2s for connecting MCP servers named by `--allowedTools` or an`mcp_tool` hook, even when`CLAUDE_CODE_MCP_STARTUP_WAIT_MS` is`0`
- Changed `/recap` to decline with a short notice when it arrives relayed from a chat thread (your own included) or from a routine or webhook; typed in the terminal, the Claude apps, Remote Control,`-p` or an SDK host, it runs as before
- Changed `/artifacts` to show its filter tabs beside the title with one-word labels (All, Mine, Shared), using the same tab bar as`/config` and`/plugin`
- Changed artifact publishing to refuse a file on a network share (a `\\host\share` path or a`/net` automount) unless it is on a mapped network drive added with`--add-dir`
- [VSCode] Changed `CLAUDE_CONFIG_DIR` in the`claudeCode.environmentVariables` setting to apply only when it is an absolute path, and passed it to terminals that continue the chat
- [Claude Tag] Changed Claude to post its private sign-in notice at every @mention from someone who hasn’t connected their Claude account, instead of going quiet after the first

**Added**

- Added `x-claude-code-prompt-id` to the gateway hint headers so LLM gateways can group the requests that serve one user prompt; opt in with`CLAUDE_CODE_GATEWAY_HINT_HEADERS=1`
- Added `availableModelsMatch` managed setting: with`"exact"` , an`availableModels` entry allows only the model version it names, so new releases stay blocked until listed
- Added `deniedModels` managed setting to block specific models, even when`availableModels` allows them
- Added MCP tool, WebFetch and WebSearch outputs to the `tool.output` OpenTelemetry span event when`OTEL_LOG_TOOL_CONTENT=1`
- Added `/doctor prompt-audit` (also`/checkup prompt-audit` ) to audit your CLAUDE.md files, skills, agents and commands for prompting patterns written for older models
- Added click-to-expand for truncated messages from your other sessions in fullscreen mode
- Added `path` to`--plugin-dir` load-failure entries in the stream-json`system/init``plugin_errors` , naming the directory that did not load
- Added an opt-in `load_test_mode` block to the Claude apps gateway config: requests are built and signed but not sent upstream, and clients get a canned reply, so a deployment can be load tested
- Added a `mantle` upstream provider to the Claude apps gateway for Amazon Bedrock’s Mantle endpoint
- [Claude Tag] Added a “Channels Claude can search” admin setting that limits Claude’s Slack search to public channels it has been added to, set per organization, workspace or channel
- [Claude Tag] Added a Back to Slack button on the page shown after connecting your Claude account, returning you to the thread you started from

**Changed**

- Changed interactive sessions on third-party providers or with telemetry off to start in auto mode when no permission mode is configured; `permissions.defaultMode` still overrides it
- Changed the `/ultrareview` launch dialog to say that reviewing a local branch may upload uncommitted changes to tracked files
- Changed the `/model` picker’s Opus row and the Default model’s name to drop “(1M context)” where Opus already has a 1M context window; the window is unchanged
- Changed prompt suggestions in the terminal to appear less often after 20 in a row go unused; using one brings them back
- Changed `--system-prompt` and`--append-system-prompt` to accept their text and`-file` forms together; the file’s text comes first
- Changed `Skill(anthropic-skills:<name>)` deny rules to also block that skill when Claude Desktop delivers it as a plugin, and`Skill(skill:<name>)` denies to match the skill’s alias and display name
- Changed `/rewind` and`/diff` lists to move on the same keybinding actions as every other list (`select:*` );`messageSelector:*` /`diff:*` rebinds still work
- Changed the `/workflows` run list to size itself like other lists: half the terminal inline, and it keeps its title on screen when the prompt shows below
- Changed `claude plugin eval` to require git 2.31 or later when git is installed; a run on an older git is refused with a message naming the version
- Changed artifact watching: a watch that was armed automatically (not one you asked for) now ends after 3.5 hours with no activity; publishing or watching the artifact again re-arms it
- Self-hosted runner: Changed lifecycle hooks’ git to skip a repository’s Git LFS `pre-push` hook, ignore a writable system`core.hooksPath` , and not sign commits without`--configure-git`
- Self-hosted runner: Changed `GIT_SSL_CAINFO` and`GIT_SSL_NO_VERIFY` under Anthropic-managed git: the runner’s own git always verifies Anthropic’s git route, and warning lines say what applies where
- Reverted the 2.1.282 reservation of the `claude-ai` name: skills, commands, workflows and MCP servers’ skills and prompts so named load again, and`Skill(claude-ai:*)` rules are ordinary prefix rules
- [Cloud sessions] Changed new routine schedules to default to a few minutes past the hour, with a note that routines set exactly on the hour can start several minutes late

**Added**

- Added a `maxProseWidth` setting that caps the width of Claude’s prose in wide terminals while tables and code blocks keep the full width
- Added a startup notice, and `/status` and`claude doctor` entries, listing telemetry variables in a project’s settings files that were ignored or that turned telemetry off
- Added the `allowClaudeInChromeWithManagedMcp` managed setting to let`claude --chrome` run alongside an exclusive`managed-mcp.json` ; the error shown when Chrome is blocked now names it
- Added `store.readiness_grace_seconds` to the Claude apps gateway so`/readyz` can stay ready through a short Postgres outage such as a database failover
- Added a scrollbar to the `/feedback` drafts list in fullscreen mode; it appears while the mouse is over the list
- [Cloud sessions] Added Claude GitHub App status to Settings › Connectors › GitHub: whether the app is installed and reachable for your account, plus steps to connect, install or reconnect
- [Cloud sessions] Added “Open repository” and “Open compare page” links to the repository menu of a cloud session whose repository is hosted on a Git server other than GitHub
- [Cloud sessions] Added attaching a repository from a different GitHub owner, such as a fork’s upstream, to a running cloud session that already has one, including sessions started from Slack

**Changed**

- Updated the `claude-api` skill: pre-output refusal billing now links to the How refusals are billed docs, mid-stream refusals bill at normal rates, and pre-output refusals count against rate limits
- Updated the `claude-api` skill to recommend`ant apply` for keeping Managed Agents resources as version-controlled files
- Changed auto mode to use the server-side classifier by default on a direct Anthropic API connection when telemetry is off (`CLAUDE_CODE_AUTO_MODE_SERVER=0` opts out)
- Changed `sandbox.excludedCommands` to ignore project and local settings entries when managed settings or`--settings` set`allowUnsandboxedCommands: false` , or managed`allowManagedDomainsOnly: true`
- Changed project and local settings to ignore OpenTelemetry variables that turn on export, set its endpoint, or capture content, like `CLAUDE_CODE_ENABLE_TELEMETRY` and`OTEL_LOG_*`
- Changed Windows/WSL managed settings so an admin policy that is present but invalid or unreadable (HKLM, `managed-settings.json` ) keeps user-writable HKCU and WSL`/etc/claude-code` from applying
- Changed `Skill(anthropic-skills:*)` and`Skill(claude-ai:*)` allow rules to cover only skills synced from claude.ai, not plugins or other skills that merely use such a name
- Changed skill folders, command files and workflow commands in the `anthropic-skills` or`claude-ai` namespace to no longer load; a plugin so named still loads but yields name ties to synced skills
- Changed MCP servers configured under the name `anthropic-skills` or`claude-ai` to list no skills or prompts (their tools still work); rename the server in your MCP configuration to list them again
- Changed the `ultracode` visuals in`/effort` and the prompt input to plain styling (no ripple, border flourish or keyword glimmer) and removed the dynamic-workflows spinner tip
- Changed the Clawd mascot’s feet in the start-up banner to sit under the corners of his body
- [Claude Tag] Changed the bordered cards Claude uses in Slack replies for plans, tables and details to render wide by default instead of a narrow width
- [Claude Tag] Changed newly connected Slack workspaces to follow the current default model instead of keeping whichever model was the default when they were connected

**Added**

- Added Claude apps gateway support for newer Claude Desktop keys in `desktop` policy blocks, including`blockReadsOutsideWorkingDirectories` and`disableBypassPermissionsMode`
- Added `assume_role` on Claude apps gateway Bedrock upstreams: the gateway calls Bedrock as an IAM role it assumes through STS, in another AWS account if needed, optionally one session per developer
- Added `guardrail: {id, version}` on Claude apps gateway Bedrock upstreams to apply an Amazon Bedrock guardrail to every request sent through them (set it on all Bedrock upstreams or none)
- Added `telemetry.resource_attributes` to the Claude apps gateway config, to put fixed labels on the telemetry of Claude Desktop and`/login` sessions
- Added `"attribution": false` in`settings.json` to hide all commit and PR attribution; older CLI versions skip a settings file that holds it, so keep the object form in files shared across versions
- Added MCP URL-mode elicitation on 2026-07-28 protocol connections, so servers can ask Claude Code to open a browser-based flow; no waiting dialog is left on screen when the server has no way to confirm completion
- Added MCP server checks to `claude plugin validate` : it reports`.mcp.json` entries that would be silently dropped at load, undeclared`${user_config.*}` references, and insecure URLs
- Added an auto mode recommendation to `/insights` that estimates how many permission prompts auto mode could have handled in your recent sessions
- Added a scrollbar to the `/skills` ,`/mcp` and`/plugin` Installed lists in fullscreen mode, like the one`/workflows` now has: it appears while the mouse is over the list and can be clicked or dragged
- [VSCode] Added a Continue/Stop prompt in the VS Code and JetBrains panels when auto mode falls back to billed classifier requests, replacing the unanswerable warning line
- [Claude Code on the web] Added a Fast mode switch to the composer’s model menu in cloud sessions, shown when your plan includes fast mode and the selected model supports it
- [Claude Code on the web] Added a settings shortcut on the GitHub setup tip and a “Troubleshoot GitHub connection” link in the repository pickers, both opening your GitHub connection page
- [Claude Tag] Added a short line in the Slack thread after someone presses Stop, naming who stopped Claude’s response and saying to mention @Claude to continue

**Changed**

- Changed send now (ctrl+enter or ctrl+x ctrl+s) to move running tools to the background instead of cancelling the turn
- Changed auto mode so that, where its classifier review runs server-side, read-only and sandboxed shell commands also wait for that review and are blocked when it flags them
- Changed `CLAUDE_CODE_AUTO_MODE_SERVER` to also apply on a direct Anthropic API connection:`0` opts out of the server-side auto mode classifier (the local classifier then counts toward usage),`1` opts in
- Changed the dangerous `rm` prompt in`--dangerously-skip-permissions` and auto mode to wait 2 minutes for an answer, then deny the command with a rewrite hint so unattended sessions keep going (`CLAUDE_CODE_DISABLE_DANGEROUS_RM_TIMEOUT=1` turns this off)
- Changed AGENTS.md support to also work on Amazon Bedrock, Google Vertex AI, Microsoft Foundry, LLM gateways, and sessions with telemetry disabled
- Changed Claude apps gateway to refuse to start when a `managedMcpServers` entry’s`envHelper` path starts with`\??\` or`/??/` , a path form current Claude Desktop refuses to run
- Changed self-hosted runners to pass system prompts to Claude Code as private files instead of command-line text, so large prompts no longer fail the launch; a wrapper or `command` hook that appends`--system-prompt` or`--append-system-prompt` must switch to`--system-prompt-file` or`--append-system-prompt-file`
- Changed queued messages to show in the conversation above the spinner instead of under it
- Changed the session artifact links under the prompt into one footer pill (`⧉ name` or`⧉ N` ) that opens`/artifacts` , which now lists this session’s artifacts first
- Changed the Artifact tool to let Claude load scripts from unpkg.com in artifact pages
- Changed hovering a list row in fullscreen mode, including in `/config` , to tint the row instead of drawing a second ❯ pointer beside the focused row’s
- Changed /mcp: each server’s row now starts with its status icon and name, says its state once, and in a narrow terminal drops trailing facts like “managed” before shortening the name
- Changed /workflows: each run’s row leads with its status icon and elapsed time, and a narrow terminal keeps the run’s name and time, dropping the agent and token counts first
- Changed Remote Control attachment downloads to reuse connections and to skip files already downloaded in the session
- Changed MCP resource lists (the resource list tool and @-mention suggestions) to skip MCP Apps UI resources; reading one by URI still works
- Changed `claude plugin uninstall --json` and the /plugin dialog to say a plugin’s data was kept when its folder stays because another installed plugin uses it or install records cannot be read
- Changed the background tasks list (`/tasks` ): pressing`x` on a running`/ultrareview` now asks for confirmation before stopping the review
- Removed the leftover “(removed)” `/agents` entry from the command menu and`/help` ; typing`/agents` still explains where the wizard went
- [Claude Tag] Changed the routine list Claude gives when asked in a Slack thread to show that thread’s own scheduled tasks by default instead of every routine in the channel

**Added**

- Added Claude Opus 5.5 (`claude-opus-5-5` ), now the default Opus model — 1M context, $4/$20 per Mtok with $0.20/Mtok cache reads
- Added mouse support to more lists in fullscreen mode: the wheel scrolls the `/skills` list, and a skill’s state options in`/plugin` can be clicked
- Added `CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH` to change the 2,048-character cap on MCP tool descriptions and server instructions for every MCP server in the session
- Added hook output sizes and the number of oversized outputs saved to a file to the `hook_execution_complete` OpenTelemetry event
- [VSCode] Added a Status dialog, with a typed `/status` , showing the session’s version, account, model and server details
- [VSCode] Added a Sandbox dialog for the sandbox mode, the unsandboxed fallback and excluded commands, opened from the panel menu or by typing `/sandbox`
- [VSCode] Added a Claude in Chrome dialog (extension status, the install, reconnect and permissions pages, the enabled-by-default setting), opened from the panel menu or by typing `/chrome`
- [VSCode] Added Export conversation, with a typed `/export` , to copy or save the conversation as plain text
- [VSCode] Added each skill’s source, token estimate and on/off state to the Slash commands dialog, with a click to change the state, and a typed `/skills` that opens it
- [VSCode] Added a typed `/plan` that switches to plan mode, sends a first planning prompt, or shows the session’s plan
- [Claude Tag] Added Slack’s native Working indicator, Stop button and thread title to Claude’s threads in channels; the indicator stays up until Claude finishes, and Stop interrupts the task
- [Claude Tag] Added a short notice in the Slack channel when a guest joining, or the last guest leaving, changes how Claude responds there under a Restrict or Channel only guest setting

**Changed**

- Reverted `ctrl+l` /`cmd+k` in fullscreen mode clearing the transcript view (added in 2.1.260); they redraw the screen again
- Changed the default model on Pro and Team Standard plans from Sonnet to Opus, matching Max, Team Premium, and Enterprise
- Changed an effort level saved before `/effort` became per-model to no longer apply to newly released models such as Opus 5.5; they start at their default until you pick a level
- Changed Opus 4.7, Opus 4.8 and Fable 5 to stop holding their launch-default effort over `/effort` in`-p` or the Agent SDK, a project, managed or`--settings``effortLevel` , or a per-model level
- Changed `/autocompact` ’s footer hint to name ←/→, the keys that adjust other ordered values
- Changed `/fast` ’s footer to name Space as the toggle key
- Self-hosted runner: Changed git in lifecycle hooks to ignore hook folders and programs named in the runner’s shared git files; local-path and `git://` remotes there now need`GIT_ALLOW_PROTOCOL`
- Changed plugin marketplaces whose name imitates a reserved marketplace name to be refused when added, and to stop loading if one was already added
- Changed `PermissionRequest` hooks: an agent-type hook no longer runs there, since its answer could never allow or deny the request; it now shows an error pointing to command or http hooks
- [VSCode] Changed “Open in New Tab” to open Claude beside the editor group you are working in rather than after the last group
- [Claude Code on the web] Changed the admin Routines on/off setting to live under Admin settings → Capabilities → Remote sessions; the Claude Code admin page now links to it
- [Claude Code on the web] Removed the empty repository picker shown when starting a session on a self-hosted environment in an organization where an admin has turned GitHub off

- Changed auto mode for Claude API and Enterprise users, and on Bedrock, Vertex, Foundry and gateways, to default to the server-side classifier, which does not charge for classifier overhead (`CLAUDE_CODE_AUTO_MODE_SERVER=0` opts out on Bedrock, Vertex, Foundry and gateways); warns on billed fallback. See https://code.claude.com/docs/en/auto-mode-classifier-billing
- Added an `Auto mode server` row to`/status` showing whether this session’s auto mode classifier runs on the server

**Added**

- Added AGENTS.md support: in a project with no CLAUDE.md, Claude Code reads AGENTS.md instead; change it under “Project instructions” in `/config`
- Added `CLAUDE_GATEWAY_PROXY_IS_EGRESS_BOUNDARY=1` for Claude apps gateways whose only egress is a forward proxy: every outbound request hands the proxy the hostname instead of resolving it locally
- Added an optional `headers:` map on Claude apps gateway upstreams, to send static headers to a proxy you run in front of a provider
- Added a line saying a background task’s update is waiting when it finishes while a panel such as `/tasks` is open
- [VSCode] Added a Sign out row to the panel menu, with `/logout` in the typed command menu
- [VSCode] Added background shells and other running tasks to the agent map, each with a Stop, and a typed `/tasks` that opens it
- [VSCode] Added a Copy response button on responses and a typed `/copy`
- [VSCode] Added a one-time notice when inactive sessions are archived automatically, and an “Unarchive all” action on the Archived sessions group
- [VSCode] Added the session’s cost and token usage to the Account & usage dialog and the session manager where plan limits do not apply (Vertex, Bedrock, Foundry, API key)
- [Claude Code on the web] Added Personal and Organization sections to the environment picker on Team and Enterprise plans, and admins can now share a personal environment with the organization

**Changed**

- Changed Fable to always appear in `/model` on the Anthropic API; it is greyed out only when your organization’s settings disable it
- Changed the Bash sandbox instructions on Bedrock, Vertex and Foundry to the first-party wording, which frames the sandbox as the boundary of what the task was given
- Changed `/ultrareview` in non-interactive sessions to refuse when the repository has no base branch or shared history
- Changed subagent results to reach the main agent under a header marking them as subagent output, with the result indented, so text in a subagent’s result cannot pass as the session’s own instructions
- Changed workflow scripts’ computed `agent()` prompts on Bedrock, Vertex and Foundry to reach the subagent framed as script-authored text, so the safety classifier does not read them as the user
- Removed the background Haiku auto-title request from `claude -p` runs launched outside an SDK or IDE
- Removed the deprecated TaskOutput tool; Claude reads a background task’s output file with Read instead, and the `taskOutputMaxChars` setting and`TASK_MAX_OUTPUT_LENGTH` no longer have any effect
- [Claude Code on the web] Changed organization environments to open as a read-only summary from the Code tab on Team and Enterprise plans, with editing under Admin settings → Cloud environments
- [Claude Code on the web] Changed the admin Claude Code setting labeled “Web” to “Cloud sessions” and removed the redundant read-only Mobile row beneath it
- [Claude Tag] Changed the Pylon credential preset in Claude Tag access bundles so admins can point it at Pylon’s EU host

- Fixed every request failing with `400 … Input tag 'advisor_20260301'` when`ANTHROPIC_BASE_URL` points at a proxy or gateway (2.1.275 regression)

**Added**

- Added the signed-in account to Claude apps gateway sign-in: when the gateway names it, you confirm it before the credential is saved, and `/status` shows it
- Added a send-now key (ctrl+enter, or ctrl+x ctrl+s) that interrupts the current turn and sends all queued messages at once; sent and queued messages show in gray until the model receives them
- Added a startup warning when a configured `otelHeadersHelper` fails, so sessions that silently export no telemetry are noticed
- Added syncing of the skills and plugins enabled on your claude.ai account to terminal sessions signed in with it; opt out with `syncClaudeAiSkills: false` or`syncClaudeAiPlugins: false`
- Added `/plugin install <plugin> --marketplace <source>` , which offers to add the marketplace before installing the plugin
- [VSCode] Added viewing, editing and deleting a saved memory inside the Memory dialog
- [VSCode] Added sending an attached image without typing any text
- [VSCode] Added a Retry link to the MCP servers dialog when the server list fails to load
- [VSCode] Added accept and reject buttons under each change in the proposed-change diff tab, so an edit can be reviewed change by change
- [Claude Code on the web] Added a “New routine” button to the page shown when a routine link no longer resolves, next to the link back to your routines list
- [Claude Tag] Added attach conditions for access bundles in Claude Tag settings: an Owner can let a bundle also apply in channels with guests or Slack Connect channels, not just member-only
- [Claude Tag] Added Amazon CloudWatch, CloudWatch Logs, Amazon SNS, Google Cloud Monitoring and Cloud Logging presets to an access bundle’s Credentials tab in Claude Tag admin settings
- [Claude Tag] Added Datadog presets for the US3, AP1, AP2 and US1-FED sites; new Datadog connections are now limited to Datadog’s read and query API routes

**Changed**

- Updated `/logout` for Claude apps gateway sign-ins to also end the session on gateways that advertise token revocation
- Changed hosted sessions to keep an unanswered permission prompt up after a container restart, instead of asking again
- Changed the Artifact tool to ask for a one-word tab icon on a first publish instead of an emoji favicon
- Changed Claude in Chrome in auto mode to skip the extension’s per-site check for classifier-approved calls, as bypass mode does, fixing `browser_batch` “Permission denied” after a redirect
- Changed plugins installed from an npm source to be fetched with `npm pack --ignore-scripts` and integrity-verified, so a package’s install scripts no longer run
- Changed scheduled and Run now routine runs to save data to, and republish the page of, an artifact you can edit without asking; public artifacts, first publishes and deletes still ask
- Removed the startup notice that told you a one-off scheduled routine had run since your last session
- [VSCode] Changed New session in a Claude editor tab to open in the sidebar when Preferred Location is set to Sidebar, instead of always opening another tab
- [VSCode] Changed a message sent while Claude is working to wait at the bottom of the conversation until Claude starts on it

**Added**

- Added a visible warning when memory usage is critical, with steps to free memory or restart safely
- Added `CLAUDE_CODE_MCP_STARTUP_WAIT_MS` to bound how long the first non-interactive turn waits for connecting MCP servers (`0` = don’t wait)
- Added `effort` attribute to the`claude_code.llm_request` OpenTelemetry trace span, matching the`api_request` event
- Added `claude_code.managed_settings_resolved` OTel event: managed-settings sources and policy helper state; redacted settings and digests with`OTEL_LOG_MANAGED_SETTINGS=1`
- Added `store.connect_timeout_seconds` to the Claude apps gateway config to lengthen the Postgres connect timeout (default 5 seconds), and improved the boot error when the database is unreachable to point to`store.postgres_url` and the configured timeout
- Added `enduser.sub` , the IdP subject, to the telemetry Claude Desktop and Cowork send through a Claude apps gateway
- Added a Claude apps gateway warning when a replica has more requests open than the 256 it sends upstream at once, and a startup log line showing that limit
- Added click-to-expand for collapsed teammate and agent messages in fullscreen mode
- [VSCode] Added continuation of the step a window reload interrupted, labeled in the chat, with a Claude Code: Continue After Reload setting to turn it off
- [VSCode] Added Memory and Instructions entries to the Customize menu: Memory shows the auto-memory toggles, the saved memories and the memory folders, and Instructions edits the CLAUDE.md files
- [VSCode] Added a `claudeCode.lockEditorGroups` setting to stop Claude from locking the editor groups it opens in
- [Claude Code on the web] Added a “Compare against” branch picker to a cloud session’s diff view, so you can diff its changes against any branch instead of only the base branch
- [Claude Tag] Added a Guests setting to the Add channel and Add workspace forms in Claude Tag admin settings, so owners can pick Inherit, Allow, Channel only or Restrict up front

**Changed**

- Changed Bedrock, Vertex, Foundry and telemetry-disabled installs to use the v2 MCP client and MCP 2026-07-28 negotiation with direct HTTP servers by default, as other installs already do (opt out: `MCP_SDK_GENERATION=v1` or`MCP_PROTOCOL_NEGOTIATION=legacy` )
- Changed `/code-review` to use leaner inline review prompts for every model that has no tuned settings of its own, instead of spawning many review subagents
- Changed `"type": "sdk"` MCP entries in`.mcp.json` , settings, plugins and agent files to be skipped with a warning: only an SDK host application can register in-process servers
- Changed artifact watching in local sessions: a new version published elsewhere no longer starts a turn; Claude learns of it from a later Artifact tool result
- Changed plugin and marketplace clones to leave Git LFS files as pointers instead of downloading them; `git lfs pull` in the checkout fetches them
- Changed self-hosted runners to skip a read-only repository the git host refuses at the access check instead of failing the session start
- Changed the `/status` GitHub line to read “Cloud sessions”, and`/web-setup` ,`/ultrareview` , and teleport messages to say “cloud session” instead of “Claude Code on the web”
- [VSCode] Changed the default global gitignore file to `$XDG_CONFIG_HOME/git/ignore` when`XDG_CONFIG_HOME` is an absolute path
- [Claude Code on the web] Changed routines to skip a run and retry for up to 72 hours when the owner’s GitHub connection is missing, instead of switching the routine off at the first failed check
- [Claude Code on the web] Changed a routine’s on-hold notice: when your subscription is paused it now tells you to turn the routine back on yourself instead of promising an automatic resume
- [Claude Tag] Removed the repeated guest-attribution note Claude appended to a Slack canvas each time it edited one in a channel using the “Channel only” guest setting

**Added**

- Added `x-claude-code-request-class` ,`x-claude-code-agent-type` ,`x-claude-code-prev-tool-durations` ,`x-claude-code-compaction` and`x-claude-code-context-compacted` request headers for LLM gateways; opt in with`CLAUDE_CODE_GATEWAY_HINT_HEADERS=1`
- Added a notification when an MCP server disconnects mid-session and automatic reconnection gives up, pointing at `/mcp`
- Added forking a session started with `claude --remote-control` or`/remote-control` from the Claude app; the fork runs as a background session on your computer
- [Claude Code on the web] Added a “Discard unsaved changes?” confirmation before the New routine page or the Edit routine dialog throws away a routine name, prompt or edit you typed

**Changed**

- Reverted a 2.1.268 change that checked Read and Edit deny rules on Bash lines the permission checker can’t analyze (`eval` ,`env -C` ); commands like`time -p make build` prompt again instead of being denied
- Changed auto mode on Bedrock, Vertex and Foundry to use the local classifier by default for now; set `CLAUDE_CODE_AUTO_MODE_SERVER=1` to use the platform’s server-side classifier
- Changed `OTEL_LOG_TOOL_DETAILS=1` to also include real agent, skill, plugin and MCP server names on cost and token metrics
- Changed sign-in with a Claude account to also request access to your claude.ai plugins
- Changed `/bug` and`/feedback` reports to include only model-behavior params (model, system prompt, tools) from the last API request, omitting request metadata and`CLAUDE_CODE_EXTRA_BODY` fields
- [Claude Code on the web] Changed the admin “Share cloud sessions” setting to live under Data and privacy instead of the Claude Code page, where Data and privacy admins can also manage it
- [Claude Code on the web] Removed the full-page desktop-app download screen that new users without a cloud environment saw on Mac and Windows; they now go straight to setup
- [Claude Tag] Changed Claude to start watching related public channels on its own, such as an incident channel a conversation depends on, instead of only when asked

**Added**

- Added fast mode in Claude Code Remote sessions (cloud and self-hosted runners): the host’s fast-mode setting or `/fast` typed in the session applies where your organization allows it
- Added mouse support to the `/config` panel in fullscreen mode: the wheel scrolls the settings list, a click on a setting’s value changes it, and the row under the pointer is highlighted
- Added `claude self-hosted-runner --drain-marker-file <path>` : when that file exists at a SIGTERM drain, the runner reports its exit to the server as a host drain (telemetry only)
- Added per-command `allowed_domains` to Bash, PowerShell and Monitor in auto mode with sandboxing: the hosts a command needs are reviewed with it and opened for it alone; other hosts are refused
- Added `omitClaudeMd` to agent frontmatter and`--agents` JSON, letting custom and plugin subagents run without user, project and local CLAUDE.md files; managed policy files still load
- Added `--accept-command <sha256>` to`claude plugin install` and`claude plugin update` to accept exactly the command a previous`--json` run displayed, instead of`-y`
- Added support for a `multiplier` above 1, up to 10, in the`modelPricing` managed setting and the Claude apps gateway`pricing` block, for marked-up internal chargeback rates
- Added a spinner tip pointing Bedrock, Vertex AI, Foundry and LLM gateway users to the Claude desktop app; the claude.ai desktop app tip now suggests `/desktop` , which offers to download the app
- [VSCode] Added an Attach Open File setting that, when turned off, stops the open file from being added to messages; selected text is still attached
- [Claude Code on the web] Added a Custom network access option to the Cloud environments editor in admin settings, with the same allowed-domains list the environment dialog on claude.ai/code offers

**Changed**

- Changed auto mode so that a skill’s or slash command’s inline `!` shell commands follow default-mode permission rules instead of the classifier; a command no rule decides runs as a reviewed tool call
- Changed auto mode so a subagent reports back to its caller through a dedicated hand-back call that the safety classifier reviews, instead of its last message being reviewed after the fact
- Changed Monitor watches to always have a deadline (at most 30 minutes; 10 in single-prompt `-p` runs) and notify Claude to re-arm, replacing the no-timeout`persistent` option
- Changed the IDE selection indicator in the prompt to a `[⧉ …]` pill that wraps with the text instead of squeezing multi-line prompts; delete it with Backspace to leave the selection out
- Changed the default dynamic workflow size to small on Pro plans and lowered the medium size guideline from 15 to 10 agents
- Changed Claude apps gateway, Bedrock, Vertex AI, and Foundry sessions so that they no longer refresh a leftover claude.ai login that the session does not use
- Updated the bundled `claude-api` skill to enable`eager_input_streaming` on streaming custom tools, and to start deliverable-shaped Managed Agents work with`user.define_outcome`
- [VSCode] Changed the on state of toggle switches from Claude orange to the editor theme’s button color
- [Claude Code on the web] Changed the Routines page on claude.ai/code to a new layout with Yours and Templates tabs and two-column routine cards that show run status, and removed its calendar view

- Fixed read-only git commands in Bash unexpectedly asking for permission after a session had been running for a while (regression in 2.1.269)

**Added**

- Added `claude plugin eval` : run a plugin’s eval suite against Claude Code and get scored, reproducible results (JSON + HTML report); see`claude plugin eval --help`
- Added `/output-style [name]` to list and switch output styles, including over Remote Control and in cloud and other headless sessions
- Added a diff of the files a Bash command changed to the Bash tool result when the Bash tool handles file edits (setting `bashEditDiffEnabled` )
- Added `OTEL_METRICS_INCLUDE_REPOSITORY` to tag OpenTelemetry metrics and events with`vcs.*` repository attributes; commit events get`vcs.ref.head.*` with`OTEL_LOG_TOOL_DETAILS`
- Added `CLAUDE_CODE_GATEWAY_MODEL_DISCOVERY_TIMEOUT_MS` to extend the LLM gateway`/v1/models` discovery timeout (default 3s)
- Added a spinner tip suggesting `/focus` for a view with just your prompt, a one-line work summary, and the response
- Added `CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS` (1–256) to raise the Workflow tool’s per-run concurrent agent limit for inference-bound fan-outs
- [VSCode] Added an agent map: an “N agents” footer pill opens a map of the session’s sub-agents with per-agent cards, Stop agent, and read-only transcripts
- [VSCode] Added a Hooks dialog to the command menu for viewing hooks and adding, editing, or removing them in user, project, and local settings; managed, plugin, and session hooks stay read-only
- [VSCode] Added live progress rows for running subagents under the tool-call groups in Focus view
- [VSCode] Added a Permission rules dialog that lists permission rules and adds or removes them in user, project, and local settings; startup-option, session-only, and managed rules stay read-only
- [VSCode] Added a Cancel button to the Switch account screen that returns to your session as the current account
- [Claude Code on the web] Added taking back a queued message in a cloud session before Claude reads it: remove it from the queue, or press Esc or Up, and the text returns to the message box
- [Claude Tag] Added a confirmation dialog before Connect all or Disconnect on a GitHub installation in admin settings, to guard against accidental organization-wide changes

**Changed**

- Changed `/ultrareview --post` to post the PR comment directly when the findings arrive and print the comment link, instead of starting a second cloud session to post it
- Changed artifact database reads that save into the session scratchpad so they no longer stop for working-folder approval
- Changed skills synced from claude.ai in cloud sessions to be named `anthropic-skills:<name>` , matching Claude Desktop; the bare name still works when nothing else uses it
- [VSCode] Changed the current-file chip in the message box: an X now removes it, replacing the Hide toggle
- [VSCode] Removed the Claude Code items from a session tab’s right-click menu and the editor title bar’s ”…” menu; they could not act on the tab the menu was opened on
- [Claude Code on the web] Changed the Cloud environments admin page to list every environment instead of capping each table at five rows behind a Show more control that could be unreachable
- [Claude Code on the web] Changed claude.ai/code for Free-plan users to open the plans page with a path to upgrade, instead of a “Disabled by org admin” page with no way forward

**Added**

- Added to the Claude apps gateway: with `pricing:` set in`gateway.yaml` , signed-in Claude Code clients receive the same rates through managed settings, so`/cost` and telemetry match the spend meter
- Added a startup warning for gateways when `access_control.allow_cidrs` is empty, and a one-time warning the first time a request arrives from a public address
- Added the `gatewayInternalNetworks` managed setting, letting administrators allow`/login` to a Claude apps gateway on their organization’s own public IPv4 block
- Added `claude self-hosted-runner --remove-session-state` (default off): delete each session’s per-session directories under`<base-dir>/_sessions/` when the session ends
- Added `configDirectory` to the output of`claude auth status --json`
- Added `--json` to`claude plugin install` ,`uninstall` ,`update` ,`enable` and`disable` , and`errorDetails` /`noteDetails` to each row of`claude plugin list --json`
- Added browser-tab icons for published artifacts, chosen by Claude to match each page
- [VSCode] Added Left and Right arrow keys to change where an always-allow permission rule is saved, for keyboard and screen reader users
- [VSCode] Added a “Claude Code: Focus last message” command that moves keyboard focus to the newest message in the conversation, for keyboard and screen reader users
- [Claude Tag] Added a link from a Slack channel’s configure page back to the organization’s Claude in Slack admin settings
- [Code Review] Added a note under the still-open findings list in follow-up reviews: resolving a finding’s thread, not just replying to it, stops later reviews from counting it as open

**Changed**

- Changed the system prompt on Bedrock, Vertex and Foundry to deliver environment, model and settings details as attachments, matching first-party sessions
- Changed Bedrock, Vertex and Foundry sessions to keep the tool list byte-stable across a conversation (late-connecting tools load deferred instead of rewriting it), matching first-party sessions
- Changed the task-tracking tools (TaskCreate/Get/Update/List, TodoWrite) to be offered only on Claude 3.x, Opus 4.0–4.7, Sonnet 4.0–4.6, Haiku 4.5; set `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` elsewhere
- Changed the artifact data-edit permission prompt in the terminal to a card that shows the document count and who can open the artifact
- Changed local Cowork sessions set to skip all approvals: the Artifact tool now refuses a local file outside the session’s folders, or behind a symlink, instead of reading it without asking
- Changed plain `WebFetch` deny and ask rules to no longer apply to Artifact tool reads and updates; use an`Artifact` rule (or`WebFetch(domain:claude.ai)` ) to block or gate them
- Changed the “N MCP servers need authentication” startup notice to announce each server once instead of at every launch
- [VSCode] Changed the Manage plugins dialog to apply installs, enables, disables and uninstalls to open sessions without a restart
- [VSCode] Changed some artifact permission prompts to omit the “don’t ask again” choice, matching the terminal
- [Claude Tag] Changed memory in public channels: each channel now keeps its own notes, and Claude no longer recalls notes it saved in other public channels; workspace notes stay shared

**Added**

- Added `maxEffortLevel` setting (top-level or per model under`modelSettings` ): caps the effort level on every provider, including Bedrock, Vertex and Foundry; users can still pick a lower level
- Added `--system-prompt-snapshot off` to render the system prompt fresh on every request instead of reusing the conversation’s recorded prompt (for iterating on prompt text)
- [Claude Tag] Added a “Use a custom connector” link to the preset connection forms in Claude Tag admin settings, so you can switch to a custom connection without starting over

**Changed**

- Self-hosted runner: Changed `--use-anthropic-git-proxy` to be reported to the server at registration and to print a warning for each session that still clones through the legacy git proxy
- Gateway: Changed `forward_user_identity` upstreams to return a 429 as-is to a developer whose email was forwarded, instead of failing over to the next upstream, so the proxy’s per-user limits hold

- Fixed a 2.1.265 regression affecting LLM-gateway and proxy setups: the undocumented `CLAUDE_CODE_USE_GATEWAY` environment variable, previously ignored unless`ANTHROPIC_BASE_URL` and`ANTHROPIC_AUTH_TOKEN` were both set, began forcing Cloud-gateway sign-in on its own in 2.1.265, so configurations that set it alongside an API key,`apiKeyHelper` , or custom auth headers failed every request with “Not signed in to the Cloud gateway”. The variable on its own is ignored again; no configuration change is needed

**Added**

- Added `user.email` and`user.groups` to the telemetry Claude Desktop and Cowork send through a Claude apps gateway, matching terminal sessions
- Added support for pointing `--plugin-dir` at a folder of plugins: each child folder with a manifest loads, and children added or removed while running are picked up
- Added a 1 GB cap on tool results saved to disk; the in-conversation preview says when a saved file was truncated
- [VSCode] Added automatic archiving of sessions inactive for a set period (new “Archive inactive sessions” setting, default 14 days)

**Changed**

- Updated the `.claude` folder permission option to say what it actually allows: editing files in the project’s`.claude` folder (or`~/.claude` ) for the session
- Changed machines with `forceLoginGatewayUrl` in managed settings to be Claude apps gateway sessions from startup, like`forceLoginMethod: "gateway"` ; a leftover claude.ai login or API key is not used
- Changed image processing to use the runtime’s built-in image support; the CLI no longer extracts a native image module to the temp directory
- Changed plugin display metadata to prefer the marketplace entry over `plugin.json` on the Installed tab and`claude plugin details` , filling gaps from`plugin.json`
- Changed Claude apps gateway sessions to export OpenTelemetry directly to a collector the gateway’s managed settings name in `OTEL_EXPORTER_OTLP_ENDPOINT` , instead of through the gateway’s relay; sessions without a named collector still use the relay

**Added**

- Added an “Organization policy” line to `/status` and`claude doctor` that says why your organization’s policy could not be loaded, such as a proxy not passing the endpoint through
- Added `bashOutputMaxChars` and`taskOutputMaxChars` settings to raise how much command and background-task output Claude receives inline before it is saved to a file, up to 128K characters
- Added `--append-subagent-system-prompt-file` to read the subagent system prompt from a file, for prompts too large to pass on the command line
- Added `/skill-doctor` to show which loaded skills go unused and what they cost in context, so you can prune them
- [VSCode] Added a “Build a custom style” walkthrough to the Output styles menu that writes a custom output style file and lists it right away
- [VSCode] Added an Add server form and a Remove action to the MCP servers dialog, so MCP servers can be added and removed without leaving the IDE
- [VSCode] Added a hollow ring in the session list for sessions open in a terminal, another VS Code window, or Claude Desktop, so they no longer look closed
- [VSCode] Added a fold button to permission and question prompts so the conversation behind them can be read without dismissing them; the space beside the prompt now scrolls the conversation
- [VSCode] Added “Archive session” to the session list’s right-click menu and gave Unarchive its own icon

**Changed**

- Changed a Claude apps gateway 403 on the managed settings load (at startup or after `/login` ) to say Claude Code may not be enabled for the organization, instead of advising a new sign-in
- Changed machines whose managed settings pin `forceLoginMethod: "gateway"` to ignore a leftover API key or claude.ai login and ask for`/login` ; Bedrock, Vertex AI, and Foundry sessions are unaffected
- Changed auto mode to treat a link that packs content into a public diagram renderer’s URL as an upload to that site: no longer auto-approved unless you asked for it
- Changed the prompt’s word-editing keys to match Bash: Ctrl+W deletes back to whitespace, Alt+F and Alt+D stop at word end, punctuation separates words; `keybindingFlavor` no longer has any effect
- Changed `/context` token counting to use a local estimate when the token-counting API is unavailable, instead of extra small-model requests
- [VSCode] Changed the model picker to one flat list of every model, with rows kept for older model spellings listed last

**Added**

- Added a diff panel that opens beside the conversation in fullscreen mode and shows your uncommitted changes as Claude edits; toggle it with `/diff`
- Added a likely cause for prompt-cache misses (e.g. tool definitions or system prompt changed, idle past the TTL) to `/cost` and the status line’s`prompt_cache` field
- Added `/reload-plugins` to headless sessions, so it appears in the Claude Code Desktop and SDK command lists
- Added a text form of `/advisor` (`/advisor` ,`/advisor <model>` ,`/advisor off` ) for the desktop app, Remote Control, and other headless (`-p` /Agent SDK) sessions
- Added `oidc.scope_on_refresh` to the Claude apps gateway for IdPs that return an id_token on refresh only when asked for`openid` again
- Added Claude apps gateway support for newer Claude Desktop keys in `desktop` policy blocks, including`userPluginMarketplacesEnabled` and`userPluginUploadsEnabled`
- [VSCode] Added the selected effort level to the footer model pill, fixed a stale effort level after switching models, and returned the footer pills to their earlier compact size
- [VSCode] Added Open and Closed to the session list’s status filter menu

**Changed**

- Reverted the 2.1.259 change applying `Read()` deny rules to Bash arguments; it denied`npm run build` under a`Read(./**/build/**)` rule in every mode and made`cd … && grep` prompt even in auto mode
- Updated the bundled `claude-api` skill so its Go, Java, and C# samples use current-generation model IDs, and clarified that cheaper worker or sub-agent models should be current-generation too
- Changed `ctrl+l` /`cmd+k` in fullscreen mode to clear the transcript view like a terminal`clear` ; scroll up to see earlier messages
- Changed permission rules with text after the closing parenthesis (e.g. `Bash(ls) x` ), which never matched anything, to be reported as invalid settings instead of being silently ignored
- Changed server-managed settings so a managed CLAUDE.md (`claudeMd` ) no longer triggers the security approval dialog; hooks, shell-command, sandbox, and unsafe`env` settings still require approval
- Changed Claude in Chrome to follow your organization’s Claude in Chrome admin setting; when an admin turns it off, `--chrome` ,`/chrome` and the browser tools are unavailable
- Changed Claude apps gateway to send `orgPluginSettings` in the list form read by Claude Desktop 1.15200.0 and later; older desktops ignore it
- Changed Claude apps gateway to also refuse to start, naming the field, when a `desktop` policy misspells a field in a nested object of a`managedMcpServers` or`orgPluginSettings` entry
- Changed commands typed at the `!` bash-mode prompt to run outside the sandbox even when strict sandbox mode (`sandbox.allowUnsandboxedCommands: false` ) is on, like typing into your own terminal
- Changed self-hosted runner `--kill-session-after-min` to release a session that is only waiting on its user (paused, resumable on the next message) instead of killing it and reporting a failure
- Removed the one-hour time limit on background commands started by subagents; they now run until they exit or are stopped, matching the main session

**Added**

- Added `managedMcpServers` managed setting: organizations can provide HTTP/SSE MCP servers to every user (same entry shape as`.mcp.json` ); entries that name a command to run are skipped
- Added `--permission-prompts none` for unattended headless hosts: anything that would prompt is denied automatically while the active permission mode (including auto mode) keeps deciding
- Added recognition of `glab mr create/merge/close/reopen/note/update` so GitLab merge requests show as`MR !N` in the collapsed tool summary and refresh the footer MR badge
- Added `--json` to`claude plugin validate` for a machine-readable validation report
- [VSCode] Added an Active quick filter and a status filter menu (Needs input, Working, Completed) to the session list sidebar

**Changed**

- Changed `allowedMcpServers` to govern only servers users add: a literal`managed-mcp.json` server your allowlist used to filter out now loads on upgrade; use`deniedMcpServers` to keep it off

- Fixed Claude Code failing to launch on macOS 12 (Monterey), a regression introduced in 2.1.255
- Fixed remote and scheduled sessions failing with “user messages must have non-empty content” after a re-sent permission approval could not be applied
