---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-api-specs-playground-pricing-puter-developer
title: "Claude Haiku 5.5 - API, Specs, Playground & Pricing - Puter Developer"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["anthropic", "claude", "tokens", "haiku", "puter", "output", "million", "model"]
source: https://developer.puter.com/ai/anthropic/claude-haiku-5-5/
source_anchor: ""
source_lines: [1, 122]
section: "Claude_Anthropic"
---
# Anthropic: Claude Haiku 5.5

`anthropic/claude-haiku-5-5`
                            
                        Try Claude Haiku 5.5 for free in your browser, and add it to your app for free with Puter.js AI API.

Try it free Add to your app```
// npm install @heyputer/puter.js
import { puter } from '@heyputer/puter.js';
puter.ai.chat("Explain quantum computing in simple terms", {
    model: "anthropic/claude-haiku-5-5"
}).then(response => {
    document.body.innerHTML = response.message.content;
});
```
```
<html>
<body>
    <script src="https://js.puter.com/v2/"></script>
    <script>
        puter.ai.chat("Explain quantum computing in simple terms", {
            model: "anthropic/claude-haiku-5-5"
        }).then(response => {
            document.body.innerHTML = response.message.content;
        });
    </script>
</body>
</html>
```
## Model Card

Claude Haiku 5.5 is the small, low-cost model in Anthropic's 5.5 family, released on October 7, 2026 and available through the API as anthropic/claude-haiku-5-5. It has a 1M token context window and up to 128k output tokens. Pricing is $0.10 per million input tokens and $0.50 per million output tokens for requests under 100,000 tokens, down from $1 and $5 for Haiku 4.5, and $0.50 and $2.50 above that. Effort levels range from low to max, with medium as the default.

Anthropic reports 72.4% on OSWorld 2.1 (offline subset, Haiku 4.5: 15.7%) and 39.2% on Terminal-Bench 4.0 at maximum effort (Haiku 4.5: 0.0%). On the same tests GPT-6 Luna scores 48.9% and 16.4%.

It is aimed at summarizing documents, classifying information, querying databases, and handling subtasks for larger agents. Anthropic recommends Opus and Sonnet for demanding coding work.

Context Window 1M

tokens

Max Output 128K

tokens

Input Cost $0.1

per million tokens

Output Cost $0.5

per million tokens

Input text, image, pdf

modalities

Tool Use Yes


Knowledge Cutoff Jun 2026


Release Date Oct 7, 2026


Output Speed 226

tokens / sec

Latency 304.78s

time to first token

## Benchmarks

How Claude Haiku 5.5 performs on standard evaluations.

### Claude Fable 5.2

Claude Fable 5.2 is Anthropic's update to Claude Fable 5.1, released on November 9, 2026. It is the publicly available model of a pair, alongside Claude Mythos 5.2, which has looser cybersecurity and biology safeguards and is limited to vetted organizations. Anthropic says it produces output up to 30% faster than the 5.1 versions. Anthropic reports 49.1% on FrontierCode 2.0 (Fable 5.1: 32.4%), 74.6% on Terminal-Bench-Science 0.1 (52.6%), and 89.3% on OSWorld 2.1 (83.5%). Pricing is $8 per million input tokens and $40 per million output tokens, down from $10 and $50 for Fable 5.1. It targets large, loosely defined coding tasks, multi-day agent work, and scientific research. Anthropic recommends Opus 5.5 for most coding and knowledge tasks.

Chat
### Claude Sonnet 5.5

Claude Sonnet 5.5 is a mid-tier model from Anthropic, released on September 28, 2026 and available through the API as anthropic/claude-sonnet-5-5. It accepts text, image, and PDF input, with a 1M token context window and up to 128k output tokens. Pricing is $2 per million input tokens and $10 per million output tokens, the same as Claude Sonnet 5. Anthropic says it generates output more than 30% faster than Sonnet 5 and can cut the cost of a task by up to 30%, because it uses fewer tokens and fewer tool calls. Anthropic reports 70.6% on Terminal-Bench 4.0 (Sonnet 5: 10.3%) and 80.1% on OSWorld 2.1 (Sonnet 5: 57.0%). It is aimed at well-scoped everyday tasks, bug fixing, agentic coding, and producing documents, slides, and spreadsheets.

Chat
### Claude Opus 5.5

Claude Opus 5.5 is Anthropic's newest flagship model, released September 22, 2026 as the successor to Claude Opus 5. Anthropic positions it for agentic coding and long-running autonomous work. Reported benchmark scores include 66.4% on Terminal-Bench 4.0 (up from 52.3% for Opus 5), 54.4% on FrontierCode, 40% on AutomationBench (up from 26.9%), 81.8% on the OSWorld 2.0 computer-use benchmark, and 67.7% on Humanity's Last Exam with tools. It runs about 30% faster than Opus 5 and costs roughly 40% less on typical workloads, at $4 per million input tokens and $20 per million output tokens, a 1M-token context window, and 128K max output tokens. That combination of long-horizon coding performance, multimodal input (text, image, PDF), tool calling, and lower cost makes it a fit for developers building coding agents, codebase-wide migrations, and research or analysis pipelines through the API.

## Frequently Asked Questions

You can access Claude Haiku 5.5 by Anthropic through Puter.js AI API. Include the library in your web app or Node.js project and start making calls with just a few lines of JavaScript â no backend and no configuration required. You can also use it with Python or cURL via Puter's OpenAI-compatible API.

Claude Haiku 5.5 is free to try with a Puter account. Every account includes a free AI allowance, and you can chat with it in the playground on this page. You can upgrade your account anytime for a larger allowance.

|  | Price per 1M tokens | 
|---|---|
| Input | $0.1 | 
| Output | $0.5 | 

Claude Haiku 5.5 was created by Anthropic and released on Oct 7, 2026.

Claude Haiku 5.5 supports a context window of 1M tokens. For reference, that is roughly equivalent to 2,000 pages of text.

Claude Haiku 5.5 can generate up to 128K tokens in a single response.

Claude Haiku 5.5 has a knowledge cutoff date of Jun 2026. This means the model was trained on data available up to that date.

Claude Haiku 5.5 accepts the following input types: text, image, pdf. It produces: text.

Yes, Claude Haiku 5.5 supports tool use (function calling), allowing it to interact with external tools, APIs, and data sources as part of its response flow.

Claude Haiku 5.5 scores 36.5 on the Artificial Analysis Intelligence Index, outperforming 61% of tracked models.

## Add Claude Haiku 5.5 to your app for free

Developers can integrate Claude Haiku 5.5 for free using the Puter.js AI API. 

 With the User-Pays Model, each user covers their own AI usage instead of the developer.
