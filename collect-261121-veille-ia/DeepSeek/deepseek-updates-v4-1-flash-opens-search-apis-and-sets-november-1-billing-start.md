---
id: collect-261121-veille-ia/deepseek/deepseek-updates-v4-1-flash-opens-search-apis-and-sets-november-1-billing-start
title: "DeepSeek Updates V4.1 Flash, Opens Search APIs and Sets November 1 Billing Start"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "model", "search", "flash", "november", "project", "task", "work"]
source: https://asiadaily.org/news/15674/
source_anchor: ""
source_lines: [1, 114]
section: "DeepSeek"
---
## A coding upgrade arrives with a public search service

DeepSeek released V4.1-Flash-1017 on October 17, 2026, updating its website, app and API together while opening two services that let developers search the web and retrieve individual pages. The company lists each Search and Fetch endpoint at $0.75 per 1,000 calls, with charging for eligible calls through server tools starting at 16:00 UTC on October 31, or midnight on November 1 in Beijing.

The release comes 37 days after V4.1 Flash first appeared on September 10. Developers can continue using the model name deepseek-flash, with the existing model API calling method and prices unchanged. DeepSeek is also releasing the updated model openly through Hugging Face and ModelScope.

DeepSeek's official release announcement puts complete project development at the centre of the update. Its examples include a model of Beijing's Forbidden City, a browser racing game, a recreation of Portal 2 using the Godot engine, and a moving 3D scene tracing history from prehistoric times to the modern era.

DeepSeek says the model performs better while taking less time and producing shorter outputs. For developers, those changes address two expenses at once: the time an automated task occupies a computer and the amount of generated text billed by the model service.

The search launch adds another part of that workflow. An AI application can now use DeepSeek's model to plan a task, its search engine to find current information, and its Fetch service to bring a selected webpage into the task.

## What wish driven programming means in practice

DeepSeek describes the project's central ambition as programming from a user's wish: someone explains what they want in ordinary language, and the AI handles more of the work needed to turn that request into a functioning project.

The four demonstrations cover different kinds of work. The Forbidden City example centres on 3D modelling. The racing game requires browser interaction. The Portal 2 recreation uses an existing game engine, Godot. Valley of Ages presents changing environments across a long span of human history.

DeepSeek says these projects were produced inside DeepSeek Harness with relatively few prompts, limited reference material and few rounds of interaction. The company attributes the improvement to reinforcement learning with self feedback, a training method in which feedback on attempted work helps shape later behaviour.

A harness is the software around a language model that manages tools, files, sessions and task execution. The model can propose code or decide what to do next; the harness provides the environment in which those actions happen. That makes the surrounding software part of the project development process, alongside the model itself.

## Where the benchmark gains are strongest

DeepSeek's published evaluations put the updated model at 77.0 on DeepSWE v1.1, up from 74.2 for the September version. That is a gain of 2.8 points on an evaluation of software engineering work. In the company's comparison, Claude Opus 5 scored 74.0 and GPT 5.6-Sol scored 73.0.

Terminal-Bench 4.0 shows a larger improvement over the previous Flash release, rising from 31.2 to 42.3, an increase of 11.1 points. ProgramBench rose from 20.3 to 29.1, while NL2Repo-Bench increased from 65.4 to 68.2.

DeepSeek also lists improvements in tasks involving agents, systems that use tools to carry out a sequence of actions. Automation-Bench v1.0.6 rose from 54.8 to 60.5, Agents' Last Exam from 31.8 to 35.4, and Apex Bench v1.1 from 64.6 to 72.1.

The updated model leads DeepSeek's comparison table on DeepSWE, Automation-Bench, Agents' Last Exam and Apex Bench, ahead of the listed models from Anthropic, OpenAI, Kimi and Zhipu. Claude Opus 5 scores higher on several other evaluations, including Terminal-Bench, ProgramBench and NL2Repo.

Testing conditions matter to those numbers. DeepSeek's figures put Terminal-Bench 4.0 at 42.3, while an Artificial Analysis evaluation lists 26.8. The detailed settings needed to explain that gap are not given. DeepSeek identifies Claude Code as its environment for Terminal-Bench and Mini-SWE-V2 for DeepSWE, illustrating how the model and its surrounding tools are evaluated together.

## Why shorter answers can lower task costs

DeepSeek calls the update a Pareto improvement in performance and cost. In plain language, its claim is that users get better results while spending less, rather than exchanging higher quality for a larger bill.

Model services commonly charge by tokens, the small pieces into which text is divided for processing. A shorter generated response uses fewer output tokens. A coding agent may produce many responses during one job, so reducing unnecessary output across those steps can reduce the total charge.

DeepSeek says task execution times also fell. That matters for projects involving repeated editing, testing and tool use, where the user is waiting for a finished result rather than a single answer. The company describes the technical release as a small update, even while claiming improvements across reasoning, coding, agent tasks and visual evaluations.

## Search finds pages, while Fetch retrieves them

DeepSeek's Search and Fetch services have different jobs. Search takes a query and returns ranked results. Fetch takes a particular webpage and retrieves its title and body as Markdown, a simple text format that preserves headings, lists and other useful structure.

A developer could, for example, use Search to locate current software documentation and then Fetch to retrieve the relevant page before asking the model to write code. Applications that combine retrieved documents with generated answers are often called retrieval augmented generation, or RAG, systems.

The services use the base address api.deepseek.com and the same API credential system as DeepSeek's model service. The endpoints are POST /v1/tools/search and POST /v1/tools/fetch. They return structured JSON data, which software can read and pass between tools.

Search accepts queries of up to 128 characters. Developers can request between one and 50 results, with 10 returned by default. Results can include titles, addresses, short summaries, longer summaries intended for models, publication dates and, where available, full page content.

DeepSeek says its search engine already supports search on its website and API and handles hundreds of millions of requests daily worldwide. The company describes an index covering hundreds of billions of webpages and support for searches in multiple languages.

DeepSeek also says it filters search ranking manipulation, attempts to influence information selected by generative AI, and low quality content generated by AI. Those measures address the information an agent encounters before it writes an answer or acts on a user's request.

## What developers will pay, and when

DeepSeek's English announcement prices Search and Fetch separately at $0.75 per 1,000 calls each. Its Chinese pricing is 5 yuan per 1,000 calls for each service. A workflow that performs one search and retrieves one page therefore uses two separately billed services.

Under the published charging rules, Search incurs a fee when it successfully returns a nonempty result list. Fetch incurs a fee when it returns page content with an HTTP 200 success status. Failed requests, empty results and timeouts are free, and requesting more search results does not change the price of that search call.

At the stated yuan rate, 1,000 successful searches followed by 1,000 successful page retrievals would cost 10 yuan in search and retrieval charges. Model processing is charged separately. That separation helps developers estimate the cost of an agent that repeatedly looks up information while working.

DeepSeek says Search and Fetch calls made through server tools in DeepSeek Harness, Claude Code and Codex are temporarily free for DeepSeek API users. Billing for that route starts at 16:00 UTC on October 31, 2026, the same moment as November 1 at 00:00 in Beijing. The temporary exemption applies to those tool calls, while the model's normal charges continue.

Each account can make up to five Search requests per second and five Fetch requests per second, with the two limits counted separately. Fetch also has additional limits for individual website domains. Requests beyond the permitted rate receive an HTTP 429 response, telling the application to slow down.

## Harness updates connect the model to daily work

DeepSeek recommends Harness v0.2.1-rc.1 or later for the updated model. The letters rc identify a release candidate, a version prepared for testing before a final release. The model was specifically optimised for DeepSeek Harness, Tencent CodeBuddy, WorkBuddy and other frameworks.

The Harness release adds ways to discover plugins from custom indexes and to ask an agent to create a plugin. It also improves dependency handling when plugins are enabled, disabled, updated or removed. Some plugin changes can take effect immediately; others require a restart.

Developers gain interfaces for reading and changing a session's working directory through the TypeScript and Python software development kits. Other additions include experimental tools for inspecting sessions, support for separate Git working directories, and automatic masking of keys and tokens found in sent messages.

The release continues work on the Claude Code Mods bridge, tested in an earlier Harness alpha. That earlier work had three runnable examples and four examples blocked by missing interfaces or display capabilities. The compatibility layer remains experimental, with the stated aim of testing how Claude Code extension capabilities fit into Harness plugins.

First time Harness users are also offered 6 yuan in promotional credit without an initial payment. The credit expires seven days after arrival and is available only while the allocation lasts. Its short validity makes it a brief testing allowance for new users.

## Open weights and deployment choices

DeepSeek's official announcement confirms that V4.1-Flash-1017 is available through Hugging Face and ModelScope. The release continues the V4.1 Flash line's use of the MIT licence, which permits broad reuse subject to its terms. Developers can therefore choose between a hosted service and operating the model themselves.

V4.1 Flash accepts text and images and produces text. Its architecture uses a mixture of experts, meaning that selected parts of the model are activated for each processing step rather than the entire model working on every token. Architecture descriptions list a backbone of 552 billion parameters, with 8 billion active during input processing and 16 billion during output generation.

A context window of roughly one million tokens allows the model to work with large amounts of material in one session. For a coding agent, that can include instructions, source files, earlier conversation and tool results. The context window is the amount it can consider, while the output limit governs how much it can generate in one response.

Hosted deployment specifications can vary. A third party API listing gives a context window of 1,048,576 tokens and a maximum completion of 131,072 tokens, alongside function calling and structured outputs defined by a JSON schema. Function calling lets the model request an external tool; structured output gives an application a predictable format to process.

The same listing offers routing across inference.net, DeepInfra and DeepSeek, moving requests to another provider when one encounters an error. For developers, the choice now spans model access, provider routing and the separate search services, each with its own operational settings and costs.

## The dates that define the rollout

DeepSeek's October release combines a model update, a Harness release candidate and public access to search infrastructure. Earlier on October 17, an automated API tracker spotted a model entry with the 1017 suffix that subsequently disappeared. DeepSeek's later announcement confirmed the release and the change across its website, app and API.

The sequence gives developers a short period to update their tools and examine search usage before the temporary free window ends:

- September 10, 2026: DeepSeek releases the original V4.1 Flash model.
- October 3, 2026: Harness v0.2.1-alpha.1 introduces the experimental Claude Code Mods bridge.
- October 17, 2026: V4.1-Flash-1017 reaches the website, app and API; Search and Fetch open to developers, and an updated Harness release candidate becomes available.
- October 31, 2026, at 16:00 UTC: Charging begins for eligible Search and Fetch calls through server tools, equivalent to November 1 at midnight in Beijing.

Developers already using deepseek-flash receive the updated model through the existing name. Those using Harness are directed to update the surrounding application as well. The practical change is a more closely connected workflow: a model trained for longer projects, tools for carrying out the work, and a search service that will soon add a separate usage charge.

## Key Points

- DeepSeek released V4.1-Flash-1017 on October 17, 2026, across its website, app and API.
- The model remains accessible as deepseek-flash, with the existing model API calling method and prices unchanged.
- DeepSeek reports a DeepSWE v1.1 score of 77.0, up from 74.2, alongside faster tasks and shorter outputs.
- Project demonstrations include Forbidden City modelling, a browser racing game, a Portal 2 recreation and Valley of Ages.
- Search and Fetch each cost $0.75, or 5 yuan under the Chinese pricing, per 1,000 successful calls.
- Charging for eligible calls through server tools begins on November 1 at midnight in Beijing.
- DeepSeek recommends Harness v0.2.1-rc.1 or later and has released model weights through Hugging Face and ModelScope.
