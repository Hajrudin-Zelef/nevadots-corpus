---
id: collect-261121-veille-ia/gemini-google/googles-tpu-8tx-and-8i-max-bandwidth-vs-confidential-inference
title: "Google’s TPU 8tx and 8i Max: bandwidth vs. confidential inference"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["inference", "confidential", "google", "chips", "network", "data", "bandwidth", "world"]
source: https://www.marketscale.com/industries/software-and-technology/googles-new-tpus-tackle-ai-memory-and-data-protection-using-chips-it-already-ships
source_anchor: ""
source_lines: [1, 51]
section: "Gemini_Google"
---
# Google’s TPU 8tx and 8i Max: bandwidth vs. confidential inference

Google has divided its eighth-generation TPU into two chips. Network World reports that TPU 8tx is Google Cloud’s first TPU with confidential computing, developed with AMD, and Zeniteq says it uses TPU 8t silicon for secured inference. Google says TPU 8i Max packages two TPU 8i chips together, doubling bandwidth and SRAM while increasing HBM capacity by about a third.

This story was produced through MarketScale. See how Software & Technology teams put it to work with Executive Thought Leadership.

Key takeaways

TPU 8i Max packages two existing TPU 8i chips together. A system can change chip packaging, host processors, networking and security without an entirely new accelerator design, according to Zeniteq. TPU 8tx adds confidential computing capabilities built in partnership with AMD, according to Network World.

Earlier this year Google divided its eighth TPU generation into two chips, one built for large-scale training and the other for low-latency inference. The company said that, compared with the previous generation, the chips deliver as much as 2.7 times the training performance and 80% better price performance for inference. Mark Lohmeyer, a vice president and general manager at Google Cloud, told Network World's Taryn Plumb that offering two chips and two systems rather than one was a first for the company, and that the new pair builds on both lines.

Neither system uses a new accelerator design, according to Zeniteq. TPU 8tx pairs the same silicon as TPU 8t with fifth-generation AMD EPYC hosts and Google's low-latency Boardfly network, and TPU 8i Max packages two existing TPU 8i chips together. For the infrastructure lead picking where inference runs, that turns the menu into a question about constraints. One system answers who can see data and model weights while they are being processed. The other answers how fast memory can feed the chip when an agent drags a huge context window behind it.

## Confidential inference on training silicon

According to Network World, TPU 8tx is the first TPU from Google Cloud with confidential computing capabilities, which were developed in partnership with AMD. Lohmeyer said the protection spans the entire path. Data is encrypted on its way from the host to the TPUs, the TPUs stay secured while running inference, and the complete system is attested, so a customer can confirm it is what it claims to be.

Two assets sit inside that boundary at once. One is the enterprise's input data, whether proprietary business records or information it must protect for regulatory reasons. The other is the model's weights, which may be valuable intellectual property belonging to the model owner.

Encryption normally covers data at rest and in transit. Confidential computing covers the remaining window, while data is being processed in memory. Attestation is the cryptographic check that lets a customer confirm the hardware and software running the job match what was promised before sensitive data is sent.

In Lohmeyer's view, customers shouldn't be forced to choose between security and performance. Network World reports that Google Cloud carried the low-latency networking design of TPU 8i over to TPU 8tx, which helps cut communication latency between chips. "Because we've reduced the diameter of the network topology, the inference just gets much faster," Lohmeyer said. He added that TPU 8tx gets 2x lower latency than TPU 8t as a result, despite running on the same chip.

The design lines up with where security researchers are pointing. Greg Touhill, director of the CERT Division at Carnegie Mellon University's Software Engineering Institute, told Federal News Network's Terry Gerton that securing AI-based systems and workflows is one of seven areas his group picked for sustained research, alongside secure-by-design engineering across hardware, software and the people who run them. TPU 8tx, which Lohmeyer said adds security directly into the hardware layer, could fit that second category of problem: securing the AI system itself.

## More bandwidth than capacity in 8i Max

## TPU 8i Max, by the numbers

Network World; Google via Zeniteq

Read the grid closely and the ratios don't match. Bandwidth and SRAM double, as you'd expect from putting two chips in one package. HBM capacity rises by only a third. Bandwidth measures how much data moves per unit of time, while capacity measures how much data can be held, so doubled bandwidth should not be read as doubled context length.

Lohmeyer framed the design as adding lanes to a highway: individual cars don't go faster, but more traffic gets through at once.

The fit is conditional. For operators running many concurrent agent sessions with very long prompts, doubled bandwidth goes straight at the bottleneck Lohmeyer called the memory wall. For short-prompt chat or classification work that already fits comfortably on a single TPU 8i, the second chip may add cost without adding much.

## The software side of Google's TPU push

On the software side, Google Cloud says llm-d 1.0, an open-source project Google co-founded with partners, is a serving stack built natively for Kubernetes. It links the model servers in a cluster into one coordinated pool, so distributed LLM inference runs as a single system across fleets that mix Google Cloud TPUs and GPUs. Its features include routing that is aware of the cache, offloading of the KV cache across tiers, and flow control based on priority. That suggests Google is attacking the same memory problem from two directions: in software, by spreading KV cache across tiers, and in hardware, through the extra bandwidth of 8i Max.

Porting may also get easier. Google and Meta together open-sourced TorchTPU, a PyTorch stack built natively to bring Google TPUs straight into the PyTorch ecosystem, where they sit next to GPUs and CPUs. Developers can now use frameworks they already know, such as Hugging Face, vLLM and TorchTitan, to train, serve and deploy models on TPUs natively. For teams whose inference code was written for GPUs, that could cut the engineering cost of even trying out a TPU system. For isolation, Google introduced Agent Compute Engine, a new compute service that offers hardware-isolated microVM sandboxes to secure and scale autonomous AI agents.

## Two claims, two different baselines

The strongest objection to reading these systems as a clean either-or is that their headline numbers can't be compared. The 2x latency improvement for TPU 8tx is measured against TPU 8t. The doubled resources in 8i Max are measured against TPU 8i. As Zeniteq points out, neither figure tells a buyer which system will finish a particular workload more cheaply, and both are Google's own claims rather than independent benchmarks.

Ask Google for TPU 8tx latency at your own context length and concurrency, and ask which parties the attestation is designed to keep out. For 8i Max, test whether your agents actually stall on memory bandwidth before paying for a second chip.

Pricing is the figure a CFO will ask for first, and it will decide whether confidential inference on TPU 8tx becomes the default for regulated workloads or a specialist option reserved for the most sensitive models.
