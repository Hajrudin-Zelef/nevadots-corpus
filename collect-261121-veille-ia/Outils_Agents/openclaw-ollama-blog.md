---
id: collect-261121-veille-ia/outils-agents/openclaw-ollama-blog
title: "OpenClaw · Ollama Blog"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["openclaw", "ollama", "models", "model", "gpt-oss", "coding", "launch", "tasks"]
source: https://ollama.com/blog/openclaw
source_anchor: ""
source_lines: [1, 69]
section: "Outils_Agents"
---
# OpenClaw

## February 1, 2026

OpenClaw is a personal AI assistant that bridges your favorite messaging platforms to AI coding agents through a centralized gateway. It runs locally on your own devices, keeping your conversations and code private.

OpenClaw integrates with WhatsApp, Telegram, Slack, Discord, iMessage, and other messaging services, allowing you to interact with AI coding agents from anywhere.

## Get started

Start by installing OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
```
**Windows**

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```
## Running with Ollama

Once installed, you can launch OpenClaw directly with Ollama to connect local/cloud models:

```
ollama launch openclaw
```
If you want to configure OpenClaw without immediately starting the service:

```
ollama launch openclaw --config
```
The gateway will auto-reload if it’s already running.

##

## Recommended models

OpenClaw requires a larger context length to complete tasks. It is recommended to use a context length of at least 64k tokens.

Here are some models that work well with OpenClaw:

| Model | Description | 
|---|---|
| qwen3-coder | Optimized for coding tasks | 
| glm-4.7 | Strong general-purpose model | 
| glm-4.7-flash | Balanced performance and speed | 
| gpt-oss:20b | Balanced performance and speed | 
| gpt-oss:120b | Improved capability | 

Ollama’s cloud models are available for free to start. Search for models on ollama.com.

| Model | Description | 
|---|---|
| kimi-k2.5 | Powerful 1T parameter model for agentic tasks | 
| minimax-m2.1 | Excellent multilingual capabilities | 
| glm-4.7 | Strong general-purpose model | 
| gpt-oss:20b | Balanced performance and speed | 
| gpt-oss:120b | Improved capability | 

Please see more models on ollama.com.

## Example screen captures

Connecting to WhatsApp

## Note

OpenClaw was previously known as Clawdbot and Moltbot. The `clawdbot` command still works as an alias.
