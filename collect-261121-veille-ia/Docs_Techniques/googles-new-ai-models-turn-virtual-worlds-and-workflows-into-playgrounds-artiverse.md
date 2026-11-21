---
id: collect-261121-veille-ia/docs-techniques/googles-new-ai-models-turn-virtual-worlds-and-workflows-into-playgrounds-artiverse
title: "Google’s New AI Models Turn Virtual Worlds and Workflows Into Playgrounds - Artiverse"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["google", "neon", "genie", "worlds", "models", "gemini", "workflows", "carbon"]
source: https://www.artiverse.ca/googles-new-ai-models-turn-virtual-worlds-and-workflows-into-playgrounds/
source_anchor: ""
source_lines: [1, 47]
section: "Docs_Techniques"
---
Google is opening two powerful AI experiences to more users and developers, linking responsive virtual worlds with models built for demanding real-world work. Genie 4 now reaches eligible Google AI Ultra subscribers through Project Genie, while Gemini 4 Neon brings fast, high-quality reasoning to software engineering, research, business tasks, and science workflows.

The two releases point in different directions, yet they share one goal: giving AI systems more useful environments to understand and change. Genie 4 creates interactive worlds that respond to keyboard controls and live prompts. Neon follows evidence across records, tests ideas, and produces artifacts that other people can inspect and edit.

## Genie 4 Brings Interactive Worlds To Developers

Google made Genie 4 available through the Gemini API and Google AI Studio on November 19, 2026, after first releasing it to “trusted testers” late last month. Eligible Google AI Ultra subscribers can now use the model in Project Genie, while the November 18 Project Genie update allowed Google AI Pro members to join a waitlist.

The developer release targets brief, controlled simulations instead of hosted worlds that applications retain and revisit across sessions. Developers use models/genie-4 through the v1beta Interactions API, creating a session that connects through WebRTC or WebSocket before sending a world setup event.

Genie 4 generates continuous 720p video at 24 frames per second with synchronized native audio. A world can begin with text, one reference image, or both, then respond to WASD and arrow-key controls for movement and camera orientation. Text events can modify the scene or supply character dialogue, creating a responsive experience rather than an offline video clip with a fixed ending.

- Sessions last up to 120 seconds.
- The strongest period for visual coherence and object permanence is 45 to 90 seconds.
- Developers must call complete() or cancel() to stop server generation and billing.
- Closing a browser tab or detaching the local transport does not immediately stop generation.
- Every output carries imperceptible SynthID watermarking.

Google lists interactive experience prototypes, spatial visualization, and training robotics and AI agents among the possible uses. WebRTC is recommended for interactive browser applications, while WebSocket supports Python and Node.js workflows, frame capture, and custom pipelines. Browser applications can use an ephemeral token limited to one interaction, keeping the long-lived API key on the backend.

## Neon Targets Complex Work Without Carbon’s Cost

Google also launched Gemini 4 Neon earlier today across the paid Gemini API in Google AI Studio and Android Studio, the Gemini app, and Google Antigravity. Google AI Pro and Ultra subscribers can access Neon through those consumer and development experiences.

Google describes Neon as bringing “frontier-level intelligence with fast end-to-end task completion across complex software engineering, knowledge work, and science workflows.” In early evaluations, it matched or exceeded Gemini 4 Carbon on overall quality at 3x the speed and 60% of the cost, according to Thai Tran.

Google places Neon between Hydrogen, its small, light, fast workhorse, and Carbon, the model recommended for the hardest extended projects. Google also describes Neon as sitting between Carbon and Neon, while its broader lineup places the model between Hydrogen and Carbon. The company summarizes the intended balance this way: “While Carbon tackles your hardest, long-horizon projects and Hydrogen handles fast everyday execution, Neon balances depth and efficiency.”

At its highest setting, Neon edges past Carbon on the general intelligence index. On the professional tasks measured by Vals, all three Google models finish within about 2.5 percentage points of one another, and Google supplies Neon’s Vals result. Artificial Analysis confirmed on X that Neon placed third on its intelligence index, though the peer results come from public leaderboard snapshots that predate Neon’s release.

Reasoning settings complicate direct comparisons because providers define them separately. Two models running at maximum effort can spend different amounts of time and money on the same question, so speed, cost, and quality must be read together.

## From Billing Records To Working Spreadsheets

Neon’s appeal rests on tasks that require more than a single answer. It can trace an unexpected charge across billing records, find flaws in unfamiliar software, test research hypotheses, and build spreadsheets that colleagues can inspect and change.

Google suggests asking Neon to examine utility statements and explain why a charge rose. A customer can ask it to cross-check receipts and other records when seeking a refund, and the model can examine PDFs, spreadsheet entries, and photographs together. Neon has to follow evidence through several steps, then produce something another person can use.

Thoughtful Lab founder Karina Nguyen said Neon helped her team understand complex software architectures quickly. In the firm’s security evaluations, she said, it also spotted subtle vulnerabilities that other models missed. Cartwheel co-founder and chief scientist Andrew Carr described using Neon in machine-learning research, where it examined prior work, tested hypotheses, and incorporated feedback as an experiment progressed.

A Google demonstration in Sheets shows how far that workflow can go. Google gave a group of Neon agents a single image, and the agents reconstructed it across 100,000 spreadsheet cells using more than 500,000 formulas to identify edges, map depth, and change the lighting. They did it without embedding the original picture or writing a script.

Neon costs $3 per million input tokens and $15 per million output tokens. Beginning December 1, its input and output rates are 25% below Carbon’s scheduled rates. Anthropic’s larger flagship Opus 5.5 has standard rates of $4 and $20, while Neon scored within about a percentage point on two engineering benchmarks.

Neon also outscored GPT-6.2 Sol on business automation, while Sol read dense professional charts more accurately. Under OpenAI’s long-context pricing, a Sol request exceeding 272,000 input tokens costs $4 per million input tokens and $15 per million output tokens for the whole request, removing its headline output-price advantage at that length.

Genie 4 and Neon now give developers two distinct ways to push AI beyond chat: one generates worlds that respond in real time, and the other turns complex evidence into useful work. With paid API access, session controls, usage quotas, and new pricing in view, Google is making both experimentation and practical deployment easier to reach.
