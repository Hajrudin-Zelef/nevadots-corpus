---
id: collect-261121-veille-ia/glm-zhipu/shanghai-ai-lab-ships-atria-dawn-full-release-744b-moe-glm-5-2-agentic-model-context-doubled-to-512k
title: "Shanghai AI Lab ships Atria Dawn full release: 744B MoE GLM-5.2 agentic model, context doubled to 512K, MIT weights — AICoder"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["full", "glm-5", "context", "atria", "dawn", "release", "model", "shanghai"]
source: https://aicoder.com/news/news-20261027-shanghai-ai-lab-atria-dawn-full-release-512k-mit
source_anchor: ""
source_lines: [1, 45]
section: "GLM_Zhipu"
---
Shanghai AI Laboratory has moved Atria Dawn from Preview to a full release, with open weights (plus an FP8 build) on Hugging Face under internlm/Atria-Dawn and on ModelScope, MIT licensed. Built on the 744B-parameter MoE GLM-5.2, it targets long-horizon research and engineering agent work; context doubles from the Preview's 256K to 512K and input is text only. The vendor table shows it leading the listed models on AutomationBench, CyberGym and SkillsBench, while still trailing Claude Opus 5 on coding tests such as SWE-bench Pro and Terminal-Bench 2.1 (all vendor-reported).

## Key Takeaways

- ✓512K context, double the Preview's 256K; built on the 744B-parameter MoE GLM-5.2, text input only
- ✓BF16 and FP8 weights on Hugging Face and ModelScope, MIT license
- ✓Vendor-reported: AutomationBench 54.7, CyberGym 86.8, SkillsBench 67.8, BFCL V4 78.4, each the top score in its table
- ✓Weak spots from the same table: SWE-bench Pro 61.7 vs 76.5 for Claude Opus 5, Terminal-Bench 2.1 81.0 vs 90.2
- ✓Local serving needs SGLang v0.5.13.post1+ or vLLM v0.23.0+; an OpenAI-compatible API also plugs into Codex

### Key Decision Metrics at a Glance

#### Turn your technical choice into a development budget

Compare 40 dev plans & simulate token costs vs $20/mo subscriptions

### Project Links & Resources

Direct Access
## In-Depth Technical Analysis

### Background and the problem

Shanghai AI Laboratory open-sourced Atria Dawn Preview in mid-September as a long-horizon agent model meant to turn research questions into verifiable results. Open-weight agent models have tended to struggle with context length and with keeping multi-step tool use stable over long experiments, large reading loads or full engineering runs. The full release is now on Hugging Face (internlm/Atria-Dawn, repo created October 23) and ModelScope, which announced it on X on October 27. The license stays MIT.

### Architecture and how it works

Per the official model card, Atria Dawn sits on the 744B-parameter MoE GLM-5.2 and covers four areas: retrieval and deep research, building software and apps, turning documents and data into reports, and security analysis in authorized environments. It is built around a full loop of analysis, planning, tool use, coding, running experiments, reading results and recovering from failures. Context rises from 256K to 512K, input is text only, and an FP8 build ships alongside. The technical report is arXiv 2609.15818.

### Benchmarks

All figures are vendor-reported. The full release posts the top score in its table on AutomationBench (54.7), CyberGym (86.8), SkillsBench (67.8), WideSearch (84.6) and BFCL V4 (78.4), against DeepSeek V4 Pro, Kimi K3, Qwen 3.8 Max, GLM 5.3, GPT 5.6 sol and Claude Opus 5. Coding is clearly weaker: SWE-bench Pro 61.7 versus 76.5 for Claude Opus 5, Terminal-Bench 2.1 81.0 versus 90.2, and NL2Repo-Bench 50.0, slightly below the Preview's 51.4. No independent third-party results have been published yet.

### Getting started

For local serving the card recommends SGLang v0.5.13.post1+ or vLLM v0.23.0+ using the GLM-5.2 recipes; at over 700B parameters this needs a multi-GPU server, and the FP8 build eases memory. The hosted API is OpenAI-compatible at discovery-api.intern-ai.org.cn/v1 with model Atria-Dawn, and the card includes a Codex config.toml custom-provider snippet. It looks best suited to research automation, office deliverables and security validation agents; for pure coding, closed frontier models still lead.

Benchmark side-by-side against alternatives, or calculate monthly token cost vs subscription break-even.

## Discussion & Comments

0
Sign in to join the discussion

Connect with AI developers to exchange benchmark insights.
