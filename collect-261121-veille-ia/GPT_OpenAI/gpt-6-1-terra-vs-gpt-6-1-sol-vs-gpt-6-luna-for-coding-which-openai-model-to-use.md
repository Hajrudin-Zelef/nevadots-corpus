---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-vs-gpt-6-1-sol-vs-gpt-6-luna-for-coding-which-openai-model-to-use
title: "GPT-6.1 Terra vs GPT-6.1 Sol vs GPT-6 Luna for coding: which OpenAI model to use"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "terra", "luna", "session", "long", "coding", "model", "default"]
source: https://bito.ai/blog/gpt-6-1-terra-vs-gpt-6-1-sol-vs-gpt-6-luna/
source_anchor: ""
source_lines: [1, 81]
section: "GPT_OpenAI"
---
OpenAI now has three models a coding team can choose between, and the question we keep hearing is which one should be the default. The new GPT-6.1 Terra sits in the middle. It costs $1 per million input tokens and $5 output, half the price of GPT-6.1 Sol and ten times the price of the small GPT-6 Luna. So we ran all three through the same coding benchmark, each at its default medium effort, to see what the middle model buys you.

Short answer: GPT-6.1 Terra is the right default for most coding work. It scored 69% overall at $0.24 a session and 2.9 minutes, against 74% for GPT-6.1 Sol at $0.98 and 10.4 minutes, and 56% for GPT-6 Luna at $0.04. Terra’s weak spot is planning a change in a long session, where it trails Sol by 13 points.

These are our own runs on real coding work, not numbers the vendor reported.

## GPT-6.1 Terra vs GPT-6.1 Sol: which is better for coding?

Sol scores 5 points higher overall, but Terra costs about a quarter as much per session and takes about a quarter of the time. Scores are percentages of the maximum, blended across our six main tasks. On quick one-shot asks the two are within two points of each other.

| Model | Overall score | Cost per session | Time per session | Long sessions | Cost per long session | Time per long session | One-shot asks | 
|---|---|---|---|---|---|---|---|
| GPT-6.1 Sol | 74% | $0.98 | 10.4 min | 78% | $2.06 | 22.7 min | 81% | 
| NewGPT-6.1 Terra | 69% | $0.24 | 2.9 min | 72% | $0.40 | 4.4 min | 79% | 
| GPT-6 Luna | 56% | $0.04 | 3.1 min | 59% | $0.06 | 4.9 min | 68% | 

All three at default (medium) effort. Best value in each column in bold.

Shorter multi-turn tasks, like a vague bug report or a test-first fix, follow the same order: Sol 72% at $0.44, Terra 67% at $0.16 and Luna 55% at $0.03.

## How fast is GPT-6.1 Terra?

Much faster than Sol. A long build session that takes Sol 23 minutes takes Terra four and a half. That was the first thing we noticed in the results.

**Most of the difference is Terra doing less work.** On our long sessions it made about 40 tool calls where Sol made 50 to 90, and it spent 1,000 to 3,000 reasoning tokens on a whole five-turn session where Sol spent 10,000 to 33,000. Luna is about as fast as Terra, at 4.9 minutes per long session.

## Where does GPT-6.1 Terra fall behind Sol?

On planning. Our long sessions are five-turn conversations on a real service: trace the relevant code, analyze a failure, plan a fix, implement it and audit the result. Here is each turn.

| Turn in a long session | Score by model | 
|---|---|
| Trace the code |  | 
| Analyze the failure |  | 
| Plan the fix |  | 
| Implement |  | 
| Audit |  | 

Reading code, Terra is as good as Sol. The plan turn is where it drops 13 points, and that’s the turn where a thin answer costs you most, because the implementation follows the plan. Terra’s plans were shorter and skipped details the answer key asks for, like how a new rate limiter should behave while Redis is down or reconnecting. By the audit both models are weak, which says more about how hard that turn is than about either one.

## Is GPT-6 Luna good enough for coding?

Only for lookups you’ll check yourself. Luna trails Terra by 10 to 15 points on every kind of work, and it was the only one of the three to fall apart on a one-shot task. Asked where an agent’s admin token is configured, what uses it and what happens if it’s unset, Luna scored 45%. Terra scored 83% on the same question. At four cents a session it’s tempting, but we wouldn’t let it touch a multi-step change.

## How much do GPT-6.1 Terra, Sol and Luna cost?

At OpenAI’s list prices, a long session cost $2.06 on Sol, $0.40 on Terra and $0.06 on Luna. The rates per million tokens are $2 input, $0.10 cached input and $10 output for Sol, $1, $0.05 and $5 for Terra, and $0.10, $0.01 and $0.50 for Luna.

## Which OpenAI model should be your default?

Make GPT-6.1 Terra the default for reading code, answering questions and small fixes. It’s within a couple of points of Sol there, about four times cheaper and much faster.

Switch to GPT-6.1 Sol when you ask for a plan, and keep it for the change that follows. That’s where the 13-point gap is, and a plan that misses a constraint costs more to fix later than the extra 18 minutes. Leave GPT-6 Luna for lookups you’ll check yourself. If you’re weighing Terra against Claude instead, see our GPT-6.1 Terra vs Claude Opus, Sonnet and Haiku benchmark. Claude Opus 5.5 is still the strongest model we test, at 85% overall.

## How we tested

The benchmark is a set of agentic coding tasks taken from real work on our own services: long five-turn build sessions, shorter multi-turn tasks (a vague bug report, a test-first fix, a design review with author pushback and a quick fix) and one-shot asks. Every model gets the same fixed prompts, default settings and no extra instructions. We ran these three OpenAI models in Codex at its default medium effort. We run Claude models in Claude Code, and we’ve found models score about the same in Claude Code, Codex and Cursor, so the tool isn’t what we’re measuring.

Terra and Luna ran four long sessions per task and two per task elsewhere. Sol’s figures pool twelve earlier long sessions with a fresh check on the same build. Every turn is graded against an answer key checked against the code, by a judge scoring five dimensions. The judge is Claude Opus 5.5, which isn’t in this race. Costs come from each session’s token counts at OpenAI’s list prices. Session-to-session noise is about 3 points, so read 1 to 2 point gaps as ties.

## FAQ

**Is GPT-6.1 Terra better than GPT-6.1 Sol for coding?**

No, Sol scores higher: 74% overall against 69%, and 84% against 71% on planning a fix. But Terra costs $0.24 a session against Sol’s $0.98.

**Is GPT-6.1 Terra faster than GPT-6.1 Sol?**

Yes, by a lot. A long coding session took Terra 4.4 minutes and Sol 22.7 minutes.

**How much does GPT-6.1 Terra cost?**

$1 per million input tokens and $5 output, half of Sol’s price. That came to $0.40 per long session, against $2.06 for Sol.

**Is GPT-6 Luna good for coding?**

Only for simple lookups. It scored 56% overall, 13 points behind GPT-6.1 Terra, and 45% on one question where Terra scored 83%.

**How does GPT-6.1 Terra compare with Claude?**

It scored 69% overall, against 76% for Claude Haiku 5.5 and 85% for Claude Opus 5.5, but it finished sessions faster than any Claude model.
