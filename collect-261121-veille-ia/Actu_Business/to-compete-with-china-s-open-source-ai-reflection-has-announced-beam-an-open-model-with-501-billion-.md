---
id: collect-261121-veille-ia/actu-business/to-compete-with-china-s-open-source-ai-reflection-has-announced-beam-an-open-model-with-501-billion-
title: "To compete with China's open-source AI, Reflection has announced 'Beam,' an open model with 501 billion parameters, claiming it is comparable to GLM 5.2, uses one-third to one-fourth the computation, and performs comparably to Qwen 3.8-Max for AI agent tasks."
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "reflection", "open", "model", "parameters", "qwen", "models", "billion"]
source: http://gigazine.net/gsc_news/en/20261006-beam-reflection-501b-open-weight-model/
source_anchor: ""
source_lines: [1, 217]
section: "Actu_Business"
---
# To compete with China's open-source AI, Reflection has announced 'Beam,' an open model with 501 billion parameters, claiming it is comparable to GLM 5.2, uses one-third to one-fourth the computation, and performs comparably to Qwen 3.8-Max for AI agent tasks.


 

**Beam**,' an open model with 501 billion parameters and 23 billion active parameters.

**Introducing Beam: Reflection's 501B open-weight model — Reflection**

**https://reflection.ai/blog/introducing-beam**

**Reflection AI unveils its first model, Beam, an open-source answer to Chinese labs | Semafor**

**https://www.semafor.com/article/10/05/2026/reflection-ai-unveils-an-open-source-answer-to-chinese-labs**

Beam is an open model born from massive investments in pre-training and reinforcement learning. For pre-training, it uses 23.8 trillion diverse, carefully selected, high-quality tokens collected from the web and proprietary licensed datasets. As a result, Reflection explains that it has succeeded in achieving performance equal to or better than other publicly available open models of similar scale.

Simultaneously, we developed the algorithms, learning environment, and infrastructure necessary to sustain reinforcement learning, which requires massive computing resources, on an extremely large scale. For reinforcement learning, we used 10,500

**NVIDIA GB 300s**to generate over 100 million rollouts (trials in which the AI model generates answers or a series of actions) in four weeks of training.

Reflection claims that by combining pre-training and reinforcement learning, they have achieved competitive performance as an open-weight model and state-of-the-art inference computation efficiency. As of the time of writing, Beam is undergoing the final stage of red teaming (safety verification through aggressive testing) and evaluation, so the model is not yet public. However, Reflection has stated that they plan to release the full weights of Beam in October 2026, and you can register for the waiting list below.

**Imageion Playground**

**https://platform.reflection.ai/login**

Beam is trained with a particular emphasis on coding and agent performance. Beam pushes the boundaries of open weight models in Europe and the US, achieving performance comparable to larger open models like

**GLM 5.2**and approaching

**Qwen 3.8-Max**in coding and agent-based tasks. While it still lags behind cutting-edge open models like

**Kimi K3**in pure capability, Beam has the advantage of efficiency during inference.

The image below summarizes the results of comparing Beam with various open models—GLM 5.2, Qwen 3.8-Max, Inkling, and Nemotron Ultra—across multiple benchmarks. The benchmarks used were

**DeepSWE**, a benchmark for AI coding agents;

**Terminal Bench**, which measures the overall practical performance capabilities of AI agents in a terminal environment;

**HLE No Tools**, which evaluates the inherent knowledge and pure thinking ability of AI models without using any external tools such as search or code execution;

**SWE Bench Pro**, which measures the ability of AI coding agents to solve complex problems at the repository level

**; SWE Bench Verified**, which measures how autonomously AI coding agents can perform software development; and

**CritPT AA**, which measures advanced physics reasoning capabilities at the researcher level. In all benchmarks, Beam demonstrated performance equivalent to the open models being compared.


 Furthermore, Beam (black) not only possesses high coding and agent capabilities, but also boasts extremely efficient inference capabilities. In advanced inference benchmarks, it achieves scores comparable to GLM-5.2 (orange) with one-third to one-fourth of the inference computation time. The difference in efficiency is even more pronounced when compared to models with over 2 trillion parameters, such as Qwen 3.8-Max (purple).

 


 Reflection states, 'Because it can deliver higher intelligence per token, Beam can provide high model capabilities at a low cost. It will be a powerful, practical model to support enterprise coding and agent-based tasks.'

 Reflection CEO Misha Raskin explained the reason for releasing Beam as an open model: 'The internet was built on open protocols. The most widely used operating systems are also open. The field of cybersecurity exists because strong encryption protocols were made open in the 1990s. Being open also leads to increased security. The best defense is decentralized. Open-source software that is commonly used is robust and field-tested because it is inspected and patched by the developer community. AI is different from software, but in many ways it is similar, and the surface area of vulnerability is far greater. A small group of security researchers working in secret cannot fix all potential problems. AI is the foundation on both the scientific and technological fronts. Opening it up is the way to distribute the benefits of secure AI most fairly. I look forward to seeing what developers and scientists build and learn from this model. I am pleased that Beam can play a small role in making AI widely accessible, secure, and beneficial to everyone.'

 We are introducing Beam today, a 500b open model that excels in coding, agentic, and scientific workloads.

 Beam is very token-efficient, pre-trained from scratch, and scaled with the largest RL run documented openly we are aware of.

 Getting here was challenging, rewarding, and personal.

 I immigrated to the USA during childhood when my parents got jobs as scientists at PNNL, a national lab located in a rural town in Washington state.

 Growing up, I developed an interest in physics because it was grounded in simple principles, was foundational, and had long-term impact.

 Physics was the foundational science of the 20th century. The first general computer (ENIAC), the transistor, GPS, and much of modern technology had its roots in physics.

 In 2016, during my graduate studies, I noticed the birth of a new foundational science - Artificial Intelligence.

 AlphaGo, the superintelligent Go agent developed by DeepMind, had come out and made me feel what was about to come.

 A neural network, trained to imitate human players, and then improve itself through trial and error, mastered the most intellectually challenging board game in the world.

 What surprised me about AlphaGo is how it was conceptually simple, general, and scalable. The system could in principle improve itself forever, it was just a function of compute.

 This is when I became “AGI pilled” as they say.

 I switched from physics to AI because I thought this would become the root node science of the 21st century, much like physics was the root node science of the 20th century.

 I believe that working on foundational science is not enough on its own. Foundational science must also remain open and widely accessible.

 We remember Newton for his contributions to physics, but he was also an alchemist.

 Alchemy was done in secret, because the artificial production of gold was deemed desirable but also dangerous with the potential do destroy entire economies. Newton even wrote his alchemy notebooks in cypher.

 On the other hand, Principia was published in the Philosophical Transactions of the Royal Society, the knowledge was made widely available, passed down for generations in what we now call Newton's laws.

 The computer, transistor, GPS were possible to build because physics as a science was open. Openness creates an ecosystem of innovation.

 The notion of openness, which is a core tenet of scientific discovery, has also become a guiding principle for how foundational technology gets built and distributed.

 The internet was built on open protocols. The most widely used operating systems are open. The field of cybersecurity exists because strong encryption protocols were made open in the 1990s.

 Open also means safer. The best defense is a distributed one. Commonly used open source software is robust and battle-tested because it is inspected and patched by a community of developers.

 AI is different but also similar to software in many ways, though with a much larger surface area of vulnerabilities. It is impossible for a small group of safety researchers working in secrecy to remediate all of the potential issues regardless of their intentions. It is like entrusting a handful of white blood cells to protect your body.

 AI is both foundational as a science and a technology, and the safest way that also ensures the benefits of AI are distributed evenly is by making it open.

 I'm excited for what developers and scientists build and learn from this model, and for Beam to play a small part in ensuring that AI is made widely accessible, safe, and beneficial to all.



 Zach Wentz, a technical staff member at Reflection, commented on their success in building such a remarkable open model with a much smaller team and fewer computing resources than he led at Meta: 'This achievement would not have been possible without a deep partnership with research, and I feel incredibly fortunate to have been able to build such a relationship at Reflection.'

 This is a bit impromptu but some thoughts are in order, this feels quite a triumphant moment personally and for the team.

 The technical stuff, we'll get to; but the thing I'm feeling in this moment after months of harder work than I could have ever imagined is how everyone else on the team matched it and exceeded my own effort.

 I thought I knew what it meant to ship, I didn't. Now I do.

 Too many people to thank, but just a quick thank you to @joespeez and @_ghorbani for the opportunity and the bet. I've never worked harder, learned as much, nor had more fun. I'm proud to work with you both.

 On the tech side of things 1.3B sandboxes is truly a frontier open RL run, and we did so with a dramatically smaller team and less compute than the one I led at Meta. An achievement like this is only possible with a deep partnership with research, one I feel so fortunate to have here at Reflection. On that latter point, I will say, this is the first time I feel like a part of our SWE Bench and tbench performance.

 There's so much to say, but I want to go hang with the team, so I will close with this. The best thing about all of this is: we did this so quickly, but we invested in the right foundation, so we're not slowing down.

 Western frontier today, frontier tomorrow.



 Another technical staff member, Alex Polozov, said, 'Beam is Reflection's first open weight model. It's the culmination of about a year of hard work, ingenuity, and camaraderie by one of the most amazing teams I've ever seen in AI. I joined Reflection in November 2025, which was when pre-training had just begun. There were only 5-10 members and a little code. We didn't have a crawler, a resilient GPU infrastructure, a deduplication pipeline, an internal evaluation, or even a sufficiently large MoE. All of that really started in 2026, which was one of the most satisfying sprints I've ever experienced.'

 I am so proud of Beam.

 Beam is Reflection's first open-weight model. It's the result of a ~year of hard work, ingenuity, and camaraderie of one of the most incredible teams I've ever seen in AI 🔥

 I joined Reflection last November. Pretraining in particular was so nascent – only 5-10 people and a bit of code! We didn't yet have a crawler, or a resilient GPU infra, or a deduplication pipeline, or in-house evals, or even sufficiently big MoEs. All that started in earnest in 2026, in one of the most satisfying sprints I've ever had the pleasure to live through.

What we nailed, I think, was a perfect blend of scientific rigor, startup intensity, team iteration, and ambition. We did not YOLO decisions in pretraining, that tends to blow up in your face when you scale up 🙂 But we also made sure to move fast and play to our strengths as a nimble, high-ownership, high-trust team. The resulting system of systematic iterations and fast cross-team back-and-forths really paid off.

 The midtraining, RL, and post-training teams really really cooked here on top of the pretrained base. The announcement below details the sheer scale and payoff of that investment, I'll let the team cover it, it's remarkable. On many agentic coding capabilities the model kept going up without any sign of a plateau.

 Feel free to read more in the announcement today – but later this month we will also drop the strongly full weights release under Apache 2.0, a detailed tech report with the cool LLM science, and partner integrations with the OSS ecosystem. It's an open model after all. I believe open intelligence is the future of AI, this is why I'm here.

 And I encourage y'all to join us on the open side 😉



 Jake Nations, who worked as the leader of the data platform team at Reflection, commented, 'We started from scratch and built all the pipelines, storage, inference, infrastructure, and tools necessary to build this model. Beam has been trained on 23.8 trillion tokens. We are most proud that the OCR pipeline we built has generated trillions of tokens from hundreds of millions of PDFs.'

 8 months ago I joined Reflection to build a frontier western open weight model. Today we're releasing Beam (501B total, 23B active with an Apache 2.0 license).

 I worked in many roles over this time, but spent a majority of the time here leading our data platform team.

 We started from zero and built out all of the: pipelines, storage, inference, infrastructure, and tooling needed to get this model built.

 Processing + storage. Beam trained on 23.8T tokens. I'm most proud of the OCR pipeline we put together generating trillions of tokens from hundreds of millions of PDFs. We scaled the data team from working with single terabytes to multiple petabytes.

 None of this happens without the team. The talent across research and engineering here is the best I've worked with, and it's why we could ship this on a first release. Looking forward to getting this model and future releases into everyone's hands soon!



 Ollama, an open-source runtime platform for managing and running open models, also welcomes Beam's arrival.

 Excited about the new open model from @reflection_ai!

 More US models coming to Ollama!



 Additionally, an interview video has been released in which the founder of Reflection talks about the story behind the development of Beam, which aims to be the 'Western version of DeepSeek.'

 Reflection's founders on building a DeepSeek of the West

 Reflection is soon releasing Beam, its first open-weight AI model, in a bid to become the DeepSeek of the West. Trained from scratch, Beam is designed for coding, reasoning, and AI agents. Reflection's benchmarks put it alongside the strongest open models, with substantially more efficient token economics.

 To mark Beam's debut, I'm joined by Reflection's co-founders: CEO @MishaLaskin, who led reward modeling for Google's Gemini, and CTO @real_ioannis, who helped create AlphaGo and AlphaZero before leading Gemini's RLHF effort. Reflection has raised $4.6 billion from investors that include Nvidia, Sequoia, and Lightspeed.

 We discuss why Chinese labs pulled ahead in open AI models, how Reflection built Beam, and why the founders think companies and countries will increasingly want to control their own models. I ask how they plan to make money from open-weight AI, whether openness can make these systems safer, and what comes next for Reflection.

 Timestamps:

 00:00 Why open models are surging

 01:06 Renting vs. owning intelligence

 03:40 Open models catch up

 05:53 Why China led open AI

 08:03 Introducing Beam

 10:05 Scaling reinforcement learning

 14:09 Training from scratch

 16:04 Safety, alignment and openness

 32:29 The Reflection origin story

 37:32 Making money on open models

 42:33 Vertical integration and computation

 48:45 Partnerships and distribution

 56:46 What's next for Reflection

 Links: https://t.co/lsXfGvSuAw

 Spotify: https://t.co/XSTrGNHb8O

 YouTube: https://t.co/UqqKA6AYmT

 Substack: https://t.co/difFf7X42Z

 Thanks to the show's premier sponsors: @Atlassian, @meetgranola, and @mercury.




**Related Posts:**
