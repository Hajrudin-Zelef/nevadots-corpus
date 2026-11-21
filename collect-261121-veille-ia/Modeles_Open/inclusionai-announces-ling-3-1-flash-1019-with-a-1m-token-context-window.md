---
id: collect-261121-veille-ia/modeles-open/inclusionai-announces-ling-3-1-flash-1019-with-a-1m-token-context-window
title: "inclusionAI announces Ling-3.1-flash-1019 with a 1M-token context window"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["authors", "model", "ling-3", "against", "news", "inclusionai", "flash-1019", "m-token"]
source: https://tensorjournal.com/en/news/inclusionai-ling-flash-mixture-experts-mit
source_anchor: ""
source_lines: [1, 30]
section: "Modeles_Open"
---
## Key facts

- Organization
- Model authors (inclusionAI/Ling-3.1-flash-1019)
- Event
- Other
- Date

The authors of Ling-3.1-flash-1019, a mixture-of-experts language model from inclusionAI, announced it on October 27, 2026. The authors say it has 560B parameters in total, with 25B active per token, and a 1M-token context window.

Each token activates 8 of the model's 512 routed experts. The authors describe a transformer layer mix of 63 KDA + 9 Gated MLA (7:1) and a vocabulary of 157,184 tokens.

The AA-Briefcase score of 1,524 Elo is company-reported: the authors claim it, against approximately 1400 Elo for their predecessor.

The authors also claim 68% against 62% on AutomationBench-AA, 26% against 18% on CritPt, and 57% against 54% on SciCode, with the predecessor's score given for each measure. On the Artificial Analysis Intelligence Index v4.3.2, they claim 44 points against 41 points for the predecessor.

The authors say the day-0 SGLang image, identified as day-0, includes support for Ling-3.1-flash, MTP (NEXTN) speculative decoding and Hopper decode optimizations.

For vLLM use specifically, the authors recommend a set of sampling settings, including enabling the model's thinking mode, and claim the settings give better performance.

The authors list the licence as MIT. They say their training-content summary covers disclosure of training content only and does not replace the technical documentation, usage terms or licence.

## Related

- News A Hugging Face blog post published October 26, 2026 pairs a DeepSeek-V4.1-Flash-1017 lead agent with Qwen3.5-9B workers and reports full runs costing between $0.87 and $3.60.
- News The model authors' SR-SWA design is not independently verified, and their benchmark results are company-reported. A demo is available on Hugging Face Spaces.
- News According to its authors, the guide is a hands-on walkthrough rather than a benchmark, with the model served locally and Ollama's cloud features disabled.
- News NVIDIA says all three optimizations reach 9.72 ms TPOT and 102.88 tok/s decode throughput in a synthetic DGX Spark workload. The figures are company-reported.
- News Meta says 38.4% of 2026 respondents selected AI-code reliability as a reason to use type hints, against 18.5% in its 2025 survey. The survey drew 1,794 responses.
- News Google DeepMind announced Genie 4 on October 27, 2026; first access goes to Google AI Ultra subscribers in Project Genie and to early partners of the Gemini API and Google AI Studio.
