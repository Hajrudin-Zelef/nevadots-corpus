---
id: collect-261121-veille-ia/muse/metas-muse-ai-agent-is-raising-privacy-questions-early
title: "Meta’s Muse AI Agent Is Raising Privacy Questions Early"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["reuters", "agent", "muse", "metas", "permissions", "privacy", "message", "text"]
source: https://finimize.com/content/metas-muse-ai-agent-is-raising-privacy-questions-early
source_anchor: ""
source_lines: [1, 23]
section: "Muse"
---
Reuters found cases where the fast-growing bot pulled in message text via broad device permissions and sometimes mixed up context between accounts.

**What's going on here?**

Meta’s consumer AI agent Muse has passed 10 million downloads since launching in September. But Reuters found cases where broad device permissions let it pull in message text and sometimes mix up context between accounts.

**What does this mean?**

Muse is central to Meta, the social media giant, and its push to make money from AI, and Reuters says the app has stayed high in download charts after promotion across Facebook, Instagram, and WhatsApp. Meta has said about a quarter of new users return daily.

Internal staff posts reviewed by Reuters describe “undesired actions”, from made-up purchase details to odd, out-of-context messages that employees feared could point to c..

int to cross-account data bleeding. One example appended supposed Capital One and Charles Schwab balances; another showed a reminder tied to a Signal message that an employee called an “extreme privacy violation.”

Reuters, working with Signal staff and the AI Now Institute, says it reproduced a way for Muse to read Signal text without Meta’s official “Connector” feature, by relying on setup permissions instead: push-notification access on Android, and on macOS a mix of accessibility and screenshot tools. The same request was blocked on an iPhone, and Apple restricted the pathway Reuters identified while adding more agent-specific permission prompts. Meta spokesperson Daniel Roberts told Reuters the issues were isolated and that many mix-ups were hallucinations that have since been addressed. The bigger point: consumer “agents” are quickly forcing operating systems to redraw what apps can see.

**Why should I care?**

**For you:***Encryption protects messages in transit not what pops up on screen.*

End-to-end encryption helps keep messages private as they move between apps, but it can’t hide what your device displays. If an AI agent gets broad permissions at setup, it may capture plaintext from endpoints like Android notification previews or whatever macOS accessibility and screenshot tools can “see”, even without an in-app connection.

That’s why iPhone-level blocking, and Apple’s new granular prompts for agent access, matters. If Apple and Google tighten defaults further, you may face more permission pop-ups for automations, but fewer paths for a helper bot to ingest or repeat sensitive snippets.
