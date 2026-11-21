---
id: collect-261121-veille-ia/google-ecosystem/google-built-a-simulated-internet-to-train-its-chrome-browsing-agents
title: "Google built a simulated internet to train its Chrome browsing agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["chrome", "agents", "simulated", "team", "google", "browsing", "internet", "built"]
source: https://www.aiaffairs.com/technology/google-mirror-dimension-chrome-browsing-agents/
source_anchor: ""
source_lines: [1, 27]
section: "Google_Ecosystem"
---
Google disclosed Mirror Dimension, a simulated internet built to train its Chrome browsing agents, in a blog post on 19 November, Wired reported. The Chrome team controls the environment, allowing agents to practise navigating websites and completing tasks without taking actions on the live web.

## Key points

- Mirror Dimension contains simulated social media, shopping and banking sites that agents use inside Chrome.
- The agents encounter hidden prompt-injection attacks as they work through tasks.
- The team altered Chrome and forged web certificates so the agents could train in the real browser.

## Mirror Dimension gives Chrome agents a practice web

The simulated sites include social media pages, shopping platforms and bank dashboards, built with the components and attributes of real websites. Agents browse them in Chrome and complete tasks, including ones that expose them to hidden prompt injections — instructions placed in web content to steer an agent away from its task.

One difficulty with a conventional test site is “eval awareness”: a model may recognise that it is being tested and behave differently from the way it would on the open web. The Chrome team used website-building agents directed by a purpose-built AI harness to populate Mirror Dimension with varied services and interactions, down to shopping promo codes and social media pages.

Charles Qian, a software engineer on the Chrome AI security team, said the team had seen agents improve their judgement about which information a task requires and when to ask a user for confirmation. The researchers described the browsing agents they tested as benign and without cyber capabilities.

## Chrome checks complicated the simulated sites

Training in the real Chrome browser required the researchers to work around its protections and validity checks, including by forging web certificates inside the controlled network. A certificate is part of how a browser checks the identity of a website. The team said those workarounds depended on its control of the environment and network and could not be used against ordinary Chrome users on the open internet.

Doug Turner, Google’s director of engineering, said the company wanted other teams inside and outside Google to consider the approach. He said the team expected to keep making the environment more realistic as agents became more sophisticated.

## Auto Browse runs on Gemini 3

Google began rolling out Auto Browse, a Gemini 3-powered Chrome feature, on 28 January 2026 for AI Pro and AI Ultra subscribers in the US, Crypto Briefing reported. The feature gives AI Pro subscribers 20 browsing tasks per day and AI Ultra subscribers 200.

Researching flights, filling out forms, managing subscriptions and collecting tax documents are tasks Auto Browse handles. It asks for explicit confirmation before sensitive actions such as logging in or making a purchase.
