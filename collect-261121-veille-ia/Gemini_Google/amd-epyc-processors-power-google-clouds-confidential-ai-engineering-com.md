---
id: collect-261121-veille-ia/gemini-google/amd-epyc-processors-power-google-clouds-confidential-ai-engineering-com
title: "AMD EPYC processors power Google Cloud’s confidential AI - Engineering.com"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["epyc", "confidential", "processors", "google", "cloud", "data", "workloads", "host"]
source: https://www.engineering.com/amd-epyc-processors-power-google-clouds-confidential-ai/
source_anchor: ""
source_lines: [1, 27]
section: "Gemini_Google"
---
The collaboration brings hardware-enforced data protection to TPU 8tx workloads, including AI inference, fine-tuning and reinforcement learning.

AMD announced that 5th Gen AMD EPYC processors are powering the host infrastructure for Google Cloud TPU 8tx, the first cloud Tensor Processing Unit (TPU) system to deliver hardware-enforced confidential computing. The expanded collaboration between the companies combines AMD EPYC CPU performance, AMD Infinity Guard security capabilities and x86 software compatibility with Google Cloud TPU AI acceleration to help enterprises deploy inference, fine-tuning and reinforcement learning workloads.<sup>1</sup> With Google Cloud’s confidential computing architecture on TPU 8tx supported by 5th Gen AMD EPYC host processors, organizations can run their most sensitive AI workloads at scale in the cloud without compromising on data protection.

The deployment extends the role of AMD EPYC processors as a foundation for accelerated AI infrastructure. Across systems built with GPUs and custom accelerators, AMD EPYC CPUs are the choice for host processing – preparing data, coordinating workloads and managing communication with accelerators. With TPU 8tx, AMD and Google Cloud bring those capabilities together with hardware-rooted security to address a critical requirement for production AI: protecting data while it is in use.

### **Enabling confidential AI with AMD EPYC processors**

In TPU 8tx systems, 5th Gen AMD EPYC processors manage workload orchestration, data preparation and high-throughput communication with TPU accelerators. The system utilizes the AMD Secure Encrypted Virtualization (SEV) capabilities in the AMD EPYC CPU, which together with the TPU, extends confidential computing protection beyond the CPU to connected accelerators, helping protect sensitive data as it moves among them.

Verifying an accelerator’s identity and security configuration before granting it direct access to protected memory helps customer prompts, model weights and intermediate data remain encrypted in CPU memory and in transit among the host and accelerators, with hardware isolation during accelerator execution. These protections help enterprises use sensitive information with confidence in reasoning and agentic AI applications.

AMD EPYC processors also underpin the broader Google Cloud confidential computing portfolio: AMD SEV technologies help protect workloads across Confidential VMs, Confidential GKE, Confidential Space, Confidential Managed Service for Apache Spark, and Confidential Dataflow. TPU 8tx extends that collaboration to confidential AI acceleration.

### **A proven host CPU Foundation across AI architectures**

As hyperscalers, frontier labs and model providers build increasingly specialized AI infrastructure, CPU selection remains central to system performance and deployment. Accelerators depend on host processors to supply data, schedule work and connect AI workloads with the applications and services around them. AMD EPYC processors are the host CPU of choice for more than 10 leading hyperscalers and frontier AI labs, spanning GPUs and custom AI accelerator ASICs.

AMD EPYC processors support this role across systems using AMD Instinct GPUs, Nvidia GPUs and custom silicon. This breadth gives customers a proven CPU platform across accelerator choices, supported by the established x86 software ecosystem.

### **Building on generations of AMD and Google Cloud collaborations**

AMD and Google Cloud have collaborated across multiple generations of EPYC processors, supporting general-purpose, compute-intensive, storage-intensive and accelerated workloads leading up to current Google Cloud Compute Engine C4D, N4D, H4D, Z4D and G4 virtual machines. The companies have also worked together for years on security technology at the corporate level, including deep reviews of AMD security processor firmware by Google security experts.

With TPU 8tx, this expanded collaboration helps enterprises run modern reasoning and agentic AI workloads in production without compromising on data protection.

For more information, visit amd.com.
