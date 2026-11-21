---
id: collect-261121-veille-ia/microsoft-actu/microsoft-unveils-2-8-trillion-parameter-ai-model-claims-major-efficiency-gains
title: "Microsoft Unveils 2.8 Trillion-Parameter AI Model, Claims Major Efficiency Gains"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "training", "company", "mai-base-2", "microsoftas", "more", "data"]
source: https://www.techrepublic.com/article/news-microsoft-mai-base-2-ai-model-efficiency/
source_anchor: ""
source_lines: [1, 42]
section: "Microsoft_Actu"
---
Microsoft is building a much larger AI model of its own, but the company says its latest breakthrough is about using computing power more efficiently.

The company on Oct. 15 announced that it has completed pre-training MAI-Base-2, a mixture-of-experts (MoE) foundation model with 2.8 trillion total parameters and 80 billion active parameters. The model succeeds MAI-Base-1, which Microsoft introduced earlier this year.

According to Microsoftâs technical announcement, MAI-Base-2 outperformed the open-weight models included in its internal evaluations. The company estimates its training approach is 4.2 to 12.3 times more compute-efficient than DeepSeek-V4-Pro-Base when matching performance on selected tests.

Microsoft trained the model using 16,000 Nvidia GB200 GPUs over 38 days in August and September. Its training run processed 50 trillion tokens, including text and visual data.

MAI-Base-2 activates only a fraction of its parameters for each input, allowing it to retain a much larger overall model without using every parameter for every operation.

Microsoft also changed its training methods, expanded its unique training data by about 36% compared with MAI-Base-1 and introduced native vision training. Images accounted for 18% of the new modelâs training mixture, supporting tasks such as chart interpretation, document analysis and spatial reasoning.

The company attributes its efficiency gains to improvements in data quality, model architecture and infrastructure. It also says the model learned from organic training data rather than distilling knowledge from other AI models.

However, the results need context. Microsoftâs comparisons rely on selected held-out evaluations, and the company has not published equivalent comparisons against every leading proprietary model. Its efficiency figures estimate the compute needed to reach comparable performance, rather than showing that MAI-Base-2 is universally superior.

## Microsoft wants control beyond the model

The announcement also advances Microsoftâs effort to build more of its own AI technology as its relationship with OpenAI evolves.

Business Insider reported that Microsoft AI CEO Mustafa Suleyman told employees the company had privately compared MAI-Base-2 with OpenAI models. Microsoft has not publicly released those results for independent assessment.

Microsoftâs commercial strategy is clearer: It plans to license MAI-Base-2âs model weights to trusted partners through Microsoft Foundry rather than release them publicly. The company says control over its training data and infrastructure will help it offer enterprise customers a more predictable route to deploying customized AI.

That approach could appeal to organizations that want to adapt a foundation model to internal documents, workflows and business requirements without developing one from scratch. However, licensing model weights is not the same as making them freely available. Pricing, access conditions and customization costs will influence whether companies choose Microsoftâs offering over genuinely open-weight alternatives.

### More Microsoft news

- Inside Microsoftâs Real-Time War Against Cybersecurity Threats
- Project Ire: Microsoft Tests AI That Autonomously Detects Malware
- Microsoft Targets âCritical AI Talentâ from Meta to Dominate Next AI Breakthroughs
- Windows 10 Support Ends Soon, Though Extended Security Updates Offers Are Available

## What businesses and users can expect

MAI-Base-2âs text and vision capabilities could eventually support enterprise applications involving documents, spreadsheets, charts and other visual information. However, Microsoft has not yet demonstrated how those capabilities will translate into commercial products.

The model is still undergoing post-training, and Microsoft has not announced general availability or pricing. Businesses should therefore not expect immediate access to MAI-Base-2 through Copilot or other Microsoft services.

Microsoft says preparations for MAI-Base-3 are already underway. For enterprise customers, the more important question is whether Microsoftâs growing investment in its own AI models will eventually deliver better performance, competitive pricing, and more deployment options.

**Other news:** **Nvidia has introduced three Nemotron 3.5 AI models** **designed to support autonomous agents across enterprise servers and local devices.**
