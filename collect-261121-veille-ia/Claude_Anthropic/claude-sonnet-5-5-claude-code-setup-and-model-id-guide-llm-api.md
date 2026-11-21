---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-claude-code-setup-and-model-id-guide-llm-api
title: "Claude Sonnet 5.5: Claude Code Setup and Model ID Guide - LLM API"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "model", "llmapi", "code", "anthropic", "base", "sonnet", "request"]
source: https://llmapi.pro/blog/claude-sonnet-5-5-api-client-setup-guide
source_anchor: ""
source_lines: [1, 92]
section: "Claude_Anthropic"
---
Use **`claude-sonnet-5-5`** as the model ID. For Claude Code through LLMAPI, set **`ANTHROPIC_BASE_URL=https://llmapi.pro`**, without `/v1`, and supply your LLMAPI key through `ANTHROPIC_AUTH_TOKEN`.

The official Claude model overview identifies Sonnet 5.5 with this ID, and the LLMAPI model catalog also lists it. Keep three checks separate: the vendor's model specifications, this site's catalog listing, and whether your own key and billing mode can complete a request.

## Configure Claude Code

The Claude Code setup guide documents the environment variables and the `claude --model` selection syntax. The examples below apply that syntax to the exact Sonnet 5.5 ID. Replace `YOUR_API_KEY` locally with your LLMAPI key, then launch Claude Code from that terminal.

### macOS or Linux

```
export ANTHROPIC_BASE_URL=https://llmapi.pro
export ANTHROPIC_AUTH_TOKEN=YOUR_API_KEY
claude --model claude-sonnet-5-5
```
### Windows PowerShell

```
$env:ANTHROPIC_BASE_URL="https://llmapi.pro"
$env:ANTHROPIC_AUTH_TOKEN="YOUR_API_KEY"
claude --model claude-sonnet-5-5
```
### Windows CMD

```
set ANTHROPIC_BASE_URL=https://llmapi.pro
set ANTHROPIC_AUTH_TOKEN=YOUR_API_KEY
claude --model claude-sonnet-5-5
```
These are configuration examples, not a report of a successful request on your account. Use the exact model ID rather than substituting an older Sonnet ID.

## Use the base URL for the client protocol

Claude Code uses the Anthropic protocol. Its LLMAPI base URL is **`https://llmapi.pro`**, not `https://llmapi.pro/v1`.

The API setup tool distinguishes three uses:

| Use | URL | 
|---|---|
| Claude Code base URL | `https://llmapi.pro` | 
| OpenAI-compatible client base URL | `https://llmapi.pro/v1` | 
| Direct Anthropic HTTP request endpoint | `https://llmapi.pro/v1/messages` | 

Do not paste the complete messages endpoint into Claude Code's base URL setting. A client constructs its request path from its protocol settings; a direct HTTP request uses the complete endpoint instead.

The setup tool checks URL formatting locally in your browser without making an API request. Use it to check the address and generate settings—not to validate credentials or prove model access.

## Official Sonnet 5.5 specifications

The official Claude model overview lists these specifications for Claude Sonnet 5.5:

| Setting | Official specification | 
|---|---|
| Claude API model ID | `claude-sonnet-5-5` | 
| Thinking | Adaptive | 
| Default effort | `high` | 
| Context window | 1M tokens | 
| Maximum output | 128K tokens | 

These are vendor specifications, not a promise that every request through your account will accept the maximum context or produce the maximum output. Check account access and applicable limits separately before relying on a large request.

For thinking behavior, the official overview specifically says that Sonnet 5.5 reports `thinking.types.between_tools.supported` as `true` and `thinking.types.disabled.supported` as `false`. It also cautions that a supported thinking setting can still be rejected for another reason, such as an incompatible effort level.

Do not transfer an entire migration checklist from another model. The Opus 5.5 overview describes breaking changes for code running on Opus 5 and explicitly extends the first three to Fable 5.1. That is not a Sonnet-specific migration checklist. Use the Sonnet-specific capability statements when deciding which settings to send.

## Distinguish the catalog from account availability

The LLMAPI catalog contains `claude-sonnet-5-5`, with `max_input_tokens` of `1000000` and `max_tokens` of `128000`. These are this site's catalog fields; keep them distinct from the official vendor specification table above.

The subscription page also lists the exact model ID. It explains that subscription models share a token-weighted usage quota, and that API billing uses a separate key and balance. For metered billing, consult the current API pricing catalog and your account's applicable console rules rather than treating official vendor prices as LLMAPI charges.

Before using Sonnet 5.5 for a task:

1. Confirm that the key belongs to the billing mode you intend to use.
2. Check your account's model access and remaining quota or balance.
3. Confirm that the configured model is exactly `claude-sonnet-5-5` .
4. Start with a small, non-sensitive request and inspect the result before attempting a large workload.

A catalog entry identifies a listed model. Do not treat the entry—or a successful local URL check—as proof that your credentials have completed a model request.

## Check connection errors without changing unrelated settings

The API setup tool gives the following troubleshooting checks:

| Response | What to check | 
|---|---|
| `401` | Check which service issued the key and whether it is active. A correct URL does not validate the credential. | 
| `404` | Compare the protocol and final request path, especially an extra or missing `/v1` . | 
| `403` or`429` | Check account access, quota and concurrency limits. Avoid repeated rapid retries. | 
| `5xx` | Retain the timestamp and request ID, then retry later or share a redacted error with support. | 

Begin with the exact model ID, the Claude Code base URL without `/v1`, and the correct account key. Use official documentation for model capabilities, LLMAPI's product pages for billing options, and your own request result to confirm access.
