---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-combines-million-token-context-with-flat-agent-pricing
title: "MiniMax M3.1 Flash Combines Million-Token Context With Flat Agent Pricing"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "tokens", "flash", "million-token", "context", "input", "output", "flat"]
source: https://medium.com/@aiindustrytoday/minimax-m3-1-flash-combines-million-token-context-with-flat-agent-pricing-27cca2f083a4
source_anchor: ""
source_lines: [1, 17]
section: "MiniMax"
---
# MiniMax M3.1 Flash Combines Million-Token Context With Flat Agent Pricing

MiniMax launched M3.1 Flash on October 19, pairing a million-token context window and adjustable reasoning with flat rates of $0.15 per million input tokens and $0.60 per million output tokens.

The launch announcement lists cached input at $0.015 per million tokens. MiniMax claims agent-task costs are 70% below its predecessor’s and median output speed exceeds 170 tokens per second.

MiniMax’s API documentation specifies that the million-token limit covers input and output combined. The same context limit applies to MiniMax M3, so the new model does not expand that capacity. Its distinguishing control is adjustable thinking depth, allowing developers to select how much reasoning to request.

The integration guide lists five effort settings, from low to max, with max as the default. Reasoning cannot be switched off. The interface supports text, image and video input alongside tool-use and tool-result messages. For multi-turn tool calls, developers must retain the full assistant response in conversation history to preserve reasoning continuity. Thinking tokens also count toward the output limit, a practical constraint for jobs with tightly capped responses.

MiniMax reports 73.7% on DeepSWE v1.1 and 61.7 points on OSWorld V2, covering software-development and computer-operation tasks. These are company-reported results, not independently verified performance across customer workflows.

The price advantage depends on the competing workload. Anthropic’s October 7 Haiku 5.5 announcement lists input/output rates of $0.10/$0.50 per million tokens for prompts up to 100,000 tokens, rising to $0.50/$2.50 above that threshold.

MiniMax’s input and output rates are therefore 50% and 20% higher than Haiku’s short-prompt tier, but 70% and 76% lower than its longer-prompt tier. These are rate comparisons, not equivalent-task cost or performance results: the prompt threshold applies to each request, and token consumption can differ between models.

M3.1 Flash is available through the MiniMax API, M Plan and MiniMax Code. MiniMax says model weights will follow, but its October 19 announcement gives no release date.
