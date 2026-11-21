---
id: collect-261121-veille-ia/actu-business/meta-released-an-816-billion-parameter-open-weight-model-that-runs-on-two-mac-studios
title: "Meta Released an 816-Billion-Parameter Open-Weight Model That Runs on Two Mac Studios."
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["model", "meta", "open-weight", "license", "muse", "open", "runs", "spark"]
source: https://medium.com/@andy.a.g/meta-released-an-816-billion-parameter-open-weight-model-that-runs-on-two-mac-studios-5a30f30f4277
source_anchor: ""
source_lines: [1, 63]
section: "Actu_Business"
---
# Meta Released an 816-Billion-Parameter Open-Weight Model That Runs on Two Mac Studios. That Is a Terabyte of Memory, and a License With a $20 Million Line.

## On October 15, Meta published the weights of Muse Spark Open, now the top US open-weight model on Artificial Analysis. The license is free for most product use, but it requires a "Built with Muse Spark Open" credit, covers models trained on its outputs and sends model-as-a-service businesses above $20 million in revenue to Meta for a separate deal. Here is how I read it as a CTO weighing self-hosting against an API.

On Thursday, October 15, Meta released the weights of Muse Spark Open, which it calls "the most capable US open-weight model to date." It is a mixture-of-experts model with 816 billion parameters, 42 billion of them active for each token, a 1-million-token context window, and text, image, video and audio input. The weights are on Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry, and seven hosts serve it through an API, including Together AI, Fireworks AI, Groq and OpenRouter.

Artificial Analysis, which had access before release, puts it at 39 on its Intelligence Index: first among US open-weight models, 22 points above Meta's own Muse Glimmer from August. But the headlines were about hardware. TechCrunch's read "Meta launches an open-weight AI model that runs on Macs"; VentureBeat's said it "runs on two paired Mac desktops." Meta says it is "the first model in its class to run on your desktop."

I build a LegalTech on Claude. For a product whose users upload documents they would not show a competitor, the case for open weights is simple: run the model where the documents already are. So I read this release the way I read any self-hosting option. License first, memory second, bill third. In September I wrote that Meta had gone back to Apache 2.0 with Glimmer. Its big model is a different deal.

## The license: free, with three lines to read twice

The weights ship under the Muse Model License. Its opening words are borrowed from the MIT license: "Permission is hereby granted, free of charge, to any person..." The rights are broad: use, copy, fine-tune, distill, merge, distribute, sell. Then come the conditions.

First, the revenue line. Section 3 covers Model as a Service, which the license defines as providing "access to artificial intelligence models as a product or service, via API or a similar interface." If the licensee and its affiliates together have gross revenue above $20 million in any consecutive 12-month period, they must notify Meta and sign a separate written agreement before offering Muse Spark Open or its derivatives that way. The section explicitly does not apply to internal use, research, other commercial activity, or using the model to power features of your own product.

Two things follow. The threshold counts the whole group's revenue, not what the model earns. And for most software companies, which put a model behind features rather than resell access to it, the clause does not bite. It reads as written for inference hosts and API resellers.

Second, attribution. Each product or service must "prominently display" the phrase "Built with Muse Spark Open", in the user interface or in the accompanying documentation, website or settings menu. For a B2B product, that makes your model choice public. In legal software, clients ask that question anyway, but I would decide where the line goes before a designer does.

Third, the definition of Derivatives covers models improved "using the Software or any outputs or results thereof." If you use Spark Open to generate synthetic training data for a smaller model, that smaller model is a derivative, and the attribution and the Usage Policy travel with it.

The Usage Policy, Exhibit A, has one line a LegalTech CTO reads slowly: you may not "engage in the unauthorized or unlicensed practice of any profession." Another forbids failing to disclose to end users any known dangers of your AI system. Neither is exotic. Both mean the professional stays in the loop by design, not by habit.

Two practical details. For licensees in Europe, the licensor is Meta Platforms Ireland Limited. And downloading from Hugging Face goes through a gated form that asks for your full legal name, date of birth and full organization name.

Meta's own summary is honest: "limited commercial terms, which allow many developers to use the model for free." Many is accurate. It is not Apache 2.0.

## The desktop: a terabyte of it

Meta's desktop setup is two Mac Studios with M5 Ultra, each with up to 512 GB of unified memory, linked so they "behave like one supercomputer with a terabyte of unified memory." Meta's developer guide also allows four Mac Studios with 256 GB each.

The arithmetic explains why. Artificial Analysis puts the NVFP4 weights at about 530 GB. Hugging Face calculates that the cache for a full 1-million-token context adds about 266 GB in BF16. That is roughly 800 GB before the operating system loads. The model card lists at least 700 GB of GPU memory for NVFP4 and 2 TB for BF16. Mixture-of-experts makes each token cheap to compute, not the model cheap to hold: as Hugging Face puts it, all 256 experts stay in memory.

Then availability. Apple said on August 25 that the Mac Studio with 512 GB "is coming in late October," and VentureBeat noted on launch day that its price was unannounced. The four-machine route could already be ordered on launch day. Engadget put four 256 GB M5 Ultra Mac Studios at roughly $38,000; VentureBeat priced a 256 GB M5 Ultra with a 1 TB SSD at $10,799 on Apple's store, or $43,196 for four, before tax.

Smaller is possible, with trade-offs. Hugging Face ran a 3-bit quantization on a previous-generation 512 GB M3 Ultra: about 344 GB of memory and about 21 tokens per second of generation, on a 1,024-token prompt. That is fine for one person chatting. It is not a server for a team.

On GPUs, Meta lists a single node of four Blackwell GPUs for NVFP4. Hugging Face's managed endpoint runs it on eight RTX Pro 6000 cards with a 131,072-token context, or on eight H200s for the full million. The context window you get depends on what you rent.

## The bill: cheaper per token, not per task

If you do not self-host, Together AI lists Spark Open at $1.25 per million input tokens and $4.25 per million output tokens, with cached input at $0.15. Artificial Analysis says Together, Fireworks and Baseten launched at those rates.

A product call of 4,000 tokens in and 500 out costs 4,000 × $1.25 / 1M = $0.005, plus 500 × $4.25 / 1M = $0.0021, so about $0.0071. The same call on Claude Sonnet 5.5, at $2 and $10, costs $0.013. At that mix of tokens, Spark Open is about 45 percent cheaper.

Per task, the order flips. On the Intelligence Index, Artificial Analysis measures $0.84 per task for Spark Open at its highest reasoning setting. Sonnet 5.5 at medium effort scores 41, two points higher, for $0.48. Spark Open generated 110 million output tokens to run the index; Sonnet 5.5 at medium, 31 million. And per Hugging Face, reasoning cannot be turned off on Spark Open. The lowest setting is "low".

One more line from Artificial Analysis. Among the nine highest-scoring open-weight models, Spark Open ranks last on the index's four agentic evaluations, while it leads every open-weight model on AA-Omniscience, its test of knowledge reliability and hallucination. Meta built it "for coding and agentic work." Your own evaluation set should test that claim on your own work, in both directions.

## What I would do before choosing

First, classify your use against Section 3, in writing. Product feature or Model as a Service? Group revenue over $20 million in any 12 months? Ten minutes now saves a renegotiation later.

Second, decide where "Built with Muse Spark Open" goes, and add the model to your inventory with its license. If you distill from it, tag the derived model too.

Third, test it hosted before buying anything. Run your own tasks on a launch host at the "low" reasoning setting and at the default, and record cost per completed task, tokens included. If your documents are not in English, test that as well: the model card says it was trained on data from more than 100 languages and "has not been evaluated on all languages contained in the pre-training data." I built BidForge to read French public tender files for small businesses. I would not take French quality on trust.

Fourth, price the hardware only if the reason is data. Self-hosting earns its place when documents cannot leave your premises, not because tokens are expensive. At $0.0071 a call, the roughly $38,000 Engadget quotes for four Mac Studios buys more than five million hosted calls of the size above, before electricity or anyone's time keeping the machines running.

Open weights tell you who may run a model. The license tells you on what terms, and the memory bill tells you who actually will.

Sources: Meta, "Muse Spark Open", https://research.meta.ai/blog/muse-spark-open (October 15, 2026); Meta AI for Developers, "Build with Muse Spark Open", https://dev.meta.ai/resources/blog/build-with-muse-spark-open (October 15, 2026); Hugging Face, "meta-models/Muse-Spark-Open-816B-A42B" model card, https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B (read October 19, 2026); Meta, "Muse Model License", https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B/blob/main/LICENSE.md (release date October 15, 2026); Hugging Face, "Welcome Muse Spark Open: open-weight frontier multimodal mixture-of-experts model from Meta", https://huggingface.co/blog/meta-muse-spark-open (October 15, 2026); Artificial Analysis, "Muse Spark Open (Xhigh) Intelligence, Performance & Price Analysis", https://artificialanalysis.ai/models/muse-spark-open (read October 19, 2026); Artificial Analysis on X, launch analysis of Muse Spark Open, https://x.com/ArtificialAnlys/status/2110893784422908225 (October 16, 2026); Artificial Analysis, "Claude Sonnet 5.5 (Medium, Default Fallback) Intelligence, Performance & Price Analysis", https://artificialanalysis.ai/models/claude-sonnet-5-5-medium (read October 19, 2026); Anthropic, "Pricing", https://docs.claude.com/en/docs/about-claude/pricing (read October 19, 2026); Artificial Analysis, "Muse Glimmer (High) Intelligence, Performance & Price Analysis", https://artificialanalysis.ai/models/muse-glimmer (read October 19, 2026); Together AI, "Pricing", https://www.together.ai/pricing (read October 19, 2026); Apple Newsroom, "Apple introduces new Mac Studio with M5 Max and M5 Ultra", https://www.apple.com/newsroom/2026/08/apple-introduces-new-mac-studio-with-m5-max-and-m5-ultra/ (August 25, 2026); VentureBeat, "Meta unveils Muse Spark Open, the new top U.S. open weights model, and it runs on two paired Mac desktops", https://venturebeat.com/technology/meta-unveils-muse-spark-open-the-new-top-u-s-open-weights-model-and-it-runs-on-two-paired-mac-desktops (October 15, 2026); Engadget, "Meta built a huge open-weight AI model for Macs, but you'll need 1TB of RAM to run it", https://www.engadget.com/2287833/meta-built-a-huge-open-weight-ai-model-for-macs-but-youll-need-1tb-of-ram-to-run-it/ (October 15, 2026); TechCrunch, "Meta launches an open-weight AI model that runs on Macs", https://techcrunch.com/2026/10/15/meta-launches-an-open-weight-model-that-runs-on-macs/ (October 15, 2026).
