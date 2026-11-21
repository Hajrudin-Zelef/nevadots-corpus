---
id: collect-261121-veille-ia/docs-techniques/what-is-open-responses
title: "What Is Open Responses?"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["response", "responses", "item", "output", "open", "openai", "input", "tools"]
source: https://dev.to/hassann/what-is-open-responses-gjp
source_anchor: ""
source_lines: [1, 142]
section: "Docs_Techniques"
---
Open Responses is an open specification, published as an OpenAPI 3.1 document, that describes one request and response shape for calling language models across providers. It is based on the OpenAI Responses API: you send `model`, `input` and optional `tools` to `POST /v1/responses`, and you get back typed items and semantic streaming events instead of a provider-specific format. OpenAI launched Open Responses on 2026-01-15, it is licensed under Apache 2.0, and on 2026-10-22 OpenAI contributed it to the Agentic AI Foundation (AAIF), the Linux Foundation body that also hosts MCP.

That last step is the news: the spec now has a technical charter, a steering committee and a neutral home. This post covers what it defines, who implements it, what the AAIF move changes, and how to check whether an endpoint you depend on conforms. If you're new to the underlying API, start with how to use the OpenAI Responses API.

The spec describes the problem plainly: providers have converged on the same building blocks (messages, tool calls, streaming, multimodal input) but each encodes them differently. Open Responses picks one encoding and writes it down.

The current OpenAPI document is version `2026-04-24` and covers a small surface:

| Operation | What it does | 
|---|---|
| `POST /responses` (`createResponse` ) | Create a response, streaming or not | 
| WebSocket on `/responses` | Same response model over a persistent socket (added 2026-04-24) | 
| `POST /responses/compact` (`compactResponse` ) | Return a compacted input window for long conversations | 

The request body accepts the fields you'd expect if you've used OpenAI's API: `model`, `input`, `instructions`, `tools`, `tool_choice`, `stream`, `store`, `previous_response_id`, `reasoning`, `text`, `truncation`, `temperature`, `top_p`, `max_output_tokens`, `prompt_cache_key` and a few more. The response object (`ResponseResource`) carries `id`, `status`, `output`, `usage`, `error` and an echo of the settings you sent. The document defines 108 schemas in total, most of them item and streaming-event types.

Three ideas carry the design.

Everything the model reads or produces is an item: a `message`, a `function_call`, a `function_call_output`, a `reasoning` item. Items are bidirectional, so the output of one turn can be fed straight back as input to the next. Each item type is discriminated by its `type` field and has its own schema.

Items are also state machines: each moves from `in_progress` to `completed` or `incomplete` (the token budget ran out), and an `incomplete` item must be the last one emitted, with the whole response marked incomplete. A status says nothing about whether the server stored the item; persistence is a separate concern.

When you set `stream: true`, the server must answer with `Content-Type: text/event-stream` and a defined event sequence. An output item opens with `response.output_item.added`; text inside it arrives as `response.content_part.added`, then repeated `response.output_text.delta` events, then `response.output_text.done` and `response.content_part.done`; the item closes with `response.output_item.done`. The stream must end with a terminal event: `response.completed`, `response.failed` or `response.incomplete`. A trailing `[DONE]` is optional, and clients must handle a stream that ends without it.

This matters most for client code: if two providers emit the same events in the same order, one SSE parser works for both. Our SSE testing guide shows how to inspect these streams event by event.

Providers can add their own item types, tools and streaming events, but they must prefix them with a provider slug. The spec's own example is `openai:web_search_call`. A client that doesn't recognize a prefixed type can skip it without breaking. This is how the spec stays portable while hosted tools like web search or file search remain provider features.

A few more rules are worth knowing before you build on the spec:

- 
**Two kinds of tools.** Externally hosted tools (functions, MCP servers) run outside the provider; for functions, the model emits a`function_call` , you run it, and you send back a`function_call_output` with the matching`call_id` . Internally hosted tools (OpenAI's file search, for example) run inside the provider without handing control back.
- 
**`tool_choice` and `allowed_tools`.**`tool_choice` takes`auto` (the default),`required` ,`none` , or an object naming one function.`allowed_tools` restricts which tools the model may call while still showing all of them, so you don't invalidate a cached prompt by editing the`tools` array. Servers must enforce it as a hard limit.
- 
**`previous_response_id`.** The server must treat the new request as`previous_response.input + previous_response.output + input` , in that order, so you don't resend the transcript.
- 
**Reasoning items** may carry raw`content` , an opaque`encrypted_content` blob, a user-safe`summary` , or any mix.
- 
**Errors** carry a`type` (`server_error` ,`invalid_request` ,`not_found` ,`model_error` ,`too_many_requests` ), plus`code` ,`param` and`message` ; mid-stream, an error is followed by`response.failed` .

The 2026-04-24 revision (changelog) added the WebSocket transport: each turn starts with a `response.create` message, one connection processes one response at a time, connections are capped at 60 minutes (error code `websocket_connection_limit_reached`), and `store: false` continuations live in connection-local memory, failing with `previous_response_not_found` once that state is gone. The same revision added `/responses/compact`, an optional `phase` field on assistant messages (`commentary` or `final_answer`), and made `logprobs` optional.

Chat Completions is still the most widely copied LLM interface. The difference is shape: Chat Completions returns choices holding a message, with tool calls tucked inside it, while the Responses model returns a flat list of typed items. A reasoning step, three tool calls and a final message become separate, ordered objects with their own status, which suits agent loops better.

We compared the three dominant formats side by side, with real requests, in Chat Completions vs Anthropic Messages vs Responses API. If you're choosing between OpenAI's own surfaces, the Agents API vs Responses API vs Agents SDK breakdown covers where Responses sits.

The supporter list includes AWS Bedrock, Databricks, Hugging Face, LM Studio, Nvidia, Ollama, OpenRouter, Red Hat, Vercel and vLLM, alongside OpenAI. Anthropic and Google aren't among the named supporters; both still ship their own native formats.

"Supports" covers a wide range, so check the details for the runtime you use:

- 
**Ollama** added`POST /v1/responses` in v0.13.3. Its compatibility docs say "only the non-stateful flavor is supported": streaming, function tools and reasoning summaries work, but`previous_response_id` ,`conversation` and`truncation` don't. You resend history yourself.
- 
**LM Studio** made its`http://localhost:1234/v1/responses` endpoint compatible in version 0.3.39, including`previous_response_id` , remote image URLs for vision models and logprobs. See our LM Studio guide for setup.

For a sense of scale, API Evangelist took a baseline on September 13: of 120 providers claiming OpenAI compatibility that publish a readable OpenAPI document, 26 declare `POST /responses`, against 105 that declare `POST /chat/completions`. The point of the baseline is to measure whether neutral governance moves that number.

AAIF announced Open Responses on 2026-10-22 at AGNTCon + MCPCon North America in San Jose, together with three other new projects: Agent Skills (from Anthropic), OpenEnv (from Hugging Face) and OpenSandbox (from Alibaba). That brings AAIF to 10 hosted projects in its first year, next to MCP, the Agent2Agent protocol, AGENTS.md, goose, agentgateway and Agent Router. If you're sorting out how those fit together, our MCP vs A2A explainer is a good start.

What changes:

- 
**Governance.** The project is now "Open Responses a Series of LF Projects, LLC," with a technical charter adopted 2026-10-21. A Technical Steering Committee, initially the project's maintainers, owns technical decisions, and participation is open to anyone who follows the charter.
- 
**Credibility for other vendors.** A spec owned by one company is a product. A spec under a foundation is easier for a competitor to adopt without looking like it's following a rival.

What doesn't change:

- 
**OpenAI's own API.** The Responses API on`api.openai.com` keeps its own features and release schedule. Open Responses describes the portable core; OpenAI-specific tools stay behind the`openai:` prefix.
- 
**The wire format.** No new spec version came with the move. The latest release is still`2026-04-24` , and dated releases are immutable.
- 
**Conformance.** Being on the supporter list doesn't mean an endpoint passes the tests. Nobody publishes a pass/fail index yet, so you check the endpoints you depend on yourself.

The project ships its own acceptance tests. The compliance page runs 10 tests in the browser against any base URL, model and key: basic text response, streaming, system prompt, tool calling, image input, multi-turn history, assistant `phase` labels, the compaction endpoint and a negative test for a compact request missing `model`. Seven more WebSocket tests (sequential turns, `store: false` continuation, reconnect recovery, cache eviction after a failed continuation) run only from the CLI:


```
git clone https://github.com/openresponses/openresponses
cd openresponses && bun install
bun run test:compliance --base-url http://localhost:11434/v1 --api-key ollama
```
Each HTTP request and each WebSocket turn gets a 30-second deadline, and a timeout fails only that test. Expect a non-stateful runtime such as Ollama to fail the continuation tests by design.

The acceptance tests tell you whether a server follows the spec today. They don't watch the endpoint your app calls every time you deploy, upgrade Ollama or switch a provider. For that, import the spec into an Apidog project and let your coding agent write and run regression tests against it with Apidog CLI.

Install the CLI and the agent skill once:


```
npm install -g apidog-cli@latest
apidog auth login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick your agent when prompted
```
Then give Claude Code, Codex or Cursor one [REDACTED PROMPT]

Download https://www.openresponses.org/openapi/openapi.json and import it into

Apidog project . Create an environment "local-ollama" with base URL

http://localhost:11434/v1. Add test cases for POST /responses: one that expects 200,

status "completed" and an output item of type "message"; one with stream: true that

expects text/event-stream ending in response.completed; one without "model" that

expects a 4xx error with an error.type field. Run them and summarize any failures.


```
The agent reads the schemas, validates its payloads before writing anything, and runs the cases:
```

bash

curl -sLO https://www.openresponses.org/openapi/openapi.json

apidog import --project  --format openapi --file ./openapi.json

apidog environment create local-ollama --project  --base-url http://localhost:11434/v1

apidog cli-schema get test-case-create

apidog cli-schema validate test-case-create --file case.json

apidog test-case create --project  --file case.json

apidog run --test-case  --project  -e  -r cli,json

```
The spec's `servers` entry points at `https://api.openai.com/v1`, so add one environment per provider and run the same cases against each: that's the portability claim, tested. Put the `apidog run ... -r junit` line in your pipeline so a runtime upgrade that drops a field fails the build ([CI/CD guide](http://apidog.com/blog/apidog-cli-ci-cd-pipeline?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation)); [Apidog CLI in Claude Code](http://apidog.com/blog/apidog-cli-in-claude-code?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) covers the agent setup in detail.
## FAQ
**Is Open Responses the same as the OpenAI Responses API?** No. Open Responses is a spec derived from it. OpenAI's Responses API is one implementation with extra OpenAI-only features; Ollama, LM Studio and vLLM are others.
**Who owns Open Responses now?** Since 2026-10-22 it is an Agentic AI Foundation project under the Linux Foundation, run as an LF Projects series with a Technical Steering Committee. It is licensed under Apache 2.0.
**Does Anthropic support Open Responses?** Anthropic isn't among the named supporters. It contributed Agent Skills to AAIF the same day, but Claude's native interface is still the Messages API.
**Where is the spec file?** At `https://www.openresponses.org/openapi/openapi.json`, with dated releases in the [GitHub repository](https://github.com/openresponses/openresponses).
## Next step
Pick the one endpoint your product calls most, run the browser acceptance tests against it, and import the spec into Apidog so your agent can keep those checks running on every deploy. [Download Apidog](https://apidog.com/download/?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) to get started.
```
## Top comments (0)
