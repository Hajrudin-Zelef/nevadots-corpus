---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-auto-annotation-best-boxes-at-a-price
title: "Gemini 4 Carbon Auto-Annotation: Best Boxes, at a Price"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "recall", "precision", "boxes", "best", "models", "images"]
source: https://www.ultralytics.com/blog/gemini-4-carbon-auto-annotation
source_anchor: ""
source_lines: [1, 153]
section: "Gemini_Google"
---
# Gemini 4 Carbon auto-annotation: the new leader on our VLM Leaderboard

Gemini 4 Carbon tops 45 models on our 36-dataset detection benchmark with 0.584 mAP<sub>50</sub> and the best precision and recall. Is it worth 3x GPT-6.1 Terra?

Listen to this article

Google built Gemini 4 Carbon for cyber defense and for what it calls "tasks that test the limits of a model's intelligence." Nothing in its launch pitches it as a detector. We asked it for bounding boxes anyway, and it drew the best ones we have measured.

On UL37, our 36-dataset object detection benchmark, Gemini 4 Carbon auto-annotation scores 0.584 mAP<sub>50</sub>, first of 45 models on the Ultralytics VLM Leaderboard. It also has the highest precision and the highest recall on the board, two columns that in October belonged to two different models. GPT-6.1 Terra, the previous leader, drops to second at 0.560.

The catch is the bill. Carbon costs $12.93 per 1,000 images, three times Terra's $4.36, and Google doubles Carbon's token price on December 1. So the real question for a labeling team is not whether Carbon is the best annotator. It is where those extra points are worth the money. Below is where Carbon earns its premium, where Terra is still the better buy, and how to run Carbon on your own data in Ultralytics Platform.

## Gemini 4 Carbon takes first place on UL37#

UL37 measures how closely a model's boxes match human labels on 360 images: 10 from each of 36 real object detection datasets, from thyroid ultrasound and thermal drone footage to retail shelves, floor plans and circuit boards. Every image runs through the same production pipeline Platform users get, with the dataset's own class names, and every dataset counts equally. The methodology is public.

Carbon's scorecard:

- **mAP<sub>50</sub>:** 0.584, #1 of 45
- **mAP<sub>50-95</sub>:** 0.371, #1 of 45, ahead of Claude Fable 5.2's 0.347
- **Precision and recall:** 0.685 and 0.642, both #1 of 45
- **Provider cost:** $12.93 per 1,000 images at Google's introductory price
- **Median time per image:** 7.6 s
- **Failed requests:** 5 of 360, none of them refusals


*UL37 mAP<sub>50</sub> for the 12 most accurate of 45 models, with each paid model's provider cost per 1,000 images.*

The lead is not a one-metric fluke. Carbon is first on mAP<sub>50</sub>, on the stricter mAP<sub>50-95</sub>, on precision and on recall. It ranks in the top five on 25 of the 36 datasets and in the top three on 17.

### What Carbon adds over Gemini 4 Argon#

Google's model card says Carbon is based on Gemini 4 Argon and spends more thinking tokens to "explore hypotheses thoroughly" and run extended verification loops. It reads text, images, audio and video, holds up to 1M tokens of context, and answers in text.

On boxes, that extra thinking pays. Gemini 4 Argon scores 0.537 mAP<sub>50</sub> and 0.343 mAP<sub>50-95</sub>, so Carbon adds 0.047 and 0.028 on the same images and pipeline, and it beats Argon on 22 of 36 datasets, losing 11 and tying 3. The extra thinking also shows up in the bill and on the clock. Both models list at the same $2 and $10 per million input and output tokens, yet Carbon costs 1.7 times as much per image, $12.93 against $7.60, and takes 7.6 seconds per image to Argon's 4.6.

## Gemini 4 Carbon leads on both precision and recall#

Precision and recall describe the two ways a draft goes wrong. Low precision means boxes on things that are not there, which a reviewer has to find and delete. Low recall means objects left unboxed, which a reviewer has to draw. Our guide to choosing a VLM by precision and recall explains how to weigh the two for your data.

In October, the leaders traded one for the other. Gemini 4 Argon was the most precise model on the board, at 0.667, but found fewer objects. GPT-6 Astra had the best recall, 0.627, but more of its boxes were wrong. Carbon beats both at their own strength: 0.685 precision and 0.642 recall.


*UL37 precision against recall for the 14 language models above 0.42 mAP<sub>50</sub>. Carbon is the only model in the top-right corner.*

SKU110K shows the difference on a crowded image. Its supermarket shelf photos hold row after row of products, all labeled with one class. Argon is very precise there, 0.90, but finds only 0.63 of the products. Terra finds more, 0.73, at 0.79 precision. Carbon matches Terra's recall at 0.74 while keeping precision at 0.84, and its 0.671 mAP<sub>50</sub> is the best any model has posted on SKU110K, ahead of Terra's 0.619.

## Where Gemini 4 Carbon sets new top scores#

Carbon holds the top score on 12 of the 36 datasets, two of them shared ties. On ten it leads outright, though on DDR retinal lesions the margin is only 0.001.


*UL37 mAP<sub>50</sub> on every dataset where Carbon ranks first, against the best of the other 44 models, largest margin first.*

Three groups stand out.

**Argon's home ground, pushed further.** Argon already led on thermal, aerial and underwater imagery, and Carbon takes all of it. Its biggest margin of any dataset is on HIT-UAV thermal drone footage: 0.689 against Argon's 0.582 and Terra's 0.433, with precision of 0.81 and recall of 0.78. It also leads on LLVIP infrared pedestrians (0.955), RUOD underwater scenes (0.788) and Airbus oil storage tanks in satellite images (0.381).


*A replay from the Gemini 4 Carbon model page on an LLVIP infrared image: one pedestrian at night, one box matching the label, 0.995 mAP<sub>50</sub>.*

**Drawings and documents.** On FloorPlanCAD 2, where the classes are doors, windows and other architectural symbols, Carbon scores 0.821 against GPT-6 Astra's previous best of 0.726. That fits Google's own numbers: its model card lists "spatial reasoning" among Carbon's strengths and shows it ahead of Argon on Blueprint-Bench 2, 58.6% to 54.4%. By domain, Carbon now ranks first in documents, UI and CAD.

**Brain MRI, plant disease and sports.** On Cheng brain tumor MRI, with meningioma, glioma and pituitary tumor classes, Carbon edges past Terra, 0.942 to 0.904. On PlantDoc leaf disease it scores 0.594 against Claude Fable 5.2's 0.521, and on basketball and hoop detection 0.882 against Fable's 0.849.

Each dataset contributes 10 images, so treat any single score as a signal to investigate, not a verdict.

## Where Gemini 4 Carbon falls short#

First place overall does not mean first everywhere. Carbon trails the best model by more than 0.2 mAP<sub>50</sub> on five datasets:

- **Thyroid ultrasound.** On TN5000, where each nodule is labeled benign or malignant, Carbon scores 0.537 to Terra's 0.935, the widest gap between the two on any dataset. Terra also stays ahead on TXL-PBC blood cell microscopy, 0.889 to 0.742.
- **Circuit boards.** On DeepPCB bare-board defects, Carbon scores 0.03 to GPT-6.1 Sol's 0.39, and it ranks seventh in the industrial PCB domain.
- **Camera traps, fisheye traffic and tennis balls.** Carbon ranks 18th on ENA24 wildlife camera traps (0.636), ninth on Fisheye8K traffic (0.250) and ninth on RacketVision tennis balls (0.516). The hosted SAM 3.1 Text model, which needs no API key, leads the first two at 0.884 and 0.478.

It also sits mid-pack on Kvasir-SEG polyp endoscopy (19th), UEC school lunch trays (16th) and SoccerDB (15th). Before labeling a large dataset in any of these domains, compare the candidates' rows on their model pages.

Security X-ray is a different story from our Claude Fable 5.2 results. Carbon refused none of the IEDXray scans and scored 0.282, second only to Argon's 0.298, while Terra scored zero there.

## Gemini 4 Carbon vs GPT-6.1 Terra, Gemini 4 Argon and Claude Fable 5.2#

| Model | UL37 mAP <sub>50</sub> | mAP <sub>50-95</sub> | Precision | Recall | Cost per 1,000 images | Median time per image | 
|---|---|---|---|---|---|---|
| **Gemini 4 Carbon** | **0.584** | **0.371** | **0.685** | **0.642** | $12.93 | 7.6 s | 
| GPT-6.1 Terra | 0.560 | 0.345 | 0.646 | 0.613 | **$4.36** | 5.1 s | 
| Claude Fable 5.2 | 0.544 | 0.347 | 0.602 | 0.620 | $37.55 | **4.6 s** | 
| Gemini 4 Argon | 0.537 | 0.343 | 0.667 | 0.585 | $7.60 | 4.6 s | 

Head to head, Carbon beats Terra on 24 datasets and loses on 10, with two ties. It beats Fable 5.2 on 22 and loses on 14.

Terra's main asset against Carbon is one dataset. Its TN5000 score is 0.40 higher than Carbon's, and that single dataset narrows the overall gap. Leave TN5000 out, and Carbon leads 0.585 to 0.549. By domain, Carbon leads in eight of 12.


*UL37 mAP<sub>50</sub> by domain, hardest first, Gemini 4 Carbon vs GPT-6.1 Terra at each provider's default reasoning effort.*

Terra keeps medical, 0.505 to 0.451 across eight datasets, and Carbon is second of 45 there. Terra's other three domain leads, in sports, roads and everyday scenes, are 0.017 or less. Carbon's widest domain leads are on retail, underwater, aerial and thermal imagery, and on security X-ray, where Terra scored zero.

## What a Gemini 4 Carbon batch costs#

The $12.93 per 1,000 images is the provider bill, calculated from Google's reported token usage at Platform's rates for Carbon: Google's introductory $2 per million input tokens and $10 per million output tokens. Google's availability post says that rate runs through November 30, 2026. From then on, Carbon costs $4 and $20, so the same images come to about $25.87 per 1,000.


*UL37 mAP<sub>50</sub> against provider cost per 1,000 images for all 26 paid models, with Carbon at Google's introductory price and at the price from December 1.*

Batch Annotation adds Platform's processing fee of $1.00 per 1,000 images, with a $0.01 minimum per run, so a 1,000-image run costs about $13.93 now and $26.87 in December. Google bills your own key directly, and Ultralytics adds no markup to tokens. For a broader comparison, see our image annotation software cost guide.

Even at December's price, Carbon costs less per image than Claude Fable 5.2 ($37.55) and GPT-6 Astra ($49.31) and scores higher than both. Against Terra, the trade is plainer: Terra delivers 96% of Carbon's mAP<sub>50</sub> for a third of today's price, and about a sixth of December's. Carbon and Terra are two of the four models the leaderboard marks "Best value," the ones no other model beats on both cost and accuracy.

Two more details from the run:

- **Carbon's own boxes do the work.** Platform tightens every model's boxes with SAM 3.1. Carbon's raw boxes already score 0.584 mAP<sub>50</sub> , and their mAP<sub>50-95</sub> is slightly higher before refinement than after, 0.379 against 0.371.
- **Few failures, no refusals.** Five of 360 requests failed and count as images with no boxes. None was a refusal.

If budget matters more than the top score, Claude Haiku 5.5 scores 0.429 for $0.37 per 1,000 images. Hosted models such as Qwen3.8 27B and SAM 3.1 Text need no API key at all.

## How to run Gemini 4 Carbon auto-annotation on your dataset#

Carbon is a class-prompted model for detection datasets with 1 to 200 classes. It returns bounding boxes for your dataset's own class names, and you review them before training.

1. Create a Gemini API key in Google AI Studio. Google offers Carbon to paying developers, so the key's project needs billing enabled.
2. Add the key under Settings > API Keys in the Platform workspace that owns your dataset. The provider key docs cover the details.
3. Open an image in the annotation editor, switch to **Smart** mode, and choose Gemini 4 Carbon under Google in the model picker.
4. Press **Predict** , or`P` , to label the current image. Try a handful of images that represent your data before scaling up.
5. On the dataset page, choose **More actions > Batch Annotation** to label every unlabeled image in one pass. Platform saves a dataset version first, so you can roll back, and it never changes existing labels.
6. Review and correct the boxes, which the editor saves automatically, then train a YOLO26 model on the result in the same workspace.

Before you commit a large dataset, open the model page. It replays Carbon's recorded boxes on every benchmark dataset beside the expected boxes and lists its score on each one.

Developers can also call Carbon from code: the Platform SDK v0.1.109 release added `gemini-4-carbon` as an image-prediction model. For more on how these models fit the editor, see how Ultralytics Platform uses AI to automate annotation and why human-in-the-loop review stays part of the workflow. For medical image analysis, treat any model's boxes as a draft that clinical experts check and approve, never as a diagnosis.

## Who should use Gemini 4 Carbon#

Choose Gemini 4 Carbon when label quality matters more than the bill. It is the most accurate auto-annotation model we have benchmarked, the leader on both precision and recall, and the clear pick for thermal, aerial, underwater, retail shelf and floor plan data. If you planned to use it for a big batch, run it before December 1, while the introductory price applies.

Choose GPT-6.1 Terra when cost matters. It stays the best value among the leaders, it is still the stronger model for thyroid ultrasound and blood cell microscopy, and its gap to Carbon on most everyday data is small. A sensible split is Terra as the default, with Carbon on the datasets where its model page shows a clear lead.

If you already pay for Argon to label images, Carbon is the upgrade: better boxes on 22 of 36 datasets for about 70% more per image. For Argon's side of the story, see our Gemini 4 Argon auto-annotation results.

## Frequently asked questions

- Yes. On our UL37 benchmark it scores 0.584 mAP <sub>50</sub> and 0.371 mAP<sub>50-95</sub> across 36 real-world detection datasets, first of 45 models on the Ultralytics VLM Leaderboard, with the highest precision (0.685) and recall (0.642) of any model. Its weakest areas are thyroid ultrasound, circuit-board defects and camera-trap wildlife images.
- It is more accurate: 0.584 mAP <sub>50</sub> to Terra's 0.560, and it wins 24 of 36 datasets to Terra's 10. Terra costs $4.36 per 1,000 images against Carbon's $12.93, answers faster, and stays far ahead on TN5000 thyroid ultrasound. Pick Carbon where its lead is large, such as thermal, aerial and shelf images, and Terra for large general batches on a budget.
- On UL37, yes. Carbon is built on Argon, scores 0.047 higher on mAP <sub>50</sub> (0.584 to 0.537) and beats it on 22 of 36 datasets. It shares Argon's introductory token price but uses more thinking tokens, so it costs about 1.7 times as much per image and takes 7.6 seconds per image to Argon's 4.6.
- About $12.93 in Google charges at the introductory price of $2 per million input tokens and $10 per million output tokens, which runs through November 30, 2026. At the $4 and $20 price that follows, the same images cost about $25.87. Batch Annotation in Ultralytics Platform adds $1.00 per 1,000 images, and you pay Google directly through your own API key.
- Yes. Even the top model averages 0.584 mAP <sub>50</sub> , and some datasets score far lower, such as 0.03 on DeepPCB circuit-board defects. Review every image, and have clinical experts approve boxes on medical data.

## Written by

## Comments<sup>1</sup>

Ask a question, share your results, or help someone else.

1. Amazing results and good to see Google back in the game!
