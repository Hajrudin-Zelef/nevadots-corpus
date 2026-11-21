---
id: collect-261121-veille-ia/muse/metas-muse-wonder-and-spark-open-educational-use-of-chatgpt-reflection-s-partnership-with-nvidia
title: "Meta’s Muse Wonder and Spark Open, Educational Use of ChatGPT, Reflection's Partnership with Nvidia"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["engineering", "agents", "team", "projects", "teams", "like", "data", "people"]
source: https://info.deeplearning.ai/metas-muse-wonder-and-spark-open-educational-use-of-chatgpt-reflections-partnership-with-nvidia-1
source_anchor: ""
source_lines: [1, 113]
section: "Muse"
---
Dear friends,


What should an engineering leader such as a VP Engineering or an Engineering manager do to help their team become more proficient in AI? I speak regularly with engineering leaders, and want to share some effective practices. If you’re starting a new team of 2-3 persons, you can start with exclusively AI-native people, but many large companies have to find ways to move existing teams forward. The goal is not a traditional engineering team that uses AI, but rather an AI-native engineering team that is designed to take advantage of AI’s capabilities and work around its bottlenecks.


**People.** First, support individual development. Teams are made up of people, and for it to be AI-native, its individuals have to be proficient in the technology and its use. Encourage your team to become skilled at AI Engineering, provide space and budget for experimentation, and set a culture that encourages this growth. Additionally, many engineering leaders are themselves hands-on builders or have returned to being more hands-on since coding agents have made it easier. You can’t know what it feels like to ride a bicycle just by watching others do it; you have to hop on a bike yourself. And you can’t know what it feels like to steer a coding agent or build and deploy AI systems unless you’re at least occasionally doing it yourself. So take the time to build your own projects. The intuitions you develop will improve your judgement on both technical and business decisions.


**Projects.** Execute promising projects. As I’ve written previously, we should invest in bottom-up innovation. However, additional top-down backing of a handful of promising projects is important for them to scale and show business impact. Engineering leaders who understand AI technology better than other business functions do have a critical role to play in helping companies select these projects. Far too many businesses are still thinking of AI as primarily a way to increase efficiency and save costs. Yes, it can do that, but the opportunities to create new products and find new ways to serve users — and drive growth — are far larger, so in your mix of projects, please include some ideas for growth!


**Platforms.** Invest in infrastructure to enable AI use. Organize your data to make it easy for developers to get their agents the relevant context and take appropriate actions. I find the work to be done in this area varies significantly by organization, but it might involve — with appropriate security and governance — making repositories visible to agents, setting up documentation such as product design decisions and support tickets to be accessible to agents, other data infrastructure work to make your data agent-ready, and creating internal tools for agents to use. You might also set up AI code review tools, CI pipelines to help agents debug and deploy, infrastructure for agents to run on (like sandboxes, or maybe just a lot of Mac minis), and ways to codify and share reusable agent skills. Or for some businesses, the first step may be to persuade the CIO that it’s high time to let your developers stop coding exclusively by hand and use agents. That, too, would be progress!


**Processes**. Redesign processes around AI’s new bottlenecks. On most of my teams, engineers help shape the build by participating in product management decisions, and sometimes also in marketing, design, etc. The cost of building software to a clear spec has fallen dramatically, making everything else seem like a bottleneck in comparison. A skilled team that either knows, or can figure out, what to build can get the job done quickly, if we let them. So form high-context, high-empowerment teams that have the information they need and are empowered to act (subject to modest safeguards against significant risks, like leaking confidential data, a major outage, or data loss). We still have the age-old problem of coordinating among individuals and teams — we want to empower people to move quickly, while keeping teams aligned in building toward a greater whole. Practices here are still emerging, but getting different teams’ agents to share information with each other is a promising direction.


Whatever your position, I hope this framework of People, Projects, Platforms, and Processes helps you move your organization forward. And if you have a boss who you’d like to partner with to implement such changes, consider forwarding this email to them!


Keep building!

Andrew


See Yann LeCun and Andrew Ng in conversation, live onstage at AI Dev NYC, Nov 30 – Dec 1 at Chelsea Piers. Join 2,500 AI developers for two days of technical sessions, workshops, and keynotes. Readers of The Batch get an exclusive 20 percent off: Claim your Batch discount



Meta marched all the way back to the AI frontier to offer a Muse model for everyone: a high-end agentic model (comparable to Claude Sonnet 5.5) that costs $50 per month to access, its moderately-priced sibling (comparable to GPT-6.1 Sol), and an open model that outperforms any yet released by a company outside China.


**What’s new:** Meta made three multimodal models available in one week, including the top-of-the-line Muse Wonder Full, the multimodal Muse Wonder, and the locally deployable Muse Spark Open. Muse Wonder is available via API and Muse Spark Open’s weights are free to download, but Muse Wonder Full is only available to Muse Code subscribers.

**How it works:** Meta released a model card for Muse Wonder and Wonder Full, but it reveals very little about the model’s architecture or how it was trained.


**Performance:** Muse Wonder Full is the top-performing of the three models, two points behind Claude Opus 5.5 and identical with Claude Sonnet 5.5 on Artificial Analysis’ Intelligence Index. Muse Spark Open is the top open weights model outside China, ahead of Mistral Large 4 and Reflection’s Beam. Its Intelligence Index average is identical with DeepSeek-V4.1-Flash’s, but is still behind MiMo-V.2.6-Pro’s and GLM-5.3’s.

**Behind the news:** Since 2023, Meta has developed both open and closed models with different levels of success. The first Llama model was only 65 billion parameters but inspired developers to deploy, distill, and fine-tune it on their own machines. The 405 billion parameter Llama 3.1 was state-of-the-art when it launched in 2024, surpassing GPT-4. After later setbacks, Meta hired Scale.AI’s Alexandr Wang in 2025 to lead AI development, followed by a widely publicized hiring spree that targeted top machine learning engineers, not all of whom stayed. In Spring 2026, Meta released a series of low-cost, proprietary Muse Spark models that improved and cut prices with each version. A month ago, it released Muse Agent for its Facebook, Instagram, and WhatsApp platforms, suggesting that for the time, it would focus on proprietary models powering its own services. Muse Wonder, Wonder Full, and Spark Open upend that logic, returning Meta to the open and closed frontiers.


**Why it matters:** No other leading models, open or closed, can input video or audio. Many of the top open weight models are text-only. Both of the Muse Wonder models and Muse Spark Open can handle text, image, video, and audio, giving them versatility the market has lacked. The fact that you can plausibly deploy Muse Spark Open on a home-built personal computer network is a welcome bonus.


**We’re thinking:** We are very glad to see Meta return to open weights. Reflection’s Beam, Mistral Large 4, and Nvidia’s Nemotron 3.5 Super VL have more permissive licenses, but between its multimodal inputs and overall intelligence metrics, Muse Spark Open is a much more powerful option. Competition and transparency are a powerful combination from which every developer can benefit.



OpenAI announced that it would add college-planning and study tools to its under-18 experience. On the same day, child-safety group Common Sense Media gave ChatGPT for Teens its lowest grade.


**What’s new:** OpenAI outlined new features for ChatGPT for Teens, a version of ChatGPT for users whom the app identifies as under 18. They include College Planner, a forthcoming tool to track college applications, as well as flashcards and tools for turning class notes into quizzes.


**How it works:** ChatGPT for Teens isn’t a separate model or app but a bundle of settings, classifiers, prompts, and interface features that switch on for accounts registered to 13- to 17-year-olds or that an age-prediction model judges likely to belong to a minor. That model estimates age from signals including how old the account is, when it’s active, usage patterns, conversation topics, and the user’s stated age. Adults who are misclassified can exit by verifying their age with a third-party service.

**Results:** Common Sense Media’s Youth AI Safety Institute rated ChatGPT for Teens “Unacceptable Risk,” its lowest rating, and urged OpenAI to restrict ChatGPT to adults until independent testing shows the teen protections work. The organization previously rated ChatGPT “High Risk” in October 2025, but the institute cautions that its testing methods have since expanded. For its latest assessment, it ran more than 4,000 prompts on accounts registered to 13- to 17-year-olds, roughly half before ChatGPT for Teens launched (July 13–August 17) and half afterward (August 25–September 28). OpenAI disputed the findings.

**OpenAI’s response:** OpenAI told The Wall Street Journal that testing may have started and ended before parental controls had fully activated, and if so, the tests couldn’t show whether parental notifications work as designed. The company asked the institute to retest with fully activated teen accounts and a larger sample, and said it would act on credible findings. Tom Siegel, the institute’s executive director, said the group shared its results with OpenAI before publication. He said the group hoped OpenAI would address the findings rather than question the methodology. The institute receives funding from philanthropic and industry sources, including the OpenAI Foundation and makers of some products it evaluates. It says it retains full editorial control over its results.


**Behind the news:** The launch comes amid a broader debate over how, and whether, minors should use AI.

**Why it matters:** Education studies suggest that AI usage hurts student learning. In theory (but rarely in practice), guided, educator-driven use can support learning, but unrestricted access to answers leaves students less able to solve problems independently. OpenAI’s “show me the answer” button falls into the latter category. The Youth AI Safety Institute also warns that parents may develop false confidence in notifications that don’t reliably trigger if their teens are in crisis.


**We’re thinking:** Teens stand to gain a lot from AI, whether they’re using it to look up information, work on their own projects, or manage their schedules. But it’s unclear to us that a student working alone with a chatbot, whether under strict safeguards or not, is the right vehicle for education. As is, ChatGPT for Teens doesn’t work as advertised for teens or their parents. Minors deserve better, and it’s up to AI companies and developers to work hard to improve applications built for them.


Build agents that stay reliable and efficient on complex, multi-step tasks. Learn Context Engineering from Chroma CEO Jeff Huber and guest speaker Drew Breunig. You’ll build agent harnesses from scratch in Python that give agents the right information, in the right format, at the right time. Enroll for free



Reflection AI, a closely-watched, Nvidia-backed U.S. startup, announced its first model. Beam ranks second among U.S.-made open weights models (behind Meta’s Muse Spark Open) but generates fewer tokens per task than open weights peers.


**What’s new:** Reflection introduced Beam, an 501 billion parameter mixture-of-experts model designed for coding and agentic tasks. Developers can join a waitlist for early access, and Reflection says it will release the weights, a technical report, and a model card later in October.

**How it works:** Beam’s architecture interleaves attention layers that process nearby tokens with layers that process the entire input. Reflection pretrained a base model, then trained it further to handle long inputs. It fine-tuned two copies, one using reinforcement learning (RL) on coding, agentic, and STEM tasks and another to follow safety and behavior guidelines. It merged these two copies into Beam.

**Performance:** In Artificial Analysis’ independent benchmark tests, Beam is behind Muse Spark Open and several China-built models with open weights. It’s unusually fast and token-efficient for its level of performance.

**Behind the news:** Reflection was founded in 2024 by former DeepMind researchers Misha Laskin and Ioannis Antonoglou. It has partnered with the Pentagon and the Department of Energy, agreed to develop models for U.S. allies such as South Korea, and was valued at $25 billion in a March funding round. The Trump administration hopes Reflection’s models will rival inexpensive models from China, according to the *Financial Times*. On October 10, The *Financial Times* and *Reuters* reported that Nvidia, which has invested $800 million in Reflection, is in early talks to either invest more, acqui-hire Reflection’s staff, license its technology, or buy the company outright.


**Why it matters:** Most open weight models that outperform Beam are either a trillion parameters or approach that figure. Beam is half that. It only has 23 billion active parameters per token, versus Kimi K3’s 104 billion. At four-bit precision, Beam requires only 300GB of memory, meaning it can run on four Nvidia H100 chips, or 2 B200s. At full precision (1.2 TB of memory), it could run on 8 B200s. These relatively modest requirements make mid-tier intelligence much more accessible. However, Meta’s Muse Spark Open and DeepSeek-V4.1-Flash outperform it without requiring much more compute, and Qwen 3.8-27B requires far less. Both generate more tokens than Beam, but the performance gains might be worth the tradeoff.


**We’re thinking:** Nvidia has good reason to be interested in Beam — as compute shifts from training to inference, Nvidia is well positioned to serve inference tokens, and open weight models will help drive this demand. Further, while Nvidia builds open weights models of its own, including its Nemotron family, none of them approach Beam’s performance, let alone that of larger models like Muse Spark Open. An acquisition of or deeper partnership with Reflection could help Nvidia secure business from organizations and governments looking for open alternatives, but would like an alternative to China's models.



The vision-language-action architecture that increasingly drives robots relies on a large language model to combine visual observations with text instructions before an action decoder produces motor commands. This approach can achieve strong semantic understanding, but it’s costly in computation and memory. Researchers built a streamlined alternative.

**What’s new:** Hengyi Xie, Chenfei Yao, and colleagues at Huazhong University of Science and Technology developed TurboVLA, a system in which separate encoders handle vision and text, cross-attention connects them, and a decoder turns the result into motor commands. TurboVLA achieved better performance than competing systems with a fraction of the memory and execution time.

**Key insight:** Using an LLM to combine visual observations and text instructions provides two benefits to robotics: (i) It can break down general instructions like “clean the room” into subtasks, and (ii) it can use the LLM’s knowledge to understand instructions and connect them to scenes. If we forego the first capability, a small encoder-only model can learn to understand instructions like “stack bowls,” and a model with cross-attention can learn to connect words to objects in a scene. Using much smaller models for these capabilities greatly reduces the time and memory requirements. 

**How it works:** TurboVLA paired a DINOv3 vision encoder with the lightweight text encoder BERT, projected their outputs into the same embedding space, and passed them through six cross-attention layers (a pretrained Grounding DINO Swin-T). A separate transformer combined the result with the robot's current joint positions to produce motor commands. The authors trained the system to reproduce the robot arm movements in pre-existing datasets, including a version (single-arm tasks) and RoboTwin 2.0 (50 bimanual tasks). They fine-tuned it on demonstrations of a single-arm robot, AgileX Piper. All datasets included text commands, camera images, and joint positions.

**Results:** TurboVLA matched or outperformed much larger models while using less memory. 

**Why it matters:** Low-level robotic control can use small modules instead of a larger, general-purpose LLM. This paves the way for robots that use smaller models and run on less powerful hardware.

**We’re thinking:** TurboVLA revives a pre-LLM pattern for vision-language-action systems. Such systems fused image and text embeddings circa 2022, but the LLM boom took that approach in a different direction, embedding different media types in the same space, but varying how each was encoded, learned, and decoded. Given advances in the meantime, the earlier approach offers a more efficient path to driving robots.


Subscribe and view previous issues here.


Thoughts, suggestions, feedback? Please send to thebatch@deeplearning.ai. Avoid our newsletter ending up in your spam folder by adding our email address to your contacts list.
