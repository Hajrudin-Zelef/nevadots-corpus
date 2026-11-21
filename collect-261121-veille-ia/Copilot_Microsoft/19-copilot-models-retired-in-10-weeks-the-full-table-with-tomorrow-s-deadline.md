---
id: collect-261121-veille-ia/copilot-microsoft/19-copilot-models-retired-in-10-weeks-the-full-table-with-tomorrow-s-deadline
title: "19 Copilot Models Retired in 10 Weeks — The Full Table, With Tomorrow's Deadline"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["opus", "replacement", "claude", "gemini", "gpt-6", "model", "named", "flash"]
source: https://dev.to/aheadkit/19-copilot-models-retired-in-10-weeks-the-full-table-with-tomorrows-deadline-h5l
source_anchor: ""
source_lines: [1, 82]
section: "Copilot_Microsoft"
---
Ten models are already gone. Three more disappear tomorrow, November 16. Six more go on December 14. That's 19 models retired from GitHub Copilot in ten weeks, across four waves. If you picked a model deliberately at any point this year, there's a real chance yours is on this list.

Yesterday I wrote about the December wave only. This is the full picture: every model, every date, every named replacement, each row read from GitHub's own changelog posts. Nothing here comes from a roundup or a summary. The links are at the bottom, and you can check any row in about ten seconds.

| Model | Named replacement | 
|---|---|
| Gemini 3.5 Flash | Gemini 3.8 Flash | 
| Gemini 3.6 Flash | Gemini 3.8 Flash | 
| Kimi K2.7 Code | Kimi K3 | 
| Claude Opus 4.7 | Claude Opus 5.5 | 

| Model | Named replacement | 
|---|---|
| Gemini 3.7 Flash | Gemini 3.8 Flash | 
| GPT-5.5 | GPT-6.1 Sol | 
| GPT-5.4 | GPT-6.1 Sol | 
| GPT-5.4 mini | GPT-6 Luna | 
| GPT-5 mini | GPT-6 Luna | 
| Grok 4.5 | Grok 4.7 | 

One warning about this wave: GitHub's first announcement, on September 18, named different replacements: GPT-5.6 Sol, GPT-5.6 Luna and Grok 4.6. By the retirement day itself, the names had moved to GPT-6.1 Sol, GPT-6 Luna and Grok 4.7. A few write-ups still quote the September names. The table above uses the final ones from the October 19 post. And yes, "GPT-6 Luna" really has no ".1". That's how the changelog spells it, and I'm reproducing it as written rather than tidying it into something it isn't.

| Model | Named replacement | 
|---|---|
| Claude Opus 4.8* | Claude Opus 5.5 | 
| Claude Opus 5 | Claude Opus 5.5 | 
| GPT-5.6 Terra | GPT-6.1 Sol or GPT-6 Luna | 

* Claude Opus 4.8 stays available to individual Copilot subscribers on annual plans, so they keep an Opus option. The changelog says so word for word. That exception is only for Opus 4.8. Opus 5 and GPT-5.6 Terra go for everyone.

Note the small trap here: Opus 5's replacement is Claude Opus 5.5. I found at least one checklist online mapping Opus 5 to GPT-6.1 Sol or Luna. That's wrong, and it's the kind of mistake that comes from reading a summary instead of the changelog. The Sol/Luna choice belongs to GPT-5.6 Terra only.

| Model | Named replacement | 
|---|---|
| Claude Fable 5 | Claude Fable 5.2 | 
| Claude Haiku 4.5* | Claude Haiku 5.5 | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | 
| Gemini 4 Argon | Gemini 4 Carbon | 
| Grok 4.6 | Grok 4.7 | 
| Kimi K3 | To be announced | 

* Same deal as Opus 4.8: Claude Haiku 4.5 stays for individual subscribers on annual plans. Everyone else loses it on December 14.

Read the first row of this wave carefully, because it's the one people keep getting backwards: Fable **5** is the model going away, and Fable **5.2** is the replacement. If you've seen 5.2 mentioned next to this deprecation, that's why. It's the answer, not the problem.

Look at the replacements column across all four waves, and three of the names in it are themselves on the retirement list.

Gemini 3.8 Flash was the named replacement in waves 1 and 2, the safe harbour for everyone fleeing the 3.5, 3.6 and 3.7 Flash models. It retires on December 14, replaced by Gemini 4 Hydrogen. Kimi K3 was the wave-1 replacement for Kimi K2.7 Code. It also retires on December 14, and GitHub hasn't named a replacement for it yet. "To be announced" is the entire entry. Grok 4.6 was the September replacement for Grok 4.5, and it retires December 14 too, replaced by Grok 4.7.

So the churn has churn inside it. Switching to the named replacement is the right move. It's what GitHub tells you to do. But it's worth knowing that the replacement you switched to in October might be the model you switch away from in December. The half-life of a "safe" model choice right now is about two months.

That also reframes what the December 14 wave really is. It's not six fresh retirements. Three of the six are models GitHub itself pointed people toward in the previous two months. The churn isn't just fast; it's eating its own answers.

The models stop being selectable. Anywhere you pick a model in Copilot, the Chat selector, agent mode, inline edits, the names simply won't be there. GitHub's wording in every post is that no action is required to remove the models after deprecation, which is the polite way of saying they'll vanish on their own.

What doesn't happen automatically is your side of it. If anything you own names one of these models explicitly, a config, an integration, a pinned setting, it won't switch to the replacement by itself. It'll break, or fall back to whatever your default is, depending on how it's written. That's the real work: finding the places where a model name is written down.

There's one more wrinkle for teams. Under default model enablement, the suggested replacements are automatically enabled for Copilot Enterprise and Copilot Business customers, unless an administrator turned off the global default or explicitly disabled the model at some point. If someone on your team did that, the replacements won't appear on their own. You enable them through the model policies in Copilot settings. Do it before the date, not the morning after, or your team spends the morning asking where their model went.

**Today, if you use Opus 5, Opus 4.8 or GPT-5.6 Terra:** tomorrow is the deadline. Open the model selector and try Opus 5.5 now, or GPT-6.1 Sol or GPT-6 Luna for Terra. These replacements are already available; this is a deprecation with a month's notice, not a launch. If one of them behaves differently on your kind of work, you want to find out today, not tomorrow morning.

**If you use Kimi K3:** you're the one with a genuine problem. No replacement is named, which means there's nothing to switch to yet. GitHub will have to name something before December 14, or Kimi users choose blind from the other five replacements. Start trying the alternatives now so you're not choosing on the deadline.

**If you use Gemini 3.8 Flash:** you've already switched once, in October. December 14 asks you to do it again, to Gemini 4 Hydrogen. Hydrogen only launched on November 12, with a 1-million-token context window and listed pricing of $1.50 per million input tokens and $7.50 per million output tokens. If you used Flash because it was quick and cheap, Hydrogen is meant to be the same trade. A week of real use will tell you whether it feels that way.

**If you manage Copilot for a team:** check your model policies now, and search your repositories and config files for the nineteen names. Anything that passes a model name to Copilot, a pinned setting or a config, needs updating to the replacement. This is the step most people skip, and it's the one that actually breaks.

**If you never picked a model yourself:** you'll probably be fine. If you just opened Copilot Chat and typed, you're on whatever default GitHub set, and defaults get updated for you. Check anyway, because "I never chose" is exactly the kind of thing you only remember getting wrong after the fact.

Pinning your setup to a specific model name is a bet with a roughly one-month expiry date right now, and the house always collects eventually. Four waves in ten weeks, nineteen models, and three replacements that are themselves retiring. That's not a one-off cleanup, that's the pace. If you can, point your tools at a role, "fast model" or "strong model", rather than a name. Where you can't, put a reminder in your calendar for the next changelog. December 14 is a month away. The next one will be too, and you'll want the same head start.

Every row in the tables above was read directly from GitHub's changelog:

- Wave 1, October 2: Selected models in GitHub Copilot deprecated
- Wave 2, October 19 (authoritative replacements): GitHub Copilot models deprecated on October 19, 2026. The September 18 announcement named superseded replacements; the October 19 post is the final word
- Wave 3, November 16: Upcoming deprecation of selected GitHub Copilot models in mid-November (announced October 15; there is no October 15 retirement wave)
- Wave 4, December 14: Upcoming deprecation of selected GitHub Copilot models in December
- Cross-check: pondero.ai's deprecation mapping agrees on all 19 rows

*P.S. Everything I write stays free — that part doesn't change. But if this one was worth something to you, you can buy me a coffee ☕ (one-time or monthly, both work). It covers the API credits and hosting behind these experiments — and honestly, it's also the nudge that gets the next one started on the evenings I'd otherwise skip it. — Salam*

## Top comments (0)
