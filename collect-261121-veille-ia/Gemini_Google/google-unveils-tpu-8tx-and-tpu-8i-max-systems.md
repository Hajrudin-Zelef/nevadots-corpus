---
id: collect-261121-veille-ia/gemini-google/google-unveils-tpu-8tx-and-tpu-8i-max-systems
title: "Google Unveils TPU 8tx and TPU 8i Max Systems"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["google", "data", "tpu", "systems", "confidential", "googles", "cloud", "chip"]
source: https://www.technology.org/2026/11/11/google-tpu-8tx-8i-max-confidential-computing-amd-epyc/
source_anchor: ""
source_lines: [1, 35]
section: "Gemini_Google"
---
## Google’s TPU 8tx and 8i Max

Data in the cloud is usually encrypted on disk and in transit, then exposed in memory while a chip works on it. Google’s new TPU 8tx aims to close that gap for AI. The company added TPU 8tx and TPU 8i Max to its eighth-generation TPU lineup on November 9, making four new TPU systems in a single year.

### **Key Takeaways**

- TPU 8tx uses the same silicon as the TPU 8t training chip, paired with fifth-generation AMD EPYC host processors, and adds hardware-level confidential computing.
- Google claims TPU 8tx delivers up to 2 times lower latency than TPU 8t, with a near-zero penalty for its security features.
- TPU 8i Max puts two TPU 8i chips in one package, doubling compute, on-chip SRAM and HBM bandwidth for long-context work.

## TPU 8tx Keeps Data Sealed

Google built TPU 8tx for regulated companies and governments that want to run frontier or proprietary models and AI agents on their most sensitive data. A secure enclave in the hardware keeps data and model weights isolated while they are being processed.

The system targets confidential inference, private fine-tuning and secure agent execution, and it also handles small-scale training. It borrows the low-latency Boardfly network from TPU 8i, which helps explain its latency gain over TPU 8t.

## TPU 8i Max Adds Memory Headroom

TPU 8i Max tackles a different limit: memory. Long context windows force a chip to hold and move huge amounts of data, and bandwidth often runs out before compute does.

By packing two TPU 8i chips into one package, Google doubles compute, SRAM and HBM bandwidth compared with a single 8i. It also fits twice the silicon into the same rack space, giving the highest compute density per rack in the TPU family.

## A Four-Part Lineup

Google introduced TPU 8t for large-scale training and TPU 8i for low-latency reasoning at its Cloud Next conference in April. The new pair completes a split by job: training, reasoning, confidential work and very long context.

Google has not given prices, availability dates or cloud regions for either system. The company also now sells TPUs for installation in customers’ own data centers.

## Why Custom Chips Keep Splitting

Custom silicon is the cloud giants’ answer to Nvidia’s pricing power. Microsoft is preparing its Maia 300 inference chip for 2027, and Amazon has made it easier to run PyTorch code on Trainium chips without rewrites. Google keeps carving its TPU line into narrower tools and pitches the new systems as hardware for AI agents that run long tasks on company data.

Confidential computing has protected CPU workloads for years, and Google built its earlier confidential virtual machines on AMD EPYC processors. Bringing it to AI accelerators could unlock workloads that banks, hospitals and governments now keep in-house. Buyers will still want proof, and critics note that Google’s launch post names no way to verify the isolation. Google’s TPU work keeps reaching new places, too, including a Suncatcher satellite that ran Gemma 4 in orbit.

*Written by Alius Noreika*
