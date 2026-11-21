---
id: collect-261121-veille-ia/modeles-open/reflection-beam-open-weight-model-provenance-is-the-product
title: "Reflection Beam Open Weight Model: Provenance Is the Product"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "model", "reflection", "open", "models", "qwen", "kimi", "flash"]
source: https://www.ctol.digital/news/reflection-ai-beam-open-weight-model-provenance-benchmarks/
source_anchor: ""
source_lines: [1, 19]
section: "Modeles_Open"
---
*Beam matches GLM 5.2 on coding and trails Qwen, GLM 5.3, Kimi K3 and DeepSeek V4.1 Flash by 6 to 30 points. What Reflection offers buyers is Apache licensed weights from a Western lab.*

A lab using more than ten thousand Nvidia GB300 GPUs for four weeks on a single training run expects the result to justify that cost. Reflection AI's result, published Monday in its launch post, is Beam. The accompanying table puts Beam level with GLM 5.2 and behind four other open models, all Chinese. Beam is therefore more a policy product than a capability product. Its target buyers are banks, governments and enterprises that will not run Chinese weights; for them, the model's origin is the feature.

The specification is large by Western standards. Beam is a sparse mixture of experts model with 501 billion total parameters and 23 billion active on any token, built for coding, reasoning and agentic work, and pretrained on 23.8 trillion tokens. It is text only. Reflection says the weights, a technical report and a model card will follow later this month under an Apache 2.0 licence, and that the model is still in final red teaming and evaluation. Early access is by waitlist.

Those claims are modest where it counts. On Terminal Bench v2.1, Beam scores 80.1 against 81.0 for GLM 5.2, 86.6 for Qwen 3.8 Max, 88.2 for GLM 5.3, 88.3 for Kimi K3 and 90.6 for DeepSeek V4.1 Flash. On DeepSWE v1.1 it scores 44.4, ahead of GLM 5.2 at 44.0 and then a long way behind Qwen 3.8 Max at 51.0, GLM 5.3 at 61.0, Kimi K3 at 68.0 and DeepSeek V4.1 Flash at 74.2. Beyond GLM 5.2, the gap to Qwen 3.8 Max is 6.5 points on Terminal Bench and 6.6 on DeepSWE, and to the strongest entry, DeepSeek V4.1 Flash, it is 10.5 and 29.8. The blog says Beam is approaching Qwen 3.8 Max on coding and agentic tasks, which the table supports on one test and strains on the other.

Reflection's table also compares Beam with Nemotron 3 Ultra, a Western model. Beam scores 80.1 to 56.4 on Terminal Bench and 80.9 to 70.7 on SWE Bench Verified. On SWE Bench Pro v2 Hard, it scores 77.2, behind GLM 5.3 at 84.3 and Kimi K3 at 88.2. Taken together, Beam is the strongest Western open-weight model in Reflection's set and level with the weakest Chinese model. Buyers may pay for that position when choosing between Beam and a Western alternative; on capability alone, it is a weak case.

The compute disclosures show where Reflection placed its bet. Reinforcement learning ran on 10.5 thousand GB300 GPUs for four weeks and generated more than 100 million rollouts, which the company calls one of the largest RL runs by any open lab. Pretraining took under four weeks on 6,144 of the same chips. In round numbers, the RL stage used about 7.1 million GPU hours and pretraining at most 4.1 million, so reinforcement learning drew at least 1.7 times the GPU time of pretraining. The spend tilts toward post training, which fits a model sold for coding and agentic work.

Reflection's finances affect what buyers can rely on. Axios reported on October 4 that the Nvidia-backed startup expects its model to "initially lag behind the most cutting edge U.S. models and be competitive with top Chinese open weight models". It has also signed large deals with Nebius and SpaceX to rent Nvidia servers. Future training runs depend on that capacity and the financing behind it, which gives enterprises a reason to want a licence that survives whatever happens to the lab. Apache 2.0 provides one: once released, the weights can be run, audited and modified inside the customer's own walls.

Provenance matters because of who the customers are. Open weight models are a small share of enterprise API usage, though they can account for a majority of usage on multi model platforms. Artificial Analysis, which runs independent evaluations, lists MiMo V2.6 Pro and GLM 5.3 as the highest intelligence open source models, followed by Kimi K3 and GLM 5.3 Flash, and all four come from Chinese labs. That pattern describes a market where open models are chosen by engineers for cost and control, then constrained by compliance. A bank or a government department that has ruled out Chinese weights has a short list, and Nemotron 3 Ultra is on it. Beam's 23.7 point lead over that model on Terminal Bench is the commercial case. The six to 30 point deficit to Chinese models is the price of the restriction.

Efficiency is Beam's strongest argument. It activates 23 billion parameters, far fewer than the larger models in the comparison. Reflection says Beam matches GLM 5.2 on advanced reasoning while using three to four times less inference compute. The company's estimate puts floating-point operations at about twice the active parameters multiplied by mean generated tokens. A model that solves fewer tasks at a fraction of the cost can still win on cost per solved task, especially for customers running coding agents at scale. The argument is fair, but rests on Reflection's estimate rather than a measured bill. The weights have not shipped, so outsiders cannot test it. The post acknowledges that Kimi K3 and other frontier open models remain ahead on raw capability. Other Western open-weight models are due this month, which could crowd the field Beam currently leads within weeks.

When the weights ship before the end of October, independent coding evaluations will put Beam below Kimi K3 and GLM 5.3 and above every Western open model. That ranking, and the inference cost people measure once they can run it, will decide whether provenance alone is enough to sell a model.
