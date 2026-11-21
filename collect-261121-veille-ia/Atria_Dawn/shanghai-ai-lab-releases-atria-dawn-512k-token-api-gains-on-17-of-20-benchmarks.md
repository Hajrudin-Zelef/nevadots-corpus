---
id: collect-261121-veille-ia/atria-dawn/shanghai-ai-lab-releases-atria-dawn-512k-token-api-gains-on-17-of-20-benchmarks
title: "Shanghai AI Lab Releases Atria Dawn: 512K-Token API, Gains on 17 of 20 Benchmarks"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["model", "version", "official", "full", "context", "tokens", "shanghai", "atria"]
source: https://xenospectrum.com/en/atria-dawn-full-release-context/
source_anchor: ""
source_lines: [1, 57]
section: "Atria_Dawn"
---
Shanghai AI Lab (Shanghai Artificial Intelligence Laboratory) has released the full version of Atria Dawn, an AI model designed to carry out complex work such as research and software development across multiple steps. The official API supports a context length of up to 512K tokens, double the 256K of the Preview version. This makes it easier to take on tasks that span multiple stages while referring to large documents and past work history. However, the full version's model weights have not yet been released, so its availability differs from the downloadable Preview version. To evaluate this update, it is necessary to distinguish between what can be used through the API and what can be run in your own environment.

## 512K-token API now available; full-version weights still pending

Atria Dawn is an agentic AI model built on GLM-5.2, a foundation model with 744 billion parameters. It uses a Mixture of Experts (MoE) architecture, which combines networks that each handle specialized processing, and aims to advance multi-step tasks using tools while keeping track of its surroundings and the progress of its work. Rather than simply answering questions, it is positioned as a model that handles an entire workflow: analyzing a problem, designing a solution, implementing code, running experiments, and verifying results.

Pandaily's October 17 report noted that the context length has expanded to 512K tokens, while the GitHub model distribution table lists 256K. The official API documentation states explicitly that the available model ID "Atria-Dawn" has a 512K context length, and explains that this limit includes both input and generated output. In other words, not all 512K tokens can be used for reference material in the input. System instructions, conversation history, document excerpts, and results returned from tools all consume context, so room must be left for generating the response. While more history can be referenced during long-running research or code fixes, deciding what to keep and what to organize or discard remains important.

Context length also differs from the number of tokens that can be generated in a single response. According to the official API documentation, the parameter that sets the output token limit accepts values from 1 to 65,536, but the actual output volume also depends on stop conditions and the remaining context capacity. Support for 512K tokens therefore does not mean a large amount of text can be generated at once; it should be understood mainly as an expansion in the amount of information that can be referenced.

Model weights are the set of numerical parameters determined through training, and they are required to run an AI model on your own servers or similar infrastructure. The availability as of October 17, 2026, based on the model distribution table in the official GitHub repository, the official API documentation, and the Preview version's model card, is as follows.

| Target | Stated context length | Availability | 
|---|---|---|
| Full version API | 512K | Usage instructions published in official documentation | 
| Full version / full version FP8 distribution table | 256K | "Coming soon" on both Hugging Face and ModelScope | 
| Preview / Preview FP8 versions | 256K | Download links listed in the official repository | 

The official API explicitly states support for 512K tokens, while the GitHub distribution table lists 256K, and no explanation is given for the discrepancy. It therefore cannot be assumed that 512K tokens will be available when the full version's weights, once released, are run in your own environment. The statement that the code and model weights are released under the MIT license likewise does not mean the full version's files are already available for download.

## Official benchmarks show gains on 17 of 20 items

According to the benchmark results published by Shanghai AI Lab, the full version of Atria Dawn improved on 17 of the 20 items that can be compared with the Preview version, was flat on 1, and declined on 2. Improvements are reported across a broad range of areas, including research, software development, tool use, and business support. However, performance did not improve on every evaluation item, which also suggests that some capabilities may have declined with the move to the full version.

This tally is based on the "Evaluation Results" published in the official GitHub repository as of October 17, 2026, extracting items for which both the full and Preview versions have recorded values and comparing their scores. Three items for which one of the results is not listed â DeepSWE, Harvey LAB, and OfficeQA Pro â are excluded from the tally. We also avoided treating unlisted results as zero and averaging scores measured with different metrics. The main improved items, and those that were flat or declined, are as follows.

| Benchmark | Preview | Full version | Difference | 
|---|---|---|---|
| JobBench | 50.3 | 57.5 | +7.2 | 
| WideSearch | 81.9 | 84.6 | +2.7 | 
| PaperBench | 87.0 | 89.6 | +2.6 | 
| SWE-bench Pro | 59.6 | 61.7 | +2.1 | 
| Harbor-Index | 17.3 | 17.3 | 0 | 
| BrowseComp | 92.5 | 91.3 | â1.2 | 
| NL2Repo-Bench | 51.4 | 50.0 | â1.4 | 

*The difference is the Preview value subtracted from the full-version value; it does not represent an improvement rate.*

The largest gain is on JobBench, which evaluates tasks close to real-world work. The score rose 7.2 points, from 50.3 for the Preview version to 57.5 for the full version, and Pandaily also highlights it as one of the notable improvements. However, the same official comparison table lists GLM 5.3 at 58.2 and Claude Opus 5 at 68.0, both above Atria Dawn's 57.5. Improving on the earlier version is clearly different from achieving the best performance in a comparison that includes competing models.

On the other hand, BrowseComp fell 1.2 points, from 92.5 to 91.3, and NL2Repo-Bench fell 1.4 points, from 51.4 to 50.0. This shows that the update to the full version did not improve every capability uniformly. Nor can an improvement on 17 of 20 items be interpreted to mean an 85% chance of better performance in actual work. When considering adoption, it is more appropriate to choose benchmarks close to the work you want to delegate and focus on comparing those results.

These figures are evaluation results published by the developer, not the results of independent verification by a third party. The published tables and explanations also do not make it possible to confirm whether conditions such as tool configuration, number of attempts, and compute usage are consistent across all comparison targets. The improvement on 17 items should therefore be understood strictly as a tally of score changes announced by the developer.

## How to put 512K of context to use in real work

Shanghai AI Lab lists four main application areas for Atria Dawn: Discovery, Creation, Delivery, and Cybersecurity. Discovery refers to collecting and organizing information and evidence and turning research questions into actionable plans, while Creation refers to developing software, machine learning systems, applications, and the like. Delivery is work that completes reports, presentations, and similar outputs from documents, data, and design requirements, and Cybersecurity targets vulnerability testing, remediation, and re-verification in authorized environments. All of these are areas that cannot be completed by answering a single question and require carrying out multiple tasks in sequence.

What these uses have in common is a flow in which intermediate results are checked and the next step is decided accordingly. In software development, for example, it is not enough to write code; one must check the execution results, investigate the cause of any errors, fix them, and test again. In research as well, the outcome of each stage, from information gathering to hypothesis consideration, experiment execution, and result analysis, must feed into the next decision. In the official GitHub repository, Shanghai AI Lab states as a development goal that such work should lead to "executable, verifiable, and reproducible results." However, this describes the model's design philosophy and is not a guarantee that it can complete every task correctly.

A large context of 512K tokens may be useful in such multi-step work. If more reference materials, code, and past trial results can be held in a single context, it becomes easier to continue work while referring to what has happened along the way. However, an increase in context length is not synonymous with improved capability as an agent. Using the API alone does not automatically set up a code execution environment or integration with external tools; the actual tool execution is handled by the application or agent side, which needs a mechanism to return the results to the model. Only with the ability to retain long histories, plus a mechanism for choosing appropriate tools and verifying execution results, can the model be applied to complex work.

There are also constraints on input format. Atria Dawn accepts text only; it is not a multimodal model that can read images or PDF files directly and understand their contents. When analyzing papers or technical reports, the necessary information must first be extracted as text and passed to the model. Even with the context length expanded to 512K, information contained in images and figures still cannot be understood directly, so how materials are preprocessed also affects the quality of the final output.

The Atria Dawn API supports three formats â Chat Completions, Messages, and Responses â and can be used from existing applications and AI agents. When considering adoption, it is important to measure the accuracy of responses and generated code, instruction-following, response time, token usage, and so on using the actual work you want to delegate. When using long contexts in particular, it is necessary to confirm not only whether large amounts of information can be input, but also whether the necessary information is referenced correctly and consistent results are obtained throughout the work.

Once the full version's weights are released, running it in your own environment, in addition to using it through the API, becomes a realistic option. However, that decision requires checking specifications such as the context length at distribution, required computing resources, and inference speed. With this full release, the API context length has expanded to 512K tokens, and the developer's own evaluations report improvements on many items. How far that performance carries over to actual research and development work, however, remains to be verified. Along with the release of the full version's weights, accuracy and processing performance when handling long contexts will be key factors in judging Atria Dawn's practical value.
