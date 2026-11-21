---
id: collect-261121-veille-ia/divers-tech/ai-developer-digest-tue-aug-11-2026
title: "AI Developer Digest — Tue, Aug 11, 2026"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-08-11"]
keywords: ["github", "ollama", "model", "releases", "code", "token", "llama", "developer"]
source: https://dev.yogeashnehra.com/digest/2026-08-11
source_anchor: ""
source_lines: [1, 59]
section: "Divers_Tech"
---
## Breaking Changes

2
## Model Releases

1
## API & SDK Changes

Nothing cleared the quality gate for a full entry this period. openai-python v2.54.0 added "new Responses model identifiers" (specific model IDs not published in release notes) — too thin on developer signal for a main entry; see Quick Hits.

## Research

Nothing cleared the quality gate this period. arXiv cs.AI and cs.CL feeds remain egress-blocked in this environment. No recognized-lab papers with code and measurable benchmark numbers from August 10–11 were confirmed via search.

## Tooling

4
## Benchmarks & Leaderboards

1
## Trends & Emerging Tech

1
## Technical Discussions

Nothing cleared the quality bar this period. No Hacker News threads with score >200 on technical AI topics from August 10–11 surfaced via search.

## Quick Hits

- **Ollama v0.32.8 (Aug 11)** — Muse Glimmer expanded from Apple Silicon-only to all platforms: NVIDIA (CUDA), AMD (ROCm), and others.`ollama pull muse-glimmer` now works on non-Apple-Silicon hardware. https://github.com/ollama/ollama/releases/tag/v0.32.8
- **Ollama v0.32.9 (Aug 11)** — Added NVIDIA Nemotron 3.5 Lightning architecture support (`ollama pull nemotron-3.5-lightning` ); fixed a boundary condition in the Muse Glimmer function calling parser that could silently produce malformed tool calls. https://github.com/ollama/ollama/releases/tag/v0.32.9
- **Claude Code patch (Aug 11)** — Fixed Bash commands silently failing under`claude-code-action` with`allowed_non_write_users` on GitHub-hosted runners (any team using Claude Code in GitHub Actions CI with restricted permissions was affected); fixed feature flags not reading subscription tier on expired login token (could wrongly prompt Max plan users to enable usage credits for Fable). Update Claude Code to pick up the CI fix. https://releasebot.io/updates/anthropic/claude-code
- **MAI-Code-1.1-Flash in GitHub Copilot (Aug 11)** — Microsoft's updated small-tier coding model rolling out in Copilot; adds native vision support (image understanding) and 73% lower list price than MAI-Code-1-Flash ($0.75/M → ~$0.20/M input); Copilot auto-routes lightweight coding tasks to it; 0.25× premium request multiplier for annual subscribers. https://github.blog/changelog/2026-08-11-mai-code-1-1-flash-available-in-github-copilot/
- **llama.cpp b10355 (Aug 10, 23:17 UTC)** — Multi-output backend sampling with token speculation; fixes CPU/GPU sampling consistency and Vulkan backend sampling compatibility. This is the backend sampler path for speculative decoding — separate from model-level speculative decoding. https://github.com/ggml-org/llama.cpp/releases/tag/b10355
- **llama.cpp b10356 (Aug 11)** — ROCm upgraded from 7.2.1 to 7.14 across Linux and Windows builds using new multi-arch wheel distribution. AMD GPU users on llama.cpp should rebuild to pick up ROCm 7.14 improvements. https://github.com/ggml-org/llama.cpp/releases/tag/b10356
- **GitHub Copilot: per-model token breakdown in usage reports (Aug 11)** — AI usage report now shows input, output, cache read, and cache write token counts per model alongside AI credits consumed. Useful for diagnosing which models are driving cache hit rates and where prompt caching is effective. https://github.blog/changelog/2026-08-11-per-model-token-breakdown-in-the-usage-report/
- **vLLM v0.27.1 (Aug 11)** — Patch on top of v0.27.0: "Support quantized DSpark Markov heads" (PR #50424). No other changes in this release. https://github.com/vllm-project/vllm/releases/tag/v0.27.1
- **openai-python v2.54.0 (Aug 11)** — New Responses model identifiers added (specific model IDs not published in release notes); audio upload metadata requirements clarified. https://github.com/openai/openai-python/releases/tag/v2.54.0

## Worth Watching (Announced, Not Yet Shipped)

| Item | ETA | Notes | 
|---|---|---|
| **Claude Code auto mode → default** | **August 14, 2026 — 3 days** | Auto mode becomes the default permission mode for Pro, Max, and Team plans. Classifier tokens no longer charged on those plans. Disable in Claude Code settings before August 14 if you prefer manual review. Enterprise/API/platform rollout follows "within the coming month." | 
| **Qwen3.8-Max open weights** | **Now 8 days overdue** | Alibaba promised open weights the week of August 10. As of August 11: no repo on Hugging Face or ModelScope, no new release date, license terms still unannounced. Watch huggingface.co/Qwen and modelscope.cn. | 
| **Grok 4.6 API endpoint** | **~4 days overdue** | Consumer launch confirmed ~August 7. `grok-4-6` still absent from xAI public API catalog. Community window estimate: August 10–16. Watch api.x.ai/docs/models. | 
| **Anthropic Workbench sunset + experimental prompt tools retirement** | **August 17, 2026 — 6 days** | `/v1/experimental/generate_prompt` ,`improve_prompt` ,`templatize_prompt` will error. Export saved prompts from the Console banner before this date. | 
| **Google Imagen 4 API shutdown** | **August 17, 2026 — 6 days** | Migrate from `client.models.generate_images()` to`client.models.generate_content()` ;`number_of_images` parameter removed. Target: Gemini 3.1 Flash Image. | 
| **OpenAI Assistants API shutdown** | **August 26, 2026 — 15 days** | `/v1/assistants` ,`/v1/threads` ,`/v1/runs` will fail. Migrate to Responses API + Conversations API — architecture change required. | 
| **OpenAI o3 retirement** | **August 26, 2026 — 15 days** | Migrate to GPT-5.6 Sol/Terra/Luna. | 
| **DALL·E GPT retirement from ChatGPT** | **August 30, 2026 — 19 days** | Download images before this date. Use ChatGPT Images going forward. | 
| **GPT-5.4 / GPT-5.4 mini retirement from Codex (sign-in)** | **August 31, 2026 — 20 days** | API key–authenticated Codex sessions unaffected. | 
| **Gemini Robotics ER 1.6 Preview shutdown** | **August 31, 2026 — 20 days** | Migrate to `gemini-robotics-er-2-preview` . | 
| **llama.cpp default server port change: 8080 → 9931** | **Upcoming (no date set)** | Migration notice merged in b10240; update docker-compose, reverse-proxy configs, and hardcoded port references now. | 
| **Google Gemini temperature/top_p/top_k — silent ignore → hard error** | **Future model gen** | Currently silently ignored; will return HTTP 400 on future Gemini model generations. Strip these parameters now. | 
| **Grok 4.7** | **Late August / early September 2026** | 2.1T parameter model; xAI describes as "all-around better than 4.6 but slightly slower to serve." | 
| **EU AI Act Article 50 — Watermarking enforcement** | **December 2, 2026** | C2PA + SynthID are the de facto standard stack. | 

Filtered from 30+ primary sources against a published quality rubric. No press releases, no fluff — only what changes what you build.
