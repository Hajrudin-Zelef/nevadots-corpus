---
id: collect-261121-veille-ia/gemini-google/nvidia-and-google-cloud-collaborate-to-advance-agentic-and-physical-ai
title: "NVIDIA and Google Cloud Collaborate to Advance Agentic and Physical AI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["nvidia", "google", "cloud", "blackwell", "gpus", "agentic", "physical", "systems"]
source: https://blogs.nvidia.com/blog/google-cloud-agentic-physical-ai-factories/
source_anchor: ""
source_lines: [1, 73]
section: "Gemini_Google"
---
NVIDIA and Google Cloud have collaborated for more than a decade, co‑engineering a full‑stack AI platform that spans every technology layer — from performance‑optimized libraries and frameworks to enterprise‑grade cloud services.

This foundation enables developers, startups and enterprises to push agentic and physical AI out of the lab and into production — from agents that manage complex workflows to robots and digital twins on the factory floor.

At Google Cloud Next this week in Las Vegas, the partnership reaches a new milestone, with advancements to expand Google Cloud AI Hypercomputer for AI factories that will power the next frontier of agentic and physical AI.

These include the new NVIDIA Vera Rubin-powered A5X bare-metal instances; a preview of Google Gemini on Google Distributed Cloud running on NVIDIA Blackwell and NVIDIA Blackwell Ultra GPUs; confidential VMs with NVIDIA Blackwell GPUs; and agentic AI on Gemini Enterprise Agent Platform with NVIDIA Nemotron open models and the NVIDIA NeMo framework.

## **Next-Generation Infrastructure: From NVIDIA Blackwell to Vera Rubin**

At Google Cloud Next, Google announced A5X powered by NVIDIA Vera Rubin NVL72 rack-scale systems, which — through extreme codesign across chips, systems and software — deliver up to 10x lower inference cost per token and 10x higher token throughput per megawatt than the prior generation.

A5X will use NVIDIA ConnectX-9 SuperNICs, combined with next-generation Google Virgo networking, scaling to up to 80,000 NVIDIA Rubin GPUs within a single site cluster and up to 960,000 NVIDIA Rubin GPUs in a multisite cluster, enabling customers to run their largest AI workloads on NVIDIA‑optimized infrastructure.

“At Google Cloud, we believe the next decade of AI will be shaped by customers’ ability to run their most demanding workloads on a truly integrated, AI‑optimized infrastructure stack,” said Mark Lohmeyer, vice president and general manager of AI and computing infrastructure at Google Cloud. “By combining Google Cloud’s scalable infrastructure and managed AI services with NVIDIA’s industry‑leading platforms, systems and software, we’re giving customers flexibility to train, tune and serve everything from frontier and open models to agentic and physical AI workloads — while optimizing for performance, cost and sustainability.”

Google Cloud’s broad NVIDIA Blackwell portfolio ranges from A4 VMs with NVIDIA HGX B200 systems to rack-scale A4X VMs with NVIDIA GB200 NVL72 and A4X Max NVIDIA GB300 NVL72 systems, all the way to fractional G4 VMs with NVIDIA RTX PRO 6000 Blackwell Server Edition GPUs.

Customers can right-size their acceleration capabilities, whether using multiple interconnected NVL72 racks that scale out to tens of thousands of NVIDIA Blackwell GPUs, a single rack that can scale up to 72 Blackwell GPUs with fifth-generation NVIDIA NVLink and NVLink 5 Switch, or just one-eighth of a GPU.

This comprehensive platform helps teams optimize every workload, from mixture-of-experts reasoning, multimodal inference and data processing to complex simulations for the next frontier of physical AI and robotics.

Leading frontier AI labs are already putting this infrastructure to work. Thinking Machines Lab is scaling its Tinker application programming interface (API) on A4X Max VMs with GB300 NVL72 systems to accelerate training, while OpenAI is running large‑scale inference on NVIDIA GB300 (A4X Max VMs) and GB200 NVL72 systems (A4X VMs) on Google Cloud for some of its most demanding inference workloads, including for ChatGPT.

## **Secure AI Wherever It Needs to Run: Sovereign and Confidential**

Google Gemini models running on NVIDIA Blackwell and Blackwell Ultra GPUs are now in preview on Google Distributed Cloud, so customers can bring Google’s frontier models wherever their most sensitive data resides.

NVIDIA Confidential Computing with the NVIDIA Blackwell platform enables Gemini models to run in a protected environment where prompts and fine‑tuning data stay encrypted and can’t be seen or altered by unauthorized parties, including the infrastructure operators.

In the public cloud, the preview of Confidential G4 VMs with NVIDIA RTX PRO 6000 Blackwell GPUs brings these protections to multi‑tenant environments — helping safeguard prompts, AI models and data so customers in regulated industries can access the power of AI without compromising on security or performance.

This is the first confidential computing offering of NVIDIA Blackwell GPUs in the cloud, giving Google Cloud customers a new foundation for secure, high‑performance AI.

## **Open Models and APIs for Agentic AI**

The NVIDIA platform on Google Cloud is optimized to run every kind of model — from Google’s frontier Gemini and Gemma families to NVIDIA Nemotron open models and the broader open weight ecosystem — equipping developers to build agentic AI systems that reason, plan and act.

NVIDIA Nemotron 3 Super is available on Gemini Enterprise Agent Platform, giving developers a direct path to discovering, customizing and deploying NVIDIA‑optimized reasoning and multimodal models for agentic workflows.

Google Cloud and NVIDIA are also making it easier to train and customize open models at scale. Managed Training Clusters on Gemini Enterprise Agent Platform introduced a new managed reinforcement learning (RL) API built with NVIDIA NeMo RL for accelerating RL training at scale while automating cluster sizing, failure recovery and job execution, so teams can focus on agent behavior and model quality instead of infrastructure management.

Cybersecurity leader CrowdStrike uses NVIDIA NeMo open libraries such as NeMo Data Designer, NeMo Automodel and NeMo Megatron Bridge to generate synthetic data and fine-tuning Nemotron and other open large language models for domain-specific cybersecurity. Running on Managed Training Clusters on Gemini Enterprise Agent Platform with NVIDIA Blackwell GPUs, these capabilities accelerate threat detection, investigation and response.

## **Building the Future of Industrial and Physical AI**

Building industrial and physical AI at scale demands powerful hardware and a combination of open models, libraries and frameworks to develop these complex end-to-end workflows.

NVIDIA AI infrastructure, open models and physical AI libraries available on Google Cloud, is mainstreaming industrial and physical AI applications, enabling customers to simulate, optimize and automate real-world workflows.

Solutions from leading industrial software providers, including Cadence and Siemens Digital Industries Software, are now available on Google Cloud, accelerated on NVIDIA AI infrastructure. These applications are powering the next-generation design, engineering and manufacturing of everything from chips to autonomous vehicles, robotics, aerospace platforms, heavy machinery and large-scale production systems.

With NVIDIA Omniverse libraries and the open source NVIDIA Isaac Sim robotics simulation framework available on Google Cloud Marketplace, developers can build physically accurate digital twins and develop custom robotics simulations pipelines to train, simulate and validate robots before real-world deployment.

NVIDIA NIM microservices for models like NVIDIA Cosmos Reason 2 can be deployed to Google Vertex AI and Google Kubernetes Engine. This enables robots and vision AI agents to see, reason and act in the physical world like humans, powering use cases such as automated data curation and annotation, advanced robot planning and reasoning, and intelligent video analytics agents for real-time insights and decision-making.

Together, these technologies help developers seamlessly move from computer-aided design to living industrial digital twins and AI‑driven robots, accelerating processes from design sign‑off to factory optimization on the NVIDIA platform running on Google Cloud.

## **Proven Impact: From Startups to Global Enterprises**

Global enterprises, AI labs and high‑growth startups are using NVIDIA and Google Cloud’s co-engineered platform to move from prototyping to production faster, including Snap, Schrödinger and Salesforce. Snap is cutting the cost of large‑scale A/B testing by shifting data pipelines to GPU‑accelerated Spark on Google Cloud. Schrödinger is shrinking weekslong drug discovery simulations into just hours with NVIDIA accelerated computing on Google Cloud.

Startups are orchestrating the next wave of AI innovation — building new agents and AI‑native applications using NVIDIA accelerated computing on Google Cloud.

As part of a broader ecosystem highlighted through NVIDIA Inception and Google for Startups, CodeRabbit and Factory are using NVIDIA Nemotron‑based models on Google Cloud to power code review and autonomous software development agents, while Aible, Mantis AI, Photoroom and Baseten are building enterprise data, video intelligence, generative imagery and managed inference solutions on the full‑stack NVIDIA platform on Google Cloud.

More than 90,000 developers have become a part of the joint NVIDIA and Google Cloud developer community in just over a year, tapping this platform to build and scale new AI applications.

In addition, NVIDIA has been honored at Next as Google Cloud Partner of the Year in two categories — AI Global Technology Partner and Infra Modernization Compute — in recognition of deep technical expertise and go-to-market alignment.

Together, NVIDIA and Google Cloud are giving customers a cloud‑scale platform to turn experimental agents and simulations into production systems that review code, secure fleets, enable new AI applications and optimize factories in the real world.

*Learn more about the companies’ collaboration by attending* *NVIDIA sessions, demos and workshops* *at Google Cloud Next.*
