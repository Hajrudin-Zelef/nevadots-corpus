---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-retired-10-models-in-october-a-3-more-go-november-16-the-full-replacement-map
title: "GitHub Copilot Retired 10 Models in October â 3 More Go November 16: The Full Replacement Map"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["models", "copilot", "october", "github", "replacement", "gpt-5", "claude", "opus"]
source: https://aicoderscope.com/blog/github-copilot-model-retirements-october-november-2026/
source_anchor: ""
source_lines: [1, 137]
section: "Copilot_Microsoft"
---
# GitHub Copilot Retired 10 Models in October â 3 More Go November 16: The Full Replacement Map

**TL;DR**: GitHub Copilot retired 10 models in October 2026 â four on October 2, six on October 19 â and retires Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra on November 16. Replacements are auto-enabled for Business and Enterprise only if your admin hasnât disabled the global model default. On a $10 Pro plan, GitHubâs suggested replacement for GPT-5.5 isnât even available to you.

|  | Individual on Pro/Pro+ | Business/Enterprise admin | BYOK in Cursor/Cline | 
|---|---|---|---|
| What breaks | Your selected model vanishes from the picker | Pinned org models stop working; replacements may not be enabled | Nothing â Copilot retirements donât touch API-key setups | 
| What to do | Switch to the mapped replacement below | Check model policies in Copilot settings before Nov 16 | Nothing now; watch the providerâs own API deprecations | 
| The catch | GPT-6.1 Sol needs Pro+ ($39/mo); Pro gets GPT-6 Luna | Auto-enablement skips models you explicitly disabled | Policy controls donât extend to BYOK models anyway | 

**Honest take**: Treat the November 16 wave as your deadline to stop pinning model IDs at all â thatâs three Copilot retirement waves in six weeks, and GPT-5.6 Terra lasted 130 days. Pick the newest default, keep one fallback enabled, and put the model name in one config file, not twenty.


A note on sourcing: github.blog and docs.github.com would not load directly from the environment this was researched in, so every changelog entry below is cited from GitHubâs own changelog URLs as surfaced through search, cross-checked against at least two independent writeups. The replacement names conflict across third-party coverage (several outlets still say âGPT-5.6 Solâ â thatâs the September announcement, superseded by retirement day). Where sources disagreed, the official changelog table wins, and the disagreement is flagged.

## Which GitHub Copilot models were retired in October 2026?

Ten models left Copilot in October 2026, in two waves. On October 2, GitHub deprecated four models across all Copilot experiences â Chat, inline edits, ask and agent modes, and code completions (changelog, Oct 2, 2026):

| Retired Oct 2, 2026 | GitHubâs suggested replacement | 
|---|---|
| Gemini 3.5 Flash | Gemini 3.8 Flash | 
| Gemini 3.6 Flash | Gemini 3.8 Flash | 
| Kimi K2.7 Code | Kimi K3 | 
| Claude Opus 4.7 | Claude Opus 5.5 | 

On October 19, six more followed â the wave GitHub had announced on September 18 (changelog, Oct 19, 2026; announcement, Sep 18, 2026):

| Retired Oct 19, 2026 | GitHubâs suggested replacement | 
|---|---|
| GPT-5.5 | GPT-6.1 Sol | 
| GPT-5.4 | GPT-6.1 Sol | 
| GPT-5.4 mini | GPT-6 Luna | 
| GPT-5 mini | GPT-6 Luna | 
| Gemini 3.7 Flash | Gemini 3.8 Flash | 
| Grok 4.5 | Grok 4.7 | 

Both changelog entries state no action is required to remove the deprecated models â they simply disappear from the model picker. The action is on the replacement side, covered below.

One detail that shows how fast this treadmill spins: GitHubâs September 18 heads-up named **GPT-5.6 Sol** and **Grok 4.6** as the replacements. By the time the retirement actually landed 31 days later, both suggested replacements had themselves been superseded â the final October 19 table points at GPT-6.1 Sol and Grok 4.7 instead. Several news sites ran the September names, which is why youâll find âCopilot retires 6 models for GPT-5.6â headlines that no longer match GitHubâs own table.

## Which models leave GitHub Copilot on November 16, 2026?

Three models: Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra, per GitHubâs October 15 changelog (Upcoming deprecation of selected GitHub Copilot models in mid-November):

| Retired Nov 16, 2026 | GitHubâs suggested replacement | 
|---|---|
| Claude Opus 4.8 | Claude Opus 5.5 | 
| Claude Opus 5 | Claude Opus 5.5 | 
| GPT-5.6 Terra | GPT-6.1 Sol or GPT-6 Luna | 

There is one carve-out: individual Copilot subscribers on **annual** plans keep access to Claude Opus 4.8 past November 16. The exception does not extend to Business or Enterprise seats â those should move to Claude Opus 5.5.

GPT-5.6 Terraâs lifespan is worth pausing on. It entered Copilot on July 9, 2026 (changelog) and exits November 16 â 130 days. If your team standardized on Terra for long agent runs in August, you got about one quarter out of that decision.

## Does your Copilot plan actually include the suggested replacement?

Not necessarily, and this is the trap in GitHubâs replacement table. GPT-6 Sol and GPT-6 Luna arrived in Copilot on September 22, 2026, and GPT-6.1 Sol followed on September 29 (changelog, Sep 22; changelog, Sep 29). Plan access per those entries:

- **GPT-6.1 Sol / GPT-6 Sol** : Copilot Pro+ ($39/mo), Max ($100/mo), Business ($19/seat), Enterprise
- **GPT-6 Luna** : all of the above plus Copilot Pro ($10/mo)

So a Pro subscriber who was using GPT-5.5 gets pointed at GPT-6.1 Sol by the deprecation notice but canât select it â on $10/month, the real replacement is GPT-6 Luna, which OpenAI positions as the lightweight, cost-efficient end of the GPT-6 family. If Luna isnât enough for your agent workloads, your options are upgrading to Pro+ or routing around Copilot entirely with GPT-6.1 Sol via BYOK in Cursor or Cline. Both GPT-6 models bill through usage-based AI Credits, the system Copilot moved to on June 1, 2026 â the mechanics are in our AI Credits billing breakdown.

Plan prices above match what GitHub listed when we verified the lineup on September 21, 2026 (Free $0 / Pro $10 / Pro+ $39 / Max $100 / Business $19 per seat) and October 2026 third-party pricing trackers agree; confirm on github.com/features/copilot/plans before buying.

## Does Copilot switch you to the replacement automatically?

For Business and Enterprise: the suggested replacements are enabled automatically under Copilotâs default model enablement â **unless** an administrator turned off the global default or explicitly disabled that specific model, in which case the replacement must be enabled manually through model policies in Copilot settings. Thatâs GitHubâs own wording across the October changelog entries, and it follows from the model-policy change GitHub shipped in late August 2026, when unconfigured models flipped to on-by-default for enterprise users.

The failure mode weâd plan around: an org that locked its model list down in August (global default off, a short allowlist of approved models) hits Monday, November 16 with Claude Opus 5 gone, Claude Opus 5.5 not on the allowlist, and every developer who had Opus 5 selected staring at a model picker with no equivalent in it. Nothing errors loudly â the model is just absent. The fix takes an admin two minutes: Copilot settings â Models (model policies) â enable Claude Opus 5.5 â but only if someone does it before the 16th rather than during the Monday-morning Slack flood. If you administer a locked-down org, check the policy list against the three November names today.

For individual Pro and Pro+ subscribers thereâs no policy layer: the retired model disappears from the picker and you pick a new one. Community reports differ on what clients do when a retired model is still named in a saved configuration â silent fallback to default or an error depending on the integration â and GitHub doesnât document this behavior, so donât rely on either.

## How do you find hardcoded retired model IDs before they bite?

Grep for them. Model names end up pinned in VS Code workspace settings, CI jobs that call agent CLIs, and team dotfiles, and those are the references nobody remembers until a pipeline fails:

```
$ grep -rniE 'gpt-5\.5|gpt-5\.4|gpt-5 ?mini|gemini-3\.[567]|grok-4\.5|opus-(4\.[78]|5)\b|gpt-5\.6|kimi-k2' \
    --include='*.json' --include='*.yml' --include='*.yaml' --include='*.toml' .
.vscode/settings.json:14:    "github.copilot.chat.model": "gpt-5.5"
.github/workflows/review.yml:23:        model: claude-opus-5
```
Every hit is a config that either already broke in October or breaks November 16. Swap each one for the mapped replacement from the tables above, and while youâre in there, ask whether that pin needs to exist â after three retirement waves in six weeks, a hardcoded Copilot model ID is a scheduled outage.

## Do the retirements affect BYOK setups in Cursor or Cline?

No. These deprecations remove models from GitHub Copilotâs hosted picker; they say nothing about the modelsâ availability on OpenAIâs, Anthropicâs, Googleâs, or xAIâs own APIs, which retire on their providersâ schedules. If you run Gemini 3.8 Flash or Kimi K3 through your own API key in Cursor or Cline, October 19 changed nothing for you. GitHubâs enterprise model policy doesnât govern BYOK models either â a point admins have raised in GitHubâs community discussions â so policy lockdowns neither protect nor break key-based setups.

The fully retirement-proof version of this is a local model: an open-weight coding model served from your own hardware canât be deprecated out from under you. Copilot itself can front local Ollama models in VS Code; hardware sizing for that lives at runaihome.com, and self-hosted FOSS alternatives like Tabby are covered at aifoss.dev.

## Why is GitHub retiring models this fast?

Because the economics upstream moved. OpenAI priced the GPT-6 family roughly 50% below equivalent GPT-5.x API rates at its September 22 launch (VentureBeatâs launch coverage), and Copilot now bills model usage through AI Credits â so every request served on an older, pricier model is margin GitHub donates to nostalgia. Mapping two retired GPT-5.x models onto one Sol tier and two minis onto one Luna tier also shrinks the support matrix GitHub has to test agent mode against.

The cadence since summer: GPT-5.6 Sol/Terra/Luna added July 9; a six-model retirement announced September 18; four models out October 2; six more October 19; three more November 16. GPT-6 Sol was superseded by GPT-6.1 Sol seven days after it reached Copilot. None of this is slowing down, and the practical conclusion isnât âGitHub is being hostileâ â itâs that model identity is now infrastructure that churns quarterly, and your tooling should treat it like a dependency with a lockfile you expect to bump, not an architectural decision.

## Which replacement should you pick?

| Your situation | Pick | Plan needed | Why | 
|---|---|---|---|
| You were on GPT-5.5/5.4 for agent work | GPT-6.1 Sol | Pro+ $39/mo or Business | GitHubâs mapped replacement; balanced interactive + agentic model | 
| You were on GPT-5.5 but pay $10 Pro | GPT-6 Luna | Pro $10/mo | Sol isnât offered on Pro; Luna is the only GPT-6 you can select | 
| You were on Claude Opus 4.8/5 | Claude Opus 5.5 | Per your current planâs model list | GitHubâs mapped replacement for both Opus retirements | 
| You were on any retired Gemini Flash | Gemini 3.8 Flash | Per your current plan | Single replacement for 3.5/3.6/3.7 Flash | 
| You pin models in CI or team configs | Stop pinning; set default + one fallback | â | Three retirement waves in six weeks; pins are scheduled outages | 
| You want out of the churn entirely | Local model via Ollama BYOK | $0 software | Canât be deprecated; needs your own GPU (see runaihome.com) | 

## FAQ

**Is this the same thing as the âCopilot retires 6 models for GPT-5.6â story?**
Same event, outdated details. That framing comes from GitHubâs September 18 announcement, which named GPT-5.6 Sol as the replacement. The actual October 19 retirement table names GPT-6.1 Sol and Grok 4.7 â and GPT-5.6 Terra is itself retired on November 16.

**Do I lose chat history or custom instructions when my model is retired?**
The changelogs describe removing models from Copilot experiences, not touching conversations or instruction files. Your repository custom instructions are model-agnostic and carry over to whatever you select next.

**Iâm an individual annual subscriber â do I really keep Claude Opus 4.8?**
Per the October 15 changelog, yes: individual subscribers on annual plans retain Opus 4.8 access past November 16, 2026. Monthly individuals and all Business/Enterprise seats donât.

**Will the replacement model cost more per request?**
GPT-6 family API pricing launched at roughly half of GPT-5.x equivalents, and in Copilot both Sol and Luna bill through usage-based AI Credits. Whether your monthly credit burn drops depends on your mix; the billing mechanics are in our AI Credits guide.

**How do I know this wonât happen again in December?**
You donât â assume it will. GitHub announced the October 19 wave 31 days ahead and the November 16 wave 32 days ahead, so watch github.blog/changelog monthly and keep model names out of anything that canât be changed in one place.

## Sources

*Last verified October 23, 2026. Model lineups and plan gates change monthly â GitHubâs changelog is the authority; re-check before changing org policy.*

Was this article helpful?

Thanks for the feedback â it helps improve future articles.

## Know which coding tool is worth paying for

Hands-on comparisons of AI coding assistants and what each one costs to run â including the local-model path. Sent only when something changes. Unsubscribe anytime.
