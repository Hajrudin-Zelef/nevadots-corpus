---
id: collect-261121-veille-ia/deepseek/microsoft-unveils-mai-base-2-and-claims-an-edge-over-deepseek
title: "Microsoft unveils MAI-Base-2 and claims an edge over DeepSeek"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["model", "microsoft", "mai-base-2", "parameters", "pre-training", "claims", "microsofts", "comparison"]
source: https://www.aiaffairs.com/technology/microsoft-mai-base-2-deepseek-pretraining-efficiency/
source_anchor: ""
source_lines: [1, 38]
section: "DeepSeek"
---
Microsoft AI said on 15 October 2026 that it had completed pre-training MAI-Base-2, a mixture-of-experts model with 2.8 trillion total parameters and 80 billion active parameters, Breaking the News reported. Microsoft claims the model is up to 12 times more efficient in pre-training computation than DeepSeek-V4-Pro-Base. Its AI chief has separately claimed an advantage over the OpenAI model behind GPT-5.6, a comparison based on Microsoft’s own tests.

## Key points

- Microsoft says MAI-Base-2 has 80 billion active parameters out of 2.8 trillion in total.
- The company says it trained the model on 16k GB200 GPUs over 38 days in August and September 2026.
- Microsoft claims an advantage of up to 12 times in pre-training FLOPs efficiency against DeepSeek-V4-Pro-Base.
- Mustafa Suleyman claims internal tests put MAI-Base-2 ahead of the OpenAI model behind GPT-5.6 while using about a quarter of the training compute.

## MAI-Base-2 activates 80 billion parameters

The difference between MAI-Base-2’s total and active parameter counts is central to its design. A mixture-of-experts model holds different groups of learned settings, or parameters, and calls on a selection of them for a given piece of work. Think of a large reference shelf from which only the relevant volumes are taken down for each question. Microsoft’s figures describe a model with 2.8 trillion parameters available across the whole system, while 80 billion are active at a time.

Writing a draft at work could draw on that larger pool of learned settings without using all of them for each piece of text. Whether it would produce a useful draft would depend on how the model performed on that writing task, rather than on its parameter counts alone.

Microsoft describes MAI-Base-2 as larger and sparser than MAI-Base-1 and says it was trained on more data. The company says its training run used 16k GB200 GPUs for 38 days during August and September 2026. Those details concern pre-training: the stage at which the model learns patterns from data through repeated computation, before the separate work of generating an answer when someone uses it.

The distinction matters to companies building on a model. The resources required to train it are one part of its economics; producing answers is another. A design that activates only a portion of its parameters makes that second question worth examining separately, while Microsoft’s published efficiency comparison concerns pre-training.

## DeepSeek-V4-Pro-Base anchors Microsoft’s efficiency claim

Microsoft says MAI-Base-2 outperformed “every open-weight model we tested on our held-out evaluations”. That is a claim about the models Microsoft selected and the evaluations it ran. The company also puts its pre-training FLOPs efficiency at up to 12 times that of DeepSeek-V4-Pro-Base, which it identifies as the strongest open base model in the comparison.

FLOPs count arithmetic operations. In this comparison, efficiency concerns how much training calculation is needed to reach a result, rather than how quickly a finished model replies. The phrase “up to” also matters: Microsoft’s figure describes the high end of its reported advantage, not a single rate that applies to every test or task.

For a company deciding what model to build on, the distinction between a base-model training result and performance during use is practical. Microsoft’s held-out evaluations test models on material reserved for assessment, while its FLOPs comparison addresses training expenditure. Neither is the same measurement as the cost of serving an answer, the reliability of an answer in a particular workflow, or the price a customer would pay.

## Suleyman compares MAI-Base-2 with GPT-5.6

Mustafa Suleyman, the CEO of Microsoft AI, told employees that MAI-Base-2 beat the OpenAI model powering GPT-5.6 in private tests of how models understand and predict patterns in information, Startup Fortune reported. Suleyman said Microsoft used about a quarter of the compute OpenAI spent training the comparable system, internally nicknamed Spud.

The claimed lead therefore rests on Microsoft’s private tests against the OpenAI system, rather than on the open-weight evaluations used for the company’s other performance claim.

The two companies remain closely connected. Microsoft holds a stake in OpenAI Group PBC valued at roughly $135 billion and remains its primary cloud and infrastructure partner. It sells OpenAI models to Azure customers and uses them in Copilot, even as Microsoft AI develops its own foundation models.

Microsoft and OpenAI revised their partnership earlier in 2026, removing a clause under which Microsoft’s access to OpenAI’s most advanced models would have ended if OpenAI’s board declared artificial general intelligence achieved. The revised terms secure Microsoft’s rights to OpenAI technology through 2032, and OpenAI is expected to return roughly $6 billion of revenue to Microsoft this year, Startup Fortune reported.

Microsoft’s own models also bear on what it earns when they are used. The company says it has shipped more than a dozen MAI models across image, voice, transcription, coding and security since last quarter. It puts gross inference margins for MAI-1 near 100%, compared with 60-70% for reselling OpenAI’s API.
