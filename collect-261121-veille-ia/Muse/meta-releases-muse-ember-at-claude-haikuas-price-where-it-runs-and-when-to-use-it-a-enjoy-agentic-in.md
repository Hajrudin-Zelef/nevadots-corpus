---
id: collect-261121-veille-ia/muse/meta-releases-muse-ember-at-claude-haikuas-price-where-it-runs-and-when-to-use-it-a-enjoy-agentic-in
title: "Meta releases Muse Ember at Claude Haikuâs price. Where it runs, and when to use it â Enjoy Agentic Industries"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "ember", "tokens", "model", "meta", "input", "claude", "code"]
source: https://enjoy.dev/blog/muse-ember/
source_anchor: ""
source_lines: [1, 151]
section: "Muse"
---
- To
- Everyone using Muse Code, or choosing a cheap model for subagents
- From
- Mo, Chief Executive
- Date
- November 9, 2026, 9:14 PM CST
- Re
- Muse Ember

# Meta releases Muse Ember at Claude Haikuâs price. Where it runs, and when to use it

On November 9, 2026, Meta released Muse Ember, a small model for subagents and repeated work, in Muse Code on every plan and in its API as muse-ember-1.0. It costs $0.10 in and $0.50 out per million tokens, the list price of Claude Haiku 5.5 and GPT-6 Luna. Artificial Analysis scores it 36, level with Haiku 5.5, at about a third of the cost per task. It isnât open-weight, and it isnât for the hardest code.

## What Meta released

On November 9, 2026, Meta introduced Muse Ember, âour smallest and most efficient model designed for complex reasoning execution and the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing.â Meta says itâs the first of its specialized models âtrained using Muse Wonder Full,â and that it rounds out the lineup: âMuse Wonder for the hardest problems, Muse Spark for coding and agentic workflows, and Muse Ember for repeatable coding and agentic tasks at scale.â

Its ID is `muse-ember-1.0`. The docs page gives it a context window of 1,048,576 tokens, up to 524,288 output tokens, and text, image, video, audio and PDF input. Reasoning effort runs from `minimal` to `max`, and âthe default is `max`,â so a request that sets no effort gets the deepest and most expensive setting.

It isnât open-weight. Metaâs models page lists Ember among the models âhosted on Meta Model API,â and names Muse Glimmer and Muse Spark Open as the ones you run yourself.

## Where you can use it

- **Muse Code.** âMuse Ember is on Model API and in Muse Code, on pay-as-you-go and with any subscription,â says its docs page. The subscription guide lists it from the Everyday Usage plan up, which the Muse Code page prices at $5 a month.
- **Metaâs Model API** , as`muse-ember-1.0` , on the Responses, Chat Completions and Messages endpoints.
- **Google Cloud and Oracle Cloud.** The announcement says âMuse Ember, Muse Spark and Muse Wonder are now available on Google Cloud and Oracle Cloud,â and Googleâs model garden lists Ember.
- **OpenRouter and OpenCode.** OpenRouter has it as`meta/muse-ember` , and OpenCodeâs Console as Muse Ember 1.0, both at Metaâs price.
- **Not Cursor or GitHub Copilot, so far.** Cursorâs model list has Muse Wonder but not Ember, and Copilotâs has no Muse model.

## What it costs

Prices per million tokens, from each companyâs own page on November 9, 2026:

| Model | Input | Cached input | Output | Long prompts, for the whole request | 
|---|---|---|---|---|
| Muse Ember | $0.10 | $0.004 | $0.50 | Over 200K input tokens: $0.30, $0.012 and $1.50 | 
| Claude Haiku 5.5 | $0.10 | $0.01 | $0.50 | Over 100,000 tokens: $0.50, $0.05 and $2.50 | 
| GPT-6 Luna | $0.10 | $0.01 | $0.50 | Over 272K input tokens: twice the input and cached rates, 1.5 times the output | 
| Muse Spark 1.3 | $1.25 | $0.15 | $4.25 | Same rate at any length | 
| Muse Wonder | $4.00 | $0.20 | $20.00 | Same rate at any length | 

Metaâs rates are on its pricing page, Anthropicâs on its own, and OpenAIâs on its pricing and Luna pages.

So Ember isnât cheaper per token than its two rivals. TechCrunch wrote: âAt 10 cents per million input tokens, Ember is significantly cheaper than frontier competitors like Haiku and Luna.â All three list the same $0.10 and $0.50. The difference is in the bill for a finished task. Artificial Analysis says Emberâs âcached input, most of the cost in our agentic evaluations, is 60% cheaper,â and that it âuses roughly a third as many output tokens as Claude Haiku 5.5.â

Four things to know before you budget:

- **Long requests cost three times as much.** A request with more than 200K input tokens âis billed at the long-context rates in full, not only for the tokens past 200K.â The count includes cached tokens and, when you continue a stored conversation, that conversation.
- **Reasoning is billed as output.** Metaâs reasoning guide says reasoning tokens count âtoward billed output tokens,â and Ember reasons at`max` unless you set a lower effort.
- **Thereâs no batch price.** Anthropic and OpenAI list batch processing at half price, $0.05 in and $0.25 out for Haiku 5.5 and Luna. Metaâs pricing page lists no batch rate.
- **On the API, your content isnât used for training.** Ember âruns in Standard mode only,â where âyour content is not used to train Meta models.â A Muse Code subscription is different: âMuse Code subscriptions default to Contributor mode,â which lets Meta train on your content until you turn it off in`/settings` .

## How good it is

The independent numbers are Artificial Analysisâs, from its own article and the results Metaâs announcement reproduces, all at `max` effort:

| Test | Muse Ember | Claude Haiku 5.5 | GPT-6 Luna | 
|---|---|---|---|
| Intelligence Index | 36 | 36 | 28 | 
| Cost per index task | $0.14 | $0.41 | $0.15 | 
| Coding Agent Index, each in its own labâs agent | 46 | 37 | 31 | 
| FrontierCode 2.0 | 27.2% | 34.9% | 25.2% | 

Ember ties Haiku 5.5 on the index at about a third of the cost, and its model page there measures 449 output tokens a second. It sits â8 points behind Muse Wonder for about 1/50th of the Cost per Task.â By Metaâs own table, Muse Spark 1.3 scores one point more, 37, at $2.39 a task.

It has two weak spots. Coding is âmixedâ: Ember beats Haiku 5.5 on Terminal-Bench 4.0, 40.9% to 32.8% in Artificial Analysisâs run, but trails it on FrontierCode, which judges whether a maintainer would merge the change. And facts: âKnowledge is the main weakness, but it rarely hallucinates.â It answers 21.1% of AA-Omniscience questions correctly, âmostly because it declines to answer 64% of questions.â That keeps its hallucination rate at 17.4%, âhalf of Claude Haiku 5.5âs.â Give it the documents or a search tool before you ask it for facts.

Metaâs own table is kinder to Haiku in one row and harder to check in others. It gives Haiku 39.2% on Terminal-Bench, Anthropicâs own Claude Code figure, and its evaluation report says the rival runs Meta did itself âremain best-effort: prompts, tools, and runtime may not be optimally tuned for proprietary models.â Those include the computer-use test, OSWorld 2.1, where Meta puts Ember at 69.5% with partial credit, against 68.9% for Haiku 5.5 and 50.5% for Luna.

## How to use it in Muse Code

To run a whole session on Ember, start with `muse --model muse-ember-1.0`, or switch with `/models` in a running session, as the configuration guide describes.

The use Meta built it for is different: Muse Wonder or Muse Spark leads, and Ember runs the subagents. Muse Code doesnât do that by itself. âBy default, a subagent runs on the same model as your main session,â says the custom agents guide, and the lead âdoesnât switch its subagents to a lower-cost model on its own.â You have two ways to ask:

1. **Name the model in your prompt** , for a single request. Metaâs example: âSplit the test suite into four parts and run each part in a subagent using muse-ember-1.0. If that model isnât available, stop and tell me.â Use the exact ID: âWith only the modelâs name, the lead guesses an ID, and Muse Code rejects a wrong one.â Meta adds that this works âwhen workflows are available in your build.â
2. **Define an agent** , for repeated runs. Save this as`agents.json` and start Muse Code with`muse --model muse-wonder-1.0 --reasoning-effort high --agents "$(cat agents.json)"` :

```
{
  "subagent": {
    "description": "Executes one well-defined coding part of a larger plan, such as implementing a module, adding tests or fixing a bug.",
    "prompt": "You receive one part of a larger plan from the lead. Complete that part as the task describes, creating or changing the files it needs. Run the checks the task names. Reply with what you changed and whether the checks pass.",
    "model": "muse-ember-1.0",
    "effort": "medium"
  }
}
```
Muse Code doesnât save what you pass with `--agents`, so pass it each time, or keep the definition as a Markdown file in `.agents/agents/` in your project. The built-in `general-purpose` subagent still runs on the leadâs model unless you define it too, so âa part meant for a Muse Ember subagent runs on Muse Wonder instead and costs more.â Before a long run, check that `muse-ember-1.0` is in your `/models` list. On a subscription, a request over 200K input tokens âuses your planâs usage faster.â

## How to use it in Claude Code and other agents

Metaâs coding agents guide connects Claude Code to its API with a Meta API key, here in `MODEL_API_KEY`. You pay Meta per token, and your Claude plan isnât used:

```
export ANTHROPIC_BASE_URL="https://api.meta.ai"
export ANTHROPIC_AUTH_TOKEN="$MODEL_API_KEY"
export ANTHROPIC_MODEL="muse-wonder-1.0"
export ANTHROPIC_DEFAULT_OPUS_MODEL="muse-wonder-1.0"
export ANTHROPIC_DEFAULT_SONNET_MODEL="muse-spark-1.3"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="muse-ember-1.0"
export CLAUDE_CODE_SUBAGENT_MODEL="muse-ember-1.0"
export ENABLE_TOOL_SEARCH="true"
```
The guideâs advice is âPin every model alias,â so that no path âfalls back to a Claude model Model API doesnât serve,â and to use `ANTHROPIC_AUTH_TOKEN`, not `ANTHROPIC_API_KEY`. Our guide to other models in Claude Code covers the other routes.

For Codex, Meta says to set `muse-wonder-1.0` as the default model and switch to `muse-spark-1.3` âfor faster, lower-cost sessions.â It doesnât suggest Ember there. In your own agent loop, Ember âaccepts the same request body as Muse Spark, so switching most examples is a model ID change.â

## When to pick Ember, and when not to

Metaâs Choose a model page sorts the work:

1. **Subagent tasks, tool loops, routing, classification, and the same job over thousands of files:** Ember.
2. **The main loop of a coding agent, and changes across many files:** Muse Wonder, or Muse Spark when the main loop has to cost less.
3. **Hard single-shot coding problems, knowledge-heavy questions and visual coding:** âuse Muse Spark or Muse Wonder rather than Muse Ember.â
4. **Audio.** Ember accepts it, but Metaâs table sends audio understanding to Muse Spark 1.2.
5. **Large offline jobs where half price matters more than speed:** Haiku 5.5 or Luna through their batch APIs.
6. **A model on your own hardware:** not Ember. See running a coding agent on a local model.

## Ember in Enjoy

Enjoy runs Muse Code on your Muse Code subscription, and its **Model** column lists the models Muse Code offers your account. Muse Code only updates that list when its own app starts, so a model released today can be missing: in the desktop app, select the circular arrow beside **Model** and Enjoy has Muse Code fetch its latest models.

A conversation can use a different model for each message, so Muse Wonder can plan a change and Ember can do the routine steps in the same thread, and you can hand the conversation to Claude Code or Codex when you want a second opinion. Enjoy shows whatâs left of each Muse Code limit once it has replied in the project.

Enjoy doesnât add Metaâs models to another labâs account, and Contributor mode stays on in Enjoy until you turn it off in Muse Codeâs settings. Enjoy is a monthly subscription, with the first seven days free (pricing).

## What Meta hasnât said

- **How big Ember is.** Itâs âour smallest,â with no parameter count.
- **Whether weights will follow.** The announcement says only: âWe have even more efficient models in training and look forward to sharing whatâs next.â
- **How much of a plan it uses** in Muse Code compared with Muse Spark or Muse Wonder.
- **Which Muse Code version first offers it.** The newest entry in the changelog, 1.4.13, doesnât mention Ember.
- **A knowledge cutoff.** The announcement, docs page and model page donât give one.

Weâll update this memo when Meta fills these in.

## 10.Questions.

- ### Is Muse Ember out?
- Yes. Meta released it on November 9, 2026. Itâs in Muse Code on pay-as-you-go and every subscription, the $5 plan included, and in Metaâs Model API as muse-ember-1.0. Meta says itâs also on Google Cloud and Oracle Cloud, and OpenRouter and OpenCodeâs Console list it.
- ### How much does Muse Ember cost?
- In Metaâs API, $0.10 per million input tokens, $0.004 cached and $0.50 output. A request with more than 200K input tokens is billed in full at $0.30, $0.012 and $1.50. In Muse Code it comes with every plan, from $5 a month, and draws on the planâs usage.
- ### Is Muse Ember cheaper than Claude Haiku 5.5?
- Not per token. Muse Ember, Claude Haiku 5.5 and GPT-6 Luna all list $0.10 in and $0.50 out per million tokens. Per task it is, by Artificial Analysisâs measure, which puts Ember at $0.14 a task and Haiku 5.5 at $0.41 for the same score, because Emberâs cached input costs less and it writes about a third as many output tokens.
- ### Is Muse Ember good for coding?
- For small, well-defined parts, yes. In Muse Code it scores 46 on Artificial Analysisâs Coding Agent Index, ahead of Claude Haiku 5.5 in Claude Code at 37 and GPT-6 Luna in Codex at 31. It trails Haiku 5.5 on FrontierCode 2.0, 27.2% to 34.9%, and Meta says to use Muse Spark or Muse Wonder for hard single-shot coding problems.
- ### Can I use Muse Ember in Claude Code?
- Yes, with a Meta API key, paid per token rather than on a Claude plan. Metaâs guide points Claude Code at its API, puts Muse Wonder on the main loop, and sets the haiku tier and the subagent model to muse-ember-1.0. Set every model variable, or some work falls back to a Claude model that Metaâs API doesnât serve.
- ### Is Muse Ember open source, and can I run it locally?
- No. Muse Ember is a hosted model, and Meta hasnât published its weights or said how big it is. Metaâs open-weight models are Muse Glimmer and Muse Spark Open, which you download and run on your own hardware.
