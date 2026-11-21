---
id: collect-261121-veille-ia/ia-agents/how-to-build-a-recursive-self-improving-agent-harness
title: "How to Build a Recursive Self-Improving Agent Harness"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["tool", "harness", "model", "recursive", "agent", "list", "turn", "tools"]
source: https://www.mindstudio.ai/blog/recursive-self-improving-agent-harness
source_anchor: ""
source_lines: [1, 92]
section: "IA_Agents"
---
## What is a recursive self-improving agent harness?

A recursive self-improving agent harness is a control loop that lets an AI model write, test, and install its own tools while it works, instead of relying on a fixed toolset defined in advance by a developer. Rather than shipping 30 or 40 generic tools that sit in the context window on every turn, the harness starts with a minimal kernel (often just shell access, file read, and file write) and lets the model build whatever task-specific capability it discovers it needs, like a screenshot tool, a video-frame extractor, or a binary file parser.

## TL;DR

- A **standard agent harness** freezes its tool list and loop rules, forcing developers to guess every capability a model might need ahead of time.
- A **recursive harness** starts with a tiny base kernel (shell, read, write) and lets the model create, unit-test, and hot-swap new tools into its own tool list mid-conversation.
- The **Gemini Interaction API** makes this possible because conversation state lives on Google’s servers and each turn can carry its own updated system instructions and tool list without restarting the session.
- A **mandatory unit test gate** on every newly created tool prevents the model from trusting broken code it just wrote, which in one run caught a screenshot tool that failed because a browser binary wasn’t installed.
- **Lifecycle hooks** like session-start and before-done let the model attach its own checks to the harness loop, including rewriting its own definition of “done.”
- Benchmark runs showed a **frontier model with shell access will build in-line capabilities anyway** , even without a safe tool-creation path, which means recursive harnesses mainly pay off when the same type of task repeats.
- Recursive harnesses carry a real risk of **self-inflicted bloat** , where the model keeps stacking checks onto its own before-done hook until the harness becomes as slow as the frozen one it was meant to replace.

*regulatory compliance and security.*

- Name
- Maria Alvarez
- Date of birth
- SSN
- Diagnosis
- Balance due
- $412.80

## How is this different from a standard agent harness?

A standard harness treats the model as the variable and the tools as fixed. Developers connect MCP servers, enumerate every function the agent might plausibly need, and hope the right tool is available when a specific problem comes up. The failure mode is familiar to anyone who has built coding agents: the tool list grows bloated with options the task never touches, while something genuinely needed (inspecting a rendered web page, reading a bug report recorded as video, parsing a custom binary format) is simply missing.

A recursive harness inverts this. It treats the tool list itself as something the model can edit. The harness still runs the same basic loop (send conversation and tools, execute tool calls, feed results back, decide when the task is finished), but the tool list and the loop’s own rules are no longer frozen. The model can add a tool on turn three and call it as a real function on turn four of the same conversation.

## How does the architecture work?

The system splits into three zones.

**Zone one** is the interaction layer, in this case the Gemini Interaction API. It keeps conversation history on Google’s servers, so each turn only needs to send new tool outputs plus the previous interaction ID rather than the full history. More importantly, every request can carry its own system instructions, settings, and tool list, which is what allows a tool written mid-task to become callable on the very next turn without restarting the session. One practical wrinkle: the turn immediately after a tool list changes misses the context cache, so it’s worth patching tool updates incrementally rather than rewriting the whole list every turn.

**Zone two** is a sandboxed environment (a locked-down macOS sandbox in this build) where the agent actually operates. It starts with a minimal kernel of three tools: shell, read file, write file. No frameworks, no pre-installed capabilities, not even a way to view images. Alongside that kernel sits a toolbox with three recursive primitives:

- **Create tool** , which requires a name, description, parameter schema, Python code, and at least one unit test. The harness runs that test inside the sandbox before registering the tool, so broken code never becomes a trusted capability.
- **Lifecycle hooks** , specifically session-start (runs before the first turn of a task) and before-done (runs when the model claims it’s finished, and can block completion if its check fails).
- **Reflective memory** , a note file the model writes for its future self, plus a short reflection step between tasks where the model reviews what passed or failed and updates its toolbox accordingly.

Remy AI Engineer Certification

### Your seat is open. *Leave certified.*

Build and deploy an app for enterprises with AI in one live afternoon. No coding.

**Zone three** sits outside the sandbox and holds the guardrails. A hidden evaluation layer scores every task; the model never sees the actual checks, only which named checks passed or failed, which prevents it from optimizing against the grader instead of the task. A separate model broker holds API keys and enforces budget limits, so any tool the agent builds that needs to call another model (to describe a video, for instance) never has direct access to a secret.

## What happened when the agent discovered it was missing a tool?

In one run, the agent was building a simple web app and had no way to see what it had rendered, since the base kernel included no image viewing. It called create tool to write a Chrome screenshot utility. The first unit test failed because the browser binary it tried to launch wasn’t installed on the host machine. The harness rejected the tool and returned the error. On the next attempt, the model located the correct Chrome binary, fixed the tool, passed the test, and used it immediately to inspect its own page. Because custom tools can return images directly as model input, that one tool gave a text-only harness the ability to see its own output. Without being prompted, the model later turned that same screenshot check into a before-done hook, so no future task could be marked complete unless a page actually rendered correctly in a browser.

## Does the recursive harness actually outperform a frozen one?

Not automatically, and this is the more interesting finding. Across a series of web app build tasks of increasing difficulty, an evolving harness and a frozen three-tool harness both passed every hidden check on every app, and finished in roughly the same amount of time. The evolving harness built a screenshot tool, a pixel comparison tool, and automated hooks along the way, but the frozen harness kept pace by writing throwaway scripts in line for each task.

The gap showed up when the test was made harder by removing image tools and the model broker entirely. The frozen agent, with no vision capability at all, used macOS’s built-in text recognition to read a reference screenshot and wrote plain Python to compare colors and edges, rebuilding a pricing page pixel by pixel. Given a silent bug report video with no visible changes, it wrote a computer vision script from scratch to match frames against reference photos and isolate the bug. The lesson: a frontier model with shell access will build whatever capability it needs in the moment, whether or not the harness offers a sanctioned way to do it.

## When does persistence actually pay off?

The clearest win for the recursive approach showed up on repeated structural tasks. Running five tasks against a custom binary file format, each requiring a nontrivial parser, the evolving harness spent extra time on the first task turning its parser into a reusable library. On the next four tasks, it reused that parser immediately and finished notably faster than the frozen harness, which rewrote the parser from scratch every time. Persistence pays off when tasks share real structure, not just surface similarity.

## What goes wrong with recursive harnesses?

Build an onboarding app for 4,000 new hires.

Build ↵
The same mechanism that makes the system useful also makes it prone to undermining itself. In one run starting from an empty toolbox on the same binary-format tasks, the agent built an impressive set of tools, but kept adding more checks to its own before-done hook after every reflection step. By the fifth task, the growing checklist consumed nearly all the time the agent had saved by reusing its parser, effectively recreating the bloat the whole approach was meant to avoid. Two related failure modes showed up elsewhere: a stricter pixel-diff tool that kept re-verifying a page that had already passed until the task ran out of time, and a before-done check that started copying the exact wording of the hidden evaluation criteria straight from the task description, edging toward gaming the grader rather than solving the task.

## Frequently Asked Questions

### What is the Gemini Interaction API’s role in this setup?

It keeps conversation state server-side and allows each turn to carry an updated tool list and system instructions, which is what lets a tool written mid-conversation become callable on the next turn without restarting the session.

### Why does the harness require unit tests before registering a new tool?

Without a test gate, a subtly broken tool would get registered and trusted anyway, and the model would act on its incorrect output for the rest of the task. The gate caught at least one real bug (a screenshot tool failing due to a missing browser binary) before it could cause downstream errors.

### Does a recursive harness make the underlying model smarter?

No. The experiments suggest a capable model will build needed capabilities in line regardless of whether the harness offers a tool-creation path. The recursive harness mainly helps with speed and reuse on tasks that share structure, not with raw problem-solving ability.

### What’s the biggest risk in letting an agent rewrite its own harness?

Self-inflicted bloat. Agents that keep adding checks to their own completion hooks after each reflection step can erode the very efficiency gains the recursive approach is meant to provide, sometimes to the point of matching or exceeding the overhead of a frozen harness.

### How does the system prevent the agent from gaming its own evaluation?

The evaluation checks run outside the sandbox and are hidden from the model, which only sees which named checks passed or failed rather than their contents. This is meant to stop the model from optimizing directly against the grading criteria instead of the actual task.
