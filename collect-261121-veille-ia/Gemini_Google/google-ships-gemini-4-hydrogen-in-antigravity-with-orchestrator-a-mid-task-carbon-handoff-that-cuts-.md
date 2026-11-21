---
id: collect-261121-veille-ia/gemini-google/google-ships-gemini-4-hydrogen-in-antigravity-with-orchestrator-a-mid-task-carbon-handoff-that-cuts-
title: "Google ships Gemini 4 Hydrogen in Antigravity with Orchestrator, a mid-task Carbon handoff that cuts quota 24% and runs 19% faster | cho.sh"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hydrogen", "carbon", "orchestrator", "google", "gemini", "model", "antigravity", "quota"]
source: https://cho.sh/mini/news/ai-2/gemini-hydrogen-orchestrator
source_anchor: ""
source_lines: [1, 37]
section: "Gemini_Google"
---
# Google ships Gemini 4 Hydrogen in Antigravity with Orchestrator, a mid-task Carbon handoff that cuts quota 24% and runs 19% faster

- **Gemini 4 Hydrogen** scores 51 on the Artificial Analysis Intelligence Index (54 at its highest setting) and 57% on Terminal-Bench 4.1, against 3.8 Flash's 18.2%, with four thinking levels: low, medium, high (default), and a new**xhigh** .
- **Orchestrator** switches between Carbon and Hydrogen inside a single trajectory: Carbon explores read-only and builds a plan, Hydrogen writes edits and runs tests, and hands back to Carbon when it hits friction. On Terminal-Bench 4.1 that reached near-Carbon success with**24% less quota** and**19% faster** completion.
- Google argues static model routers break down on long-running agents because the difficulty of the work changes as the task evolves, which is the stated reason for a dynamic bidirectional handoff instead of routing once from the first prompt.
- Hydrogen rolls out to paid **Google AI Pro and Ultra** subscribers. Orchestrator ships as Orchestrator (Carbon) and Orchestrator (Argon) for Ultra, and as Orchestrator (Argon) for Pro, with enterprise access through Gemini for business.
- The **Antigravity SDK** lets developers wire Hydrogen and Carbon into explicit pipelines by role; Google cites an open-source issue triage demo where Hydrogen agents triage and code while a Carbon agent handles refactors, new features, and security checks.

## Hacker News opinions

Isn't this just what Opus already does under the hood? I use Opus 5.5 on high for everything and my bill went down, and I doubt the same model is doing the file searches. I don't see why Google needed a new name for it instead of making it the default.

Same here, my usage dropped when I switched models. My theory is a smarter model just needs fewer tokens to finish the same task.

Excellent mode. I don't have to keep switching models and effort levels by hand.

I already pin models to subagents by difficulty, so handing that call to the model makes me nervous. Models are optimistic about their own abilities. That said, plenty of tasks are boilerplate or test authoring that a flash model could handle.

The only reason to bother with this is saving quota, and the value for money in Antigravity is bad enough that you're better off using Claude Opus or GPT Sol for everything.

This is Anthropic's advisor mode and GitHub's HydraFusion pipeline again. What I don't get is caching: how is it not constantly compacting when routing between models?

Advisor has to send the whole context to the other model, but the advisor's output tokens are few so it stays cheap. Orchestrator delegating to subagents as needed should be fine for cache.

I can't tell which model is supposed to be better. I think Carbon is the big one and Hydrogen is the Flash equivalent, and I only know that from a leak.

OpenAI's naming is the least confusing of the lot: Luna the moon, Terra the earth, Sol the sun, Astra from astrum. Gemini's is the one nobody can parse.

Whatever they did to this page, scrolling feels awful, and swipe left and right to navigate is dead. Scroll behavior should be behind a permission.

Argon and Carbon are so slow that Opus finishes several things in the time they take for one task. I switched to 3.8 Flash, so a new Flash-class model is welcome. Gemini also hoovers up a bazillion files into context for something as simple as updating a docstring.

25% savings seems low for co-trained models with harness support. We bridge Opus to DeepSeek Flash over ACP and see 60%.

Is this usable on a plain Pro account or do I need Ultra? I burned through my Pro tokens in four minutes with Argon.

Hydrogen shows up on Pro for me, it replaced 3.8 Flash. Orchestrator is the Ultra-only part.
