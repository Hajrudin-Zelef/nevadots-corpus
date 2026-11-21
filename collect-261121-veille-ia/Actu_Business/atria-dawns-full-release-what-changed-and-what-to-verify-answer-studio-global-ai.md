---
id: collect-261121-veille-ia/actu-business/atria-dawns-full-release-what-changed-and-what-to-verify-answer-studio-global-ai
title: "Atria Dawn’s Full Release: What Changed, and What to Verify | Answer | Studio Global AI"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-10-17"]
keywords: ["full", "release", "preview", "atria", "model", "context", "available", "report"]
source: https://www.studioglobal.ai/discover/answers/what-did-shanghai-artificial-intelligence-laboratory-announce-6ae06d234ab47242a38f4689
source_anchor: ""
source_lines: [1, 43]
section: "Actu_Business"
---
Research answer

Shanghai Artificial Intelligence Laboratory announced Atria Dawn’s full release on October 17, 2026, describing a 744-billion-parameter mixture-of-experts model built on GLM-5.2 for research and engineering tasks that involve understanding an environment, using tools and completing multiple steps.21 The announcement advertised a 512K-token context window and improvements over the September Preview.2144

The main caveat is that an advertised context length is not necessarily the limit available through every deployment. The supplied sources also differ on when weights became available: launch-day reporting said they were still pending, while a later report said the full and FP8 weights had appeared on Hugging Face.21

Atria Dawn is presented as an agent that can move from an open-ended task toward a verifiable result. Its described workflow includes analyzing a problem, planning an approach, calling tools, implementing code, running experiments, examining results and recovering from failures.21

The Preview model card organizes related work into Discovery, Creation, Delivery and Cybersecurity. These labels broadly cover finding and organizing evidence, building artifacts, completing professional deliverables and security work. The supplied evidence does not independently establish that the full release has a different modality from Preview; one source describes Preview as text-input-only.3539

The Atria Dawn repository lists a 512K context window for the full release, while the Preview download table lists 256K for its standard and FP8 versions.2124 A report about the October 17 launch also described the hosted API as supporting up to 512K tokens.8

These figures should not be treated as a guarantee for every way of using the model. Before building around a long context, check the selected API or inference setup for its current context limit, output limit and model identifier. The supplied sources do not establish that every downloadable configuration and hosted endpoint has identical limits.

The release materials report gains over Preview on several tasks. They also show that not every benchmark moved in the same direction. These are published model results, not independent replications in the evidence available here.2344

| Benchmark | Full release | Preview | Reported change | 
|---|---|---|---|
| JobBench | 57.5 | 50.3 | +7.2 points2344 | 
| WideSearch | — | — | +2.7 points44 | 
| PaperBench | 89.6 | 87.0 | +2.6 points2344 | 
| AutomationBench | 54.7 | 53.8 | +0.9 points23 | 
| SkillsBench | 67.8 | 66.4 | +1.4 points23 | 
| SWE-bench Pro | 61.7 | 59.6 | +2.1 points23 | 
| BrowseComp | 91.3 | 92.5 | −1.2 points23 | 

The table suggests improvement on most of these measures, but BrowseComp is a counterexample: its listed score is lower for the full release than for Preview. The source excerpts do not provide the WideSearch scores themselves, so only the reported point change is shown.

At launch, reporting said the full weights were still coming soon. A report published later in October said the full and FP8 models had subsequently appeared on Hugging Face.21 The repository lists Hugging Face and ModelScope links for the full release, but the supplied later availability report confirms Hugging Face—not a later publication on ModelScope.211

For hosted use, the repository documents an API model identifier and a China-region endpoint; an API page describes support for Chat Completions, Messages and Responses.2443 Check the current provider documentation before integrating, since the available evidence does not establish current quotas or service limits.

The Preview repository links to standard and FP8 weights on Hugging Face and ModelScope and lists 256K context for both.24 The available excerpts are not sufficient to verify the precise full-release licensing terms or the contents and requirements of setup guides for SGLang, vLLM and Codex. In particular, do not infer the weight license from a code license without checking the applicable model repository.

Studio Global AI

This page includes a source-backed answer you can continue inside Studio Global.

Shanghai AI Lab announced Atria Dawn’s full release on October 17, 2026, advertising a 512K token context window and gains over Preview.

Shanghai AI Lab announced Atria Dawn’s full release on October 17, 2026, advertising a 512K token context window and gains over Preview. The model is built on a 744 billion parameter GLM 5.2 mixture of experts base and is designed for multi step research and engineering work, including tool use and recovery from failures.[21]

The full weights were reported as pending on launch day and later reported on Hugging Face; the evidence provided does not confirm later full weight availability on ModelScope.[2][1]
