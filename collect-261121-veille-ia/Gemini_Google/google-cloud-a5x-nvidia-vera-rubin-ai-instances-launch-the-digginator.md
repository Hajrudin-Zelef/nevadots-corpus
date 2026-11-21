---
id: collect-261121-veille-ia/gemini-google/google-cloud-a5x-nvidia-vera-rubin-ai-instances-launch-the-digginator
title: "Google Cloud A5X NVIDIA Vera Rubin AI Instances Launch - The Digginator"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "cloud", "nvidia", "vera", "rubin", "training", "platform", "instances"]
source: https://thedigginator.com/google-cloud-a5x-nvidia-vera-rubin-ai-instances/
source_anchor: ""
source_lines: [1, 71]
section: "Gemini_Google"
---
# Google Cloud Brings NVIDIA Vera Rubin AI Infrastructure to A5X Instances, Targeting Large-Scale AI Training

The upcoming A5X rollout combines NVIDIA’s next-generation Rubin computing platform with Google Cloud networking and liquid cooling, while early customers explore reinforcement learning and AI-powered creative workflows.

## Google Cloud Prepares A5X Launch for Advanced AI Workloads

Google Cloud plans to introduce general availability of its A5X instances next month, bringing NVIDIA’s Vera Rubin computing platform to cloud customers seeking infrastructure for advanced AI training, inference and large-scale reinforcement learning. The company also plans to expand availability into Europe.

The A5X platform combines NVIDIA Vera Rubin NVL72 systems, NVIDIA ConnectX-9 SuperNICs, Google Cloud’s Virgo network and its fourth-generation advanced liquid-cooling technology. The integrated design targets demanding AI workloads, particularly agentic inference and reinforcement learning systems that need to generate experiences, evaluate results and improve continuously.

Google Cloud also plans to scale its Vera Rubin-based infrastructure to as many as 960,000 Rubin GPUs across multiple sites, outlining an ambitious expansion of its AI computing capacity.

Rather than treating training and inference as separate infrastructure challenges, the platform is designed to bring compute, networking and storage together through Google Cloud’s AI Hypercomputer. That approach could be particularly useful for AI systems that learn through repeated interactions rather than relying solely on a fixed training dataset.

## AlphaGo Creator David Silver’s Company Is an Early Customer

London-based Ineffable Intelligence, founded by David Silver, the researcher known for his work on AlphaGo, was the first customer to access A5X. The company is using the platform to advance large-scale reinforcement learning and develop what it calls “superlearner” systems — AI models designed to improve continuously through experience.

Reinforcement learning involves training a system through interactions, feedback and evaluations. Scaling this process requires infrastructure that can generate new experiences and use the resulting information to update a model without excessive delays between those stages.

“We’re building superlearners that continually improve through their own experience,” Silver said.

He explained that the research requires substantial computing power and a close connection between experience generation and training, allowing improvements to influence subsequent interactions more quickly.

Silver also highlighted the combination of compute performance and high-bandwidth connectivity in NVIDIA’s Vera CPU and Rubin GPUs, including communication between GPUs within a rack. According to him, those capabilities could allow Ineffable Intelligence to pursue larger and more ambitious reinforcement learning experiments.

The company’s early access provides an initial example of how Google Cloud expects A5X to support AI development beyond conventional model training.

## WPP Tests Creative AI Agents on a Single A5X Instance

The platform is also being used to explore enterprise applications. WPP, the London-based marketing and communications company, has demonstrated a creative agent reference architecture running on NVIDIA Vera Rubin-based A5X instances.

The system uses NVIDIA Nemotron models and NVIDIA OpenShell, part of NVIDIA’s reference architecture for building safer AI agents.

These agents can translate conversational instructions into multistep creative workflows, including video generation, upscaling and restyling. Human approval is required before the workflows execute, while OpenShell provides governed sandboxes intended to control access to sensitive brand assets and creative tools.

In WPP’s initial tests, workloads that typically required a cluster of multiple nodes ran on a single A5X instance.

That result suggests the platform may offer a more consolidated environment for certain complex creative AI tasks. However, the initial test does not establish that every comparable workload will achieve the same consolidation or performance gains.

For marketing organizations handling proprietary assets and brand-specific material, the combination of agent automation and controlled execution is also important. The objective is not simply to generate creative content, but to enable AI agents to carry out more of the workflow while preserving human oversight and restrictions on sensitive resources.

## Google Cloud Expands Confidential Computing for AI

Alongside the A5X rollout, Google Cloud is expanding its focus on protecting AI workloads that process proprietary models, sensitive information and enterprise data.

The company will be among the first cloud providers to offer NVIDIA Confidential Computing at rack scale on bare-metal instances such as A4X Max, which are powered by NVIDIA GB300 NVL72 systems.

Google Cloud is also making Confidential Computing generally available on G4 virtual machines powered by NVIDIA RTX PRO 6000 Blackwell Server Edition GPUs. The instances are intended to provide a flexible environment for confidential AI inference and fine-tuning.

Customers including Vertebrae, Polaris Cloud and Confidential Core AI are using Confidential G4 for applications involving conversational privacy, verifiable agent execution and policy-governed enterprise intelligence.

Confidential computing is designed to help protect data and workloads while they are being processed. That capability can be especially relevant as businesses give AI agents access to internal information, proprietary models and tools capable of taking actions.

The broader deployment challenge is ensuring that these systems can operate at scale without exposing sensitive information or granting agents more access than they need. The specific protections available depend on the implementation and configuration of the underlying infrastructure.

## Power Efficiency Becomes Part of the Infrastructure Plan

Google Cloud and NVIDIA are also exploring NVIDIA DSX MaxLPS capabilities for Vera Rubin-based infrastructure, with the aim of optimizing performance per watt.

The effort reflects a growing constraint on large-scale AI expansion: adding more computing capacity also increases pressure on data-center power budgets and cooling systems.

By improving the amount of useful computing work delivered for each unit of power, the companies aim to expand capacity within existing energy limits. The announcement describes this as an area of exploration, rather than a completed deployment or a quantified efficiency improvement.

## What Comes Next for A5X

The immediate milestone is the planned general availability of A5X instances next month, followed by an intended expansion into Europe. Google Cloud’s longer-term plan to deploy up to 960,000 Rubin GPUs across multiple sites indicates that the company is preparing for substantial demand, although the announcement does not provide a detailed deployment timetable for that full capacity.

For AI developers, the initial use cases span two different priorities: Ineffable Intelligence is pursuing systems that learn through repeated experience, while WPP is testing agents that can carry out complex creative tasks under human supervision.

Together, these examples illustrate the workloads Google Cloud is targeting with its integrated AI infrastructure. The next measure of A5X’s impact will be how broadly customers can access the instances and how effectively they perform under real-world training, inference and agentic workloads.
