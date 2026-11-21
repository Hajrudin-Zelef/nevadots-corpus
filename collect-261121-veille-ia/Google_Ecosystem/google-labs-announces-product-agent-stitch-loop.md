---
id: collect-261121-veille-ia/google-ecosystem/google-labs-announces-product-agent-stitch-loop
title: "Google Labs Announces Product Agent “Stitch Loop”"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["stitch", "google", "loop", "agent", "product", "labs", "users", "live"]
source: https://jetstream.blog/en/google-labs-stitch-loop/
source_anchor: ""
source_lines: [1, 19]
section: "Google_Ecosystem"
---
- Google Labs has announced “Stitch Loop,” a long-running product agent that continuously improves products 24/7 by integrating with codebases and live metrics, serving as a new experimental feature for the UI design tool “Stitch.”
- An agent that connects to codebases and live metrics to autonomously identify issues that need fixing and continuously improves products around the clock until goals are achieved.
- Waitlist registration (stitch.google.com/loop/waitlist) is now open to all users globally.

On Wednesday, November 18, 2026 (local time), Google’s experimental projects division, Google Labs, announced “Stitch Loop,” a long-running product agent representing a new direction for the AI-native design platform “Google Stitch (Stitch).”

“Stitch Loop” is an agent that connects to codebases and live metrics to autonomously identify issues that need fixing, continuously improving products 24/7 until set goals are achieved. While conventional coding agents complete their role once a pull request (PR) is merged, post-production releases require ongoing improvements such as fixing vulnerabilities, speeding up checkouts, and identifying drop-off factors buried in noisy feedback.

To get started, users connect product context—such as logs, user behavior, and feedback—via “GitHub,” integrations with over 180 external services, custom MCP servers, or local CLI uploads. In addition to three default loops (Cybersecurity / Latency / Accessibility), users can add custom loops at any time, such as one aimed at improving checkout conversion rates.

Each loop operates autonomously through four steps: “Suggestions” (proposing opportunities for improvement to achieve goals), “Solutions” (presenting fixes including trade-offs, scopes, and design proposals), “Build” (writing code in an isolated cloud sandbox and creating pull requests with screenshots/videos), and “Reflect” (learning and improving accuracy by monitoring live data, followed by continuous monitoring after goals are met). Note that final approval authority always remains with the user; code is never merged automatically without reviewing the pull request.

Particularly in terms of security, the platform features a dedicated loop combining the cybersecurity capabilities of the cutting-edge AI model “Gemini 4 Carbon” with “CodeMender,” an autonomous code security improvement agent from the AI research lab Google DeepMind. It scans the codebase for vulnerabilities before incidents occur and generates pull requests with proposed fixes along with rationale.

Furthermore, “Stitch Loop” functions as a service layer for coding agents such as the agent execution infrastructure “Google Antigravity,” allowing persistent product context to be shared via Stitch CLI or MCP. It also integrates with “Slack,” enabling users to receive daily insight digests or create new loops simply by mentioning “@stitch.”

Waitlist registration (stitch.google.com/loop/waitlist) for “Stitch Loop” is now open to all users globally. Priority access will be provided to eligible “Google AI Ultra” members.

Source: Google (https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-loop/)
