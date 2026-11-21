---
id: collect-261121-veille-ia/qwen/swap-to-qwen4-max-the-exact-setup-for-your-coding-agent-your-always-on-agent-and-your-automations
title: "Swap to Qwen4-Max: the exact setup for your coding agent, your always-on agent and your automations"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["openrouter", "agent", "model", "qwen4-max", "qwen", "coding", "alibaba", "tool"]
source: https://codewithnishant.dev/free/qwen4-max-swap-setup/
source_anchor: ""
source_lines: [1, 247]
section: "Qwen"
---
The reel showed the cost. This page is the setup. Three places to swap: your coding agent, your always-on AI agent and the automations you already run every day. Each step comes from the tool's own docs, linked, and anything I put together from two docs is labelled.

Same test task: about $3 on Qwen4-Max, about $10 on Opus.

// Build this with your AI agent

## Build this with your AI agent

Want a shortcut? Copy the prompt below into any AI assistant with this guide's link. It asks which tools you use, picks the right section for each one and walks you through it. It buys nothing and will not touch your keys.

Read this guide: https://codewithnishant.dev/free/qwen4-max-swap-setup/ and the official pages it links to.
Help me swap one of my AI tools to Qwen4-Max:
1. Ask me which of these I use: Qwen Code, Cline, Kilo Code, Claude Code, Hermes Agent, OpenClaw, n8n, Zapier. Also ask whether I already have an OpenRouter key or an Alibaba Cloud Model Studio key.
2. Tell me which section of the guide covers each tool and which key type I should use for it (interactive coding tool or unattended job).
3. Walk me through ONE tool at a time, using the exact model ID and commands from that section.
4. Tell me what to run to confirm the swap worked, and how to switch back to my old model.
Never ask me to paste an API key into this chat. I will put keys in my own settings. Do not buy, subscribe or change anything without asking me first.

## 01. Before you swap: model IDs and which key

**The short answer:** the model is called `qwen4-max` on Alibaba Cloud and `qwen/qwen4-max` on OpenRouter. OpenRouter is the easiest single key for every tool below.

| Where you call it | Model ID | Source | 
|---|---|---|
| Alibaba Cloud Model Studio (Qwen Cloud) | `qwen4-max` | Alibaba model page | 
| OpenRouter | `qwen/qwen4-max` | OpenRouter model page | 
| OpenClaw, through OpenRouter | `openrouter/qwen/qwen4-max` | OpenClaw's `openrouter/<provider>/<model>` pattern | 

- **Price:** $1.50 per million input tokens, $4.50 per million output, $0.075 per million for cached input (OpenRouter, same on Artificial Analysis).
- **What it takes in and gives out:** text, image and video in per Alibaba and OpenRouter (Artificial Analysis lists text and image), text out, tool calling and structured output supported. Context is 1M tokens per Alibaba and OpenRouter, 984k per Artificial Analysis.
- **Closed weights:** you can only reach it through an API. Smaller open Qwen4 models are promised, not out.

**Which key where, said once.** Alibaba lists three kinds of key for its models. Token Plan and Coding Plan keys are for interactive coding tools only, not backend services (Alibaba endpoint list). OpenClaw's docs go further and warn that using a Token Plan for cron jobs or unattended scripts can suspend the subscription or revoke the key (OpenClaw Qwen page). So: a coding tool you sit in front of can use any of the three. Anything that runs by itself (sections 03 and 04) should use a pay-as-you-go Model Studio key or an OpenRouter key.

**Where to get an OpenRouter key:** openrouter.ai/keys. Put it in the tool's own settings or `.env` file. Never paste it into a chat.

## 02. Your coding agent

**The short answer:** if your coding agent lets you add a custom model, you can point it at Qwen4-Max in a few minutes. Four agents below, each from its own docs.

### Qwen Code

Alibaba's own terminal agent. Docs: Qwen Code model providers.

Qwen Code reads models from `modelProviders` in `settings.json`, takes the key from an environment variable you name in `envKey`, and lets you switch with `/model`. Its docs say OpenRouter works through the `openai` provider type. Add this to `~/.qwen/settings.json`:

```
{
  "env": { "OPENROUTER_API_KEY": "sk-or-your-key-here" },
  "modelProviders": {
    "openai": [
      {
        "id": "qwen/qwen4-max",
        "name": "Qwen4-Max (OpenRouter)",
        "envKey": "OPENROUTER_API_KEY",
        "baseUrl": "https://openrouter.ai/api/v1"
      }
    ]
  }
}
```
Then open Qwen Code and run `/model`. Qwen Code says edits to `modelProviders` are picked up in a running session, so reopen the `/model` list if you do not see the new entry yet. Going direct to Alibaba instead: use `"id": "qwen4-max"`, `"envKey": "DASHSCOPE_API_KEY"` and the Singapore address `https://dashscope-intl.aliyuncs.com/compatible-mode/v1` from Alibaba's endpoint page.

**What is assembled, not copied:** the entry is the shape from Qwen Code's docs with the Qwen4-Max ID filled in. The same docs say effort levels are passed through for the `qwen4` family of model names, topping out at `xhigh`. I did not run it.

### Cline and Kilo Code (through OpenRouter)

Docs: Cline with OpenRouter, Kilo Code with OpenRouter.

1. **Cline:** click the settings icon in the Cline panel, choose**OpenRouter** in the API Provider dropdown, paste your OpenRouter key, then pick the model in the Model dropdown. Look for`qwen/qwen4-max` .
2. **Kilo Code:** open Settings (gear icon), go to the Providers tab, add OpenRouter with your key. Kilo stores it in your`kilo.json` . Then choose`qwen/qwen4-max` as the model.
3. Give it one small, real task and read the result before you hand it anything big.

OpenRouter shows only one provider serving this model right now (Alibaba Cloud International) and forwards every request to it without routing, so there is no second provider to fail over to.

### Claude Code, with Alibaba's Anthropic-compatible endpoint

Docs: Alibaba Cloud, Claude Code with Model Studio.

**The short answer:** yes, it works, and it is Alibaba's own documented path, not a workaround. Alibaba lists an Anthropic-compatible endpoint for Qwen models and a note for Qwen4-Max: set Claude Code's Opus slot to `qwen4-max`.

Alibaba's example for the Token Plan, with the Opus line set to Qwen4-Max as its note says. Put it in `~/.claude/settings.json` (macOS and Linux) and replace `YOUR_API_KEY`:

```
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "ANTHROPIC_BASE_URL": "https://token-plan.ap-southeast-1.maas.aliyuncs.com/apps/anthropic",
    "ANTHROPIC_MODEL": "auto",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "qwen3.6-flash",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "qwen3.8-flash",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "qwen4-max",
    "CLAUDE_CODE_SUBAGENT_MODEL": "auto",
    "CLAUDE_CODE_MAX_CONTEXT_TOKENS": "983616"
  }
}
```
- If long answers get cut off, Alibaba says to set `CLAUDE_CODE_MAX_OUTPUT_TOKENS=131072` for`qwen4-max` .
- Check it with Alibaba's own test: `claude -p "Hello"` . Open a new terminal after you change the file.
- **Pay-as-you-go instead of Token Plan:** Alibaba's pay-as-you-go example uses`https://{WorkspaceId}.ap-southeast-1.maas.aliyuncs.com/apps/anthropic` with your Model Studio key and shows`qwen3.7-max` . Putting`qwen4-max` in that example is my assembly from two parts of the same page, not a tested config.
- **Heads up:** this points your whole Claude Code at Alibaba's models. To switch back, remove the`env` block you added.

## 03. Your always-on AI agent

**The short answer:** an always-on agent runs by itself, so use a pay-as-you-go key here (OpenRouter is the shortest path), not a Token Plan or Coding Plan key. Both agents below have a documented way to change the model.

### Hermes Agent

Docs: OpenRouter's Hermes page, Hermes providers.

1. Run `hermes model` in your terminal, outside a chat. It is the setup wizard for providers and models. Choose**OpenRouter** , enter your key and pick the model.
2. Or set it by hand. Put the key in `~/.hermes/.env` as`OPENROUTER_API_KEY=sk-or-...` (Hermes keeps keys there and model settings in`config.yaml` ), then edit`~/.hermes/config.yaml` :

```
model:
  provider: openrouter
  default: qwen/qwen4-max
```
1. Inside a running chat, `/model` only switches between providers you have already set up. To add a new provider, exit and run`hermes model` again.
2. Optional safety net: Hermes supports `fallback_providers` in the same file, so a failed request can fall back to another model:

```
fallback_providers:
  - provider: openrouter
    model: ~anthropic/claude-sonnet-latest
```
The Hermes docs also list a native **Alibaba** provider that uses a `DASHSCOPE_API_KEY` for programmatic and production use. I did not confirm that it already lists `qwen4-max`, so the OpenRouter route above is the one I can point to. The `qwen/qwen4-max` line is the OpenRouter docs' format with the Qwen4-Max ID filled in.

### OpenClaw

Docs: OpenClaw with OpenRouter, agent models config, model selection.

1. Connect OpenRouter once: `openclaw onboard --auth-choice openrouter-api-key` . OpenClaw's docs say this defaults to`openrouter/auto` .
2. Switch to Qwen4-Max: `openclaw models set openrouter/qwen/qwen4-max` . The docs pattern is`openrouter/<provider>/<model>` , and any ref in that form resolves against OpenRouter's live model list.
3. Check what is live with `openclaw models list` .
4. Optional fallback. In the agent config, `agents.defaults.model` takes a primary plus ordered fallbacks:

```
{
  agents: {
    defaults: {
      model: {
        primary: "openrouter/qwen/qwen4-max",
        fallbacks: ["anthropic/claude-opus-4-6"],
      },
    },
  },
}
```
**If you get "model is not allowed":** OpenClaw's docs say this comes from `agents.defaults.modelPolicy.allow`. When that list is set, a model outside it is refused, so add the model to that list.

OpenClaw also has its own Qwen provider (Qwen Cloud). Its docs list pay-as-you-go setup with `openclaw onboard --auth-choice qwen-standard-api-key`, but their examples show older Qwen models, so I could not confirm Qwen4-Max there yet. The fallback ID in the example above comes from OpenClaw's own docs, so change it to whichever model you trust as your backup.

**Before you leave an agent running overnight:** OpenRouter shows one provider for this model with uptime and availability below 100% over the last three days (availability 92.64% when I looked), and Artificial Analysis calls it wordy. A fallback line is worth the two minutes.

## 04. The automations you already run

**The short answer:** in n8n the model is one dropdown on the chat-model node. In Zapier the built-in AI step has no Qwen option, so use the OpenRouter app.

### n8n

Docs: n8n OpenRouter Chat Model node.

1. Open the workflow with your AI Agent (or chain) node.
2. On its **Chat Model** input, replace the current model node with**OpenRouter Chat Model** .
3. Create an OpenRouter credential with your API key.
4. In the **Model** field, choose Qwen4 Max (`qwen/qwen4-max` ). n8n's docs say it loads the list from OpenRouter, and you only see models your account can use.
5. Run the workflow once on a real, harmless example and compare the output with your old model's.

The node's options include a response format (Text or JSON), maximum tokens, temperature, timeout and retries. If your workflow needs strict JSON, set the format and test it, because that is where model swaps usually break.

### Zapier

Docs: AI by Zapier, Zapier's OpenRouter app.

AI by Zapier, the built-in AI step, lists these providers: OpenAI, Anthropic, Google Gemini, Azure OpenAI and Amazon Bedrock. Qwen is not one of them, so the swap goes through OpenRouter:

1. In your Zap, click **+** to add an action and search for**OpenRouter** .
2. Choose the **Generate Response** action. Zapier lists it as calling the selected model to generate an AI response to your prompt.
3. Connect your OpenRouter account with your key.
4. Fill the **Prompt** , an optional**System Prompt** and the required**Model** field with`qwen/qwen4-max` .
5. Map the output into your next step, then test the Zap on a real example.

I did not verify how the Model field shows up in Zapier's editor, whether a dropdown or free text, or how your plan counts this step against your task limit. Check both before you move a high-volume Zap.

## 05. Cost per task vs Opus, and where Opus still wins

**The short answer:** on Artificial Analysis's test task, Qwen4 Max costs $3.27 and Claude Opus 5.5 costs $10.04. It is cheaper, not smarter.

| Model | Cost per task | Intelligence score | 
|---|---|---|
| OpenAI GPT-6.1 Sol (max) | $1.27 | 55 | 
| Qwen4 Max | $3.27 | 43 | 
| Claude Opus 5.5 (max) | $10.04 | 59 | 

Source: Artificial Analysis, cost to run its Intelligence Index tasks, read on 16 November 2026. This is one test, not your bill.

- **Where Qwen4-Max wins:** price. Artificial Analysis says it is about 3x cheaper per task than Alibaba's previous big model, and part of the reason is that it used 39% fewer output tokens.
- **Where Opus still wins (Alibaba's own chart):** ProgramBench, which rebuilds whole programs. Opus scores 65.0, Qwen4-Max 31.5. For that kind of work, stay on Opus.
- **Where Sol still wins:** GPT-6.1 Sol is both cheaper ($1.27) and higher-scoring (55) on Artificial Analysis. If price per task is all you care about, Qwen4-Max is not the cheapest smart model.
- **Where Alibaba says Qwen4-Max is ahead of Opus (its own numbers, not independent):** legal agent work 21.3 vs 9.2, long multi-step tasks 46.7 vs 43.4, SaaS workflow automation 72.5 vs 69.5, and coding agent tasks 75.7 vs 74.2. Treat those as Alibaba's claims until you have tried your own work.
- **The catch:** Artificial Analysis calls Qwen4 Max very verbose, 127M output tokens on its tests against a 78.6M median. Wordy answers cost money, so watch your real cost per finished job, not the price per token.

## 06. Move the routine jobs, not everything

The saving shows up when you run lots of tasks. A good first move is simple: pick one routine job you already run (a lead follow-up draft, a document summary, a support reply), swap only that, and compare the result and the cost on a week of real runs.

1. Keep the old model as a fallback or one click away.
2. Change one tool at a time so you know what caused a difference.
3. Read the first ten outputs yourself before you trust the rest.
4. Keep Opus for the hard stuff, like building whole apps from scratch.

More on models and agents: Claude Fable 5.2 settings that matter, Gemini 4 Hydrogen guide and code mode for AI agents.

## FAQ

### What is the model ID for Qwen4-Max?

On Alibaba Cloud Model Studio the model ID is qwen4-max. On OpenRouter it is qwen/qwen4-max. In OpenClaw the OpenRouter version is written openrouter/qwen/qwen4-max.

### Is Qwen4-Max really 3x cheaper than Claude Opus?

On Artificial Analysis's own test task, yes: $3.27 per task for Qwen4 Max against $10.04 for Claude Opus 5.5. That is one benchmark task, not your bill. Your cost depends on how many tokens your own jobs use and how often the model needs a retry.

### Does Qwen4-Max beat Claude Opus?

Not overall. Artificial Analysis scores Qwen4 Max 43 and Opus 5.5 59. In Alibaba's own launch chart Qwen4-Max is ahead on a few agent tests, but Opus scores 65.0 against 31.5 on ProgramBench, which rebuilds whole programs. Keep Opus for that kind of work.

### Can I use Claude Code with Qwen4-Max?

Alibaba's own Claude Code page says yes, through an Anthropic-compatible endpoint, and shows how to map qwen4-max to Claude Code's Opus slot. The Token Plan and Coding Plan keys on that page are for interactive coding tools only, not for backend services.

### Which key should I use for always-on agents and automations?

Use a pay-as-you-go key, either an Alibaba Cloud Model Studio key or an OpenRouter key. Alibaba says Token Plan and Coding Plan keys are for interactive coding tools, and OpenClaw's docs warn that using Token Plan for cron jobs or unattended scripts can suspend the subscription or revoke the key.

### Is Qwen4-Max open source?

No. Qwen4-Max is a closed-weights model you reach through an API. Alibaba has said open weights for smaller Qwen4 models will come later.

## What I checked, and what I did not

On 16 November 2026 I read Alibaba's Qwen4-Max page, Alibaba's Claude Code page, its endpoint list, the OpenRouter model page, the docs for Qwen Code, Cline, Kilo Code, Hermes Agent, OpenClaw, n8n and Zapier, and the Artificial Analysis page. Every command and setting comes from those pages.

I did not run any of these setups, so nothing here is a tested result. Where I filled the Qwen4-Max ID into a pattern from the docs (the Qwen Code entry, the Hermes and OpenClaw lines, the Claude Code pay-as-you-go variant), the text says so. Tools change fast, so if a menu or command looks different on your machine, the linked docs are the source of truth. Benchmark numbers marked as Alibaba's are from Alibaba's own launch chart, not an independent test.

One key, one tool at a time, and Opus kept for the hard stuff. That is how the three-times saving shows up.
