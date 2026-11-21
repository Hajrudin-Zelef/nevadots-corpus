---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-coding-agent-setup
title: "DeepSeek V4.1 Flash Coding Agent Setup"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "model", "deepseek-flash", "anthropic", "opencode", "claude", "code", "flash"]
source: https://www.deepseekv4guide.org/guides/v4-1-flash-coding-agents
source_anchor: ""
source_lines: [1, 96]
section: "DeepSeek"
---
# DeepSeek V4.1 Flash for Coding Agents: OpenCode, Claude Code & Harness

How to run DeepSeek V4.1 Flash in Claude Code, OpenCode, OpenClaw and other coding agents: env vars, /connect, harness minimal mode and scaffold scores.

Updated Oct 17, 2026: 1017 is tuned for DeepSeek Harness v0.2.1-rc.1 or later and for Tencent CodeBuddy and WorkBuddy. See the 1017 release notes.

## The Official Integrate with AI Tools Guide

DeepSeek maintains a dedicated Integrate with AI Tools page that documents the supported ways to point an existing coding agent at deepseek-flash. Rather than a bespoke integration per tool, DeepSeek exposes two compatible endpoints and lets each tool's own provider configuration do the rest<sup>[1]</sup>.

That design is why V4.1 Flash slots into agents that were never written for DeepSeek. Claude Code, OpenCode and OpenClaw all speak one of those two protocols, so the integration is a configuration change rather than a plugin<sup>[1]</sup>.

The API model name is deepseek-flash. Both endpoints accept it, and both support tool calls, JSON output and the reasoning-effort parameter that governs agent behaviour<sup>[2]</sup><sup>[3]</sup>.

- OpenAI-compatible base_url: https://api.deepseek.com<sup>[1]</sup>
- Anthropic-compatible base_url: https://api.deepseek.com/anthropic<sup>[1]</sup>
- Model id: deepseek-flash<sup>[2]</sup>
- Official partners with first-class support: OpenCode and WorkBuddy, including CodeBuddy<sup>[3]</sup>

## Claude Code Environment Variables

For Claude Code, DeepSeek's guide sets the Anthropic base URL and then overrides each Claude model slot so it resolves to the DeepSeek model. The main slot uses deepseek-flash[1m], which requests the 1M-token context variant<sup>[1]</sup>.

Two details matter. First, the [1m] suffix is how you opt into the long-context variant; the subagent and haiku slots use the plain deepseek-flash id because they run shorter, cheaper loops<sup>[1]</sup>.

Second, CLAUDE_CODE_EFFORT_LEVEL=max pins reasoning effort for the agent. DeepSeek's benchmark table was produced at maximum effort, so this setting reproduces the conditions behind the headline agentic scores, at roughly 2.5x the output tokens of a low-effort run<sup>[1]</sup><sup>[5]</sup>.

- ANTHROPIC_BASE_URL must include /anthropic<sup>[1]</sup>
- deepseek-flash[1m] means the 1M-context variant<sup>[1]</sup>
- Subagent and haiku slots use plain deepseek-flash<sup>[1]</sup>
- CLAUDE_CODE_EFFORT_LEVEL=max matches benchmark settings<sup>[1]</sup><sup>[5]</sup>

Effort is continuous from 1 to 100. Max reproduces the published scores but costs more; 60-80 recovers most accuracy for less than half the tokens<sup>[5]</sup>.

```
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_API_KEY="<DeepSeek API Key>"
export ANTHROPIC_MODEL="deepseek-flash[1m]"
export ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-flash[1m]"
export ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-flash[1m]"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-flash"
export CLAUDE_CODE_SUBAGENT_MODEL="deepseek-flash"
export CLAUDE_CODE_EFFORT_LEVEL="max"
```
## OpenCode and the /connect Flow

OpenCode supports V4.1 Flash through its /connect command. You select DeepSeek as the provider, paste an API key, and choose DeepSeek-V4.1-Flash from the model list. DeepSeek states the flow requires OpenCode v1.14.24 or newer<sup>[1]</sup>.

Because OpenCode talks to DeepSeek's OpenAI-compatible endpoint, tools, file edits and shell execution behave exactly as they do with any other backend. The model swap does not change OpenCode's agent loop<sup>[1]</sup><sup>[3]</sup>.

The scaffold-sensitivity data is worth remembering here. In DeepSeek's own runs, OpenCode scored 65.5 on DeepSWE v1.1 and 85.0 on Terminal-Bench 2.1, below the 74.2 and 90.6 the model reached under DeepSeek Harness Minimal. The tool wrapped around the model changes the result<sup>[4]</sup>.

## OpenClaw Onboarding

OpenClaw connects through its onboarding flow. You run the install script, step through onboard, choose DeepSeek as the provider, and set the model to deepseek-flash. The installers are install.sh on macOS and Linux and install.ps1 on Windows<sup>[1]</sup>.

OpenClaw's onboarding writes the provider and model into its configuration, so once the flow completes the agent uses V4.1 Flash for every session. There is no DeepSeek-specific fork of the tool<sup>[1]</sup>.

## Why Benchmarks Use Harness Minimal Mode

DeepSeek evaluated its agentic benchmarks in the Minimal mode of DeepSeek Harness, a deliberately stripped runtime with only two tools: bash and str_replace_editor. The company positions Minimal as the fair, reproducible way to measure a model without a scaffold's extra machinery helping or hurting it<sup>[1]</sup><sup>[7]</sup><sup>[8]</sup>.

That choice explains a number that otherwise looks odd. The official DeepSWE v1.1 score of 74.2 comes from Harness Minimal, while third-party scaffolds land lower on the same model, not because the model is worse there but because the harness differs<sup>[4]</sup>.

The Harness project itself is an open-source agent runtime where everything, including the main loop, is a plugin. Minimal mode strips it to the essentials for benchmarking; Standard mode is the full coding agent, and Code mode adds a TypeScript orchestration SDK<sup>[7]</sup>.

## The Multi-Scaffold Score Table

DeepSeek published a scaffold table showing the same V4.1 Flash model under eight different coding agents at max effort, with N=8 on DeepSWE v1.1 and N=3 on Terminal-Bench 2.1, a 1M context and a 500-step cap<sup>[4]</sup>.

The gap between the best and worst scaffold is about nine points on DeepSWE and six on Terminal-Bench 2.1. For teams choosing a coding agent, that spread is often larger than the difference between two adjacent frontier models<sup>[4]</sup>.

| Scaffold | DeepSWE v1.1 | Terminal-Bench 2.1 | 
|---|---|---|
| DeepSeek Harness Minimal | 74.2 | 90.6 | 
| mini-SWE agent | 72.6 | 90.3 | 
| DeepSeek Harness Standard | 70.5 | 85.8 | 
| Claude Code | 69.8 | 88.0 | 
| DeepSeek Harness PTC | 67.6 | 85.8 | 
| Pi | 66.2 | 86.1 | 
| Codex | 65.6 | 84.1 | 
| OpenCode | 65.5 | 85.0 | 

## WorkBuddy, CodeBuddy and Official Support

DeepSeek names WorkBuddy, including CodeBuddy, and OpenCode as official partners that already support V4.1 Flash at launch. Official support means the provider is configured for deepseek-flash and the tool has validated tool calling and reasoning parsing against the model<sup>[3]</sup>.

Official support is the shortest path to a working setup, because the tool vendor has handled parser compatibility. Community integrations, by contrast, may need the deepseek_v41 tokenizer and parser modes configured manually when pointing at a self-hosted endpoint<sup>[1]</sup><sup>[5]</sup>.

## Choosing an Agent and Migration Risks

V4.1 Flash replaces the old V4-Flash line and, from September 14, 2026, absorbs deepseek-v4-pro traffic too. Any coding agent pinned to a legacy model id is now being served by V4.1 Flash behind the scenes<sup>[2]</sup><sup>[3]</sup>.

That silent reroute is the real integration risk. A scaffold tuned around V4-Pro's output style may see behaviour drift even though the endpoint string did not change. Re-run your agent regression tests rather than assuming the swap is transparent<sup>[6]</sup>.

The pragmatic selection rule is to match the scaffold to the task. Input-heavy, cacheable agent loops benefit most from the 8B prefill path and the compressed KV cache, while long-horizon autonomous work still needs careful effort tuning<sup>[5]</sup><sup>[6]</sup>.
