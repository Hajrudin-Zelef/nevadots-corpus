---
id: collect-261121-veille-ia/google-ecosystem/google-mirror-dimension-is-a-closed-web-environment-to-train-chrome-ai-agents
title: "Google Mirror Dimension is a closed web environment to train Chrome AI agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "google", "chrome", "mirror", "dimension", "environment", "built", "train"]
source: https://moneycontrol.com/technology/google-mirror-dimension-is-a-closed-web-environment-to-train-chrome-ai-agents-article-14085484.html/amp
source_anchor: ""
source_lines: [1, 17]
section: "Google_Ecosystem"
---
Google has built an isolated version of the internet, called Mirror Dimension, to train and test the AI browsing agents its Chrome team is developing with Gemini models. As reported by WIRED, the company described the project in a blog post on November 19. Google fully controls the environment, where agents complete tasks on realistic websites without reaching the public web.

**Why Google needed a separate web**

Browsing agents have to understand how real websites work and how to resist web-based attacks. The Chrome team found that small test setups were of limited use for this training. Models can also tell when they are being evaluated and change their behaviour, an issue known as eval awareness. At the same time, releasing untested agents on the live internet poses risks, so Google built a closed alternative.

**What Mirror Dimension includes**

The environment hosts social media platforms, shopping sites and banking dashboards, each built with the structure and features of real websites. Agents work inside Chrome and complete tasks as they would online. Along the way, they face security tests such as hidden prompt-injection attacks. Charles Qian, a software engineer on the Chrome AI security team, told WIRED that the agents have become better at judging what information a task needs and when to ask the user for confirmation.

**How Google built it**

To build at scale, the team used groups of website and app development agents guided by a custom AI harness. These agents produced working services and data flows, from promo code fields on shopping pages to populated social media profiles. To run standard Chrome inside the setup, researchers had to work around the browser’s safeguards, including by creating forged web certificates. Google said these methods rely on full control of the network and cannot be used against regular Chrome users.

**Wider access planned**

Google said the agents tested so far have no cyber capabilities. It plans to open Mirror Dimension to other teams across the company. Doug Turner, Google’s director of engineering, said Google will continue to invest in the approach and make the environment more realistic as models advance.
