---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-benchmarks-price-specs
title: "GPT-6.1 Terra Benchmarks, Price & Specs"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-15"]
keywords: ["independent", "source", "active", "gpt-6", "terra", "index", "benchmarks", "evaluations"]
source: https://www.trustthebench.com/models/openai-gpt-6-1-terra
source_anchor: ""
source_lines: [1, 95]
section: "GPT_OpenAI"
---
## Measured results · Artificial Analysis

Independent evaluations of GPT-6.1 Terra (Max configuration). Values are reported as published; nothing here is estimated or derived. Snapshot 2026-11-15.

- Intelligence Index
- 51.3
- Cost to run the Index
- $0.96
- Time per task
- 14.1 min
- Omniscience index
- 29.6
- Reasoning model
- Yes
- Cached-input price
- $0.05 / 1M

### Reasoning & knowledge

- AA-Omniscience Accuracy47.6%
- CritPt28.0%
- CritPt v2 (Hard)68.0%
- HLE-Diamond47.3%
- Humanity's Last Exam46.3%

### Coding & terminal agents

- FrontierCode 1.136.0%
- FrontierCode-AA 2.042.3%
- SciCode55.1%
- Terminal-Bench 4.055.6%
- Terminal-Bench-Science 0.157.6%

### Agentic work

- AA-Briefcase v1.1 (Elo)1401 Elo
- AutomationBench-AA60.2%
- GDPval-AA (normalized Elo)1476 Elo
- Multimodal Knowledge Agent (MKA)43.8%

### Long context & documents

### Reasoning-effort variants evaluated

| Variant | Intelligence Index | Cost to run Index | Output speed (tok/s) | 
|---|---|---|---|
| Max | 51.3 | $0.96 | 70.8 | 
| Xhigh | 45.3 | $0.40 | 68.1 | 
| High | 42.4 | $0.26 | 65.0 | 
| Medium | 38.2 | $0.17 | 63.7 | 
| Non-reasoning | 30.5 | $0.23 | 65.4 | 
| Low | 29.4 | $0.07 | 62.5 | 

## Headline Evaluation Breakdown

Six independently measured evaluations (Artificial Analysis)
## Sourced Benchmark Evaluations (18)

Verified performance across authoritative benchmarks with provenance tracking.
| Benchmark | Status | Score | Trust | Date | Source Type | Provenance | 
|---|---|---|---|---|---|---|
| HLE-Diamond | Active | **47.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| VIALS-AA | Active | **60.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AutomationBench-AA | Active | **60.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Multimodal Knowledge Agent (MKA) | Active | **43.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| MLCR-AA (Medical Long Context Reasoning) | Active | **30.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| GDP.pdf | Active | **28.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode-AA 2.0 | Active | **42.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-Omniscience (Non-Hallucination) | Active | **65.5%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-Omniscience (Accuracy) | Active | **47.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode | Active | **36%** | 98 | 2026-11-15 | independent | Source ↗ | 
| CritPt v2 (Hard) | Active | **68%** | 93 | 2026-11-15 | independent | Source ↗ | 
| CritPt | Active | **28%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-LCR (Long Context Reasoning) | Active | **82.7%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench-Science 0.1 | Active | **57.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench 4.0 | Active | **55.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| SciCode | Active | **55.1%** | 93 | 2026-11-15 | independent | Source ↗ | 
| MMMU-Pro | Active | **84.7%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Humanity's Last Exam | Active | **46.2%** | 93 | 2026-11-15 | independent | Source ↗ | 

```
from openai import OpenAI
client = OpenAI()
response = client.chat.completions.create(
    model="openai-gpt-6-1-terra",
    messages=[{"role": "user", "content": "Evaluate multi-step logic problem"}],
    temperature=0.2
)
print(response.choices[0].message.content)
```
## Evaluation & Sourcing Notes

Scores listed for **GPT-6.1 Terra** represent verified evaluations extracted from official research papers, independent evaluation suites (HELM, LMSYS, OpenCompass, LiveBench), and verified audit reports.

All benchmarks marked as *saturated* or *deprecated* reflect historical performance where the benchmark no longer provides active discriminative power.
