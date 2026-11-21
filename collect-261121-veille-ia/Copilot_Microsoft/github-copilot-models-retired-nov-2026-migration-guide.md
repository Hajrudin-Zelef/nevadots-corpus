---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-models-retired-nov-2026-migration-guide
title: "GitHub Copilot Models Retired Nov 2026: Migration Guide"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "model", "github", "opus", "evidence", "technical", "models", "claude"]
source: https://sxf.si/signals/github-copilot-models-retired-november-2026/
source_anchor: ""
source_lines: [1, 43]
section: "Copilot_Microsoft"
---
SXF RESEARCH EXPLAINER

## What the technical evidence means.

TECHNICAL ANALYSIS / 01

## Which GitHub Copilot models were deprecated on November 16?

GitHub identifies Claude Opus 4.8, Claude Opus 5 and GPT-5.6 Terra as deprecated across most Copilot experiences from November 16, 2026. The affected workflows include Copilot Chat, inline edits, ask and agent modes, and code completions. This is a model-availability change, not an announcement that GitHub Copilot itself is discontinued.

For a developer whose model picker changed unexpectedly, first identify the exact old identifier, Copilot plan, subscription type and organization model policy. In particular, the announcement expressly keeps Claude Opus 4.8 for eligible annual individual subscribers, while Claude Opus 5 may still participate in an authorized refusal fallback even after disappearing from normal selection.

Check primary-source evidence â

TECHNICAL ANALYSIS / 02

## Which replacements should developers select?

GitHub names Claude Opus 5.5 as the alternative for both Opus 4.8 and Opus 5. For GPT-5.6 Terra it lists GPT-6.1 Sol or GPT-6 Luna. These are suggested alternatives, not evidence that old and new models have identical instruction following, security behavior, cost or latency.

A controlled migration compares completion correctness, tests passed, accepted pull requests and human editing time for the same representative tasks. If you also use the separate Gemini 4 Hydrogen model in GitHub Copilot, treat it as another test candidate rather than silently substituting its billing or capabilities for the recommendations in the deprecation notice.

Check primary-source evidence â

TECHNICAL ANALYSIS / 03

## Why are Business and Enterprise model policies important?

GitHub says suggested alternatives are enabled automatically where the organization's default model enablement permits it. If a team has disabled the global default, administrators must actively allow the replacement through Copilot model policies. A missing model can therefore be the intended result of governance rather than a software bug.

Inventory configuration files, prompts, agent definitions, internal documentation and approved-model lists. Map old model labels to eligible replacements, verify billing restrictions and check that project-level safeguards still work. The SXF Copilot alternatives guide offers a broader comparison of developer tools when a replacement changes the desired workflow.

Check primary-source evidence â

TECHNICAL ANALYSIS / 04

## SXF migration checklist: avoid surprise regressions

Prepare a small task portfolio covering repository search, bug repair, multi-file refactoring, coding-agent tool calls and code review. Run the baseline with the current permitted model, then repeat with the replacement under similar instructions. Capture test results and cost-per-accepted-change rather than judging only the wording of the model response.

A staged team rollout should record which users or policies authorize each model, avoid assuming the retired identifier is still selectable, and provide rollback to an approved alternative where available. Confirm changes on the actual Copilot client because enterprise policy and individual entitlements can differ from the short public announcement.

Check primary-source evidence â
