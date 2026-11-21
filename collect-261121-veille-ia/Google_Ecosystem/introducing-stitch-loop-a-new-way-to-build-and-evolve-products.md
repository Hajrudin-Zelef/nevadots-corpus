---
id: collect-261121-veille-ia/google-ecosystem/introducing-stitch-loop-a-new-way-to-build-and-evolve-products
title: "Introducing Stitch Loop: A new way to build and evolve products"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["stitch", "loop", "build", "products", "product", "google", "security", "introducing"]
source: https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-loop/
source_anchor: ""
source_lines: [1, 36]
section: "Google_Ecosystem"
---
# Introducing Stitch Loop: A new way to build and evolve products

Coding agents let you build and ship new features fast, but their job ends the moment your pull request merges. Once code hits production, the real work starts: patching vulnerabilities, speeding up checkout, and diagnosing signup drop-offs buried in noisy feedback. For lean teams, balancing post-launch maintenance with continuous improvement is a constant challenge, yet it’s also what drives some of the world’s most successful products.

Today, we are introducing Stitch Loop, a long-running product agent from Google Labs that connects to your codebase and live metrics, finds what needs fixing, and keeps working 24/7 on your product until your goals are met.

Millions of designers, developers, and founders already use Stitch to turn ideas into working interfaces in minutes. Together, Stitch and Stitch Loop are steps toward our broader ambition for Stitch: creating agents to help lean teams tackle the hardest parts of creating great products from ideation through continuous optimization.

## See how it works

You start by connecting GitHub and your product context (logs, user behavior, feedback, etc) with over 180 integrations, custom MCP servers, or local CLI uploads. Grounded in that context, Stitch Loop runs evergreen loops to continuously optimize toward the measurable outcomes you care about. You get three default loops out of the box and can add your own anytime. Each loop runs through four steps:

1. **Suggestions:** Stitch Loop surfaces opportunities to make progress towards your goals, grounded in evidence from your product.
2. **Solutions:** Every suggestion comes with one or more solutions— each with tradeoffs and scoping, along with visual design options where applicable.
3. **Build:** Once you choose a direction, Stitch Loop writes the code in an isolated cloud sandbox and opens a GitHub pull request for your approval. When relevant, the PR will contain screenshots and videos of the work Loop did.
4. **Reflect:** Stitch Loop monitors live product data as it works toward your goal, learning from every positive or negative result to sharpen the next round of insights. Once the goal is met, it keeps watching in case the metric slips.

You always have the final call. Nothing merges until you review the PR.

## Automate codebase security

Stitch Loop comes with built-in loops for cybersecurity, latency, and accessibility, plus the ability to create custom loops like improving checkout conversion.

Security is often the hardest gap to cover for lean teams shipping quickly. Stitch Loop includes a dedicated security loop that pairs Gemini 4 Carbon's cyber security capabilities with CodeMender, Google DeepMind's autonomous agent that improves code security. Together, they scan your codebase for security vulnerabilities before they become incidents. Stitch Loop flags each issue it finds with evidence and opens a PR with a proposed fix for your review.

## Use it with your agents and existing workflows

You can use Stitch Loop as a service layer for coding agents, like Google Antigravity, or bring it into the places where your team already works, like Slack.

Your preferred coding agent can pull persistent product context from Stitch Loop via the Stitch CLI and MCP to power mixed-model setups and fully autonomous improvements in your existing workflow.

In Slack, just use @stitch to get daily insight digests and create new loops.

## Join the waitlist

The Stitch Loop waitlist opens today for all users, globally, with eligible Google AI Ultra users getting priority access. Join at stitch.google.com/loop/waitlist and share your feedback in the Google Labs Discord.
