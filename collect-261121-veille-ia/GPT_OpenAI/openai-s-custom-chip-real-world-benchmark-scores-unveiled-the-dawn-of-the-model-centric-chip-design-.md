---
id: collect-261121-veille-ia/gpt-openai/openai-s-custom-chip-real-world-benchmark-scores-unveiled-the-dawn-of-the-model-centric-chip-design-
title: "OpenAI's Custom Chip Real-World Benchmark Scores Unveiled: The Dawn of the 'Model-Centric Chip Design' Era"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["chip", "openai", "jalapeno", "benchmark", "nvidia", "models", "design", "chips"]
source: https://eu.36kr.com/en/p/3956339823525255
source_anchor: ""
source_lines: [1, 95]
section: "GPT_OpenAI"
---
Nine months later, OpenAI's first chip performance report has finally been unveiled.

On August 25, at the Hot Chips conference, OpenAI's Head of Hardware Richard Ho took the stage and released the first public benchmark results for the Jalapeño chip.

In the InferenceX benchmark test by SemiAnalysis, this inference chip co-developed by OpenAI and Broadcom outperformed Nvidia's Blackwell system, the most advanced inference processor on the current market, in two key metrics: tokens per user and throughput per kilowatt.

Specifically, OpenAI used SemiAnalysis's public benchmark tool InferenceX to conduct comparative tests with Nvidia on three public large models: GPT-OSS 120B, DeepSeek R1 670B and Kimi K2.5 1T. The results show that on all three models, the peak throughput per watt of Jalapeño is 1.5 to 1.9 times that of the Nvidia system. With the same power consumption, Jalapeño can complete nearly twice as many tasks.

Comparison of peak throughput per kilowatt of the three models ｜ Image source: OpenAI

The gap is even larger in low-latency scenarios. When running DeepSeek R1, the maximum single-user generation speed is about 700 tokens per second, while Nvidia GB300 only reaches about 169 tokens per second. In conversion, the speed of Jalapeño is 4.9 times that of the latter, which means the user waiting time is reduced by nearly three quarters.

Comparison of throughput-latency curves of Jalapeño and Nvidia GB300 running DeepSeek R1 ｜ Image source: OpenAI

"Jalapeño can process more AI tasks per unit of power consumption and return responses faster at the same time. With a single architecture, Jalapeño achieves higher throughput and lower latency, while existing hardware systems usually need to make trade-offs between the two," OpenAI summarized in its official blog. According to the plan, this chip will only be deployed in "very small scale" at the end of 2026, and mass production will not start until 2027.

But what makes this chip unique is not just its stronger performance. It is the first step for a leading model manufacturer to implement self-developed chips, and it is a chip built specifically for models. Over the past two decades, the default rule of the industry has been "adapting models to chips": manufacturers adapt to whatever chips Nvidia releases; but this time the order is reversed. What exactly makes this chip built for models so different?

## **01**

## **AI chip development takes only 9 months**

The architecture design of Jalapeño is mainly led by OpenAI, with Broadcom participating in implementation, networking and interconnection, and Celestica responsible for board and rack integration.

At present, the chip is set at a rated power consumption of 700W, equipped with HBM4, with a single-package memory bandwidth of about 15.4TB/s, and an independent I/O chip to handle communication inside and outside the rack. The theoretical computing power of the B0 version of MXFP4 is 13.4 PFLOPS.

From initial design to tapeout, Jalapeño took only 9 months. Counting from October 2025, when OpenAI and Broadcom officially announced their cooperation, to the release of complete third-party witnessed benchmark results today, the whole process took less than a year in total.

What is the normal pace of the chip industry? For a high-performance ASIC, the conventional cycle from architecture definition, RTL design, verification, physical implementation to tapeout starts at 18 to 24 months. This cycle has been compressed extremely slowly for decades, as it is often limited by the speed at which engineers understand requirements, write code and complete verification.

Google's TPU and Amazon's first-generation Trainium took two years to go through all the processes. As a company that has never made chips before, OpenAI cut the cycle by more than half on its first attempt.

Behind the rapid chip development is AI accelerating the chip manufacturing process itself. According to introductions, in some attention and MoE modules of GPT-OSS, the AI-generated kernels are 1.5 to 1.8 times faster than the previous versions written by human experts. With Codex and GPT-Astra, OpenAI completed the porting and optimization of DeepSeek R1 and Kimi K2.5 in only about two months.

This AI capability is no longer limited to model tuning, but has penetrated into the whole process of chip design, driver development, hardware and software adaptation, etc.

More importantly, this is not a one-off acceleration sprint. According to public information, the benchmark results of OpenAI are still from the A0 version of the engineering chip, and the improved B0 version has entered the manufacturing stage of TSMC's N3P process. The second-generation chip is already under in-depth development, and the third-generation is in the planning stage.

When the R&D cycle of chips is compressed from 24 months to 9 months, it means that more than twice as many chip iterations can be completed in the same period, and each generation can absorb the latest insights of model architecture. The path of using self-owned models to accelerate self-owned chip design has begun to be accessible to companies with cutting-edge model and chip projects. The rhythm of "one generation a year" is no longer exclusive to established chip giants, and OpenAI wants to take a shortcut directly with AI.

## **02**

## **The secret to outperforming Nvidia: keep data in place**

**If you want to understand the design philosophy of this chip, there is only one sentence: move less data.**

There is a harsh common sense in the chip industry: "Computation is almost free, but data movement is expensive." The bottleneck of large model inference does not lie in computing power, but in that for every token generated, a huge amount of model weights and context cache (KV cache) need to be moved in and out of the memory. Most of the time, the computing units of general-purpose GPUs are not computing, but waiting for data. This is also the root cause of many chips that have high peak performance on paper but can only exert 60% to 70% of their performance under actual loads.

Almost all architectural choices of Jalapeño revolve around this pain point. It cuts the computing cores and HBM4 memory into corresponding "slices", and each core has a low-latency direct access to its own slice of memory, which is equivalent to not letting workers go to a public warehouse to pick up materials, but equipping each person with a dedicated toolbox.

At the same time, the 15.4TB/s memory bandwidth of a single package ensures that the toolbox is large enough and fast enough. The official statement is that the "data" that the model repeatedly uses when generating responses can be "explicitly placed and kept locally", clearly arranged at hand on the workbench for processing, without repeated remote calls, saving the time of moving data back and forth.

The most notable architectural choice is that on Jalapeño, prefill (processing user input) and decode (generating tokens one by one) are completed by the same accelerator, instead of splitting prefill to a dedicated CPX chip like Nvidia's Rubin.

OpenAI's reasoning is very practical: the composition of real traffic during chip operation is constantly changing. From chat to inference models to Agents, the proportion of input, cache and output tokens has completely changed. If two types of chips are configured in a fixed proportion today, half of them will be idle when the traffic changes tomorrow.

Rather than pursuing the optimal performance on paper, it is better to let every chip be able to do everything. This is one of the most vivid manifestations of "building chips for models": let chip design follow the changes of self-owned model traffic, rather than following traditional logic.

Taking GPT-OSS 120B as an example, in terms of efficiency, it processes about 85,400 tokens per second per kilowatt, completing nearly twice as many tasks with the same 1 kWh of electricity; in terms of speed, the maximum single-user generation speed is 1459 tokens per second, while the competitor's is 535 tokens per second.

Comparison of peak single-user generation speed of the three models ｜ Image source: OpenAI

Moreover, the larger the model, the more obvious the advantage: on DeepSeek R1 with 670 billion parameters, the efficiency is 1.7 times higher, and in low-latency mode, the speed is 700 tokens per second for single user versus 169 tokens per second for competitors; on Kimi K2.5 with trillion parameters, the minimum latency from question to answer is 1.56 seconds, while the competitor takes 5.31 seconds.

When the interaction speed is fixed at the mainstream level of 100 tokens per second per user, the throughput of Jalapeño on Kimi K2.5 is more than 9 times that of Nvidia GB300, which means that with the same service quality, it can serve 9 times more user requests at the same time.

Comparison of throughput per kilowatt at the same speed when running Kimi K2.5 on Jalapeño and Nvidia GB300 ｜ Image source: OpenAI

**But this report card also has flaws and does not mean a complete victory.** We can see that the benchmark data is mainly provided by OpenAI, and SemiAnalysis only witnessed the operation on site in the laboratory, and did not independently complete all the tests. Moreover, all the public results of Jalapeño come from single-round inference with about 8k input and 1k output, which is a relatively easy load for tuning. At the same time, OpenAI has not released any public data for agent tasks with longer context and multi-round interaction. It should be noted that Agent is precisely the scenario with the fastest growing inference demand at present, and it is also the scenario that most tests the real capabilities of systems such as routing and prefix caching.

## **03**

## **Chips built for models start to compete for AI pricing power**

There is another ambition hidden in this report card. In this benchmark test, the two competing models DeepSeek R1 and Kimi K2.5 received exactly the same treatment as the self-owned GPT-OSS. Although the MLA attention mechanism adopted by DeepSeek is its self-developed architecture, OpenAI still implemented the underlying core computing code required for this architecture from scratch.

Developing underlying code specifically for the private architecture of competitors goes far beyond the need to "prove that the chip is general-purpose". It is more like implicitly proving that any model can quickly reach the optimal performance on this chip.

This pipeline points to a business that has not yet been announced. If the cost per token of Jalapeño is indeed lower than that of Nvidia, OpenAI can completely lease inference computing power to other model companies just like AWS sells cloud services: first build infrastructure for itself, and then open up the excess capacity. OpenAI has not made any public statement about "selling computing power" so far, but the long-term ambition of a company is often first revealed in its engineering decisions.

Performance of running DeepSeek R1 on Jalapeño and Nvidia GB300 ｜ Image source: OpenAI

Coincidentally, the day before Jalapeño's report card was released, on August 24, Nvidia also released the benchmark results of Vera Rubin.

Nvidia announced that the Rubin rack-scale system has been fully put into production, and released that under the Artificial Analysis benchmark, the new platform runs Gemma 4 31B, achieving 3400 output tokens per second in the 100,000-token long-context scenario, which is 4 times faster than the closest competing product. Combined with previous actual measurements by CoreWeave, the token throughput per megawatt of Vera Rubin NVL72 is 10 times that of the previous generation GB200.

More dramatically, on the same day, Apple also launched the world's first mass-produced 2nm chip M6, which is first installed on Mac mini, with 40% higher multi-threaded performance than the previous generation, and dual 16-core neural engines bringing doubled local AI computing power. The M5 Ultra at the same event pushes the unified memory of Mac to 512GB, and multiple Mac Studios can also form a cluster to run distributed inference. Apple is obviously focusing on hardware, hoping to keep data local and avoid paying by tokens, making Mac another option beyond cloud computing power.

48 hours, three chips. Putting them together, we can see that mobile giants, leading computing power manufacturers and top model companies have taken three completely different paths for chips in the AI era.

Apple bets on manufacturing process and end-side deployment, using the most advanced technology to put AI into the box on everyone's desk, bypassing the pay-as-you-go billing of the cloud; Nvidia bets on generality and system, with seven chips cooperating into one machine to maintain a half-generation lead over all models; OpenAI bets on vertical specialization, hardening the requirements of its own models directly into the circuit to achieve the ultimate cost per token.

The three paths are technically incompatible, but they have turned the computing power business from a monopoly of one company into a game of three forces. From the moment this report card is released, the largest buyer of model enterprises has used its own chips, its own test questions and its own cost structure to give another answer for the first time.

This article is from the WeChat official account
