---
id: collect-261121-veille-ia/google-ecosystem/google-labs-stitch-loop-reads-your-live-metrics-and-opens-fix-prs-a-ai-insiders
title: "Google Labs' Stitch Loop reads your live metrics and opens fix PRs â AI Insiders"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "loop", "stitch", "agent", "pull", "labs", "product", "opens"]
source: https://talent.aiinsiders.net/article/google-labs-stitch-loop-reads-your-live-metrics-and-opens
source_anchor: ""
source_lines: [1, 21]
section: "Google_Ecosystem"
---
Google Labs has introduced Stitch Loop, an agent that stays attached to a product after launch, reads its metrics, and proposes code changes through GitHub pull requests. Google announced the experiment on November 18 on its blog, in a post by Justin Toupin, director of product management at Google Labs. It targets the stretch of work that begins once a coding agentâs pull request has merged.

Toupinâs argument is that coding agents stop at the merge, while the expensive work follows: fixing security holes, making checkout faster, and tracing why signups drop. Lean teams rarely have the hours for that. Stitch Loop is Googleâs attempt to hand it to software that does not clock out.

Setup starts with a GitHub connection plus whatever product context a team has, such as logs, user behavior, and feedback. Google lists more than 180 integrations, custom MCP servers (the tool-calling protocol Anthropic created), and local CLI uploads as ways to feed it. From there the agent works in loops, and three ship by default: security, latency, and accessibility. Teams can add their own, and Googleâs example is a loop for checkout conversion.

Each loop runs four steps. Suggestions surfaces opportunities backed by evidence from the product. Solutions attaches one or more options to each suggestion, with tradeoffs, scoping, and visual design alternatives where relevant. Build has the agent write code inside a sandboxed cloud environment, then open a pull request that can include screenshots and video clips of what changed. Reflect keeps reading live data afterward, learns from good and bad outcomes, and continues to watch the metric once the goal is met.

The security loop is the most concrete piece. Google says it pairs the cyber capabilities of Gemini 4 Carbon with CodeMender, an autonomous code-security agent from Google DeepMind. It scans a codebase for vulnerabilities, flags each finding with evidence, and opens a pull request with a proposed fix. Google has not published detection rates, false-positive counts, or any customer results for it.

Control stays with the team. Google states that nothing merges until a person reviews the pull request. That keeps the agent in the position of a very persistent contributor, and it moves the bottleneck to the reviewer. An agent that surfaces suggestions around the clock can generate more pull requests than a small team has time to read.

Stitch Loop can also serve other agents. Coding tools such as Google Antigravity can draw lasting product context out of it through the Stitch CLI and MCP, which Google says supports mixed-model setups. In Slack, typing @stitch returns daily insight digests and lets users create new loops.

Stitch Loop is also a change of direction for Stitch, which Google says has a user base in the millions, spanning designers, developers, and founders who go from an idea to a working interface. The company describes agents for the whole path from ideation to continuous optimization as its broader ambition for the product.

The caveats are the usual ones for a first-party announcement. Googleâs post contains no benchmarks, no pricing, and no description of how the agent decides that a metric has slipped. Every claim about what the loops achieve comes from the company itself.

The waitlist opens to all users worldwide, and eligible Google AI Ultra subscribers get priority access. Teams that want to judge it should join now, and run the first loop against one metric they already track, so the pull requests can be compared with what an engineer would have shipped.

*Google Labs, âIntroducing Stitch Loop: A new way to build and evolve products,â blog.google, November 18, 2026.*
