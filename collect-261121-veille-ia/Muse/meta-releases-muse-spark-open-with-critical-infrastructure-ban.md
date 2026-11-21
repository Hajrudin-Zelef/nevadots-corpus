---
id: collect-261121-veille-ia/muse/meta-releases-muse-spark-open-with-critical-infrastructure-ban
title: "Meta Releases Muse Spark Open With Critical Infrastructure Ban"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["meta", "model", "muse", "spark", "license", "open", "critical", "infrastructure"]
source: https://www.beri.net/article/meta-muse-spark-open-816b-open-weights-license-critical-infrastructure-ban-20m-hosting-clause-vs-kimi-k3-glm-5-3
source_anchor: ""
source_lines: [1, 86]
section: "Muse"
---
**If you run a utility, a telecom network, a transport system or anything defense-adjacent, your lawyers have to clear Muse Spark Open before your engineers benchmark it.** Meta released the 816-billion-parameter weights on October 15 under a license whose usage policy prohibits "operation of critical infrastructure, transportation technologies, or heavy machinery" and any use related to "military, warfare, nuclear industries or applications, espionage" (Muse Model License, Exhibit A). Everyone else gets a model that is free for internal use and that Meta's own table puts behind Kimi K3, GLM 5.3 and Qwen3.8 on Terminal-Bench 2.1 and ahead of all three on tool use.

The clauses are old. Both match, word for word, sections 2.a and 2.d of the Llama 3 acceptable use policy. If your legal team already wrote a position on Llama, most of the memo exists.

## What Meta Released on October 15

Muse Spark Open is a mixture-of-experts model with 816B total parameters, 42B active per token and a 1M-token context window, according to Meta's announcement, which calls it "the most capable US open-weight model to date." It accepts text, image, video and audio and returns text (Implicator). It is built on Muse Spark 1.2 with extra safety post-training so it can run without Meta's hosted guardrails (SiliconANGLE).

The weights are on Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry Models. Hosted APIs launched at Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI (Meta).

This is Meta's second-tier model. Its best work stays closed in Muse Wonder, and Alexandr Wang said of the release, "These aren't the most capable models that we've developed or released" (Implicator). On the Artificial Analysis Intelligence Index the closed Muse Spark 1.3 scores 48 against the open model's 39 (OrcaRouter).

## What the Muse Model License Prohibits

The license grants broad rights and attaches the conditions an enterprise has to check: category bans in the usage policy, a revenue trigger for hosted resale, and an attribution label. The grant itself is generous. You may use, copy, fine-tune, distill, merge, distribute, sublicense and sell the software, and Meta claims no ownership of outputs (model card).

An acceptable use policy is the list of activities a model license forbids however you obtained or hosted the weights. Exhibit A of the Muse license carries two clauses that decide the question for whole industries. Section 2.1 prohibits use related to "Military, warfare, nuclear industries or applications, espionage." Section 2.4 prohibits activities presenting "a risk of death or bodily harm," including "Operation of critical infrastructure, transportation technologies, or heavy machinery."

Read the verbs. Section 2.4 targets *operation*. A utility using the model to summarize rate-case filings or draft outage letters to customers is a long way from operating the grid. An agent that reads SCADA telemetry and proposes switching orders is close to it. The license text we read does not define "critical infrastructure," so that line is your counsel's to draw, and the answer belongs on paper before anyone downloads weights.

Section 2.1 is wider. "Nuclear industries or applications" carries no "operation of" qualifier. A utility with nuclear generation in its fleet, or a supplier to one, has a harder question than a water authority does, even for back-office use.

The Hugging Face repository is also gated: the access form asks for legal name, date of birth and organisation, and it cannot be edited after you submit it (OrcaRouter). Whoever fills it in is making a representation for the company.

## The $20M Clause Hits Resellers, Not Internal Platforms

The revenue trigger only applies when you sell model access to third parties. Section 3 says that if "the aggregate gross revenue of the Licensee and its affiliates exceeds twenty (20) million US dollars" in any consecutive 12 months, you must notify Meta and sign a separate written agreement before offering the model as a service through an API or similar interface (license). Internal use and research are excluded, and so is building the model into your own product (Trending Topics).

So the clause bites a SaaS company that wants to expose a raw Muse endpoint to its customers. It does not bite an internal platform team serving the model to employees. That puts Meta in line with Moonshot: the Kimi K3 license sets the same $20 million threshold for "Model as a Service" businesses (Simon Willison). Z.ai's GLM 5.3 license sets its bar far higher, requiring a Z.ai security review only above $10 billion in revenue (GIGAZINE).

The attribution clause reaches further than most teams expect. Any product using the model or a "Derivative" must "prominently display 'Built with Muse Spark Open'" in its interface, documentation, website or settings, and Derivatives include any model "created, trained, modified (including fine-tuned), distilled, or otherwise improved" using the software or its outputs (license). A small model you distill from Muse outputs carries the label.

## Where It Trails and Where It Leads

On Meta's own numbers, Muse Spark Open loses on terminal coding and wins on multi-tool agent work. All figures below come from the Hugging Face model card, and none had been independently verified at launch (Implicator).

| Benchmark | Muse Spark Open | GLM 5.3 | Kimi K3 | Qwen3.8 | 
|---|---|---|---|---|
| Terminal-Bench 2.1 | 76.4 | 83.9 | 85.0 | 82.0 | 
| Artificial Analysis Intelligence Index | 39 | 45 | 44 | 40 | 
| SWE-Atlas-QnA | 58 | 59 | 66 | 62 | 
| MCP-Atlas (tool use) | 89.2 | 84.2 | 82.3 | 84.5 | 
| JobBench (agentic) | 61.8 | 61.4 | 54.3 | 52.7 | 
| StrongREJECT v2 (lower is better) | 0.5 | 62.9 | 66.6 | 65.6 | 

MCP-Atlas covers 220 tools across 36 live Model Context Protocol servers, and the model has to pick the tools and their order from a plain-language task (SiliconANGLE). If your workload is an agent orchestrating internal systems, that is the row to weigh. If it is a coding agent working in a shell, Kimi K3's 8.6-point lead on Terminal-Bench is the row.

On the Artificial Analysis index the model's 39 ranks ninth of 118 open-weight models above 150B parameters (OrcaRouter), though Implicator notes that at launch even that figure came from Meta's own table. The StrongREJECT gap says the model is much harder to jailbreak than the Chinese models. Test the other side of that on your own traffic: count how often it refuses prompts your staff are allowed to send.

Treat the table the way you would any vendor's slide: only numbers you can replicate belong in the decision.

## What It Costs to Run

Hosted, it is the cheapest of the four. Together AI lists Muse Spark Open at $1.25 per million input tokens and $4.25 per million output, against $1.40 and $4.40 for GLM-5.3, $2.00 and $6.00 for Qwen3.8-2.4T-A95B, and $3.00 and $15.00 for Kimi K3 (Together AI pricing). Kimi K3's output rate is about 3.5 times Muse's. Artificial Analysis puts Muse at $0.84 per completed Intelligence Index task, 32nd of 40 (OrcaRouter).

Self-hosting is a large commitment. The model card asks for at least 700GB of VRAM for the NVFP4 build and about 2TB for BF16 (model card). Meta's developer post says the NVFP4 checkpoint runs on four Blackwell Ultra GPUs or eight H200s (Meta developer blog). At the full 1M-token context in BF16, the KV cache alone is roughly 266GB (OrcaRouter). Your node loads all 816B parameters even though only 42B fire per token, the same sizing trap as every large MoE, and the quantization choice changes accuracy as well as memory (our format comparison).

The Apple demo is real but narrow. Apple's tests produced 24.99 tokens per second on two 512GB M5 Ultra Mac Studios and 36.73 on four 256GB units, using an 8,192-token context and 128 generated tokens, which says little about long coding sessions. The 256GB configuration starts at $10,799, so four units pass $43,000 (Implicator).

## Who Should Pick Something Else

If your use case sits near Exhibit A, the alternatives cost you either benchmark points or provenance questions. The best US-origin model under a permissive license on the Artificial Analysis board, NVIDIA's Nemotron 3.5 Super VL, scores 26.49 against Muse's 39 (OrcaRouter). Kimi K3, GLM 5.3 and Qwen3.8 score higher, but a utility or defense supplier will face the supply-chain review that Chinese weights now draw, and Kimi K3 and GLM 5.3 ship under custom licenses of their own, not MIT (ChatSlide, GIGAZINE). Have counsel read those files for use restrictions too.

The other US options are still arriving. Reflection's Beam is pitched on Apache 2.0, and Mistral Large 4 had no licence when we last looked. For air-gapped sites, Apache weights on vLLM remain the default that avoids a license review.

## What to Do Before Your Bake-Off

**This Week:**

1. Send Exhibit A sections 2.1 and 2.4 to counsel with three real use cases from your backlog, including the one closest to operations, and ask for a yes or no on each.
2. Pull your Llama acceptable use memo if you have one. The military, nuclear and critical-infrastructure clauses match the Llama 3 policy word for word, so most of the analysis carries over.
3. Decide who submits the gated Hugging Face access form, since it binds the company and cannot be edited afterwards.

**This Month:**

1. Run 50 of your own tasks through Muse Spark Open, Kimi K3 and GLM 5.3 on a hosted API before buying hardware. Score terminal coding and multi-tool orchestration separately, since that is where the published numbers split.
2. Log the refusal rate on prompts your policy allows. StrongREJECT measures resistance to attacks and says nothing about how often the model refuses permitted work.
3. Ask product teams whether anything planned exposes model inference to customers through an API. That is the only route into the $20 million clause, and it needs a Meta agreement before launch.

**Before You Buy Hardware:**

1. Size for at least 700GB of VRAM at NVFP4 plus the KV cache at the context length you actually use, and add the "Built with Muse Spark Open" label to the UX backlog for any product or distilled model that ships on it.

## The Bigger Picture

The leading open-weight labs have converged on revenue triggers: Moonshot and Meta at $20 million for hosted resale, Z.ai at $10 billion for a security review. Meta is the one that also carries category bans over from Llama. For most enterprises the bans never touch a use case, and Muse Spark Open is the cheapest hosted model in its class with the best tool-use score Meta published. For a grid operator, a nuclear supplier or a defense contractor, those two clauses settle the decision before any benchmark runs. Get counsel's yes or no on your use case closest to operations before anyone submits the access form.
