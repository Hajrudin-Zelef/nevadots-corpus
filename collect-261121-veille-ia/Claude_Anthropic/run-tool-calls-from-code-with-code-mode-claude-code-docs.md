---
id: collect-261121-veille-ia/claude-anthropic/run-tool-calls-from-code-with-code-mode-claude-code-docs
title: "Run tool calls from code with Code Mode - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "mode", "claude", "tool", "calls", "session", "each", "cant"]
source: https://code.claude.com/docs/en/code-mode
source_anchor: ""
source_lines: [1, 107]
section: "Claude_Anthropic"
---
`/feedback`.`Ctrl+O` to open the transcript viewer. Each piece of code appears there as a collapsed **Code**row, with a hint for how to expand it. Try Code Mode on tasks that take many related tool calls, such as:

- A change that repeats across many files
- A search whose results need filtering before Claude reads them
- Several independent commands that can run at the same time

- **Resuming** : you can resume a Code Mode session only in the terminal, with Code Mode on
- **Auto mode** : auto mode’s classifier reviews the code, not each tool call
- **Long-running code** : unless code can run in the background, you wait for it to finish, and it stops after ten minutes
- **Hooks** : you can see tool calls refused when a`PreToolUse` hook can’t run or times out
- **Unavailable features** : you can’t use Remote Control, Claude in Chrome, or computer use
- **Moving the session** : you can’t move the session to the Desktop app or turn it into a background session, and`/fork` can’t start a background copy

- **Version** : Claude Code v2.1.323 or later, on either release channel. An older version starts without Code Mode, with or without the reason`it isn't open to your account yet`
- **Account** : a Claude subscription or Console account. You can’t use Code Mode through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloud’s Agent Platform, Microsoft Foundry, or Claude Models on Google Cloud (Anthropic managed)
- **Organization** : your organization doesn’t use the HIPAA configuration or zero data retention
- **Model** : Claude Opus 5.5, Sonnet 5.5, Haiku 5.5, or Fable 5.2
- **Surface** : an interactive`claude` session in a terminal. Code Mode doesn’t work with`-p` , the Agent SDK, the VS Code panel, the Desktop app, or cloud sessions
- **Agent teams** : to use Code Mode with agent teams on, you need v2.1.326 or later. Teammates run in Code Mode in the default in-process display mode, and start without it in split panes
- **Installation** : the native build of Claude Code
- **Windows** : Git for Windows is installed

`DISABLE_TELEMETRY`. For the fix for each, see Code Mode doesn’t start.
Turn on Code Mode, give Claude a task, and look at the code that Claude wrote.
Check your version

Start Claude Code with Code Mode on

`CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` environment variable to `1`. To try Code Mode, set it in your shell. To turn on Code Mode for every new session, set it in your user settings.
- macOS, Linux, WSL
- Windows PowerShell
- settings.json

Confirm that you want Code Mode

**Yes, turn on Code Mode**. Claude Code restarts and shows this line near the top of the session:

Give Claude a task with many tool calls

Read the code Claude wrote

`Ctrl+O` and expand a **Code**row to see the JavaScript behind the tool calls. Press

`Ctrl+O` again to return to the conversation.`CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` from your shell and from `~/.claude/settings.json`, then start a new session. You can also keep the variable and change its value to `0` in either place. With a `0` in one place, Code Mode stays off even if you set `1` in the other.
Add `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` with the value `0` to your managed settings so that nobody in your organization can use Code Mode:
`0` is in place. You can’t turn on Code Mode for your users from managed settings.
Your permission mode decides how the code that Claude writes is checked and whether you can talk to Claude while the code runs.
In auto and `bypassPermissions` modes, code can run from start to finish without a permission prompt. Some calls still prompt you, such as the actions no mode auto-approves.
Each tool call in the code goes through your `deny` and `ask` permission rules and `PreToolUse` hooks under the tool’s usual name, such as `Bash` or an MCP tool’s name. Beyond rules and hooks, a classifier reviews the code in auto mode, and you answer prompts in the modes that ask:
- **Auto mode** : the classifier reviews each piece of code before it runs, not each tool call. It doesn’t see values computed at run time, such as a path taken from search output, so keep rules or hooks for calls you must control. You can still be prompted for some calls, such as calls in code that you approved yourself after repeated classifier blocks
- **Other permission modes** : you’re prompted for each tool call that would prompt you in that mode

`PreToolUse` hook that allows or blocks shell commands, make it check each part of a command, as Bash rules do. In Code Mode, a `git push` in a folder named `app` can reach your hook as `cd 'app' && git push`.
To check the code itself, write a `PreToolUse` hook that matches `REPL`, the tool that runs the code, and read the JavaScript from `tool_input.code`. To check what the code returns, see Check code output with a hook.
To see a prompt for a specific command in auto or `bypassPermissions` mode, add an `ask` rule for it. You’re prompted for the forms of the command that the rule matches. To be prompted for commands that start with `git push`, save this rule in `~/.claude/settings.json`:
`git push origin main`. You see a prompt for that command.
To read and approve code before it starts, add an `ask` rule for `REPL`, the tool that runs the code. Save this rule in `~/.claude/settings.json`:
`Ctrl+O`, expand the **Code**row, and press

`Ctrl+O` again to return. From the main conversation, you don’t see a subagent’s code in that view.
If you approve in auto mode, the code starts even when the classifier would have blocked it, and its project edits and read-only commands run without a prompt. If Claude reports that the code can’t start a subagent, remove the rule.
To stop the code at a prompt for one of its tool calls, answer **No**without a comment. These results apply to prompts that aren’t labeled as coming from a subagent:

- **No, or `Esc`** : the code stops, and changes from calls that already finished stay in place
- **No, with a comment** : the call is refused, and the code can continue with its other calls

- **Waiting** : Claude answers after the code finishes, and the code stops after ten minutes. In Manual and`acceptEdits` modes, time you spend at a tool’s permission prompt doesn’t count toward the ten minutes. Before v2.1.328, that time counted.
- **Background** : you can keep talking to Claude, and the code can run for up to two hours

`bypassPermissions` mode. If Claude still waits, check your current mode and how you started the session:
- **You switched to another mode** : press`Shift+Tab` until you’re back in the mode you started in
- **You started in another mode** : if auto mode is available to you, exit, then run`claude --continue --permission-mode auto` with Code Mode on
- **You’re on a version before v2.1.328 and picked the session from the `claude --resume` list** : exit, update Claude Code, then run the same`claude --resume` command with Code Mode on. To stay on your version, replace`--resume` with`--continue`
- **You passed tool names to `--tools`** : start again in auto mode without that flag

`Esc`. To stop background code after Claude finishes responding, run `/tasks` in the session, select the running task, and press `x`.
After you stop the code, check `/tasks` for subagents, shell commands, and MCP calls that the code started and that are still running. To stop one, select it and press `x`.
To check or redact what the code returns before Claude reads it, write a `PostToolUse` hook that matches `REPL`, the tool that runs the code. The same hook runs when background code finishes and when code throws an error.
In your `PostToolUse` hook, read the output from `tool_response`, which has fields such as these:
`tool_response` as `updatedToolOutput`, and build the copy this way:
- **Change the text** : redact`result` ,`stdout` ,`stderr` , and`error` . Handle a`result` that’s missing or`null` , and expect`error` only when the code failed or was stopped
- **Keep the shape** : return`code` ,`stdout` , and`stderr` as strings, and return`error` as a string or leave it out. If you break this shape, Claude gets the original output
- **Remove images and PDFs** : if the copy has`images` or`documents` to hide, set that field to`[]`

`notes.txt` result, print this JSON:
`@`, and check that Claude’s reply shows your replacement text.
Return a changed copy of `tool_response` as `updatedToolOutput` even when your hook blocks or hits an error:
- **Your hook blocks** : print the redacted copy in the same JSON as`"decision": "block"` . Without the copy, Claude still reads the original output
- **Your hook hits an error** : catch the error, and print a copy with placeholder text in the fields that you redact. If the hook crashes, times out, or prints invalid JSON, Claude gets the original output

`UserPromptSubmit` hook, move that check to your `PostToolUse` hook. On v2.1.328 or later, `UserPromptSubmit` hooks don’t receive code output, even when an administrator sets them in managed settings.
`Code Mode (experimental) isn't available` near the top of the session means that Claude Code started without Code Mode. The reason follows on the same line, and `/status` shows it too. Find the reason in this table, make the change, and start a new session. For a reason that isn’t listed, check the requirements.
`claude --version`. If your version is older than v2.1.323, update. Then check that `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` is `1` in your shell or user settings, and remove any `0` from those and from any `--settings` file.
In auto mode, if you approve code yourself after repeated classifier blocks, you can be asked about tool calls in that code and in earlier code that’s still running. With no IDE connected, the prompt can show a countdown:
`PreToolUse` hook didn’t give a decision, which refuses the call even when `onFailure` is unset:
- **`the hook failed`** : on a call from code, your hook couldn’t start, was killed, exited with 126, 127, or a code above 128, or returned an HTTP error or invalid output. Fix the hook
- **`the hook did not finish`** : your hook timed out on a call from code, or on any other tool call once Claude has run code in the session. Raise the hook’s`timeout`

`command` or `http` hook fails or times out, set `"onFailure": "continue"` on it. If a `command` hook exits with an ordinary code such as 1, the call goes ahead unless you set `"onFailure": "block"`.
If you expected output from code and see `UserPromptSubmit operation blocked by hook` in Claude’s reply, your `UserPromptSubmit` hook received the output as a prompt and blocked it. If Claude reports only the hook’s reason, ask it to quote the message it received.
On v2.1.328 or later, `UserPromptSubmit` hooks don’t receive code output. To let the output through on an earlier version, update Claude Code or change the hook:
- **Update** : if you rely on the hook to check code output, first move the check to a`PostToolUse` hook. Then update Claude Code to v2.1.328 or later
- **Stay on your version** : change the hook so that it doesn’t block when`source` in its input is`poll_event`

- Permission modes: compare auto mode with the modes that prompt you
- Hooks reference: write `PreToolUse` and`PostToolUse` hooks
- Managed settings: apply one configuration across an organization
