---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-retires-3-models-nov-16-your-migration-checklist
title: "GitHub Copilot Retires 3 Models Nov 16: Your Migration Checklist"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["opus", "github", "copilot", "model", "claude", "models", "gpt-6", "changelog"]
source: https://dev.to/zforzain2000/github-copilot-retires-3-models-nov-16-your-migration-checklist-2ml6
source_anchor: ""
source_lines: [1, 481]
section: "Copilot_Microsoft"
---
GitHub published a changelog entry on October 15 with a date and three

  model names: on November 16, 2026, Claude Opus 4.8, Claude Opus 5, and

  GPT-5.6 Terra disappear from every GitHub Copilot experience. Not just

  chat - inline edits, ask and agent modes, code completions, all of it.

  I read the entry twice, then checked what it does not say, because the

  sentence that worries me is the one most people will skip: "Please

  update your workflows and integrations to use the supported models

  before this date." This post is what that sentence actually asks you to

  do, step by step, before the clock runs out.


On November 16, 2026, GitHub removes Claude Opus 4.8, Claude Opus 5, and

GPT-5.6 Terra from every Copilot surface. The suggested swaps are Claude

Opus 5.5 (for both Opus models) and GPT-6.1 Sol or GPT-6 Luna (for Terra).

Only individual annual subscribers keep Opus 4.8; everyone else loses all

three. Audit anything that names these models by ID this week - prompts,

scripts, agents, CI jobs, IDE settings - and confirm the replacements are

enabled in your org's model policies. GitHub removes the old models

itself; your job is everything that points at them.

GitHub's changelog table is unambiguous, so I am reproducing it exactly

rather than paraphrasing it:

| Model | Deprecation date | Suggested alternative | 
|---|---|---|
| Claude Opus 4.8* | 2026-11-16 | Claude Opus 5.5 | 
| Claude Opus 5 | 2026-11-16 | GPT-6.1 Sol or GPT-6 Luna | 
| GPT-5.6 Terra | 2026-11-16 | GPT-6.1 Sol or GPT-6 Luna | 

* Claude Opus 4.8 stays available to individual GitHub Copilot

subscribers on annual plans. That is the only exception. Everything else

in this section applies to everyone.

Two details in the changelog wording matter more than the table itself.

First, the scope: "across all GitHub Copilot experiences (including

Copilot Chat, inline edits, ask and agent modes, and code completions)."

If your mental model is "the model picker in chat," widen it now. The

model you pinned for inline edits in VS Code settings, the one your

Copilot CLI script passes with a flag, the one your custom agent

definition names - all of them are in scope until you prove otherwise.

Second, this is the third wave of a steady pattern, not a one-off. GitHub

retired earlier model generations through 2026 on the same cadence. The

takeaway is not that Opus 5 was bad. The takeaway is that any model name

you hardcode into your workflow has a shelf life measured in months, and

the changelog is the only advance notice you get.

The Opus 4.8 carve-out is narrow, and narrow exceptions are where teams

make their most expensive wrong assumptions. Read it carefully: Claude

Opus 4.8 "will remain available to individual GitHub Copilot subscribers

on annual plans so they retain an Opus offering."

That sentence leaves three groups out in the cold. If you pay monthly as

an individual, you lose Opus 4.8. If your seat comes through Copilot

Business or Copilot Enterprise, you lose Opus 4.8 - organization-managed

seats are explicitly not covered. And nobody keeps Opus 5 or GPT-5.6

Terra on any plan.

Here is the judgment call I would make if I qualified for the exception:

I would still migrate. Running agent workflows on Opus 4.8 while the

rest of your team runs Opus 5.5 splits you across two model generations,

and the exception is a retention gesture, not a commitment. GitHub framed

it as "so they retain an Opus offering" - Opus 5.5 is the offering going

forward. The exception keeps you comfortable today and stranded later.

Use the remaining weeks to move to 5.5 and test it, same as everyone

else.


  GitHub does not specify what happens to a request that names a retired

  model after November 16. It may fail outright, or it may get silently

  rerouted to whatever default GitHub assigns. I am not going to pick one

  of those for you as fact, because the changelog does not. The safe

  engineering assumption is that a pinned reference is broken by default,

  and that is how the checklist below treats it.


This is the part the changelog's one sentence - "update your workflows

and integrations" - actually requires. I have ordered it the way I would

run it: the audit first, the admin check second, the judgment calls

third.


  

    The three retiring names are Claude Opus 4.8, Claude Opus 5, and

    GPT-5.6 Terra. The replacements are Claude Opus 5.5, GPT-6.1 Sol, and

    GPT-6 Luna. Commit that mapping now, because the rest of the work is

    finding every place the old names live. A checklist built on the wrong

    scope is worse than no checklist - it gives you confidence you have

    not earned.

  

  

    Chat pickers are the easy part. The failures show up in unattended

    places: prompt files, CI workflows, Copilot CLI scripts, custom agent

    definitions, workspace settings that store a default model, and the

    team runbooks that tell people which model to select. Run a search

    across your repos before the cutoff. Two searches cover the common

    spellings:


````
```
git grep -niE 'opus[ -]4[.-]8|5\.6[ -]terra'
git grep -niE 'opus[ -]5([^.0-9-]|$)'
```
````
```
The second pattern matches `opus-5` and `Opus 5` while skipping
`opus-5-5` and `Opus 5.5`. Adjust for how your tooling spells model
IDs - display names and machine identifiers often differ. Treat every
hit as a bug until it points at Opus 5.5, GPT-6.1 Sol, or GPT-6 Luna.
```

  

    This is the step that silently breaks teams even when every

    individual does everything right. Under default model enablement, the

    replacements are automatically enabled for Copilot Business and

    Enterprise customers - unless an administrator turned off the global

    default or explicitly disabled the model. If your org did either,

    the new models stay off until someone enables them through the model

    policies in Copilot settings. Open your enterprise or org model

    configuration, find Opus 5.5, GPT-6.1 Sol, and GPT-6 Luna, and

    confirm a non-admin user can actually select them in chat. A

    replacement that exists but is not selectable is the same as no

    replacement.

  

  

    Prompts tuned against Opus 5's output will not necessarily behave the

    same on Opus 5.5, and prompts written for Terra will not behave the

    same on Sol or Luna. Until November 16 you can still run the old

    model and the new one side by side inside the tool your team already

    uses. That is the window. Pick the three or four prompts your team

    runs most, run each against both models, and diff the results. Yes,

    you pay for both runs. The cost of two eval runs is trivial next to

    the cost of discovering a behavior change in production on November

    17.

  

  

    GitHub gave Terra users two replacements in different categories,

    and the cheaper one will become your default by habit if you let it.

    Do not accept that silently. Run both GPT-6.1 Sol and GPT-6 Luna

    against a few of your real tasks this week and compare quality, not

    just price. The price-per-token math only matters after a model

    clears your quality bar. I would rather pay Sol's rate for a model

    that passes my checks than save money on Luna output I have to

    regenerate.

  

  

    The grep sweep finds machine references. It does not find the

    onboarding doc that says "select Opus 5 for the hard refactors," the

    wiki page with last quarter's model recommendations, or the Slack

    message pinned in your team channel. Search those too. A runbook that

    names a dead model trains every new hire to configure a broken

    default.

  

  

    GitHub is explicit: "No action is required to remove the models

    after they have been deprecated." It removes them. Resist the urge

    to build a removal ritual or a post-cutoff cleanup ticket. The work

    is all on the replacement side - enabled, selectable, evaluated,

    documented. Once that is done, the cutoff is a nonevent for you,

    which is the whole point.

  


- Removing the retired models from the picker after Nov 16
- Auto-enabling the replacements under default model enablement
- Keeping Opus 4.8 for individual annual subscribers
- Finding every pinned model name in repos, scripts, agents, CI, and docs
- Enabling the replacements when your org turned off the global default
- Re-running evals on the replacements before the window closes
- Choosing between Sol and Luna deliberately

Here is the part of the story most coverage gets wrong by omission. None

of these models is being retired by the company that makes it. Anthropic's

own model schedule keeps Claude Opus 5 available on its platforms until

at least July 24, 2027, and Claude Opus 4.8 until at least May 28, 2027 -

both well past the November 16 Copilot cutoff. After the date, you can

still call them through the vendors' own APIs.

That distinction is worth more than trivia. It means November 16 is a

routing event, not a capability event. If you have workflows that genuinely

need Opus 5's exact behavior and Copilot is your only route to it, the

migration question is not "which Copilot model" but "which route" -

Anthropic's API, a different IDE integration, or a gateway. I covered the

cost math of routing work between models in my Haiku 5.5 cost

post, and the same logic applies

here: know what each route costs per task before you commit to it.

And if you are rethinking which AI coding setup deserves your money after

all this churn, I compared the current pricing landscape across the major

tools in my X vs Cursor cost breakdown

- a retirement wave is a good moment to check whether your whole stack still earns its seat.

I am going to name the framework because it is the decision rule I will

actually use from now on:

**The Dependency Rule:** a pinned model name is a dependency with a

lifecycle. Inventory it like one.


When you pin `some-model-5` in a config, you have done exactly what you

do when you pin `some-library@5` in package.json: you have taken a

dependency on a specific version of someone else's software, with a

deprecation date you do not control and a migration path you did not

write. The healthy version of this is the healthy version of dependency

management. Keep a short inventory: which model IDs each workflow uses,

which config selects them, who owns the lifecycle notices, and what

behavior must survive a swap. Record the version where the provider

exposes it - a friendly display name may not describe the exact

configuration you tested.

The unhealthy version is what most teams have today: model names

sprinkled through dotfiles, prompt files, and chat settings, owned by

nobody, discovered only when the picker entry vanishes. This wave cost

those teams a scramble. The next wave - and there will be a next wave,

the cadence through 2026 makes that plain - will cost them the same

scramble unless the inventory gets built now, while the memory of this

one is fresh.

Sixty days is a short life for a flagship model. Shorter than most

procurement cycles, shorter than a careful evaluation, shorter than the

comparison post that takes six weeks to write. The measurement is the

only thing that compounds. Build the inventory, run the evals, and stop

treating a model picker like a permanent fixture.

{/* MONETIZATION NOTE (not rendered): the links below are official product

    and documentation pages. If GitHub, Anthropic, or an AI tooling vendor

    opens an affiliate program, swap in affiliate links here and update the

    disclosure. Never claim affiliate status before it exists. */}

| Tool | What it does | Why it matters here | 
|---|---|---|
| GitHub Changelog | The official Nov 16 deprecation notice | The primary source for the table, the exception, and the enablement rules | 
| GitHub Copilot docs | Supported models and model policies | Where admins enable the replacement models | 
| Anthropic API | Direct API access to Claude models | Opus 5 and 4.8 stay available here until mid-2027 at the earliest | 


  The links above are plain official links, not affiliate links. If that

  changes I will say so on the page, same policy as every post here.


Three: Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra. GitHub's October

15, 2026 changelog entry says the deprecation covers every Copilot

experience - Chat, inline edits, ask and agent modes, and code

completions. The suggested replacements are Claude Opus 5.5 for both Opus

models and GPT-6.1 Sol or GPT-6 Luna for Terra.

Only one carve-out exists: individual GitHub Copilot subscribers on

annual plans keep Claude Opus 4.8, so they retain an Opus offering. The

exception does not cover monthly individual plans, Copilot Business, or

Copilot Enterprise seats - and it does not apply to Opus 5 or GPT-5.6

Terra for anyone.

Anything that names a retiring model by ID is at risk: prompt files, CI

workflows, Copilot CLI scripts, custom agent definitions, workspace

settings with a pinned default model, and team runbooks. GitHub's

changelog asks you to update workflows and integrations before the date

but does not spell out the exact failure mode - requests may fail outright

or get rerouted to a different default. Treat every pinned reference as

broken until you move it.

GitHub's suggested swaps are Claude Opus 5.5 (for Opus 4.8 and Opus 5)

and GPT-6.1 Sol or GPT-6 Luna (for GPT-5.6 Terra). Under default model

enablement, the replacements are automatically enabled for Copilot

Business and Enterprise customers unless an admin turned off the global

default or explicitly disabled the model. If that default is off, enable

them through the model policies in Copilot settings.

No. GitHub's changelog states plainly that no action is required to

remove the models after they are deprecated - GitHub removes them. Your

job is the other direction: making sure the replacements are enabled and

selectable, and updating anything that pins the old names, before

November 16.

Just in Copilot. Anthropic's own model schedule keeps Claude Opus 5

available on its platforms until at least July 24, 2027 and Claude Opus

4.8 until at least May 28, 2027. This is a Copilot routing decision, not

a model shutdown - the same model can be alive at the vendor and dead

inside your IDE.

Model retirements, eval reruns, workflow audits - this is the kind of

unglamorous work that keeps a team's AI tooling from breaking on a Monday

morning. I help teams and founders handle exactly this: auditing pinned

model references across repos and CI, setting up replacement evals, and

building the model inventory so the next retirement wave is a nonevent.

If your team runs on Copilot and November 16 is approaching fast, reach

out through the contact section on my homepage and we will

sort it before the cutoff.


  A pinned model name is a dependency with a lifecycle. Inventory it like one.


## Top comments (0)
