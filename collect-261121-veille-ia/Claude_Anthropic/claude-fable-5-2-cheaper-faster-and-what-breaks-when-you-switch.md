---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-cheaper-faster-and-what-breaks-when-you-switch
title: "Claude Fable 5.2: Cheaper, Faster, and What Breaks When You Switch"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "anthropic", "claude", "opus", "output", "effort", "model", "faster"]
source: https://www.tldl.io/blog/claude-fable-5-2-launch
source_anchor: ""
source_lines: [1, 72]
section: "Claude_Anthropic"
---
Anthropic released **Claude Fable 5.2** and **Claude Mythos 5.2** on November 9, 2026. Fable 5.2 replaces Fable 5.1 as Anthropic's most capable broadly available model. It costs less, Anthropic says it generates output up to 30% faster, and it posts large gains on agentic coding, science, and robotics benchmarks.

The short version: **if you already run Fable 5.1, you should move. It's cheaper and stronger. Budget time for four breaking API changes, though, and don't assume it beats Opus 5.5 on cost for routine work.**

## Pricing: 20% lower than Fable 5.1

|  | Fable 5.2 | Fable 5.1 | Opus 5.5 | 
|---|---|---|---|
| Input (per 1M tokens) | $8 | $10 | $4 | 
| Output (per 1M tokens) | $40 | $50 | $20 | 
| Cache read | $0.20 | $0.25 | $0.20 | 
| 5-minute cache write | $10 | $12.50 | $5 | 
| 1-hour cache write | $16 | $20 | $8 | 

Batch API requests are half price ($4 input, $20 output). An optional **fast mode** generates output up to 2.5x faster at $16 input and $80 output per million tokens. It's available on the Claude API, Claude Models on Google Cloud, Microsoft Foundry (Anthropic hosted), and in Claude Code. US-only inference costs 1.1x the standard rates.

Cache reads stay at 2.5% of the input price, the same multiplier Fable 5.1 had. That matters for agents that resend a large, stable prefix on every turn.

The model ID is `claude-fable-5-2`. It has a 1M-token context window, 128K max output tokens, and a June 2026 knowledge cutoff. It's on the Claude API, Claude Platform on AWS, Amazon Bedrock (`global.anthropic.claude-fable-5-2`, bedrock-runtime only), both Google Cloud options, and Microsoft Foundry.

## Benchmarks

Anthropic compared Fable 5.2 with Fable 5.1, Opus 5.5, OpenAI's GPT-6 Astra, and Google's Gemini 4 Carbon. Scores are as Anthropic published them, at each model's highest available effort unless noted.

| Benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | 49.1% | 32.4% | 45.5% | 42.8% | 41.1%¹ | 
| FrontierSWE v2 (agentic coding) | 69.9% | 56.3% | 62.3% | 65.5% | 61.2% | 
| AutomationBench (business workflows) | 51.4% | 31.4% | 42.5% | 41.4% | 53.9% | 
| Terminal-Bench-Science 0.1 | 74.6% | 52.6% | 58.7% | 68.1% | 64.8% | 
| OSWorld 2.1 (computer use, partial offline set) | 89.3% | 83.5% | 87.2% | 79.5% | 81.5% | 
| RoboDojo Sim (robotics) | 64.9% | 27.6% | 39.4% | 29.8% | — | 
| Remote Labor Index | 41.3%² | 17.9% | 21.3% | 20.8% | — | 
| Humanity's Last Exam (with tools) | 74.2% | 65.6% | 67.7% | 57.2% | — | 
| Chartography (no tools) | 73.4% | 44.8% | 64.4% | 71.0% | 72.5% | 

¹ Gemini 4 Carbon at medium effort. ² Fable 5.2 at high effort.

Three things stand out. The jump over Fable 5.1 is large on long-horizon agent work: Remote Labor Index more than doubles, and RoboDojo goes from 27.6% to 64.9%. Fable 5.2 isn't first everywhere, though. Gemini 4 Carbon leads AutomationBench, and on the AA-Briefcase knowledge-work eval, Opus 5.5 has a higher Elo (1800) than Fable 5.2 (1785), even though Fable 5.2 has the better rubric score. And these are vendor-run numbers on benchmarks that mostly aren't public yet, so treat them as a shortlist signal, not a verdict.

Anthropic also describes demonstrations: 1,000 Fable 5.2 agents formalizing Fermat's Last Theorem in 30 hours, the model designing three working electronic devices in a day, and progress on two open problems in black-hole physics. These show what's possible with heavy orchestration. They don't tell you how the model will do on your tickets.

## Fable 5.2 or Opus 5.5?

Per token, Fable 5.2 costs exactly twice as much as Opus 5.5. Anthropic's argument is cost per task, not per token: on FrontierCode 2.0, Fable 5.2 at high effort scored about the same as Opus 5.5 at max effort, for roughly half the cost per task.

Anthropic's own docs still say to start most workloads on Opus 5.5, and move to Fable 5.2 when your evals at higher Opus effort fall short. That's sound advice. Use Fable 5.2 for multi-hour agent sessions, hard migrations, and research tasks where a failed run costs more than the token bill. Keep Opus 5.5, or Sonnet 5.5 at $2/$10, for chat, extraction, and everyday coding. You can also combine them: Fable 5.2 can act as an advisor or orchestrator for cheaper Claude models.

Default effort is `high`. In Anthropic's guidance, most of the gain is already present at `high`. Use `xhigh` for the hardest long-running coding work, and save `max` for measuring the ceiling.

## Claude Mythos 5.2: same model, restricted access

Mythos 5.2 is the same model at the same price, with fewer cybersecurity and biology safeguards. It's available only to organizations verified through Anthropic's programs: the Cyber Verification Program for security teams and the Life Sciences Verification Program for researchers. Claude Enterprise customers outside those programs get Mythos 5.2 indirectly through Claude Security, Anthropic's codebase vulnerability scanner.

For everyone on Fable 5.2, Anthropic reports about 55% fewer safety-classifier blocks than on Fable 5.1 in early testing across claude.ai, Claude Code, and Cowork (roughly 60% fewer in cybersecurity and 40% fewer in biology). Refusals still return HTTP 200 with `stop_reason: "refusal"`, so keep handling them; Opus 4.8 and Opus 5 are the permitted fallback models.

## What breaks when you switch

Changing the model string from `claude-fable-5-1` to `claude-fable-5-2` is most of the work, but Anthropic lists four breaking changes:

1. **Editing earlier turns invalidates thinking blocks on every account.** If your code changes the system prompt, tools, or any earlier message and then sends old thinking blocks back, the request returns a 400. On Fable 5.1 this was only enforced for accounts created on or after August 31, 2026, so older accounts may hit it for the first time. Keep history append-only.
2. **Only Fable 5.2 and Mythos 5.2 can read Fable 5.2 thinking blocks.** If a router or fallback moves a conversation to another model, including Opus 5.5 or Fable 5.1, that model silently loses the earlier reasoning.
3. **The `computer_20251124` computer-use tool returns a 400** everywhere except Amazon Bedrock. Move to`computer_toolset_20260801` .
4. **With the advisor tool, a Fable 5.2 executor needs a Fable 5.2 or Mythos 5.2 advisor.**

Rules carried over from Fable 5.1 still apply: adaptive thinking is always on, forced `tool_choice` returns a 400, you should omit `temperature`, `top_p`, and `top_k`, and 30-day data retention is required unless Anthropic authorizes zero data retention.

Expect some behavior changes even without code changes. Prompt lines asking the model to skip thinking no longer work at `medium` effort and above. On long tasks, turns can end early with a list of what's left, especially at lower effort. And the model follows style rules literally, so a hard sentence-length limit can make its writing choppy.

## Bottom line

For teams already paying Fable prices, 5.2 is an easy decision: it's 20% cheaper, faster, and much stronger on agentic work. Plan a day to audit history edits, model routing, and computer-use tools. For teams on Opus 5.5, run your hardest evals on both before switching, and compare cost per completed task, not cost per token.
