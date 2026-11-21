---
id: collect-261121-veille-ia/cursor-ide/talk-to-your-coding-agent-what-cursor-s-realtime-voice-chat-actually-changes
title: "Talk to Your Coding Agent: What Cursor's Realtime Voice Chat Actually Changes"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["voice", "agent", "chat", "cursor", "coding", "dictation", "call", "work"]
source: https://dev.to/yehechuhai/talk-to-your-coding-agent-what-cursors-realtime-voice-chat-actually-changes-4bc1
source_anchor: ""
source_lines: [1, 42]
section: "Cursor_IDE"
---
Cursor's November 6 changelog entry is short — "Voice Chat with Agents" — but it changes the shape of a coding session. You can now talk with your agents in a realtime voice call: describe a change out loud, ask what an agent is working on, and hear updates as the work moves forward. Here is what the feature actually does, where it works, and where voice helps — and where it does not.

Voice chat works in the Agents Window, the IDE, on the web, and in Cursor for iOS. To start a call, wait until the agent is idle and the chat input is empty, then click the waveform button labeled **Start voice chat**. It sits where Send usually is, next to the microphone button for dictation.

On iOS, you can also say "Hey Siri, call Cursor" to start a call with a new agent, and mute or hang up from the Lock Screen with a Live Activity.

The important part is that the conversation does not stop when the agent starts working. While that agent runs, you can keep talking: ask what changed in a diff, check on a run, search earlier messages and calls with this agent, or switch between workers in a Project that has them. The voice agent listens, replies, and hands coding work to the agent in your chat.

The coding agent also speaks for itself. It shares short progress notes while it works and a brief answer when it is done, so you usually hear a short update instead of the full written reply. That design choice matters: voice is a summary channel, not a transcript channel.

Voice chat runs on Grok Voice. You can pick from many voices in Voice settings, including Ara, Eve, Leo, Rex, and Sal. With Privacy Mode on, voice chat runs with zero data retention — a detail worth noting if you work in a codebase with sensitive code or client data, since voice input is one more surface where data can leak.

Cursor is explicit that voice chat and dictation are two separate features. Dictation, which earlier releases called Voice Mode, still turns your speech into text in the chat input, and Cmd+Shift+Space still toggles it in the IDE. Dictation is speech-to-text; voice chat is a two-way conversation where the agent answers out loud.

That distinction decides which one to reach for. If you want to write a precise prompt — a function signature, an exact error message, a file path — dictation puts your words into the box and you can edit them before sending. If you want to stay in flow while an agent works — asking what it just changed while you look at something else — voice chat keeps the loop open without pulling your hands back to the keyboard.

Three situations benefit most:

**1. Reviewing while the agent works.** Asking "what changed in that diff?" out loud while the run is still going turns dead waiting time into review time. You are interrogating the work as it lands, instead of reading a finished summary after the fact.

**2. Hands-busy moments.** Walking through a design decision, standing at a whiteboard, or working on a second machine — anywhere typing a follow-up would break your train of thought, a spoken question keeps the agent moving.

**3. Multi-worker projects.** Switching between workers by voice, and searching earlier messages and calls with an agent, makes the conversation history a working memory you can query instead of a log you have to scroll.

A few honest limits:

- 
**You will not hear the full answer.** The agent deliberately gives short updates and a brief answer when done. If the change is subtle — a one-line logic fix, a race condition, a security-sensitive edit — you still need to read the diff. Voice is for orientation, not verification.
- 
**Voice is a bad fit for precision.** Spoken file paths, identifiers, and error strings are error-prone. Use dictation or typed input when exactness matters.
- 
**Check the privacy setting.** Zero data retention only applies with Privacy Mode on. If your work involves proprietary code, confirm the setting before your first call, not after.
- 
**It is a new surface for misunderstandings.** A spoken instruction is harder to review before it is acted on than a typed prompt. Keep voice instructions short and verifiable, and check what the agent did — the same discipline as any agent workflow, just without the paper trail in front of you.

Cursor shipped app building in the same week (its November 5 changelog lets you describe an app and get a deployed, shareable link), and voice chat is the other half of that direction: less time in the editor, more time directing. The IDE is becoming a place where you supervise agents rather than type every keystroke yourself — and the interface for supervision is starting to be a conversation, not a text box.

Whether that is faster depends on the task. For exploratory work, long refactors, and review-while-it-runs, talking to the agent is plausibly quicker than typing. For precise, surgical edits, the keyboard is not going anywhere. Update Cursor to try voice chat — and start with a task where a short spoken answer is enough.

*Source: Cursor changelog, "Voice Chat with Agents" (November 6, 2026): https://cursor.com/changelog/voice-chat — re-check the original for the latest platform availability and voice options.*

## Top comments (0)
