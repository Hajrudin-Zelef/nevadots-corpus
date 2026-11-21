---
id: collect-261121-veille-ia/gemini-google/historic-first-google-tpu-runs-gemma-4-in-orbit-htx-insights
title: "Historic First! Google TPU Runs Gemma 4 in Orbit | HTX Insights"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "space", "satellite", "orbit", "first", "also", "year", "about"]
source: https://www.htx.com/news/historic-first-google-tpu-runs-gemma-4-in-orbit-h2hGS0rL/
source_anchor: ""
source_lines: [1, 176]
section: "Gemini_Google"
---
"Write a haiku about being in space."

A few days ago, Google engineers sent this prompt via a ground station to a satellite orbiting the Earth.

Upon receiving the command, the satellite's onboard TPU activated, and the pre-deployed Gemma 4 began executing the inference task. Later, when the satellite passed near Svalbard, Norway, the ground station finally received the reply from space.

Blue marble below

Data streams in silent grace

Starlight feeds my core.


Not long ago, Google CEO Sundar Pichai excitedly shared this news,**calling it the "Hello World" moment of Project Suncatcher. This is also the first time a data-center-class TPU has generated and returned a complete AI response in orbit.**

From announcing last year that it would send AI compute into space to now actually receiving a poem from orbit, Google's space data center plan has finally taken a step toward actual operation.

## **Google TPU Is Really Running in Space**

The starting point of this experiment dates back to a month ago.

On October 1, the Project Suncatcher prototype satellite, developed by Google in partnership with satellite company Planet, entered Earth orbit aboard SpaceX's Transporter-18 rideshare mission.

In the weeks that followed, the engineering team successively completed debugging of the satellite's various systems, started the host machine and TPU computing equipment, and carried out early machine learning tests.

Until this week, the team finally attempted to have the satellite perform a real generative AI task.

After completing the first response, the team also tried having the model generate other forms of space-themed poetry and queried the technical status of the computing payload through natural language prompts. Google said subsequent experiments will gradually shift toward more practical system tests to observe how the TPU performs under orbital thermal stress and radiation conditions.

According to TechCrunch,**this prototype satellite carries four Google TPUs and can provide up to about 1 kW of power, but due to current system power supply constraints, only one TPU can be started at a time, with an operating time of about 20 minutes.**

This also brings to mind the situation a year ago when Google first proposed this plan.

In November 2025, we introduced this project in *AI Space Race? Nvidia H100 Just Went to Space, and Google Project Suncatcher Also Wants to Send TPUs Up*.

According to the initially announced schedule, Google planned to launch two prototype satellites with Planet in early 2027 to test key technologies such as in-orbit TPU operation and inter-satellite optical communication. After that, Google further adjusted the experiment schedule, using Planet's satellite platform to carry out first-stage verification ahead of time, and successfully sent the TPU into orbit in October this year.

Google is also not the first company to bring large models into space.

Last November, startup Starcloud sent a Starcloud-1 satellite carrying an Nvidia H100 GPU into orbit, and subsequently completed experiments running a large language model and training a small model based on nanoGPT.

In August this year, SpaceX and Nvidia announced a partnership to develop Starmind AI1, planning to use Rubin GPUs and Vera CPUs to build a larger-scale satellite computing payload.

From Nvidia GPUs to Google TPUs, space AI computing is attracting more and more tech companies.

## **From a Single TPU to a Real Space Data Center**

Why go to such great lengths to send AI chips into space?

This question has repeatedly appeared in the comment sections of various companies over the past year.

Actually, the answer is simple: power supply.

According to research results previously published by Google, under appropriate orbital conditions,**space solar panels can generate up to about 8 times as much electricity as under ground conditions. If the satellite operates in a dawn-dusk sun-synchronous orbit, it can also receive nearly continuous sunlight and may not even need to rely heavily on large batteries.**

Around this goal, Project Suncatcher does not adopt the idea of building a large space-station-style data center, but rather has a large number of relatively small satellites form a tight formation, each carrying solar panels and TPUs, and then connecting them using free-space optical communication technology.

In a research paper previously published by Google and now published in *Joule*, the team showed an example formation design consisting of 81 satellites. These satellites operate in low Earth orbit at an average altitude of about 650 kilometers, with the entire cluster radius at about 1 kilometer, and the distance between some adjacent satellites varying within a range of about 100 to 200 meters.

Why have the satellites fly so close together? Simply put, it is for convenient communication.

In ground data centers, large-scale model training usually requires distributing tasks across many accelerators, and different chips must frequently exchange intermediate computation results. If communication bandwidth is insufficient or latency is too high, even with enough compute chips, the performance of the entire system may be severely limited.

Google hopes to move a similar distributed computing approach into space, so satellites need to establish bandwidth connections far exceeding traditional satellite communication requirements.**According to the team's research, to achieve computing capability comparable to ground data centers in the future, the transmission rate of inter-satellite links needs to reach 10 Tbps.**

To approach this goal, Google is studying the use of dense wavelength-division multiplexing and spatial multiplexing technologies to increase laser communication bandwidth, while allowing satellites to operate in relatively tight formations.

In previous ground experiments, the team already achieved a one-way transmission rate of 800 Gbps and a bidirectional combined rate of 1.6 Tbps using a pair of optical communication transceivers.

This is a considerable number, but there is still a gap from the inter-satellite communication capability needed in the future, and communication between ground devices is completely different in engineering difficulty from maintaining stable connections among multiple satellites moving at high speed in orbit.

To keep dozens or even hundreds of satellites in appropriate relative positions over the long term, Google also needs to consider factors such as Earth's non-spherical gravitational field, atmospheric drag, and orbital perturbations, and assist formation control through physical models and machine learning methods.

At the same time, data exchange between satellites is only part of the problem.**The data transmission problem between the ground and satellites is even harder to solve.**

Especially for tasks that require frequent reading of large amounts of ground data, updating model parameters, or responding to user requests, how to balance communication windows, transmission bandwidth, and latency will also affect actual operating efficiency.

By contrast, for processing data generated by Earth observation satellites in orbit,**if image analysis can be completed directly in orbit and then results rather than all raw data are sent back to the ground, it may be possible to reduce the downlink communication burden. This is the area where space computing has the greatest advantage.**

However, Google's goal is clearly not limited to this. What it wants to verify is whether a truly scalable AI computing cluster can be formed in space in the future.

This concept has also been questioned by many netizens.

Previously, in Reddit's r/space community, someone specifically raised a question: since building and maintaining a space data center is so difficult, why not put data centers under the sea?

This question is not without practical basis. Microsoft's earlier Project Natick verified the operational feasibility of underwater data centers, and building data centers in cold regions such as Iceland and Northern Europe is already a practical option for reducing part of the cooling burden.

The question with going to space is: even if more solar energy can be obtained in space, is the computing capability it can provide really worth such great effort?

The answer is obviously not that simple yet.

## **Heat Dissipation? Space Radiation? Cost?**

After the news was announced on October 30, discussions on X about the feasibility of space data centers appeared again.

User Luke Leisher remarked rather sarcastically: "How is this possible? Didn't the smartest people on the internet tell me that the laws of physics have determined orbital data centers are completely impossible?"

Another user, Robert de Negro, raised a more specific question:**The real difficulty may be scale. If a satellite eventually carries a large amount of computing equipment, then managing the heat generated by dozens or even hundreds of chips is completely different from managing a small number of chips.**

In the end, heat dissipation in space is still an unavoidable issue.

On Earth, computing equipment can remove heat through airflow, liquid cooling, and other methods, and the heat is ultimately discharged into the surrounding environment through corresponding facilities. But in the near-vacuum of space, there is no air convection available, and the waste heat generated by chips is ultimately released outward mainly through thermal radiation.

This means that**a space data center must transfer heat generated by computing equipment to radiators with sufficient area through thermally conductive structures and cooling fluids, and then emit it in the form of electromagnetic radiation.**

Google previously introduced a cooling scheme combining heat pipes and radiators, and has already tested it in a thermal vacuum chamber simulating the space thermal environment. Although this technical route is not unfamiliar in the aerospace field, as the power of computing equipment continues to increase, the area, mass, and structural relationship of the cooling system with other satellite equipment must all be redesigned.

At present, the Project Suncatcher prototype satellite carries four TPUs but can still only start one at a time and operate for about 20 minutes, which precisely shows that**the current power and thermal management capabilities still have great room for improvement before large-scale continuous computing can be achieved.**

Moreover, the heat dissipation problem will ultimately come back to cost.

In related discussions on Hacker News, some netizens argued that the controversy is not about whether humans know how to dissipate heat through thermal radiation, but rather**how many radiators, heat conduction pipes, and other supporting equipment need to be added in order to dissipate enough heat. Once these extra weights are included, satellite launch costs will also rise accordingly.**

Another issue that requires long-term verification is space radiation.

For satellites that rely on solar power generation, sunlight is an important energy source,**but the radiation environment such as high-energy solar particles and galactic cosmic rays may also affect electronic equipment.** When high-energy particles interact with sensitive regions in chips, they may cause data bit flips in memory, turning an original 0 into a 1, or causing errors in certain computation results.

In response, Google has previously carried out dedicated ground experiments.

The research team used a 67 MeV proton beam to test the sixth-generation Trillium TPU and observe the effects of total ionizing dose and single-event effects on the chip. The results showed that**the high-bandwidth memory system is a relatively sensitive part, but related anomalies did not begin to appear until the cumulative dose reached about 2 krad (Si), nearly three times the expected five-year mission dose under shielded conditions.**

**At a test dose of up to 15 krad (Si) per chip, the team also did not observe permanent hard failures attributable to total ionizing dose.**

Such results make Google more optimistic about the space adaptability of existing TPUs, but ground particle beam experiments cannot fully simulate long-term radiation, thermal cycling, and other complex factors in the real orbital environment. Therefore, one important purpose of this satellite mission is precisely to further verify chip performance in the real environment.

Even if heat dissipation and radiation problems can both be solved engineering-wise, Google must also face the long-term maintenance and upgrading of computing equipment.

Ground data centers can gradually replace old equipment as AI chip performance improves, while power supply, network, and cooling infrastructure can continue to be used. But for satellites, chips, solar panels, radiators, and communication systems are often highly integrated. After equipment fails or computing chips gradually become outdated, how to continue using the remaining infrastructure becomes a complex issue.

Someone on Hacker News also specifically pointed out that space computing facilities may face a different depreciation logic from ground data centers:**if a GPU or TPU on a satellite has already lost competitiveness, even if its solar panels and cooling system can still be used, it may not be easy to continue realizing the original value.**

This does not yet take into account orbital debris, micrometeoroids that spacecraft may encounter, and additional issues such as regular orbit maintenance and final deorbit disposal of satellites.

Therefore, the debate about space data centers often ultimately comes down to the same question: cost.

In the research paper previously published by Google, the company specifically analyzed future launch costs and gave an important reference number — **200 US dollars per kilogram**.

Under this assumption, after amortizing launch costs according to equipment service life and power supply, this may correspond to**a cost of about 810 US dollars per kilowatt per year**.

By comparison, the paper cites US ground data center energy expenditures of about**570 to 3,000 US dollars per kilowatt per year**.

From this point of view, space data centers are somewhat feasible. But Google's calculation is very rough, and some netizens on Reddit spotted the problem at a glance.

## **"Launch Cost of 200 US Dollars per Kilogram?"**

Google's calculation has an assumption: that future rocket reusability and launch scale will continue to improve and maintain the historically observed cost decline trend.

According to the paper's estimate,**to reach a cost level of 200 US dollars per kilogram around 2035 along this trend, a scale of about 180 Starship launches per year may be needed.**

Obviously, this is somewhat optimistic.

In fact, Google itself is not naive and does not hold expectations for short-term commercialization of space data centers.

On October 1 this year, in an interview with NPR, Project Suncatcher lead Travis Beals said he does not believe space data centers will become cheaper than ground-based ones within the next five years, and that truly achieving economic competitiveness may still take longer.

Google defines Project Suncatcher as a long-term "moonshot." Next, according to the currently announced roadmap, Google plans to launch two satellites with high-speed optical communication capabilities in 2027 to further verify the technical conditions required for inter-satellite interconnection and distributed machine learning tasks.

As for further expanding this system into a truly commercially competitive space data center, more time is needed for technological accumulation, as well as joint progress in launch costs, hardware reliability, and system architecture.

For the space AI computing network Google is trying to build, this little poem is still only the first reply.

Reference links:

https://blog.google/innovation-and-ai/models-and-research/google-research/project-suncatcher-satellite-response/

https://research.google/blog/exploring-a-space-based-scalable-ai-infrastructure-system-design/

https://doi.org/10.1016/j.joule.2026.102678

https://services.google.com/fh/files/misc/suncatcher_paper.pdf

https://techcrunch.com/2026/10/30/googles-first-data-center-satellite-is-online-and-writing-poetry/

https://www.opb.org/article/2026/10/01/google-launches-project-suncatcher-a-step-towards-ai-data-centers-in-space/

This article is from the WeChat public account "Machine Heart" (ID: almosthuman2014), author: AI-focused
