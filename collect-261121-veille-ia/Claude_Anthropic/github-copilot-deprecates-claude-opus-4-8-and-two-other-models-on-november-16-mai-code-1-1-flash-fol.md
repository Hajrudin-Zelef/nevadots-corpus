---
id: collect-261121-veille-ia/claude-anthropic/github-copilot-deprecates-claude-opus-4-8-and-two-other-models-on-november-16-mai-code-1-1-flash-fol
title: "GitHub Copilot deprecates Claude Opus 4.8 and two other models on November 16; MAI-Code-1.1-Flash follows on December 18 and VS Code removes the Local harness in February 2027 - AI News"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-16"]
keywords: ["copilot", "code", "model", "harness", "claude", "flash", "opus", "november"]
source: https://vpsranking.com/news/ai/ai-2026-11-16-github-copilot-models-deprecated-local-harness/
source_anchor: ""
source_lines: [1, 89]
section: "Claude_Anthropic"
---
On November 16 and 17, 2026, GitHub announced three retirements and migrations affecting GitHub Copilot: three models deprecated as of November 16, MAI-Code-1.1-Flash deprecated on December 18, and removal of the "Local harness" used by Copilot in VS Code in February 2027.

**In short:** Claude Opus 4.8, Claude Opus 5 and GPT-5.6 Terra are already deprecated; the replacements are Claude Opus 5.5, or GPT-6.1 Sol / GPT-6 Luna. MAI-Code-1.1-Flash users should switch to the latest MAI Code Flash model by December 18. Anyone still using the Local harness in VS Code needs to move to the Copilot harness and check customization compatibility before February 2027.

## Dates and replacements for all three changes

| Item | Date | Replacement | 
|---|---|---|
| Claude Opus 4.8 | November 16, 2026 (done) | Claude Opus 5.5 | 
| Claude Opus 5 | November 16, 2026 (done) | Claude Opus 5.5 | 
| GPT-5.6 Terra | November 16, 2026 (done) | GPT-6.1 Sol or GPT-6 Luna | 
| MAI-Code-1.1-Flash | December 18, 2026 | Latest MAI Code Flash model | 
| Local harness in VS Code | Removed in February 2027 (deprecated in VS Code 1.146) | Copilot harness | 

This is separate from the November 13 notice that six other models, including Claude Fable 5, will be deprecated on December 14; the models and dates differ.

## Three models deprecated as of November 16

The deprecation covers most Copilot experiences, including Copilot Chat, inline edits, ask and agent modes, and code completions. It was announced in advance on October 15. GitHub asks users to update their workflows and integrations to use supported models.

There are two exceptions. Claude Opus 4.8 remains available to individual Copilot subscribers on annual plans. Claude Opus 5 has been removed from the model picker but remains available for "runtime refusal fallback" (a fallback when a model refuses a request at runtime) through existing authorized access. Because it is gone from the picker, it can no longer be chosen manually (a reading of the notice, not its wording).

GPT-5.6 Terra has two suggested replacements, so you choose either GPT-6.1 Sol or GPT-6 Luna yourself.

### What Business and Enterprise administrators should check

- Under default model enablement, the suggested replacements are enabled automatically for Copilot Enterprise and Copilot Business.
- If an administrator has turned off the global default or explicitly disabled a replacement model, it is not enabled automatically; enable it through its model policy in Copilot settings.
- Once enabled, the model appears in the Copilot Chat model selector in supported experiences. Existing enterprise model policies continue to apply.
- No action is required to remove the deprecated models.

## MAI-Code-1.1-Flash will be deprecated on December 18

Following the November 17 launch of MAI Code 2 Flash, GitHub announced that MAI-Code-1.1-Flash will be deprecated across all Copilot experiences on December 18. The replacement is the "latest MAI Code Flash model", and GitHub asks users to update workflows and integrations before December 18.

Copilot Enterprise administrators may need to enable the replacement through its model policy. Administrators can verify availability by checking their individual Copilot settings and confirming that the policy is enabled for that model. No action is needed to remove the older model.

## VS Code removes the Local harness in February 2027

A harness is the software that coordinates the model, context (the information passed to the task) and tools for Copilot agent sessions. VS Code 1.146 deprecates the older Local harness, which will be removed in February 2027. Its replacement, the Microsoft Copilot harness, is built into VS Code and has been the default for new sessions since VS Code 1.143.

After switching to the Copilot harness, you keep working in VS Code and agent commands still run on your own machine; your files are not moved to the cloud. During the three-month transition, the Local harness remains available alongside it where your organization's policy permits, but new features and improvements will focus on the Copilot harness.

### What the Copilot harness offers

The Copilot harness is built on the Copilot SDK and shares its coding-agent runtime with Copilot CLI and the GitHub Copilot app. While the session's Agent Host process keeps running, you can continue the same session between the editor and the Agents window in VS Code. Editor-provided tools still require a connected editor.

- Copilot Fusion chooses models and, on supported tiers, coordinates drafting and review.
- Assisted permissions reduce approval interruptions. Calls not approved by the risk assessment still come to you for confirmation.
- Copilot connectors bring context from your team's services without configuring MCP servers yourself (preview).
- Task boards for planning and interactive diagrams with canvases (preview).

If you access models through GitHub Copilot, you can keep your existing plan, including Copilot Free. Individual features and Fusion tiers depend on your plan, settings and organization policy.

### What users and administrators need to do

1. If you still use the Local harness, start a new session in VS Code and select Copilot under Session Target.
2. Review your customizations for compatibility before February 2027. Switching harnesses does not automatically convert every customization.
3. If you administer VS Code, review any vscode.temporaryDefaultToLocalHarness exception and verify your required workflows, permission controls and sandbox configuration with the Copilot harness.
4. If a workflow blocks your move, report a VS Code issue. GitHub also provides a Local harness transition guide.

Two cautions apply. The temporary enterprise-managed setting will be removed along with the Local harness, so keeping the exception configured will not keep the Local harness available. And while administrators can enforce managed runtime permissions and sandbox (isolated environment) restrictions with the Copilot harness that the Local harness cannot enforce, these are configured separately: selecting the Copilot harness does not enable sandboxing.

## Who needs to check what

If you specified Claude Opus 4.8, Claude Opus 5 or GPT-5.6 Terra, it is already time to switch to the replacements (except annual-plan individual subscribers using Opus 4.8). MAI-Code-1.1-Flash users have until December 18, and people and organizations using the Local harness in VS Code have until February 2027. Business and Enterprise administrators should check model policies for the replacements, and whoever administers VS Code should check the exception and sandbox settings.

Related official sources

- Primary source: https://github.blog/changelog/2026-11-16-github-copilot-models-deprecated-on-november-16-2026
- GitHub Changelog: MAI-Code-1.1-Flash deprecation on December 18 (Nov 17): https://github.blog/changelog/2026-11-17-upcoming-deprecation-of-mai-code-1-1-flash-in-december
- GitHub Changelog: VS Code retires the Local harness in February 2027 (Nov 16): https://github.blog/changelog/2026-11-16-vs-code-retires-the-local-harness-in-february-2027
- GitHub Changelog: separate Nov 13 notice of six models to be deprecated on Dec 14: https://github.blog/changelog/2026-11-13-upcoming-deprecation-of-selected-github-copilot-models-in-december

📚 AI Guides

### Compare and choose

### Development and agents

### By job

### Learning and basics

### Work tools and certification

### Infrastructure Guide for AI Tools

AI Agents operate via SSH, CLI, and APIs. GUI-based shared hosting is hard for AI Agents to use.
