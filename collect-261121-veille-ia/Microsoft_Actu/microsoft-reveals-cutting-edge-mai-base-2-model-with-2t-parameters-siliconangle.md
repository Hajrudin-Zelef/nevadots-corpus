---
id: collect-261121-veille-ia/microsoft-actu/microsoft-reveals-cutting-edge-mai-base-2-model-with-2t-parameters-siliconangle
title: "Microsoft reveals cutting-edge MAI-Base-2 model with 2T parameters - SiliconANGLE"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-16"]
keywords: ["mai-base-2", "model", "microsoft", "siliconangle", "more", "parameters", "through", "training"]
source: https://siliconangle.com/2026/10/16/microsoft-reveals-cutting-edge-mai-base-2-model-with-2t-parameters/
source_anchor: ""
source_lines: [1, 37]
section: "Microsoft_Actu"
---
### Microsoft reveals cutting-edge MAI-Base-2 model with 2T parameters

Microsoft Corp. has developed a new large language model that it says is significantly more efficient than leading open-source alternatives.

The company’s artificial intelligence research group detailed the algorithm on Thursday.

MAI-Base-2 is not a full-fledged LLM but rather a base model, a kind of no-frills prototype. Developers turn base models into LLMs by putting them through several additional rounds of training. That extra training usually focuses on adding reasoning features and safety guardrails.

The most capable open-source base model on the market today is DeepSeek-V4-Pro-Base. Microsoft says that MAI-Base-2 outperformed it on a series of internal benchmarks that contained science, programming and math questions. Furthermore, MAI-Base-2 is up to 12 times more hardware-efficient by certain measures.

MAI-Base-2 features a mixture of experts, or MoE, architecture, which means that it only uses a subset of its artificial neurons to process prompts. That’s more efficient than activating the entire neural network. MAI-Base-2 has 2 trillion parameters and activates 80 billion of them when it receives a prompt.

A language model’s artificial neurons are organized into clusters called layers. Microsoft’s previous-generation base model mixed MoE layers with modules based on a less efficient design called FFN. MAI-Base-2 is made up entirely of MoE layers, which is one of the contributors to its performance.

Microsoft trained the model for 38 days using 16,000 of Nvidia Corp.’s GB200 chips. The training dataset used in the project comprised 50 trillion tokens. About a fifth of the tokens were used to train a computer vision module integrated into MAI-Base-2. The module makes the model better than its predecessor at tasks such as understanding scientific charts.

Developers train LLMs by observing the manner in which they complete tasks and providing feedback on how they can improve. That feedback, which is generated algorithmically, takes the form of a mathematical object called a gradient. The gradient travels through the layers of the model being trained and re-calibrates each one to boost its output quality.

Gradients change every time they pass through an LLM layer. Under certain circumstances, those changes cause bugs that can disrupt the training workflow. Microsoft employed a method called gradient clipping to prevent that issue from affecting MAI-Base-2. It also used Muon, a popular open-source AI tool, to speed up the training workflow.

Microsoft will license MAI-Base-2’s weights to “trusted partners” through its Azure cloud platform. Additionally, it will use the model to power a new series of LLMs. The company plans to share more information about those models in the near future.

##### Photo: Microsoft

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
