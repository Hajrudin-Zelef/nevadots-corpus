---
id: collect-261121-veille-ia/gpt-openai/gpt-6-luna-openai-s-fast-everyday-model-explained
title: "GPT-6 Luna: OpenAI's Fast Everyday Model, Explained"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-22"]
keywords: ["luna", "gpt-6", "model", "openai", "tokens", "everyday", "chat", "replies"]
source: https://gpt.me/models/gpt-6-luna
source_anchor: ""
source_lines: [1, 78]
section: "GPT_OpenAI"
---
## GPT-6 Luna at a glance

GPT-6 Luna is the smallest and quickest model in OpenAI's GPT-6 generation. OpenAI released it on September 22, 2026, together with GPT-6 Sol, and describes it as its most efficient model for focused, high-volume work. Since October 8, 2026 it also powers the free and Go plans of ChatGPT.

On GPT.me, Luna is the model that answers when you open the chat and type. It replies almost at once, reads images you attach, and handles the bulk of everyday questions well. Chat with GPT-6 Luna for free, no account needed.

- **Maker:** OpenAI (GPT.me is independent and not affiliated with OpenAI).
- **Input:** text and images.**Output:** text.
- **Context window:** 1,050,000 tokens on OpenAI's platform, with up to 128,000 output tokens.
- **Knowledge cut-off:** May 18, 2026, according to OpenAI's model documentation.
- **API price:** $0.10 per million input tokens and $0.50 per million output tokens (OpenRouter list price, Nov 2026).

## Where Luna sits in the GPT-6 family

Since GPT-5.6, OpenAI names its models by generation plus tier. Luna is the entry tier: the cheapest and fastest member of the family. Above it are GPT-6.1 Terra, a mid-tier model with stronger writing and visual reasoning; GPT-6 Sol and GPT-6.1 Sol, the high-end models for complex coding and analysis; and GPT-6 Astra, the flagship. OpenAI says Sol and Luna were trained with methods similar to those used for Astra, so Luna is a scaled-down member of the same generation, not a separate product line.

Luna replaced GPT-5.6 Luna, which listed at $0.20 per million input tokens and $1.20 per million output tokens. The new model costs less on both counts. Compared with Terra, Luna's API price is a tenth, and compared with Astra it is a hundredth. That price gap is the main reason it serves so many free and high-volume products.

## What GPT-6 Luna is good at

Luna works best when the task is clear and the answer does not need a long chain of reasoning. In practice that covers most of what people type into a chat box:

- **Quick explanations.** A term, a historical event, how something works. Weather vs climate.
- **Summaries and extraction.** Paste an article or a long email and ask for the main points, dates or action items. The summarizer does this in one step.
- **Everyday writing.** Replies, short posts, product descriptions, cover-letter openers. Decline a meeting politely.
- **Rewriting and translation.** Change tone, shorten a paragraph, or turn English into Spanish and back with the translator.
- **Reading images.** Describe a photo, read text from a screenshot or explain a simple chart.
- **Brainstorming.** Names, gift ideas, ten angles for a school project.

## Where Luna falls short, and when to press Think

A small, fast model trades some depth for speed. You will notice it in a few places:

- **Multi-step maths and logic.** Luna can skip a step or carry a wrong number forward, especially when it answers without thinking first.
- **Tricky code.** It writes short functions well but misses edge cases in larger pieces of code.
- **Subtle writing.** For a speech, a sensitive message or a long story, its drafts can feel flat.
- **Recent events.** Its knowledge ends in May 2026, and our chat cannot search the web to fill the gap.

When you hit one of these, press the *Think* chip. The next answer then comes from GPT-6.1 Terra, which spends a few seconds reasoning before it replies. Our Luna vs Terra comparison shows the differences in more detail.

## Luna on GPT.me compared with ChatGPT and the API

It is the same model everywhere, but the setup around it differs:

- **On GPT.me** Luna is tuned for quick replies with very little hidden reasoning and a moderate answer length. Very long conversations are trimmed from the oldest messages, so the model sees only the recent part. The huge context window exists on OpenAI's side but is not usable here. There is no web search, voice or image creation.
- **In ChatGPT** Luna sits inside a full product with search, file uploads, voice and saved history, plus ChatGPT's own thinking settings.
- **Through the API** developers choose the reasoning effort themselves, from none up to max, and can send very long documents. See our GPT API guide for code.

## Getting better answers from Luna

1. **Be specific about the output.** "Five bullet points, plain words" works better than "summarize".
2. **Give it the text.** Luna is strongest when it works on material you paste rather than on what it remembers.
3. **Ask it to check itself.** For numbers, add "show the calculation", then read the steps.
4. **Split big jobs.** Ask for an outline first, then each part.

More ideas are in our guide to GPT prompts that work and in the prompt library.

## FAQ

### Is GPT-6 Luna free?

On GPT.me, yes: it is the default model and you get a few free messages every day without an account. ChatGPT's free plan also uses GPT-6 Luna. Through the API it is paid per token.

### Is GPT-6 Luna the same model ChatGPT uses?

ChatGPT's free and Go plans use GPT-6 Luna in the main chat since October 2026. Paid plans use GPT-6 Sol. ChatGPT adds its own tools and settings around the model, so answers can differ in style.

### Can GPT-6 Luna see images?

Yes. It accepts images as input. In our chat you can attach one image per message; it cannot create images.

### How current is GPT-6 Luna's knowledge?

OpenAI lists a knowledge cut-off of May 18, 2026. It does not know later events unless you paste the information into the chat.

### Should I use Luna or Terra?

Start with Luna for everyday questions. Switch to GPT-6.1 Terra with the Think chip for maths, logic, careful writing or when Luna's answer looks wrong.
