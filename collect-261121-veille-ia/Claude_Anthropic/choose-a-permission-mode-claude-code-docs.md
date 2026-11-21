---
id: collect-261121-veille-ia/claude-anthropic/choose-a-permission-mode-claude-code-docs
title: "Choose a permission mode - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "mode", "code", "default", "permission", "doesnt", "settings", "manual"]
source: https://code.claude.com/docs/en/permission-modes?refid
source_anchor: ""
source_lines: [1, 506]
section: "Claude_Anthropic"
---
`default`.
`manual` as an alias for `default` wherever you type the value, for example `claude --permission-mode manual` or `"defaultMode": "manual"`.
Each mode has its own rule for writes to protected paths.
Modes set the baseline. Layer permission rules on top to pre-approve or block specific tools. Deny rules block in every mode, including `bypassPermissions`. Deny and ask rules don’t apply to `EndConversation` as long as Claude still has at least one other tool it can call. Allow rules have no effect in `bypassPermissions`.
Claude Code doesn’t auto-approve the following in any mode, including `bypassPermissions`, though you can turn off the prompt for dangerous `rm` commands in `bypassPermissions` mode. Each bullet links to the section that says what happens instead in each mode:
- Tools matched by an explicit ask rule
- 
Connector tools your organization set to `ask` , in sessions where that setting reaches Claude Code
- 
Tools that require user interaction: the built-in `AskUserQuestion` tool and MCP tools marked`requiresUserInteraction`
- 
Recognized `rm` and`rmdir` critical-path removals, which no allow rule or`PreToolUse` hook`"allow"` approves, and Bash commands flagged`Dangerous rm operation not ruled out`
- The cross-session messaging safeguards
- 
Reads outside the working directories while `permissions.blockReadsOutsideWorkingDirectories` is on: recognized file-reading Bash commands prompt even in auto mode and`bypassPermissions` mode, and so does any unsandboxed retry that needs approval to run outside the sandbox. Requires Claude Code v2.1.257 or later.
A command the shell parser can’t trace, such as one that changes directory more than once or runs a subshell, prompts the same way even when it names no outside path. This prompt doesn’t apply when the command runs in the sandbox and the sandbox enforces the block.
- 
Writes outside the working directories while `permissions.blockWritesOutsideWorkingDirectories` is on: Claude Code refuses`Write` ,`Edit` , and`NotebookEdit` calls there, apart from some of its own files. The key doesn’t limit shell commands, MCP tools, or the programs hooks run, so use sandboxing to limit where shell commands write. Requires Claude Code v2.1.314 or later.

`could not fully check your permission rules` either. Approve the prompt if you trust the command. If Claude Code denies the command, ask Claude to reword it.
Permission modes decide whether Claude asks before an action, and the Bash sandbox and outer isolation boundaries decide what an action can reach once it runs. Each row below pairs a goal with the flags or settings that get you there and the isolation it needs, as a starting point. Available modes lists what runs without a prompt in each mode.
1. 
The `--permission-mode` flag, or`--dangerously-skip-permissions`
2. 
`permissions.defaultMode` in a settings file
If you set`"auto"` in`.claude/settings.json` or`.claude/settings.local.json` , the value doesn’t take effect, and Claude Code then uses the built-in default rather than a`defaultMode` from`~/.claude/settings.json` . If you set`"bypassPermissions"` in those two files, it doesn’t take effect either, and the session starts in Manual mode. The other values apply from any settings file.
3. The built-in default

`auto` default requires Claude Code v2.1.228 or later on macOS, Linux, and WSL, and v2.1.233 or later on native Windows. On earlier versions, the built-in default is Manual.
The built-in default depends on how you run Claude Code. The first row that matches your session applies. The table covers sessions you start in a terminal or through the VS Code extension; for the desktop app and claude.ai, see the Desktop and Web tabs in Switch permission modes.
`auto` but auto mode isn’t available to the session, Claude Code starts the session in Manual instead. Auto mode is unavailable when the session doesn’t meet the availability requirements, such as a settings file turning it off or a model that doesn’t support it, or when Anthropic has temporarily turned it off server-side.
The first time the built-in default starts one of your sessions in auto mode, Claude Code shows a notice that links to this page:
- In a terminal, once, at the top of the session
- In the VS Code extension, as a card on the new-conversation screen that stays until you dismiss it

`~/.claude/settings.json` sets a `defaultMode` other than `auto` and no other settings file sets one, your sessions keep starting in that mode.
On Pro, Max, Ultra, and Team plans, and in sessions that don’t fetch feature flags, Claude Code asks once whether to change the setting to auto mode. The question appears in the terminal or in the VS Code extension, and if you decline, your setting stays as it is.
In Claude Code v2.1.302 or later, `claude -p` and Agent SDK runs with no permission mode set start in Manual mode. This mode appears as `default` in SDK options and settings. Calls needing approval are denied unless you approve them with an allow rule, hook, `canUseTool` callback, or `--permission-prompt-tool`.
To get these calls approved, add allow rules or start in auto mode:
- **Add allow rules** : Claude runs the calls you list. Pass`--allowedTools "Bash(npm test)"` to`claude -p` , or set your SDK’s allowed-tools option.
- **Start in auto mode** : a classifier model reviews calls, so you list nothing beforehand. Pass`--permission-mode auto` to`claude -p` , or set your SDK’s permission mode option.

- CLI
- TypeScript
- Python

`DISABLE_TELEMETRY`.
You can set the starting permission mode for one session, or as a default for every session on a machine, in a project, or in an organization. When more than one settings file sets `permissions.defaultMode`, settings precedence decides, so a project or managed value outranks `~/.claude/settings.json`. To change the permission mode of a session that’s already running, see Switch permission modes.
`default`. Save it in `~/.claude/settings.json`:
`⏸ manual mode on` in the status bar.
If your organization has the HIPAA configuration applied, terminal and VS Code sessions on Claude Code v2.1.326 or later start in auto mode by default. On v2.1.285 through v2.1.325, an eligible session starts in Manual mode by default.
To keep starting in Manual mode, see Start in a different permission mode. To turn off auto mode or `bypassPermissions` for your organization:
- **Remove auto mode** : set`permissions.disableAutoMode` to`"disable"` in managed settings
- **Block `bypassPermissions`** : set`permissions.disableBypassPermissionsMode` to`"disable"` in managed settings

- CLI
- VS Code
- JetBrains
- Desktop
- Web and mobile

**During a session**: press

`Shift+Tab` to cycle permission modes. From `auto`, the first press switches to `default`, and the cycle then runs `default` → `acceptEdits` → `plan`. Optional modes slot in after `plan`. The status bar shows the active mode as a gray `⏸ manual mode on` for `default`, or as `⏵⏵ accept edits on`, `⏸ plan mode on`, `⏵⏵ auto mode on`, `⏵⏵ don't ask on`, or `⏵⏵ bypass permissions on`.Watch the status bar in this clip of a session that started in auto mode. Each time you press `Shift+Tab`, it changes from `auto mode on` to `manual mode on`, `accept edits on`, `plan mode on`, and back to `auto mode on`.
- `auto` : appears when auto mode is available; cycling to it switches permission modes without a confirmation prompt
- `bypassPermissions` : appears after you start with`--permission-mode bypassPermissions` ,`--dangerously-skip-permissions` ,`--allow-dangerously-skip-permissions` , or`permissions.defaultMode: "bypassPermissions"` in user,`--settings` , or managed settings. The`--allow-` variant adds the permission mode to the cycle without activating it
- `dontAsk` : never appears in the cycle; set it with`--permission-mode dontAsk`

`plan`, with `bypassPermissions` first and `auto` last. If you have both enabled, you will cycle through `bypassPermissions` on the way to `auto`.
**From a Bash permission prompt**: in the Manual and

`acceptEdits` permission modes, when auto mode is available, Claude Code adds **Yes, and switch to auto mode**to a Bash command’s permission prompt. Select it to approve the command and switch the session to auto mode. PowerShell tool prompts don’t offer the option. Requires Claude Code v2.1.247 or later.Claude Code doesn’t add the option to prompts forced by one of your

`ask` rules or by a hook, because auto mode still shows you those prompts, so switching wouldn’t remove them.
**At startup**: pass the permission mode as a flag.

**As a default**: set

`permissions.defaultMode` at the scope you want, as described in Start in a different permission mode.The same `--permission-mode` flag works with `-p` for non-interactive runs.`acceptEdits` mode lets Claude create and edit files in your working directory without prompting. The status bar shows `⏵⏵ accept edits on` while this mode is active.
In addition to file edits, `acceptEdits` mode auto-approves common filesystem Bash commands: `mkdir`, `touch`, `rm`, `rmdir`, `mv`, `cp`, and `sed`. Except for `touch`, these commands are also auto-approved when prefixed with safe environment variables such as `LANG=C` or `NO_COLOR=1`, or process wrappers such as `timeout`, `nice`, or `nohup`. Like file edits, auto-approval applies only to paths inside your working directory or `additionalDirectories`.
These still prompt:
- Paths outside that scope
- Writes to protected paths
- Recognized `rm` and`rmdir` critical-path removals
- `touch` with anything beyond plain file paths, for example an option, a glob, or a prefix such as`timeout 5` or`LANG=C`
- All other Bash commands except the built-in read-only set

`acceptEdits` mode also auto-approves `Set-Content`, `Add-Content`, `Clear-Content`, and `Remove-Item` on in-scope paths, along with their common aliases. The same scope and protected-path rules apply, and `Remove-Item` gets its own check. A positional argument that contains a quote character, such as the apostrophe in `Set-Content .\notes.txt "It's done"`, still prompts even on in-scope paths, because Claude Code can’t statically validate an argument whose quoted and unquoted readings differ. Pass the content through a named parameter such as `-Value` to avoid the prompt.
Use `acceptEdits` when you want to review changes in your editor or via `git diff` after the fact rather than approving each edit inline.
Press `Shift+Tab` once from Manual mode to enter it, or start with it directly:
- **Interactive terminal sessions with bypass permissions available** : neither the classifier nor a prompt applies to planning commands. Skip all checks with bypassPermissions mode covers the few things that still prompt there.
- **Auto mode available and the `useAutoModeDuringPlan` setting on** , which it is by default: the classifier reviews shell commands other than critical-path removals instead of prompting you. Approved commands run, and rejected ones are blocked.
- **Auto mode not available, or `useAutoModeDuringPlan` off** : commands outside the built-in read-only set prompt for approval, including when the sandbox’s auto-allow mode is enabled.

`Shift+Tab` or prefixing a single prompt with `/plan`. You can also start in plan mode from the CLI:
`Shift+Tab` again to leave plan mode. At the approval prompt, press `Esc` first, since pressing `Shift+Tab` there approves the plan.
When the plan is ready, Claude presents it and asks how to proceed. From that prompt you can choose:
- **Yes, and use auto mode** : approve and start in auto mode. If auto mode isn’t available to your session, for example because your organization turned it off, this option reads**Yes, auto-accept edits** . If you started the session with bypass permissions enabled, the option reads**Yes, and switch to BYPASS PERMISSIONS (no further prompts) for this session** instead.
- **Yes, manually approve edits** : approve and review each edit individually.
- **No, keep planning** : stay in plan mode and tell Claude what to change.

`Shift+Tab`, or prefix your next prompt with `/plan`.
Press `Ctrl+G` to open the proposed plan in your default text editor and edit it directly before Claude proceeds. When `showClearContextOnPlanAccept` is enabled, the list gains a first option that approves the plan and clears the planning context.
Accepting a plan also gives the session a generated title based on the plan, unless you’ve already named the session.
To make plan mode the default for a project’s terminal sessions, set `defaultMode` to `plan` in `.claude/settings.json`, placed as the example under Start in a different permission mode shows. Conversations the VS Code extension starts don’t read project settings for the starting permission mode. There, set `claudeCode.initialPermissionMode` to `plan` in your VS Code user settings instead.
Auto mode lets Claude execute without routine permission prompts. A separate classifier model reviews actions before they run, blocking anything that escalates beyond your request, targets unrecognized infrastructure, or appears driven by hostile content Claude read. Explicit ask rules still force a prompt.
With Claude Code v2.1.283 or later, auto mode is the built-in starting permission mode for interactive terminal and VS Code sessions on every plan and provider. On earlier versions, it’s the built-in starting permission mode only on Pro, Max, Ultra, and Team plans.
By default, the classifier doesn’t review recognized `rm` and `rmdir` critical-path removals, such as `rm -rf /` or `rm -rf ~`. Critical paths covers what happens to them in each permission mode.
Auto mode also nudges Claude to keep working without stopping for clarifying questions, though Claude still asks when your prompt or a skill explicitly relies on it. For stronger autonomous behavior in a mode that still prompts you, set the Proactive output style instead.
Auto mode is available only when your account meets all of these requirements:
- 
**Plan** : All plans.
- 
**Organization** : on Team and Enterprise, auto mode is available by default. Administrators can turn it off for the organization by setting`permissions.disableAutoMode` to`"disable"` in managed settings.
- 
**Provider and model** : available by default on these providers, with these models:
  - **Anthropic API, Claude Platform on AWS, and Claude Models on Google Cloud (Anthropic managed)** : Claude Opus 4.6 or later, Sonnet 4.6 or later, Haiku 5.5, or a Fable model
  - **Amazon Bedrock, Google Cloud’s Agent Platform, Microsoft Foundry, and signed-in Claude apps gateway sessions** : only Claude Sonnet 5 or later, Opus 4.7 or later, Haiku 5.5, and the Fable models

`disableAutoMode`. Anthropic may also have turned auto mode off server-side, or the server may have rejected auto mode for your account. A session that received either answer keeps auto mode off until the session ends, so start a new session later.
A separate message that names a model and says auto mode “cannot determine the safety” of an action means a classifier request failed. That failure is usually transient, but on Amazon Bedrock it can repeat until your account can invoke the named model. See the error reference for the causes and what to do.
If you set `defaultMode: "auto"` in settings and a terminal session starts in Manual mode with no error, the setting is likely in `.claude/settings.json` or `.claude/settings.local.json`. `auto` doesn’t take effect from those files. Move it to `~/.claude/settings.json`. For a conversation the VS Code extension started, check the extension’s own list in Switch permission modes instead.
On Amazon Bedrock, Google Cloud’s Agent Platform, Microsoft Foundry, and signed-in Claude apps gateway sessions, auto mode is available by default. If you don’t set a permission mode there, interactive sessions start in auto mode, and `claude -p` or Agent SDK runs start in Manual mode, whose config value is `default`:
- **Interactive terminal and VS Code sessions** : to start them in a permission mode other than auto, set`permissions.defaultMode` as Start in a different permission mode describes, or pick one from the VS Code extension’s mode indicator. Which mode a session starts in lists the Claude Code versions that start them in auto mode.
- **`claude -p` and Agent SDK runs** : to start one in auto mode, pass`--permission-mode auto` to`claude -p` , or pass`permissionMode: "auto"` in TypeScript or`permission_mode="auto"` in Python. If you pass no permission mode, see Tool calls denied in`claude -p` and Agent SDK runs, which also covers earlier Claude Code versions.

`disableAutoMode` to `"disable"` in managed settings. This removes `auto` from the `Shift+Tab` cycle, and a session started with `--permission-mode auto` starts in Manual instead. A session already running in auto mode leaves it when the setting reaches that session from an admin-deployed source, and shows `auto mode disabled by settings`. Before v2.1.251, a running session kept auto mode until it ended.
In auto mode, Claude Code can ask the server to check the actions that the decision order sends for review, as part of the session’s model requests, in place of sending its own classifier requests. These sessions ask:
- **A direct connection to the Anthropic API** : in interactive terminal sessions and in`-p` , Agent SDK, VS Code extension, and desktop app sessions, whatever your plan or account type, as Anthropic rolls it out. Check your session type against these minimum Claude Code versions:
  - In interactive terminal sessions, asking the server requires Claude Code v2.1.271 or later on Pro, Max, Ultra, and Team plans. On Enterprise plans and Claude API accounts, those sessions require v2.1.278 or later.
  - In `-p` and VS Code extension sessions, asking the server requires Claude Code v2.1.278 or later
  - In Agent SDK and desktop app sessions, asking the server requires Claude Code v2.1.281 or later
  - From v2.1.282, a session that doesn’t fetch feature flags, for example because you turned telemetry off, asks the server by default in any kind of session
- **A cloud provider, or an LLM gateway or proxy** : on Claude Platform on AWS, Amazon Bedrock, Google Cloud’s Agent Platform, and Microsoft Foundry, and whenever you point`ANTHROPIC_BASE_URL` at an LLM gateway or proxy, whatever your plan. Asking by default requires Claude Code v2.1.278 or later.
- **A signed-in Claude apps gateway session** : requires Claude Code v2.1.280 or later
- **A cloud session in an Anthropic-hosted environment** : whatever your plan, when you start it from the web, the mobile app, or the desktop app with**Cloud** selected. Sessions started by a cloud scheduled task ask the server as Anthropic rolls it out.

- 
**The server doesn’t review the session** : a response completes with no review results, or the server answers that it doesn’t review this session. The most common causes are an LLM gateway or proxy that drops the request for review or the results, and a platform, region, or credential that doesn’t have server-side checks yet. Claude Code falls back to its own classifier requests. Once that fallback holds for the rest of the session, it shows a notice about classifier request charges on accounts where those requests are billed.
- 
**The server gives no verdict for an action** : Claude Code denies the action rather than run it unreviewed. On any connection, this happens when the response ends before the review results arrive or the results arrive in a form Claude Code can’t read. An LLM gateway or proxy that cuts responses short or rewrites the results can cause either. On a direct connection to the Anthropic API, it also happens when the server’s check fails for the action, for example by timing out. The server returned no safety verdict covers the denial message, what happens when denials repeat, and what to do.
If a hook or mod changes an action’s input after the server’s review, Claude Code can deny the action with no verdict. In interactive terminal and VS Code sessions on a direct connection to the Anthropic API, the denial’s text includes`a hook changed this call's input after auto mode reviewed it` . Its error entry covers what to do.

`CLAUDE_CODE_AUTO_MODE_SERVER=0`. On a direct connection to the Anthropic API, the variable requires Claude Code v2.1.281 or later. Setting it to `1` there turns server review on in a session that doesn’t have it yet, unless you’ve also set `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1`. If you set `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` and leave `CLAUDE_CODE_AUTO_MODE_SERVER` unset, Claude Code also stops asking the server, except as Disable pre-release capabilities describes.
By default, classifier requests go to Claude Sonnet 5. If `/status` shows `Auto mode server` as `Enabled`, they’re sent only for calls outside the server’s review.
Find the first row in this table that matches your session. You get that row’s model in place of Sonnet 5.
`ANTHROPIC_DEFAULT_SONNET_MODEL` is unavailable from your provider or excluded by your organization’s model policy, Claude Code uses a fallback model. That’s your session’s model, or an Opus model in a Fable session.
With Claude Code v2.1.302 or later, set `CLAUDE_CODE_AUTO_MODE_MODEL` to a full model ID to run the classifier on a model other than the default. Your cloud provider’s own full model IDs work too. This `~/.claude/settings.json` example sets Claude Sonnet 4.6:
`--debug-file`:
`curl https://example.com`, then search the log for `classifier_request_started`:
- **Your model after `model=`** : the variable took effect
- **A different model after `model=`** : search the log for`ignoring CLAUDE_CODE_AUTO_MODE_MODEL` , which gives the reason and any step to take
- **No such line** : read`Auto mode server` in`/status` at the Claude Code prompt:
  - **`Enabled`** : the server is set to review this session’s calls.
  - **`Disabled`** : this check hasn’t tested your model yet. Make sure the permission mode is auto and no permission rule matches`curl` , then run the check again with the sandbox off.

`git remote add` or `git remote set-url` isn’t trusted, and everything else is treated as external until you configure trusted infrastructure.
**Blocked by default**:

- 
Downloading and executing code, like `curl | bash`
- Sending sensitive data to external endpoints
- Production deploys and migrations
- Mass deletion on cloud storage
- Granting IAM or repo permissions
- Modifying shared infrastructure
- Irreversibly destroying files that existed before the session
- Force push
- Committing or pushing a change that would send secrets or sensitive data outside the repository when it runs, or widen what a deploy exposes. This covers a CI workflow or deploy configuration that passes a secret to a destination that doesn’t already receive it, a script or setup step that reads a secret store and sends the data out, and a config change that widens what a deploy publishes, such as a registry, visibility, artifact, or sourcemap setting. The check applies on any branch, applies even when the repository is public, and fires when the change is committed or pushed, whether or not that commit or push triggers the pipeline; clearing it requires naming the execution effect, not only the commit or push
- 
`git reset --hard` ,`git checkout -- .` ,`git restore .` ,`git clean -fd` ,`git stash drop` , or`git stash clear` , which the classifier presumes would discard uncommitted changes
- 
`git commit --amend` when the commit at HEAD was not created in this session
- 
From v2.1.198, `git commit --amend` when the commit at HEAD has already been pushed. A message-only reword is not blocked:`--amend -m` with nothing newly staged, on a commit that Claude created during this session
- 
`terraform destroy` ,`pulumi destroy` ,`cdk destroy` , or`terragrunt destroy` , and applying a plan that destroys resources
- Writing to a secret manager, or changing DNS records or TLS certificates
- Merging a pull request no human has approved, approving Claude’s own pull request, or disabling CI checks
- 
Posting a comment that is itself a command to automation, such as `atlantis apply` or a bot’s`/deploy` or`/merge`
- Toggling, ramping, or deleting a production feature flag
- Applying infrastructure changes to a protected IaC scope, or draining and removing cluster nodes
- 
Writes to a shared compute cluster that reach beyond the resource you named, such as a label selector or `--all` that catches other users’ jobs
- Creating Kubernetes resources that run on every node or intercept cluster traffic, such as DaemonSets and admission webhooks
- Interactive shells or port-forwards into a sensitive remote target
- Opening a tunnel or reverse shell that makes a local service reachable from the public internet
- Printing a live credential or token into the transcript or a file
- Accessing a location listed as a sensitive data location in your environment, copying data out of one, or sending data from one to an audience the entry excludes
- Routing a package install around your internal package registry to a public registry. This applies when an internal registry or mirror is listed in your environment or when you’ve told Claude in the conversation that one exists
- 
Running a command with a flag that disarms a safety guard, like `--insecure`
- 
Launching an autonomous agent loop that runs without human approval or a sandbox, such as one started with `--dangerously-skip-permissions` or`--no-sandbox` . This includes running a third-party agent or eval harness with isolation and per-action approval disabled, such as a runner started with`--yes-always`
- Claude in Chrome browser actions that could send page content, cookies, or credentials off-origin
- 
Deleting files in `/tmp` ,`$TMPDIR` , or another shared scratch or cache directory by wildcard, glob, or age filter rather than by a specific named path
- Including sensitive details in content sent, uploaded, published, or written to other people or shared systems, when your own message didn’t authorize those details for that recipient. PR and issue bodies, commit messages, and comments count as this kind of outbound content when the repository is outside the trust boundary or public, including your organization’s own public repositories; internal file paths, code names, live API response data such as emails or account identifiers, and infrastructure identifiers count as sensitive details. Live personal data from an API response in a PR or issue body, such as an email address, an account or organization identifier, or a usage metric, requires you to name those details and the recipient regardless of the repository’s visibility or trust boundary
- Sending keystrokes to Claude Code’s own tmux pane to drive its own interface, which the classifier treats as Claude changing its own permissions or oversight
- Commenting out, deleting, or force-passing a test or assertion that guards security behavior, such as auth, access control, input validation, or sandboxing
- Deleting or tearing down a stateful resource Claude didn’t create in the session, when no more specific deletion rule applies and you didn’t name that resource
- 
Repointing an API base URL, proxy endpoint, webhook receiver, or registry mirror at a third-party host that doesn’t fit the task, including in example files like `.env.example`
- 
Changing where pushes go with `git remote set-url` or`git remote add` , unless you named the new remote
- Pushing secrets or personal or entrusted data to a repository known to be public, or pushing confidential material there that isn’t part of that repository’s own work. A dotfiles repository’s own subject matter is the one exception for personal or entrusted data, and content from a private repository reaching any public surface is blocked the same way. When a repository’s visibility isn’t established, the classifier doesn’t block on that alone; it judges the content against the other rules instead
- 
Opening a pull request against a different repository or organization, forking with `gh repo fork` , or pushing to a third-party repository, unless you named that external target
- Content from a sensitive local store, or from a file whose name, path, or type marks it as sensitive, entering a commit, a push, PR or issue text, a gist or paste, or a package publish, unless you named both the source and the destination. Session transcripts and conversation logs, credential and configuration dot-folders such as SSH keys, cloud credentials, browser profiles, and shell history, and user-data exports all count, and the repository being private doesn’t clear it
- 
Writing to Claude Code session transcripts, the `.jsonl` history files under`~/.claude/projects/` or your configured config directory, whether directly or through a shell command. The rule also covers the metadata lines Claude Code appends to each transcript entry for its own checks. Reading a transcript isn’t blocked
- 
A recursive forced delete such as `rm -rf "$VAR"` or`Remove-Item -Recurse -Force $dir` whose target is a shell variable that isn’t assigned in the conversation the classifier sees, or a glob rooted at such a variable. Without that assignment, the classifier can’t tell what the command would delete.
The block clears when you name the exact path being deleted, or when Claude re-runs the delete with the resolved literal path written into the command. Deletes whose target the classifier can resolve aren’t affected.
A glob directly under the variable, as in`rm -rf "$VAR"/*` , is a critical path instead. For a`Remove-Item` target that is a bare`*` or ends in`/*` or`\*` , see Remove-Item in PowerShell: Claude Code denies that command outright.

- Requesting credentials from the cloud instance-metadata endpoint, such as `169.254.169.254` , or explicitly authenticating a cloud, cluster, or registry call with the machine’s own service-account or node identity
- Reaching a public host by a route other than a direct request, such as a tunnel, a reverse shell, or a resolver or proxy configuration rewritten to point outside
- Reading credentials that belong to the host rather than to your task, such as node certificates or the node’s container-registry auth
- Connecting to or scanning sibling containers, pods, or VMs that Claude didn’t start, or the node beneath the container

`autoMode.environment`.
Claude Code v2.1.261 and later also block these by default:
- Posting or writing a link to a public paste, diagram, or data-sharing service in a message, PR or issue text, a document, or anywhere else the link will be opened or fetched, when the URL itself carries the content being shared, unless you named that service

**Allowed by default**:

- Local file operations in your working directory
- Installing dependencies declared in your lock files or manifests
- 
Reading `.env` and sending credentials to their matching API
- Read-only HTTP requests
- 
Pushing to any branch of the repository you’re working in, including the default branch. A non-default branch whose name marks it as a deploy or publication target, such as `production` or`gh-pages` , isn’t covered: the classifier judges a push there on its own terms.
A push’s content is still checked against the classifier’s other rules, and`permissions.deny` rules can still block push commands as written in every mode. The remote’s own branch protection still applies.
- Deleting the exact jobs Claude created earlier in the same session
- Reading, reviewing, or writing security-related code, configs, and threat models as part of your task
- Messages between agents working together in the same multi-agent session
- 
Sending data to the trusted domains, buckets, and services you list in `environment` . This covers data flow only, not destructive or credential operations on the same infrastructure
- Claude in Chrome navigation to a trusted internal domain, localhost, or a URL you named

`claude auto-mode defaults` to print the rule lists built into Claude Code as JSON. If routine actions get blocked, an administrator can add trusted repos, buckets, and services via the `autoMode.environment` setting: see Configure auto mode.
Pushing to any branch of the repository you’re working in and creating a pull request that matches your request run without a prompt, unless the push or pull request falls under the blocked list, such as secrets or sensitive data leaving the repository, or a pull request that targets a different repository or organization. To require a human checkpoint before these commands while staying in auto mode, add `permissions.ask` rules, which match the command as written: see Common boundaries.
While `permissions.blockReadsOutsideWorkingDirectories` is off, file reads other than reads from network paths run without a prompt in auto mode, including reads outside the working directories. The first time Claude uses the Read, Grep, or Glob tool on a path outside them, Claude Code asks whether to allow that read.
The prompt doesn’t appear in `-p` runs, Agent SDK sessions, background sessions, the VS Code chat panel, or the desktop app. There, the read runs.
Whatever you answer, Claude keeps working:
- **Yes, and keep allowing any reads outside the working directories** : the read runs, later reads outside the working directories run as before, and Claude Code records your answer so the prompt doesn’t appear again
- **No, and block reads outside the working directories from now on** : the read is refused, and Claude Code sets`permissions.blockReadsOutsideWorkingDirectories` to`true` in your user settings, which makes the file tools refuse such reads in this and every later session and every permission mode. To lift the block, remove the setting from every settings file that sets it, then restart each running session. For one directory this session, use`/add-dir` .
- **No, and ask again next time** : the read is refused, and the next read outside the working directories prompts again
- **Yes, but ask again next time** : the read runs, nothing is saved, and the next read outside the working directories prompts again

- **Name the action and its specifics** : include the specific thing that makes it dangerous, such as the branch of a force push. If you name only the verb, as in “you can force-push”, the block can stay in place.
- **Expect it to cover one action** : an approval covers the destructive action you named, so a later action is blocked again unless you granted the approval as standing. To stop approving a routine pattern one action at a time, add it to`autoMode.allow` .
- **Some blocks stay in place** : the classifier’s precedence order sets out which blocks your approval can reach. To run a step it won’t clear, leave auto mode and answer the permission prompt.

- **A blocked action** : Claude Code shows a notification and lists the action in`/permissions` under the**Recently blocked** tab, where you can approve the action so Claude may retry.
- **Repeated blocks** : if the classifier blocks an action 3 times in a row or 20 times total, auto mode pauses and Claude Code resumes prompting. Approving the prompted action resumes auto mode. See Repeated-block thresholds for how the blocks are counted.
- **No verdict from the classifier** : when a safety check separate from auto mode refuses the classifier’s own request, or the classifier’s response doesn’t parse, Claude Code blocks the action without the notification or the**Recently blocked** entry. See Auto mode cannot determine the safety of an action for the message each case shows and what to do.
- **No verdict from the server** : under server-side classifier review, Claude Code denies an action the server gives no verdict for, and can stop the turn after ten responses in a row with no verdict. See The server returned no safety verdict.
- **A mode switch during a check** : if you switch permission modes while a classifier check is pending, Claude Code discards a verdict the new mode wouldn’t have requested. You’re prompted for approval instead, or the action is auto-denied in`dontAsk` mode.

`-p` run without a `--permission-prompt-tool` has no prompt to fall back to. When repeated blocks reach a threshold, the action doesn’t run and Claude keeps working. Claude Code doesn’t stop the run.
Repeated blocks usually mean the classifier is missing context about your infrastructure. Use `/feedback` to report false positives, or have an administrator configure trusted infrastructure.
The following sections cover the order Claude Code evaluates an action in, how the classifier reviews subagent work, and what classifier calls add in cost and latency.
## How the classifier evaluates actions


How the classifier evaluates actions

1. Actions matching your allow, ask, or deny rules resolve immediately, with these exceptions:
  - Writes to protected paths route to the classifier even when an allow rule matches. When the protected path is the file that a symlinked settings file points to, the write can prompt you instead, as the protected paths list describes
  - No allow rule approves recognized `rm` and`rmdir` critical-path removals
  - MCP tools marked `requiresUserInteraction` prompt you directly even when an allow rule matches, and so do connector tools your organization set to`ask` in sessions where that setting reaches Claude Code
  - A shell command that carries per-command allowed domains also routes to the classifier even when an allow rule matches, because a rule approves the command, not its hosts
  - Ask rules that match on a command’s content, such as `Bash(git push *)` , fall back to a permission prompt
  - A write that the symlink check resolves to a protected path prompts you when the path Claude requested isn’t itself protected
  - A read from a network path prompts you even when an allow rule matches
2. Read-only actions and file edits in your working directory are auto-approved, except writes to protected paths and a read that shows the first-read prompt.
  - In a session with server-side classifier review, file edits, read-only shell commands, and sandboxed shell commands wait for that review and are blocked if it flags them
  - A write inside your working directory that the symlink check resolves to a location outside it prompts you
  - When Claude reads an artifact someone else made, the approval cases listed in that section apply
  - A read from a network path prompts you
3. Everything else goes to the classifier, apart from critical-path removals under their default handling. The connector tools and `requiresUserInteraction` MCP tools that prompt you directly in step 1 never reach the classifier either, so neither an org-required approval nor a consent step is auto-approved
  - With Code Mode on, the code that Claude writes goes to the classifier once before it runs, and the tool calls inside it don’t. You can still block a call with a `deny` rule or require a prompt with an`ask` rule, but Claude can run Bash commands in forms the rules don’t match
  - Outside Code Mode, the classifier reviews messages Claude sends to another agent with `SendMessage` before Claude Code delivers them. If an allow rule matches`SendMessage` or a`PreToolUse` hook returns`"allow"` , a message can skip this review. The review also runs in plan mode while the classifier reviews commands. Requires Claude Code v2.1.222 or later.
4. With Code Mode on, the code that Claude writes goes to the classifier once before it runs, and the tool calls inside it don’t. You can still block a call with a 
5. If the classifier blocks, Claude receives the reason. In most sessions the reason is the name of the policy or rule the classifier matched, such as `[Destructive or Unwanted Actions]` ; see Review denials

- Blanket `Bash(*)` or`PowerShell(*)`
- Wildcarded interpreters like `Bash(python*)`
- A wrapper followed only by a wildcard, like `Bash(timeout *)`
- Shell rules that start with a wildcard, like `Bash(* --version)`
- Some wildcarded paths, like `Bash($HOME/bin/*)` or`PowerShell(C:\tools\x.exe *)`
- Package-manager run commands
- `Agent` allow rules
- `Monitor` allow rules, because Claude Code runs Monitor commands through the shell

`Bash(npm test)` stay in effect. Claude Code restores the dropped rules when you leave auto mode. Before v2.1.236, Claude Code left `Monitor` allow rules in effect in auto mode, so a rule that matched the whole tool approved Monitor commands without classifier review.Claude Code can also run `git status` itself before a command that would discard uncommitted work, such as `git reset --hard` or `rm -rf`, and show the classifier whether staged, modified, or untracked work is present. Claude Code reports untracked files in that check even when the repository’s git configuration sets `status.showUntrackedFiles=no`.Under server-side classifier review, `git reset --hard` can be blocked on a clean working tree. After the block, the classifier can see the clean status and allow Claude’s retry.In the classifier requests sent by Claude Code itself, the classifier sees user messages, tool calls other than read-only lookups such as file reads and searches, and your CLAUDE.md content. Tool results are stripped from those requests, so hostile content in a file or web page can’t manipulate the classifier directly.You can annotate a call’s result with a PostToolUse hook’s `classifierContext` field, which the classifier reads as application-provided context. The field requires Claude Code v2.1.236 or later.A separate server-side probe scans incoming tool results and flags suspicious content before Claude reads it. For more on how these layers work together, see the auto mode announcement and the engineering deep dive.
## How auto mode handles subagents


How auto mode handles subagents

1. Before a subagent starts, the delegated task description is evaluated, so a dangerous-looking task is blocked at spawn time.
2. While the subagent runs, each of its actions goes through the same decision order as in the parent session, with the same block and allow rules. Any `permissionMode` in the subagent’s frontmatter is ignored.
3. When the subagent finishes, the classifier reviews its work and its final report before the parent reads the report. When the classifier flags the subagent’s work or report, or a separate API safety check refuses the review, the report is still delivered, prepended with a security warning. When the classifier is unavailable for the review, the report arrives with a note to verify the subagent’s work before acting on it.

## Cost and latency


Cost and latency

`/model` selection. See which model they run on.Classifier calls count toward your token usage on Enterprise plans. They also count on accounts that use the Claude API, Claude Platform on AWS, Amazon Bedrock, Google Cloud’s Agent Platform, or Microsoft Foundry. Each check sends a portion of the transcript plus the pending action, adding a round-trip before execution.Where the server reviews the actions as part of the session’s model requests, there are no separate classifier calls to count. See Server-side classifier review.Sandboxed network access adds no per-connection classifier requests. The classifier judges the hosts a command names together with the command in one review, and Claude Code checks each connection against the approved list without calling the classifier again.`dontAsk` mode, Claude Code auto-denies every tool call that would otherwise prompt you, though a mod you installed can still approve some of those calls. To check which calls a mod can approve, see Extend permissions with hooks. Don’t rely on `dontAsk` to stop a tool call a mod makes itself.
Claude still runs actions that need no approval in Manual mode, such as file reads inside your working directories and read-only Bash commands. Actions matching your `permissions.allow` rules and calls approved by a PreToolUse hook run too. Use this mode for CI pipelines or restricted environments where you pre-define what Claude may do. The status bar shows `⏵⏵ don't ask on` in this mode.
These calls are denied where you’d otherwise see a prompt:
- **Your `ask` rules** : Claude Code denies calls matching your explicit`ask` rules.
- **`AskUserQuestion`** : Claude Code denies this built-in tool even if your allow rules match it.
- **Connector tools your organization set to `ask`** : these are denied in sessions where that setting reaches Claude Code.
- **MCP tools marked `_meta["anthropic/requiresUserInteraction"]`** : these are denied because their approval card needs an answer this mode can’t collect.

`PreToolUse` hook can’t approve these:
- Recognized `rm` and`rmdir` critical-path removals, such as`rm -rf /` and`rm -rf ~`
- Reads from a network path

`defaultMode: "dontAsk"`. See bypassPermissions for details.
Set it at startup with the flag:
`bypassPermissions` mode disables permission prompts and safety checks so tool calls execute immediately, including writes to protected paths.
Even in `bypassPermissions` mode, each of the actions no mode auto-approves gets a prompt or a denial, though you can turn off the prompt for dangerous `rm` commands. Reading another organization’s public artifact needs your approval, and this mode doesn’t ask for it, so Claude can’t read one. The Remove-Item in PowerShell denies also apply in this mode.
Two cross-session messaging safeguards still apply in this mode, and in interactive terminal plan-mode sessions where bypass permissions are available:
- The `isolatePeerMachines` approval prompt for messages to your sessions beyond this machine still appears.
- When no `crossSessionInbound` value applies, Claude Code holds an inbound message from another of your sessions for your approval, and delivers without asking only when the sending session identifies itself as also bypassing permission prompts. If you leave the permission mode while messages are held, Claude Code re-applies the inbound rules and delivers any held message they now accept.

`rm` and `rmdir` critical-path removals still prompt.
Plan mode keeps its blocks wherever Claude Code runs without an interactive terminal, including non-interactive runs with `-p`, Agent SDK sessions, and conversations in the VS Code extension’s chat panel. There, `--allow-dangerously-skip-permissions` makes `bypassPermissions` selectable later.
You can’t enter `bypassPermissions` from a session you started without it enabled. Enable it at launch with `permissions.defaultMode: "bypassPermissions"` or with an enabling flag:
`--dangerously-skip-permissions` flag is equivalent.
- **If you accept** : Claude Code sets`skipDangerousModePermissionPrompt` to`true` in`~/.claude/settings.json` , so later sessions skip the dialog. To see the dialog again, remove the key from that file or set it to`false` . The`skipDangerousModePermissionPrompt` reference lists the other settings files where you or your organization can set it.
- **If you decline** : Claude Code exits.

`--setting-sources` without `user`, the dialog can appear again although you accepted it earlier. To skip it, add `user`, as in `--setting-sources user,project`.
In non-interactive mode no dialog is shown.
A background session honors your acceptance when it’s recorded in user or managed settings:
- 
With no acceptance recorded, `claude --bg --permission-mode bypassPermissions` is refused. Accept the dialog in an interactive session first.
- 
With `skipDangerousModePermissionPrompt` set only in`.claude/settings.local.json` , the background session starts with the bypass request ignored:
  - With `--allow-dangerously-skip-permissions` ignored,`bypassPermissions` is left out of its`Shift+Tab` cycle.
  - With `--permission-mode bypassPermissions` or`--dangerously-skip-permissions` ignored, the session starts in Manual mode instead.
 `"skipDangerousModePermissionPrompt": true` to`~/.claude/settings.json` , then start a new background session.
- With 

`bypassPermissions` mode when running as root or under `sudo`:
`defaultMode: "bypassPermissions"` or `"dontAsk"` from your settings files, so a repository’s checked-in settings can’t start a cloud session in bypass-permissions mode. The setting is ignored silently and the session starts in the permission mode shown in the mode dropdown instead. See Switch permission modes for which modes cloud sessions offer.
Claude Code refuses `bypassPermissions` in a session you start with `--restricted`. `--restricted` requires Claude Code v2.1.248 or later. If you see `Error: bypassPermissions not supported in restricted mode`, remove the flag or setting that enables `bypassPermissions`, such as `--dangerously-skip-permissions`. To keep `bypassPermissions`, start without `--restricted` or `CLAUDE_CODE_RESTRICTED` instead.
This notice means the background session didn’t find your acceptance of the bypass permissions warning in user or managed settings, so it ignored the bypass request made at launch:
`"skipDangerousModePermissionPrompt": true` to `~/.claude/settings.json`, then start a new background session.
If `claude agents` shows the bypass permissions warning although your interactive sessions skip it, check whether `skipDangerousModePermissionPrompt` is set in `.claude/settings.local.json` or a `--settings` file. Interactive sessions count the key there as your acceptance of the warning, and `claude agents` doesn’t. Whether or not you find it there, accept the warning to add the key to `~/.claude/settings.json`, where `claude agents` looks for it.
A small set of paths holds repository state and Claude’s own configuration, so each mode has its own rule for writes to them:
`--restricted`, which requires Claude Code v2.1.248 or later, the classifier can’t approve protected-path writes.
While `permissions.blockWritesOutsideWorkingDirectories` is on, Claude Code refuses `Write`, `Edit`, and `NotebookEdit` writes to settings files in every permission mode.
In the modes that route protected-path writes to the classifier, a write that the symlink check resolves to a protected path prompts you instead when the path Claude requested isn’t itself protected.
`permissions.allow` rules in settings files do not pre-approve protected-path writes. The safety check runs before Claude Code evaluates allow rules from settings, so an entry such as `Edit(.claude/**)` in `~/.claude/settings.json` or `.claude/settings.json` does not change the per-mode outcome in the table above. In permission modes that prompt, the prompt for a write to the project’s `.claude/` folder or to `~/.claude/` can offer one of these session-scoped options:
- For the project’s `.claude/` folder:**Yes, and allow Claude to edit files in this project’s .claude folder for this session**
- For `~/.claude/` :**Yes, and allow Claude to edit files in its ~/.claude folder for this session**

- `.git`
- `.config/git`
- `.vscode`
- `.idea`
- `.husky`
- `.cargo`
- `.devcontainer`
- `.yarn`
- `.mvn`
- `.claude` , with a few exceptions, such as:
  - Claude’s own git worktrees under `.claude/worktrees/`
  - The current session’s own plan files in `~/.claude/plans/` , or in the`plansDirectory` you set
  - A background session’s own scratch directory at `~/.claude/jobs/<id>/tmp/`
  - Markdown files in the project’s auto memory directory, such as `~/.claude/projects/<project>/memory/` , in a session started without`--restricted`
  - Markdown files in a subagent memory directory, such as `.claude/agent-memory/` , in a session started without`--restricted`
- Claude’s own git worktrees under 
- A directory you loaded with `--plugin-dir` , because Claude Code reloads and runs a mod’s code from it when a file changes
- `~/.claude/dev-mods/` , or`dev-mods/` in the directory`CLAUDE_CONFIG_DIR` points to, where Claude saves the mods it writes for you

- `.gitconfig` ,`.gitmodules`
- `.bashrc` ,`.bash_profile` ,`.bash_login` ,`.bash_aliases` ,`.bash_logout` ,`.zshrc` ,`.zprofile` ,`.zshenv` ,`.zlogin` ,`.zlogout` ,`.profile` ,`.envrc`
- `.npmrc` ,`.yarnrc` ,`.yarnrc.yml` ,`.pnp.cjs` ,`.pnp.loader.mjs` ,`.pnpmfile.cjs` ,`bunfig.toml` ,`.bunfig.toml`
- `.bazelrc` ,`.bazelversion` ,`.bazeliskrc`
- `.pre-commit-config.yaml` ,`lefthook.yml` ,`lefthook.yaml` ,`.lefthook.yml` ,`.lefthook.yaml`
- `gradle-wrapper.properties` ,`maven-wrapper.properties`
- `.devcontainer.json`
- `.ripgreprc` ,`pyrightconfig.json`
- `.mcp.json` ,`.claude.json`
- `remote-settings.json` , the stored copy of your organization’s server-managed settings, and the signature, sync, and consent files Claude Code keeps with it, in`~/.claude` or the directory`CLAUDE_CONFIG_DIR` points to
- A file you passed to `--settings` ,`--mcp-config` , or`--agents` when you started the session
- The file that your user, project, or local settings file points to when the settings file is itself a symbolic link, for example into a dotfiles repository. In the modes that route protected-path writes to the classifier, a write to this file prompts you instead, even when an allow rule matches. If the file’s own path is also that of a settings file, such as `.claude/settings.json` in another folder, the write goes to the classifier as other protected-path writes do

`rm` and `rmdir` commands, such as the filesystem root, your home directory, and your working directory. Claude Code checks these commands before they run, and no allow rule approves one whose target it recognizes as a critical path. The check can miss some commands, so it doesn’t replace an operating-system control such as sandboxing.
A critical-path removal is an `rm` or `rmdir` command whose target Claude Code recognizes as a critical path. No `PreToolUse` hook that returns `"allow"` approves one either. An allow rule or `PreToolUse` hook can’t approve one even in the permission modes that skip other prompts.
These controls apply whether or not the check recognizes a command:
- **Deny rules** : a deny rule that matches the command blocks it, though a mod you installed can still approve some of those calls
- **Sandboxing** : sandboxing has the operating system limit which paths Bash commands can write to. Sandboxed commands can still delete your working directory
- **Strict sandbox mode** : by default a sandboxed command can be retried outside the sandbox, and an allow rule or`bypassPermissions` mode can approve that retry with no prompt. Strict sandbox mode turns the retry off

`Remove-Item` and the `cmd` removal built-ins have their own checks, covered in Remove-Item in PowerShell.
Claude Code treats an `rm` or `rmdir` target as a critical path when it is any of the following:
- The filesystem root
- Top-level directories, meaning any direct child of the root, such as `/usr` ,`/etc` , or`/data`
- Your home directory. On Windows, its 8.3 short name counts too, such as `C:\Users\LONGNA~1`
- Windows drive roots and their top-level directories, such as `C:\` and`C:\Windows` . Spellings such as`\\?\C:\` and`\\localhost\C$` count as`C:\`
- Your working directory and its parents
- Your additional working directories and their parents, but only when the removal is a glob under one of them, such as `rm -rf <dir>/*` .`rm -rf <dir>` on the directory itself doesn’t trigger this check

`/mnt/c` or `/Volumes/USB`, see The mounted drive prompt.
The checks on the home directory’s 8.3 short name and on the `\\?\C:\` and `\\localhost\C$` spellings require Claude Code v2.1.292 or later.
Claude Code also treats the following `rm` and `rmdir` targets as critical paths. The last column says why each one counts.
`CLAUDE_CODE_DISABLE_SUBSTITUTION_RM_PROMPT=1` in the environment that launches Claude Code.
Claude Code also looks inside these constructs:
- **Nested commands** : a subshell with`(...)` , a brace group with`{ ...; }` , command substitution with`$(...)` or backticks, or process substitution with`<(...)` . Claude Code finds a critical-path removal whether it sits inside the nested form, as in`(rm -rf ~)` or`echo "$(rm -rf ~)"` , or elsewhere in the same command.
- **Inline scripts** : a script passed to`sh` ,`bash` ,`zsh` , or a similar POSIX shell with`-c` , as in`bash -c 'rm -rf ~'` .
  - When the script is double-quoted, the invoking shell expands its variables before the inner shell receives the script. In `find . -name '*.tmp' -exec sh -c "rm -rf \"$1\"/*" _ {} \;` , the command expands to a removal from the filesystem root once per match, and Claude Code treats it as a critical-path removal.
  - A single-quoted script that binds `$1` to a real value, as`sh -c 'rm -rf "$1"/*' _ {}` does, isn’t flagged.
  - If you see `This shell -c script runs rm and could not be checked` , read the script before you approve it. The command counts as a critical-path removal.
- When the script is double-quoted, the invoking shell expands its variables before the inner shell receives the script. In 

`-c` script, such as `~`, set `CLAUDE_CODE_DISABLE_INLINE_SHELL_RM_PROMPT=1` in the environment that launches Claude Code.
How to rewrite a command so it passes the check depends on which of the other targets it uses:
- **A glob or trailing slash under a variable such as `$DIR`** : guard each expansion so the shell stops with an error when the variable is unset or empty, as in`rm -rf "${DIR:?}"/*` , or use a literal path. A removal whose expansions are all guarded that way passes this check, so in`bypassPermissions` mode it runs without a prompt unless another critical-path check flags it.
- **A glob or trailing slash under a variable that is normally set, such as `$HOME`** : use a literal path.
- **A variable assigned from a directory-printing substitution** : use a literal path. A`"${D:?}"` guard doesn’t clear this check, because the variable isn’t empty.
- **A target that is only command substitution output** : run the substitution on its own first, then remove the literal paths it prints. The prompt tells Claude to do the same.

`rm` and says how to rewrite it so the check passes.
The full reason reads:
`rm` present. In `auto` or `bypassPermissions` mode, a denial with this sentence comes from the same check:
`bypassPermissions` included, though you can turn off that prompt in `bypassPermissions` mode. You can’t approve the command with a `Bash` or `Bash(*)` allow rule, sandbox auto-allow mode, or a `PreToolUse` hook returning `"allow"`. Read the command, then approve the prompt or, after a denial, run it in your shell.
The mounted drive prompt appears on macOS, Linux, and WSL when a recursive `rm` targets a listed mounted drive path, such as `/mnt/c` or `/Volumes/USB/build`. Its reason reads `Dangerous rm operation on mounted drive path: <path>`. A `/*` target gets the prompt without the recursive flag, since Claude Code reads `rm /Volumes/USB/build/*` as a removal of `/Volumes/USB/build`.
Even with an allow rule for `rm`, Claude Code handles the command as a critical-path removal: a prompt or a denial depending on your permission mode. Approve the prompt to let the `rm` run, or turn the prompt off.
Match the target of your recursive `rm` or `/*` removal against these path shapes, where each placeholder such as `<drive>` or `<name>` stands for exactly one path segment:
`rm` on `/Volumes`, `/media`, or `/mnt` itself, you see the critical path reason `Dangerous rm operation on critical path: <path>` instead.
A relative target counts from your working directory, so `rm -rf build` while you work in `/Volumes/USB` targets `/Volumes/USB/build` and gets the prompt.
To stop getting the mounted drive prompt while prompts from other causes stay, start Claude Code with `CLAUDE_CODE_DISABLE_MOUNTED_DRIVE_RM_PROMPT=1` in its environment. For example, set the variable in your shell and start `claude` from it:
`/Volumes/USB` and for the other listed paths. If you allow `Bash(rm *)`, use Manual mode, and work in `/Volumes/USB`, then `rm -rf build` runs with no prompt.
A prompt that still shows `Dangerous rm operation on mounted drive path` means the variable isn’t reaching the session:
- **Variable in a settings file** : Claude Code ignores the variable in a settings file’s`env` block, whichever kind of settings file, so set the variable where you start`claude` instead
- **Session started without the variable** : start`claude` again from an environment where the variable is set, since a running session doesn’t pick it up

`rm`, an `rm` on a mounted drive path can still prompt, without the `Dangerous rm operation on mounted drive path` reason:
- **Target outside your working directories** :`rm -rf /Volumes/USB/other` while you work in`/Volumes/USB/project` gets a prompt. To stop that prompt, add a directory that contains the target to the session
- **Target with a `*`** :`rm -rf build/*` gets a prompt. Approve that prompt to run the command, or ask Claude to name each target without a`*`
- **Target that is a critical path** :`/mnt` itself and your working directory itself stay critical paths, so`rm -rf` on either gets a prompt. Approve that prompt to let the`rm` run

`auto` mode and without a time limit. In modes that ask, a `PermissionRequest` hook can answer the prompt.
To run these removals with no prompt in `bypassPermissions` mode, see Turn off the critical-path removal prompt.
In `auto` and `bypassPermissions` modes, the terminal prompt for a critical-path removal shows a two-minute countdown:
- If the countdown runs out before you answer, Claude Code denies the command and tells Claude what to do instead, so an unattended session keeps working.
- Press any key while the prompt is open to stop the countdown and keep the prompt waiting for your answer.
- After three of these prompts run out unanswered in a session, Claude Code stops showing them and denies further critical-path removals immediately. Sending a new message starts the count over.

`auto` mode denial outside the terminal.
Away from the terminal, you don’t get that prompt:
`CLAUDE_CODE_DISABLE_DANGEROUS_RM_TIMEOUT` to `1`:
- **`bypassPermissions`** : the terminal prompt has no time limit, and the VS Code chat panel asks you
- **`auto`** : the classifier decides, in the terminal, the VS Code chat panel,`-p` runs and Agent SDK sessions

`env` option, not in a Claude Code settings file. For the chat panel, add it to `claudeCode.environmentVariables` in VS Code’s own user `settings.json`, then start a new conversation:
`rm` and `rmdir` commands that target critical paths with no prompt in `bypassPermissions` mode, start Claude Code with `CLAUDE_CODE_EVEN_MORE_DANGEROUSLY_SKIP_PERMISSIONS=1` in its environment. This requires Claude Code v2.1.311 or later. Claude Code reads the variable from the launch environment only and ignores it in a settings file’s `env` block. For example, set it in your shell and start `claude` from there:
- macOS, Linux, WSL
- Windows PowerShell
- Windows CMD

`bypassPermissions` mode itself, not in plan mode, and only for what the critical-path check flags in the Bash and Monitor tools. That includes commands Claude Code can’t fully parse, which also run with no prompt.
To check whether `CLAUDE_CODE_EVEN_MORE_DANGEROUSLY_SKIP_PERMISSIONS` is working, read the line Claude Code prints at startup. In a `-p` run, it’s on stderr. In an Agent SDK app, pass the `stderr` option to read it. The line says one of these:
- **`is set, so dangerous rm commands can run without a prompt`** : removals can run with no prompt once managed settings and plugin hooks have loaded. If removals keep getting a prompt or denial, see Why the critical-path removal prompt stays on
- **`has no effect in this session`** : removals keep their usual prompt or denial for the whole session. To change that, find your case under Why the critical-path removal prompt stays on
- **`has no effect while the Windows registry cannot be read for managed settings`** : removals keep their usual prompt or denial, and the variable takes effect with no restart if a later read of the registry succeeds. If removals keep getting a prompt or denial, see Why the critical-path removal prompt stays on

`stderr` option sees no line, and removals can still run with no prompt. Otherwise, check that you set the variable in the launch environment, that the session can enter `bypassPermissions` mode, and that `claude` isn’t inside another session. Some sessions start without the variable.
If you see one of these lines and didn’t set the variable, find where it’s set, such as your shell profile, and remove it.
If `CLAUDE_CODE_EVEN_MORE_DANGEROUSLY_SKIP_PERMISSIONS` has no effect and removals keep their usual prompt or denial, check for causes such as these:
- **Something else handles permission prompts** : a`--permission-prompt-tool` or an Agent SDK`canUseTool` callback without`--permission-prompts none` , a`PermissionRequest` hook with any matcher, or a mod that handles that event. Remove the hook, mod, tool, or callback and restart`claude`
- **The VS Code extension’s chat panel or the Desktop app runs the session** : start`claude` in a terminal instead, such as an IDE’s integrated terminal
- **A rule covers Bash, Edit, or Read** : a deny or ask rule that targets one of these tools, at launch or once Claude Code reads it during the session, even if you remove it later. Remove the rule and restart`claude` . To leave a tool out, use`--tools` instead of a deny rule
- **`permissions.blockReadsOutsideWorkingDirectories` is on** : remove the setting from every settings file that sets it to`true` , then restart`claude`
- **`claude` runs inside another session** : a`claude` started from a session’s Bash tool or a hook, even with the variable on its command line. Start`claude` from your own shell. Don’t rely on this nested-session limit as a security boundary
- **A `PreToolUse` hook answers `ask` for a removal** : change what the hook returns
- **Claude Code starts the session for you** : background sessions, agent team teammates in split panes, and sessions a Remote Control server creates start without the variable. Start`claude` from your own shell
- **Managed settings can’t be read** : Claude Code hasn’t successfully read your managed settings sources. If server-managed settings keep failing to load, see Remote managed settings failed to load. For any other failure that isn’t a registry query, ask whoever manages that source to fix it
- **The Windows registry can’t be queried** : on Windows and in WSL, the variable needs an answer from the registry, even when WSL doesn’t use it for settings. The query can fail all session on a non-English Windows, in WSL with interop off, or in a container on a WSL2 kernel. If interop is off, turn it on. Otherwise, run those removals yourself

`Remove-Item` and the `cmd` built-ins `rd`, `rmdir`, `del`, and `erase` their own checks, separate from the `rm` critical-path list. For `Remove-Item`, the outcome depends on the target, and the first matching case applies:
- **System paths** : the filesystem root and its top-level directories, drive roots and their top-level directories, and your home directory. Claude Code denies the command in every mode, without asking you.
- **Wildcards** : a bare`*` , or any target ending in`/*` or`\*` , including a glob under a shell variable such as`$dir/*` . Claude Code denies the command in every mode, without asking you, before the classifier sees it.
- **Your working directory or one of its parents, with `-Recurse`** : Claude Code treats the command like any other that needs approval in your permission mode, so it asks you in modes that ask, sends it to the classifier in`auto` mode, and denies it in`dontAsk` mode.`bypassPermissions` mode skips this check.

`rd`, `rmdir`, `del`, and `erase` when Claude runs them through `cmd`, as in `cmd /c rd /s /q C:\Users`. By default, Claude Code denies such a command in every mode, without asking you. This `cmd` check requires Claude Code v2.1.283 or later.
When judging a `cmd` target, Claude Code treats a PowerShell variable that follows literal text as empty. That makes `cmd /c rd /s /q "C:\$name"` a removal of `C:\`, so it is denied too. A trailing wildcard counts as the folder it empties, so `cmd /c del /q C:\*` is denied and `cmd /c del /q dist\*` in your project is not.
To turn the `cmd` check off, set `CLAUDE_CODE_DISABLE_POWERSHELL_CMD_RM_DENY=1` in the environment that launches Claude Code. Claude Code ignores this variable in a settings file’s `env` block. `Remove-Item` on a system path stays denied either way.
- Permissions: allow, ask, and deny rules; managed policies
- Configure auto mode: tell the classifier which infrastructure your organization trusts
- Hooks: custom permission logic via `PreToolUse` and`PermissionRequest` hooks
- Security: safeguards and best practices
- Sandboxing: filesystem and network isolation for Bash commands
- Non-interactive mode: run Claude Code with the `-p` flag
