---
id: collect-261121-veille-ia/muse/meta-muse-spark-open-brings-816b-parameter-ai-to-mac-studios
title: "Meta Muse Spark Open brings 816B-parameter AI to Mac Studios"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["model", "muse", "open", "spark", "developers", "meta", "studios", "hardware"]
source: https://yourstory.com/ai-story/meta-muse-spark-open-816b-ai-mac-studios
source_anchor: ""
source_lines: [1, 33]
section: "Muse"
---
What if running a massive AI model did not always require a data centre? Meta is bringing that idea closer to developers with Muse Spark Open, an 816-billion-parameter model designed to run locally on two Apple Mac Studios powered by M5 Ultra chips.

Models of this scale are usually associated with extensive cloud infrastructure. Muse Spark Open offers another option, allowing developers to run a powerful AI system on specialised desktop hardware rather than relying entirely on remote servers.

Designed for coding, complex reasoning and multi-step tasks, the model has 816 billion parameters in total, with 42 billion active parameters. Instead of using all its parameters for every response, it activates only a portion as needed, helping balance performance and efficiency. Here's all that you need to know!

## What can an 816B model do?

Muse Spark Open supports a context window of 1 million tokens. This determines how much information a model can consider at once, making a large window useful for working with lengthy documents, extensive codebases and detailed project histories.

Meta is also targeting agentic AI workflows, where systems can plan and execute multiple steps towards a goal instead of simply responding to individual prompts. Potential applications include fixing software bugs, adding features to apps and analysing video footage frame by frame.

That combination of long-context processing and multi-step execution could make the model useful for developers handling complex projects.

## Powerful AI, closer to the developer’s desk

The hardware setup is the headline feature.

Meta says Muse Spark Open has been optimised for Apple’s M5 Ultra Mac Studio using the MLX framework. Two Mac Studios can be connected to provide enough unified memory to load and run the model.

This is not a plug-and-play option for the average home user. The hardware remains expensive, and running a model locally still requires specialised equipment. But it gives developers, startups and research teams another way to explore large AI systems, with greater control over deployment and potentially sensitive data.

## Where developers can access it

Muse Spark Open is available through inference platforms including Groq, OpenRouter, Baseten, Modal, Nebius, Fireworks AI, and Together AI. Model weights can also be downloaded through Microsoft Foundry Models, Hugging Face, Google Cloud Model Garden and Amazon SageMaker AI.

Supported inference engines include vLLM, SGLang, Apple MLX, TensorRT-LLM, llama.cpp, Hugging Face Transformers and Unsloth. Developers can also customise the model using PyTorch.

However, open weights do not mean unrestricted use. Muse Spark Open is released under the Muse Model License, which includes limited commercial terms. Businesses should review the licence before building products around it.

Meta says the model has undergone safety training and evaluation under its Superintelligence Scaling Framework, meeting the company’s release thresholds.

Muse Spark Open will not make large-scale AI hardware requirements disappear. But it gives developers another way to experiment with a model of this size, without depending exclusively on cloud infrastructure.
