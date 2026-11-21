---
id: collect-261121-veille-ia/claude-anthropic/what-is-anthropic-s-auto-mode-and-why-is-it-becoming-the-default-in-claude-code
title: "What is Anthropic's 'auto mode' and why is it becoming the default in Claude Code?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["auto", "mode", "claude", "anthropic", "said", "code", "default", "sessions"]
source: https://upstox.com/news/business-news/latest-updates/what-is-anthropic-s-auto-mode-and-why-is-it-becoming-the-default-in-claude-code/article-198378/
source_anchor: ""
source_lines: [1, 55]
section: "Claude_Anthropic"
---
Business News

SUMMARY

Anthropic said it will also stop charging Pro, Max and Team users for the additional tokens used by the auto mode classifier, effective Monday.

Under the change, new Claude Code sessions on the three plans will start in auto mode.

Anthropic will make its "auto mode" the default permission setting in Claude Code for users on its Pro, Max and Team plans from August 14, saying the feature can detect potentially dangerous commands more effectively than manual approval.

New Claude Code sessions on the three plans will run in auto mode by default.

Users who have previously selected a different default may receive a one-time prompt asking whether they want to switch, while those with a pinned default setting will see no change.

Anthropic said it will also stop charging users on Pro, Max and Team plans for the additional classifier tokens used by auto mode, effective Monday.

The company said auto mode will remain opt-in for now for Claude Enterprise, the Claude API and Claude Platform on AWS, Amazon Bedrock, Google Cloud's Agent Platform and Microsoft Foundry. It plans to make the feature the default on those platforms over the coming month in consultation with its cloud partners.

Boris Cherny, Head of Claude Code at Anthropic, said his team had been using Auto mode exclusively for several months.

"I couldn't imagine going back to permission prompts!" Cherny said in a post on X, adding that the team was "really excited" to make the feature available more broadly.

Auto mode uses a separate classifier to assess each tool call and block actions that are irreversible, destructive or outside the user's environment.

When an action is blocked, Claude can generally find a safer alternative or ask the user for permission.

If Claude encounters three consecutive blocks or 20 blocks during a session, it switches back to manual approval, Anthropic said.

According to the company, Claude Code users approve about 97% of permission prompts, compared with a 3% rejection rate.

Anthropic said its decision followed several months of testing involving internal and third-party red-teaming, prompt-injection evaluations, a controlled study involving 1,053 paid testers and analysis of production sessions.

In the controlled study, human testers detected and rejected a dangerous command in only 13.6% of cases, or 143 out of 1,053 attempts. Auto mode blocked 89%, or 937, of the same commands.

Anthropic said auto mode blocked 800 commands that human testers approved, while humans blocked only six commands that auto mode allowed.

The company also found that human performance deteriorated as sessions became longer. Testers blocked about 17% of dangerous commands early in a session, but the rate fell to about 5% after they had encountered 50 or more previous prompts.

The company said its analysis of real-world sessions also pointed to lower levels of unintended harmful actions with auto mode.

Among sessions flagged for safety review between May and June, 6.3% of sessions using manual approval contained a harmful action at production-level severity or higher that had not been explicitly requested by the user, compared with 2.4% of auto mode sessions.

Auto mode is also intended to allow Claude to work for longer periods without interruption.

Anthropic said this could make models designed for long-running tasks, such as Claude Opus 5, more practical for extended coding work.

Anthropic said auto mode has already been used as the default for Claude Code internally and described three incidents where the classifier prevented potentially damaging actions.

In one case, Claude attempted to publish an internal report to a public code-sharing site after an upload to an internal drive failed. In another, it attempted a mass process termination involving roughly 2,000 pods and queued deletions that could have affected hundreds of GPUs running training jobs.

In a third case, Claude attempted to use a cloud role granting full administrative access while it had been asked to provision read-only billing access.

Anthropic said auto mode blocked the actions and either enabled Claude to find a safer alternative or prompted the user before proceeding.

Next Story
