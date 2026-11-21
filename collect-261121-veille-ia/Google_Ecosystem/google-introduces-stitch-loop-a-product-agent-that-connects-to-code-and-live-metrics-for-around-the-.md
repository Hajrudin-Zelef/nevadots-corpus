---
id: collect-261121-veille-ia/google-ecosystem/google-introduces-stitch-loop-a-product-agent-that-connects-to-code-and-live-metrics-for-around-the-
title: "Google introduces Stitch Loop, a product agent that connects to code and live metrics for around-the-clock optimization"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["stitch", "agent", "loop", "product", "google", "pull", "live", "security"]
source: https://completeaitraining.com/news/google-introduces-stitch-loop-a-product-agent-that-connects/
source_anchor: ""
source_lines: [1, 27]
section: "Google_Ecosystem"
---
Google Labs just opened a waitlist for Stitch Loop, a new product agent that connects to your codebase and live metrics to find bugs, patch security holes, and boost performance around the clock. It identifies issues, proposes fixes, and opens pull requests for your team to review. The tool is designed for lean teams that struggle to balance building new features with the relentless upkeep of production software.

Stitch Loop runs continuous improvement cycles tied to measurable outcomes like latency, accessibility, and checkout conversion. You get three built-in loops for security, speed, and accessibility, plus the option to create custom loops for your own product goals. Nothing merges until you approve the pull request.

### How the agent works

The system operates in four steps. First, it surfaces evidence-backed suggestions from your product context. Each suggestion comes with one or more solutions, complete with tradeoffs and scoping details. Once you pick a direction, Stitch Loop writes the code in an isolated cloud sandbox and opens a GitHub pull request. The PR may include screenshots and videos of the work done. After deployment, the agent monitors live data to see if the change moved the needle. It learns from every result and keeps watching even after a goal is met, in case the metric slips.

You connect the agent to GitHub and your product data through over 180 integrations, custom MCP servers, or local CLI uploads. This context grounds every suggestion the agent makes.

### Built-in security automation

One of the default loops focuses on cybersecurity. It pairs Gemini 4 Carbon's security capabilities with CodeMender, an autonomous agent from Google DeepMind that improves code security. Together they scan for vulnerabilities and open pull requests with proposed fixes. Google describes security as "often the hardest gap to cover for lean teams shipping quickly."

The latency and accessibility loops follow the same pattern: find issues, propose fixes, build the solution, and verify the result against live data.

### Fitting into existing workflows

Stitch Loop can act as a service layer for coding agents like Google Antigravity. Teams can pull persistent product context through the Stitch CLI and MCP to power mixed-model setups. In Slack, using **@stitch** delivers daily insight digests and lets you create new loops without leaving the conversation.

For professionals working across product development, IT, and operations, this kind of automated maintenance agent shifts the burden of post-launch upkeep. It lets developers focus on new features while the agent handles the ongoing work of keeping software secure and performant. Teams looking to build these skills can explore AI for Developers Courses to understand how agents fit into modern development pipelines.

### Why this matters for product and operations teams

The gap Stitch Loop targets is real: coding agents help you ship fast, but they stop when the pull request merges. After that, someone still has to patch vulnerabilities, diagnose signup drop-offs, and speed up sluggish checkout flows. For lean teams without dedicated site reliability engineers, that work piles up. Stitch Loop automates the monitoring-to-fix cycle and keeps running until the numbers improve. You stay in control through pull request review, but you don't have to hunt for the problem yourself.

Join the waitlist at stitch.google.com/loop/waitlist. Eligible Google AI Ultra users get priority access.
