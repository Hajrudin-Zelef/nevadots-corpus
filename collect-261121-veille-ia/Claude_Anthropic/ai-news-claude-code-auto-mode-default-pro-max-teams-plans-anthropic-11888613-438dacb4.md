---
id: collect-261121-veille-ia/claude-anthropic/ai-news-claude-code-auto-mode-default-pro-max-teams-plans-anthropic-11888613-438dacb4
title: "Ai news claude code auto mode default pro max teams plans anthropic 11888613"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["mode", "auto", "claude", "anthropic", "code", "default", "plans", "users"]
source: ""
source_anchor: ""
source_lines: [1, 13]
section: "Claude_Anthropic"
---
Anthropic says it has stopped charging users on select plans for the additional classifier tokens required for auto mode.
Photo Credit: Anthropic
Anthropic has announced that it is making Claude Code more autonomous, with the auto mode soon becoming the default. The feature allows Claude to execute coding tasks without repeatedly asking users for permission, while a classifier evaluates tool calls and blocks actions that are considered irreversible. Anthropic says auto mode has performed better than manual permission reviews in its testing. The company has also introduced additional safeguards covering prompt injection, data exfiltration, destructive Git commands, and sensitive data access.
In a blog post, Anthropic said that new Claude Code sessions on Pro, Max, and Team plans will run in auto mode by default beginning August 14. Users who have selected a different default will receive a one-time prompt asking whether they wish to switch. Meanwhile, those who have already pinned auto mode as default will see no change.
Starting August 14, auto mode will be the default permission mode in Claude Code for Pro, Max, and Team users.
Auto mode reviews shell commands and actions with a separate classifier. In testing, it caught 89% of dangerous commands. Manual approval caught 14%.
The company said that it has stopped charging users on the aforementioned plans for the additional classifier tokens required for auto mode.
When auto mode is enabled, each Claude Code tool call is routed through a classifier instead of showing permission prompts before individual actions. The system is designed to block actions that are irreversible, destructive, or directed outside the user's environment, the company explained. Claude can look for a safer alternative or ask the user for permission to proceed if an action is blocked.
However, it eventually falls back to manual approval after three consecutive blocks or 20 blocks during a session.
Anthropic cited developer habits when responding to permission prompts as the reason behind this change. According to the company's data, users approve 97 percent of permission requests in Claude Code, while they reject 39 percent of plans presented for approval.
The company has also added several safety mechanisms, including hard-deny rules for actions such as data exfiltration, rules governing access to and sharing of sensitive information, checks of Git status before destructive Git operations, and prompt injection screening for content sourced from websites, files, and other tool outputs.
Auto mode is currently opt-in for Enterprise users, as well as for those accessing Claude Code through the Claude API, Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry, and other listed enterprise platforms. The company says it plans to make it the default across these services in the coming month.
Get your daily dose of tech news, reviews, and insights, in under 80 characters on Gadgets 360 Turbo. Connect with fellow tech lovers on our Forum. Follow us on X, Facebook, WhatsApp, Threads and Google News for instant updates. Catch all the action on our YouTube channel.
