---
id: collect-261121-veille-ia/gpt-openai/openai-s-game-changing-move-self-developed-inference-chip-codename-jalapeno-outperforms-nvidia
title: "OpenAI's Game-Changing Move: Self-Developed Inference Chip Codename 'Jalapeño' Outperforms NVIDIA"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "chip", "jalapeno", "model", "inference", "self-developed", "first", "nvidia"]
source: https://eu.36kr.com/en/p/3955555203447945
source_anchor: ""
source_lines: [1, 114]
section: "GPT_OpenAI"
---
When I woke up, news about OpenAI was everywhere.

First, X user Leo@synthwavedd exclusively claimed: **OpenAI has completed a new pre-training, codenamed "Bel", with a parameter size reportedly exceeding 10 trillion. It may be the successor of Doug and could become the foundational model for Astra / GPT-6...**

After searching around, no accurate official information was found. But what is certain is that OpenAI's first self-developed inference chip has delivered remarkable results.

Just now, **OpenAI announced the latest test results of its first self-developed inference chip Jalapeño:** it has achieved a major breakthrough. **This chip is specially designed for large language model inference. With a brand-new architecture design, it can increase throughput and reduce latency at the same time, achieving both goals while maintaining high energy efficiency. It has outperformed the efficiency of NVIDIA GB200 and GB300 systems in multiple model tests.**

Shortly afterwards, OpenAI CEO Sam Altman also posted on X: "We built a chip that's really fast."

It is worth noting that as OpenAI's first self-developed inference chip, **Jalapeño is not a chip for training the next generation of GPT models, but is optimized for the operation phase after the model is launched.**

In simple terms: the training phase enables the model to acquire capabilities, while the inference phase enables the model to quickly respond to user requests. Jalapeño is mainly designed to solve the second problem.

OpenAI stated that traditional hardware systems often have to make trade-offs between two metrics:

- Higher throughput: processing more requests per unit of time;
- Lower latency: enabling users to get responses faster.

For example, large-scale batch processing can improve overall efficiency, but may increase the waiting time for a single request; while pursuing extremely low latency may sacrifice resource utilization. The goal of Jalapeño is to optimize both in one architecture.

In terms of specific performance, OpenAI stated that in multiple large model inference tests, **Jalapeño outperforms NVIDIA GB200 and GB300 systems.** The tested models include: **OpenAI GPT-OSS 120B; DeepSeek R1 670B; Moonshot AI Kimi K2.5 1T.**

What is particularly notable is the appearance of **DeepSeek R1 and Kimi K2.5**, which seems to indicate that Jalapeño is not only optimized for OpenAI's own models, but can adapt to different large model workloads.

Interestingly, Jalapeño translates to "Mexican chili", and as soon as this news was released, it immediately sparked ridicule and heated discussions from netizens from all sides.

"Are you going to name your chip after a chili? Can't wait to try it out."

For OpenAI, the launch of Jalapeño shows that OpenAI is trying to open up the entire chain of "model - software - chip - data center".

It is also worth mentioning that OpenAI's Jalapeño chip this time was developed in cooperation with Cerebras at the hardware level.

Tibo posted: "This capability is made possible by our deep partnership with Cerebras and their unique hardware architecture. In the future, this cooperation will further push the boundaries of the 'ultra-fast' experience.

I am very much looking forward to our continued cooperation to continuously push the limits on the Cerebras platform, explore how to run our most capable models at the fastest speed, and bring this experience to customers with the highest performance requirements."

In fact, OpenAI is not the first AI company to develop self-designed chips. Many major AI companies have long entered the chip development field. For example, Google launched TPU; Amazon developed Trainium and Inferentia; Microsoft advanced Maia; Meta has also deployed self-developed AI accelerators...

The underlying logic is very similar: general-purpose GPUs are flexible enough, but they are not optimized for all AI workloads. For companies that run massive AI requests every day, if they can design chips for their own models, software systems and service methods, they will have the opportunity to further reduce costs...

Now let's take a closer look at the capabilities of Jalapeño.

## **Faster, More Power Efficient**

The core advantage of Jalapeño is to complete more AI work under the same power consumption while returning responses faster. Existing systems usually need to make trade-offs between throughput and latency, while Jalapeño attempts to achieve higher throughput and lower latency with the same architecture.

OpenAI emphasizes that this advantage is not only reflected in its own models, but also covers externally developed models, proving that Jalapeño is a more general-purpose inference architecture.

On three public models, GPT-OSS 120B, DeepSeek R1 670B and Kimi K2.5 1T, Jalapeño achieves 1.5 to 1.9 times more AI workload per watt at peak throughput, with end-to-end latency reduced to 1/1.7 to 1/3.6 of that of the comparison system; for highly interactive workloads, the performance advantage reaches 2.1 to 4.1 times.

On GPT-OSS 120B, DeepSeek R1 670B and Kimi K2.5 1T, the peak throughput per watt of Jalapeño reaches 1.9 times, 1.7 times and 1.5 times that of the current best systems respectively.

Among them, on the largest Kimi K2.5, the peak performance per watt of Jalapeño is increased by about 1.5 times, and the end-to-end latency is reduced by about 3.4 times.

On Kimi K2.5 1T, as the single-user decoding speed increases, the throughput per watt advantage of Jalapeño over GB300 expands from 1.5 times at peak to up to 56.1 times.

OpenAI does not only compare the peak performance of a single chip, but pays more attention to one metric: how much useful AI work can be done per unit of power while meeting the latency requirements of users and interactive Agents.

The reason is also related to Agents. Agents often need to complete multiple steps continuously, and the seemingly small latency in one request will accumulate continuously during the execution of the entire task.

This is also where Jalapeño's performance is most obvious in the low-latency range. Taking DeepSeek R1 as an example, under the conditions corresponding to the previous best TBT of the comparison system, Jalapeño achieves a throughput of 12,258 mixed TPS/kW per kilowatt, which is about 104.3 times higher than the 118 of GB300.

On DeepSeek R1 670B, the peak throughput per watt of Jalapeño is about 1.7 times that of GB300; at the same decoding speed, the advantage expands up to 104.3 times.

OpenAI uses the public benchmark InferenceX from SemiAnalysis for testing, and compares it with current leading commercial systems. From high-throughput services to highly interactive, low-latency scenarios, Jalapeño achieves a better combination of per-watt performance and latency on the three public models, and is located on the Pareto frontier.

At different operating points of DeepSeek R1 670B, Jalapeño forms a better combination of throughput per watt, interaction speed and end-to-end latency, staying on the Pareto frontier.

The rated power consumption of Jalapeño is 700W, but in the workload of this test, the actual continuous power consumption always remains at 550W or below.

## **Built for Agents**

Jalapeño was designed from the very beginning around current and future large language models, especially interactive Agents.

Different stages of LLM inference have different bottlenecks: Prefill is more dependent on computing power, Decode is more limited by memory bandwidth, and data movement between cores and chips will also increase latency.

Therefore, OpenAI co-designed the chip, memory, network, software and rack-level system. Model states including KV Cache can be explicitly placed and kept locally as much as possible, allowing Jalapeño to adapt to both Prefill and Decode, and adjust as the workload of both changes.

**OpenAI believes that this is exactly the key feature of Agent workloads.**

Interestingly, **AI is not only the object that Jalapeño serves, but also directly participated in the development of this chip.**

With the help of models at different stages, the OpenAI team only took 9 months from the initial design to the Tapeout (finalization of the tapeout design). The models are used to explore different implementations, shorten the design, measurement and verification cycles, and also participate in optimizing the arithmetic circuits of the chip.

Jalapeño is also designed to be a clear and predictable programming target for both humans and models. Using Codex driven by GPT-Astra, the team only took 2 months to get three open-weight models that were not originally in Jalapeño's initial production plan to run with high performance.

In some GPT-OSS Attention and MoE modules, the implementation generated by Codex is even 1.5 to 1.8 times faster than the original handwritten version by human experts.

## **Deployment by the End of the Year, Second and Third Generations Are Already on the Way**

**OpenAI plans to start deploying Jalapeño into its own computing infrastructure by the end of 2026.** At present, the team is still conducting production verification, improving software, preparing for large-scale operation, and continuing to verify performance on more models.

Subsequent products have also been launched, Gen 2 has entered the in-depth development stage, and Gen 3 has also taken shape.

OpenAI summarizes the efficiency changes brought by Jalapeño into three gears: Ultra-fast mode can achieve the efficiency that was only available in Fast mode in the past; Fast mode can achieve the efficiency that was only available in Batch mode in the past; the Batch mode itself further improves efficiency.

However, self-developed chips do not mean that OpenAI is ready to get rid of NVIDIA.

OpenAI clearly stated that meeting the growing demand for AI requires more computing power from all available sources, and the company will continue to deploy NVIDIA and other partners' accelerators on a large scale, covering both training and inference workloads.

What's your opinion? Welcome to leave a message and communicate in the comment area!

**Reference links:**

https://x.com/OpenAI/status/2092300846675505602

https://x.com/sama/status/2092339694210040187

https://openai.com/index/jalapeno-first-results/

https://techcrunch.com/2026/08/25/openais-jalapeno-chip-is-built-for-fast-inference-at-scale-benchmarks-show/

This article is from the WeChat official account
