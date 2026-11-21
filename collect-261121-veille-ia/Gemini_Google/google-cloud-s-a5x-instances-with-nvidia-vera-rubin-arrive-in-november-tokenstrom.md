---
id: collect-261121-veille-ia/gemini-google/google-cloud-s-a5x-instances-with-nvidia-vera-rubin-arrive-in-november-tokenstrom
title: "Google Cloud's A5X instances with Nvidia Vera Rubin arrive in November – Tokenstrom"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["nvidia", "google", "cloud", "rubin", "confidential", "vera", "instances", "according"]
source: https://tokenstrom.com/posts/google-cloud-s-a5x-instances-with-nvidia-vera-rubin-arrive-in-november/
source_anchor: ""
source_lines: [1, 48]
section: "Gemini_Google"
---
### What's going on

Google Cloud and Nvidia announced on October 21, 2026 that the new A5X instances will be generally available next month, in November 2026. An instance is a machine you can rent from a cloud provider. A5X is based on Nvidia's rack-scale Vera Rubin NVL72 system. The platform targets training, inference (running finished models) and, above all, large-scale reinforcement learning, where AI systems learn through trial and feedback. According to Unite.ai, the announcement was made at GTC Berlin. Availability is planned to expand to Europe. The companies didn't give an exact launch date.

### What's inside A5X

A5X brings together several building blocks:

- Nvidia Vera Rubin NVL72: According to Unite.ai, this rack-sized system contains 72 Rubin GPUs, 36 Vera CPUs, NVLink 6 interconnects, ConnectX-9 SuperNICs and BlueField-4 DPUs. Nvidia unveiled the Rubin platform on January 5, 2026.
- ConnectX-9 SuperNICs: high-speed network adapters.
- Google Cloud Virgo Network: Google's own data center network.
- Fourth-generation liquid cooling from Google Cloud.

A5X is described as part of Google's "AI Hypercomputer," where compute, networking and storage run as one integrated system. Reinforcement learning repeats a loop: generate experience, evaluate results, update the model. According to the announcement, that takes infrastructure that supports the whole workflow at scale.

### Scale

Google Cloud plans to scale its Vera Rubin infrastructure to up to 960,000 Rubin GPUs across multiple sites. According to blockchain.news and Unite.ai, the scaling targets come from the initial announcement in April 2026: up to 80,000 GPUs at a single site and up to 960,000 across multiple sites. These figures refer to clusters, not to a single instance. From the same April announcement, Unite.ai cites vendor claims that inference cost per token will fall by up to 10x and token throughput per megawatt will rise by up to 10x, in each case compared with the previous generation. None of this has been independently verified.

### First customers

**Ineffable Intelligence**, a London company founded by AlphaGo creator David Silver, got access first. It is working on "superlearners," systems that learn continuously from their own experience. Silver said you need a lot of compute and a tight link between generating experience and training on it, so that every improvement to the agent quickly shapes its next experience. The fast connection between the Vera CPU and the Rubin GPUs, and between the GPUs within the rack, helps with that.

**WPP**, a London-based marketing group, showed a reference architecture for creative agents on A5X, using Nvidia Nemotron models and Nvidia OpenShell. The agents turn conversational requests into multistep workflows such as video generation, upscaling and restyling, with a human approving before execution. OpenShell provides secured sandboxes (isolated environments) that control access to sensitive brand assets and creative tools. In early tests, a workload that would otherwise have needed a multi-server cluster ran on a single A5X instance.

### Confidential computing and energy

Google Cloud wants to be among the first providers to offer Nvidia Confidential Computing at rack level for bare-metal instances, such as A4X Max with GB300 NVL72. With confidential computing, data and models stay protected even while being processed, even on someone else's hardware. According to Unite.ai, "Confidential Bare Metal" is a new option alongside Confidential Containers and Confidential VMs. Confidential computing is also becoming generally available on G4 VMs with RTX PRO 6000 Blackwell Server Edition, for confidential inference and fine-tuning. Named users include Vertebrae, Polaris Cloud and Confidential Core AI.

On energy demand, both companies are evaluating Nvidia DSX MaxLPS to optimize performance per watt and fit more capacity into an existing power budget. So far that is only an evaluation, not a finished feature.

### Our take

The news mainly confirms the timeline: Rubin hardware should be rentable in a major cloud from November. That matters for companies that need agent and RL training, especially with the announced European availability. Plenty remains open: the sources give no pricing, GPUs per instance, memory, network bandwidth or regions. The performance claims come from the vendors, and the WPP results are early internal tests. The 960,000 GPUs are a build-out target, not the current state.

## Ask the AI assistant

Questions about this article, installation or background? Our assistant knows this page and runs on our own machine, not in someone else's cloud.

**Connecting …**

The Assistant is an AI (Gemma 4) and can make mistakes – please check important details against the source. Questions are used only to generate an answer and are deleted after one hour. Please don't enter any personal data.

## Comments

What do you think? Already tried it? Write it here, no sign-up needed.

Comments are public. Links aren't allowed, and we delete abusive posts. You can delete your own comment at any time in this browser. Learn more in the Privacy Policy.
