---
id: collect-261121-veille-ia/modeles-open/reflection-ai-debuts-open-source-beam-model-with-501b-parameters-siliconangle
title: "Reflection AI debuts open-source Beam model with 501B parameters - SiliconANGLE"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["beam", "reflection", "model", "open-source", "siliconangle", "parameters", "billion", "debuts"]
source: https://siliconangle.com/2026/10/05/reflection-ai-debuts-open-source-beam-model-with-501b-parameters/
source_anchor: ""
source_lines: [1, 37]
section: "Modeles_Open"
---
### Reflection AI debuts open-source Beam model with 501B parameters

Reflection AI Inc. today introduced Beam, an open-source large language model with 501 billion parameters.

The launch comes a few months after the startup raised funding at a $25 billion valuation. Around the same time, Reflection AI reportedly inked a $6.3 billion deal with SpaceX Corp. to rent Nvidia GB300 NVL72 appliances. It used those systems, which each contain 72 graphics cards, to train Beam.

Reflection AI benchmarked its model against GLM-5.2, an open-source LLM that includes about 250 billion more parameters. The company determined that Beam can perform some tasks better while using between one third and one fourth the hardware. Furthermore, it says that the LLM approaches the performance of Qwen 3.8-Max, a model with over 2 trillion parameters.

The launch is particularly notable because many of the open-source ecosystem’s most advanced LLMs were created by Chinese companies. That includes Qwen 3.8-Max and GLM-5.2. Beam is the first open-source model from a U.S. startup to have demonstrated comparable or better performance. However, free LLMs continue to trail frontier models such as Anthropic PBC’s Claude Fable 5.1.

Reflection AI kicked off the development of Beam by training a relatively small prototype model. It then created a series of successively larger, more capable algorithms. The workflow eventually produced Beam Base, the foundation on which Beam is based.

The company developed Beam Base using a cluster of 6,144 graphics cards. It trained the model on 23.8 trillion tokens sourced from the public web and commercial sources. That dataset included a significant amount of software code. It created custom filters for each programming language to remove low-quality files.

Beam Base was developed in under four weeks. It subsequently performed midtraining, an optimization process that extended the model’s context window and enhanced its reasoning capabilities. That phase of the project set the stage for the third, most hardware-intensive phase of the training workflow.

Reflection AI spun up 10,000 GB300 graphics cards and used them to launch 1.3 billion reinforcement learning sandboxes. Those are virtual environments in which an AI model learns new skills. Beam’s sandboxes were optimized for tasks such as generating code, searching the web and running AI agents.

The company says the project’s reinforcement learning phase took only four weeks. It avoided unnecessary delays by developing software that prevented malfunctions from interrupting the workflow. It says its cluster achieved a median recovery time of eight minutes across the 71 errors that cropped up during training.

Beam is initially available through an early access program. Reflection AI plans to release its weights, documentation and fine-tuning tools later this month.

##### Photo of Reflection AI founders Misha Laskin and Ioannis Antonoglou: Lightspeed

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
