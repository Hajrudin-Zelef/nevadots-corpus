---
id: collect-261121-veille-ia/atria-dawn/23-pages-silently-placed-in-the-repositorywhat-atria-dawn-preview-really-released-wasn-t-a-model-but
title: "23 Pages Silently Placed in the Repository—What Atria Dawn Preview Really Released Wasn't a Model, but a 'Record of Building with AI'｜アイドリ | AI-Driven Lab"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["model", "dawn", "atria", "preview", "released", "wasn", "glm-5", "pages"]
source: https://note.com/ai_driven/n/n5c7a1f8d3331?hl=en
source_anchor: ""
source_lines: [1, 246]
section: "Atria_Dawn"
---
# 23 Pages Silently Placed in the Repository—What Atria Dawn Preview Really Released Wasn't a Model, but a 'Record of Building with AI'

## Target Audience for This Article

- Primary Audience: Engineers currently selecting open-weight agentic models, and PMs/executives who want to grasp the reality of AI utilization through data.
- Technical Level: Primarily aimed at intermediate users, among beginner/intermediate/advanced levels.
- Prerequisite Knowledge: Assumes familiarity with terms like MoE, context length, and post-training, as well as experience using coding agents like Claude Code or Codex.
- What You Will Gain: The ability to explain the capabilities and limitations of Atria Dawn Preview in your own words, and material to help decide how to position AI agents within your company.

## Summary

On September 14, 2026, the Shanghai AI Laboratory released the agentic model "Atria Dawn Preview" under the MIT license. It features a specialized configuration: based on Z.ai's GLM-5.2, with a 256K context window, and text-only capabilities. In this article, we first organize the specifications and 16 benchmarks from the official README, and grasp the essence of the "Verifiable Experience Pipeline" training method. In the latter half, we decode a 23-page technical report that was sitting in the repository but not linked from the README, tracking the "division of labor between AI and humans" shown by the development records of 56 people and 769 tasks. After reading this, you should know whether you should try this model and which metrics you should use to measure your company's AI utilization.

Every time a new model comes out, many people have the habit of checking the benchmark table first. I am one of them. However, regarding this release, looking at the table isn't very interesting. There are several rows where it loses to Claude Opus 5, and in the rows where it wins, the difference is only a few points. It ends with a "meh."

What was interesting was after I git cloned the repository. There was a single PDF lying in the root that wasn't linked anywhere in the README. 23 pages. The title is "Atria Dawn: The Dawn of Agentic Superintelligence." The content wasn't bragging about the model, but rather **an empirical record of what AI and humans each did during the process of developing this model.**

Let's look at it in order.

## First, just the facts: Who is Atria Dawn Preview?

Atria Dawn Preview is a large language model specialized for agents, released by the Shanghai AI Laboratory. It is distributed via internlm (the same organization as the Shoshei series) on Hugging Face, and via Shanghai_AI_Laboratory on ModelScope. The license is MIT. The weights are available in two versions: BF16 and FP8 quantized.

The GitHub repository was created on September 12, 2026, and the README was finalized in its current form on September 14. As of this writing, it has 82 stars and is still quiet.

### The base is GLM-5.2. What the Shanghai AI Lab did was the "post-processing"

The beginning of the README states: "Built on the 744B parameter MoE **GLM-5.2** foundation model." In other words, the pre-training is the work of another company, and the Shanghai AI Lab was responsible for everything from post-training onwards.

There is also circumstantial evidence. The total parameter count displayed on the Hugging Face model card is 753B. The architecture tag is glm_moe_dsa. As for the official local deployment instructions, it even instructs to **refer directly to the recipe for GLM-5.2**. They have no intention of hiding it.

Regarding how strong this GLM-5.2 base is, I delved into the 753B scale and the mechanism of DSA (Densely Sparse Attention) in my previous article, "GLM-5.2 Thorough Explanation: A Chinese Coding Model with 1 Million Tokens and 'Opus-Class' Performance Released under MIT." If you grasp the capabilities of the foundation first, this story should be much easier to read.

And what's interesting is that there is already another model finished in a different direction from the same base.

The GLM-5.3 released by Z.ai itself in August also had a base model completely identical to GLM-5.2. I have written in detail about this strategy of "using the same base and only changing the post-processing" in "A Thorough Explanation of GLM-5.3: 50% Performance Increase Despite the Same Base Model, and Unexpected Cyber Defense Capabilities". Viewing this Atria release as a case study of **another organization executing the same idea** makes its positioning clear.

### 256K context, and an agent that "cannot read images"

There are two points to note regarding specifications.

**(1) Context is 256K tokens** While the base GLM-5.2 claimed 1M tokens, Atria has narrowed it down to 256K. It seems they prioritized the consistency of execution within that range over length.

**(2) Text-only** This is the most impactful point in practice. The official README explicitly states that if you send an image, it returns "Atria-Dawn-Preview is not a multimodal model." An agent that cannot read screenshots. Honestly, I doubted my eyes when I first saw it. For those who want to entrust browser operations or UI debugging to an agent, this is quite a heavy constraint.

The fact that the official side even provides a workaround is quite conscientious, and I will touch on that later.

## Benchmarks: The rows where they lose are the ones worth reading

The evaluation published by the official side consists of 16 items. First, let's list them all.

### The 5 wins and the 4 losses

Atria achieved the highest values in 5 categories: DeepSearchQA (96.0), BrowseComp (92.5), BFCL v4 (77.0), AutomationBench (53.8), and CyberGym (86.5). According to the report, it leads the second-place Qwen 3.8 Max by 4.1 points in AutomationBench and leads GLM 5.3 by 2.9 points in BFCL v4. It can be summarized as a winning pattern skewed toward search, tool calling, and security.

So, what about the side where they are losing? Comparing the gaps with Claude Opus 5, SWE-bench Pro is 59.6 vs 74.7, Terminal-Bench 2.1 is 78.3 vs 90.2, JobBench is 50.3 vs 68.0, and GDPval is 1583 vs 1768. None of these are small margins. **There is still a clear gap in software engineering and long-duration professional tasks** is the straightforward interpretation.

On the other hand, I also want to look at how close the results are in the rows where they placed second. SkillsBench is 0.3 points behind the leader, Workspace-Bench is 0.8 behind, and Workspace-Bench-Lite is 1.9 behind. The claim that their strengths are not skewed toward a single point has a certain level of persuasiveness.

Here, I would like to point out one thing I appreciate. The official chart in Figure 1 **also includes items where the company is ranked last**. In both GDPval and SWE-Bench Pro, the blue bar is on the far left and is the lowest. Since it is not uncommon for release materials of this type to only show charts where they are winning, I felt a genuine sense of goodwill here.

### The small asterisk in CyberGym

The CyberGym panel in the official chart includes one bar with an annotation. Below the legend, it says in small print, "* Claude Mythos Preview." In other words, for a row where Claude Opus 5 is marked as "–" in the table, they have filled it in the chart with another top-tier model from Anthropic.

CyberGym went from 84.5 to 86.5, slightly surpassing GLM-5.3—this composition itself feels like déjà vu. Z.ai itself explained that GLM-5.3's cyber capabilities grew as an unintended byproduct. Two models processed from the same base are both showing growth in security benchmarks. I think it is too good to be a coincidence, but since I am not at a stage where I can provide evidence, I will leave this as just a point of observation.

In any case, **these are all self-reported values from the vendor**. As of the time of writing this article, I have not been able to confirm any third-party reproduction verification.

### Beyond the benchmarks: What did they actually build?

Perhaps the actual deliverables are more eloquent than the numbers. The report includes several actual items generated by the model.

Other records include building a MiniOS (with serial shell, disk access, persistent file system, and interpreter) from an empty workspace in about 20 minutes, and confirming persistence and auto-boot by launching QEMU twice. There is also a case of processing over 100GB of weather data to train a ViT with over 0.4B parameters for 45,000 steps, handling 69 weather variables. Another case involved rewriting from Triton to native CUDA to accelerate decoding, achieving a 1.46x speedup across 7 batch sizes.

All of these are presented alongside evidence that they were 'brought to a working state.' This obsession stems from the training method we will look at next.

## Verifiable Experience Pipeline: Only verified experiences are included in training

The core of Atria's training is the Verifiable Experience Pipeline. The mechanism itself is simple, and almost disappointingly straightforward.

The key is the method of verification. Passing tests, experimental metrics, file or application states, geometric shape checks, and source corroboration. **The design is to prepare 'signals that can be verified from the outside' for each domain, and only adopt them as experience when they match those signals.** Incomplete, contradictory, redundant, or invalid behavioral trajectories are discarded. The analysis results of failures are fed back into the improvement of the next tasks and environment.

In short, it is about training only on 'verified results' rather than 'plausible-looking answers.' When it outputs CAD, it checks the shape; when it fixes a vulnerability, it re-tests. The fact that all the artifacts in Figure 4 were paired with verification is a direct reflection of how the training was constructed.

## 23 pages silently placed in the repository

Now, to the main point.

In the root of the repository, there is a file named atria-dawn-preview.pdf. According to the PDF metadata, it was generated on September 14, 2026, at 15:28 (UTC), and is 23 pages long, typeset in LaTeX. However, **it is not linked anywhere from the README**. The Citation section, which should contain reference information, remains commented out, and a note reading <!-- TODO: Add chart annotations. --> remains on the evaluation chart. The contact information listed consists of two Fudan University email addresses.

It feels like they just left it there before it was ready to be released.

And the content consists of 3 pages of benchmark explanations, 3 pages of case studies, and the majority of the rest is devoted to 'an analysis of the process of creating this model together with AI'.

### Three stages of role transformation

The paper first organizes the relationship between AI and researchers into three stages.

(a) is the era when the model was merely the subject of research, (b) is the stage where it became possible to entrust clear tasks such as implementing code modules or log analysis, and (c) is the present—a 'project-level colleague' where the AI plans and iterates within goals and evaluation criteria set by humans. A question mark is placed at (d). How far will recursive self-improvement (RSI) go, and what will the human role become?

As a diagram, this is a common classification. What made it valuable was what came after, the part where the actual data appeared.

### 56 people, 769 tasks, 96.5% AI usage rate

The analysis covers 769 task records and agent logs from 56 participants. Out of 739 tasks where AI usage was clear, AI was used in 713 of them. **96.5%**.

Furthermore, the daily median number of agent actions per human prompt—in other words, "how much gets done with a single word"—increased 2.6 times in four weeks, from 11.0 on August 7 to 28.5 on September 4. However, since the interquartile range also widened, it does not mean everyone progressed at the same rate.

### 'I wouldn't have even started without AI' accounts for 33.2%

This is the figure that struck me the most.

When participants were asked, "Could you have completed your assigned portion without AI while keeping the scope, quality requirements, and resources fixed?", out of 455 completed tasks with responses, **151 (33.2%) were answered as "impossible without AI."** Moreover, these 151 tasks were distributed among 27 out of 56 people—roughly half. This means it wasn't just a bias from a few heavy users.

Time saved and work that became possible in the first place are two different things. If you measure productivity solely by "how many hours were saved," this one-third disappears entirely. For those measuring the impact of AI adoption within their companies, I think this is a perspective worth taking home.

### AI proposes, humans decide

So, if you rely on AI that much, do you end up handing over the decision-making as well? The answer was clearly no.

For determining methods and parameters, "AI proposes, human chooses" was the most common at 55.4%. Even so, the percentage ultimately chosen by humans reached 85.5%. For goals and scope, it was 93.4%, and for acceptance criteria, 81.9%. On the other hand, the percentage decided by AI remained within the range of 6.1% to 9.2% for all items.

To emphasize this further, even when narrowed down to the 151 tasks that were "impossible without AI," humans still chose the final goal in 144 cases (95.4%). The intuition that AI takes over decision-making as dependency increases does not hold true.

### When stuck, what humans did was not taking over

Talking only about when things go well is not enough. The paper asks participants to recall the "most significant obstacles" in each task.

Of the 588 tasks, 76.0% moved forward through human intervention, 23.0% were recovered by the AI on its own, and 1.0% remained unresolved. Up to this point, it is as imagined.

What surprised me was the bias in the content. "Adding context/clarifying requirements" accounted for 35.2%, and "isolating causes/changing methods" accounted for 34.7%. In contrast, "fixing part of the work" was 3.2%, and "taking over the main task" was only 0.7% (4 cases). **Human help was biased about 18 times toward changing what the agent knew**.

The handling of deliverables follows the same pattern. Substantial revisions were made in 354 out of 627 tasks (56.5%), but 75.4% of those were **fixed by the AI itself** after receiving human feedback. Humans intervened directly in 19.2%. And 95.9% of the output was included in the final deliverable in some form. The fact that there were many substantial revisions should be read not as a sign that the deliverable was rejected, but as a sign that it was being rewritten while the draft remained.

The paper makes a point here. It warns not to read the increase in actions per prompt from 11.0 to 28.5 as "increased autonomy." **It is just that one human judgment has come to ripple across more agent actions**.

There was also an honest passage about permissions. It mentions that many researchers used Codex's yolo mode or Claude Code's skip-permissions mode so that long-running executions wouldn't stop while waiting for approval. And they admit themselves that "the boundary of permissions was determined by convenience, not intentional design." ...I suspect quite a few people reading this can relate.

## To actually try it: 3 entry points and a text-only wall

### API/Local Deployment

For users outside China, the entry point is the API console (api.atria-asi.ai) and its documentation; for those within China, it is the Intern-series token plans. As of this writing, the official token pricing has not been announced.

To run it yourself, you need SGLang v0.5.13.post1 or later, or vLLM v0.23.0 or later. However, as mentioned above, the referenced guide is for GLM-5.2. Since it is a 753B-scale MoE, it requires a data-center-grade GPU configuration, as the weights alone reach the 1.5TB range in full BF16 precision, even with FP8. This is not the type of model you test on a laptop.

### How to bypass the text-only constraint

When using it with Codex, simply adding the provider is not enough. Because Codex assumes all models are multimodal, images are attached via -i/--image or pasting into the TUI, causing the endpoint to return a 400 error.

The official workaround is to prepare your own model catalog JSON, declare "input_modalities": ["text"], and strip the images on the client side. Additionally, you must set context_window and max_context_window to 256000; otherwise, Codex will use conservative default values, wasting the context. You need Codex CLI 0.154.0 or later.

For Claude Code, a Python script is provided that blocks PDF and image reading by the Read tool via the PreToolUse hook. The idea is to filter it at the hook level.

It feels convoluted, doesn't it? I think so too. But on the flip side, it also means that **they are seriously anticipating real-world use in agent harnesses.** There are not many model cards that go into this much detail.

## My Opinion: How to Handle This Model Now

I will start with the conclusion. **Do not view it as a candidate to replace your primary model. Test it as a specialized tool for deep investigation and security verification.** This is my position at this moment.

There are three reasons. First, the areas where it excels are neatly skewed toward search, tool calling, and vulnerability analysis. DeepSearchQA 96.0 and BrowseComp 92.5 are directly effective for cross-company knowledge research tasks. Second, the gap between SWE-bench Pro and JobBench is significant, meaning it cannot replace Opus 5-class models for daily coding or long-duration professional tasks. Third, since it cannot read images, it is inherently unsuitable for UI verification or workflows involving screenshots.

The fact that it is under the MIT license is a major advantage. For organizations that cannot move data off-site, having one more option for a self-hosted agent model is significant. However, all figures are self-reported, and third-party verification is yet to come. If you are going to evaluate it, the most reliable way is to run your actual tasks—whether they are research projects or actual vulnerability checks—in parallel under fixed conditions.

And one more thing. The most important takeaway from this release is not the model's performance, but **that 23-page document.** Many organizations measure the effectiveness of AI adoption by the "man-hours saved." However, the record of 769 tasks showed that what AI increased was not just speed, but **the scope of work that could be undertaken,** and that what remained for humans was not execution, but judgment.

When looking back at your company's AI usage, try counting "how many jobs would not have been started without AI" alongside "how many hours were saved." This report was worth reading just to take that question home with you.

## Summary

To summarize Atria Dawn Preview in one phrase, it is **an agent-specific machine strong in search and security, built by adding only verification-focused post-processing to another company's base model, GLM-5.2.** It features 256K context, is text-only, and is MIT-licensed. It holds the top spot in 5 out of 16 benchmarks, but is clearly outperformed by Claude Opus 5 in software engineering and long-duration professional tasks.

The core of its learning, the Verifiable Experience Pipeline, was a straightforward design that only retains experiences verified by external signals. And the real highlight is the 23-page report not even linked from the README—96.5% AI usage rate, 33.2% of tasks impossible without AI, 55.4% of method proposals by AI, 85.5% of final decisions by humans, and 69.9% of recovery from failures involving "information transfer" rather than "taking over."

First, try opening that PDF from the repository. It is worth reading before you even try running the model.

## References and Materials Used

- Atria Dawn Preview Official GitHub Repository: https://github.com/atria-asi/Atria-Dawn-Preview
- Atria Dawn Preview Model Card (Hugging Face): https://huggingface.co/internlm/Atria-Dawn-Preview
- Atria-Dawn-Preview-FP8 (Hugging Face): https://huggingface.co/internlm/Atria-Dawn-Preview-FP8
- ModelScope Model Page: https://www.modelscope.cn/models/Shanghai_AI_Laboratory/Atria-Dawn-Preview
- Official Website: https://atria-asi.ai/
- Technical Report "Atria Dawn: The Dawn of Agentic Superintelligence" (atria-dawn-preview.pdf included in the above GitHub repository, September 14, 2026)
- * Figures 1, 4, and 6 are cited from the official repository and the included report mentioned above. Figures 2, 3, 5, 7, and 8 were created by the author based on official information and values published in the report. All benchmark scores are self-reported values by the Shanghai AI Laboratory, and independent verification by third parties has not been confirmed as of the time of writing. Specifications and terms of service are based on information publicly available as of September 15, 2026.

## Related Articles

- GLM-5.2 In-Depth Analysis: A Chinese Coding Model with 1 Million Tokens and "Opus-Class" Performance Released under MIT — An article explaining the base model of Atria itself. You can grasp the 753B scale and the mechanism of DSA.
- GLM-5.3 In-Depth Analysis: 50% Performance Increase with the Same Base Model, Plus Unexpected Cyber Defense Capabilities — A preceding case study where the same base model was post-processed in a different direction. This is also the source of the comparison target, CyberGym 84.5.

If you found this article helpful, please give it a like❤️ or leave a comment! If you have any questions, feel free to ask in the comments section.

## Innovating your business with technology

Thank you for reading this article. Are you worried about issues like "I want to build a system, but the initial investment is too high" or "I want to develop an IoT product, but I'm anxious about both the technology and the costs"?

### ✨ Develop SaaS with 0 yen initial cost. PonoTech's Micro-SaaS Service

We at PonoTech Co., Ltd. will **develop a custom SaaS for you with 0 yen initial cost**.

**Features of Micro-SaaS Development**

- **0 yen initial cost** : Free development of web apps, mobile apps, and desktop apps
- **Provided via monthly subscription** : Start using it with a monthly fee based on the features you need
- **Buyout option** : You can also purchase the system as your own company asset while using the subscription

"I want a custom SaaS for my company, but a multi-million yen initial investment is tough." This is the optimal solution for companies like yours.

### 🔧 Supporting manufacturing through IoT contract development × subsidy utilization

We provide end-to-end development from hardware to software. Build your IoT system while reducing costs by up to 66% by leveraging subsidies.

**Reduce system development costs by leveraging subsidies**

 As an IT introduction support provider certified by the Ministry of Economy, Trade and Industry, we offer the following services as a one-stop solution.

- **Subsidy diagnosis and application support** : Proposing the optimal subsidy with an 82.8% adoption rate record
- **End-to-end support from requirements definition to development** : Development structure 100% compliant with subsidy requirements
- **Labor-saving subsidies, manufacturing subsidies, etc.** : Support for utilizing subsidies of up to 80 million yen

### 📚 Latest book 'ComfyUI Master Guide' released on May 9th!

The image generation AI book 'ComfyUI Master Guide,' co-authored by our representative Yasuhiro Hayano, who possesses deep insights into AI technology, was released by SB Creative Corp. on May 9, 2025. If you want to understand the forefront of AI image generation, please take a look.

### 📈 Ideal for companies with these challenges

- “I want a dedicated SaaS, but I want to keep initial investment low.”
- “I want to develop an IoT product, but I need expertise in both hardware and software.”
- “I want to utilize subsidies, but I don't know how to apply.”
- “I want to start with a subscription and buy it out if necessary.”

### 🌟 Creating a secure future together with three strengths

1. **Micro-SaaS development** : Build a dedicated system with 0 yen initial cost
2. **IoT contract development** : Consistent support from hardware to software
3. **Subsidy utilization** : Reduce development costs by up to 66%

Please take advantage of our free initial consultation. Let's think about your company's growth strategy together from both technical and management perspectives. 

Click here to contact Ponotech Co., Ltd.!

▶ **Take the first step to accelerate growth through technology**

— Creating the future with technology. Ponotech Co., Ltd.
