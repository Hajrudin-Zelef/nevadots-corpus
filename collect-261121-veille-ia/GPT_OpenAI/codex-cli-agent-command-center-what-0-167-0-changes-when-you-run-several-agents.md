---
id: collect-261121-veille-ia/gpt-openai/codex-cli-agent-command-center-what-0-167-0-changes-when-you-run-several-agents
title: "Codex CLI agent command center: what 0.167.0 changes when you run several agents"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "command", "center", "agents", "agent", "unread", "session", "no-daemon"]
source: http://dev.to/systemdesignsimple/codex-cli-agent-command-center-what-01670-changes-when-you-run-several-agents-23pj
source_anchor: ""
source_lines: [1, 84]
section: "GPT_OpenAI"
---
Originally published on System Design Simple: https://systemdesignsimple.com/codex-cli-agent-command-center/

Codex CLI 0.167.0 opens an agent command center with `codex agents --no-daemon`, marks unread results, keeps drafts across backends and previews images.

*Drafted with AI assistance, then checked and edited by a human engineer.*

**Short answer:** Codex CLI 0.167.0, released November 6, 2026, adds an agent command center you open with `codex agents --no-daemon`. The command center and session picker mark unread results, drafts survive a backend switch, and local image attachments show a preview on supported terminals. It is a supervision release, not a new model.


| Key fact | Detail | 
|---|---|
| Released | November 6, 2026, 01:40 UTC (GitHub release rust-v0.167.0) | 
| Open it with | `codex agents --no-daemon` | 
| Unread markers | In the command center and the session picker | 
| Background server default | Still on ( `daemon_auto_start` ), the flag is for this view | 

Codex CLI 0.167.0, released on November 6, 2026, adds an agent command center that you open with one command: `codex agents --no-daemon`. It marks results you have not read, keeps your draft when you switch backends, and previews local image attachments while you write. There is no new model in this release. What changed is supervision: one place to watch several coding agents at once, so you stop re-checking every session by hand.

This article is for developers who run Codex CLI for real work and have started leaving two or three agents running in parallel. If you split your work between the two tools, how we use Claude Code and Codex CLI together is covered in its own piece.

The release notes list the command center first: "Open the agent command center with `codex agents --no-daemon`. The command center and session picker now mark unread results."

The same release adds, in its own words:

- Switch between connected backends from the terminal session picker and keep your drafts when switching.
- Preview local image attachments while composing on supported terminals.
- Expand patch details directly in the transcript while an approval prompt is open.
- Saved chats stay visible after you switch model providers; if a chat cannot resume, you can read its transcript.
- 
`Ctrl+L` clears the terminal display while keeping earlier messages available through`Ctrl+T` .
- Google Cloud Vertex AI setup sits behind the `gcp_setup_wizard` flag.

One agent is easy to watch. Run three or four in parallel and the slow part stops being typing; it is remembering what each one finished and which result you have not seen yet. Switching to a terminal and scrolling back is a small job, and doing it four times is the whole job. That is the problem the command center solves: one screen, every agent, the ones with unread results marked.

| Running agents | What costs you time | 
|---|---|
| 1 | Waiting for it | 
| 3 to 4 | Finding which one finished and what you have not read | 
| 3 to 4 with unread markers | Opening the marked ones first | 

Unread markers in two places, the command center and the session picker, turn "check every session" into "check the marked ones". It is not work being done faster, it is work you no longer repeat.

```
npm install -g @openai/codex@0.167.0
codex --version  # codex-cli 0.167.0
codex agents --no-daemon
```
Start two or three short tasks in separate sessions first, so the command center has something to show.

Three smaller lines in the release notes are worth a look, because each one removes a daily annoyance.

- 
**Patch details inside the approval prompt.** You can expand what a change does before you approve it, instead of approving blind or scrolling back through the transcript.
- 
**A transcript when a chat cannot resume.** After switching model providers, saved chats stay visible, and a chat that cannot resume still shows you its transcript.
- 
**Ctrl+L with Ctrl+T.** Ctrl+L clears the screen, and earlier messages stay one keystroke away through Ctrl+T. A clean screen no longer means a lost history.

None of these needs a setting; they start working as soon as you update.

Keeping that history one keystroke away matters, because it is the record the session continues from — why an agent re-sends its history.

Since 0.157.0, interactive sessions start a shared background server automatically. The `--no-daemon` option opens this one view without relying on that server. It does not change the default: the `daemon_auto_start` flag is still on, and `codex features disable daemon_auto_start` is the setting for every session. We cover that check in our 5-minute background server audit.

Most release coverage will skip this one because there is no new model in it. The shift it shows is more useful: coding agents are moving from one long chat to a team you supervise. A command center with unread markers is the first interface that treats them that way. If you only run one agent at a time, this release is a minor update. If you run three, it is the first version that respects how you actually work.

Good: one command to see everything, unread markers where you look, drafts kept across backends, and patch details before you approve. Worth knowing: image previews depend on your terminal, the Vertex AI setup is behind a flag, and the background server default is unchanged, so read --no-daemon as "this view only", not "server off".

- 
**One agent at a time** : update when convenient. You will notice the image preview and the cleaner picker.
- 
**Two or more agents** : update and make`codex agents --no-daemon` your first check when you return to your desk. Open unread results first, then pending approvals.
- 
**Teams** : test draft keeping on your exact backend mix, and decide the background server default once, in writing, before the team updates.

Update to Codex CLI 0.167.0 or later and run `codex agents --no-daemon`. It shows your agent sessions and marks results you have not read yet.

It runs the agents overview without relying on the shared background server, which Codex CLI starts automatically since 0.157.0. It does not change that default; `codex features disable daemon_auto_start` does.

Yes, from 0.167.0 you can switch between connected backends from the terminal session picker and keep your drafts when switching.

No. It opens this one view without relying on the shared background server. The default stays on, and `codex features disable daemon_auto_start` is the command that changes it for every session.

## Top comments (0)
