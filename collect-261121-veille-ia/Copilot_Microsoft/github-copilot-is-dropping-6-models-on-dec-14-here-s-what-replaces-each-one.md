---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-is-dropping-6-models-on-dec-14-here-s-what-replaces-each-one
title: "GitHub Copilot Is Dropping 6 Models on Dec 14 — Here's What Replaces Each One"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["model", "copilot", "github", "models", "replacement", "fable", "claude", "haiku"]
source: https://dev.to/aheadkit/github-copilot-is-dropping-6-models-on-dec-14-heres-what-replaces-each-one-1g8c
source_anchor: ""
source_lines: [1, 51]
section: "Copilot_Microsoft"
---
On December 14, 2026, six models disappear from GitHub Copilot. Not one at a time, not with a warning in the editor. All six, on the same day, across every Copilot experience: Chat, inline edits, ask and agent modes, and code completions.

GitHub posted the list on November 13. That gives you a month. Here's the full list, what replaces each one, and what to actually do before the date, because one of these replacements doesn't exist yet, and one of the models on the list has an exception that saves some of you.

| Model going away | Dec 14 replacement | Notes | 
|---|---|---|
| Claude Fable 5 | Claude Fable 5.2 | Straight upgrade | 
| Claude Haiku 4.5 | Claude Haiku 5.5 | See the exception below | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | The new default in Search and AI Mode | 
| Gemini 4 Argon | Gemini 4 Carbon | Straight upgrade | 
| Grok 4.6 | Grok 4.7 | Straight upgrade | 
| Kimi K3 | To be announced | No replacement named yet | 

Read the first row carefully: it's Fable **5** going away, and Fable **5.2** is the replacement. If you've seen Fable 5.2 mentioned in the same breath as this deprecation, that's why. It's the answer, not the problem. Same pattern down the list: every model gets a newer sibling, except Kimi K3, whose replacement GitHub hasn't named.

Claude Haiku 4.5 stays available for one group: individual Copilot subscribers on annual plans. If that's you, the deprecation doesn't apply. GitHub says so in a footnote on the announcement. Everyone else loses Haiku 4.5 on December 14, same as the rest.

If you're not sure which plan you're on, that's the first thing to check. Don't assume. The difference between "my model is gone" and "nothing changes for me" is a billing page.

The models stop being selectable. Anywhere you pick a model in Copilot: the Chat model selector, agent mode, inline edits — the six names will simply not be there anymore. GitHub's wording is that no action is required to remove the models after deprecation, which is the polite way of saying they'll vanish on their own.

What doesn't happen automatically is your side of it. If you have workflows, scripts, or integrations that name one of these models explicitly, they won't magically switch to the replacement. They'll break, or fall back to whatever your default is, depending on how they're written. That's the real work of the next month: finding the places where a model name is written down.

**If you use Copilot as an individual:** open the model selector and try the replacement now. The replacements are already available. This is a deprecation with a month's notice, not a launch. Spend a week with Fable 5.2, Hydrogen, Carbon, Grok 4.7 or Haiku 5.5, whichever matches what you use. If one of them behaves differently on your kind of work, you want to find out in November, not on the morning of the 14th.

**If you use Kimi K3:** you're the one with a genuine problem. No replacement is named, which means there's nothing to switch to yet. Keep an eye on the changelog. GitHub will have to name something before December 14, or Kimi users will be left choosing from the other five replacements with no guidance. If Kimi is your daily driver, start trying the alternatives now so you're not choosing blind.

**If you manage Copilot for a team:** check your model policies. Under default model enablement, the suggested alternatives are automatically enabled for Copilot Enterprise and Copilot Business customers — unless an administrator has turned off the global default or explicitly disabled the model. If someone on your team turned the global default off at some point, the replacements won't appear on their own. You enable them through the model policies in Copilot settings, and then users see them in the Chat model selector. Do that before the 14th, not after, or your team will spend the morning asking where their model went.

**If you have automation that names a model:** search your repositories and workflow files for the six names. Anything that passes a model name to Copilot: a script, a config, a pinned setting — needs updating to the replacement. This is the step most people will skip, and it's the one that actually breaks.

The most interesting swap on the list is Gemini 3.8 Flash to Gemini 4 Hydrogen, because Hydrogen only launched on November 12, two days before this announcement. Google's new model replaces Flash as the default in Search and AI Mode, with a 1-million-token context window and listed pricing of $1.50 per million input tokens and $7.50 per million output tokens. Google also reports big jumps on its own benchmarks: Terminal-Bench 4.1 from 18.2% to 57%, and 2.5× faster task completion.

That's the pattern to notice. Flash was the fast, cheap, everywhere model. Hydrogen is being positioned as the same thing, only newer. If you used Flash because it was quick and cheap, Hydrogen is meant to be the same trade. Whether it actually feels that way in your editor is something only a week of real use will tell you — which is another reason to switch early rather than on the deadline.

Most people will do nothing, and for most people nothing bad will happen. If you never picked a model yourself. You just opened Copilot Chat and typed — you're on whatever default GitHub set, and defaults get updated for you. You'll open the editor on December 15 and everything will look the same, except the model answering you has a different name.

The people who get bitten are the ones who chose deliberately: the developer who switched to Haiku 4.5 because it was fast on their codebase, the team that standardised on Grok 4.6, the script that pins Kimi K3 by name. If you chose, you need to choose again. If you never chose, you're fine — but check anyway, because "I never chose" is exactly the kind of thing you only remember getting wrong after the fact.

A practical timeline, if you want one: this week, open the selector and try your replacement for a day. Next week, update anything that names the old model: configs, scripts, docs. December 13, check the selector one last time. That's it. Three small jobs, spread over a month, and December 14 becomes a non-event.

Model deprecations in Copilot aren't a one-off. The shape is always the same: a month's notice, a table of replacements, one model with a "to be announced." The lesson isn't about any single model. It's that pinning your workflow to a specific model name is a bet with a one-month expiry date, and the house always collects eventually.

The practical version of that lesson: if you can, point your tools at a role: "fast model," "strong model" — rather than a name. Where you can't, put a reminder in your calendar for the next changelog. December 14 is a month away. The next one will be too, and you'll want the same head start.

- The full deprecation table, the Haiku 4.5 exception, and the admin guidance: Upcoming deprecation of selected GitHub Copilot models in December (GitHub Changelog, Nov 13, 2026)
- Gemini 4 Hydrogen launch details and pricing: Google Gemini 4 Hydrogen launch coverage and KnowFlowHub

*P.S. Everything I write stays free — that part doesn't change. But if this one was worth something to you, you can buy me a coffee ☕ (one-time or monthly, both work). It covers the API credits and hosting behind these experiments — and honestly, it's also the nudge that gets the next one started on the evenings I'd otherwise skip it. — Salam*

## Top comments (0)
