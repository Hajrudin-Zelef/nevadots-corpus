---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-beats-claude-opus-5-5
title: "Gemini 4 Neon beats Claude Opus 5.5"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-22"]
keywords: ["neon", "opus", "tokens", "output", "million", "claude", "input", "gemini"]
source: https://medium.com/data-science-in-your-pocket/gemini-4-neon-beats-claude-opus-5-5-8a2cc714d68f
source_anchor: ""
source_lines: [1, 111]
section: "Gemini_Google"
---
# Gemini 4 Neon beats Claude Opus 5.5

## The useful question is not “Who is smarter?”

Give two AI agents the same messy assignment. Ask them to inspect a repository, read a financial chart, use a terminal, call external tools, and return a finished piece of work. A simple question-answer benchmark cannot capture that workflow. You now care about planning, recovery, context management, tool use, token consumption, and whether the model knows when to stop.

That is the right frame for comparing **Google’s Gemini 4 Neon** and **Anthropic’s Claude Opus 5.5**. Both are positioned as production models for complex, multi-step work. Both offer a 1 million-token context window. Both sit near the top of current independent evaluations. Yet they make different economic and operational trade-offs.

Neon arrived on November 18, 2026, with Google emphasizing fast end-to-end execution across coding, knowledge work, and science. Google’s launch post repeatedly returns to the same idea: frontier-level capability at a lower cost per task. Opus 5.5, released September 22, 2026, is presented by Anthropic as its strongest Opus model for long-running agents, serious coding, and professional work.

The short version? Neon looks like the stronger value-and-breadth candidate. Opus remains a serious choice for sustained agentic engineering, especially if your workflow already runs on Claude. The detailed numbers are less one-sided than either launch narrative.

## Pricing: the easy comparison, with a hidden complication

At list price, Neon costs **$3 per million input tokens** and **$15 per million output tokens**. Cached input drops to **$0.15 per million tokens**. Opus 5.5 costs **$4 per million input tokens** and **$20 per million output tokens**, with cache reads at **$0.20 per million tokens**.

That gives Neon a straightforward 25% price advantage across standard input and output. A synthetic request using 100,000 uncached input tokens and producing 50,000 output tokens would cost:

- **Neon:**`(0.1 × $3) + (0.05 × $15) = $1.05` .
- **Opus 5.5:**`(0.1 × $4) + (0.05 × $20) = $1.40` .

If the input is fully cached, the same request drops to approximately **$0.765 on Neon** and **$1.02 on Opus**. At that point, output tokens dominate the bill.

But agent economics are not just a rate-card calculation. A cheaper model can become more expensive if it needs extra retries, rereads files, produces verbose reasoning, or fails verification. A more expensive model can win if it solves the task in fewer turns. This is why **cost per successfully completed task** is more useful than cost per million tokens.

Anthropic says Opus 5.5 costs about 40% less than Opus 5 for typical token-billed workloads, combining lower prices with improved token efficiency. That comparison is against its predecessor, not against Neon. Google similarly claims substantial task-cost savings for Neon against competing frontier models. Treat both as vendor-reported efficiency claims until you reproduce them on your own traces.

## Context and output: equal workspace, different delivery capacity

Both models provide a **1 million-token context window**. In practical terms, that creates room for large repositories, lengthy reports, dense spreadsheets, and multimodal source material. The more interesting difference is maximum output capacity.

Vals AI’s model entries list Neon at up to **1 million output tokens**, while Opus 5.5 is listed at **128,000**. On paper, that is an 8x difference.

Do not interpret this as “Neon writes better long documents.” Maximum output is an engineering ceiling, not a quality score. A million-token answer can be redundant, drifting, or impossible to review. Where the ceiling matters is long-running generation pipelines, large synthetic-data jobs, extended code transformations, and agents that must emit substantial structured artifacts without manually stitching many responses.

For most interactive work, disciplined summarization and explicit intermediate state will matter more than the theoretical maximum. A good agent should compress its working memory, preserve decisions, and avoid turning context size into an excuse for unstructured prompting.

## The overall benchmark result is basically a near tie

The cleanest head-to-head evidence comes from using the same independent evaluator for both models. In Vals AI’s November 19 snapshot, the overall results are:

- **Vals Index:** Neon 70.87% ±0.94, Opus 5.5 70.14% ±1.00.
- **Ranking at that snapshot:** Neon #2 of 42, Opus #5 of 42.
- **Reported evaluation cost:** $7.405 per test for Neon, $24.77 for Opus.
- **Reported evaluation latency:** 31 minutes 25 seconds for Neon, 84 minutes 31 seconds for Opus.

The score gap is just **0.73 percentage points**, smaller than either reported uncertainty range. Calling Neon the decisive intelligence winner would overread the data. The more defensible conclusion is that the models are close on this aggregate suite, while Neon’s evaluated runs were substantially cheaper and faster.

Those cost and latency figures are not universal API guarantees. They describe Vals’ evaluation setup, which used high reasoning settings and potentially long agentic tasks. Interactive latency, batch throughput, cache behavior, and tool latency can look very different in production.

Still, this is commercially important. If two models are statistically close on the work you care about, a large difference in evaluation cost becomes a genuine systems advantage.

## Coding: one tie, one Neon lead, and one warning

The coding numbers show why benchmark aggregation can hide the actual story.

- **Terminal-Bench 4.0:** 61.62% for Neon and 61.62% for Opus, a direct tie in Vals’ listed results.
- **Vibe Code Bench:** Neon 93.70%, Opus 90.29%, a 3.41-point lead for Neon.
- **Terminal-Bench Science:** Neon 62.86%, Opus 48.57%, a larger 14.29-point separation.

Neon therefore has the better numerical case in this snapshot. But coding quality is not reducible to a single resolve rate. Repository work also involves locating the root cause, limiting the blast radius of edits, preserving style, running tests, and explaining the resulting diff.

Anthropic’s product material leans heavily into those behaviors. It describes Opus 5.5 as finding root causes before editing, checking its work, coordinating subagents, and sustaining long sessions with less oversight. Those are valuable engineering properties, but they are workflow claims rather than a direct refutation of Neon’s benchmark scores.

Google’s more interesting technical framing is Neon as an orchestrator for parallel Hydrogen subagents inside Antigravity. That points to a hierarchical agent design: a higher-judgment model plans and delegates, while cheaper workers execute narrower tasks. If that pattern works on your repository, the effective comparison is no longer “Neon versus Opus.” It becomes **a Neon-led model swarm versus a Claude-centered agent loop**, including the quality of the surrounding tools.

## Agent benchmarks need a comparability warning

AutomationBench initially looks like a major Neon win. Google reports **56.3%**, while Anthropic’s Opus page lists **40.0%**. Both reference Zapier’s automation evaluation, but the surrounding conditions are not identical.

Anthropic notes that its early-access runs did not use fallback models and safeguard interventions counted as failures. Vals also reports fallback behavior for Opus in parts of its own evaluation suite. These details can materially change agent scores, especially on long tasks with many opportunities for a refusal or recovery event.

So the honest reading is directional: Neon has strong evidence for automation and orchestration, while Opus’ score needs to be interpreted alongside its safeguard and fallback configuration. A ranking that hides those variables is not a controlled model comparison.

For production, track the complete agent trajectory. A final answer score alone will not tell you whether the model recovered cleanly, exceeded its permissions, or silently delegated the hard part to another system.

## Knowledge work and science favor Neon in this snapshot

The professional-work results are some of Neon’s strongest evidence:

- **Legal Research Bench:** Neon 56.25%, Opus 50.48%.
- **Finance Agent Bench v2.1:** Neon 68.56%, Opus 58.59%.
- **BioMysteryBench:** Neon 80.00%, Opus 79.26%.

Legal and finance show meaningful Neon leads in Vals’ runs. BioMystery is effectively close. That combination suggests Neon is not merely a coding model with a cheaper API. It has a credible case for document-heavy, multimodal, analytical production work.

Opus should not be dismissed here. Anthropic specifically targets financial analysis, dense document interpretation, charts, spreadsheets, slides, and computer use. Its ecosystem may still be the better operational fit for teams already using Claude across professional workflows. The benchmark says something about measured task performance, not the cost of migrating your prompts, tools, permissions, and review process.

## Safety belongs in the evaluation harness

As agents gain tool access, safety stops being a separate policy document. It becomes part of task completion.

Google highlights Neon’s agent-honesty testing, prompt-injection resistance, alignment monitoring, and reduced under-service or overreach. Anthropic pairs Opus 5.5 with expanded safeguards and a system card, while Vals reports fallback and refusal behavior. These measurements are related, but they are not interchangeable.

A useful internal evaluation should record more than accuracy:

- **Task success:** completed correctly without human rescue.
- **Unit economics:** total tokens, tool cost, retries, and cost per solved task.
- **Operational behavior:** tool calls, wall-clock time, fallbacks, and recovery attempts.
- **Change quality:** tests passed, review comments, diff size, and unrelated edits.
- **Safety behavior:** refusals, prompt-injection failures, scope violations, and unnecessary destructive actions.

Run the same tasks with the same tools, repository state, thinking budget, and fallback policy. Otherwise, you are comparing two agent systems rather than isolating the models.

## Which one should you choose?

**Choose Gemini 4 Neon** if your priority is high-volume production work, lower list-price token costs, a very large output ceiling, multimodal analysis, and strong current results across Vals’ coding, finance, legal, and science evaluations. Its clearest advantage is the combination of near-frontier aggregate performance and much lower reported evaluation cost.

**Choose Claude Opus 5.5** if your workflow is already built around Claude Code or Anthropic’s platform, and you value deliberate long-horizon coding, root-cause debugging, subagent coordination, and a mature professional-work ecosystem. The overall Vals score is close enough that migration should be justified by measured workload gains, not leaderboard momentum.

My practical verdict is this: **Neon wins the stronger evidence-based value case in the current snapshot. Opus remains a credible engineering choice, not a model that has suddenly become obsolete.** For a serious deployment, take 20 to 50 representative tasks, run both through identical harnesses, and optimize for verified completions per dollar. The model that finishes your real work reliably is the one that wins.
