---
id: collect-261121-veille-ia/google-ecosystem/google-built-a-secret-mirror-dimension-internet-just-to-train-chromes-ai
title: "Google built a secret ‘mirror dimension’ internet just to train Chrome’s AI"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-20"]
keywords: ["google", "built", "fake", "chromes", "internet", "just", "chrome", "mirror"]
source: https://piunikaweb.com/2026/11/20/google-chrome-mirror-dimension-ai-training/
source_anchor: ""
source_lines: [1, 18]
section: "Google_Ecosystem"
---
Google has built a giant fake version of the internet just to stop Chrome’s AI browsing agents from causing chaos on the real web.

In a Chrome Security blog post, engineers Aaron Selya and Charles Qian showed off what they are calling the “mirror dimension.” It is a sealed-off sandbox packed with thousands of simulated websites, including fake online stores, social feeds, and bank dashboards, all built to train Chrome’s auto browse feature.

Testing AI agents on live sites is messy since pages change constantly and a confused bot can do real damage. Basic test environments have their own headache though, because newer AI models actually notice fake URLs or half-baked pages and start acting differently, which researchers call *“eval awareness.”*

To keep the AI fooled, Chrome lets the agent visit real addresses like YouTube while quietly routing traffic to local copies. Because Chrome normally blocks that with a privacy warning, engineers installed custom certificates inside the sandbox, something outsiders couldn’t pull off on a regular device.

Google doesn’t build these fake sites by hand either, relying on AI sub-agents to code and test working checkouts and promo codes. That fits right next to its recent Modern Web Guidance 1.0 release aimed at stopping AI tools from writing outdated, buggy webpages.

Once the fake web is running, engineers try to hijack the agent with sneaky hidden instructions, known as prompt injections, tucked into images or page code. They even had to hide test tags behind the scenes because the AI kept noticing strange links.

Speaking to WIRED, Qian explained why the team went to such lengths.

Agents are not programmed, so you can’t predict every possible thing an agent might see; the exploration space that we have to address is huge, and the web is hostile.


Google says this automated pipeline runs over 1,000 attack tests a day per engineer, roughly 30 times faster than manual testing. Meanwhile, we’ve already reported on Google admitting that it uses AI to find and eliminate bugs and security flaws in Chrome itself.
