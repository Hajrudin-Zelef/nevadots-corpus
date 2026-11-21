---
id: collect-261121-veille-ia/microsoft-actu/microsoft-opens-its-decision-model-to-all-built-to-say-when-it-is-unsure-a-ai-insiders
title: "Microsoft opens its decision model to all, built to say when it is unsure â AI Insiders"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "model", "decision", "models", "built", "answers", "foundry", "request"]
source: https://talent.aiinsiders.net/article/microsoft-opens-its-decision-model-to-all-built-to-say-when
source_anchor: ""
source_lines: [1, 19]
section: "Microsoft_Actu"
---
Microsoft made Microsoft-Decision-1.1 generally available on November 17, a model that does not write answers but chooses between options and reports how sure it is. It is sold through Microsoft Foundry, OpenRouter, and Vercel. The pitch is that software can lean on a cheap, quick model for routine judgment calls and keep expensive models for hard work.

A decision model is the plain idea here. A chatbot produces text. A decision model takes a request, looks at a list of actions the developer has allowed, and picks one. Think of a mail room clerk who sorts each parcel into a bin and, when a label is smudged, sets it aside for a human instead of guessing. The model returns its pick plus a confidence score, and the developer sets the threshold for when the app acts, flags, or escalates.

Achint Srivastava, a VP of Software Engineering in Microsoftâs Office of the CTO, wrote the announcement on the companyâs Command Line blog. His argument is that general models are rewarded for finishing tasks and giving answers, so they tend to assume things and sound confident even when they are wrong. Microsoft trained this one to be consistent across similar cases, to split its confidence when two answers are both valid, and to abstain when information is missing. Microsoft lists routing, classification, and prioritization as typical jobs, plus verification and steering a workflow. Version 1.1 also takes images, including photos and screenshots.

Under the hood, Microsoft started from Gemma 4 26B A4B, a mixture-of-experts model of middling size, and ran it on a custom inference engine built for decisions instead of token-by-token writing. It runs on A100, H100/200, and GB200/300 hardware. Training paired supervised fine-tuning with reinforcement learning. The data came from hundreds of synthetic and licensed sets covering tens of thousands of task types.

The performance claims are Microsoftâs. On the Artificial Analysis Decision Index v2.1, which scores quality, calibration, and latency, the model scores 68, a gain of six points over version one. Microsoft says that beats Jev 1.14, Googleâs Gemini Decision 1, and OpenAIâs GPT-6 Luna (Decisions). Speed is more than double the first model at the same cost. Separately, Microsoft ran 102 benchmarks covering more than 385,000 questions held out of training. It reports the top average accuracy among eight decision models and first place in seven of the nine capability categories. Those internal tests have not been independently reproduced.

Adoption numbers are early. More than 4,000 customers tried it in the roughly five weeks after the public preview launched last month, for steering requests between models and sorting support tickets, among other uses such as tagging, classification, and automating business processes. Named early users are Epic, Equinor, Atos Group, and Raiffeisen Bank International.

Inside Microsoft, Copilot uses it to decide whether a request goes to Copilot or Cowork. Microsoft says its accuracy on that call matches an LLMâs, with 2.2x the speed and 4.4x lower cost. In Cowork it powers Auto, which tunes how much reasoning effort each task gets. It can also act as a judge in Foundry Evaluations, and Microsoftâs engineers who carry the pager use it to pull relevant knowledge during live incidents.

Pricing in Foundry covers input only, because output tokens are free. The rate is $0.042 per million tokens on Global Standard, $0.046 in the US Data Zone, and $0.05 in the EU and APAC Data Zone. At those rates, a router that reads a few thousand tokens per request costs a fraction of a cent, which changes the math on putting a gate in front of every call.

The test for any team building agents is its own traffic: run the router and the abstain threshold against your logged requests, and compare escalation rates with what a large model costs today.

*Microsoft, Command Line blog (commandline.microsoft.com), by Achint Srivastava, November 17, 2026.*
