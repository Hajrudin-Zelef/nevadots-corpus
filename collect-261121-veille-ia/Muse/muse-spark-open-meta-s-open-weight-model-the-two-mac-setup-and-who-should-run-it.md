---
id: collect-261121-veille-ia/muse/muse-spark-open-meta-s-open-weight-model-the-two-mac-setup-and-who-should-run-it
title: "Muse Spark Open: Meta's Open-Weight Model, the Two-Mac Setup, and Who Should Run It"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "model", "meta", "open", "metas", "card", "post"]
source: https://digitalmatters.me/artificial-intelligence-ai/meta-muse-spark-open/
source_anchor: ""
source_lines: [1, 147]
section: "Muse"
---
**Muse Spark Open** is an open-weight model Meta released on October 15, 2026, with 816 billion total parameters, 42 billion active per token and a 1 million token context window. Meta built it for coding and agent work, and it worked with Apple so the model runs on two linked Mac Studios with the M5 Ultra chip.

This post covers what Meta shipped, how it differs from Muse Spark 1.1 and the Muse agent, what it takes to run at your desk, and what the license says. It ends with whether an agency or nonprofit should use it. It reflects Meta’s and Apple’s pages as of October 2026.

The short version: Muse Spark Open is free to download for most organizations, but it is not cheap to run. Apple’s own test needed two 512GB Mac Studios to fit it, and that memory option ships in late October. Most teams will try it through a hosted API first. Read the license before you build on it: it requires a "Built with Muse Spark Open" notice in your product, and Meta says the model is not intended for anyone under 18.

## What Meta released with Muse Spark Open

Meta’s launch post from Meta Superintelligence Labs calls it "the most capable US open-weight model to date." That is Meta’s claim, and the benchmark table on its own model card shows a mixed picture (more on that below).

The Hugging Face model card fills in the details.

| Spec | Muse Spark Open | Source | 
|---|---|---|
| Size | 816B total, 42B active, mixture of experts (8 of 256 experts per token) | Model card | 
| Context | 1M tokens | Meta blog, model card | 
| Input and output | Text, image, video and audio in; text out | Model card | 
| Weights | BF16 and NVFP4 (4-bit) | Hugging Face collection | 
| License | Muse Model License, limited commercial terms | Meta blog, model card | 

There is one model, in two weight formats. Meta did not release smaller sizes. The weights are on Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry. Hosted access is through Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI. Meta’s post lists no prices, so each provider sets its own.

## How it differs from Muse Spark 1.1 and Meta Muse

Meta now uses the Muse name for several products. Here is how this one fits.

**Muse Spark 1.1** is Meta’s closed-weight model, sold through the paid Meta Model API. We covered it in our Muse Spark 1.1 explainer. You cannot download it. Muse Spark Open’s weights you can download and run yourself. Meta’s model menu also lists a Muse Spark 1.2, which we have not covered.

**Meta Muse** is the consumer agent that runs on a cloud computer Meta operates. Our Meta Muse explainer covers it. Muse Spark Open is a model, not an agent product.

**Muse Code** is Meta’s terminal coding tool, from our Muse Code post. Meta tested Muse Spark Open with it on Macs. Meta’s examples include bug fixes, new features and a full build of Ice Cubes, an open-source Mastodon client, plus driving DaVinci Resolve to review and crop video.

Meta’s blog post does not compare Muse Spark Open with Muse Spark 1.1 directly. On Terminal-Bench 2.1, Meta’s card lists the open model at 76.4. Our July post reported Muse Spark 1.1 at about 80.0 from third-party testing. Different test conditions, so treat that gap as directional.

## How Muse Spark Open scores against other open models

All of these numbers are Meta’s, run at its highest reasoning setting. The comparison models come from Meta’s own table.

| Benchmark | Muse Spark Open | Best other open model on Meta’s table | 
|---|---|---|
| MCP-Atlas (tool use) | 89.2 | Qwen3.8: 84.5 | 
| OSWorld Verified (computer use) | 80.4 | Kimi K3: 84.8 | 
| Terminal-Bench 2.1 (coding) | 76.4 | Kimi K3: 85.0 | 
| DeepSWE 1.1 (long coding) | 60 | Kimi K3: 68 | 

The pattern matches the closed Spark models. It leads on tool use and trails on harder coding. On Artificial Analysis’s combined index, Meta’s table lists it at 39, behind GLM 5.3 at 45.

## What it takes to run Muse Spark Open locally

This is where the headline and the hardware part ways. Apple’s Machine Learning Research team published a technical write-up on the setup the same day.

Apple’s numbers are plain. In BF16, the parameters alone need over 1.6TB. In 4-bit NVFP4, they need about 493GB. That still does not fit on one 512GB Mac Studio once the software’s own memory use is added. So the minimum is two machines.

The Macs connect over Thunderbolt 5, using RDMA support added in macOS 26.2, and MLX version 0.30.1 or later. Apple measured these speeds at an 8,192-token context:

| Setup | Memory | Speed (tokens per second) | 
|---|---|---|
| Two M5 Ultra Mac Studios, split by layer | 1TB total | 15.74 | 
| Two M5 Ultra Mac Studios, split by tensor | 1TB total | 24.99 | 
| Four M5 Ultra Mac Studios, split by tensor | 2TB total | 36.73 | 

About 25 tokens per second is usable for one person. Apple did not publish results near the 1 million token limit. Long contexts need more memory, so expect less room in practice.

On a GPU server, the model card says NVFP4 needs at least 700GB of GPU memory, and BF16 needs 2TB. That is a multi-GPU server, not a workstation.

## What the hardware costs

Apple’s Mac Studio announcement lists the M5 Ultra model from $5,499. That base model does not have enough memory. Apple says the 512GB configuration is "coming in late October," and it has not posted a price for it in the pages we read.

MacRumors reports that the 256GB configuration already costs over $10,000. Two 512GB machines will cost more than $20,000 before cables, storage and someone’s time to set them up.

Memory prices are the reason. Our local AI hardware prices post traced how 2026 memory shortages pushed desktop AI hardware up while API prices held. That math applies here too.

## What Apple contributed

Apple’s part is hardware and software. Meta says it optimized the model for MLX, Apple’s open-source machine learning framework, and for the M5 Ultra Mac Studio. Meta also provides an MLX adapter that pulls the weights from Hugging Face.

Apple’s write-up supplies the parts that make two Macs act as one: the Thunderbolt 5 link, RDMA in macOS and MLX’s distributed backend. Neither company’s page describes a financial deal. MacRumors reports that Apple CEO John Ternus congratulated Meta in a post on X.

## The Muse Model License, in plain terms

The license is the part to read before building anything. The model card says it is not an OSI-approved open-source license. Four terms matter most for web teams.

- **Attribution.** You must “prominently display” the phrase “Built with Muse Spark Open” in the product’s interface, documentation, website or settings.
- **Hosting for others.** If you offer the model to third parties as a service and your revenue tops $20 million a year, you need a separate agreement with Meta. Internal use and powering your own product’s features are excluded.
- **Age.** Meta’s usage policy says the model “is not intended for individuals under the age of 18.”
- **Access.** Downloading requires a Hugging Face account and a form with your legal name, date of birth and organization name.

Meta says it "claims no ownership of outputs."

**Education clients, read the age line.** If you build for K-12 schools, youth programs or any audience that includes minors, Muse Spark Open’s usage policy says the model is not intended for them. Check this before proposing it to a client.

## Should an agency or nonprofit use Muse Spark Open?

**Privacy.** Running the model on your own Macs means prompts and documents stay on hardware you control. That is the strongest reason to look at it. But a local model is not automatically secure, as our local AI agent security post showed.

**Cost.** Over $20,000 in hardware buys about 25 tokens per second for one user. For most small teams, a hosted API will cost less unless volume is high or data rules require local hosting. Our Kimi K3 self-hosting post walks through the same trade-off.

**Fit.** For most nonprofit work, such as drafting, summarizing and tagging content, a smaller model is enough. Meta’s own Muse Glimmer is a 30B model under Apache 2.0 that runs on one GPU. Our local AI models guide covers that range.

Where Muse Spark Open does make sense: an agency doing heavy coding or agent work on client code that cannot leave the building, with budget for two top-tier Mac Studios.

## How it compares with other open-weight releases

Other labs have taken similar paths with different licenses. DeepSeek V4 Pro uses the MIT license at 1.6 trillion parameters, per our DeepSeek V4 Pro post. Meta’s earlier Llama models used the Llama Community License, with a 700 million monthly user threshold, as covered in What Is Llama?. Muse Spark Open’s license is tighter than MIT or Apache 2.0, and closer to Llama’s. Our open vs closed AI models explainer covers what those labels mean.

## Frequently Asked Questions

## What is Muse Spark Open?

An open-weight model from Meta Superintelligence Labs, released October 15, 2026. It has 816 billion total parameters, 42 billion active, a 1 million token context and text output. Meta built it for coding and agent tasks.

## Is Muse Spark Open open source?

No. The weights are downloadable, but the Muse Model License is not an OSI-approved open-source license. It adds attribution, age and large-scale hosting terms.

## Can I run Muse Spark Open on one Mac?

Not according to Apple’s testing. The 4-bit version needs about 493GB for parameters alone, which does not fit on one 512GB Mac Studio once overhead is added. Apple’s tests used two or four linked M5 Ultra Mac Studios.

## How fast is it on two Mac Studios?

Apple measured about 25 tokens per second on two M5 Ultra Mac Studios and about 37 on four, at an 8,192-token context.

## Where can I download it?

On Hugging Face after filling out an access form, and through Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry.

## Can I use it without buying hardware?

Yes. Meta lists Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI as hosted providers. Meta did not publish prices, so compare providers.

## What does the license require?

A “Built with Muse Spark Open” notice in your product. A separate agreement with Meta if you resell model access and earn over $20 million a year. No use by people under 18.

## How is it different from Muse Spark 1.1?

Muse Spark 1.1 is closed weight and sold through Meta’s paid API. Muse Spark Open’s weights can be downloaded and run on your own hardware.

## What did Apple contribute?

The Mac Studio hardware, the MLX framework, and macOS support for linking Macs over Thunderbolt 5 with RDMA. Apple also published the speed and memory tests.

## Should a small nonprofit run it locally?

Usually not. The hardware costs over $20,000, and a hosted API or a smaller open model will handle most nonprofit work. Local hosting makes sense when data cannot leave your control and the budget allows.
