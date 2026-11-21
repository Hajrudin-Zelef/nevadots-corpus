---
id: collect-261121-veille-ia/google-ecosystem/google-launches-stitch-loop-an-ai-agent-that-keeps-improving-your-app-after-launch
title: "Google Launches Stitch Loop, an AI Agent That Keeps Improving Your App After Launch"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "stitch", "loop", "agent", "product", "after", "keeps", "pull"]
source: https://aiparabellum.com/google-stitch-loop-autonomous-product-agent-waitlist/
source_anchor: ""
source_lines: [1, 68]
section: "Google_Ecosystem"
---
Google Labs just introduced Stitch Loop, an experimental AI product agent that plugs into your codebase and live metrics, then keeps working on your app long after it ships. Instead of stopping when code merges, the agent hunts for problems, proposes fixes and opens pull requests for your team to approve. A waitlist opened today for users worldwide.

## What Is Google Stitch Loop?

Stitch started as a Google Labs tool that turns ideas into working interfaces, and Google says millions of designers, developers and founders already use it. Stitch Loop is described as a “new direction” for that product. It is a long-running agent meant to handle the part of software work that begins after launch.

Google’s pitch is simple. Today’s coding agents help you build features fast, but their job ends once a pull request merges. After that, someone still has to patch security holes, speed up slow pages and figure out why people drop off during signup.

Stitch Loop aims to take on that ongoing work around the clock. Google frames it as a helper for lean teams that cannot dedicate staff to constant maintenance and tuning.

## How the Stitch Loop Agent Works

You start by connecting GitHub and your product context, such as logs, user behavior and customer feedback. Google says it supports more than 180 integrations, plus custom MCP servers and local CLI uploads. From there, the agent runs what Google calls evergreen loops that work toward measurable goals.

### The four steps in every loop

1. Suggestions: the agent points out chances to move toward your goal, backed by evidence from your product data.
2. Solutions: each suggestion comes with one or more fixes, along with tradeoffs, scoping and design options where they apply.
3. Build: once you pick a direction, it writes the code in an isolated cloud sandbox and opens a GitHub pull request, sometimes with screenshots or videos of its work.
4. Reflect: it watches live data, learns from wins and misses, and keeps monitoring after the goal is reached in case the metric slips.

One rule stays fixed throughout. Nothing merges until a person reviews and approves the pull request, so your team keeps the final say on every change.

## Built-In Loops for Security, Speed and Accessibility

Every workspace comes with three default loops, and you can add custom ones for goals like better checkout conversion. Here is how the launch lineup breaks down.

| Loop | What it targets | Notes from Google | 
|---|---|---|
| Security | Vulnerabilities in your codebase | Pairs Gemini 4 Carbon with CodeMender, Google DeepMind’s code security agent | 
| Latency | Slow parts of your product | Included by default | 
| Accessibility | Accessibility gaps in your interface | Included by default | 
| Custom | Any goal you define, such as conversions | Add your own loops at any time | 

The security loop is the headline feature. Google calls security the hardest gap for small teams that ship quickly, and it uses the cyber skills of Gemini 4 Carbon to scan code before issues turn into incidents. Each finding comes with evidence and a proposed fix in a pull request.

## Working With Other Coding Agents and Slack

Stitch Loop is not meant to replace the tools you already use. Google says it can act as a service layer for coding agents such as Google Antigravity, and any coding agent you prefer can pull product context from it through the Stitch CLI and MCP.

That opens the door to mixed setups, where one agent writes features and Stitch Loop supplies the live product context. It is a similar idea to how teams already pair agents with automated code review tools to catch mistakes before they ship.

Teams can also use it in Slack. Typing @stitch brings daily insight digests and lets you create new loops without leaving chat.

## Who Can Get Stitch Loop and When

The Stitch Loop waitlist is open today for all users globally. Google says eligible Google AI Ultra subscribers will get priority access, and it is collecting feedback in the Google Labs Discord.

Google has not shared pricing or a date for wider availability. Because it is labeled an experiment, features and access rules may change as testing goes on.

## What This Means for You

If you run a small product team, Stitch Loop is worth watching. It targets the tedious upkeep that often slips when everyone is busy shipping new features.

- Join the waitlist early if post-launch maintenance eats into your roadmap, especially if you already pay for Google AI Ultra.
- Treat its pull requests like any teammate’s work and review each one, since the agent proposes changes but you approve them.
- Think about what data you are comfortable sharing, because the agent works best with logs, user behavior and feedback connected.
- Pair it with your existing security threat detection practices rather than relying on one automated scanner.

For developers, the bigger trend is clear. AI agents are moving from one-off coding help toward always-on roles, a shift we have tracked across recent AI news from Google and its rivals.

## Sources

Primary source: Google’s Stitch Loop announcement.

## Conclusion

Stitch Loop shows Google pushing AI agents past the moment code ships and into the daily work of keeping a product healthy. It connects to your code and live data, runs loops for security, speed and accessibility, and opens pull requests that people still have to approve. The waitlist is open now, with Google AI Ultra users first in line. Pricing and a full release date are still unknown, so early testers will shape what this experiment becomes.
