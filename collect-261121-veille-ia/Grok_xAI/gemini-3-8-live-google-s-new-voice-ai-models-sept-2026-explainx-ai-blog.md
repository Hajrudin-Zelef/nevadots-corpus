---
id: collect-261121-veille-ia/grok-xai/gemini-3-8-live-google-s-new-voice-ai-models-sept-2026-explainx-ai-blog
title: "Gemini 3.8 Live: Google's New Voice AI Models (Sept 2026) | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-15"]
keywords: ["voice", "gemini", "live", "google", "models", "extended", "thinking", "background"]
source: https://www.explainx.ai/blog/google-gemini-3-8-live-extended-thinking-2026
source_anchor: ""
source_lines: [1, 84]
section: "Grok_xAI"
---
Google shipped two new voice models on September 15, 2026: **Gemini 3.8 Live** and **Gemini 3.8 Live Extended Thinking**, its latest push into near-real-time spoken dialogue. The pitch is narrow but concrete — talking to Gemini should feel less like waiting for a chatbot and more like a live conversation with someone who can act on what you say while still talking to you.

This lands the same week as TypeSafe AI's Jev launch, another bet on cutting the latency between input and useful action — a signal that low-latency, in-the-loop AI is becoming its own competitive front, separate from the usual reasoning-benchmark race.

## TL;DR

| Question | Answer | 
|---|---|
| What are the two models? | Gemini 3.8 Live (fast, cost-efficient dialogue) and Gemini 3.8 Live Extended Thinking (deeper multi-step reasoning) | 
| What's new? | Both reason and act in the background — executing tool calls and API requests while the conversation continues, rather than going silent | 
| How good is it? | 3.8 Live Extended Thinking scores #1 overall on Artificial Analysis' Speech-to-Speech Quality Index (82.6), 68.6% on τ-Voice agentic tasks, and 97.7% on Big Bench Audio | 
| Where can I use it? | Gemini API, Google AI Studio, Gemini Enterprise (private preview), Search Live, and — for 3.8 Live Extended Thinking — the Gemini app and Workspace (Docs, Gmail, Keep) | 
| Who's building on it? | Agora, Fishjam, LangChain, LiveKit, Pipecat, Vercel, Vision Agents at the infra layer; Salesforce, Genspark, and Lumeris as early enterprise partners | 
| Is the audio watermarked? | Yes, with SynthID, embedded directly in the generated audio | 

## What actually changed

Google frames this release around one problem: voice agents that go quiet while they "think" feel broken, even if the eventual answer is right. Both new models are built to avoid that dead air.

**Gemini 3.8 Live** is the cost-efficient, high-throughput model — Google's building block for scaled voice agents. It processes visual input in near real time, detects and switches between 97 languages mid-conversation without a manual reset, and executes tool calls and API requests in the background while continuing to talk. It placed second in the Speech Agent Arena and, per Google, remains "highly cost-effective" for teams building at volume.

**Gemini 3.8 Live Extended Thinking** is the model for harder, multi-step voice tasks — the kind that would normally force a pause. Instead of going silent, it "reasons and speaks simultaneously," using natural verbal bridging ("Let me check that…") while working, and narrates progress on longer background tasks as it goes. On Artificial Analysis' Speech-to-Speech Quality Index, it now holds the top overall spot at 82.6, alongside 68.6% on τ-Voice agentic task completion, 35.1% on Sierra's banking-specific τ-Voice-banking benchmark, and 97.7% on Big Bench Audio. Google also cites ServiceNow's EVA-Bench as evidence the two models "push the Pareto frontier" for balancing accuracy against conversational quality on complex workflows.

## Why this matters for builders, not just consumers

The headline features aren't just consumer polish — they change what's practical to build with a voice interface today:

- **Background tool execution** means a voice agent can acknowledge a request immediately ("sure, checking now") and keep talking while a slower API call, database lookup, or multi-step workflow finishes, instead of the user staring at a silent app.
- **Mid-conversation language switching** removes a real integration headache for multilingual support lines and international consumer products — no session reset, no separate model per locale.
- **Visual grounding in near real time** lets a voice session incorporate what a camera or screen is currently showing, not just what was said — relevant for anything from remote troubleshooting to accessibility tools.

That's also why the initial partner list leans toward voice infrastructure providers rather than end-consumer apps: Agora, Fishjam, LangChain, LiveKit, Pipecat, Vercel, and Vision Agents all handle the real-time media streaming plumbing so application teams can build the interaction layer rather than the transport layer. On the enterprise side, Salesforce, Genspark, and Lumeris are cited as early adopters specifically citing latency, conversational fluidity, and tool-calling reliability — the same three properties this release is built around.

## How it fits next to the rest of Google's Gemini lineup

Gemini 3.8 Live sits alongside Gemini 3.8 Flash and the broader Gemini model family as Google's answer to the reasoning-heavy releases coming out of OpenAI and Anthropic. Rather than chasing frontier benchmark scores across the board, Google has consistently leaned into two areas where it has a structural advantage: multilingual fluency (baked into Search and Translate for over a decade) and product-surface distribution (Search, Workspace, Android). This release doubles down on both — voice quality and background execution are exactly the properties that matter when a model is reached through a phone call, a car, or a screen reader, rather than a chat window.

## Rollout details

| Model | Developers | Enterprise | Consumer | 
|---|---|---|---|
| Gemini 3.8 Live | Gemini API, Google AI Studio | Private preview in Gemini Enterprise; coming to Gemini Enterprise for Customer Experience | Search Live | 
| Gemini 3.8 Live Extended Thinking | Gemini API, Google AI Studio | Private preview in Gemini Enterprise; coming to Enterprise for Customer Experience and Workspace business customers | Gemini Live app; Google AI Pro/Ultra subscribers in Workspace (Docs), and all Google AI subscribers in Gmail and Keep | 

Both models are rolling out starting September 15, 2026. Google has not published a standalone pricing page for either model at launch; developers building against them today should check the Gemini API pricing docs directly, since Live-tier pricing has historically differed from Google's standard text-generation rate cards.

## What voice-first products should actually test before adopting it

If you're building a voice product and deciding whether to move to Gemini 3.8 Live, the marketing claims translate into a short list of things worth verifying yourself before committing:

- **Interruption handling.** A model that "reasons while speaking" needs to gracefully handle a user interrupting mid-sentence — test this explicitly, since background-execution demos rarely showcase interruption edge cases.
- **Tool-call latency under load.** Background execution is only useful if the tool call actually completes faster than the conversation naturally continues; test with your real backend, not a demo API that returns instantly.
- **Language-switch reliability with accents and code-switching.** Automatic 97-language detection is a strong claim — bilingual speakers who switch languages mid-sentence (common in many markets) are the hardest case, and worth testing directly rather than assuming the benchmark languages represent your users.
- **Cost at your actual call volume.** Voice API pricing structures (per-minute versus per-token) can make total cost hard to estimate from a rate card alone — run a realistic load test before committing budget.

None of this is unique to Google — it's the same due-diligence checklist that applies to any voice-model migration, including comparisons against OpenAI's realtime voice API or open-source speech-to-speech stacks. The point of a benchmark-topping launch announcement is to earn a place on your shortlist, not to skip the evaluation step entirely.

## The honest caveats

Google's own comparisons lean on benchmarks it selected — the Speech-to-Speech Quality Index, τ-Voice, and EVA-Bench are all real, third-party-run evaluations, but none of them is a universal standard the way MMLU or SWE-bench has become for text models, so treat the "#1 overall" claim as true within that specific evaluation, not as a blanket statement across every voice benchmark. Google also hasn't published raw end-to-end latency numbers for this release (unlike TypeSafe AI's Jev launch the same week, which leads with exact millisecond figures) — "near real-time" and "background execution" are qualitative claims until you test them against your own workload. If sub-second responsiveness is the deciding factor for your use case, benchmark it directly rather than taking the marketing copy at face value.

## FAQ

**What is Gemini 3.8 Live?**
Google's newest near-real-time dialogue model, launched September 15, 2026, built for cost-efficient voice conversations with visual grounding and background tool execution.

**What is Gemini 3.8 Live Extended Thinking?**
The higher-intelligence sibling, built for complex multi-step voice tasks — it reasons and speaks simultaneously and tops Artificial Analysis' Speech-to-Speech Quality Index at 82.6.

**How fast is it compared to older voice models?**
Google hasn't published raw latency numbers, but both models execute tool calls in the background while continuing the conversation, avoiding the "dead air" of earlier voice-agent designs.

**Is Gemini 3.8 Live available today?**
Yes — rolling out now in the Gemini API, Google AI Studio, private preview in Gemini Enterprise, and Search Live, with Extended Thinking also live in the Gemini app and parts of Workspace.

**Does it support multiple languages mid-conversation?**
Yes, automatically detecting and switching between 97 supported languages without a manual session reset.

**How is the audio watermarked?**
With SynthID, an imperceptible watermark embedded directly in the generated audio to keep AI-generated speech detectable.

## Related reading

*Figures and rollout details in this piece reflect Google's September 15, 2026 announcement. Pricing, availability by region, and benchmark standings may change as the models move out of preview.*
