---
id: collect-261121-veille-ia/gemini-google/what-changed-in-google-antigravity-s-november-update-remote-agents-voice-mode-and-a-native-jupyter-v
title: "What Changed in Google Antigravity's November Update: Remote Agents, Voice Mode, and a Native Jupyter Viewer"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-03"]
keywords: ["google", "remote", "antigravity", "work", "agents", "voice", "mode", "november"]
source: https://dev.to/moruku36/what-changed-in-google-antigravitys-november-update-remote-agents-voice-mode-and-a-native-1ph9
source_anchor: ""
source_lines: [1, 53]
section: "Gemini_Google"
---
On November 3, 2026, Google Antigravity shipped an update that moves agent work into the cloud — and lets you talk to it.

Until now, an agent session lived and died with your laptop. Close the lid, and the work stopped. A permission prompt waited for your click.

This release goes after both.


**Remote agents**: conversations run in the cloud, even with your laptop closed (Google AI Pro and Ultra, rolling out)
**Voice mode**: talk to the agent via Gemini Live, and answer permission prompts out loud (rolling out)- A native Jupyter notebook viewer
- A redesigned Automations page, plus downloads and history in the built-in browser
- Partner plugins in the Marketplace: Canva, Figma, Linear, Notion, Stripe, Supabase, Vercel

Source: the official changelog entry for November 3, 2026.

Conversations in a cloud project now run in an isolated, Google-managed environment — not on your machine. Close the browser or sleep the laptop; long tasks keep running.

You feed work through resources: GitHub repositories, Google Drive files, or uploads. Start from the app, the CLI, or antigravity.google.com/remote. A personal account on Google AI Pro or Ultra is required.

**Common misconception:** this is not remote control of your own PC. That is the separate Remote Control feature, and it needs your machine on and online. Remote agents fit repository-scoped work. Tasks that depend on uncommitted local changes or a local dev server are a poor fit.

Voice mode runs on Gemini Live. Click **Start Voice mode** in an empty message box, pick one of nine voices, and talk.

You can hand off a task out loud, steer it while it runs, and answer permission prompts by voice. Its work still appears in the conversation to read along.

**Common misconception:** this is not dictation. Dictation turns speech into text. Here the conversation itself is spoken, and a transcript is saved when it ends.

`.ipynb` files now open inside the app, with code cells, Markdown, tables, images, and outputs rendered. It is a viewer only — no built-in way to run cells.

The Automations page now groups items into Scheduled, Always on, and Paused, with Next run and Last run times and a full-page creation form.

The built-in browser improved too: downloads save without prompts, history powers address-bar suggestions, and syntax highlighting now loads on demand across 173 languages.

The Marketplace added partner plugins: Canva, Figma, Linear, Notion, Stripe, Supabase, and Vercel.

The same day, DeepMind released Cheminformatics Skills through the Science plugin. Built on RDKit, with ChEMBL and PubChem connectors, they search compounds, run matched molecular pair analysis, and propose new molecular designs.

Antigravity has no separate price. Your Google AI plan sets the limits (Plans, Pricing). Prices are US prices.

| Plan | Price | Quota | 
|---|---|---|
| Free | $0 | A quota refreshed weekly. Gemini models. Local-first | 
| Google AI Pro | $19.99/mo | Refreshes every five hours, up to a weekly limit. Claude Sonnet 5.5 / Opus 5.5 (non-trial). Remote agents | 
| Google AI Ultra | $99.99 / $199.99/mo | 5x / 20x the Pro quota | 

Quota scales with how much work the agent does. Google does not publish exact numbers.

- Both headline features are rolling out gradually. An update does not guarantee you will see them yet.
- Remote agents need a personal account on a paid plan. Some plugin features are not yet available for business accounts.
- Jupyter support is view-only.

Execution is moving off your machine. Input is moving off the keyboard. Antigravity is shifting from a tool you use while seated to an agent you hand work to.

## Top comments (0)
