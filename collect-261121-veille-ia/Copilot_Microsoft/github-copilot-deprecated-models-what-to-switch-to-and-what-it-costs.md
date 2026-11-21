---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-deprecated-models-what-to-switch-to-and-what-it-costs
title: "GitHub Copilot deprecated models: what to switch to and what it costs"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["wave", "github", "gemini", "rates", "claude", "gpt-5", "unchanged", "flash"]
source: https://pondero.ai/coding/guides/github-copilot-model-deprecation/
source_anchor: ""
source_lines: [1, 211]
section: "Copilot_Microsoft"
---
## Table of Contents

# GitHub Copilot deprecated models: what to switch to and what it costs

GitHub has pulled 10 Copilot models since October 2, 2026, and marked three more for November 16. Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code, and Claude Opus 4.7 went first, deprecated across Copilot Chat, inline edits, ask and agent modes, and code completions on October 2 (GitHub Changelog, wave one). Six more follow on October 19 (GitHub Changelog, wave two), and on October 15 GitHub announced a third wave: Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra are gone on November 16 (GitHub Changelog, wave three). On November 13, a fourth wave arrived: Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6, and Kimi K3 retire December 14 (GitHub Changelog, wave four). That's 19 models retired or scheduled across four waves in ten weeks. GitHub names one suggested alternative per model, two for GPT-5.6 Terra, none yet for Kimi K3, and says nothing about price.

So we lined every swap up against GitHub's own per-token rate card (GitHub Docs, models and pricing). Of the 19 retiring models, nine swaps get cheaper and five are flat, while two cost more outright: Kimi K2.7 Code's swap to Kimi K3 lists at more than three times the old input price, and GitHub's suggested replacement for GPT-5.4 costs more than two newer OpenAI models on the same page. A sixth case, Gemini 4 Argon's swap to Carbon, is flat today but jumps to nearly double on December 1, two weeks before Argon disappears. Two more swaps have no price at all yet: Gemini 3.8 Flash's successor, Gemini 4 Hydrogen, isn't on GitHub's rate card, and Kimi K3 (the model that just got expensive above) doesn't have a named successor either, now that it's retiring in its own right. Below: a per-model pick, the bill impact, and a one-line sweep for every place an old model name is still pinned, including the ones you may have just pinned fixing the last wave.

## All 19 retiring models, mapped

The class column is the label GitHub prints next to each model on its rate card (Lightweight, Versatile, Powerful), the only behavior signal GitHub publishes for these swaps.

| Retiring model | Gone on | GitHub's suggested swap | Class change on GitHub's rate card | Source | 
|---|---|---|---|---|
| Gemini 3.5 Flash | 2026-10-02 | Gemini 3.8 Flash | Lightweight to Versatile | Wave one, rates | 
| Gemini 3.6 Flash | 2026-10-02 | Gemini 3.8 Flash | Versatile, unchanged | Wave one, rates | 
| Kimi K2.7 Code | 2026-10-02 | Kimi K3 | Versatile to Powerful | Wave one, rates | 
| Claude Opus 4.7 | 2026-10-02 | Claude Opus 5 | Powerful, unchanged | Wave one, rates | 
| Gemini 3.7 Flash | 2026-10-19 | Gemini 3.8 Flash | Versatile, unchanged | Wave two, rates | 
| GPT-5.5 | 2026-10-19 | GPT-5.6 Sol | Powerful, unchanged | Wave two, rates | 
| GPT-5.4 | 2026-10-19 | GPT-5.6 Sol | Versatile to Powerful | Wave two, rates | 
| GPT-5.4 mini | 2026-10-19 | GPT-5.6 Luna | Lightweight, unchanged | Wave two, rates | 
| GPT-5 mini | 2026-10-19 | GPT-5.6 Luna | Lightweight, unchanged | Wave two, rates | 
| Grok 4.5 | 2026-10-19 | Grok 4.6 | Versatile, unchanged | Wave two, rates | 
| Claude Opus 4.8 | 2026-11-16 | Claude Opus 5.5 | Powerful, unchanged | Wave three, rates | 
| Claude Opus 5 | 2026-11-16 | Claude Opus 5.5 | Powerful, unchanged | Wave three, rates | 
| GPT-5.6 Terra | 2026-11-16 | GPT-6.1 Sol or GPT-6 Luna | Versatile to Powerful (Sol) or Versatile to Lightweight (Luna) | Wave three, rates | 
| Claude Fable 5 | 2026-12-14 | Claude Fable 5.2 | Powerful, unchanged | Wave four, rates | 
| Claude Haiku 4.5 | 2026-12-14 | Claude Haiku 5.5 | Versatile to Lightweight | Wave four, rates | 
| Gemini 3.8 Flash | 2026-12-14 | Gemini 4 Hydrogen | Unknown, not on the rate card yet | Wave four, Hydrogen GA | 
| Gemini 4 Argon | 2026-12-14 | Gemini 4 Carbon | Powerful, unchanged | Wave four, rates | 
| Grok 4.6 | 2026-12-14 | Grok 4.7 | Versatile, unchanged | Wave four, rates | 
| Kimi K3 | 2026-12-14 | To be announced | Powerful, no successor named | Wave four | 

*Source: github.blog, captured November 14, 2026.*

Two pins from earlier waves are now themselves retiring. If you swept wave one and moved to Gemini 3.8 Flash or Kimi K3, or swept wave two and moved to Grok 4.6, go find those lines again: all three are gone December 14, same as everything else in this table.

Watch the GPT-5.4 and Kimi K2.7 Code rows. Both get bumped up a class, and both swaps raise the per-token price (GitHub Docs). Gemini 3.5 Flash also moves up a class, but its replacement is cheaper per token; everyone else stays put. All three November retirements swap into a cheaper pick too, and Opus 4.8 and Opus 5 do it without even changing class. Haiku 4.5 is the only December swap that moves down a class, from Versatile to Lightweight, and it's still cheaper than what it replaces; Fable, Argon, and Grok hold their class.

**Exception: Opus 4.8 isn't gone for everyone.** GitHub's wave-three notice carves out one group: "Claude Opus 4.8 will remain available to individual GitHub Copilot subscribers on annual plans so they retain an Opus offering. The deprecation of Claude Opus 4.8 does not apply to these customers" (wave three). Everyone else on Opus 4.8, plus everyone on Opus 5, moves to Opus 5.5.

**A third pick landed after wave three's notice.** GitHub made GPT-6.1 Terra generally available on October 26, 2026, pitched as a lower-cost option than GPT-6.1 Sol for "everyday agentic coding and terminal work." It's live for Pro, Pro+, Max, Business, and Enterprise across VS Code, Visual Studio, JetBrains IDEs, GitHub.com, GitHub Mobile, and more, billed at OpenAI's provider list pricing (GitHub Changelog, GPT-6.1 Terra). Anyone retiring GPT-5.6 Terra on November 16 now has a third option alongside GitHub's suggested Sol and Luna; the pricing and credit math are in the table below.

**Same exception, now for Haiku 4.5.** Wave four's notice repeats the wave-three carve-out for a different model: "Claude Haiku 4.5 will remain available to individual GitHub Copilot subscribers on annual plans. The deprecation of Claude Haiku 4.5 does not apply to these customers" (wave four). Everyone else on Haiku 4.5 moves to Haiku 5.5.


## What happens if you do nothing

Read GitHub's wording literally. All four notices say the models are deprecated "across all GitHub Copilot experiences," ask you to "update your workflows and integrations to use the supported models before this date," and add that "no action is required to remove the models after they have been deprecated" (wave one, wave two, wave three, wave four).

GitHub says nothing beyond that. Neither post says Copilot will quietly reroute a saved selection to the suggested alternative. GitHub cleans up its picker; anything on your side that names the old model by string is yours to fix. (Our September 8 news piece covered the wave-one announcement.)

In practice, old model names hide in four places, all documented in GitHub's CLI and agent references:

- Copilot CLI reads a model flag at launch or the `COPILOT_MODEL` environment variable (CLI command reference). CI scripts are the usual offender.
- A `model` key in`~/.copilot/settings.json` (personal) or`.github/copilot/settings.json` (committed to the repo) (CLI config reference).
- Custom agent profiles, through the `model` frontmatter property (leave it unset and the agent inherits the default model) (custom agents reference).
- Per-subagent overrides under `subagents.agents` in settings, where a`modelPolicy` of`"required"` locks the choice so it can't be overridden from the picker (CLI config reference).

Sweep for all 19 at once. Spelling varies by surface (the docs write both `Kimi-K2.7-Code` and `Kimi K2.7 Code`), so the pattern tolerates dashes, dots, and spaces:

```
# Run from the repo root. Checks repo config, workflows, agent profiles,
# and your personal Copilot CLI settings for all 19 retiring models.
grep -rnIiE 'gemini[-. ]?3\.[567][-. ]?flash|kimi[-. ]?k2\.7|opus[-. ]?4\.7|opus[-. ]?4\.8|opus[-. ]?5\b|gpt-5\.[45]|gpt-5[-. ]mini|grok[-. ]?4\.5|gpt-5\.6[-. ]?terra|fable[-. ]?5\b|haiku[-. ]?4\.5|gemini[-. ]?3\.8[-. ]?flash|gemini[-. ]?4[-. ]?argon|grok[-. ]?4\.6\b|kimi[-. ]?k3\b' \
  .github/ ~/.copilot/settings.json 2>/dev/null
# And the environment your shell and CI runners inherit
env | grep -i COPILOT_MODEL
```
Four false positives to ignore: `gpt-5\.[45]` catches GPT-5.4 nano, which is on none of the four waves; `opus[-. ]?5\b` also matches Opus 5.5 pins, which is the new pick and needs no change; `fable[-. ]?5\b` also matches Fable 5.1 and Fable 5.2 pins, neither of which is retiring; and `kimi[-. ]?k3\b` is the only December model without a successor, so a hit there means you wait, not edit.

Each hit is a one-line edit. For a repo-wide default, `auto` hands the choice back to Copilot:

```
// .github/copilot/settings.json (the file accepts JSON with comments)
{
  "model": "auto"
}
```
For an agent that genuinely needs a specific model, pin the new name in its profile:

```
---
description: Reviews pull requests for migration risk
model: claude-opus-5.5
---
```
If you'd rather stop chasing deprecation notices in CI altogether, Business and Enterprise orgs have a boring, durable option. GitHub designated GPT-5.3-Codex its long-term support model on March 18, 2026 and commits to keeping it available for one year from that date (GitHub Docs, base and LTS models). The alternative is Auto with a tier: `/model auto efficiency`, `balance`, or `intelligence` in the CLI (CLI command reference), covered in our auto-tier news piece.

## Does the replacement cost more?

Prices below are per million tokens, input then output, straight from GitHub's models and pricing page, fetched 2026-10-02 (the four wave-three rows were fetched 2026-10-16, the GPT-6.1 Terra row was fetched 2026-10-27, and the six wave-four rows were fetched 2026-11-14). The session column reuses the rubric from our AI Credits pricing guide: a medium agent session that reads 200K tokens and writes 50K, no caching, at 1 AI credit = $0.01, rounded to the nearest credit.

| Swap | Old price (in / out) | New price (in / out) | Credits per medium session, old to new | Source | 
|---|---|---|---|---|
| Gemini 3.5 Flash to 3.8 Flash | $1.50 / $9.00 | $0.75 / $3.75 (promo) | 75 to 34 | GitHub Docs | 
| Gemini 3.6 or 3.7 Flash to 3.8 Flash | $0.75 / $3.75 | $0.75 / $3.75 (promo) | 34 to 34 | GitHub Docs | 
| Kimi K2.7 Code to Kimi K3 | $0.95 / $4.00 | $3.00 / $15.00 | 39 to 135 | GitHub Docs | 
| Claude Opus 4.7 to Opus 5 | $5.00 / $25.00 | $5.00 / $25.00 | 225 to 225 | GitHub Docs | 
| GPT-5.5 to GPT-5.6 Sol | $5.00 / $30.00 | $4.00 / $20.00 | 250 to 180 | GitHub Docs | 
| GPT-5.4 to GPT-5.6 Sol | $2.50 / $15.00 | $4.00 / $20.00 | 125 to 180 | GitHub Docs | 
| GPT-5.4 mini to GPT-5.6 Luna | $0.75 / $4.50 | $0.20 / $1.20 | 38 to 10 | GitHub Docs | 
| GPT-5 mini to GPT-5.6 Luna | $0.25 / $2.00 | $0.20 / $1.20 | 15 to 10 | GitHub Docs | 
| Grok 4.5 to Grok 4.6 | $2.00 / $6.00 | $2.00 / $6.00 | 70 to 70 | GitHub Docs | 
| Claude Opus 4.8 to Opus 5.5 | $5.00 / $25.00 | $4.00 / $20.00 | 225 to 180 | GitHub Docs | 
| Claude Opus 5 to Opus 5.5 | $5.00 / $25.00 | $4.00 / $20.00 | 225 to 180 | GitHub Docs | 
| GPT-5.6 Terra to GPT-6.1 Sol | $2.00 / $12.00 | $2.00 / $10.00 | 100 to 90 | GitHub Docs | 
| GPT-5.6 Terra to GPT-6 Luna | $2.00 / $12.00 | $0.10 / $0.50 | 100 to 5 | GitHub Docs | 
| GPT-5.6 Terra to GPT-6.1 Terra | $2.00 / $12.00 | $1.00 / $5.00 | 100 to 45 | GitHub Docs | 
| Claude Fable 5 to Fable 5.2 | $10.00 / $50.00 | $8.00 / $40.00 | 450 to 360 | GitHub Docs | 
| Claude Haiku 4.5 to Haiku 5.5 | $1.00 / $5.00 | $0.50 / $2.50 (long context, above 100K input) | 45 to 23 | GitHub Docs | 
| Gemini 3.8 Flash to Gemini 4 Hydrogen | $0.75 / $3.75 (promo) | not on rate card yet | 34 to unknown | Hydrogen GA | 
| Gemini 4 Argon to Gemini 4 Carbon | $2.00 / $10.00 (promo, through Dec 1) | $2.00 / $10.00 (promo, through Dec 1), then $4.00 / $20.00 | 90 to 90, then 180 | GitHub Docs | 
| Grok 4.6 to Grok 4.7 | $2.00 / $6.00 | $2.00 / $6.00 | 70 to 70 | GitHub Docs | 
| Kimi K3 to [no successor named] | $3.00 / $15.00 | no successor priced | 135 to unknown | Wave four | 

A few details on that page change the math, and none of the four deprecation notices mention them.

The Gemini 3.8 Flash price is promotional. A footnote on the pricing page holds Gemini 3.6, 3.7, and 3.8 Flash at $0.75 input and $3.75 output through December 31, 2026, and doesn't say what comes after. Put a reminder in your calendar for January.

Cache writes apply to GPT-5.6 models. Per the same page, Luna charges $0.25 and Sol $5.00 per million cache-write tokens, and earlier OpenAI models in Copilot had no cache write cost at all. Long agent runs that build a big cached context pay it; our session column leaves it out, so treat the Sol and Luna figures as a floor.

Luna has a long-context tier that GPT-5 mini didn't. Above 200K input tokens it moves to $0.40 input and $1.80 output (GitHub Docs). Point it at a whole monorepo and its input price passes GPT-5 mini's old $0.25.

GPT-6 Luna bills a cache write too, and its cheap default rate only holds to 272K tokens. Luna charges $0.125 per million cache-write tokens by default and jumps to $0.20 input / $0.75 output past that threshold, still a fraction of GPT-5.6 Terra's $2.00 / $12.00 either way (GitHub Docs).

GPT-6.1 Terra bills a cache write too, same family rule. Per the same page, it charges $1.25 per million cache-write tokens by default up to 272K input tokens, then steps up to $2.00 input / $7.50 output past that threshold. Our 45-credit session figure above leaves the cache write out, so treat it as a floor, same as every other row in this table.

Haiku 5.5's cheap rate caps out earlier than any other swap in this table. Per GitHub's pricing page, the showcase $0.10 input / $0.50 output rate only holds to 100K input tokens, half of Luna's and Grok's thresholds and well under GPT-6 Luna's 272K. Our 200K-token medium session clears that line, so the honest comparison uses the long-context row, $0.50 / $2.50, not the headline number GitHub prints first.

Argon and Carbon both carry a promo that ends December 1, 2026, two weeks before Argon retires. Carbon's post-promo price is on the page: it roughly doubles, to $4.00 input / $0.20 cached / $20.00 output. Argon's isn't, because GitHub doesn't need to publish a long-term price for a model with twelve days left either way (GitHub Docs).

Gemini 4 Hydrogen, GitHub's pick for the retiring Gemini 3.8 Flash, has no rate-card row as of this writing. GitHub's November 13 release notice says only that it "is billed at provider list pricing under usage-based billing" and points back to the same pricing page (GitHub Changelog, Gemini 4 Hydrogen). Rollout is also gradual per that notice, so check the model picker before assuming it's there.

Kimi K3's own retirement notice lists its successor as "To be announced." Anyone who moved to K3 for a reason, price or otherwise, has nothing to evaluate yet.

### Where we'd go instead of GitHub's pick

Three of GitHub's suggestions leave money on the table, judged by its own rate card.

If you ran GPT-5.4, look at GPT-5.6 Terra first. It stays in the Versatile class at $2.00 / $12.00, about 100 credits per medium session against Sol's 180 (GitHub Docs). On Pro+ and above, GPT-6.1 Sol lists at $2.00 / $10.00 (about 90 credits), and GitHub says that in early testing it finished tasks with noticeably fewer tokens and steps than the GPT-6 and GPT-5.6 families (GitHub Changelog, Sept 29).

Opus 4.7 users get a cheaper upgrade than the one GitHub suggests. Opus 5.5 lists at $4.00 / $20.00, about 180 credits per medium session to Opus 5's 225 (GitHub Docs). GitHub reports it resolved tasks comparably to Opus 5 with significantly fewer steps and tokens, on Pro+, Max, Business, and Enterprise (GitHub Changelog, Sept 22).

Kimi K2.7 Code users who chose it for price should know K3 is a different budget: 135 credits a session where you used to spend 39 (GitHub Docs). Gemini 3.8 Flash (34) or GPT-5.6 Luna (10) keeps you at or under the old spend. Trial one of them on a real task before you accept the jump.

Opus 4.8 and Opus 5 users don't have a decision to make: GitHub's only suggested swap, Opus 5.5, is also the cheaper one, about 180 credits a session against 225 for either retiring model (GitHub Docs). GPT-5.6 Terra users now have three picks instead of two. GitHub's two suggested swaps bracket the field: GPT-6 Luna undercuts everything at about 5 credits a session, and GPT-6.1 Sol sits at 90, the only one of the three that moves up to the Powerful class (GitHub Docs). GPT-6.1 Terra, which GitHub shipped October 26, lands in between at about 45 credits and keeps the Versatile class GPT-5.6 Terra was already in (GitHub Changelog, GPT-6.1 Terra; GitHub Docs). Terra solved noticeably more terminal and C# coding tasks than GPT-5.6 Terra in GitHub's own internal testing (GitHub Changelog, GPT-6.1 Terra), making it the closest like-for-like upgrade if you don't need Luna's rock-bottom price or Sol's extra class. Try Terra first; drop to Luna if the bill still stings, or step up to Sol if a task needs the reasoning headroom.

Wave four has two swaps with nothing to compare and one where the comparison depends on the date you move. Fable 5 and Haiku 4.5 users can take GitHub's default pick without a second thought: Fable 5.2 drops from 450 credits a session to 360, and Haiku 5.5 drops from 45 to 23 once you account for its 100K long-context threshold. Grok 4.6 to Grok 4.7 is flat at 70, no decision needed. Gemini 4 Argon users should move before December 1 if they can; Carbon costs the same as Argon during the promo but doubles to 180 credits a session the day the promo ends, two weeks ahead of Argon's own retirement. Gemini 3.8 Flash and Kimi K3 users are stuck watching the changelog: GitHub hasn't priced Gemini 4 Hydrogen yet, and Kimi K3 doesn't have a named successor at all.

None of this touches tab-complete. Code completions and next edit suggestions aren't billed in AI credits and stay unlimited on every paid plan (GitHub Docs), so the retirements only move the chat, agent, and CLI side of your bill.

## For Copilot Business and Enterprise admins

**Admin check, due before October 19.** The two earlier notices word admin access differently. Wave two says that under default model enablement the suggested alternatives are switched on automatically for Business and Enterprise "unless an administrator has turned off the global default or explicitly disabled the model" (wave two). Wave one only says admins "may need to enable access to the alternative models through their model policies" (wave one). If your org has turned off the global default (background in our Global Model Policy GA coverage), the replacements won't appear on their own; GitHub says you enable them through each model's policy instead (wave two). Open your org or enterprise Copilot model settings today and confirm Gemini 3.8 Flash, Kimi K3, Claude Opus 5, GPT-5.6 Sol, GPT-5.6 Luna, and Grok 4.6 are enabled, plus any cheaper alternative from the section above that you want people to reach for, before October 19.

**Second admin check, due before November 16.** Wave three repeats wave two's auto-enable wording: Opus 5.5, GPT-6.1 Sol, and GPT-6 Luna turn on by default for Business and Enterprise "unless an administrator has turned off the global default or explicitly disabled the model" (wave three). Confirm those three are enabled in your org or enterprise Copilot model settings before November 16.

**Third admin check, due before December 14.** Wave four uses the same auto-enable wording again: Fable 5.2, Haiku 5.5, Gemini 4 Carbon, and Grok 4.7 turn on by default for Business and Enterprise "unless an administrator has turned off the global default or explicitly disabled the model" (wave four). Gemini 4 Hydrogen, already GA as of November 13, follows the same rule as any new model: Business and Enterprise admins manage it through the model policy, and it's "automatically enabled unless an administrator has turned off the global default or explicitly disables this model" (GitHub Changelog, Gemini 4 Hydrogen). Kimi K3 has nothing to enable yet, there's no successor. Confirm the four named models before December 14, and check back on Hydrogen's rollout, which GitHub says is gradual.


All three checks above describe the settings-page click-through. As of October 19, 2026, that is no longer the only way in. GitHub shipped GET and PATCH REST endpoints for Copilot model policies at four governance levels: enterprise (`/enterprises/{enterprise}/copilot/model-policies`), enterprise teams (`/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies`), enterprise apps (`/enterprises/{enterprise}/copilot/apps/model-policies`), and organizations (`/orgs/{org}/copilot/model-policies`) (GitHub Changelog, model policy API). GitHub says enterprise policies can delegate model control down to organizations in organization mode, or to teams and apps in enterprise teams mode (GitHub Changelog, model policy API). For a platform team running the two admin checks above across dozens of orgs, that is the difference between clicking through each org's Copilot settings by hand and running a GET to audit which orgs still have Opus 5.5, GPT-6.1 Sol, or GPT-6 Luna disabled, then a PATCH to turn them on everywhere at once before the November 16 cutover. The web UI still works and isn't going away; the API is the added option for anyone managing model policy at scale.

The "global default" that both checks above reference by name reached general availability on October 22, with GitHub enforcing it in stages through October 26, so it can land for one org in your enterprise a few days before another (GitHub Changelog, global default GA). Once it takes effect, any eligible generally available feature or client capability an admin left Unconfigured, including the "MCP servers in Copilot" settings, gets set to whatever the org picked: Enabled, Disabled, or Let organizations decide. A feature an admin already set explicitly keeps that explicit setting, and a preview you opted into early keeps the opt-in even after it ships GA (GitHub Changelog, global default GA).

*Source: github.blog, captured October 23, 2026.*

This is separate from the per-model policy checks above, so it needs its own look: open AI Controls → Copilot → Default policy for new features before October 26 and confirm what's set there. An org that never touched this setting is about to get Enabled, Disabled, or Let organizations decide applied to every unconfigured eligible feature on its behalf, and that includes MCP server access, not just which models show up in the picker.

## Three more admin-visibility changes landed the same week

GitHub shipped three more changes alongside the global default policy, two on October 22 and one on October 23. None touch pricing, but anyone running the admin checks above should know about all three.

| Change | Shipped | What it does | Source | 
|---|---|---|---|
| Named skills, agents, and MCP servers in usage metrics | Oct 22 | Usage metrics API reports the configured name of a custom skill, agent, MCP server, slash command, or plugin instead of grouping it under `other` or`custom` ; each list now holds up to 25 entries per record, up from five | GitHub Changelog | 
| Third-party client totals ( `totals_by_client` ) | Oct 23 | A new array in the usage metrics API counts requests and tokens by tool, including `opencode` ,`claude_code` ,`codex` ,`zed` , and`copilot_cli` , closing a gap for tools that don't send editor telemetry | GitHub Changelog | 
| Industry comparisons in the impact dashboard | Oct 22 | Opt-in benchmarking of agentic adoption and the pull-request merge-rate gap against a peer group chosen by company size and industry | GitHub Changelog | 

Before October 22, a custom skill, agent, or MCP server your org built showed up in usage reports as an anonymous `other` or `custom` line. Now the `totals_by_skill`, `totals_by_custom_agent`, `totals_by_mcp`, `totals_by_slash_cmd`, and `totals_by_plugin` arrays report the name as configured, trimmed to 128 characters, across per-user 1-day and 28-day reports, aggregated 1-day reports, and `day_totals` in aggregated 28-day reports (GitHub Changelog, Oct 22). GitHub reports names from Copilot CLI 1.0.97 and later, the Copilot app, ACP integrations like JetBrains, and the Copilot SDK v1.0.21 and later; an IDE's built-in chat telemetry doesn't carry names, and history only starts from the days processed after the release (GitHub Changelog, Oct 22).

The next day GitHub closed a related gap. A new `totals_by_client` array in the enterprise, organization, enterprise-user, and organization-user reports counts requests and tokens by the tool that sent them: `opencode`, `claude_code`, `codex`, `zed`, `copilot_cli`, `vscode`, `jetbrains`, and more, with unrecognized clients grouped under `other` (GitHub Changelog, Oct 23). Tools outside the IDE, OpenCode included, don't send editor telemetry, so their Copilot usage was invisible to the existing `totals_by_ide` breakdown until now (GitHub Changelog, Oct 23). One caution: `request_count` counts model calls, not prompts, so don't line it up directly against interaction totals.

The impact dashboard added an opt-in "Industry comparison" section the same day. Pick a company size band and one of ten industries, and it ranks your agent-first and multi-agent adoption, plus the pull-request merge rate gap between agentic and nonagentic users, against the peer median and, as more businesses opt in, the top decile (GitHub Changelog, Oct 22). It's off by default, the aggregated benchmarks carry no identifying data and are deleted after 30 days, and none of it reaches the usage metrics API or NDJSON exports: this is a dashboard-only feature (GitHub Changelog, Oct 22).

## What to do today

Our picks. A solo dev on Pro who mostly does inline edits and chat should move Flash-family pins to Gemini 3.8 Flash and the minis to GPT-5.6 Luna, which cost the same or less per session than what they replace (GitHub Docs), but should also expect to redo that move again soon: Gemini 3.8 Flash itself retires December 14. An agent-heavy dev on Pro+ or Max should send Opus 4.7 to Opus 5.5 and GPT-5.4 to GPT-6.1 Sol, both cheaper per token than GitHub's suggested swaps (GitHub Docs). Team leads should point CI and shared configs at `auto` or the LTS model, GPT-5.3-Codex (GitHub Docs, LTS). Anyone on Opus 4.8 or Opus 5 should just take GitHub's Opus 5.5 swap, it is already the cheaper pick. GPT-5.6 Terra users who want the closest match to what they're used to should try GPT-6.1 Terra first, about half GPT-6.1 Sol's price and the model GitHub's own testing put ahead of the one it's replacing; chase the smallest bill with GPT-6 Luna instead, or step up to Sol for the extra class. For the December wave: take GitHub's Fable 5.2 and Grok 4.7 swaps without a second thought, expect roughly half the per-session cost moving Haiku 4.5 to Haiku 5.5, move off Gemini 4 Argon before December 1 if the date works for you, and if you're on Gemini 3.8 Flash or Kimi K3, there's nothing to decide yet, just watch the changelog.

The checklist:

- Run the grep sweep above and replace every hit, including the wave-three models you have until November 16 and the wave-four models you have until December 14. Pay special attention if you swept wave one or two already: Gemini 3.8 Flash, Grok 4.6, and Kimi K3 were your fixes then and are retiring now.
- Admins: confirm the replacement models are enabled in Copilot model policies before November 16 (wave three) and again before December 14 (wave four).
- If you ran Kimi K2.7 Code or GPT-5.4, trial a cheaper alternative from the table before accepting GitHub's default swap. GPT-5.6 Terra users should trial GPT-6.1 Terra or GPT-6 Luna before accepting Sol.
- Set a January reminder to recheck Gemini Flash pricing once the promo ends on December 31 (GitHub Docs), and a December 1 reminder for Gemini 4 Argon and Carbon's promo.
- Watch the changelog for Gemini 4 Hydrogen's rate-card entry and Kimi K3's named successor, neither exists yet.
- Admins who want peer benchmarking: opt in to the Copilot impact dashboard's industry comparison, which is off by default (GitHub Changelog).

If you're weighing whether your plan still fits after the swaps, the AI Credits pricing guide has the per-plan allowance math, and current plans are at github.com/features/copilot. Coming from our June model-picker guides (agentic security picks, MAI-Code-1-Flash)? This table supersedes their roster.

Two more things shipped this week, both unrelated to the retirements. Computer use reached public preview in Copilot CLI and the Copilot app on macOS and Windows on October 1 (GitHub Changelog). And on October 23, GitHub shipped a consolidated GitHub app for Microsoft Teams that turns an issue notification into a pull request when someone mentions @GitHub in the chat; the old GitHub notifications app is being renamed GitHub (deprecated) and retires November 16 (GitHub Changelog).

## What changed

- **November 14, 2026** : Added a fourth deprecation wave, announced November 13: Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6, and Kimi K3 retire December 14, 2026. Folded all six into the mapping and pricing tables, added a Haiku 4.5 annual-plan exception callout matching the existing Opus 4.8 one, flagged that Gemini 3.8 Flash, Grok 4.6, and Kimi K3 were themselves wave one and two replacement picks and are now retiring in turn, noted that Gemini 4 Hydrogen (GitHub's pick for Gemini 3.8 Flash, GA November 13) has no published rate-card price yet, and noted Kimi K3 has no named successor. Added a third admin check due before December 14 and updated the grep sweep, picks, and checklist for all 19 models now tracked across four waves.
- **October 27, 2026** : Added GPT-6.1 Terra, which GitHub made generally available October 26 as a cheaper everyday-coding and terminal sibling to GPT-6.1 Sol. Folded it into the pricing table as a third pick for GPT-5.6 Terra's November 16 retirement, about 45 credits a session against Luna's 5 and Sol's 90, and updated the "Where we'd go instead" and "What to do today" guidance to cover it. Left GPT-6.1 Sol's own entry untouched beyond noting the new cheaper sibling.
- **October 24, 2026** : Added three usage-visibility changes GitHub shipped October 22 to 23: named skills, agents, and MCP servers in the usage metrics API; a new`totals_by_client` breakdown that captures tools like OpenCode outside the IDE; and opt-in industry-comparison benchmarking in the Copilot impact dashboard. This guide's October 23 update called the first of these out of scope; grouping all three with the existing admin section instead keeps the governance context together rather than spreading it across more one-off guides. Also noted, as an unrelated aside, the new consolidated GitHub app for Microsoft Teams that turns a notification into a pull request.
- **October 23, 2026** : Added the global default policy for Copilot features, which GitHub took to general availability October 22 with enforcement rolling out through October 26: unconfigured eligible features and MCP server access now get set to an org's chosen default, with a dated screenshot of the admin settings page. Left out two same-day GitHub changelog entries (named skills/agents/MCP servers in the usage metrics API; a Visual Studio feature bundle) as out of scope for a model-deprecation and pricing guide.
- **October 20, 2026** : Added the Copilot model-policy REST API GitHub shipped October 19: GET and PATCH endpoints for enterprise, enterprise team, enterprise app, and organization model policies, noted alongside the existing settings-page path in the admin section.
- **October 16, 2026** : Added the third deprecation wave GitHub announced October 15: Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra retire November 16, 2026, swapping to Claude Opus 5.5 and GPT-6.1 Sol or GPT-6 Luna. Reworded the lede and retitled the piece to drop the single-month framing now that it tracks three waves, added the Opus 4.8 individual-annual-plan exception as a callout, and added a GPT-6 Luna pricing row pulled fresh from GitHub's rate card.
