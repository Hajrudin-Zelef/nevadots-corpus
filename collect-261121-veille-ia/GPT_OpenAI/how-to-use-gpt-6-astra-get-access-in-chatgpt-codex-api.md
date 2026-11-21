---
id: collect-261121-veille-ia/gpt-openai/how-to-use-gpt-6-astra-get-access-in-chatgpt-codex-api
title: "How to Use GPT-6 Astra: Get Access in ChatGPT, Codex, API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "gpt-6", "chatgpt", "model", "openai", "access", "plan", "plans"]
source: https://www.layer3labs.io/guides/how-to-use-gpt-6-astra
source_anchor: ""
source_lines: [1, 186]
section: "GPT_OpenAI"
---
# How to Use GPT-6 Astra: Getting Started on Every Platform

Learn every way to access GPT-6 Astra, which plan each requires, and practical first steps to show its strengths in your business workflows.

Quick answer: on an eligible paid plan (ChatGPT Plus, Pro, Business, Enterprise, or an OpenAI API billing account), open a new chat and pick GPT-6 Astra from the model dropdown, or set `model: gpt-6-astra` in an API request; if it is missing, the rollout has not reached the account yet or the plan needs upgrading.

Getting started with GPT-6 Astra means understanding where it's available, which plans include access, and how it differs from the tools you may already use.

OpenAI introduced GPT-6 Astra, its newest general-purpose AI model, to selected organizations on September 3, 2026, before making it publicly available on September 4. The model is accessible through consumer apps, cloud platforms, and the OpenAI API. It's built to handle much larger contexts and more advanced computer-based workflows.

Compared with ChatGPT or the GPT-5.6 line, Astra's biggest distinction is its context window of more than one million tokens. It also offers stronger performance in software engineering, browsing, and complex professional tasks. According to OpenAI, Astra is faster and more reliable across multi-step workflows, with an emphasis on advanced computer use and document production. New capabilities include recurrent depth reasoning and stronger cybersecurity controls.

For regulated businesses, technical teams, and professional services, Astra can support document automation, research, and code-heavy workflows, particularly when a task involves large amounts of information or sensitive processes that require added context and security.

This guide covers the key steps for getting started with GPT-6 Astra, whichever platform you prefer.

## Release Details and Access Methods for GPT-6 Astra

GPT-6 Astra is an OpenAI model that launched on September 3, 2026 to a limited set of organisations, followed by general release on September 4, 2026. Users access the model through a paid ChatGPT plan of Plus or higher, an OpenAI API billing account, or AWS. ChatGPT 6 Astra and ChatGPT Astra 6 are other names for the same model.

ChatGPT Free and Go plans do not include GPT-6 Astra, and OpenAI offers no free tier or free trial. To learn more about unpaid access rules, read Is GPT-6 Astra free?. For a comparison, consult GPT-6.1 Sol vs GPT-6 Astra.

Running GPT-6 Astra requires no separate download or installer; GPT-6 Astra runs inside the ChatGPT web app, mobile apps, Codex, and the API. On a ChatGPT Plus account, GPT-6 Astra appears in ChatGPT Work and Codex as rollout reaches that user, and it does not show in the ordinary chat list. OpenAI is also rolling out GPT-6 Pro across Pro, Business, and Enterprise plans.

Developers can call the model in the API using the model identifier gpt-6-astra. OpenAI prices usage at $10 per 1M input tokens, $50 per 1M output tokens, and $1 per 1M cached input tokens. You can examine GPT-6 Astra pricing or inspect the OpenAI model documentation.

## Where to Access GPT-6 Astra and Which Plans Apply

You can use GPT-6 Astra on multiple platforms, but each platform comes with its own plan and access requirements. At launch, Astra is available through OpenAI’s ChatGPT web interface, consumer mobile apps, the OpenAI API, and AWS cloud integrations.

Within ChatGPT the rollout is not uniform: GPT-6 Pro, powered by Astra, is rolling out on the Pro, Business, and Enterprise plans, while Plus plans include Astra in ChatGPT Work and Codex. On Plus it does not appear in the ordinary chat model list. If you use the API, you can select the 'gpt-6-astra' model, but usage is billed at published rates. AWS also lists Astra for business and enterprise teams as an API endpoint. Daybreak early-access organizations already have Astra enabled.

Verify which plan is active before starting, because capabilities and usage limits can change, and availability updates appear on OpenAI’s own site.

ChatGPT Free and Go plans do not include GPT-6 Astra. Using the model requires a paid ChatGPT subscription on Plus or higher, or an OpenAI API billing account. To review plan rates and a sample monthly budget, see our GPT-6 Astra pricing breakdown and our detailed overview of GPT-6 Astra API pricing.

- ChatGPT Pro, Business, and Enterprise web/chat apps: access rolls out automatically. On Plus, Astra appears in ChatGPT Work and Codex only.
- OpenAI API: available with API key, billed at $10 per 1M input tokens and $50 per 1M output tokens.
- AWS (Amazon Web Services): available for integration, see AWS documentation for region-by-region rollout.
- Daybreak early testers: Astra shipped ahead of general release.
- Always check OpenAI’s site for the current rollout and eligibility.

## Can You Use ChatGPT's GPT-6 Astra for Free?

You cannot use ChatGPT's GPT-6 Astra for free. OpenAI does not include GPT-6 Astra in the ChatGPT Free or Go plans, and the model offers no separate free trial or free tier. Access requires either a paid ChatGPT plan on Plus or higher, or an OpenAI API billing account.

Subscribers on ChatGPT Plus receive access in ChatGPT Work and Codex as the rollout reaches each account. Teams on Pro, Business, and Enterprise plans run GPT-6 Pro on Astra. The model requires no separate download, running entirely within the ChatGPT web and mobile apps or through the API.

API access costs $10 for input and $50 for output per 1 million tokens, alongside $1 for cached input. To see how these account rules work, read Is GPT-6 Astra free? or review the complete GPT-6 Astra pricing schedule. Anyone checking academic access can also consult the guide on GPT-6 Astra for students.

## Model Selection Steps by Plan

To use GPT-6 Astra, you select the model inside existing ChatGPT interfaces or pass its identifier to the Application Programming Interface (API), as there is no separate download or account signup. Where you click to launch the model depends on your specific subscription tier.

- ChatGPT Free and Go: Neither plan includes GPT-6 Astra, meaning no toggle exists for these accounts. You can review non-paid limitations in our GPT-6 Astra free tier guide.
- Plus: Astra does not appear in the standard chat dropdown. In the desktop application, click Work and choose the model name at the bottom of the message box. For programming sessions, click ChatGPT in the top-left corner, select Codex, and click Select effort at the bottom-right.
- Pro, Business, and Enterprise: Astra is rolling out automatically to the standard model dropdown menu. GPT-6 Pro is powered by Astra.
- API: Developers access Astra by setting the model parameter to `gpt-6-astra` . Calls cost $10 per 1 million input tokens and $50 per 1 million output tokens, as detailed in our GPT-6 Astra pricing guide.

## How to Select GPT-6 Astra on Each Platform

You select GPT-6 Astra in ChatGPT by switching to Work mode in the desktop app or choosing Codex mode from the menu. The model does not appear in the standard chat dropdown on ChatGPT Plus accounts. Subscribers on Pro, Business, and Enterprise plans also receive GPT-6 Astra in ordinary chat as OpenAI expands the rollout.

In Work mode, a reasoning-effort slider sits directly beside the model selector. In Codex mode, the "Select effort" control lets you choose the model and the effort level together. This setting controls how much the model reasons before answering a prompt. Higher reasoning effort increases response time and uses more compute, so set the level according to task complexity.

Developers calling the model programmatically specify 'model: gpt-6-astra' in their OpenAI API requests. For enterprise integrations, GPT-6 Astra is also available as a cloud endpoint on AWS to support internal infrastructure.

- ChatGPT Desktop App (Work mode): Open the desktop app and click "Work". Click the model name at the bottom of the message box, pick GPT-6 Astra, and adjust the effort slider next to it.
- ChatGPT (Codex mode): Click "ChatGPT" in the top-left corner and select "Codex" from the dropdown. Click "Select effort" at the bottom-right of the message box, then pick GPT-6 Astra along with an effort level.
- ChatGPT standard chat: Pro, Business, and Enterprise subscribers can pick GPT-6 Astra from the standard model dropdown once OpenAI rolls it out to their accounts.
- OpenAI API: Pass 'model: gpt-6-astra' in the request body.
- AWS: Select GPT-6 Astra as a cloud endpoint for enterprise and business infrastructure.

## App Downloads, Mobile Access, Accounts, and API Keys

GPT-6 Astra requires no separate software installation or standalone account signup. The model runs inside the standard ChatGPT web interface and existing mobile apps. You do not need to download an extra program, desktop client, or browser extension to start.

On phones and tablets, the model is available through the regular ChatGPT app on iOS and Android from the App Store or Google Play. Sign in with your existing OpenAI credentials, open a new chat, and choose GPT-6 Astra from the model dropdown as described in the model-selection section.

Account access follows your existing subscription tier. Plus, Pro, Business, and Enterprise accounts receive access based on the rules outlined in the availability-access section. There is no separate Astra registration or secondary billing profile.

You do not need an application programming interface (API) key to chat with the model. An API key is required only for software developers calling OpenAI endpoints programmatically. To compare monthly subscriptions for chat access, check the GPT-6 Astra pricing guide, or see the GPT-6 Astra API pricing guide to review developer token rates.

## Best First Steps: Workflows to Show Astra’s Strengths

The most effective way to show GPT-6 Astra’s capabilities is to pick an initial workflow that uses its larger context and upgraded reasoning.

Try feeding Astra a long, complex document such as a contract, research report, or technical system description and request structured output, policy analysis, or code review. You can also ask it to summarize, answer questions about multiple files, solve a math or logic problem that crosses many inputs, or generate entire code modules with explanations.

OpenAI says Astra is strongest at advanced computer use, research, and document workflows. Results appear faster and with fewer command errors, especially on longer or more iterative tasks.

- Upload or paste a large technical document for summary or structured extraction.
- Request a software component or code review across multiple files in one session.
- Submit detailed policy, compliance, or risk analysis queries.
- Try a multi-step professional flow (e.g., research–draft–presentation) in one prompt sequence.

## Prompting Tips and Model-Specific Notes

Prompting GPT-6 Astra follows similar best practices to earlier models, but the expanded context window and new reasoning method unlock new approaches.

Take advantage of the model’s ability to process long multi-document prompts; cite parts of the input directly or reference previously generated outputs. Use structured prompts for code generation, file search, or compliance checking. GPT-6 Astra can call functions and handle images and web input directly, so prompt it to use those features where enabled.

Note the model’s 'recurrent depth' can make its reasoning path less transparent, so for high-stakes uses, break tasks into steps and request justified output for critical results.

- Use longer prompts up to the 1M token context limit, especially for multi-part workflows.
- Specify output structure when possible (tables, JSON, sections) to improve reliability.
- Instruct Astra to show reasoning when accuracy is critical.
- For software/code, spell out requirements in sequence and request full module explanations.

## Why GPT-6 Astra Is Not Showing Up for You

If GPT-6 Astra is missing from your model list, the most likely reason is that it sits somewhere else in your account. OpenAI announced Astra on September 3, 2026 and rolled it out gradually, and availability can differ between Chat, Work, and Codex on the same login. Checking only the chat window is a common reason people conclude they do not have it.

Plan tier decides which surface you get. GPT-6 Pro, powered by GPT-6 Astra, is rolling out in ChatGPT on the Pro, Business, and Enterprise plans. Plus plans include GPT-6 Astra in ChatGPT Work and Codex as the rollout reaches an account. On Plus it is absent from the ordinary chat model list. A Plus subscriber looking for Astra in the chat picker is looking in the wrong place. That is expected behavior. Nothing is wrong with the account. Free and Go do not include it.

Three further causes come up often. On an Enterprise workspace Astra is disabled by default until an administrator enables it, so an eligible seat can still see nothing. In Codex the model id is the exact string gpt-6-astra, and support for it landed in Codex CLI v0.153.1 on September 3, 2026. A client older than that cannot list the model no matter what the account is entitled to. OpenAI's API returns a 403 when a request comes from an unsupported country, region, or territory.

Buying your way forward only works for one of these. Plus subscribers can buy credits for more Work and Codex usage. Credits do not grant early access during a rollout. No amount of spend fixes a stale client, an admin toggle, or a region block. Diagnose first: work out whether the failure happens at sign-in, at model-list time, or per request, and the cause usually names itself. For the same diagnosis across other vendors, see why a new AI model isn't showing up for you.

- Check Work and Codex as well as Chat. The same account can have Astra in one and not the other.
- On Plus, Astra lives in Work and Codex. It is absent from the chat model list.
- On Enterprise, an admin has to enable Astra in the OpenAI admin console first.
- In Codex, run codex models to see what your account can actually reach, and confirm your CLI is v0.153.1 or newer.
- Credits buy more usage. They do not buy earlier access.

## Common Setup Mistakes and How to Avoid Them

The most frequent problems with GPT-6 Astra come from plan eligibility, incorrect model selection, and exceeding context or output limits.

Users sometimes select an old model by mistake or try to use Astra features before their access tier updates. Large prompts can exceed the context window if not checked, and output over 128,000 tokens is truncated. Billing may increase for complex jobs with high output, so always check current token pricing and limits on OpenAI’s site. For what actually happens when a prompt crosses the 1M token context limit, see our breakdown of GPT-6 Astra's limits.

Astra’s new reasoning can cause responses to be less step-by-step; consider breaking up complicated or high-stakes tasks.

- Not confirming your account/plan grants Astra access.
- Neglecting to select the Astra model in app or API.
- Submitting prompts that exceed context/output limits.
- Assuming your old safety and compliance controls still apply, when Astra’s are stricter.
- Ignoring billing changes. Check OpenAI’s pricing page for latest rates.

## The Names People Use for GPT-6 Astra

If you searched for GPT Astra, ChatGPT Astra, Astra 6, or just Astra, you are looking for GPT-6 Astra. They all refer to the same model, and only GPT-6 Astra is the official model name.

OpenAI first named Astra in an August 1, 2026 research post about mathematics. Six days later, OpenAI paused parts of the work after a safety evaluation.

Astra remained the only public name during the pre-launch period. The model shipped as GPT-6 Astra on September 3, 2026, and general release followed on September 4. Coverage written before September 3 says Astra or OpenAI Astra, because the shipped name did not exist yet.

People also type Chat GPT Astra, Astra GPT, and GPT Astra 6 when they mean the same model. OpenAI does not use those variants as official names.

A Pro account shows GPT-6 Pro rather than Astra in the model list. That is the plan-level product name for the same model, not a second one.

The one name that must be exact is the model id used in the API and in Codex: `gpt-6-astra`. Search terms can vary in spacing and word order, but a near-miss model id fails outright in an API request or a Codex command.

## Using GPT-6 Astra for Web Design and Roblox Game Development

GPT-6 Astra handles web design and game development by writing code, suggesting layouts, and debugging errors through its general programming capabilities. The model lacks a live preview. It writes and debugs Hypertext Markup Language (HTML), Cascading Style Sheets (CSS), JavaScript (JS), and React code, but you must view the layout in your own browser.

For Roblox projects, GPT-6 Astra writes Luau code, explains Roblox Studio application programming interface (API) methods, and fixes broken game logic. Roblox Studio has no direct connection to OpenAI models. You must manually copy the generated Luau scripts into your Roblox Studio project to test them in your game.

## How to Use GPT-6 Astra

To use GPT-6 Astra effectively, start by choosing an interface that exposes the exact model you need for your workload. Test a new configuration on a single representative task, such as drafting a routine email or parsing a short document, before expanding to full automation. Inspect the saved output directly to confirm formatting, accuracy, and adherence to your prompt instructions.

For coding tasks, Cursor serves as an editor option when the target model appears on its supported list. Check current model availability before setup to confirm the exact model is listed. Once connected, save your prompts and review the generated diffs locally before applying changes to your main codebase.

## Frequently Asked Questions

- There is no separate download for GPT-6 Astra. The model requires no standalone software installation, running directly inside the standard ChatGPT web interface, existing mobile apps, or through the OpenAI application programming interface (API). To use it, update or open the ChatGPT app and check your plan and rollout status. Free and Go plans do not include it, and Plus subscribers should see the "Why GPT-6 Astra Is Not Showing Up for You" section on why Astra may not show up in the ordinary chat list.
- You can access GPT-6 Astra if you have a ChatGPT Plus, Pro, Business, or Enterprise account, or if you have API access through OpenAI or AWS.
- No, you do not need ChatGPT Pro if you already have Plus, unless you specifically want GPT-6 Astra inside the ordinary chat window. ChatGPT Plus already includes GPT-6 Astra access through ChatGPT Work and Codex, but the model does not appear in the standard chat model dropdown list. ChatGPT Pro, Business, and Enterprise plans expose the model directly inside the default chat window, so Pro is only needed if you prefer that interface over Work or Codex.
- Set the model parameter to 'gpt-6-astra' in your API request. Check OpenAI’s documentation for current usage instructions.
- GPT-6 Astra supports a context window of 1,050,000 tokens and a maximum output of 128,000 tokens per response. Exceeding these limits will truncate your output.
- OpenAI lists GPT-6 Astra at $10 per million input tokens and $50 per million output tokens on the API. Always verify rates on OpenAI’s pricing page, as they may change.
- OpenAI states that GPT-6 Astra is the first to achieve Critical cybersecurity capability under its Preparedness Framework. Advanced controls withhold certain features for safety.
- Yes, the model supports large documents, image input, function calling, file search, and web input. Availability depends on your account and platform.
- Check your plan status, look for eligibility updates, and verify whether your account is included in the rollout. Astra may not yet be enabled for all users.
- Break the instruction into smaller, single-step messages first. GPT-6 Astra's recurrent-depth reasoning reworks a task internally before answering, so one long multi-part instruction is more likely to get partially followed than several short ones sent in sequence. Also confirm Astra is selected in the model menu rather than a prior model, since a missed selection is a common cause of an instruction going unrecognized.
- Ask for the image directly inside a ChatGPT conversation with Astra selected, the same way you would ask for text. For example, type "draw a rocket launching over a city at sunset." ChatGPT's built-in image generation runs inside the same conversation as whichever model you have selected, so the request reaches it without a separate tool or menu.
- No, GPT-6 Astra has no native 3D modeling, STL export, or direct Blender-control feature published by OpenAI. It can handle image input, generate a reference image in ChatGPT, or attempt to write a raw STL or OBJ mesh or a Blender Python script as a code-generation task. OpenAI has not published that workflow as supported or tested, so results vary and you would need to open, check, and likely fix the output in Blender or a CAD application.
- Yes, your existing custom GPTs, saved prompts, and skills remain available with GPT-6 Astra because ChatGPT manages them at the account level instead of inside a single model. Switching models in the dropdown should leave those stored configurations intact across your chats. Because Astra relies on recurrent-depth reasoning, your prompting style may need slight adjustments, so an existing prompt benefits from a quick re-check instead of a full rewrite.

### Book Your Free AI Compliance Review

Book a 30-minute session with Layer3Labs to discuss how GPT-6 Astra could fit into your compliance, automation, or workflow goals. We focus on regulated industries and can help you navigate safe adoption of new AI releases.

Book Now
