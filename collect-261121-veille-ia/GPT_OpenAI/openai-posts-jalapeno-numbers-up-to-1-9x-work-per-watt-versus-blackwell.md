---
id: collect-261121-veille-ia/gpt-openai/openai-posts-jalapeno-numbers-up-to-1-9x-work-per-watt-versus-blackwell
title: "OpenAI Posts Jalapeño Numbers: Up to 1.9x Work Per Watt Versus Blackwell"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "jalapeno", "nvidia", "chip", "times", "watt", "work", "models"]
source: https://www.shashi.co/2026/08/openai-posts-jalapeno-numbers-up-to-19x.html
source_anchor: ""
source_lines: [1, 45]
section: "GPT_OpenAI"
---
In June, OpenAI and Broadcom said Jalapeño beat the state of the art on performance per watt. They named no comparison chip and attached no figure. On August 25, at Hot Chips, OpenAI chip chief Richard Ho filled that blank: 1.5 to 1.9 times more AI work per watt at peak throughput, 1.7 to 3.6 times lower end-to-end latency, measured against Nvidia's Blackwell-generation systems across three public models (OpenAI, 2026).

## June Handed Over a Wafer. August Handed Over a Range.

On June 24, nine months after they started building it together, OpenAI and Broadcom handed Sam Altman and Greg Brockman a real Jalapeño chip (OpenAI, 2026). Early chips were already running lab jobs at the speed and power OpenAI wanted, including GPT-5.3-Codex-Spark. The official performance tests were not done, so June offered a claim, not a number (TechCrunch, 2026).

## Plan Against 1.7x Latency. Treat 3.6x as the Best Case.

Chip research firm SemiAnalysis ran its public speed test, InferenceX, and watched some of the tests inside OpenAI's lab (The Decoder, 2026). OpenAI provided the other numbers. The tests used three public models: GPT-OSS 120B, DeepSeek R1 670B, and Kimi K2.5 1T. Jalapeño did 1.5 to 1.9 times more work per watt than the Nvidia systems in the test, and answered 1.7 to 3.6 times faster. On jobs that need a quick reply, the speed gain rose to 2.1 to 4.1 times (Interesting Engineering, 2026).

Use the low end of the range. 1.7 times faster on the slowest of the three models is still a gain. It is also less than half the 3.6 times figure most headlines use. Plan with 1.7 times. Treat 3.6 times as the best day on the easiest model.

"Achieves high throughput and low latency simultaneously, a first in the industry."

— Richard Ho, OpenAI (Yahoo Finance, 2026)

## The Slides Measured Blackwell. Vera Rubin Was Already Shipping.

Every figure above uses Nvidia's GB300, the Blackwell-generation part Richard Ho called the leading option on the public benchmarking system OpenAI used (Bloomberg, 2026). Vera Rubin had begun shipping by the Hot Chips talk. Jalapeño was not tested against it (Tom's Hardware, 2026).

shashi.co flagged this custom silicon program in June, before it had a public name, as one of several hyperscaler efforts to cut a single supplier's hold on compute cost. Coverage then called it Titan and named Broadcom for design and Samsung for memory. Jalapeño is that program with a published range. The range beats the Nvidia generation that sold last year.

## You Cannot Order Jalapeño. You Can Only Watch the API.

Jalapeño serves already-trained models. It stays inside OpenAI's buildings. Deployment is planned by the end of 2026, next to continued purchases of Nvidia and other accelerators (Interesting Engineering, 2026). Training, the more expensive half of OpenAI's compute bill, remains on Nvidia hardware for now. Richard Ho told Bloomberg the company still needs "a lot of Nvidia" (Bloomberg, 2026). A second-generation design is nearing tape-out. Early work on a third generation has started.

You cannot buy this chip. If it cuts OpenAI's costs or speeds up answers, you will see that in what you pay to use OpenAI's models and how fast those models reply.

OpenAI has not published a cost-per-token figure tied to the chip.

**Sources**

Bloomberg. "OpenAI Claims Its New Chips Can Outperform Nvidia Processors in Tests." Bloomberg, 25 Aug. 2026, www.bloomberg.com.

OpenAI. "OpenAI and Broadcom Unveil LLM-Optimized Inference Chip." OpenAI, 24 Jun. 2026, openai.com.

TechCrunch. "OpenAI Unveils Its First Custom Chip, Built by Broadcom." TechCrunch, 24 Jun. 2026, techcrunch.com.

The Decoder. "OpenAI's First Custom Chip 'Jalapeño' Reportedly Beats Nvidia's Blackwell and Rubin in Inference Benchmarks." The Decoder, 25 Aug. 2026, the-decoder.com.

Tom's Hardware. "OpenAI's 700W Jalapeño ASIC Outpaces 1,400W Nvidia Flagship GPU." Tom's Hardware, 25 Aug. 2026, tomshardware.com.

Yahoo Finance. "OpenAI Says Its New Chip Outperforms Nvidia's Blackwell as Nvidia Prepares Earnings Release." Yahoo Finance, 26 Aug. 2026, finance.yahoo.com.

Interesting Engineering. "OpenAI Chip Beats Nvidia Systems With 1.9x More Work per Watt." Interesting Engineering, 26 Aug. 2026, interestingengineering.com.
