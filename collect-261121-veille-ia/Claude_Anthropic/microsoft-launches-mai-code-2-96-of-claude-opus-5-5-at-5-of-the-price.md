---
id: collect-261121-veille-ia/claude-anthropic/microsoft-launches-mai-code-2-96-of-claude-opus-5-5-at-5-of-the-price
title: "Microsoft Launches MAI Code 2: 96% of Claude Opus 5.5 at 5% of the Price"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-17"]
keywords: ["code", "microsoft", "tokens", "coding", "models", "price", "billion", "million"]
source: https://chainaiwire.com/microsoft-mai-code-2-specs-pricing-benchmarks/
source_anchor: ""
source_lines: [1, 82]
section: "Claude_Anthropic"
---
**At a glance:** Microsoft launched MAI Code 2 and MAI Code 2 Flash on 17 November 2026 at Ignite, its first in-house coding models built from scratch. Code 2 carries about 2.8 trillion total parameters with 77 billion active per token, reads a million tokens of context, and costs $0.50 per million input tokens and $2.50 per million output tokens at launch pricing in GitHub Copilot. Microsoft says it delivers 98% of GPT-6.1 Sol’s coding quality at about 40% of the cost, and 96% of Claude Opus 5.5 at 5% of the cost. Those figures come from Microsoft’s own evaluation, and no independent lab has reproduced them yet.

Microsoft used the opening day of Ignite in San Francisco to make its clearest move yet against the AI labs it has long relied on. Its Microsoft AI division released seven home-grown models in one go, and the coding pair is the headline: MAI Code 2, a large model for deep debugging and repository-wide work, and MAI Code 2 Flash, a smaller model for fast, high-volume coding. Both are generally available in GitHub Copilot from launch day. The timing was pointed. OpenAI shipped GPT-6.2 Astra the same day, and Microsoft’s pitch is that its own models now do nearly the same work for a fraction of the price.

## Two models, one training recipe

Both models were pre-trained and post-trained by Microsoft from scratch, on the same sparse mixture-of-experts transformer design with an integrated vision encoder, and they differ mainly in size. MAI Code 2 has 77 billion active parameters, 3 billion embedded, and roughly 2.8 trillion total, pre-trained on about 50 trillion tokens with a further 150 billion tokens of long-context extension. MAI Code 2 Flash has 12 billion active and about 398 billion total parameters, trained on roughly 25 trillion tokens using logit distillation from a larger in-house teacher model. Both read up to 1,050,000 tokens of context, accept text and image input, and return up to 128,000 output tokens. Training ran from August to November 2026, and the knowledge cut-off is August 2026.

The post-training is where Microsoft tried to close the gap with the frontier labs. Reinforcement learning ran directly inside the GitHub Copilot CLI and VS Code agent harnesses used in production, so improvements measured offline should carry over to real developer workflows. Reasoning effort is configurable from low to max, trading latency and cost for quality, and the models handle multi-file edits, debugging, refactoring, test-driven verification and long terminal sessions. They can also work from screenshots and UI mockups, turning an image into working front-end code.

## Benchmarks: Microsoft’s own numbers

Microsoft ran both models on public agentic coding benchmarks through its MSBench platform, using the GitHub Copilot CLI as the harness and maximum reasoning effort. The results, from Microsoft’s Ignite announcement and the official model card, are Microsoft’s own, and no outside lab has yet published independent scores.

| Benchmark | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| DeepSWE 1.1 | 73.2% | 59.9% | 
| TerminalBench 2.1 | 84.6% | 78.3% | 
| SWE Atlas | 43.1% | 44.8% | 

Averaged across four benchmarks, DeepSWE 1.1, TerminalBench 2.1, SWE Atlas and FrontierCode v1.1, Microsoft says Code 2 reaches 98% of GPT-6.1 Sol’s coding quality at about 40% of the cost, and 96% of Claude Opus 5.5 at 5% of the cost. Flash, it says, matches GPT-6 Luna on three coding benchmarks at 65% of the cost, and Claude Haiku 5.5 at 20%. Microsoft also says developers in a blind test preferred Code 2’s front-end output to GPT-6 Sol’s 64% of the time. All of these comparisons were calculated at the launch promotional price, which halves the per-token rates, and Microsoft has not published a comparison at the standard price.

## Pricing compared

The price is the point of the launch. In GitHub Copilot, MAI Code 2 costs $0.50 per million input tokens, $0.025 per million cached input tokens, and $2.50 per million output tokens until 17 May 2027, when the rates double to $1.00, $0.05 and $5.00. Code 2 Flash is listed at $0.05 input and $0.25 output, also doubling in May. Even at the doubled standard rates, Code 2 undercuts the $8 / $40 charged by GPT-6.2 Astra and Claude Fable 5.2 by a wide margin, and it is cheaper than Gemini 4 Neon at $3 / $15.

| Model | Input / 1M tokens | Output / 1M tokens | Context window | Active parameters | 
|---|---|---|---|---|
| MAI Code 2 | $0.50* | $2.50* | 1M tokens | 77B | 
| MAI Code 2 Flash | $0.05* | $0.25* | 1M tokens | 12B | 
| GPT-6.2 Astra | $8 | $40 | Not disclosed | Not disclosed | 
| Claude Fable 5.2 | $8 | $40 | Not disclosed | Not disclosed | 
| Gemini 4 Neon | $3 | $15 | 1M tokens | Not disclosed | 
| Qwen4-Max | $1.50 | $4.50 | 1M tokens | 71B | 

* Promotional price in GitHub Copilot until 17 May 2027; rates double after that date.

## Where you can use it

MAI Code 2 is available in GitHub Copilot on the Pro, Pro+, Max, Business and Enterprise plans, in the model picker and through Copilot Fusion, across VS Code, Visual Studio, the Copilot CLI, the cloud agent, the Copilot app, github.com, GitHub Mobile, JetBrains, Xcode and Eclipse. Code 2 Flash covers those plans plus Free and Student, and it is also in limited-access preview in Microsoft Foundry, callable through an OpenAI-compatible Responses API. The larger Code 2 model has no Foundry deployment at launch. Microsoft named OpenRouter, Vercel, Hermes, OpenCode and Kilo as future distribution channels, with no date given.

One date is worth marking. GitHub announced that the previous generation MAI-Code-1.1-Flash will be retired from all Copilot experiences on 18 December 2026. Anyone whose workflow still points at it has a month to move.

## What it is not built for

Microsoft draws a clear boundary around the launch. Both models are built for everyday enterprise coding, and the company says they are explicitly not yet tuned for harder work such as fundamental science research and very long-horizon agentic tasks. At release the models are optimised for English code production only, and the model card notes that performance outside English and on less common languages and frameworks has not been extensively evaluated. The weights are not distributed; both models are offered only as hosted services on Azure, and Microsoft says the models are not intended for fully autonomous systems that take consequential actions without human oversight.

Safety gets a full section in the 24-page model card. Microsoft runs a two-tier frontier biosafety and cybersafety system over deployments, and rates both models High on CBRN and offensive cyber risk, in line with other similar models such as GPT-5.6 Luna and GPT-5.6 Sol. On AgentDojo, which tests resistance to instructions planted in tool output, the attack success rate was 0.02% for Code 2 and 0.00% for Flash.

## Seven models, and a bigger shift

Code 2 was not the only launch. The Ignite line-up also included MAI Cyber 2 Flash for defensive security work, which Microsoft says scores 92.3% on a cybersecurity benchmark at under 2% of the cost per task of a leading frontier model; MAI Image 2.7 for image generation and editing; MAI Realtime 1 for live speech-to-speech; Dragon Copilot Transcribe, built on MAI Transcribe 2, for medical transcription; and Decision 1.1, a router model Microsoft says is 50 times faster and 79 times cheaper than GPT-6.1 Sol. Microsoft also announced MAI Licensed Weights, giving trusted customers access to Microsoft AI model weights through its Frontier Co. programme.

The contrast with the week’s other big release is sharp. Alibaba’s Qwen4-Max went for raw scale at 2.4 trillion parameters, while K2-Horizon went fully open with training data and checkpoints. Microsoft went for price and distribution instead: models that are good enough for the coding work developers do every day, priced so teams can run agents around the clock, and shipped straight into the tool developers already use. Whether the benchmark claims hold up outside Microsoft’s own harness is the question the next few weeks will answer.

## What to watch

- **Independent benchmarks:** whether outside labs reproduce the 96% and 98% figures, and at what reasoning effort.
- **The May price rise:** whether teams stay on Code 2 when the promotional rates double on 17 May 2027.
- **Third-party availability:** when Code 2 reaches OpenRouter, Vercel and the other named channels, and at what price.
- **The 18 December retirement:** the MAI-Code-1.1-Flash switch-off in Copilot, and whether Flash users notice the change.
- **Rival pricing:** whether OpenAI, Anthropic or Google answer a coding model that costs a twentieth of their flagships.

### What is MAI Code 2?

Microsoft’s new flagship agentic coding model, launched 17 November 2026 at Ignite. It is a mixture-of-experts model with about 2.8 trillion total parameters and 77 billion active per token, a one-million-token context window, and configurable reasoning effort. It is built for everyday enterprise coding: taking code to production, deep debugging, multi-file implementation and repository-wide reasoning.

### How much does MAI Code 2 cost?

In GitHub Copilot, MAI Code 2 costs $0.50 per million input tokens, $0.025 per million cached input tokens, and $2.50 per million output tokens at launch pricing until 17 May 2027, when the rates double to $1.00, $0.05 and $5.00. MAI Code 2 Flash costs $0.05 input and $0.25 output, also doubling in May.

### How does MAI Code 2 compare with Claude Opus 5.5 and GPT-6.1 Sol?

Microsoft says that, averaged across four coding benchmarks, MAI Code 2 delivers 98% of GPT-6.1 Sol’s coding quality at about 40% of the cost, and 96% of Claude Opus 5.5 at 5% of the cost. These are Microsoft’s own figures from its MSBench evaluation, calculated at the promotional price, and no independent lab has reproduced them yet.

### Where can I use MAI Code 2?

In GitHub Copilot, on Pro, Pro+, Max, Business and Enterprise plans, in the model picker and through Copilot Fusion, across VS Code, Visual Studio, the Copilot CLI, the cloud agent, the Copilot app, github.com, GitHub Mobile, JetBrains, Xcode and Eclipse. MAI Code 2 Flash is also available on Free and Student plans, and in limited-access preview in Microsoft Foundry.

### What is MAI Code 2 Flash?

The smaller, faster model in the same family, with 12 billion active and about 398 billion total parameters. Microsoft positions it for high-volume work such as code explanations, targeted edits, routine fixes and rapid iterations. It costs $0.05 input and $0.25 output per million tokens at launch pricing.
