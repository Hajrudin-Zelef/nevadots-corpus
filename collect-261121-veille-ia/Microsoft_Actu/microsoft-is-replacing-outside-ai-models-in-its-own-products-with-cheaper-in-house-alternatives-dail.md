---
id: collect-261121-veille-ia/microsoft-actu/microsoft-is-replacing-outside-ai-models-in-its-own-products-with-cheaper-in-house-alternatives-dail
title: "Microsoft is replacing outside AI models in its own products with cheaper in-house alternatives · Daily AI Report"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-19"]
keywords: ["microsoft", "code", "percent", "gpt-6", "models", "model", "flash", "costs"]
source: https://www.wyx.asia/article/ea5534b4d9f6502f207fef2c5ac563aa?lang=en
source_anchor: ""
source_lines: [1, 97]
section: "Microsoft_Actu"
---
The Decoder ↗ · 2026-11-19 02:47 · UTC+8

# Microsoft is replacing outside AI models in its own products with cheaper in-house alternatives

Media reports The Decoder Source language: English

Microsoft's AI division MAI unveiled seven new models for code, security, images, language, and routing at Ignite.

## Full text

Key Points

Microsoft's AI division unveiled seven specialized models for code, IT security, image editing, speech, and workflow decisions at its Ignite conference. All are aimed at cutting operating costs for always-on AI agents.

The models target OpenAI's and Anthropic's systems as cheaper alternatives. The coding model MAI Code 2 nearly matches GPT-6.1 Sol and Claude Opus 5.5 on internal benchmarks at a fraction of the cost. MAI Cyber 2 Flash edges past GPT-6 Astra on the CyberGym-E2E vulnerability test.

Beyond the image model MAI Image 2.7, the speech model MAI Realtime 1, and the Google Gemma-based routing model Decision 1.1, Microsoft will let enterprise customers fine-tune selected model weights with their own data in the Azure cloud.

Microsoft AI (MAI) is targeting always-on AI agents with new models for code, security, images, speech, and routing. The main yardstick is the lineup of its own partner, OpenAI.

At its Ignite conference, MAI unveiled seven new models, releasing them in a single batch rather than spread across the year, just as it did at Build in June. Microsoft is positioning the new lineup primarily on price-to-performance. The company cites rapidly rising token spending among customers that want to run AI agents around the clock. Its code models target everyday enterprise coding tasks and aren't yet optimized for harder work such as foundational research or long-horizon agentic workflows, Microsoft says.

MAI Code 2 approaches GPT-6.1 Sol and Opus 5.5 at a promotional price

According to Microsoft, MAI Code 2 averages 98 percent of GPT-6.1 Sol's performance across four benchmarks at 40 percent of the cost. Against Claude Opus 5.5, it reaches 96 percent of the performance at 5 percent of the cost. Microsoft tested on DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, and FrontierCode v1.1. Three of the tests ran on Microsoft's internal MSBench platform using the GitHub Copilot CLI without sub-agents. Microsoft is calculating a 50 percent promotional discount for its own model.

In blind comparisons of frontend work such as UIs, games, data visualizations, and SVG graphics, human evaluators preferred MAI Code 2's output 64 percent of the time over GPT-6 Sol, Microsoft says.

MAI Code 2 trails GPT-6.1 Sol and Opus 5.5 slightly on success rate but costs only a fraction per task, calculated at the promotional price. | Image: Microsoft

The successor to MAI-Code-1.1-Flash, MAI Code 2 Flash, is designed for code explanations, targeted edits, and routine fixes. It's said to match GPT-6 Luna and Claude Haiku 5.5 at 65 and 20 percent of their respective costs.

Microsoft derived MAI Work Flash from the Flash model. It runs in Microsoft 365 and Fabric IQ. On internal Excel and PowerPoint benchmarks, it matches GPT-6 Luna, Microsoft says. In Word, it performs similarly to GPT-6.1 Sol while consuming about 42 percent fewer input tokens. For generating DAX queries in Power BI, it outperforms GPT-5.6 Sol, GPT-6 Astra, and Opus 5.5.

In GitHub Copilot, MAI Code 2 costs $0.50 per million input tokens and $2.50 per million output tokens at the promotional price through May 17, 2027, then $1 and $5. MAI Code 2 Flash costs $0.05 and $0.25 until then, rising to $0.10 and $0.50.

Security model with a cleaned-up benchmark

MAI Cyber 2 Flash solves 92.3 percent of the tasks in the CyberGym-E2E vulnerability benchmark, Microsoft says. GPT-6 Astra reaches 91.5 percent but needs twice the reasoning budget. Per task, the MAI model costs less than 2 percent of GPT-6 Astra, with per-token costs at one hundredth. On WebExploitBench, it scores 40.4 percent, ahead of GPT-5.6 Cyber at 38.4 percent.

MAI Cyber 2 Flash solves slightly more tasks than GPT-6 Astra on CyberGym-E2E while using fewer tokens at a fraction of the cost. | Image: Microsoft

During evaluation, Microsoft found that 10 percent of the roughly 900 CyberGym tasks draw on the same open-source projects and vulnerability reports as parts of its own training data. The reference solutions never entered training, the company says. Microsoft removed those tasks for all models anyway and scored only the rest.

A comparison with GPT-6 Cyber is missing so far. The model is available only to selected customers. Agentic benchmarks with tools still need extra safety measures, Microsoft says. The company plans to publish those results in the coming weeks.

The context window spans one million tokens, four times the size of MAI Cyber 1.1 Flash. Output tokens cost 86 percent less than the predecessor's. MAI Cyber models already handle nearly 40 percent of scans on Microsoft's security platform MDASH, the press release states.

Microsoft lists MAI Cyber 2 Flash at $0.10 per million input tokens and $0.50 per million output tokens, compared with $10 and $50 for GPT-6 Astra. On CyberGym-E2E, that works out to $0.45 per task instead of $34.38.

Microsoft's decision model now runs on Google's Gemma

Microsoft-Decision-1.1 handles routing, classification, prioritization, and workflow control. The model picks from developer-approved actions and returns calibrated confidence scores. When uncertainty is high, apps can escalate instead of acting on a wrong decision. If information is incomplete, the model is designed to abstain.

Its foundation is Google's mixture-of-experts model Gemma 4 26B A4B. The predecessor was built on Qwen3.5-9B. Microsoft trained the new model with supervised fine-tuning and reinforcement learning on hundreds of synthetic and licensed datasets and runs it on a proprietary inference engine.

On the Decision Index v2.1 from Artificial Analysis, Decision 1.1 scores 68 points. That's six more than its predecessor and above Jev 1.14, Gemini Decision 1, and GPT-6 Luna (Decisions). Microsoft also tested it on 102 internal benchmarks with more than 385,000 questions, where it achieves the highest average accuracy among eight decision models.

The model now accepts images, including screenshots. In Copilot, it decides whether a request goes to Copilot or Cowork. It performs as accurately as a large language model but 2.2 times faster and 4.4 times cheaper, Microsoft says. In Cowork, its Auto function controls how much reasoning effort a task receives.

Microsoft charges $0.042 per million input tokens, or $0.05 in the EU data zone. Output tokens are free.

On the Decision Index from Artificial Analysis, Decision 1.1 gains six points over its predecessor and responds more than twice as fast. | Image: Microsoft

Another specialized model appears in the Book of News for Ignite. Microsoft-Retriever-1 gathers relevant information from enterprise documents for agents in Foundry IQ. Instead of running a single search, the model plans its queries, evaluates the results, and issues follow-ups until it has enough material for a well-grounded answer. Microsoft says that's faster and more efficient than previous methods. Retriever-1 launches as a preview. The Model Router in Foundry now also selects automatically from an expanded pool of models.

Image editing just ahead of OpenAI

MAI Image 2.7 leads the Artificial Analysis image-editing leaderboard with 1,215 Elo points from 5,478 votes. It's four points ahead of GPT Image 2.6 Sunburst (max) at one-fifth the cost. For text-to-image generation, the model ranks fourth on that leaderboard. MAI Image 2.7 costs about $35 per 1,000 images. Flash costs about $10.

According to Artificial Analysis, MAI Image 2.7 and 2.7 Flash match GPT Image 2.6 Sunburst's editing quality at much lower prices. | Image: Microsoft

The model can change the color, material, or position of individual objects without altering the rest of the image. It also generates transparent backgrounds. The smaller Flash variant costs less than a third and powers Canvas, a new image editor in MAI Labs. MAI Image has been the default model in Bing Image Creator since June. Adobe, Figma Weave, Minute (Sports Illustrated), Wirestock, and Manus work with MAI Image, Microsoft says.

MAI Image 2.7 is designed to edit and isolate individual elements of an image without altering the rest. | Image: Microsoft

Speech model compatible with OpenAI's API

MAI Realtime 1 is Microsoft's first speech-to-speech model. It's designed to handle interruptions, wait until speakers finish, and recognize requests even with background noise. For harder tasks, it falls back to a more powerful reasoning engine billed per token. The base rate is charged per audio minute. On Big Bench Audio, τ-Voice, and tests with Dynamics 365, it outperforms GPT Realtime 2.1, Microsoft says.

Because it's compatible with the OpenAI Realtime API, existing voice apps can switch over easily. Agoda and Reservations.ai are testing the model. Microsoft also plans to bring it to the Dynamics 365 Contact Center and Dragon Copilot, where nurses would use it to practice patient conversations.

MAI Realtime 1 scores highest on Big Bench Audio at comparatively low cost, according to Microsoft's own measurement. | Image: Microsoft

Dragon Copilot Transcribe, based on MAI Transcribe 2, now handles all doctor-patient transcriptions in Dragon Copilot. That amounts to 10 million conversations per month, Microsoft says. In multilingual workflows, the error rate drops by half. For its own large models, Microsoft now uses the Maia 200 chip, the second generation of its AI accelerator, said to deliver 1.7 times more tokens per watt.

Microsoft charges one US cent per audio minute. Reasoning costs an extra $1 per million input tokens and $5 per million output tokens.

Licensed weights available only through the Microsoft account team

With MAI Licensed Weights, companies can fine-tune MAI models with their own data and run them in their own Azure environment. Data and customizations stay with the customer. Microsoft says it will use them for training its own models only with explicit consent.

The company positions the offering as an alternative to open weights, whose provenance and behavior are hard for enterprises to verify. One example is US carrier AT&T, where a customized model for 5G network operations analysis reportedly outperformed GPT-6.1 Sol on standard tasks at roughly one-fiftieth of the cost. A launch date and pricing are still pending. Interested companies should contact their Microsoft account team. The offering is set to arrive "shortly" through Microsoft's Frontier Company program.

MAI Code 2 and Code 2 Flash are available in GitHub Copilot, with Code 2 Flash also in a limited preview in Foundry. OpenRouter, Vercel, Hermes, Open Code, and Kilo are expected to follow. MAI Image 2.7 and Flash are in public preview in Foundry and on fal, Genspark, OpenRouter, and Vercel. Decision 1.1 is generally available in Foundry, on OpenRouter, and through Vercel's AI Gateway. MAI Realtime 1 can be tried in MAI Labs and is available as a Foundry preview, free until the public preview launches. MAI Cyber 2 Flash is exclusive to Microsoft's security products. In MDASH, it will automatically replace the existing model in the MAI profile for eligible customers starting mid-December.

In April, Microsoft and OpenAI restructured their partnership. OpenAI can now sell its products through any cloud provider, but Azure remains the primary cloud partner. Microsoft holds a non-exclusive license to OpenAI's models and products through 2032.
