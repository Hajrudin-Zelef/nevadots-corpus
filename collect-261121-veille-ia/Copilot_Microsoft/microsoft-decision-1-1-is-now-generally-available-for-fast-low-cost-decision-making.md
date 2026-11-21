---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-decision-1-1-is-now-generally-available-for-fast-low-cost-decision-making
title: "Microsoft-Decision-1.1 is now generally available for fast, low-cost decision making"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["decision", "microsoft-decision-1", "software", "model", "available", "making", "intelligence", "models"]
source: https://commandline.microsoft.com/microsoft-decision-model-ga/
source_anchor: ""
source_lines: [1, 68]
section: "Copilot_Microsoft"
---
# Microsoft-Decision-1.1 is now generally available for fast, low-cost decision making

          Microsoft-Decision-1.1 is now generally available in Microsoft Foundry and through OpenRouter and Vercel. The model is powering experiences across Microsoft and has been used by more than 4,000 customers since launching in public preview last month.

          
      
   
  
    
As software developers, we’ve always wanted to embed intelligence inside our software, applications, and workflows, rather than anticipate all scenarios and hardcode every decision. LLMs have shown they have the general intelligence to enable this new software architecture, but three challenges stand in the way of deploying it to software at scale: cost, latency, and control.

It’s well-known that cheaper, faster models can be integrated in more places in software. But *control* is equally important. To integrate AI more broadly in software, especially at enterprise scale, we must be able to rely on the decisions a model makes.

Generative models are increasingly intelligent, but the way they’re trained works against the control and reliability that software needs. They’re rewarded for completing tasks, providing answers, and reaching goals, so they tend to make assumptions, sound confident, and take action, even when doing so is wrong or risky.

What the industry needs, and is excited about, is a new kind of model designed and trained from first principles to make more reliable decisions: a decision model.

We recently released Microsoft-Decision-1 as our first answer to this challenge. Microsoft’s decision models are designed to make calibrated decisions in the face of uncertainty across a wide range of real-world software use cases. They choose among permitted actions, within constraints set by the developer, and provide calibrated confidence scores so applications can defer when uncertainty is high. And because they specialize in decision-making, they’re significantly faster and less expensive than general-purpose LLMs for decision-making tasks.

Today we’re **announcing the general availability of Microsoft-Decision-1.1**, with greater intelligence, better calibration, and more than twice the speed of our first model, all at a low cost. It’s designed to make structured decisions in software at scale, for tasks like routing, classification, prioritization, verification, and workflow control. And we’ve added multimodal support, so the model can now accept images, such as photos and screenshots, as part of a request. **Microsoft-Decision-1.1 is ready for enterprise production deployment and is available in Microsoft Foundry and through OpenRouter and Vercel.**

## How we built Microsoft-Decision-1.1

Making intelligence foundational to software requires a model that’s smart at making decisions, fast enough to keep applications responsive, and affordable enough to embed everywhere. Achieving all three together is the main challenge of building a decision model and requires joint innovation across modeling and infrastructure. Here’s how we approached this:

- **Latency:** Decision models need to operate in real time, even when processing complex requests with long inputs and hundreds of questions. Unlike traditional LLM inference engines optimized for generating text token by token, Microsoft-Decision-1.1 uses a custom inference engine optimized for the computational demands of decision models.
- **Cost:** Our custom inference engine optimizes for low cost, not just low latency, by efficiently routing requests across our fleet of GPUs. We also based on a mid-sized mixture-of-experts model, Gemma 4 26B A4B, which has enough active parameters to deliver the intelligence needed for decisions, while remaining efficient enough to run across a flexible range of hardware (A100, H100/200, GB200/300).
- **Intelligence:** Decision models need to stay small enough to run everywhere, generalize well to new or unseen tasks, and be trained specifically to make decisions. To get there, we combined supervised fine-tuning and reinforcement learning with hundreds of synthetic and licensed datasets spanning tens of thousands types of tasks from a wide range of industries to teach the model to make nuanced judgments rather than memorize answers, discriminate between closely related choices, recognize uncertainty, and know when to answer vs. abstain.
- **Calibration:** We also trained the model to know what it doesn’t know: to give consistent answers across similar scenarios, accurately report its level of confidence in its answers, split confidence when more than one answer is valid, and abstain when information is incomplete. This allows developers to set their own thresholds for when to act, flag, or escalate.

## Benchmarks

On the Artificial Analysis Decision Index v2.1, which scores model quality, calibration, and latency, Microsoft-Decision-1.1 scores 68, up six points from our first model and ahead of Jev 1.14, Google’s Gemini Decision 1, and OpenAI’s GPT-6 Luna (Decisions). Microsoft-Decision-1.1 is more than twice as fast as our first model, at the same cost.

We’ve also done our own benchmarking. It’s easy to overfit a model to a narrow subset of decision tasks if benchmarks aren’t comprehensive. We need to know whether our model’s quality generalizes to tasks across a wide range of use cases, including those that lack public benchmarks. We evaluated Microsoft-Decision-1.1 on 102 benchmarks spanning more than 385,000 questions kept blind from training. Tasks span routing, ranking, long context, multilingual and out-of-distribution tasks, reasoning, safety, and more.

In our benchmarking, Microsoft-Decision-1.1 achieved the highest average accuracy of eight decision models, leading in seven of nine capability categories.

## Customer adoption

In just over five weeks since launching, more than 4,000 customers have used the model for tasks ranging from model routing and customer support triage to business process automation, tagging, and classification. Early adopters include Epic, Equinor, Atos Group, and Raiffeisen Bank International. The breadth of industries and use cases we’ve seen in such a short time underscores the potential for fast, cost-effective decision models to transform a wide range of applications. We’re excited to see what customers build next.

## How we’re using the model across Microsoft

We’re embedding Microsoft-Decision-1.1’s specialized intelligence into products across Microsoft to make fast, efficient decisions while reserving larger models for more complex tasks. A few examples:

- In Copilot, Microsoft-Decision-1.1 routes user requests to the right experience, determining whether they’re best handled by Copilot or Cowork. It makes that call as accurately as a large language model, 2.2x faster and at 4.4x lower cost.
- In Cowork, it powers Auto, dynamically adjusting how much reasoning effort Cowork applies to each task.
- Microsoft-Decision-1.1 is now available to be used as a judge in Foundry Evaluations to measure performance, quality, and safety.
- Our on-call engineers use it to retrieve relevant knowledge to respond to live incidents across logs, ticketing systems, calls, messages, and other data sources.

## Try it yourself

You can try this interactive mock demo, which lets you integrate Microsoft-Decision-1.1 into an agent’s architecture, including as a model router, scheduler, and content filter.

## Pricing

In Microsoft Foundry, input tokens:

- Global Standard: $0.042 per million tokens
- US Data Zone: $0.046 per million tokens
- EU and APAC Data Zone: $0.05 per million tokens

Output tokens are free.

## Getting started

You can get started with Microsoft-Decision-1.1 today:
