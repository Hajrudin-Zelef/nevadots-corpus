---
id: collect-261121-veille-ia/muse/meta-releases-muse-spark-open-to-challenge-chinese-ai-models-with-limits-on-hardware-and-commercial-
title: "Meta releases Muse Spark Open to challenge Chinese AI models, with limits on hardware and commercial use"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["open", "meta", "model", "muse", "spark", "chinese", "models", "developers"]
source: https://asiadaily.org/news/15613/
source_anchor: ""
source_lines: [1, 124]
section: "Muse"
---
## An 816 billion parameter model enters the open AI race

Meta released Muse Spark Open on October 15, 2026, giving developers an artificial intelligence model they can download, modify and operate themselves. The model contains 816 billion parameters, supports a context window of 1 million tokens and, according to Meta, can run on two Apple M5 Ultra Mac Studio computers configured with enough memory.

The release brings another American contender into a market where Chinese developers have gained substantial ground. Hugging Face's assessment published in March 2026 found that Chinese models accounted for 41% of downloads over the preceding year, surpassing American models. Alibaba's Qwen, Moonshot AI's Kimi and Z.ai's GLM are among the models competing for developers who want control over their own AI systems.

Meta calls Muse Spark Open its most capable American open weights model. Its own tests show a more detailed picture: the model leads several listed competitors on certain tests of tools and task completion, while Chinese rivals score higher on several coding and professional work evaluations.

Meta, the parent company of Facebook, Instagram and WhatsApp, is also keeping its most advanced model, Muse Wonder, closed. The release therefore serves two goals: spreading technology developers can adapt and preserving a more powerful product for commercial services.

For businesses, the practical decision involves more than benchmark scores. Muse Spark Open offers control over deployment and data, but its memory requirements are substantial, and its license includes a separate agreement requirement for some companies selling access to the model.

## What developers can download and change

Meta Superintelligence Labs is identified as the author in the official Muse Spark Open model card. It describes a system built for coding, reasoning through several steps and completing tasks with software tools.

Model weights are the numerical settings learned during training. Making them downloadable allows another developer to run the model, adapt its behavior and integrate it into a product without sending every request to Meta. Closed models generally provide access through services controlled by their developers.

Open weights and open source describe different levels of access. A complete open source release would also make relevant training code and data available under suitable terms. Muse Spark Open is distributed under Meta's own license, which sets conditions on use and redistribution.

The model accepts text, images, video and audio, and produces text. A developer could use it to inspect a screenshot, interpret a chart or work through a recorded meeting alongside written instructions. Its context window of 1 million tokens determines how much material it can consider together. Tokens are small units of text or other encoded information processed by the system.

Meta's model overview highlights coding, perception of images and documents, and workflows involving several AI agents. An agent is a system that can take steps toward a task, such as calling a tool, examining the result and deciding what to do next.

## Where Meta leads and where Chinese rivals lead

Meta's benchmark table gives Muse Spark Open a score of 61.8 on JobBench, compared with 61.4 for GLM 5.3 and 54.3 for Kimi K3. On MCP-Atlas, a test involving tool use, Meta reports 89.2 for its model, ahead of Qwen3.8 at 84.5, GLM 5.3 at 84.2 and Kimi K3 at 82.3.

Coding results favor the Chinese competitors on several measures. On Terminal-Bench 2.1, Meta reports 76.4 for Muse Spark Open, compared with 83.9 for GLM 5.3 and 85.0 for Kimi K3. Kimi's advantage is 8.6 points. On DeepSWE 1.1, Muse Spark Open scores 60, against 67 for GLM and 68 for Kimi.

The DeepSWE comparisons use each model's preferred or best available surrounding agent software. That software helps organize the work, so the reported result reflects the model working within a particular setup. Meta also uses different named reasoning settings for the competing models.

Meta reports an Artificial Analysis Intelligence Index score of 39 for Muse Spark Open, above the 26 listed for Thinking Machines' Inkling and 23 for Nvidia's Nemotron 3 Ultra. Its table places GLM 5.3 at 45, Kimi K3 at 44 and Qwen3.8 at 40. These are figures published by Meta in its model card.

The buying decision consequently depends on the work. A company building an assistant that coordinates tools may value different results from a team asking AI to solve difficult software maintenance problems. Meta's release offers strengths in particular tasks while facing strong competition across the wider test set.

## Why downloadable models have gained ground

Hugging Face's 41% download figure helps explain the commercial pressure on American developers. Downloadable models let organizations build on an existing system rather than pay to train a large model from the beginning. They can also adjust it for their own terminology, procedures and applications.

OpenRouter reported that open weights models handled 60% of token volume from requests originating in the United States on its platform in September. Chinese developers supplied most of its traffic involving open models. That measure describes activity on OpenRouter, while the Hugging Face figure measures downloads on a different platform over a different period.

Meta Chief AI Officer Alexandr Wang presented the competition in national terms, arguing that American companies need to release models developers can adopt widely.

Most of the world's AI will run on open models. They should be American.


Businesses can choose where to host downloadable models, including models developed abroad. Running a system on an organization's own infrastructure can keep prompts and documents within that environment, provided its connected tools and services are configured accordingly. The location of processing depends on the deployment the customer chooses.

## Two desktop computers, substantial memory requirements

Meta's October 15 announcement describes an Apple setup using two M5 Ultra Mac Studios, each with up to 512 GB of unified memory. Apple's MLX framework allows the machines to work together and hold the model across roughly a terabyte of memory.

Muse Spark Open uses a mixture of experts architecture. It contains many specialized groups of parameters and activates only some for each token it processes. Of its 816 billion total parameters, 42 billion are active at a time, about 5.1% of the total. This reduces the computation needed for each step, while storing the full model still requires extensive memory.

The model card lists at least 700 GB of accelerator memory for the NVFP4 version and 2 TB for BF16. These are different numerical formats for storing weights. NVFP4 uses fewer bits and less memory; BF16 uses more memory, while Meta notes that compressed inference can produce small quality differences in some cases.

Apple's 512 GB Mac Studio configuration was due later in October, and its price had not been announced at the launch. A machine with the required M5 Ultra chip and 256 GB of memory started at $10,799. Four at that listed price would cost $43,196 before tax and accessories and provide the same combined memory capacity as the proposed pair.

Meta demonstrated local workflows involving changes to Ice Cubes, a Mastodon application, and operating DaVinci Resolve to inspect footage and prepare vertical crops. It did not give a complete price or a measured text generation rate for the proposed pair of computers.

## Apple and Meta find common ground

Apple and Meta worked together for months on the deployment. The partnership joins companies whose relationship had been strained by disputes over privacy and advertising. Meta estimated that Apple's privacy changes introduced in 2021 would reduce its revenue in 2022 by $10 billion.

Meta had already bought hundreds of Mac computers for its Superintelligence team to experiment with local AI and agents. Wang credited AI researcher and former Apple director Daniel Gross as one reason the collaboration came together.

Apple gains a demonstration of its hardware handling a very large AI model outside a data center. Meta gains another deployment route for developers who want to work locally. Neither company committed publicly to a wider partnership.

Meta also supports AMD, Intel and Nvidia hardware through several inference engines, the software that runs a trained model. Its announcement lists SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, MLX, llama.cpp and Unsloth.

Performance improvements vary by component and system. Meta reports speed increases of 1.03 to 3.95 times for individual Helion GPU kernels, which are small computing routines. For the complete workflow on AMD MI350 hardware, it reports a 3% speed increase.

## What free use permits and where it stops

The Muse Model License allows many developers to run, modify and adapt the model without paying Meta. A specific commercial provision applies to qualifying businesses that provide models as a service, meaning they sell customers access to a hosted model.

When a licensee and its affiliates exceed $20 million in aggregate gross revenue during any consecutive 12 months, qualifying model serving activity requires notification to Meta and a separate written agreement. The revenue calculation covers the company and its affiliates, rather than just sales generated by Muse Spark Open.

Internal use and other stated activities have exemptions from that provision. The license also requires products using the model or covered derivatives to display the attribution "Built with Muse Spark Open." Businesses planning a deployment therefore need to examine both their intended activity and the applicable conditions.

Developers can download weights through Amazon SageMaker AI, Google Cloud Model Garden, Hugging Face and Microsoft Foundry Models. Hosted access is available through Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI.

Together AI lists serverless pricing at $1.25 per million input tokens and $4.25 per million output tokens. At those rates, processing 1 million input tokens and generating 1 million output tokens would cost $5.50. The economics of local operation depend instead on hardware spending, utilization, electricity and technical support.

## Safety work extends beyond the download

Meta says Muse Spark Open is derived from Muse Spark, released in August, with additional safety training intended to prepare it for use outside Meta's own protective systems. Its model card evaluates harmful content, actions taken through tools, privacy, and risks involving cybersecurity, chemistry, biology and loss of control.

Meta reports that the released model refused 99.2% of attempts on its internal cyber jailbreak benchmark. A jailbreak is an attempt to get a model to bypass its restrictions. Across three other cybersecurity test sets combined, Meta reports a refusal rate of 94.8%.

Developers still have responsibilities when connecting the model to real systems. Meta recommends safeguards tailored to the application, including human confirmation before irreversible actions. An assistant that can edit code, move files or invoke external tools needs controls over which actions it can perform.

The model card says the system can produce inaccurate, biased or objectionable responses and can make mistakes during complex reasoning. It was trained on data from more than 100 languages, but Meta has not evaluated every language included in training. Developers adapting the model must test their own applications and languages, particularly after changing its weights or behavior.

## A release that also supports Meta's closed business

Meta is keeping Muse Wonder, its more advanced model, proprietary. Wang said safety and capability considerations influence which models the company releases for others to download.

Because of both safety and capabilities, we will generally not open-source our most powerful models.


Meta also sees free access as a way to attract developers who may later pay for more tools and computing capacity through its planned cloud business. Developers can begin experimenting independently, then buy hosted services as their requirements grow.

The competitive schedule extends beyond Meta. France's Mistral announced Mistral Large 4 earlier in October, describing it as its largest and most capable model, with 1 trillion total parameters and 49 billion active parameters. Its weights were scheduled for publication on October 27. Reflection AI also planned to release its Beam model during October.

The main milestones are:

- August 2026: Meta released Muse Spark and the smaller open weights model Muse Glimmer.
- Early October: Mistral announced Mistral Large 4.
- October 15: Meta released Muse Spark Open.
- Later in October: Apple's Mac Studio configuration with 512 GB of memory was due to arrive.
- October 27: Mistral scheduled publication of the Large 4 weights.

Meta's release gives organizations another substantial model they can operate and adapt themselves. Its appeal rests on the combination of task performance, local deployment options and license terms, while its commercial strategy leaves the company's most powerful technology behind a paid service.

## Key Points

- Meta released Muse Spark Open on October 15, 2026, with 816 billion total parameters and 42 billion active at a time.
- The model accepts text, images, video and audio, produces text, and supports a context window of 1 million tokens.
- Meta's tests show strengths in tool use, while Kimi K3 and GLM 5.3 lead on several coding measures.
- Local operation requires substantial memory. Meta's proposed desktop setup uses two M5 Ultra Mac Studios with 512 GB each.
- Qualifying model serving businesses above the $20 million revenue threshold need a separate Meta agreement.
- Muse Wonder remains closed, while Meta uses downloadable models to attract developers and potential cloud customers.
