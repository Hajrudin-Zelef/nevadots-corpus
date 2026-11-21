---
id: collect-261121-veille-ia/mistral/mistral-large-4-explained-the-trillion-parameter-open-model-that-only-uses-about-5-of-itself-at-a-ti
title: "Mistral Large 4, explained: the trillion-parameter open model that only uses about 5% of itself at a time | Emergent Mind"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["model", "mistral", "token", "large", "about", "experts", "weights", "every"]
source: https://www.emergentmind.com/mistral-large-4
source_anchor: ""
source_lines: [1, 96]
section: "Mistral"
---
## The short version

On October 29, 2026, Mistral released the weights of Mistral Large 4, which it nicknames "Le Chonk." It is a mixture of experts: 128 smaller networks plus one shared one, and for every token a small router picks just four of the 128 to run. So about 49 billion of its roughly 1 trillion parameters work at a time. That keeps each step cheap, but every expert still has to sit in memory, which at one byte per parameter is about a terabyte.

## 01What happened

At 10:21 am Eastern on October 29, 2026, Mistral announced that it was releasing the weights of "a new and improved Le Chonk," its flagship Mistral Large 4. The model had been available through Mistral's own service in a public preview since earlier in the month; this release adds the downloadable weights, posted on Hugging Face alongside an announcement post.

Weights are the numbers a model learned in training. Releasing them means you can run the model on your own hardware, study it, and fine tune it, instead of only reaching it through a company's website or API.

Mistral's model card describes a model with about 1 trillion parameters in total, 49 billion active per token (52 billion counting the input and output layers), a 1 million token context window, and text and image input with text output. NVIDIA says it was trained from scratch on 3,800 Grace Blackwell GPUs.

## 02How it works: a room of 128 experts

A parameter is one of the learned numbers inside a model. A token is the chunk of text a model reads or writes in one step, often a word or part of one.

In an ordinary "dense" model, every parameter is used for every token. A mixture of experts splits much of the model into many smaller networks, called experts. A small router looks at each token and sends it to just a few of them. Mistral Large 4 has 128 experts and picks 4 per token, plus 1 shared expert that runs every time.

Choose a word to see which experts it lights up.

An illustration, not the real router: here each word is hashed to 4 of the 128 dots. The real router is learned in training and works on tokens, which are often smaller than words. The blue dot is the shared expert that always runs.

That is why a trillion-parameter model can answer at the cost of a much smaller one: the work per token scales with the 49 billion active parameters, not the trillion. It is also why the headline numbers on the model's name, "1T" and "A52B," look so different.

## 03The catch: you still have to hold all of it

The router can pick any expert for the next token, so all 128 have to be loaded and ready. Saving compute does not save memory.

Checking the memory math

1,050 GB

8 bits (FP8), the format Mistral published: about a terabyte.

This is plain arithmetic on Artificial Analysis's count of 1.05 trillion parameters (see sources below). Real files differ a little, because some parts of a model are usually kept at higher precision. Mistral published the weights in FP8 (8 bits each) and in NVIDIA's 4-bit NVFP4 format, and its own serving examples use 8 GPUs.

Squeezing each parameter into fewer bits is called quantization. It shrinks the model and costs some accuracy. Within minutes of the release, Unsloth said its 1-bit version runs on a Mac Studio and keeps 91% top-1 accuracy, a measure of how often it picks the same next token as the full model. That is Unsloth's own claim.

## 04How good is it?

Mistral calls it the best open-weight model for coding. Its claims, from the announcement:

- 19.7% on Cognition's FrontierCode 2.0, the top score among open models.
- 48% on the Artificial Analysis Coding Agent Index, effectively tied with GLM-5.3 while using 47% fewer tokens per task.
- A 96.6% average refusal rate on malicious cyber requests, which Mistral says is the highest among the open models it measured.

The independent view: Artificial Analysis says Large 4 is "narrowly" the top open-weights model developed outside China, scoring 26 on its Intelligence Index v5, one point up from the preview. On its Cyber Index it scores 51, third among open models behind DeepSeek V4.1 Flash (57) and MiMo-V2.6-Pro (56).

## 05What you can do with it

The weights come under the Mistral Ecosystem Open-Weight license, which Mistral describes as a modified MIT license. You can use, modify, fine tune and build commercial products on it. The exception: a business that offers the model itself as a hosted service needs a separate commercial license from Mistral. If you just want to call it, Mistral's API charges $0.70 per million input tokens and $2.10 per million output tokens.

## 06Reactions

        "Good progress in the last few weeks! And now the weights"


        Showed Hugging Face's TRL library fine tuning the 1-trillion-parameter model with LoRA adapters on a single node of 8 B300 GPUs, on day one. "Open weights = open training after all."


        Announced Transformers v5.22.0, which adds support for Mistral Large 4.


## 07Timeline

1. **Earlier in October 2026.** Mistral Large 4 arrives in public preview on Mistral's own service.
2. **Oct 28, 2026.** Mistral launches its Responses API in preview, listing Large 4 among the models it serves.
3. **Oct 29, 2026, 10:21 am ET.** Mistral releases the weights of the improved model on Hugging Face.
4. **Oct 29, 2026, within the hour.** Day-one support from NVIDIA, Hugging Face Transformers, SGLang and Unsloth; Artificial Analysis publishes its scores.

## 08Questions people are asking

## Is it really "open source"?

It is open weights: you can download and run it, and the license allows commercial use. The training data and code are not released, and businesses selling it as a hosted model need a separate license, so it is not open source in the strict sense.

## Can I run it on my laptop?

Not at full precision. The FP8 weights alone are around a terabyte. Unsloth says a 1-bit version runs on a Mac Studio, at some cost in accuracy. Most people will reach it through an API or a cloud GPU server.

## Why does it say both 49B and 52B active?

The model card says 49 billion active parameters per token, or 52 billion counting the embedding and output layers, which every token passes through. The "A52B" in the model's name uses the larger number.

## Is a mixture of experts the same as having 128 separate AIs?

No. The experts are pieces inside one model, trained together, and the router choosing between them is learned too. No single expert is a model on its own, and experts usually do not line up neatly with topics the way the name suggests.

## Why is it called Le Chonk?

It is Mistral's own nickname for the model, used in its announcement. "Chonk" is internet slang for a big, round cat.

## 09Updates

- Mistral posted a demo: from a short prompt, it says, the model wrote a quantum chemistry solver from scratch, checked its output and built a 3D view of a molecule breaking. That is Mistral's own showcase, not an outside test.
- This page went up. It covers everything known as of October 29, 2026.
