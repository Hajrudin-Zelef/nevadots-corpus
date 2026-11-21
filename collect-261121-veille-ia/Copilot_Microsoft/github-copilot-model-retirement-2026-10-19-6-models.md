---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-model-retirement-2026-10-19-6-models
title: "GitHub Copilot model retirement 2026-10-19: 6 models"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-19"]
keywords: ["copilot", "github", "models", "gpt-5", "claude", "opus", "model", "gemini"]
source: https://qcode.cc/en/github-copilot-model-retirement-2026-10-19
source_anchor: ""
source_lines: [1, 147]
section: "Copilot_Microsoft"
---
# GitHub Copilot retires six models on 2026-10-19: what replaces them, how to verify

As of 2026-10-19, GitHub's 2026-09-18 notice is unchanged: on 2026-10-19 six models leave every Copilot experience - Chat, inline edits, ask and agent modes, code completions - each with an official replacement, and the full six-row mapping is copied in highlight two. Before that, on 2026-10-02, GitHub posted «Selected models in GitHub Copilot deprecated» and removed Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code and Claude Opus 4.7 the same day. This page covers the date, which name disappears, which one replaces it, which line you edit, and how to prove it worked. On 2026-10-15 GitHub announced the next batch, deprecated on 2026-11-16 (Claude Opus 4.8*, Claude Opus 5 and GPT-5.6 Terra).

Updated 2026-11-18

One key for Claude, GPT and leading Chinese models, billed per token, up and running in minutes.

- Billed per token — live rates on /models
- Pay by card (Visa / Mastercard), Apple Pay, Google Pay or crypto
- Self-service: top up and activate instantly

## Key points

### six models, one date

The official sentence: “We will deprecate the following models across all GitHub Copilot experiences (including Copilot Chat, inline edits, ask and agent modes, and code completions) on October 19th, 2026”. That is the same day this page writes as 2026-10-19.

### The official table, row by row

Straight from the official table: Gemini 3.7 Flash → Gemini 3.8 Flash; GPT-5.5 → GPT-5.6 Sol; GPT-5.4 → GPT-5.6 Sol; GPT-5.4 mini → GPT-5.6 Luna; GPT-5 mini → GPT-5.6 Luna; Grok 4.5 → Grok 4.6. Every row carries the 2026-10-19 date.

### Alternatives switch on by default

The same sentence names Copilot Chat, inline edits, ask and agent modes, and code completions. Pins sitting in agent-mode settings or completions config are invisible if you only open the chat dropdown. The notice also states that under default model enablement the suggested alternatives are enabled automatically for Copilot Enterprise and Copilot Business, unless an admin turned off the global default or explicitly disabled the model.

### In effect from 2026-10-19, and nothing to clean up

Per the notice, suggested alternatives are automatically enabled for Copilot Enterprise and Business under default model enablement - unless an admin turned off the global default or explicitly disabled the model. Then nothing arrives on its own. This batch's mappings take effect on 2026-10-19; the notice also states that nothing has to be removed by hand after the retirements, so your work is replacing the old names still pinned in your configuration with the official alternatives.

## What this is

As of 2026-10-19, this page tracks three batches of in-product model turnover inside GitHub Copilot: the 2026-09-01 batch removed Claude Sonnet 4.5/4.6, Opus 4.5/4.6, Gemini 3.1 Pro and Raptor Mini (see related pages); the 2026-10-02 batch took effect the day it was posted and removed Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code and Claude Opus 4.7; the 2026-10-19 batch removes six non-Claude rows.

Two things to keep apart. First, Copilot names are product display names, not vendor API ids: Copilot's GPT-5.4 is not OpenAI's gpt-5.4-cyber (whose removal from the API OpenAI's deprecations page set for 2026-10-01), and Claude Opus 4.7, gone from Copilot, is still Active in Anthropic's deprecations table with a «not sooner than» date of 2027-04-16.

Second, GitHub will not rewrite pinned names for you: the notice says to update your workflows and integrations before the date, and afterwards nothing needs removing - No action is required to remove the models after they have been deprecated. One more thing: the 2026-11-16 batch was announced on 2026-10-15, and Claude Opus 4.8 is currently Active in Anthropic's deprecations table, with retirement no earlier than 2027-05-28; a footnote on that notice keeps Claude Opus 4.8 for individual subscribers on annual plans, and that deprecation does not apply to them.

## What happened

On 2026-09-18 (published 19:00 UTC) GitHub shipped a changelog entry under the copilot label - upcoming-deprecation-of-selected-github-copilot-models-in-mid-october - carrying the six-row table (Model / Deprecation date / Suggested alternative). Re-checked on 2026-10-19, the text and the six rows are unchanged.

In between, on 2026-10-02 (18:24 UTC), GitHub posted «Selected models in GitHub Copilot deprecated», which reads «As of today, October 2, 2026, we have deprecated the following models», and removed four that day: Gemini 3.5 Flash and Gemini 3.6 Flash to Gemini 3.8 Flash, Kimi K2.7 Code to Kimi K3, Claude Opus 4.7 to Claude Opus 5.5. On 2026-10-15 GitHub published a further notice announcing the next batch, deprecated on 2026-11-16.

## Timeline

2026-09-18: GitHub's changelog announces the 2026-10-19 deprecation of six Copilot models, with the suggested-alternative table and the auto-enable clause.

2026-10-02: GitHub posts «Selected models in GitHub Copilot deprecated» and removes Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code and Claude Opus 4.7 from every Copilot experience that day, with Gemini 3.8 Flash, Kimi K3 and Claude Opus 5.5 as alternatives.

2026-10-19: the six mappings take effect on the same day and all four surfaces lose the models at once. With default enablement the alternatives are available automatically; enterprises that switched off the global default have to enable them by hand in the model policy beforehand. Nothing has to be removed by hand after the retirements. The next batch is set for 2026-11-16 (announced by GitHub on 2026-10-15).

## Confirmed vs caution

### Officially confirmed

From the 2026-09-18 notice (unchanged on 2026-10-19): the 2026-10-19 date; the four experience surfaces; the six old-to-new mappings; automatic enablement of suggested alternatives for Enterprise/Business under default model enablement unless an admin disabled the global default or the model; updating workflows and integrations is on your side; no cleanup action after deprecation; Enterprise customers with questions go to their account team.

From the 2026-10-02 notice: Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code and Claude Opus 4.7 removed across the same surfaces as of that day, with Gemini 3.8 Flash, Gemini 3.8 Flash, Kimi K3 and Claude Opus 5.5 as alternatives; Enterprise admins may need to enable alternatives in their model policies; no removal action needed.

The 2026-10-15 notice states: Claude Opus 4.8*, Claude Opus 5 and GPT-5.6 Terra are deprecated on 2026-11-16, with Claude Opus 5.5, Claude Opus 5.5 and GPT-6.1 Sol or GPT-6 Luna as the alternatives; a footnote keeps Claude Opus 4.8 for individual subscribers on annual plans, and that deprecation does not apply to them.

### ⚠️ Caution

Not stated by the notices: that enterprises can request to keep the old models (the notices only point to the account team or account manager - no documented exemption); that personal plans are entirely unaffected (the 2026-10-15 notice adds a footnote for individual subscribers on annual plans who keep Claude Opus 4.8, and says nothing of the kind for the other models); that the 2026-11-16 batch is already gone (the notice says 2026-11-16); any model list beyond these batches; that GitHub rewrites hardcoded model ids for you; and anything about Grok 4.6 or Gemini 3.8 Flash on QCode (QCode does not offer Gemini or Grok).

The notices cover neither Copilot-side subscription prices nor quotas. All of this was checked against the notices as fetched on 2026-10-19.

## Same vs differs

### Same

All of these batches are GitHub-announced removals inside the Copilot product, each with a suggested-replacement table, and none means the model vendors shut the same ids down on their own APIs the same day: Claude Opus 4.7, removed from Copilot on 2026-10-02, is still Active in Anthropic's deprecations table, and so is Claude Opus 4.8, due to go on 2026-11-16. OpenAI's 2026-10-01 removal of gpt-5.4-cyber is likewise its own calendar.

### Differs

Lead time differs: the 2026-09-01 batch was announced on 2026-07-31 and the 2026-10-19 batch on 2026-09-18, roughly a month ahead each, while the 2026-10-02 batch was posted «As of today» and took effect immediately. Scope differs too: 09-01 took Claude Sonnet 4.5/4.6, Opus 4.5/4.6 and others (see the first-wave page), 10-02 took Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code and Claude Opus 4.7, and 10-19 takes six non-Claude rows toward Gemini 3.8 Flash, GPT-5.6 Sol, GPT-5.6 Luna and Grok 4.6.

The 2026-11-16 batch was announced on 2026-10-15 and takes the rows that include Claude (Claude Opus 4.8*, Claude Opus 5 and GPT-5.6 Terra).

## What to do

Two audiences. If you stay inside Copilot: this batch is already gone from Copilot, so switch whatever still points at an old name to the official replacement (for the 2026-10-19 batch the replacement targets are Gemini 3.8 Flash, GPT-5.6 Sol, GPT-5.6 Luna and Grok 4.6, for the 2026-11-16 batch the alternatives are the ones in the notice), then run a real request through each Copilot surface - the pins that break are usually the ones buried in agent settings and completions configs; the models from 2026-10-02 are gone as well, so configs still pinned to them need changing now; the 2026-10-02 notice likewise says Enterprise admins may need to enable alternatives through their model policies.

Enterprise and Business admins: confirm the global default is on and no replacement is explicitly disabled, otherwise re-enable it in the model policy. If you move the call to an API: put an exact id in the model field - `gpt-5.6-sol`, `gpt-6.1-sol`, `claude-opus-4-7`, `claude-opus-5-5`, `claude-opus-4-8`, `claude-opus-5` and `kimi-k3` are callable on QCode - send a minimal request, take the 200, then watch the usage dashboard. Verification means calls you observed after the change, not a switch you glanced at.

## On QCode

As of 2026-10-19: of the models Copilot has removed or is about to remove, `claude-opus-4-7` (gone from Copilot since 2026-10-02) remains callable on QCode - Copilot only withdraws its in-product entry, and Anthropic's table still lists `claude-opus-4-7` as Active. QCode does not currently provide `gpt-5.6-luna` or `gpt-6-luna`; please use another GPT model, such as `gpt-6.1-sol`, `gpt-6-sol` or `gpt-5.6-terra`.

Of the ids matching the official alternatives, `gpt-5.6-sol`, `gpt-6.1-sol`, `claude-opus-5-5`, `claude-opus-4-8`, `claude-opus-5`, `gpt-5.6-terra` and `kimi-k3` are callable on QCode. The Gemini and Grok models are not offered on QCode. QCode sells API access and does not provide Copilot; which models Copilot offers follows GitHub's notices.

Billing is per token; see /models for each model's price. Not ready for a monthly plan? Pay-as-you-go Packs start at $4 for a one-time quota you use within 7 days (top-ups start at $5).

## FAQ

## Which parts of Copilot are affected on 2026-10-19? Already live.

The official sentence names Copilot Chat, inline edits, ask and agent modes, and code completions (and it says across all experiences). All of them take effect on 2026-10-19; checking only the chat model picker misses agent-mode and completions pins.

## Do Enterprise/Business admins still have to act today?

Normally no: the suggested alternatives are automatically enabled under default model enablement. The exception is an admin who turned off the global default or explicitly disabled a replacement model - then someone must enable it in the model policy now, or users have no alternative from today on.

## Which models are deprecated next, on 2026-11-16? Is that the same as OpenAI removing gpt-5.4-cyber on 2026-10-01?

The 2026-10-15 notice lists them row by row: Claude Opus 4.8* to Claude Opus 5.5; Claude Opus 5 to Claude Opus 5.5; GPT-5.6 Terra to GPT-6.1 Sol or GPT-6 Luna, all deprecated on 2026-11-16. A footnote adds that individual subscribers on annual plans keep Claude Opus 4.8, and that the deprecation does not apply to them. A different thing: gpt-5.4-cyber runs on OpenAI's API calendar, the deprecations page set its removal from the API for 2026-10-01 and tells you to migrate to «the most capable cyber model available to you», while Copilot's GPT-5.4 to GPT-5.6 Sol is an in-product turnover. The two calendars point to different targets, so do not merge them into one shutdown table.

## The change is already live - how do I verify my configuration moved?

Send a real request per Copilot surface after switching and confirm the returned model name is the replacement. API callers: take a 200 on the exact new id and then check the usage dashboard. Observed calls count; eyeballing a dropdown does not.

## After the retirements, must I delete the old models from settings?

No: the notice says nothing has to be removed by hand after the models have been deprecated. Your work is replacing the old names that are still pinned and verifying that with one real request per surface, not just a glance at the screen.

## If I call these models via an API instead, which line changes?

The model field in the request body, set to an exact id. Those ids are removed from Copilot or about to be; the QCode callable list is a separate thing. As of 2026-10-19 the relevant ids callable on QCode are: gpt-5.6-sol, gpt-6.1-sol, claude-opus-5-5, claude-opus-4-8, claude-opus-4-7 and kimi-k3. QCode does not currently provide gpt-5.6-luna or gpt-6-luna, so please use another GPT model, such as gpt-6.1-sol, gpt-6-sol or gpt-5.6-terra; the Gemini and Grok models are not offered on QCode. Prices: /models.

## Sources

GitHub changelog posts: 2026-09-18 https://github.blog/changelog/2026-09-18-upcoming-deprecation-of-selected-github-copilot-models-in-mid-october/ (published 2026-09-18 19:00 UTC), 2026-10-02 https://github.blog/changelog/2026-10-02-selected-models-in-github-copilot-deprecated/ (RSS pubDate 2026-10-02 18:24:19 +0000), 2026-10-15 https://github.blog/changelog/2026-10-15-upcoming-deprecation-of-selected-github-copilot-models-in-mid-november/ and the first-wave notice of 2026-07-31, all fetched and checked on 2026-10-19. OpenAI's deprecations page (gpt-5.4-cyber removal on 2026-10-01) and Anthropic's model deprecations table (status of claude-opus-4-7 and claude-opus-4-8) were checked the same day. First-wave details: /github-copilot-claude-deprecation-sep-2026. The QCode callable range follows /models.

## Replace the old names still pinned in your config

As of 2026-10-19, gpt-5.6-sol, gpt-6.1-sol, claude-opus-5-5, claude-opus-4-8, claude-opus-4-7 and kimi-k3 all run on one QCode key; billing is per token, prices on /models.

## Related

### ChatGPT Pro usage limits in Codex (October 2026)

Pro 200 cut canceled on 2026-10-29, Pro 100 and Pro 500 doubled; Pro 500 and Ultrafast at 8x / 6x.

### Codex 401 Unauthorized: Causes and Fixes

codex login, switching to an API key, auth.json, env_key vs requires_openai_auth, and 401 checks for QCode.

### GPT-5.6 Sol vs Terra vs Luna

Two of this wave's replacements are Sol and Luna; picking among three tiers is this guide.

### Crypto payment guide

Nine coins, 56 networks, zero platform fee for AI coding API credits.

Checked on 2026-10-19. Not affiliated with GitHub or Microsoft. All GitHub-side dates, mappings and quotations are transcribed from the official notices of 2026-09-18, 2026-10-02 and 2026-10-15; the official pages are authoritative, and model availability follows /models. QCode sells API access, does not provide GitHub Copilot, and cannot speak for Copilot's admin policies.
