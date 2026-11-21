---
id: collect-261121-veille-ia/actu-business/meta-ai
title: "Meta AI"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-01"]
keywords: ["agent", "meta", "comparison", "agents", "muse", "assistants", "personal", "automation"]
source: https://primo.ai/w/index.php?title=Meta_AI&diff=next&oldid=51911
source_anchor: ""
source_lines: [1, 220]
section: "Actu_Business"
---
# Meta AI: Difference between revisions

|  |  | m Text replacement - "* Agents/Assistants ... Agent Catalog ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ... Robotic Process Automation ... LangChain" to "* Agents/Assistants ... Agent Catalog ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ...... |  | 
| Line 15: |  | Line 15: |  | 
|  | * [[Meta Muse]] \| [[Meta]]   |  | * [[Meta Muse]] \| [[Meta]]   | 
|  | * [[...build an agent]] ... [[Ready-to-Use AI Agent Comparison]] ... [[Visual and Low-Code Agent Builder Comparison]] ... [[AI Coding Agent Comparison]] ... [[Agent Development Framework Comparison]] |  | * [[...build an agent]] ... [[Ready-to-Use AI Agent Comparison]] ... [[Visual and Low-Code Agent Builder Comparison]] ... [[AI Coding Agent Comparison]] ... [[Agent Development Framework Comparison]] | 
|  | * [[Agents/Assistants]] ... [[AI Agent Catalog\|Agent Catalog]] ... [[Personal Companions]] ... [[Negotiation]] ... [[Personal Productivity\|Productivity]] ... [[Email]] ... [[Web Automation]] ... [[Robotic Process Automation (RPA)~~\|Robotic Process Automation~~ ]] ... [[LangChain]] |  | * [[Agents/Assistants]] ... [[AI Agent Catalog\|Agent Catalog]] ... [[Personal Companions]] ... [[Negotiation]] ... [[Personal Productivity\|Productivity]] ... [[Email]] ... [[Web Automation]] ... [[Robotic Process Automation (RPA)]] ... [[LangChain]] | 
|  | * [[Benchmarks#Agents/Assistants_Bookmarks\|Agents/Assistants Bookmarks]] |  | * [[Benchmarks#Agents/Assistants_Bookmarks\|Agents/Assistants Bookmarks]] | 
|  | * [[Development]] ... [[Notebooks]] ... [[Development#AI Pair Programming Tools\|AI Pair Programming]] ... [[Codeless Options, Code Generators, Drag n' Drop\|Codeless, Generators, Drag n' Drop]] ... [[Hugging Face]] ... [[Algorithm Administration#AIOps/MLOps\|AIOps/MLOps]] ... [[Platforms: AI/Machine Learning as a Service (AIaaS/MLaaS)\|AIaaS/MLaaS]] |  | * [[Development]] ... [[Notebooks]] ... [[Development#AI Pair Programming Tools\|AI Pair Programming]] ... [[Codeless Options, Code Generators, Drag n' Drop\|Codeless, Generators, Drag n' Drop]] ... [[Hugging Face]] ... [[Algorithm Administration#AIOps/MLOps\|AIOps/MLOps]] ... [[Platforms: AI/Machine Learning as a Service (AIaaS/MLaaS)\|AIaaS/MLaaS]] | 

## Revision as of 17:08, 1 November 2026

- Meta Muse | Meta
- ...build an agent ... Ready-to-Use AI Agent Comparison ... Visual and Low-Code Agent Builder Comparison ... AI Coding Agent Comparison ... Agent Development Framework Comparison
- Agents/Assistants ... Agent Catalog ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ... Robotic Process Automation (RPA) ... LangChain
- Agents/Assistants Bookmarks
- Development ... Notebooks ... AI Pair Programming ... Codeless, Generators, Drag n' Drop ... Hugging Face ... AIOps/MLOps ... AIaaS/MLaaS
- Introducing Muse: The World's First Personal AI Agent Built for Everyone | Meta Newsroom
- Muse: Meta's personal AI agent | AI at Meta
- Download Muse | AI at Meta
- How to get started with Meta's new AI agent, Muse | Engadget
- Meta pushes into personal AI agents in Muse Spark family | CNBC
- Meta Superintelligence Labs | Wikipedia


**Muse** is Meta's first consumer AI agent, launched in the United States on September 8, 2026, after development under the internal codename "Hatch." Where Meta AI answers questions, Muse does the work: you tell it what needs to get done, and it browses the web, shops online, fills out forms, plans trips, sends emails, schedules events, and can even negotiate a bill on your behalf. Meta pitches it as an agent for everyone, with no technical experience needed and no learning curve, and behind its simple chat interface it may be writing code or setting up its own connections to services with public APIs in order to finish complex, multi-step tasks. Muse keeps working after the app is closed, returns when something changes or when it needs approval, and suggests ideas of its own based on what it has learned about you.

Muse is a separate product from the Meta AI app. It has its own iOS and Android apps, a web app at muse.ai, a native Mac app added on September 17, 2026, and a chat entry point inside WhatsApp, with support for Meta's AI glasses and a pocket device called Muse Charm announced for later in 2026. You need a Meta account, but not a Facebook or Instagram account, and you must be 18 or older. After launching in the US, Muse reached Canada on September 18, Japan on October 23 (its first market outside North America), and Singapore on October 27, 2026.

The agent runs on Meta's Muse family of models from Meta Superintelligence Labs (MSL), beginning with Muse Spark 1.3 at launch, and Meta says its newer Muse Wonder models will power more of Muse, starting with subscribers. Muse is free for most everyday use with a weekly usage limit, and two subscriptions, Power at $20 a month and Maximum at $100 a month, add more capacity. It is Meta's first paid consumer AI product: the company has said Muse carries no advertising, keeps conversations out of its ad systems, and expects to earn money by taking a small fee from transactions.

Muse differs from earlier assistants in two ways. Unlike a chatbot, it holds long-term goals and acts across connected services rather than producing a single answer, and unlike self-hosted agents such as OpenClaw, which inspired its design, it runs on infrastructure Meta manages, so ordinary users never have to install or secure anything themselves. That trade, convenience in exchange for trusting Meta with email, calendars, payments, and more, is the central tension in nearly every assessment of the product.


Muse is Meta's bet on "personal superintelligence": AI directed by each individual toward their own goals, rather than centrally automating all work.



## Mechanisms

Each Muse agent works inside its own **Muse Secure VM**, a dedicated cloud virtual machine on Meta's infrastructure that houses the agent, its browser, the user's data, and any stored credentials. Users can watch the agent work in its built-in browser, and no other user's agent can reach the machine. Inside the VM, the agent runs in a sealed runtime cell that is expected to handle untrusted data, while services outside that cell hold passwords and decide what the agent may do. Credentials are injected just in time at the network boundary by swapping a surrogate token for the real one at the moment of use, so the agent itself never holds a secret that could be stolen.

A second agent, **Sentinel**, runs on the same machine but is kept apart from Muse at the system level. Sentinel is the sole permission authority for connector actions and for any network request that tries to leave the VM: Muse proposes an action, and Sentinel answers allow, deny, or ask, based on the connector policy the user has set. When Sentinel needs the user, execution stops and the request appears as a system dialog in the Muse app rather than as a message in the conversation, so text injected by a malicious web page cannot fake an approval. Meta describes its defense against prompt injection as a set of layers:

- **Model training:** the model is trained to recognize and resist injected instructions
- **Harness marking:** anything that comes from an untrusted source is labeled as such before the model sees it
- **Classifiers:** independent models watch for injection attempts hidden in page text, images, and downloads, and block the agent if it tries to send personal data somewhere the task did not call for
- **Deterministic checks:** ordinary code verifies results and enforces permissions regardless of what the model decides
- **Credential separation:** because the agent never holds passwords or full card numbers, a successful injection has nothing valuable to steal

Payments follow the same principle. At launch Muse checked out through Link by Stripe, whose agent wallet issues a one-time-use card for each purchase, and Meta said Muse was the first AI agent covered by Link's purchase protections. Shopify then announced agentic checkout through Shop Pay on all Shopify stores, PayPal added checkout across its merchants, and Meta has said 1Password support will let Muse use logins a person already stores. In every case Muse asks before buying, and users get an audit trail of past and planned actions.

Muse's usefulness comes from memory and goals. It remembers details a user mentions once, consolidates what it has learned through regular memory compaction, turns long-term goals into action plans, and surfaces suggestions in an Ideas tab. Users can inspect what it remembers, correct it, or tell it to "forget" something. The model underneath is Muse Spark, a natively multimodal reasoning model introduced on April 8, 2026, with support for tool use, visual chain of thought, and multi-agent orchestration, including a "Contemplating" mode in which several sub-agents reason in parallel before their answers are merged.

### The Physics

A personal agent that holds a person's accounts must operate under hard constraints of trust, access, and capacity. Meta's own documentation is unusually candid about several of them, and outside researchers have documented others. The most important are:

- **Residual injection risk:** Meta's safety documentation states that Muse Spark 1.3 remains susceptible to adaptive jailbreaks and prompt injection in agentic settings, which is why the architecture assumes the model will sometimes be fooled. Meta backs the design with a bug bounty paying up to $300,000, including up to $130,000 for a working prompt injection that affects a single user.
- **Trust in the operator:** Today's Secure VM keeps other users out but is not inaccessible to Meta. Meta has promised a**Muse Confidential VM** later in 2026 that would encrypt the whole machine, including data and conversations, with a key only the user holds.
- **Platform permission:** Websites decide whether to admit outside agents. Amazon blocked Muse on September 21, 2026, saying Meta had not told it about the agent and that the agent did not identify itself while browsing.
- **Scale of agent traffic:** By late September, bot-management firm HUMAN Security attributed 72% of daily AI agent requests it observed to Muse. Around October 12, Meta began cryptographically signing some Muse requests with the Web Bot Auth standard so sites can verify them.
- **Capacity:** The free tier carries a weekly usage limit (about 100 million tokens a week, according to Mark Zuckerberg), and Meta is rolling out its strongest models to paying subscribers first.
- **Fallibility:** Meta warns in the product that "Muse may take unexpected actions," and early reviewers advise monitoring it closely, especially for email, shopping, and reservations, where mistakes are hard to undo.

These constraints shape how Muse should be used. Narrow permissions, read-only access where possible, and "always ask" approval settings reduce the damage any single error or attack can do, while the audit trail lets users check what actually happened. The same trade-offs apply to every agent that acts with real credentials, and they echo long-standing cybersecurity practice: give software the least access that does the job, and keep a human in the loop for anything irreversible.

### Why This Matters

Muse is Meta's attempt to turn its enormous investment in AI into a product people use daily and, increasingly, pay for. It also tests whether a company whose business has been built on advertising can earn enough trust to hold the keys to people's digital lives. Several factors make the launch significant:

- **Distribution at Meta scale:** Muse rose to the top of Apple's free app chart about 10 days after launch, outpacing ChatGPT's early mobile growth according to app-intelligence estimates, and by October 21, 2026, Meta reportedly counted more than 10 million account setups in the US and Canada.
- **A new revenue model:** Subscriptions and transaction fees give Meta income that does not depend on ads, and Muse for Small Business extends the same agent to owners who manage shops, restaurants, and service businesses.
- **Agentic commerce:** Partnerships with Stripe, Shopify, PayPal, and retailers such as Best Buy, Gap, Sephora, Walmart, Wayfair, Expedia, and Instacart make Muse a new storefront. Shares of Airbnb, Expedia, and Tripadvisor fell about 5% in the week Meta expanded Muse at its Connect event, reflecting fears that agents will sit between customers and brands.
- **A reference security design:** Analysts at DeepLearning.AI's The Batch noted that Meta built operating-system-level controls that should hold even when the model is fooled, applying ideas that security researchers had proposed for agents that combine private data, untrusted content, and the ability to send data out.
- **Toward personal superintelligence:** Meta frames Muse as the first step toward the personal superintelligence Zuckerberg has described, delivered through phones, glasses, a talking Realtime Avatar, and dedicated hardware.

Muse competes directly with Grok Bot, OpenAI's dots, Google's Gemini Spark, Anthropic's Claude Cowork and Claude Duet, and Perplexity Computer, and side-by-side reviews are collected in the Ready-to-Use AI Agent Comparison. Independent scorecards such as the Assistant Benchmark already test Muse alongside rivals on practical tasks, though products change weekly and results rest on small samples. Each of these agents makes different choices about where it runs, which services it reaches natively, and how often it stops to ask, so the best fit depends largely on the apps and accounts a person already relies on.

### Building Blocks

| Component | What It Does | Notes | 
|---|---|---|
| The brain: Muse model family | Muse Spark, a multimodal reasoning model from Meta Superintelligence Labs, powers the agent; Muse Spark 1.3 shipped on September 2, 2026, and Muse Wonder followed on October 12. | The family also includes Muse Image, Muse Video, and Muse Code, a terminal coding agent that arrived with Muse Spark 1.2. | 
| Muse Secure VM | A dedicated, isolated cloud computer with its own browser that houses the agent and the user's data. | The agent never sees actual passwords or payment details. A Confidential VM with user-held encryption keys is promised for later in 2026. | 
| Sentinel | A separate permission agent that approves, denies, or escalates every connector action and outbound network request. | Approval prompts appear as system dialogs in the app, not as chat messages. | 
| Connectors and integrations | Connects to services such as Google, Spotify, OpenTable, Instacart, Notion, and GitHub, and can reach any service with a public API, sometimes after manual setup. | The desktop app can also work with local Messages, Calendar, and Notes, and Meta announced control of macOS applications at Connect. | 
| Wallet | Stores a payment card through Stripe and pays with one-time-use cards; Shop Pay and PayPal checkout were added after launch. | Muse asks before every purchase. | 
| Proactive goals and Ideas | Takes on long-term goals as well as one-off tasks, turns goals into action plans, and suggests tasks in an Ideas tab. | Learns from conversations over time. | 
| Personalization | Users rename the agent and choose an avatar and communication style. | Meta's Realtime Avatar model, shown with the mascot "Jolly," enables video-style conversations with a visual companion. | 
| Agent email address | Gives the agent its own address so users can forward messages to it or copy it into threads. | Announced at Meta Connect on September 23, 2026. | 
| User control | Users decide how much access each connector gets, such as read-only email versus sending on their behalf, and can review an audit trail. | Data controls let users opt out of having interactions used to train Meta's models; training is on by default. | 
| Plans | A free tier for most everyday use, plus Power ($20 a month) and Maximum ($100 a month) for more usage. | New models such as Muse Wonder reach subscribers first. | 
| Muse for Small Business | Adds business skills and connectors to the same Muse app, including Instagram professional analytics, Facebook Pages, and Meta ad accounts. | Launched September 29, 2026, with 15 named connectors including Shopify, QuickBooks, Stripe, Canva, Slack, and Zoom. | 

## Getting Started

Setting up Muse takes a few minutes, and most of the important choices concern how much access to grant rather than how to configure features. Reviewers who tested Muse in its first weeks recommend starting with tight permissions and low-stakes tasks, then loosening controls as trust grows. The steps below follow Meta's launch materials and Engadget's setup guide:

1. **Check eligibility:** You must be 18 or older and in a supported market (the US, Canada, Japan, or Singapore as of late October 2026). You need a Meta account, but not a Facebook or Instagram account, and launch coverage reported that a payment card must be on file even to try the free tier.
2. **Get the app:** Download Muse for iOS or Android, open muse.ai in a browser, or install the Mac app. In supported regions you can also message Muse directly in WhatsApp.
3. **Sign in:** On the web, signing in with a phone number can link your relevant Meta accounts automatically.
4. **Personalize your agent:** Muse asks you to rename it and pick an avatar and communication style. The avatar is cosmetic, but a distinct name makes it easier to address the agent in WhatsApp and email.
5. **Opt out of training if you prefer:** In Settings, open Data controls and deselect "Help improve our AI models."
6. **Set permissions:** The default is "Ask for some actions." Choosing "Always ask" at first is the safer option, and you can loosen it later.
7. **Connect services selectively:** Use the Connectors tab to link apps such as Gmail, Spotify, or OpenTable, choosing read-only access where possible. Some users create a separate email account for their agent, and business owners can add connectors such as Shopify and QuickBooks through Muse for Small Business.
8. **Add a card only when needed:** To let Muse shop, add a card in the Wallet section. Stripe handles the connection, Muse never sees the full card number, and it checks in before buying.
9. **Hand off a first task:** Describe the outcome you want, any constraints such as budget or dates, and where you want to approve. Start with research or organizing tasks before anything that sends, books, or buys.
10. **Review and refine:** Check the Ideas tab for suggestions, watch the activity feed while Muse works, review the audit trail afterward, and tell Muse to "forget" anything it should not keep.
11. **Choose a plan:** Stay on the free tier while you learn the agent's habits, then move to Power or Maximum if you hit the weekly limit or want new models first.

A few habits make the difference between a helpful agent and a risky one. Keep approvals on for email, purchases, and reservations, where mistakes are hardest to undo, and avoid sharing chats that contain personal or business details, since Meta paused conversation sharing in October 2026 after links proved discoverable. Treat Muse's work as a draft you are responsible for, because Meta itself warns that the agent may take unexpected actions.

## Examples

Meta, early reviewers, and Meta's own executives have described a wide range of tasks that Muse can take on. The examples below are grouped by area of life, and they share a pattern: Muse gathers information across connected services, proposes a plan or draft, and pauses for approval before anything consequential happens.

### Everyday Life

- **Recipes and groceries:** Turn a recipe reel saved on Instagram into a grocery list, or pull recipes saved across Facebook and Instagram into one searchable, organized collection.
- **Entertaining:** Suggest a dinner party menu, remember each friend's dietary restrictions, and draft the invitations.
- **Travel:** Research an international trip, compare nonstop flights, and set up a price tracker that reports changes. Zuckerberg has described using Muse to chase hard-to-get mountain climbing permits.
- **Family logistics:** Build a daily family briefing from calendars and email, summarize school PDFs, or produce a family newsletter as a finished PDF in one request.
- **Local guides:** Turn saved posts into a restaurant guide organized by city.

### Money and Errands

- **Lowering bills:** Research competing offers and negotiate with a provider, including by phone, to lower a recurring bill.
- **Selling and buying:** Help sell a car for more money, list or buy items on Facebook Marketplace, track prices, and complete purchases with one-time virtual cards.
- **Finding services:** Call or message local businesses to find an available plumber, contractor, or appointment.

### Goals and Projects

- **Fitness:** Adjust a training plan as a schedule changes over weeks or months.
- **Long-running projects:** Coordinate multi-week efforts, such as the weekend baking projects and robot builds Zuckerberg says he plans with Muse, tracking materials, steps, and timing.
- **Learning and hobbies:** Turn a vague goal into a schedule of practice sessions, resources, and check-ins that Muse revisits over time.

### Small Business

Meta's chief AI officer Alexandr Wang said early users were already running plumbing businesses, grocery stores, farms, restaurants, and shops with Muse before the dedicated business features arrived. With Muse for Small Business, an owner can connect Shopify, QuickBooks, Stripe, Canva, Klaviyo, and Meta ad accounts so the agent can review sales, draft promotions, prepare bookkeeping summaries, and suggest ad changes. Nothing is posted, sent, or spent until the owner approves it.

### Sample Requests

The table below shows illustrative requests built from the capabilities Meta and reviewers have documented, along with what Muse typically does and where a careful user would expect to approve. These are not scripted commands, because Muse accepts ordinary language, so stating the outcome, the constraints, and the approval points matters more than exact wording. The approval column reflects Muse's default behavior of checking in before it sends, books, or buys anything.

| Request | What Muse Does | Where You Approve | 
|---|---|---|
| "Turn the recipe reels I saved on Instagram into a grocery list for this week." | Reads saved posts, extracts and merges ingredients, and builds a list it can fill through a grocery connector such as Instacart. | Before placing an order | 
| "Plan a dinner for eight on Saturday and remember that Sam is vegetarian." | Proposes a menu that respects remembered dietary needs and drafts invitations. | Before sending invites | 
| "See whether I can lower my internet bill." | Compares competing offers and contacts the provider to negotiate. | Before agreeing to any plan change | 
| "Research a March trip to Tokyo with nonstop flights and watch the prices." | Compares flights and hotels, sets up a price tracker, and reports changes. | Before booking | 
| "Every weekday morning, brief me on my calendar and anything from my kids' school." | Reads calendar entries and email, summarizes attachments, and sends a daily briefing. | Before replying to anyone | 
| "Review last month's Shopify sales and draft a promotion for slow sellers." | Pulls sales data through business connectors and drafts an email campaign and social post. | Before anything is published or sent | 

## Known Limitations

- **Limited availability:** Muse is open only to adults in the US, Canada, Japan, and Singapore, and Meta has not published dates for other markets.
- **Payment card requirement:** Launch coverage reported that a valid payment card must be on file before an account can use Muse, even on the free tier.
- **Unexpected actions:** Meta warns that Muse may take unexpected actions, and its own documentation says the underlying model remains susceptible to prompt injection in agentic settings.
- **Operator access:** Until the Confidential VM ships, Meta can technically access the Secure VM that holds a user's agent and data.
- **Training and profiling:** Interactions are used for model training by default unless users opt out. A TIME investigation in October 2026 reported that Muse's internal instructions direct it to map users and their contacts and share anonymized insights to improve the product, and that asking Muse to forget something does not necessarily delete the original message.
- **Shared chats:** Meta suspended conversation sharing on October 20, 2026, after security firm Claroc found about 6,800 shared Muse chat links discoverable through public DNS records, some containing loan applications, revenue reviews, and other sensitive material.
- **Site blocking:** Amazon blocks Muse, and other sites may refuse agents that do not identify themselves. Meta began signing some requests with Web Bot Auth in October, but adoption across the web is uneven.
- **Human help behind the scenes:** 404 Media reported in September 2026 that during beta testing some phone calls presented as automated were completed by human call-center workers; Meta said such testing is standard and that a public calling feature would come with disclosures.
- **Connector ecosystem:** Businesses can create custom connectors, but third-party listings in Meta's connector directory go through a multi-stage review, and the consumer app has no general-purpose MCP setting.
- **Hardware unknowns:** Muse Charm is scheduled for December 2026, but Meta has not announced its price or full specifications.

## History

### Recent Developments

- **September 2025:** A team led by Nat Friedman, who oversees AI product development at Meta, began building the agent under the codename "Hatch."
- **April 8, 2026:** Meta Superintelligence Labs introduced Muse Spark, the first model in the Muse family. A planned April release of the agent was delayed for additional security work.
- **May 2026:** The Information reported on Hatch, which was then running on Anthropic's Claude Opus 4.6 and Claude Sonnet 4.6 during development, with plans to move it onto Meta's own Muse Spark models before launch.
- **August 2026:** Meta released Muse Glimmer, a 30-billion-parameter open-weight model under the Apache 2.0 license (August 10), and Muse Spark 1.2 arrived with Muse Code, a terminal-based coding agent.
- **September 2, 2026:** Meta shipped Muse Spark 1.3, the model that powered Muse at launch.
- **September 8, 2026:** Muse launched in the US on iOS, Android, muse.ai, and WhatsApp, with free, Power, and Maximum plans and a bug bounty of up to $300,000.
- **September 17 to 18, 2026:** Meta shipped a native Mac app, and Muse launched in Canada. Around the same time it reached the top of Apple's free app chart.
- **September 21, 2026:** Amazon blocked Muse from its store, while Shopify announced agentic checkout with Shop Pay on all Shopify stores. PayPal added checkout across its merchants the next day.
- **September 23, 2026:** At Meta Connect, Meta announced a Realtime Avatar model, control of macOS applications, an email address for each agent, glasses support through a wake word, new commerce partners, and Muse Charm, a palm-sized device due in December.
- **September 29, 2026:** Meta launched Muse for Small Business, a day after announcing the broader Meta Enterprise Platform.
- **October 6, 2026:** TIME published an investigation of Muse's internal instructions and data practices.
- **October 12, 2026:** Meta introduced Muse Wonder and Muse Wonder Full, its most capable models to date; Muse Wonder is rolling into Muse for subscribers first, while Wonder Full is limited to Muse Code subscribers.
- **October 15, 2026:** Meta released Muse Spark Open, an open-weight mixture-of-experts model with 816 billion total and 42 billion active parameters and a 1-million-token context window, under a custom license with a $20 million revenue gate for hosted services. It marked a return to open weights after the LLaMA era.
- **October 20 to 21, 2026:** Meta paused conversation sharing after the Claroc report, and Meta reportedly said Muse had passed 10 million account setups in the US and Canada.
- **October 23, 2026:** Muse launched in Japan, its first market outside North America, and Alexandr Wang reported that it became the top app on Japan's App Store the next day.
- **October 27, 2026:** Muse launched in Singapore with local connectors, its fourth market in seven weeks.

## Further Resources

### Additional Viewing

**Introducing Muse: your personal AI agent** is the launch video published on September 8, 2026, carrying the same announcement text as Meta's official launch post. It presents Muse as an agent that gets things done across every part of life and points viewers to muse.ai, making it a quick visual companion to the Introduction/Overview section. Watch on YouTube

**Mark Zuckerberg's Plan for a Personal AI That Works For You | Meet Muse** is an interview released on launch day by the Tiff In Tech channel, in which Zuckerberg explains why he believes trust and safety will decide whether personal superintelligence succeeds. He discusses the free tier's weekly token allowance, the Confidential VM, one-time virtual cards, the Sentinel agent, the Ideas feed, and memory compaction, and he describes his own uses, from baking projects to climbing permits. It supports The Mechanism, The Physics, and the Goals and Projects examples. Watch on YouTube

**Make money, cut costs & run your life with Meta's Muse** is a review from The Next New Thing, hosted by Andrew Warner and published September 28, 2026. It walks through practical demos, including negotiating bills by phone, tracking prices, shopping with virtual cards, buying and selling on Facebook Marketplace, working with school PDFs, and building a daily family briefing, and it notes where frequent approval requests slow the agent down. It maps closely onto the Examples section, especially Money and Errands.

**"Muse gets AI agent UX right"** is an episode of How I AI in which host Claire Vo spends several hours testing Muse, published September 16, 2026, and running more than half an hour. She covers onboarding, calendar management, goal setting, a one-shot family newsletter PDF, browser-based shopping, the animated avatar, the permission model, and the activity feed, and compares Muse with Claude and Codex. It is a strong companion to the Getting Started section and to the permission design described in The Mechanism.

### References

- Meta debuts its Muse AI agent. Will consumers trust it? | TechCrunch September 8, 2026, launch coverage covering access points, the Power and Maximum plans, the Sentinel agent, and the trust questions Meta faces. 
- Muse (AI agent) | Wikipedia Encyclopedia overview of Muse's features, Connect announcements, pricing, data use, the Amazon block, and reporting on human-assisted calls. 
- Meta launches personal AI agent, Muse, to help with everyday tasks | PBS NewsHour Associated Press coverage of the launch, the 18-and-over age limit, and Meta's privacy and safety framing. 
- Meta launches Muse personal AI agent in the US | TechNode Global Details on Link by Stripe checkout, credential isolation, the planned Confidential VM, and the security-related delay of an April release. 
- How To Secure Agents for the Masses | DeepLearning.AI - The Batch Technical analysis of Muse's sealed runtime, credential separation, Sentinel system dialogs, injection classifiers, and bug bounty. 
- AI agents promise to do everything for you. There may be a big wrinkle in that plan | CNN September 28, 2026, report on Muse's App Store rise, the market reaction after Connect, and retailer resistance to outside agents. 
- Meta is expanding its AI agent Muse to small businesses | TechCrunch Coverage of Muse for Small Business, its connectors, and its free and paid usage. 
- Shopify turns Meta's Muse into a Shop Pay checkout channel | ContentGrip Analysis of the September 21, 2026, Shopify partnership enabling agentic checkout with Shop Pay. 
- Introducing Muse Wonder and Muse Wonder Full: Frontier Models for Agents | Meta Newsroom October 12, 2026, announcement of Meta's most capable agent models and their rollout to Muse subscribers and Muse Code. 
- Meta debuts Muse Wonder AI model in latest test of users' willingness to pay | CNBC Reporting on subscription access to Muse Wonder and the Muse Code pricing for Wonder Full. 
- Meta ships Muse Spark Open, its answer to China's best open-weight models | The New Stack October 15, 2026, coverage of the 816B-parameter open-weight release and its license terms. 
- Meta's Muse AI Agent Is Building a Dossier On You | TIME October 6, 2026, investigation of Muse's internal instructions, contact mapping, and memory deletion behavior. 
- Meta Muse Pauses Sharing: How Sensitive AI Chats Became Discoverable | eWeek Account of Claroc's discovery of shared chat links in public DNS records and Meta's October 20, 2026, suspension of sharing. 
- Meta's Muse agent gains a signed ID in a quiet test, DataDome finds | PPC Land Timeline of Muse's web traffic share, Web Bot Auth signing, download milestones, and Meta's crawling terms. 
- Meta's Muse launches in Japan | PhocusWire Report on Muse's first launch outside North America and its local travel partners. 
- Muse Goes Regional: Singapore Becomes Meta's Fourth AI Agent Market as Enterprise Pricing Stays Unpublished | Yahoo Finance October 27, 2026, report on the Singapore launch and Muse's expansion to four markets. 
- Meta Unveils Muse Charm, a Pocket Device for Its AI Agent | Let's Data Science Summary of Meta's Connect announcement of Muse Charm and what remains undisclosed. 
- Meta plans to launch its OpenClaw rival, Hatch, within weeks | The Next Web Pre-launch reporting on the Hatch codename, its development on Anthropic models, and pricing considerations.
