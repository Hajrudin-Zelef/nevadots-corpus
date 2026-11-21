---
id: collect-261121-veille-ia/google-ecosystem/google-labs-unveils-stitch-loop-24-7-product-optimisation-agent
title: "Google Labs unveils Stitch Loop 24/7 product optimisation agent"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["google", "stitch", "loop", "labs", "product", "gemini", "agent", "waitlist"]
source: https://ashutoshai.com/ai-compass/2026/11/18/google-labs-stitch-loop-product-agent/
source_anchor: ""
source_lines: [1, 43]
section: "Google_Ecosystem"
---
Google Labs introduced Stitch Loop on 18 November 2026 as an always-on post-launch product agent experiment inside Stitch, opening a global waitlist at stitch.google.com/loop/waitlist with priority access for eligible Google AI Ultra subscribers. Rather than stopping once an initial feature merges, Stitch Loop connects a teamâs GitHub codebase to live production logs, user behaviour and customer feedback through more than 180 integrations, custom MCP servers or local CLI uploads.

## Four-stage evergreen loops, CodeMender security scans and Antigravity context

Every loop iterates continuously through four stages: surfacing evidence-backed Suggestions, scoping Solutions with trade-offs and visual UI options, writing code in an isolated cloud sandbox to open a GitHub pull request with screenshots and videos (Build), and tracking live post-merge metrics so regressions trigger fresh passes (Reflect). Nothing merges until a human reviews and approves the pull request.

Out of the box, Stitch Loop ships built-in loops for latency, accessibility and cybersecurity, where the security loop combines Gemini 4 Carbon with Google DeepMindâs autonomous CodeMender agent to flag vulnerabilities and draft patches, alongside custom loops such as checkout conversion. External coding agents including Google Antigravity can pull persistent product context via the Stitch CLI and MCP, while @stitch posts daily digests in Slack. Google has not announced Stitch Loop pricing.

## Technical details

- Availability and waitlist
- Global Google Labs experiment waitlist at stitch.google.com/loop/waitlist (Google AI Ultra priority)
- Four-step execution cycle
- Suggestions, Solutions (with visual options), isolated cloud-sandbox Build PRs and post-merge Reflect
- Built-in and custom loops
- Cybersecurity (Gemini 4 Carbon + DeepMind CodeMender), latency, accessibility and custom metric goals
- External workflow surfaces
- GitHub PRs, 180+ telemetry integrations, Stitch CLI, MCP context server and @stitch in Slack

## Release details & context

### Serving persistent product context to mixed-model coding harnesses

Beyond opening its own sandboxed pull requests, Google Labs designed Stitch Loop to operate as a shared product-context layer for third-party and first-party coding agents. Through the Stitch CLI and MCP server, external harnesses can query accumulated user-behaviour evidence, prior loop outcomes and active metric targets to run mixed-model engineering setups inside a developerâs existing IDE or terminal workflow.

Teams working primarily in chat can invoke @stitch inside Slack to receive daily insight digests summarising metric shifts and spin up new optimisation loops without opening the web dashboard.

## Common questions

### Can Stitch Loop merge code changes into production automatically without human review?

No, Stitch Loop writes patches inside an isolated cloud sandbox and opens a GitHub pull request with supporting screenshots or videos, requiring manual human approval before anything merges.

## Sources

1. PrimaryGoogle Labs Blog: Introducing Stitch Loop: A new way to build and evolve productsblog.google
2. OfficialStitch Loop Global Waitliststitch.google.com
3. On XStitch by Google (@stitchbygoogle) launch thread on Xx.com

## Google release timeline (42 launches tracked)

1. Google makes Gemini Tasks free in six countries on Gemini 4 HydrogenGoogle expands Gemini Tasks free of charge across six countries on Gemini 4 Hydrogen with email RSVPs, phone quoting and Google Pay checkout.
2. Google AI Studio adds custom domains and HD Android emulator buildsGoogle AI Studio adds direct custom-domain purchasing and hosting alongside HD audio Android emulation, Firestore sync and personal Play publishing.
