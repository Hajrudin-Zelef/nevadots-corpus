---
id: collect-261121-veille-ia/gpt-openai/open-responses-wants-to-be-the-common-language-of-ai-models-and-openai-wrote-the-dictionary
title: "Open Responses wants to be the common language of AI models, and OpenAI wrote the dictionary"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-01-15"]
keywords: ["responses", "open", "openai", "language", "code", "models", "model", "back"]
source: https://www.i-scoop.eu/open-responses/
source_anchor: ""
source_lines: [1, 102]
section: "GPT_OpenAI"
---
OpenAI has handed Open Responses to the Agentic AI Foundation, the Linux Foundation body that also looks after MCP. Open Responses is an open specification for how software talks to a language model. It defines how you send a request, how tool calls and reasoning come back and how the answer streams in. Write your code in that format once and you can switch models or providers without rewriting your integration. There is a catch. The standard starts from OpenAI’s own API, and Anthropic and Google are not on board for now.

## What is Open Responses?

Open Responses is a specification, not a product and not a model. It describes, in an OpenAPI schema, what a request to a language model looks like and what comes back. OpenAI launched it on 15 January 2026, with OpenRouter, Hugging Face, Vercel, Ollama, LM Studio and vLLM among the first partners. It is based on the Responses API, OpenAI’s newer API, which has more agent features than the older Chat Completions.

Three building blocks set it apart from the older approach:

- **Items.** A response is no longer a single block of text but a sequence of items, such as a message, a function call or a reasoning step. Every item has an id, a type and a status that shows whether it is still in progress, incomplete or completed.
- **Semantic streaming.** Instead of loose chunks of text you get named events, such as`response.output_text.delta` for new text and`response.completed` when everything is done. Every stream ends with a final state, so your app knows whether it succeeded or failed.
- **Tools in two places.** You run a function tool yourself in your own app, and MCP tools fall in the same group. A tool the provider hosts, such as web search, runs at the provider, which sends the result straight back.

Reasoning gets its own item with three optional fields: the raw reasoning, a summary for the user and an encrypted version that you pass back to the provider unread. Providers may add their own features, but only with their name as a prefix, as in `openai:web_search_call`. An app that does not know such an extension must be able to ignore it. It is all in the specification. The code is licensed under Apache 2.0 and the documentation under CC BY 4.0.

## Why a common API is needed

Almost every language model provider offers the same building blocks: messages, tool calls, function calling and images as input. Each one just writes them down differently. If you want to switch models, you have to translate those differences every time, or use a library that does it for you.

In 2026 there are three big dialects. OpenAI’s format, first Chat Completions and now Responses, became the template many others copied. Anthropic’s Messages API gained ground on the back of Claude Code. DeepSeek, Moonshot, Z.ai and MiniMax now offer endpoints that mimic Anthropic’s API. DeepSeek, for example, lets you run Claude Code on a DeepSeek model. Google made its own Interactions API generally available in June 2026 and recommends it for new Gemini projects.

Every translation layer is code you have to maintain, and it breaks when a provider changes something. That makes switching more expensive than it needs to be. It is one form of the lock-in we described in why models are rented but your context is owned.

## What changes now the Agentic AI Foundation runs it?

The Agentic AI Foundation (AAIF) started in December 2025 with three projects: Anthropic’s MCP, Block’s goose and OpenAI’s AGENTS.md. On 22 October 2026, at AGNTCon + MCPCon in San Jose, it announced four new projects, which brings the count to ten:

- Open Responses from OpenAI;
- Agent Skills from Anthropic, the format for skills an agent only loads when it needs them;
- OpenEnv from Hugging Face, for building environments in which agents train with reinforcement learning;
- OpenSandbox from Alibaba, a platform in which agents run code safely.

For Open Responses the move means OpenAI no longer decides alone. A technical steering committee takes the technical decisions. At first it consists of the project’s maintainers, and its meetings are meant to be public. Contributors keep their copyright, according to the governance page. An OpenAI spokesperson told The New Stack that developers get a shared format for working with models this way, with fewer provider-specific integrations to maintain. The foundation wants to be the same neutral home for Open Responses that it already is for MCP, the protocol that connects agents to tools and data.

## Who already supports Open Responses?

The project site lists NVIDIA, Vercel, OpenRouter, Hugging Face, LM Studio, Databricks, Red Hat, AWS, Ollama, OpenAI, vLLM and Llama Stack as supporters. Four implementations work today, each with its own limits:

| Who | What you get | Watch out | 
|---|---|---|
| OpenRouter | One endpoint for models from many providers | Stateless: you send the whole conversation every time | 
| Vercel AI Gateway | Since 15 January 2026 you switch provider by changing the model name, with fallback to another provider | You pay and work through Vercel as an intermediary | 
| Databricks Model Serving | One endpoint for Claude, Gemini and open models | Stateless, and for Claude and Gemini Databricks halves the temperature, because their own range runs from 0 to 1 | 
| Ollama | Since version 0.13.3, also on your own computer | Stateless only | 

Notably, you can already reach Claude and Gemini in the Open Responses format through Databricks, Vercel or OpenRouter. An intermediary translates for you. Going directly to Anthropic or Google, you cannot.

## Why Anthropic and Google are not on board yet

Anthropic is not on the list of supporters and sticks to its Messages API. Yet Anthropic is a member of the AAIF and donated Agent Skills to the same foundation on the same day. Google has its own standard in the Interactions API, which it only just made generally available. For both it is a strategic choice. Others already copy their own APIs, and Open Responses starts from the format of their biggest competitor.

There is a precedent in the other direction. MCP came from Anthropic, and still OpenAI adopted the protocol in March 2025. Developer Sam Witteveen expects, according to InfoQ, that open models from labs such as Qwen, Kimi and DeepSeek will support both Open Responses and Anthropic’s API. Then the question is no longer which standard wins, but whether two remain instead of three. Voice is already moving that way: ElevenLabs built Eleven Realtime on OpenAI’s protocol so voice agents are easier to switch.

## Where it chafes

The name says open, but the origin is plain. According to the contributor guide, the core schema files were copied from OpenAI’s API. Developer Rituraj Pramanik told InfoQ that building an open standard on OpenAI’s API was “slightly ironic, but practical”. He sees fragmentation as the bigger problem. On OpenAI’s developer forum the tone was sharper. One user argued that the specification simply inherits OpenAI’s limitations, such as the missing `logit_bias`.

More important for anyone building with it, the standard leaves a lot open. For reasoning, providers may support any combination of the three fields. Nils Durner therefore advises checking every implementation yourself. The differences are already visible:

- OpenRouter, Ollama and Databricks are stateless, while the specification describes `previous_response_id` to continue a conversation without resending everything.
- If you use Gemini through Databricks, you have to send back every function call with its encrypted content unchanged. Otherwise the next request is rejected.
- Structured output for Claude through Databricks does not work together with streaming or reasoning.

There are acceptance tests. In your browser you can test an endpoint on ten points, from a plain text answer to streaming, tool calls, image input and multi-turn conversations. Seven extra tests for the WebSocket connection run from the command line. Tests for the client side are still missing, something Simon Willison already asked for in January.

## What you can do with it today

**If you build with AI models yourself,** Open Responses is mainly a direction. For anything new, choose the Responses format over Chat Completions, because that is where OpenAI, the open source tools and now the AAIF are heading. Keep the conversation in your own app, because most implementations make you send the whole conversation every time anyway. Avoid prefixed extensions if you want to be able to switch, and test your provider with the acceptance tests before you move. If you want agents that are not tied to one model at all, a pluggable runtime such as DeepSeek Harness follows the same logic one level up.

OpenAI’s official Python library works with any provider that speaks the format. This example talks to a model in Ollama on your own computer. For OpenRouter or Vercel you only change the address, the key and the model name.

```
from openai import OpenAI
client = OpenAI(base_url="http://localhost:11434/v1/", api_key="ollama")
response = client.responses.create(
    model="qwen3:8b",
    input="Explain in two sentences what an API is.",
)
print(response.output_text)
```
**If you decide on AI in your organisation,** ask your vendors whether their platform supports Open Responses. It lowers the cost of switching models, but it does not remove it. Your prompts, your evaluations and your expectations are often tuned to one model. For European companies that want to keep the option of moving to a European or self-hosted model later, a common interface is a cheap insurance policy, as long as you check what each provider actually implements.

## Frequently asked questions about Open Responses

### Is Open Responses the same as OpenAI’s Responses API?

No. Open Responses is an open specification based on the Responses API. An API is compliant if it implements the specification or a superset of it. OpenAI’s API can therefore do more, such as its own built-in tools.

### Does Open Responses cost anything?

No. The specification is free and open, under the Apache 2.0 licence. You only pay your provider for using the model.

### Can I use Claude or Gemini through Open Responses?

Yes, through an intermediary such as OpenRouter, Vercel AI Gateway or Databricks. Not directly at Anthropic or Google.

### What is the difference between Open Responses and MCP?

MCP connects an agent to tools and data sources. Open Responses governs how your app talks to the model itself. They complement each other, and the specification counts MCP among the tools that run outside the provider.

## The real test comes from Anthropic and Google

A standard only becomes a standard when your competitor speaks it. MCP became one the day OpenAI adopted it. Open Responses now has a neutral home, a steering committee and a long list of supporters, but the two players that make the difference speak their own language. Until they join, Open Responses is the common language of everyone except Anthropic and Google. That is less than the name promises, but a lot more than existed a year ago.
