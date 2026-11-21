---
id: collect-261121-veille-ia/modeles-open/elorian-glimpse-the-vlm-that-looks-twice-beer-and-code
title: "Elorian Glimpse: The VLM That Looks Twice | Beer and Code"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["elorian", "glimpse", "model", "image", "text", "code", "vision", "reasoning"]
source: https://blog.beerandcode.com.br/en/news/elorian-glimpse
source_anchor: ""
source_lines: [1, 192]
section: "Modeles_Open"
---
Every VLM you use today looks at the image once and then reasons over the summary it wrote for itself. Elorian Glimpse is the first one that goes back for a second look, and that changes where computer vision with LLMs breaks in production.

Glimpse is the first model from Elorian, a Palo Alto startup founded by former Google DeepMind researchers. It shipped on October 16, 2026 as a research preview. The idea is easy to explain and hard to pull off: instead of turning the image into text and forgetting about it, the model crops a region, pulls a specific video frame, and runs code to measure things, all in the middle of its reasoning.

In this post you'll see the flaw Glimpse goes after, how its "visual scratch space" works, the benchmarks Elorian published (including where it loses), what it costs, how to call it today, and where it's worth testing in your product.

## TL;DR

- **What it is:** Glimpse, a visual-reasoning vision language model (VLM) from Elorian. Text, image, and video in. Text only out.
- **The thesis:** the model revisits the visual evidence while it thinks, instead of reasoning over its own description of the image.
- **Benchmarks:** all reported by Elorian, at maximum reasoning effort. There are 12 tests, 11 of them with a GPT-6 Astra score: Glimpse beats Astra on 4, ties on 1, and loses on 6. No independent evaluation published so far.
- **Cost/Access:** $0.60 per million input tokens and $3 per million output tokens, according to Elorian. Elorian's own API, the OpenRouter catalog (`elorian/glimpse` ), and a dedicated endpoint on Together AI, waitlist-only for now.
- **Status:** research preview. Not something to put on your production critical path tomorrow.

## The factory defect in VLMs: look once, hallucinate later

To understand Glimpse, you first need to understand what a VLM, a vision language model, does today.

The standard flow goes like this: the image goes through a vision encoder, becomes a handful of tokens, and those tokens enter the context alongside your prompt. From there, the model is an LLM like any other, predicting the next token. If you want the fundamentals of how that works under the hood, our post on what an LLM is explains it the way that matters to people who build.

The problem is the "once."

The image is encoded at the start. After that, all of the reasoning happens in text. When the model "thinks" about the chart, it's thinking about the description it generated of the chart, not the chart itself. If it misread the Y axis on the first pass, the whole chain of reasoning is built on that mistake. With a lot of confidence.

That's why VLMs get counts wrong. They misread values on dense charts. They confuse a camera cut with camera movement in video. It's not a lack of intelligence. It's a lack of evidence at the moment it matters.

Andrew Dai, Elorian's CEO, told TechCrunch where the idea came from: he took a photo of a board game he'd just played and asked a frontier model who was winning. The model couldn't answer. Board games, he says, have their own visual language, with icons and symbols that don't translate easily into text. "Humans can look at an Ikea manual, with no text, and assemble the furniture. Why can't AI models?"

The team behind it has pedigree. Dai spent 12 years at Google DeepMind, the team includes people who worked on Gemini and Apple's MM-1, and the company raised $55 million in a seed round led by Striker, Menlo Ventures, and Altimeter, with participation from NVIDIA and Jeff Dean.

Pedigree isn't proof. But it explains why the launch deserves attention.

And here's the practical point: anyone doing computer vision with LLMs already knows the problem is rarely the model. It's the pipeline around it: cropping before you send, validating the output, measuring where it fails. We build this kind of multimodal pipeline live, every week, in the Clã Beer and Code, Beer and Code's paid community where we build things together, not through pre-recorded lessons.

## How Elorian Glimpse works: the "visual scratch space"

Elorian calls the mechanism a "visual scratch space." Instead of looking once, Glimpse gets a visual scratchpad where it can go back to the image as many times as it needs during reasoning.

According to the official announcement and TechCrunch's coverage, it can:

1. **Zoom in and crop regions.** Read a tiny label? Go back, crop, read it again at higher resolution.
2. **Revisit video segments with dense sampling.** Elorian's example shows the model rewatching a segment, going from about 8 to 14 frames per second to understand a fast movement.
3. **Run code to measure.** Measure distance, compare positions, track a point across frames. Instead of "guessing" the train is slowing down, it tracks the edge of a window frame by frame and calculates.
4. **Coordinate subagents** that exchange text, visual artifacts, and internal representations with each other.

All of this was trained end to end. It's not an agent you assemble by wrapping tools around a regular VLM.

Some honest context: the idea isn't new. OpenAI added crop-and-zoom to the reasoning of its o-series models in 2025, as TechCrunch points out. The difference Elorian claims is having built this into the whole model as a native capability, not as a bolted-on tool.

Another detail that matters for cost: according to the company, Glimpse was post-trained on top of an open-weights base model, by a small team, with "only a few hundred GPUs." And it's substantially smaller than the frontier models it compares itself against. Smaller usually means cheaper. We'll get there.

## The benchmarks Elorian published (and why none of them are independent yet)

First, the fine print. Every number below comes from Elorian's page. The models ran at maximum reasoning effort, each with its native harness. GPT-6 Astra, GPT-6.1 Sol, and Claude Opus 5.5 with Code Interpreter. On a good chunk of the video tests, Sol and Opus ran at `high` effort, not `max`. And as of this post, I haven't found any independent evaluation reproducing these numbers.

With that said, the table:

| Benchmark | What it measures | Glimpse | GPT-6 Astra | GPT-6.1 Sol | Opus 5.5 | 
|---|---|---|---|---|---|
| WGO Bench | Robotics (manipulation events) | **42.70** | 39.42 | 38.91 | 12.34 | 
| ERQA | Robotics | **83.75** | **83.75** | 82.45 | 82.20 | 
| BabyVision | Visual perception | 95.36 | **97.68** | 97.16 | n/a | 
| HSS | Scene understanding | 46.36 | **53.60** | 46.60 | 49.81 | 
| CharXiv-R | Scientific chart reasoning | 92.90 | 92.10 | 92.60 | **93.40** | 
| UGC-VideoCap | Short video captioning | **82.66** | 80.32 | 80.41 | 76.35 | 
| DREAM-1K | Video captioning | 76.04 | **79.05** | 77.93 | 72.05 | 
| TOMATO | Temporal reasoning | 93.60 | **96.02** | 89.02 | 72.44 | 
| Video-MME v2 | Temporal reasoning | 56.68 | 64.06 | **65.27** | n/a | 
| E.T. Bench | Temporal reasoning | **63.55** | n/a | 59.79 | n/a | 
| FSC-147 | Counting | **83.36** | 82.02 | 82.69 | 73.78 | 
| CountQA | Counting | 82.66 | **86.42** | **86.42** | 82.20 | 

Reading it carefully:

- **The headline is true, but narrow.** Glimpse beats GPT-6 Astra on scientific charts (CharXiv-R) and short video captioning (UGC-VideoCap), as TechCrunch says. It also wins on WGO Bench and FSC-147, and ties on ERQA.
- **It loses more tests than it wins.** Against Astra, it loses on BabyVision, HSS, DREAM-1K, TOMATO, Video-MME v2, and CountQA. On long video and heavy temporal reasoning, Astra and Sol stay ahead.
- **E.T. Bench is left out of the count.** Elorian didn't publish an Astra score for that test. Glimpse leads the row, but barely: 63.55 vs 63.45 for Gemini 3.8 Flash.
- **On CharXiv-R, the leader is Opus 5.5** , at 93.40. Glimpse beats Astra, not the table.
- **The gaps are small.** 92.90 vs 92.10 is within what harness, prompt, and test version can swing.

My read: Glimpse isn't the best vision model in the world. It's a smaller model that gets close to the biggest ones on vision, winning precisely on the tasks where "looking again" makes a difference: counting, measuring, reading chart details, describing short videos accurately.

That's consistent with the thesis. And it still needs someone outside to confirm it.

Do not just follow the news — master it. Hands-on AI Engineering, live, every week, in the largest community in Brazil.

Join the Clã
## What it costs and how to call it today (OpenRouter / Together)

Price is the strongest argument in this launch. According to the official page:

| Model | Input (1M tokens) | Output (1M tokens) | 
|---|---|---|
| Elorian Glimpse | $0.60 | $3.00 | 
| Gemini 3.8 Flash (introductory pricing) | $0.75 | $3.75 | 
| GPT-6.1 Sol | $2.00 | $10.00 | 
| Claude Opus 5.5 | $4.00 | $20.00 | 
| GPT-6 Astra | $10.00 | $50.00 | 

In practice: 94% cheaper than Astra, 85% cheaper than Opus 5.5, and 70% cheaper than Sol, at list price.

A back-of-the-napkin estimate (mine, with hypothetical numbers): reading a chart with about 3k input tokens and 8k output tokens, reasoning included. On Glimpse that's about $0.026. On Astra, about $0.43. Elorian itself showed something similar in a robot planning test: $0.035 per episode vs $0.249 for Astra.

But there's a catch that Elorian itself admits: revisiting the image costs time and tokens. Token efficiency is the company's stated next priority. In other words, the price per token is low, but the model may burn more tokens per task. Measure cost per solved task, not per million.

Where to call it:

- **Elorian's own API and chat** , with a free trial on the platform (chat access is rolling out gradually to people on the waitlist).
- **OpenRouter** , as`elorian/glimpse` , through the OpenAI-compatible API. The docs note that the model isn't available on EU in-region routing.
- **Together AI** , as a dedicated deployment only. The page says "Launching soon," with a waitlist.

A minimal call in Laravel through OpenRouter, sending a chart to read:

```
use Illuminate\Support\Facades\Http;
$imagem = base64_encode(file_get_contents(storage_path('app/graficos/vendas-q3.png')));
$resposta = Http::withToken(config('services.openrouter.key'))
    ->timeout(300) // visual reasoning at high effort can take a while
    ->post('https://openrouter.ai/api/v1/chat/completions', [
        'model' => 'elorian/glimpse',
        'messages' => [[
            'role' => 'user',
            'content' => [
                ['type' => 'text', 'text' => 'What is the value of the September bar? Reply with only the number and the unit.'],
                ['type' => 'image_url', 'image_url' => ['url' => "data:image/png;base64,{$imagem}"]],
            ],
        ]],
    ])
    ->throw()
    ->json('choices.0.message.content');
```
Before you plug it in, check the model page on OpenRouter for which modalities the provider accepts there, especially video, and what the context limit is. That wasn't documented as of this post.

## Where to test it in your product (and where it's not worth switching)

Visual reasoning with AI isn't a single category. Breaking it down by what the numbers and the architecture suggest:

**Worth testing:**

- **Reading charts and dashboards.** This is where the look-again thesis pays off the most, and CharXiv-R backs it up. Scanned financial reports, dashboard screenshots, charts from papers.
- **Counting.** Inventory on shelves, vehicles in a yard, parts on a production line. Elorian showed the model counting machines on a construction site with a timestamp for each one.
- **Short video inspection.** Security cameras, inspection videos, UGC that needs detailed captions. UGC-VideoCap is its test.
- **Robotics labeling.** Splitting an episode into timestamped pick-and-place events is exactly the use case the company is targeting first.

**Not worth switching (yet):**

- **Text-heavy documents.** PDFs, invoices, contracts. There the problem is OCR, not visual reasoning, and a dedicated model tends to be better and cheaper. We compared this in our Mistral OCR 4 test.
- **Long video and heavy temporal reasoning.** On Video-MME v2 and TOMATO, Astra and Sol stay ahead.
- **Low-latency flows.** Looking again takes time. If the user is waiting on the screen, test your p95 first.
- **Anything critical in production.** It's a research preview. No SLA, no track record, no independent evaluation.

The cheap way to decide is the usual one: pick 30 real images from your domain with the correct answer, run them through your current model and through Glimpse, and compare accuracy and cost per task. Lab benchmarks don't know your use case.

## Limitations and things to watch

**The numbers are Elorian's.** All of them. With a harness chosen by the company and maximum reasoning effort. As of publishing this, no independent evaluator has reproduced the results.

**Some examples aren't recorded runs.** The page notes that some examples use reviewed reference answers, not traces of the model actually running. The tire example, for instance, is explicitly a qualitative answer, not a simulation.

**It burns tokens to look again.** The company admits that careful visual review consumes time and tokens. A low price per token doesn't guarantee a low bill.

**Text-only output.** It doesn't generate images or return robot actions. Direct robot control is listed as future work.

**Privacy.** Customer images, documents with faces, internal camera footage: all of that leaves your infrastructure. Mask or anonymize before sending, like you would with any API.

## Quick FAQ

**What is a VLM?**
A VLM, or vision language model, is a language model that accepts images (and sometimes video) as input. The image is turned into tokens by a vision encoder and enters the context alongside the text. What sets Glimpse apart is going back to the image during reasoning, instead of using it only once at the start.

**Is Elorian Glimpse better than GPT-6 Astra?**
Partly. By Elorian's own numbers, it wins on scientific charts, short video captioning, counting on FSC-147, and robotics on WGO Bench, and ties on ERQA. It loses on the other six tests with an Astra score, mainly long video and temporal reasoning. And it costs about 94% less.

**How much does Glimpse cost?**
$0.60 per million input tokens and $3 per million output tokens, according to Elorian. That's cheaper than Gemini 3.8 Flash at its introductory price. But since the model revisits the image, it may spend more tokens per task. Measure cost per solved task.

**Can I use Glimpse in production?**
You can test it, but it's a research preview: no SLA and no independent evaluation yet. Use it in async flows, with a fallback to another model and an eval on real data from your domain before making any decision.

## Conclusion

Elorian Glimpse didn't win the table. It won an argument: a smaller, much cheaper model gets close to the biggest ones on vision because it goes back to look at the evidence instead of trusting its own description.

If the thesis holds up under independent evaluation, the next step for VLMs isn't a bigger encoder. It's reasoning that consults the image the way it consults a tool. And then counting, chart reading, and video inspection stop being the places where LLMs with vision break in production.

Until then, treat it like any research preview: test it on your use case, measure cost per task, and don't swap anything on your critical path based on a lab's table. If your use case is documents, not images, the Mistral OCR 4 comparison is your next step.

{AI Engineer} — apaixonado por Laravel, arquitetura de software e construir produtos com impacto. Compartilho aqui tutoriais, descobertas e reflexões sobre o dia a dia de engenharia.

There is no shortage of content. What is missing is someone to untangle it: what matters now is how to implement it the right way. In the Clã you get that live, every week, with people who have already filtered out the noise.

Join the Clã
