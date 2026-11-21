---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-can-stop-api-tasks-during-safety-checks
title: "GPT-6 Astra can stop API tasks during safety checks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "astra", "safety", "model", "request", "says", "gpt-6", "tasks"]
source: https://blog.invidelabs.com/gpt-6-astra-api-safety-stops/
source_anchor: ""
source_lines: [1, 47]
section: "GPT_OpenAI"
---
OpenAI released GPT-6 Astra on September 3 as the first model it says meets the Critical cybersecurity threshold in its Preparedness Framework. Its production monitoring can flag legitimate work, including defensive security tasks. ChatGPT and Codex can pause for human review; a flagged API request can end with HTTP `403` and no general way to resume the conversation.

Astra is initially rolling out to enterprises in OpenAI's Trusted Access Program. Broader ChatGPT, API, and Amazon Bedrock access is due over the coming days. The API model ID is `gpt-6-astra`.

## Critical capability comes with stricter boundaries

OpenAI's Critical rating is its own assessment, not one assigned by an external body. Its safety overview says Astra can identify previously unknown vulnerabilities and develop exploits against well-protected systems when given the right tools and access.

Tested without production safeguards on the original ExploitBench, Astra scored 100% against Sol's 78.5%. The newer ExploitBench (June-August 2026) covers 20 high-severity V8 vulnerabilities across 13 stable Chrome releases. Astra achieved arbitrary code execution in 39.0% of cases against Sol's 11.5%. Some tasks may not permit arbitrary code execution under the evaluation constraints, so 100% may not be attainable. OpenAI says Astra also found and used two previously unknown zero-days. OpenAI has not named them and says it is disclosing them to maintainers.

After the Hugging Face evaluation incident, OpenAI built a test for whether a model would exceed its authorized scope. Without production safeguards, GPT-5.6 Sol did so in 48% of cases and Astra in none, according to OpenAI.

The production model will still refuse advanced requests such as creating proof-of-concept exploits. OpenAI says less restrictive Astra safeguards will become available in the coming weeks through OpenAI Daybreak, its existing Trusted Access for Cyber program. The change is intended to unlock vulnerability and proof-of-concept validation, malware analysis, and detection engineering.

## Astra safety checks change the API contract

When misalignment monitoring blocks a request before streaming begins, the GPT-6 Astra API returns HTTP `403`, error type `invalid_request_error`, and code `misalignment_policy_violation`. OpenAI says integrations should match the code rather than message text. Streaming clients also need to handle an error after output has started.

OpenAI's Daybreak troubleshooting guide describes a separate cyber-safeguard path. Depending on the surface, it can block or slow a request, return `cyber_policy`, or reroute work to a fallback model. A rerouted request may therefore complete on another model instead of producing a 403.

Responses API conversations using persisted reasoning, WebSockets, or OpenAI compaction can be stopped automatically. Requests without them are monitored and can generate webhooks, but are not automatically stopped. Chat Completions is outside this monitoring system, although other safety checks still apply. Endpoint and context choices are now also safety-routing decisions.

Applications should not automatically retry a blocked workflow. There is no general resume mechanism, and the monitoring runs asynchronously. An action may already have completed before the system detects a concern; stopping later work does not reverse earlier tool actions.

Subscribe to `safety.alert.created`, then retrieve its alert from `GET /v1/safety/alerts/{id}` with `api.safety.alerts.read` permission. `request_paused: true` means the block registered, not that execution stopped. `reason` can be null, including on ZDR requests. For a blocked defensive task, the same troubleshooting guide asks Support tickets to include the exact message, model, product surface, timestamp, request ID, and a redacted task description. Teams should not send secrets, private keys, exploit targets, or confidential third-party data unless OpenAI directs them to an approved support path.

Approved API projects do have a Daybreak path. The troubleshooting guide says an organization admin enables Daybreak Blue in Project Settings for each project, then creates a new project key or refreshes its credentials. Blue uses `gpt-daybreak-blue-latest`, mapped to `gpt-5.6-sol`; Red uses `gpt-daybreak-red-latest`, mapped to `gpt-5.6-cyber`. With approved API-key access, Codex CLI can run `codex -m gpt-daybreak-blue-latest`; the `gpt-` prefix is required.

Those controls gate Blue and Red, not reduced-refusal access to Astra today. OpenAI's Daybreak FAQ says most customers must keep Astra under standard safeguards or switch to a model supporting Daybreak Blue. Trusted Access neither removes every refusal nor includes Zero Data Retention by default. On Bedrock, the IDs are `openai.gpt-daybreak-blue-5.6-sol` and `openai.gpt-5.6-cyber`; the `-latest` aliases are unavailable.

The shape of a misalignment stop is documented. Its frequency on ordinary workloads is not. Teams should test real conversations, streaming paths, and tool side effects instead of treating a benchmark pass as production evidence.

## GPT-6 Astra pricing has a long-context cliff

The model page lists Standard pricing at $10 per million input tokens, $1 for cached input, $12.50 for cache writes, and $50 for output. Batch and Flex cost half the Standard rates. Fast mode costs twice the applicable rate.

Above 272,000 input tokens, OpenAI charges twice the input and cache rates and 1.5 times the output rate for the entire request. Astra supports a 1,050,000-token context window and 128,000 max output, so an agent can cross this cost boundary well before its technical limit. The knowledge cutoff is April 30, 2026.

## Benchmark leadership is not uniform

OpenAI's comparison table is less tidy than its broad performance claims. On the Artificial Analysis Intelligence Index v4.1.1, Astra scores 61.2, behind Claude Fable 5.1 at 65.7, Claude Opus 5 at 63.1, and Claude Fable 5 at 62.1. Its Coding Agent Index v1.4 score of 67.0 also trails Fable 5 at 68.1 and Fable 5.1 at 67.2.

OpenAI also says Astra's written reasoning was harder to monitor than GPT-5.6 Sol's in tests that asked the models to evade monitoring. It attributes the change partly to Astra using fewer written steps. That caveat matters because monitoring helps justify deployment at this capability level.

Astra's stronger stated cyber capability now comes with a documented integration contract: coverage varies by API path, a blocked workflow should not be retried, and earlier actions may survive the stop. Security teams can build for those mechanics today. They still need production testing to learn how often the controls interrupt legitimate work.

## Member discussion
