---
id: collect-261121-veille-ia/microsoft-actu/from-model-choice-to-your-durable-advantage-build-ai-that-keeps-improving-microsoft-community-hub
title: "From model choice to your durable advantage: Build AI that keeps improving | Microsoft Community Hub"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["models", "model", "foundry", "microsoft", "more", "data", "teams", "cost"]
source: https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/from-model-choice-to-your-durable-advantage-build-ai-that-keeps-improving/4573609
source_anchor: ""
source_lines: [1, 85]
section: "Microsoft_Actu"
---
Every organization now has access to the same frontier models. What sets your business apart is how you put those models to work with your data, your expertise, and your definition of success. In our conversations with customers, three priorities come up again and again: flexibility to choose the right model for each task, control to turn their own data, expertise and feedback into durable advantage, and confidence in the models and platform they build on. Their expertise should strengthen their business, not deepen their dependence on any one AI lab.

With more than 11,000 models from Microsoft, OpenAI, Anthropic, Meta, Mistral, the open-source community and others, **Microsoft Foundry** **gives organizations the breadth to match models to their needs.** Today at Microsoft Ignite, we’re announcing updates across Foundry Models that help teams make that choice pay off: new models from Microsoft AI (MAI) and partners, intelligent model routing, more ways to run open models, and new training options that turn your data and feedback into models built for your business.

## Make it fit: Start with what good looks like

The leaderboard is a starting point, but what matters is how a model performs on your workload. In Foundry, you define success first, including quality, latency, cost per task and data residency, then evaluate candidate models against your own data with built-in and custom evaluators. The model that clears your success bar wins, not the one at the top of a benchmark.

Different jobs call for a different balance of capability, speed and cost. Today’s new models give you more options to match each one.

#### *Coding and agentic software development*

Developers need AI that can take on their hardest engineering work while keeping everyday coding fast and cost-effective. **MAI-Code-2** is built for demanding agentic coding work, such as multi-step tasks, complex refactors and agent-driven development. **MAI-Code-2-Flash** is tuned for high-volume everyday coding. Developers can choose the balance of performance and cost that fits each task instead of using one model for everything. Try both today in GitHub, and access Code-2-Flash through a limited access preview in Foundry.

#### *High-volume decision-making*

Teams need to make high-volume, structured decisions quickly and cost-effectively to keep AI applications moving. Alongside complex work such as multi-step coding, these focused tasks call for a different balance of speed, cost and capability, helping teams reserve larger models for the complex reasoning that needs them most.

**Microsoft-Decision-1.1 is now generally** **available**. Built on Gemma, it delivers fast, cost-effective decisions across routing, classification, prioritization and verification, and now supports image inputs. Find it today in the Foundry model catalog.

#### *Creative media generation*

Creative teams need to explore ideas quickly without losing the visual consistency that makes their brand recognizable. **MAI-Image-2.7 and MAI-Image-2.7-Flash** bring sharper editing and more consistent image generation, helping brands keep the same visual identity from the first asset to the last. Both are now in public preview in Foundry – find them in the Foundry model catalog.

Through our new partnership with **fal on Microsoft Foundry**, we’re also bringing Kling v3 Pro, MiniMax H3 Max, MiniMax H3 Max Turbo, LTX-2.5 Pro and LTX-2.5 Fast to Foundry Models, with more to come. Developers get more options for multimodal applications that combine video, images and other AI capabilities. Find all of them in the Foundry model catalog.

#### *Cybersecurity and vulnerability remediation*

Security teams need to move from identifying a potential vulnerability to confirming the risk and fixing the code. They need tools that support that full workflow while keeping the time and cost of each investigation manageable.

**MAI-Cyber-2-Flash** is purpose-built for cybersecurity. Its new capabilities span code vulnerability discovery, validation and remediation. Because it was trained inside the Codename MDASH harness, it helps security teams find, confirm and fix vulnerabilities faster and more cost-effectively. It will be available through Codename MDASH.

#### *Frontier and efficient models from Meta*

Meta’s Muse Wonder, Muse Spark 1.3 and Muse Ember are coming soon to Microsoft Foundry. They give teams frontier, balanced and high-efficiency options to match each step of an agentic workflow, from long-horizon planning and complex coding to fast, cost-efficient execution at scale. Find them in the Foundry model catalog.

# Make it better: Let Foundry choose the model for each request

Not every request needs the most capable model. For teams that would rather focus on building than pick a model for every task, **model router** in Foundry Models matches each prompt to the right model in real time, so simple requests use smaller, faster models and complex ones get frontier reasoning. Today we’re making intelligent selection easier to adopt: 

- **Automatic model selection by default.** In supported Foundry projects, automatic model selection is on by default, so developers can start building without first deploying the model router.
- **More models.** Model router now chooses from 44 models from OpenAI, Anthropic, xAI, DeepSeek, Meta and others.

- **Model router in AI Gateway.** AI Gateway customers can turn on model router to add intelligent model selection to their existing setup.

# Make it scale: Run and improve models on your terms

Choosing the model is just the start. Getting value in production means running models efficiently and improving them over time against your business goals.

#### *More choices for running open models: Baseten joins Microsoft Foundry*

Open models are moving fast, and Foundry is keeping pace. Baseten on Microsoft Foundry, now in public preview, brings Baseten-optimized open models such as Kimi-K3, GLM 5.3, GLM-5.3-Flash and DeepSeek V4.1-Flash-1017 into Foundry as a Microsoft first-party service, tuned for speed, throughput and cost. Start with serverless pay-as-you-go, globally or in the US Data Zone, and scale with provisioned throughput starting in December. Baseten joins Fireworks, already generally available on Foundry, so you can evaluate, deploy and govern open models that fit each workload in the same Foundry workflow you use for agents, all on your existing Azure agreement with Microsoft support.

#### *Train your models in Microsoft Foundry* 

Anyone can call a general-purpose model. Your data and expertise are your edge, and training in Foundry turns them into a model built for your business, with as much control as you want.

With Managed Training (previously managed fine-tuning), bring your data and choose a method such as supervised or reinforcement fine-tuning, and Foundry runs the job. Want to own the loop? Programmed Training, now in public preview, lets you write your training recipe in Python, from sampling to scoring to weight updates, while Foundry runs it on serverless GPUs. Start with a cookbook of ready-made recipes and open-weight models such as gpt-oss-120b and Qwen3.8-27B. There are no GPU clusters to manage, and trained models can be evaluated and deployed in the same Foundry workflow you use for agents.

Customers are already seeing results. Heidi, an AI care partner for clinicians, used Programmed Training to post-train an open model for clinical notes that matches a widely used frontier model on its clinical quality metrics, at an estimated 87% lower cost per note. Docusign cut AI processing costs by 90% with fine-tuned models in Foundry.

#### *Run open and custom models on dedicated infrastructure: Managed Compute is now generally available* 

Some workloads need more than serverless access. Teams running open-weight, partner or custom models often want dedicated capacity, predictable performance and control over how a model is deployed, without managing GPU clusters or building their own MLOps stack. Managed Compute in Microsoft Foundry is now generally available, with Data Zone availability for workloads that need to keep data processing within a geographic boundary. Deploy models on dedicated infrastructure that Foundry manages for you, billed at an hourly GPU rate. You get the control of running open models yourself without the operational overhead. And because Managed Compute is part of Foundry, those models are evaluated, governed and monitored in the same workflow you use for agents and every other model in your system.

Together with serverless options like Baseten and Fireworks and deployment types such as Batch, Standard and Provisioned, Managed Compute gives teams another lever to match each workload to the right operating point for cost, performance and control.

# One continuous improvement loop

Models will keep changing, and they should. The model is no longer the architecture. What lasts is the loop around it: define what good means, continuously climb toward it and stay on target as new models arrive. Every supported model in Foundry, whether from Microsoft, a partner or the open community, can be evaluated, optimized, trained and deployed in that same workflow. And because identity, security and data governance run through Microsoft Entra, Microsoft Defender and Microsoft Purview, you can switch the model and keep the controls.

That’s the idea behind Foundry as your AI app and agent factory: the right model for every task, trained on what makes you different, with the control to keep making it better.

# Get started

- 
Explore the new models in the Foundry model catalog
- 
Experiment with Baseten on Foundry
- 
Learn more about the model router here
- 
Explore how you can train a model here and request preview access to build your first experiment.
- 
Explore related sessions:
