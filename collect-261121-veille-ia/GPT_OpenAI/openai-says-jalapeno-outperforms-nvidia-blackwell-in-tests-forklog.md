---
id: collect-261121-veille-ia/gpt-openai/openai-says-jalapeno-outperforms-nvidia-blackwell-in-tests-forklog
title: "OpenAI says Jalapeño outperforms Nvidia Blackwell in tests | ForkLog"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-25"]
keywords: ["openai", "jalapeno", "times", "nvidia", "latency", "tests", "chip", "versus"]
source: https://forklog.com/en/openai-says-jalapeno-outperforms-nvidia-blackwell-in-tests/
source_anchor: ""
source_lines: [1, 51]
section: "GPT_OpenAI"
---
# OpenAI says Jalapeño outperforms Nvidia Blackwell in tests

OpenAI says Jalapeño outperforms Nvidia GB200/300 on efficiency and latency.

On August 25, OpenAI published the first measured results for its inference chip Jalapeño. In company tests, the accelerator delivered 1.5–1.9 times more compute per watt and 1.7–3.6 times lower latency than systems based on Nvidia GB200 and GB300.

Since announcing Jalapeño, our first custom inference chip, we’ve been testing it and the system around it.

The results show a major advance: more intelligence from every watt and faster responses, delivering both higher throughput and lower latency in one architecture without… pic.twitter.com/vj7VOrA8pP

— OpenAI (@OpenAI) August 25, 2026



For the most interactive workloads, Jalapeño’s performance was 2.1–4.1 times higher. OpenAI plans to begin deploying the processor in its own infrastructure by the end of 2026.

## Jalapeño compared across three AI models

The company tested the accelerator using SemiAnalysis’s public InferenceX benchmark. It measures the full request processing pipeline and allows simultaneous evaluation of throughput, power consumption, and latency.

For GPT-OSS 120B, OpenAI compared Jalapeño with Nvidia GB200. The in-house chip delivered roughly 1.9 times higher peak performance per kilowatt: 85,448 versus 44,960 mixed TPS/kW. Total latency was 1.03 seconds versus 1.8 seconds.

On DeepSeek R1 670B, the competitor was GB300. Jalapeño achieved 19,641 mixed TPS/kW versus 11,781, an advantage of about 1.7 times. Latency was 3.6 times lower: 1.65 seconds versus 5.99 seconds.

On Kimi K2.5 1T, OpenAI’s chip showed 18,195 mixed TPS/kW versus 11,862 for GB300. Latency was 1.56 seconds versus 5.31 seconds. Thus, the advantage reached roughly 1.5 times and 3.4 times, respectively.

For power comparisons, OpenAI used the accelerators’ manufacturer-stated power ratings. Jalapeño is rated at 700 W, GB200 at 1,200 W, and GB300 at 1,400 W. The company added that in its tests the chip’s sustained power draw did not exceed 550 W.

## AI helped design the custom chip

OpenAI used its AI models while developing Jalapeño. According to the developers, this helped take the project from the initial design to handing off the final design to manufacturing in nine months. Neural networks assisted with exploring implementation options, validating decisions, and optimizing arithmetic blocks.

AI was used after the processor was created as well. With Codex based on GPT-Astra, engineers optimized three models with open weights for Jalapeño in under two months, even though they were not initially in the production plan.

For specific attention and mixture-of-experts blocks in GPT-OSS, AI-generated implementations were 1.5–1.8 times faster than engineer-written solutions. OpenAI emphasized that the advantage applies only to the selected components, not the entire model.

## OpenAI expanded its AI stack

In a separate post on compute strategy, OpenAI CFO Sarah Friar called Jalapeño part of a broader vertically integrated infrastructure. It includes data centers, processors, memory, networks, AI models, software, and end products.

According to Friar, the custom chip gives OpenAI greater control over inference costs and lets it choose hardware for specific tasks. The company plans to route workloads where it gets the best performance-to-price ratio and to maintain “pricing discipline” with suppliers.

OpenAI’s compute portfolio includes Microsoft and Nvidia, as well as AWS, AMD, Broadcom, Cerebras, CoreWeave, Oracle, SB Energy, and SoftBank. The company stressed that building Jalapeño does not mean abandoning third-party accelerators. It will continue to use partners’ processors broadly for both training and inference.

The economic bet is to lower the cost per unit of compute. OpenAI expects that higher hardware performance will allow it to serve more requests with the same resources and improve the revenue-to-infrastructure-cost ratio.

Friar also expects a reverse effect. She cited the Jevons paradox: greater efficiency may not reduce total compute consumption but increase it as new economically justified AI workloads emerge.

Jalapeño will be the first generation of OpenAI’s own platform. The second-generation processor is already at an advanced stage of development, and the company has begun designing the third. The first Jalapeño units are planned for deployment in OpenAI’s infrastructure by the end of the year.

In August, OpenAI temporarily slowed the scaling of new AI models and paused for two weeks the reinforcement learning of its latest systems intended for subsequent deployment.
