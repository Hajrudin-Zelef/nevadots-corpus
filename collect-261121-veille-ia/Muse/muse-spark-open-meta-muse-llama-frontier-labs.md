---
id: collect-261121-veille-ia/muse/muse-spark-open-meta-muse-llama-frontier-labs
title: "Muse Spark Open - Meta (Muse / Llama) | Frontier Labs"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["muse", "model", "context", "files", "llama", "memory", "real", "mlx-community"]
source: http://krakowiak.eu/frontier-labs/meta/muse-spark-open-816b-a42b
source_anchor: ""
source_lines: [1, 63]
section: "Muse"
---
- released
- 2026-10-15
- params
- 816B-A42B MoE
- context
- 1M
- weights
- -
- license
- conditional: Muse Model License

## Where it runs

Best quantization that fits each memory size, how much is left for context, and how many GB the model reads per generated token (fewer = faster on a Mac, set by active parameters).

| memory | best quant | weights | left for context | GB per token | 
|---|---|---|---|---|
| 128 GB | does not fit |  |  |  | 
| 256 GB | does not fit |  |  |  | 
| 512 GB | Q3 tight, quality drops | 392 GB | 88 GB | 18.4 | 
| Multi-GPU | Q8 | 898 GB | spread over several GPUs or machines | 44.6 | 

Estimates from the parameter count with a margin for the KV cache (Q4 about 0.6 GB per billion parameters). Real files differ by a few percent.

GGUF and MLX files for this model on Hugging Face, from the lab and known quantizers, with real file sizes. Leave about 10% of memory for context.

| Repo | Files | Downloads | 
|---|---|---|
| unsloth/Muse-Spark-Open-816B-A42B-GGUF ↗ | UD-IQ1_S 175.6 GB UD-IQ1_M 208.9 GB UD-IQ2_XXS 225.6 GB UD-IQ2_S 275.5 GB UD-Q2_K_XL 311.2 GB UD-IQ3_XXS 340.4 GB UD-Q3_K_XL 401.5 GB UD-Q4_K_XL 505.5 GB | 10,397 | 
| mlx-community/Muse-Spark-Open-816B-A42B-4bit ↗ | 4-bit g64 532.8 GB | 5,816 | 
| ggml-org/Muse-Spark-Open-816B-A42B-GGUF ↗ | Q2_K 304.1 GB Q4_K_S 470.1 GB Q8_0 871.6 GB | 3,705 | 
| mlx-community/Muse-Spark-Open-816B-A42B-oQ3e ↗ | 3-bit mixed g64 371.5 GB | 1,633 | 
| mlx-community/Muse-Spark-Open-816B-A42B-oQ4e ↗ | 4-bit mixed g64 477.6 GB | 931 | 
| mlx-community/Muse-Spark-Open-816B-A42B-3bit ↗ | 3-bit g64 436.1 GB | 718 | 

Greyed out = bigger than a 512 GB Mac can hold. Files marked ik_llama.cpp need that fork and will not load in llama.cpp, LM Studio or Ollama.

## Benchmarks

Individual results from Artificial Analysis (the evals behind the AA index), best measured variant of the model. Higher is better.

- 
                            Humanity's Last Exam**43.9%**Very hard expert questions across many fields, biology and medicine included.
- 
                            GPQA Diamond**89.0%**Graduate-level biology, chemistry and physics questions that are hard to look up.
- 
                            Knowledge**42.0%**Share of factual questions the model answers correctly from memory (AA-Omniscience).
- 
                            Says "I don't know"**72.0%**When it does not know the answer, how often it admits it instead of making something up. Higher = less hallucination.
- 
                            Long-context reasoning**78.3%**Reasoning over very long documents.
- 
                            Vision (MMMU-Pro)**80.9%**Questions that need reading images, charts and diagrams.
- 
                            SciCode**58.5%**Writing code for real scientific research problems.
- 
                            CritPt**14.9%**Research-level physics reasoning.
- 
                            Omniscience index**25.8**Knowledge minus hallucination, from -100 to 100. Rewards models that know a lot and do not guess.

Source: Artificial Analysis, updated 2026-11-05.

Numbers come from model cards and config files. The ~Q4 size is an estimate (about 0.6 GB per billion params), real usage depends on the quant and context.
