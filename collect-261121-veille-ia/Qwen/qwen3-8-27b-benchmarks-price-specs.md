---
id: collect-261121-veille-ia/qwen/qwen3-8-27b-benchmarks-price-specs
title: "Qwen3.8 27B Benchmarks, Price & Specs"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-15"]
keywords: ["independent", "source", "active", "qwen3", "index", "reasoning", "benchmarks", "evaluations"]
source: https://www.trustthebench.com/models/qwen-qwen3-8-27b
source_anchor: ""
source_lines: [1, 99]
section: "Qwen"
---
## Measured results · Artificial Analysis

Independent evaluations of Qwen3.8 27B (Xhigh configuration). Values are reported as published; nothing here is estimated or derived. Snapshot 2026-11-15.

- Intelligence Index
- 20.1
- Cost to run the Index
- $3.53
- Time per task
- 42.8 min
- Omniscience index
- -6.4
- Reasoning model
- Yes
- Cached-input price
- $0.1 / 1M

### Reasoning & knowledge

- AA-Omniscience Accuracy17.3%
- CritPt5.4%
- CritPt v2 (Hard)4.0%
- GPQA Diamond90.5%
- HLE-Diamond15.3%
- Humanity's Last Exam33.9%

### Coding & terminal agents

- FrontierCode 1.134.2%
- FrontierCode-AA 2.014.9%
- SciCode46.6%
- Terminal-Bench 2.179.8%
- Terminal-Bench 4.05.6%
- Terminal-Bench-Science 0.10.5%

### Agentic work

- AA-Briefcase v1.1 (Elo)1391 Elo
- AutomationBench-AA48.2%
- EnterpriseOps-Gym-AA44.2%
- GDPval-AA (normalized Elo)1426 Elo
- τ³-Banking48.0%

### Long context & documents

### Reasoning-effort variants evaluated

| Variant | Intelligence Index | Cost to run Index | Output speed (tok/s) | 
|---|---|---|---|
| Xhigh | 20.1 | $3.53 | 42.8 | 
| Medium | 17.5 | $2.16 | 44.9 | 
| Low | 15.7 | $1.91 | 44.1 | 
| Non-reasoning | 10.0 | $3.77 | 40.9 | 

## Headline Evaluation Breakdown

Six independently measured evaluations (Artificial Analysis)
## Sourced Benchmark Evaluations (20)

Verified performance across authoritative benchmarks with provenance tracking.
| Benchmark | Status | Score | Trust | Date | Source Type | Provenance | 
|---|---|---|---|---|---|---|
| CritPt | Active | **5.4%** | 93 | 2026-11-15 | independent | Source ↗ | 
| EnterpriseOps-Gym-AA | Active | **44.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AutomationBench-AA | Active | **48.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| MLCR-AA (Medical Long Context Reasoning) | Active | **22.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| GDP.pdf | Active | **16.4%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode-AA 2.0 | Active | **14.9%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-Omniscience (Non-Hallucination) | Active | **71.4%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-Omniscience (Accuracy) | Active | **17.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| HLE-Diamond | Active | **15.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| CritPt v2 (Hard) | Active | **4%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode | Active | **34.2%** | 98 | 2026-11-15 | independent | Source ↗ | 
| AA-LCR (Long Context Reasoning) | Active | **82%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench-Science 0.1 | Active | **0.5%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench 4.0 | Active | **5.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench 2.1 | Active | **79.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| τ³-Banking | Active | **48%** | 93 | 2026-11-15 | independent | Source ↗ | 
| SciCode | Active | **46.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| MMMU-Pro | Active | **76.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Humanity's Last Exam | Active | **33.9%** | 93 | 2026-11-15 | independent | Source ↗ | 
| GPQA | Nearing Saturation | **90.5%** | 78 | 2026-11-15 | independent | Source ↗ | 

```
# 1. Pull and execute model locally with Ollama
ollama run qwen-qwen3-8-27b
# 2. Or invoke via local OpenAI-compatible endpoint
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-qwen3-8-27b",
    "messages": [{"role": "user", "content": "Analyze reasoning chains on AIME 2026"}]
  }'
```
## Evaluation & Sourcing Notes

Scores listed for **Qwen3.8 27B** represent verified evaluations extracted from official research papers, independent evaluation suites (HELM, LMSYS, OpenCompass, LiveBench), and verified audit reports.

All benchmarks marked as *saturated* or *deprecated* reflect historical performance where the benchmark no longer provides active discriminative power.
