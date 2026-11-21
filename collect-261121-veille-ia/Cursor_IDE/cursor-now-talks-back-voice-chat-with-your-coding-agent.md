---
id: collect-261121-veille-ia/cursor-ide/cursor-now-talks-back-voice-chat-with-your-coding-agent
title: "Cursor Now Talks Back: Voice Chat With Your Coding Agent"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: ["2026-11-07"]
keywords: ["voice", "agent", "cursor", "chat", "coding", "call", "loud", "talks"]
source: http://dev.to/paullovelace/cursor-now-talks-back-voice-chat-with-your-coding-agent-4hnb
source_anchor: ""
source_lines: [1, 44]
section: "Cursor_IDE"
---
Cursor shipped Voice Chat with Agents on 6 November 2026, and it is the most human thing the editor has done in a while. You can now talk to your coding agent out loud, in real time, and it talks back — short spoken progress notes while it works, and a brief spoken answer when it finishes, instead of making you read the whole written reply.

I have not used it hands-on, and I am not going to pretend otherwise. Everything below is what Cursor published in its changelog and voice-chat docs, checked 7 November 2026. The honest hands-on review — the one where someone actually talks through a real debugging session, hits the rough edges, and finds out where the voice gets in the way — does not exist yet. This is the explainer that tells you what it is, how to start it, and where it stops.

A realtime two-way voice call with the coding agent, inside Cursor. It works in the Agents Window, the IDE, on web, and in Cursor for iOS. You describe a change out loud, ask what the agent is doing, and hear progress while it works. The voice agent listens, replies, and hands the coding work to the agent in your chat. While that agent runs, you can keep talking — ask what changed in a diff, check on a run, search earlier messages and calls, or switch between workers in a Project that has them.

The coding agent speaks for itself too. Short progress notes while it works, then a brief spoken answer when it is done. That is the design choice that matters: voice is a terrible medium for long answers, so Cursor does not read you the full reply. You get the summary out loud, and the detail stays in the chat where you can read it.

Under the hood, the voice part runs on Grok Voice. You pick a voice in Voice settings — Ara, Eve, Leo, Rex, and Sal are the named options. With Privacy Mode on, voice chat runs with zero data retention, which is the one line in the docs I would actually read twice before using this on client code. And the coding agent keeps whatever model you selected — Grok Voice only handles the speech, not the coding.

It is one button, in the place you would expect. When the agent is idle and the chat input is empty, the waveform button labelled "Start voice chat" sits where Send usually sits. Click it and you are in a call.

On iOS there is a hands-free route: say "Hey Siri, call Cursor". Lock Screen and Dynamic Island controls arrive in iOS 1.16.0, so you can manage the call without unlocking the phone.

One thing to get straight, because the naming invites confusion: this is not dictation. Dictation — earlier called Voice Mode — still just turns your speech into text in the chat input, and Cmd+Shift+Space toggles it in the IDE. Voice chat is a two-way conversation where the agent answers out loud. Dictation is you talking at the keyboard. Voice chat is you talking to the agent.

Every new feature has a catch, and Cursor published this one's limits unusually honestly. Here they are:

- 
**You need a signed-in Cursor account.** API-key sessions cannot start voice chat at all. If you run Cursor on an API key, this feature simply does not exist for you.
- 
**There is a daily call limit.** Cursor has not published the number. That is a real gap — "a daily limit" with no number attached means you cannot plan around it, and heavy users will only discover it by hitting it.
- 
**About 10 minutes of silence hangs up by default** when the agent is idle. Walk away mid-call and it ends itself. Sensible, but worth knowing before you leave a call open while you think.
- 
**The start button is hidden in a lot of places.** Subagents and Project workers, archived chats, Claude Code chats, and chats owned by another teammate — none of them show it. If you cannot find the button, you are probably in one of those.
- 
**No new plan price was announced with the launch.** Voice chat is not a paid add-on announcement; it is a feature inside Cursor as it stands. Do not let anyone sell you a "voice plan" that does not exist.

**If you think out loud while you code,** this is built for you. Some people reason by talking — rubber-duck debugging, but the duck answers back and then edits the code. Describing a change in words, hearing what the agent is doing while it does it, asking about a diff without switching windows: that is a genuine workflow, not a demo trick.

**If you code away from the keyboard,** it matters even more. On iOS, or with your hands full, or on a walk while the agent runs a long task — a voice call is the only interface that works there. The Siri shortcut and the Lock Screen controls are the tell: Cursor knows this feature's best moment is when you are not sitting at the desk.

**If you are the kind of person who reads every word of every agent reply,** you will probably turn it off. Spoken summaries are lossy by design. If your workflow is "read the diff, read the reasoning, then decide," a voice that tells you the short version is a slower path to the same information. And if you work in a shared office or an open plan, talking to your editor out loud is a social question as much as a technical one.

Two days, two Cursor launches that point in the same direction. Build Live Apps on 5 November said the editor wants to be where things get shipped. Voice Chat on 6 November says the editor wants to be something you talk to, not just type at. Cursor is betting that the next interface for coding agents is a conversation — spoken, interruptible, running in the background while the work happens.

It is a reasonable bet, with one honest caveat: I have not heard it work. Voice quality, latency, how well Grok Voice handles accents and technical vocabulary, whether the spoken summaries are actually useful or just pleasant — none of that is knowable from a changelog. The feature is real, the limits are published, and the daily call limit is unpublished, which is the one number I want before I would lean on this for a full workday.

So here is my question: if you use Cursor, would you actually talk to your agent — or is typing still faster than explaining something out loud? I am genuinely curious, because that answer decides whether this is a new default or a feature you try once on a Friday afternoon.

*Sources: Cursor changelog, 6 November 2026 — "Voice Chat with Agents" (Cursor changelog); Cursor docs — Voice Chat (cursor.com/docs/agent/voice-chat); Fulcrum Insights AI brief, 7 November 2026. Facts checked 7 November 2026. No hands-on testing was performed for this article. The daily call limit number is not published by Cursor and is stated as unknown here, not invented. No new plan price was announced with this launch. This breaking article is not the daily buyer-intent post, and it contains no affiliate links.*

## Top comments (0)
