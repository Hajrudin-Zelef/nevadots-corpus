---
id: collect-261121-veille-ia/qwen/alibaba-launches-qwen4-max-with-training-attention-optimizations-siliconangle
title: "Alibaba launches Qwen4-Max with training, attention optimizations - SiliconANGLE"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-16"]
keywords: ["qwen4-max", "training", "siliconangle", "alibaba", "model", "attention", "launches", "optimizations"]
source: https://siliconangle.com/2026/11/16/alibaba-launches-qwen4-max-with-training-attention-optimizations/
source_anchor: ""
source_lines: [1, 37]
section: "Qwen"
---
### Alibaba launches Qwen4-Max with training, attention optimizations

Alibaba Group Holding Ltd. today introduced Qwen4-Max, its newest and most capable large language model.

The algorithm not only generates higher-quality output than its predecessor but also costs less. Those improvements are the result of a new architecture, Qwen4, that made the LLM’s training run more efficient. Additionally, Alibaba has equipped the model with an upgraded attention mechanism.

Qwen4-Max’s backbone, the part of an LLM that functions as its main processing engine, comprises 2.4 trillion parameters. The model activates only 71 billion at any given time. The backbone is supported by 328 billion so-called N-gram parameters, which function as a kind of cache. They save the results of oft-repeated calculations and thereby remove the need to compute them from scratch every time.

An LLM’s artificial neurons are organized into sets known as layers. When the model learns something new during training, a piece of data called a gradient travels through the layers and refines their output. The gradient moves through them one by one starting from the last layer, which finalizes and displays prompt responses.

Qwen4-Max includes a feature that enables the gradient to skip some layers. That feature, or residual, is effectively a shortcut inside the LLM. Its purpose is to reduce errors during the training workflow, which in turn lowers costs and improves output quality.

Residuals can also be found in many other LLMs. Historically, the mechanism was implemented as a single gradient transportation channel. Qwen4-Max’s implementation of the technology features four channels. Moreover, those channels have a “gate” that can adjust the movement of gradients to further improve the training process.

Qwen4-Max’s attention mechanism, a module that helps it understand user prompts, features two key customizations. The first is a technology called Gated DeltaNet that lowers the amount of hardware needed to process large prompts. The second optimization, QSA, skips pieces of data that the LLM doesn’t need to answer a prompt. That arrangement further lowers infrastructure costs.

Alibaba compared Qwen4-Max with its predecessor, Qwen3.8-Max, using an internal benchmark that includes knowledge work tasks. The former model achieved a higher score and used up to 35% fewer tokens to complete tasks. Those savings grew to 49% when the models were tested using the Terminal-Bench 4.0 suite of coding challenges.

Qwen4-Max is available through Alibaba’s cloud platform. The company plans to open-source a collection of smaller models based on the same technologies further down the line. It’s also open-sourcing Qwen-Live-Delivery-Plugin, an extension that makes Qwen4-Max better at performing time-consuming tasks.

When an LLM is assigned a long-horizon project, users often have to wait until the model completes its work before they can review the results. Alibaba’s plugin shortens the wait. It enables Qwen4-Max to quickly deliver an initial draft and then iteratively add improvements. Users can give the model feedback along the way to refine its work.

##### Image: Unsplash

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
