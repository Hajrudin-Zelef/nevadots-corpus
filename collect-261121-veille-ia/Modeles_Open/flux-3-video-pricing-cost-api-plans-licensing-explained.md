---
id: collect-261121-veille-ia/modeles-open/flux-3-video-pricing-cost-api-plans-licensing-explained
title: "Flux 3 Video Pricing: Cost, API Plans, & Licensing Explained"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["video", "flux", "pricing", "licensing", "second", "plans", "model", "audio"]
source: https://www.layer3labs.io/guides/flux-3-video-pricing
source_anchor: ""
source_lines: [1, 100]
section: "Modeles_Open"
---
# Flux 3 Video Pricing: Plans, API Costs, and Licensing

Understanding the cost structure and licensing for Black Forest Labs' multimodal model

In 2026, Black Forest Labs introduced Flux 3, a multimodal AI model designed for generating video, audio, and images from text, images, and keyframes. Flux 3 supports native audio, multilingual speech, and outputs up to 20-second video clips in one generation, with options for styles and multiple shots.

Flux 3 is different from prior video models like Sora or Veo by offering a single model that handles not just video, but also audio, image, and action-prediction tasks. Its workflow allows starting from multiple input types and integrating speech or background audio directly within generated videos, and it is available through an API and an in-browser playground.

For regulated industry teams exploring advanced video automation, Flux 3's API access and private weight licensing options change how you can deploy AI in workflow-sensitive settings. Understanding its pricing and licensing is key if you need to manage compliance risk, control data residency, or negotiate private weights through BFL sales.

## Published FLUX 3 Video Rates (October 2026)

Black Forest Labs (BFL) prices FLUX 3 Video from $0.06 per second for drafts to $0.45 per second for video continuation. Standard text-to-video and image-to-video generation costs $0.15 per second at 720p, $0.24 at 1080p, $0.28 at 1440p, and $0.41 at 2160p on BFL's pricing docs. Billing is pay as you go in credits, at $0.01 per credit. The API and Playground charge the same rates, with no subscription or seat fee.

Video clips run up to 20 seconds, and total generation costs scale directly with clip duration and output resolution. A 10-second clip at 1080p costs $2.40 (10 × $0.24). A 20-second 720p draft costs $1.20 (20 × $0.06). A 720p draft can be enhanced into a full-quality render. Output above 720p is the same generation finished by BFL's video upsampler, billed at the delivered resolution.

| Mode | 720p | 1080p | 1440p | 2160p | 
|---|---|---|---|---|
| Text or image to video | $0.15 | $0.24 | $0.28 | $0.41 | 
| Video continuation | $0.21 | $0.27 | $0.31 | $0.45 | 
| Draft (720p only) | $0.06 | N/A | N/A | N/A | 

Buyers evaluating still image generation can consult FLUX 3 Image pricing, or examine the underlying architecture in FLUX 3 Video explained.

## Overview of Flux 3 Video Pricing Structure

Flux 3 Video's pricing structure includes multiple options: pay-as-you-go API plans and enterprise agreements for large-scale deployments. The model is offered through a public API, Playground access, and private weight terms negotiated through BFL sales.

API plans provide scalable access for production workloads, while companies that want weights must contact BFL sales. For organizations with strict compliance and integration requirements, enterprise agreements provide tailored commercial terms and support.

BFL now lists per-second API rates on its pricing docs (see the first section). Private weight access and enterprise terms are not listed and go through BFL sales.

## API Pricing and Billing Models

Flux 3 Video API is designed for scalable, production use, with a pay-as-you-go or custom volume-based billing model. The API gives users direct programmatic access to generate video, audio, and images.

Users can start in the playground for initial experiments and later integrate the API for larger or automated workflows, with scaling supported by the platform.

BFL bills the API per second of output; the rate table in the first section lists each mode and resolution.

- Pay-as-you-go or custom enterprise billing models
- Billed per second of output, by mode and resolution
- Per-second rates published on BFL's pricing docs
- API supports text-to-video, image-to-video, and audio integration in a single workflow

## Private Weights and Commercial Licensing Model

Black Forest Labs offered private weight access during FLUX 3 Video's early access. Companies that want weights for data governance or customization must now contact BFL sales.

FLUX 3 Video weights have never been published for public download.

API rates are published per second, but BFL publishes no prices, restrictions, or terms for weight access.

BFL displays company-level SOC 2 Type II and ISO 27001:2022 badges in its site footer, though it does not state that they cover FLUX 3 Video specifically.

- Weight access is handled case by case through BFL sales, with no published terms
- Public self-hosting is not available; private weights require sales arrangement
- Check license details and costs before deploying for commercial use

## Playground Access and Absence of Free Tier

Flux 3 Video is accessible through a playground that lets users experiment with model capabilities without writing code or signing a long-term agreement.

Black Forest Labs' API pricing lists no free tier, free credits, or trial. Playground access uses the same pay-as-you-go credit pricing as the API ($0.01 per credit).

Users must purchase credits to test the model, as BFL offers no free trial or public self-hosted option.

## Worked Cost Examples for Common Use Cases

Flux 3 Video bills per second of output, so these examples use BFL's published rates from the first section.

For short-form social video (up to 20 seconds with audio), product demos, or quick test generations, cost depends on total seconds rendered and output resolution.

A 20-second social clip at 1080p costs $4.80 (20 × $0.24), and testing it first as a 720p draft costs $1.20 (20 × $0.06).

In one Layer3 Labs gaming client case (where private model weights were negotiated for in-house content controls), commercial license negotiations with the model vendor introduced contract terms around data residency and volume tiers that were not obvious from the product site. This sort of negotiation is likely for any deployment in regulated industries.

- Social video (10 to 20 seconds): $1.50 to $3.00 at 720p, or $2.40 to $4.80 at 1080p
- Product demos with audio: likely higher due to media complexity
- Custom volume discounts available in enterprise terms
- Always request a worked quote aligned to your actual output and compliance requirements

## Flux 3 Video Pricing vs Other Video Models

Like Veo, Kling, Seedance, and Runway, FLUX 3 Video is a closed-weight cloud API model for public use, distinguishing itself instead through native synchronized audio, draft mode, and keyframe inputs.

BFL publishes per-second API rates, so Flux 3 Video costs can be compared directly with other video models, while custom enterprise and private weight terms require a quote. Firms in regulated sectors may seek private weight agreements to negotiate data governance in their contract, though public self-hosting is not available.

If pricing transparency or the ability to negotiate custom terms is important, contacting Black Forest Labs for a tailored quote and comparing it to Veo or similar products is recommended.

- Cloud API with private weight access via sales; no public open weights
- Enterprise/compliance terms explicit for Flux 3
- Flux 3 lists per-second API rates publicly; custom and enterprise pricing requires contacting sales

## Frequently Asked Questions

- No, Black Forest Labs lists no free tier, free credits, or trial for FLUX 3 Video. Both the API and the Playground require paid credits at published pay-as-you-go rates.
- No, Black Forest Labs has never published open weights for FLUX 3 Video, and public self-hosting is not available. Private weight access was offered during early access and requires direct negotiation with BFL sales.
- Public self-hosting is not available, and weights have never been published for open download; companies wanting weights must contact BFL sales. Additionally, SOC 2 and ISO 27001 badges on BFL's site are company-level certifications not stated to cover FLUX 3 Video specifically.
- Black Forest Labs displays SOC 2 Type II and ISO 27001:2022 badges in its site footer as company-level certifications, but does not state they cover FLUX 3 Video specifically. Deployment terms go through Black Forest Labs sales.
