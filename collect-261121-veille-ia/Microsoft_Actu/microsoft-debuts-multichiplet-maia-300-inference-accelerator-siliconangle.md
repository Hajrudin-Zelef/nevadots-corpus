---
id: collect-261121-veille-ia/microsoft-actu/microsoft-debuts-multichiplet-maia-300-inference-accelerator-siliconangle
title: "Microsoft debuts multichiplet Maia 300 inference accelerator - SiliconANGLE"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["maia", "microsoft", "data", "accelerator", "inference", "chip", "computing", "memory"]
source: https://siliconangle.com/2026/10/12/microsoft-debuts-multichiplet-maia-300-inference-accelerator/
source_anchor: ""
source_lines: [1, 41]
section: "Microsoft_Actu"
---
### Microsoft debuts multichiplet Maia 300 inference accelerator

Microsoft Corp. today pulled back the curtains on Maia 300, its newest internally developed artificial intelligence accelerator.

The chip is optimized for inference, the process of generating responses to AI queries. Microsoft estimates that it can process 40% more tokens per megawatt than “leading GPU-based systems,” which is likely a reference to Nvidia Corp. products. The company said the Maia chip series will become its primary source of inference capacity over time.

Maia 300 features four computing chiplets made using Taiwan Semiconductor Manufacturing Co.’s latest two-nanometer node. The chiplets are flanked by eight HBM4 modules with 288 gigabytes of combined capacity, the same amount as Nvidia Corp.’s flagship Rubin accelerator. However, Maia 300 has about 20% less memory bandwidth than Rubin. Memory bandwidth is a measure of how much fast data can be transferred to and from RAM.

AI chips usually perform inference by repeatedly shifting data between their computing and memory circuits. That constant movement slows down processing. According to Microsoft, Maia 300 features a dataflow architecture designed to minimize data movement. When possible, the computing chiplets keep information in an internal SRAM cache instead of sending it to the HBM stacks.

In situations where using HBM can’t be avoided, Maia 300’s computing chiplets send data to a memory stack that is physically nearby. That’s faster than transmitting information to an HBM stack on the other end of the chip. For added measure, Maia 300 compresses data into lightweight formats such as MXFP4. Those formats’ reduced size enables data to move faster inside the chip.

“Narrower representations reduce memory footprint and data movement, allowing the system to support larger models, longer contexts, and more concurrent sessions within the same power envelope,” Microsoft executives Rani Borkar and Saurabh Dighe wrote in a blog post.

Maia 300’s computing and memory modules are supported by what Microsoft calls a device control chiplet. It manages the flow of power to the processor’s core components, collects telemetry and performs other supporting tasks. There are also two other auxiliary chiplets that manage the movement of data in and out of the accelerator.

Microsoft plans to deploy Maia 300 in trays that will feature four accelerators apiece. Those trays, in turn, will be placed in liquid-cooled racks with a total of 96 accelerators. Microsoft expects the racks to provide about seven times the token throughput of a system equipped with its previous-generation Maia 200 chip. The company debuted the latter accelerator in January and already uses it in production.

Microsoft intends to group its Maia 300 racks into so-called superpods with a total of 12,288 accelerators. The individual chips will be linked together using a custom Ethernet-based network stack. The network can adjust how much bandwidth is allocated to in-rack traffic and how much is available for inter-rack communications, which enables it to tailor its configuration to each workload. The result is increased performance.

Usually, chips exchange data differently depending on whether they’re in the same rack or in different racks. As a result, switching between the two networking modes requires them to use the host server’s central processing unit or a standalone network chip. Maia 300, in contrast, uses the “same communication semantics” for both tasks. That removes the need for the extra hardware and thereby makes clusters more efficient.

“Jobs can use capacity across rack boundaries, reducing stranded accelerators when workload sizes do not align with the physical rack configuration,” Borkar and Dighe explained.

Microsoft plans to start deploying Maia 300 in production next year. The company expects its custom silicon to power multiple gigabytes’ worth of AI computing capacity.

##### Image: Microsoft

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
