---
id: collect-261121-veille-ia/muse/meta-releases-816b-parameter-muse-spark-open-for-coding-and-agent-work-siliconangle
title: "Meta releases 816B-parameter Muse Spark Open for coding and agent work - SiliconANGLE"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["meta", "muse", "model", "spark", "open", "coding", "agent", "work"]
source: https://siliconangle.com/2026/10/15/meta-releases-816b-parameter-muse-spark-open-for-coding-and-agent-work/
source_anchor: ""
source_lines: [1, 41]
section: "Muse"
---
### Meta releases 816B-parameter Muse Spark Open for coding and agent work

Meta Platforms Inc. today released open weights for its Muse Spark artificial intelligence model, which had been proprietary since its April debut.

The release, called Muse Spark Open, is built for coding and long-running agent work. Meta describes it as the most capable open-weight model to come out of a U.S. lab so far. Only 42 billion of the model’s 816 billion parameters switch on for any given token. Prompts can mix text with images, audio or video, and Meta has set the context window at up to 1 million tokens.

The starting point was Muse Spark 1.2, shipped in August, and Meta then added safety post-training so the model holds up without the guardrails of Meta’s own services. Meta said Muse Spark Open cleared the release thresholds in its Meta Superintelligence Scaling Framework, with a full evaluation report still to come. A technical writeup from Hugging Face Inc. says training leaned on what Meta calls “real developer workflows,” including multi-file refactors and long debugging sessions.

Meta also tested the model away from the data center, on a pair of Apple Inc. Mac Studio desktops with M5 Ultra chips that Apple’s MLX framework linked into one system. The two machines had about a terabyte of memory between them.

Muse Code ran on that setup while the coding terminal worked through bug fixes and new features for the open-source Mastodon client Ice Cubes. Footage in DaVinci Resolve was the other test, and the model went through it frame by frame to pick out and crop the best shots for vertical video.

In Meta’s own benchmark table, Muse Spark Open scored 61.8 on the JobBench agent test, more than twice the 31.1 recorded by Inkling, the U.S. open model from Thinking Machines Lab Inc. Nvidia Corp.’s Nemotron 3 Ultra trailed further at 27.2.

Chinese open models are stronger at coding. Kimi K3 led the Terminal-Bench 2.1 coding test at 85, and GLM 5.3 and Qwen3.8 both finished above Muse Spark Open’s 76.4.

On tool use the order flips, and Muse Spark Open’s 89.2 on MCP-Atlas beat every rival Meta listed, the Chinese models included. The benchmark gives a model plain-language tasks without naming the tools needed to complete them. Working out which of 220 tools across 36 live Model Context Protocol servers to call, and in what order, is left to the model.

The weights come with a new Muse Model License carrying what Meta calls “limited commercial terms,” and no fee applies to developers who adapt the model or ship it inside their own software. Meta requires a separate agreement from any company that sells the model as a hosted service and has passed $20 million in gross revenue over a 12-month period.

Products built on it have to show a “Built with Muse Spark Open” label. Military and critical infrastructure uses are off-limits.

Meta’s previous open-weight release was the much smaller Muse Glimmer, a 30 billion-parameter model that went out in August. The company’s most capable systems remain closed, and the Muse Wonder models introduced Monday are available only through the Meta Model API and Muse Code.

Seven inference providers offer hosted access at launch, Groq Inc. and Fireworks AI Inc. among them. The weights can also be downloaded from Hugging Face or from the model catalogs of Amazon Web Services Inc., Google LLC and Microsoft Corp.

##### Image: Meta

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
