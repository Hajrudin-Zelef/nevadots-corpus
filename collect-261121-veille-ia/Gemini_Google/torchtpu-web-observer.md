---
id: collect-261121-veille-ia/gemini-google/torchtpu-web-observer
title: "TorchTPU - Web Observer"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "systems", "compute", "training", "vahdat", "four", "silicon", "about"]
source: https://web-observer.com/tag/torchtpu/
source_anchor: ""
source_lines: [1, 113]
section: "Gemini_Google"
---
**Takeaway:** On November 9, 2026, Google expanded its eighth-generation Tensor Processing Unit portfolio with two additional systems: **Google TPU 8tx**, aimed at secure and confidential inference (and small-scale training), and **TPU 8i Max**, aimed at massive context windows and what the company calls the highest compute density per rack in the family. In a blog post the same day, Amin Vahdat, senior vice president and chief technologist for AI and infrastructure, wrote that the additions mark four TPU systems in a single year — joining TPU 8t and TPU 8i from Google Cloud Next in April. These are new *systems*, not new chips: 8tx is built on the same silicon as 8t and paired with 5th Generation AMD EPYC CPU hosts and the Boardfly network; 8i Max combines two TPU 8i chips on one package and, Google says, delivers double the compute, double the SRAM, and double the HBM bandwidth of TPU 8i. Vahdat also called TPUs “a rapidly growing business with nearly 100 customers.” The post does not publish pricing, regions, or a general-availability date.

This is a custom-silicon and systems story about how Google says it is specializing an existing accelerator generation for confidential enterprise work and long-context agents. It is not a process-node launch and should not be read as a datasheet: Monday’s blog lists no peak TFLOPS, no HBM capacity for the new packages, and no list prices. It is also distinct from Google’s separate plan for a $17 billion first data center in Wyoming — more specialized compute here, campus-scale power and land there. A board inquiry at Nvidia, opened a day earlier, is separate corporate-governance news from the same week. Google’s TPU post does not mention it, and the two should not be collapsed.

## A chip is not a system

Vahdat opened with a distinction that Data Center Dynamics’ Charlotte Trueman correctly treated as the lead. Designing a new chip, he wrote, “means laying out billions of microscopic transistors on a silicon wafer in a multi-year process bound by the laws of physics and a complex, long-duration manufacturing lifecycle.” A system “is about how we bring together the compute, networking, and storage around the chip,” including packaging, high-speed memory, optical networking, and security. That is how Google can announce four systems in one year without four new die designs. Trueman put it plainly: Google “reconfigured the compute, networking, storage, and packaging around its TPU 8t (for training) and TPU 8i (for inference).” SiliconANGLE’s Kyt Dotson was looser, calling the additions “two new specialized silicon chips.” Google’s wording is the safer one.

Google made that modularity a founding bet, Vahdat wrote, because “AI software would evolve much faster than silicon manufacturing ever could,” allowing “specialized supercomputers in a matter of months rather than years.” TechTarget’s Joab Jackson, writing from a briefing, quoted Mark Lohmeyer, Google Cloud’s vice president and general manager for AI and computing infrastructure, saying AI is no longer “mostly about how much compute you had available” but also about data movement, memory efficiency, latency, cost, and security — and that you “can’t optimize that infrastructure just by focusing on the chip.”

## Four systems in one year: 8t, 8i, then 8tx and 8i Max

In April, Google introduced TPU 8t for large-scale training and TPU 8i for real-time reasoning. Vahdat’s November recap says those two “offer up to 2.7 times better training performance and 80% better inference price performance, respectively, versus our previous generation of TPUs.” Those relatives belong to the April pair, and they compare Google’s new systems with its own prior TPUs — not with anyone else’s accelerators. He described TPU 8t as “a training powerhouse for massive foundation models with maximum ‘goodput’ — or the useful intelligence delivered per watt of energy used,” and TPU 8i as “the reasoning engine, designed to break the memory wall, cutting inter-chip communication delays by up to half.” Monday’s systems are “two new configurations of these foundational systems” for hardware-level data security and “million-token context scale”:

- **TPU 8t** for scale-up training.
- **TPU 8i** for low-latency reasoning.
- **TPU 8tx** for confidential computing.
- **TPU 8i Max** for massive context windows and the highest compute density per rack.

TechTarget added calendar context the blog omitted: the April variants were “still due to go online by the end of this year.” Google is widening the eighth-generation menu before it has, in that account, broadly finished shipping the first two entries. Readers should not assume TPU 8tx or 8i Max are in a region picker today.

## What Google TPU 8tx adds: confidential inference on training silicon

Google TPU 8tx is the third eighth-generation system. Vahdat wrote that it is “optimized for secure and confidential inference and is also great for small-scale training,” and that it is built for “regulated enterprises and governments running frontier or proprietary models and autonomous agents alongside their most sensitive data.” Data is usually encrypted at rest and in transit, then unlocked in memory while a processor works on it. TPU 8tx, Google says, “builds security directly into the hardware layer, keeping sensitive data and model weights protected and isolated even while in active use.”

The system is “built on the same silicon as TPU 8t and paired with 5th Generation AMD EPYC CPU hosts.” Data Center Dynamics and SiliconANGLE both shortened this to “custom AMD” hosts; Google’s post names fifth-generation EPYC processors. SiliconANGLE also paraphrased the isolation story as a “secure enclave” — the outlet’s language, not Vahdat’s. TPU 8tx adopts “the same low-latency Boardfly network we created for real-time reasoning,” and Google claims “up to 2x lower latency compared to TPU 8t with a near-zero performance penalty.” TechTarget reported Lohmeyer saying data and weights “remain encrypted and isolated at the hardware level during processing, with a near-zero performance penalty and half the latency than the TPU 8t.” Half the latency is another way of stating that “up to 2x lower” figure. Google published no absolute latency, throughput, or independent audit.

The pitch is that training-grade compute and a fast inference network can live on one confidential system, so a team can fine-tune on private data, distill a large model, and train agents with reinforcement learning “without needing to migrate sensitive data between separate clusters.” Analyst Patrick Moorhead, quoted by TechTarget, restated that use case for “a bank or government agency.” Those are illustrations, not named design wins in Google’s post.

## TPU 8i Max: two chips, one package, and the memory wall

TPU 8i Max, the fourth system, is aimed at a different bottleneck. “When dozens of AI agents work together across an entire software codebase or hours of video at once, memory bandwidth quickly becomes the bottleneck,” Vahdat wrote. Google’s answer was packaging, not a new inference die: “we took two low-latency TPU 8i chips and combined them onto a single package.” Google says the result “scales compute and memory in lockstep, delivering double the compute, double the SRAM and double the HBM bandwidth of TPU 8i,” and that it “fits twice the silicon within the existing rack footprint,” so operators “can deploy significantly more computing power per square foot and per watt.” Monday’s blog does not publish an absolute SRAM figure, an HBM capacity, or watts per rack.

TechTarget reported that the two-chip design gives “the 8i Max… 768 MB of on-chip static RAM and 17.2 TB/s of high-bandwidth memory (HBM) bandwidth.” Those totals are consistent with a straightforward doubling of TPU 8i’s SRAM and HBM bandwidth, but Google did not reprint either the 8i baselines or the doubled numbers on November 9. SiliconANGLE wrote that the system delivers “double the high-bandwidth memory” of TPU 8i. Google’s wording was HBM *bandwidth*, not a published doubling of HBM capacity. Dotson also tied Google’s “million-token context scale” language to Gemini’s widely discussed 1-million-token window — roughly 750,000 words or 1,500 pages, he wrote — and noted that many smaller models still sit between 128,000 and 256,000 tokens. That framing is SiliconANGLE’s. Google did not give TPU 8i Max a rated context length.

## Nearly 100 customers, and a wider software door

Vahdat said today’s TPU uses include training and serving Gemini, agentic workloads, Google Cloud customers, and “allowing customers to deploy TPUs directly into their own data centers.” He called TPUs “a rapidly growing business with nearly 100 customers — including frontier labs, trading firms, retailers, universities, and more.” That figure is Google’s claim; the blog names none of them. TechTarget used a slightly broader phrase — “more than 100 customers and partners” — and mentioned Anthropic, “which will expand its use of next-generation TPUs beginning in 2027,” plus a Caltech cancer-research group that Moorhead said had moved some work from its own HPC system onto Cloud TPUs. Those identifications stay with TechTarget. They are not a roster from Monday’s post, and Anthropic’s reported 2027 expansion is not a ship date for TPU 8tx or 8i Max. TechTarget was explicit: “Google did not provide any availability dates and pricing.”

On software, Vahdat said Google open-sourced TorchTPU “last month,” so “PyTorch code and Hugging Face models now run natively on TPUs,” with “out-of-the-box TPU backends” for vLLM and SGLang. A new open-source TPU agent suite, in preview, is described as plugging into Claude Code, Cursor, and Google Antigravity to automate onboarding, diagnose bottlenecks, and autotune kernels in plain language. A new open-source XLA Serving Engine, he wrote, lets teams train in Python and serve “directly in a low-latency C++ runtime across TPUs, GPUs, and CPUs.” Google also said a native vLLM-TorchTPU stack “achieved state-of-the-art price-performance marks” on InferenceX and AgentX, tests it described as covering realistic agentic coding and long-context work. That “state-of-the-art” line is Google’s; the post includes no results table.

## Agent Compute Engine, TPU Sync, and slicing the fleet

Because agents are stateful, Vahdat wrote, they “frequently pause for seconds or hours while waiting on a database lookup, an external software tool, or human feedback.” Google’s new Agent Compute Engine software “automatically suspends and resumes idle sessions, cutting operational infrastructure costs.” No percentage saving was published. TPU Sync, which Google says it is open-sourcing, is described as the model-state layer for TPU clusters. The claim is that it “turns chip HBM, host DRAM, shared storage, and the data center network into a single, co-designed memory hierarchy,” automatically tiering large KV caches off the chip in milliseconds, streaming context across disaggregated prefill and decode nodes, and synchronizing reinforcement-learning weights. Google says that can “cut KV transfer latency and Time to First Token (TTFT) by more than 90%.” That 90% figure is a software claim, not a hardware spec for 8i Max.

Cloud Pathways is now generally available on Google Kubernetes Engine. Vahdat described it as “the Google DeepMind runtime behind Gemini,” using a single-controller architecture and pathwaysutils so customers can “orchestrate large-scale TPU systems across multiple slices as if they were one local machine” for pre-training, reinforcement learning, and multi-host inference, with colocated Python sidecars, sub-minute slice hot-swapping, and elastic resharding. GKE dynamic slicing is also generally available. Google says it “decouples physical hardware provisioning from logical slice formation using optical circuit switching (OCS) technology,” assembling cubes into larger topologies (super-slicing) or splitting them into isolated workloads (sub-slicing) without “rigid, manual nodepool recreation cycles.” The promised gains are “up to 5x” faster job startup and “up to 4.5x” faster fault recovery. Those multipliers are Google’s.

## Rack density still runs into power and land

Monday’s hardware story is about doing more inside an existing rack: twice the silicon in the same footprint, more compute per square foot and per watt, and a confidential path that avoids copying sensitive data between clusters. Google’s larger infrastructure story is still about finding enough electricity and land to turn those racks on. That is the link, and the limit, of comparing this launch with Google’s November 5 plan for a $17 billion first Wyoming data center. The Wyoming announcement was about a long-term campus, a contracted 702 MW fuel-cell development with carbon capture, and air cooling in a water-constrained county. It did not mention TPU 8tx or 8i Max, and Monday’s TPU post does not mention Wyoming.

The same caution applies to Anthropic’s reported agreement to pay about $3.4 billion for 1.7 gigawatts of Caterpillar gas-generator commitments. That Financial Times story, which Reuters could not immediately verify, is about factory queues for generation equipment — not a TPU win, and not the same fact as TechTarget naming Anthropic as a TPU customer that plans to expand next-generation use in 2027. Google is talking about compute density per watt inside a rack; parts of the industry are still shopping for the watts themselves. A Nvidia board investigation opened on November 8 after reports that CEO Jensen Huang had been the target of a foreign intelligence effort is likewise adjacent market news, not a subplot of this launch. Nvidia’s directors said they had no reason to believe national security or company secrets were compromised and that Huang remains CEO. Google did not mention Nvidia in the TPU post.

For buyers, the useful facts are narrower than “two more TPUs.” Google is specializing a generation it already designed, and the software around those boxes is as much of the pitch as the packages. None of that, on the public evidence, comes with a price, a region, or an order date. Vahdat closed by saying Google is using DeepMind’s AlphaChip and Gemini “to deliver chip layouts in hours rather than weeks,” and he pointed to a November 9 broadcast at 9 a.m. Pacific Time.

## What this is — and what it is not

| Claim | Status | Notes | 
|---|---|---|
| Google announced TPU 8tx and TPU 8i Max on November 9, 2026, as the third and fourth eighth-generation TPU systems in one year | Confirmed by Google | Vahdat blog; joins TPU 8t and TPU 8i from April | 
| TPU 8tx and TPU 8i Max are new chips on a new process | No, not as Google described them | 8tx uses TPU 8t silicon; 8i Max packages two TPU 8i chips | 
| TPU 8t: up to 2.7× better training performance vs prior Google TPUs; TPU 8i: 80% better inference price-performance; 8i cuts inter-chip delays by up to half | Google’s April-generation claims, restated Nov. 9 | Versus Google’s previous TPU generation | 
| TPU 8tx isolates data and model weights at the hardware layer even while in use; also suited to small-scale training | Stated by Google | Aimed at regulated enterprises and governments | 
| TPU 8tx pairs TPU 8t silicon with 5th Generation AMD EPYC hosts and the Boardfly network | Stated by Google | DCD/SiliconANGLE said “custom AMD”; “secure enclave” is SiliconANGLE’s paraphrase | 
| TPU 8tx delivers up to 2× lower latency than TPU 8t with a near-zero performance penalty | Google’s claim | TechTarget: Lohmeyer said half the latency of TPU 8t; no absolute figures | 
| Fine-tuning, distillation, and reinforcement learning can run on one confidential 8tx system without moving sensitive data between clusters | Stated by Google | A product claim, not a named customer case study | 
| TPU 8i Max doubles compute, SRAM, and HBM bandwidth versus TPU 8i and fits twice the silicon in the existing rack footprint | Stated by Google | No absolute Max specs in the Nov. 9 blog | 
| TPU 8i Max has 768 MB of SRAM and 17.2 TB/s of HBM bandwidth | Reported by TechTarget; not in Google’s blog | Consistent with a 2× reading of TPU 8i; Google did not reprint the totals | 
| TPU 8i Max is rated for a 1-million-token context window | Not stated by Google | SiliconANGLE connected “million-token context scale” to Gemini’s 1M-token window | 
| TPUs have nearly 100 customers, including frontier labs, trading firms, retailers, and universities | Stated by Google | TechTarget: “more than 100 customers and partners”; no names in Vahdat’s post | 
| Anthropic will expand next-generation TPU use in 2027; Caltech has moved some research onto Cloud TPUs | Reported by TechTarget | Not an announced GA date for 8tx or 8i Max | 
| TorchTPU open-sourced last month; vLLM/SGLang TPU backends; TPU agent suite in preview; XLA Serving Engine new and open-source | Stated by Google | Agent suite described as working with Claude Code, Cursor, and Google Antigravity | 
| vLLM-TorchTPU achieved state-of-the-art InferenceX and AgentX price-performance | Google’s claim | No results table in the blog | 
| Agent Compute Engine suspends and resumes idle agent sessions | Stated by Google | No percentage cost saving published | 
| TPU Sync cuts KV-transfer latency and TTFT by more than 90% | Google’s claim | Open-sourced model-state layer across HBM, DRAM, storage, and the network | 
| Cloud Pathways and GKE dynamic slicing are generally available; slicing can cut job startup by up to 5× and fault recovery by up to 4.5× | Stated by Google | Pathways: DeepMind runtime behind Gemini; slicing uses optical circuit switching | 
| Google published pricing, regions, or a general-availability date for TPU 8tx and TPU 8i Max | No | Absent from Google’s blog and TechTarget’s briefing recap | 

## FAQ

### What did Google announce on November 9, 2026?

Google announced two new eighth-generation TPU systems: Google TPU 8tx, for secure and confidential inference plus small-scale training, and TPU 8i Max, for massive context windows and higher compute density per rack. Amin Vahdat wrote that they join TPU 8t and TPU 8i, bringing the generation to four systems in a single year.

### What is the difference between a new TPU chip and a new TPU system?

In Google’s telling, a new chip is a multi-year exercise in laying out transistors. A new system is how compute, networking, storage, packaging, memory, and security are arranged around a chip. TPU 8tx uses the same silicon as TPU 8t; TPU 8i Max puts two TPU 8i chips on one package. Data Center Dynamics described the launch as a reconfiguration of the April systems.

### What is Google TPU 8tx designed to do?

Google says TPU 8tx keeps sensitive data and model weights protected and isolated at the hardware layer even while they are being used. It pairs TPU 8t silicon with 5th Generation AMD EPYC hosts and the Boardfly network, and Google claims up to 2× lower latency than TPU 8t with a near-zero performance penalty. The intended users are regulated enterprises and governments that want to fine-tune, distill, and train agents on private data without moving that data between clusters.

### What is TPU 8i Max, and did Google publish full specs?

TPU 8i Max combines two TPU 8i chips in a single package. Google says that doubles compute, SRAM, and HBM bandwidth versus TPU 8i and places twice the silicon in the existing rack footprint. The November 9 blog does not publish absolute SRAM, HBM-capacity, TFLOPS, power, or price figures. TechTarget reported 768 MB of on-chip SRAM and 17.2 TB/s of HBM bandwidth; those numbers are not in Vahdat’s post.

### Did Google say when these systems will be available, or what they will cost?

No. The company blog does not include a general-availability date, a price, or a list of cloud regions. TechTarget, after a briefing, wrote the same. It separately reported that the April TPU 8t and 8i systems were still due to go online by the end of 2026, and that Anthropic plans to expand next-generation TPU use beginning in 2027 — which is not a ship date for TPU 8tx or 8i Max.

### What software did Google announce alongside the hardware?

Vahdat said Google open-sourced TorchTPU last month so PyTorch and Hugging Face models run natively on TPUs, with TPU backends for vLLM and SGLang. He also described a preview TPU agent suite (Claude Code, Cursor, Google Antigravity), a new open-source XLA Serving Engine, Agent Compute Engine for idle sessions, TPU Sync (which Google says can cut KV-transfer latency and time to first token by more than 90%), and the general availability of Cloud Pathways and GKE dynamic slicing. Google claimed state-of-the-art InferenceX and AgentX price-performance for its vLLM-TorchTPU stack.

### How many TPU customers does Google say it has?

Vahdat wrote that TPUs are “a rapidly growing business with nearly 100 customers,” including frontier labs, trading firms, retailers, and universities, and that some customers can now deploy TPUs in their own data centers. He did not name them. TechTarget used the phrase “more than 100 customers and partners” and mentioned Anthropic and a Caltech research group.

### Is this the same story as Google’s Wyoming data center or Nvidia’s board investigation?

No. The Wyoming announcement is about a $17 billion first campus in the state and how Google plans to power it, including a 702 MW fuel-cell project. The Nvidia item is a board-level inquiry, opened November 8, after reports that Jensen Huang had been targeted by a foreign intelligence effort; the board said it had no reason to believe secrets were compromised. Monday’s TPU post is a product-and-systems announcement.

Sources: Amin Vahdat, “Our newest TPUs: 8tx and 8i Max, built for the agentic era,” Google (November 9, 2026); Charlotte Trueman, Data Center Dynamics (November 9, 2026); Kyt Dotson, SiliconANGLE (November 9, 2026); Joab Jackson, TechTarget (November 9, 2026). Quotations about the chip-versus-system distinction, the four-system portfolio, TPU 8tx, TPU 8i Max, the “nearly 100 customers” figure, and the software stack are from Vahdat’s post. Mark Lohmeyer’s briefing remarks, the 768 MB / 17.2 TB/s figures, the note that April systems were still due online by the end of 2026, and the Anthropic 2027 and Caltech mentions are from TechTarget. SiliconANGLE’s “secure enclave” language and 1-million-token Gemini framing are that outlet’s. We have not seen a public TPU 8tx or 8i Max datasheet, price list, or availability schedule.
