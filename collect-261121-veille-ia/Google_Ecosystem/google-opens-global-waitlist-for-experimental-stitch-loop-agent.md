---
id: collect-261121-veille-ia/google-ecosystem/google-opens-global-waitlist-for-experimental-stitch-loop-agent
title: "Google opens global waitlist for experimental Stitch Loop agent"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "stitch", "agent", "loop", "product", "global", "waitlist", "opens"]
source: https://ai.info/news/google-labs-stitch-loop-global-waitlist
source_anchor: ""
source_lines: [1, 39]
section: "Google_Ecosystem"
---
The Pulse

# Google opens global waitlist for experimental Stitch Loop agent

Stitch Loop is designed to suggest and build software improvements using code and live product data, with changes subject to review. The global waitlist gives eligible Google AI Ultra users priority access.

Google Labs has opened a global waitlist for Stitch Loop, an experimental agent designed to propose and build software improvements using a team’s code and live product data. In a November 18 post, the company said the agent would submit its proposed changes for human review rather than merge them automatically.

Eligible Google AI Ultra users get priority access. The tool is part of a broader direction for Stitch, Google’s product for turning ideas into working interfaces.

## Connecting code and product data

The experiment connects to GitHub and uses product context such as logs, user behavior and feedback. Google says that context can be supplied through more than 180 integrations, custom MCP servers or local command-line uploads.

Teams can set measurable outcomes for the agent to work toward. Google says it runs ongoing loops that surface opportunities grounded in product evidence, propose solutions, build a selected change and then monitor the result.

Each suggestion comes with one or more solutions, including tradeoffs and scope, according to Google. Where applicable, those solutions also include visual design options.

## Changes go through review

Once a user chooses a direction, Stitch Loop writes code in an isolated cloud sandbox and opens a GitHub pull request for approval. When relevant, the pull request contains screenshots and videos of the work.

Nothing merges until the user reviews the pull request. Google says the agent monitors live product data as it works toward a goal and learns from positive and negative results to inform subsequent suggestions.

After a goal is met, the agent is designed to keep watching the associated metric in case it slips. The company describes the work as continuing around the clock, with the aim of helping teams maintain and improve products after release.

## Built-in and custom loops

The experiment includes three default loops covering cybersecurity, latency and accessibility. Teams can also create their own loops; Google gives improving checkout conversion as an example.

The security loop combines Gemini 4 Carbon’s cybersecurity capabilities with CodeMender, a Google DeepMind agent for improving code security. Google says the tools scan a codebase for vulnerabilities, flag issues with evidence and open a pull request proposing a fix for review.

Stitch Loop can also serve as a layer for coding agents such as Google Antigravity. According to Google, other coding agents can retrieve persistent product context through the Stitch CLI and MCP, including for workflows that use different models.

In Slack, users can use @stitch to receive daily insight digests and create loops. Google says millions of designers, developers and founders already use Stitch, and positions the new experiment alongside it as part of an effort to help teams work from initial ideas through continuing product optimization.

## Sources

1. Original source · Blog post Google blog.google
