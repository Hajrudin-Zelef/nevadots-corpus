---
id: collect-261121-veille-ia/copilot-microsoft/6-github-copilot-models-discontinued-check-the-updated-migration-targets-since-the-initial-announcem
title: "6 GitHub Copilot models discontinued. Check the updated migration targets since the initial announcement｜noahs_nalog/AIツール実務レター/あなたのモチベーション"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["models", "model", "gpt-5", "announcement", "copilot", "migration", "github", "check"]
source: https://note.com/noahs_nalog/n/nf34dd26096d2?hl=en
source_anchor: ""
source_lines: [1, 55]
section: "Copilot_Microsoft"
---
# 6 GitHub Copilot models discontinued. Check the updated migration targets since the initial announcement

If you are using procedures or settings with fixed model names in GitHub Copilot, the discontinuation of these models is not just a simple list cleanup. The behavior of chat, inline editing, Ask, agents, and code completion will change, requiring you to redo internal procedures and quality checks.

On October 19, 2026, GitHub discontinued 6 Copilot models across most Copilot experiences. What is important is that some of the recommended migration targets have changed between the September 18 advance notice and the October 19 implementation announcement.

## The 6 discontinued models

The affected models are Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5. While no deletion action is required, workflows and settings based on these must be updated to the supported models.

The migration targets indicated in the implementation announcement are as follows.

- Gemini 3.7 Flash → Gemini 3.8 Flash
- GPT-5.5 / GPT-5.4 → GPT-6.1 Sol
- GPT-5.4 mini / GPT-5 mini → GPT-6 Luna
- Grok 4.5 → Grok 4.7

## Relying only on the September announcement article will lead to incorrect migration targets

In the September 18 notice, the replacements for GPT-5.5 and GPT-5.4 were GPT-5.6 Sol, the mini series were GPT-5.6 Luna, and Grok 4.5 was Grok 4.6. In the October 19 implementation announcement, these have been updated to GPT-6.1 Sol, GPT-6 Luna, and Grok 4.7, respectively.

Therefore, if you use Japanese articles or internal memos from September as they are, they will conflict with the current official recommendations. Even if the model discontinuation date is the same, please check the migration targets in the latest implementation announcement.

## Do not assume automatic switching equals quality assurance

Copilot Enterprise administrators must verify if access to the replacement models is permitted by the model policy. Once enabled, they will appear in the Copilot Chat model selection dropdown in VS Code and github.com.

However, successor models are not compatible versions that return the same answers as the old models. Code generation, tool selection, response speed, usage, and adherence to instructions may change. Do not consider the task complete just by replacing the model name; perform regression testing with important business prompts and representative repositories.

## 3 areas to check today

The first is the model selection dropdown in VS Code and github.com. The second is the organization or enterprise Copilot model policy. The third is the old model names remaining in configuration files, procedure manuals, training materials, and internal templates.

If you have fixed model names in APIs or automation, also check the failure logs. Even if users can manually select a different model, the same problem will recur if hardcoded values remain in shared workflows.

## The first deliverable is a migration table

Beginners should summarize the "old model, latest recommended target, usage location, person in charge, and test results" in one document. For models where the usage location is unknown, search in the order of IDE settings, organizational policies, agent definitions, and internal documentation.

The test items are the success rate of representative tasks, the number of review corrections, response time, and additional usage volume. Even if you use them for contract development or in-house development, simply changing models does not necessarily increase revenue directly. Determine cost savings based on whether downtime, rework, and inquiry handling are reduced.

## Keep a ledger of retirement dates even after migration

As seen this time, recommended destinations may be updated after the initial announcement. It is necessary to maintain a ledger containing the model name, retirement date, announcement URL, implementation URL, and last confirmation date, and to check official information again on the implementation date.

Organizations that are not using the target models this time do not need to take emergency action. However, to prepare for future retirements, listing the locations of fixed models will speed up decision-making next time.

Related article: Adding AI agents won't make work faster. The "three foundations" to prepare first

## Source

- GitHub Changelog "GitHub Copilot models deprecated on October 19, 2026" 
https://github.blog/changelog/2026-10-19-github-copilot-models-deprecated-on-october-19-2026/
- GitHub Changelog "Upcoming deprecation of selected GitHub Copilot models in mid-October" 
https://github.blog/changelog/2026-09-18-upcoming-deprecation-of-selected-github-copilot-models-in-mid-october/
