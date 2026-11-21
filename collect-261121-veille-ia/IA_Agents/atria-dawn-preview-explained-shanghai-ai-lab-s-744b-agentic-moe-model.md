---
id: collect-261121-veille-ia/ia-agents/atria-dawn-preview-explained-shanghai-ai-lab-s-744b-agentic-moe-model
title: "Atria Dawn Preview Explained: Shanghai AI Lab's 744B Agentic MoE Model"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-09-11"]
keywords: ["model", "dawn", "atria", "shanghai", "agentic", "preview", "shipped", "glm-5"]
source: https://miraflow.ai/blog/atria-dawn-preview-shanghai-ai-lab-744b-agentic-moe-2026
source_anchor: ""
source_lines: [1, 377]
section: "IA_Agents"
---
# Atria Dawn Preview Explained: Shanghai AI Lab's 744B Agentic MoE Model

Shanghai AI Lab's Atria Dawn Preview is a 744B-parameter agentic MoE model built on GLM-5.2 with DeepSeek Sparse Attention. Here is how its architecture, benchmarks, and tool-use loop actually work.

Shanghai AI Laboratory shipped Atria Dawn Preview on September 11, 2026, without a blog post, without a press release, and without pricing attached to it [5]. What appeared instead was a GitHub repository credited to Shanghai Artificial Intelligence Laboratory, sitting under a new org called atria-asi, alongside a matching Hugging Face repository published under the lab's existing internlm org [1] [2]. A day later, an FP8-quantized checkpoint followed [5]. Three days after that, a technical paper with more than 185 listed authors, titled Atria Dawn: The Dawn of Agentic Superintelligence, appeared on arXiv, reversing the usual paper-first release order most labs follow [3] [5].

That quiet sequencing is a signal worth reading before the spec sheet. This is a lab that has spent years building InternLM as a foundation-model research program, not chasing a headline launch cycle. What it shipped is a 744-billion-parameter Mixture-of-Experts model built on top of Z.ai's GLM-5.2 foundation, extended with agentic post-training aimed specifically at long-horizon research and engineering work [1] [2]. Not a chat model. Not a single-turn coding assistant. A model built to carry a research idea, something read out of a paper or proposed by a scientist, all the way through to executable experiments, reproducible metrics, and a report that others can inspect [2] [3].

If you build or evaluate agentic systems for a living, that framing should sound familiar and also a little audacious. Long-horizon agent loops, the kind that read a codebase, run a test suite, interpret a failure, and try again, are exactly where most frontier models still degrade the more steps a task takes. Atria Dawn Preview is Shanghai AI Lab's specific bet on how to make that loop hold together, and this post works through what is actually verifiable about that bet: the real architecture underneath the "744B agentic MoE" headline, how DeepSeek Sparse Attention and GLM-5.2's routing shape its active-parameter cost, what its own reported benchmarks say and do not say against DeepSeek V4 Pro, Kimi K3, Qwen3.8-Max, GLM-5.3, GPT-5.6, and Claude Opus 5, and how to actually load, serve, and call it, whether through Hugging Face transformers, an OpenAI-compatible endpoint, or a real tool-use loop.

## What Actually Shipped on September 11

Strip away the framing for a moment and look at the plain facts published alongside the release. Atria Dawn Preview is a Mixture-of-Experts model with 744 billion total parameters, built on the GLM-5.2 foundation model that Z.ai shipped as its own flagship open-weight release earlier in 2026 [1] [6]. The model card lists the architecture tag `glm_moe_dsa`, which, as the next section covers in detail, points directly at the two structural pieces Shanghai AI Lab inherited from Z.ai's base model: a Mixture-of-Experts feed-forward design and DeepSeek Sparse Attention [1] [7].

Two checkpoints shipped together. Atria-Dawn-Preview is the full-precision instruct model, and Atria-Dawn-Preview-FP8 is a quantized variant meant to cut the memory footprint required to actually serve a model this size [1] [2]. Both are published with a 256K token context window, and both are released under an MIT license, with weights available on Hugging Face and ModelScope and code on GitHub [1] [4]. That license choice matters more than it might look at first glance. MIT is about as permissive as an open-weight license gets, meaning commercial use, fine-tuning, redistribution, and self-hosting are all fair game with no royalty or attribution obligations beyond keeping the license notice intact, a genuinely different posture than a model shipped under a restrictive research-only license.

The model is explicitly text-input only. There is no image, audio, or video understanding built in, and the documentation is direct about it: Atria "accepts text input only" [1]. For a model whose entire pitch is agentic research and engineering work, that is a real, specific limitation worth internalizing early rather than discovering after building a workflow around it, and the production notes section later in this post covers what that means in practice.

Two hosted, OpenAI-compatible API endpoints are documented for teams that would rather call the model than self-host it: an international endpoint at `api.atria-asi.ai/v1`, and a China-region endpoint at `discovery.intern-ai.org.cn` [2]. The repository also documents ready-made integration configs for three coding agent tools specifically, Codex, Claude Code, and Kimi Code, each pointed at the hosted endpoint with text-only input enforced [2]. That is a telling detail on its own. Shanghai AI Lab is not positioning Atria as a general-purpose chatbot competing for consumer mindshare. It is positioning it as a drop-in backend for the exact class of coding-agent tools engineers already use daily.

The arXiv paper that followed the code by three days carries a quote from Tao Gui, an associate professor at Fudan University involved in the project, that frames the intent plainly: "Researchers lose a great deal of time to the work that sits between an idea and a result. Atria Dawn Preview is built to absorb that work and leave the scientific judgment where it belongs, with the researcher" [4]. That is the thesis this entire post tests against real numbers, real code, and real tradeoffs, not just the marketing language it was framed in.

## The Workload Atria Dawn Was Actually Built For

Before getting into the mechanism, it helps to see the specific shape of problem Shanghai AI Lab built this model to solve, because both the architecture choices and the training methodology only make sense once you see the workload behind them.

Picture a realistic research automation task. A scientist has a method described in a paper, or a rough idea sketched in a notebook, and needs someone, or something, to turn that into a working implementation: write the code, configure the experiment, run it against real data, read the results, notice when something failed, and try a different approach. That loop, what the documentation calls the full sequence of problem analysis, solution design, tool use, code implementation, experiment execution, result analysis, and failure recovery, is the explicit design target for Atria Dawn Preview, not a byproduct of general capability [2] [3].

That is a meaningfully different target than most instruction-tuned models are trained against. A model tuned primarily on single-turn question answering and short coding exercises gets good at producing a plausible-looking answer on the first try. It does not automatically get good at noticing, twelve steps into a long agent session, that a test is failing for a subtle reason, and adjusting course instead of confidently repeating the same broken approach. Shanghai AI Lab's answer to that gap is what its paper calls a Verifiable Experience Pipeline, a training methodology that connects tool-mediated interactions to executable environments and externally verified outcomes [3]. In plain terms, the model is not just trained on static examples of what a good agent trajectory looks like. It is trained against environments where its actions produce real, checkable consequences, a test either passes or it does not, a script either runs or it throws an error, and that verified outcome, not a human's subjective rating, is what shapes the next round of training.

This is part of a broader shift across the field in 2026, not a technique unique to Shanghai AI Lab. Training against real, executable environments rather than static human-labeled datasets has become the dominant approach for building models that hold together across long agentic sessions, and it is worth reading in the context of the industry-wide pattern our explainer on environment scaling in RL training covers in more depth, since several of September 2026's other notable model launches lean on the same underlying idea of scaling up the diversity and realism of training environments rather than just scaling parameter count.

What makes the Atria Dawn paper genuinely unusual, beyond the model release itself, is that Shanghai AI Lab treated its own internal use of the model as a case study and published the data. Researchers analyzed 769 task records from 56 participants alongside the model's own agent logs, comparing how tasks got done when researchers worked with Atria Dawn against how they would have gone otherwise [3] [4]. When evaluators assessed completed tasks under comparable conditions, they rated roughly one-third of the AI-assisted completed tasks as infeasible without AI assistance [3] [5]. The more interesting pattern in the data is qualitative rather than a single headline number: the agent frequently proposed methods and implemented revisions on its own initiative, while the human participants retained most of the final decisions and steered exploration through judgment and feedback rather than doing the implementation work themselves [3]. The paper frames this as a shift from task-level execution toward what it calls project-level partnership, where human effort concentrates on deciding research priorities and interpreting what evidence should mean, rather than on the mechanical work of getting an experiment to actually run [3] [5].

Read skeptically, that is still a self-reported internal study, run by the lab that also built the model being evaluated, with the lab's own researchers doing the rating. It is not a substitute for independent, third-party evaluation, and this post treats it accordingly later when discussing the benchmark numbers. But as a documented, specific claim about how a model changed 769 real tasks for 56 real people, it is a genuinely more substantive piece of evidence than the vague "boosts productivity" language most model releases settle for, and it is worth taking seriously as a hypothesis even while withholding full confidence until someone outside Shanghai AI Lab replicates it.

## Inside the Architecture: GLM-5.2, MoE Routing, and DeepSeek Sparse Attention

This is the section worth reading slowly if you actually plan to evaluate this model for production use, because "744B parameters" and "40B active parameters" describe two very different things, and the gap between them is the entire reason a model this large is even a serious candidate for real workloads.

### A 744B Model That Touches 40B Parameters Per Token

The `glm_moe_dsa` architecture tag on Atria's model card is not decoration. It names the two structural components that define how the model actually spends compute, and both are inherited from the GLM-5.2 base model rather than introduced by Shanghai AI Lab's post-training [1] [8].

GLM-5.2's Mixture-of-Experts design routes each token to a small subset of its available experts rather than running every token through the model's full parameter set. Independent architecture analysis of GLM-5.2 puts the routing at 256 experts per MoE layer, with 8 routed experts plus 1 always-active shared expert selected per token, out of the model's 744 billion total parameters [9]. The first three transformer blocks in the stack use dense feed-forward networks rather than MoE routing, and the remaining 75 blocks are MoE layers, for 78 transformer blocks total [9]. The practical result of that routing scheme is roughly 40 billion active parameters per token, out of the full 744 billion, a ratio of about one weight in eighteen actually doing work on any single forward pass [9].

Atria's own model card does not republish a separate active-parameter figure specific to the post-trained checkpoint, which is expected: agentic post-training on top of a base model changes what the weights have learned, not the shape of the router that decides which weights get used. Since the architecture tag explicitly identifies the base as GLM-5.2 and Atria's post-training does not alter routing topology, the 40B-active-per-744B-total figure independently reported for GLM-5.2 is the correct lens for understanding what actually runs when Atria processes a token [1] [9].

### What the DSA Suffix Actually Stands For

DSA stands for DeepSeek Sparse Attention, a mechanism GLM-5, GLM-5.1, and GLM-5.2 all adopted alongside Multi-head Latent Attention, both originally developed in DeepSeek's own model line [7] [8]. It is worth confirming this from more than one independent technical source before treating it as settled, since the model card itself does not spell out the acronym: both Sebastian Raschka's architecture-gallery writeup and NVIDIA's own NeMo AutoModel documentation for GLM-5-family models independently identify DSA as DeepSeek Sparse Attention and describe the same underlying mechanism, which is a solid basis for treating the attribution as correct [7] [8].

The core idea behind DSA is a small, cheap neural network called a Lightning Indexer that runs ahead of the actual attention computation on each layer [7]. Rather than computing full attention across every token in a long context, which gets quadratically more expensive as context length grows, DSA breaks the work into three stages. Index: the Lightning Indexer scores how relevant every existing key-value block is to the current query, a cheap operation compared to full attention. Select: only the highest-scoring blocks, capped at the top 2,048 tokens per attention head, get carried forward. Attend: the expensive, full attention computation only actually runs over that much smaller selected set, not the entire context [7] [8].

GLM-5.2 layers a further optimization on top called IndexShare. Rather than running the full Lightning Indexer computation fresh on every one of its 78 layers, GLM-5.2 runs the complete indexer once every four layers and lets the layers in between reuse that same set of selected token indices [7]. Reported architecture analysis attributes roughly a 2.9x reduction in per-token FLOPs at a 1-million-token context window to the combination of sparse selection and cross-layer index reuse, compared to a dense attention mechanism attending to the full context on every layer [7].

One specific, checkable detail is worth flagging honestly rather than glossing over. GLM-5.2's own native context window, as reported in independent architecture coverage, extends to 1 million tokens [9]. Atria Dawn Preview's published context window is 256K tokens, for both the standard and FP8 checkpoints [1] [6]. Neither the model card nor the arXiv paper explains this gap directly, so it is worth treating as an open question rather than asserting a definitive reason. A reasonable, though unconfirmed, hypothesis is that agentic post-training on verified-outcome environments benefits from a narrower, more heavily exercised context range during training, trading the base model's maximum theoretical window for more reliable behavior within the range the agent actually operates in during long tool-use sessions. Readers evaluating this model for a workload that genuinely needs context beyond 256K tokens should treat that as a hard published ceiling regardless of what the base model supports, not something to route around by assuming inherited capability.

### Worked Example: What the Sparse-Attention and Active-Parameter Math Looks Like

None of the ratios above need re-deriving to trust them; they come from independent published analysis of the base architecture. But it is worth seeing them laid out programmatically, both to build real intuition for the scale of the gap between total and active compute, and as a template for reasoning about your own workload's cost profile before committing infrastructure planning to it.

python```
/code
# Illustrative math for the two sparsity mechanisms Atria Dawn Preview
# inherits from GLM-5.2: MoE routing sparsity (8 routed + 1 shared expert
# of 256 per layer, ~40B active of 744B total [9]) and DeepSeek Sparse
# Attention's token-selection sparsity (top-2,048 tokens per head,
# regardless of total context length [7][8]).
total_params_b = 744        # total parameters, billions [1]
active_params_b = 40        # active parameters per token, billions [9]
context_window = 256_000    # Atria's published context window, tokens [1]
dsa_selected_tokens = 2048  # tokens actually attended per query, per head [7]
moe_active_ratio = active_params_b / total_params_b
attention_selected_ratio = dsa_selected_tokens / context_window
print(f"MoE active-parameter ratio:      {moe_active_ratio:.1%} "
      f"({active_params_b}B active of {total_params_b}B total)")
print(f"DSA attended-token ratio:        {attention_selected_ratio:.2%} "
      f"({dsa_selected_tokens:,} of {context_window:,} tokens fully attended)")
# IndexShare reuses one full indexer pass across every 4 layers [7],
# so the indexer's own overhead is paid roughly once per 4 layers
# rather than once per layer.
total_layers = 78            # 3 dense + 75 MoE blocks [9]
indexshare_group = 4
indexer_passes = total_layers // indexshare_group
print(f"Full indexer passes across {total_layers} layers with IndexShare: "
      f"{indexer_passes} (vs {total_layers} without reuse)")
```

Running that script makes two separate savings visible at once. The MoE routing means any single token only ever touches about 5.4 percent of the model's total parameter count, a routing sparsity that is common across large MoE models but still worth seeing as a raw ratio rather than an abstraction. The attention sparsity is a separate, additive effect layered on top: at a full 256K-token context, a query attending only to its top 2,048 selected tokens is doing full attention math over roughly 0.8 percent of the available context, with the Lightning Indexer's cheap relevance-scoring pass covering the rest. These are two different mechanisms, MoE routing sparsity and attention sparsity, solving two different cost problems, and Atria inherits both from its GLM-5.2 foundation rather than reinventing either.

For a useful point of comparison, DeepSeek's own V4.1-Flash model, covered in our architecture breakdown of its Causal Encoder-Decoder design, tackles a similar goal, cutting the real compute cost of processing very long, input-heavy agentic contexts, through a structurally different mechanism: splitting the model into a dedicated causal encoder and a smaller decoder rather than sparse token selection within a unified stack. Both approaches land at a similar underlying insight even though the engineering looks different: a standard dense-attention, uniformly-active transformer spends compute in the same way regardless of whether a token is easy or hard to process, and both labs concluded that assumption was leaving real efficiency on the table for exactly the kind of long, lopsided agentic workloads both models target.

## Benchmarks in Context: Where Atria Wins and Where It Doesn't

Shanghai AI Lab published results across a table the GitHub repository organizes into five categories, matching the framing this post opened with: Discovery, Creation, Tool Use, Delivery, and Cybersecurity [2]. Independent reporting on the release states that across 16 total benchmarks in that table, Atria Dawn Preview posts the single highest listed score on five of them [5] [6]. Rather than repeating that summary claim at face value, here are the specific named-model comparisons worth actually sitting with.

| Category / Benchmark | Atria Dawn Preview | DeepSeek V4 Pro | Kimi K3 | Qwen3.8-Max | GLM-5.3 | GPT-5.6 (Sol) | Claude Opus 5 | 
|---|---|---|---|---|---|---|---|
| Discovery -- DeepSearchQA | 96.0 | -- | 95.9 | -- | 94.7 | 93.2 | -- | 
| Discovery -- BrowseComp | 92.5 | -- | -- | -- | -- | -- | -- | 
| Tool Use -- BFCL v4 | 77.0 | 71.4 | 69.1 | -- | 74.1 | -- | -- | 
| Tool Use -- AutomationBench | 53.8 | -- | -- | -- | -- | -- | -- | 
| Creation -- SWE-bench Pro | 59.6 | 58.3 | 61.6 | 65.1 | 60.3 | 61.4 | 74.7 | 
| Cybersecurity -- CyberGym | 86.5 | 83.3 | 78.7 | 73.8 | 84.5 | 83.6 | -- | 
| Delivery -- JobBench | 50.3 | 54.1 | 54.3 | 52.7 | 58.2 | 45.4 | 68.0 | 

**Discovery is where Atria's numbers look strongest relative to the field.** On DeepSearchQA, a benchmark built around multi-step research and evidence retrieval, Atria scores 96.0, ahead of Kimi K3's 95.9, GLM-5.3's 94.7, and GPT-5.6 Sol's 93.2 [1] [2]. That is a genuinely tight race at the top, with the gap between first and fourth place spanning less than three points, which is worth noting explicitly rather than treating a nominal first-place finish as a decisive win. Independent reporting also credits Atria with the highest reported BrowseComp score in the lab's own table, at 92.5 [5] [6], reinforcing that research-and-evidence-discovery tasks are where the Verifiable Experience Pipeline's training emphasis shows up most clearly in the numbers.

**Tool Use tells a similar story.** On BFCL v4, the Berkeley Function-Calling Leaderboard's fourth version, a benchmark that specifically measures whether a model correctly selects and formats tool and function calls, Atria scores 77.0, ahead of GLM-5.3's 74.1 and DeepSeek V4 Pro's 71.4 [1]. Independent coverage also lists AutomationBench at 53.8 as Atria's highest reported score in that table [5] [6]. Given that Atria is explicitly marketed as a drop-in backend for coding-agent tools like Claude Code and Codex, a strong BFCL v4 result is directly relevant evidence, since function-calling correctness is the specific skill those integrations depend on.

**Cybersecurity is Atria's other clear strength.** On CyberGym, a benchmark built around real-world vulnerability analysis and remediation tasks, Atria scores 86.5, ahead of GLM-5.3 at 84.5, DeepSeek V4 Pro at 83.3, GPT-5.6 Sol at 83.6, and Kimi K3 at 78.7 [1] [2]. This is also the one category where the model card attaches an explicit usage caveat, discussed in full in the production notes section below: the documented capability is framed specifically as "analyzing security issues, validating vulnerabilities, applying fixes, and performing re-validation in authorized environments" [2], language that matters as much as the score itself for anyone actually deploying this capability.

**Creation is where the honest picture gets more mixed.** On SWE-bench Pro, a harder, more realistic successor to the original SWE-bench built around real pull-request-level software engineering tasks, Atria scores 59.6. That is ahead of DeepSeek V4 Pro's 58.3, but behind Kimi K3's 61.6, GLM-5.3's 60.3, GPT-5.6 Sol's 61.4, Qwen3.8-Max's 65.1, and well behind Claude Opus 5's 74.7, which leads the entire comparison set by more than 15 points [1]. That is not a small gap to wave away. For a model whose entire pitch is turning research ideas into working, executable code, landing mid-pack rather than at the top on the field's most realistic software engineering benchmark is a genuinely important limitation to weigh, not a footnote. Our earlier piece on benchmark contamination and methodology in SWE-bench, LiveCodeBench, and MLE-bench is worth reading alongside this number if you are deciding how much weight to put on any single coding benchmark score, since methodology differences between benchmark versions can meaningfully shift what a given score actually represents.

**Delivery is Atria's weakest relative category by a clear margin.** On JobBench, a benchmark focused on document and presentation delivery, office-productivity-style tasks, Atria scores 50.3, behind DeepSeek V4 Pro's 54.1, Kimi K3's 54.3, Qwen3.8-Max's 52.7, GLM-5.3's 58.2, and dramatically behind Claude Opus 5's 68.0 [1]. Notably, Atria's JobBench score is the only comparison in this entire table where it also trails GPT-5.6 Sol, which posts 45.4, meaning even a model Atria beats on every other listed category still edges it out here. This is a specific, checkable weak point rather than a vague caveat, and it lines up with the model's own stated design focus: Atria was built around research and engineering workflows specifically, not general office-document generation, and the JobBench numbers reflect that scoping honestly rather than contradicting the model's own positioning.

Two caveats are worth stating plainly before moving on. First, every number in this table is self-reported by Shanghai AI Lab, run on the lab's own infrastructure and evaluation harness, the same caveat that applies to essentially every model release's launch-day benchmark table across the industry. No independent third-party lab had published a replication of these specific figures at the time of writing. Second, comparison models named in the table, DeepSeek V4 Pro, Kimi K3, Qwen3.8-Max, GLM-5.3, GPT-5.6 Sol, and Claude Opus 5, are themselves evolving fast, and a benchmark table is a snapshot, not a permanent ranking. Readers evaluating this model for their own workload should treat this table as a starting hypothesis to validate against a held-out set of their own representative tasks, not a final verdict, the same advice our Kimi K3 and Qwen3.8-Max breakdowns give for those two models specifically.

## How to Actually Load, Serve, and Call Atria Dawn Preview

### Loading the Model Directly with Hugging Face Transformers

For teams evaluating the model locally, or building a custom serving stack rather than using the hosted API, standard Hugging Face transformers tooling works against the published weights.

python```
/code
# Loading Atria Dawn Preview directly with Hugging Face transformers.
# Text input only -- do not pass image or audio content [1].
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
model_id = "internlm/Atria-Dawn-Preview"  # or "internlm/Atria-Dawn-Preview-FP8"
tokenizer = AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    torch_dtype=torch.bfloat16,
    device_map="auto",
    trust_remote_code=True,
)
messages = [
    {"role": "system", "content": "You are a research agent that implements, runs, and verifies experiments."},
    {"role": "user", "content": "Read the attached method description and draft a plan to reproduce its core experiment."},
]
inputs = tokenizer.apply_chat_template(
    messages, add_generation_prompt=True, return_tensors="pt"
).to(model.device)
output = model.generate(
    inputs,
    max_new_tokens=1024,
    temperature=0.3,
    do_sample=True,
)
print(tokenizer.decode(output[0][inputs.shape[-1]:], skip_special_tokens=True))
```

A few details worth getting right from the start. The model is large enough, 744 billion total parameters even with only about 40 billion active per token, that `device_map="auto"` alone is not a serious production serving strategy for anything beyond a single evaluation run. The next subsection covers a real serving stack. And because the model is text-only, any pipeline that assumes image or document inputs can simply be passed through, the way many multimodal models now handle a PDF or screenshot directly, needs a separate OCR or document-parsing step in front of Atria rather than relying on native ingestion [1].

### Serving at Real Scale with vLLM or SGLang

The GitHub repository documents two supported inference engines for self-hosted deployment at scale: SGLang, version 0.5.13.post1 or later, and vLLM, version 0.23.0 or later [1] [2]. Both are the standard high-throughput serving engines most teams running large open-weight models in production already use, so integration is mostly a matter of pointing an existing serving pipeline at the new checkpoint rather than building new infrastructure from scratch.

bash```
/code
# Serving Atria Dawn Preview at scale with vLLM (v0.23.0+) [1][2].
# The FP8 checkpoint is the realistic starting point for most
# self-hosting teams -- see the VRAM footprint comparison below.
pip install "vllm>=0.23.0"
vllm serve internlm/Atria-Dawn-Preview-FP8 \
  --tensor-parallel-size 8 \
  --gpu-memory-utilization 0.90 \
  --max-model-len 262144 \
  --trust-remote-code \
  --port 8000
# Equivalent SGLang path (v0.5.13.post1+) [1][2]:
# pip install "sglang[all]>=0.5.13.post1"
# python -m sglang.launch_server \
#   --model-path internlm/Atria-Dawn-Preview-FP8 \
#   --tp 8 \
#   --context-length 262144 \
#   --port 8000
```

The FP8 checkpoint is the more realistic starting point for most self-hosting teams, since it roughly halves the raw weight storage and VRAM footprint compared to the standard checkpoint, a tradeoff the production notes section below works through with real numbers.

### Calling the Hosted, OpenAI-Compatible Endpoint

For teams that would rather call a hosted API than manage GPU infrastructure directly, Shanghai AI Lab documents two OpenAI-compatible endpoints: an international endpoint at `api.atria-asi.ai/v1`, and a China-region endpoint at `discovery.intern-ai.org.cn` [2]. Because the API is OpenAI-compatible, any existing codebase built against the standard `openai` Python client, or an equivalent client in another language, can switch to Atria by changing the base URL, the API key, and the model string, without rewriting the surrounding integration.

python```
/code
# Calling the hosted, OpenAI-compatible Atria Dawn Preview endpoint.
# International endpoint: https://api.atria-asi.ai/v1
# China-region endpoint: https://discovery.intern-ai.org.cn [2]
# Model id is exactly "Atria-Dawn-Preview", as used in the repository's
# own Codex and Kimi Code integration configs [2].
from openai import OpenAI
import os
client = OpenAI(
    api_key=os.environ["ATRIA_API_KEY"],
    base_url="https://api.atria-asi.ai/v1",
)
response = client.chat.completions.create(
    model="Atria-Dawn-Preview",
    messages=[
        {"role": "system", "content": "You are a research agent operating in an authorized sandboxed environment."},
        {"role": "user", "content": "Analyze the failing unit test in scheduler.py, propose a fix, and validate it."},
    ],
    temperature=0.2,
    max_tokens=2000,
)
print(response.choices[0].message.content)
print(f"prompt_tokens={response.usage.prompt_tokens} completion_tokens={response.usage.completion_tokens}")
```

The exact model identifier to use in the `model` field is `Atria-Dawn-Preview`, as documented in the repository's own configuration examples for Codex and Kimi Code integrations [2]. Those same repository configs show the intended IDE integration pattern directly: Codex's provider config points its `wire_api` field at `responses` and reads the API key from an `ATRIA_API_KEY` environment variable, and Kimi Code's provider config does the equivalent through its own OpenAI-compatible provider type [2]. If you are setting this up for an actual coding agent tool rather than direct API calls, using those documented configs directly is more reliable than reverse-engineering an integration from general OpenAI-compatible API documentation, since the repository's own examples reflect exactly how Shanghai AI Lab tested the integration.

### A Real Tool-Use Agent Loop

The benchmarks in the previous section, BFCL v4 in particular, measure exactly this pattern: does the model correctly decide when to call a tool, format that call correctly, and use the tool's result to make progress toward a goal. Here is what that loop actually looks like in code, structured the way a real coding or research agent built on Atria would run it, calling a tool, executing it, feeding the result back, and repeating until the task is verified complete.

python```
/code
# A minimal tool-use agent loop against Atria Dawn Preview's
# OpenAI-compatible endpoint: plan, call a tool, execute, feed the
# result back, and stop only once the task is verified complete.
# This is the pattern BFCL v4 and the failure-recovery training
# objective both measure [2][3].
from openai import OpenAI
import os, json, subprocess
client = OpenAI(api_key=os.environ["ATRIA_API_KEY"], base_url="https://api.atria-asi.ai/v1")
tools = [{
    "type": "function",
    "function": {
        "name": "run_tests",
        "description": "Run the project's test suite and return pass/fail output.",
        "parameters": {"type": "object", "properties": {}, "required": []},
    },
}]
def run_tests():
    result = subprocess.run(["pytest", "-q"], capture_output=True, text=True)
    return {"exit_code": result.returncode, "output": result.stdout[-2000:]}
messages = [
    {"role": "system", "content": "You are a coding agent. Fix the failing test, verifying with run_tests before finishing."},
    {"role": "user", "content": "The test suite in this repo has a failing test. Fix it and confirm the fix."},
]
for step in range(6):  # bounded retry budget, mirrors failure-recovery training
    response = client.chat.completions.create(
        model="Atria-Dawn-Preview", messages=messages, tools=tools, tool_choice="auto",
    )
    msg = response.choices[0].message
    messages.append(msg.model_dump(exclude_none=True))
    if not msg.tool_calls:
        print("Agent finished:", msg.content)
        break
    for call in msg.tool_calls:
        if call.function.name == "run_tests":
            result = run_tests()
            messages.append({
                "role": "tool",
                "tool_call_id": call.id,
                "content": json.dumps(result),
            })
```

The structure in that example, plan, call a tool, execute, verify, decide whether to continue or stop, mirrors the failure-recovery step in Atria's own stated design goals directly [2] [3]. A model trained only on single-turn examples tends to either give up after one failed tool call or loop indefinitely without making real progress. A model trained against a Verifiable Experience Pipeline, where tool outcomes are externally checked rather than just plausible-looking, is specifically optimized to treat a failed test result as information to act on rather than noise to ignore, which is the actual behavior this benchmark category is trying to measure.

If you want to see the agentic loop itself rendered as motion rather than a static diagram, here is a generation prompt built around the same assembly-line mapping used in the hero image above, written for a text-to-video model.

A clean vector technical diagram animation on a pale cream background with a thin outlined border, drawn as an assembly-line cross-section. A small glowing capsule labeled RESEARCH IDEA travels along a conveyor belt from left to right, lighting up each labeled station in sequence as it passes through: PROBLEM ANALYSIS, SOLUTION DESIGN, TOOL USE, CODE IMPLEMENTATION, EXPERIMENT EXECUTION, and RESULT ANALYSIS. Partway through, the capsule briefly dims and reverses along a dashed path back to CODE IMPLEMENTATION, labeled FAILURE RECOVERY, before resuming forward motion. The capsule finally reaches a final station labeled REPORT, where it dissolves into a small stack of paper icons. Camera holds a steady wide static side-view shot, flat color fills in muted blue, teal, and amber with no photographic shading, smooth continuous left-to-right motion, every label short and legible throughout, no logos, no people, no garbled text.

## Production Notes: When to Actually Reach for Atria Dawn Preview

### The Real Self-Hosting Footprint

A 744-billion-parameter model, even one with a sparse MoE router that only activates about 40 billion parameters per token, is not a laptop-friendly download. The parameter count that matters for infrastructure planning is the total weight storage and VRAM footprint required to hold the full model resident for serving, since a MoE router can send any token to any of its 256 experts, meaning the entire expert pool has to be loaded and available even though only a fraction of it activates on any given forward pass.

python```
/code
# Rough VRAM/storage footprint comparison for self-hosting Atria Dawn
# Preview: the standard BF16 checkpoint vs the FP8 checkpoint [1].
# This is a weight-storage estimate only -- real serving VRAM also
# needs headroom for KV cache and batching on top of these numbers.
total_params = 744e9  # total parameters, all experts resident [1][9]
bytes_per_param_bf16 = 2
bytes_per_param_fp8 = 1
bf16_bytes = total_params * bytes_per_param_bf16
fp8_bytes = total_params * bytes_per_param_fp8
def to_gb(n_bytes):
    return n_bytes / (1024 ** 3)
def to_tb(n_bytes):
    return n_bytes / (1024 ** 4)
print(f"BF16 checkpoint: {to_gb(bf16_bytes):,.0f} GB (~{to_tb(bf16_bytes):.2f} TB)")
print(f"FP8 checkpoint:  {to_gb(fp8_bytes):,.0f} GB (~{to_tb(fp8_bytes):.2f} TB)")
gpu_vram_gb = 80  # a single current-generation 80GB accelerator
for label, size_bytes in [("BF16", bf16_bytes), ("FP8", fp8_bytes)]:
    gpus_needed = -(-to_gb(size_bytes) // gpu_vram_gb)  # ceiling division
    print(f"{label}: at least {int(gpus_needed)} x {gpu_vram_gb}GB GPUs just for weights "
          f"(before KV cache and batching headroom)")
```

That gap, roughly 1.5 terabytes for the standard BF16 checkpoint versus roughly 744 gigabytes for the FP8 checkpoint, is the real, practical reason the FP8 variant shipped alongside the standard one rather than as an afterthought [1]. At current-generation 80GB accelerators, the standard checkpoint realistically needs a multi-node cluster to serve at all, while the FP8 checkpoint fits, with real headroom for KV cache and batching, on a single well-provisioned 8-GPU node. For most teams outside of a well-funded infrastructure org, the FP8 checkpoint is the only self-hosting path worth seriously evaluating, and the hosted API is worth defaulting to for anything short of a team that already runs serious GPU infrastructure for other workloads.

### The MIT License Is a Real Advantage, With Real Costs Attached

An MIT license on a model at this parameter count is a meaningfully different proposition than the same license on a 7-billion-parameter model a hobbyist can fine-tune on a single GPU over a weekend. The permissiveness is real: no royalty obligations, no usage restrictions beyond the license notice, full rights to fine-tune, redistribute, and build commercial products on top of the weights [1] [4]. But the cost of exercising that freedom does not disappear, it just moves from a per-token API bill to an infrastructure bill. A team choosing to self-host is trading a variable, usage-based cost for a large, mostly-fixed capital and operational cost, and that tradeoff only pencils out at real production volume, not for a handful of research experiments a month. For anyone weighing that tradeoff seriously, it is worth comparing against multi-agent orchestration approaches that get strong results from smaller, cheaper models coordinated together rather than one very large model doing everything; our breakdown of Sakana Fugu Ultra v2's orchestration approach covers a genuinely different architectural philosophy aimed at a similar class of agentic problems, worth reading as a real alternative rather than assuming a single 744B model is the only path to strong agentic performance.

### The Cybersecurity Caveat Deserves to Be Taken Literally

Atria's documented cybersecurity capability, the one behind its 86.5 CyberGym score, is scoped explicitly to "authorized environments" in the model's own documentation [2]. That phrase is doing real work, not just covering legal bases. CyberGym itself, as a benchmark, is built around realistic vulnerability analysis and remediation tasks precisely because that is the skill worth measuring, and a model that is genuinely capable at finding and validating vulnerabilities is, by the same token, genuinely capable of finding and exploiting them outside a sanctioned context. Nothing in the model's architecture or deployment tooling enforces that authorization boundary automatically. It is a policy and deployment-practice responsibility for whoever integrates the model, not a built-in technical safeguard, and any team deploying Atria's cybersecurity capability into a real workflow needs its own access controls, scoping, and audit trail around what environments the model is actually allowed to probe, exactly the same operational discipline a team would apply to a skilled human penetration tester rather than treating a capable model as inherently self-limiting.

### Text-Only Is a Real Constraint, Not a Minor Footnote

Because Atria accepts text input only, any workflow that currently depends on a model reading a screenshot, a diagram, a scanned PDF, or a chart directly needs a separate step in front of Atria to convert that content into text first [1]. For a research-agent workload specifically, that is a real gap worth planning around explicitly: a lot of real scientific and engineering documentation includes figures, plots, and diagrams that carry information a purely text-based model cannot access directly. Teams building a serious research-automation pipeline on top of Atria should budget for a document-parsing or vision-model front end as a first-class part of the architecture, not an edge case to handle later.

### For Teams Building Consumer AI Content Tools

Miraflow's own generation pipeline reflects the same underlying shift this post has been describing throughout, even though it runs on different models entirely. Turning a topic into a finished short video inside Text2Shorts is not one model call, it is a multi-step agentic sequence: generate a script, generate scene-by-scene visual prompts based on that script, select a voice, and assemble the result, with a human able to edit or regenerate any step along the way rather than accepting a single opaque output. AI Clipping follows the same pattern from the other direction, ingesting a full long-form video, analyzing it end to end, and producing several ranked, captioned short clips rather than one static output. Neither of those Miraflow tools runs on Atria Dawn Preview specifically, but the broader engineering pressure behind both of them, breaking a complex creative or analytical task into a verifiable multi-step loop rather than hoping a single prompt gets it right, is exactly the same pressure Shanghai AI Lab is responding to with the Verifiable Experience Pipeline. It is a useful reminder that "agentic" is not a marketing label reserved for research labs. It is a real, increasingly common architectural pattern across AI tooling generally, from a 744-billion-parameter research model down to a browser-based content generation pipeline.

## Common Mistakes to Avoid

**Assuming the base model's 1M-token context carries over to Atria.** GLM-5.2's native architecture supports up to 1 million tokens of context, but Atria Dawn Preview's published context window, for both checkpoints, is 256K tokens [1] [9]. Treat 256K as the real, hard limit rather than assuming inherited capability from the base model.

**Treating 744 billion parameters as the number that predicts cost or latency.** The routing-based active-parameter figure, roughly 40 billion per token, is what actually predicts compute cost during inference. The total parameter count predicts the storage and VRAM footprint required to hold the full expert pool resident, a separate and equally important number, but conflating the two leads to badly wrong intuitions about both cost and infrastructure requirements.

**Citing the benchmark table as independently verified.** Every number in the comparison table above is Shanghai AI Lab's own self-reported result. It is specific, falsifiable, and worth taking seriously, especially given the lab's established research track record, but it is not the same evidentiary standard as an independent third-party replication, and it is worth saying so explicitly if citing these numbers elsewhere.

**Deploying the cybersecurity capability without real access controls.** The "authorized environments" language in the model's own documentation is a scoping statement, not a built-in technical safeguard. Treat any deployment of Atria's vulnerability-analysis capability with the same operational discipline you would apply to giving a skilled human tester access to production systems.

**Assuming FP8 quantization is free precision loss.** The FP8 checkpoint roughly halves the memory and storage footprint of the standard checkpoint, which is genuinely useful for self-hosting feasibility, but any quantization step is a real tradeoff against numerical precision. Running your own evaluation on the FP8 checkpoint against representative tasks, rather than assuming benchmark numbers reported for the standard checkpoint transfer identically, is worth the relatively small additional effort before committing production traffic to it.

**Picking this model purely from its Discovery and Cybersecurity scores while ignoring its Delivery score.** Atria's JobBench result, 50.3, trails five of the six other named models in this comparison, including GPT-5.6 Sol. If your actual workload leans toward document generation and office-productivity-style tasks rather than research discovery or vulnerability analysis, the categories where Atria scores strongest are not the categories that will determine your real-world results.

## Frequently Asked Questions

**What does the DSA suffix in** **glm_moe_dsa** **actually mean?** DSA stands for DeepSeek Sparse Attention, an attention mechanism originally developed in DeepSeek's model line and adopted by Z.ai's GLM-5, GLM-5.1, and GLM-5.2 models, which Atria Dawn Preview is built on. It uses a lightweight Lightning Indexer to select roughly the top 2,048 most relevant tokens per attention head, then runs full attention only over that smaller selected set rather than the entire context [7] [8].

**How many parameters does Atria Dawn Preview actually activate per token?** Independent architecture analysis of the GLM-5.2 base model, which Atria's post-training builds on without altering the router's shape, reports roughly 40 billion active parameters per token out of 744 billion total, through a Mixture-of-Experts design routing to 8 experts plus 1 shared expert out of 256 available experts per layer [9].

**Is Atria Dawn Preview a fine-tune of GLM-5.2, or a new architecture?** It is agentic post-training on top of GLM-5.2's existing MoE-plus-DSA architecture, not a new architecture family. The model card's `glm_moe_dsa` tag and independent reporting both confirm the underlying structure is inherited from Z.ai's base model, with Shanghai AI Lab's contribution being the Verifiable Experience Pipeline training methodology layered on top [1] [3] [6].

**Can I use Atria Dawn Preview commercially?** Yes. Both checkpoints are released under an MIT license, which permits commercial use, fine-tuning, and redistribution without royalty obligations [1] [4].

**Does Atria Dawn Preview support images or other multimodal input?** No. The model documentation states it accepts text input only. Any workflow needing image, audio, or document understanding needs a separate model or preprocessing step in front of Atria [1].

**How does Atria Dawn Preview compare to Claude Opus 5 for coding tasks?** On SWE-bench Pro, a realistic pull-request-level software engineering benchmark, Claude Opus 5 scores 74.7 against Atria's 59.6, the largest single gap anywhere in the published comparison table [1]. Atria's own strongest categories are Discovery and Cybersecurity, not general software engineering, so teams with coding-heavy workloads specifically should weigh that gap seriously rather than defaulting to Atria based on its other benchmark wins.

**Are Shanghai AI Lab's benchmark numbers independently verified?** Not as of this writing. The comparison figures come from Shanghai AI Lab's own published table, run on its own evaluation infrastructure [1] [2]. They are specific and credible given the lab's research track record, but no independent third-party lab had replicated them at the time this post was written.

**What does the "authorized environments" language around cybersecurity use actually mean in practice?** It means the model's vulnerability-analysis and remediation capability is documented for use within systems the deploying team is authorized to test, not as a general-purpose scanning tool to point at arbitrary infrastructure. It is a documented scoping statement, not an enforced technical restriction, so responsibility for staying within that boundary falls on whoever deploys the capability [2].

## Conclusion

Atria Dawn Preview is a genuinely substantive release wearing an unassuming name. Underneath the "744B agentic MoE" headline is a specific, inheritable architecture, GLM-5.2's Mixture-of-Experts routing paired with DeepSeek Sparse Attention's Lightning Indexer, that lets a 744-billion-parameter model run at roughly 40 billion active parameters per token, and a training methodology, the Verifiable Experience Pipeline, built specifically to hold together across the kind of long, multi-step research and engineering loops that break most models trained only on single-turn examples. The benchmark story is genuinely mixed rather than a clean win, and that honesty is worth preserving rather than smoothing over: strong, competitive results in Discovery and Cybersecurity, a real and specific gap against Claude Opus 5 on realistic software engineering work, and a clearly weaker showing on document-delivery tasks that were never the model's stated focus in the first place.

None of that mixed picture makes this an unserious release. An MIT-licensed, 744-billion-parameter model with real, checkable, categorized benchmark results and a documented, OpenAI-compatible path to both self-hosting and hosted API access is exactly the kind of concrete evidence worth engaging with directly, tool by tool, category by category, rather than either dismissing it as hype or accepting its strongest numbers as the whole story. For any team evaluating agentic models for real research or engineering workloads in September 2026, Atria Dawn Preview belongs on the shortlist to test against your own tasks, with a clear-eyed view of exactly where it is likely to help and where a different model is still the better call.

## References and Sources

[1] Hugging Face. "internlm/Atria-Dawn-Preview model card."

[2] GitHub. "atria-asi/Atria-Dawn-Preview repository."

[3] arXiv. "Atria Dawn: The Dawn of Agentic Superintelligence."

[4] Yahoo Finance. "Atria Releases Atria Dawn Preview."

[5] AI Weekly. "Shanghai AI Lab Ships Atria Dawn Preview, a 744B Agentic MoE."

[6] Pandaily. "Shanghai AI Lab Releases Atria Dawn Preview, a 744B MoE Agentic Model."

[7] Raschka, Sebastian. "DeepSeek Sparse Attention."

[8] NVIDIA. "GLM-5 / GLM-5.1 / GLM-5.2 (MoE + DSA), NeMo AutoModel documentation."

[9] MindStudio. "Mixture of Experts Architecture Explained: How GLM 5.2 Runs 40B Active Parameters."

[10] llm-stats.com. "Atria Dawn Preview: Benchmarks, Pricing & Context Window."
