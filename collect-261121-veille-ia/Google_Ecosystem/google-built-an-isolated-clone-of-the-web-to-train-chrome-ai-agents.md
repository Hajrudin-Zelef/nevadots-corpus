---
id: collect-261121-veille-ia/google-ecosystem/google-built-an-isolated-clone-of-the-web-to-train-chrome-ai-agents
title: "Google built an isolated clone of the web to train Chrome AI agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "chrome", "google", "built", "train", "team", "isolated", "sites"]
source: https://pivotnews.ai/five/google-built-an-isolated-clone-of-the-web-to-train-chrome
source_anchor: ""
source_lines: [1, 17]
section: "Google_Ecosystem"
---
# Google built an isolated clone of the web to train Chrome AI agents

The Chrome team's self-contained “Mirror Dimension” fills fake social, shopping and banking sites with hidden prompt-injection attacks so Gemini-powered browsing agents can be stress-tested without touching the live web, and Google now wants to open it to other internal teams.

Google's Chrome team has built an entirely controlled alternate-universe internet to train and test the AI browsing agents it is developing with the company's Gemini models. The environment, called the “Mirror Dimension,” was detailed by Google in a blog post on November 19 and reported the same day by WIRED.[1]

The reasoning behind it is practical. Web-use agents need to learn how real sites and platforms behave and how to resist web attacks, so narrow test environments only go so far. Models can also develop “eval awareness,” behaving differently once they work out they are being tested. Turning unrefined agents loose on the open web carries its own risks. The Mirror Dimension sits between those two options: an isolated but sprawling ecosystem of social media sites, shopping sites and bank dashboards built with the components and attributes of real websites, where agents complete tasks in Chrome and face resilience tests along the way, including hidden prompt-injection attacks.[1]

## Evidence

"Agents are not programmed so you can't predict every possible thing an agent might see, the exploration space that we have to address is huge and the web is hostile," Charles Qian, a software engineer on the Chrome AI security team, told WIRED. "Any user can ask any prompt on any website, so we really need to explore that space as thoroughly as we can. And we've seen the agent start to develop a mature sense of judgement when it comes to knowing what information is actually needed for a task and when to punt back to the user for a confirmation."[1]

Populating a fake web at that scale was its own engineering problem. The team used large numbers of website and web app development agents, directed by a purpose-built AI harness, to generate realistic services and data flows down to promo code fields on shopping sites and filled-in social media pages. Running agents inside real Chrome, which the team considers necessary for accurate training, meant working around the browser's own protections: researchers effectively hacked Chrome, including forging web certificates to satisfy validity checks. Google says none of those techniques would work against ordinary Chrome users, because they depend on the team's total control of the environment and network.[1]

## What's next

The browsing assistant agents tested so far are benign and have no cyber capabilities, according to the researchers, who want to open the Mirror Dimension to other teams across Google. “We're sharing this approach so other teams both internally and externally think about doing it this way,” said Doug Turner, Google's director of engineering. "This is the first step, there's going to be continued investment in this approach to train models. And I would imagine as they become more sophisticated we'll have to continue working on this environment to make it more and more realistic."[1]
