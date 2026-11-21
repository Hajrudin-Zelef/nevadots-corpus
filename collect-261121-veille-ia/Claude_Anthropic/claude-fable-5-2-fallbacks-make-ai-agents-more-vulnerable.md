---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-fallbacks-make-ai-agents-more-vulnerable
title: "Claude Fable 5.2 fallbacks make AI agents more vulnerable"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "model", "opus", "anthropic", "requests", "code", "more"]
source: https://www.notebookcheck.net/Claude-Fable-5-2-fallbacks-make-AI-agents-more-vulnerable.1446771.0.html
source_anchor: ""
source_lines: [1, 17]
section: "Claude_Anthropic"
---
Anthropic released Claude Fable 5.2 on November 9. According to Anthropic's announcement, the model generates output up to 30% faster than Fable 5.1, and early testing showed about 55% fewer blocked requests. The system card also reports a result that matters if you use Claude as an agent, for example to read email, operate websites or work on code you did not write. In those settings, Fable 5.2 hands requests to an older model more often, and that older model is easier to attack.

## Why Fable 5.2 passes some requests on

Fable 5.2 is the same underlying model as Claude Mythos 5.2, which Anthropic only offers to vetted organizations. To make Fable available to everyone, classifiers check every request for offensive cybersecurity, risky biology and parts of AI development. When a classifier flags a request, a less capable model answers instead, Claude Opus 4.8 for cyber topics and Claude Opus 5 for biology. The switch is visible. Claude shows a notice, and the response is labeled with the model that answered. The key detail is on Anthropic's help page, which says the checks review everything the model reads, including memory, connector content, web search results and files. That means text you never typed yourself can trigger the switch.

## What the system card measured

In a prompt injection attack, someone hides instructions in an email, a web page or a file. If the agent reads that text and follows it, it might forward private messages to the attacker as if the user had asked for it. On Gray Swan's indirect prompt injection benchmark, the attack success rate against Fable 5.2 after 15 attempts was 4.09%. Fable 5.1 came in at 1.02% and Opus 5.5 at 0.96%. Anthropic attributes the gap mainly to fallbacks. Of the attempts against Fable 5.2, 53% were served by Opus 4.8, compared with 23% for Fable 5.1. Attacks succeeded 2.05% of the time when Fable 5.2 answered itself and 5.17% of the time when Opus 4.8 did.

The gap is larger in a second test, where an adaptive attacker had 200 attempts per coding scenario. Without safeguards, 89.82% of attempts against Fable 5.2 succeeded, and almost all of the responses came from Opus 4.8. With the prompt injection probes Anthropic runs in its products, the rate dropped to 28.16%, while Opus 5.5 reached 11.13% under the same conditions. None of the 43 requests that Fable 5.2 answered itself in this test were compromised.

## How to turn off automatic switching

Automatic model switching is on by default on the web, in the mobile and desktop apps, in Cowork and in Claude Code. You can turn it off under Settings and Capabilities, or in Claude Code under Config and MODEL & OUTPUT. The toggle is labeled "Switch models when a message is flagged." A flagged request then pauses instead of going to Opus 4.8, and you decide whether to edit the message or pick another model yourself. That is less convenient. If you let Fable 5.2 work through inboxes, websites or third-party repositories, though, it stops those requests from being handed to the weaker model. Injected content in coding tools is not a hypothetical risk either, as the tampered Claude Code extension on Cursor's marketplace showed at the end of October.

For most tasks, Anthropic recommends starting with Claude Opus 5.5 and moving up to Fable only for the most demanding work. On the Pro plan, Fable normally runs on usage credits, as it did with Fable 5.1. As a launch offer, Pro subscribers can use the new model as part of their plan until November 16.
