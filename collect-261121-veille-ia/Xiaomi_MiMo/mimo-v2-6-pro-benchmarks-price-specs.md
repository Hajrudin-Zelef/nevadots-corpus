---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-benchmarks-price-specs
title: "MiMo-V2.6-Pro Benchmarks, Price & Specs"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-11-15"]
keywords: ["independent", "source", "active", "mimo-v2", "reasoning", "benchmarks", "evaluations", "price"]
source: https://www.trustthebench.com/models/xiaomi-mimo-v2-6-pro
source_anchor: ""
source_lines: [1, 86]
section: "Xiaomi_MiMo"
---
## Measured results · Artificial Analysis

Independent evaluations of MiMo-V2.6-Pro. Values are reported as published; nothing here is estimated or derived. Snapshot 2026-11-15.

- Intelligence Index
- 29.9
- Cost to run the Index
- $0.26
- Time per task
- 36.1 min
- Omniscience index
- -4.5
- Reasoning model
- Yes
- Cached-input price
- $0.0036 / 1M

### Reasoning & knowledge

- AA-Omniscience Accuracy34.8%
- CritPt26.6%
- CritPt v2 (Hard)34.0%
- HLE-Diamond15.2%
- Humanity's Last Exam49.4%

### Coding & terminal agents

- FrontierCode 1.145.4%
- FrontierCode-AA 2.024.0%
- SciCode60.9%
- Terminal-Bench 4.034.8%
- Terminal-Bench-Science 0.15.7%

### Agentic work

- AA-AnalystAgent37.5%
- AA-Briefcase v1.1 (Elo)1512 Elo
- AutomationBench-AA58.6%
- GDPval-AA (normalized Elo)1692 Elo
- Multimodal Knowledge Agent (MKA)37.3%

### Long context & documents

## Headline Evaluation Breakdown

Six independently measured evaluations (Artificial Analysis)
## Sourced Benchmark Evaluations (18)

Verified performance across authoritative benchmarks with provenance tracking.
| Benchmark | Status | Score | Trust | Date | Source Type | Provenance | 
|---|---|---|---|---|---|---|
| AA-Omniscience (Accuracy) | Active | **34.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| VIALS-AA | Active | **43.5%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-AnalystAgent | Active | **37.5%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AutomationBench-AA | Active | **58.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Multimodal Knowledge Agent (MKA) | Active | **37.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| MLCR-AA (Medical Long Context Reasoning) | Active | **12.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| GDP.pdf | Active | **19.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode-AA 2.0 | Active | **24%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-Omniscience (Non-Hallucination) | Active | **39.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| FrontierCode | Active | **45.4%** | 98 | 2026-11-15 | independent | Source ↗ | 
| HLE-Diamond | Active | **15.2%** | 93 | 2026-11-15 | independent | Source ↗ | 
| CritPt v2 (Hard) | Active | **34%** | 93 | 2026-11-15 | independent | Source ↗ | 
| CritPt | Active | **26.6%** | 93 | 2026-11-15 | independent | Source ↗ | 
| AA-LCR (Long Context Reasoning) | Active | **86.3%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench-Science 0.1 | Active | **5.7%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Terminal-Bench 4.0 | Active | **34.8%** | 93 | 2026-11-15 | independent | Source ↗ | 
| SciCode | Active | **60.9%** | 93 | 2026-11-15 | independent | Source ↗ | 
| Humanity's Last Exam | Active | **49.4%** | 93 | 2026-11-15 | independent | Source ↗ | 

```
# 1. Pull and execute model locally with Ollama
ollama run xiaomi-mimo-v2-6-pro
# 2. Or invoke via local OpenAI-compatible endpoint
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "xiaomi-mimo-v2-6-pro",
    "messages": [{"role": "user", "content": "Analyze reasoning chains on AIME 2026"}]
  }'
```
## Evaluation & Sourcing Notes

Scores listed for **MiMo-V2.6-Pro** represent verified evaluations extracted from official research papers, independent evaluation suites (HELM, LMSYS, OpenCompass, LiveBench), and verified audit reports.

All benchmarks marked as *saturated* or *deprecated* reflect historical performance where the benchmark no longer provides active discriminative power.
