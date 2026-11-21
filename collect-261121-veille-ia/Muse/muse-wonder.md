---
id: collect-261121-veille-ia/muse/muse-wonder
title: "Muse Wonder"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "wonder", "model", "mode", "code", "available", "meta", "full"]
source: https://dev.meta.ai/docs/muse-wonder?team_id=1741581793652561&project_id=29035399122726954
source_anchor: ""
source_lines: [1, 31]
section: "Muse"
---
Muse Wonder is Meta's most capable model, built for complex software engineering and long-horizon agentic work. It makes and verifies changes across many files in real repositories, and it plans, delegates to sub-agents, calls tools in parallel, and recovers from errors across many steps, following detailed instructions closely.

| **Model ID** | `muse-wonder-1.0` | 
| **Input** | Text, image, video, audio, and PDF | 
| **Output** | Text | 
| **Context window** | 1,048,576 tokens | 
| **Reasoning effort** | `"minimal"` to`"max"` ; the default is`"max"` | 
| **Endpoints** | Responses, Chat Completions, Messages | 
| **Pricing** | Pay-as-you-go, Standard mode only; see Muse Wonder pricing | 
| **Rate limits** | See rate limits | 

Use Muse Wonder when capability matters more than cost per token:

- **Multi-file coding changes** : make and verify changes across many files in a real repository.
- **Long-running agents** : plan, delegate to sub-agents, and work through multi-hour runs with parallel tool calls.

Muse Wonder also handles image understanding, video and audio understanding, tool calling, structured output, and search grounding. For high-volume, latency-sensitive, or cost-sensitive work, see Choose a model.

Muse Wonder supports every reasoning effort level from `"minimal"` to `"max"`. For agentic coding, start with `"high"`. Save `"xhigh"` and `"max"` for the hardest problems, such as tracing a concurrency bug or planning a large migration, where a correct answer matters more than speed.

On Model API, `"max"` on `muse-wonder-1.0` doesn't select Muse Wonder Full, which is available only in Muse Code.

Muse Wonder takes the same requests as Muse Spark on the same endpoints. Follow the quickstart and set the model to `muse-wonder-1.0`. Most examples in these docs that use `muse-spark-1.3` run on Muse Wonder with that change; Switch between models lists the settings that differ.

Muse Wonder comes in two variants. Muse Wonder is on Model API and in Muse Code. Muse Wonder Full, the higher-capability variant, is available only in Muse Code with a subscription, so setting reasoning effort to `"max"` on Muse Wonder doesn't select it.

With a subscription, Muse Wonder is available on every plan and Muse Wonder Full on the Power Usage and Maximum Usage plans, each in Standard mode and Contributor mode.

Muse Wonder and Muse Wonder Full aren't currently available in South Korea, on Model API or in Muse Code.

In Standard mode, your content — the prompts, code, files, and context you send, plus the model responses you get back — is not used to train Meta models. Muse Code subscribers can also use Contributor mode, which is on by default: it gives you more usage in exchange for letting Meta use your content to train future Meta models, and you can turn it off at any time in your settings. For Muse Wonder, Contributor mode isn't available on pay-as-you-go. See Pricing and rate limits for rates.
