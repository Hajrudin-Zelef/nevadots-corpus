---
id: collect-261121-veille-ia/google-ecosystem/google-built-a-walled-off-mirror-dimension-internet-to-train-and-test-chrome-s-ai-agents
title: "Google built a walled-off 'Mirror Dimension' internet to train and test Chrome's AI agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "chrome", "google", "built", "internet", "test", "train", "mirror"]
source: https://devisdead.ai/news/google-built-a-walled-off-mirror-dimension-internet-to-train-and-test-chrome-s-a
source_anchor: ""
source_lines: [1, 19]
section: "Google_Ecosystem"
---
To train its AI browsing agents safely, Google's Chrome team built an entire alternate internet it fully controls, Lily Hay Newman reports for WIRED.

## Why

Agents that use the web need practice on realistic sites and need hardening against attacks. Small test environments only go so far, and models can develop "eval awareness," acting differently when they sense a test. But loosing untested agents on the real internet is a bad idea. So Google built the Mirror Dimension, an isolated but sprawling web of social sites, shops and bank dashboards with all the parts of real ones. Agents complete tasks in Chrome while facing tests like hidden prompt-injection attacks.

Chrome AI security engineer Charles Qian says agents aren't programmed, the web is hostile and any user can ask anything on any site, so the space has to be explored thoroughly. He says the agent has started showing mature judgment about what information a task really needs and when to check back with the user.

## How it was built

The team used armies of web-development agents in a custom harness to churn out realistic services and data flows, down to promo codes on shopping sites. Because Chrome's protections would block a fake web, researchers essentially hacked their own browser, forging certificates so agents could run in real Chrome. They stress none of that works against ordinary users; it relies on owning the whole network.

The browsing agents tested have no cyber capabilities. Google's director of engineering Doug Turner says the goal is to share the approach inside and outside Google and keep making the environment more realistic.

**Scoreboard update:** AI +1. Agents built a whole fake internet so other agents can learn to browse safely.

Stay undead.

## Comments 0
