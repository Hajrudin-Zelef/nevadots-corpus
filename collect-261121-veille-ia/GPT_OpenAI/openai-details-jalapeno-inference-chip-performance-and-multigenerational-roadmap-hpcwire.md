---
id: collect-261121-veille-ia/gpt-openai/openai-details-jalapeno-inference-chip-performance-and-multigenerational-roadmap-hpcwire
title: "OpenAI Details Jalapeño Inference Chip Performance and Multigenerational Roadmap - HPCwire"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-27"]
keywords: ["openai", "jalapeno", "performance", "more", "chip", "across", "power", "latency"]
source: https://www.hpcwire.com/off-the-wire/openai-details-jalapeno-inference-chip-performance-and-multigenerational-roadmap/
source_anchor: ""
source_lines: [1, 51]
section: "GPT_OpenAI"
---
## Off the Wire Press Releases

Aug. 27, 2026 — Since announcing Jalapeño, OpenAI’s first custom inference chip, the company has been testing the chip and the system built around it. The results show a significant performance advance: Jalapeño can serve more AI work per unit of power while also returning responses more quickly. Jalapeño delivers both higher throughput and lower latency with one architecture, where existing hardware systems often have to make a tradeoff between the two.

For customers, that can mean faster responses, more responsive agents, and more reliable access as demand grows. OpenAI’s mission is to ensure that artificial general intelligence benefits all of humanity. These gains will help make increasingly capable AI more affordable and more broadly available.

OpenAI models also accelerated Jalapeño’s development. Earlier generations helped the team design and bring up the chip, while the company’s latest models are accelerating how OpenAI optimizes and programs it. Jalapeño’s performance extends across GPT‑OSS 120B, DeepSeek R1, and Kimi K2.5 1T, showing that the architecture works across models developed both inside and outside OpenAI. Across all three, Jalapeño delivered 1.5 to 1.9 times more AI work per watt at peak throughput and 1.7 to 3.6 times lower end-to-end latency than the comparison systems. For highly interactive workloads, it delivered 2.1 to 4.1 times higher performance.

Jalapeño is also evidence of a broader full-stack advantage. OpenAI can design models, products, serving software, chips, memory, networking, and systems together, using what OpenAI learns from real workloads to improve every layer of the stack. Jalapeño is working first-party silicon with measured results, and it is the beginning of a multigenerational platform. In the months ahead, OpenAI will ramp Jalapeño to deliver faster, more capable, and more efficient products for customers.

**How OpenAI Measured Jalapeño’s Performance**

OpenAI evaluates performance at a matched user experience, measuring how much useful AI work each system can complete per unit of power while meeting the latency customers and interactive agents require. This matters especially for agents, which need to complete many steps in sequence, so delays can compound across an entire task.

To understand how Jalapeño performs in practice, OpenAI tested it on InferenceX, a public benchmark from SemiAnalysis that measures the full process of serving an AI request. OpenAI compared Jalapeño with leading commercially available AI systems across the tested operating range, from high-throughput serving to highly interactive, low-latency use. Jalapeño delivered a better combination of throughput, power efficiency, and latency. Although performance is sometimes reported per chip, OpenAI believes the more useful standard is performance per unit of power.

Across all three public models, Jalapeño delivered a better combination of performance per watt and latency across the tested operating range, placing it on the Pareto frontier. To compare the systems consistently, OpenAI normalized the results using each accelerator’s published chip power rating. Jalapeño is rated at 700 watts, although its measured sustained power remained at or below 550 watts on the workloads tested.

Jalapeño performed strongly across GPT‑OSS 120B, DeepSeek R1 670B, and Kimi K2.5 1T. On Kimi, the largest public model OpenAI tested, it delivered approximately 1.5 times higher peak performance per watt and 3.4 times lower end-to-end latency than the comparison system. In OpenAI’s internal testing, Jalapeño’s advantage widened further on frontier OpenAI models, suggesting that the architecture becomes more valuable as workloads grow larger and more demanding.

**Architecting for speed and efficiency within a single chip**

Jalapeño’s gains come from designing the chip, memory, network, software, and rack-scale system together around real language-model workloads. Language-model inference moves through several distinct phases with different bottlenecks. Prefill, when the system processes a prompt, is compute-intensive, while decode, when the system generates the response token by token, is constrained more by memory bandwidth. Communication can also add latency when data must move between cores and chips, leaving some processing units idle while they wait. A system that excels at one phase can lose that advantage while waiting for data or moving model state between different resources.

OpenAI designed Jalapeño to minimize data movement and communication delays. This means that model state, including the KV cache used while generating a response, can be explicitly placed and kept local while the system activates the right combination of compute, memory, and networking for each inference phase. The network is integral to the architecture. Its large domain allows the entire workload to remain within one connected system, minimizing data movement and helping the complete request stay fast and efficient from beginning to end. The result is a balanced and fungible accelerator that can support changing model architectures, excel at both prefill and decode, and adapt as the balance between them changes, a defining feature of agentic workloads.

**OpenAI Used AI to Design the Chip, and Designed the Chip So AI Could Program It**

AI played a direct role in Jalapeño’s development, enabling the team to move from initial design to tapeout in nine months by exploring implementations, shortening design, measurement, and verification loops, and continuously iterating on model workloads. AI also helped optimize the chip’s arithmetic circuits, allowing the team to fit more compute performance into the chip on schedule.

Jalapeño was designed as a clear, predictable programming target for both humans and AI. Engineers can describe work through local tensors, explicit communication, and predictable synchronization. AI can then optimize how that work is mapped, placed, scheduled, and coordinated across the system. That clear, predictable structure gives AI a tractable way to tackle the traditionally difficult problem of parallel programming.

Supporting each new model family still requires new kernels and model-specific optimizations. Using Codex with GPT‑Astra, the team brought three open-weight models that were not part of Jalapeño’s original production plan to high performance within two months. This demonstrated both the flexibility of the architecture and the speed at which AI can help with programming. For selected GPT‑OSS attention and mixture-of-experts blocks, AI-generated implementations ran 1.5 to 1.8 times faster than the existing human-expert-written implementations. Those figures apply to the selected blocks, not the full model, but they point toward a powerful new development loop.

**The Path Ahead for Efficient, Ultra-Fast Inference**

AI infrastructure is valuable because of the useful real-world work it enables. By producing more useful work from the same power and hardware, Jalapeño can help OpenAI serve more demand and lower the cost of delivering a successful result. For OpenAI, that can improve operating leverage by allowing useful work and revenue to grow faster than the cost to serve. It can also support broader adoption and continued investment in better models, products, and infrastructure. Faster inference can enable faster iteration and new use cases.

Jalapeño expands what is possible for efficient, low-latency inference:

- Ultra-fast-mode inference at efficiencies previously available only in fast mode
- Fast-mode inference at efficiencies previously available only in batched mode
- Higher efficiency for batched-mode inference

OpenAI plans to begin deploying Jalapeño within the company’s compute infrastructure by the end of the year. It is the first generation of a multigenerational roadmap: Gen 2 is deep in development, and Gen 3 is taking shape. Each generation will build on what OpenAI learns and further advance both efficiency and speed.

Meeting growing demand for AI will require more compute from every available source. OpenAI will continue to widely deploy accelerators from NVIDIA and other partners for both training and inference workloads. OpenAI’s mission is to ensure that artificial general intelligence benefits all of humanity.

As OpenAI prepares for deployment, the company is continuing production qualification, maturing the software, preparing to operate Jalapeño at scale, and validating performance across more models. The results so far show what is possible when the full system is designed together: more responsive, capable, and agentic AI delivered more efficiently to more people.

*Source: OpenAI*
