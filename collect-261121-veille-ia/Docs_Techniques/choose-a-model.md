---
id: collect-261121-veille-ia/docs-techniques/choose-a-model
title: "Choose a model"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["muse", "model", "spark", "wonder", "ember", "muse-spark-1", "agent", "reasoning"]
source: https://dev.meta.ai/docs/choose-a-model?team_id=1074055538425878&project_id=1366748018914493
source_anchor: ""
source_lines: [1, 44]
section: "Docs_Techniques"
---
Muse Wonder, Muse Spark, and Muse Ember are reasoning models that take the same requests on the same endpoints, so moving a workload between them is a model ID change. Pick one by what each part of your app needs — or run them together in the same agent.

|  | Muse Wonder | Muse Spark | Muse Ember | 
|---|---|---|---|
| **Built for** | Complex software engineering and long-horizon agentic work | Agentic and coding work at scale | Efficient execution of high-volume, repeated work | 
| **Use it when** | Capability matters more than cost per token | The main loop or a harder task needs strong reasoning at a lower cost than Muse Wonder | A larger model plans, and sub-agents run many bounded tasks | 
| **Model IDs** | `muse-wonder-1.0` | `muse-spark-1.3` (recommended),`muse-spark-1.2` ,`muse-spark-1.1` | `muse-ember-1.0` | 
| **Input** | Text, image, video, audio, and PDF | Text, image, video, audio, and PDF | Text, image, video, audio, and PDF | 
| **Context window** | 1,048,576 tokens | 1,048,576 tokens | 1,048,576 tokens | 
| **Reasoning effort** | `"minimal"` to`"max"` ; default`"max"` | `"minimal"` to`"max"` on`muse-spark-1.3` ; default`"high"` | `"minimal"` to`"max"` ; default`"max"` | 
| **Contributor mode on Model API** | Not available | `muse-spark-1.3-contributor` ,`muse-spark-1.2-contributor` | Not available | 
| **Pricing** | Muse Wonder pricing | Muse Spark pricing | Muse Ember pricing, with long-context rates over 200K input tokens | 
| **Availability** | Muse Wonder availability | Muse Spark availability | Muse Ember availability | 

| Workload | Model | 
|---|---|
| Multi-file coding changes in a real repository | Muse Wonder | 
| Multi-hour agent runs and sub-agent orchestration | Muse Wonder | 
| The main loop of a coding agent or agent framework | Muse Wonder | 
| Computer use | Muse Wonder, or Muse Spark or Muse Ember at lower cost | 
| A lower-cost main loop for agents and assistants | Muse Spark | 
| Knowledge-heavy questions answered without retrieval | Muse Spark or Muse Wonder | 
| Sub-agent tasks handed out by a main agent, including coding subtasks | Muse Ember | 
| Tool calls, tight tool loops, and latency-sensitive turns | Muse Ember | 
| Classification, routing, and other high-volume requests | Muse Ember | 
| Running the same task over thousands of images, files, or records | Muse Ember | 
| Discounted prices on Model API in Contributor mode | Muse Spark, through a Contributor-mode variant | 
| Audio understanding | Muse Spark 1.2 | 
| Speech to text | Muse Voice Transcribe | 
| Image generation and editing | Muse Image | 
| Image and video segmentation | Segment Anything Model | 

Most agents don't need one model for everything. Put Muse Wonder on the main loop, where planning and multi-step reasoning decide the outcome, or Muse Spark when the main loop needs to cost less. Put Muse Ember on sub-agents and background tasks, which run many short, focused jobs where cost adds up: the larger model plans, and Muse Ember handles the volume.

The harnesses you already use support this split:

- **Claude Code and the Claude Agent SDK** : map the`opus` alias to`muse-wonder-1.0` , the`sonnet` alias to`muse-spark-1.3` , and the`haiku` alias and sub-agent model to`muse-ember-1.0` . See Set up Claude Code and Build with the Claude Agent SDK.

Moving a workload from one model to another is a model ID change. Keep these differences in mind:

- **Reasoning effort** :`muse-wonder-1.0` ,`muse-spark-1.3` , and`muse-ember-1.0` accept`"minimal"` to`"max"` .`"max"` isn't available on`muse-spark-1.2` ,`muse-spark-1.1` , or`muse-spark-1.3-contributor` . When a request sets no effort, Muse Wonder and Muse Ember use`"max"` and Muse Spark uses`"high"` ; see Reasoning.
- **Audio input** : response quality for audio on Muse Spark 1.3 may be degraded. Use Muse Spark 1.2 or Muse Voice Transcribe instead.
- **Long-context pricing** : Muse Wonder and Muse Spark charge the same rate at any context length. Muse Ember bills a request with more than 200K input tokens at long-context rates, so compare costs at your real input sizes; see Muse Ember pricing.
- **Harder work** : for hard single-shot coding problems, knowledge-heavy questions, or visual coding, use Muse Spark or Muse Wonder rather than Muse Ember.
