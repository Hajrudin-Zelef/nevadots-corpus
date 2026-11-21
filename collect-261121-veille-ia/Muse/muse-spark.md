---
id: collect-261121-veille-ia/muse/muse-spark
title: "Muse Spark"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "muse-spark-1", "contributor", "mode", "pay-as-you-go", "model", "standard"]
source: https://dev.meta.ai/docs/muse-spark?team_id=2431753403985778&project_id=3419047751588389
source_anchor: ""
source_lines: [1, 40]
section: "Muse"
---
Muse Spark is Meta's model for agentic and coding work at scale, such as multi-step tool loops, software engineering assistants, and long-context reasoning. It costs less than Muse Wonder, and on Model API it also offers a discounted Contributor mode.

| **Standard mode model IDs** | `muse-spark-1.3` ,`muse-spark-1.2` ,`muse-spark-1.1` | 
| **Contributor-mode variants** | `muse-spark-1.3-contributor` ,`muse-spark-1.2-contributor` | 
| **Input** | Text, image, video, audio, and PDF | 
| **Output** | Text | 
| **Context window** | 1,048,576 tokens | 
| **Reasoning effort** | `"minimal"` to`"max"` on`muse-spark-1.3` ; see Versions. The default is`"high"` | 
| **Endpoints** | Responses, Chat Completions, Messages | 
| **Pricing** | Pay-as-you-go; see Muse Spark pricing | 
| **Rate limits** | See rate limits | 

Use Muse Spark for agentic and coding work that needs strong reasoning at a lower cost than Muse Wonder:

- **Multi-step tool loops** : plan and drive tool calls across many turns.
- **Knowledge-heavy questions** : answer questions that depend on broad knowledge, without a retrieval step.

Muse Spark also handles image understanding, video and audio understanding, tool calling, structured output, and search grounding. For complex, long-running tasks where capability matters more than cost, see Choose a model.

Muse Spark comes in three versions. All take text, image, video, audio, and PDF input, return text, and have a 1,048,576-token context window. They differ by capability:

- **Muse Spark 1.3** (`muse-spark-1.3` ): the latest version, tuned for agentic workflows (multi-step tool, browser, and long-horizon tasks) with improved coding over 1.2. Supports all reasoning effort levels, including the`"max"` level for extended reasoning (not available with`muse-spark-1.3-contributor` ).**Recommended for new work.**
- **Muse Spark 1.2** (`muse-spark-1.2` ): the previous version.
- **Muse Spark 1.1** (`muse-spark-1.1` ): the original version.

`muse-spark-1.3` is the default model in most code examples in these docs. The coding agents, agent frameworks, and computer use guides use `muse-wonder-1.0` for long-running agentic work.

Follow the quickstart to set your key and send your first request to `muse-spark-1.3`.

Every Muse Spark version is available on pay-as-you-go. All versions except Muse Spark 1.1 are also available in Muse Code, on pay-as-you-go and with any subscription.

| Version | Model API (pay-as-you-go) | Muse Code | 
|---|---|---|
| Muse Spark 1.3 ( `muse-spark-1.3` ) | Standard mode | Pay-as-you-go and every plan | 
| Muse Spark 1.3 Contributor ( `muse-spark-1.3-contributor` ) | Contributor mode | Pay-as-you-go and every plan | 
| Muse Spark 1.2 ( `muse-spark-1.2` ) | Standard mode | Pay-as-you-go and every plan | 
| Muse Spark 1.2 Contributor ( `muse-spark-1.2-contributor` ) | Contributor mode | Pay-as-you-go and every plan | 
| Muse Spark 1.1 ( `muse-spark-1.1` ) | Standard mode | Not available | 

In Standard mode, your content — the prompts, code, files, and context you send, plus the model responses you get back — is not used to train Meta models. In Contributor mode, your content may be used to train future Meta models, in exchange for a lower price. On pay-as-you-go, switch between the two by choosing the model ID: for example, `muse-spark-1.3` for Standard mode or `muse-spark-1.3-contributor` for Contributor mode. Muse Spark 1.1 is available in Standard mode only. See Pricing and rate limits for rates.
