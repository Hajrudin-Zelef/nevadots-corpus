---
id: collect-261121-veille-ia/actu-business/google-built-a-fake-internet-so-chromes-ai-doesnt-break-your-real-one
title: "Google built a fake internet so Chrome’s AI doesn’t break your real one"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["google", "real", "doesnt", "mirror", "built", "agents", "fake", "chromes"]
source: https://www.digitaltrends.com/computing/google-builds-fake-internet-to-train-and-test-gemini-ai-agents/
source_anchor: ""
source_lines: [1, 23]
section: "Actu_Business"
---
If you’ve watched any Doctor Strange movie, you might be familiar with the concept of the mirror world. It’s a world that reflects our own, but nothing happening there affects ours.

Google is doing the same thing for testing its AI. As the company is using its Gemini models to power its AI browsing features, there was a need to take extra precautions so it doesn’t completely upend our browsing experience.

In a blog post on its website, Google shared its solution. The company has created a mirror dimension of the web that it completely controls.

## What is “Mirror Dimension” Google created?

Google’s Mirror Dimension is similar to the concept of what we discussed. It’s a full copy of the web that nobody outside the team can visit, stocked with social networks, shopping sites, and bank dashboards built from the same parts real sites use.

Agents can freely roam around here, completing tasks the team assigns them. They also have to fend off attacks that the team uses to test their security and resilience.

## Why not just test the agents on the real web?

The web doesn’t follow a single protocol. Each website is different, so you cannot hard-code what an agent needs to do to navigate it. That means agents will have to use trial-and-error methods to learn how to execute a task.

And since these agents can cause real harm, Google just cannot let them lose on the web to do what they please. We are already seeing the impacts of untested AI code on the world, with multiple US services suffering several days of outages in the past few days.

Smaller test setups do not solve it either, since models can figure out they are being evaluated and behave differently while watched. A fake web that feels real fixes both problems, and building it took creative cheating, including forged certificates and workarounds for Chrome’s own safety checks, so the browser would treat an agent like any other visitor.

## So what does this mean for you?

Mostly that the agent showing up in your browser has already had thousands of bad days somewhere you will never see, and that is a good thing, because every mess it makes in there is one it hopefully won’t make on your actual internet.
