---
id: collect-261121-veille-ia/mistral/mistral-large-4-goes-open-weight-at-1t-parameters
title: "Mistral Large 4 Goes Open Weight at 1T Parameters"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-11-03"]
keywords: ["mistral", "large", "model", "open", "parameters", "license", "tokens", "weights"]
source: https://www.technology.org/2026/11/03/mistral-large-4-open-weights-hugging-face-modified-mit/
source_anchor: ""
source_lines: [1, 37]
section: "Mistral"
---
## Europe’s Biggest Open Model

Mistral trained its new flagship from scratch on 3,800 Nvidia Grace Blackwell GPUs in its own European data centers, and now anyone can download the result. The French company has released the open weights of Mistral Large 4, a natively multimodal model with about 1 trillion parameters, on Hugging Face under a modified MIT license, three weeks after showing it as an API-only preview.

### **Key Takeaways**

- Mistral Large 4, nicknamed “Le Chonk,” is a Mixture-of-Experts model with about 1 trillion total parameters and roughly 49 billion active per token.
- The Mistral Ecosystem Open-Weight License allows free use, fine-tuning and commercial products, but companies selling the model as a hosted service need a separate commercial license.
- Mistral says the model leads open-weight models on coding benchmarks and uses 47% fewer tokens than rivals on similar tasks, though these are vendor claims.

## What Is Inside

Large 4 uses a Mixture-of-Experts design. Of its roughly 1 trillion parameters, about 49 billion work on each token, or 52 billion when embedding and output layers are counted.

The model reads text and images, handles a context window of 1 million tokens and understands more than 160 languages. The main checkpoint, Mistral-Large-4-1T-A52B, ships in 8-bit FP8 format, with a smaller NVFP4 version aimed at Blackwell GPUs.

Running it locally still requires data center class Nvidia hardware. Deployment works through vLLM and SGLang, and Hugging Face says transformers and llama.cpp supported it from day one.

## Three More Weeks of Training

Mistral first showed Large 4 on October 6 as a public preview available only through its API. The company says about three more weeks of reinforcement learning followed, bringing large gains in coding, defensive cybersecurity and reasoning efficiency.

According to Mistral, Large 4 now tops open models on the Artificial Analysis Coding Agent Index and ranks first among open-weight models on FrontierCode 2.0 and Terminal-Bench 4.0. It also says the model matches China’s GLM-5.3 while using 47% fewer tokens. Independent tests have yet to confirm those numbers.

## License and Price

The license keeps most MIT freedoms. Developers can use, modify, retrain and build the model into commercial products at no cost. The one carve-out targets resellers: any business offering Large 4 as a model-as-a-service needs a commercial license from Mistral.

Users who prefer not to host it can pay for API access at $0.70 per million input tokens, $0.07 per million cached input tokens and $2.10 per million output tokens through Mistral Studio, the Mistral API or OpenRouter. Microsoft Foundry is the first cloud platform to offer it.

## Open Weights and Cyber Defense

Large 4 is the largest open-weight frontier model from a Western lab, and Mistral pitches it squarely at sovereignty and security. Governments and companies can now download the weights and run them on their own servers, a selling point in Europe, where reliance on US cloud providers worries policymakers. The release lands weeks after Meta published its 816-billion-parameter Muse Spark Open, so the open-weight field is getting crowded at the top. Mistral had previewed the model earlier in October.

Cyber skills cut both ways. Tests have shown Chinese models such as GLM-5.3 completing multi-stage cyberattacks in controlled settings, and some labs now trim hacking abilities before releasing weights. Mistral frames Large 4 as a defensive tool that helps organizations protect themselves against cyber threats, but once weights are public, the company cannot control how they are used.

*Written by Vytautas Valinskas*
