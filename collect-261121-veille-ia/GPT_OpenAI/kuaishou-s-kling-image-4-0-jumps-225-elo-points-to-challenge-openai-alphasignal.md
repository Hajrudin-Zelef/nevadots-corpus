---
id: collect-261121-veille-ia/gpt-openai/kuaishou-s-kling-image-4-0-jumps-225-elo-points-to-challenge-openai-alphasignal
title: "Kuaishou's Kling Image 4.0 Jumps 225 Elo Points to Challenge OpenAI | AlphaSignal"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["image", "kling", "editing", "edits", "text", "points", "openai", "output"]
source: https://alphasignal.ai/news/kuaishou-s-kling-image-4-0-jumps-225-elo-points-to-challenge-openai
source_anchor: ""
source_lines: [1, 115]
section: "GPT_OpenAI"
---
## Takeaways

- Kling Image 4.0 ranks #7 in Text to Image and #6 in Image Editing on Artificial Analysis.
- It gains over 225 Elo on its predecessor, Kling Image 3.0 Omni, which ranked #86 and #67.
- Prices are $63 per 1k text-to-image and $66 per 1k edits at 2K, under a third of OpenAI rivals.
- Microsoft's MAI-Image-2.7 still ranks higher on both boards for less money.
- It adds native 8K output, 2,000-character text, 10-image sets and long-page output via the Kling API.
- Reference images are now billed at 0.8 units each, and Identity Preserving Edits lag at #15.

Kuaishouâs Kling Image 4.0 now ranks No. 7 for text-to-image generation and No. 6 for image editing on Artificial Analysis's blind-vote leaderboards. Kling Image 3.0 Omni ranked No. 86 and No. 67, respectively, giving the new model gains of more than 225 Elo points on each board. Every higher-ranked entry comes from OpenAI or Microsoft AI.

Kling advertises photographic realism, native 8K output, multilingual text rendering, and sets of up to 10 related images from one prompt. The benchmark supports the gains in text, layout, and editing quality, though it does not test 8K detail or consistency across image sets. For developers, the trade-off combines stronger output with new reference-image charges and higher prices than Microsoftâs leading models.

## A 225-point Elo swing

Artificial Analysis calculates Elo from blind head-to-head comparisons in which reviewers choose between two outputs without seeing the model names. Kling Image 4.0 scores 1,179.93 on the text-to-image board and 1,182.89 on the editing leaderboard. It outperforms Kling Image 3.0 Omni in every tested editing action and text-to-image capability.

| Artificial Analysis image-editing leaders, with Elo scores rounded |  |  |  | 
|---|---|---|---|
| Rank | Model | Elo | Price per 1,000 images | 
|---|---|---|---|
| 1 | MAI-Image-2.7 | 1,215 | $34.82 | 
| 2 | GPT Image 2.6 Sunburst (max) | 1,211 | $210.72 | 
| 3 | GPT Image 2.6 Flare (max) | 1,205 | $210.72 | 
| 4 | GPT Image 2.5 Sunburst (max) | 1,202 | $210.72 | 
| 5 | GPT Image 2.5 Flare (max) | 1,185 | $210.72 | 
| 6 | Kling Image 4.0 | 1,183 | $66.00 | 
| 7 | MAI-Image-2.7-Flash | 1,179 | $10.24 | 

At $66 per 1,000 edits, Kling sits two Elo points behind GPT Image 2.5 Flare (max) while costing less than one-third as much. Microsoft retains the price advantage: MAI-Image-2.7 ranks higher for about half Klingâs price, and its Flash variant trails Kling by four points while costing less than one-sixth as much.

Klingâs listed price covers a 2K output, its smallest single-image size, while most competing entries are priced at 1K. Elo also measures aggregate preference across the benchmarkâs prompts and voters. It does not establish reliability for a specific workload, and rankings can move as more votes arrive.

## Text and layout drive the gains

Klingâs largest improvements appear in text rendering and layout, which both rose from outside the top 80 to inside the top 10. The layout benchmark covers flows, arrows, visual hierarchy, and multi-panel compositions.

### Where Kling gained ground

- **Object-level edits:** tied for No. 1 when adding, removing, or replacing objects.
- **Editing breadth:** ranked in the top five for five of seven editing actions.
- **UI and UX design:** No. 4, up from outside the top 80.
- **Retail, ecommerce, and social edits:** No. 4 in both benchmark categories.
- **Lighting and material:** No. 5 in both capabilities.

### Faces remain the clearest gap

- **Identity-preserving edits:** No. 15, indicating weaker facial consistency across revisions.
- **Marketing and advertising edits:** No. 11.
- **Productivity and knowledge-work generation:** No. 11.

## One transformer handles reading and drawing

Klingâs developer overview describes a single autoregressive framework for visual understanding and image generation. The transformer interprets prompts and reference images, then emits image tokens sequentially, much as a language model emits text tokens. Generation remains inside that system instead of moving to a separate diffusion model. Shared interpretation and generation could contribute to the text and layout gains, although the benchmark does not isolate architecture as the cause.

### Large prompts, long pages, and 10-image sets

- **Resolution:** single images and image sets support 2K, 4K, and native 8K output with standard or adaptive aspect ratios.
- **Text:** prompts can contain up to 15,000 tokens, while Kling claims an image can contain up to 2,000 rendered characters.
- **Languages:** English, Japanese, Korean, Spanish, Portuguese, simplified Chinese, and traditional Chinese.
- **Image sets:** one prompt can produce up to 10 related images. Kling says an internal planner assigns content to each frame while maintaining the subject and creative direction.
- **Long images:** a dedicated endpoint generates tall assets such as ecommerce detail pages and webpage mockups.

Artificial Analysis generated square 2K outputs and publishes no resolution benchmark, so its ranking provides no evidence for 8K detail. It also does not measure consistency across a 10-image set. Production evaluations should use representative source assets, typography, faces, aspect ratios, and target dimensions.

## References now add to the bill

Kling bills through prepaid units, and Image 4.0 charges for reference images and reference elements that were free with earlier models. At the cited $300 package rate, each unit costs $0.00384. A 2K output consumes 16 units, a 4K output consumes 24, an 8K output consumes 40, and each reference adds 0.8 units.

### A reusable cost function

```
UNIT_USD = 0.00384  # $300 prepaid package
OUTPUT_UNITS = {"2K": 16, "4K": 24, "8K": 40}
REFERENCE_UNITS = 0.8
def estimate_cost(resolution, image_count=1, reference_count=0):
    if resolution not in OUTPUT_UNITS:
        raise ValueError("resolution must be 2K, 4K, or 8K")
    if not 1 <= image_count <= 10:
        raise ValueError("image_count must be between 1 and 10")
    if reference_count < 0:
        raise ValueError("reference_count cannot be negative")
    units = OUTPUT_UNITS[resolution] * image_count
    units += REFERENCE_UNITS * reference_count
    return round(units * UNIT_USD, 6)
estimate_cost("2K")          # $0.061440
estimate_cost("8K")          # $0.153600
estimate_cost("2K", 10, 4)   # $0.626688
```
At that rate, 1,000 standalone 2K generations cost $61.44, while 1,000 edits with one reference cost $64.51. Those figures are close to Artificial Analysisâs listings of $63 and $66. Each additional reference adds $0.003072 per request, so reference-heavy pipelines need to include input count in their cost estimates.

## Endpoints, polling, and access

Kling Image 4.0 is available through Kling Creative Studio, Klingâs API, and third-party hosts. According to the API changelog, requests use structured parameters, select the model through the URL path, and retrieve task status through a GET request.

| Kling Image 4.0 API routes |  | 
|---|---|
| Operation | Method and path | 
|---|---|
| Text-to-image and image-to-image generation | `/image-generation/kling-4.0` | 
| Long-image generation | `/long-image-generation/kling-4.0` | 
| Task status | `GET /tasks` | 

The model version appears directly in each generation path, while task polling remains separate. Kling also lists a Flash variant on its API price sheet; the rankings and capability results here refer to the full Kling Image 4.0 model.

## Where Kling fits

Klingâs benchmark profile and output formats align most closely with commercial work that combines dense typography, structured layouts, and related assets:

- Product shots, packaging mockups, and long ecommerce detail pages.
- UI mockups, diagrams, and multi-panel layouts.
- Thumbnails, promotional cards, and social campaigns requiring up to 10 coordinated images.
- Print and large-format assets that can use native 8K output.

Face-critical pipelines require additional evaluation because identity-preserving edits rank No. 15. Teams optimizing for benchmark score per dollar have a stronger option in MAI-Image-2.7. For multilingual typography, page-length layouts, and high-resolution output, Kling Image 4.0 now ranks near the top of the generation leaderboard while charging roughly one-third of the listed price for OpenAIâs leading editing models.
