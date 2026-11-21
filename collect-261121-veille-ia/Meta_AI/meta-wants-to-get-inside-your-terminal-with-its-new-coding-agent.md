---
id: collect-261121-veille-ia/meta-ai/meta-wants-to-get-inside-your-terminal-with-its-new-coding-agent
title: "Meta wants to get inside your terminal with its new coding agent"
domain: meta-ai
role: reference
task: article
actors: []
dates: ["2026-08-06"]
keywords: ["muse", "spark", "meta", "agent", "code", "coding", "model", "agents"]
source: https://www.theregister.com/ai-and-ml/2026/08/06/meta-wants-to-get-inside-your-terminal-with-its-new-coding-agent/5283717
source_anchor: ""
source_lines: [1, 53]
section: "Meta_AI"
---
AI AND ML

# Meta wants to get inside your terminal with its new coding agent

Muse Code showcases Muse Spark's fresh software engineering chops

To demonstrate the capabilities of its next-generation Muse Spark model, Meta has released a terminal coding agent called Muse Code that it thinks can help developers to tidy up their software projects.

Meta co-trained Muse Code on version 1.2 of its Muse Spark model, also released this week and now apparently boasting improved code generation smarts.

Developers can think of this beta release as the equivalent to OpenAI Codex or Anthropic’s Claude Code, two other LLM-based service offerings tweaked for the modern coder. Meta designed its new agent to be handy at planning changes to a codebase, writing the code and validating the results.

Currently, Meta has Muse Spark locked away as a proprietary, closed-weight model hosted in the cloud, an approach its rivals also embrace but which departs from the open-weight approach Meta previously implemented with its Llama models.

But Meta CEO Mark Zuckerberg did not rule out opening up Muse Spark in the future. “I'll have more to share on that soon,” he replied to a question posed on X about Muse Spark being open source.

### An agent orchestrator

Muse Code is best described as an agent orchestrator that runs on your command line.

As Zuck noted in a series of X messages, when a developer starts a task, Muse Code fires up background agents to maintain a context file that other sub-agents doing the work can consult should they lose their way. The tool logs every action before execution, so no work is lost. Multiple agents can work in parallel on the task using their own isolated work trees.

“Your working copy is never touched,” Zuck wrote.

In one test, the agent platform simultaneously built six features for a single game, with no collisions among the agents, Zuckerberg enthused.

“TBH it's a good harness,” boasted Hongyu Ren, a researcher for Meta’s Superintelligence Labs, on X.

### Remedial coding

Muse Code relies on Meta’s Muse Spark Large Language Model (not to be confused with the Apache Spark big data cruncher).

Muse Spark 1.2 is the third release in four months from Meta Superintelligence Labs, a unit that Meta stood up in June 2025 to reinvigorate the company’s AI efforts and pursue creation of a personalized AI “superintelligence” that focuses on deep reasoning and long-horizon planning.

The first model from this group, Muse Spark, is a multi-modal model able to digest and reason against text, images, video, audio, and even PDFs. It supports agents in long-running tasks.

With the first release of the model in April, Meta boffins admitted in the announcement that they needed to work more on Spark’s coding abilities. The new 1.2 release addressed that deficiency.

In another test, Muse Spark, running on Nvidia Hopper GPUs, tackled a kernel optimization task. Its agents made over 1,000 tool calls over a 24-hour period.

“It kept finding substantial improvements well beyond the initial exploration phase,” Zuckerberg wrote.

### Understand the mission

In his X missives, Zuckerberg included a somewhat vague chart comparing the performance of Muse Spark against other commercial models, using two industry benchmarks – Terminal-Bench 2.1 and DeepSWE 1.1 – that evaluate how autonomous agents act like software engineers, as well as a Meta Internal Coding bench too.

The benchmarks all showed Muse Spark to be close to the best, but never the very best, at understanding the assigned engineering task and executing it with as little mission creep as possible. Muse Spark performed honorably compared to Opus 5, GPT5.6 Terra, Grok 4.5 and Gemini 3.6. The scores are tightly clustered, so they all did well (except occasionally Gemini, the current laggard du jour).

So, Muse Spark is competitive anyway, though one eagle-eyed commenter wondered why OpenAI’s midline GPT5.6 Terra was used, instead of the top-tier GPT5.6 Sol.

Installing the agent within your command line is possible through a curl command. Poly-model enthusiasts can also tap into Muse Spark via OpenRouter, or through its API.

Muse Spark’s actual intelligence is metered at US$1.25 per million input tokens and US$4.25 per million output tokens. Discounts are available and the service offers a respectable 1 million token context window. ®
