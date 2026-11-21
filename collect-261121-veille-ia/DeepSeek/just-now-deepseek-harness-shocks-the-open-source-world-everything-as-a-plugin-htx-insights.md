---
id: collect-261121-veille-ia/deepseek/just-now-deepseek-harness-shocks-the-open-source-world-everything-as-a-plugin-htx-insights
title: "Just Now, DeepSeek Harness Shocks the Open Source World: Everything as a Plugin | HTX Insights"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "harness", "also", "open", "source", "just", "model", "models"]
source: https://www.htx.com/news/Project%20Updates-qGtgQrHc/
source_anchor: ""
source_lines: [1, 187]
section: "DeepSeek"
---
Today in the early morning, DeepSeek V4 Pro official version was released, triggering a sensation. Now, half a day later, **DeepSeek Harness** (Developer Preview) is also here!

Of course, this is not surprising, as this project has been hyped for a long time. For example, Cui Tianyi from the DeepSeek Harness team has been posting teasers on social networks and recruiting talent for the team.

We also gained early access to DeepSeek Harness in early August, getting a sneak peek at this agent framework that is destined to bring new changes to the AI community.

Open Source Address: https://github.com/deepseek-ai/deepseek-harness

For instance, here we let DeepSeek Harness, configured with the official DeepSeek-V4-Flash, build a first-person zombie shooting game for us. Without any intervention midway, we got a playable, albeit imperfect, product in just over 30 minutes.

Considering the recent popularity of Andrej Karpathy's approach to generating 3D worlds with AI, we also had DeepSeek Harness(V4-Flash) take on the "Huaqiang Buys Watermelons" benchmark: recreating the classic "Huaqiang Buys Watermelons" clip into a 3D animation based on text description (again, a one-shot prompt result):

Overall, although far from perfect, this animation largely restores the story plot, and the character relationships are mostly discernible. In contrast, using the same prompt with Codex configured with GPT-5.6 sol-xhigh produced a much inferior animation:

Keep in mind, DeepSeek-V4-Flash's parameter scale is far smaller than GPT-5.6 sol. It's conceivable that DeepSeek Harness must have played a major role.

Today, with the official release of DeepSeek V4 Pro, we also connected this model to DeepSeek Harness and ran it again:

The effect is indeed somewhat better.

Next, looking at the project structure, it's astonishing: **the repository already contains over 230 workspace members**, with code distributed across areas like packages/, apps/, examples/, python/, native/, vendor/, website/, etc. File system, terminal, subprocess, PTY, language server, web access, skills, sub-agents, workflows, planning mode, session persistence, settings, credentials, telemetry—almost every capability has its own package.

If we compare ordinary Agent projects to a pre-assembled computer, then DeepSeek Harness is more like a massive solderless breadboard: models, tools, interfaces, storage, security policies, and context management can all be plugged in or pulled out.

It provides a default assembly scheme, but it's clear that what DeepSeek truly wants to create is not a fixed-form "DeepSeek programming assistant", but **a way of assembling agents**.

## **What is DeepSeek Harness?**

Let's clarify a common confusion first: DeepSeek Harness is not a new DeepSeek model, nor is it merely an API client. It is **an SDK and application framework for building, running, and extending agents**. It can connect to DeepSeek models by default (and can easily be customized to connect to other models), allowing models to read projects, modify files, run commands, manage tasks, delegate subtasks, and interact with users via Web UI, full-screen terminal, headless commands, or automation protocols.

DeepSeek Harness web client provides a convenient entry to configure other model services, without requiring users to manually edit configuration files.

The AI community is no longer unfamiliar with the term "Harness". Its original meaning includes harness, wire harness, restraining device, etc. Abstracting it further, its function is to connect power to a workable mechanism while preventing that power from running wild. Specifically for AI, a Harness is responsible for connecting the model to the file system, shell, code editor, web pages, and other agents, while recording what it has done, limiting what it can do, and deciding whether to retry, cancel, compress context, or hand the problem back to the user when something goes wrong.

This might also explain why the project's codebase and package count are so large. After all, it involves a wide range of tasks and tool selections, including whether tool calls can be parallelized, whether cancel commands can truly stop child processes, whether tool results pollute the context, where new messages from the user during model execution should be inserted, how to reconstruct the model input when a session is restored, what tools sub-agents have, whether file writes go beyond the workspace, and whether the content seen during interface replay matches real-time execution.

DeepSeek Harness attempts to formalize all these issues as system capabilities.

## **Everything as a Plugin**

The most striking design philosophy of DeepSeek Harness is "**Everything as a Plugin**", even the Agent Loop itself is considered a plugin.

The project is built on the Cordis micro-kernel. A running Harness is essentially a Cordis Context. Different packages register services, events, and capabilities with the Context, ultimately combined by the configuration file into a runnable agent system.

packages/core/ is the core of the entire system, containing Session, System Prompt, Tools, Agent, and Agent Loop. They address the most fundamental questions: what is a session, how are system prompts assembled, how are tools registered and invoked, how are Agents created, and how does one round of conversation proceed from user input to model request, tool execution, and final response.

Beyond the core are numerous capability packages:

- packages/llm/ is responsible for model adapters and streaming output;
- packages/shell/, packages/subprocess/, and packages/terminal/ are responsible for one-shot commands, process trees, and persistent terminals;
- packages/fs/ is responsible for file read/write, editing, search, and policy restrictions;
- packages/lsp/ connects to language servers, allowing the Agent to gain semantic-level code navigation beyond just text search;
- packages/web/ is responsible for search and web scraping;
- packages/skill/ manages reusable skills;
- packages/subagent/ and packages/workflow/ extend a single Agent into a multi-agent system capable of delegation and orchestration.

Looking further out, planning, goals, todos, background tasks, context compression, session querying, session titles, credentials, user settings, approval mechanisms, and telemetry are also split into independent capabilities. The most interesting aspect of this structure is that it embodies a near-obsessive sense of boundaries: who owns the interface, who is responsible for the implementation, who presents the capability to the model—these should not be mixed together.

The project documentation breaks down typical capabilities into three layers: Interface, Implementation, and Consumer.

Take Bash as an example. The interface defines what "executing a command" is, the local implementation is responsible for actually creating the process, and the model-facing tool package is responsible for turning this capability into a schema and results understandable by the model. In the future, if the local shell needs to be replaced with a remote container, cloud sandbox, or enterprise execution platform, theoretically only the implementation layer needs to be replaced, without rewriting the model tools and Agent Loop.

This is typical framework thinking. It may make the repository appear large in its early stages, but it also shows that DeepSeek Harness's goal is not to create a finished product only maintainable by the official team, but to allow different deployers to swap models, storage, security policies, add tools, or even replace the agent loop.

Here, we also see DeepSeek's consistent commitment to **true open source**!

## **cordis.yml**

## **Assembling Different Agents with One Configuration**

The plugin architecture is ultimately realized for developers through cordis.yml. The configuration file lists plugin names, stable IDs, and parameters, determining exactly which set of capabilities the current Agent possesses.

The same codebase can be assembled into completely different product forms. Add DeepSeek LLM adapter, file system, Bash, and TUI to get a terminal programming agent; switch the interaction interface to the Web plugin to get a browser application; use the Headless entry, and it will accept a task, complete model and tool rounds, print the answer, and exit; replace it with an ACP or JSON-RPC frontend, and it becomes an automated service drivable by other programs.

Configuration also supports overlay layers. TUI and Web UI can share a base configuration, overlaid with their respective interface plugins and parameters; personal configuration sits in the final layer. This way, deployers don't need to duplicate the entire configuration tree; they only need to replace specific plugins. However, there's a detail to note: configuration patches replace the entire config of the target plugin, not a deep merge. If you only write a new field, the original API Key, base URL, or other parameters might disappear altogether. It's explicit, but may not align with a first-time user's intuition.

The project also allows reading environment variables and runtime expressions via !!js in YAML, for example, getting the key from DEEPSEEK_API_KEY. The configuration only references credential names, with keys resolved during actual invocation. The Web UI writes keys to $DSH_HOME/.credentials.yaml, while environment variables and .env can serve as fallback sources for automation or local development; keys should not be directly written into cordis.yml or enter session logs.

## **Agent Loop**

## **Not a Loop, But a Set of Traffic Rules**

The core code of many early Agent projects could be simplified to a few lines: send messages to the model, if the model returns a tool call, execute the tool, then send the result back to the model, until the model outputs text. DeepSeek Harness certainly does this too, but it breaks this process into a strict lifecycle.

A user input initiates a Turn, a Turn can contain multiple Steps; a Step corresponds to one model request and its subsequent tool execution(s). Before the request, the system assembles the stable system prompt, current runtime environment, tool schema, and session messages; after the request, the model's streaming chunks, complete messages, tool calls, tool results, and finish reasons all enter the event stream.

The aforementioned zombie shooting game executed 3 turns, 127 steps.

Tools are not simply "call upon getting the function name". They go through pre-call policies, irreversible security guards, actual execution, post-processing, content organization, and result notification. Allow/deny, timeouts, retries, metric statistics, and appending context can all be hooked into different points of the pipeline. Tools can declare that calls under certain parameter types are concurrency-safe, and the scheduler will allow consecutive read-only tasks to run in parallel; once a call that modifies state or whose safety cannot be determined is encountered, it is treated as a barrier, waiting for previous tasks to finish before executing exclusively.

This design might seem like installing an air traffic control system on a country road. But when an Agent starts searching ten files simultaneously, running tests, accepting user follow-up instructions, and also needs to allow cancellation at any time, these rules quickly transform from "over-engineering" into "the thing you wish you had earlier in an accident investigation report".

It also seriously handles the destination of mid-execution messages. New content sent by a user while the Agent is working could be the next task or steering instructions for the current work. The system distinguishes between queued messages, context injection, and Steering, and uses receipts to confirm whether a particular steering instruction actually entered a specific model request. In other words, it cares not just about "message received" but also "at which step did the model actually see it".

## **Session Log**

## **The True Source of Authority for the Entire System**

Another noteworthy design in DeepSeek Harness is the Session Log.

The project stipulates that anything the model sees must be reconstructible from the log. User messages, runtime environment context, model request info, streaming output, tool calls and results, compression events, permission switches, cancellation reasons—all enter the append-only session stream as events. The interface, persistence, recovery, forking, telemetry, and replay should not each maintain an "almost correct" state; they should all derive from the same event source.

This principle solves a very thorny problem in Agent systems: when a task fails, can we actually know what the model saw at that time?

If the system only saves the final chat text, many crucial factors are lost. Perhaps the workspace state was just injected before the model request, perhaps tool results were truncated, perhaps the system automatically switched model routing, perhaps the user changed direction mid-stream. DeepSeek Harness saves records sufficient to reconstruct messages at request boundaries, and original streaming chunks are also preserved, ensuring consistency between the interface and replay.

Session persistence itself is still a plugin. The project provides backends like JSONL and SQLite. Querying capabilities can prioritize accessing live sessions or search historical records via SQLite full-text search. Resume continues work using the original session, while Fork derives a new session from a definite historical boundary. For developers, this provides a unified foundation for debugging, evaluation, auditing, and automation.

## **From One Agent to a Group of Agents**

DeepSeek Harness already includes various sub-agent and workflow capabilities.

The main Agent can delegate tasks to sub-agents. Sub-agents can be newly created instances, forks from a completion boundary of an existing session, or external child processes connected via ACP.

The aforementioned zombie shooting game created 5 parallel sub-agents.

The scoping design here is important. Each Agent has its own context layer, seeing specific tools, prompts, and commands. One sub-agent might be restricted to only search and analysis, while another is allowed to modify files. Capabilities registered in an Agent's scope are automatically cleaned up with the Agent's lifecycle, without relying on global naming conventions to maintain isolation.

Workflows go a step further: they allow scripting to drive multi-agent orchestration, connecting multiple subtasks, structured outputs, and continuations. The project simultaneously provides Goals, Plans, Todos, and Background Tasks. These aren't just four UI widgets with similar names; they represent different lifecycle collaboration states. Planning Mode records the current collaboration phase, Goals can persist across the same session, Todos provide a lightweight task list for the model, and Background Tasks manage actual work that's still running.

This shows that Harness aims to cover more than just "Q&A style programming". It hopes to support long tasks, parallel investigations, automated runs, and external system coordination. Whether models can stably handle so many mechanisms is another test; at least the framework has built the steering wheel, dashboard, and brakes.

## **Web, TUI, Headless, and SDK**

For regular users, the project recommends the Web UI, which defaults to listening on http://127.0.0.1:3080. It provides conversation, session sidebar, permission selection, planning mode, tool cards, and workspace interaction.

The Web UI also provides four Agent preset modes. They are not four completely independent agents, nor do they merely change prompt styles. Instead, they are based on the same Harness host, equipping the current session with different tools, prompts, and runtime capabilities:

**Standard Mode**: The most fully-featured general-purpose coding Agent, providing file editing, Shell, file & web search, Skills, Planning, Goals, Sub-agents, and Workflows, suitable for most daily development tasks.

**PTC Mode**: Retains all capabilities of Standard Mode, while presenting tools to the model via the Code Mode SDK. The model can write a TypeScript program, combining multiple operations in a single `run_code` call, reducing the overhead of back-and-forth between model and tools, making it more suitable for complex tasks with longer call chains.

**Minimalist Mode**: Provides only persistent Bash and `str_replace_editor` tools. The smaller toolset reduces selection and context burden, suitable for coding tasks with clear paths where you want the Agent to act directly.

**Creator Mode**: Builds upon Standard Mode by adding Cordis runtime inspection, temporary plugin experimentation, and Agent preset creation guidance. The Agent can not only use existing tools but also explore and reconfigure its own runtime, thereby creating new custom presets. Since it can run plugin code written by the model, this is a high-trust mode for advanced users.

This set of presets is perhaps the most intuitive productized expression of "Everything as a Plugin": the underlying model routing, session persistence, sandbox, and approval are still provided by the shared host; the preset only determines which specific capabilities are loaded into an Agent Context. Thus, the same Web UI can switch from a two-tool minimalist Agent to a Standard Mode capable of orchestrating sub-agents, or even further into a Creator Mode that can modify itself.

For example, here in Creator Mode, we had DeepSeek Harness, connected to DeepSeek V4 Pro, create a "Three-Column Mode" not present in the official Web UI:

Beyond the Web UI, the TUI targets developers who prefer staying in the terminal.

Headless Mode is suitable for scripts and CI: it accepts a task, waits for the Agent to fully stabilize, outputs the last valid reply, and exits. If a program needs structured events and continuous control, it should use ACP or JSON-RPC/Python SDK.

For automation, the project provides ACP service and JSON-RPC entry. The Python SDK drives the included JSON-RPC runtime, allowing Python applications to start sessions, send tasks, receive notifications without directly embedding the Node kernel. The repository also includes examples like Code Mode, self-referential Cordis, and MCP memory service.

Notably, these entry points are not four independently evolving Agents. They share the core capability model, session event semantics, and most foundational plugins. They are not simply replacing a UI layer but rather assembling product forms like Web, one-shot tasks, and automated services through different bundles. This is the most tangible outcome of "Everything as a Plugin".

## **Agents Can Inspect and Even Modify Themselves**

DeepSeek Harness also provides a set of self-referential Cordis tools. They don't enter Standard, PTC, or Minimalist modes but are offered through the "Creator Mode" in the Web UI as an explicit advanced entry. After selecting this preset, the Agent can inspect the current runtime's plugin tree and dynamically mount or unmount temporary plugins.

This sounds a bit like letting a car change its own engine on the highway, so the project doesn't enable it by default. It's suitable for research or advanced automation scenarios: the model can temporarily write an event listener, register a new tool, provide a service, and uninstall it after the task is complete.

Self-modifying Agents can easily become mere concept demos, but Harness at least places it within the existing plugin lifecycle. Dynamic plugins still run under Cordis's Context and Effect mechanisms, with clear cleanup paths for registered items.

It's far from safe and sound, but it shows where this architecture truly aims to go: agents don't just use capabilities; they can also reconfigure their own runtime within controlled boundaries.

The design behind Cordis can be referenced in the paper officially released concurrently: **"A Programming Paradigm for Spatiotemporal Composability"**:

Paper Address: https://github.com/cordiverse/paper

## **Security Policy**

Once a programming agent gains file system and Shell permissions, it can modify code, install dependencies, start processes, and even touch the host environment outside the workspace. DeepSeek Harness clearly treats this as a fundamental infrastructure issue, not something to be hastily resolved by adding a confirmation popup to the UI.

The project defaults to `workspace-write` mode, restricting command execution and file modifications to the current workspace and permitted temporary directories, combined with an `ask` approval policy for operations requiring expanded permissions. A more permissive `danger-full-access` mode exists but must be explicitly chosen by the deployer; it won't be packaged as a seemingly harmless compatibility option.

Tool calls also go through pre-call policies, monotonic safety guards, execution wrappers, and post-processing. Operations rejected by a guard cannot be re-allowed by subsequent plugins; commands requiring expanded permissions must explain the reason and retry via the approval mechanism. The file system, Bash, and subprocess share the same sandboxing strategy, avoiding a fractured boundary where "commands are restricted, but file tools can bypass it".

More commendably, DeepSeek Harness adopts a "fail-closed" principle. If the system cannot confirm that the isolation mechanism is truly effective, it refuses execution rather than silently degrading to unprotected operation. Permission switches, approval requests, tool parameters, execution results, and cancellation reasons also enter the Session Log, preserving evidence for post-audit and issue reproduction.

This design doesn't eliminate all risks of agents performing local operations, but it reflects a rare engineering attitude: **security is a system constraint spanning configuration, execution, approval, logging, and recovery mechanisms**. The model can propose actions, but it is ultimately Harness that decides whether the action can occur.

## **DeepSeek Aims to Be More Than "Just Another Codex"**

Looking only at the Web UI or TUI, it's easy to misunderstand DeepSeek Harness as the DeepSeek version of Codex, Claude Code, or other programming assistants. But judging from the repository structure, its goal is clearly more foundational.

The default application is certainly important; it gives developers a tool that can read/write projects, run commands, plan tasks, and invoke sub-agents directly. But what truly occupies the center of the project is the replaceable capability interfaces, event-driven lifecycle, authoritative session log, and declarative composition. In other words, the finished Agent product is more like the first customer of this SDK.

This also fills a previously less visible piece in DeepSeek's model ecosystem. Models determine the upper limit of intelligence; Harness determines how that intelligence enters the real environment, how it uses tools, how it retains state, and how it works within permission boundaries. For enterprise developers, the latter often matters more than having a few extra buttons in the chat window because it determines whether the system can be audited, extended, replaced, and maintained long-term.

DeepSeek Harness is far from the "install and everything is smooth" stage yet, but it already demonstrates a quite complete technical judgment: an Agent should not be an increasingly bloated loop but a set of combinable, observable, and replaceable capabilities; a session should not be just a chat history but a record of operational facts; a tool should not be just a function but should simultaneously possess policies, logs, and presentation protocols.

Therefore, the most noteworthy aspect of DeepSeek Harness is not whether it can replace the programming assistant you're using today, but rather the extent to which it has made DeepSeek's answer to Agent engineering public.

This article is from the WeChat public account "机器之心" (ID: almosthuman2014), author: Machine Heart following DSH.
