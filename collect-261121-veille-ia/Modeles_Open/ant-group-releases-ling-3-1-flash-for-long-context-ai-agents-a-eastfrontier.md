---
id: collect-261121-veille-ia/modeles-open/ant-group-releases-ling-3-1-flash-for-long-context-ai-agents-a-eastfrontier
title: "Ant Group Releases Ling-3.1-Flash for Long-Context AI Agents Â· EastFrontier"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-01"]
keywords: ["ling-3", "flash", "work", "technode", "reported", "model", "home", "release"]
source: https://eastfrontier.com/2026/10/01/ant-group-releases-ling-3-1-flash-for-long-context-ai-agents/
source_anchor: ""
source_lines: [1, 31]
section: "Modeles_Open"
---
### **A One-Million-Token Target With a 256K Opening**

Ant Groupâs InclusionAI released Ling-3.1-Flash on September 30, presenting a large-model specification aimed at long-running AI-agent work. TechNode reported that the model has about 560 billion total parameters, with about 25 billion activated for each token. The Chinese technology outlet IT Home independently described the same approximate total and per-token active-parameter figures. These are announced specifications reported by the two outlets, rather than independently measured performance results.

The release arrives with an advertised design ceiling of up to one million tokens of context. That number is central to InclusionAIâs description of Ling-3.1-Flash, yet it is a stated maximum, not a published demonstration that the model can reliably process a million-token workload. TechNode does not supply a benchmark score, a quality comparison, a latency figure, or a throughput measurement. IT Home says it appends scores from multiple task evaluations, but neither report provides methodology sufficient for an independent performance comparison, latency, or throughput measurement. The available release details therefore establish the companyâs target configuration without settling how it performs across particular agent tasks.

The initial access terms set a narrower operational boundary. TechNode and IT Home report a two-week free trial with a 256,000-token context limit. TechNode reported that InclusionAI plans to make the larger context window available after that trial. A reader evaluating the release today should distinguish the 256K limit in the trial from the one-million-token context length described for the later rollout.

Ling-3.1-Flash also sits within a model family whose public description is broader than this particular version. The official Ant Ling site describes Ling as a general-purpose large-language-model family built on a mixture-of-experts, or MoE, design. Its page prominently displays Ling-3.0-Flash and general documentation, rather than a Ling-3.1-Flash model card. That supports describing Ling as an MoE-based family, but it does not disclose the new releaseâs expert count, layers, routing method, training data, or activation ratio.

### **Ant Group Positions Ling for Agent, Search, and Office Work**

InclusionAI is positioning the model for agent tasks, search, office software, and specialist applications, according to TechNode. IT Home similarly reported development and optimization goals spanning general-purpose agents, search, daily office work, and software development. The targets frame the release around work that can involve a continuing task history, documents, and code, all categories cited in the reporting alongside the one-million-token context specification. They should be read as the teamâs intended uses and development focus, not proof of production performance in each area.

The agent emphasis connects the release to a recent product direction across Chinaâs AI sector. Earlier coverage of Alibaba Cloudâs agent-control product layer documented another companyâs effort to package agent orchestration for users. Ling-3.1-Flash comes from Ant Groupâs ecosystem, not Alibaba, and the two releases make no shared performance claim. Their common subject is the effort to build and deploy systems that manage multi-step work, rather than simply return a single response.

For Ling-3.1-Flash, the sparse public details are especially important. IT Home reported that the team continues to improve capabilities in medical, financial, and materials-science research scenarios. They do not provide deployment evidence, customer names, test outcomes, or domain-specific evaluations for those fields. Treating those scenarios as validated applications would go beyond the disclosed information. The available account supports a more limited conclusion: Antâs team is naming those areas as part of its continuing development focus.

The parameter figures also require a similarly careful reading. About 25 billion activated parameters per token is a reported specification within a model said to have about 560 billion total parameters. It does not, on its own, reveal the modelâs cost, inference speed, hardware requirements, or the exact amount of computation involved in a real workload. The official Ling family description identifies MoE as the design category, while the release reporting leaves Ling-3.1-Flashâs specific internal arrangement undisclosed.

### **The Open-Source Plan Remains a Future Step**

After the two-week trial, InclusionAI plans to enable the one-million-token context window and release Ling-3.1-Flash as open source, TechNode reported. IT Home likewise described the larger window and open-source release as planned after the free experience ends and the service becomes paid. The timing language matters: neither report says the modelâs weights are already open source, and neither announces a completed public download.

The official availability pages reinforce that distinction. Ant Ling links visitors to documentation, a chat or studio entry point, and a Hugging Face page for Ling-3.0-Flash. InclusionAIâs public GitHub and Hugging Face organization pages identify an Ant Group AGI initiative and list earlier Ling materials, but the navigated pages did not show a Ling-3.1-Flash repository or model card. Those pages establish an official ecosystem for Ant Groupâs AI work; they do not establish that the September 30 model is currently downloadable or open-weight.

That pending state sets Ling-3.1-Flash apart from completed releases already visible in the same ecosystem. It also provides a narrow point of comparison with MiniMaxâs M3.1 Flash Preview, another recent flash-branded model update covered by EastFrontier. The two announcements should not be treated as a benchmark matchup because the Ling reports provide no comparative test data. In Lingâs case, the verifiable near-term facts are the September 30 launch, the two-week 256K trial, and InclusionAIâs stated plan for a larger context option and an open-source release afterward.

InclusionAIâs official site says it is backed by Ant Group, while its GitHub organization characterizes its projects as part of Ant Groupâs AGI initiative. That relationship explains why the release is presented as an Ant Group launch while the model work is associated with InclusionAI and Ant Ling. It does not require treating InclusionAI as a legally separate company, a status not established by the public pages reviewed.

For developers and enterprises assessing the announcement, the useful facts are deliberately bounded. Ling-3.1-Flash has been released with reported approximate parameter counts and a long-context ambition; the free trial reportedly exposes 256,000 tokens for two weeks; and the one-million-token option plus open-source availability remain plans for after that period. Evidence on price, API limits outside the stated trial context, model weights, benchmarks, and real-world reliability has not been disclosed in the reporting or on the official pages available for this release.
