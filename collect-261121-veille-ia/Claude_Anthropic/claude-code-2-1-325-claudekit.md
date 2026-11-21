---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-325-claudekit
title: "Claude Code 2.1.325 — ClaudeKit"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "commands", "sandbox", "sandboxed", "without", "code", "permission", "them"]
source: https://claudekit.io/en/updates/claude-code-2-1-325/
source_anchor: ""
source_lines: [1, 210]
section: "Claude_Anthropic"
---
## What changed

Claude Code 2.1.325 shipped on November 13, 2026. It has no headline feature; most of what matters is changed behavior. Sandboxed Bash commands now follow the normal permission flow by default, and read-only git commands in other repositories and some skills run by scheduled tasks now ask first. Much of the release’s 139 changes fix permission rules and the sandbox, sign-in for background sessions, and MCP tasks.

## New features

- 
**[VSCode] Prompt suggestions**Prompt suggestions now appear in the extension wherever the terminal shows them. Tab accepts the suggested next prompt, and `promptSuggestionEnabled: false` in Claude Code’s settings.json turns them off.
- 
**[VSCode] New sign-in options**A Console sign-in that creates no API key, and a field to paste the redirect URL when signing in to an MCP server from the panel in SSH, WSL and container windows.
- 
**New hints and notes**
  - When your organization’s policy blocks a request, a line under the error says how to continue: run `/rewind` and change the message, or run`/clear` .
  - Once Claude has read or edited a Jupyter notebook, a spinner tip in VS Code and similar integrated terminals covers re-running a notebook cell.
  - Claude gets a note when a sandboxed `git checkout` ,`git stash` or autostash pull cannot update protected files such as those in`.claude` .
- When your organization’s policy blocks a request, a line under the error says how to continue: run 

## Key improvements

- 
**Sandboxed Bash commands follow the permission flow (`sandbox.autoAllowBashIfSandboxed`)**Sandboxed Bash commands used to run without prompts. They now follow the normal permission flow by default. Set `sandbox.autoAllowBashIfSandboxed: true` to keep running them without prompts. If managed settings turn the sandbox on, set it in your highest-ranking managed source.
- 
**More things ask first**
  - Read-only git commands now ask first in a repository that isn’t the project’s or one you trusted. To run them without prompts, name the repository with `--add-dir` , not the`additionalDirectories` setting.
  - Scheduled tasks and `/loop` ask before running a skill that declares`allowed-tools` or hooks. Add`Skill(name)` to`permissions.allow` to run it unattended.
  - “Always allow” in Desktop, IDE and SDK hosts offers a rule alone only when saving it would allow that command again, so a scheduled task that relied on one can now wait for you.
  - `/login` asks for permission to change your privacy settings.
- Read-only git commands now ask first in a repository that isn’t the project’s or one you trusted. To run them without prompts, name the repository with 
- 
**Tighter sandbox and worktree isolation**
  - The sandbox refuses sandboxed writes to `.claude` settings, skills, agents, commands and hooks in added directories, as it does in the launch folder.
  - An isolated worktree session or agent is refused `git bisect run` ,`git maintenance register|start` and`scalar` ’s set-up commands, and is told what still works.
  - `EnterWorktree` refuses a`WorktreeCreate` hook path that goes through a symlink inside the project; have the hook print the real path.
  - Self-hosted runners refuse a session whose session folder or repository path is a symbolic link, other than a `checkout` hook’s checkout.
  - `-p` and SDK runs in a worktree or submodule mounted into a container apply`.claude/settings.local.json` allow rules only once the folder is trusted.
- The sandbox refuses sandboxed writes to 
- 
**Fewer programs get credentials**Programs that Claude Code’s sandbox starts for its own use, such as its network bridge, run without Claude Code’s sign-in tokens and API keys in their environment. ripgrep, local git commands and the sandbox runtime’s own helper programs run without the mTLS client key’s passphrase in their environment. The account email check now also refuses your email as a client’s identity in scripts a command runs, file writes, web fetches and MCP calls.
- 
**Plugin and marketplace changes**
  - Plugin MCP bundles (`.mcpb` ,`.dxt` ) unpack under the plugins directory, not`.mcpb-cache` in the plugin’s folder, so read-only plugin folders load. One fetched by URL downloads again once.
  - Plugins of a remote marketplace that managed settings declare, enable and list in `prependPlugins` or`appendPlugins` count as the organization’s.`pluginDigests` is deprecated and ignored.
  - Plugin loading skips, and `claude plugin validate` reports, some unsafe paths and links in a plugin’s folder.
  - `.claude/skills` folders in`--plugin-dir` plugins no longer load from a session started above them; start Claude Code in the plugin folder instead.
  - Marketplace `ref` accepts the full ID of any commit on a branch or tag. Clients before this version can’t clone such a pin, so a marketplace pinned this way stops loading there.
  - Plugin marketplace and plugin downloads use a CONNECT tunnel when `ALL_PROXY` is the only proxy set; a proxy that refuses CONNECT now fails them.
  - The add-marketplace confirmations, plugin recommendation prompts and plugin install warning say that plugins can run code on your computer, with access to your files and shell.
- Plugin MCP bundles (
- 
**Subagents cut off at the output token limit**Such a subagent used to return only the end of its answer. It is now asked for the whole answer again, more briefly.
- 
**`claude remote-control` exit codes**It now exits with status 4 when it stops by itself (single-session runs still exit 0) and 69 when it can’t register, so a service manager can start it again.
- 
**`CLAUDE_CODE_MAX_CONTEXT_TOKENS` covers more model IDs**It also sets the context window of unrecognized model IDs that start with `claude-` , such as a gateway’s`claude-sonnet-latest` .
- 
**Usage accounting and startup**
  - `/cost` ,`$.session.usage()` ,`--max-budget-usd` and the cost and token metrics count a mod’s`$.model.complete` and`$.model.classify` calls.
  - After 8 turns in a row started by background or scheduled tasks, prompt suggestions in the terminal refresh less often, saving usage until you press a key or return to the terminal.
  - With a keyless Console sign-in or other Anthropic profile login, a token that is still valid is read without waiting for the credentials lock.
  - `claude -p` with no cached account data waits for that data before its first request, so a first run in a clean environment can start a few seconds later.
- 
**Questions from MCP tasks**Esc on a background MCP task’s question now means “answer later”: nothing is sent to the server, and the question stays in `/tasks` . Claude is also told when a task is waiting for your answer, and can point you to`/tasks` .
- 
**Cloud and Remote Control changes**
  - Under Remote Control, `/resume` and`/teleport` disconnect first when the connected session has a share link, or when that can’t be checked; run`/remote-control` to connect again.
  - Claude Code on the web, cloud scheduled tasks and Cowork cloud sessions start despite `sandbox.failIfUnavailable` , since the hosted container has no command sandbox.
  - Requests for settings, permission rules, hooks or status over a cloud or self-hosted session’s connection return less or are refused; what applies in the session is unchanged.
  - Spend limit and shared budget errors in cloud sessions and under Remote Control no longer say “ask your admin to raise it”.
- Under Remote Control, 
- 
**Clearer messages**
  - The `HTTPS authentication failed` message for private plugin marketplaces says git’s terminal prompts and askpass programs are disabled, and it also appears with newer git versions.
  - The `claude --desktop` sign-in error names the API key, provider or setting that stops a Claude account sign-in from working.
  - The `/voice` “No audio detected” message on macOS and Windows says where to pick your microphone.
  - Messages shown when a background session can’t use its session’s API key or token no longer suggest adding the key to settings.
- The 
- 
**Other changes**
  - tmux and iTerm2 teammates, and new sessions from the agents view opened with ←, start in the session’s folder instead of where the shell last ran `cd` .
  - Claude’s instructions now say to keep files you’ll want out of its temporary scratchpad folder, which can be cleared.
  - The bundled dataviz skill gives better part-to-whole advice for static images and for parts with a natural order, such as age bands.
  - [VSCode] The note for an untrusted folder says why the extension is off and how to turn it on; the error shown when the system refuses to run Claude Code says your IT team can allow a blocked program; the sign-in card says what to do when the browser shows a connection error for localhost; and the install link’s marketplace confirmation button reads “Trust marketplace and continue”.
- tmux and iTerm2 teammates, and new sessions from the agents view opened with ←, start in the session’s folder instead of where the shell last ran 

## Bug fixes

### Permissions, security and sandbox

- **A security issue in how zstd-compressed responses are decoded.**
- **A gap in Bash permission checks where some compound commands could run without asking** — these now ask for permission.
- **A Bash allow rule that could run a different program than the one it names** , and**some deny and ask rules not applying to certain Bash commands** .
- **Relative deny and ask permission rules (e.g. `Read(./.env)`) no longer covering the project after a `cd`** — they now also apply in added directories.
- **Deny and ask rules starting with `mcp__plugin_*_` missing a plugin’s MCP server or tool spelled in another letter case.**
- **Deny rules not being applied to a permission approval that arrives after Claude Code restarts** , in self-hosted runner and`claude remote-control` sessions.
- **A slash command’s `allowed-tools` grants applying to background agents woken during or after its turn.**
- **A committed `.claude/settings.local.json` that could be treated as the user’s own** when Claude Code was started in certain subfolders of a repository.
- **A managed `strictPluginOnlyCustomization` lock on agents and skills not taking effect in sessions that were already running.**
- **A session sometimes going back to an earlier permission mode after a subagent’s plan was approved.**
- **A connector tool’s approval prompt in cloud sessions being answered automatically while plugins were still loading.**
- **Sandboxed commands being able to write to the native updater’s download folder in some cases** , and**the Bash sandbox not fully protecting a worktree’s settings and hooks, or a linked worktree’s shared git folder** .
- **`/feedback` and `/bug` uploading other sessions** — they now upload them only when they were recorded by the signed-in account or organization.
- **Linux: sandboxed Bash commands failing with `bwrap: Can't create file`** when the working directory was below an`allowRead` folder inside a`denyRead` , such as under a symlinked home directory.

### Artifacts

- **Claude asking for permission before reading your own artifact or its data.**
- **Deny and ask permission rules and hooks for the Artifact tool not applying in some cloud sessions.**
- **Some subagents changing an artifact that people outside your organization can see, or uploading files to it, without asking first** when an allow rule is set or uploads were approved earlier.
- **Some artifact publishes skipping the confirmation for pages that people outside your organization can see** when “don’t ask again” had been chosen for Artifact.
- **Auto mode judging only the first change to an artifact that people outside your organization can see** — its yes now covers that change alone.

### Sign-in and background sessions

- **Background sessions started on your claude.ai login using (and billing) an API key or token from the shell that started the background service.**
- **More cases of a session moving to another sign-in in the background** — these moves are now refused, with a message saying what works.
- **A second Enter in the exit dialog stopping background work after the move to the background was refused.**
- **“Sign in with your Console account” in `/login` replacing your existing login when you pressed Esc right after the browser step** , and**the browser showing the sign-in success page in some cases where sign-in failed or was canceled** .
- **`claude auth status` and other short commands leaving a lock behind on a Console profile** , which made the next session hang for about a minute, and**leftover `.oauth_refresh.lock.owner.tmp.*` files piling up in the config directory** .
- **AWS `credential_process` helpers and Google Cloud default credentials failing when Claude Code is started without a `HOME` environment variable.**
- **Token counting on Amazon Bedrock sending the conversation to Bedrock when the `allowedProviders` managed setting does not allow it.**
- **`/fast` saying “Fast mode has been disabled by your organization” under the keyless Console sign-in** , where Claude Code had not asked the server, and**`/model` saying that your organization restricts a model when Claude Code could not check that** , for example with no network.
- **Claude Desktop’s third-party deployments staying on “Stopping” until a token renewal finished** (once it includes this version).

### Models, subagents and conversations

- **Subagents with their own model being moved to the plan-mode model when the model setting is `opusplan` or `haiku`.**
- **Subagents on a different model sizing file reads and MCP results with the main session’s model.**
- **Sessions on many third-party gateways and Foundry stopping with a context length error on every turn instead of compacting.**
- **A saved `sonnet[1m]` model setting being rewritten on a new install to a model id the API rejects.**
- **Recent turns sometimes going missing from a resumed session after Claude Code stopped without a clean exit.**
- **Shell commands running in the launch directory after the working directory was deleted** — the command now fails with a message and can be re-issued.
- **An approved plan edit being lost when the `plansDirectory` folder can’t take it** — the plan is now saved in the default plans folder, and a warning says where and why.
- **Brief-mode sessions, such as Dispatch, sending a reply again after the turn had already replied.**
- **Sessions showing as waiting for input when a finished pull request only needed your own review or merge.**
- **One invalid value in an Agent SDK `applyFlagSettings()` call silently discarding all settings from earlier calls** — a call that would do this is now refused with an error.
- **An unrecognized `teammateMode` settings value being ignored without a warning** —`claude -p` , the startup warning and`claude doctor` now list it as a skipped entry (still not applied).

### MCP

- **`/mcp` still listing a plugin’s MCP server after a plugin reload removed it while it was connecting.**
- **An MCP server staying “needs authentication”, and its `mcp_tool` hooks being skipped, after a token refresh that was refused only for a moment.**
- **MCP tool calls from an SDK host answering “Not connected” after a tool call had reconnected the MCP server** , and**MCP elicitation requests being cancelled without asking in headless mode after a server’s connection was replaced during a tool call** .
- **An MCP task getting stuck after its question was closed with Esc or asked again** — the footer now says it needs your input, and Enter in`/tasks` shows the question again.
- **Background MCP tasks that stopped updating after another call to the same server lost the connection** , when a later call reconnects.
- **A background MCP task’s question opening over a message you were still typing** — it now waits until you send or clear the draft, and the spinner and turn timer keep running.
- **Claude Desktop and SDK sessions listing a folder’s own MCP servers (`.mcp.json`, local) as host-added or not at all, and not connecting WebSocket ones** , after a folder move or pre-warmed start.
- **Claude saying a working Atlassian, Asana or Sentry connector had failed in cloud sessions** when a plugin listed it at a different URL.

### Code Mode

- **Code Mode scripts ignoring a worktree entered with `EnterWorktree`, writing `~` paths into the project, and leaving `/mcp` showing an MCP server as connected after its sign-in expired.**
- **Words typed with a Yes at a permission dialog or plan approval not reaching Claude in Code Mode.**
- **Claude starting work by itself when you resumed a session that ended during a Code Mode script** — it now waits for your next prompt.

### Worktrees and git

- **`/security-review` and skills’ inline git commands stopping with an error** in worktrees of bare repositories,`--separate-git-dir` checkouts and Android`repo` checkouts.
- **`worktree.sparsePaths` and sparse plugin and marketplace clones leaving out the repository’s root files on older git versions** , such as 2.34.
- **`--worktree` skipping a `WorktreeCreate` hook from a plugin in `CLAUDE_CODE_PLUGIN_SEED_DIR` on the first run in a new home directory.**
- **Self-hosted runner sessions with several repositories being unable to use a repository added mid-session.**
- **`/teleport`’s stash choice in some unusual git states** — it now stops and says what to do.

### Display and commands

- **Long options in lists and permission prompts being partly overwritten when they wrap** , and**first-run screens losing lines on narrow terminals** .
- **Startup warnings about permission rules and the “N MCP servers need authentication” notice being recorded as seen when they were scrolled out of view.**
- **The welcome header showing control characters or splitting across lines** when an agent’s name or the working folder contains them or a line break.
- **The red error shown when Claude tries a command only you can run** — a dim line now says it runs only when you type it at the prompt.
- **Focus view hiding the reply to your prompt when a scheduled task replied after it** , and**the prompt sometimes missing from a background subagent’s view when fullscreen is off** .
- **`/stats` showing an Active days total that was one day too high.**
- **`/insights` saying a report was ready when no session qualified for analysis, and writing two values into its HTML report without escaping them.**
- **`/voice` saying voice mode changed when managed settings, `--settings`, or the project’s `.claude/settings.json` or `.claude/settings.local.json` kept it as it was** — it now says what holds it.
- **The IDE welcome dialog showing Cmd+Esc for Quick Launch on Windows and Linux** , and**Slack app tips showing when your organization’s policy denies cloud sessions** .
- **`claude purge` hanging silently when there is no terminal to read keys from** , and its project picker and`-i` prompts ignoring the keyboard when stdin is piped.
- **`claude plugin eval --output-dir ''` writing the report into the current folder** — it now exits with an error.
- **`claude -p` text output on a terminal passing control sequences other than text styling** — pipes and files are unchanged.

### OpenTelemetry

- **A missing OpenTelemetry `user_prompt` event for a message sent while tool calls run** — the event has`delivery.mode: "mid_turn"` and the running turn’s`prompt.id` .
- **The `user_prompt` event leaving out earlier messages’ text** when queued messages merged into one turn in VS Code, or when one message had an image.

### Windows

- **A start-up stall that could last about a second when a PATH entry sits under a folder your account cannot open.**
- **`/resume` refusing or hiding sessions whose folder was recorded with a different drive-letter case or slashes** , and**an in-session `/resume` dropping a project’s saved trust** when the session was recorded with a different drive-letter case.

### VS Code

- **The model picker’s check mark staying on the old model** after Claude Code restarted on another model or the model was switched over Remote Control.
- **The effort selector offering levels above the effort cap set by your organization or the `maxEffortLevel` setting** , and**the sign-in screen offering sign-ins that the managed `allowedProviders` setting refuses** .
- **The permission prompt for MCP tools not saying why Claude Code asks when a safety check raises the question.**
- **The message for organization settings that couldn’t load saying to run a command again** — it now says to try again, matching the tab’s Retry link.

### Claude Code on the web and Claude Tag

- **New cloud sessions cutting MCP tool descriptions loaded through tool search at 4,096 characters** — the limit is now 16,384, as in other Claude Code sessions.
- **[Claude Tag] Artifact links pasted in Slack getting no card** when the poster’s Claude sign-in had expired in a workspace their organization doesn’t manage; a plain card now appears.

## Notes

- **If you relied on sandboxed Bash commands running without prompts** , they now follow the normal permission flow. Set`sandbox.autoAllowBashIfSandboxed: true` to keep the old behavior; if managed settings turn the sandbox on, set it in your highest-ranking managed source.
- **If scheduled tasks or `/loop` run a skill that declares `allowed-tools` or hooks** , add`Skill(name)` to`permissions.allow` to run it unattended. With the “Always allow” change in Desktop, IDE and SDK hosts, a scheduled task that relied on such a rule can now wait for you.
- **To run read-only git commands without prompts in a repository outside the project** , name it with`--add-dir` , not the`additionalDirectories` setting.
- **If you pin a marketplace `ref` to a full commit ID** , clients before this version can’t clone it and the marketplace stops loading there.`pluginDigests` is now ignored.
- **If `ALL_PROXY` is your only proxy setting** , plugin marketplace and plugin downloads now use a CONNECT tunnel and fail through a proxy that refuses CONNECT.
- **If a service manager runs `claude remote-control`** , it can restart on exit status 4 (stopped by itself) and 69 (can’t register).
- **If you develop plugins with `--plugin-dir`** , start Claude Code in the plugin folder for its`.claude/skills` to load, and have`WorktreeCreate` hooks print the real path rather than one through a symlink inside the project.
- **A security issue in how zstd-compressed responses are decoded is fixed.** The release notes give no further detail.
- This post covers the main items only. See the GitHub release notes for the full list.
