---
id: collect-261121-veille-ia/claude-anthropic/anthropic-launches-claude-fable-5-1-with-stronger-coding-and-lower-cache-costs-igeeksblog
title: "Anthropic launches Claude Fable 5.1 with stronger coding and lower cache costs – iGeeksBlog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "anthropic", "model", "work", "users", "coding", "cache"]
source: https://www.igeeksblog.com/anthropic-claude-fable-5-1/
source_anchor: ""
source_lines: [1, 35]
section: "Claude_Anthropic"
---
Anthropic launched Claude Fable 5.1 on September 1, 2026, as the successor to Fable 5. The company calls it its most capable generally available Claude model, built for long-running coding, research, and knowledge work.

The release arrives less than three months after Claude Fable 5 became Anthropic’s first public Mythos-class model. Fable 5.1 is aimed at tasks that take hours, use tools across multiple steps, and need the model to review its own work.

## Fable 5.1 focuses on long-running work

Anthropic says the gains are concentrated in tasks that need Claude to keep context, use tools, and check its work:

- **Coding:** It can handle multi-file features, large refactors, migrations, debugging, and code review across long sessions. It can write tests and use vision to compare its output with a design.
- **Research and files:** It follows multistep web research and can turn a question into a finished document, spreadsheet, or slide deck.
- **Computer use and vision:** It operates browsers and desktop apps more reliably, recovers from failed steps, and reads dense charts, tables, and filings inside PDFs.

For developers, Fable 5.1 has a 1-million-token context window, a maximum output of 128,000 tokens, and the API ID `claude-fable-5-1`. Anthropic’s own benchmark table reports 52.6% for Fable 5.1 versus 24.7% for Fable 5 on Terminal-Bench-Science 0.1, and 73.4% versus 70.5% on CursorBench 3.2.0. Those are company-reported results, not independent tests.

For Apple developers, the release also arrives alongside Claude Code’s recent iOS Simulator support on Mac. The feature lets Claude Code build, run, and debug iPhone apps while the simulator sits beside the conversation.

## Cache reads are cheaper, but base rates stay the same

Fable 5.1 keeps Fable 5’s API rates of $10 per million input tokens and $50 per million output tokens. The model documentation lists cache reads at $0.25 per million tokens, down 75% from Fable 5’s $1 rate.

Anthropic estimates that change will lower typical workloads by around 25%. Highly agentic workloads could save up to about 45%, but those are Anthropic’s workload estimates rather than a guaranteed subscription discount.

## Access depends on the plan

Fable 5.1 is available on Claude’s web, mobile, desktop, Cowork, Code, Design, Microsoft 365, and Claude Tag products. It is available on paid Pro, Max, Team, and Enterprise plans, but not on the Free plan.

The current plan rules differ by tier. Max users and premium Team or seat-based Enterprise users can use Fable models for up to 50% of their weekly usage limits at no extra cost. Pro users and standard Team or seat-based Enterprise users need usage credits from the start. API and usage-based Enterprise access use standard token billing. Developers can also use the model through Amazon Web Services, Google Cloud, and Microsoft Foundry.

Long-running work is also the focus of Claude Cowork’s move to iPhone and web, which lets users start, check, and steer remote work while a session continues away from the desktop.

## Fable 5.1 is public, Mythos 5.1 is restricted

Anthropic launched Claude Mythos 5.1 alongside Fable 5.1. The two models share the same underlying model, but Mythos has different, more permissive safeguards and is limited to trusted access programs for vetted cyberdefenders and life sciences professionals.

Fable 5.1 remains the public version, but flagged cybersecurity and biology requests may be routed to Claude Opus 4.8 or Opus 5 instead. Anthropic says users are not charged Fable rates for requests handled by those fallback models.
