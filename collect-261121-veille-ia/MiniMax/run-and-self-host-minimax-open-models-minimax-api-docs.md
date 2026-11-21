---
id: collect-261121-veille-ia/minimax/run-and-self-host-minimax-open-models-minimax-api-docs
title: "Run and self-host MiniMax Open Models - MiniMax API Docs"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "page", "runtime", "open", "model", "models", "weights", "run"]
source: https://platform.minimax.io/docs/guides/local-deploy
source_anchor: ""
source_lines: [1, 16]
section: "MiniMax"
---
**self-hosted deployment**on servers or clusters. The H3 page also provides a ComfyUI local-workflow path. You provide the hardware, operate the runtime, configure access control when applicable, and implement content safeguards. For a managed service, elastic scaling, or the complete MiniMax Platform feature set, use the MiniMax API.

- **Stable** : Uses a fixed runtime version and provides a reproducible reference command.
- **Preview** : An official deployment path is available, but the upstream installation method or some capabilities are still changing.
- **Experimental** : Depends on a prerelease runtime, or some hardware combinations have not completed end-to-end validation.

- You are responsible for capacity planning, scaling, monitoring, recovery, and upgrades.
- Quickstarts listen only on the local machine by default. Before exposing a service, configure authentication, TLS, network isolation, and remote-media access restrictions.
- Open weights do not include MiniMax Platform managed files, caching, content safeguards, or platform-only workflows.
- Community quantizations, converted weights, and other inference frameworks are outside this section’s validated scope unless a model page explicitly lists them.

1. Read the target model’s License, open-release scope, and verified capabilities.
2. Prepare the GPU, host memory, disk, driver, and CUDA environment specified on the model page.
3. Pin the model revision, inference runtime version, or image digest. Do not let production deployments drift with `main` ,`dev` , or`latest` .
4. Prepare a Hugging Face cache or offline weights. For mirrors in mainland China, use only the official source listed on the model page.
5. Complete the health check and minimal request before applying quantization, parallelism, offloading, or throughput tuning.
