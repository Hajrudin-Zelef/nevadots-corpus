---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-287-claude-code-releases
title: "Claude Code 2.1.287 · Claude Code Releases"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "added", "improved", "prompt", "plugin", "code", "background", "tool"]
source: https://releases.oikon48.dev/en/2.1.287/
source_anchor: ""
source_lines: [1, 439]
section: "Claude_Anthropic"
---
## New features

9
#### Add Claude Mods

Added Claude Mods: plugins may now modify deeper behavior

#### Add You should know mod

Added You should know, a built-in mod where a side agent watches your back and flags things you or Claude might miss. Turn it on with /plugin enable cc-plugin-you-should-know@builtin (for first-party sessions with telemetry on)

#### [VSCode] Add Run in background

Added "Run in background" to a running command or sub-agent, to move it to the background and keep working

#### Support URL prompts from MCP servers

Added URL prompts from MCP servers on the 2025-11-25 protocol, for example to sign in. If a server no longer connects after this update, add "bareElicitationCapability": true to its MCP config entry

## More changes (5) Fewer changes

#### Add name filter to agents view

Added an n:<text> filter to the agents view that matches session names and tasks; a filter now shows matches in collapsed sections and Enter opens the first match

#### Add prompt_text to user_prompt event

Added prompt_text to the OpenTelemetry user_prompt event, a copy of prompt for backends that nest dotted keys; drop or mask it wherever you drop or mask prompt (anthropics/claude-code#70763)

#### Warn on Windows when no shell tool

Windows: Added a startup warning when denying the Bash tool also turns off the PowerShell tool, so Claude has no shell tool

#### Built-in gh api for self-hosted runners

Self-hosted runner: Added a built-in gh api (REST only) for sessions that use Anthropic-managed git on macOS and Linux machines where the GitHub CLI is not installed

#### [VSCode] Show background output in agent map

Added the output of background shells and Monitors to their cards in the agent map

## Improvements

30
#### Faster Bash tool on Windows

Windows: Improved Bash tool speed by removing a subshell that ran before every command

#### Default 1M context on cloud providers

Changed Opus 4.7+ and Fable to use a 1M context window by default on Bedrock, Vertex, Foundry and the Claude apps gateway, with no [1m] suffix (CLAUDE_CODE_DISABLE_1M_CONTEXT=1 keeps 200K)

#### Queue claude agents replies

Changed replies from claude agents to arrive as queued messages; slash commands other than /stop sent while a turn is running now run when it ends

▤ In the docs
#### Confirm shell writes through risky symlinks

Changed a shell write through a repo-committed symlink onto a sensitive file or out of the working tree to name where it lands and wait for a person, on lines with a ~ target too

## More changes (26) Fewer changes

#### Improve /config navigation and layout

Improved /config: settings that cycle show angle-bracket markers and step both ways with the left and right arrow keys, narrow terminals stack each value under its label, and PgUp/PgDn page the list

#### Clearer plugin marketplace errors

Improved plugin marketplace errors to say in plain words why a marketplace was ignored or refused, and what to do

#### Flag missing plugin dependencies

Improved plugin listings to note when a plugin's dependencies were not installed, and updating a plugin now retries an install that did not finish

#### Clearer Bedrock model ID errors

Improved the Claude apps gateway's error when Amazon Bedrock rejects a model ID: developers now see which model is unavailable, and the gateway log names the ID that was sent

#### Keep web fetches running on priority messages

Improved SDK sessions so a message sent with priority "now" no longer cancels a running web fetch or web search; it keeps loading in the background

#### Arrow keys toggle /memory settings

Improved /memory: the left and right arrow keys now flip its on/off settings, such as Auto-memory

#### Recognize skill names typed mid-message

Improved /skill names typed mid-message: Claude is now told they are skills, including disable-model-invocation ones

#### Better contrast in light themes

Improved the contrast of the prompt input border in light themes and of the prompt marker before your earlier messages

#### Retry failed file uploads once

Improved delivery of files Claude sends from cloud sessions and Remote Control: an upload that fails on a timeout, a network error or a 502, 503 or 504 is now retried once

#### Suggest retrying temporary file failures

Improved what Claude says when a file cannot be sent for a reason that may be temporary: it now mentions that you can ask for the file again in a few minutes

#### Dashed lines for held message prompts

Improved the prompt for a held message from another session to show the message between dashed lines, matching other permission prompts

#### Dashed lines for tool permission prompts

Improved MCP and other tool permission prompts to show the tool call between dashed lines, matching file edit prompts

#### Faster MCP retries in headless mode

Improved MCP startup in headless mode: a remote server whose first connect fails transiently is now retried without waiting for the slowest server to finish connecting

#### Stream large files from remote sessions

Improved files Claude sends from a remote session: large files now stream from disk instead of being read into memory, and a file over the size limit is refused with the server's limit named

#### Explain refused file sends

Improved the explanation Claude gives when the server refuses a file it sends from a Remote Control or cloud session, such as an oversized image

#### Lighter handling of large MCP results

Improved handling of large MCP tool results: less memory, smaller session files, and no extra upload to count tokens for results far over the limit

#### Prompt for writes to protected files

Changed whole-tool Bash allow rules and allowing hooks to prompt for, not run, shell writes to files Claude Code's file tools refuse outright (the Anthropic profile store, the host credentials file)

#### Paste on mouse button release

Changed right-click paste on Windows and Linux, and middle-click paste on Linux, to happen when the button is released; moving the pointer away before releasing cancels it

#### Defer all tools with alwaysLoad false

Changed MCP server alwaysLoad: false to defer all of that server's tools behind tool search

#### Remove screen reader line pause

Changed screen reader mode to write new or changed lines without first pausing with the cursor at the start of the line; set CLAUDE_AX_PREPARK_MS=50 to restore the pause

#### Keep effort on automatic model switches

Changed automatic model switches after a flagged message to keep your current effort level instead of the new model's default

#### Show waiting prompts oldest first

Changed waiting permission prompts to show oldest first, so a new prompt no longer covers the one you're reading (prompts with a countdown still open on top)

#### [VSCode] Clearer Manage plugins errors

Improved the Manage plugins dialog: a failed marketplace add, remove or refresh now says what went wrong

#### [VSCode] Connect editor sessions to Chrome

Changed the Claude in Chrome "Enabled by default" switch to also connect the editor's own sessions, which still ask before browser actions

#### [Claude Tag] Stop reposting task lists

Improved Claude's task list in long Slack threads: background work no longer reposts it as a new message on its own, so people following the thread aren't notified

#### [Code Review] Explain locked conversation failures

Improved the failed-review card on a pull request whose conversation is locked: it now says the lock blocked the review and that nothing was posted or charged

## Bug fixes

67
#### Fix duplicate folder CLAUDE.md

Fixed a folder's CLAUDE.md being attached a second time after resuming a session or after a compaction

#### Keep always-ask for dangerous rm

Fixed a dangerous rm (such as one on / or the home directory) losing its always-ask safeguard when the same command also redirected output to a ~ or wildcard path

#### Enforce ceilings for __proto__ MCP tool

Fixed organization per-tool permission ceilings being silently dropped for an MCP tool named __proto__

## More changes (64) Fewer changes

#### Fix fast mode in agent-owned sessions

Fixed fast mode staying off in remote sessions owned by an agent with no user account, even when the organization allows it

#### Fix Remote Control message stalls

Fixed Remote Control not receiving messages for minutes at a time when a reconnect request got no response; it now gives up after 30 seconds and retries

#### Fix repeated asyncRewake wakeups

Fixed hooks configured with asyncRewake waking Claude over and over with "found issues" notifications when the hook's script file is missing; the broken hook is now reported once

#### Fix tool heartbeats during stalled streams

Fixed tool heartbeats not reaching SDK hosts while the model's response stream was stalled with no data arriving

#### Respect availableModels in startup checks

Fixed Bedrock and Vertex startup model checks ignoring an enforced availableModels list, which could collapse /model to one Opus row

#### Fix Chrome picker JSON parse error

Fixed the Claude in Chrome browser picker showing a JSON parse error when Chrome could not be reached

#### Saved Fable default follows newest

Fixed picking Fable in /model on a claude.ai login saving the current version's id, so your saved default now follows the newest Fable like Opus and Sonnet do

#### Keep thinking when switching models

Fixed switching between Opus 5.5 and Sonnet 5.5 (/model, opusplan) rewriting earlier MCP tool announcements, which could drop earlier extended thinking

#### Show Bedrock guardrail messages mid-response

Fixed Amazon Bedrock Guardrails blocks that arrive mid-response ending the turn with an API error instead of the guardrail's message when the reply began with thinking

#### Fix repeated model fallback after switch

Fixed claude -p and SDK sessions repeating a model fallback on every later message after the model was switched while a reply was running

#### Reopen sessions after worktree removal

Fixed background sessions that could not be reopened from claude agents after the agent exited and removed the worktree the session was started in

#### Fix /advisor pairing checks

Fixed /advisor pairing checks: Sonnet 5.5 can now advise Opus 4.7 and 4.8, and advisors the API would refuse are flagged up front instead of being silently dropped

#### Explain Bash permission prompts plainly

Fixed Bash permission prompts showing internal parser names such as "Contains simple_expansion" instead of a plain explanation

#### Fix fullscreen exit while scrolling

Fixed a cause of fullscreen sessions on slow or busy machines exiting with "Claude Code exited after an unrecoverable interface error" while a scroll key was held in a long conversation

#### Fix paging advice for MCP JSON

Fixed Claude being told to page large MCP results saved as JSON with Read's offset and limit, which cannot split one long line

#### Fix attribution reminder after compaction

Fixed the commit attribution reminder being delivered inside a tool result after a compaction

#### Fix screen reader cursor in search boxes

Fixed screen reader mode leaving the cursor away from the typed text in search boxes (such as /resume and /permissions) and sign-in code fields

#### Allow empty Enter on /rewind summarize

Fixed screen reader mode refusing Enter with nothing typed on /rewind's summarize options, whose added context is optional

#### Remove misleading Tab hint

Fixed screen reader mode showing a "Tab to amend" hint on approval prompts, where Tab does nothing

#### Fix misleading screen reader key hints

Fixed screen reader mode listing arrow keys that do nothing in /permissions and /mcp, and saying "Select with numbers" in empty menus or while a search box has the keys

#### Include changed lines for screen readers

Fixed screen reader mode leaving out the changed lines in file edit approval prompts and other diffs

#### Stop repeated screen reader announcements

Fixed screen reader mode sending the claude --teleport progress screen, and an MCP form field while it is being checked, to the screen reader again on every spinner frame

#### Fix experimental betas opt-out

Fixed CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS not removing the structured-output format from session-title and prompt-hook requests, which Bedrock-backed gateways reject

#### Keep top lines for screen readers

Fixed screen reader mode leaving out the top lines of a second approval prompt, a changed /config row or the rejected-plan line when the previous screen was taller than the terminal window

#### Fix late message_stop for cut replies

Fixed --include-partial-messages sending a cut-short reply's message_stop late or never, so apps could show the reply as still in progress

#### Show pending prompts in claude agents

Fixed claude agents sometimes not showing the permission prompt a background session is waiting on

#### Fix /ultrareview gitattributes advice

Fixed /ultrareview giving advice about .git/info/attributes when the upload stops on a committed .gitattributes it cannot read, such as one saved as UTF-16

#### Fix remote-control behind HTTP proxy

Fixed claude remote-control failing to register behind an HTTP proxy with a misleading "Check your organization permissions" error (anthropics/claude-code#97352)

#### Stop leaking executable handle to sandbox

Fixed sandboxed Bash commands on Linux inheriting an open handle on the Claude Code executable

#### Respect Reduce motion for spinners

Fixed the running-tool dot and three spinners still moving with the "Reduce motion" setting on, and /rewind's confirm screen updating its "ago" time while you type a note

#### Slow agents time updates for screen readers

Fixed times in claude agents changing every second in screen reader mode; they now change at most every 10 seconds

#### Show OAuth token revoked error

Fixed a revoked claude.ai login showing a generic API Error: 401 instead of "OAuth token revoked"; in -p mode the error now starts with "Failed to authenticate"

#### Fix /ultrareview variable copy advice

Fixed /ultrareview upload refusals advising you to copy a variable named by a repository's settings file into your own user settings

#### Stream forked skill turns

Fixed --output-format stream-json and the SDK not streaming the turns of a context: fork skill run by typing /<skill> as the prompt, as they do for the Skill tool's fork

#### Keep error messages out of issues

Fixed /feedback and /bug: the pre-filled GitHub issue no longer includes your recent error messages, and the confirmation screen now lists them as part of the report

#### Allow plain http for plugin installs

Fixed claude plugin marketplace add --sparse and git-subdir plugin installs failing with "transport 'http' not allowed" when the repository is served over plain http

#### Keep history across compaction restarts

Fixed cloud sessions sometimes losing the earlier conversation when the session restarted while it was being compacted

#### Fix plugin archive cache corruption

Fixed a plugin reload that overlapped the startup --plugin-url download corrupting the session's cached copy of the plugin archive

#### Clearer /desktop timeout errors

Fixed /desktop quoting partial output when opening Claude Desktop timed out or printed too much output; the error now names the cause

#### Fix duplicate MCP connector tool calls

Fixed an MCP connector tool call occasionally running twice, or the connector's calls failing until restart, when its server changed which MCP protocol version it supports

#### Run synced plugin SessionStart hooks

Fixed SessionStart hooks from synced plugins not running in new cloud sessions

#### Exclude internal callbacks from hook counts

Fixed the transcript's "N hooks ran" summary and the verbose debug log's matched-hooks count including Claude Code's internal callbacks, so one configured hook no longer shows as two

#### Wait longer for file uploads

Fixed files Claude sends from cloud and Remote Control sessions failing when the upload finished just after the 30-second timeout; it now waits 35 seconds

#### Load skills from repos added mid-session

Fixed repositories added mid-session in cloud and SDK sessions not loading their skills and plugins, and loading CLAUDE.md late, after Claude changed directory

#### Send oversized images from remote sessions

Fixed PNG, JPEG and WebP images over 8,000 pixels on a side failing to send from a remote session; Claude now sends a scaled-down copy

#### Deliver all attached files from apps

Fixed messages sent from the Claude apps with 17 to 20 attached files delivering only the first 16

#### Fix false MCP auth-needed status

Fixed headless sessions reporting an MCP server as needing authentication after one refused call, even though later calls succeed

#### Keep Remote Control running during idle sleep

macOS: Fixed Remote Control sessions started with claude remote-control stopping mid-turn when the Mac went to idle sleep

#### Explain piped input in interactive mode

Windows: Fixed interactive claude hanging or crashing with "Raw mode is not supported" when its input is piped or redirected; it now says why and exits (use -p for piped input)

#### Fix duplicate Authorization header in checks

Bedrock, Vertex, Mantle: Fixed model availability checks under CLAUDE_CODE_SKIP_*_AUTH sending a different Authorization header than real requests when ANTHROPIC_CUSTOM_HEADERS repeats it

#### [VSCode] Fix misleading save timeout errors

Fixed settings dialogs blaming a timeout when Claude Code's reply was too large to confirm a save

#### [VSCode] Fix duplicate tabs for cloud sessions

Fixed reopening a cloud session that the side bar already brought to this machine opening it again in a new tab; the side bar is shown instead

#### [VSCode] List new cloud sessions

Fixed the side bar's Web tab not listing cloud sessions started after the window loaded; a failed load now says "Remote server is not connected" instead of "No web sessions yet"

#### [VSCode] Fix duplicate Claude process on reload

Fixed a tab restored after a reload starting a second Claude process on a conversation the side bar already has open; it now shows the "still open somewhere else" notice

#### [VSCode] Fix links shown as plain text

Fixed tool-row file links, session-list links and two hints showing in plain text

#### [VSCode] Fix running commands shown as failed

Fixed a background agent's still-running command showing as failed once the main turn ended

#### [VSCode] Run user /usage and /context commands

Fixed a user's own /usage or /context command opening the extension's dialog instead of running when picked from the command menu

#### [VSCode] Fix plan preview file links

Fixed file links in the plan preview tab doing nothing when clicked; they now open the file like links in chat replies

#### [VSCode] Fix remote editor tab timeouts

Fixed opening a tool's input or output in an editor tab failing with "Timeout waiting after 1000ms" on remote hosts such as WSL when the tab is slow to appear

#### [Cloud sessions] Fix GitHub token fetch failures

Fixed occasional failures to fetch from or push to GitHub when GitHub briefly refused a newly issued access token

#### [Claude Tag] Stop unneeded failure warnings

Fixed Claude posting a failure warning, such as a spend limit notice, in a Slack thread when a background event like GitHub activity woke it and nobody was waiting on a reply

#### [Claude Tag] Show all spend limit channels

Fixed Claude Tag's spend limits page in admin settings leaving out recently created and private channels in organizations with many channels

#### [Code Review] End comments on full sentences

Fixed finding comments and their "Why this was flagged" text stopping mid-sentence; they now end on a complete sentence

#### [Code Review] Review latest commit after failures

Fixed Code Review skipping a pull request after a new push when its review had failed twice on the previous commit; it now reviews the latest commit
