---
id: collect-261121-veille-ia/modeles-open/ant-group-publishes-ling-3-1-flash-weights-under-an-mit-licence
title: "Ant Group publishes Ling-3.1-flash weights under an MIT licence"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "ling-3", "flash", "weights", "billion", "window", "trial", "parameters"]
source: https://www.aiaffairs.com/technology/ant-group-ling-3-1-flash-weights-mit-licence/
source_anchor: ""
source_lines: [1, 34]
section: "Modeles_Open"
---
Ant Group’s inclusionAI has published the weights for Ling-3.1-flash on Hugging Face under an MIT licence, Pandaily reported on 15 October. The release follows the end of a free API trial on 14 October and gives developers access to the model itself, rather than only to Ant’s hosted service. Ling-3.1-flash has 560 billion total parameters, activates 25 billion for each token it processes and has a context window of up to 1 million tokens.

## Key points

- The weights are available on Hugging Face under an MIT licence.
- The model has 560 billion total parameters, with 25 billion active per token.
- Its million-token context window is larger than the 256K-token limit of the free API trial.
- Ant provides deployment guidance for SGLang and vLLM, alongside a reference hardware setup.

## The switch after 14 October

Ant announced Ling-3.1-flash on 30 September with a two-week free API trial limited to 256K tokens of context. At the announcement, the team said it planned to make the weights available and open the full million-token window when the trial ended and the service moved to paid access. The free period closed on 14 October, and the weights were published alongside that change.

A context window is the amount of material a model can take into account in a request, measured in tokens rather than pages. The million-token figure describes Ling-3.1-flash’s window; the free trial exposed a smaller 256K-token limit. Those are different conditions for using the same model, so results obtained through the trial would have involved a shorter maximum input than the model’s stated capacity.

Sorting a long set of work documents could involve passing a larger stretch of text in one request than the free-period service accepted, if the text fits within the million-token window. That would make it possible to work with more of the material at once, while still leaving the quality of any answer to depend on how the model handles the material it receives.

## Ling-3.1-flash routes tokens through eight of 512 experts

Ling-3.1-flash uses a mixture-of-experts design: it contains many sets of model parameters, but routes each token through only some of them. Its architecture has 512 routed experts and one shared expert, with eight routed experts activated for a token. That distinction explains how Ant can describe a model with 560 billion parameters in total while putting 25 billion to work on each token. The total is the model’s capacity, not the amount used at every step.

The other part of the design concerns attention, the mechanism that lets a model draw on earlier text while processing later text. Ant’s model card, as reported by Pandaily, describes 63 KDA linear-attention layers and nine Gated MLA layers, arranged at a 7:1 ratio, as well as four dense layers. The mix matters particularly for a model built to accept long inputs: different parts of its stack handle the growing sequence in different ways, while the expert routing determines which parameters are used for each token.

Ling-3.1-flash builds on Ling-3.0-flash, which Ant released in July. The earlier model has 124 billion total parameters, with 5.1 billion active, across a 42-layer hybrid stack, OrcaRouter reported on 12 October. Ant also released the multimodal Ling-3.0-flash-VL in September. The new model extends that line with a larger expert pool and a stated million-token window.

Ant says Ling-3.1-flash improves its performance on general-purpose agents, search, office work and software engineering. It reports strong results on coding and agent benchmarks including Terminal-Bench 4.0, SWE-bench Pro and SkillsBench, as well as HealthBench Professional. Those are Ant’s reported evaluations of the model; the release also names finance and materials science among areas in which the company says it has made gains.

## SGLang guidance and the LingCode harness

For developers running the weights, the model card recommends SGLang and provides an image that supports multi-token-prediction speculative decoding. In plain terms, speculative decoding tries to prepare more than one piece of an answer before the main model has finished checking it, rather than waiting for every piece in turn. Ant’s reference server configurations use eight H200 GPUs or four GB300 GPUs, and the card also provides a vLLM recipe.

That deployment guidance matters alongside the licence. Downloadable weights give a developer the model to run, while a serving recipe describes one way to put it into operation. The reference GPU configurations are Ant’s setups for that job, not a measurement of what every deployment will need; the reported architecture and parameter counts describe the model, while the serving software describes how Ant proposes to run it.

The card introduces LingCode, a coding harness built on the open-source agent Pi. Ant has tuned it for jobs that can run for hours, require hundreds of tool calls and repeatedly compress earlier context so work can continue. Ant says it is considering making LingCode open source. The Ling-3.1-flash model is also listed on ModelScope and OpenRouter.
