---
id: collect-261121-veille-ia/google-ecosystem/google-builds-isolated-web-replica-to-test-chrome-ai-agents-and-prompt-injection-defenses-mallory
title: "Google Builds Isolated Web Replica to Test Chrome AI Agents and Prompt-Injection Defenses | Mallory"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "chrome", "google", "isolated", "test", "prompt-injection", "builds", "replica"]
source: https://mallory.ai/stories/01a1f4fc-1ccb-71d0-a847-5a624004be02
source_anchor: ""
source_lines: [1, 7]
section: "Google_Ecosystem"
---
# Google Builds Isolated Web Replica to Test Chrome AI Agents and Prompt-Injection Defenses

Emerging Technology Risk · 8 sources · Updated 14h ago · First seen Nov 19, 2026

Google’s Chrome team built the **Mirror Dimension**, an isolated web environment for training and evaluating AI browsing agents as it develops Gemini-powered features. Synthetic social networks, shopping sites and banking dashboards let agents perform tasks in real Chrome without exposing public websites to unintended actions. A local interception proxy supports realistic browsing, while the environment prevents side effects from persisting between runs and provides a clean, repeatable baseline—addressing the instability and safety risks of testing on live websites.

The environment also exposes agents to security challenges, including **hidden prompt-injection attacks**, allowing researchers to assess behavior under controlled conditions. Researchers used privileged control of the test environment to bypass browser checks, including creating forged web certificates; Google emphasized that these techniques would not work against regular Chrome users on the open internet. Google plans to expand access to more internal teams and improve realism. For security leaders evaluating browser agents, the work highlights the importance of repeatable, isolated testing that measures resistance to malicious web content without risking real-world transactions or other unintended actions.
