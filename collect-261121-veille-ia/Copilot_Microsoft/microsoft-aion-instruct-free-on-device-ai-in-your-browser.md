---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-aion-instruct-free-on-device-ai-in-your-browser
title: "Microsoft Aion Instruct: Free On-Device AI in Your Browser"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-19"]
keywords: ["aion", "instruct", "microsoft", "model", "on-device", "browser", "free", "them"]
source: http://invovai.blogspot.com/2026/11/microsoft-aion-instruct-2026.html
source_anchor: ""
source_lines: [1, 161]
section: "Copilot_Microsoft"
---
Imagine opening your web browser and asking it to summarise a long article, rewrite a rough email, or sort a pile of customer reviews, and getting an answer in seconds without a single word of your text leaving your computer. No subscription, no API key, no waiting for a distant server, and no worry that your private documents are being uploaded somewhere else. That is the promise of Aion Instruct, Microsoft's new small language model that now lives inside Microsoft Edge. On 19 November 2026, Microsoft announced that Aion Instruct is generally available, and the announcement matters far beyond the world of browser developers. It is one of the clearest signs yet that useful AI is moving out of the cloud and onto the devices we already own, and it could change how millions of people work, write, read, and make decisions on the web every day.

For anyone who uses AI tools for productivity, this is a development worth understanding properly. Aion Instruct is not another chatbot you have to sign up for, and it is not a premium feature locked behind a monthly fee. It is a free, open-weights model that ships inside a mainstream browser, ready for websites to use and for anyone to download and experiment with. In this guide, we explain what Aion Instruct is, what changed on 19 November 2026, how on-device AI in Edge actually works, what you can realistically use it for at work and at home, and where its limits lie. We also compare it honestly with the big cloud AI tools most people already use, so you can decide when on-device AI is the right choice and when you should still reach for something bigger.

## Aion Instruct

Aion Instruct is a small language model, often shortened to SLM, built by Microsoft for on-device AI. It has 2.3 billion parameters, which is the number that describes the size of the model, and it has been tuned to follow instructions, which is why it carries the name Instruct. In plain English, it is a compact AI brain designed to run on ordinary computers rather than in a giant data centre. It is built for everyday text work: writing and rewriting, summarising, answering questions about a document, following instructions, and producing structured outputs such as lists and short summaries.

The full open-weights release is published on Hugging Face under the name microsoft/Aion-1.0-Instruct-2.3B, and it is free to download. Open weights means that Microsoft has published the trained model files themselves, so developers and researchers can download them, inspect them, run them, and build on top of them, subject to the licence terms. That openness matters because it builds trust. Anyone can see exactly which model is running, test it, and check how it behaves, instead of having to take a company's word for it.

Aion Instruct is the new built-in small language model in Microsoft Edge, where it replaces Phi-4-mini, the model that previously did this job. Microsoft says that at 2.3 billion parameters, Aion is smaller and faster than Phi, while delivering comparable quality for common web tasks such as text synthesis and generation, summarisation, instruction following, and structured outputs. Because it is smaller, it can also run on many more devices than its predecessor, which is a big part of the point. A model that only works on the newest, most powerful machines helps very few people. A model that runs on ordinary laptops and desktops can help almost everyone.

It is worth being clear about what Aion Instruct is not. It is not a replacement for large cloud models such as GPT, Claude, or Gemini, and Microsoft does not present it as one. It is a specialist for fast, everyday, on-device tasks. Think of it like the difference between a pocket calculator and a supercomputer. You would not use a calculator to model the climate, but for the sums you actually do every day, it is faster, cheaper, and always there when you need it. Aion Instruct plays a similar role for AI on the web.

## What Changed on 19 November 2026

The announcement Microsoft published on its Edge developer blog on 19 November 2026 contained three updates that arrived together, and each one matters for a different group of people. Understanding the three parts separately makes the whole story much clearer.

### Aion Instruct reaches general availability

The first and biggest update is that Aion Instruct is now generally available. That means it has moved out of preview and into a finished, supported release. It is available in two forms at the same time. First, it is available as an open-weights model on Hugging Face, free to download. Second, it is available as the built-in small language model inside Microsoft Edge itself. Microsoft says it incorporated feedback from early adopters and refined the model's training before taking it to general availability, which is the normal path from a promising preview to a product ordinary users can rely on.

The announcement also confirmed that Aion Instruct is coming to Windows later in 2026, laying the foundation for richer on-device AI experiences across Windows and Copilot+ PCs. That is an important detail for the future. Today the model lives in the browser, but Microsoft's plan is clearly to make the same kind of on-device intelligence available across the operating system, so that Windows features can use it too. If you own a Copilot+ PC, this is a development to watch closely, because it suggests that more of the AI features on your machine will soon run locally rather than in the cloud.

### The Prompt API origin trial opens on live sites

The second update is an origin trial for the Prompt API. Until now, the Prompt API had only been available as a developer preview in the Edge Canary and Dev channels, which are the early test versions of the browser used mainly by developers. An origin trial changes that. It lets website developers register their live site and call the Prompt API, and therefore Aion Instruct, from real production pages in Microsoft Edge, testing the feature with a much broader audience.

For ordinary users, the practical meaning is simple. Websites you visit in Edge will soon be able to offer AI features powered by the model already sitting in your browser, without sending your text to a server. For developers, it is a chance to bring on-device AI features to a site, evaluate them under real-world conditions, and refine the implementation based on actual usage. Microsoft also says the feedback from this trial will help inform discussions with other browser vendors and the web community as the Prompt API is considered for standardisation, which could eventually mean similar on-device AI features in other browsers too.

### A developer preview of the Decisions API

The third update is a developer preview of an experimental Decisions API, powered by Aion Instruct, available in the Edge Canary and Dev channels for prototyping. This one is aimed squarely at developers, and Microsoft is clear that both the API and its implementation may change as it learns from developer feedback. Even so, it points to a genuinely new kind of on-device AI capability, and it is worth understanding.

The Decisions API enables a website to ask yes-or-no, multiple-choice, or scoring questions about a piece of text and get back probabilities and a confidence value, computed in a single pass without any text generation. In other words, instead of asking an AI model to write an answer, you ask it to judge something and tell you how sure it is. Microsoft gives examples such as understanding a user's intent and acting on it, sorting and routing user-generated content, or scoring and ranking items in real time. Imagine a support inbox that can instantly tell whether a message is a complaint, a question, or a refund request, and how confident it is in that judgement, all without the message ever leaving the user's device. That is the kind of task the Decisions API is built for.

## How On-Device AI in Edge Works

To appreciate why Aion Instruct matters, it helps to understand how on-device AI in Edge actually works, because it is quite different from the AI most people are used to.

With a cloud AI tool, your text travels over the internet to a server, the server runs a huge model, and the answer travels back to you. That works well, but it has costs. It needs an internet connection, it takes time, it usually costs money at scale, and it means your data leaves your device. On-device AI flips that around. The model is downloaded to your computer once, and then it runs locally, on your own hardware. Your text stays on your machine, the answers come back faster because there is no round trip to a server, and the whole thing keeps working even when your connection is slow or unavailable.

Edge handles the download sensibly. Microsoft says Edge downloads Aion Instruct only when a site or feature first needs it, and only on devices that can run it well. You do not have to go looking for the model or install anything yourself. It arrives quietly in the background when it is needed, and only if your device can handle it. Just as importantly, users stay in control. Microsoft says users can turn off on-device AI in Edge settings, which removes the model and stops future downloads. If you do not want on-device AI on your machine, you can switch it off.

The Prompt API is the bridge between websites and the model. It lets a site send a prompt, which is just an instruction plus some text, to the built-in model and get a generated answer back. Because the model is already in the browser, the site does not need to stand up its own inference servers, pay for cloud compute, or ask the user to create an account with an AI provider. The website simply calls the API, and the browser does the work. Microsoft describes this as part of a hybrid approach. Developers can use on-device AI models when they are available in the browser and well suited to the task, and switch to a configured cloud provider to extend device coverage or access more powerful inference when needed. That hybrid idea is sensible. It means small, fast, private tasks run locally, while the hardest jobs can still go to the cloud when they need to.

The Decisions API works differently, and that difference is what makes it interesting. A normal language model generates text one word at a time, which takes time and computing power. The Decisions API skips the text generation entirely. It looks at a piece of text, considers a question about it, and returns a structured answer in one pass: a label, the probability for each possible answer, and a confidence value. Because it does not generate text, it is fast and cheap to run, which makes it practical for jobs that need to happen instantly and repeatedly, such as classifying incoming messages, filtering content, or scoring reviews as they arrive.

## Practical Productivity Uses for Aion Instruct

The technology is interesting, but the real question for most readers is what it can actually do for them. Here are the practical productivity uses where a small on-device model like Aion Instruct makes the most sense.

### Writing help, right where you work

The most obvious use is writing assistance inside the browser. Many people write emails, reports, social media posts, and messages in browser-based tools such as Gmail, Outlook on the web, Google Docs, Notion, and countless other web apps. With Aion Instruct built into Edge, those web apps can offer rewriting, shortening, expanding, and tone adjustment without sending your draft to a server. You could ask the model to make a rough email sound more professional, turn a long paragraph into two crisp sentences, or draft a reply based on the thread you are reading. Because the work happens locally, it is fast enough to feel instant, and you do not have to copy sensitive text into a separate AI website.

### Summarising long pages and documents

Summarisation is one of the tasks Microsoft specifically names for Aion Instruct, and it is easy to see why. Long articles, research papers, meeting notes, and reports are a daily reality for knowledge workers, students, and managers. An on-device model can read a page you already have open and give you the key points in a few bullet points, without uploading the document anywhere. That is useful for a quick sense of whether a twenty-page report is worth reading in full, for catching up on a long email thread, or for pulling the action items out of meeting notes. Because the model runs locally, you can summarise confidential material, such as a contract or an internal memo, without worrying that it has been sent to a third party.

### Fast decisions and sorting

The Decisions API opens up a different kind of productivity use: sorting and triage. Think of a small business owner who receives dozens of customer messages a day, or a support team that needs to route each new ticket to the right person. A website powered by the Decisions API could look at each message and instantly decide whether it is a complaint, a sales enquiry, or a request for help, and how urgent it is, with a confidence score attached. Low-confidence cases can go to a human for review, while clear-cut ones are routed automatically. The same idea applies to sorting reviews, flagging inappropriate content, or ranking a list of items by relevance. These are jobs where a fast, structured judgement is more valuable than a beautifully written paragraph, and they are exactly what a small model running locally is good at.

### Offline and low-connectivity use

One of the most underrated benefits of on-device AI is that it works when the internet does not. If you are on a plane, on a train with patchy signal, in a rural area with slow broadband, or simply dealing with an outage, cloud AI tools stop working. A model that lives in your browser keeps going. You can still summarise a document you have saved locally, polish a draft, or get help structuring your notes. For students, travellers, and anyone who works away from a reliable connection, that independence is a genuine productivity gain, not just a technical curiosity.

### Everyday help for students and small businesses

For students, Aion Instruct could power free study tools built into the websites they already use: explaining a difficult paragraph in simpler words, turning lecture notes into flashcards or quiz questions, or helping structure an essay outline. For small businesses, it could bring AI features to their own websites and tools without the cost and complexity of cloud AI bills. A small shop could add a help feature to its site that answers common questions, or a freelancer could use a browser-based tool to tidy up proposals and quotes, all without paying per request. Because the model is free and runs on the user's device, the economics change completely. AI features stop being a premium add-on and start being a standard part of the web.

## How to Try Aion Instruct

Trying Aion Instruct is refreshingly simple compared with most AI tools, because there are two straightforward routes, depending on whether you are a regular user or a developer.

### Use it in Microsoft Edge

For most people, the easiest route is simply to use Microsoft Edge. Aion Instruct ships as the browser's built-in model, so there is nothing to install separately. Edge downloads the model only when a site or feature first needs it, and only on devices that can run it well, so you may not even notice it arriving. As websites adopt the Prompt API, the AI features will appear inside the sites you already visit, in Edge. If you want to check that on-device AI is available, look in Edge settings, where Microsoft says you can also turn the feature off and remove the model if you prefer. Keep Edge updated to the latest version, because on-device AI features are rolling out with recent releases, and the Prompt API origin trial is tied to current versions of the browser.

### Download it from Hugging Face

If you are a developer, researcher, or enthusiast, you can download the model directly from Hugging Face, where it is published as microsoft/Aion-1.0-Instruct-2.3B. It is free to download, and the open-weights release means you can run it in your own projects, test it against your own data, and build tools on top of it. The model is released under the MIT licence, according to the model card, which is one of the most permissive open-source licences, making it easy to use in both personal and commercial projects. Be aware that running the model yourself requires a machine with enough memory and a suitable setup, and the model card is the best place to check the current technical requirements before you download it. Quantised versions that use less memory are expected in future, according to reports of the release, which should make it easier to run on more modest hardware over time.

### For developers: the Prompt API and Decisions API

Developers who want to build with on-device AI in the browser should start with Microsoft's official documentation and playground samples. To use the Prompt API on a live site, you need to register for the Prompt API origin trial, which involves registering your domain and adding the trial token to your pages. Microsoft provides developer documentation, a playground sample to experiment with, and a GitHub discussion where you can share feedback on the API's design and the model's performance. The Decisions API is earlier in its life. It is available as a developer preview in Edge Canary and Dev channels, so you will need one of those test builds of Edge to try it, and you should expect the API to change. Microsoft provides an explainer, developer documentation, and a playground sample for it as well, and it is actively asking developers what they are building and whether the design works for them.

## Privacy and Offline Benefits

Privacy is arguably the strongest reason to care about on-device AI, and it is worth spelling out clearly. When an AI model runs on your device, your prompts and your documents do not need to be sent to a company's servers. That matters for personal privacy, and it matters even more in professional settings. Lawyers, doctors, accountants, journalists, and anyone handling confidential information often cannot paste sensitive material into a cloud AI tool, because doing so would breach client confidentiality, company policy, or data protection rules. An on-device model changes that calculation. The text stays on the machine, so the risk of exposure is far lower.

There are also practical benefits beyond privacy. On-device AI removes the per-request cost that cloud AI carries, which is why websites can offer AI features to every visitor without worrying about the bill. It reduces latency, because there is no network round trip, so answers feel instant. It keeps working offline, as described above. And it gives users control: the model can be turned off in Edge settings, and the download only happens when it is needed. None of this means on-device AI is right for every task, but for the large number of everyday jobs that do not need a giant model, the privacy and speed advantages are hard to ignore.

## Limitations of a Small 2.3B Model

Honesty matters here, because a 2.3 billion parameter model is small by modern standards, and small models have real limits. Microsoft is upfront about this, and the model card makes it clear that Aion Instruct has not been trained to excel in world knowledge. In practice, that means you should expect it to be good at working with text you give it, and much less reliable when you ask it about facts it is supposed to know from memory.

Complex, multi-step reasoning is another weak point. A small model can follow a clear instruction and handle a well-defined task, but it is more likely to struggle with problems that require careful step-by-step thinking, nuanced judgement, or connecting ideas across a long and complicated document. It can also make mistakes, miss details, and occasionally produce a confident-sounding answer that is simply wrong. That is true of all AI models, but it is more common with smaller ones, so fact-checking matters even more.

The model also has a limited context window, which is the amount of text it can consider at once. Very long documents may need to be summarised in chunks rather than all at once. It supports a limited set of languages, focused on major world languages such as English, German, Spanish, French, Italian, Portuguese, Chinese, and Japanese, so speakers of other languages may find it less useful. And because it is designed for text, it will not help you generate images, analyse video, or write complex code in the way larger cloud models can. None of these limits make Aion Instruct useless. They simply define the jobs it is good at, and the jobs where you should still reach for a bigger tool.

## Aion Instruct Compared With Cloud AI Tools

The fairest way to think about Aion Instruct is not as a competitor to cloud AI tools, but as a complement to them. Each approach has strengths the other lacks, and the smartest setup uses both.

Cloud AI tools such as ChatGPT, Claude, Gemini, and Microsoft Copilot run huge models on powerful servers. They are far more capable at deep reasoning, creative writing, complex coding, and answering questions from their broad knowledge. They can handle very long documents, work in many languages, and produce polished, detailed outputs. The trade-off is that they need an internet connection, they usually cost money for full access, they are slower because of the network round trip, and your data leaves your device.

Aion Instruct is the opposite in almost every way. It is smaller and less capable at hard reasoning, but it is free, fast, private, and available offline. It runs on your device, so there is no subscription, no per-request fee, and no data leaving your machine. For everyday tasks such as summarising a page, rewriting a paragraph, or classifying a message, it is often the better tool, because the task is simple enough that a small model can do it well, and the speed and privacy advantages matter more than raw power.

Microsoft's hybrid intelligence idea captures this well. The vision is that the browser uses the on-device model when it is available and well suited to the task, and switches to a cloud provider when the job needs more power or the device cannot run the local model. That is a sensible future, and it is one users will barely notice. They will simply get fast, free AI help for everyday jobs, and deeper cloud AI help when they need it, without having to think about which model is doing the work.

There is also a cost angle that matters for websites and small businesses. A site that wants to add AI features using a cloud model has to pay for every request, which adds up quickly with a large audience. A site that uses the Prompt API and Aion Instruct pays nothing per request, because the work happens on the visitor's own device. That could make AI features affordable for small sites, blogs, and independent developers who could never justify a cloud AI bill, and it could mean that many more websites start offering genuinely useful AI help.

## Pros and Cons

Every tool has strengths and weaknesses, and Aion Instruct is no exception. Here is an honest summary.

### Pros

Aion Instruct is free, both as an open-weights download on Hugging Face and as the built-in model in Microsoft Edge, with no subscription or per-request cost. It runs on your device, so your text stays private and is not uploaded to a server. It is fast, because there is no network round trip, and it works offline, which matters on planes, trains, and anywhere with poor connectivity. It is small enough to run on many more devices than its predecessor, Phi-4-mini, while delivering comparable quality for common web tasks. It is open weights, which builds trust and lets developers and researchers inspect it, test it, and build on it. It supports practical everyday tasks such as writing, summarising, instruction following, and structured outputs. And it comes with two useful APIs, the Prompt API for general generation and the Decisions API for fast structured judgements, giving developers flexible ways to build with it.

### Cons

As a small 2.3 billion parameter model, Aion Instruct is less capable than large cloud models at complex reasoning, deep analysis, and creative work. It is not trained to excel in world knowledge, so it can miss details, make errors, and produce confident but wrong answers, which means its output needs checking. It has a limited context window and a limited set of supported languages. It is designed for text only, so it will not help with images, video, or other media. The Prompt API is still in an origin trial rather than a finished standard, and the Decisions API is an experimental preview that may change, so developers should not build production features on them without watching for updates. Finally, on-device AI only works on devices that can run the model well, so not every user will have access to it straight away.

## Frequently Asked Questions

### Is Aion Instruct really free?

Yes. Aion Instruct is free to download as an open-weights model from Hugging Face, and it ships as the built-in small language model in Microsoft Edge at no extra cost. There is no subscription or per-request fee for using the model itself. Websites that use it through the Prompt API do not pay per request either, because the work happens on the user's device. That said, always check the official Microsoft and Edge pages for the current details, because features, availability, and terms can change over time.

### Do I need an internet connection to use Aion Instruct?

You need an internet connection the first time, because Edge has to download the model, and only when a site or feature first needs it. After that, the model runs on your device, so many tasks can work offline. A website that uses the Prompt API still needs to load in the browser, but the AI processing itself happens locally. That makes on-device AI genuinely useful on flights, in areas with poor signal, and during internet outages.

### How is Aion Instruct different from Phi-4-mini?

Aion Instruct replaces Phi-4-mini as the built-in small language model in Microsoft Edge. Microsoft says Aion is smaller and faster than Phi, while delivering comparable quality for common web tasks such as summarisation, text generation, instruction following, and structured outputs. Because it is smaller, it can also run on many more devices. The change is an upgrade in efficiency and reach rather than a completely different kind of model.

### Can I use Aion Instruct for work documents and private information?

That is one of its best use cases. Because the model runs on your device, your text does not need to be sent to a server, which makes it far more suitable for sensitive material than cloud AI tools. Even so, you should still follow your employer's policies and check the settings on any website that uses the model, because the site itself may handle your data in its own way. On-device processing removes the biggest privacy risk, but it does not replace common sense.

### What can the Decisions API do that the Prompt API cannot?

The Prompt API generates text, such as a summary or a rewritten paragraph. The Decisions API does something different: it answers yes-or-no, multiple-choice, or scoring questions about a piece of text in a single pass, returning probabilities and a confidence value without generating any text. That makes it fast and cheap for jobs such as classifying messages, routing support tickets, scoring reviews, or understanding user intent. It is currently an experimental developer preview in Edge Canary and Dev, so it is for prototyping rather than production use.

### Will Aion Instruct come to Windows and Copilot+ PCs?

Yes, that is Microsoft's stated plan. The announcement says Aion Instruct is coming to Windows later in 2026, laying the foundation for richer on-device AI experiences across Windows and Copilot+ PCs. That suggests the same model will power more Windows features in future, bringing the same benefits of speed, privacy, and offline use to the operating system itself. Check the official Microsoft and Windows pages for the latest timing and availability, as rollout plans can change.

## Conclusion

Aion Instruct is a quiet but important step in the story of AI. It does not have the headline drama of a giant new cloud model, and it will not write your novel or solve your hardest research problem. What it does is something arguably more useful for most people: it puts capable, free, private AI inside the browser you already use, ready to help with the everyday text tasks that fill a working day. Summarising, rewriting, classifying, and deciding, all without a subscription, without uploading your words to a server, and without waiting for a distant data centre.

The 19 November 2026 announcement matters because it brings three pieces together at once. The model itself is generally available, free, and open. The Prompt API origin trial lets real websites use it on live pages. And the Decisions API preview points to a new kind of fast, structured on-device judgement that could make websites smarter in ways that have nothing to do with chatbots. Together, they describe a future in which AI is not a separate website you visit, but a built-in capability of the web itself.

For everyday users, the advice is simple. Keep Microsoft Edge updated, try the AI features as they appear on the sites you use, and use on-device AI for the quick, private, everyday jobs it is good at, while keeping your cloud AI tools for the deep, complex work that needs a bigger model. For developers and small businesses, now is the time to experiment, because the Prompt API origin trial and the free open-weights model make it possible to add AI features without the cost and complexity that used to stand in the way. Aion Instruct will not replace cloud AI, but it does not need to. It fills a gap that cloud AI never could, and for millions of people, that gap is where most of their daily work actually happens.

## 0 Comments
