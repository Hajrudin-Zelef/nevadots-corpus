---
id: collect-261121-veille-ia/gemini-google/google-tpus-head-to-oracle-cloud-as-oci-adds-rival-ai-chips
title: "Google TPUs Head to Oracle Cloud as OCI Adds Rival AI Chips"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["oracle", "google", "cloud", "tpus", "chips", "nvidia", "inference", "available"]
source: https://for.you.com/posts/google-tpus-head-to-oracle-cloud-as-oci-adds-rival-ai-chips-2b9ba24f
source_anchor: ""
source_lines: [1, 45]
section: "Gemini_Google"
---
## What Oracle announced in Las Vegas

On October 27, Oracle used its AI World conference to make a broad change to its cloud hardware strategy. Google's Tensor Processing Units will be offered through Oracle Cloud Infrastructure (OCI), and the two companies say this is the first time the chips will be available on a hyperscale cloud outside Google itself.6 Under the arrangement, Oracle puts the TPUs in its own data centers and provides the power, cooling and networking.10 The chips go on sale in calendar 2027, and capacity is supposed to grow over several years after that.5

The TPU deal was the biggest of several. On the same day, Oracle said Nvidia's Vera Rubin NVL72 rack systems and Vera CPUs were generally available on OCI, and that the two companies plan to grow OCI Superclusters to multi-gigawatt scale.3 Oracle also lined up inference hardware from Cerebras and SambaNova, along with Jalapeño, the inference chip OpenAI built with Broadcom. It signed a multiyear Intel Xeon agreement that includes planned OCI X14 instances on Diamond Rapids, as well as new parts from AMD and Positron.317 Futurum's Brendan Burke counted compute agreements with five silicon suppliers signed in one day. He called OCI the first hyperscale cloud built around "silicon plurality."3

Investors liked it. Oracle shares climbed about 5% in afternoon trading to $132.28, though the stock was still down 31% for the year.8

## The TPU deal is a change for Google too

Most coverage treated this as an Oracle story. It also says a lot about Google. Until now, the only way to rent TPUs was from Google Cloud.1 One report says this is the first time Google is selling TPUs for use in someone else's data center, rather than only renting out capacity.4 The same day, The Information and Bloomberg reported that OpenAI plans to buy Google TPUs directly. Neither company would comment, and no volumes or prices were disclosed.9

Taken together, these reports suggest Google is starting to sell its custom silicon as a merchant product, not just using it to win customers for its own cloud. One analysis notes that the Oracle deal lets Google reach more buyers without building more Google Cloud regions.1 Oracle's OCI chief Karan Batta called it a "multi-year, multi-gigawatt partnership" and said Oracle will get each new TPU generation on the day Google launches it.2 That pledge is a statement of intent. It is not a published schedule or contract term, and Oracle has released no capacity figures to support it.1

The chips involved are Google's eighth generation, which comes in two versions. The TPU 8t is for training, and Oracle's release says it delivers nearly three times the per-pod compute of the previous generation.6 The TPU 8i is for inference. Google claims it offers 80% better inference performance per dollar than its predecessor, and one outlet warns that buyers should check that vendor number independently before relying on it.12 The 8i also uses a network design called Boardfly, which reportedly cuts the maximum number of hops across a pod from 16 to 7. That matters most for multi-step agent workloads, where small delays add up.12

## Nvidia is still the anchor

The coverage splits on what this means for Nvidia. The Register read the TPU deal as Oracle trying to reduce its heavy reliance on Nvidia, a supplier it has already signed deals worth tens of billions of dollars with.5 CNBC's interview with OCI president Mahesh Thiagarajan points the other way. One summary of that interview says he stressed the move is not a shift away from Nvidia but a response to tight chip supply.16

The product details favor Thiagarajan's version. Vera Rubin systems are available now, while TPUs and Cerebras won't arrive until 2027 and Jalapeño and SambaNova's SN50 have no OCI date at all.15 Positron's Atlas inference server was the only new non-Nvidia accelerator Oracle described as available on launch day.15 Oracle also said OpenAI trained and served GPT-6 Astra on an OCI Supercluster with more than 100,000 Nvidia Blackwell GPUs.1 Futurum describes the multi-gigawatt Vera Rubin expansion as keeping Nvidia the "anchor tenant" of Oracle's buildout.3

The most accurate reading is that Oracle is not leaving Nvidia. It is gaining bargaining power. A cloud with five accelerator families can play suppliers against each other and send each job to the cheapest chip that fits, even if most 2027 workloads still run on GPUs.1 Nvidia doesn't lose this year. The bigger risk for Nvidia is that inference, the fastest-growing workload, becomes a market where its GPUs are one option among several rather than the default.

## Inference costs are driving the change

Almost every non-Nvidia chip in the announcement is aimed at inference, where cost per token and response time decide purchases. Oracle's original Nvidia business was built on large training runs.1 Futurum argues that inference is splitting into stages that suit different hardware. GPUs handle prefill, which processes the prompt. Specialized chips take decode, which generates the response one token at a time.3 SambaNova's pitch matches that split exactly: its SN50 handles decode next to GPUs. SambaNova claims up to 5x the speed of competing chips and 3x lower total cost of ownership in a 20 kW air-cooled rack.3

The cost claims across the lineup are large, and all of them come from the vendors. OpenAI says Jalapeño delivers 1.5 to 1.9 times the throughput per watt of the systems it was compared against, on specific models. Positron's 3x to 5x cost advantage reportedly applies only to targeted workloads. SambaNova's six-month payback target assumes 70% billable utilization.15 None of these figures has been tested on OCI, and Oracle has published no prices for any of the new accelerators.1 For a rough idea of TPU pricing, Google Cloud's own page reportedly lists an older tpu7x configuration at $13.80 an hour on demand, with lower rates for committed use.1 Until Oracle publishes its own prices, enterprise buyers have no way to compare.

CPUs are part of the same story. AI agents spend much of their time running code and calling tools, not doing GPU math. So Oracle now buys CPUs from four suppliers: Nvidia's Vera, Intel's Diamond Rapids, AMD EPYC and Ampere's Arm chips.3 One report says OCI is offering 150,000 standalone 88-core Vera CPUs so customers like OpenAI can scale agent workloads without leaving GPUs idle.

## Oracle's own silicon is the network

Oracle has no in-house accelerator, unlike Amazon with Trainium or Microsoft with Maia 200.8 Futurum argues that Oracle's proprietary chip strategy is really its Acceleron networking. A custom SmartNIC lets Google TPUs keep their native interconnects inside the pod while connecting to OCI storage and databases outside it.3 Acceleron uses AMD Pensando DPUs and optical links at 400G and 800G.12

In this view, Oracle's margins come from owning the connective layer under everyone else's chips, not from reselling them. The question Futurum raises, and the main open question here, is whether owning the network earns enough to cover buying all that compute at market prices.3

## Money and complexity

The weak points are financial and operational. Oracle has not said how it will pay for the new capacity, and it has kept its fiscal 2027 capital spending plan at $90 billion to $95 billion.10 Its cloud infrastructure revenue reportedly grew 121% and its backlog stands at $664 billion, but $28.5 billion in capital spending pushed free cash flow to negative $5.4 billion.8 Jensen Huang has said Vera Rubin systems can bring Nvidia up to $40 billion in revenue per gigawatt, which suggests the multi-gigawatt commitment is worth tens of billions of dollars.3

Running five or six chip platforms also means five or six software stacks: CUDA, XLA, Cerebras' SDK and SambaNova's dataflow tools. Hiding those differences from customers will fall to OCI's schedulers and Kubernetes layer.3

Some details differ between outlets. One says TPUs arrive in early 2027, while Oracle's own wording is calendar 2027.95 Cerebras' timing is listed as the first half of 2027 in one account and simply 2027 in another.1615 Those gaps point to the main caution. Much of what Oracle announced is roadmap, not product. The deal changes the hardware race on paper now. Whether it changes it in practice depends on 2027: TPUs arriving on schedule, named customers signing up for them, and independent tests showing whether the promised inference savings hold up.8
