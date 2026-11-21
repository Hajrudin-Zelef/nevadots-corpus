---
id: collect-261121-veille-ia/gemini-google/google-stitch-loop-an-always-on-product-agent-that-fixes-security-speed-and-accessibility-via-pull-r
title: "Google Stitch Loop: An Always-On Product Agent That Fixes Security, Speed and Accessibility via Pull Requests"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["stitch", "google", "product", "loop", "agent", "security", "pull", "teams"]
source: https://i10x.ai/news/google-stitch-loop-product-agent-github-pull-requests
source_anchor: ""
source_lines: [1, 50]
section: "Gemini_Google"
---
Google wants an AI agent to keep working on your product after it ships. On 18 November 2026 Google Labs introduced Stitch Loop, an experimental, long-running product agent that connects to a team’s codebase and live metrics, finds what needs fixing and opens GitHub pull requests that a human must approve before anything merges.

## Quick Take

1. **What it is:** an always-on product agent from Google Labs, part of a new direction for its Stitch design tool.
2. **Built-in loops:** cybersecurity, latency and accessibility, plus custom loops such as improving checkout conversion.
3. **Security engine:** pairs Gemini 4 Carbon’s cyber security capabilities with CodeMender, Google DeepMind’s code security agent.
4. **Human control:** every change arrives as a pull request; nothing merges until someone reviews it.
5. **Access:** a global waitlist opens today, with priority for eligible Google AI Ultra users.

## In plain English

Most AI coding tools wait for you to ask for something. Stitch Loop watches your live app on its own, notices problems like slow pages, security holes or accessibility gaps, writes a fix in a safe test environment and sends it to you for a yes or no. It then checks whether the fix actually moved the numbers.

## What happened

Justin Toupin, Director of Product Management at Google Labs, announced Stitch Loop in a Google blog post. “Millions of designers, developers, and founders already use Stitch to turn ideas into working interfaces in minutes,” he wrote, calling Stitch and Stitch Loop “steps toward our broader ambition for Stitch: creating agents to help lean teams tackle the hardest parts of creating great products.”

## How it works

Teams connect GitHub and product context such as logs, user behaviour and feedback through more than 180 integrations, custom MCP servers or local CLI uploads. Each loop then runs four steps:

- **Suggestions:** opportunities toward a goal, grounded in evidence from the product.
- **Solutions:** one or more options with trade-offs and scoping, plus visual design options where relevant.
- **Build:** code written in an isolated cloud sandbox and a pull request opened, sometimes with screenshots and videos.
- **Reflect:** it monitors live data, learns from results and keeps watching after a goal is met in case the metric slips.

Stitch Loop can also act as a service layer for coding agents such as Google Antigravity: any coding agent can pull product context through the Stitch CLI and MCP. In Slack, @stitch posts daily insight digests and creates new loops.

## Why it matters

**For lean teams**, post-launch work such as security fixes and performance tuning often gets dropped. A loop that proposes evidence-based fixes could cover some of that gap.

**For developers**, it shifts AI from writing new features to maintaining live systems, while keeping review in the familiar pull-request flow.

**For Google**, it is a showcase for Gemini 4 Carbon and CodeMender in a product setting, competing with third-party coding agents.

## What’s still open

- Pricing and usage limits after the waitlist phase.
- How well the security loop performs on real codebases and how many false positives it creates.
- What data access teams must grant and how product data is handled.

## Sources

- Google, “Introducing Stitch Loop: A new way to build and evolve products,” blog post by Justin Toupin, 18 November 2026

## i10x Perspective

Coding agents are moving from the editor into operations. Stitch Loop’s bet is that an agent tied to live metrics, not prompts, will find the work worth doing. The pull-request gate is the right default; the open question is how many PRs a small team can actually review. More from Google.
