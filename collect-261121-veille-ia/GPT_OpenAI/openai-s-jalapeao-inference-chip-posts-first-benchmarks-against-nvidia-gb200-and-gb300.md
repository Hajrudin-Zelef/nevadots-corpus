---
id: collect-261121-veille-ia/gpt-openai/openai-s-jalapeao-inference-chip-posts-first-benchmarks-against-nvidia-gb200-and-gb300
title: "OpenAI's JalapeÃ±o Inference Chip Posts First Benchmarks Against Nvidia GB200 and GB300"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "jalapeao", "inference", "chip", "gb300", "nvidias", "against", "said"]
source: https://www.webpronews.com/openais-jalapeno-chip-takes-aim-at-nvidias-dominance-in-ai-inference/
source_anchor: ""
source_lines: [1, 22]
section: "GPT_OpenAI"
---
OpenAI has published the first performance figures for JalapeÃ±o, the custom inference chip it co-developed with Broadcom. The company says the chip does more AI work per watt than Nvidia’s rack systems and responds faster. It released the results Aug. 25 and presented them at the Hot Chips conference.

On SemiAnalysis’s public InferenceX benchmark, OpenAI tested three open models: GPT-OSS 120B, DeepSeek R1 670B and Moonshot AI’s Kimi K2.5 1T. It reported 1.5 to 1.9 times more throughput per watt at peak and 1.7 to 3.6 times lower end-to-end latency than the comparison systems. The GPT-OSS comparison was against Nvidia’s GB200, and the DeepSeek and Kimi comparisons were against GB300. For highly interactive workloads, OpenAI said the advantage was 2.1 to 4.1 times. Richard Ho, who runs OpenAI’s hardware program, called the results “a very, very significant performance advance over state-of-the-art,” TechRadar reported.

## Read the fine print

The comparisons are normalized to each chip’s rated power: 700 watts for JalapeÃ±o versus 1,200 watts for GB200 and 1,400 watts for GB300. OpenAI said JalapeÃ±o’s measured sustained power stayed at or below 550 watts on the tested workloads. TechRadar pointed out several caveats:

- The figures are self-reported.
- Measured per package rather than per watt, Nvidia’s GB300 still leads on absolute throughput by 20% to 25%.
- JalapeÃ±o ran single-token prediction with no speculative decoding. Against a GB300 configured with multi-token prediction, as Nvidia’s production deployments often are, the peak efficiency lead falls to roughly 1.5 times.
- OpenAI didn’t compare the chip with Nvidia’s newer Vera Rubin.

## Design and timeline

OpenAI said it designed JalapeÃ±o to “minimize data movement and communication delays” by keeping model state, including the KV cache, local to the chip. TechRadar reported that each package pairs a compute die with six HBM4 stacks, for 216 GiB of memory at 15.4 TB/s.

OpenAI said AI helped it go from initial design to tapeout in nine months. Using Codex, the team also brought three open-weight models outside the original production plan up to high performance within two months. The published numbers come from first-run A0 silicon. TechRadar said a B0 stepping with roughly 25% better performance per watt is already in the fab.

OpenAI plans to start deploying JalapeÃ±o in its own infrastructure by the end of the year, with production ramping through 2027. A second generation is “deep in development.” The company also said it “will continue to widely deploy accelerators from NVIDIA and other partners for both training and inference workloads.”

Even so, analysts see a threat to Nvidia’s inference business. Omdia’s Alexander Harrowell told CNBC that “this is the biggest competitive threat to NVIDIA,” according to TechRadar. One of the largest buyers of inference hardware now has a working in-house alternative.
