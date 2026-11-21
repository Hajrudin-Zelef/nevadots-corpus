---
id: collect-261121-veille-ia/grok-xai/xai-launches-grok-4-7-its-most-powerful-model-yet-for-coding-agents-and-knowledge-work
title: "xAI launches Grok 4.7, its most powerful model yet for coding, agents, and knowledge work"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "coding", "work", "knowledge", "agents", "context", "launches"]
source: https://www.datastudios.org/post/xai-launches-grok-4-7-its-most-powerful-model-yet-for-coding-agents-and-knowledge-work
source_anchor: ""
source_lines: [1, 271]
section: "Grok_xAI"
---
xAI launches Grok 4.7, its most powerful model yet for coding, agents, and knowledge work

Sep 21

6 min read

Updated: Sep 23

xAI has released Grok 4.7, its new frontier model for coding, agentic workloads, and professional knowledge work, combining a larger base model with longer reinforcement-learning training and stronger performance on tasks that require sustained execution.

Compared with Grok 4.6, xAI says the new model is better at checking its own work, managing longer context, and remaining effective on tasks that can take hours to complete. The company also trained Grok 4.7 to work natively with the Grok Bot harness, extending the model's focus beyond individual prompts toward persistent agent workflows.

Grok 4.7 has a 500,000-token context window, accepts text and image input, supports four reasoning levels, and can use function calling, web search, X search, and code execution. It is available through the xAI API as grok-4.7, as well as through Grok Build, Cursor, and supported model gateways.

........

GROK 4.7

SPECIFICATION

Context window

500,000 tokens

Knowledge cutoff

May 2026

Input

Text and images

Output

Text

Reasoning

Low, medium, high, xhigh

Default reasoning

High

Standard input price

$2 / 1M tokens

Standard cached input

$0.50 / 1M tokens

Standard output price

$6 / 1M tokens

APIs

Responses API, Chat Completions

Tools

Function calling, web search, X search, code execution

Main workloads

Coding, agents, professional knowledge work

........

··········


A LARGER MODEL TRAINED FOR LONGER TASKS

Grok 4.7 uses a new and larger base model than Grok 4.6.

xAI also extended the reinforcement-learning stage and changed its task distribution toward harder problems, particularly workloads that require many hours rather than a short chain of responses. The company reports improvements in self-verification and long-context management as a result.

These changes address a different problem from context capacity alone. A 500,000-token window determines how much information can remain available to the model, but an agent operating over a large repository or a long professional workflow must also decide which information remains relevant, preserve its objective across successive actions, detect incorrect intermediate results, and avoid allowing an early error to propagate through the rest of the task.

Grok 4.7 therefore combines larger context capacity with training specifically aimed at remaining effective while a task continues. xAI has also trained the model to understand the Grok Bot harness natively, connecting the underlying model more closely to its own persistent-agent infrastructure.

··········


CODING AND AGENT PERFORMANCE

The launch results show improvements across several evaluations associated with coding and autonomous execution.

On CursorBench 4.0, which xAI describes as stressing longer-running coding tasks, Grok 4.7 reaches 46.3%, compared with 40.4% for Grok 4.6. On DeepSWE v1.1, the published scores rise from 65.2% to 71.0%. The largest absolute difference among the three main coding and terminal evaluations appears on Terminal-Bench 4.0, where Grok 4.7 reaches 38.0% versus 20.3% for Grok 4.6.

........

BENCHMARK

GROK 4.6

GROK 4.7

DIFFERENCE

CursorBench 4.0

40.4%

46.3%

+5.9 pp

DeepSWE v1.1

65.2%

71.0%

+5.8 pp

Terminal-Bench 4.0

20.3%

38.0%

+17.7 pp

........

Data Studios calculation: the final column shows the arithmetic percentage-point difference between the results reported by xAI.

The comparison should not be interpreted as a perfectly controlled model-to-model experiment. xAI uses different reasoning-effort configurations in parts of the published evaluation, including xhigh for Grok 4.7 and high for Grok 4.6, while the Grok 4.7 DeepSWE result is reported at high reasoning effort. For that reason, the table reports the observed percentage-point differences without converting them into claims about the intrinsic percentage improvement of the underlying model.

··········

[[ADPLUS_300x250_1]]

GROK 4.7 ALSO TARGETS PROFESSIONAL KNOWLEDGE WORK

xAI is positioning Grok 4.7 for professional work beyond software engineering.

The company's launch evaluations include multi-hour office tasks and workloads associated with law, healthcare, finance, and engineering. xAI specifically cites improvements in document and presentation creation and reports higher Grok 4.7 results on both GDPval and AA Briefcase.

On AA Briefcase v1.1, the published score increases from 1,546 for Grok 4.6 to 1,657 for Grok 4.7. The Harvey Legal Agent Benchmark rises from 15.8% to 19.6%, HealthBench Professional from 48.5% to 56.7%, and EEBench from 53.0% to 64.0%.

........

PROFESSIONAL EVALUATION

GROK 4.6

GROK 4.7

DIFFERENCE

AA Briefcase v1.1

1,546

1,657

+111

Harvey Legal Agent Benchmark

15.8%

19.6%

+3.8 pp

HealthBench Professional

48.5%

56.7%

+8.2 pp

EEBench

53.0%

64.0%

+11.0 pp

........

The distribution of the reported gains is consistent with xAI's training strategy: the model is being optimized for extended work across several professional domains rather than for coding alone.

These remain vendor-reported benchmark results. Production performance will depend on the task, reasoning configuration, available tools, surrounding agent architecture, and the amount of human review required.

··········

THE 500,000-TOKEN CONTEXT IS PART OF A LARGER AGENT ARCHITECTURE

The 500,000-token context window gives Grok 4.7 enough capacity for large codebases, long document collections, previous agent actions, tool results, and detailed instructions.

xAI nevertheless recommends context compaction for long agent loops, showing that the intended architecture does not rely on continuously filling the entire context window.

The documentation also recommends setting a prompt_cache_key, which helps route related requests consistently and increases the probability that reusable context is served from cache rather than processed again at full input cost.

Grok 4.7 additionally returns encrypted reasoning content automatically through the Responses API. Developers can pass those reasoning items unchanged into subsequent requests, allowing reasoning state to persist through longer multi-turn interactions without exposing the underlying reasoning content.

The model therefore combines four elements relevant to persistent agents: large context, context compaction, prompt caching, and preserved reasoning state.

··········

PRICING CHANGES WHEN PROMPTS EXCEED 200,000 TOKENS

Grok 4.7 starts at $2 per million input tokens, $0.50 per million cached input tokens, and $6 per million output tokens for prompts below 200,000 tokens.

For prompts above 200,000 tokens, xAI's September 21 release notes specify rates of $4 per million input tokens, $1 per million cached input tokens, and $12 per million output tokens.

........

PROMPT SIZE

INPUT

CACHED INPUT

OUTPUT

Below 200k tokens

$2 / 1M

$0.50 / 1M

$6 / 1M

Above 200k tokens

$4 / 1M

$1 / 1M

$12 / 1M

........

This pricing structure makes context management economically relevant rather than purely technical. A workload repeatedly carrying more than 200,000 prompt tokens crosses into the higher pricing tier, so compaction and caching can directly affect total inference cost.

For agent deployments, cost per completed task is therefore a more informative operational measure than headline token price alone. A model that uses more reasoning but completes a task with fewer retries may have a lower final workflow cost. A system that repeatedly reloads large contexts or fails late in long execution chains can produce the opposite result.

··········

GROK 4.7 FAST AND AVAILABILITY

Grok 4.7 is available through the xAI API, Grok Build, Cursor, third-party coding harnesses, model routers, and cloud platforms. xAI lists OpenRouter, Vercel, and Cloudflare among supported model gateways.

It is the default model in Grok Build and is available through the xAI API under the grok-4.7 identifier.

A separate Grok 4.7 Fast option uses the same underlying model on faster serving infrastructure. xAI states that it provides approximately twice the output speed at twice the standard token rates. Grok 4.7 Fast is currently available through Cursor and Grok Build, but not through the public xAI API.

A US regional API endpoint is also available for workloads requiring inference to remain in the United States, with token usage priced at a 10% premium.

··········

GROK 4.7 PUSHES XAI FURTHER INTO LONG-RUNNING AI WORK

The main change in Grok 4.7 is not any single benchmark result or the 500,000-token context window.

xAI has combined a larger base model with longer reinforcement learning, harder multi-hour training tasks, stronger self-verification, managed long context, persistent reasoning, and direct integration with an agent harness.

The benchmark results published at launch also extend across coding, terminal operations, office work, legal tasks, healthcare, and engineering rather than clustering around one narrow capability.

For production deployments, the useful comparison with Grok 4.6 will be whether these changes improve successful task completion per unit of time and cost. Completion rate, retries, total token consumption, tool calls, elapsed execution time, and human correction provide a more useful measurement of that shift than an isolated benchmark score.

Grok 4.7 gives xAI a model explicitly designed around that type of workload: AI that remains active long enough to complete substantial professional tasks rather than stopping after producing an individual response.
