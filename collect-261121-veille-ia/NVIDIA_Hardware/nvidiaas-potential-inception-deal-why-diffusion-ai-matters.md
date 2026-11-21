---
id: collect-261121-veille-ia/nvidia-hardware/nvidiaas-potential-inception-deal-why-diffusion-ai-matters
title: "Nvidiaâs Potential Inception Deal: Why Diffusion AI Matters"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["inception", "mercury", "more", "diffusion", "nvidia", "acquisition", "deal", "reports"]
source: https://www.theneuron.ai/news/nvidia-inception-acquisition-diffusion-ai/
source_anchor: ""
source_lines: [1, 118]
section: "NVIDIA_Hardware"
---
*Inception is shutting down its Mercury API as reports point to a potential Nvidia acquisition. For a company already delivering capable AI at more than 1,000 tokens per second, this could be the beginning of something much bigger.*

Iâve been bullish on Inception for more than a year. Iâve interviewed cofounder Stefano Ermon, tested the companyâs Mercury models, and watched its diffusion approach get steadily more capable without giving up the speed that first caught my attention.

So yes, the possibility of Nvidia buying this company has me excited.

Inception has already built AI that, by its own benchmarks, competes with GPT-6 Luna while generating more than 1,000 tokens per second. I want to see what this team can build with more money, more compute, and the engineers who know that hardware better than anyone.

We donât know whether a deal will happen. But thereâs a pretty compelling reason to hope this technology gets that opportunity.

## Whatâs reported, and whatâs confirmed

The Information reports that Nvidia is nearing an acquisition of Inception. TheFlyâs summary of the reporting describes advanced discussions and a proposed price of at least $500 million.

Thatâs a reported deal, not an announced acquisition.

Nvidia also isnât the first company linked to Inception. Reuters reporting summarized by Techmeme in May described acquisition discussions with Microsoft, earlier interest from SpaceX, and an asking price above $1 billion. Those earlier reports donât tell us whoâs still interested or what the eventual terms might be.

What we can confirm is that Inceptionâs current API is going away.

The companyâs official wind-down notice says the Mercury API and platform dashboard will remain available through December 28, 2026. After that, requests will stop being served and customers will lose access to the platform and its data. Inception tells developers to save their records and migrate their integrations before the cutoff.

The notice doesnât explain the decision or connect it to an acquisition.

For anyone who built a product around Mercury, thatâs a frustrating situation. The possibility of a great next chapter doesnât keep an existing application running. Those customers need to make plans now.

But closing an API doesnât tell us what happens to the technology behind it.

## Mercury changes how AI generates an answer

Most familiar language models generate text one token at a time, with each new token depending on what came before it. This is called autoregressive generation.

Inceptionâs diffusion models can work on multiple tokens in parallel, refining blocks of text over successive passes. Think of an editor improving several parts of a draft as the answer takes shape.

Thereâs one distinction worth getting right: this doesnât mean Mercury has abandoned transformers. Inceptionâs original announcement explicitly describes a transformer making those revisions. Diffusion changes the generation process.

And that change can get more useful work out of the hardware on each pass.

When I spoke with Ermon for The Neuron, he described the benefit simply: âfor the same cost, you get more things out.â

That idea stuck with me. If you can improve how the model uses the GPU, you have another way to make AI faster and cheaper. You can also spend some of that advantage on a more capable model.

The question has been how far that approach can go.

## The quality is catching up. The speed is still there.

With Mercury 3, Inception reports 1,068 tokens per second on Nvidia GPUs, alongside quality matching or exceeding GPT-6 Luna and Claude Haiku 5.5 across a range of benchmarks.

The company also says Mercury 3 has more than 40 times the total parameters of Mercury 1 while retaining similar generation speed.

But look at whatâs already on the table: a model that can compete on useful work while generating at an extraordinary pace.

Thatâs plenty to get excited about.

In my Mercury 3 review, I ran eight API tests covering writing, business arithmetic, explanations, coding, hallucination resistance, and logic. The reported total cost was about four cents.

The coding output particularly impressed me. Mercury produced a substantial single-file dungeon crawler, and the JavaScript passed a syntax check. It also passed 2,000 randomized map-generation checks for the conditions I tested.

It had a few rough edges elsewhere. It missed some writing constraints and introduced some incorrect supporting details. My tests werenât a controlled benchmark study though.

Still, I came away excited about their future and putting it to use in tools like OpenClaw right away. This is useful technology with room to improve, and the pace of improvement was encouraging.

## Fast agents can do more before you lose patience

Speed matters much more when a model gets called repeatedly.

A browser agent might inspect a page, choose an action, read the result, and decide what to do next. A coding agent might generate a change, run a tool, examine an error, and try again. Each model call adds another wait.

Save two seconds across each of 20 sequential calls and youâve saved 40 seconds. Thatâs a simple illustration, but anyone who regularly uses agents knows how noticeable those waits become.

Faster generation wonât fix a slow website or make a test suite finish instantly. It can reduce the time spent waiting on the model, though. That leaves room for another attempt or another check within the same time budget.

This is why I care about the combination of speed and quality. A fast model that needs constant correction isnât much help. A capable model that can keep an agent moving is something developers can build around.

## Nvidia could give this team a much bigger shot

Inception announced a $50 million financing round in 2025. It has already taken diffusion language models from research into commercial use.

Iâd love to see what happens when the team can run larger experiments, train more ambitious models, and put more engineers into the software that makes them run well.

More money doesnât guarantee a frontier model. There are still difficult research problems here, and the speed advantage has to hold up as capability grows. But those are problems worth putting serious resources behind.

Nvidia is an especially interesting potential owner because itâs already exploring the approach.

Its Nemotron-Labs Diffusion work includes autoregressive generation, diffusion, and a mode that uses diffusion to draft tokens for autoregressive verification. Nvidia has released model weights and training code, and its researchers describe how parallel generation can improve GPU utilization.

My read is that Inception could bring valuable commercial experience to an effort Nvidia already wants to pursue.

The business logic makes sense, too. Better models that run efficiently on Nvidia hardware could make more applications affordable to build. That gives Nvidia a reason to keep pushing the technology.

## And yes, Iâd love to see open weights

This is where my optimism gets a little ahead of the available facts, so letâs be clear: thereâs no confirmed plan to release Mercuryâs weights.

Iâd love to see it happen.

Nvidiaâs existing model releases make it a possibility worth considering. Researchers could test the claims more thoroughly. Developers could adapt the models to specialized work. Teams running their own infrastructure could experiment with serving them more efficiently.

That would give a lot more people a chance to find out what diffusion language models are good at, where they still struggle, and how to improve them.

The release terms would matter. Downloadable weights arenât automatically the same thing as open source, and access to training code or data would be separate questions.

But wider access could give this work a much larger community of builders.

For now, customers have a real shutdown deadline and the acquisition remains a report. Neither should get lost in the excitement.

Iâm still rooting for this one. Inception has already built something impressive. Put that team alongside Nvidiaâs engineers, give it the compute to keep experimenting, and letâs see how close diffusion can get to the frontier.

I really want to try that next model.

## More on Inception and diffusion AI

- **I Tested Mercury 3. Diffusion Language Models Just Got Serious.** â My hands-on tests of Mercury 3, including what impressed me, where it stumbled, and what eight API runs actually cost.
- **Inceptionâs Mercury 2: The First Reasoning Diffusion Model** â How Inception brought reasoning to its diffusion models, and why the combination of speed and intelligence matters for agents.
- **Inception AI Mercury Text & Code Diffusion Models Carve a New Path** â My conversation with Stefano Ermon about generating text in parallel and getting more useful work out of the same GPUs.
- **Diffusion LLMs Just Got Their First Serious Transparency Test** â A look at research exploring whether we can understand how diffusion models reason as they refine an answer.
- **Watch: The $0.25-Per-Million-Tokens AI Model That Feels Like Magic** â Grant Harvey and I sit down with Ermon on The Neuron podcast to discuss Mercury, GPU efficiency, model quality, and the possibilities for real-time AI.
- **Watch: Our original Inception interview with Stefano Ermon** â The conversation at AWS re:Invent about how diffusion works for text and why Ermon believes it can make language models faster and cheaper.
