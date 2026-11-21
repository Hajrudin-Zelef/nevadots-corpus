---
id: collect-261121-veille-ia/nvidia-hardware/amd-ships-256-core-zen-6-epyc-chips-as-ai-agents-put-the-cpu-back-in-the-spotlight-orbit-news-networ
title: "AMD Ships 256-Core Zen 6 EPYC Chips as AI Agents Put the CPU Back in the Spotlight | Orbit News Network"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-12"]
keywords: ["chips", "epyc", "agents", "server", "processors", "cloud", "core", "ships"]
source: https://onnxews.com/story/amd-6th-gen-epyc-zen-6-shipping-2026
source_anchor: ""
source_lines: [1, 31]
section: "NVIDIA_Hardware"
---
Technology

# AMD Ships 256-Core Zen 6 EPYC Chips as AI Agents Put the CPU Back in the Spotlight

AMD's sixth-generation EPYC server processors are now shipping, with up to 256 cores per chip and a pitch aimed squarely at the AI agent boom.

**Orbit News Network**Nov 12, 2026 · 5 min read

AMD has put its newest server chips on general sale, betting that the rise of AI agents will make the humble data-center processor a growth engine again. The company announced on Tuesday, November 10, that its sixth-generation EPYC SP7 processors, built on the new Zen 6 architecture, are generally available, with systems now shipping from leading server vendors. Cloud versions from Microsoft Azure and Oracle Cloud Infrastructure are coming soon.

The launch is aimed squarely at agentic AI, the software that lets AI systems plan, call tools, query databases and carry out multi-step jobs on their own. Graphics processors run the underlying models, but much of the surrounding work — spinning up sandboxes, feeding data to accelerators and juggling dozens of agents at once — lands on the CPU. AMD chair and chief executive Lisa Su told analysts on the company's third-quarter earnings call that agents “are quickly becoming the next killer app for CPUs in the cloud.” She added that early demand for the new chips, code-named Venice, “is the strongest we have seen for any new EPYC generation,” according to the company's prepared remarks.

The headline chip in the first family to ship, the EPYC 9006 SP7, tops out at 256 cores and 512 threads in a single socket, while a 96-core part runs as fast as 5.3 GHz. The platform adds PCIe Gen 6 connectivity and supports MRDIMM-12800 memory at up to 1.6 terabytes per second per socket. Server makers have committed more than 240 platforms to the new processors, about 20 percent more than for the previous Turin generation, and the first of those machines are now shipping.

AMD is pitching the launch directly against its two big rivals, Nvidia and Intel, and its headline numbers are vendor claims rather than independent test results. The company says a system built on its top-end EPYC 9996 delivers up to 3.7 times the performance of a system built on Nvidia's Vera CPU and up to 2.2 times the performance of an Intel Xeon 6980P system, with twice the performance per dollar of the Intel machine. For agent sandboxes, it claims roughly 3.3 times as many agents per CPU watt as Vera and about 2.5 times as many as the Xeon 6980P.

The customer list is the part AMD will be judged on. Meta is the lead customer and has been validating Venice platforms ahead of a large-scale rollout. Microsoft has opened a preview of Azure HDv2 virtual machines, the first cloud platform on the new chips, and Oracle plans OCI Compute E7 instances powered by the processors. Dell, HPE, Lenovo and Supermicro are shipping servers, and HPE alone has added four ProLiant Gen13 models built around the new chips.

AMD is also selling the chips by the rack. Its Helion design, introduced in October, comes in two CPU-only versions — one liquid-cooled and one air-cooled — plus an air-cooled model that pairs CPUs with GPUs for local inference. The densest setup holds up to 144 Venice processors and nearly 74,000 threads in a single rack. Su said the first Helion systems ship this quarter, and cloud providers are installing Venice now ahead of instance rollouts planned for early 2027.

The stakes for AMD are rising fast. Data-center revenue jumped 97 percent to a record $8.5 billion in the third quarter and made up 64 percent of total sales, up from 47 percent a year earlier, as cloud and enterprise server sales each more than doubled. Su told analysts she expects the server market to reach about $220 billion by 2030, and the new chips are AMD's bid to take a bigger share of it as AI workloads spread beyond the GPU.

Three more lines are still to come: the SP8 platform, the 9006X and a low-power 9006 LP, which AMD expects in 2027. “AI is significantly expanding the role of the CPU across every layer of the data center,” said Dan McNamara, the AMD executive who runs its compute and enterprise AI business. Matt Kimball, an analyst at Moor Insights and Strategy, told Data Center Knowledge that the core counts let companies with fixed power budgets run existing workloads on fewer servers and free space for AI. “It’s huge for those enterprise organizations that are in the beginning stages of the AI-ification of their business,” he said.

The competition is not standing still. Nvidia now sells its own data-center CPU, and Arm has published tests putting its AGI CPU ahead of EPYC on some agentic workloads. What happens next will depend less on launch-day claims than on how many of those 240-plus platforms turn into real deployments — and on whether cloud customers, who have been waiting for Azure and Oracle instances, start buying in volume when they arrive. As of November 11, the first systems are shipping, the cloud previews are open, and AMD's biggest test is just beginning.

**Why it matters:** This launch matters because it tests whether the CPU still has a starring role in the AI era. For two years the story has been all GPUs, but agents need orchestration, memory and tool-calling — work that runs on processors like these. If AMD's bet pays off, the data-center CPU becomes a growth business again and the company gains leverage against Nvidia and Intel at the exact moment both are pushing their own alternatives. If the cloud rollouts slip or independent tests undercut AMD's claims, the launch will look like a strong quarter of marketing rather than a turning point. Watch the Azure and Oracle instance launches and the next round of data-center revenue for the verdict.

Reporting: AMD Newsroom · WebProNews

Spot an error? Orbit News corrects articles promptly and notes the correction in the article. See our Editorial Policy, or flag an error by messaging us on Facebook, X, or Instagram.
