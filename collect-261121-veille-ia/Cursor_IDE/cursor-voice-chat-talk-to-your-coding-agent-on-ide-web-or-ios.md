---
id: collect-261121-veille-ia/cursor-ide/cursor-voice-chat-talk-to-your-coding-agent-on-ide-web-or-ios
title: "Cursor Voice Chat: Talk to Your Coding Agent on IDE, Web, or iOS"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agent", "chat", "voice", "cursor", "call", "coding", "talk", "docs"]
source: https://pondero.ai/coding/guides/cursor-voice-agent-calls-guide/
source_anchor: ""
source_lines: [1, 96]
section: "Cursor_IDE"
---
## Table of Contents

# Cursor Voice Chat: Talk to Your Coding Agent on IDE, Web, or iOS

You can now phone your coding agent. On November 6, 2026, Cursor shipped "Voice Chat with Agents": a realtime, two-way spoken call with whatever agent is running in your chat, available at once in the IDE, on the web, in the Agents Window, and in Cursor for iOS (per Cursor's changelog). You talk, it talks back, and it keeps working the whole time.

That is a different thing from dictation. Cursor already had a mic button that turns speech into typed text. This is a live conversation where the agent answers out loud, hands tasks to the coding model mid-call, and reports back while you keep talking about something else. Here is what shipped, how to turn it on, what it costs, and the four places it quietly won't show up.

## What actually shipped

Voice chat runs on Grok Voice, xAI's speech-to-speech model, while the coding agent in your chat keeps using whatever model you already picked for it (per Cursor's docs). During a call, the voice agent can hand off a task to the coding agent, answer read-only questions about a diff or a run, search that agent's message and call history, switch between workers in a Project, or just hang up when you say you're done.

The coding agent decides what is worth saying out loud. It speaks short progress notes while it works and a brief answer when it finishes; if it skips a spoken update for a given turn, the call reads the written reply instead (per Cursor's docs). You are not hearing every line of a long response. You are hearing the parts that would make you check your phone anyway.

The transcript lands in the chat as plain text, and cloud agent transcripts sync to your other devices while the call is live, unless Privacy Mode is on.

## How to start a call

Three steps, the same shape on every surface:

1. Open an agent chat. In the Agents Window, any chat works, new or existing. In the IDE, the chat needs at least one message already in it.
2. Clear the chat input. When the agent is idle and the input is empty, a waveform button labeled **Start voice chat** appears where Send normally sits, next to the separate microphone button for dictation.
3. Click it and allow microphone access when your OS or browser asks. The call connects, and the agent checks in after a few seconds if you don't start talking first.

On iOS there's a fourth path: say "Hey Siri, call Cursor" to start a call with a new agent without opening the app, then mute or hang up from the Lock Screen Live Activity or the expanded Dynamic Island. The compact Dynamic Island doesn't show those controls, so you have to tap through to the expanded view first (per Cursor's docs).

Once you're on a call, you talk the way you'd brief a teammate:

```
"Can you check what's going on with the checkout endpoint?
It's throwing a 500 on an empty cart. Reproduce it, add a
guard, and open a PR. Don't touch the payment flow."
```
The voice agent hands that straight to the coding agent and keeps the call open. A minute later you might hear:

```
Agent: "Found it. Empty cart skips the discount check.
Fix is up, running the test suite now."
```
That second block is the pattern Cursor's docs describe for a spoken progress note: short, specific, and timed to when you'd actually want an update, not a transcript of every tool call.

## Voice chat vs. dictation

Cursor has had speech-to-text dictation for a while, and the new feature doesn't replace it. The two sit side by side, and the docs lay out the difference plainly:

|  | Voice chat | Dictation | 
|---|---|---|
| Button | Waveform, "Start voice chat" | Microphone, "Start voice input" | 
| What you get | A spoken reply from the agent | Text dropped into the chat input | 
| Who sends the prompt | The voice agent hands off the work | You review the text, then send it yourself | 
| Shortcut | None | `Cmd+Shift+Space` in the IDE, hold`Ctrl+M` in the Agents Window | 

Source: Cursor docs, Voice chat and dictation, fetched November 8, 2026.

Dictation is the lower-friction choice when you just want to avoid typing a long prompt. Voice chat is the one worth reaching for when you want to check on a run, ask a follow-up, or redirect an agent without breaking stride to open the laptop.

## Who gets it and what it costs

Signing up is the only gate the docs actually name. Voice chat needs a signed-in Cursor account; a session authenticated with only an API key cannot start one, and if you try, the call fails with "Realtime voice requires sign-in" even though the button still shows (per Cursor's docs).

Cost is a messier question right now, because Cursor's pricing isn't really Cursor's pricing anymore. xAI folded Grok, Grok Bot, Cursor, and X Premium into a single plan called the X Subscription on October 12, 2026, with tiers replacing the old standalone Cursor ladder (per x.ai's announcement). Checking cursor.com/pricing on November 8, 2026 shows the current lineup: Free ($0), Plus ($8/month), Premium ($30/month), Super ($100/month), Ultra ($200/month), Teams (starting at $30/user/month), and a custom Enterprise tier. That is a full rebuild from the $20 Pro / $60 Pro+ / $200 Ultra structure Cursor ran as recently as July. Plus is the first paid tier, and its feature list on the pricing page specifically names "Voice" among the bundled Grok capabilities, where the Free tier's description stops at limited agent and chat requests.

Neither the pricing page nor the voice chat docs spell out a hard per-tier lock on the feature beyond that. If you're on Free and the waveform button doesn't connect, the account gate is the more likely explanation than the plan itself. Either way, budget for at least Plus if a live voice call with your agent is the reason you're signing up.

## Where it won't show up

The waveform button is conditional, and the gaps are specific enough to save you a bug report. Per Cursor's FAQ, **Start voice chat** stays hidden in:

- Subagents and Project workers
- Archived chats
- Claude Code chats opened inside Cursor
- Chats owned by a teammate, not you

In the IDE specifically, it's also hidden while a cloud agent is still streaming its response, and when dictation itself is turned off in settings. If none of those apply and the button is still missing, the fix in the docs is simply to update Cursor.

Two limits apply once you're actually on a call. A call hangs up automatically after about 10 minutes of silence from you while the agent has no work in progress, and there's a daily cap on voice calls that Cursor doesn't publish a number for. Hit it, and Cursor tells you when you can call again rather than letting the call silently fail (per Cursor's docs).

## Five voices, one privacy switch

Grok Voice ships with several named voices, and Cursor's changelog calls out five by name: Ara, Eve, Leo, Rex, and Sal (per Cursor's changelog). Pick a default in Voice or Agent Voice settings, or change it mid-call; a per-agent choice sticks to that specific agent instead of resetting to your default next time.

Privacy Mode is the one setting worth checking before you put anything sensitive through a call. Turn it on and voice chat runs with zero data retention, which also stops the cloud agent transcript sync to your other devices while the call is live. Teams accounts get a team-wide privacy mode toggle for admins who don't want that decision left to individual engineers.

## How this stacks up against the other voice-coding bets

Cursor isn't the first agent to pick up a microphone. OpenAI's Codex CLI shipped push-to-talk voice input back in version 0.105.0 on February 26, 2026: hold the spacebar, release it, and your words convert to text and get sent to Codex, with no audio reply (per x-cmd.com's coverage of the release). Anthropic followed with a Voice Mode for Claude Code starting March 3, 2026, rolled out to roughly 5% of users first; you type `/voice`, then speak commands like "refactor the authentication middleware," though reporting at the time didn't confirm whether Claude talks back (per TechCrunch).

Cursor's angle is the two-way call itself, plus the mobile telephony layer nobody else has matched yet: a Siri trigger, a Lock Screen Live Activity, and mute or hang-up controls that behave like an actual phone call rather than a voice-typing shortcut bolted onto a terminal.

## Should you turn it on

If you're already paying for Cursor at Plus or above, yes, with one low-stakes test first. Open a chat, clear the input, hit the waveform button, and give the agent something small like a one-line bug fix before you trust it with anything that matters. The commute-and-check-in workflow, where you kick off a fix at lunch and talk through the diff on the walk back, is where this earns its slot over just typing.

Handling anything you wouldn't want synced to a second device or retained anywhere? Flip on Privacy Mode before your first real call, not after. And if what you actually wanted was faster prompt entry rather than a conversation, skip the call entirely and use dictation; it's the simpler tool for that job.

Not on Cursor yet and curious whether this tips the decision? The feature alone isn't worth switching editors for, but paired with cloud agents and the mobile app, it's a reasonable nudge if you were already circling Cursor's plans.
