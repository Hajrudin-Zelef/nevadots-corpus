---
id: collect-261121-veille-ia/nvidia-hardware/amd-ships-256-core-zen-6-epyc-venice-cpus
title: "AMD Ships 256-Core Zen 6 EPYC Venice CPUs"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-10"]
keywords: ["epyc", "venice", "cpus", "chips", "more", "system", "intel", "agents"]
source: https://aiincontext.com/articles/amd-ships-256-core-zen-6-epyc-venice-chips-as-ai-agents-drive-cpu-demand
source_anchor: ""
source_lines: [1, 51]
section: "NVIDIA_Hardware"
---
## Why it matters

- AMD's sixth-generation EPYC 'Venice' chips went on general sale November 10, 2026, with up to 256 cores and 512 threads per socket.
- Server makers committed more than 240 platforms, 20% more than for the Turin generation.
- AMD claims up to 3.7x the performance of an Nvidia Vera system and up to 2.2x an Intel Xeon 6980P system (vendor figures).
- AMD data-center revenue hit a record $8.5 billion in Q3, up 97% year over year, making up 64% of total sales.
- Meta is lead customer; Microsoft opened an Azure HDv2 preview; the Helion rack holding 144 Venice chips ships this quarter.

Server makers have committed more than 240 platforms to AMD's sixth-generation EPYC processors — 20% more than for the previous Turin generation — and the first of those machines are now shipping. AMD put the chips on general sale Tuesday, November 10, 2026, wagering that the spread of AI agents will turn the data-center CPU back into a growth engine.

The logic behind the bet is straightforward. GPUs run the models, but agents also spin up sandboxes, call tools, query databases and feed data to accelerators, and that work lands on CPUs. Lisa Su, AMD's chair and chief executive, told analysts last week that agents "are quickly becoming the next killer app for CPUs in the cloud." She added that early demand for the new chips, code-named Venice, "is the strongest we have seen for any new EPYC generation," according to AMD's prepared remarks for its third-quarter earnings call.

## What exactly is shipping?

The first family to ship, the EPYC 9006 SP7, is built on the Zen 6 architecture and tops out at 256 cores and 512 threads per socket. A 96-core part in the family runs as fast as 5.3 GHz. The platform adds PCIe Gen 6 connectivity and supports MRDIMM-12800 memory at up to 1.6 terabytes per second per socket, the company said in a product blog post.

Three more lines are still to come: the SP8, the 9006X, and a low-power 9006 LP variant. Data Center Knowledge reported that AMD expects those to arrive in 2027.

The launch pitch targets Nvidia and Intel directly. A system built on AMD's top-end EPYC 9996 delivers up to 3.7 times the performance of a Vera-based system and up to 2.2 times that of an Intel Xeon 6980P system, with twice the performance per dollar of the Intel machine, AMD said in its launch announcement. For agent sandboxes specifically, the company claims roughly 3.3 times as many agents per CPU watt as Vera and 2.5 times as many as the Xeon 6980P. All of those are vendor figures, not independent test results.

Dan McNamara, who runs AMD's compute and enterprise AI business, framed the opportunity in the launch materials. "AI is significantly expanding the role of the CPU across every layer of the data center," he said.

## Why does the CPU matter in an AI story?

For two decades the narrative in AI infrastructure has centered on GPUs and accelerators, with CPUs cast in a supporting role. Agentic AI changes that arithmetic. Every agent that runs a task needs CPU cycles for orchestration, sandboxed execution, database access and data preparation before any GPU does inference. The result is a contested market: Nvidia now sells its own data-center CPU in the form of Vera, and Arm has published tests putting its AGI CPU ahead of EPYC on agentic workloads. AMD's launch is its answer to both.

The competitive framing also carries financial weight. Intel's Xeon has long dominated the general-purpose server market, and Nvidia is pushing vertically integrated systems that pair its own CPUs with its GPUs. If agents really do multiply CPU demand, the winner of that three-way contest captures a slice of a server market that Su said she expects to reach about $220 billion by 2030.

## Who is buying?

Meta is the lead customer and has been validating Venice platforms ahead of a large-scale rollout. Microsoft opened a preview of Azure HDv2 virtual machines, making Azure the first cloud platform on the new chips.

"Azure is where much of the most advanced Frontier AI model development happens, and that requires extraordinary amounts of CPU infrastructure to process the data used to train those models," said Ananth Sankaranarayanan, Microsoft's vice president of AI infrastructure.

Oracle plans OCI Compute E7 instances on the processors. On the hardware side, Dell, HPE, Lenovo and Supermicro are shipping servers, with HPE alone adding four ProLiant Gen13 models. AMD announced the availability in a November 10 post on X, writing that the Zen 6-based CPUs "provide the performance and flexibility to support growing agentic AI deployments as well as demanding enterprise, cloud and HPC workloads."

## How big is this for AMD's business?

The stakes for AMD keep rising. Data-center revenue jumped 97% to a record $8.5 billion in the third quarter and made up 64% of total sales, up from 47% a year earlier, as WebProNews reported last week. Su said cloud and enterprise server sales each more than doubled in the quarter.

Analysts see the enterprise angle as the quieter half of the story. Matt Kimball, vice president and principal analyst at Moor Insights & Strategy, told Data Center Knowledge that the high core counts let CIOs with fixed power budgets run existing workloads on fewer servers and free space for AI. "It's huge for those enterprise organizations that are in the beginning stages of the AI-ification of their business," he said.

## What about rack-scale systems?

AMD is also selling the chips by the rack, not just by the socket. Its Helion design, introduced in October, comes in two CPU-only versions — one liquid-cooled and one air-cooled — plus an air-cooled model that pairs CPUs with GPUs for local inference. The densest configuration holds up to 144 Venice processors and nearly 74,000 threads in a single rack.

Su said the first Helion systems ship this quarter, and cloud providers are installing Venice processors now ahead of instance rollouts planned for early 2027.

The launch gives AMD a concrete product answer to Nvidia's Vera and Intel's Xeon at the exact moment AI agents are pulling CPU demand upward. Whether AMD's vendor benchmarks hold up under independent testing — and whether Meta, Microsoft and Oracle translate early deployments into volume orders — will determine if Venice becomes the growth engine Su is promising.

Source: webpronews.com
