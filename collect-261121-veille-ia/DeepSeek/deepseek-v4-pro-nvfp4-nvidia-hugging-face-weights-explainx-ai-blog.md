---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-nvfp4-nvidia-hugging-face-weights-explainx-ai-blog
title: "DeepSeek V4 Pro NVFP4: Nvidia Hugging Face Weights | explainx.ai Blog"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["nvfp4", "nvidia", "weights", "hugging", "face", "deepseek", "august", "quantized"]
source: https://www.explainx.ai/blog/nvidia-deepseek-v4-pro-nvfp4-huggingface-quantized-2026
source_anchor: ""
source_lines: [1, 182]
section: "DeepSeek"
---
**Nvidia listed `nvidia/DeepSeek-V4-Pro-0813-NVFP4` on Hugging Face**, packaging **DeepSeek V4 Pro** weights in **NVFP4** for **lower-memory inference** on **supported GPUs**. The **model page** appeared on **Hacker News** in November 2026.

This is **not** a repeat of **August's V4 Pro launch**. The **Hugging Face card** dates to **August–September 2026**; **November HN** attention is what brings it back for builders who missed the **NVFP4** artifact.

## What changed versus August 2026

| Layer | August V4 Pro launch | Nvidia NVFP4 HF repo | 
|---|---|---|
| Announcement owner | **DeepSeek** product/API | **Nvidia** Hugging Face org | 
| Artifact | **API, pricing, features** | **Quantized weights** | 
| Goal | **Frontier coding model access** | **Efficient self-host inference** | 
| Precision | **Full precision serving** | **NVFP4 compressed weights** | 

Builders who only use **DeepSeek's API** can ignore this repo. Teams with **Blackwell-class** or other **FP4-capable** clusters should evaluate **VRAM savings**.

## NVFP4 in one paragraph

**NVFP4** is Nvidia's **4-bit FP** format for **tensor cores** that implement **FP4** paths. It trades **numerical precision** for **capacity and throughput**, similar in role to **Qwen3 NVFP4 multimodal releases** explainx.ai already covered.

Always run **your eval suite** after quantization — **coding**, **tool-use**, and **long-context** tasks degrade differently.

## How to try the weights

1. Open **huggingface.co/nvidia/DeepSeek-V4-Pro-0813-NVFP4** and read**hardware requirements** .
2. Match **driver, CUDA, and inference server** versions Nvidia documents for**FP4** .
3. Compare **latency and quality** against**GLM NVFP4 vLLM guides** patterns if you use**vLLM** .
4. Keep **August API** path as fallback when**quantized self-host** fails tasks.

## Community harness context

explainx.ai's **J-Space Cognition Suite post** discussed **community harnesses** around **V4 Pro**. A **quantized checkpoint** may change **harness scores** — rerun before you trust **leaderboard claims**.

## Export-control and licensing reminders

**DeepSeek** models carry **US policy** and **enterprise procurement** scrutiny covered in **China distillation reporting**. **Weights on Hugging Face** do not remove **compliance review** for **your jurisdiction**.

## When to skip this repo

- **No FP4 GPU** in your fleet.
- **You need exact-match API behavior** from**DeepSeek-hosted** endpoints.
- **Your task requires maximum precision** (some**finance** or**safety** evals).

## Hardware planning table (conceptual)

| Constraint | NVFP4 self-host | API | 
|---|---|---|
| CapEx GPUs | **High upfront** | **Low** | 
| Per-token opex | **Power + staff** | **Vendor bill** | 
| Privacy | **Strong if air-gapped** | **Policy dependent** | 
| Update cadence | **You merge weights** | **Vendor ships** | 

Quantized model weights and GPU memory footprint for NVFP4 DeepSeek V4 Pro inference

*Image: Library reference — quantized checkpoints demand disciplined load and eval scripts.*

## TensorRT and server stack notes

Nvidia typically pairs **NVFP4** artifacts with **TensorRT-LLM** or **vLLM** plugins documented in **TensorRT 11 guides**. Before you pin production, run **A/B** against **BF16** baselines on:

- **Multi-file repo edits** (V4 Pro's strength in**August launch** marketing)
- **JSON tool calls** with**strict schemas**
- **128k+ context** if the card supports it

Quantization surprises often appear only on **long-context** **needle** tests.

## Memory math (illustrative, not vendor quotes)

Exact **VRAM** depends on **parallelism** and **KV cache dtype**. Teams use **NVFP4** when **a single H100-class GPU** must host a **frontier coder** that **BF16** would **OOM**. Measure **your** batch size; do not extrapolate from **blog tables**.

## Security and supply chain

Download **only** from **`nvidia/` org** on Hugging Face. Verify **commit hashes** in **internal SBOMs**. **Hugging Face security tradeoffs** apply to **every** weight pull.

## Relation to Nvidia open-model recipes

See **Dynamo recipes for Korean open models** for how Nvidia bundles **reference inference** stacks. This **DeepSeek NVFP4** card is another **artifact** in that **ecosystem play**: make **frontier-class** names **runnable** on **Nvidia silicon**.

## Related reading

## Benchmark expectations after quantization

**MMLU**, **HumanEval**, and **SWE-bench** scores from **August marketing** assumed **full-precision** serving. Expect **small regressions** on **numeric-heavy** coding tasks when you move to **NVFP4**. Document **acceptance thresholds** before you flip traffic.

Run **regression suites** nightly if you **auto-pull** **Hugging Face** revisions — **Transformers integration contracts** posts show how **silent tokenizer fixes** break pipelines.

## Multi-GPU deployment sketch

Large **V4 Pro-class** models typically need **tensor parallel** across **multiple GPUs** even with **FP4**. Follow patterns from **GLM NVFP4 two-DGX guide**:

1. **Shard** weights across**NVLink** nodes.
2. **Pin****NVFP4** kernels in**vLLM** or**TRT-LLM** config.
3. **Warm up** with**representative batch** sizes.
4. **Monitor****KV cache** growth on**long agent loops** .

## Licensing and attribution

**DeepSeek** licenses and **Nvidia redistribution** terms live on the **model card**. **Enterprise legal** should confirm **redistribution** rights if you **embed weights** in **appliances** you **sell**.

## When Nvidia publishes more NVFP4 families

This repo landed beside **Qwen3 NVFP4** cards. Pattern suggests **Nvidia** will **NVFP4-wrap** every **frontier name** customers request on **Blackwell** — treat **HN front-page** as **ops signal**, not **product launch**.

## Cost worked example (hypothetical)

Suppose **BF16** needs **8 GPUs** and **NVFP4** needs **4** for the same **throughput target** on **your** harness. Savings are **hardware rent**, not **zero ops** — **quantization engineering** and **eval staff** still bill hours.

## FAQ for platform engineers

**Should we mirror weights internally?** Yes, for **air-gapped** **CI** that cannot hit Hugging Face every deploy.

**Does this replace Nemotron for coding?** Only if **your evals** say so — **Nemotron coding recipes** may still win on **latency**.

**Will DeepSeek API prices drop?** **Unrelated** to **Nvidia HF repo** — watch **DeepSeek pricing pages**, not **this artifact**.

## Historical context: August versus November artifacts

When **V4 Pro** launched, teams debated **API versus self-host** economics in **terminal bench posts**. **November's NVFP4 card** is the **self-host half** catching up — useful if **API rate limits** or **data residency** block you.

## Observability hooks

Log **per-request** **precision path** (**fp4 vs fallback**) in **your gateway**. When **quality tickets** arrive, you can tell whether **quantization** or **prompt drift** caused regression.

## Container and CI sizing

**Multi-hundred-GB** downloads affect **CI cache** budgets. Mirror to **internal artifact registry** and **pin sha256** in **Terraform** modules so **reproducible deploys** do not **re-pull** on every **pod restart**.

## Partner ecosystem

**CoreWeave**, **Lambda**, and **Nebius** may expose **managed V4 Pro NVFP4** before you **bare-metal** it. Compare **$/token** after **staff cost** — **AI VPS pricing claimed-vs-verified** reminds that **rented GPUs** shift monthly.

## Research angle

Quantization papers often report **perplexity**; production cares about **tool success rate**. Publish **your** **internal tables** if you run **serious** **A/B** — the **HN crowd** wants **numbers**, not **ad copy**.

## Edge cases

- **Fine-tunes** atop**NVFP4 base** may be**unsupported** — check**trainer** docs.
- **Multi-modal****V4** variants may need**different cards** — read**tags** on**Hugging Face** .
- **Export rules** if you**ship appliances****outside US** — legal, not**ML** .

## Step-by-step first deploy (checklist)

1. **Provision** a**FP4-capable** GPU VM with**matching driver** .
2. **Clone**`nvidia/DeepSeek-V4-Pro-0813-NVFP4` with**git lfs** .
3. **Install****vLLM** or**TRT-LLM** version**Nvidia** documents for**FP4** .
4. **Run** a**smoke prompt** from**August V4 Pro examples** .
5. **Compare** output to**API** on**same prompt** .
6. **Enable****Prometheus****GPU metrics** before**any user traffic** .

## Failure modes we have seen elsewhere

**OOM during weight load** usually means **wrong parallel sharding**. **Garbage outputs** often mean **wrong tokenizer** revision. **Slow tokens** may mean **FP4 kernels disabled** and **silent BF16 fallback**.

## Why Nvidia publishes DeepSeek-quantized cards

**Chip demand** follows **runnable model names**. **NVFP4** artifacts make **DeepSeek** a **reference workload** on **Blackwell marketing slides**, similar to **Qwen** cards. That is **distribution strategy**, not **endorsement** of every **downstream use**.

## Closing builder takeaway

Download the card when **you already committed to self-host V4 Pro** and **need VRAM relief**. Stay on **API** if **eval staff** is thin or **compliance** forbids **weight custody**. The **HN post** is a **pointer**, not a **mandate**.

## Alignment with Nvidia Dynamo and NCCL ops

Teams already running **Dynamo recipes** should verify whether **FP4 kernels** integrate with **existing NCCL groups** or need **separate serving pools**. Mixing **precision paths** in one **Kubernetes** **Deployment** without **node selectors** causes **nondeterministic** **OOMs**.

## Documentation debt to close internally

After first deploy, write an **internal runbook**: **model revision**, **GPU SKU**, **tokens/s**, **quality delta vs API**, and **rollback command**. Future you will need it when **Hugging Face** bumps **metadata** without a **semver** tag.

## When to wait for the next quant release

If **V4 Pro** ships a **0814** checkpoint with **bugfixes**, this **0813 NVFP4** artifact may **stagnate**. Watch **Nvidia org** on Hugging Face for **new revisions** before you **freeze** production.

## Related internal link for coding agents

If **coding agents** pull models from Hugging Face automatically, add **allow-list** policy for **`nvidia/*` repos** so interns do not **mix** **NVFP4** and **BF16** checkpoints in the same **Compose** file by mistake.

Schedule a **quarterly re-eval** because **FP4 kernel** quality improves with **each driver release**, and stale internal runbooks become **wrong** quickly on **fast-moving** **Blackwell** fleets. File a ticket if **tokens per watt** does not improve after **quantization** on your **standard** **batch size** and **context length**.

Keep a pinned driver version in staging until production validates FP4 quality.

*Read the Hugging Face model card for license terms and exact GPU requirements before any production deploy.*
