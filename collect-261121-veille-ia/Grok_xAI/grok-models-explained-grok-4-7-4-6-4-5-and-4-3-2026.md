---
id: collect-261121-veille-ia/grok-xai/grok-models-explained-grok-4-7-4-6-4-5-and-4-3-2026
title: "Grok Models Explained: Grok 4.7, 4.6, 4.5 and 4.3 (2026)"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "models", "tokens", "build", "coding", "fast", "reasoning"]
source: https://www.ai-toolbox.co/grok-models/grok-models-explained-2026
source_anchor: ""
source_lines: [1, 315]
section: "Grok_xAI"
---
xAI's newest Grok model is Grok 4.7, released September 21, 2026 with a 500K token context window, on the API, in Cursor and in Grok Build. The rest of the lineup is Grok 4.6 (August 2026, 500K), Grok 4.5 (July 2026, 500K), Grok 4.3 and Grok 4.20, which hold 1M tokens, and Grok Build 0.1, a 256K coding model. In the Grok apps, xAI's new X Subscription plan pages promise the "latest Grok models" on Premium and higher without naming a model per plan, while grok.com's model picker now labels its Expert and Heavy modes as Grok 4.7. Older models such as Grok 4, Grok 4.1 Fast and Grok 3 were retired from the API on May 15, 2026 (docs.x.ai and x.ai, October 2026). The newest model is not the one with the biggest context window, which is the most common mistake when choosing.

This guide explains which Grok models exist as of October 5, 2026, what each one is for, their context windows and reasoning settings, when each was released, which ones come with which Grok plan, and how to pick. It also covers xAI's image, video and voice models, and what happened to the names you may still see in older articles. Every specification comes from xAI's model pages, release notes and announcements, linked in the sources at the end. Prices are not repeated here; they are in our Grok pricing and plans guide.

By Adi Leviim, Co-Founder at Infi Developments. 7+ years building AI productivity tools. Specifications checked against docs.x.ai and x.ai on October 5, 2026.

Which Grok Models Are Available Today?

xAI's model documentation lists six Grok text models on the API: Grok 4.7, Grok 4.6, Grok 4.5, Grok 4.3, Grok 4.20 (in reasoning, non-reasoning and multi-agent versions) and Grok Build 0.1. All six accept text and image input and return text. xAI's own advice on its models page is short: "For everything else, including code, use Grok 4.7. It is the most capable model we've built."

Model

API name

On the API since

Context window

Reasoning effort

What xAI says it is for

Grok 4.7

grok-4.7

September 21, 2026

500K tokens

low, medium, high (default), xhigh

Frontier model for coding, agentic tasks and knowledge work

Grok 4.6

grok-4.6

August 12, 2026

500K tokens

low, medium, high (default), xhigh

Previous flagship for coding, agentic tasks and knowledge work, with a focus on long-running agents

Grok 4.5

grok-4.5

July 8, 2026

500K tokens

low, medium, high (default), xhigh

Coding model for agentic software, engineering and workflow tasks

Grok 4.3

grok-4.3

Before May 2026 (no date published)

1M tokens

none, low (default), medium, high, xhigh

Fast, reliable model with strong tool calling and instruction following

High-speed agentic tool calling with strict prompt adherence

Grok 4.20 Multi-Agent (beta)

grok-4.20-multi-agent-0309

March 10, 2026

1M tokens

Reasoning

Multiple agents collaborating in parallel on deep research

Grok Build 0.1

grok-build-0.1

May 19, 2026 (early access)

256K tokens

Reasoning

Agentic coding workflows

xAI's release notes say both Grok 4.7 and Grok 4.6 have "no text output limit." Like every Grok model, they do not know current events unless you enable the Web Search or X Search tools. For per-model token prices, see the Grok API pricing table.

Grok Model Release Timeline

xAI released or retired a Grok model roughly every month in 2026, which is why so many articles still name models that no longer exist. These dates come from xAI's API release notes and news posts.

July 9, 2025: Grok 4 launches, alongside a new SuperGrok Heavy tier with Grok 4 Heavy.

November 19, 2025: Grok 4.1 Fast becomes available in the xAI Enterprise API.

March 10, 2026: Grok 4.20 and Grok 4.20 Multi-Agent go live on the API.

May 15, 2026: xAI retires grok-4-0709, the Grok 4.1 Fast and Grok 4 Fast models, grok-code-fast-1 and grok-3 from the API. Requests to those names are redirected to Grok 4.3, or to Grok Build 0.1 for grok-code-fast-1.

May 19, 2026: Grok Build 0.1, xAI's coding model, enters early access.

July 8, 2026: Grok 4.5 arrives on the API (xAI's announcement post is dated July 16).

July 22, 2026: Grok 4.5 rolls out on grok.com, X and the iOS and Android apps, and xAI says it "now powers Grok wherever you use it." grok.com's model picker now names the model again, labeling Expert and Heavy as Grok 4.7.

August 7, 2026: Imagine Image 2.0 is announced.

August 12, 2026: Grok 4.6 is released.

September 21, 2026: Grok 4.7 is released on the xAI API, in Cursor and in Grok Build, at the same price as Grok 4.6, with a Grok 4.7 Fast variant at twice the price in Cursor and Grok Build only.

November 2, 2026: grok-imagine-image-quality and grok-imagine-image-pro are now served by grok-imagine-image-2.0 at low quality.

Grok 4.7 vs Grok 4.6 vs Grok 4.5 vs Grok 4.3: What Changed

Grok 4.7, Grok 4.6 and Grok 4.5 trade half of Grok 4.3's context window for stronger coding and agent performance. Grok 4.3 holds 1M tokens; the three newer models hold 500K.

Grok 4.7 uses "a new, larger base model compared to Grok 4.6," with a longer reinforcement learning run weighted toward tasks that take many hours, and xAI says it is better at verifying its own work and managing longer context. It is served "at the same price and speed as Grok 4.6." In xAI's announcement table, Grok 4.7 at xhigh effort scores 46.3% on CursorBench 4.0 against 40.4% for Grok 4.6 at high, 37.6% on Terminal-Bench 4.0 against 20.3%, and 71.0% on DeepSWE v1.1 (at high effort) against 65.2%; on GDPval it reaches an Elo of 1695 against 1605. These are xAI's published figures, not independent tests.

Grok 4.5 was, in xAI's words, "trained alongside Cursor" on datasets spanning coding, science, engineering and math, with reinforcement learning across hundreds of thousands of multi-step software tasks. xAI says it is served at about 80 tokens per second and used about 4.2 times fewer output tokens than Opus 4.8 (max) per SWE Bench Pro task (15,954 against 67,020 on average). Its reported scores include 83.3% on Terminal Bench 2.1 and 64.7% on SWE Bench Pro.

Grok 4.6 builds on Grok 4.5 "with a particular focus on long-running agents and more ambitious interactive and visual work." xAI says it had a longer supplemental training run and adds more self-testing on long tasks. In xAI's own evaluation table, Grok 4.6 High scores 61 on the Artificial Analysis Intelligence Index against 56 for Grok 4.5 High, 69.9% on CursorBench 3.2 against 66.7%, and 65.9% on DeepSWE 1.1 against 54%. These are xAI's published figures, not independent tests.

Grok 4.3 remains useful for jobs the newer pair does not cover. It has the 1M window, a "none" reasoning setting for fast, cheap non-reasoning calls, and lower per-token prices than Grok 4.7, Grok 4.6 and Grok 4.5. The 20% Batch API discount, once a Grok 4.3 advantage, now applies to Grok 4.7, Grok 4.6 and Grok 4.5 as well (docs.x.ai, October 25, 2026). It is also where most retired model names now redirect. Our older Grok 4.5 vs Grok 4.3 comparison goes deeper on that pair.

Which Grok Model Should You Use?

Pick by the job and the size of your input, not by the version number.

Software engineers and coding agents: Grok 4.7, xAI's recommended code model, through the API, Cursor or Grok Build. Grok Build 0.1 is the lighter coding option inside a 256K window.

Analysts feeding in very large documents or codebases: Grok 4.3 or Grok 4.20, because a 1M window takes twice what Grok 4.7 or Grok 4.6 accepts in one request.

Researchers running deep, multi-source investigations: Grok 4.20 Multi-Agent on the API, or Ultra in the Grok apps, which xAI describes as having the most powerful Chat models.

Developers processing large volumes overnight: Grok 4.3 or Grok 4.20 through the Batch API, the cheapest Grok text models that get the 20% batch discount.

Latency-sensitive apps that do not need reasoning: Grok 4.3 with reasoning set to none, or Grok 4.20 non-reasoning.

Everyday users of grok.com or X: you do not choose an API model, but grok.com's picker offers Auto (chooses Fast or Expert), Fast (quick responses), Expert (thinks hard) and Heavy (a team of experts), and labels Expert and Heavy as Grok 4.7; xAI's plan pages promise the latest Grok models from Premium up.

Grok Context Windows: Why the Newest Model Is Not the Largest

A context window is how much text a model can consider in one request, and in Grok's lineup Grok 4.3 and Grok 4.20 hold 1M tokens, twice Grok 4.7, Grok 4.6 and Grok 4.5 at 500K, while Grok Build 0.1 holds 256K. If you routinely paste long contracts, research folders or whole repositories, a 1M model can matter more than the newest one.

Two details change the maths. First, xAI bills every token in a request at higher long context rates once a prompt reaches 200K tokens, on every Grok text model, so a large window is available but not cheap to fill. Second, the numbers above are API limits. xAI does not publish a token figure for the Grok apps; its plan pages only describe relative usage, with Premium at 4x, Super at 15x and Ultra at 50x the Plus allowance. Earlier figures such as "2M tokens for Grok 4.20" or "128K on the free web app" do not appear on xAI's current model, pricing or plan pages.

Long Grok chats still fill up without warning. The AI Toolbox context window meter estimates how full a conversation is and how many messages remain, so you can move to a fresh chat before earlier context drops out. To measure a prompt before sending it, use the token counter.

Which Grok Models Come With Each Plan?

xAI's X Subscription plan pages, read on October 25, 2026, do not list a model per plan: Premium and higher promise the "latest Grok models," and Ultra adds the "most powerful Chat models." On grok.com itself, the model picker labels Expert and Heavy as Grok 4.7. What changes with the plan is how much you can use Grok and which tools come with it.

Free: limited Chat, Imagine and Grok Build. No video generation, no Grok API access and no Grok Bot.

Plus ($8 a month): base usage across Grok Chat, Voice, Imagine and Build, Grok API access and an X checkmark. No Grok Bot or video.

Premium ($30): 4x the Plus usage, the latest Grok models, Grok Bot (Grok Bot 1.0 since November 11, 2026, shareable AI teammates in Slack, Microsoft Teams and video calls), and image and video generation.

Super ($100): 15x the Plus usage, 1080p video and priority model access at peak times.

Ultra ($200): 50x the Plus usage, the most powerful Chat models and the earliest access to new products and models.

Older SuperGrok plans: SuperGrok Lite, SuperGrok, SuperGrok Plus and SuperGrok Heavy continue for existing subscribers. xAI does not publish which model they run.

Monthly and annual prices for each tier, and how X Premium fits in, are in the Grok pricing guide.

Grok Imagine, Voice and Other xAI Models

Beyond the text models, xAI's docs list dedicated models for images, video and voice. xAI's recommendation table maps images to Grok Imagine Image 2.0, video to Grok Imagine Video 1.5 and voice to the Grok Voice API.

Grok Imagine Image 2.0: image generation and editing with low, medium or auto quality at 1K or 2K, and up to five source images per edit since August 28, 2026.

Grok Imagine Video 1.5: text-to-video, image-to-video and reference-to-video with optional preset voices, up to 1080p.

Grok Voice Think Fast 2.0: xAI's speech-to-speech model, released July 29, 2026, with separate speech-to-text and text-to-speech APIs.

How Grok Model Names and Aliases Work

Every Grok API model has a fixed name plus aliases that move to newer versions automatically. xAI's convention is that the plain model name points to the latest stable version, a name ending in "-latest" points to the latest version, and a name ending in a date points to one specific release that never changes.

Retired names keep working but are served by a different model. Since May 15, 2026, a request to a retired reasoning model is answered by Grok 4.3 with low reasoning effort, and a request to a retired non-reasoning model by Grok 4.3 with no reasoning, billed at Grok 4.3 rates.

Retired model name

Now served by

grok-4-0709 (Grok 4)

grok-4.3, low reasoning

grok-4-1-fast-reasoning

grok-4.3, low reasoning

grok-4-1-fast-non-reasoning

grok-4.3, no reasoning

grok-4-fast-reasoning

grok-4.3, low reasoning

grok-4-fast-non-reasoning

grok-4.3, no reasoning

grok-3

grok-4.3, no reasoning

grok-code-fast-1

grok-build-0.1

If your code still calls one of these names, set the replacement model explicitly so you control the reasoning effort you pay for, as xAI recommends in its migration guide.

Using AI Toolbox With Any Grok Model

AI Toolbox (formerly ChatGPT Toolbox) is a Chrome extension that works inside Grok on x.com/i/grok and grok.com, so it behaves the same whichever model or plan you use. It adds tools for managing your conversation history rather than changing the model: full-text search across your synced Grok messages, folders and subfolders, Smart Tags, a prompt library, the context meter, and export as TXT, Markdown, JSON or PDF.

AI Toolbox Grok module

Free

Premium

Full-text search

25 results per search

Unlimited results

Folders

2 folders

Unlimited folders

Export conversations

TXT, one chat at a time

TXT, Markdown, JSON, PDF

Bulk export and delete

Not included

Included

Context window meter

Included

Included

Premium for the Grok module is $9.99 a month or $99 lifetime, and the purchase is linked to your X handle. All Access unlocks the ChatGPT, Gemini, Claude and Grok modules for $199 once; it is an extension license, not an AI subscription. For step-by-step guides, see finding a specific Grok message, managing Grok chat history and the Grok power-user guide. Still choosing an assistant? Our Grok vs ChatGPT and Grok vs Claude comparisons cover where each leads.

Limitations and What xAI Documents Well

xAI's developer documentation is thorough for API models: each model page states the context window, reasoning settings, batch support and rate limits. That makes the API side of this guide easy to verify. The gaps are elsewhere. xAI publishes no release date for Grok 4.3 in its release notes, no token figure for the context the Grok apps give each plan, and no independent benchmark data; the scores quoted above are xAI's own. None of the xAI pages cited here mention a Grok 5 release, so this guide does not predict one.

Model names change quickly at xAI, and aliases can silently point to a newer model, so check the models page at docs.x.ai before hard-coding a model into production. AI Toolbox does not change which model Grok uses or what your plan allows.

Frequently Asked Questions

What is the latest Grok model?

Grok 4.7 is the latest Grok model. xAI released it on September 21, 2026 as its frontier model for coding, agentic tasks and knowledge work, with a 500K token context window, on the xAI API, in Cursor and in Grok Build. In the Grok apps, xAI's plan pages promise the latest Grok models on Premium and higher without naming one per plan, and grok.com's model picker labels its Expert and Heavy modes as Grok 4.7. xAI has not released a newer Grok text model since.

Which Grok model has the biggest context window?

Grok 4.3 and Grok 4.20 have the biggest context windows among current Grok models, at 1M tokens each. Grok 4.7, Grok 4.6 and Grok 4.5 hold 500K tokens, and Grok Build 0.1 holds 256K. These are API limits from docs.x.ai; xAI does not publish a token figure for the Grok apps.

Is Grok 4.6 better than Grok 4.3?

For coding and agent tasks, Grok 4.6 is the stronger of the two in xAI's positioning, and Grok 4.7 has since replaced it as xAI's recommended model. Grok 4.3 still has advantages: a 1M token window against 500K, a no-reasoning mode and lower prices, $1.25 and $2.50 per million tokens against $2.00 and $6.00. For very large inputs or high-volume cheap calls, Grok 4.3 can be the better fit.

What happened to Grok 4.1 Fast and Grok 4?

xAI retired Grok 4 (grok-4-0709), Grok 4.1 Fast, Grok 4 Fast and Grok 3 from its API on May 15, 2026. Requests to those model names still work but are served by Grok 4.3 and billed at Grok 4.3 rates. grok-code-fast-1 now redirects to Grok Build 0.1.

Is Grok 4 Heavy still available?

Grok 4 Heavy launched with the SuperGrok Heavy tier in July 2025, but xAI's current model and plan pages no longer name it, and SuperGrok Heavy has not been sold to new customers since The X Subscription replaced it on October 12, 2026. Ultra is now the top individual plan. grok.com does offer a Heavy mode (a team of experts), but its picker labels it Grok 4.7, not Grok 4 Heavy. On the API, the closest option is Grok 4.20 Multi-Agent.

Can free Grok users use Grok 4.6?

Free users get Grok with limited Chat, Imagine and Grok Build. xAI's plan pages do not name a model per plan, but grok.com's model picker now labels its Expert and Heavy modes as Grok 4.7 rather than Grok 4.6. Video generation, Grok API access and Grok Bot are not included on Free.

Does AI Toolbox work with every Grok model?

Yes. AI Toolbox runs in your browser on x.com/i/grok and grok.com, so it does not depend on the model or plan you use. It adds full-text search, folders, Smart Tags, a prompt library, a context meter and export on top of Grok: TXT of any single chat for free, and Markdown, JSON and PDF on Premium.

A Full Stack Developer with 7+ years of experience building AI productivity tools. Leads product development and frontend architecture for AI Toolbox, the Chrome extension suite (ChatGPT, Gemini, Grok, and Claude modules) that helps users search, organize, and export their AI conversations.

Liked this guide? Get the next one first.

Join 40,000+ readers getting practical prompts, new features, and subscriber-only deals. No spam, unsubscribe in one click.

New features and product updates before anyone else

Time-saving prompts and workflows for ChatGPT, Gemini, Claude, and Grok
